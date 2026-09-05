# Сеть и сетевые сообщения

> [⬅ Вернуться к оглавлению вики](../README.md)

> [Индекс справочника builtins](./README.md)

Сетевые builtins FTEQW делятся на три больших класса: запись серверных сообщений из SSQC (`Write*`), чтение custom-пакетов на стороне CSQC (`read*` и `ReadPicture`) и вспомогательные сетевые функции вроде `multicast`, `stuffcmd`, `sendevent`, `sendpacket` и браузера серверов.

Основа всей системы записи — **канал сообщения**, который всегда передаётся первым аргументом в `WriteByte`, `WriteShort`, `WriteString` и остальные `Write*` builtins.

- `MSG_BROADCAST` — ненадёжная отправка всем клиентам.
- `MSG_ONE` — надёжная отправка одному клиенту из `msg_entity`. В QuakeWorld-режиме этот путь считается устаревшим: длинные сообщения могут разбиваться неудобно для клиента, поэтому для custom CSQC-пакетов лучше не использовать его напрямую.
- `MSG_ALL` — надёжная отправка всем клиентам.
- `MSG_INIT` — запись в signon-буфер. Эти данные увидят и те клиенты, которые подключатся позже. Буфер очищается только при смене карты, поэтому его нельзя засорять повторяющимися событиями.
- `MSG_MULTICAST` — запись во временный multicast-буфер. После заполнения нужно вызвать `multicast(where, MULTICAST_*)`, чтобы реально разослать пакет.

Для `MSG_MULTICAST` используются режимы `MULTICAST_ALL`, `MULTICAST_PHS`, `MULTICAST_PVS`, `MULTICAST_ONE`, `MULTICAST_ONE_NOSPECS`, а также их надёжные варианты `*_R`. На практике это главный способ слать **SSQC → CSQC** custom-сообщения: сервер пишет `SVC_CGAMEPACKET` в буфер `MSG_MULTICAST`, затем добавляет свой тип события и полезные данные, а клиент читает их в `CSQC_Parse_Event` через `readbyte`, `readcoord`, `readstring` и остальные парные builtins.

Важно помнить ещё три правила:

1. Формат чтения обязан в точности совпадать с форматом записи. Если сервер записал `WriteByte`, потом `WriteCoord`, клиент обязан читать `readbyte`, потом `readcoord` в том же порядке.
2. `WriteCoord`/`readcoord` и `WriteAngle`/`readangle` работают в сетевом формате с ограниченной точностью. Базовое допущение — fixed-point координаты около 13.3 бита и 8-битные углы, но при наличии расширений вроде `FTE_PEXT_FLOATCOORDS` движок может согласовать более точный формат. Поэтому читать такие поля нужно только их парными builtins, а не `readfloat`.
3. Для custom SSQC → CSQC сообщений безопаснее всего схема `WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET)` + дальнейшие `Write*` + `multicast(...)`. Сам движок при необходимости сам обработает служебную длину пакета для прокси и protocol translation.

## Функции

### WriteByte
`void(float to, float val) WriteByte = #52;`

* **to** — канал записи, обычно один из `MSG_*`.
* **val** — беззнаковое 8-битное значение от `0` до `255`.

#### Описание и логика работы
`WriteByte` пишет ровно 1 байт. Это самый дешёвый по трафику способ передать небольшое целое: тип события, флаг, индекс режима, количество до 255. При выходе за диапазон движок не останавливает код, а отбрасывает старшие биты и записывает усечённое значение; в developer-режиме выводится предупреждение о truncation. Для `MSG_ONE` должен быть заранее выставлен `msg_entity`. Для SSQC → CSQC custom-пакетов обычно используется не `MSG_ONE`, а `MSG_MULTICAST` с последующим `multicast(..., MULTICAST_ONE_R)`.

#### Практические сценарии использования
```
// код SSQC
const float EV_FLASH = 1;

void(entity pl, float flashcolor) SendFlashEvent =
{
	msg_entity = pl;
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, EV_FLASH);
	WriteByte(MSG_MULTICAST, flashcolor); // 0..255
	multicast(pl.origin, MULTICAST_ONE_R);
};
```
```
// код CSQC
const float EV_FLASH = 1;
float hud_flash_color;

void() CSQC_Parse_Event =
{
	float ev = readbyte();
	if (ev == EV_FLASH)
		hud_flash_color = readbyte();
};
```

### WriteChar
`void(float to, float val) WriteChar = #53;`

* **to** — канал записи `MSG_*`.
* **val** — знаковое 8-битное значение от `-128` до `127`.

#### Описание и логика работы
`WriteChar` удобен для маленьких signed-дельт: откат отдачи, смещение камеры, изменение счётчика в пределах одного байта. По размеру он так же дёшев, как `WriteByte`, но интерпретируется как signed. Значения вне диапазона усекутся по битовой маске; например `130` на чтении превратится в `-126`. Если вам нужна только неотрицательная величина, лучше использовать `WriteByte`, чтобы не тратить диапазон попусту.

#### Практические сценарии использования
```
// код SSQC
const float EV_KICK = 2;

void(entity pl, float kick_pitch) SendKick =
{
	msg_entity = pl;
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, EV_KICK);
	WriteChar(MSG_MULTICAST, kick_pitch); // например -20..20
	multicast(pl.origin, MULTICAST_ONE_R);
};
```
```
// код CSQC
const float EV_KICK = 2;
float view_kick_pitch;

void() CSQC_Parse_Event =
{
	float ev = readbyte();
	if (ev == EV_KICK)
		view_kick_pitch = readchar();
};
```

### WriteShort
`void(float to, float val) WriteShort = #54;`

* **to** — канал записи `MSG_*`.
* **val** — знаковое 16-битное значение, обычно `-32768..32767`.

