# Ввод, интерфейс и клавиатура CSQC

> [⬅ Вернуться к оглавлению вики](../README.md)

> [Индекс справочника builtins](./README.md)

Этот раздел собран для builtins CSQC, которые управляют вводом, курсором, привязками клавиш и встроенным браузером на стороне клиента. Для CSQC особенно важны две системы движка: **key destination** определяет, кто сейчас получает клавиатуру (`game`, `console`, `menu`), а **bindmaps** позволяют держать несколько независимых наборов биндов и переключать активную раскладку без переписывания `bind`-строк. Часть перечисленных ниже функций также зарегистрирована в MenuQC — им посвящена отдельная статья [Функции MenuQC](./13-menuqc-builtins.md).

## Функции

### getinputstate
`float(float inputsequencenum) getinputstate = #345;`

* **inputsequencenum** — номер input-frame из клиентского журнала команд, который нужно загрузить в `input_*` globals.

#### Описание и логика работы
`getinputstate` доступна только в CSQC и нужна прежде всего для prediction-кода. Она берёт сохранённый кадр пользовательского ввода, копирует его в глобалы `input_timelength`, `input_angles`, `input_movevalues`, `input_buttons`, `input_impulse` и возвращает ненулевое значение при успехе. По комментариям FTEQW нормальный рабочий диапазон — `servercommandframe < sequence <= clientcommandframe`; при паузе, неверном `seat` или отсутствующем кадре builtin вернёт `0` и не даст полагаться на несуществующее состояние.

#### Практические сценарии использования
```
void() CSQC_ReplayLatestInput =
{
	local float seq;

	seq = clientcommandframe;
	if (!getinputstate(seq))
		return;

	if (input_buttons & 1)
		dprint("attack is held on the newest predicted input\n");

	dprint(sprintf("move %g %g %g for %g ms\n",
		input_movevalues_x,
		input_movevalues_y,
		input_movevalues_z,
		input_timelength));
};
```

### getkeybind
`string(float keynum) getkeybind = #342;`

* **keynum** — qscancode/Quake key number, для которого нужно получить текущую привязку.

#### Описание и логика работы
`getkeybind` доступна в CSQC и MenuQC. Базовое объявление показывает только `keynum`, а текущая реализация FTEQW дополнительно понимает необязательные `bindmap` и `modifier`; без них возвращается обычная команда для нажатия без модификаторов. Если номер клавиши неизвестен, не поддерживается или на неё ничего не назначено, builtin возвращает пустую строку.

#### Практические сценарии использования
```
string() DescribeUseKey =
{
	local float key;
	local string bind;

	key = stringtokeynum("e");
	if (key < 0)
		return "key E is not recognised";

	bind = getkeybind(key);
	if (bind == "")
		return "E is currently unbound";

	return "E => " + bind;
};
```

### setkeybind
`float(float key, string bind, optional float bindmap, optional float modifier) setkeybind = #630;`

* **key** — qscancode/Quake key number, который нужно переназначить.
* **bind** — строка команды, которая будет выполняться при нажатии.
* **bindmap** — необязательный номер альтернативной bindmap; `0` означает обычный базовый bind.
* **modifier** — необязательная маска модификатора для прямой записи варианта вроде `Ctrl+K`; если указан `bindmap`, движок использует bindmap и игнорирует modifier.

#### Описание и логика работы
`setkeybind` доступна в CSQC и MenuQC и позволяет менять привязки без генерации текстовой команды `bind`. Для положительного `bindmap` запись идёт не в основной bind-slot, а в одну из альтернативных карт биндов, которые потом включаются через `setbindmaps`. Несмотря на сигнатуру с `float`-возвратом, текущая реализация ведёт себя как командный builtin и не даёт полезного результата для проверки успеха; если важна верификация, сразу перечитайте значение через `getkeybind`.

#### Практические сценарии использования
```
void() Menu_BindQuickSave =
{
	local float key;

	key = stringtokeynum("F6");
	if (key < 0)
		return;

	// Обычный bind в основной карте.
	setkeybind(key, "save quick");
};
```

### getkeydest
`float() getkeydest = #602;`

* Аргументов нет.

#### Описание и логика работы
`getkeydest` существует в MenuQC и сообщает, куда сейчас направлена клавиатура для этого слоя UI. Система key destination в движке различает как минимум `game`, `console` и `menu`, но именно этот builtin в FTEQW практически оперирует двумя устойчивыми значениями: `0` для игрового режима и `2` для MenuQC-меню. Консоль как destination существует на уровне движка и может быть открыта поверх игры или меню, однако через `getkeydest`/`setkeydest` на неё нельзя надёжно переключаться как на отдельный режим, поэтому MenuQC-коду лучше трактовать значение как «мы в меню» или «мы не в меню».

