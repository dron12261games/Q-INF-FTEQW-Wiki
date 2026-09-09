# Браузер серверов и мастер-сервер

> [⬅ Вернуться к оглавлению вики](../README.md)

> [Индекс справочника builtins](./README.md)

В MenuQC и CSQC браузер серверов работает не напрямую с сетью, а через **host cache** — локальный кеш записей о найденных серверах. Сначала движок получает адреса и ответы от мастер-серверов, LAN-поиска и отдельных игровых серверов, а затем складывает нормализованные поля вроде `name`, `map`, `ping`, `numplayers`, `gamedir` и `serverinfo` в этот кеш. Builtins семейства `gethostcache*` читают эти поля по индексу записи, а `sethostcachemask*`, `sethostcachesort`, `resorthostcache` и `refreshhostcache` управляют тем, какие записи видны пользователю и в каком порядке. В той же группе находятся соседние сетевые/системные builtins, которые часто встречаются рядом в `fteextensions.qc`, но не все из них относятся именно к host cache браузера серверов.

## Функции

### addwantedhostcachekey
`void(string key) addwantedhostcachekey = #623;`

* **key** — имя serverinfo-ключа, который меню собирается читать как дополнительное поле host cache.

#### Описание и логика работы
`addwantedhostcachekey` исторически предназначена для предварительной регистрации пользовательского ключа из serverinfo, чтобы браузер серверов знал, какие дополнительные данные вас интересуют. В текущей реализации FTEQW builtin фактически сводится к раннему вызову `gethostcacheindexforkey(key)`: полезного возвращаемого значения нет, но движок резервирует индекс для этого имени. Это удобно, когда вы хотите заранее договориться о нестандартных полях вроде [`teamplay`](../38-cvars-reference/04-network-server-cvars.md#teamplay), `skill` или ключей конкретного мода и потом читать их через `gethostcachestring` или `gethostcachenumber`.

#### Практические сценарии использования
```
float field_teamplay;

void() Menu_InitCustomServerFields =
{
// Регистрируем нестандартный ключ заранее, чтобы дальше работать уже с числовым индексом.
addwantedhostcachekey("teamplay");
field_teamplay = gethostcacheindexforkey("teamplay");
};

string(float row) Menu_ServerTeamplayLabel =
{
local string value;

value = gethostcachestring(field_teamplay, row);
if (value == "")
return "teamplay: unknown";
return "teamplay: " + value;
};
```

### gethostcacheindexforkey
`float(string key) gethostcacheindexforkey = #622;`

* **key** — текстовое имя поля в host cache или serverinfo, которое нужно превратить в числовой индекс.

#### Описание и логика работы
`gethostcacheindexforkey` преобразует строковое имя поля в числовой handle, который потом дешевле передавать в `gethostcachestring`, `gethostcachenumber`, `sethostcachemaskstring`, `sethostcachemasknumber` и `sethostcachesort`. Для стандартных имён движок знает готовые соответствия: например, `name`/[`hostname`](../38-cvars-reference/04-network-server-cvars.md#hostname), `address`/`cname`, `gamedir`/`game`, `numplayers`, `numhumans`, `map`, `ping`, `serverinfo`, `player0`, `player1` и так далее. Если имя не относится к стандартным ключам, движок регистрирует его как custom key, после чего этим же индексом можно пользоваться повторно во всех последующих вызовах.

#### Практические сценарии использования
```
float field_name;
float field_ping;
float field_address;

void() Menu_ResolveCommonServerFields =
{
field_name = gethostcacheindexforkey("name");
field_ping = gethostcacheindexforkey("ping");
field_address = gethostcacheindexforkey("cname");
};

string(float row) Menu_FormatServerRow =
{
return sprintf("%s  %g ms  %s",
gethostcachestring(field_name, row),
gethostcachenumber(field_ping, row),
gethostcachestring(field_address, row));
};
```

### gethostcachenumber
`float(float fld, float hostnr) gethostcachenumber = #621;`

* **fld** — индекс поля, обычно полученный из `gethostcacheindexforkey`.
* **hostnr** — индекс строки в текущем видимом списке серверов после фильтрации и сортировки.

#### Описание и логика работы
`gethostcachenumber` читает числовое значение из выбранной записи host cache. Индекс `hostnr` относится не ко всем когда-либо найденным серверам, а именно к текущему **видимому** отсортированному списку; поэтому после `resorthostcache` и `refreshhostcache` прежние номера строк могут измениться. Функция особенно полезна для полей `ping`, `numplayers`, `numhumans`, `maxplayers`, [`timelimit`](../38-cvars-reference/04-network-server-cvars.md#timelimit), [`fraglimit`](../38-cvars-reference/04-network-server-cvars.md#fraglimit), `protocol`, `isfavorite` и других числовых флагов. Если запись с таким номером сейчас не существует, текущая реализация возвращает `-1`.

#### Практические сценарии использования
```
float field_ping;
float field_numhumans;
float field_maxplayers;

void() Menu_InitNumericFields =
{
field_ping = gethostcacheindexforkey("ping");
field_numhumans = gethostcacheindexforkey("numhumans");
field_maxplayers = gethostcacheindexforkey("maxplayers");
};

string(float row) Menu_FormatPopulation =
{
return sprintf("%g/%g players, %g ms",
gethostcachenumber(field_numhumans, row),
gethostcachenumber(field_maxplayers, row),
gethostcachenumber(field_ping, row));
};
```

### gethostcachestring
`string(float type, float hostnr) gethostcachestring = #612;`

* **type** — индекс строкового поля в host cache.
* **hostnr** — индекс строки в текущем видимом списке серверов.

#### Описание и логика работы
`gethostcachestring` возвращает строковое значение поля для выбранного сервера: имя, карту, адрес, gamedir, сырой `serverinfo`, данные игрока (`player0`, `player1`, ...) или произвольный custom key. Для стандартных полей builtin сразу читает уже известное значение из кеша; для части нестандартных или player-ключей движок может лениво запросить расширенный `getinfo/getstatus`, если подробные данные ещё не получены. При неверном `hostnr` или отсутствии строки функция возвращает пустую строку, поэтому UI-код должен считать `""` нормальным состоянием «данных пока нет».

#### Практические сценарии использования
```
float field_name;
float field_map;
float field_address;

void() Menu_InitStringFields =
{
field_name = gethostcacheindexforkey("name");
field_map = gethostcacheindexforkey("map");
field_address = gethostcacheindexforkey("cname");
};

string(float row) Menu_ServerSummary =
{
return sprintf("%s on %s (%s)",
gethostcachestring(field_name, row),
gethostcachestring(field_map, row),
gethostcachestring(field_address, row));
};
```

### gethostcachevalue
`float(float type) gethostcachevalue = #611;`

* **type** — один из глобальных идентификаторов `SLIST_*`, определяющий, какое агрегированное состояние списка нужно прочитать.

#### Описание и логика работы
`gethostcachevalue` читает не поле отдельного сервера, а состояние всей подсистемы браузера. На практике чаще всего нужны `SLIST_HOSTCACHEVIEWCOUNT` (сколько строк сейчас видно после фильтров), `SLIST_HOSTCACHETOTALCOUNT` (сколько серверов вообще известно движку), `SLIST_SORTFIELD` и `SLIST_SORTDESCENDING`. В текущем FTEQW запрос `SLIST_HOSTCACHEVIEWCOUNT` и `SLIST_HOSTCACHETOTALCOUNT` заодно прокачивает внутренний цикл опроса (`CL_QueryServers`/poll sockets), так что простое перечитывание этих значений помогает меню получать свежие ответы без отдельного «тика». Счётчики `SLIST_MASTERQUERYCOUNT`, `SLIST_MASTERREPLYCOUNT`, `SLIST_SERVERQUERYCOUNT` и `SLIST_SERVERREPLYCOUNT` объявлены, но в текущей реализации возвращают `0`.

#### Практические сценарии использования
```
string() Menu_ServerListStatus =
{
local float visible;
local float total;
local float sortfld;
local float descending;

visible = gethostcachevalue(SLIST_HOSTCACHEVIEWCOUNT);
total = gethostcachevalue(SLIST_HOSTCACHETOTALCOUNT);
sortfld = gethostcachevalue(SLIST_SORTFIELD);
descending = gethostcachevalue(SLIST_SORTDESCENDING);

return sprintf("visible %g / total %g, sort field %g, desc %g",
visible, total, sortfld, descending);
};
```

### refreshhostcache
`void(optional float dopurge) refreshhostcache = #620;`

* **dopurge** — необязательный флаг полного сброса видимости перед новым опросом; `1` скрывает старые ответы до повторного отклика серверов, `0` оставляет старые записи видимыми до обновления.

#### Описание и логика работы
`refreshhostcache` запускает новый цикл запросов к мастер-серверам, LAN-источникам и самим игровым серверам. Это асинхронная операция: builtin только инициирует переопрос, а реальные ответы приходят позже, поэтому меню обычно вызывает её при открытии экрана, по кнопке Refresh или по таймеру, а затем каждый кадр перечитывает `gethostcachevalue(SLIST_HOSTCACHEVIEWCOUNT)`. Если передать `dopurge = 1`, движок временно помечает уже известные серверы как «нужно подтвердить заново», и список может заметно опустеть до прихода новых ответов.

#### Практические сценарии использования
```
float next_refresh_time;

void() Menu_OpenServerBrowser =
{
refreshhostcache(1); // полный переопрос при входе в меню серверов
next_refresh_time = time + 10;
};

void() Menu_ServerBrowserFrame =
{
if (time >= next_refresh_time)
{
refreshhostcache(0); // мягкое обновление без очистки уже видимых строк
next_refresh_time = time + 10;
}

// Сам факт чтения помогает движку обработать новые ответы.
gethostcachevalue(SLIST_HOSTCACHEVIEWCOUNT);
};
```

### resethostcachemasks
`void() resethostcachemasks = #615;`

* Аргументов нет.

#### Описание и логика работы
`resethostcachemasks` очищает весь набор ранее добавленных фильтров, созданных через `sethostcachemaskstring` и `sethostcachemasknumber`. Важно, что builtin только удаляет правила из внутреннего списка, но сама видимая выборка не перестраивается автоматически: чтобы пользователь сразу увидел результат, после сброса обычно вызывают `resorthostcache`. Типичный сценарий — кнопка **Clear filters**, повторное открытие экрана браузера или переключение между профилями фильтрации.

#### Практические сценарии использования
```
void() Menu_ClearServerFilters =
{
resethostcachemasks();
resorthostcache();
};
```

### resorthostcache
`void() resorthostcache = #618;`

* Аргументов нет.

#### Описание и логика работы
`resorthostcache` заново прогоняет все записи через активные маски и пересобирает видимый отсортированный список. После этого индексы `hostnr`, которые вы раньше использовали в `gethostcachestring` и `gethostcachenumber`, могут указывать уже на другие серверы или выйти за границы диапазона. Именно поэтому UI обычно хранит не только выделенную строку, но и проверяет её против нового `SLIST_HOSTCACHEVIEWCOUNT` после каждой пересортировки.

#### Практические сценарии использования
```
float server_selected;

void() Menu_RebuildServerView =
{
resorthostcache();

if (server_selected >= gethostcachevalue(SLIST_HOSTCACHEVIEWCOUNT))
server_selected = gethostcachevalue(SLIST_HOSTCACHEVIEWCOUNT) - 1;
if (server_selected < 0)
server_selected = -1;
};
```

### sethostcachemasknumber
`void(float mask, float fld, float num, float op) sethostcachemasknumber = #617;`

* **mask** — идентификатор/флаг правила; в текущей реализации practically используется только бит `512`, который превращает правило в OR-условие, иначе правило работает как AND.
* **fld** — индекс числового поля, к которому применяется сравнение.
* **num** — числовое значение для сравнения.
* **op** — оператор сравнения `SLIST_TEST_*`, например `SLIST_TEST_GREATER`, `SLIST_TEST_NOTEQUAL`, `SLIST_TEST_EQUAL`.

#### Описание и логика работы
`sethostcachemasknumber` добавляет числовое правило фильтрации к текущему списку масок. Правила накапливаются, пока вы не вызовете `resethostcachemasks`, а реально начинают влиять на видимую выборку после `resorthostcache`. Через неё удобно прятать пустые серверы, заполненные серверы, слишком высокий пинг, неподходящий протокол или любые числовые custom keys, которые мод публикует в `serverinfo`.

#### Практические сценарии использования
```
void() Menu_ShowJoinableServersOnly =
{
local float field_freeplayers;
local float field_numhumans;

resethostcachemasks();

field_freeplayers = gethostcacheindexforkey("freeplayers");
field_numhumans = gethostcacheindexforkey("numhumans");

// Оставляем только серверы, где есть хотя бы одно свободное место.
sethostcachemasknumber(0, field_freeplayers, 0, SLIST_TEST_GREATER);
// И дополнительно убираем совсем пустые серверы.
sethostcachemasknumber(0, field_numhumans, 0, SLIST_TEST_GREATER);

resorthostcache();
};
```

### sethostcachemaskstring
`void(float mask, float fld, string str, float op) sethostcachemaskstring = #616;`

* **mask** — идентификатор/флаг правила; бит `512` делает правило OR-условием, при `0` правило включается как AND.
* **fld** — индекс строкового поля, по которому фильтруется список.
* **str** — образец строки для сравнения.
* **op** — строковый оператор сравнения `SLIST_TEST_*`, например `SLIST_TEST_CONTAINS`, `SLIST_TEST_EQUAL`, `SLIST_TEST_STARTSWITH`.

#### Описание и логика работы
`sethostcachemaskstring` добавляет строковый фильтр к видимой выборке серверов. Наиболее частые поля для неё — `name`, `map`, `gamedir`, `mod`, `address` и любые custom keys, которые сервер сообщает текстом. Как и числовая версия, builtin лишь добавляет правило во внутренний список; чтобы пользователь сразу увидел новый список, после серии вызовов нужно сделать `resorthostcache`.

#### Практические сценарии использования
```
void(string wanted_gamedir, string name_part) Menu_FilterServers =
{
local float field_gamedir;
local float field_name;

resethostcachemasks();

field_gamedir = gethostcacheindexforkey("gamedir");
field_name = gethostcacheindexforkey("name");

if (wanted_gamedir != "")
sethostcachemaskstring(0, field_gamedir, wanted_gamedir, SLIST_TEST_EQUAL);
if (name_part != "")
sethostcachemaskstring(0, field_name, name_part, SLIST_TEST_CONTAINS);

resorthostcache();
};
```

### sethostcachesort
`void(float fld, float descending) sethostcachesort = #619;`

* **fld** — индекс поля, по которому должен сортироваться список.
* **descending** — флаг направления; обычно `0` для возрастания и `1` для убывания.

#### Описание и логика работы
`sethostcachesort` меняет активное поле сортировки, но не перестраивает список автоматически — для применения нужен отдельный `resorthostcache`. По исходникам движка второй аргумент технически трактуется как набор битовых флагов сортировки, однако MenuQC обычно использует только младший бит `0/1` для обычного переключения ascending/descending. Сортировать можно как по числовым полям (`ping`, `numplayers`, `maxplayers`), так и по строковым (`name`, `map`, `gamedir`).

#### Практические сценарии использования
```
void(float descending) Menu_SortByPing =
{
local float field_ping;

field_ping = gethostcacheindexforkey("ping");
sethostcachesort(field_ping, descending);
resorthostcache();
};
```

### getgamedirinfo
`string(float n, float prop) getgamedirinfo = #626;`

* **n** — индекс мода в перечислении движка; `-1` означает текущий активный gamedir.
* **prop** — одно из свойств `GGDI_*`: `GGDI_GAMEDIR`, `GGDI_DESCRIPTION`, `GGDI_OVERRIDES`, `GGDI_LOADCOMMAND`, `GGDI_ICON`, `GGDI_GAMEDIRLIST`.

#### Описание и логика работы
`getgamedirinfo` перечисляет известные движку моды и возвращает их свойства как строки. Это соседняя с браузером серверов builtin: она полезна, когда меню хочет показать список модов теми же терминами, которыми потом фильтруется host cache по `gamedir`. Для `n = -1` функция описывает уже загруженный мод, а для выходящих за диапазон индексов возвращает null/пустую строку. На практике особенно полезны `GGDI_DESCRIPTION` для человекочитаемого названия, `GGDI_LOADCOMMAND` для корректного переключения мода и `GGDI_ICON` для готового пути/шейдера иконки.

#### Практические сценарии использования
```
string() Menu_CurrentModLabel =
{
local string gamedir;
local string desc;

gamedir = getgamedirinfo(-1, GGDI_GAMEDIR);
desc = getgamedirinfo(-1, GGDI_DESCRIPTION);

if (desc == "")
return gamedir;
return sprintf("%s (%s)", desc, gamedir);
};

void() Menu_ListInstalledMods =
{
local float i;
local string gamedir;

for (i = 0; ; i = i + 1)
{
gamedir = getgamedirinfo(i, GGDI_GAMEDIR);
if (!gamedir)
break;

dprint(sprintf("mod %g: %s\n", i, getgamedirinfo(i, GGDI_DESCRIPTION)));
}
};
```

### getextresponse
`string() getextresponse = #624;`

* Аргументов нет.

#### Описание и логика работы
[`getextresponse`](04-network-messages-builtins.md#getextresponse) присутствует в наборе `FTE_CSQC_SERVERBROWSER`, но в текущем FTEQW остаётся заглушкой: клиентская реализация просто возвращает пустую строку. Из-за этого builtin нельзя использовать как надёжный источник данных ни для браузера серверов, ни для внешних сетевых ответов. Если вы встретили её в старом или переносимом коде, рассматривайте как legacy API и сразу закладывайте безопасное поведение для пустого результата.

#### Практические сценарии использования
```
void() Menu_DebugExtResponse =
{
local string s;

s = getextresponse();
if (s == "")
dprint("getextresponse returned nothing\n");
else
dprint(sprintf("ext response: %s\n", s));
};
```

### calltimeofday
`__deprecated("Use strftime.") void() calltimeofday = #231;`

* Аргументов нет.

#### Описание и логика работы
`calltimeofday` не относится к host cache и вообще не предназначена для MenuQC: в `fteextensions.qc` она объявлена для CSQC/SSQC и помечена как deprecated. При вызове движок немедленно ищет в вашем QuakeC функцию `timeofday`, а если она существует, синхронно вызывает её с аргументами `secs`, `mins`, `hour`, `day`, `mon`, `year`, `strvalue`. Если callback не объявлен, builtin просто ничего не делает. Для нового кода FTEQW прямо рекомендует [`strftime`](02-string-builtins.md#strftime), потому что он гибче и не завязан на скрытый callback.

#### Практические сценарии использования
```
float tod_hour;
float tod_minute;
string tod_stamp;

void(float secs, float mins, float hour, float day, float mon, float year, string strvalue) timeofday =
{
tod_hour = hour;
tod_minute = mins;
tod_stamp = strvalue;
};

void() CSQC_UpdateClockFromEngine =
{
calltimeofday();
dprint(sprintf("time now %g:%g (%s)\n", tod_hour, tod_minute, tod_stamp));
};
```

### openportal
`void(entity portal, float state) openportal = #207;`

* **portal** — сущность, связанная с areaportal: на Q2BSP это обычно сам `func_areaportal`, на Q3BSP — дверь, для которой уже определены области.
* **state** — новое состояние портала: обычно `1` для открытия и `0` для закрытия.

#### Описание и логика работы
`openportal` тоже не имеет отношения к host cache и не является MenuQC builtin: она существует в игровой QuakeC для картовых порталов Q2/Q3. Её задача — сообщить движку, должен ли areaportal сейчас пропускать видимость и звук между областями карты. На Q2BSP builtin ориентируется на [`style`](../39-entity-keys-reference/01-worldspawn-common-keys.md#style) у `func_areaportal`, а на Q3BSP использует дверь и ранее вычисленные области после [`setorigin`](03-entity-world-builtins.md#setorigin). Для браузера серверов эта функция не нужна, но в исходном списке builtins она находится рядом и поэтому часто попадает в общие справочники.

#### Практические сценарии использования
```
void() func_areaportal_use =
{
if (self.frame == 0)
{
self.frame = 1;
openportal(self, 1); // открыть areaportal
}
else
{
self.frame = 0;
openportal(self, 0); // закрыть areaportal
}
};
```

### getpackagemanagerinfo
`string(int n, int prop) getpackagemanagerinfo = #0:getpackagemanagerinfo;`

* **n** — `int`, индекс пакета в текущем списке package manager subsystem.
* **prop** — `int`, какой атрибут пакета нужно вернуть; движок ожидает один из `GPMI_*` кодов вроде `GPMI_NAME`, `GPMI_TITLE`, `GPMI_VERSION`, `GPMI_INSTALLED`, `GPMI_ACTION`, `GPMI_MAPS`, `GPMI_PREVIEWIMG` и т.д.

#### Описание и логика работы

`getpackagemanagerinfo` даёт CSQC/MenuQC доступ к списку пакетов, которые знает встроенный package manager движка. Официальная докстрока говорит, что сама функция только читает данные, а реальные действия выполняются отдельными консольными командами `pkg ...`; по реализации видно, что все значения возвращаются строками, включая filesize, статус установки, проценты загрузки, действия и список карт. Перебор обычно делают с `n = 0` вверх до тех пор, пока `getpackagemanagerinfo(n, GPMI_NAME)` не вернёт пустую строку; при первом запросе `n == 0` движок ещё и пинает `PM_AreSourcesNew(true)`, чтобы обновить сведения об источниках. Если пакет скрыт и не находится в одном из активных состояний, либо индекс/поле некорректны, результат будет пустой строкой.

#### Практические сценарии использования

```
void() ListKnownPackages =
{
    local float i;
    local string name;

    for (i = 0; (name = getpackagemanagerinfo(i, GPMI_NAME)) != ""; i = i + 1)
        print(name, " -> ", getpackagemanagerinfo(i, GPMI_INSTALLED), "\n");
};
```

## Смежные страницы

- [Поиск серверов и мастер-серверы](../25-server-browser-masters/README.md)
- [Menu QuakeC](../16-quakec-scripting/menu-quakec.md)
- [Индекс справочника builtins](./README.md)