#### Описание и логика работы
`WriteShort` занимает 2 байта и подходит для средних по диапазону чисел: HP, урон, score delta, индексы ресурсов, таймеры в миллисекундах. Документация FTEQW отдельно отмечает, что значения до `65535` не вызывают warning, но `readshort` всё равно трактует их как signed, то есть числа больше `32767` станут отрицательными. Сетевой расход вдвое выше `WriteByte`, но вдвое ниже `WriteLong` и `WriteFloat`.

#### Практические сценарии использования
```
// код SSQC
const float EV_SCORE = 3;

void(entity pl, float delta) SendScoreDelta =
{
	msg_entity = pl;
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, EV_SCORE);
	WriteShort(MSG_MULTICAST, delta);
	multicast(pl.origin, MULTICAST_ONE_R);
};
```
```
// код CSQC
const float EV_SCORE = 3;
float last_score_delta;

void() CSQC_Parse_Event =
{
	float ev = readbyte();
	if (ev == EV_SCORE)
		last_score_delta = readshort();
};
```

### WriteLong
`void(float to, float val) WriteLong = #55;`

* **to** — канал записи `MSG_*`.
* **val** — 32-битное целое, передаваемое через аргумент типа `float`.

#### Описание и логика работы
`WriteLong` пишет 4 байта signed integer. Это полезно для битовых масок, CRC, timestamp-счётчиков и любых значений, не помещающихся в `short`. Но у builtin есть важное ограничение: аргумент QuakeC имеет тип `float`, поэтому без потерь гарантируются только 24 последовательных бита точности. Для полноценных 32-битных int нужен `WriteInt`, а не `WriteLong`. По трафику это столько же, сколько `WriteFloat`, но без хранения дробной части.

#### Практические сценарии использования
```
// код SSQC
const float EV_FLAGS = 4;

void(entity pl, float itemmask) SendItemMask =
{
	msg_entity = pl;
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, EV_FLAGS);
	WriteLong(MSG_MULTICAST, itemmask);
	multicast(pl.origin, MULTICAST_ONE_R);
};
```
```
// код CSQC
const float EV_FLAGS = 4;
float hud_itemmask;

void() CSQC_Parse_Event =
{
	float ev = readbyte();
	if (ev == EV_FLAGS)
		hud_itemmask = readlong();
};
```

### WriteAngle
`void(float to, float val) WriteAngle = #57;`

* **to** — канал записи `MSG_*`.
* **val** — угол в градусах.

#### Описание и логика работы
`WriteAngle` записывает угол в сетевом формате движка. Базовое предположение документации — 8 бит, то есть 256 квантов на полный круг, а не непрерывные 360 градусов. Это дёшево по трафику и отлично подходит для yaw/pitch коротких событий, но мелкие изменения будут округляться. Формат нельзя безопасно читать через `readfloat`; использовать нужно только `readangle`. Если для вашей механики важна буквальная плавающая точка, передавайте угол через `WriteFloat`.

#### Практические сценарии использования
```
// код SSQC
const float EV_LOOK = 5;

void(entity pl, float yaw) SendForcedLook =
{
	msg_entity = pl;
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, EV_LOOK);
	WriteAngle(MSG_MULTICAST, yaw);
	multicast(pl.origin, MULTICAST_ONE_R);
};
```
```
// код CSQC
const float EV_LOOK = 5;
float forced_yaw;

void() CSQC_Parse_Event =
{
	float ev = readbyte();
	if (ev == EV_LOOK)
		forced_yaw = readangle();
};
```

### WriteCoord
`void(float to, float val) WriteCoord = #56;`

* **to** — канал записи `MSG_*`.
* **val** — одна координатная ось или другая пространственная величина.

#### Описание и логика работы
`WriteCoord` кодирует число в координатном сетевом формате. В базовой форме это как минимум около 13.3 fixed-point: диапазон порядка `-4096..4095.875` с шагом `1/8`. В developer-режиме движок предупреждает, если значение выходит за этот диапазон при классическом fixed-point кодировании. При negotiated extensions вроде `FTE_PEXT_FLOATCOORDS` формат может стать точнее, поэтому клиент обязан читать значение только через `readcoord`. По трафику `coord` обычно дешевле полного `float`, но не является точным заменителем `WriteFloat`.

#### Практические сценарии использования
```
// код SSQC
const float EV_MARK = 6;

void(entity pl, vector org) SendMarker =
{
	msg_entity = pl;
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, EV_MARK);
	WriteCoord(MSG_MULTICAST, org_x);
	WriteCoord(MSG_MULTICAST, org_y);
	WriteCoord(MSG_MULTICAST, org_z);
	multicast(org, MULTICAST_ONE_R);
};
```
```
// код CSQC
const float EV_MARK = 6;
vector marker_org;

void() CSQC_Parse_Event =
{
	float ev = readbyte();
	if (ev == EV_MARK)
		marker_org = [readcoord(), readcoord(), readcoord()];
};
```

### WriteString
`void(float to, string val) WriteString = #58;`

* **to** — канал записи `MSG_*`.
* **val** — null-terminated строка.

#### Описание и логика работы
`WriteString` передаёт строку переменной длины вместе с завершающим нулём. Это удобно для имён, сообщений журнала, путей к ресурсам и сериализованных команд. Цена по сети линейно зависит от длины строки, поэтому для частых сообщений лучше передавать индекс или enum, а не полные тексты. Документация FTEQW отдельно предупреждает, что перекодировки не происходит: сервер и клиент должны одинаково понимать кодировку, иначе UTF-8 может интерпретироваться некорректно.