#### Практические сценарии использования
```
float() Menu_IsCapturingKeyboard =
{
	if (getkeydest() == 2)
		return 1;
	return 0;
};
```

### setkeydest
`void(float dest) setkeydest = #601;`

* **dest** — целевой destination для клавиатуры: практически используйте `0` для возврата в игру и `2` для передачи фокуса MenuQC.

#### Описание и логика работы
`setkeydest` доступна только в MenuQC. Значение `2` поднимает/удерживает MenuQC как активное меню, а `0` убирает MenuQC-фокус и возвращает ввод игровому слою. Историческое промежуточное значение для message/console-пути в текущем FTEQW не является рабочим публичным режимом: при попытке передать неподдерживаемое число движок сообщает об ошибке builtin.

#### Практические сценарии использования
```
void() Menu_CloseCurrentScreen =
{
	if (getkeydest() == 2)
		setkeydest(0); // закрываем MenuQC и возвращаем клавиатуру игре
};
```

### getbindmaps
`vector() getbindmaps = #631;`

* Аргументов нет.

#### Описание и логика работы
`getbindmaps` доступна в CSQC и MenuQC и возвращает в `x` и `y` два активных альтернативных bindmap-слота, которые движок использует как fallback после обычных биндов. Это не список всех существующих карт, а именно текущая пара «подключённых» раскладок: `0` означает, что слот отключён, положительные числа обозначают независимые наборы привязок. Такой механизм удобен для переключаемых схем оружия, транспорта, режимов редактора и других контекстов, где одна и та же клавиша должна подхватывать другой набор команд без переписывания основной конфигурации.

#### Практические сценарии использования
```
vector active_maps;

void() HUD_PrintBindmapState =
{
	active_maps = getbindmaps();
	dprint(sprintf("bindmaps: primary=%g secondary=%g\n", active_maps_x, active_maps_y));
};
```

### setbindmaps
`float(vector bm) setbindmaps = #632;`

* **bm** — вектор, где `x` и `y` задают два активных bindmap-слота, а `z` игнорируется.

#### Описание и логика работы
`setbindmaps` доступна в CSQC и MenuQC и переключает активную пару альтернативных bindmap-слоёв. FTEQW нормализует некорректные значения: положительные допустимые номера включаются, всё остальное превращается в `0`, то есть слот отключается. В отличие от `setkeybind`, здесь возвращаемое значение осмысленно: текущая реализация записывает неноль после применения запроса.

#### Практические сценарии использования
```
void(float use_alt_layout) HUD_SelectWeaponBindmap =
{
	local vector maps;

	maps = '0 0 0';
	if (use_alt_layout)
		maps_x = 2; // первая альтернативная карта активна
	else
		maps_x = 1; // обычная пользовательская карта оружия

	if (setbindmaps(maps))
		dprint(sprintf("weapon bindmap switched to %g\n", maps_x));
};
```

### getmousepos
`vector() getmousepos = #66;`
`vector() getmousepos = #344;`

* Аргументов нет.

#### Описание и логика работы
Имя `getmousepos` существует в двух вариантах: старый MenuQC builtin `#66` и CSQC builtin `#344`. Оба считаются историческими и в документации движка прямо сопровождаются рекомендацией опираться на `*_InputEvent`, потому что возвращаемое значение зависит от текущего cursor mode: в абсолютном режиме это позиция курсора, а в relative/delta-режиме — накопленное смещение с последнего чтения. Для некурсорных сценариев, особенно в CSQC, прямое чтение дельт через `CSQC_InputEvent` надёжнее; кроме того, само чтение `getmousepos` сбрасывает накопленные дельты.

#### Практические сценарии использования
```
vector ui_mouse;

void() Menu_UpdateCursorHotspot =
{
	setcursormode(1);
	ui_mouse = getmousepos();

	if (ui_mouse_x >= 320 && ui_mouse_y >= 200)
		dprint("cursor is over the lower-right half of the menu\n");
};
```

### setmousetarget
`void(float trg) setmousetarget = #603;`

* **trg** — режим маршрутизации мыши: `1` для relative/delta ввода, `2` для absolute cursor ввода.