#### Практические сценарии использования
```
// код SSQC
const float EV_OBJECTIVE = 7;

void(entity pl, string text) SendObjective =
{
	msg_entity = pl;
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, EV_OBJECTIVE);
	WriteString(MSG_MULTICAST, text);
	multicast(pl.origin, MULTICAST_ONE_R);
};
```
```
// код CSQC
const float EV_OBJECTIVE = 7;
string objective_text;

void() CSQC_Parse_Event =
{
	float ev = readbyte();
	if (ev == EV_OBJECTIVE)
		objective_text = readstring();
};
```

### WriteEntity
`void(float to, entity val) WriteEntity = #59;`

* **to** — канал записи `MSG_*`.
* **val** — server-side entity, чей номер нужно передать.

#### Описание и логика работы
`WriteEntity` передаёт сетевой индекс сущности. Размер кодирования специально не фиксирован документацией, поэтому читать значение нужно только через `readentitynum`. Важный practical caveat: у клиента к моменту чтения может ещё не быть всех полей этой сущности, особенно при latency и сложной синхронизации, поэтому `readentitynum` часто используют как отложенную ссылку — сохраняют номер и перепривязываются к реальной сущности позже. Для передачи именно CSQC-owned entity handle builtin не подходит.

#### Практические сценарии использования
```
// код SSQC
const float EV_ATTACKER = 8;

void(entity pl, entity attacker) SendAttacker =
{
	msg_entity = pl;
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, EV_ATTACKER);
	WriteEntity(MSG_MULTICAST, attacker);
	multicast(pl.origin, MULTICAST_ONE_R);
};
```
```
// код CSQC
const float EV_ATTACKER = 8;
float attacker_entnum;

void() CSQC_Parse_Event =
{
	float ev = readbyte();
	if (ev == EV_ATTACKER)
		attacker_entnum = readentitynum();
};
```

### WriteFloat
`void(float buf, float fl) WriteFloat = #280;`

* **buf** — канал записи `MSG_*`.
* **fl** — 32-битный float без дополнительного сетевого сжатия.

#### Описание и логика работы
`WriteFloat` всегда пишет полный IEEE-like 32-битный float без промежуточного округления в `short`, `coord` или `angle`. Это дороже, чем `WriteByte`, `WriteShort`, `WriteCoord` и `WriteAngle`, но сохраняет дробную часть полностью. Используйте его для значений, где действительно нужна точность: коэффициенты отдачи, интерполяционные веса, нестандартные временные метки. Читаться такой формат должен только через `readfloat`.

#### Практические сценарии использования
```
// код SSQC
const float EV_SPREAD = 9;

void(entity pl, float spread) SendSpread =
{
	msg_entity = pl;
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, EV_SPREAD);
	WriteFloat(MSG_MULTICAST, spread);
	multicast(pl.origin, MULTICAST_ONE_R);
};
```
```
// код CSQC
const float EV_SPREAD = 9;
float weapon_spread;

void() CSQC_Parse_Event =
{
	float ev = readbyte();
	if (ev == EV_SPREAD)
		weapon_spread = readfloat();
};
```

### WritePicture
`void(float to, string s, float sz) WritePicture = #501;`

* **to** — канал записи `MSG_*`.
* **s** — имя изображения.
* **sz** — желаемый size limit; в FTEQW игнорируется.

#### Описание и логика работы
`WritePicture` формально предназначен для передачи изображения по сети, но в FTEQW реализация упрощена: builtin фактически пишет строку имени картинки и затем размер `0`. То есть по смыслу это «`WriteString` с договорённостью, что на клиенте будет `ReadPicture`». Аргумент `sz` в FTEQW не используется. Такой формат полезен для HUD-иконок и меню-изображений, когда CSQC сможет скачать/подгрузить ресурс самостоятельно.

#### Практические сценарии использования
```
// код SSQC
const float EV_ICON = 10;

void(entity pl) SendPickupIcon =
{
	msg_entity = pl;
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, EV_ICON);
	WritePicture(MSG_MULTICAST, "gfx/hud/armor.tga", 0);
	multicast(pl.origin, MULTICAST_ONE_R);
};
```
```
// код CSQC
const float EV_ICON = 10;
string pickup_icon;

void() CSQC_Parse_Event =
{
	float ev = readbyte();
	if (ev == EV_ICON)
		pickup_icon = ReadPicture();
};
```

### WriteUnterminatedString
`void(float target, string str) WriteUnterminatedString = #456;`

* **target** — канал записи `MSG_*`.
* **str** — строка без автоматического завершающего `\0`.

#### Описание и логика работы
`WriteUnterminatedString` записывает байты строки подряд, но не добавляет null terminator. В текущей реализации движка это буквально цикл из `WriteChar` по каждому символу. Бuiltin нужен для packet formats, где длина строки хранится отдельно или строка является частью более крупного бинарного блока. Это компактнее и безопаснее обычного `WriteString`, если следом сразу идут другие поля, которые не должны считаться продолжением текста.

#### Практические сценарии использования
```
// код SSQC
const float EV_MAGIC = 11;

void(entity pl) SendMagic =
{
	msg_entity = pl;
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, EV_MAGIC);
	WriteByte(MSG_MULTICAST, 4);
	WriteUnterminatedString(MSG_MULTICAST, "PING");
	WriteShort(MSG_MULTICAST, 1500);
	multicast(pl.origin, MULTICAST_ONE_R);
};
```
```
// код CSQC
const float EV_MAGIC = 11;
float ping_msec;

void() CSQC_Parse_Event =
{
	float ev = readbyte();
	float len;

	if (ev != EV_MAGIC)
		return;

	len = readbyte();
	if (len == 4 && readbyte() == 80 && readbyte() == 73 && readbyte() == 78 && readbyte() == 71)
		ping_msec = readshort();
};
```

### readbyte
`float() readbyte = #360;`

* **возвращаемое значение** — беззнаковое 8-битное число `0..255`.