#### Описание и логика работы
`setmousetarget` доступна в CSQC и MenuQC и переключает, должен ли текущий модуль получать мышь как движение-дельту или как абсолютный экранный курсор. Значение `1` подходит для игровых оверлеев и сценариев, где вы сами интерпретируете смещения, а `2` — для интерфейсов с кнопками, drag-and-drop и встроенными веб-виджетами. Любое иное значение считается ошибкой builtin.

#### Практические сценарии использования
```
void(float panel_open) HUD_SetInventoryMouseMode =
{
	if (panel_open)
	{
		setcursormode(1);
		setmousetarget(2); // абсолютный курсор для UI
	}
	else
	{
		setmousetarget(1); // снова относительные дельты
		setcursormode(0);
	}
};
```

### getmousetarget
`float() getmousetarget = #604;`

* Аргументов нет.

#### Описание и логика работы
`getmousetarget` доступна в CSQC и MenuQC и возвращает текущее состояние той же системы, что переключает `setmousetarget`: `1` означает relative/delta поток, `2` — absolute cursor поток. Это удобно, когда один и тот же код должен корректно работать и в геймплейном HUD, и в полноценных окнах интерфейса, не предполагая заранее, какой режим оставил предыдущий экран.

#### Практические сценарии использования
```
string() HUD_DescribeMouseRouting =
{
	if (getmousetarget() == 2)
		return "mouse is feeding absolute UI coordinates";
	return "mouse is feeding relative deltas";
};
```

### setcursormode
`void(float usecursor, optional string cursorimage, optional vector hotspot, optional float scale) setcursormode = #343;`

* **usecursor** — `1`, если модулю нужен освобождённый видимый курсор; `0`, если мышь должна быть захвачена движком как обычный игровой relative input.
* **cursorimage** — необязательное имя изображения курсора; пустая строка снимает ранее заданное кастомное изображение.
* **hotspot** — необязательная точка активного клика внутри картинки курсора.
* **scale** — необязательный масштаб курсора.

#### Описание и логика работы
`setcursormode` доступна в CSQC и MenuQC и просит движок либо отпустить мышь в абсолютный режим, либо снова захватить её для игры. Если указан `cursorimage`, FTEQW пытается использовать его как курсор без конфликтов с консолью; при поддержке платформы это может быть hardware cursor, а при отсутствии поддержки движок рисует программную эмуляцию. Практически `setcursormode(1, ...)` часто сочетают с `setmousetarget(2)` и чтением абсолютных координат, а `setcursormode(0)` — с возвратом к HUD без свободного указателя.

#### Практические сценарии использования
```
void() Menu_EnablePointer =
{
	// Освобождаем мышь и ставим собственный курсор с hotspot в левом верхнем углу.
	setcursormode(1, "gfx/ui/cursor.tga", '0 0 0', 1);
};
```

### setsensitivityscaler
`void(float sens) setsensitivityscaler = #346;`

* **sens** — временный множитель мышиной чувствительности для текущего клиента.

#### Описание и логика работы
`setsensitivityscaler` существует только в CSQC. Она не переписывает пользовательский cvar чувствительности, а именно временно масштабирует итоговую мышиную чувствительность на стороне клиента, что удобно для зума, турелей, биноклей и похожих режимов. Если вам нужно вернуть обычное поведение, просто снова вызовите builtin с `1`.

#### Практические сценарии использования
```
void(float scoped) CSQC_UpdateZoomSensitivity =
{
	if (scoped)
		setsensitivityscaler(0.35);
	else
		setsensitivityscaler(1);
};
```

### keynumtostring
`string(float keynum) keynumtostring = #340;`
`string(float keynum) keynumtostring = #609;`

* **keynum** — qscancode/Quake key number, который нужно отобразить человеку.

#### Описание и логика работы
Под именем `keynumtostring` скрываются две родственные точки входа: CSQC-версия `#340` и MenuQC-версия `#609`. Обе возвращают человекочитаемое имя клавиши в том же стиле, что и консольная команда `bind`; для печатных ASCII-клавиш это обычно сам символ, для специальных — имена вроде `SPACE`, `ENTER`, `MOUSE1`. Для отрицательных или нераспознанных кодов движок старается вернуть диагностическую строку вместо падения, но переносимый QuakeC-код всё равно должен считать такие значения признаком ошибки.

#### Практические сценарии использования
```
string(float keynum) UI_FormatKeyLabel =
{
	local string name;

	name = keynumtostring(keynum);
	if (name == "")
		return "<unbound>";
	return name;
};
```

### keynumtostring_csqc
`string(float keynum) keynumtostring_csqc = #340;`