#### Описание и логика работы
`readbyte` — парный reader для `WriteByte`. Вызывать его можно только в тех точках, где CSQC действительно читает сетевой поток: прежде всего в `CSQC_Parse_Event`, а также в некоторых callback-путях чтения entity updates. Вне этих точек движок abort-ит вызов. Бuiltin незаменим для чтения opcode custom-сообщения, чисел-энумов и компактных флагов.

#### Практические сценарии использования
```
// код SSQC
const float EV_STATE = 12;

void(entity pl, float stateid, float countdown) SendState =
{
	msg_entity = pl;
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, EV_STATE);
	WriteByte(MSG_MULTICAST, stateid);
	WriteByte(MSG_MULTICAST, countdown);
	multicast(pl.origin, MULTICAST_ONE_R);
};
```
```
// код CSQC
const float EV_STATE = 12;
float arena_state;
float arena_countdown;

void() CSQC_Parse_Event =
{
	float ev = readbyte();
	if (ev == EV_STATE)
	{
		arena_state = readbyte();
		arena_countdown = readbyte();
	}
};
```

### readchar
`float() readchar = #361;`

* **возвращаемое значение** — signed 8-битное число `-128..127`.

#### Описание и логика работы
`readchar` считывает ровно тот signed byte, который был записан `WriteChar`. Он удобен там, где отрицательные значения имеют смысл сами по себе: отклонение прицела, signed impulse, delta от предыдущего значения. Если читать `WriteByte` через `readchar`, все значения выше `127` начнут интерпретироваться как отрицательные.

#### Практические сценарии использования
```
// код SSQC
const float EV_RECOIL = 13;

void(entity pl, float recoil_x, float recoil_y) SendRecoil =
{
	msg_entity = pl;
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, EV_RECOIL);
	WriteChar(MSG_MULTICAST, recoil_x);
	WriteChar(MSG_MULTICAST, recoil_y);
	multicast(pl.origin, MULTICAST_ONE_R);
};
```
```
// код CSQC
const float EV_RECOIL = 13;
vector recoil_delta;

void() CSQC_Parse_Event =
{
	float ev = readbyte();
	if (ev == EV_RECOIL)
		recoil_delta = [readchar(), readchar(), 0];
};
```

### readshort
`float() readshort = #362;`

* **возвращаемое значение** — signed 16-битное число.

#### Описание и логика работы
`readshort` читает значения, переданные `WriteShort`. Это стандартный выбор для 2-байтовых полей: health, armor, ammo reserve, счёт таймера, компактные индексы и т. п. Если на сервере сознательно было отправлено значение `50000`, клиент через `readshort` получит отрицательное число, потому что builtin всегда трактует 16 бит как signed.

#### Практические сценарии использования
```
// код SSQC
const float EV_AMMO = 14;

void(entity pl, float shells) SendAmmoReserve =
{
	msg_entity = pl;
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, EV_AMMO);
	WriteShort(MSG_MULTICAST, shells);
	multicast(pl.origin, MULTICAST_ONE_R);
};
```
```
// код CSQC
const float EV_AMMO = 14;
float reserve_shells;

void() CSQC_Parse_Event =
{
	float ev = readbyte();
	if (ev == EV_AMMO)
		reserve_shells = readshort();
};
```

### readlong
`float() readlong = #363;`

* **возвращаемое значение** — signed 32-битное целое, возвращённое в QuakeC как `float`.

#### Описание и логика работы
`readlong` предназначен для данных из `WriteLong` или `WriteInt`. Для значений, которые изначально были полноценными 32-битными битовыми масками, QuakeC всё равно получит их как `float`, так что с очень большими точными int нужно быть аккуратнее. На практике builtin удобен для long-таймеров, state masks и CRC-подобных идентификаторов, когда дробная часть не нужна.

#### Практические сценарии использования
```
// код SSQC
const float EV_SEED = 15;

void(entity pl, float seed) SendRandomSeed =
{
	msg_entity = pl;
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, EV_SEED);
	WriteLong(MSG_MULTICAST, seed);
	multicast(pl.origin, MULTICAST_ONE_R);
};
```
```
// код CSQC
const float EV_SEED = 15;
float local_seed;

void() CSQC_Parse_Event =
{
	float ev = readbyte();
	if (ev == EV_SEED)
		local_seed = readlong();
};
```

### readangle
`float() readangle = #365;`

* **возвращаемое значение** — угол, декодированный из сетевого формата `WriteAngle`.

#### Описание и логика работы
`readangle` возвращает уже развёрнутый угол в градусах, но точность этого значения ограничена исходным сетевым кодированием. Поэтому его хорошо использовать для orientation hints, стартовых yaw/pitch, коротких анимационных импульсов. Для частой синхронизации плавной камеры лучше сравнить расход трафика между `WriteAngle` и `WriteFloat` и выбирать сознательно.

#### Практические сценарии использования
```
// код SSQC
const float EV_COMPASS = 16;

void(entity pl, float north_yaw) SendCompassYaw =
{
	msg_entity = pl;
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, EV_COMPASS);
	WriteAngle(MSG_MULTICAST, north_yaw);
	multicast(pl.origin, MULTICAST_ONE_R);
};
```
```
// код CSQC
const float EV_COMPASS = 16;
float compass_yaw;

void() CSQC_Parse_Event =
{
	float ev = readbyte();
	if (ev == EV_COMPASS)
		compass_yaw = readangle();
};
```

### readcoord
`float() readcoord = #364;`

* **возвращаемое значение** — координата, декодированная из формата `WriteCoord`.