* **keynum** — код клавиши, который MenuQC-код хочет представить в CSQC-совместимом builtin-слоте.

#### Описание и логика работы
`keynumtostring_csqc` — deprecated MenuQC alias для CSQC builtin `#340`. Практической новой функциональности он не даёт: это тот же перевод `keynum -> readable bind name`, оставленный ради совместимости со старыми menu.dat, которые ожидали CSQC-номер builtin. Если вы пишете новый код только под FTEQW, обычно проще использовать обычный `keynumtostring` MenuQC-ветки.

#### Практические сценарии использования
```
string() Menu_ShowLegacyConfirmKey =
{
	local float key;

	key = stringtokeynum_csqc("ENTER");
	return "Press " + keynumtostring_csqc(key) + " to confirm";
};
```

### keynumtostring_menu
`string(float keynum) keynumtostring_menu = #609;`

* **keynum** — код клавиши, который CSQC-код хочет пропустить через MenuQC-совместимый alias.

#### Описание и логика работы
`keynumtostring_menu` — CSQC alias для MenuQC builtin `#609`. Он полезен в переносимом коде, который переиспользует старые MenuQC helper-функции внутри CSQC/HUD слоя и хочет сохранить прежние имена вызовов. Поведение такое же, как у обычного `keynumtostring`: modifiers отдельно не кодируются, а возвращается базовое имя клавиши.

#### Практические сценарии использования
```
string() HUD_ShowMenuStyleEscapeName =
{
	local float key;

	key = stringtokeynum("ESCAPE");
	return keynumtostring_menu(key);
};
```

### keynumtostring_omgwtf
`string(float keynum) keynumtostring_omgwtf = #520;`

* **keynum** — код клавиши для legacy CSQC alias.

#### Описание и логика работы
`keynumtostring_omgwtf` — ещё один deprecated alias к той же базовой логике преобразования keynum в строку. Никаких дополнительных возможностей у него нет; имя историческое, оставленное только ради совместимости с существующим QuakeC. В новой документации и новом коде разумнее считать его переходным синонимом `keynumtostring`.

#### Практические сценарии использования
```
string() HUD_LegacyKeyName =
{
	local float key;

	key = stringtokeynum("TAB");
	return keynumtostring_omgwtf(key);
};
```

### stringtokeynum
`float(string keyname) stringtokeynum = #341;`
`float(string key) stringtokeynum = #614;`

* **keyname/key** — текстовое имя клавиши в формате, совместимом с командой `bind`.

#### Описание и логика работы
Как и `keynumtostring`, builtin `stringtokeynum` существует в двух основных слотах: CSQC `#341` и MenuQC `#614`. Он превращает имя клавиши в qscancode/Quake key number, понимая те же имена, что и консольный `bind`, включая `K_*`-синонимы и одиночные символы. Важно, что публичный builtin рассчитан именно на базовую клавишу: строки с модификаторами вроде `Ctrl+K` в FTEQW не считаются переносимым успешным вводом для этой функции и обычно приводят к `-1`.

#### Практические сценарии использования
```
float() UI_FindPauseKey =
{
	local float key;

	key = stringtokeynum("PAUSE");
	if (key < 0)
		return stringtokeynum("ESCAPE");
	return key;
};
```

### stringtokeynum_csqc
`float(string keyname) stringtokeynum_csqc = #341;`

* **keyname** — имя клавиши для MenuQC-кода, который ожидает старый CSQC builtin-номер.

#### Описание и логика работы
`stringtokeynum_csqc` — deprecated MenuQC alias к CSQC-варианту `stringtokeynum`. Он нужен прежде всего для совместимости со старыми menu.dat/портами, где helper-код жёстко ссылался на builtin `#341`. Поведение такое же: обычные имена клавиш распознаются, а modifier-комбинации не стоит использовать как вход этой функции.

#### Практические сценарии использования
```
float() Menu_LegacyAcceptKey =
{
	return stringtokeynum_csqc("ENTER");
};
```

### stringtokeynum_menu
`float(string key) stringtokeynum_menu = #614;`

* **key** — имя клавиши для CSQC-кода, который хочет звать MenuQC-совместимый слот.

#### Описание и логика работы
`stringtokeynum_menu` — CSQC alias к MenuQC builtin `#614`. Он особенно полезен, когда один и тот же helper-файл подключается и в MenuQC, и в CSQC, а вы хотите оставить исходное имя вызова из menu-ветки. Как и сам MenuQC-вариант, builtin ориентирован на базовые key names из `bind`; `ctrl/shift/alt` как часть текста здесь не нужно считать надёжно поддержанным API.

#### Практические сценарии использования
```
float() HUD_MenuStyleBackKey =
{
	return stringtokeynum_menu("ESCAPE");
};
```

### findkeysforcommand
`string(string command, optional float bindmap) findkeysforcommand = #521;`
`string(string command, optional float bindmap) findkeysforcommand = #610;`

* **command** — точная bind-строка, для которой нужно найти назначенные клавиши.
* **bindmap** — необязательный номер bindmap; историческая документация legacy CSQC-слота советует не полагаться на него как на полностью переносимый фильтр.

#### Описание и логика работы
`findkeysforcommand` доступна и в CSQC, и в MenuQC, но исторически существует в двух слотах: deprecated CSQC `#521` и menu/shared `#610`. Она возвращает строку-список ключей, которую следует разбирать через `tokenize`, а не обычным сравнением одной строки. В legacy документации подчёркнуто, что формат несовместим с `tokenize_console`, что модификаторы не поддерживаются и что пустой результат означает отсутствие биндов; для современного FTEQW также полезно помнить, что новый код с modifier-aware поиском лучше строить вокруг `findkeysforcommandex`, если вы можете требовать его наличие.

#### Практические сценарии использования
```
string() UI_FirstJumpKey =
{
	local string raw;

	raw = findkeysforcommand("+jump");
	if (tokenize(raw) <= 0)
		return "<not bound>";

	if (argv(0) == "-1")
		return "<not bound>";

	return keynumtostring(stof(argv(0)));
};
```

### gecko_create
`float(string name, optional string initialURI) gecko_create = #487;`

* **name** — имя shader/texture-слота, через который браузерная вкладка потом рисуется в интерфейсе.
* **initialURI** — необязательный стартовый URI; если не указан, движок создаёт браузер с начальным `http:`-адресом/пустым состоянием загрузчика.

#### Описание и логика работы
`gecko_create` доступна в CSQC и MenuQC и создаёт встроенную браузерную поверхность, которую затем можно рисовать как обычный 2D shader через `drawpic`. По документации FTEQW имя не должно конфликтовать с уже существующим shader или реальным ресурсом на диске. Главное ограничение — builtin зависит от внешнего browser/media plugin: при его отсутствии создание вернёт `0`, и никакая последующая `gecko_*`-логика не должна считаться активной.

#### Практические сценарии использования
```
float browser_ready;

void() HelpBrowser_Init =
{
	browser_ready = gecko_create("browser/help", "ui/help/index.html");
	if (browser_ready)
		gecko_resize("browser/help", 1024, 768);
};
```

### gecko_destroy
`void(string name) gecko_destroy = #488;`

* **name** — имя браузерного shader, созданного через `gecko_create`.

#### Описание и логика работы
`gecko_destroy` доступна в CSQC и MenuQC и освобождает созданную браузерную поверхность. Если shader с таким именем не найден, builtin просто ничего не делает, так что дополнительная защитная проверка обычно не обязательна. Вызывать её полезно при закрытии тяжёлых меню, смене экранов или выгрузке интерфейса, чтобы не держать лишний browser instance в памяти.

#### Практические сценарии использования
```
void() HelpBrowser_Shutdown =
{
	gecko_destroy("browser/help");
};
```

### gecko_navigate
`void(string name, string URI) gecko_navigate = #489;`

* **name** — имя браузерного shader.
* **URI** — адрес, команда или специальная строка для встроенного браузера.

#### Описание и логика работы
`gecko_navigate` доступна в CSQC и MenuQC и отправляет браузеру команду навигации. Для обычных сценариев это URL или путь к странице; комментарии FTEQW отдельно отмечают специальные команды вида `cmd:focus` и `cmd:unfocus`, которыми интерфейс может явно отдавать встроенной странице клавиатурный фокус. Если браузер не создан или плагин отсутствует, вызов тихо ничего не меняет.

#### Практические сценарии использования
```
void() HelpBrowser_OpenNews =
{
	gecko_navigate("browser/help", "https://example.org/news.html");
	gecko_navigate("browser/help", "cmd:focus");
};
```

### gecko_keyevent
`float(string name, float key, float eventtype, optional float charcode) gecko_keyevent = #490;`