#### Описание и логика работы
`readcoord` скрывает детали negotiated protocol: fixed-point это, floatcoords или другой совместимый wire format. Именно поэтому нельзя заменять его `readfloat`, даже если в вашей сборке сервер и клиент уже умеют `FTE_PEXT_FLOATCOORDS`. Для осей вектора вызывайте builtin по одному разу на каждую компоненту и соблюдайте исходный порядок записи.

#### Практические сценарии использования
```
// код SSQC
const float EV_SPAWNFX = 17;

void(entity pl, vector fxorg) SendSpawnFx =
{
	msg_entity = pl;
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, EV_SPAWNFX);
	WriteCoord(MSG_MULTICAST, fxorg_x);
	WriteCoord(MSG_MULTICAST, fxorg_y);
	WriteCoord(MSG_MULTICAST, fxorg_z);
	multicast(fxorg, MULTICAST_ONE_R);
};
```
```
// код CSQC
const float EV_SPAWNFX = 17;
vector spawn_fx_org;

void() CSQC_Parse_Event =
{
	float ev = readbyte();
	if (ev == EV_SPAWNFX)
		spawn_fx_org = [readcoord(), readcoord(), readcoord()];
};
```

### readfloat
`float() readfloat = #367;`

* **возвращаемое значение** — полный 32-битный float.

#### Описание и логика работы
`readfloat` используется только вместе с `WriteFloat`. Это самый предсказуемый вариант для передачи non-integer параметров между SSQC и CSQC, но и самый дорогой среди распространённых числовых форматов. Используйте его там, где важнее точность и простота, чем экономия трафика.

#### Практические сценарии использования
```
// код SSQC
const float EV_ZOOM = 18;

void(entity pl, float zoom) SendZoomValue =
{
	msg_entity = pl;
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, EV_ZOOM);
	WriteFloat(MSG_MULTICAST, zoom);
	multicast(pl.origin, MULTICAST_ONE_R);
};
```
```
// код CSQC
const float EV_ZOOM = 18;
float weapon_zoom;

void() CSQC_Parse_Event =
{
	float ev = readbyte();
	if (ev == EV_ZOOM)
		weapon_zoom = readfloat();
};
```

### readstring
`string() readstring = #366;`

* **возвращаемое значение** — null-terminated строка из потока сообщения.

#### Описание и логика работы
`readstring` читает строку, записанную `WriteString`. Это основной reader для текстовых payload-ов сервер → клиент: названия задач, имена режимов, пути к материалам, JSON-подобные компактные блобы. Как и у writer-стороны, сетевой расход зависит от длины строки, а интерпретация байтов зависит от того, одинаково ли сервер и клиент понимают кодировку.

#### Практические сценарии использования
```
// код SSQC
const float EV_CENTERHINT = 19;

void(entity pl, string hint) SendCenterHint =
{
	msg_entity = pl;
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, EV_CENTERHINT);
	WriteString(MSG_MULTICAST, hint);
	multicast(pl.origin, MULTICAST_ONE_R);
};
```
```
// код CSQC
const float EV_CENTERHINT = 19;
string center_hint;

void() CSQC_Parse_Event =
{
	float ev = readbyte();
	if (ev == EV_CENTERHINT)
		center_hint = readstring();
};
```

### readentitynum
`float() readentitynum = #368;`

* **возвращаемое значение** — server-side entity number.

#### Описание и логика работы
`readentitynum` — единственный корректный парный reader для `WriteEntity`. Он возвращает именно номер сущности, а не полноценный `entity`-handle CSQC. Это важно: клиент может получить номер раньше, чем успеет получить все поля сущности или вообще раньше, чем сущность попадёт в доступный CSQC-мир. Поэтому типичный паттерн — сохранить entnum и пытаться найти/проверить соответствующий объект позже.

#### Практические сценарии использования
```
// код SSQC
const float EV_TRACK = 20;

void(entity pl, entity target) SendTrackTarget =
{
	msg_entity = pl;
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, EV_TRACK);
	WriteEntity(MSG_MULTICAST, target);
	multicast(target.origin, MULTICAST_ONE_R);
};
```
```
// код CSQC
const float EV_TRACK = 20;
float track_target_entnum;

void() CSQC_Parse_Event =
{
	float ev = readbyte();
	if (ev == EV_TRACK)
		track_target_entnum = readentitynum();
};
```

### ReadPicture
`string() ReadPicture = #501;`

* **возвращаемое значение** — имя картинки, пригодное для `drawpic` и похожих 2D-функций.

#### Описание и логика работы
`ReadPicture` читает payload от `WritePicture`. Внутри FTEQW это чтение имени изображения, затем short-размера, затем пропуск самого blob-а. Так как `WritePicture` в FTEQW фактически посылает имя картинки и size `0`, builtin играет роль `readstring` с дополнительной download/precache-логикой: картинка может начать отображаться корректно только после догрузки ресурса, а до этого её размеры бывают неточными.

#### Практические сценарии использования
```
// код SSQC
const float EV_BADGE = 21;

void(entity pl) SendBadge =
{
	msg_entity = pl;
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, EV_BADGE);
	WritePicture(MSG_MULTICAST, "gfx/hud/badge_gold.tga", 0);
	multicast(pl.origin, MULTICAST_ONE_R);
};
```
```
// код CSQC
const float EV_BADGE = 21;
string badge_pic;

void() CSQC_Parse_Event =
{
	float ev = readbyte();
	if (ev == EV_BADGE)
		badge_pic = ReadPicture();
};
```

### multicast
`void(vector where, float set) multicast = #82;`

* **where** — точка мира для PHS/PVS-фильтрации; для `*_ONE*` и `*_ALL*` обычно игнорируется.
* **set** — режим из семейства `MULTICAST_*`.

#### Описание и логика работы
`multicast` не записывает данные сам по себе, а **диспатчит** уже заполненный буфер `MSG_MULTICAST`. Это ключевой builtin для селективной рассылки SSQC → клиенты, включая `SVC_CGAMEPACKET`. `MULTICAST_PHS` и `MULTICAST_PVS` режут аудиторию по слышимости/видимости точки `where`, `MULTICAST_ONE*` отправляют одному клиенту из `msg_entity`, `*_R` используют надёжный канал. Для custom CSQC-пакетов это основной безопасный путь, потому что сообщение не рвётся между пакетами так, как может рваться старый `MSG_ONE`.

#### Практические сценарии использования
```
// код SSQC
const float EV_EXPLODE = 22;

void(vector org, float radius) SendExplosionFx =
{
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, EV_EXPLODE);
	WriteCoord(MSG_MULTICAST, org_x);
	WriteCoord(MSG_MULTICAST, org_y);
	WriteCoord(MSG_MULTICAST, org_z);
	WriteShort(MSG_MULTICAST, radius);
	multicast(org, MULTICAST_PVS);
};
```
```
// код CSQC
const float EV_EXPLODE = 22;
vector explosion_org;
float explosion_radius;

void() CSQC_Parse_Event =
{
	float ev = readbyte();
	if (ev == EV_EXPLODE)
	{
		explosion_org = [readcoord(), readcoord(), readcoord()];
		explosion_radius = readshort();
	}
};
```

### stuffcmd
`void(entity client, string s, optional string s2, optional string s3, optional string s4, optional string s5, optional string s6, optional string s7) stuffcmd = #21;`

* **client** — клиент, которому отправляется команда.
* **s..s7** — части итоговой команды; в конце обязательно нужен `\n`.

#### Описание и логика работы
`stuffcmd` отправляет строку в клиентскую консоль на исполнение. Это мощный, но опасный механизм: команды зависят от конкретного клиента и легко ломают UX или безопасность мода. В отличие от custom `SVC_CGAMEPACKET`, тут вы фактически удалённо нажимаете команды в клиентском console buffer. Для массовой рассылки и demo-фильтров есть отдельный `stuffcmdflags`, но он в эту статью не входит. Если клиент перехватывает stuffcmd в `CSQC_Parse_StuffCmd`, можно построить более контролируемый протокол поверх текстовых команд.

#### Практические сценарии использования
```
// код SSQC
void(entity pl) SendHudWarn =
{
	stuffcmd(pl, "cmd hudwarn lowammo\n");
};
```
```
// код CSQC
float lowammo_warn_until;

void(string msg) CSQC_Parse_StuffCmd =
{
	tokenize(msg);
	if (argv(0) == "cmd" && argv(1) == "hudwarn" && argv(2) == "lowammo")
	{
		lowammo_warn_until = time + 1;
		return;
	}
};
```

### clientcommand
`void(entity e, string s) clientcommand = #440;`

* **e** — клиент, от имени которого будет выполнена команда.
* **s** — строка команды без сетевого round-trip.

#### Описание и логика работы
`clientcommand` на стороне SSQC выполняет команду так, как будто её прислал указанный клиент. В текущей реализации движок проверяет, что сущность действительно является подключённым клиентом, временно подменяет `host_client`/`sv_player` и вызывает разбор пользовательской команды. Это удобно для переиспользования уже существующего command parser-а, когда событие пришло не из консоли игрока, а из триггера, UI или скриптовой логики. Бuiltin не посылает сообщение на клиент — он выполняет серверную часть команды локально.

#### Практические сценарии использования
```
// код SSQC
float readycount;

void(entity pl) ForceReadyFromTrigger =
{
	clientcommand(pl, "ready");
};

void(string cmd) SV_ParseClientCommand =
{
	tokenize(cmd);
	if (argv(0) == "ready")
	{
		readycount = readycount + 1;
		return;
	}
};
```
```
// код CSQC
const float EV_READYCOUNT = 23;
float remote_readycount;

void() CSQC_Parse_Event =
{
	float ev = readbyte();
	if (ev == EV_READYCOUNT)
		remote_readycount = readshort();
};
```

### sendevent
`void(string evname, string evargs, ...) sendevent = #359;`

* **evname** — имя события.
* **evargs** — строка сигнатуры аргументов.
* **...** — до 6 аргументов, кодируемых по `evargs`.

#### Описание и логика работы
`Sendevent` — это путь **CSQC → SSQC**. Он вызывает на сервере функцию вида `CSEv_<evname>_<evargs>`. Документация перечисляет буквы `v`, `e`, `f`, `i`; текущая реализация движка также обрабатывает `s`, `p`, `F`, `u`, `I`, `U`. Для `e` отправляется `.entnum` сущности. Это не потоковый бинарный протокол как `Write*`, а готовый RPC-подобный вызов, поэтому он удобен для UI-кнопок, редакторов, локальных инструментов и прочих клиентских запросов к серверной логике.

#### Практические сценарии использования
```
// код SSQC
void CSEv_ping_fv(float seq, vector at)
{
	msg_entity = self;
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, 24);
	WriteLong(MSG_MULTICAST, seq);
	WriteCoord(MSG_MULTICAST, at_x);
	WriteCoord(MSG_MULTICAST, at_y);
	WriteCoord(MSG_MULTICAST, at_z);
	multicast(at, MULTICAST_ONE_R);
}
```
```
// код CSQC
void() SendPing =
{
	sendevent("ping", "fv", 1001, view_origin);
};

void() CSQC_Parse_Event =
{
	if (readbyte() == 24)
		last_ping_seq = readlong();
};
```

### sendpacket
`float(string destaddress, string content) sendpacket = #242;`

* **destaddress** — адрес вида `ip:port`, DNS-имя или другой формат, понятный движку.
* **content** — тело out-of-band UDP-пакета.