* **name** — имя браузерного shader.
* **key** — keynum/scan code, который нужно передать браузеру.
* **eventtype** — тип события клавиатуры; обычно сюда передают значение `IE_KEYDOWN` или `IE_KEYUP` из `*_InputEvent`.
* **charcode** — необязательный Unicode-символ; если его не передать, движок попытается использовать ASCII-код для простых клавиш.

#### Описание и логика работы
`gecko_keyevent` доступна в CSQC и MenuQC и пересылает key event во встроенный браузер. Возвращаемое ненулевое значение означает, что событие было принято браузерным слоем, а не потеряно из-за отсутствующего shader/plugin. Это основной мост между `CSQC_InputEvent`/`Menu_InputEvent` и HTML-формами, полями ввода, клавиатурной навигацией по странице.

#### Практические сценарии использования
```
float browser_focus;

float(float evtype, float scanx, float chary, float devid) CSQC_InputEvent =
{
	if (!browser_focus)
		return 0;

	if (evtype == IE_KEYDOWN || evtype == IE_KEYUP)
		return gecko_keyevent("browser/help", scanx, evtype, chary);

	return 0;
};
```

### gecko_mousemove
`void(string name, float x, float y) gecko_mousemove = #491;`

* **name** — имя браузерного shader.
* **x** — горизонтальная координата внутри браузерной поверхности в диапазоне `0..1`.
* **y** — вертикальная координата внутри браузерной поверхности в диапазоне `0..1`.

#### Описание и логика работы
`gecko_mousemove` доступна в CSQC и MenuQC и сообщает встроенному браузеру положение указателя относительно самой веб-поверхности, а не всего экрана. Поэтому обычно сначала переводят экранные координаты курсора в локальные UV-координаты прямоугольника, где рисуется страница. Вне диапазона `0..1` поведение зависит от плагина и layout страницы, так что безопаснее ограничивать значения самим.

#### Практические сценарии использования
```
vector browser_origin;
vector browser_size;

void() HelpBrowser_UpdateMouse =
{
	local vector mouse;
	local float u;
	local float v;

	mouse = getmousepos();
	u = (mouse_x - browser_origin_x) / browser_size_x;
	v = (mouse_y - browser_origin_y) / browser_size_y;

	if (u < 0 || v < 0 || u > 1 || v > 1)
		return;

	gecko_mousemove("browser/help", u, v);
};
```

### gecko_resize
`void(string name, float w, float h) gecko_resize = #492;`

* **name** — имя браузерного shader.
* **w** — желаемая ширина браузерной поверхности в пикселях.
* **h** — желаемая высота браузерной поверхности в пикселях.

#### Описание и логика работы
`gecko_resize` доступна в CSQC и MenuQC и запрашивает у браузерного движка новый размер рендер-буфера. Это влияет не только на чёткость картинки, но и на layout страницы, потому что HTML/CSS пересчитываются под новый viewport. Обычно builtin вызывают при открытии окна, изменении разрешения UI или переключении между компактным и полноэкранным режимом одного и того же встроенного браузера.

#### Практические сценарии использования
```
void(float fullscreen) HelpBrowser_Resize =
{
	if (fullscreen)
		gecko_resize("browser/help", 1920, 1080);
	else
		gecko_resize("browser/help", 960, 540);
};
```

### gecko_get_texture_extent
`vector(string name) gecko_get_texture_extent = #493;`

* **name** — имя браузерного shader, размеры которого нужно запросить.

#### Описание и логика работы
`gecko_get_texture_extent` доступна в CSQC и MenuQC и возвращает вектор, где `x` и `y` содержат текущие пиксельные размеры браузерной текстуры, а `z` — дополнительное значение aspect ratio, которое сообщает media/browser backend. Если браузер ещё не создан, плагин отсутствует или размер пока не известен, безопасно ожидать нули. Эта функция полезна для диагностики, подстройки `drawpic`-прямоугольника и для ожидания момента, когда страница действительно инициализировала рендер-буфер.

#### Практические сценарии использования
```
vector browser_extent;

void() HelpBrowser_DebugExtent =
{
	browser_extent = gecko_get_texture_extent("browser/help");
	dprint(sprintf("browser texture: %g x %g, aspect %g\n",
		browser_extent_x,
		browser_extent_y,
		browser_extent_z));
};
```

## Смежные страницы

- [Menu QuakeC](../16-quakec-scripting/menu-quakec.md)
- [Клавиатура, бинды и устройства ввода](../19-config-console/key-bindings-input-devices.md)
- [Встроенные веб-страницы в игре](../31-embedded-web-browser/in-game-web-pages.md)
- [Индекс справочника builtins](./README.md)