#### Описание и логика работы
`sendpacket` посылает connectionless UDP-пакет и автоматически добавляет в начало четыре байта `255`. Это тот же стиль, что у стандартных QuakeWorld packet-команд. Функция доступна и в SSQC, и в CSQC/MenuQC. На сервере входящие connectionless-пакеты может разбирать `SV_ParseConnectionlessPacket(sender, body)`. Возвращаемое значение — сетевой статус-код движка; при ошибке маршрута используется `NETERR_NOROUTE`.

#### Практические сценарии использования
```
// код SSQC
float(string sender, string body) SV_ParseConnectionlessPacket =
{
	if (body == "mod_ping")
	{
		sendpacket(sender, "mod_pong");
		return 1;
	}
	return 0;
};
```
```
// код CSQC
float ping_sent;

void() QueryServerOutOfBand =
{
	ping_sent = sendpacket("127.0.0.1:27500", "mod_ping");
};
```

### deltalisten
`float(string modelname, float(float isnew) updatecallback, float flags) deltalisten = #371;`

* **modelname** — имя модели, за обновлениями которой надо следить, либо `"*"` для всех modelindex.
* **updatecallback** — функция CSQC, вызываемая на каждом delta-update; аргумент `isnew` сообщает, новая это сущность или уже известная.
* **flags** — режимные биты обработки delta-сущности.

#### Описание и логика работы
`deltalisten` относится к CSQC и позволяет подписаться на сетевые entity updates, которые обычно обслуживает сам движок. В callback движок уже обновил стандартные поля сущности и, если flags не запрещают это, выполнил обычную интерполяцию. По коду реализации видно, что flags влияют в том числе на lerp/trails/light-поведение listened-сущностей. Это полезно, когда вы хотите перехватить сетевые обновления определённой серверной модели без перевода её в полноценный `SendEntity`-протокол.

#### Практические сценарии использования
```
// код SSQC
void() worldspawn =
{
	precache_model("progs/flag.mdl");
};

void() SpawnArenaFlag =
{
	entity e = spawn();
	setmodel(e, "progs/flag.mdl");
	e.origin = '512 256 64';
};
```
```
// код CSQC
float(float isnew) FlagDelta =
{
	if (isnew)
		self.alpha = 0.5;
	return 1;
};

void() CSQC_Init =
{
	deltalisten("progs/flag.mdl", FlagDelta, 0);
};
```

### netaddress_resolve
`string(string dnsname, optional float defport) netaddress_resolve = #625;`

* **dnsname** — DNS-имя или строковый адрес для разрешения.
* **defport** — порт по умолчанию, если в строке адреса его нет.

#### Описание и логика работы
`netaddress_resolve` синхронно вызывает системное разрешение адреса и возвращает первый результат строкой в формате движка. В серверной таблице builtin-ов прямо отмечено, что lookup блокирующий. При неудаче возвращается пустая строка. Функция полезна как подготовительный шаг перед `sendpacket`, особенно если адрес задаётся пользователем или cvar-ом, а не жёстко зашит как IP.

#### Практические сценарии использования
```
// код SSQC
void() ReportMatchStart =
{
	string addr = netaddress_resolve("stats.example.org", 27910);
	if (addr != "")
		sendpacket(addr, "match_started");
};
```
```
// код CSQC
void() ResolveRelay =
{
	string addr = netaddress_resolve("127.0.0.1", 27500);
	if (addr != "")
		sendpacket(addr, "mod_ping");
};
```

### getextresponse
`string() getextresponse = #624;`

* **возвращаемое значение** — строка ответа подсистемы server browser.

#### Описание и логика работы
`getextresponse` относится к browser/server-list API, но текущая реализация FTEQW фактически является stub: и на клиентской стороне, и в серверной таблице builtin отмечен как пустой/заглушка. Практически это означает, что рассчитывать на содержательный ответ не стоит: обычно вы получите пустую строку. Документировать builtin всё равно полезно, чтобы было понятно, почему код старых модов ничего из него не извлекает.

#### Практические сценарии использования
```
// код MenuQC или CSQC
string browser_reply;

void() PollBrowserReply =
{
	browser_reply = getextresponse();
	if (browser_reply == "")
		browser_reply = "no pending extension response";
};
```
```
// код SSQC
float(string sender, string body) SV_ParseConnectionlessPacket =
{
	// getextresponse не участвует в обработке server-side пакета напрямую.
	return 0;
};
```

### redirectcmd
`DEP void(entity to, string str) redirectcmd = #101;`

* **to** — клиент, которому будет переслан текстовый вывод команды.
* **str** — одна консольная команда.

#### Описание и логика работы
`redirectcmd` выполняет серверную консольную команду и перенаправляет её текстовый вывод указанному клиенту. Реализация откладывает выполнение до конца кадра, когда QuakeC уже не исполняется, чтобы безопаснее переживать map change и похожие случаи. Это deprecated-инструмент, но он всё ещё полезен для административных справок, удалённого `status` и диагностических отчётов. В отличие от `stuffcmd`, клиент ничего не исполняет — он лишь получает текстовый вывод.

#### Практические сценарии использования
```
// код SSQC
void(entity admin) ShowServerStatus =
{
	redirectcmd(admin, "status");
};
```
```
// код CSQC
// redirectcmd не даёт structured packet: клиент просто увидит текст в своей консоли.
void(string msg) CSQC_Parse_StuffCmd =
{
};
```

### isserver
`float() isserver = #60;`
`float() isserver = #350;`

* **возвращаемое значение** — 0, 0.5 или 1 в зависимости от локального режима.

#### Описание и логика работы
Имя одно, но builtin встречается в двух контекстах: MenuQC (`#60`) и CSQC (`#350`). В обоих случаях смысл одинаковый — может ли локальная консоль напрямую влиять на сервер. Для dedicated/чистого remote client возвращается `0`. Для listen/single-player код движка возвращает `0.5`, а для полноценного локального многопользовательского сервера — `1`. То есть проверять нужно не на `== 1`, а на `> 0`, если вам важен сам факт локального доступа.

#### Практические сценарии использования
```
// код MenuQC или CSQC
float can_edit_server;

void() RefreshLocalServerFlag =
{
	can_edit_server = (isserver() > 0);
};
```
```
// код SSQC
void(entity pl) SendSaveAllowed =
{
	if (pl.classname == "player")
		stuffcmd(pl, "echo local server available\n");
};
```

### clientcount
`float() clientcount = #61;`

* **возвращаемое значение** — максимальное число клиентских слотов локального сервера.

#### Описание и логика работы
`clientcount` нужен прежде всего MenuQC и имеет смысл только тогда, когда движок уже держит локальный сервер. Реализация на клиентской стороне возвращает `sv.allocated_client_slots`, а без локального сервера — `0`. Это не число реально подключённых игроков, а именно размер серверного client array, поэтому builtin полезен для UI, lobby-логики и диагностики listen server-а.

#### Практические сценарии использования
```
// код MenuQC
float local_slots;

void() UpdateLobbySlots =
{
	local_slots = clientcount();
};
```
```
// код SSQC
void(entity pl) SendLobbyInfo =
{
	stuffcmd(pl, "echo lobby slots updated\n");
};
```

### clientstate
`float() clientstate = #62;`

* **возвращаемое значение** — `0` для dedicated, `1` для idle/disconnected, `2` для connecting или connected.

#### Описание и логика работы
`clientstate` — клиентский builtin из MenuQC. Он показывает, находится ли локальный движок вне игры, пытается ли подключиться или уже имеет соединение. Реализация считает значением `2` не только полноценное подключение, но и промежуточные состояния connect/loopback-start, что полезно для корректного отображения меню «Connecting…». Это не статус конкретного игрока на сервере, а именно состояние локального клиента.

#### Практические сценарии использования
```
// код MenuQC
float menu_state;

void() TickMainMenu =
{
	menu_state = clientstate();
};
```
```
// код SSQC
void(entity pl) GreetNewPlayer =
{
	stuffcmd(pl, "echo welcome\n");
};
```

### clienttype
`float(entity client) clienttype = #455;`

* **client** — сущность, которую нужно проверить.

#### Описание и логика работы
`clienttype` работает в SSQC и различает четыре случая: `CLIENTTYPE_DISCONNECTED` (`0`), `CLIENTTYPE_REAL` (`1`), `CLIENTTYPE_BOT` (`2`) и `CLIENTTYPE_NOTACLIENT` (`3`). Это удобно при рассылке сообщений и выполнении логики, которую нельзя применять к ботам или пустым слотам. Реализация движка прямо отмечает, что любые сообщения для bot-clients будут игнорироваться, поэтому `clienttype` полезен как ранняя проверка перед `stuffcmd`, `MSG_ONE` или крупными reliable-пакетами.

#### Практические сценарии использования
```
// код SSQC
void(entity pl) SendOnlyToHumans =
{
	if (clienttype(pl) != CLIENTTYPE_REAL)
		return;

	msg_entity = pl;
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, 25);
	multicast(pl.origin, MULTICAST_ONE_R);
};
```
```
// код CSQC
void() CSQC_Parse_Event =
{
	if (readbyte() == 25)
		human_only_banner = time + 2;
};
```

### isdemo
`float() isdemo = #349;`

* **возвращаемое значение** — `0` вне demo, `1` при обычном demo playback, `2` при MVD playback.

#### Описание и логика работы
`isdemo` — клиентский builtin для CSQC/MenuQC. В отличие от типичного boolean API, FTEQW различает обычную запись и MVD: при MVD возвращается `2`, что важно для интерфейсов наблюдателя и функций, умеющих работать с несколькими игроками. Это удобный флажок для UI, отладки и отключения клиентских команд, которые не должны работать в playback-режиме.

#### Практические сценарии использования
```
// код CSQC
float demo_mode;

void() UpdateReplayUi =
{
	demo_mode = isdemo();
};
```
```
// код SSQC
void(entity pl) SendLiveOnlyObjective =
{
	stuffcmd(pl, "echo objective updated\n");
};
```

### isbackbuffered
`float(entity player) isbackbuffered = #234;`

* **player** — клиент, для которого проверяется сетевой backlog.

#### Описание и логика работы
`isbackbuffered` — серверный диагностический builtin. Он возвращает ненулевое значение, если у клиента уже накопились backbuffer-данные и их очистка займёт несколько сетевых кадров. Это прямой сигнал снизить объём новых reliable-сообщений: отложить `stuffcmd`, уменьшить поток `MSG_ALL`/`MSG_ONE`, заменить крупные тексты на компактные коды. Особенно полезно перед отправкой больших custom CSQC-пакетов или серий console commands.

#### Практические сценарии использования
```
// код SSQC
void(entity pl, string text) SafeSendHint =
{
	if (isbackbuffered(pl))
		return;

	msg_entity = pl;
	WriteByte(MSG_MULTICAST, SVC_CGAMEPACKET);
	WriteByte(MSG_MULTICAST, 26);
	WriteString(MSG_MULTICAST, text);
	multicast(pl.origin, MULTICAST_ONE_R);
};
```
```
// код CSQC
string queued_hint;

void() CSQC_Parse_Event =
{
	if (readbyte() == 26)
		queued_hint = readstring();
};
```

## Смежные страницы

- [Client-Side QuakeC (CSQC)](../16-quakec-scripting/client-side-quakec-csqc.md)
- [Сетевые протоколы и мультиплеер](../24-network-protocols-multiplayer/README.md)
- [Индекс справочника builtins](./README.md)
