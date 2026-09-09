# Быстрая навигация по API (сводные таблицы)

> [⬅ Вернуться к оглавлению вики](../README.md)

Эта статья не объясняет ничего сама по себе — это чисто справочный, максимально плотный указатель по всем элементам API движка, задокументированным в разделах 37–41. Если вы уже примерно знаете, как называется нужная функция, cvar, ключ сущности или директива, но не помните деталей — ищите имя в соответствующей таблице (Ctrl+F) и переходите по ссылке в первом столбце: она ведёт прямо на разбор этого элемента с сигнатурой, описанием логики и примерами кода. Если вы не знаете, с чего начать, а хотите изучить тему последовательно — вернитесь в [оглавление вики](../README.md) и откройте один из разделов 37–41 целиком, а не эту страницу.

## Содержание

- [Встроенные функции QuakeC (builtins)](#встроенные-функции-quakec-builtins)
  - [Точки входа QuakeC: SSQC](#точки-входа-quakec-ssqc)
  - [Точки входа QuakeC: CSQC](#точки-входа-quakec-csqc)
  - [Точки входа QuakeC: MenuQC](#точки-входа-quakec-menuqc)
  - [Математика и работа с векторами](#математика-и-работа-с-векторами)
  - [Строки и текст](#строки-и-текст)
  - [Сущности и игровой мир](#сущности-и-игровой-мир)
  - [Сеть и сетевые сообщения](#сеть-и-сетевые-сообщения)
  - [Звук](#звук)
  - [Файлы, буферы, хеш-таблицы и базы данных](#файлы-буферы-хеш-таблицы-и-базы-данных)
  - [Прекэш и игровые ресурсы](#прекэш-и-игровые-ресурсы)
  - [Рендеринг и сцена CSQC](#рендеринг-и-сцена-csqc)
  - [Ввод, интерфейс и клавиатура CSQC](#ввод-интерфейс-и-клавиатура-csqc)
  - [Скелетная анимация и модели](#скелетная-анимация-и-модели)
  - [Браузер серверов и мастер-сервер](#браузер-серверов-и-мастер-сервер)
  - [Системные функции, отладка и cvar](#системные-функции-отладка-и-cvar)
  - [Функции MenuQC (меню, экран загрузки)](#функции-menuqc-меню-экран-загрузки)
  - [Редактор карт, криптография и разные редкие builtins](#редактор-карт-криптография-и-разные-редкие-builtins)
- [Переменные движка (cvar)](#переменные-движка-cvar)
  - [Видео, экран и общий рендеринг](#видео-экран-и-общий-рендеринг)
  - [Освещение, тени и материалы](#освещение-тени-и-материалы)
  - [Звук](#звук-1)
  - [Сеть, сервер и мультиплеер](#сеть-сервер-и-мультиплеер)
  - [Физика и игровой процесс](#физика-и-игровой-процесс)
  - [Интерфейс, консоль и управление](#интерфейс-консоль-и-управление)
  - [Системные, отладочные и прочие cvar](#системные-отладочные-и-прочие-cvar)
- [Ключи сущностей карты (entity keys)](#ключи-сущностей-карты-entity-keys)
  - [Общие ключи, worldspawn и глобальные настройки уровня](#общие-ключи-worldspawn-и-глобальные-настройки-уровня)
  - [Свет и освещение](#свет-и-освещение)
  - [Триггеры и логические сущности](#триггеры-и-логические-сущности)
  - [Двери, платформы и подвижная геометрия](#двери-платформы-и-подвижная-геометрия)
  - [Монстры, NPC и точки появления игрока](#монстры-npc-и-точки-появления-игрока)
  - [Предметы и оружие](#предметы-и-оружие)
- [Директивы языка материалов (.shader)](#директивы-языка-материалов-shader)
  - [Директивы уровня материала](#директивы-уровня-материала)
  - [Директивы уровня стадии](#директивы-уровня-стадии)
- [Директивы языка частиц (.particles)](#директивы-языка-частиц-particles)
  - [Директивы эффекта](#директивы-эффекта)
  - [Директивы поведения и появления](#директивы-поведения-и-появления)


## Встроенные функции QuakeC (builtins)

Всего задокументировано: **744** builtin-функций и точек входа (включая точки входа SSQC/CSQC/MenuQC и отдельно посчитанные CSQC- и MenuQC-варианты одноимённых функций). Полный постатейный разбор — в разделе [«37. Встроенные функции QuakeC»](../37-quakec-builtins-reference/README.md).


### Точки входа QuakeC: SSQC

| Элемент | Сигнатура / Описание |
|---|---|
| [`SetNewParms`](../37-quakec-builtins-reference/00-entry-points.md#setnewparms) | `void() SetNewParms` |
| [`SetChangeParms`](../37-quakec-builtins-reference/00-entry-points.md#setchangeparms) | `void() SetChangeParms` |
| [`ClientConnect`](../37-quakec-builtins-reference/00-entry-points.md#clientconnect) | `void() ClientConnect` |
| [`PutClientInServer`](../37-quakec-builtins-reference/00-entry-points.md#putclientinserver) | `void() PutClientInServer` |
| [`ClientKill`](../37-quakec-builtins-reference/00-entry-points.md#clientkill) | `void() ClientKill` |
| [`PlayerPreThink`](../37-quakec-builtins-reference/00-entry-points.md#playerprethink) | `void() PlayerPreThink` |
| [`PlayerPostThink`](../37-quakec-builtins-reference/00-entry-points.md#playerpostthink) | `void() PlayerPostThink` |
| [`StartFrame`](../37-quakec-builtins-reference/00-entry-points.md#startframe) | `void() StartFrame` |
| [`EndFrame`](../37-quakec-builtins-reference/00-entry-points.md#endframe) | `void() EndFrame` |
| [`ClientDisconnect`](../37-quakec-builtins-reference/00-entry-points.md#clientdisconnect) | `void() ClientDisconnect` |
| [`main`](../37-quakec-builtins-reference/00-entry-points.md#main-устаревшая-не-вызывается) | `void() main` |

### Точки входа QuakeC: CSQC

| Элемент | Сигнатура / Описание |
|---|---|
| [`CSQC_Init`](../37-quakec-builtins-reference/00-entry-points.md#csqc_init) | `void(float apilevel, string enginename, float engineversion) CSQC_Init` |
| [`CSQC_WorldLoaded`](../37-quakec-builtins-reference/00-entry-points.md#csqc_worldloaded) | `void() CSQC_WorldLoaded` |
| [`CSQC_UpdateView`](../37-quakec-builtins-reference/00-entry-points.md#csqc_updateview) | `void(float vwidth, float vheight, float notmenu) CSQC_UpdateView` |
| [`CSQC_InputEvent`](../37-quakec-builtins-reference/00-entry-points.md#csqc_inputevent) | `float(float evtype, float scanx, float chary, float devid) CSQC_InputEvent` |
| [`CSQC_ConsoleCommand`](../37-quakec-builtins-reference/00-entry-points.md#csqc_consolecommand) | `float(string cmd) CSQC_ConsoleCommand` |
| [`CSQC_Parse_StuffCmd`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_stuffcmd) | `void(string msg) CSQC_Parse_StuffCmd` |
| [`CSQC_Parse_CenterPrint`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_centerprint) | `float(string msg) CSQC_Parse_CenterPrint` |
| [`CSQC_Parse_Print`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_print) | `void(string printmsg, float printlvl) CSQC_Parse_Print` |
| [`CSQC_Ent_Update`](../37-quakec-builtins-reference/00-entry-points.md#csqc_ent_update) | `void(float isnew) CSQC_Ent_Update` |
| [`CSQC_Event_Sound`](../37-quakec-builtins-reference/00-entry-points.md#csqc_event_sound) | `float(float entnum, float channel, string soundname, float vol, float attenuation, vector pos, float pitchmod, float flags) CSQC_Event_Sound` |
| [`CSQC_Ent_Remove`](../37-quakec-builtins-reference/00-entry-points.md#csqc_ent_remove) | `void() CSQC_Ent_Remove` |
| [`CSQC_Shutdown`](../37-quakec-builtins-reference/00-entry-points.md#csqc_shutdown) | `void() CSQC_Shutdown` |
| [`CSQC_UpdateViewLoading`](../37-quakec-builtins-reference/00-entry-points.md#csqc_updateviewloading) | `void(float vwidth, float vheight, float notmenu) CSQC_UpdateViewLoading` |
| [`CSQC_DrawHud`](../37-quakec-builtins-reference/00-entry-points.md#csqc_drawhud) | `void(vector viewsize, float scoresshown) CSQC_DrawHud` |
| [`CSQC_DrawScores`](../37-quakec-builtins-reference/00-entry-points.md#csqc_drawscores) | `void(vector viewsize, float scoresshown) CSQC_DrawScores` |
| [`CSQC_Parse_Event`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_event) | `void() CSQC_Parse_Event` |
| [`CSQC_Parse_Damage`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_damage) | `float(float save, float take, vector inflictororg) CSQC_Parse_Damage` |
| [`CSQC_Parse_SetAngles`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_setangles) | `float(vector angles, float isdelta) CSQC_Parse_SetAngles` |
| [`CSQC_PlayerInfoChanged`](../37-quakec-builtins-reference/00-entry-points.md#csqc_playerinfochanged) | `void(float playernum) CSQC_PlayerInfoChanged` |
| [`CSQC_ServerInfoChanged`](../37-quakec-builtins-reference/00-entry-points.md#csqc_serverinfochanged) | `void() CSQC_ServerInfoChanged` |
| [`CSQC_Input_Frame`](../37-quakec-builtins-reference/00-entry-points.md#csqc_input_frame) | `void() CSQC_Input_Frame` |
| [`CSQC_RendererRestarted`](../37-quakec-builtins-reference/00-entry-points.md#csqc_rendererrestarted) | `void(string rendererdescription) CSQC_RendererRestarted` |
| [`CSQC_GenerateMaterial`](../37-quakec-builtins-reference/00-entry-points.md#csqc_generatematerial) | `string(string shadername) CSQC_GenerateMaterial` |
| [`CSQC_ConsoleLink`](../37-quakec-builtins-reference/00-entry-points.md#csqc_consolelink) | `float(string text, string info) CSQC_ConsoleLink` |
| [`CSQC_Ent_Spawn`](../37-quakec-builtins-reference/00-entry-points.md#csqc_ent_spawn) | `void(float newentnum) CSQC_Ent_Spawn` |
| [`CSQC_ServerSound`](../37-quakec-builtins-reference/00-entry-points.md#csqc_serversound) | `float(float channel, string soundname, vector pos, float vol, float attenuation, float flags) CSQC_ServerSound` |
| [`CSQC_Parse_TempEntity`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_tempentity) | `float() CSQC_Parse_TempEntity` |
| [`CSQC_MapEntityEdited`](../37-quakec-builtins-reference/00-entry-points.md#csqc_mapentityedited) | `void(int entidx, string newentdata) CSQC_MapEntityEdited` |

### Точки входа QuakeC: MenuQC

| Элемент | Сигнатура / Описание |
|---|---|
| [`m_init`](../37-quakec-builtins-reference/00-entry-points.md#m_init) | `void() m_init` |
| [`m_shutdown`](../37-quakec-builtins-reference/00-entry-points.md#m_shutdown) | `void() m_shutdown` |
| [`m_toggle`](../37-quakec-builtins-reference/00-entry-points.md#m_toggle) | `void(float show) m_toggle` |
| [`m_draw`](../37-quakec-builtins-reference/00-entry-points.md#m_draw) | `void(vector screensize) m_draw` |
| [`m_drawloading`](../37-quakec-builtins-reference/00-entry-points.md#m_drawloading) | `void(vector screensize, float opaque) m_drawloading` |
| [`m_keydown`](../37-quakec-builtins-reference/00-entry-points.md#m_keydown) | `void(float scan, float chr) m_keydown` |
| [`m_keyup`](../37-quakec-builtins-reference/00-entry-points.md#m_keyup) | `void(float scan, float chr) m_keyup` |
| [`Menu_InputEvent`](../37-quakec-builtins-reference/00-entry-points.md#menu_inputevent) | `float(float evtype, float scanx, float chary, float devid) Menu_InputEvent` |
| [`m_consolecommand`](../37-quakec-builtins-reference/00-entry-points.md#m_consolecommand) | `float(string cmd) m_consolecommand` |
| [`m_gethostcachecategory`](../37-quakec-builtins-reference/00-entry-points.md#m_gethostcachecategory) | `float(float hostcachenum) m_gethostcachecategory` |
| [`Menu_RendererRestarted`](../37-quakec-builtins-reference/00-entry-points.md#menu_rendererrestarted) | `void(string rendererdescription) Menu_RendererRestarted` |
| [`GameCommand`](../37-quakec-builtins-reference/00-entry-points.md#gamecommand) | `void(string cmdtext) GameCommand` |

### Математика и работа с векторами

| Элемент | Сигнатура / Описание |
|---|---|
| [`acos`](../37-quakec-builtins-reference/01-math-vector-builtins.md#acos) | `float(float c) acos = #472;` |
| [`asin`](../37-quakec-builtins-reference/01-math-vector-builtins.md#asin) | `float(float s) asin = #471;` |
| [`atan`](../37-quakec-builtins-reference/01-math-vector-builtins.md#atan) | `float(float t) atan = #473;` |
| [`atan2`](../37-quakec-builtins-reference/01-math-vector-builtins.md#atan2) | `float(float c, float s) atan2 = #474;` |
| [`tan`](../37-quakec-builtins-reference/01-math-vector-builtins.md#tan) | `float(float a) tan = #475;` |
| [`sin`](../37-quakec-builtins-reference/01-math-vector-builtins.md#sin) | `float(float angle) sin = #60;` |
| [`cos`](../37-quakec-builtins-reference/01-math-vector-builtins.md#cos) | `float(float angle) cos = #61;` |
| [`sqrt`](../37-quakec-builtins-reference/01-math-vector-builtins.md#sqrt) | `float(float value) sqrt = #62;` |
| [`pow`](../37-quakec-builtins-reference/01-math-vector-builtins.md#pow) | `float(float value, float exp) pow = #97;` |
| [`log`](../37-quakec-builtins-reference/01-math-vector-builtins.md#log) | `float(float v, optional float base) log = #532;` |
| [`rint`](../37-quakec-builtins-reference/01-math-vector-builtins.md#rint) | `float(float value) rint = #36;` |
| [`ceil`](../37-quakec-builtins-reference/01-math-vector-builtins.md#ceil) | `float(float value) ceil = #38;` |
| [`floor`](../37-quakec-builtins-reference/01-math-vector-builtins.md#floor) | `float(float value) floor = #37;` |
| [`fabs`](../37-quakec-builtins-reference/01-math-vector-builtins.md#fabs) | `float(float value) fabs = #43;` |
| [`bound`](../37-quakec-builtins-reference/01-math-vector-builtins.md#bound) | `float(float minimum, float val, float maximum) bound = #96;` |
| [`min`](../37-quakec-builtins-reference/01-math-vector-builtins.md#min) | `float(float a, float b, ...) min = #94;` |
| [`max`](../37-quakec-builtins-reference/01-math-vector-builtins.md#max) | `float(float a, float b, ...) max = #95;` |
| [`mod`](../37-quakec-builtins-reference/01-math-vector-builtins.md#mod) | `float(float dividend, float divisor) mod = #245;` |
| [`random`](../37-quakec-builtins-reference/01-math-vector-builtins.md#random) | `float() random = #7;` |
| [`randomvec`](../37-quakec-builtins-reference/01-math-vector-builtins.md#randomvec) | `vector() randomvec = #91;` |
| [`randomvector`](../37-quakec-builtins-reference/01-math-vector-builtins.md#randomvector) | `vector() randomvector = #41;` |
| [`bitshift`](../37-quakec-builtins-reference/01-math-vector-builtins.md#bitshift) | `float(float number, float quantity) bitshift = #218;` |
| [`anglemod`](../37-quakec-builtins-reference/01-math-vector-builtins.md#anglemod) | `float(float value) anglemod = #102;` |
| [`changepitch`](../37-quakec-builtins-reference/01-math-vector-builtins.md#changepitch) | `void(entity ent) changepitch = #63;` |
| [`changeyaw`](../37-quakec-builtins-reference/01-math-vector-builtins.md#changeyaw) | `void() changeyaw = #49;` |
| [`normalize`](../37-quakec-builtins-reference/01-math-vector-builtins.md#normalize) | `vector(vector v) normalize = #9;` |
| [`vlen`](../37-quakec-builtins-reference/01-math-vector-builtins.md#vlen) | `float(vector v) vlen = #12;` |
| [`vtos`](../37-quakec-builtins-reference/01-math-vector-builtins.md#vtos) | `string(vector val) vtos = #27;` |
| [`vectoangles`](../37-quakec-builtins-reference/01-math-vector-builtins.md#vectoangles) | `vector(vector fwd, optional vector up) vectoangles = #51;` |
| [`vectoyaw`](../37-quakec-builtins-reference/01-math-vector-builtins.md#vectoyaw) | `float(vector v, optional entity reference) vectoyaw = #13;` |
| [`makevectors`](../37-quakec-builtins-reference/01-math-vector-builtins.md#makevectors) | `void(vector vang) makevectors = #1;` |
| [`vectorvectors`](../37-quakec-builtins-reference/01-math-vector-builtins.md#vectorvectors) | `void(vector dir) vectorvectors = #432;` |
| [`rotatevectorsbyangle`](../37-quakec-builtins-reference/01-math-vector-builtins.md#rotatevectorsbyangle) | `void(vector angle) rotatevectorsbyangle = #235;` |
| [`rotatevectorsbyvectors`](../37-quakec-builtins-reference/01-math-vector-builtins.md#rotatevectorsbyvectors) | `void(vector fwd, vector right, vector up) rotatevectorsbyvectors = #236;` |
| [`rotatevectorsbytag`](../37-quakec-builtins-reference/01-math-vector-builtins.md#rotatevectorsbytag) | `vector(entity ent, float tagnum) rotatevectorsbytag = #244;` |
| [`project`](../37-quakec-builtins-reference/01-math-vector-builtins.md#project) | `vector(vector v) project = #311;` |
| [`unproject`](../37-quakec-builtins-reference/01-math-vector-builtins.md#unproject) | `vector(vector v) unproject = #310;` |
| [`crc16`](../37-quakec-builtins-reference/01-math-vector-builtins.md#crc16) | `__deprecated("Use digest_hex") float(float caseinsensitive, string s, ...) crc16 = #494;` |
| [`htos`](../37-quakec-builtins-reference/01-math-vector-builtins.md#htos) | `string(int value) htos = #262;` |
| [`itos`](../37-quakec-builtins-reference/01-math-vector-builtins.md#itos) | `string(int value) itos = #260;` |
| [`stoi`](../37-quakec-builtins-reference/01-math-vector-builtins.md#stoi) | `int(string s) stoi = #259;` |
| [`stoh`](../37-quakec-builtins-reference/01-math-vector-builtins.md#stoh) | `int(string s) stoh = #261;` |
| [`str2chr`](../37-quakec-builtins-reference/01-math-vector-builtins.md#str2chr) | `float(string str, float index) str2chr = #222;` |
| [`chr2str`](../37-quakec-builtins-reference/01-math-vector-builtins.md#chr2str) | `string(float chr, ...) chr2str = #223;` |
| [`anglesub`](../37-quakec-builtins-reference/01-math-vector-builtins.md#anglesub) | `float(float newangle, float oldangle) anglesub = #0:anglesub;` |
| [`crossproduct`](../37-quakec-builtins-reference/01-math-vector-builtins.md#crossproduct) | `vector(vector v1, vector v2) crossproduct = #0:crossproduct;` |
| [`ftoi`](../37-quakec-builtins-reference/01-math-vector-builtins.md#ftoi) | `int(float) ftoi = #0:ftoi;` |
| [`ftou`](../37-quakec-builtins-reference/01-math-vector-builtins.md#ftou) | `__uint(float) ftou = #0:ftou;` |
| [`itof`](../37-quakec-builtins-reference/01-math-vector-builtins.md#itof) | `float(int, optional float shift, float mask=24) itof = #0:itof;` |
| [`logarithm`](../37-quakec-builtins-reference/01-math-vector-builtins.md#logarithm) | `float(float v, optional float base) logarithm = #0:logarithm;` |
| [`utof`](../37-quakec-builtins-reference/01-math-vector-builtins.md#utof) | `float(__uint, optional float shift, float mask=24) utof = #0:utof;` |

### Строки и текст

| Элемент | Сигнатура / Описание |
|---|---|
| [`strlen`](../37-quakec-builtins-reference/02-string-builtins.md#strlen) | `float(string s) strlen = #114;` |
| [`strlennocol`](../37-quakec-builtins-reference/02-string-builtins.md#strlennocol) | `float(string s) strlennocol = #476;` |
| [`strcat`](../37-quakec-builtins-reference/02-string-builtins.md#strcat) | `string(string s1, optional string s2, optional string s3, optional string s4, optional string s5, optional string s6, optional string s7, optional string s8) strcat = #115;` |
| [`substring`](../37-quakec-builtins-reference/02-string-builtins.md#substring) | `string(string s, float start, float length) substring = #116;` |
| [`stov`](../37-quakec-builtins-reference/02-string-builtins.md#stov) | `vector(string s) stov = #117;` |
| [`strzone`](../37-quakec-builtins-reference/02-string-builtins.md#strzone) | `string(string s, ...) strzone = #118;` |
| [`strunzone`](../37-quakec-builtins-reference/02-string-builtins.md#strunzone) | `void(string s) strunzone = #119;` |
| [`strcasecmp`](../37-quakec-builtins-reference/02-string-builtins.md#strcasecmp) | `float(string s1, string s2) strcasecmp = #229;` |
| [`strncasecmp`](../37-quakec-builtins-reference/02-string-builtins.md#strncasecmp) | `float(string s1, string s2, float len, optional float s1ofs, optional float s2ofs) strncasecmp = #230;` |
| [`strncmp`](../37-quakec-builtins-reference/02-string-builtins.md#strncmp) | `float(string s1, string s2, optional float len, optional float s1ofs, optional float s2ofs) strncmp = #228;` |
| [`strstrofs`](../37-quakec-builtins-reference/02-string-builtins.md#strstrofs) | `float(string s1, string sub, optional float startidx) strstrofs = #221;` |
| [`strtolower`](../37-quakec-builtins-reference/02-string-builtins.md#strtolower) | `string(string s) strtolower = #480;` |
| [`strtoupper`](../37-quakec-builtins-reference/02-string-builtins.md#strtoupper) | `string(string s) strtoupper = #481;` |
| [`strreplace`](../37-quakec-builtins-reference/02-string-builtins.md#strreplace) | `string(string search, string replace, string subject) strreplace = #484;` |
| [`strireplace`](../37-quakec-builtins-reference/02-string-builtins.md#strireplace) | `string(string search, string replace, string subject) strireplace = #485;` |
| [`strpad`](../37-quakec-builtins-reference/02-string-builtins.md#strpad) | `string(float pad, string str1, ...) strpad = #225;` |
| [`strconv`](../37-quakec-builtins-reference/02-string-builtins.md#strconv) | `string(float ccase, float redalpha, float redchars, string str, ...) strconv = #224;` |
| [`strdecolorize`](../37-quakec-builtins-reference/02-string-builtins.md#strdecolorize) | `string(string s) strdecolorize = #477;` |
| [`strftime`](../37-quakec-builtins-reference/02-string-builtins.md#strftime) | `string(float uselocaltime, string format, ...) strftime = #478;` |
| [`sprintf`](../37-quakec-builtins-reference/02-string-builtins.md#sprintf) | `string(string fmt, ...) sprintf = #627;` |
| [`tokenize`](../37-quakec-builtins-reference/02-string-builtins.md#tokenize) | `float(string s) tokenize = #441;` |
| [`tokenize_console`](../37-quakec-builtins-reference/02-string-builtins.md#tokenize_console) | `float(string str) tokenize_console = #514;` |
| [`tokenizebyseparator`](../37-quakec-builtins-reference/02-string-builtins.md#tokenizebyseparator) | `float(string s, string separator1, ...) tokenizebyseparator = #479;` |
| [`argv`](../37-quakec-builtins-reference/02-string-builtins.md#argv) | `string(float n) argv = #442;` |
| [`argv_start_index`](../37-quakec-builtins-reference/02-string-builtins.md#argv_start_index) | `float(float idx) argv_start_index = #515;` |
| [`argv_end_index`](../37-quakec-builtins-reference/02-string-builtins.md#argv_end_index) | `float(float idx) argv_end_index = #516;` |
| [`validstring`](../37-quakec-builtins-reference/02-string-builtins.md#validstring) | `float(string str) validstring = #81;` |
| [`ftos`](../37-quakec-builtins-reference/02-string-builtins.md#ftos) | `string(float val) ftos = #26;` |
| [`stof`](../37-quakec-builtins-reference/02-string-builtins.md#stof) | `float(string s) stof = #81;` |
| [`altstr_count`](../37-quakec-builtins-reference/02-string-builtins.md#altstr_count) | `float(string str) altstr_count = #82;` |
| [`altstr_get`](../37-quakec-builtins-reference/02-string-builtins.md#altstr_get) | `string(string str, float num) altstr_get = #84;` |
| [`altstr_prepare`](../37-quakec-builtins-reference/02-string-builtins.md#altstr_prepare) | `string(string str) altstr_prepare = #83;` |
| [`altstr_set`](../37-quakec-builtins-reference/02-string-builtins.md#altstr_set) | `string(string str, float num, string setval) altstr_set = #85;` |
| [`uri_escape`](../37-quakec-builtins-reference/02-string-builtins.md#uri_escape) | `string(string in) uri_escape = #510;` |
| [`uri_unescape`](../37-quakec-builtins-reference/02-string-builtins.md#uri_unescape) | `string(string in) uri_unescape = #511;` |
| [`argescape`](../37-quakec-builtins-reference/02-string-builtins.md#argescape) | `string(string s) argescape = #295;` |
| [`stringwidth`](../37-quakec-builtins-reference/02-string-builtins.md#stringwidth) | `float(string text, float usecolours, optional vector fontsize) stringwidth = #327;` |
| [`stringtokeynum`](../37-quakec-builtins-reference/02-string-builtins.md#stringtokeynum) | `float(string keyname) stringtokeynum = #341;` |
| [`str2chr`](../37-quakec-builtins-reference/02-string-builtins.md#str2chr) | `float(string str, float index) str2chr = #222;` |
| [`chr2str`](../37-quakec-builtins-reference/02-string-builtins.md#chr2str) | `string(float chr, ...) chr2str = #223;` |
| [`altstr_ins`](../37-quakec-builtins-reference/02-string-builtins.md#altstr_ins) | `DEP string(string str, float num, string set) altstr_ins = #86;` |
| [`base64decode`](../37-quakec-builtins-reference/02-string-builtins.md#base64decode) | `__variant*(string base64str, __out int bytes) base64decode = #0:base64decode;` |
| [`base64encode`](../37-quakec-builtins-reference/02-string-builtins.md#base64encode) | `string(__variant *ptr, int bytes, optional int offset) base64encode = #0:base64encode;` |
| [`instr`](../37-quakec-builtins-reference/02-string-builtins.md#instr) | `string(string input, string token) instr = #206;` |
| [`matchpattern`](../37-quakec-builtins-reference/02-string-builtins.md#matchpattern) | `float(string s, string pattern, float matchrule) matchpattern = #538;` |
| [`strcmp`](../37-quakec-builtins-reference/02-string-builtins.md#strcmp) | `#define strcmp strncmp` |
| [`strtrim`](../37-quakec-builtins-reference/02-string-builtins.md#strtrim) | `string(string s) strtrim = #0:strtrim;` |

### Сущности и игровой мир

| Элемент | Сигнатура / Описание |
|---|---|
| [`spawn`](../37-quakec-builtins-reference/03-entity-world-builtins.md#spawn) | `entity() spawn = #14;` |
| [`remove`](../37-quakec-builtins-reference/03-entity-world-builtins.md#remove) | `void(entity e) remove = #15;` |
| [`find`](../37-quakec-builtins-reference/03-entity-world-builtins.md#find) | `entity(entity start, .string fld, string match) find = #18;` |
| [`findchain`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findchain) | `entity(.string field, string match, optional .entity chainfield) findchain = #402;` |
| [`findchainflags`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findchainflags) | `entity(.float fld, float match, optional .entity chainfield) findchainflags = #450;` |
| [`findchainfloat`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findchainfloat) | `entity(.float fld, float match, optional .entity chainfield) findchainfloat = #403;` |
| [`findflags`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findflags) | `entity(entity start, .float field, float match) findflags = #449;` |
| [`findfloat`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findfloat) | `entity(entity start, .__variant fld, __variant match) findfloat = #98;` |
| [`findradius`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findradius) | `entity(vector org, float rad, optional .entity chainfield) findradius = #22;` |
| [`nextent`](../37-quakec-builtins-reference/03-entity-world-builtins.md#nextent) | `entity(entity e) nextent = #47;` |
| [`setmodel`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setmodel) | `void(entity e, string m) setmodel = #3;` |
| [`setmodelindex`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setmodelindex) | `void(entity e, float mdlindex) setmodelindex = #333;` |
| [`setorigin`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setorigin) | `void(entity e, vector o) setorigin = #2;` |
| [`setsize`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setsize) | `void(entity e, vector min, vector max) setsize = #4;` |
| [`checkbottom`](../37-quakec-builtins-reference/03-entity-world-builtins.md#checkbottom) | `float(entity ent) checkbottom = #40;` |
| [`droptofloor`](../37-quakec-builtins-reference/03-entity-world-builtins.md#droptofloor) | `float() droptofloor = #34;` |
| [`walkmove`](../37-quakec-builtins-reference/03-entity-world-builtins.md#walkmove) | `float(float yaw, float dist, optional float settraceglobals) walkmove = #32;` |
| [`movetogoal`](../37-quakec-builtins-reference/03-entity-world-builtins.md#movetogoal) | `void(float step) movetogoal = #67;` |
| [`touchtriggers`](../37-quakec-builtins-reference/03-entity-world-builtins.md#touchtriggers) | `void(optional entity ent, optional vector neworigin) touchtriggers = #279;` |
| [`pointcontents`](../37-quakec-builtins-reference/03-entity-world-builtins.md#pointcontents) | `float(vector pos) pointcontents = #41;` |
| [`checkclient`](../37-quakec-builtins-reference/03-entity-world-builtins.md#checkclient) | `entity() checkclient = #17;` |
| [`checkpvs`](../37-quakec-builtins-reference/03-entity-world-builtins.md#checkpvs) | `float(vector viewpos, entity entity) checkpvs = #240;` |
| [`num_for_edict`](../37-quakec-builtins-reference/03-entity-world-builtins.md#num_for_edict) | `float(entity ent) num_for_edict = #512;` |
| [`edict_num`](../37-quakec-builtins-reference/03-entity-world-builtins.md#edict_num) | `entity(float entnum) edict_num = #459;` |
| [`etof`](../37-quakec-builtins-reference/03-entity-world-builtins.md#etof) | `float(entity e) etof = #79;` |
| [`ftoe`](../37-quakec-builtins-reference/03-entity-world-builtins.md#ftoe) | `entity(float f) ftoe = #80;` |
| [`etos`](../37-quakec-builtins-reference/03-entity-world-builtins.md#etos) | `string(entity ent) etos = #65;` |
| [`wasfreed`](../37-quakec-builtins-reference/03-entity-world-builtins.md#wasfreed) | `float(entity ent) wasfreed = #353;` |
| [`copyentity`](../37-quakec-builtins-reference/03-entity-world-builtins.md#copyentity) | `entity(entity from, optional entity to) copyentity = #400;` |
| [`aim`](../37-quakec-builtins-reference/03-entity-world-builtins.md#aim) | `vector(entity player, float missilespeed) aim = #44;` |
| [`traceline`](../37-quakec-builtins-reference/03-entity-world-builtins.md#traceline) | `void(vector v1, vector v2, float flags, entity ent) traceline = #16;` |
| [`tracebox`](../37-quakec-builtins-reference/03-entity-world-builtins.md#tracebox) | `void(vector start, vector mins, vector maxs, vector end, float nomonsters, entity ent) tracebox = #90;` |
| [`tracetoss`](../37-quakec-builtins-reference/03-entity-world-builtins.md#tracetoss) | `void(entity ent, entity ignore) tracetoss = #64;` |
| [`traceon`](../37-quakec-builtins-reference/03-entity-world-builtins.md#traceon) | `void() traceon = #29;` |
| [`traceoff`](../37-quakec-builtins-reference/03-entity-world-builtins.md#traceoff) | `void() traceoff = #30;` |
| [`makestatic`](../37-quakec-builtins-reference/03-entity-world-builtins.md#makestatic) | `void(entity e) makestatic = #69;` |
| [`setspawnparms`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setspawnparms) | `void(entity player) setspawnparms = #78;` |
| [`spawnclient`](../37-quakec-builtins-reference/03-entity-world-builtins.md#spawnclient) | `entity() spawnclient = #454;` |
| [`dropclient`](../37-quakec-builtins-reference/03-entity-world-builtins.md#dropclient) | `void(entity player) dropclient = #453;` |
| [`runstandardplayerphysics`](../37-quakec-builtins-reference/03-entity-world-builtins.md#runstandardplayerphysics) | `void(entity ent) runstandardplayerphysics = #347;` |
| [`getstati`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getstati) | `int(float stnum) getstati = #330;` |
| [`getstatf`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getstatf) | `float(float stnum, optional float firstbit, optional float bitcount) getstatf = #331;` |
| [`getstats`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getstats) | `string(float stnum) getstats = #332;` |
| [`clientstat`](../37-quakec-builtins-reference/03-entity-world-builtins.md#clientstat) | `void(float num, float type, .__variant fld) clientstat = #232;` |
| [`globalstat`](../37-quakec-builtins-reference/03-entity-world-builtins.md#globalstat) | `void(float num, float type, string name) globalstat = #233;` |
| [`forceinfokey`](../37-quakec-builtins-reference/03-entity-world-builtins.md#forceinfokey) | `void(entity player, string key, string value) forceinfokey = #213;` |
| [`serverkey`](../37-quakec-builtins-reference/03-entity-world-builtins.md#serverkey) | `string(string key) serverkey = #354;` |
| [`infokey`](../37-quakec-builtins-reference/03-entity-world-builtins.md#infokey) | `string(entity e, string key) infokey = #80;` |
| [`infoadd`](../37-quakec-builtins-reference/03-entity-world-builtins.md#infoadd) | `infostring(infostring old, string key, string value) infoadd = #226;` |
| [`infoget`](../37-quakec-builtins-reference/03-entity-world-builtins.md#infoget) | `string(infostring info, string key) infoget = #227;` |
| [`matchclientname`](../37-quakec-builtins-reference/03-entity-world-builtins.md#matchclientname) | `entity(string match, optional float matchnum) matchclientname = #241;` |
| [`entityfieldname`](../37-quakec-builtins-reference/03-entity-world-builtins.md#entityfieldname) | `string(float fieldnum) entityfieldname = #497;` |
| [`entityfieldtype`](../37-quakec-builtins-reference/03-entity-world-builtins.md#entityfieldtype) | `float(float fieldnum) entityfieldtype = #498;` |
| [`numentityfields`](../37-quakec-builtins-reference/03-entity-world-builtins.md#numentityfields) | `float() numentityfields = #496;` |
| [`getentityfieldstring`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getentityfieldstring) | `string(float fieldnum, entity ent) getentityfieldstring = #499;` |
| [`putentityfieldstring`](../37-quakec-builtins-reference/03-entity-world-builtins.md#putentityfieldstring) | `float(float fieldnum, entity ent, string s) putentityfieldstring = #500;` |
| [`getentitytoken`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getentitytoken) | `string(optional string resetstring) getentitytoken = #355;` |
| [`parseentitydata`](../37-quakec-builtins-reference/03-entity-world-builtins.md#parseentitydata) | `float(entity e, string s, optional float offset) parseentitydata = #613;` |
| [`getentity`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getentity) | `__variant(float entnum, float fieldnum) getentity = #504;` |
| [`resourcestatus`](../37-quakec-builtins-reference/03-entity-world-builtins.md#resourcestatus) | `float(float resourcetype, float tryload, string resourcename) resourcestatus = #286;` |
| [`physics_addforce`](../37-quakec-builtins-reference/03-entity-world-builtins.md#physics_addforce) | `void(entity e, vector force, vector relative_ofs) physics_addforce = #541;` |
| [`physics_addtorque`](../37-quakec-builtins-reference/03-entity-world-builtins.md#physics_addtorque) | `void(entity e, vector torque) physics_addtorque = #542;` |
| [`physics_enable`](../37-quakec-builtins-reference/03-entity-world-builtins.md#physics_enable) | `void(entity e, float physics_enabled) physics_enable = #540;` |
| [`terrain_edit`](../37-quakec-builtins-reference/03-entity-world-builtins.md#terrain_edit) | `__variant(float action, optional vector pos, optional float radius, optional float quant, ...) terrain_edit = #278;` |
| [`setattachment`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setattachment) | `void(entity e, entity tagentity, string tagname) setattachment = #443;` |
| [`checkcommand`](../37-quakec-builtins-reference/03-entity-world-builtins.md#checkcommand) | `float(string name) checkcommand = #294;` |
| [`registercommand`](../37-quakec-builtins-reference/03-entity-world-builtins.md#registercommand) | `void(string cmdname, optional string desc) registercommand = #352;` |
| [`isfunction`](../37-quakec-builtins-reference/03-entity-world-builtins.md#isfunction) | `float(string s) isfunction = #607;` |
| [`callfunction`](../37-quakec-builtins-reference/03-entity-world-builtins.md#callfunction) | `void(.../*, string funcname*/) callfunction = #605;` |
| [`externcall`](../37-quakec-builtins-reference/03-entity-world-builtins.md#externcall) | `__variant(float prnum, string funcname, ...) externcall = #201;` |
| [`externset`](../37-quakec-builtins-reference/03-entity-world-builtins.md#externset) | `void(float prnum, __variant newval, string varname) externset = #204;` |
| [`externvalue`](../37-quakec-builtins-reference/03-entity-world-builtins.md#externvalue) | `__variant(float prnum, string varname) externvalue = #203;` |
| [`builtin_find`](../37-quakec-builtins-reference/03-entity-world-builtins.md#builtin_find) | `float(string builtinname) builtin_find = #100;` |
| [`changelevel`](../37-quakec-builtins-reference/03-entity-world-builtins.md#changelevel) | `void(string mapname, optional string newmapstartspot) changelevel = #70;` |
| [`chat`](../37-quakec-builtins-reference/03-entity-world-builtins.md#chat) | `void(string filename, float starttag, entity edict) chat = #214;` |
| [`empty`](../37-quakec-builtins-reference/03-entity-world-builtins.md#empty) | `void() empty = #245..#249;` |
| [`entityfieldref`](../37-quakec-builtins-reference/03-entity-world-builtins.md#entityfieldref) | `field_t(float fieldnum) entityfieldref = #0:entityfieldref;` |
| [`entityprotection`](../37-quakec-builtins-reference/03-entity-world-builtins.md#entityprotection) | `float(entity e, float nowreadonly) entityprotection = #0:entityprotection;` |
| [`eprint`](../37-quakec-builtins-reference/03-entity-world-builtins.md#eprint) | `void(entity e) eprint = #31;` |
| [`find_list`](../37-quakec-builtins-reference/03-entity-world-builtins.md#find_list) | `entity*(.__variant fld, __variant match, int type=EV_STRING, __out int count) find_list = #0:find_list;` |
| [`findentity`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findentity) | `findentity` — alias из `fteextensions.qc` для `entity(entity start, .__variant fld, __variant match) findfloat = #98;` |
| [`findentityfield`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findentityfield) | `float(string fieldname) findentityfield = #0:findentityfield;` |
| [`findradius_list`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findradius_list) | `entity*(vector org, float rad, __out int foundcount, int sort=0) findradius_list = #0:findradius_list;` |
| [`generateentitydata`](../37-quakec-builtins-reference/03-entity-world-builtins.md#generateentitydata) | `string(entity e) generateentitydata = #0:generateentitydata;` |
| [`plaque_draw`](../37-quakec-builtins-reference/03-entity-world-builtins.md#plaque_draw) | `void(entity targ, float stringno) plaque_draw = #79;` |
| [`pushmove`](../37-quakec-builtins-reference/03-entity-world-builtins.md#pushmove) | `float(entity pusher, vector move, vector amove) pushmove = #0;` |
| [`qtest_canreach`](../37-quakec-builtins-reference/03-entity-world-builtins.md#qtest_canreach) | `DEP float(vector v) qtest_canreach = #39;` |
| [`readserverentitystate`](../37-quakec-builtins-reference/03-entity-world-builtins.md#readserverentitystate) | `void(float flags, float simtime) readserverentitystate = #369;` |
| [`readsingleentitystate`](../37-quakec-builtins-reference/03-entity-world-builtins.md#readsingleentitystate) | `readsingleentitystate` — незарегистрированный закомментированный слот `#370` из старого `EXT_CSQC_1`. |
| [`removeentity`](../37-quakec-builtins-reference/03-entity-world-builtins.md#removeentity) | `void(entity ent) removeentity = #0:removeentity;` |
| [`route_calculate`](../37-quakec-builtins-reference/03-entity-world-builtins.md#route_calculate) | `void(entity ent, vector dest, int denylinkflags, void(entity ent, vector dest, int numnodes, nodeslist_t *nodelist) callback) route_calculate = #0:route_calculate;` |
| [`runclientphys`](../37-quakec-builtins-reference/03-entity-world-builtins.md#runclientphys) | `runclientphys` — это внутреннее имя реализации; в QuakeC рабочий builtin называется `void(entity ent) runstandardplayerphysics = #347;` |
| [`te_gunshotquad`](../37-quakec-builtins-reference/03-entity-world-builtins.md#te_gunshotquad) | `void(vector org) te_gunshotquad = #412;` |
| [`te_lightning2`](../37-quakec-builtins-reference/03-entity-world-builtins.md#te_lightning2) | `void(entity own, vector start, vector end) te_lightning2 = #429;` |
| [`te_lightning3`](../37-quakec-builtins-reference/03-entity-world-builtins.md#te_lightning3) | `void(entity own, vector start, vector end) te_lightning3 = #430;` |
| [`te_muzzleflash`](../37-quakec-builtins-reference/03-entity-world-builtins.md#te_muzzleflash) | `void(entity ent) te_muzzleflash = #0:te_muzzleflash;` |
| [`te_spikequad`](../37-quakec-builtins-reference/03-entity-world-builtins.md#te_spikequad) | `void(vector org) te_spikequad = #413;` |
| [`te_superspikequad`](../37-quakec-builtins-reference/03-entity-world-builtins.md#te_superspikequad) | `void(vector org) te_superspikequad = #414;` |
| [`undefined`](../37-quakec-builtins-reference/03-entity-world-builtins.md#undefined) | `undefined` — это не рабочий builtin, а метка зарезервированных слотов под номерами `#458`, `#470`, `#505..#509` и `#539`. |

### Сеть и сетевые сообщения

| Элемент | Сигнатура / Описание |
|---|---|
| [`WriteByte`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writebyte) | `void(float to, float val) WriteByte = #52;` |
| [`WriteChar`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writechar) | `void(float to, float val) WriteChar = #53;` |
| [`WriteShort`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writeshort) | `void(float to, float val) WriteShort = #54;` |
| [`WriteLong`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writelong) | `void(float to, float val) WriteLong = #55;` |
| [`WriteAngle`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writeangle) | `void(float to, float val) WriteAngle = #57;` |
| [`WriteCoord`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writecoord) | `void(float to, float val) WriteCoord = #56;` |
| [`WriteString`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writestring) | `void(float to, string val) WriteString = #58;` |
| [`WriteEntity`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writeentity) | `void(float to, entity val) WriteEntity = #59;` |
| [`WriteFloat`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writefloat) | `void(float buf, float fl) WriteFloat = #280;` |
| [`WritePicture`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writepicture) | `void(float to, string s, float sz) WritePicture = #501;` |
| [`WriteUnterminatedString`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writeunterminatedstring) | `void(float target, string str) WriteUnterminatedString = #456;` |
| [`readbyte`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readbyte) | `float() readbyte = #360;` |
| [`readchar`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readchar) | `float() readchar = #361;` |
| [`readshort`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readshort) | `float() readshort = #362;` |
| [`readlong`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readlong) | `float() readlong = #363;` |
| [`readangle`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readangle) | `float() readangle = #365;` |
| [`readcoord`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readcoord) | `float() readcoord = #364;` |
| [`readfloat`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readfloat) | `float() readfloat = #367;` |
| [`readstring`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readstring) | `string() readstring = #366;` |
| [`readentitynum`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readentitynum) | `float() readentitynum = #368;` |
| [`ReadPicture`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readpicture) | `string() ReadPicture = #501;` |
| [`multicast`](../37-quakec-builtins-reference/04-network-messages-builtins.md#multicast) | `void(vector where, float set) multicast = #82;` |
| [`stuffcmd`](../37-quakec-builtins-reference/04-network-messages-builtins.md#stuffcmd) | `void(entity client, string s, optional string s2, optional string s3, optional string s4, optional string s5, optional string s6, optional string s7) stuffcmd = #21;` |
| [`clientcommand`](../37-quakec-builtins-reference/04-network-messages-builtins.md#clientcommand) | `void(entity e, string s) clientcommand = #440;` |
| [`sendevent`](../37-quakec-builtins-reference/04-network-messages-builtins.md#sendevent) | `void(string evname, string evargs, ...) sendevent = #359;` |
| [`sendpacket`](../37-quakec-builtins-reference/04-network-messages-builtins.md#sendpacket) | `float(string destaddress, string content) sendpacket = #242;` |
| [`deltalisten`](../37-quakec-builtins-reference/04-network-messages-builtins.md#deltalisten) | `float(string modelname, float(float isnew) updatecallback, float flags) deltalisten = #371;` |
| [`netaddress_resolve`](../37-quakec-builtins-reference/04-network-messages-builtins.md#netaddress_resolve) | `string(string dnsname, optional float defport) netaddress_resolve = #625;` |
| [`getextresponse`](../37-quakec-builtins-reference/04-network-messages-builtins.md#getextresponse) | `string() getextresponse = #624;` |
| [`redirectcmd`](../37-quakec-builtins-reference/04-network-messages-builtins.md#redirectcmd) | `DEP void(entity to, string str) redirectcmd = #101;` |
| [`isserver`](../37-quakec-builtins-reference/04-network-messages-builtins.md#isserver) | `float() isserver = #60;` |
| [`clientcount`](../37-quakec-builtins-reference/04-network-messages-builtins.md#clientcount) | `float() clientcount = #61;` |
| [`clientstate`](../37-quakec-builtins-reference/04-network-messages-builtins.md#clientstate) | `float() clientstate = #62;` |
| [`clienttype`](../37-quakec-builtins-reference/04-network-messages-builtins.md#clienttype) | `float(entity client) clienttype = #455;` |
| [`isdemo`](../37-quakec-builtins-reference/04-network-messages-builtins.md#isdemo) | `float() isdemo = #349;` |
| [`isbackbuffered`](../37-quakec-builtins-reference/04-network-messages-builtins.md#isbackbuffered) | `float(entity player) isbackbuffered = #234;` |
| [`csqc_cvar_defstring`](../37-quakec-builtins-reference/04-network-messages-builtins.md#csqc_cvar_defstring) | `string(string s) csqc_cvar_defstring = #482;` |
| [`cvars_haveunsaved`](../37-quakec-builtins-reference/04-network-messages-builtins.md#cvars_haveunsaved) | `float() cvars_haveunsaved = #0:cvars_haveunsaved;` |
| [`findkeysforcommand_dp`](../37-quakec-builtins-reference/04-network-messages-builtins.md#findkeysforcommand_dp) | `DEP string(string command, optional float bindmap) findkeysforcommand_dp = #610;` |
| [`findkeysforcommand_menu`](../37-quakec-builtins-reference/04-network-messages-builtins.md#findkeysforcommand_menu) | `string(string command, optional float bindmap) findkeysforcommand_menu = #610;` |
| [`findkeysforcommandex`](../37-quakec-builtins-reference/04-network-messages-builtins.md#findkeysforcommandex) | `string(string command, optional float bindmap) findkeysforcommandex = #0:findkeysforcommandex;` |
| [`forceinfokeyblob`](../37-quakec-builtins-reference/04-network-messages-builtins.md#forceinfokeyblob) | `void(entity player, string key, void *data, int size) forceinfokeyblob = #0:forceinfokeyblob;` |
| [`getlocaluserinfo`](../37-quakec-builtins-reference/04-network-messages-builtins.md#getlocaluserinfo) | `string(float seat, string keyname) getlocaluserinfo = #0:getlocaluserinfo;` |
| [`getlocaluserinfoblob`](../37-quakec-builtins-reference/04-network-messages-builtins.md#getlocaluserinfoblob) | `int(float seat, string keyname, void *outptr, int maxsize) getlocaluserinfoblob = #0:getlocaluserinfoblob;` |
| [`getplayerkeyblob`](../37-quakec-builtins-reference/04-network-messages-builtins.md#getplayerkeyblob) | `int(float playernum, string keyname, optional void *outptr, int size) getplayerkeyblob = #0:getplayerkeyblob;` |
| [`getplayerkeyfloat`](../37-quakec-builtins-reference/04-network-messages-builtins.md#getplayerkeyfloat) | `float(float playernum, string keyname, optional float assumevalue) getplayerkeyfloat = #0:getplayerkeyfloat;` |
| [`getplayerkeyvalue`](../37-quakec-builtins-reference/04-network-messages-builtins.md#getplayerkeyvalue) | `string(float playernum, string keyname) getplayerkeyvalue = #348;` |
| [`getplayerstat`](../37-quakec-builtins-reference/04-network-messages-builtins.md#getplayerstat) | `__variant(float playernum, float statnum, float stattype) getplayerstat = #0:getplayerstat;` |
| [`readdouble`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readdouble) | `__double() readdouble = #0:readdouble;` |
| [`readint`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readint) | `int() readint = #0:readint;` |
| [`readint64`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readint64) | `__int64() readint64 = #0:readint64;` |
| [`readuint64`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readuint64) | `__uint64() readuint64 = #0;` |
| [`serverkeyblob`](../37-quakec-builtins-reference/04-network-messages-builtins.md#serverkeyblob) | `int(string key, optional void *ptr, int maxsize) serverkeyblob = #0:serverkeyblob;` |
| [`serverkeyfloat`](../37-quakec-builtins-reference/04-network-messages-builtins.md#serverkeyfloat) | `float(string key, optional float assumevalue) serverkeyfloat = #0:serverkeyfloat;` |
| [`setlocaluserinfo`](../37-quakec-builtins-reference/04-network-messages-builtins.md#setlocaluserinfo) | `void(float seat, string keyname, string newvalue) setlocaluserinfo = #0:setlocaluserinfo;` |
| [`setlocaluserinfoblob`](../37-quakec-builtins-reference/04-network-messages-builtins.md#setlocaluserinfoblob) | `void(float seat, string keyname, void *outptr, int size) setlocaluserinfoblob = #0:setlocaluserinfoblob;` |
| [`uri_get`](../37-quakec-builtins-reference/04-network-messages-builtins.md#uri_get) | `float(string uril, float id, optional string postmimetype, optional string postdata) uri_get = #513;` |
| [`uri_post`](../37-quakec-builtins-reference/04-network-messages-builtins.md#uri_post) | `float(string uril, float id, optional string postmimetype, optional string postdata, optional float strbuf) uri_post = #513;` |

### Звук

| Элемент | Сигнатура / Описание |
|---|---|
| [`sound`](../37-quakec-builtins-reference/05-sound-builtins.md#sound) | `void(entity e, float chan, string samp, float vol, float atten, optional float speedpct, optional float flags, optional float timeofs) sound = #8;` |
| [`ambientsound`](../37-quakec-builtins-reference/05-sound-builtins.md#ambientsound) | `void (vector pos, string samp, float vol, float atten) ambientsound = #74;` |
| [`localsound`](../37-quakec-builtins-reference/05-sound-builtins.md#localsound) | `void(string soundname, optional float channel, optional float volume) localsound = #177;` |
| [`pointsound`](../37-quakec-builtins-reference/05-sound-builtins.md#pointsound) | `void(vector origin, string sample, float volume, float attenuation) pointsound = #483;` |
| [`soundlength`](../37-quakec-builtins-reference/05-sound-builtins.md#soundlength) | `float(string sample) soundlength = #534;` |
| [`getsoundtime`](../37-quakec-builtins-reference/05-sound-builtins.md#getsoundtime) | `float(entity e, float channel) getsoundtime = #533;` |
| [`precache_sound`](../37-quakec-builtins-reference/05-sound-builtins.md#precache_sound) | `string(string s) precache_sound = #19;` |
| [`precache_sound2`](../37-quakec-builtins-reference/05-sound-builtins.md#precache_sound2) | `string(string str) precache_sound2 = #76;` |
| [`SetListener`](../37-quakec-builtins-reference/05-sound-builtins.md#setlistener) | `void(vector origin, vector forward, vector right, vector up, optional float reverbtype) SetListener = #351;` |
| [`getchannellevel`](../37-quakec-builtins-reference/05-sound-builtins.md#getchannellevel) | `float(entity e, float channel) getchannellevel = #0:getchannellevel;` |
| [`getqueuedaudiotime`](../37-quakec-builtins-reference/05-sound-builtins.md#getqueuedaudiotime) | `float() getqueuedaudiotime = #0:getqueuedaudiotime;` |
| [`getsoundindex`](../37-quakec-builtins-reference/05-sound-builtins.md#getsoundindex) | `float(string soundname, optional float queryonly) getsoundindex = #0:getsoundindex;` |
| [`queueaudio`](../37-quakec-builtins-reference/05-sound-builtins.md#queueaudio) | `float(int hz, int channels, int type, void *data, unsigned int frames) queueaudio = #0:queueaudio;` |
| [`setup_reverb`](../37-quakec-builtins-reference/05-sound-builtins.md#setup_reverb) | `void(float reverbslot, reverbinfo_t *reverbinfo, int sizeofreverbinfo_t) setup_reverb = #0:setup_reverb;` |
| [`soundnameforindex`](../37-quakec-builtins-reference/05-sound-builtins.md#soundnameforindex) | `string(float sndindex) soundnameforindex = #0:soundnameforindex;` |
| [`soundupdate`](../37-quakec-builtins-reference/05-sound-builtins.md#soundupdate) | `float(entity e, float channel, string newsample, float volume, float attenuation, float pitchpct, float flags, float timeoffset) soundupdate = #0:soundupdate;` |
| [`stopsound`](../37-quakec-builtins-reference/05-sound-builtins.md#stopsound) | `void(entity ent, float channel) stopsound = #0:stopsound;` |

### Файлы, буферы, хеш-таблицы и базы данных

| Элемент | Сигнатура / Описание |
|---|---|
| [`fopen`](../37-quakec-builtins-reference/06-files-database-builtins.md#fopen) | `filestream(string filename, float mode, optional float mmapminsize) fopen = #110;` |
| [`fclose`](../37-quakec-builtins-reference/06-files-database-builtins.md#fclose) | `void(filestream fhandle) fclose = #111;` |
| [`fgets`](../37-quakec-builtins-reference/06-files-database-builtins.md#fgets) | `string(filestream fhandle) fgets = #112;` |
| [`fputs`](../37-quakec-builtins-reference/06-files-database-builtins.md#fputs) | `void(filestream fhandle, string s, optional string s2, optional string s3, optional string s4, optional string s5, optional string s6, optional string s7) fputs = #113;` |
| [`fexists`](../37-quakec-builtins-reference/06-files-database-builtins.md#fexists) | `float(string fname) fexists = #653;` |
| [`fcopy`](../37-quakec-builtins-reference/06-files-database-builtins.md#fcopy) | `float(string src, string dst) fcopy = #650;` |
| [`fremove`](../37-quakec-builtins-reference/06-files-database-builtins.md#fremove) | `float(string fname) fremove = #652;` |
| [`frename`](../37-quakec-builtins-reference/06-files-database-builtins.md#frename) | `float(string src, string dst) frename = #651;` |
| [`rmtree`](../37-quakec-builtins-reference/06-files-database-builtins.md#rmtree) | `float(string path) rmtree = #654;` |
| [`writetofile`](../37-quakec-builtins-reference/06-files-database-builtins.md#writetofile) | `void(filestream fh, entity e) writetofile = #606;` |
| [`loadfromfile`](../37-quakec-builtins-reference/06-files-database-builtins.md#loadfromfile) | `void(string s) loadfromfile = #530;` |
| [`loadfromdata`](../37-quakec-builtins-reference/06-files-database-builtins.md#loadfromdata) | `void(string s) loadfromdata = #529;` |
| [`whichpack`](../37-quakec-builtins-reference/06-files-database-builtins.md#whichpack) | `string(string filename, optional enumflags:float{WP_REFERENCEPACKAGE,WP_FULLPACKAGEPATH} flags) whichpack = #503;` |
| [`search_begin`](../37-quakec-builtins-reference/06-files-database-builtins.md#search_begin) | `searchhandle(string pattern, enumflags:float{SB_CASEINSENSITIVE=1<<0,SB_FULLPACKAGEPATH=1<<1,SB_ALLOWDUPES=1<<2,SB_FORCESEARCH=1<<3,SB_MULTISEARCH=1<<4} flags, float quiet, optional string filterpackage) search_begin = #444;` |
| [`search_end`](../37-quakec-builtins-reference/06-files-database-builtins.md#search_end) | `void(searchhandle handle) search_end = #445;` |
| [`search_getsize`](../37-quakec-builtins-reference/06-files-database-builtins.md#search_getsize) | `float(searchhandle handle) search_getsize = #446;` |
| [`search_getfilename`](../37-quakec-builtins-reference/06-files-database-builtins.md#search_getfilename) | `string(searchhandle handle, float num) search_getfilename = #447;` |
| [`buf_create`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_create) | `strbuf() buf_create = #460;` |
| [`buf_del`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_del) | `void(strbuf bufhandle) buf_del = #461;` |
| [`buf_getsize`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_getsize) | `float(strbuf bufhandle) buf_getsize = #462;` |
| [`buf_copy`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_copy) | `void(strbuf bufhandle_from, strbuf bufhandle_to) buf_copy = #463;` |
| [`buf_loadfile`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_loadfile) | `float(string filename, strbuf bufhandle) buf_loadfile = #535;` |
| [`buf_writefile`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_writefile) | `float(filestream filehandle, strbuf bufhandle, optional float startpos, optional float numstrings) buf_writefile = #536;` |
| [`buf_sort`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_sort) | `void(strbuf bufhandle, float sortprefixlen, float backward) buf_sort = #464;` |
| [`buf_implode`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_implode) | `string(strbuf bufhandle, string glue) buf_implode = #465;` |
| [`buf_cvarlist`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_cvarlist) | `void(strbuf strbuf, string pattern, string antipattern) buf_cvarlist = #517;` |
| [`bufstr_add`](../37-quakec-builtins-reference/06-files-database-builtins.md#bufstr_add) | `float(strbuf bufhandle, string str, float ordered) bufstr_add = #468;` |
| [`bufstr_free`](../37-quakec-builtins-reference/06-files-database-builtins.md#bufstr_free) | `void(strbuf bufhandle, float string_index) bufstr_free = #469;` |
| [`bufstr_get`](../37-quakec-builtins-reference/06-files-database-builtins.md#bufstr_get) | `string(strbuf bufhandle, float string_index) bufstr_get = #466;` |
| [`bufstr_set`](../37-quakec-builtins-reference/06-files-database-builtins.md#bufstr_set) | `void(strbuf bufhandle, float string_index, string str) bufstr_set = #467;` |
| [`bufstr_find`](../37-quakec-builtins-reference/06-files-database-builtins.md#bufstr_find) | `float(float bufhandle, string match, float matchrule, float startpos, float step) bufstr_find = #537;` |
| [`hash_createtab`](../37-quakec-builtins-reference/06-files-database-builtins.md#hash_createtab) | `hashtable(float tabsize, optional float defaulttype) hash_createtab = #287;` |
| [`hash_destroytab`](../37-quakec-builtins-reference/06-files-database-builtins.md#hash_destroytab) | `void(hashtable table) hash_destroytab = #288;` |
| [`hash_add`](../37-quakec-builtins-reference/06-files-database-builtins.md#hash_add) | `void(hashtable table, string name, __variant value, optional float typeandflags) hash_add = #289;` |
| [`hash_delete`](../37-quakec-builtins-reference/06-files-database-builtins.md#hash_delete) | `__variant(hashtable table, string name) hash_delete = #291;` |
| [`hash_get`](../37-quakec-builtins-reference/06-files-database-builtins.md#hash_get) | `__variant(hashtable table, string name, optional __variant deflt, optional float requiretype, optional float index) hash_get = #290;` |
| [`hash_getkey`](../37-quakec-builtins-reference/06-files-database-builtins.md#hash_getkey) | `string(hashtable table, float idx) hash_getkey = #292;` |
| [`memalloc`](../37-quakec-builtins-reference/06-files-database-builtins.md#memalloc) | `__variant*(int size) memalloc = #384;` |
| [`memfree`](../37-quakec-builtins-reference/06-files-database-builtins.md#memfree) | `void(__variant *ptr) memfree = #385;` |
| [`memcpy`](../37-quakec-builtins-reference/06-files-database-builtins.md#memcpy) | `void(__variant *dst, __variant *src, int size) memcpy = #386;` |
| [`memfill8`](../37-quakec-builtins-reference/06-files-database-builtins.md#memfill8) | `void(__variant *dst, int val, int size) memfill8 = #387;` |
| [`memgetval`](../37-quakec-builtins-reference/06-files-database-builtins.md#memgetval) | `__variant(__variant *dst, float ofs) memgetval = #388;` |
| [`memsetval`](../37-quakec-builtins-reference/06-files-database-builtins.md#memsetval) | `void(__variant *dst, float ofs, __variant val) memsetval = #389;` |
| [`memptradd`](../37-quakec-builtins-reference/06-files-database-builtins.md#memptradd) | `__variant*(__variant *base, float ofs) memptradd = #390;` |
| [`sqlconnect`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlconnect) | `float(optional string host, optional string user, optional string pass, optional string defaultdb, optional string driver) sqlconnect = #250;` |
| [`sqldisconnect`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqldisconnect) | `void(float serveridx) sqldisconnect = #251;` |
| [`sqlopenquery`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlopenquery) | `float(float serveridx, void(float serveridx, float queryidx, float rows, float columns, float eof, float firstrow) callback, float querytype, string query) sqlopenquery = #252;` |
| [`sqlclosequery`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlclosequery) | `void(float serveridx, float queryidx) sqlclosequery = #253;` |
| [`sqlreadfield`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlreadfield) | `string(float serveridx, float queryidx, float row, float column) sqlreadfield = #254;` |
| [`sqlreadfloat`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlreadfloat) | `float(float serveridx, float queryidx, float row, float column) sqlreadfloat = #258;` |
| [`sqlerror`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlerror) | `string(float serveridx, optional float queryidx) sqlerror = #255;` |
| [`sqlescape`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlescape) | `string(float serveridx, string data) sqlescape = #256;` |
| [`sqlversion`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlversion) | `string(float serveridx) sqlversion = #257;` |
| [`digest_hex`](../37-quakec-builtins-reference/06-files-database-builtins.md#digest_hex) | `string(string digest, string data, ...) digest_hex = #639;` |
| [`fork`](../37-quakec-builtins-reference/06-files-database-builtins.md#fork) | `float(optional float sleeptime) fork = #210;` |
| [`sleep`](../37-quakec-builtins-reference/06-files-database-builtins.md#sleep) | `void(float sleeptime) sleep = #212;` |
| [`createbuffer`](../37-quakec-builtins-reference/06-files-database-builtins.md#createbuffer) | `void*(int bytes) createbuffer = #0:createbuffer;` |
| [`digest_ptr`](../37-quakec-builtins-reference/06-files-database-builtins.md#digest_ptr) | `string(string digest, void *data, int length, optional int offset) digest_ptr = #0:digest_ptr;` |
| [`fread`](../37-quakec-builtins-reference/06-files-database-builtins.md#fread) | `int(filestream fhandle, void *ptr, int size, optional int offset) fread = #0:fread;` |
| [`fseek`](../37-quakec-builtins-reference/06-files-database-builtins.md#fseek) | `int(filestream fhandle, optional int newoffset) fseek = #0:fseek;` |
| [`fseek64`](../37-quakec-builtins-reference/06-files-database-builtins.md#fseek64) | `__int64(filestream fhandle, optional __int64 newoffset) fseek64 = #0:fseek64;` |
| [`fsize`](../37-quakec-builtins-reference/06-files-database-builtins.md#fsize) | `int(filestream fhandle, optional int newsize) fsize = #0:fsize;` |
| [`fsize64`](../37-quakec-builtins-reference/06-files-database-builtins.md#fsize64) | `__int64(filestream fhandle, optional __int64 newsize) fsize64 = #0:fsize64;` |
| [`fwrite`](../37-quakec-builtins-reference/06-files-database-builtins.md#fwrite) | `int(filestream fhandle, void *ptr, int size, optional int offset) fwrite = #0:fwrite;` |
| [`hash_getcb`](../37-quakec-builtins-reference/06-files-database-builtins.md#hash_getcb) | `void(hashtable table, void(string keyname, __variant val) callback, optional string name) hash_getcb = #293;` |
| [`json_find_object_child`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_find_object_child) | `jsonnode(jsonnode node, string name) json_find_object_child = #0:json_find_object_child;` |
| [`json_free`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_free) | `void(jsonnode node) json_free = #0:json_free;` |
| [`json_get_child_at_index`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_get_child_at_index) | `jsonnode(jsonnode node, int childindex) json_get_child_at_index = #0:json_get_child_at_index;` |
| [`json_get_float`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_get_float) | `float(jsonnode node) json_get_float = #0:json_get_float;` |
| [`json_get_integer`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_get_integer) | `int(jsonnode node) json_get_integer = #0:json_get_integer;` |
| [`json_get_length`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_get_length) | `int(jsonnode node) json_get_length = #0:json_get_length;` |
| [`json_get_name`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_get_name) | `string(jsonnode node) json_get_name = #0:json_get_name;` |
| [`json_get_string`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_get_string) | `string(jsonnode node) json_get_string = #0:json_get_string;` |
| [`json_get_value_type`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_get_value_type) | `json_type_e(jsonnode node) json_get_value_type = #0:json_get_value_type;` |
| [`json_parse`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_parse) | `jsonnode(string data) json_parse = #0:json_parse;` |
| [`memcmp`](../37-quakec-builtins-reference/06-files-database-builtins.md#memcmp) | `int(__variant *dst, __variant *src, int size, optional int srcoffset, optional int dstoffset) memcmp = #0:memcmp;` |
| [`memrealloc`](../37-quakec-builtins-reference/06-files-database-builtins.md#memrealloc) | `__variant*(void *oldptr, int newsize) memrealloc = #0:memrealloc;` |
| [`memstrsize`](../37-quakec-builtins-reference/06-files-database-builtins.md#memstrsize) | `float(string s) memstrsize = #0:memstrsize;` |
| [`search_fopen`](../37-quakec-builtins-reference/06-files-database-builtins.md#search_fopen) | `filestream(searchhandle handle, float num) search_fopen = #0:search_fopen;` |
| [`search_getfilemtime`](../37-quakec-builtins-reference/06-files-database-builtins.md#search_getfilemtime) | `string(searchhandle handle, float num) search_getfilemtime = #0:search_getfilemtime;` |
| [`search_getfilesize`](../37-quakec-builtins-reference/06-files-database-builtins.md#search_getfilesize) | `float(searchhandle handle, float num) search_getfilesize = #0:search_getfilesize;` |
| [`search_getpackagename`](../37-quakec-builtins-reference/06-files-database-builtins.md#search_getpackagename) | `string(searchhandle handle, float num) search_getpackagename = #0:search_getpackagename;` |
| [`sqlescapeblob`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlescapeblob) | `string(float serveridx, __variant *ptr, int maxsize) sqlescapeblob = #0:sqlescapeblob;` |
| [`sqlreadblob`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlreadblob) | `int(float serveridx, float queryidx, float row, float column, __variant *ptr, int maxsize) sqlreadblob = #0:sqlreadblob;` |

### Прекэш и игровые ресурсы

| Элемент | Сигнатура / Описание |
|---|---|
| [`precache_file`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#precache_file) | `string(string s) precache_file = #68;` |
| [`precache_file2`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#precache_file2) | `string(string str) precache_file2 = #77;` |
| [`precache_model`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#precache_model) | `string(string s) precache_model = #20;` |
| [`precache_model2`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#precache_model2) | `string(string str) precache_model2 = #75;` |
| [`precache_pic`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#precache_pic) | `string(string name, optional float flags) precache_pic = #317;` |
| [`precache_vwep_model`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#precache_vwep_model) | `float(string mname) precache_vwep_model = #532;` |
| [`getmodelindex`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#getmodelindex) | `float(string modelname, optional float queryonly) getmodelindex = #200;` |
| [`modelnameforindex`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#modelnameforindex) | `string(float mdlindex) modelnameforindex = #334;` |
| [`frameforname`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#frameforname) | `float(float modidx, string framename) frameforname = #276;` |
| [`frametoname`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#frametoname) | `string(float modidx, float framenum) frametoname = #284;` |
| [`frameduration`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#frameduration) | `float(float modidx, float framenum) frameduration = #277;` |
| [`skinforname`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#skinforname) | `float(float mdlindex, string skinname) skinforname = #237;` |
| [`skintoname`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#skintoname) | `string(float modidx, float skin) skintoname = #285;` |
| [`shaderforname`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#shaderforname) | `float(string shadername, optional string defaultshader, ...) shaderforname = #238;` |
| [`findfont`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#findfont) | `float(string s) findfont = #356;` |
| [`loadfont`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#loadfont) | `float(string fontname, string fontmaps, string sizes, float slot, optional float fix_scale, optional float fix_voffset) loadfont = #357;` |
| [`changepic`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#changepic) | `DEP_CSQC void(string slot, string picname, optional entity player) changepic = #107;` |
| [`drawgetimagesize`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#drawgetimagesize) | `vector(string picname) drawgetimagesize = #318;` |
| [`iscachedpic`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#iscachedpic) | `float(string name) iscachedpic = #316;` |
| [`freepic`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#freepic) | `void(string name) freepic = #319;` |
| [`addprogs`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#addprogs) | `float(string progsname) addprogs = #202;` |
| [`frameforaction`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#frameforaction) | `float(float modidx, int actionid) frameforaction = #0:frameforaction;` |
| [`getmodeleventidx`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#getmodeleventidx) | `float(float modidx, float framenum, int eventidx, __out float timestamp, __out int code, __out string data) getmodeleventidx = #0:getmodeleventidx;` |
| [`getnextmodelevent`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#getnextmodelevent) | `float(float modidx, float framenum, __inout float basetime, float targettime, __out int code, __out string data) getnextmodelevent = #0:getnextmodelevent;` |
| [`modelframecount`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#modelframecount) | `float(float mdlidx) modelframecount = #0:modelframecount;` |
| [`processmodelevents`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#processmodelevents) | `void(float modidx, float framenum, __inout float basetime, float targettime, void(float timestamp, int code, string data) callback) processmodelevents = #0:processmodelevents;` |
| [`spriteframe`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#spriteframe) | `string(string modelname, int frame, float frametime) spriteframe = #0:spriteframe;` |

### Рендеринг и сцена CSQC

| Элемент | Сигнатура / Описание |
|---|---|
| [`addentity`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#addentity) | `void(entity ent) addentity = #302;` |
| [`addentities`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#addentities) | `void(float mask) addentities = #301;` |
| [`clearscene`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#clearscene) | `void() clearscene = #300;` |
| [`renderscene`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#renderscene) | `void() renderscene = #304;` |
| [`getproperty`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#getproperty) | `__variant(float property) getproperty = #309;` (алиас `getviewprop`) |
| [`setproperty`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#setproperty) | `float(float property, ...) setproperty = #303;` (алиас `setviewprop`) |
| [`getresolution`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#getresolution) | `vector(float vidmode, optional float forfullscreen) getresolution = #608;` |
| [`R_BeginPolygon`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#r_beginpolygon) | `void(string texturename, optional float flags, optional float is2d) R_BeginPolygon = #306;` |
| [`R_EndPolygon`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#r_endpolygon) | `void() R_EndPolygon = #308;` |
| [`R_PolygonVertex`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#r_polygonvertex) | `void(vector org, vector texcoords, vector rgb, float alpha) R_PolygonVertex = #307;` |
| [`drawcharacter`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawcharacter) | `float(vector position, float character, vector size, vector rgb, float alpha, optional float drawflag) drawcharacter = #320;` |
| [`drawfill`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawfill) | `float(vector position, vector size, vector rgb, float alpha, optional float drawflag) drawfill = #323;` |
| [`drawline`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawline) | `void(float width, vector pos1, vector pos2, vector rgb, float alpha, optional float drawflag) drawline = #315;` |
| [`drawpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawpic) | `float(vector position, string pic, vector size, vector rgb, float alpha, optional float drawflag) drawpic = #322;` |
| [`drawrawstring`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawrawstring) | `float(vector position, string text, vector size, vector rgb, float alpha, optional float drawflag) drawrawstring = #321;` |
| [`drawresetcliparea`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawresetcliparea) | `void(void) drawresetcliparea = #325;` |
| [`drawsetcliparea`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawsetcliparea) | `void(float x, float y, float width, float height) drawsetcliparea = #324;` |
| [`drawstring`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawstring) | `float(vector position, string text, vector size, vector rgb, float alpha, float drawflag) drawstring = #326;` |
| [`drawsubpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawsubpic) | `void(vector pos, vector sz, string pic, vector srcpos, vector srcsz, vector rgb, float alpha, optional float drawflag) drawsubpic = #328;` |
| [`movepic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#movepic) | `void(string slot, float x, float y, float zone, optional entity player) movepic = #106;` |
| [`showpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#showpic) | `void(string slot, string picname, float x, float y, float zone, optional entity player) showpic = #104;` |
| [`hidepic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#hidepic) | `void(string slot, optional entity player) hidepic = #105;` |
| [`changepic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#changepic) | `void(string slot, string picname, optional entity player) changepic = #107;` |
| [`iscachedpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#iscachedpic) | `float(string name) iscachedpic = #316;` |
| [`freepic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#freepic) | `void(string name) freepic = #319;` |
| [`drawgetimagesize`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawgetimagesize) | `vector(string picname) drawgetimagesize = #318;` (алиас `draw_getimagesize`) |
| [`stringwidth`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#stringwidth) | `float(string text, float usecolours, optional vector fontsize) stringwidth = #327;` |
| [`adddecal`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#adddecal) | `void(string shadername, vector origin, vector up, vector side, vector rgb, float alpha) adddecal = #375;` |
| [`boxparticles`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#boxparticles) | `void(float effectindex, entity own, vector org_from, vector org_to, vector dir_from, vector dir_to, float countmultiplier, optional float flags) boxparticles = #502;` |
| [`particle`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#particle) | `void(vector pos, vector dir, float colour, float count) particle = #48;` |
| [`particle2`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#particle2) | `void(vector org, vector dmin, vector dmax, float colour, float effect, float count) particle2 = #215;` |
| [`particle3`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#particle3) | `void(vector org, vector box, float colour, float effect, float count) particle3 = #216;` |
| [`particle4`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#particle4) | `void(vector org, float radius, float colour, float effect, float count) particle4 = #217;` |
| [`particleeffectnum`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#particleeffectnum) | `float(string effectname) particleeffectnum = #335;` |
| [`particleeffectquery`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#particleeffectquery) | `string(float efnum, float body) particleeffectquery = #374;` |
| [`pointparticles`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#pointparticles) | `void(float effectnum, vector origin, optional vector dir, optional float count) pointparticles = #337;` |
| [`trailparticles`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#trailparticles) | `void(float effectnum, entity ent, vector start, vector end) trailparticles = #336;` |
| [`effect`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#effect) | `void(vector org, string modelname, float startframe, float endframe, float framerate) effect = #404;` |
| [`dynamiclight_add`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#dynamiclight_add) | `float(vector org, float radius, vector lightcolours, optional float style, optional string cubemapname, optional float pflags) dynamiclight_add = #305;` |
| [`dynamiclight_get`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#dynamiclight_get) | `__variant(float lno, float fld) dynamiclight_get = #372;` |
| [`dynamiclight_set`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#dynamiclight_set) | `void(float lno, float fld, __variant value) dynamiclight_set = #373;` |
| [`lightstyle`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#lightstyle) | `void(float lightstyle, string stylestring, optional vector rgb) lightstyle = #35;` |
| [`lightstylestatic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#lightstylestatic) | `void(float style, float val, optional vector rgb) lightstylestatic = #5;` |
| [`getlight`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#getlight) | `vector(vector org) getlight = #92;` |
| [`con_draw`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#con_draw) | `void(string conname, vector pos, vector size, float fontsize) con_draw = #393;` |
| [`con_getset`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#con_getset) | `string(string conname, string field, optional string newvalue) con_getset = #391;` |
| [`con_input`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#con_input) | `float(string conname, float inevtype, float parama, float paramb, float paramc) con_input = #394;` |
| [`con_printf`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#con_printf) | `void(string conname, string messagefmt, ...) con_printf = #392;` |
| [`RegisterTempEnt`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#registertempent) | `float(float attributes, string effectname, ...) RegisterTempEnt = #208;` |
| [`CustomTempEnt`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#customtempent) | `void(float type, vector pos, ...) CustomTempEnt = #209;` |
| [`te_beam`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_beam) | `void(entity own, vector start, vector end) te_beam = #431;` |
| [`te_blood`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_blood) | `void(vector org, vector dir, float count) te_blood = #405;` |
| [`te_bloodqw`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_bloodqw) | `void(vector org, optional float count) te_bloodqw = #239;` |
| [`te_bloodshower`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_bloodshower) | `void(vector mincorner, vector maxcorner, float explosionspeed, float howmany) te_bloodshower = #406;` |
| [`te_customflash`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_customflash) | `void(vector org, float radius, float lifetime, vector color) te_customflash = #417;` |
| [`te_explosion`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_explosion) | `void(vector org) te_explosion = #421;` |
| [`te_explosion2`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_explosion2) | `void(vector org, float color, float colorlength) te_explosion2 = #427;` |
| [`te_explosionquad`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_explosionquad) | `void(vector org) te_explosionquad = #415;` |
| [`te_explosionrgb`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_explosionrgb) | `void(vector org, vector color) te_explosionrgb = #407;` |
| [`te_flamejet`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_flamejet) | `void(vector org, vector vel, float howmany) te_flamejet = #457;` |
| [`te_gunshot`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_gunshot) | `void(vector org, optional float count) te_gunshot = #418;` |
| [`te_knightspike`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_knightspike) | `void(vector org) te_knightspike = #424;` |
| [`te_lavasplash`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_lavasplash) | `void(vector org) te_lavasplash = #425;` |
| [`te_lightning1`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_lightning1) | `void(entity own, vector start, vector end) te_lightning1 = #428;` |
| [`te_lightningblood`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_lightningblood) | `void(vector pos) te_lightningblood = #219;` |
| [`te_particlecube`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_particlecube) | `void(vector mincorner, vector maxcorner, vector vel, float howmany, float color, float gravityflag, float randomveljitter) te_particlecube = #408;` |
| [`te_particlerain`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_particlerain) | `void(vector mincorner, vector maxcorner, vector vel, float howmany, float color) te_particlerain = #409;` |
| [`te_particlesnow`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_particlesnow) | `void(vector mincorner, vector maxcorner, vector vel, float howmany, float color) te_particlesnow = #410;` |
| [`te_plasmaburn`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_plasmaburn) | `void(vector org) te_plasmaburn = #433;` |
| [`te_smallflash`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_smallflash) | `void(vector org) te_smallflash = #416;` |
| [`te_spark`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_spark) | `void(vector org, vector vel, float howmany) te_spark = #411;` |
| [`te_spike`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_spike) | `void(vector org) te_spike = #419;` |
| [`te_superspike`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_superspike) | `void(vector org) te_superspike = #420;` |
| [`te_tarexplosion`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_tarexplosion) | `void(vector org) te_tarexplosion = #422;` |
| [`te_teleport`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_teleport) | `void(vector org) te_teleport = #426;` |
| [`te_wizspike`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_wizspike) | `void(vector org) te_wizspike = #423;` |
| [`addentity_lighting`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#addentity_lighting) | `void(entity ent, vector dir, vector ambient, vector diffuse) addentity_lighting = #0:addentity_lighting;` |
| [`addtrisoup_simple`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#addtrisoup_simple) | `void(string texturename, int flags, trisoup_simple_vert_t *verts, int *indexes, int numindexes) addtrisoup_simple = #0:addtrisoup_simple;` |
| [`customtempent`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#customtempent) | `void(float type, vector pos, ...) CustomTempEnt = #209;` |
| [`drawrotpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawrotpic) | `void(vector pivot, vector mins, vector maxs, string pic, vector rgb, float alpha, float angle, optional float drawflag) drawrotpic = #0:drawrotpic;` |
| [`drawrotpic_dp`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawrotpic_dp) | `void(vector pivot, string pic, vector size, vector mins, float angle, vector rgb, float alpha, optional float drawflag) drawrotpic_dp = #329;` |
| [`drawrotsubpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawrotsubpic) | `void(vector pivot, vector mins, vector maxs, string pic, vector txmin, vector txsize, vector rgb, vector alphaandangles) drawrotsubpic = #0:drawrotsubpic;` |
| [`dynamiclight_spawnstatic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#dynamiclight_spawnstatic) | `float(vector org, float radius, vector rgb) dynamiclight_spawnstatic = #0:dynamiclight_spawnstatic;` |
| [`getlightstyle`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#getlightstyle) | `string(float style, optional __out vector rgb) getlightstyle = #0:getlightstyle;` |
| [`getlightstylergb`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#getlightstylergb) | `vector(float style) getlightstylergb = #0:getlightstylergb;` |
| [`getlocationname`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#getlocationname) | `string(vector org) getlocationname = #0:getlocationname;` |
| [`pointcontentsmask`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#pointcontentsmask) | `__uint(vector org, optional float worldonly) pointcontentsmask = #0:pointcontentsmask;` |
| [`R_EndPolygonRibbon`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#r_endpolygonribbon) | `void(float radius, vector texcoordbias) R_EndPolygonRibbon = #0:R_EndPolygonRibbon;` |
| [`r_readimage`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#r_readimage) | `int*(string filename, __out int width, __out int height, __out int format) r_readimage = #0:r_readimage;` |
| [`r_uploadimage`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#r_uploadimage) | `void(string imagename, int width, int height, void *pixeldata, optional int datasize, optional int format) r_uploadimage = #0:r_uploadimage;` |
| [`registertempent`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#registertempent) | `float(float attributes, string effectname, ...) RegisterTempEnt = #208;` |
| [`remapshader`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#remapshader) | `void(string oldshader, string newshader) remapshader = #0:remapshader;` |
| [`setcolor`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#setcolor) | `setcolor(...)` — данная встроенная функция не реализована и является жесткой заглушкой (номера: CSQC=#401). |
| [`trailparticles_dp`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#trailparticles_dp) | `void(float effectindex, entity ent, vector start, vector end) trailparticles_dp = #336;` |
| [`V_CalcRefdef`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#v_calcrefdef) | `V_CalcRefdef(...)` — данная встроенная функция не реализована и является жесткой заглушкой (номера: CSQC=#640). |

### Ввод, интерфейс и клавиатура CSQC

| Элемент | Сигнатура / Описание |
|---|---|
| [`getinputstate`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getinputstate) | `float(float inputsequencenum, optional float seat) getinputstate = #345;` |
| [`getkeybind`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getkeybind) | `string(float keynum) getkeybind = #342;` |
| [`setkeybind`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setkeybind) | `float(float key, string bind, optional float bindmap, optional float modifier) setkeybind = #630;` |
| [`getkeydest`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getkeydest) | `float() getkeydest = #602;` |
| [`setkeydest`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setkeydest) | `void(float dest) setkeydest = #601;` |
| [`getbindmaps`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getbindmaps) | `vector() getbindmaps = #631;` |
| [`setbindmaps`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setbindmaps) | `float(vector bm) setbindmaps = #632;` |
| [`getmousepos`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getmousepos) | `vector() getmousepos = #66;` |
| [`setmousetarget`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setmousetarget) | `void(float trg) setmousetarget = #603;` |
| [`getmousetarget`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getmousetarget) | `float() getmousetarget = #604;` |
| [`setcursormode`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setcursormode) | `void(float usecursor, optional string cursorimage, optional vector hotspot_and_scale) setcursormode = #343;` |
| [`setsensitivityscaler`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setsensitivityscaler) | `void(float sens) setsensitivityscaler = #346;` |
| [`keynumtostring`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#keynumtostring) | `string(float keynum) keynumtostring = #340;` |
| [`keynumtostring_csqc`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#keynumtostring_csqc) | `string(float keynum) keynumtostring_csqc = #340;` |
| [`keynumtostring_menu`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#keynumtostring_menu) | `string(float keynum) keynumtostring_menu = #609;` |
| [`keynumtostring_omgwtf`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#keynumtostring_omgwtf) | `string(float keynum) keynumtostring_omgwtf = #520;` |
| [`stringtokeynum`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#stringtokeynum) | `float(string keyname) stringtokeynum = #341;` |
| [`stringtokeynum_csqc`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#stringtokeynum_csqc) | `float(string keyname) stringtokeynum_csqc = #341;` |
| [`stringtokeynum_menu`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#stringtokeynum_menu) | `float(string key) stringtokeynum_menu = #614;` |
| [`findkeysforcommand`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#findkeysforcommand) | `string(string command, optional float bindmap) findkeysforcommand = #521;` |
| [`gecko_create`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_create) | `float(string name, optional string initialURI) gecko_create = #487;` |
| [`gecko_destroy`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_destroy) | `void(string name) gecko_destroy = #488;` |
| [`gecko_navigate`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_navigate) | `void(string name, string URI) gecko_navigate = #489;` |
| [`gecko_keyevent`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_keyevent) | `float(string name, float key, float eventtype, optional float charcode) gecko_keyevent = #490;` |
| [`gecko_mousemove`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_mousemove) | `void(string name, float x, float y) gecko_mousemove = #491;` |
| [`gecko_resize`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_resize) | `void(string name, float w, float h) gecko_resize = #492;` |
| [`gecko_get_texture_extent`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_get_texture_extent) | `vector(string name) gecko_get_texture_extent = #493;` |
| [`CL_RotateMoves`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#cl_rotatemoves) | `void(vector anglechange, optional float seat) CL_RotateMoves = #638;` |
| [`clipboard_get`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#clipboard_get) | `void(int cliptype) clipboard_get = #0:clipboard_get;` |
| [`clipboard_set`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#clipboard_set) | `void(int cliptype, string text) clipboard_set = #0:clipboard_set;` |
| [`drawtextfield`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#drawtextfield) | `float(vector pos, vector size, float alignflags, string text) drawtextfield = #0:drawtextfield;` |
| [`gecko_getproperty`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_getproperty) | `string(string shadname, string propname) gecko_getproperty = #0:gecko_getproperty;` |
| [`getcursormode`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getcursormode) | `float(float effective) getcursormode = #0:getcursormode;` |
| [`setmousepos`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setmousepos) | `void(vector newpos) setmousepos = #0:setmousepos;` |
| [`setwindowcaption`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setwindowcaption) | `void(string newcaption) setwindowcaption = #0:setwindowcaption;` |

### Скелетная анимация и модели

| Элемент | Сигнатура / Описание |
|---|---|
| [`skel_build`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_build) | `float(float skel, entity ent, float modelindex, float retainfrac, float firstbone, float lastbone, optional float addfrac) skel_build = #264;` |
| [`skel_copybones`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_copybones) | `void(float skeldst, float skelsrc, float startbone, float entbone) skel_copybones = #274;` |
| [`skel_create`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_create) | `float(float modlindex, optional float useabstransforms) skel_create = #263;` |
| [`skel_delete`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_delete) | `void(float skel) skel_delete = #275;` |
| [`skel_find_bone`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_find_bone) | `float(float skel, string tagname) skel_find_bone = #268;` |
| [`skel_get_boneabs`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_get_boneabs) | `vector(float skel, float bonenum) skel_get_boneabs = #270;` |
| [`skel_get_bonename`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_get_bonename) | `string(float skel, float bonenum) skel_get_bonename = #266;` |
| [`skel_get_boneparent`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_get_boneparent) | `float(float skel, float bonenum) skel_get_boneparent = #267;` |
| [`skel_get_bonerel`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_get_bonerel) | `vector(float skel, float bonenum) skel_get_bonerel = #269;` |
| [`skel_get_numbones`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_get_numbones) | `float(float skel) skel_get_numbones = #265;` |
| [`skel_mmap`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_mmap) | `float*(float skel) skel_mmap = #282;` |
| [`skel_premul_bone`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_premul_bone) | `void(float skel, float bonenum, vector org, optional vector fwd, optional vector right, optional vector up) skel_premul_bone = #272;` |
| [`skel_premul_bones`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_premul_bones) | `void(float skel, float startbone, float endbone, vector org, optional vector fwd, optional vector right, optional vector up) skel_premul_bones = #273;` |
| [`skel_ragupdate`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_ragupdate) | `float(entity skelent, string dollcmd, float animskel) skel_ragupdate = #281;` |
| [`skel_set_bone`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_set_bone) | `void(float skel, float bonenum, vector org, optional vector fwd, optional vector right, optional vector up) skel_set_bone = #271;` |
| [`skel_set_bone_world`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_set_bone_world) | `void(entity ent, float bonenum, vector org, optional vector angorfwd, optional vector right, optional vector up) skel_set_bone_world = #283;` |
| [`gettagindex`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#gettagindex) | `float(entity ent, string tagname) gettagindex = #451;` |
| [`gettaginfo`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#gettaginfo) | `vector(entity ent, float tagindex) gettaginfo = #452;` |
| [`getsurfaceclippedpoint`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfaceclippedpoint) | `vector(entity e, float s, vector p) getsurfaceclippedpoint = #439;` |
| [`getsurfacenearpoint`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfacenearpoint) | `float(entity e, vector p) getsurfacenearpoint = #438;` |
| [`getsurfacenormal`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfacenormal) | `vector(entity e, float s) getsurfacenormal = #436;` |
| [`getsurfacenumpoints`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfacenumpoints) | `float(entity e, float s) getsurfacenumpoints = #434;` |
| [`getsurfacenumtriangles`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfacenumtriangles) | `float(entity e, float s) getsurfacenumtriangles = #628;` |
| [`getsurfacepoint`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfacepoint) | `vector(entity e, float s, float n) getsurfacepoint = #435;` |
| [`getsurfacepointattribute`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfacepointattribute) | `vector(entity e, float s, float n, float a) getsurfacepointattribute = #486;` |
| [`getsurfacetexture`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfacetexture) | `string(entity e, float s) getsurfacetexture = #437;` |
| [`getsurfacetriangle`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfacetriangle) | `vector(entity e, float s, float n) getsurfacetriangle = #629;` |
| [`applycustomskin`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#applycustomskin) | `void(entity e, float skinobj) applycustomskin = #378;` |
| [`loadcustomskin`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#loadcustomskin) | `float(string skinfilename, optional string skindata) loadcustomskin = #377;` |
| [`setcustomskin`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#setcustomskin) | `void(entity e, string skinfilename, optional string skindata) setcustomskin = #376;` |
| [`releasecustomskin`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#releasecustomskin) | `void(float skinobj) releasecustomskin = #379;` |
| [`setcolors`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#setcolors) | `__deprecated("No RGB support.") void(entity ent, float colours) setcolors = #401;` |
| [`skel_build_ptr`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_build_ptr) | `float(float skel, int numblends, skelblend_t *weights, int structsize) skel_build_ptr = #0:skel_build_ptr;` |
| [`skel_postmul_bone`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_postmul_bone) | `void(float skel, float bonenum, vector org, optional vector fwd, optional vector right, optional vector up) skel_postmul_bone = #0:skel_postmul_bone;` |
| [`skel_postmul_bones`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_postmul_bones) | Стандартного публичного объявления QuakeC для `skel_postmul_bones` в штатных defs FTEQW нет; в исходниках есть внутренняя C-реализация диапазонного post-multiply, но обычный QC-код не должен рассчитывать на неё как на доступный builtin. |

### Браузер серверов и мастер-сервер

| Элемент | Сигнатура / Описание |
|---|---|
| [`addwantedhostcachekey`](../37-quakec-builtins-reference/11-server-browser-builtins.md#addwantedhostcachekey) | `void(string key) addwantedhostcachekey = #623;` |
| [`gethostcacheindexforkey`](../37-quakec-builtins-reference/11-server-browser-builtins.md#gethostcacheindexforkey) | `float(string key) gethostcacheindexforkey = #622;` |
| [`gethostcachenumber`](../37-quakec-builtins-reference/11-server-browser-builtins.md#gethostcachenumber) | `float(float fld, float hostnr) gethostcachenumber = #621;` |
| [`gethostcachestring`](../37-quakec-builtins-reference/11-server-browser-builtins.md#gethostcachestring) | `string(float type, float hostnr) gethostcachestring = #612;` |
| [`gethostcachevalue`](../37-quakec-builtins-reference/11-server-browser-builtins.md#gethostcachevalue) | `float(float type) gethostcachevalue = #611;` |
| [`refreshhostcache`](../37-quakec-builtins-reference/11-server-browser-builtins.md#refreshhostcache) | `void(optional float dopurge) refreshhostcache = #620;` |
| [`resethostcachemasks`](../37-quakec-builtins-reference/11-server-browser-builtins.md#resethostcachemasks) | `void() resethostcachemasks = #615;` |
| [`resorthostcache`](../37-quakec-builtins-reference/11-server-browser-builtins.md#resorthostcache) | `void() resorthostcache = #618;` |
| [`sethostcachemasknumber`](../37-quakec-builtins-reference/11-server-browser-builtins.md#sethostcachemasknumber) | `void(float mask, float fld, float num, float op) sethostcachemasknumber = #617;` |
| [`sethostcachemaskstring`](../37-quakec-builtins-reference/11-server-browser-builtins.md#sethostcachemaskstring) | `void(float mask, float fld, string str, float op) sethostcachemaskstring = #616;` |
| [`sethostcachesort`](../37-quakec-builtins-reference/11-server-browser-builtins.md#sethostcachesort) | `void(float fld, float descending) sethostcachesort = #619;` |
| [`getgamedirinfo`](../37-quakec-builtins-reference/11-server-browser-builtins.md#getgamedirinfo) | `string(float n, float prop) getgamedirinfo = #626;` |
| [`getextresponse`](../37-quakec-builtins-reference/11-server-browser-builtins.md#getextresponse) | `string() getextresponse = #624;` |
| [`calltimeofday`](../37-quakec-builtins-reference/11-server-browser-builtins.md#calltimeofday) | `__deprecated("Use strftime.") void() calltimeofday = #231;` |
| [`openportal`](../37-quakec-builtins-reference/11-server-browser-builtins.md#openportal) | `void(entity portal, float state) openportal = #207;` |
| [`getpackagemanagerinfo`](../37-quakec-builtins-reference/11-server-browser-builtins.md#getpackagemanagerinfo) | `string(int n, int prop) getpackagemanagerinfo = #0:getpackagemanagerinfo;` |

### Системные функции, отладка и cvar

| Элемент | Сигнатура / Описание |
|---|---|
| [`error`](../37-quakec-builtins-reference/12-system-debug-builtins.md#error) | `void(string err, ...) error = #10;` |
| [`objerror`](../37-quakec-builtins-reference/12-system-debug-builtins.md#objerror) | `void(string err, ...) objerror = #11;` |
| [`print`](../37-quakec-builtins-reference/12-system-debug-builtins.md#print) | `void(string s, ...) print = #339;` |
| [`bprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#bprint) | `void(float msglvl, string s, optional string s2, optional string s3, optional string s4, optional string s5, optional string s6, optional string s7) bprint = #23;` |
| [`msprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#msprint) | `void(float clientnum, string text, ...) msprint = #6;` |
| [`cprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cprint) | `void(string s, ...) cprint = #338;` |
| [`sprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#sprint) | `void(entity client, float msglvl, string s, optional string s2, optional string s3, optional string s4, optional string s5, optional string s6) sprint = #24;` |
| [`centerprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#centerprint) | `void(entity ent, string text, optional string text2, optional string text3, optional string text4, optional string text5, optional string text6, optional string text7) centerprint = #73;` |
| [`dprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#dprint) | `void(string s, ...) dprint = #25;` |
| [`coredump`](../37-quakec-builtins-reference/12-system-debug-builtins.md#coredump) | `void() coredump = #28;` |
| [`crash`](../37-quakec-builtins-reference/12-system-debug-builtins.md#crash) | `void() crash = #72;` |
| [`stackdump`](../37-quakec-builtins-reference/12-system-debug-builtins.md#stackdump) | `void() stackdump = #73;` |
| [`breakpoint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#breakpoint) | `void() breakpoint = #6;` |
| [`cvar`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar) | `float(string name) cvar = #45;` |
| [`cvar_set`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar_set) | `void(string cvarname, string valuetoset) cvar_set = #72;` |
| [`cvar_setf`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar_setf) | `void(string cvar, float val) cvar_setf = #176;` |
| [`cvar_string`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar_string) | `string(string cvarname) cvar_string = #448;` |
| [`cvar_defstring`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar_defstring) | `string(string name) cvar_defstring = #482;` |
| [`cvar_description`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar_description) | `string(string cvarname) cvar_description = #518;` |
| [`cvar_type`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar_type) | `float(string name) cvar_type = #495;` |
| [`registercvar`](../37-quakec-builtins-reference/12-system-debug-builtins.md#registercvar) | `float(string name, string value, optional float flags) registercvar = #93;` |
| [`checkextension`](../37-quakec-builtins-reference/12-system-debug-builtins.md#checkextension) | `float(string extname) checkextension = #99;` |
| [`logfrag`](../37-quakec-builtins-reference/12-system-debug-builtins.md#logfrag) | `void(entity killer, entity killee) logfrag = #79;` |
| [`setpause`](../37-quakec-builtins-reference/12-system-debug-builtins.md#setpause) | `void(float pause) setpause = #531;` |
| [`localcmd`](../37-quakec-builtins-reference/12-system-debug-builtins.md#localcmd) | `void(string s, ...) localcmd = #46;` |
| [`abort`](../37-quakec-builtins-reference/12-system-debug-builtins.md#abort) | `void(optional __variant ret) abort = #211;` |
| [`argc`](../37-quakec-builtins-reference/12-system-debug-builtins.md#argc) | `float() argc = #0:argc;` |
| [`checkbuiltin`](../37-quakec-builtins-reference/12-system-debug-builtins.md#checkbuiltin) | `float(__variant funcref) checkbuiltin = #0:checkbuiltin;` |
| [`externrefcall`](../37-quakec-builtins-reference/12-system-debug-builtins.md#externrefcall) | `__deprecated("Redundant") __variant(float prnum, void() func, ...) externrefcall = #205;` |

### Функции MenuQC (меню, экран загрузки)

| Элемент | Сигнатура / Описание |
|---|---|
| [`addentity`](../37-quakec-builtins-reference/13-menuqc-builtins.md#addentity) | `void(entity ent) addentity = #302;` |
| [`addentities`](../37-quakec-builtins-reference/13-menuqc-builtins.md#addentities) | `void(float mask) addentities = #301;` |
| [`clearscene`](../37-quakec-builtins-reference/13-menuqc-builtins.md#clearscene) | `void() clearscene = #300;` |
| [`renderscene`](../37-quakec-builtins-reference/13-menuqc-builtins.md#renderscene) | `void() renderscene = #304;` |
| [`getproperty`](../37-quakec-builtins-reference/13-menuqc-builtins.md#getproperty) | `__variant(float property) getproperty = #309;` (алиас `getviewprop`) |
| [`setproperty`](../37-quakec-builtins-reference/13-menuqc-builtins.md#setproperty) | `float(float property, ...) setproperty = #303;` (алиас `setviewprop`) |
| [`getresolution`](../37-quakec-builtins-reference/13-menuqc-builtins.md#getresolution) | `vector(float vidmode, optional float forfullscreen) getresolution = #608;` |
| [`R_BeginPolygon`](../37-quakec-builtins-reference/13-menuqc-builtins.md#r_beginpolygon) | `void(string texturename, optional float flags, optional float is2d) R_BeginPolygon = #306;` |
| [`R_EndPolygon`](../37-quakec-builtins-reference/13-menuqc-builtins.md#r_endpolygon) | `void() R_EndPolygon = #308;` |
| [`R_PolygonVertex`](../37-quakec-builtins-reference/13-menuqc-builtins.md#r_polygonvertex) | `void(vector org, vector texcoords, vector rgb, float alpha) R_PolygonVertex = #307;` |
| [`drawcharacter`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawcharacter) | `float(vector position, float character, vector scale, vector rgb, float alpha, optional float flag) drawcharacter = #454;` |
| [`drawfill`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawfill) | `float(vector position, vector size, vector rgb, float alpha, optional float flag) drawfill = #457;` |
| [`drawline`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawline) | `void(float width, vector pos1, vector pos2, vector rgb, float alpha, optional float flag) drawline = #466;` |
| [`drawpic`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawpic) | `float(vector position, string pic, vector size, vector rgb, float alpha, optional float flag) drawpic = #456;` |
| [`drawrawstring`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawrawstring) | `float(vector position, string text, vector scale, vector rgb, float alpha, optional float flag) drawrawstring = #455;` |
| [`drawresetcliparea`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawresetcliparea) | `void(void) drawresetcliparea = #459;` |
| [`drawsetcliparea`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawsetcliparea) | `void(float x, float y, float width, float height) drawsetcliparea = #458;` |
| [`drawstring`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawstring) | `float(vector position, string text, vector scale, vector rgb, float alpha, float flag) drawstring = #467;` |
| [`drawsubpic`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawsubpic) | `void(vector pos, vector sz, string pic, vector srcpos, vector srcsz, vector rgb, float alpha, float flag) drawsubpic = #469;` |
| [`iscachedpic`](../37-quakec-builtins-reference/13-menuqc-builtins.md#iscachedpic) | `float(string name) iscachedpic = #451;` |
| [`drawgetimagesize`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawgetimagesize) | `vector(string picname) drawgetimagesize = #460;` |
| [`stringwidth`](../37-quakec-builtins-reference/13-menuqc-builtins.md#stringwidth) | `float(string text, float usecolours, optional vector fontsize) stringwidth = #468;` |
| [`con_draw`](../37-quakec-builtins-reference/13-menuqc-builtins.md#con_draw) | `void(string conname, vector pos, vector size, float fontsize) con_draw = #393;` |
| [`con_getset`](../37-quakec-builtins-reference/13-menuqc-builtins.md#con_getset) | `string(string conname, string field, optional string newvalue) con_getset = #391;` |
| [`con_input`](../37-quakec-builtins-reference/13-menuqc-builtins.md#con_input) | `float(string conname, float inevtype, float parama, float paramb, float paramc) con_input = #394;` |
| [`con_printf`](../37-quakec-builtins-reference/13-menuqc-builtins.md#con_printf) | `void(string conname, string messagefmt, ...) con_printf = #392;` |
| [`dynamiclight_add`](../37-quakec-builtins-reference/13-menuqc-builtins.md#dynamiclight_add) | `float(vector org, float radius, vector lightcolours, optional float style, optional string cubemapname, optional float pflags) dynamiclight_add = #305;` |
| [`dynamiclight_get`](../37-quakec-builtins-reference/13-menuqc-builtins.md#dynamiclight_get) | `__variant(float lno, float fld) dynamiclight_get = #372;` |
| [`dynamiclight_set`](../37-quakec-builtins-reference/13-menuqc-builtins.md#dynamiclight_set) | `void(float lno, float fld, __variant value) dynamiclight_set = #373;` |
| [`getkeybind`](../37-quakec-builtins-reference/13-menuqc-builtins.md#getkeybind) | `string(float keynum) getkeybind = #342;` |
| [`setkeybind`](../37-quakec-builtins-reference/13-menuqc-builtins.md#setkeybind) | `float(float key, string bind, optional float bindmap, optional float modifier) setkeybind = #630;` |
| [`getkeydest`](../37-quakec-builtins-reference/13-menuqc-builtins.md#getkeydest) | `float() getkeydest = #602;` |
| [`setkeydest`](../37-quakec-builtins-reference/13-menuqc-builtins.md#setkeydest) | `void(float dest) setkeydest = #601;` |
| [`getbindmaps`](../37-quakec-builtins-reference/13-menuqc-builtins.md#getbindmaps) | `vector() getbindmaps = #631;` |
| [`setbindmaps`](../37-quakec-builtins-reference/13-menuqc-builtins.md#setbindmaps) | `float(vector bm) setbindmaps = #632;` |
| [`getmousepos`](../37-quakec-builtins-reference/13-menuqc-builtins.md#getmousepos) | `vector() getmousepos = #66;` |
| [`setmousetarget`](../37-quakec-builtins-reference/13-menuqc-builtins.md#setmousetarget) | `void(float trg) setmousetarget = #603;` |
| [`getmousetarget`](../37-quakec-builtins-reference/13-menuqc-builtins.md#getmousetarget) | `float() getmousetarget = #604;` |
| [`setcursormode`](../37-quakec-builtins-reference/13-menuqc-builtins.md#setcursormode) | `void(float usecursor, optional string cursorimage, optional vector hotspot, optional float scale) setcursormode = #343;` |
| [`keynumtostring`](../37-quakec-builtins-reference/13-menuqc-builtins.md#keynumtostring) | `string(float keynum) keynumtostring = #609;` |
| [`keynumtostring_csqc`](../37-quakec-builtins-reference/13-menuqc-builtins.md#keynumtostring_csqc) | `string(float keynum) keynumtostring_csqc = #340;` |
| [`stringtokeynum`](../37-quakec-builtins-reference/13-menuqc-builtins.md#stringtokeynum) | `float(string key) stringtokeynum = #614;` |
| [`stringtokeynum_csqc`](../37-quakec-builtins-reference/13-menuqc-builtins.md#stringtokeynum_csqc) | `float(string keyname) stringtokeynum_csqc = #341;` |
| [`findkeysforcommand`](../37-quakec-builtins-reference/13-menuqc-builtins.md#findkeysforcommand) | `string(string command, optional float bindmap) findkeysforcommand = #610;` |
| [`gecko_create`](../37-quakec-builtins-reference/13-menuqc-builtins.md#gecko_create) | `float(string name, optional string initialURI) gecko_create = #487;` |
| [`gecko_destroy`](../37-quakec-builtins-reference/13-menuqc-builtins.md#gecko_destroy) | `void(string name) gecko_destroy = #488;` |
| [`gecko_navigate`](../37-quakec-builtins-reference/13-menuqc-builtins.md#gecko_navigate) | `void(string name, string URI) gecko_navigate = #489;` |
| [`gecko_keyevent`](../37-quakec-builtins-reference/13-menuqc-builtins.md#gecko_keyevent) | `float(string name, float key, float eventtype, optional float charcode) gecko_keyevent = #490;` |
| [`gecko_mousemove`](../37-quakec-builtins-reference/13-menuqc-builtins.md#gecko_mousemove) | `void(string name, float x, float y) gecko_mousemove = #491;` |
| [`gecko_resize`](../37-quakec-builtins-reference/13-menuqc-builtins.md#gecko_resize) | `void(string name, float w, float h) gecko_resize = #492;` |
| [`gecko_get_texture_extent`](../37-quakec-builtins-reference/13-menuqc-builtins.md#gecko_get_texture_extent) | `vector(string name) gecko_get_texture_extent = #493;` |

### Редактор карт, криптография и разные редкие builtins

| Элемент | Сигнатура / Описание |
|---|---|
| [`brush_calcfacepoints`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#brush_calcfacepoints) | `int(int faceid, brushface_t *in_faces, int numfaces, vector *points, int maxpoints) brush_calcfacepoints = #0:brush_calcfacepoints;` |
| [`brush_create`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#brush_create) | `int(float modelidx, brushface_t *in_faces, int numfaces, int contents, optional int brushid) brush_create = #0:brush_create;` |
| [`brush_delete`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#brush_delete) | `void(float modelidx, int brushid) brush_delete = #0:brush_delete;` |
| [`brush_findinvolume`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#brush_findinvolume) | `int(float modelid, vector *planes, float *dists, int numplanes, int *out_brushes, int *out_faces, int maxresults) brush_findinvolume = #0:brush_findinvolume;` |
| [`brush_get`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#brush_get) | `int(float modelidx, int brushid, brushface_t *out_faces, int maxfaces, int *out_contents) brush_get = #0:brush_get;` |
| [`brush_getfacepoints`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#brush_getfacepoints) | `int(float modelid, int brushid, int faceid, vector *points, int maxpoints) brush_getfacepoints = #0:brush_getfacepoints;` |
| [`brush_selected`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#brush_selected) | `float(float modelid, int brushid, int faceid, float selectedstate) brush_selected = #0:brush_selected;` |
| [`bulleten`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#bulleten) | `bulleten` — удалённый legacy-builtin; исторически упоминался у слота `#243`, но в текущих таблицах FTEQW не экспортируется. |
| [`cin_close`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#cin_close) | `void(string id) cin_close = #462;` |
| [`cin_getstate`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#cin_getstate) | `float(string id) cin_getstate = #464;` |
| [`cin_open`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#cin_open) | `float(string file, string id) cin_open = #461;` |
| [`cin_restart`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#cin_restart) | `void(string id) cin_restart = #465;` |
| [`cin_setstate`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#cin_setstate) | `void(string id, float newstate) cin_setstate = #463;` |
| [`controller_query`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#controller_query) | `void(float device) controller_query = #740;` |
| [`controller_rumble`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#controller_rumble) | `void(float device, float lowmult, float highmult, float msec) controller_rumble = #741;` |
| [`controller_rumbletriggers`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#controller_rumbletriggers) | `void(float device, float leftmult, float rightmult, float msec) controller_rumbletriggers = #742;` |
| [`crypto_getencryptlevel`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#crypto_getencryptlevel) | `string(string serveraddress) crypto_getencryptlevel = #635;` |
| [`crypto_getidfp`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#crypto_getidfp) | `string(string serveraddress) crypto_getidfp = #634;` |
| [`crypto_getidstatus`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#crypto_getidstatus) | `float(string serveraddress) crypto_getidstatus = #643;` |
| [`crypto_getkeyfp`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#crypto_getkeyfp) | `string(string serveraddress) crypto_getkeyfp = #633;` |
| [`crypto_getmyidfp`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#crypto_getmyidfp) | `string(float slot) crypto_getmyidfp = #637;` |
| [`crypto_getmyidstatus`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#crypto_getmyidstatus) | `float(float slot) crypto_getmyidstatus = #641;` |
| [`crypto_getmykeyfp`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#crypto_getmykeyfp) | `string(float slot) crypto_getmykeyfp = #636;` |
| [`free_pic`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#free_pic) | `void(string picname) free_pic = #453;` |
| [`gettime`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#gettime) | `float(optional float timetype) gettime = #519;` |
| [`gettimed`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#gettimed) | `__double(optional int timetype) gettimed = #0:gettimed;` |
| [`gettimef`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#gettimef) | `float(optional float timetype) gettimef = #519;` |
| [`gp_getlayout`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#gp_getlayout) | `float(float devid) gp_getlayout = #0:gp_getlayout;` |
| [`gp_rumble`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#gp_rumble) | `void(float devid, float amp_low, float amp_high, float duration) gp_rumble = #0:gp_rumble;` |
| [`gp_rumbletriggers`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#gp_rumbletriggers) | `void(float devid, float left, float right, float duration) gp_rumbletriggers = #0:gp_rumbletriggers;` |
| [`gp_setledcolor`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#gp_setledcolor) | `void(float devid, vector color) gp_setledcolor = #0:gp_setledcolor;` |
| [`gp_settriggerfx`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#gp_settriggerfx) | `void(float devid, void *data, int size) gp_settriggerfx = #0:gp_settriggerfx;` |
| [`js_run_script`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#js_run_script) | `string(string javascript) js_run_script = #0:js_run_script;` |
| [`map_builtin`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#map_builtin) | `float(string builtinname, float opcodenum) map_builtin = #220;` |
| [`patch_create`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#patch_create) | `int(float modelidx, int oldpatchid, patchvert_t *in_controlverts, patchinfo_t in_info) patch_create = #0:patch_create;` |
| [`patch_evaluate`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#patch_evaluate) | `int(patchvert_t *in_controlverts, patchvert_t *out_renderverts, int maxout, patchinfo_t *inout_info) patch_evaluate = #0:patch_evaluate;` |
| [`patch_getcp`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#patch_getcp) | `int(float modelidx, int patchid, patchvert_t *out_controlverts, int maxcp, patchinfo_t *out_info) patch_getcp = #0:patch_getcp;` |
| [`patch_getmesh`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#patch_getmesh) | `int(float modelidx, int patchid, patchvert_t *out_verts, int maxverts, patchinfo_t *out_info) patch_getmesh = #0:patch_getmesh;` |
| [`print_csqc`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#print_csqc) | `void(string text, ...) print_csqc = #339;` |
| [`removeinstant`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#removeinstant) | `void(entity ent) removeinstant = #0:removeinstant;` |
| [`setwatchpoint`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#setwatchpoint) | `void(string name, float evaltype, void *ptr) setwatchpoint = #0:setwatchpoint;` |
| [`stachievement_query`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#stachievement_query) | `stachievement_query(...)` — данная встроенная функция не реализована и является жесткой заглушкой (номера: CSQC=#731, MenuQC=#731). |
| [`stachievement_register`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#stachievement_register) | `stachievement_register(...)` — данная встроенная функция не реализована и является жесткой заглушкой (номера: CSQC=#735, MenuQC=#735). |
| [`stachievement_unlock`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#stachievement_unlock) | `stachievement_unlock(...)` — данная встроенная функция не реализована и является жесткой заглушкой (номера: CSQC=#730, MenuQC=#730). |
| [`ststat_increment`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#ststat_increment) | `ststat_increment(...)` — данная встроенная функция не реализована и является жесткой заглушкой (номера: CSQC=#733, MenuQC=#733). |
| [`ststat_query`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#ststat_query) | `ststat_query(...)` — данная встроенная функция не реализована и является жесткой заглушкой (номера: CSQC=#734, MenuQC=#734). |
| [`ststat_register`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#ststat_register) | `ststat_register(...)` — данная встроенная функция не реализована и является жесткой заглушкой (номера: CSQC=#736, MenuQC=#736). |
| [`ststat_setvalue`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#ststat_setvalue) | `ststat_setvalue(...)` — данная встроенная функция не реализована и является жесткой заглушкой (номера: CSQC=#732, MenuQC=#732). |
| [`videoplaying`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#videoplaying) | `float() videoplaying = #355;` |

## Переменные движка (cvar)

Всего задокументировано: **1279** cvar. Полный постатейный разбор — в разделе [«38. Переменные движка»](../38-cvars-reference/README.md).


### Видео, экран и общий рендеринг

| Элемент | Сигнатура / Описание |
|---|---|
| [`crosshair`](../38-cvars-reference/01-video-rendering-cvars.md#crosshair) | `cvar crosshair(boolean/int, "1")` |
| [`crosshaircorrect`](../38-cvars-reference/01-video-rendering-cvars.md#crosshaircorrect) | `cvar crosshaircorrect(boolean/int, "0")` |
| [`crosshairimage`](../38-cvars-reference/01-video-rendering-cvars.md#crosshairimage) | `cvar crosshairimage(string, "")` |
| [`crosshairsize`](../38-cvars-reference/01-video-rendering-cvars.md#crosshairsize) | `cvar crosshairsize(int, "8")` |
| [`d_lodbias`](../38-cvars-reference/01-video-rendering-cvars.md#d_lodbias) | `cvar d_lodbias(float, "0")` |
| [`ffov`](../38-cvars-reference/01-video-rendering-cvars.md#ffov) | `cvar ffov(float/string, "")` |
| [`fov`](../38-cvars-reference/01-video-rendering-cvars.md#fov) | `cvar fov(float, "90")` |
| [`gl_affinemodels`](../38-cvars-reference/01-video-rendering-cvars.md#gl_affinemodels) | `cvar gl_affinemodels(boolean/int, "0")` |
| [`gl_compress`](../38-cvars-reference/01-video-rendering-cvars.md#gl_compress) | `cvar gl_compress(boolean/int, "0")` |
| [`gl_dither`](../38-cvars-reference/01-video-rendering-cvars.md#gl_dither) | `cvar gl_dither(boolean/int, "1")` |
| [`gl_finish`](../38-cvars-reference/01-video-rendering-cvars.md#gl_finish) | `cvar gl_finish(boolean/int, "0")` |
| [`gl_lerpimages`](../38-cvars-reference/01-video-rendering-cvars.md#gl_lerpimages) | `cvar gl_lerpimages(boolean/int, "1")` |
| [`gl_max_size`](../38-cvars-reference/01-video-rendering-cvars.md#gl_max_size) | `cvar gl_max_size(int, "8192")` |
| [`gl_motionblur`](../38-cvars-reference/01-video-rendering-cvars.md#gl_motionblur) | `cvar gl_motionblur(float, "0")` |
| [`gl_motionblurscale`](../38-cvars-reference/01-video-rendering-cvars.md#gl_motionblurscale) | `cvar gl_motionblurscale(float, "1")` |
| [`gl_overbright`](../38-cvars-reference/01-video-rendering-cvars.md#gl_overbright) | `cvar gl_overbright(int, "1")` |
| [`gl_picmip`](../38-cvars-reference/01-video-rendering-cvars.md#gl_picmip) | `cvar gl_picmip(int, "0")` |
| [`gl_picmip2d`](../38-cvars-reference/01-video-rendering-cvars.md#gl_picmip2d) | `cvar gl_picmip2d(int, "0")` |
| [`gl_picmip_other`](../38-cvars-reference/01-video-rendering-cvars.md#gl_picmip_other) | `cvar gl_picmip_other(int, "0")` |
| [`gl_picmip_sprites`](../38-cvars-reference/01-video-rendering-cvars.md#gl_picmip_sprites) | `cvar gl_picmip_sprites(int, "0")` |
| [`gl_picmip_world`](../38-cvars-reference/01-video-rendering-cvars.md#gl_picmip_world) | `cvar gl_picmip_world(int, "0")` |
| [`gl_polyblend`](../38-cvars-reference/01-video-rendering-cvars.md#gl_polyblend) | `cvar gl_polyblend(boolean/int, "1")` |
| [`gl_smoothcrosshair`](../38-cvars-reference/01-video-rendering-cvars.md#gl_smoothcrosshair) | `cvar gl_smoothcrosshair(boolean/int, "1")` |
| [`gl_texture_anisotropy`](../38-cvars-reference/01-video-rendering-cvars.md#gl_texture_anisotropy) | `cvar gl_texture_anisotropy(int, "4")` |
| [`gl_texturemode`](../38-cvars-reference/01-video-rendering-cvars.md#gl_texturemode) | `cvar gl_texturemode(string, "GL_LINEAR_MIPMAP_LINEAR")` |
| [`gl_texturemode2d`](../38-cvars-reference/01-video-rendering-cvars.md#gl_texturemode2d) | `cvar gl_texturemode2d(string, "GL_LINEAR")` |
| [`mod_external_vis`](../38-cvars-reference/01-video-rendering-cvars.md#mod_external_vis) | `cvar mod_external_vis(boolean/int, "1")` |
| [`r_drawflat`](../38-cvars-reference/01-video-rendering-cvars.md#r_drawflat) | `cvar r_drawflat(boolean/int, "0")` |
| [`r_drawviewmodel`](../38-cvars-reference/01-video-rendering-cvars.md#r_drawviewmodel) | `cvar r_drawviewmodel(boolean/int, "1")` |
| [`r_fxaa`](../38-cvars-reference/01-video-rendering-cvars.md#r_fxaa) | `cvar r_fxaa(boolean/int, "0")` |
| [`r_lodbias`](../38-cvars-reference/01-video-rendering-cvars.md#r_lodbias) | `cvar r_lodbias(int, "0")` |
| [`r_lodscale`](../38-cvars-reference/01-video-rendering-cvars.md#r_lodscale) | `cvar r_lodscale(float, "5")` |
| [`r_noframegrouplerp`](../38-cvars-reference/01-video-rendering-cvars.md#r_noframegrouplerp) | `cvar r_noframegrouplerp(boolean/int, "0")` |
| [`r_nolerp`](../38-cvars-reference/01-video-rendering-cvars.md#r_nolerp) | `cvar r_nolerp(boolean/int, "0")` |
| [`r_projection`](../38-cvars-reference/01-video-rendering-cvars.md#r_projection) | `cvar r_projection(int, "0")` |
| [`r_renderscale`](../38-cvars-reference/01-video-rendering-cvars.md#r_renderscale) | `cvar r_renderscale(float, "1")` |
| [`r_showtris`](../38-cvars-reference/01-video-rendering-cvars.md#r_showtris) | `cvar r_showtris(boolean/int, "0")` |
| [`r_viewmodel_fov`](../38-cvars-reference/01-video-rendering-cvars.md#r_viewmodel_fov) | `cvar r_viewmodel_fov(float/string, "")` |
| [`r_viewmodel_quake`](../38-cvars-reference/01-video-rendering-cvars.md#r_viewmodel_quake) | `cvar r_viewmodel_quake(boolean/int, "0")` |
| [`r_wateralpha`](../38-cvars-reference/01-video-rendering-cvars.md#r_wateralpha) | `cvar r_wateralpha(float, "1")` |
| [`r_waterstyle`](../38-cvars-reference/01-video-rendering-cvars.md#r_waterstyle) | `cvar r_waterstyle(int, "1")` |
| [`r_waterwarp`](../38-cvars-reference/01-video-rendering-cvars.md#r_waterwarp) | `cvar r_waterwarp(float, "1")` |
| [`scr_fov_mode`](../38-cvars-reference/01-video-rendering-cvars.md#scr_fov_mode) | `cvar scr_fov_mode(int, "4")` |
| [`vid_bpp`](../38-cvars-reference/01-video-rendering-cvars.md#vid_bpp) | `cvar vid_bpp(int, "0")` |
| [`vid_conautoscale`](../38-cvars-reference/01-video-rendering-cvars.md#vid_conautoscale) | `cvar vid_conautoscale(float, "0")` |
| [`vid_conheight`](../38-cvars-reference/01-video-rendering-cvars.md#vid_conheight) | `cvar vid_conheight(int, "0")` |
| [`vid_conwidth`](../38-cvars-reference/01-video-rendering-cvars.md#vid_conwidth) | `cvar vid_conwidth(int, "0")` |
| [`vid_depthbits`](../38-cvars-reference/01-video-rendering-cvars.md#vid_depthbits) | `cvar vid_depthbits(int, "0")` |
| [`vid_desktopsettings`](../38-cvars-reference/01-video-rendering-cvars.md#vid_desktopsettings) | `cvar vid_desktopsettings(boolean/int, "0")` |
| [`vid_displayfrequency`](../38-cvars-reference/01-video-rendering-cvars.md#vid_displayfrequency) | `cvar vid_displayfrequency(int, "0")` |
| [`vid_fullscreen`](../38-cvars-reference/01-video-rendering-cvars.md#vid_fullscreen) | `cvar vid_fullscreen(int, "2")` |
| [`vid_hardwaregamma`](../38-cvars-reference/01-video-rendering-cvars.md#vid_hardwaregamma) | `cvar vid_hardwaregamma(int, "1")` |
| [`vid_height`](../38-cvars-reference/01-video-rendering-cvars.md#vid_height) | `cvar vid_height(int, "0")` |
| [`vid_multisample`](../38-cvars-reference/01-video-rendering-cvars.md#vid_multisample) | `cvar vid_multisample(int, "0")` |
| [`vid_renderer`](../38-cvars-reference/01-video-rendering-cvars.md#vid_renderer) | `cvar vid_renderer(string, "")` |
| [`vid_srgb`](../38-cvars-reference/01-video-rendering-cvars.md#vid_srgb) | `cvar vid_srgb(int, "0")` |
| [`vid_vsync`](../38-cvars-reference/01-video-rendering-cvars.md#vid_vsync) | `cvar vid_vsync(int, "0")` |
| [`vid_width`](../38-cvars-reference/01-video-rendering-cvars.md#vid_width) | `cvar vid_width(int, "0")` |
| [`_vid_renderer_opts`](../38-cvars-reference/01-video-rendering-cvars.md#_vid_renderer_opts) | `cvar _vid_renderer_opts(string, "")` |
| [`brightness`](../38-cvars-reference/01-video-rendering-cvars.md#brightness) | `cvar brightness(float, "0.0")` |
| [`gl_ati_truform_type`](../38-cvars-reference/01-video-rendering-cvars.md#gl_ati_truform_type) | `cvar gl_ati_truform_type(int, "1")` |
| [`gl_blacklist_texture_compression`](../38-cvars-reference/01-video-rendering-cvars.md#gl_blacklist_texture_compression) | `cvar gl_blacklist_texture_compression(int, "0")` |
| [`gl_blend2d`](../38-cvars-reference/01-video-rendering-cvars.md#gl_blend2d) | `cvar gl_blend2d(int, "1")` |
| [`gl_blendsprites`](../38-cvars-reference/01-video-rendering-cvars.md#gl_blendsprites) | `cvar gl_blendsprites(int, "0")` |
| [`gl_conback`](../38-cvars-reference/01-video-rendering-cvars.md#gl_conback) | `cvar gl_conback(string, "")` |
| [`gl_cshiftpercent`](../38-cvars-reference/01-video-rendering-cvars.md#gl_cshiftpercent) | `cvar gl_cshiftpercent(int, "100")` |
| [`gl_detail`](../38-cvars-reference/01-video-rendering-cvars.md#gl_detail) | `cvar gl_detail(int, "0")` |
| [`gl_detailscale`](../38-cvars-reference/01-video-rendering-cvars.md#gl_detailscale) | `cvar gl_detailscale(int, "5")` |
| [`gl_driver`](../38-cvars-reference/01-video-rendering-cvars.md#gl_driver) | `cvar gl_driver(string, "")` |
| [`gl_font`](../38-cvars-reference/01-video-rendering-cvars.md#gl_font) | `cvar gl_font(string, "")` |
| [`gl_immutable_buffers`](../38-cvars-reference/01-video-rendering-cvars.md#gl_immutable_buffers) | `cvar gl_immutable_buffers(int, "1")` |
| [`gl_immutable_textures`](../38-cvars-reference/01-video-rendering-cvars.md#gl_immutable_textures) | `cvar gl_immutable_textures(int, "1")` |
| [`gl_lateswap`](../38-cvars-reference/01-video-rendering-cvars.md#gl_lateswap) | `cvar gl_lateswap(int, "0")` |
| [`gl_lightmap_average`](../38-cvars-reference/01-video-rendering-cvars.md#gl_lightmap_average) | `cvar gl_lightmap_average(int, "0")` |
| [`gl_lightmap_nearest`](../38-cvars-reference/01-video-rendering-cvars.md#gl_lightmap_nearest) | `cvar gl_lightmap_nearest(int, "0")` |
| [`gl_load24bit`](../38-cvars-reference/01-video-rendering-cvars.md#gl_load24bit) | `cvar gl_load24bit(int, "1")` |
| [`gl_maxdist`](../38-cvars-reference/01-video-rendering-cvars.md#gl_maxdist) | `cvar gl_maxdist(int, "0")` |
| [`gl_mindist`](../38-cvars-reference/01-video-rendering-cvars.md#gl_mindist) | `cvar gl_mindist(int, "1")` |
| [`gl_nocolors`](../38-cvars-reference/01-video-rendering-cvars.md#gl_nocolors) | `cvar gl_nocolors(int, "0")` |
| [`gl_nohwblend`](../38-cvars-reference/01-video-rendering-cvars.md#gl_nohwblend) | `cvar gl_nohwblend(int, "1")` |
| [`gl_outline`](../38-cvars-reference/01-video-rendering-cvars.md#gl_outline) | `cvar gl_outline(int, "0")` |
| [`gl_outline_width`](../38-cvars-reference/01-video-rendering-cvars.md#gl_outline_width) | `cvar gl_outline_width(int, "2")` |
| [`gl_overbright_all`](../38-cvars-reference/01-video-rendering-cvars.md#gl_overbright_all) | `cvar gl_overbright_all(int, "0")` |
| [`gl_overbright_models`](../38-cvars-reference/01-video-rendering-cvars.md#gl_overbright_models) | `cvar gl_overbright_models(int, "0")` |
| [`gl_part_flame`](../38-cvars-reference/01-video-rendering-cvars.md#gl_part_flame) | `cvar gl_part_flame(int, "1")` |
| [`gl_pbolightmaps`](../38-cvars-reference/01-video-rendering-cvars.md#gl_pbolightmaps) | `cvar gl_pbolightmaps(int, "1")` |
| [`gl_polyblend_edgesize`](../38-cvars-reference/01-video-rendering-cvars.md#gl_polyblend_edgesize) | `cvar gl_polyblend_edgesize(int, "128")` |
| [`gl_schematics`](../38-cvars-reference/01-video-rendering-cvars.md#gl_schematics) | `cvar gl_schematics(int, "0")` |
| [`gl_screenangle`](../38-cvars-reference/01-video-rendering-cvars.md#gl_screenangle) | `cvar gl_screenangle(int, "0")` |
| [`gl_shadeq1_name`](../38-cvars-reference/01-video-rendering-cvars.md#gl_shadeq1_name) | `cvar gl_shadeq1_name(string, "*")` |
| [`gl_shaftlight`](../38-cvars-reference/01-video-rendering-cvars.md#gl_shaftlight) | `cvar gl_shaftlight(float, "0.8")` |
| [`gl_simpleitems`](../38-cvars-reference/01-video-rendering-cvars.md#gl_simpleitems) | `cvar gl_simpleitems(int, "0")` |
| [`gl_specular`](../38-cvars-reference/01-video-rendering-cvars.md#gl_specular) | `cvar gl_specular(float, "0.3")` |
| [`gl_specular_fallback`](../38-cvars-reference/01-video-rendering-cvars.md#gl_specular_fallback) | `cvar gl_specular_fallback(float, "0.05")` |
| [`gl_specular_fallbackexp`](../38-cvars-reference/01-video-rendering-cvars.md#gl_specular_fallbackexp) | `cvar gl_specular_fallbackexp(int, "1")` |
| [`gl_specular_power`](../38-cvars-reference/01-video-rendering-cvars.md#gl_specular_power) | `cvar gl_specular_power(int, "32")` |
| [`msg_filter_frags`](../38-cvars-reference/01-video-rendering-cvars.md#msg_filter_frags) | `cvar msg_filter_frags(int, "0")` |
| [`msg_filter_pickups`](../38-cvars-reference/01-video-rendering-cvars.md#msg_filter_pickups) | `cvar msg_filter_pickups(int, "0")` |
| [`pr_allowbutton1`](../38-cvars-reference/01-video-rendering-cvars.md#pr_allowbutton1) | `cvar pr_allowbutton1(int, "1")` |
| [`pr_autocreatecvars`](../38-cvars-reference/01-video-rendering-cvars.md#pr_autocreatecvars) | `cvar pr_autocreatecvars(int, "1")` |
| [`pr_brokenfloatconvert`](../38-cvars-reference/01-video-rendering-cvars.md#pr_brokenfloatconvert) | `cvar pr_brokenfloatconvert(int, "0")` |
| [`pr_compatabilitytest`](../38-cvars-reference/01-video-rendering-cvars.md#pr_compatabilitytest) | `cvar pr_compatabilitytest(int, "0")` |
| [`pr_coreonerror`](../38-cvars-reference/01-video-rendering-cvars.md#pr_coreonerror) | `cvar pr_coreonerror(int, "1")` |
| [`pr_csqc_coreonerror`](../38-cvars-reference/01-video-rendering-cvars.md#pr_csqc_coreonerror) | `cvar pr_csqc_coreonerror(int, "1")` |
| [`pr_csqc_formenus`](../38-cvars-reference/01-video-rendering-cvars.md#pr_csqc_formenus) | `cvar pr_csqc_formenus(int, "1")` |
| [`pr_csqc_maxedicts`](../38-cvars-reference/01-video-rendering-cvars.md#pr_csqc_maxedicts) | `cvar pr_csqc_maxedicts(int, "65536")` |
| [`pr_csqc_memsize`](../38-cvars-reference/01-video-rendering-cvars.md#pr_csqc_memsize) | `cvar pr_csqc_memsize(int, "-1")` |
| [`pr_debugger`](../38-cvars-reference/01-video-rendering-cvars.md#pr_debugger) | `cvar pr_debugger(string, "debugger")` |
| [`pr_droptofloorunits`](../38-cvars-reference/01-video-rendering-cvars.md#pr_droptofloorunits) | `cvar pr_droptofloorunits(int, "256")` |
| [`pr_enable_profiling`](../38-cvars-reference/01-video-rendering-cvars.md#pr_enable_profiling) | `cvar pr_enable_profiling(int, "0")` |
| [`pr_enable_uriget`](../38-cvars-reference/01-video-rendering-cvars.md#pr_enable_uriget) | `cvar pr_enable_uriget(int, "1")` |
| [`pr_engine`](../38-cvars-reference/01-video-rendering-cvars.md#pr_engine) | `cvar pr_engine(string, " -")` |
| [`pr_ext_dp_qc_getsurface`](../38-cvars-reference/01-video-rendering-cvars.md#pr_ext_dp_qc_getsurface) | `cvar pr_ext_dp_qc_getsurface(string, "")` |
| [`pr_fixbrokenqccarrays`](../38-cvars-reference/01-video-rendering-cvars.md#pr_fixbrokenqccarrays) | `cvar pr_fixbrokenqccarrays(int, "0")` |
| [`pr_gc_threaded`](../38-cvars-reference/01-video-rendering-cvars.md#pr_gc_threaded) | `cvar pr_gc_threaded(int, "1")` |
| [`pr_imitatemvdsv`](../38-cvars-reference/01-video-rendering-cvars.md#pr_imitatemvdsv) | `cvar pr_imitatemvdsv(int, "0")` |
| [`pr_maxedicts`](../38-cvars-reference/01-video-rendering-cvars.md#pr_maxedicts) | `cvar pr_maxedicts(int, "131072")` |
| [`pr_no_parsecommand`](../38-cvars-reference/01-video-rendering-cvars.md#pr_no_parsecommand) | `cvar pr_no_parsecommand(int, "0")` |
| [`pr_no_playerphysics`](../38-cvars-reference/01-video-rendering-cvars.md#pr_no_playerphysics) | `cvar pr_no_playerphysics(int, "1")` |
| [`pr_nonetaccess`](../38-cvars-reference/01-video-rendering-cvars.md#pr_nonetaccess) | `cvar pr_nonetaccess(int, "0")` |
| [`pr_overridebuiltins`](../38-cvars-reference/01-video-rendering-cvars.md#pr_overridebuiltins) | `cvar pr_overridebuiltins(int, "1")` |
| [`pr_precachepic_slow`](../38-cvars-reference/01-video-rendering-cvars.md#pr_precachepic_slow) | `cvar pr_precachepic_slow(int, "0")` |
| [`pr_sourcedir`](../38-cvars-reference/01-video-rendering-cvars.md#pr_sourcedir) | `cvar pr_sourcedir(string, "src")` |
| [`pr_ssqc_memsize`](../38-cvars-reference/01-video-rendering-cvars.md#pr_ssqc_memsize) | `cvar pr_ssqc_memsize(int, "-1")` |
| [`pr_tempstringcount`](../38-cvars-reference/01-video-rendering-cvars.md#pr_tempstringcount) | `cvar pr_tempstringcount(string, "")` |
| [`pr_tempstringsize`](../38-cvars-reference/01-video-rendering-cvars.md#pr_tempstringsize) | `cvar pr_tempstringsize(int, "4096")` |
| [`r_bloodstains`](../38-cvars-reference/01-video-rendering-cvars.md#r_bloodstains) | `cvar r_bloodstains(int, "1")` |
| [`r_bluelight_colour`](../38-cvars-reference/01-video-rendering-cvars.md#r_bluelight_colour) | `cvar r_bluelight_colour(string, "0.5 0.5 3.0 200")` |
| [`r_bouncysparks`](../38-cvars-reference/01-video-rendering-cvars.md#r_bouncysparks) | `cvar r_bouncysparks(int, "1")` |
| [`r_brightlight_colour`](../38-cvars-reference/01-video-rendering-cvars.md#r_brightlight_colour) | `cvar r_brightlight_colour(string, "2.0 1.0 0.5 400")` |
| [`r_clear`](../38-cvars-reference/01-video-rendering-cvars.md#r_clear) | `cvar r_clear(int, "0")` |
| [`r_clearcolour`](../38-cvars-reference/01-video-rendering-cvars.md#r_clearcolour) | `cvar r_clearcolour(string, "0.12 0.12 0.12")` |
| [`r_clutter_density`](../38-cvars-reference/01-video-rendering-cvars.md#r_clutter_density) | `cvar r_clutter_density(int, "0")` |
| [`r_clutter_distance`](../38-cvars-reference/01-video-rendering-cvars.md#r_clutter_distance) | `cvar r_clutter_distance(int, "1024")` |
| [`r_coronas`](../38-cvars-reference/01-video-rendering-cvars.md#r_coronas) | `cvar r_coronas(int, "0")` |
| [`r_decal_noperpendicular`](../38-cvars-reference/01-video-rendering-cvars.md#r_decal_noperpendicular) | `cvar r_decal_noperpendicular(int, "1")` |
| [`r_deluxemapping`](../38-cvars-reference/01-video-rendering-cvars.md#r_deluxemapping) | `cvar r_deluxemapping(int, "1")` |
| [`r_dimlight_colour`](../38-cvars-reference/01-video-rendering-cvars.md#r_dimlight_colour) | `cvar r_dimlight_colour(string, "2.0 1.0 0.5 200")` |
| [`r_dodgymiptex`](../38-cvars-reference/01-video-rendering-cvars.md#r_dodgymiptex) | `cvar r_dodgymiptex(int, "1")` |
| [`r_dodgypcxfiles`](../38-cvars-reference/01-video-rendering-cvars.md#r_dodgypcxfiles) | `cvar r_dodgypcxfiles(int, "0")` |
| [`r_dodgytgafiles`](../38-cvars-reference/01-video-rendering-cvars.md#r_dodgytgafiles) | `cvar r_dodgytgafiles(int, "0")` |
| [`r_drawentities`](../38-cvars-reference/01-video-rendering-cvars.md#r_drawentities) | `cvar r_drawentities(int, "1")` |
| [`r_drawflame`](../38-cvars-reference/01-video-rendering-cvars.md#r_drawflame) | `cvar r_drawflame(int, "1")` |
| [`r_drawviewmodelinvis`](../38-cvars-reference/01-video-rendering-cvars.md#r_drawviewmodelinvis) | `cvar r_drawviewmodelinvis(int, "0")` |
| [`r_editlights`](../38-cvars-reference/01-video-rendering-cvars.md#r_editlights) | `cvar r_editlights(int, "0")` |
| [`r_explosionlight`](../38-cvars-reference/01-video-rendering-cvars.md#r_explosionlight) | `cvar r_explosionlight(int, "1")` |
| [`r_explosionlight_colour`](../38-cvars-reference/01-video-rendering-cvars.md#r_explosionlight_colour) | `cvar r_explosionlight_colour(string, "4.0 2.0 0.5")` |
| [`r_explosionlight_fade`](../38-cvars-reference/01-video-rendering-cvars.md#r_explosionlight_fade) | `cvar r_explosionlight_fade(string, "0.784 0.92 0.48")` |
| [`r_fastturb`](../38-cvars-reference/01-video-rendering-cvars.md#r_fastturb) | `cvar r_fastturb(int, "0")` |
| [`r_fastturbcolour`](../38-cvars-reference/01-video-rendering-cvars.md#r_fastturbcolour) | `cvar r_fastturbcolour(string, "0.1 0.2 0.3")` |
| [`r_fb_bmodels`](../38-cvars-reference/01-video-rendering-cvars.md#r_fb_bmodels) | `cvar r_fb_bmodels(int, "1")` |
| [`r_fb_models`](../38-cvars-reference/01-video-rendering-cvars.md#r_fb_models) | `cvar r_fb_models(int, "1")` |
| [`r_floorcolour`](../38-cvars-reference/01-video-rendering-cvars.md#r_floorcolour) | `cvar r_floorcolour(string, "64 64 128")` |
| [`r_fog_cullentities`](../38-cvars-reference/01-video-rendering-cvars.md#r_fog_cullentities) | `cvar r_fog_cullentities(int, "1")` |
| [`r_fog_exp2`](../38-cvars-reference/01-video-rendering-cvars.md#r_fog_exp2) | `cvar r_fog_exp2(int, "1")` |
| [`r_fog_linear`](../38-cvars-reference/01-video-rendering-cvars.md#r_fog_linear) | `cvar r_fog_linear(int, "0")` |
| [`r_fog_permutation`](../38-cvars-reference/01-video-rendering-cvars.md#r_fog_permutation) | `cvar r_fog_permutation(int, "1")` |
| [`r_font_linear`](../38-cvars-reference/01-video-rendering-cvars.md#r_font_linear) | `cvar r_font_linear(int, "1")` |
| [`r_graphics`](../38-cvars-reference/01-video-rendering-cvars.md#r_graphics) | `cvar r_graphics(int, "1")` |
| [`r_greenlight_colour`](../38-cvars-reference/01-video-rendering-cvars.md#r_greenlight_colour) | `cvar r_greenlight_colour(string, "0.5 3.0 0.5 200")` |
| [`r_grenadetrail`](../38-cvars-reference/01-video-rendering-cvars.md#r_grenadetrail) | `cvar r_grenadetrail(int, "1")` |
| [`r_hdr_framebuffer`](../38-cvars-reference/01-video-rendering-cvars.md#r_hdr_framebuffer) | `cvar r_hdr_framebuffer(int, "0")` |
| [`r_hdr_irisadaptation`](../38-cvars-reference/01-video-rendering-cvars.md#r_hdr_irisadaptation) | `cvar r_hdr_irisadaptation(int, "0")` |
| [`r_hdr_irisadaptation_fade_down`](../38-cvars-reference/01-video-rendering-cvars.md#r_hdr_irisadaptation_fade_down) | `cvar r_hdr_irisadaptation_fade_down(float, "0.5")` |
| [`r_hdr_irisadaptation_fade_up`](../38-cvars-reference/01-video-rendering-cvars.md#r_hdr_irisadaptation_fade_up) | `cvar r_hdr_irisadaptation_fade_up(float, "0.1")` |
| [`r_hdr_irisadaptation_maxvalue`](../38-cvars-reference/01-video-rendering-cvars.md#r_hdr_irisadaptation_maxvalue) | `cvar r_hdr_irisadaptation_maxvalue(int, "4")` |
| [`r_hdr_irisadaptation_minvalue`](../38-cvars-reference/01-video-rendering-cvars.md#r_hdr_irisadaptation_minvalue) | `cvar r_hdr_irisadaptation_minvalue(float, "0.5")` |
| [`r_hdr_irisadaptation_multiplier`](../38-cvars-reference/01-video-rendering-cvars.md#r_hdr_irisadaptation_multiplier) | `cvar r_hdr_irisadaptation_multiplier(int, "2")` |
| [`r_ignoreentpvs`](../38-cvars-reference/01-video-rendering-cvars.md#r_ignoreentpvs) | `cvar r_ignoreentpvs(int, "1")` |
| [`r_ignoremapprefixes`](../38-cvars-reference/01-video-rendering-cvars.md#r_ignoremapprefixes) | `cvar r_ignoremapprefixes(int, "0")` |
| [`r_imageextensions`](../38-cvars-reference/01-video-rendering-cvars.md#r_imageextensions) | `cvar r_imageextensions(string, "")` |
| [`r_keepimages`](../38-cvars-reference/01-video-rendering-cvars.md#r_keepimages) | `cvar r_keepimages(int, "0")` |
| [`r_lavaalpha`](../38-cvars-reference/01-video-rendering-cvars.md#r_lavaalpha) | `cvar r_lavaalpha(string, "")` |
| [`r_lavastyle`](../38-cvars-reference/01-video-rendering-cvars.md#r_lavastyle) | `cvar r_lavastyle(int, "1")` |
| [`r_lerpmuzzlehack`](../38-cvars-reference/01-video-rendering-cvars.md#r_lerpmuzzlehack) | `cvar r_lerpmuzzlehack(int, "1")` |
| [`r_loadlit`](../38-cvars-reference/01-video-rendering-cvars.md#r_loadlit) | `cvar r_loadlit(int, "1")` |
| [`r_loadsurfenvmaps`](../38-cvars-reference/01-video-rendering-cvars.md#r_loadsurfenvmaps) | `cvar r_loadsurfenvmaps(int, "1")` |
| [`r_max_gpu_bones`](../38-cvars-reference/01-video-rendering-cvars.md#r_max_gpu_bones) | `cvar r_max_gpu_bones(string, "")` |
| [`r_menutint`](../38-cvars-reference/01-video-rendering-cvars.md#r_menutint) | `cvar r_menutint(string, "0.68 0.4 0.13")` |
| [`r_meshpitch`](../38-cvars-reference/01-video-rendering-cvars.md#r_meshpitch) | `cvar r_meshpitch(int, "1")` |
| [`r_meshroll`](../38-cvars-reference/01-video-rendering-cvars.md#r_meshroll) | `cvar r_meshroll(int, "1")` |
| [`r_mirroralpha`](../38-cvars-reference/01-video-rendering-cvars.md#r_mirroralpha) | `cvar r_mirroralpha(int, "1")` |
| [`r_muzzleflash_colour`](../38-cvars-reference/01-video-rendering-cvars.md#r_muzzleflash_colour) | `cvar r_muzzleflash_colour(string, "1.5 1.3 1.0 200")` |
| [`r_muzzleflash_fade`](../38-cvars-reference/01-video-rendering-cvars.md#r_muzzleflash_fade) | `cvar r_muzzleflash_fade(string, "1.5 0.75 0.375 1000")` |
| [`r_netgraph`](../38-cvars-reference/01-video-rendering-cvars.md#r_netgraph) | `cvar r_netgraph(int, "0")` |
| [`r_noaliasshadows`](../38-cvars-reference/01-video-rendering-cvars.md#r_noaliasshadows) | `cvar r_noaliasshadows(int, "0")` |
| [`r_nolerp_list`](../38-cvars-reference/01-video-rendering-cvars.md#r_nolerp_list) | `cvar r_nolerp_list(string, "")` |
| [`r_nolightdir`](../38-cvars-reference/01-video-rendering-cvars.md#r_nolightdir) | `cvar r_nolightdir(int, "0")` |
| [`r_norefresh`](../38-cvars-reference/01-video-rendering-cvars.md#r_norefresh) | `cvar r_norefresh(int, "0")` |
| [`r_noshadow_list`](../38-cvars-reference/01-video-rendering-cvars.md#r_noshadow_list) | `cvar r_noshadow_list(string, "r_noEntityCastShadowList")` |
| [`r_novis`](../38-cvars-reference/01-video-rendering-cvars.md#r_novis) | `cvar r_novis(int, "0")` |
| [`r_part_beams`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_beams) | `cvar r_part_beams(int, "1")` |
| [`r_part_classic_expgrav`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_classic_expgrav) | `cvar r_part_classic_expgrav(int, "10")` |
| [`r_part_classic_opaque`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_classic_opaque) | `cvar r_part_classic_opaque(int, "0")` |
| [`r_part_classic_square`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_classic_square) | `cvar r_part_classic_square(int, "0")` |
| [`r_part_contentswitch`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_contentswitch) | `cvar r_part_contentswitch(int, "1")` |
| [`r_part_density`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_density) | `cvar r_part_density(int, "1")` |
| [`r_part_maxdecals`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_maxdecals) | `cvar r_part_maxdecals(int, "8192")` |
| [`r_part_maxparticles`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_maxparticles) | `cvar r_part_maxparticles(int, "65536")` |
| [`r_part_rain`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_rain) | `cvar r_part_rain(int, "0")` |
| [`r_part_sparks`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_sparks) | `cvar r_part_sparks(int, "1")` |
| [`r_particle_tracelimit`](../38-cvars-reference/01-video-rendering-cvars.md#r_particle_tracelimit) | `cvar r_particle_tracelimit(string, "0x7fffffff")` |
| [`r_particlesystem`](../38-cvars-reference/01-video-rendering-cvars.md#r_particlesystem) | `cvar r_particlesystem(string, "script")` |
| [`r_polygonoffset_shadowmap_factor`](../38-cvars-reference/01-video-rendering-cvars.md#r_polygonoffset_shadowmap_factor) | `cvar r_polygonoffset_shadowmap_factor(float, "0.05")` |
| [`r_polygonoffset_shadowmap_offset`](../38-cvars-reference/01-video-rendering-cvars.md#r_polygonoffset_shadowmap_offset) | `cvar r_polygonoffset_shadowmap_offset(int, "0")` |
| [`r_polygonoffset_stencil_factor`](../38-cvars-reference/01-video-rendering-cvars.md#r_polygonoffset_stencil_factor) | `cvar r_polygonoffset_stencil_factor(float, "0.01")` |
| [`r_polygonoffset_stencil_offset`](../38-cvars-reference/01-video-rendering-cvars.md#r_polygonoffset_stencil_offset) | `cvar r_polygonoffset_stencil_offset(int, "1")` |
| [`r_polygonoffset_submodel_factor`](../38-cvars-reference/01-video-rendering-cvars.md#r_polygonoffset_submodel_factor) | `cvar r_polygonoffset_submodel_factor(int, "0")` |
| [`r_polygonoffset_submodel_map`](../38-cvars-reference/01-video-rendering-cvars.md#r_polygonoffset_submodel_map) | `cvar r_polygonoffset_submodel_map(string, "e?m? r?m? hip?m?")` |
| [`r_polygonoffset_submodel_offset`](../38-cvars-reference/01-video-rendering-cvars.md#r_polygonoffset_submodel_offset) | `cvar r_polygonoffset_submodel_offset(int, "0")` |
| [`r_portaldrawplanes`](../38-cvars-reference/01-video-rendering-cvars.md#r_portaldrawplanes) | `cvar r_portaldrawplanes(int, "0")` |
| [`r_portalonly`](../38-cvars-reference/01-video-rendering-cvars.md#r_portalonly) | `cvar r_portalonly(int, "0")` |
| [`r_portalrecursion`](../38-cvars-reference/01-video-rendering-cvars.md#r_portalrecursion) | `cvar r_portalrecursion(int, "1")` |
| [`r_powerupglow`](../38-cvars-reference/01-video-rendering-cvars.md#r_powerupglow) | `cvar r_powerupglow(int, "1")` |
| [`r_redlight_colour`](../38-cvars-reference/01-video-rendering-cvars.md#r_redlight_colour) | `cvar r_redlight_colour(string, "3.0 0.5 0.5 200")` |
| [`r_refract_fbo`](../38-cvars-reference/01-video-rendering-cvars.md#r_refract_fbo) | `cvar r_refract_fbo(int, "1")` |
| [`r_refractreflect_scale`](../38-cvars-reference/01-video-rendering-cvars.md#r_refractreflect_scale) | `cvar r_refractreflect_scale(float, "0.5")` |
| [`r_replacemodels`](../38-cvars-reference/01-video-rendering-cvars.md#r_replacemodels) | `cvar r_replacemodels(string, "md3 md2 md5mesh")` |
| [`r_rocketlight`](../38-cvars-reference/01-video-rendering-cvars.md#r_rocketlight) | `cvar r_rocketlight(int, "1")` |
| [`r_rocketlight_colour`](../38-cvars-reference/01-video-rendering-cvars.md#r_rocketlight_colour) | `cvar r_rocketlight_colour(string, "2.0 1.0 0.25 200")` |
| [`r_rockettrail`](../38-cvars-reference/01-video-rendering-cvars.md#r_rockettrail) | `cvar r_rockettrail(int, "1")` |
| [`r_showbboxes`](../38-cvars-reference/01-video-rendering-cvars.md#r_showbboxes) | `cvar r_showbboxes(int, "0")` |
| [`r_showfields`](../38-cvars-reference/01-video-rendering-cvars.md#r_showfields) | `cvar r_showfields(int, "0")` |
| [`r_slimealpha`](../38-cvars-reference/01-video-rendering-cvars.md#r_slimealpha) | `cvar r_slimealpha(string, "")` |
| [`r_slimestyle`](../38-cvars-reference/01-video-rendering-cvars.md#r_slimestyle) | `cvar r_slimestyle(string, "")` |
| [`r_softwarebanding`](../38-cvars-reference/01-video-rendering-cvars.md#r_softwarebanding) | `cvar r_softwarebanding(int, "0")` |
| [`r_speeds`](../38-cvars-reference/01-video-rendering-cvars.md#r_speeds) | `cvar r_speeds(int, "0")` |
| [`r_sprite_backfacing`](../38-cvars-reference/01-video-rendering-cvars.md#r_sprite_backfacing) | `cvar r_sprite_backfacing(int, "0")` |
| [`r_stainfadeammount`](../38-cvars-reference/01-video-rendering-cvars.md#r_stainfadeammount) | `cvar r_stainfadeammount(int, "1")` |
| [`r_stainfadetime`](../38-cvars-reference/01-video-rendering-cvars.md#r_stainfadetime) | `cvar r_stainfadetime(int, "1")` |
| [`r_stereo_convergence`](../38-cvars-reference/01-video-rendering-cvars.md#r_stereo_convergence) | `cvar r_stereo_convergence(int, "0")` |
| [`r_stereo_method`](../38-cvars-reference/01-video-rendering-cvars.md#r_stereo_method) | `cvar r_stereo_method(int, "0")` |
| [`r_stereo_separation`](../38-cvars-reference/01-video-rendering-cvars.md#r_stereo_separation) | `cvar r_stereo_separation(int, "4")` |
| [`r_subdivisions`](../38-cvars-reference/01-video-rendering-cvars.md#r_subdivisions) | `cvar r_subdivisions(int, "2")` |
| [`r_telealpha`](../38-cvars-reference/01-video-rendering-cvars.md#r_telealpha) | `cvar r_telealpha(string, "")` |
| [`r_telestyle`](../38-cvars-reference/01-video-rendering-cvars.md#r_telestyle) | `cvar r_telestyle(int, "1")` |
| [`r_temporalscenecache`](../38-cvars-reference/01-video-rendering-cvars.md#r_temporalscenecache) | `cvar r_temporalscenecache(string, "")` |
| [`r_tessellation`](../38-cvars-reference/01-video-rendering-cvars.md#r_tessellation) | `cvar r_tessellation(int, "0")` |
| [`r_tessellation_level`](../38-cvars-reference/01-video-rendering-cvars.md#r_tessellation_level) | `cvar r_tessellation_level(int, "5")` |
| [`r_torch`](../38-cvars-reference/01-video-rendering-cvars.md#r_torch) | `cvar r_torch(int, "0")` |
| [`r_tracker_fadetime`](../38-cvars-reference/01-video-rendering-cvars.md#r_tracker_fadetime) | `cvar r_tracker_fadetime(int, "1")` |
| [`r_tracker_frags`](../38-cvars-reference/01-video-rendering-cvars.md#r_tracker_frags) | `cvar r_tracker_frags(int, "0")` |
| [`r_tracker_lines`](../38-cvars-reference/01-video-rendering-cvars.md#r_tracker_lines) | `cvar r_tracker_lines(int, "8")` |
| [`r_tracker_time`](../38-cvars-reference/01-video-rendering-cvars.md#r_tracker_time) | `cvar r_tracker_time(int, "4")` |
| [`r_tracker_w`](../38-cvars-reference/01-video-rendering-cvars.md#r_tracker_w) | `cvar r_tracker_w(float, "0.5")` |
| [`r_tracker_x`](../38-cvars-reference/01-video-rendering-cvars.md#r_tracker_x) | `cvar r_tracker_x(float, "0.5")` |
| [`r_tracker_y`](../38-cvars-reference/01-video-rendering-cvars.md#r_tracker_y) | `cvar r_tracker_y(float, "0.333")` |
| [`r_vertexdlights`](../38-cvars-reference/01-video-rendering-cvars.md#r_vertexdlights) | `cvar r_vertexdlights(int, "0")` |
| [`r_viewpreselgun`](../38-cvars-reference/01-video-rendering-cvars.md#r_viewpreselgun) | `cvar r_viewpreselgun(int, "0")` |
| [`r_wallcolour`](../38-cvars-reference/01-video-rendering-cvars.md#r_wallcolour) | `cvar r_wallcolour(string, "128 128 128")` |
| [`r_walltexture`](../38-cvars-reference/01-video-rendering-cvars.md#r_walltexture) | `cvar r_walltexture(string, "")` |
| [`r_wireframe_smooth`](../38-cvars-reference/01-video-rendering-cvars.md#r_wireframe_smooth) | `cvar r_wireframe_smooth(int, "0")` |
| [`ruleset_allow_larger_models`](../38-cvars-reference/01-video-rendering-cvars.md#ruleset_allow_larger_models) | `cvar ruleset_allow_larger_models(int, "1")` |
| [`v_bonusflash`](../38-cvars-reference/01-video-rendering-cvars.md#v_bonusflash) | `cvar v_bonusflash(int, "1")` |
| [`v_centermove`](../38-cvars-reference/01-video-rendering-cvars.md#v_centermove) | `cvar v_centermove(float, "0.15")` |
| [`v_centerspeed`](../38-cvars-reference/01-video-rendering-cvars.md#v_centerspeed) | `cvar v_centerspeed(int, "500")` |
| [`v_contentblend`](../38-cvars-reference/01-video-rendering-cvars.md#v_contentblend) | `cvar v_contentblend(int, "1")` |
| [`v_cshift_empty`](../38-cvars-reference/01-video-rendering-cvars.md#v_cshift_empty) | `cvar v_cshift_empty(string, "130 80 50 0")` |
| [`v_cshift_lava`](../38-cvars-reference/01-video-rendering-cvars.md#v_cshift_lava) | `cvar v_cshift_lava(string, "255 80 0 150")` |
| [`v_cshift_slime`](../38-cvars-reference/01-video-rendering-cvars.md#v_cshift_slime) | `cvar v_cshift_slime(string, "0 25 5 150")` |
| [`v_cshift_water`](../38-cvars-reference/01-video-rendering-cvars.md#v_cshift_water) | `cvar v_cshift_water(string, "130 80 50 128")` |
| [`v_damagecshift`](../38-cvars-reference/01-video-rendering-cvars.md#v_damagecshift) | `cvar v_damagecshift(int, "1")` |
| [`v_deathtilt`](../38-cvars-reference/01-video-rendering-cvars.md#v_deathtilt) | `cvar v_deathtilt(int, "1")` |
| [`v_depthsortentities`](../38-cvars-reference/01-video-rendering-cvars.md#v_depthsortentities) | `cvar v_depthsortentities(int, "0")` |
| [`v_gunkick`](../38-cvars-reference/01-video-rendering-cvars.md#v_gunkick) | `cvar v_gunkick(int, "0")` |
| [`v_gunkick_q2`](../38-cvars-reference/01-video-rendering-cvars.md#v_gunkick_q2) | `cvar v_gunkick_q2(int, "1")` |
| [`v_idlescale`](../38-cvars-reference/01-video-rendering-cvars.md#v_idlescale) | `cvar v_idlescale(int, "0")` |
| [`v_ipitch_cycle`](../38-cvars-reference/01-video-rendering-cvars.md#v_ipitch_cycle) | `cvar v_ipitch_cycle(int, "1")` |
| [`v_ipitch_level`](../38-cvars-reference/01-video-rendering-cvars.md#v_ipitch_level) | `cvar v_ipitch_level(float, "0.3")` |
| [`v_iroll_cycle`](../38-cvars-reference/01-video-rendering-cvars.md#v_iroll_cycle) | `cvar v_iroll_cycle(float, "0.5")` |
| [`v_iroll_level`](../38-cvars-reference/01-video-rendering-cvars.md#v_iroll_level) | `cvar v_iroll_level(float, "0.1")` |
| [`v_iyaw_cycle`](../38-cvars-reference/01-video-rendering-cvars.md#v_iyaw_cycle) | `cvar v_iyaw_cycle(int, "2")` |
| [`v_iyaw_level`](../38-cvars-reference/01-video-rendering-cvars.md#v_iyaw_level) | `cvar v_iyaw_level(float, "0.3")` |
| [`v_kickpitch`](../38-cvars-reference/01-video-rendering-cvars.md#v_kickpitch) | `cvar v_kickpitch(float, "0.6")` |
| [`v_kickroll`](../38-cvars-reference/01-video-rendering-cvars.md#v_kickroll) | `cvar v_kickroll(float, "0.6")` |
| [`v_kicktime`](../38-cvars-reference/01-video-rendering-cvars.md#v_kicktime) | `cvar v_kicktime(float, "0.5")` |
| [`v_pentcshift`](../38-cvars-reference/01-video-rendering-cvars.md#v_pentcshift) | `cvar v_pentcshift(int, "1")` |
| [`v_powerupshell`](../38-cvars-reference/01-video-rendering-cvars.md#v_powerupshell) | `cvar v_powerupshell(int, "0")` |
| [`v_projectionmode`](../38-cvars-reference/01-video-rendering-cvars.md#v_projectionmode) | `cvar v_projectionmode(int, "0")` |
| [`v_quadcshift`](../38-cvars-reference/01-video-rendering-cvars.md#v_quadcshift) | `cvar v_quadcshift(int, "1")` |
| [`v_ringcshift`](../38-cvars-reference/01-video-rendering-cvars.md#v_ringcshift) | `cvar v_ringcshift(int, "1")` |
| [`v_suitcshift`](../38-cvars-reference/01-video-rendering-cvars.md#v_suitcshift) | `cvar v_suitcshift(int, "1")` |
| [`v_viewheight`](../38-cvars-reference/01-video-rendering-cvars.md#v_viewheight) | `cvar v_viewheight(int, "0")` |
| [`vid_baseheight`](../38-cvars-reference/01-video-rendering-cvars.md#vid_baseheight) | `cvar vid_baseheight(string, "")` |
| [`vid_devicename`](../38-cvars-reference/01-video-rendering-cvars.md#vid_devicename) | `cvar vid_devicename(string, "")` |
| [`vid_dpi_x`](../38-cvars-reference/01-video-rendering-cvars.md#vid_dpi_x) | `cvar vid_dpi_x(int, "0")` |
| [`vid_dpi_y`](../38-cvars-reference/01-video-rendering-cvars.md#vid_dpi_y) | `cvar vid_dpi_y(int, "0")` |
| [`vid_minsize`](../38-cvars-reference/01-video-rendering-cvars.md#vid_minsize) | `cvar vid_minsize(string, "320 200")` |
| [`vid_triplebuffer`](../38-cvars-reference/01-video-rendering-cvars.md#vid_triplebuffer) | `cvar vid_triplebuffer(int, "1")` |
| [`vid_winthread`](../38-cvars-reference/01-video-rendering-cvars.md#vid_winthread) | `cvar vid_winthread(string, "")` |
| [`vid_wndalpha`](../38-cvars-reference/01-video-rendering-cvars.md#vid_wndalpha) | `cvar vid_wndalpha(int, "1")` |
| [`viewsize`](../38-cvars-reference/01-video-rendering-cvars.md#viewsize) | `cvar viewsize(int, "100")` |
| [`vk_khr_dedicated_allocation`](../38-cvars-reference/01-video-rendering-cvars.md#vk_khr_dedicated_allocation) | `cvar vk_khr_dedicated_allocation(string, "")` |
| [`vk_khr_get_memory_requirements2`](../38-cvars-reference/01-video-rendering-cvars.md#vk_khr_get_memory_requirements2) | `cvar vk_khr_get_memory_requirements2(string, "")` |
| [`vk_khr_push_descriptor`](../38-cvars-reference/01-video-rendering-cvars.md#vk_khr_push_descriptor) | `cvar vk_khr_push_descriptor(string, "")` |
| [`vk_khr_ray_query`](../38-cvars-reference/01-video-rendering-cvars.md#vk_khr_ray_query) | `cvar vk_khr_ray_query(string, "")` |
| [`worker_count`](../38-cvars-reference/01-video-rendering-cvars.md#worker_count) | `cvar worker_count(string, "")` |
| [`worker_flush`](../38-cvars-reference/01-video-rendering-cvars.md#worker_flush) | `cvar worker_flush(int, "1")` |
| [`worker_sleeptime`](../38-cvars-reference/01-video-rendering-cvars.md#worker_sleeptime) | `cvar worker_sleeptime(int, "0")` |

### Освещение, тени и материалы

| Элемент | Сигнатура / Описание |
|---|---|
| [`gl_skyboxdist`](../38-cvars-reference/02-lighting-materials-cvars.md#gl_skyboxdist) | `cvar gl_skyboxdist(float, "0")` |
| [`mod_map_lights`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_map_lights) | `cvar mod_map_lights(int, "0")` |
| [`mod_map_texscale`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_map_texscale) | `cvar mod_map_texscale(float, "1")` |
| [`mod_terrain_ambient`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_terrain_ambient) | `cvar mod_terrain_ambient(float, "0.5")` |
| [`mod_terrain_shadow_dist`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_terrain_shadow_dist) | `cvar mod_terrain_shadow_dist(float, "2048")` |
| [`mod_terrain_shadows`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_terrain_shadows) | `cvar mod_terrain_shadows(bool, "0")` |
| [`mod_terrain_sundir`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_terrain_sundir) | `cvar mod_terrain_sundir(vector3, "0.4 0.7 2")` |
| [`r_bloom`](../38-cvars-reference/02-lighting-materials-cvars.md#r_bloom) | `cvar r_bloom(float, "0")` |
| [`r_bloom_downsize`](../38-cvars-reference/02-lighting-materials-cvars.md#r_bloom_downsize) | `cvar r_bloom_downsize(bool, "0")` |
| [`r_bloom_filter`](../38-cvars-reference/02-lighting-materials-cvars.md#r_bloom_filter) | `cvar r_bloom_filter(vector3, "0.7 0.7 0.7")` |
| [`r_bloom_initialscale`](../38-cvars-reference/02-lighting-materials-cvars.md#r_bloom_initialscale) | `cvar r_bloom_initialscale(float, "1")` |
| [`r_bloom_retain`](../38-cvars-reference/02-lighting-materials-cvars.md#r_bloom_retain) | `cvar r_bloom_retain(float, "1")` |
| [`r_bloom_size`](../38-cvars-reference/02-lighting-materials-cvars.md#r_bloom_size) | `cvar r_bloom_size(float, "4")` |
| [`r_fastsky`](../38-cvars-reference/02-lighting-materials-cvars.md#r_fastsky) | `cvar r_fastsky(bool, "0")` |
| [`r_fastskycolour`](../38-cvars-reference/02-lighting-materials-cvars.md#r_fastskycolour) | `cvar r_fastskycolour(vector3, "0")` |
| [`r_forceprogramify`](../38-cvars-reference/02-lighting-materials-cvars.md#r_forceprogramify) | `cvar r_forceprogramify(int, "0")` |
| [`r_glsl_precache`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_precache) | `cvar r_glsl_precache(bool, "0")` |
| [`r_glsl_skybox_autorotate`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_skybox_autorotate) | `cvar r_glsl_skybox_autorotate(bool, "1")` |
| [`r_glsl_skybox_orientation`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_skybox_orientation) | `cvar r_glsl_skybox_orientation(vector4, "0 0 0 0")` |
| [`r_halfrate`](../38-cvars-reference/02-lighting-materials-cvars.md#r_halfrate) | `cvar r_halfrate(bool, "0")` |
| [`r_shadow_playershadows`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_playershadows) | `cvar r_shadow_playershadows(bool, "1")` |
| [`r_shadow_raytrace`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_raytrace) | `cvar r_shadow_raytrace(bool, "0")` |
| [`r_shadow_realtime_dlight`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_dlight) | `cvar r_shadow_realtime_dlight(bool, "1")` |
| [`r_shadow_realtime_dlight_ambient`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_dlight_ambient) | `cvar r_shadow_realtime_dlight_ambient(float, "0")` |
| [`r_shadow_realtime_dlight_diffuse`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_dlight_diffuse) | `cvar r_shadow_realtime_dlight_diffuse(float, "1")` |
| [`r_shadow_realtime_dlight_shadows`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_dlight_shadows) | `cvar r_shadow_realtime_dlight_shadows(bool, "1")` |
| [`r_shadow_realtime_dlight_specular`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_dlight_specular) | `cvar r_shadow_realtime_dlight_specular(float, "4")` |
| [`r_shadow_realtime_world`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_world) | `cvar r_shadow_realtime_world(bool, "0")` |
| [`r_shadow_realtime_world_importlightentitiesfrommap`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_world_importlightentitiesfrommap) | `cvar r_shadow_realtime_world_importlightentitiesfrommap(int, "0")` |
| [`r_shadow_realtime_world_lightmaps`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_world_lightmaps) | `cvar r_shadow_realtime_world_lightmaps(float, "0")` |
| [`r_shadow_realtime_world_shadows`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_world_shadows) | `cvar r_shadow_realtime_world_shadows(bool, "1")` |
| [`r_shadow_scissor`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_scissor) | `cvar r_shadow_scissor(bool, "1")` |
| [`r_shadow_shadowmapping`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_shadowmapping) | `cvar r_shadow_shadowmapping(bool, "1")` |
| [`r_shadow_shadowmapping_bias`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_shadowmapping_bias) | `cvar r_shadow_shadowmapping_bias(float, "0.03")` |
| [`r_shadow_shadowmapping_depthbits`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_shadowmapping_depthbits) | `cvar r_shadow_shadowmapping_depthbits(int, "16")` |
| [`r_shadow_shadowmapping_nearclip`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_shadowmapping_nearclip) | `cvar r_shadow_shadowmapping_nearclip(float, "1")` |
| [`r_shadow_shadowmapping_precision`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_shadowmapping_precision) | `cvar r_shadow_shadowmapping_precision(float, "1")` |
| [`r_shadows_fakedistance`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadows_fakedistance) | `cvar r_shadows_fakedistance(float, "1024")` |
| [`r_shadows_focus`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadows_focus) | `cvar r_shadows_focus(vector3, "0 0 0")` |
| [`r_shadows_throwdirection`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadows_throwdirection) | `cvar r_shadows_throwdirection(vector3, "0 0 -1")` |
| [`r_skybox`](../38-cvars-reference/02-lighting-materials-cvars.md#r_skybox) | `cvar r_skybox(string, "")` |
| [`r_skycloudalpha`](../38-cvars-reference/02-lighting-materials-cvars.md#r_skycloudalpha) | `cvar r_skycloudalpha(float, "1")` |
| [`r_skyfog`](../38-cvars-reference/02-lighting-materials-cvars.md#r_skyfog) | `cvar r_skyfog(float, "0.5")` |
| [`r_sun_colour`](../38-cvars-reference/02-lighting-materials-cvars.md#r_sun_colour) | `cvar r_sun_colour(vector3, "0 0 0")` |
| [`r_sun_dir`](../38-cvars-reference/02-lighting-materials-cvars.md#r_sun_dir) | `cvar r_sun_dir(vector3, "0.2 0.5 0.8")` |
| [`r_vertexlight`](../38-cvars-reference/02-lighting-materials-cvars.md#r_vertexlight) | `cvar r_vertexlight(bool, "0")` |
| [`gl_flashblend`](../38-cvars-reference/02-lighting-materials-cvars.md#gl_flashblend) | `cvar gl_flashblend(int, "0")` |
| [`gl_flashblendscale`](../38-cvars-reference/02-lighting-materials-cvars.md#gl_flashblendscale) | `cvar gl_flashblendscale(float, "0.35")` |
| [`gl_menutint_shader`](../38-cvars-reference/02-lighting-materials-cvars.md#gl_menutint_shader) | `cvar gl_menutint_shader(int, "1")` |
| [`gl_workaround_ati_shadersource`](../38-cvars-reference/02-lighting-materials-cvars.md#gl_workaround_ati_shadersource) | `cvar gl_workaround_ati_shadersource(int, "1")` |
| [`mod_terrain_defaulttexture`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_terrain_defaulttexture) | `cvar mod_terrain_defaulttexture(string, "")` |
| [`mod_terrain_networked`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_terrain_networked) | `cvar mod_terrain_networked(int, "0")` |
| [`mod_terrain_savever`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_terrain_savever) | `cvar mod_terrain_savever(string, "")` |
| [`r_ambient`](../38-cvars-reference/02-lighting-materials-cvars.md#r_ambient) | `cvar r_ambient(int, "0")` |
| [`r_dynamic`](../38-cvars-reference/02-lighting-materials-cvars.md#r_dynamic) | `cvar r_dynamic(int, "1")` |
| [`r_fullbright`](../38-cvars-reference/02-lighting-materials-cvars.md#r_fullbright) | `cvar r_fullbright(int, "0")` |
| [`r_fullbrightSkins`](../38-cvars-reference/02-lighting-materials-cvars.md#r_fullbrightskins) | `cvar r_fullbrightSkins(float, "0.8")` |
| [`r_glsl_emissive`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_emissive) | `cvar r_glsl_emissive(int, "1")` |
| [`r_glsl_offsetmapping`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_offsetmapping) | `cvar r_glsl_offsetmapping(int, "0")` |
| [`r_glsl_offsetmapping_reliefmapping`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_offsetmapping_reliefmapping) | `cvar r_glsl_offsetmapping_reliefmapping(int, "0")` |
| [`r_glsl_offsetmapping_scale`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_offsetmapping_scale) | `cvar r_glsl_offsetmapping_scale(float, "0.04")` |
| [`r_glsl_pbr`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_pbr) | `cvar r_glsl_pbr(int, "0")` |
| [`r_glsl_turbscale_reflect`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_turbscale_reflect) | `cvar r_glsl_turbscale_reflect(int, "1")` |
| [`r_glsl_turbscale_refract`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_turbscale_refract) | `cvar r_glsl_turbscale_refract(int, "1")` |
| [`r_lightflicker`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightflicker) | `cvar r_lightflicker(int, "1")` |
| [`r_lightmap`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightmap) | `cvar r_lightmap(int, "0")` |
| [`r_lightmap_format`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightmap_format) | `cvar r_lightmap_format(string, "")` |
| [`r_lightmap_saturation`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightmap_saturation) | `cvar r_lightmap_saturation(int, "1")` |
| [`r_lightprepass`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightprepass) | `cvar r_lightprepass(int, "0")` |
| [`r_lightstylescale`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightstylescale) | `cvar r_lightstylescale(int, "1")` |
| [`r_lightstylesmooth`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightstylesmooth) | `cvar r_lightstylesmooth(int, "0")` |
| [`r_lightstylesmooth_limit`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightstylesmooth_limit) | `cvar r_lightstylesmooth_limit(int, "2")` |
| [`r_lightstylespeed`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightstylespeed) | `cvar r_lightstylespeed(int, "10")` |
| [`r_particledesc`](../38-cvars-reference/02-lighting-materials-cvars.md#r_particledesc) | `cvar r_particledesc(string, "")` |
| [`r_postprocshader`](../38-cvars-reference/02-lighting-materials-cvars.md#r_postprocshader) | `cvar r_postprocshader(string, "")` |
| [`r_shaderblobs`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shaderblobs) | `cvar r_shaderblobs(int, "0")` |
| [`r_shadow_bumpscale_basetexture`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_bumpscale_basetexture) | `cvar r_shadow_bumpscale_basetexture(int, "0")` |
| [`r_shadow_bumpscale_bumpmap`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_bumpscale_bumpmap) | `cvar r_shadow_bumpscale_bumpmap(int, "4")` |
| [`r_shadow_heightscale_basetexture`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_heightscale_basetexture) | `cvar r_shadow_heightscale_basetexture(int, "0")` |
| [`r_shadow_heightscale_bumpmap`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_heightscale_bumpmap) | `cvar r_shadow_heightscale_bumpmap(int, "1")` |
| [`r_shadow_realtime_nonworld_lightmaps`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_nonworld_lightmaps) | `cvar r_shadow_realtime_nonworld_lightmaps(int, "1")` |
| [`r_shadows`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadows) | `cvar r_shadows(int, "0")` |
| [`r_showshaders`](../38-cvars-reference/02-lighting-materials-cvars.md#r_showshaders) | `cvar r_showshaders(int, "0")` |
| [`r_stains`](../38-cvars-reference/02-lighting-materials-cvars.md#r_stains) | `cvar r_stains(int, "0")` |
| [`ruleset_allow_shaders`](../38-cvars-reference/02-lighting-materials-cvars.md#ruleset_allow_shaders) | `cvar ruleset_allow_shaders(int, "1")` |

### Звук

| Элемент | Сигнатура / Описание |
|---|---|
| [`capturesound`](../38-cvars-reference/03-audio-cvars.md#capturesound) | `cvar capturesound(целое, "1")` |
| [`capturesoundbits`](../38-cvars-reference/03-audio-cvars.md#capturesoundbits) | `cvar capturesoundbits(целое, "16")` |
| [`capturesoundchannels`](../38-cvars-reference/03-audio-cvars.md#capturesoundchannels) | `cvar capturesoundchannels(целое, "2")` |
| [`cl_voip_capturedevice`](../38-cvars-reference/03-audio-cvars.md#cl_voip_capturedevice) | `cvar cl_voip_capturedevice(строка, "")` |
| [`cl_voip_send`](../38-cvars-reference/03-audio-cvars.md#cl_voip_send) | `cvar cl_voip_send(целое, "0")` |
| [`cl_voip_test`](../38-cvars-reference/03-audio-cvars.md#cl_voip_test) | `cvar cl_voip_test(целое, "0")` |
| [`cl_voip_vad_delay`](../38-cvars-reference/03-audio-cvars.md#cl_voip_vad_delay) | `cvar cl_voip_vad_delay(дробное, "0.3")` |
| [`cl_voip_vad_threshhold`](../38-cvars-reference/03-audio-cvars.md#cl_voip_vad_threshhold) | `cvar cl_voip_vad_threshhold(целое, "15")` |
| [`mastervolume`](../38-cvars-reference/03-audio-cvars.md#mastervolume) | `cvar mastervolume(дробное, "1")` |
| [`media_hijackwinamp`](../38-cvars-reference/03-audio-cvars.md#media_hijackwinamp) | `cvar media_hijackwinamp(целое, "0")` |
| [`media_repeat`](../38-cvars-reference/03-audio-cvars.md#media_repeat) | `cvar media_repeat(целое, "1")` |
| [`media_shuffle`](../38-cvars-reference/03-audio-cvars.md#media_shuffle) | `cvar media_shuffle(целое, "1")` |
| [`music_fade`](../38-cvars-reference/03-audio-cvars.md#music_fade) | `cvar music_fade(целое, "1")` |
| [`music_playlist_index`](../38-cvars-reference/03-audio-cvars.md#music_playlist_index) | `cvar music_playlist_index(целое, "-1")` |
| [`nosound`](../38-cvars-reference/03-audio-cvars.md#nosound) | `cvar nosound(целое, "0")` |
| [`s_al_debug`](../38-cvars-reference/03-audio-cvars.md#s_al_debug) | `cvar s_al_debug(целое, "0")` |
| [`s_al_disable`](../38-cvars-reference/03-audio-cvars.md#s_al_disable) | `cvar s_al_disable(целое, "0")` |
| [`s_al_hrtf`](../38-cvars-reference/03-audio-cvars.md#s_al_hrtf) | `cvar s_al_hrtf(строка, "")` |
| [`s_al_reference_distance`](../38-cvars-reference/03-audio-cvars.md#s_al_reference_distance) | `cvar s_al_reference_distance(дробное, "120")` |
| [`s_al_use_reverb`](../38-cvars-reference/03-audio-cvars.md#s_al_use_reverb) | `cvar s_al_use_reverb(целое, "1")` |
| [`s_al_velocityscale`](../38-cvars-reference/03-audio-cvars.md#s_al_velocityscale) | `cvar s_al_velocityscale(дробное, "1")` |
| [`snd_ignorecueloops`](../38-cvars-reference/03-audio-cvars.md#snd_ignorecueloops) | `cvar snd_ignorecueloops(целое, "0")` |
| [`snd_ignoregamespeed`](../38-cvars-reference/03-audio-cvars.md#snd_ignoregamespeed) | `cvar snd_ignoregamespeed(целое, "0")` |
| [`snd_loadasstereo`](../38-cvars-reference/03-audio-cvars.md#snd_loadasstereo) | `cvar snd_loadasstereo(целое, "0")` |
| [`snd_playbackrate`](../38-cvars-reference/03-audio-cvars.md#snd_playbackrate) | `cvar snd_playbackrate(дробное, "1")` |
| [`tts_mode`](../38-cvars-reference/03-audio-cvars.md#tts_mode) | `cvar tts_mode(целое, "1")` |
| [`wasapi_buffersize`](../38-cvars-reference/03-audio-cvars.md#wasapi_buffersize) | `cvar wasapi_buffersize(дробное, "0.01")` |
| [`wasapi_exclusive`](../38-cvars-reference/03-audio-cvars.md#wasapi_exclusive) | `cvar wasapi_exclusive(целое, "0")` |
| [`wasapi_forcechannels`](../38-cvars-reference/03-audio-cvars.md#wasapi_forcechannels) | `cvar wasapi_forcechannels(целое, "0")` |
| [`wasapi_forcerate`](../38-cvars-reference/03-audio-cvars.md#wasapi_forcerate) | `cvar wasapi_forcerate(целое, "0")` |
| [`_cl_voip_capturedevice_opts`](../38-cvars-reference/03-audio-cvars.md#_cl_voip_capturedevice_opts) | `cvar _cl_voip_capturedevice_opts(string, "")` |
| [`_s_device_opts`](../38-cvars-reference/03-audio-cvars.md#_s_device_opts) | `cvar _s_device_opts(string, "")` |
| [`cl_chatsound`](../38-cvars-reference/03-audio-cvars.md#cl_chatsound) | `cvar cl_chatsound(int, "1")` |
| [`cl_cursor_bias_x`](../38-cvars-reference/03-audio-cvars.md#cl_cursor_bias_x) | `cvar cl_cursor_bias_x(float, "0.0")` |
| [`cl_cursor_bias_y`](../38-cvars-reference/03-audio-cvars.md#cl_cursor_bias_y) | `cvar cl_cursor_bias_y(float, "0.0")` |
| [`cl_enemychatsound`](../38-cvars-reference/03-audio-cvars.md#cl_enemychatsound) | `cvar cl_enemychatsound(string, "misc/talk.wav")` |
| [`cl_maxfps_slop`](../38-cvars-reference/03-audio-cvars.md#cl_maxfps_slop) | `cvar cl_maxfps_slop(int, "3")` |
| [`cl_predict_players_frac`](../38-cvars-reference/03-audio-cvars.md#cl_predict_players_frac) | `cvar cl_predict_players_frac(float, "0.9")` |
| [`cl_predict_players_latency`](../38-cvars-reference/03-audio-cvars.md#cl_predict_players_latency) | `cvar cl_predict_players_latency(float, "1.0")` |
| [`cl_predict_players_nudge`](../38-cvars-reference/03-audio-cvars.md#cl_predict_players_nudge) | `cvar cl_predict_players_nudge(float, "0.02")` |
| [`cl_staticsounds`](../38-cvars-reference/03-audio-cvars.md#cl_staticsounds) | `cvar cl_staticsounds(int, "1")` |
| [`cl_teamchatsound`](../38-cvars-reference/03-audio-cvars.md#cl_teamchatsound) | `cvar cl_teamchatsound(string, "misc/talk.wav")` |
| [`cl_voip_autogain`](../38-cvars-reference/03-audio-cvars.md#cl_voip_autogain) | `cvar cl_voip_autogain(int, "0")` |
| [`cl_voip_bitrate`](../38-cvars-reference/03-audio-cvars.md#cl_voip_bitrate) | `cvar cl_voip_bitrate(int, "3000")` |
| [`cl_voip_capturingvol`](../38-cvars-reference/03-audio-cvars.md#cl_voip_capturingvol) | `cvar cl_voip_capturingvol(float, "0.5")` |
| [`cl_voip_codec`](../38-cvars-reference/03-audio-cvars.md#cl_voip_codec) | `cvar cl_voip_codec(string, "")` |
| [`cl_voip_ducking`](../38-cvars-reference/03-audio-cvars.md#cl_voip_ducking) | `cvar cl_voip_ducking(float, "0.5")` |
| [`cl_voip_micamp`](../38-cvars-reference/03-audio-cvars.md#cl_voip_micamp) | `cvar cl_voip_micamp(int, "2")` |
| [`cl_voip_noisefilter`](../38-cvars-reference/03-audio-cvars.md#cl_voip_noisefilter) | `cvar cl_voip_noisefilter(int, "1")` |
| [`cl_voip_play`](../38-cvars-reference/03-audio-cvars.md#cl_voip_play) | `cvar cl_voip_play(int, "1")` |
| [`cl_voip_showmeter`](../38-cvars-reference/03-audio-cvars.md#cl_voip_showmeter) | `cvar cl_voip_showmeter(int, "1")` |
| [`dpcompat_precachesoundhack`](../38-cvars-reference/03-audio-cvars.md#dpcompat_precachesoundhack) | `cvar dpcompat_precachesoundhack(int, "0")` |
| [`dtls_psk_hint`](../38-cvars-reference/03-audio-cvars.md#dtls_psk_hint) | `cvar dtls_psk_hint(string, "")` |
| [`dtls_psk_key`](../38-cvars-reference/03-audio-cvars.md#dtls_psk_key) | `cvar dtls_psk_key(string, "")` |
| [`dtls_psk_user`](../38-cvars-reference/03-audio-cvars.md#dtls_psk_user) | `cvar dtls_psk_user(string, "")` |
| [`fs_basepath`](../38-cvars-reference/03-audio-cvars.md#fs_basepath) | `cvar fs_basepath(string, "")` |
| [`fs_cache`](../38-cvars-reference/03-audio-cvars.md#fs_cache) | `cvar fs_cache(int, "2")` |
| [`fs_game`](../38-cvars-reference/03-audio-cvars.md#fs_game) | `cvar fs_game(string, "")` |
| [`fs_gamepath`](../38-cvars-reference/03-audio-cvars.md#fs_gamepath) | `cvar fs_gamepath(string, "")` |
| [`fs_hidesyspaths`](../38-cvars-reference/03-audio-cvars.md#fs_hidesyspaths) | `cvar fs_hidesyspaths(int, "0")` |
| [`fs_homepath`](../38-cvars-reference/03-audio-cvars.md#fs_homepath) | `cvar fs_homepath(string, "")` |
| [`fs_noreexec`](../38-cvars-reference/03-audio-cvars.md#fs_noreexec) | `cvar fs_noreexec(int, "0")` |
| [`fs_packageprioritisation`](../38-cvars-reference/03-audio-cvars.md#fs_packageprioritisation) | `cvar fs_packageprioritisation(int, "1")` |
| [`mod_litsprites_force`](../38-cvars-reference/03-audio-cvars.md#mod_litsprites_force) | `cvar mod_litsprites_force(int, "0")` |
| [`net_dns_ipv4`](../38-cvars-reference/03-audio-cvars.md#net_dns_ipv4) | `cvar net_dns_ipv4(int, "1")` |
| [`net_dns_ipv6`](../38-cvars-reference/03-audio-cvars.md#net_dns_ipv6) | `cvar net_dns_ipv6(int, "1")` |
| [`qws_builddate`](../38-cvars-reference/03-audio-cvars.md#qws_builddate) | `cvar qws_builddate(string, "")` |
| [`qws_buildnum`](../38-cvars-reference/03-audio-cvars.md#qws_buildnum) | `cvar qws_buildnum(string, "")` |
| [`qws_fullname`](../38-cvars-reference/03-audio-cvars.md#qws_fullname) | `cvar qws_fullname(string, "")` |
| [`qws_homepage`](../38-cvars-reference/03-audio-cvars.md#qws_homepage) | `cvar qws_homepage(string, "")` |
| [`qws_name`](../38-cvars-reference/03-audio-cvars.md#qws_name) | `cvar qws_name(string, "")` |
| [`qws_platform`](../38-cvars-reference/03-audio-cvars.md#qws_platform) | `cvar qws_platform(string, "")` |
| [`qws_version`](../38-cvars-reference/03-audio-cvars.md#qws_version) | `cvar qws_version(string, ".")` |
| [`r_coronas_fadedist`](../38-cvars-reference/03-audio-cvars.md#r_coronas_fadedist) | `cvar r_coronas_fadedist(int, "256")` |
| [`r_coronas_intensity`](../38-cvars-reference/03-audio-cvars.md#r_coronas_intensity) | `cvar r_coronas_intensity(int, "1")` |
| [`r_coronas_mindist`](../38-cvars-reference/03-audio-cvars.md#r_coronas_mindist) | `cvar r_coronas_mindist(int, "128")` |
| [`r_coronas_occlusion`](../38-cvars-reference/03-audio-cvars.md#r_coronas_occlusion) | `cvar r_coronas_occlusion(string, "")` |
| [`r_editlights_cursordistance`](../38-cvars-reference/03-audio-cvars.md#r_editlights_cursordistance) | `cvar r_editlights_cursordistance(int, "1024")` |
| [`r_editlights_cursorgrid`](../38-cvars-reference/03-audio-cvars.md#r_editlights_cursorgrid) | `cvar r_editlights_cursorgrid(int, "1")` |
| [`r_editlights_cursorpushback`](../38-cvars-reference/03-audio-cvars.md#r_editlights_cursorpushback) | `cvar r_editlights_cursorpushback(int, "0")` |
| [`r_editlights_cursorpushoff`](../38-cvars-reference/03-audio-cvars.md#r_editlights_cursorpushoff) | `cvar r_editlights_cursorpushoff(int, "4")` |
| [`r_editlights_import_ambient`](../38-cvars-reference/03-audio-cvars.md#r_editlights_import_ambient) | `cvar r_editlights_import_ambient(int, "0")` |
| [`r_editlights_import_diffuse`](../38-cvars-reference/03-audio-cvars.md#r_editlights_import_diffuse) | `cvar r_editlights_import_diffuse(int, "1")` |
| [`r_editlights_import_radius`](../38-cvars-reference/03-audio-cvars.md#r_editlights_import_radius) | `cvar r_editlights_import_radius(int, "1")` |
| [`r_editlights_import_specular`](../38-cvars-reference/03-audio-cvars.md#r_editlights_import_specular) | `cvar r_editlights_import_specular(int, "1")` |
| [`r_font_postprocess_mono`](../38-cvars-reference/03-audio-cvars.md#r_font_postprocess_mono) | `cvar r_font_postprocess_mono(int, "0")` |
| [`r_font_postprocess_outline`](../38-cvars-reference/03-audio-cvars.md#r_font_postprocess_outline) | `cvar r_font_postprocess_outline(int, "0")` |
| [`r_part_sparks_textured`](../38-cvars-reference/03-audio-cvars.md#r_part_sparks_textured) | `cvar r_part_sparks_textured(int, "1")` |
| [`r_part_sparks_trifan`](../38-cvars-reference/03-audio-cvars.md#r_part_sparks_trifan) | `cvar r_part_sparks_trifan(int, "1")` |
| [`rank_parms_first`](../38-cvars-reference/03-audio-cvars.md#rank_parms_first) | `cvar rank_parms_first(int, "0")` |
| [`rank_parms_last`](../38-cvars-reference/03-audio-cvars.md#rank_parms_last) | `cvar rank_parms_last(int, "31")` |
| [`ruleset_allow_overlong_sounds`](../38-cvars-reference/03-audio-cvars.md#ruleset_allow_overlong_sounds) | `cvar ruleset_allow_overlong_sounds(int, "1")` |
| [`s_al_distancemodel`](../38-cvars-reference/03-audio-cvars.md#s_al_distancemodel) | `cvar s_al_distancemodel(int, "2")` |
| [`s_al_dopplerfactor`](../38-cvars-reference/03-audio-cvars.md#s_al_dopplerfactor) | `cvar s_al_dopplerfactor(float, "1.0")` |
| [`s_al_max_distance`](../38-cvars-reference/03-audio-cvars.md#s_al_max_distance) | `cvar s_al_max_distance(int, "1000")` |
| [`s_al_rolloff_factor`](../38-cvars-reference/03-audio-cvars.md#s_al_rolloff_factor) | `cvar s_al_rolloff_factor(int, "1")` |
| [`s_al_speedofsound`](../38-cvars-reference/03-audio-cvars.md#s_al_speedofsound) | `cvar s_al_speedofsound(float, "343.3")` |
| [`s_al_static_listener`](../38-cvars-reference/03-audio-cvars.md#s_al_static_listener) | `cvar s_al_static_listener(int, "0")` |
| [`s_ambientfade`](../38-cvars-reference/03-audio-cvars.md#s_ambientfade) | `cvar s_ambientfade(int, "100")` |
| [`s_ambientlevel`](../38-cvars-reference/03-audio-cvars.md#s_ambientlevel) | `cvar s_ambientlevel(float, "0.3")` |
| [`s_bits`](../38-cvars-reference/03-audio-cvars.md#s_bits) | `cvar s_bits(int, "16")` |
| [`s_buffersize`](../38-cvars-reference/03-audio-cvars.md#s_buffersize) | `cvar s_buffersize(int, "0")` |
| [`s_device`](../38-cvars-reference/03-audio-cvars.md#s_device) | `cvar s_device(string, "")` |
| [`s_doppler`](../38-cvars-reference/03-audio-cvars.md#s_doppler) | `cvar s_doppler(int, "0")` |
| [`s_doppler_max`](../38-cvars-reference/03-audio-cvars.md#s_doppler_max) | `cvar s_doppler_max(int, "2")` |
| [`s_doppler_min`](../38-cvars-reference/03-audio-cvars.md#s_doppler_min) | `cvar s_doppler_min(float, "0.5")` |
| [`s_eax`](../38-cvars-reference/03-audio-cvars.md#s_eax) | `cvar s_eax(int, "0")` |
| [`s_inactive`](../38-cvars-reference/03-audio-cvars.md#s_inactive) | `cvar s_inactive(int, "1")` |
| [`s_khz`](../38-cvars-reference/03-audio-cvars.md#s_khz) | `cvar s_khz(целое/строка, "48")` |
| [`s_linearresample`](../38-cvars-reference/03-audio-cvars.md#s_linearresample) | `cvar s_linearresample(int, "1")` |
| [`s_linearresample_stream`](../38-cvars-reference/03-audio-cvars.md#s_linearresample_stream) | `cvar s_linearresample_stream(int, "0")` |
| [`s_loadas8bit`](../38-cvars-reference/03-audio-cvars.md#s_loadas8bit) | `cvar s_loadas8bit(int, "0")` |
| [`s_localvolume`](../38-cvars-reference/03-audio-cvars.md#s_localvolume) | `cvar s_localvolume(int, "1")` |
| [`s_mixahead`](../38-cvars-reference/03-audio-cvars.md#s_mixahead) | `cvar s_mixahead(float, "0.1")` |
| [`s_mixerthread`](../38-cvars-reference/03-audio-cvars.md#s_mixerthread) | `cvar s_mixerthread(int, "1")` |
| [`s_noextraupdate`](../38-cvars-reference/03-audio-cvars.md#s_noextraupdate) | `cvar s_noextraupdate(int, "0")` |
| [`s_nominaldistance`](../38-cvars-reference/03-audio-cvars.md#s_nominaldistance) | `cvar s_nominaldistance(int, "1000")` |
| [`s_numspeakers`](../38-cvars-reference/03-audio-cvars.md#s_numspeakers) | `cvar s_numspeakers(int, "2")` |
| [`s_precache`](../38-cvars-reference/03-audio-cvars.md#s_precache) | `cvar s_precache(int, "1")` |
| [`s_show`](../38-cvars-reference/03-audio-cvars.md#s_show) | `cvar s_show(int, "0")` |
| [`s_swapstereo`](../38-cvars-reference/03-audio-cvars.md#s_swapstereo) | `cvar s_swapstereo(int, "0")` |
| [`show_fps_x`](../38-cvars-reference/03-audio-cvars.md#show_fps_x) | `cvar show_fps_x(int, "-1")` |
| [`show_fps_y`](../38-cvars-reference/03-audio-cvars.md#show_fps_y) | `cvar show_fps_y(int, "-1")` |
| [`sv_cullentities_trace`](../38-cvars-reference/03-audio-cvars.md#sv_cullentities_trace) | `cvar sv_cullentities_trace(string, "")` |
| [`sv_loadentfiles_dir`](../38-cvars-reference/03-audio-cvars.md#sv_loadentfiles_dir) | `cvar sv_loadentfiles_dir(string, "")` |
| [`sv_sound_land`](../38-cvars-reference/03-audio-cvars.md#sv_sound_land) | `cvar sv_sound_land(string, "demon/dland2.wav")` |
| [`sv_sound_watersplash`](../38-cvars-reference/03-audio-cvars.md#sv_sound_watersplash) | `cvar sv_sound_watersplash(string, "misc/h2ohit1.wav")` |
| [`sv_voip`](../38-cvars-reference/03-audio-cvars.md#sv_voip) | `cvar sv_voip(int, "1")` |
| [`sv_voip_echo`](../38-cvars-reference/03-audio-cvars.md#sv_voip_echo) | `cvar sv_voip_echo(int, "0")` |
| [`sv_voip_record`](../38-cvars-reference/03-audio-cvars.md#sv_voip_record) | `cvar sv_voip_record(int, "0")` |
| [`sys_clockprecision`](../38-cvars-reference/03-audio-cvars.md#sys_clockprecision) | `cvar sys_clockprecision(int, "1")` |
| [`sys_clocktype`](../38-cvars-reference/03-audio-cvars.md#sys_clocktype) | `cvar sys_clocktype(string, "")` |
| [`sys_colorconsole`](../38-cvars-reference/03-audio-cvars.md#sys_colorconsole) | `cvar sys_colorconsole(int, "1")` |
| [`sys_disableTaskSwitch`](../38-cvars-reference/03-audio-cvars.md#sys_disabletaskswitch) | `cvar sys_disableTaskSwitch(int, "0")` |
| [`sys_disableWinKeys`](../38-cvars-reference/03-audio-cvars.md#sys_disablewinkeys) | `cvar sys_disableWinKeys(int, "0")` |
| [`sys_extrasleep`](../38-cvars-reference/03-audio-cvars.md#sys_extrasleep) | `cvar sys_extrasleep(int, "0")` |
| [`sys_highpriority`](../38-cvars-reference/03-audio-cvars.md#sys_highpriority) | `cvar sys_highpriority(int, "0")` |
| [`sys_keepscreenon`](../38-cvars-reference/03-audio-cvars.md#sys_keepscreenon) | `cvar sys_keepscreenon(int, "1")` |
| [`sys_linebuffer`](../38-cvars-reference/03-audio-cvars.md#sys_linebuffer) | `cvar sys_linebuffer(int, "1")` |
| [`sys_nostdout`](../38-cvars-reference/03-audio-cvars.md#sys_nostdout) | `cvar sys_nostdout(int, "0")` |
| [`sys_orientation`](../38-cvars-reference/03-audio-cvars.md#sys_orientation) | `cvar sys_orientation(string, "landscape")` |
| [`sys_platform`](../38-cvars-reference/03-audio-cvars.md#sys_platform) | `cvar sys_platform(string, "")` |
| [`sys_timestamps`](../38-cvars-reference/03-audio-cvars.md#sys_timestamps) | `cvar sys_timestamps(int, "0")` |
| [`sys_vibrate`](../38-cvars-reference/03-audio-cvars.md#sys_vibrate) | `cvar sys_vibrate(int, "1")` |
| [`tls_ignorecertificateerrors`](../38-cvars-reference/03-audio-cvars.md#tls_ignorecertificateerrors) | `cvar tls_ignorecertificateerrors(int, "0")` |
| [`tls_provider`](../38-cvars-reference/03-audio-cvars.md#tls_provider) | `cvar tls_provider(string, "")` |

### Сеть, сервер и мультиплеер

| Элемент | Сигнатура / Описание |
|---|---|
| [`allow_download_configs`](../38-cvars-reference/04-network-server-cvars.md#allow_download_configs) | `cvar allow_download_configs(булево, "0")` |
| [`allow_download_copyrighted`](../38-cvars-reference/04-network-server-cvars.md#allow_download_copyrighted) | `cvar allow_download_copyrighted(булево, "0")` |
| [`allow_download_demos`](../38-cvars-reference/04-network-server-cvars.md#allow_download_demos) | `cvar allow_download_demos(булево, "1")` |
| [`allow_download_logs`](../38-cvars-reference/04-network-server-cvars.md#allow_download_logs) | `cvar allow_download_logs(булево, "0")` |
| [`allow_download_maps`](../38-cvars-reference/04-network-server-cvars.md#allow_download_maps) | `cvar allow_download_maps(булево, "1")` |
| [`allow_download_models`](../38-cvars-reference/04-network-server-cvars.md#allow_download_models) | `cvar allow_download_models(булево, "1")` |
| [`allow_download_pakcontents`](../38-cvars-reference/04-network-server-cvars.md#allow_download_pakcontents) | `cvar allow_download_pakcontents(целое перечисление, "0")` |
| [`allow_download_pakmaps`](../38-cvars-reference/04-network-server-cvars.md#allow_download_pakmaps) | `cvar allow_download_pakmaps(целое перечисление, "0")` |
| [`allow_download_skins`](../38-cvars-reference/04-network-server-cvars.md#allow_download_skins) | `cvar allow_download_skins(булево, "1")` |
| [`allow_download_sounds`](../38-cvars-reference/04-network-server-cvars.md#allow_download_sounds) | `cvar allow_download_sounds(булево, "1")` |
| [`coop`](../38-cvars-reference/04-network-server-cvars.md#coop) | `cvar coop(булево/целое, "")` |
| [`deathmatch`](../38-cvars-reference/04-network-server-cvars.md#deathmatch) | `cvar deathmatch(целое перечисление, "1")` |
| [`filterban`](../38-cvars-reference/04-network-server-cvars.md#filterban) | `cvar filterban(булево, "1")` |
| [`fraglimit`](../38-cvars-reference/04-network-server-cvars.md#fraglimit) | `cvar fraglimit(целое, "")` |
| [`hostname`](../38-cvars-reference/04-network-server-cvars.md#hostname) | `cvar hostname(строка, "unnamed")` |
| [`maxclients`](../38-cvars-reference/04-network-server-cvars.md#maxclients) | `cvar maxclients(целое, "8")` |
| [`maxspectators`](../38-cvars-reference/04-network-server-cvars.md#maxspectators) | `cvar maxspectators(целое, "8")` |
| [`net_compress`](../38-cvars-reference/04-network-server-cvars.md#net_compress) | `cvar net_compress(булево, "0")` |
| [`net_enabled`](../38-cvars-reference/04-network-server-cvars.md#net_enabled) | `cvar net_enabled(булево, "1")` |
| [`net_enable_http`](../38-cvars-reference/04-network-server-cvars.md#net_enable_http) | `cvar net_enable_http(булево, "1")` |
| [`net_enable_qtv`](../38-cvars-reference/04-network-server-cvars.md#net_enable_qtv) | `cvar net_enable_qtv(целое перечисление, "2")` |
| [`net_enable_rtcbroker`](../38-cvars-reference/04-network-server-cvars.md#net_enable_rtcbroker) | `cvar net_enable_rtcbroker(булево, "1")` |
| [`net_enable_tls`](../38-cvars-reference/04-network-server-cvars.md#net_enable_tls) | `cvar net_enable_tls(булево, "1")` |
| [`net_enable_websockets`](../38-cvars-reference/04-network-server-cvars.md#net_enable_websockets) | `cvar net_enable_websockets(булево, "0")` |
| [`net_hybriddualstack`](../38-cvars-reference/04-network-server-cvars.md#net_hybriddualstack) | `cvar net_hybriddualstack(булево, "1")` |
| [`net_ice_allowmdns`](../38-cvars-reference/04-network-server-cvars.md#net_ice_allowmdns) | `cvar net_ice_allowmdns(булево, "1")` |
| [`net_ice_allowstun`](../38-cvars-reference/04-network-server-cvars.md#net_ice_allowstun) | `cvar net_ice_allowstun(булево, "1")` |
| [`net_ice_allowturn`](../38-cvars-reference/04-network-server-cvars.md#net_ice_allowturn) | `cvar net_ice_allowturn(булево, "1")` |
| [`net_ice_broker`](../38-cvars-reference/04-network-server-cvars.md#net_ice_broker) | `cvar net_ice_broker(строка URL, "tls://master.frag-net.com:27950")` |
| [`net_ice_relayonly`](../38-cvars-reference/04-network-server-cvars.md#net_ice_relayonly) | `cvar net_ice_relayonly(булево, "0")` |
| [`net_ice_servers`](../38-cvars-reference/04-network-server-cvars.md#net_ice_servers) | `cvar net_ice_servers(строка списка, "")` |
| [`net_mtu`](../38-cvars-reference/04-network-server-cvars.md#net_mtu) | `cvar net_mtu(целое, "1440")` |
| [`password`](../38-cvars-reference/04-network-server-cvars.md#password) | `cvar password(строка, "")` |
| [`qtv_maxstreams`](../38-cvars-reference/04-network-server-cvars.md#qtv_maxstreams) | `cvar qtv_maxstreams(целое или пусто, "0")` |
| [`rcon_password`](../38-cvars-reference/04-network-server-cvars.md#rcon_password) | `cvar rcon_password(строка, "")` |
| [`spectator_password`](../38-cvars-reference/04-network-server-cvars.md#spectator_password) | `cvar spectator_password(строка, "")` |
| [`sv_banproxies`](../38-cvars-reference/04-network-server-cvars.md#sv_banproxies) | `cvar sv_banproxies(булево, "0")` |
| [`sv_bigcoords`](../38-cvars-reference/04-network-server-cvars.md#sv_bigcoords) | `cvar sv_bigcoords(булево, "1")` |
| [`sv_calcphs`](../38-cvars-reference/04-network-server-cvars.md#sv_calcphs) | `cvar sv_calcphs(целое перечисление, "2")` |
| [`sv_crypt_rcon`](../38-cvars-reference/04-network-server-cvars.md#sv_crypt_rcon) | `cvar sv_crypt_rcon(строка/переключатель, "")` |
| [`sv_cullplayers_trace`](../38-cvars-reference/04-network-server-cvars.md#sv_cullplayers_trace) | `cvar sv_cullplayers_trace(булево/целое, "")` |
| [`sv_demoClearOld`](../38-cvars-reference/04-network-server-cvars.md#sv_democlearold) | `cvar sv_demoClearOld(булево, "0")` |
| [`sv_demoExtensions`](../38-cvars-reference/04-network-server-cvars.md#sv_demoextensions) | `cvar sv_demoExtensions(целое перечисление, "1")` |
| [`sv_demofps`](../38-cvars-reference/04-network-server-cvars.md#sv_demofps) | `cvar sv_demofps(целое, "30")` |
| [`sv_demoMaxDirAge`](../38-cvars-reference/04-network-server-cvars.md#sv_demomaxdirage) | `cvar sv_demoMaxDirAge(время/целое, "0")` |
| [`sv_demoMaxDirCount`](../38-cvars-reference/04-network-server-cvars.md#sv_demomaxdircount) | `cvar sv_demoMaxDirCount(целое, "500")` |
| [`sv_demoMaxDirSize`](../38-cvars-reference/04-network-server-cvars.md#sv_demomaxdirsize) | `cvar sv_demoMaxDirSize(размер/строка, "100mb")` |
| [`sv_demoMaxSize`](../38-cvars-reference/04-network-server-cvars.md#sv_demomaxsize) | `cvar sv_demoMaxSize(размер/строка, "")` |
| [`sv_demoUseCache`](../38-cvars-reference/04-network-server-cvars.md#sv_demousecache) | `cvar sv_demoUseCache(булево/целое, "")` |
| [`sv_demo_write_csqc`](../38-cvars-reference/04-network-server-cvars.md#sv_demo_write_csqc) | `cvar sv_demo_write_csqc(булево/целое, "")` |
| [`sv_guidkey`](../38-cvars-reference/04-network-server-cvars.md#sv_guidkey) | `cvar sv_guidkey(строка, "")` |
| [`sv_heartbeat_checks`](../38-cvars-reference/04-network-server-cvars.md#sv_heartbeat_checks) | `cvar sv_heartbeat_checks(булево, "1")` |
| [`sv_heartbeat_interval`](../38-cvars-reference/04-network-server-cvars.md#sv_heartbeat_interval) | `cvar sv_heartbeat_interval(целое, "110")` |
| [`sv_heartbeattimeout`](../38-cvars-reference/04-network-server-cvars.md#sv_heartbeattimeout) | `cvar sv_heartbeattimeout(целое, "300")` |
| [`sv_limittics`](../38-cvars-reference/04-network-server-cvars.md#sv_limittics) | `cvar sv_limittics(целое, "3")` |
| [`sv_listen_dp`](../38-cvars-reference/04-network-server-cvars.md#sv_listen_dp) | `cvar sv_listen_dp(булево, "0")` |
| [`sv_listen_nq`](../38-cvars-reference/04-network-server-cvars.md#sv_listen_nq) | `cvar sv_listen_nq(целое перечисление, "0")` |
| [`sv_maxdrate`](../38-cvars-reference/04-network-server-cvars.md#sv_maxdrate) | `cvar sv_maxdrate(скорость/целое, "500000")` |
| [`sv_maxgames`](../38-cvars-reference/04-network-server-cvars.md#sv_maxgames) | `cvar sv_maxgames(целое, "100")` |
| [`sv_maxrate`](../38-cvars-reference/04-network-server-cvars.md#sv_maxrate) | `cvar sv_maxrate(скорость/целое, "50000")` |
| [`sv_maxservers`](../38-cvars-reference/04-network-server-cvars.md#sv_maxservers) | `cvar sv_maxservers(целое, "10000")` |
| [`sv_maxtic`](../38-cvars-reference/04-network-server-cvars.md#sv_maxtic) | `cvar sv_maxtic(дробное, "0.1")` |
| [`sv_minping`](../38-cvars-reference/04-network-server-cvars.md#sv_minping) | `cvar sv_minping(целое, "")` |
| [`sv_mintic`](../38-cvars-reference/04-network-server-cvars.md#sv_mintic) | `cvar sv_mintic(дробное, "0.013")` |
| [`sv_nailhack`](../38-cvars-reference/04-network-server-cvars.md#sv_nailhack) | `cvar sv_nailhack(булево, "1")` |
| [`sv_playerslots`](../38-cvars-reference/04-network-server-cvars.md#sv_playerslots) | `cvar sv_playerslots(целое или пусто, "")` |
| [`sv_protocol`](../38-cvars-reference/04-network-server-cvars.md#sv_protocol) | `cvar sv_protocol(строка списка, "")` |
| [`sv_public`](../38-cvars-reference/04-network-server-cvars.md#sv_public) | `cvar sv_public(целое перечисление, "0")` |
| [`sv_rconlim`](../38-cvars-reference/04-network-server-cvars.md#sv_rconlim) | `cvar sv_rconlim(целое, "4")` |
| [`sv_reconnectlimit`](../38-cvars-reference/04-network-server-cvars.md#sv_reconnectlimit) | `cvar sv_reconnectlimit(целое, "0")` |
| [`sv_reliable_sound`](../38-cvars-reference/04-network-server-cvars.md#sv_reliable_sound) | `cvar sv_reliable_sound(булево, "0")` |
| [`sv_reportheartbeats`](../38-cvars-reference/04-network-server-cvars.md#sv_reportheartbeats) | `cvar sv_reportheartbeats(целое перечисление, "2")` |
| [`sv_serverip`](../38-cvars-reference/04-network-server-cvars.md#sv_serverip) | `cvar sv_serverip(строка адреса, "")` |
| [`sv_slaverequery`](../38-cvars-reference/04-network-server-cvars.md#sv_slaverequery) | `cvar sv_slaverequery(целое, "120")` |
| [`sv_timestamplen`](../38-cvars-reference/04-network-server-cvars.md#sv_timestamplen) | `cvar sv_timestamplen(целое, "60")` |
| [`sv_use_dns`](../38-cvars-reference/04-network-server-cvars.md#sv_use_dns) | `cvar sv_use_dns(булево/строка, "")` |
| [`teamplay`](../38-cvars-reference/04-network-server-cvars.md#teamplay) | `cvar teamplay(целое, "")` |
| [`timelimit`](../38-cvars-reference/04-network-server-cvars.md#timelimit) | `cvar timelimit(целое, "")` |
| [`timeout`](../38-cvars-reference/04-network-server-cvars.md#timeout) | `cvar timeout(целое, "65")` |
| [`zombietime`](../38-cvars-reference/04-network-server-cvars.md#zombietime) | `cvar zombietime(целое, "2")` |
| [`allow_download`](../38-cvars-reference/04-network-server-cvars.md#allow_download) | `cvar allow_download(int, "1")` |
| [`allow_download_locs`](../38-cvars-reference/04-network-server-cvars.md#allow_download_locs) | `cvar allow_download_locs(int, "1")` |
| [`allow_download_other`](../38-cvars-reference/04-network-server-cvars.md#allow_download_other) | `cvar allow_download_other(int, "0")` |
| [`allow_download_packages`](../38-cvars-reference/04-network-server-cvars.md#allow_download_packages) | `cvar allow_download_packages(int, "1")` |
| [`allow_download_particles`](../38-cvars-reference/04-network-server-cvars.md#allow_download_particles) | `cvar allow_download_particles(int, "1")` |
| [`allow_download_refpackages`](../38-cvars-reference/04-network-server-cvars.md#allow_download_refpackages) | `cvar allow_download_refpackages(int, "1")` |
| [`allow_download_root`](../38-cvars-reference/04-network-server-cvars.md#allow_download_root) | `cvar allow_download_root(int, "0")` |
| [`allow_download_textures`](../38-cvars-reference/04-network-server-cvars.md#allow_download_textures) | `cvar allow_download_textures(int, "1")` |
| [`allow_download_wads`](../38-cvars-reference/04-network-server-cvars.md#allow_download_wads) | `cvar allow_download_wads(int, "1")` |
| [`capturerate`](../38-cvars-reference/04-network-server-cvars.md#capturerate) | `cvar capturerate(int, "30")` |
| [`cl_download_csprogs`](../38-cvars-reference/04-network-server-cvars.md#cl_download_csprogs) | `cvar cl_download_csprogs(int, "1")` |
| [`cl_download_mapsrc`](../38-cvars-reference/04-network-server-cvars.md#cl_download_mapsrc) | `cvar cl_download_mapsrc(string, "")` |
| [`cl_download_packages`](../38-cvars-reference/04-network-server-cvars.md#cl_download_packages) | `cvar cl_download_packages(int, "1")` |
| [`cl_download_redirection`](../38-cvars-reference/04-network-server-cvars.md#cl_download_redirection) | `cvar cl_download_redirection(int, "2")` |
| [`cl_download_wait`](../38-cvars-reference/04-network-server-cvars.md#cl_download_wait) | `cvar cl_download_wait(int, "1")` |
| [`cl_downloads`](../38-cvars-reference/04-network-server-cvars.md#cl_downloads) | `cvar cl_downloads(int, "1")` |
| [`com_fullgamename`](../38-cvars-reference/04-network-server-cvars.md#com_fullgamename) | `cvar com_fullgamename(string, "fs_gamename")` |
| [`com_gamedirnativecode`](../38-cvars-reference/04-network-server-cvars.md#com_gamedirnativecode) | `cvar com_gamedirnativecode(int, "0")` |
| [`com_highlightcolor`](../38-cvars-reference/04-network-server-cvars.md#com_highlightcolor) | `cvar com_highlightcolor(string, "ANSI colour to be used for highlighted text, used when com_parseutf8 is active.")` |
| [`com_parseutf8`](../38-cvars-reference/04-network-server-cvars.md#com_parseutf8) | `cvar com_parseutf8(int, "1")` |
| [`com_protocolname`](../38-cvars-reference/04-network-server-cvars.md#com_protocolname) | `cvar com_protocolname(string, "com_gamename")` |
| [`com_protocolversion`](../38-cvars-reference/04-network-server-cvars.md#com_protocolversion) | `cvar com_protocolversion(int, "3")` |
| [`drate`](../38-cvars-reference/04-network-server-cvars.md#drate) | `cvar drate(int, "3000000")` |
| [`fraglog_public`](../38-cvars-reference/04-network-server-cvars.md#fraglog_public) | `cvar fraglog_public(int, "1")` |
| [`gl_blacklist_generatemipmap`](../38-cvars-reference/04-network-server-cvars.md#gl_blacklist_generatemipmap) | `cvar gl_blacklist_generatemipmap(int, "1")` |
| [`host_speeds`](../38-cvars-reference/04-network-server-cvars.md#host_speeds) | `cvar host_speeds(int, "0")` |
| [`net_enable_`](../38-cvars-reference/04-network-server-cvars.md#net_enable_) | `cvar net_enable_(int, "0")` |
| [`net_enable_dtls`](../38-cvars-reference/04-network-server-cvars.md#net_enable_dtls) | `cvar net_enable_dtls(string, "")` |
| [`net_enable_qizmo`](../38-cvars-reference/04-network-server-cvars.md#net_enable_qizmo) | `cvar net_enable_qizmo(int, "1")` |
| [`net_fakeloss`](../38-cvars-reference/04-network-server-cvars.md#net_fakeloss) | `cvar net_fakeloss(int, "0")` |
| [`net_fakemtu`](../38-cvars-reference/04-network-server-cvars.md#net_fakemtu) | `cvar net_fakemtu(int, "0")` |
| [`net_ice_debug`](../38-cvars-reference/04-network-server-cvars.md#net_ice_debug) | `cvar net_ice_debug(int, "0")` |
| [`net_ice_exchangeprivateips`](../38-cvars-reference/04-network-server-cvars.md#net_ice_exchangeprivateips) | `cvar net_ice_exchangeprivateips(int, "0")` |
| [`net_ice_usewebrtc`](../38-cvars-reference/04-network-server-cvars.md#net_ice_usewebrtc) | `cvar net_ice_usewebrtc(string, "")` |
| [`net_upnpigp`](../38-cvars-reference/04-network-server-cvars.md#net_upnpigp) | `cvar net_upnpigp(int, "0")` |
| [`pausable`](../38-cvars-reference/04-network-server-cvars.md#pausable) | `cvar pausable(string, "")` |
| [`qtv_password`](../38-cvars-reference/04-network-server-cvars.md#qtv_password) | `cvar qtv_password(string, "")` |
| [`qtv_streamport`](../38-cvars-reference/04-network-server-cvars.md#qtv_streamport) | `cvar qtv_streamport(string, "")` |
| [`qtvcl_eztvextensions`](../38-cvars-reference/04-network-server-cvars.md#qtvcl_eztvextensions) | `cvar qtvcl_eztvextensions(int, "1")` |
| [`qtvcl_forceversion1`](../38-cvars-reference/04-network-server-cvars.md#qtvcl_forceversion1) | `cvar qtvcl_forceversion1(int, "0")` |
| [`r_image_downloadsizelimit`](../38-cvars-reference/04-network-server-cvars.md#r_image_downloadsizelimit) | `cvar r_image_downloadsizelimit(int, "131072")` |
| [`rate`](../38-cvars-reference/04-network-server-cvars.md#rate) | `cvar rate(int, "30000")` |
| [`samelevel`](../38-cvars-reference/04-network-server-cvars.md#samelevel) | `cvar samelevel(string, "")` |
| [`sb_showfraglimit`](../38-cvars-reference/04-network-server-cvars.md#sb_showfraglimit) | `cvar sb_showfraglimit(int, "0")` |
| [`sb_showtimelimit`](../38-cvars-reference/04-network-server-cvars.md#sb_showtimelimit) | `cvar sb_showtimelimit(int, "0")` |
| [`skill`](../38-cvars-reference/04-network-server-cvars.md#skill) | `cvar skill(string, "")` |
| [`spawn`](../38-cvars-reference/04-network-server-cvars.md#spawn) | `cvar spawn(string, "")` |
| [`sv_aim`](../38-cvars-reference/04-network-server-cvars.md#sv_aim) | `cvar sv_aim(int, "2")` |
| [`sv_autooffload`](../38-cvars-reference/04-network-server-cvars.md#sv_autooffload) | `cvar sv_autooffload(int, "0")` |
| [`sv_autosave`](../38-cvars-reference/04-network-server-cvars.md#sv_autosave) | `cvar sv_autosave(int, "5")` |
| [`sv_chatfilter`](../38-cvars-reference/04-network-server-cvars.md#sv_chatfilter) | `cvar sv_chatfilter(int, "0")` |
| [`sv_cheatpc`](../38-cvars-reference/04-network-server-cvars.md#sv_cheatpc) | `cvar sv_cheatpc(int, "125")` |
| [`sv_cheats`](../38-cvars-reference/04-network-server-cvars.md#sv_cheats) | `cvar sv_cheats(int, "0")` |
| [`sv_cheatspeedchecktime`](../38-cvars-reference/04-network-server-cvars.md#sv_cheatspeedchecktime) | `cvar sv_cheatspeedchecktime(int, "30")` |
| [`sv_cmdlikercon`](../38-cvars-reference/04-network-server-cvars.md#sv_cmdlikercon) | `cvar sv_cmdlikercon(int, "0")` |
| [`sv_compatiblehulls`](../38-cvars-reference/04-network-server-cvars.md#sv_compatiblehulls) | `cvar sv_compatiblehulls(int, "1")` |
| [`sv_csqc_progname`](../38-cvars-reference/04-network-server-cvars.md#sv_csqc_progname) | `cvar sv_csqc_progname(string, "csprogs.dat")` |
| [`sv_csqcdebug`](../38-cvars-reference/04-network-server-cvars.md#sv_csqcdebug) | `cvar sv_csqcdebug(int, "0")` |
| [`sv_demoAutoCompress`](../38-cvars-reference/04-network-server-cvars.md#sv_demoautocompress) | `cvar sv_demoAutoCompress(string, "")` |
| [`sv_demoAutoPrefix`](../38-cvars-reference/04-network-server-cvars.md#sv_demoautoprefix) | `cvar sv_demoAutoPrefix(string, "auto_")` |
| [`sv_demoAutoRecord`](../38-cvars-reference/04-network-server-cvars.md#sv_demoautorecord) | `cvar sv_demoAutoRecord(int, "0")` |
| [`sv_demoCacheSize`](../38-cvars-reference/04-network-server-cvars.md#sv_democachesize) | `cvar sv_demoCacheSize(string, "0x80000")` |
| [`sv_demoDir`](../38-cvars-reference/04-network-server-cvars.md#sv_demodir) | `cvar sv_demoDir(string, "demos")` |
| [`sv_demoDirAlt`](../38-cvars-reference/04-network-server-cvars.md#sv_demodiralt) | `cvar sv_demoDirAlt(string, "")` |
| [`sv_demoExtraNames`](../38-cvars-reference/04-network-server-cvars.md#sv_demoextranames) | `cvar sv_demoExtraNames(string, "")` |
| [`sv_demoPings`](../38-cvars-reference/04-network-server-cvars.md#sv_demopings) | `cvar sv_demoPings(int, "10")` |
| [`sv_demoPrefix`](../38-cvars-reference/04-network-server-cvars.md#sv_demoprefix) | `cvar sv_demoPrefix(string, "")` |
| [`sv_demoSuffix`](../38-cvars-reference/04-network-server-cvars.md#sv_demosuffix) | `cvar sv_demoSuffix(string, "")` |
| [`sv_demotxt`](../38-cvars-reference/04-network-server-cvars.md#sv_demotxt) | `cvar sv_demotxt(int, "1")` |
| [`sv_dlURL`](../38-cvars-reference/04-network-server-cvars.md#sv_dlurl) | `cvar sv_dlURL(string, "")` |
| [`sv_floodprotect`](../38-cvars-reference/04-network-server-cvars.md#sv_floodprotect) | `cvar sv_floodprotect(int, "1")` |
| [`sv_floodprotect_interval`](../38-cvars-reference/04-network-server-cvars.md#sv_floodprotect_interval) | `cvar sv_floodprotect_interval(int, "4")` |
| [`sv_floodprotect_messages`](../38-cvars-reference/04-network-server-cvars.md#sv_floodprotect_messages) | `cvar sv_floodprotect_messages(int, "4")` |
| [`sv_floodprotect_sendmessage`](../38-cvars-reference/04-network-server-cvars.md#sv_floodprotect_sendmessage) | `cvar sv_floodprotect_sendmessage(string, "")` |
| [`sv_floodprotect_silencetime`](../38-cvars-reference/04-network-server-cvars.md#sv_floodprotect_silencetime) | `cvar sv_floodprotect_silencetime(int, "10")` |
| [`sv_floodprotect_suicide`](../38-cvars-reference/04-network-server-cvars.md#sv_floodprotect_suicide) | `cvar sv_floodprotect_suicide(int, "1")` |
| [`sv_ftp`](../38-cvars-reference/04-network-server-cvars.md#sv_ftp) | `cvar sv_ftp(int, "0")` |
| [`sv_ftp_port`](../38-cvars-reference/04-network-server-cvars.md#sv_ftp_port) | `cvar sv_ftp_port(int, "21")` |
| [`sv_ftp_port_range`](../38-cvars-reference/04-network-server-cvars.md#sv_ftp_port_range) | `cvar sv_ftp_port_range(int, "0")` |
| [`sv_fulllevel`](../38-cvars-reference/04-network-server-cvars.md#sv_fulllevel) | `cvar sv_fulllevel(int, "51")` |
| [`sv_fullredirect`](../38-cvars-reference/04-network-server-cvars.md#sv_fullredirect) | `cvar sv_fullredirect(string, "")` |
| [`sv_gameplayfix_honest_tracelines`](../38-cvars-reference/04-network-server-cvars.md#sv_gameplayfix_honest_tracelines) | `cvar sv_gameplayfix_honest_tracelines(int, "1")` |
| [`sv_gameplayfix_radialmaxvelocity`](../38-cvars-reference/04-network-server-cvars.md#sv_gameplayfix_radialmaxvelocity) | `cvar sv_gameplayfix_radialmaxvelocity(int, "0")` |
| [`sv_gameplayfix_setmodelrealbox`](../38-cvars-reference/04-network-server-cvars.md#sv_gameplayfix_setmodelrealbox) | `cvar sv_gameplayfix_setmodelrealbox(int, "1")` |
| [`sv_gameplayfix_setmodelsize_qw`](../38-cvars-reference/04-network-server-cvars.md#sv_gameplayfix_setmodelsize_qw) | `cvar sv_gameplayfix_setmodelsize_qw(int, "0")` |
| [`sv_gameplayfix_spawnbeforethinks`](../38-cvars-reference/04-network-server-cvars.md#sv_gameplayfix_spawnbeforethinks) | `cvar sv_gameplayfix_spawnbeforethinks(int, "0")` |
| [`sv_gameplayfix_stepdown`](../38-cvars-reference/04-network-server-cvars.md#sv_gameplayfix_stepdown) | `cvar sv_gameplayfix_stepdown(int, "0")` |
| [`sv_gamespeed`](../38-cvars-reference/04-network-server-cvars.md#sv_gamespeed) | `cvar sv_gamespeed(int, "1")` |
| [`sv_getrealip`](../38-cvars-reference/04-network-server-cvars.md#sv_getrealip) | `cvar sv_getrealip(int, "0")` |
| [`sv_hideinactivegames`](../38-cvars-reference/04-network-server-cvars.md#sv_hideinactivegames) | `cvar sv_hideinactivegames(int, "1")` |
| [`sv_highchars`](../38-cvars-reference/04-network-server-cvars.md#sv_highchars) | `cvar sv_highchars(int, "1")` |
| [`sv_http`](../38-cvars-reference/04-network-server-cvars.md#sv_http) | `cvar sv_http(int, "0")` |
| [`sv_http_port`](../38-cvars-reference/04-network-server-cvars.md#sv_http_port) | `cvar sv_http_port(int, "80")` |
| [`sv_listen_q3`](../38-cvars-reference/04-network-server-cvars.md#sv_listen_q3) | `cvar sv_listen_q3(int, "0")` |
| [`sv_listen_qw`](../38-cvars-reference/04-network-server-cvars.md#sv_listen_qw) | `cvar sv_listen_qw(int, "1")` |
| [`sv_loadentfiles`](../38-cvars-reference/04-network-server-cvars.md#sv_loadentfiles) | `cvar sv_loadentfiles(int, "1")` |
| [`sv_mapcheck`](../38-cvars-reference/04-network-server-cvars.md#sv_mapcheck) | `cvar sv_mapcheck(int, "1")` |
| [`sv_master`](../38-cvars-reference/04-network-server-cvars.md#sv_master) | `cvar sv_master(int, "0")` |
| [`sv_masterport`](../38-cvars-reference/04-network-server-cvars.md#sv_masterport) | `cvar sv_masterport(string, " ")` |
| [`sv_masterport_tcp`](../38-cvars-reference/04-network-server-cvars.md#sv_masterport_tcp) | `cvar sv_masterport_tcp(string, "")` |
| [`sv_maxaim`](../38-cvars-reference/04-network-server-cvars.md#sv_maxaim) | `cvar sv_maxaim(int, "22")` |
| [`sv_nopvs`](../38-cvars-reference/04-network-server-cvars.md#sv_nopvs) | `cvar sv_nopvs(int, "0")` |
| [`sv_phs`](../38-cvars-reference/04-network-server-cvars.md#sv_phs) | `cvar sv_phs(int, "1")` |
| [`sv_ping_ignorepl`](../38-cvars-reference/04-network-server-cvars.md#sv_ping_ignorepl) | `cvar sv_ping_ignorepl(int, "0")` |
| [`sv_playermodelchecks`](../38-cvars-reference/04-network-server-cvars.md#sv_playermodelchecks) | `cvar sv_playermodelchecks(int, "0")` |
| [`sv_port`](../38-cvars-reference/04-network-server-cvars.md#sv_port) | `cvar sv_port(string, "Port number to list on for inbound udp-based connections (including dtls variants). Can be a list for multiple ports. If ips are included then binds to that specific interface. Whether any specific protocol is accepted depends upon other settings.")` |
| [`sv_port_ipv6`](../38-cvars-reference/04-network-server-cvars.md#sv_port_ipv6) | `cvar sv_port_ipv6(string, "")` |
| [`sv_port_ipx`](../38-cvars-reference/04-network-server-cvars.md#sv_port_ipx) | `cvar sv_port_ipx(string, "")` |
| [`sv_port_natpmp`](../38-cvars-reference/04-network-server-cvars.md#sv_port_natpmp) | `cvar sv_port_natpmp(string, "If set (typically to 5351), automatically configures your router's port forwarding. You can instead specify the full ip address of your router (192.168.1.1:5351 for example). Your router must have NAT-PMP supported and enabled.")` |
| [`sv_port_rtc`](../38-cvars-reference/04-network-server-cvars.md#sv_port_rtc) | `cvar sv_port_rtc(string, "/")` |
| [`sv_port_tcp`](../38-cvars-reference/04-network-server-cvars.md#sv_port_tcp) | `cvar sv_port_tcp(string, "")` |
| [`sv_port_tcp6`](../38-cvars-reference/04-network-server-cvars.md#sv_port_tcp6) | `cvar sv_port_tcp6(string, "")` |
| [`sv_port_unix`](../38-cvars-reference/04-network-server-cvars.md#sv_port_unix) | `cvar sv_port_unix(string, "@qsock.fte")` |
| [`sv_progs`](../38-cvars-reference/04-network-server-cvars.md#sv_progs) | `cvar sv_progs(string, "")` |
| [`sv_protocol_nq`](../38-cvars-reference/04-network-server-cvars.md#sv_protocol_nq) | `cvar sv_protocol_nq(string, "")` |
| [`sv_pupglow`](../38-cvars-reference/04-network-server-cvars.md#sv_pupglow) | `cvar sv_pupglow(string, "")` |
| [`sv_pure`](../38-cvars-reference/04-network-server-cvars.md#sv_pure) | `cvar sv_pure(string, "")` |
| [`sv_readlevel`](../38-cvars-reference/04-network-server-cvars.md#sv_readlevel) | `cvar sv_readlevel(int, "0")` |
| [`sv_realip_kick`](../38-cvars-reference/04-network-server-cvars.md#sv_realip_kick) | `cvar sv_realip_kick(int, "0")` |
| [`sv_realip_timeout`](../38-cvars-reference/04-network-server-cvars.md#sv_realip_timeout) | `cvar sv_realip_timeout(int, "10")` |
| [`sv_realiphostname_ipv4`](../38-cvars-reference/04-network-server-cvars.md#sv_realiphostname_ipv4) | `cvar sv_realiphostname_ipv4(string, "")` |
| [`sv_realiphostname_ipv6`](../38-cvars-reference/04-network-server-cvars.md#sv_realiphostname_ipv6) | `cvar sv_realiphostname_ipv6(string, "")` |
| [`sv_resetparms`](../38-cvars-reference/04-network-server-cvars.md#sv_resetparms) | `cvar sv_resetparms(int, "0")` |
| [`sv_savefmt`](../38-cvars-reference/04-network-server-cvars.md#sv_savefmt) | `cvar sv_savefmt(string, "")` |
| [`sv_showconnectionlessmessages`](../38-cvars-reference/04-network-server-cvars.md#sv_showconnectionlessmessages) | `cvar sv_showconnectionlessmessages(int, "0")` |
| [`sv_showpredloss`](../38-cvars-reference/04-network-server-cvars.md#sv_showpredloss) | `cvar sv_showpredloss(int, "0")` |
| [`sv_sortlist`](../38-cvars-reference/04-network-server-cvars.md#sv_sortlist) | `cvar sv_sortlist(int, "3")` |
| [`sv_specprint`](../38-cvars-reference/04-network-server-cvars.md#sv_specprint) | `cvar sv_specprint(int, "3")` |
| [`sv_spectalk`](../38-cvars-reference/04-network-server-cvars.md#sv_spectalk) | `cvar sv_spectalk(int, "1")` |
| [`sv_sql_defaultdb`](../38-cvars-reference/04-network-server-cvars.md#sv_sql_defaultdb) | `cvar sv_sql_defaultdb(string, "")` |
| [`sv_sql_driver`](../38-cvars-reference/04-network-server-cvars.md#sv_sql_driver) | `cvar sv_sql_driver(string, "")` |
| [`sv_sql_host`](../38-cvars-reference/04-network-server-cvars.md#sv_sql_host) | `cvar sv_sql_host(string, "127.0.0.1")` |
| [`sv_sql_password`](../38-cvars-reference/04-network-server-cvars.md#sv_sql_password) | `cvar sv_sql_password(string, "")` |
| [`sv_sql_username`](../38-cvars-reference/04-network-server-cvars.md#sv_sql_username) | `cvar sv_sql_username(string, "")` |
| [`sv_userinfo_bytelimit`](../38-cvars-reference/04-network-server-cvars.md#sv_userinfo_bytelimit) | `cvar sv_userinfo_bytelimit(int, "8192")` |
| [`sv_userinfo_keylimit`](../38-cvars-reference/04-network-server-cvars.md#sv_userinfo_keylimit) | `cvar sv_userinfo_keylimit(int, "128")` |
| [`sv_writelevel`](../38-cvars-reference/04-network-server-cvars.md#sv_writelevel) | `cvar sv_writelevel(int, "35")` |
| [`vK_khr_fragment_shading_rate`](../38-cvars-reference/04-network-server-cvars.md#vk_khr_fragment_shading_rate) | `cvar vK_khr_fragment_shading_rate(string, "")` |

### Физика и игровой процесс

| Элемент | Сигнатура / Описание |
|---|---|
| [`cl_anglespeedkey`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_anglespeedkey) | `cvar cl_anglespeedkey(float, "1.5")` |
| [`cl_backspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_backspeed) | `cvar cl_backspeed(float/string, "")` |
| [`cl_fastaccel`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_fastaccel) | `cvar cl_fastaccel(boolean/int, "1")` |
| [`cl_forwardspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_forwardspeed) | `cvar cl_forwardspeed(float, "400")` |
| [`cl_iDrive`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_idrive) | `cvar cl_iDrive(boolean/int, "1")` |
| [`cl_instantrotate`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_instantrotate) | `cvar cl_instantrotate(boolean/int, "1")` |
| [`cl_lerp_driftbias`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_lerp_driftbias) | `cvar cl_lerp_driftbias(float, "0")` |
| [`cl_lerp_driftfrac`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_lerp_driftfrac) | `cvar cl_lerp_driftfrac(float, "0")` |
| [`cl_lerp_smooth`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_lerp_smooth) | `cvar cl_lerp_smooth(int, "2")` |
| [`cl_movement`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_movement) | `cvar cl_movement(boolean/int, "1")` |
| [`cl_nopred`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_nopred) | `cvar cl_nopred(boolean/int, "0")` |
| [`cl_pitchspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_pitchspeed) | `cvar cl_pitchspeed(float, "150")` |
| [`cl_predict_extrapolate`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_predict_extrapolate) | `cvar cl_predict_extrapolate(int/string, "")` |
| [`cl_predict_timenudge`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_predict_timenudge) | `cvar cl_predict_timenudge(float, "0")` |
| [`cl_rollangle`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_rollangle) | `cvar cl_rollangle(float, "2.0")` |
| [`cl_rollspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_rollspeed) | `cvar cl_rollspeed(float, "200")` |
| [`cl_run`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_run) | `cvar cl_run(boolean/int, "0")` |
| [`cl_sidespeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_sidespeed) | `cvar cl_sidespeed(float, "400")` |
| [`cl_smartjump`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_smartjump) | `cvar cl_smartjump(boolean/int, "1")` |
| [`cl_upspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_upspeed) | `cvar cl_upspeed(float, "400")` |
| [`cl_yawspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_yawspeed) | `cvar cl_yawspeed(float, "140")` |
| [`pm_airstep`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_airstep) | `cvar pm_airstep(boolean/int/string, "")` |
| [`pm_autobunny`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_autobunny) | `cvar pm_autobunny(boolean/int/string, "")` |
| [`pm_bunnyfriction`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_bunnyfriction) | `cvar pm_bunnyfriction(boolean/int/string, "")` |
| [`pm_bunnyspeedcap`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_bunnyspeedcap) | `cvar pm_bunnyspeedcap(float/string, "")` |
| [`pm_edgefriction`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_edgefriction) | `cvar pm_edgefriction(float/string, "")` |
| [`pm_flyfriction`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_flyfriction) | `cvar pm_flyfriction(float/string, "")` |
| [`pm_ktjump`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_ktjump) | `cvar pm_ktjump(float/string, "")` |
| [`pm_pground`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_pground) | `cvar pm_pground(boolean/int/string, "")` |
| [`pm_slidefix`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_slidefix) | `cvar pm_slidefix(boolean/int/string, "")` |
| [`pm_slidyslopes`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_slidyslopes) | `cvar pm_slidyslopes(boolean/int/string, "")` |
| [`pm_stepdown`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_stepdown) | `cvar pm_stepdown(boolean/int/string, "")` |
| [`pm_walljump`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_walljump) | `cvar pm_walljump(int/string, "")` |
| [`pm_watersinkspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_watersinkspeed) | `cvar pm_watersinkspeed(float/string, "")` |
| [`pushlatency`](../38-cvars-reference/05-physics-gameplay-cvars.md#pushlatency) | `cvar pushlatency(float, "-999")` |
| [`sv_accelerate`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_accelerate) | `cvar sv_accelerate(float, "10")` |
| [`sv_airaccelerate`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_airaccelerate) | `cvar sv_airaccelerate(float, "0.7")` |
| [`sv_antilag`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_antilag) | `cvar sv_antilag(int/string, "")` |
| [`sv_antilag_frac`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_antilag_frac) | `cvar sv_antilag_frac(float/string, "")` |
| [`sv_brokenmovetypes`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_brokenmovetypes) | `cvar sv_brokenmovetypes(boolean/int, "0")` |
| [`sv_friction`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_friction) | `cvar sv_friction(float, "4")` |
| [`sv_gameplayfix_blowupfallenzombies`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_blowupfallenzombies) | `cvar sv_gameplayfix_blowupfallenzombies(boolean/int, "0")` |
| [`sv_gameplayfix_droptofloorstartsolid`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_droptofloorstartsolid) | `cvar sv_gameplayfix_droptofloorstartsolid(boolean/int, "0")` |
| [`sv_gameplayfix_findradiusdistancetobox`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_findradiusdistancetobox) | `cvar sv_gameplayfix_findradiusdistancetobox(boolean/int, "0")` |
| [`sv_gameplayfix_grenadebouncedownslopes`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_grenadebouncedownslopes) | `cvar sv_gameplayfix_grenadebouncedownslopes(boolean/int, "0")` |
| [`sv_gameplayfix_multiplethinks`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_multiplethinks) | `cvar sv_gameplayfix_multiplethinks(boolean/int, "1")` |
| [`sv_gameplayfix_noairborncorpse`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_noairborncorpse) | `cvar sv_gameplayfix_noairborncorpse(boolean/int, "0")` |
| [`sv_gameplayfix_nolinknonsolid`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_nolinknonsolid) | `cvar sv_gameplayfix_nolinknonsolid(boolean/int, "1")` |
| [`sv_gameplayfix_trappedwithin`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_trappedwithin) | `cvar sv_gameplayfix_trappedwithin(boolean/int, "0")` |
| [`sv_gravity`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gravity) | `cvar sv_gravity(float, "800")` |
| [`sv_maxspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_maxspeed) | `cvar sv_maxspeed(float, "320")` |
| [`sv_maxvelocity`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_maxvelocity) | `cvar sv_maxvelocity(float, "10000")` |
| [`sv_nqplayerphysics`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_nqplayerphysics) | `cvar sv_nqplayerphysics(string/int, "auto")` |
| [`sv_pushplayers`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_pushplayers) | `cvar sv_pushplayers(float, "0")` |
| [`sv_spectatormaxspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_spectatormaxspeed) | `cvar sv_spectatormaxspeed(float, "500")` |
| [`sv_stepheight`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_stepheight) | `cvar sv_stepheight(float/string, "")` |
| [`sv_stopspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_stopspeed) | `cvar sv_stopspeed(float, "100")` |
| [`sv_wallfriction`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_wallfriction) | `cvar sv_wallfriction(float, "1")` |
| [`sv_wateraccelerate`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_wateraccelerate) | `cvar sv_wateraccelerate(float, "10")` |
| [`sv_waterfriction`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_waterfriction) | `cvar sv_waterfriction(float, "4")` |
| [`chase_active`](../38-cvars-reference/05-physics-gameplay-cvars.md#chase_active) | `cvar chase_active(int, "0")` |
| [`chase_back`](../38-cvars-reference/05-physics-gameplay-cvars.md#chase_back) | `cvar chase_back(int, "48")` |
| [`chase_right`](../38-cvars-reference/05-physics-gameplay-cvars.md#chase_right) | `cvar chase_right(int, "0")` |
| [`chase_up`](../38-cvars-reference/05-physics-gameplay-cvars.md#chase_up) | `cvar chase_up(int, "24")` |
| [`cl_bob`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_bob) | `cvar cl_bob(float, "0.02")` |
| [`cl_bobcycle`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_bobcycle) | `cvar cl_bobcycle(float, "0.6")` |
| [`cl_bobmodel`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_bobmodel) | `cvar cl_bobmodel(int, "0")` |
| [`cl_bobmodel_side`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_bobmodel_side) | `cvar cl_bobmodel_side(float, "0.15")` |
| [`cl_bobmodel_speed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_bobmodel_speed) | `cvar cl_bobmodel_speed(int, "7")` |
| [`cl_bobmodel_up`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_bobmodel_up) | `cvar cl_bobmodel_up(float, "0.06")` |
| [`cl_bobup`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_bobup) | `cvar cl_bobup(float, "0.5")` |
| [`cl_predict_players`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_predict_players) | `cvar cl_predict_players(int, "1")` |
| [`pm_noround`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_noround) | `cvar pm_noround(int, "0")` |
| [`pm_stepheight`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_stepheight) | `cvar pm_stepheight(string, "")` |
| [`temp1`](../38-cvars-reference/05-physics-gameplay-cvars.md#temp1) | `cvar temp1(int, "0")` |

### Интерфейс, консоль и управление

| Элемент | Сигнатура / Описание |
|---|---|
| [`cl_anglespeedkey`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_anglespeedkey) | `cvar cl_anglespeedkey(дробное, "1.5")` |
| [`cl_backspeed`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_backspeed) | `cvar cl_backspeed(дробное/пустая строка, "")` |
| [`cl_chatmode`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_chatmode) | `cvar cl_chatmode(целое 0-2, "2")` |
| [`cl_clock`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_clock) | `cvar cl_clock(целое 0-2, "0")` |
| [`cl_fastaccel`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_fastaccel) | `cvar cl_fastaccel(логическое, "1")` |
| [`cl_forwardspeed`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_forwardspeed) | `cvar cl_forwardspeed(дробное, "400")` |
| [`cl_gameclock`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_gameclock) | `cvar cl_gameclock(целое 0-2, "0")` |
| [`cl_iDrive`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_idrive) | `cvar cl_iDrive(логическое, "1")` |
| [`cl_instantrotate`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_instantrotate) | `cvar cl_instantrotate(логическое, "1")` |
| [`cl_keypad`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_keypad) | `cvar cl_keypad(логическое, "1")` |
| [`cl_movespeedkey`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_movespeedkey) | `cvar cl_movespeedkey(дробное, "2.0")` |
| [`cl_pitchspeed`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_pitchspeed) | `cvar cl_pitchspeed(дробное, "150")` |
| [`cl_run`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_run) | `cvar cl_run(логическое, "0")` |
| [`cl_sendchatstate`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_sendchatstate) | `cvar cl_sendchatstate(логическое, "1")` |
| [`cl_sidespeed`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_sidespeed) | `cvar cl_sidespeed(дробное, "400")` |
| [`cl_smartjump`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_smartjump) | `cvar cl_smartjump(логическое, "1")` |
| [`cl_upspeed`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_upspeed) | `cvar cl_upspeed(дробное, "400")` |
| [`cl_yawspeed`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_yawspeed) | `cvar cl_yawspeed(дробное, "140")` |
| [`con_centernotify`](../38-cvars-reference/06-ui-console-input-cvars.md#con_centernotify) | `cvar con_centernotify(логическое, "0")` |
| [`con_displaypossibilities`](../38-cvars-reference/06-ui-console-input-cvars.md#con_displaypossibilities) | `cvar con_displaypossibilities(логическое, "1")` |
| [`con_echochat`](../38-cvars-reference/06-ui-console-input-cvars.md#con_echochat) | `cvar con_echochat(логическое, "0")` |
| [`con_maxlines`](../38-cvars-reference/06-ui-console-input-cvars.md#con_maxlines) | `cvar con_maxlines(целое, "1024")` |
| [`con_notify_w`](../38-cvars-reference/06-ui-console-input-cvars.md#con_notify_w) | `cvar con_notify_w(дробное, "1")` |
| [`con_notify_x`](../38-cvars-reference/06-ui-console-input-cvars.md#con_notify_x) | `cvar con_notify_x(дробное, "0")` |
| [`con_notify_y`](../38-cvars-reference/06-ui-console-input-cvars.md#con_notify_y) | `cvar con_notify_y(дробное, "0")` |
| [`con_notifylines`](../38-cvars-reference/06-ui-console-input-cvars.md#con_notifylines) | `cvar con_notifylines(целое, "4")` |
| [`con_notifytime`](../38-cvars-reference/06-ui-console-input-cvars.md#con_notifytime) | `cvar con_notifytime(дробное, "3")` |
| [`con_notifytime_chat`](../38-cvars-reference/06-ui-console-input-cvars.md#con_notifytime_chat) | `cvar con_notifytime_chat(дробное, "8")` |
| [`con_numnotifylines_chat`](../38-cvars-reference/06-ui-console-input-cvars.md#con_numnotifylines_chat) | `cvar con_numnotifylines_chat(целое, "8")` |
| [`con_savehistory`](../38-cvars-reference/06-ui-console-input-cvars.md#con_savehistory) | `cvar con_savehistory(логическое, "1")` |
| [`con_separatechat`](../38-cvars-reference/06-ui-console-input-cvars.md#con_separatechat) | `cvar con_separatechat(логическое, "0")` |
| [`con_showcompletion`](../38-cvars-reference/06-ui-console-input-cvars.md#con_showcompletion) | `cvar con_showcompletion(логическое, "1")` |
| [`con_stayhidden`](../38-cvars-reference/06-ui-console-input-cvars.md#con_stayhidden) | `cvar con_stayhidden(целое 0-3, "1")` |
| [`con_textsize`](../38-cvars-reference/06-ui-console-input-cvars.md#con_textsize) | `cvar con_textsize(целое, "8")` |
| [`con_timeformat`](../38-cvars-reference/06-ui-console-input-cvars.md#con_timeformat) | `cvar con_timeformat(строка формата времени, "(%H:%M:%S) ")` |
| [`con_timestamps`](../38-cvars-reference/06-ui-console-input-cvars.md#con_timestamps) | `cvar con_timestamps(логическое, "0")` |
| [`in_builtinkeymap`](../38-cvars-reference/06-ui-console-input-cvars.md#in_builtinkeymap) | `cvar in_builtinkeymap(логическое, "0")` |
| [`in_dinput`](../38-cvars-reference/06-ui-console-input-cvars.md#in_dinput) | `cvar in_dinput(логическое, "0")` |
| [`in_nonstandarddeadkeys`](../38-cvars-reference/06-ui-console-input-cvars.md#in_nonstandarddeadkeys) | `cvar in_nonstandarddeadkeys(логическое, "1")` |
| [`in_rawinput`](../38-cvars-reference/06-ui-console-input-cvars.md#in_rawinput) | `cvar in_rawinput(логическое, "0")` |
| [`in_rawinput_keyboard`](../38-cvars-reference/06-ui-console-input-cvars.md#in_rawinput_keyboard) | `cvar in_rawinput_keyboard(логическое, "0")` |
| [`in_simulatemultitouch`](../38-cvars-reference/06-ui-console-input-cvars.md#in_simulatemultitouch) | `cvar in_simulatemultitouch(логическое, "0")` |
| [`in_xflip`](../38-cvars-reference/06-ui-console-input-cvars.md#in_xflip) | `cvar in_xflip(логическое, "0")` |
| [`in_xinput`](../38-cvars-reference/06-ui-console-input-cvars.md#in_xinput) | `cvar in_xinput(целое 0-3, "1")` |
| [`joyexponent`](../38-cvars-reference/06-ui-console-input-cvars.md#joyexponent) | `cvar joyexponent(дробное, "1")` |
| [`joyonly`](../38-cvars-reference/06-ui-console-input-cvars.md#joyonly) | `cvar joyonly(логическое, "0")` |
| [`joypitchsensitivity`](../38-cvars-reference/06-ui-console-input-cvars.md#joypitchsensitivity) | `cvar joypitchsensitivity(дробное, "0.5")` |
| [`joypitchthreshold`](../38-cvars-reference/06-ui-console-input-cvars.md#joypitchthreshold) | `cvar joypitchthreshold(дробное, "0.19")` |
| [`joyrollsensitivity`](../38-cvars-reference/06-ui-console-input-cvars.md#joyrollsensitivity) | `cvar joyrollsensitivity(дробное, "1.0")` |
| [`joyrollthreshold`](../38-cvars-reference/06-ui-console-input-cvars.md#joyrollthreshold) | `cvar joyrollthreshold(дробное, "0.118")` |
| [`joyyawsensitivity`](../38-cvars-reference/06-ui-console-input-cvars.md#joyyawsensitivity) | `cvar joyyawsensitivity(дробное, "1.0")` |
| [`joyyawthreshold`](../38-cvars-reference/06-ui-console-input-cvars.md#joyyawthreshold) | `cvar joyyawthreshold(дробное, "0.19")` |
| [`m_filter`](../38-cvars-reference/06-ui-console-input-cvars.md#m_filter) | `cvar m_filter(дробное 0-2, "0")` |
| [`m_forcewheel`](../38-cvars-reference/06-ui-console-input-cvars.md#m_forcewheel) | `cvar m_forcewheel(целое 0-2, "1")` |
| [`m_forcewheel_threshold`](../38-cvars-reference/06-ui-console-input-cvars.md#m_forcewheel_threshold) | `cvar m_forcewheel_threshold(целое, "32")` |
| [`m_helpismedia`](../38-cvars-reference/06-ui-console-input-cvars.md#m_helpismedia) | `cvar m_helpismedia(логическое, "0")` |
| [`m_longpressthreshold`](../38-cvars-reference/06-ui-console-input-cvars.md#m_longpressthreshold) | `cvar m_longpressthreshold(дробное, "1")` |
| [`m_slidethreshold`](../38-cvars-reference/06-ui-console-input-cvars.md#m_slidethreshold) | `cvar m_slidethreshold(дробное, "10")` |
| [`m_touchmajoraxis`](../38-cvars-reference/06-ui-console-input-cvars.md#m_touchmajoraxis) | `cvar m_touchmajoraxis(логическое, "1")` |
| [`sbar_teamstatus`](../38-cvars-reference/06-ui-console-input-cvars.md#sbar_teamstatus) | `cvar sbar_teamstatus(целое 0-2, "1")` |
| [`scr_loadingrefresh`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_loadingrefresh) | `cvar scr_loadingrefresh(логическое, "0")` |
| [`scr_loadingscreen_aspect`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_loadingscreen_aspect) | `cvar scr_loadingscreen_aspect(целое -1/0/1/2, "0")` |
| [`scr_loadingscreen_picture`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_loadingscreen_picture) | `cvar scr_loadingscreen_picture(строка/путь, "gfx/loading")` |
| [`scr_loadingscreen_scale`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_loadingscreen_scale) | `cvar scr_loadingscreen_scale(дробное, "1")` |
| [`scr_loadingscreen_scale_limit`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_loadingscreen_scale_limit) | `cvar scr_loadingscreen_scale_limit(целое, "2")` |
| [`scr_scoreboard_afk`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_afk) | `cvar scr_scoreboard_afk(логическое, "1")` |
| [`scr_scoreboard_backgroundalpha`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_backgroundalpha) | `cvar scr_scoreboard_backgroundalpha(дробное, "0.5")` |
| [`scr_scoreboard_drawtitle`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_drawtitle) | `cvar scr_scoreboard_drawtitle(логическое, "1")` |
| [`scr_scoreboard_fillalpha`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_fillalpha) | `cvar scr_scoreboard_fillalpha(дробное, "0.7")` |
| [`scr_scoreboard_forcecolors`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_forcecolors) | `cvar scr_scoreboard_forcecolors(логическое, "0")` |
| [`scr_scoreboard_newstyle`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_newstyle) | `cvar scr_scoreboard_newstyle(логическое, "1")` |
| [`scr_scoreboard_ping_status`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_ping_status) | `cvar scr_scoreboard_ping_status(список из 4 чисел, "25 50 100 150")` |
| [`scr_scoreboard_showflags`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_showflags) | `cvar scr_scoreboard_showflags(целое 0-2, "2")` |
| [`scr_scoreboard_showfrags`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_showfrags) | `cvar scr_scoreboard_showfrags(логическое, "0")` |
| [`scr_scoreboard_showhealth`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_showhealth) | `cvar scr_scoreboard_showhealth(целое 0-3, "3")` |
| [`scr_scoreboard_showlocation`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_showlocation) | `cvar scr_scoreboard_showlocation(логическое, "1")` |
| [`scr_scoreboard_showruleset`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_showruleset) | `cvar scr_scoreboard_showruleset(целое 0-2, "1")` |
| [`scr_scoreboard_showweapon`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_showweapon) | `cvar scr_scoreboard_showweapon(логическое, "1")` |
| [`scr_scoreboard_teamscores`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_teamscores) | `cvar scr_scoreboard_teamscores(логическое, "1")` |
| [`scr_scoreboard_teamsort`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_teamsort) | `cvar scr_scoreboard_teamsort(логическое, "0")` |
| [`scr_scoreboard_titleseperator`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_titleseperator) | `cvar scr_scoreboard_titleseperator(логическое, "1")` |
| [`scr_showdisk`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_showdisk) | `cvar scr_showdisk(логическое, "0")` |
| [`scr_showloading`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_showloading) | `cvar scr_showloading(логическое, "1")` |
| [`scr_showobituaries`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_showobituaries) | `cvar scr_showobituaries(логическое, "0")` |
| [`show_speed`](../38-cvars-reference/06-ui-console-input-cvars.md#show_speed) | `cvar show_speed(логическое, "0")` |
| [`sys_osk`](../38-cvars-reference/06-ui-console-input-cvars.md#sys_osk) | `cvar sys_osk(логическое, "0")` |
| [`cl_clock_x`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_clock_x) | `cvar cl_clock_x(int, "0")` |
| [`cl_clock_y`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_clock_y) | `cvar cl_clock_y(int, "-1")` |
| [`cl_cursor`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_cursor) | `cvar cl_cursor(string, "")` |
| [`cl_cursor_scale`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_cursor_scale) | `cvar cl_cursor_scale(float, "1.0")` |
| [`cl_gameclock_x`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_gameclock_x) | `cvar cl_gameclock_x(int, "0")` |
| [`cl_gameclock_y`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_gameclock_y) | `cvar cl_gameclock_y(int, "-1")` |
| [`cl_prydoncursor`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_prydoncursor) | `cvar cl_prydoncursor(string, "")` |
| [`cl_standardchat`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_standardchat) | `cvar cl_standardchat(int, "0")` |
| [`cl_vrui_force`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_vrui_force) | `cvar cl_vrui_force(int, "0")` |
| [`cl_vrui_lock`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_vrui_lock) | `cvar cl_vrui_lock(int, "1")` |
| [`con_logcenterprint`](../38-cvars-reference/06-ui-console-input-cvars.md#con_logcenterprint) | `cvar con_logcenterprint(int, "1")` |
| [`con_ocranaleds`](../38-cvars-reference/06-ui-console-input-cvars.md#con_ocranaleds) | `cvar con_ocranaleds(int, "2")` |
| [`con_textfont`](../38-cvars-reference/06-ui-console-input-cvars.md#con_textfont) | `cvar con_textfont(string, "")` |
| [`con_window`](../38-cvars-reference/06-ui-console-input-cvars.md#con_window) | `cvar con_window(int, "0")` |
| [`contrast`](../38-cvars-reference/06-ui-console-input-cvars.md#contrast) | `cvar contrast(float, "1.0")` |
| [`crosshairalpha`](../38-cvars-reference/06-ui-console-input-cvars.md#crosshairalpha) | `cvar crosshairalpha(int, "1")` |
| [`crosshaircolor`](../38-cvars-reference/06-ui-console-input-cvars.md#crosshaircolor) | `cvar crosshaircolor(string, "255 255 255")` |
| [`dpcompat_console`](../38-cvars-reference/06-ui-console-input-cvars.md#dpcompat_console) | `cvar dpcompat_console(int, "0")` |
| [`gamma`](../38-cvars-reference/06-ui-console-input-cvars.md#gamma) | `cvar gamma(float, "1.0")` |
| [`in_forceseat`](../38-cvars-reference/06-ui-console-input-cvars.md#in_forceseat) | `cvar in_forceseat(int, "0")` |
| [`in_rawinput_rdp`](../38-cvars-reference/06-ui-console-input-cvars.md#in_rawinput_rdp) | `cvar in_rawinput_rdp(int, "0")` |
| [`in_skipplayerone`](../38-cvars-reference/06-ui-console-input-cvars.md#in_skipplayerone) | `cvar in_skipplayerone(int, "1")` |
| [`in_vraim`](../38-cvars-reference/06-ui-console-input-cvars.md#in_vraim) | `cvar in_vraim(int, "1")` |
| [`in_windowed_mouse`](../38-cvars-reference/06-ui-console-input-cvars.md#in_windowed_mouse) | `cvar in_windowed_mouse(int, "1")` |
| [`joystick`](../38-cvars-reference/06-ui-console-input-cvars.md#joystick) | `cvar joystick(int, "0")` |
| [`m_forward`](../38-cvars-reference/06-ui-console-input-cvars.md#m_forward) | `cvar m_forward(int, "1")` |
| [`m_pitch`](../38-cvars-reference/06-ui-console-input-cvars.md#m_pitch) | `cvar m_pitch(float, "0.022")` |
| [`m_side`](../38-cvars-reference/06-ui-console-input-cvars.md#m_side) | `cvar m_side(float, "0.8")` |
| [`m_yaw`](../38-cvars-reference/06-ui-console-input-cvars.md#m_yaw) | `cvar m_yaw(float, "0.022")` |
| [`pr_menu_coreonerror`](../38-cvars-reference/06-ui-console-input-cvars.md#pr_menu_coreonerror) | `cvar pr_menu_coreonerror(int, "1")` |
| [`pr_menu_memsize`](../38-cvars-reference/06-ui-console-input-cvars.md#pr_menu_memsize) | `cvar pr_menu_memsize(string, "64m")` |
| [`r_globalskin_count`](../38-cvars-reference/06-ui-console-input-cvars.md#r_globalskin_count) | `cvar r_globalskin_count(int, "10")` |
| [`r_globalskin_first`](../38-cvars-reference/06-ui-console-input-cvars.md#r_globalskin_first) | `cvar r_globalskin_first(int, "100")` |
| [`r_part_rain_quantity`](../38-cvars-reference/06-ui-console-input-cvars.md#r_part_rain_quantity) | `cvar r_part_rain_quantity(int, "1")` |
| [`r_skin_overlays`](../38-cvars-reference/06-ui-console-input-cvars.md#r_skin_overlays) | `cvar r_skin_overlays(int, "1")` |
| [`rcon_address`](../38-cvars-reference/06-ui-console-input-cvars.md#rcon_address) | `cvar rcon_address(string, "")` |
| [`rcon_level`](../38-cvars-reference/06-ui-console-input-cvars.md#rcon_level) | `cvar rcon_level(int, "20")` |
| [`scr_allowsnap`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_allowsnap) | `cvar scr_allowsnap(int, "0")` |
| [`scr_autoid`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_autoid) | `cvar scr_autoid(int, "1")` |
| [`scr_autoid_armor`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_autoid_armor) | `cvar scr_autoid_armor(int, "1")` |
| [`scr_autoid_enemycolour`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_autoid_enemycolour) | `cvar scr_autoid_enemycolour(string, "The colour for the text on the nametags of non-team members.")` |
| [`scr_autoid_health`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_autoid_health) | `cvar scr_autoid_health(int, "1")` |
| [`scr_autoid_team`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_autoid_team) | `cvar scr_autoid_team(int, "0")` |
| [`scr_autoid_teamcolour`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_autoid_teamcolour) | `cvar scr_autoid_teamcolour(string, "The colour for the text on the nametags of team members.")` |
| [`scr_autoid_weapon`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_autoid_weapon) | `cvar scr_autoid_weapon(int, "1")` |
| [`scr_autoid_weapon_mask`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_autoid_weapon_mask) | `cvar scr_autoid_weapon_mask(int, "126")` |
| [`scr_centersbar`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_centersbar) | `cvar scr_centersbar(int, "2")` |
| [`scr_centertime`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_centertime) | `cvar scr_centertime(int, "2")` |
| [`scr_conalpha`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_conalpha) | `cvar scr_conalpha(float, "0.7")` |
| [`scr_consize`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_consize) | `cvar scr_consize(float, "0.5")` |
| [`scr_conspeed`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_conspeed) | `cvar scr_conspeed(int, "2000")` |
| [`scr_diskicontimeout`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_diskicontimeout) | `cvar scr_diskicontimeout(float, "0.3")` |
| [`scr_neticontimeout`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_neticontimeout) | `cvar scr_neticontimeout(float, "0.3")` |
| [`scr_printspeed`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_printspeed) | `cvar scr_printspeed(int, "16")` |
| [`scr_showdisk_x`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_showdisk_x) | `cvar scr_showdisk_x(int, "-24")` |
| [`scr_showdisk_y`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_showdisk_y) | `cvar scr_showdisk_y(int, "0")` |
| [`scr_sshot_compression`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_sshot_compression) | `cvar scr_sshot_compression(int, "75")` |
| [`scr_sshot_prefix`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_sshot_prefix) | `cvar scr_sshot_prefix(string, "screenshots/fte-")` |
| [`scr_sshot_type`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_sshot_type) | `cvar scr_sshot_type(string, "png")` |
| [`scr_turtlefps`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_turtlefps) | `cvar scr_turtlefps(int, "10")` |
| [`scr_usekfont`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_usekfont) | `cvar scr_usekfont(int, "0")` |
| [`v_contrastboost`](../38-cvars-reference/06-ui-console-input-cvars.md#v_contrastboost) | `cvar v_contrastboost(float, "1.0")` |
| [`v_gammainverted`](../38-cvars-reference/06-ui-console-input-cvars.md#v_gammainverted) | `cvar v_gammainverted(int, "0")` |
| [`vid_desktopgamma`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_desktopgamma) | `cvar vid_desktopgamma(int, "0")` |
| [`vid_gl_context_compatibility`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_gl_context_compatibility) | `cvar vid_gl_context_compatibility(int, "1")` |
| [`vid_gl_context_debug`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_gl_context_debug) | `cvar vid_gl_context_debug(int, "0")` |
| [`vid_gl_context_es`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_gl_context_es) | `cvar vid_gl_context_es(int, "0")` |
| [`vid_gl_context_forwardcompatible`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_gl_context_forwardcompatible) | `cvar vid_gl_context_forwardcompatible(int, "0")` |
| [`vid_gl_context_noerror`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_gl_context_noerror) | `cvar vid_gl_context_noerror(string, "")` |
| [`vid_gl_context_robustness`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_gl_context_robustness) | `cvar vid_gl_context_robustness(int, "1")` |
| [`vid_gl_context_selfreset`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_gl_context_selfreset) | `cvar vid_gl_context_selfreset(int, "1")` |
| [`vid_gl_context_version`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_gl_context_version) | `cvar vid_gl_context_version(string, "")` |
| [`vid_preservegamma`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_preservegamma) | `cvar vid_preservegamma(int, "0")` |

### Системные, отладочные и прочие cvar

| Элемент | Сигнатура / Описание |
|---|---|
| [`_cl_disconnectreason`](../38-cvars-reference/07-system-misc-cvars.md#_cl_disconnectreason) | `cvar _cl_disconnectreason(string, "")` |
| [`_pext_infoblobs`](../38-cvars-reference/07-system-misc-cvars.md#_pext_infoblobs) | `cvar _pext_infoblobs(int, "0")` |
| [`_pext_lerptime`](../38-cvars-reference/07-system-misc-cvars.md#_pext_lerptime) | `cvar _pext_lerptime(int, "0")` |
| [`_pext_vrinputs`](../38-cvars-reference/07-system-misc-cvars.md#_pext_vrinputs) | `cvar _pext_vrinputs(int, "0")` |
| [`_q3bsp_bihtraces`](../38-cvars-reference/07-system-misc-cvars.md#_q3bsp_bihtraces) | `cvar _q3bsp_bihtraces(int, "0")` |
| [`allow_f_cmdline`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_cmdline) | `cvar allow_f_cmdline(int, "0")` |
| [`allow_f_fakeshaft`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_fakeshaft) | `cvar allow_f_fakeshaft(int, "1")` |
| [`allow_f_modified`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_modified) | `cvar allow_f_modified(int, "1")` |
| [`allow_f_ruleset`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_ruleset) | `cvar allow_f_ruleset(int, "1")` |
| [`allow_f_scripts`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_scripts) | `cvar allow_f_scripts(int, "1")` |
| [`allow_f_server`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_server) | `cvar allow_f_server(int, "1")` |
| [`allow_f_skins`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_skins) | `cvar allow_f_skins(int, "1")` |
| [`allow_f_system`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_system) | `cvar allow_f_system(int, "0")` |
| [`allow_f_version`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_version) | `cvar allow_f_version(int, "1")` |
| [`allow_skybox`](../38-cvars-reference/07-system-misc-cvars.md#allow_skybox) | `cvar allow_skybox(string, "")` |
| [`allow_splitscreen`](../38-cvars-reference/07-system-misc-cvars.md#allow_splitscreen) | `cvar allow_splitscreen(string, "")` |
| [`auth_validateclients`](../38-cvars-reference/07-system-misc-cvars.md#auth_validateclients) | `cvar auth_validateclients(int, "1")` |
| [`b_switch`](../38-cvars-reference/07-system-misc-cvars.md#b_switch) | `cvar b_switch(string, "")` |
| [`baseskin`](../38-cvars-reference/07-system-misc-cvars.md#baseskin) | `cvar baseskin(string, "")` |
| [`bottomcolor`](../38-cvars-reference/07-system-misc-cvars.md#bottomcolor) | `cvar bottomcolor(int, "12")` |
| [`capturecodec`](../38-cvars-reference/07-system-misc-cvars.md#capturecodec) | `cvar capturecodec(string, "tga")` |
| [`capturedemoheight`](../38-cvars-reference/07-system-misc-cvars.md#capturedemoheight) | `cvar capturedemoheight(int, "0")` |
| [`capturedemowidth`](../38-cvars-reference/07-system-misc-cvars.md#capturedemowidth) | `cvar capturedemowidth(int, "0")` |
| [`capturedriver`](../38-cvars-reference/07-system-misc-cvars.md#capturedriver) | `cvar capturedriver(string, "")` |
| [`capturemessage`](../38-cvars-reference/07-system-misc-cvars.md#capturemessage) | `cvar capturemessage(string, "")` |
| [`capturethrottlesize`](../38-cvars-reference/07-system-misc-cvars.md#capturethrottlesize) | `cvar capturethrottlesize(int, "0")` |
| [`cfg_reload_on_gamedir`](../38-cvars-reference/07-system-misc-cvars.md#cfg_reload_on_gamedir) | `cvar cfg_reload_on_gamedir(int, "1")` |
| [`cfg_save_aliases`](../38-cvars-reference/07-system-misc-cvars.md#cfg_save_aliases) | `cvar cfg_save_aliases(int, "1")` |
| [`cfg_save_all`](../38-cvars-reference/07-system-misc-cvars.md#cfg_save_all) | `cvar cfg_save_all(string, "")` |
| [`cfg_save_auto`](../38-cvars-reference/07-system-misc-cvars.md#cfg_save_auto) | `cvar cfg_save_auto(int, "0")` |
| [`cfg_save_binds`](../38-cvars-reference/07-system-misc-cvars.md#cfg_save_binds) | `cvar cfg_save_binds(int, "1")` |
| [`cfg_save_buttons`](../38-cvars-reference/07-system-misc-cvars.md#cfg_save_buttons) | `cvar cfg_save_buttons(int, "0")` |
| [`cfg_save_infos`](../38-cvars-reference/07-system-misc-cvars.md#cfg_save_infos) | `cvar cfg_save_infos(int, "1")` |
| [`cfg_save_name`](../38-cvars-reference/07-system-misc-cvars.md#cfg_save_name) | `cvar cfg_save_name(string, "fte")` |
| [`cl_aliasoverlap`](../38-cvars-reference/07-system-misc-cvars.md#cl_aliasoverlap) | `cvar cl_aliasoverlap(int, "1")` |
| [`cl_autotrack`](../38-cvars-reference/07-system-misc-cvars.md#cl_autotrack) | `cvar cl_autotrack(string, "auto")` |
| [`cl_autotrack_team`](../38-cvars-reference/07-system-misc-cvars.md#cl_autotrack_team) | `cvar cl_autotrack_team(string, "")` |
| [`cl_beam_alpha`](../38-cvars-reference/07-system-misc-cvars.md#cl_beam_alpha) | `cvar cl_beam_alpha(int, "1")` |
| [`cl_beam_trace`](../38-cvars-reference/07-system-misc-cvars.md#cl_beam_trace) | `cvar cl_beam_trace(int, "0")` |
| [`cl_c2sdupe`](../38-cvars-reference/07-system-misc-cvars.md#cl_c2sdupe) | `cvar cl_c2sdupe(int, "0")` |
| [`cl_c2sImpulseBackup`](../38-cvars-reference/07-system-misc-cvars.md#cl_c2simpulsebackup) | `cvar cl_c2sImpulseBackup(int, "3")` |
| [`cl_c2sMaxRedundancy`](../38-cvars-reference/07-system-misc-cvars.md#cl_c2smaxredundancy) | `cvar cl_c2sMaxRedundancy(int, "5")` |
| [`cl_c2spps`](../38-cvars-reference/07-system-misc-cvars.md#cl_c2spps) | `cvar cl_c2spps(int, "0")` |
| [`cl_chasecam`](../38-cvars-reference/07-system-misc-cvars.md#cl_chasecam) | `cvar cl_chasecam(int, "1")` |
| [`cl_countpendingpl`](../38-cvars-reference/07-system-misc-cvars.md#cl_countpendingpl) | `cvar cl_countpendingpl(int, "0")` |
| [`cl_crossx`](../38-cvars-reference/07-system-misc-cvars.md#cl_crossx) | `cvar cl_crossx(int, "0")` |
| [`cl_crossy`](../38-cvars-reference/07-system-misc-cvars.md#cl_crossy) | `cvar cl_crossy(int, "0")` |
| [`cl_crypt_rcon`](../38-cvars-reference/07-system-misc-cvars.md#cl_crypt_rcon) | `cvar cl_crypt_rcon(int, "1")` |
| [`cl_csqc_nodeprecate`](../38-cvars-reference/07-system-misc-cvars.md#cl_csqc_nodeprecate) | `cvar cl_csqc_nodeprecate(int, "0")` |
| [`cl_csqcdebug`](../38-cvars-reference/07-system-misc-cvars.md#cl_csqcdebug) | `cvar cl_csqcdebug(int, "0")` |
| [`cl_deadbodyfilter`](../38-cvars-reference/07-system-misc-cvars.md#cl_deadbodyfilter) | `cvar cl_deadbodyfilter(int, "0")` |
| [`cl_delay_packets`](../38-cvars-reference/07-system-misc-cvars.md#cl_delay_packets) | `cvar cl_delay_packets(int, "0")` |
| [`cl_demoreel`](../38-cvars-reference/07-system-misc-cvars.md#cl_demoreel) | `cvar cl_demoreel(int, "0")` |
| [`cl_demospeed`](../38-cvars-reference/07-system-misc-cvars.md#cl_demospeed) | `cvar cl_demospeed(float, "1")` |
| [`cl_dlemptyterminate`](../38-cvars-reference/07-system-misc-cvars.md#cl_dlemptyterminate) | `cvar cl_dlemptyterminate(int, "1")` |
| [`cl_expsprite`](../38-cvars-reference/07-system-misc-cvars.md#cl_expsprite) | `cvar cl_expsprite(int, "1")` |
| [`cl_fakeframes`](../38-cvars-reference/07-system-misc-cvars.md#cl_fakeframes) | `cvar cl_fakeframes(int, "0")` |
| [`cl_fullpitch`](../38-cvars-reference/07-system-misc-cvars.md#cl_fullpitch) | `cvar cl_fullpitch(int, "0")` |
| [`cl_gibfilter`](../38-cvars-reference/07-system-misc-cvars.md#cl_gibfilter) | `cvar cl_gibfilter(int, "0")` |
| [`cl_gunanglex`](../38-cvars-reference/07-system-misc-cvars.md#cl_gunanglex) | `cvar cl_gunanglex(int, "0")` |
| [`cl_gunangley`](../38-cvars-reference/07-system-misc-cvars.md#cl_gunangley) | `cvar cl_gunangley(int, "0")` |
| [`cl_gunanglez`](../38-cvars-reference/07-system-misc-cvars.md#cl_gunanglez) | `cvar cl_gunanglez(int, "0")` |
| [`cl_gunx`](../38-cvars-reference/07-system-misc-cvars.md#cl_gunx) | `cvar cl_gunx(int, "0")` |
| [`cl_guny`](../38-cvars-reference/07-system-misc-cvars.md#cl_guny) | `cvar cl_guny(int, "0")` |
| [`cl_gunz`](../38-cvars-reference/07-system-misc-cvars.md#cl_gunz) | `cvar cl_gunz(int, "0")` |
| [`cl_hightrack`](../38-cvars-reference/07-system-misc-cvars.md#cl_hightrack) | `cvar cl_hightrack(int, "0")` |
| [`cl_hudswap`](../38-cvars-reference/07-system-misc-cvars.md#cl_hudswap) | `cvar cl_hudswap(int, "0")` |
| [`cl_idlefps`](../38-cvars-reference/07-system-misc-cvars.md#cl_idlefps) | `cvar cl_idlefps(int, "60")` |
| [`cl_legacystains`](../38-cvars-reference/07-system-misc-cvars.md#cl_legacystains) | `cvar cl_legacystains(int, "1")` |
| [`cl_lerp_maxdistance`](../38-cvars-reference/07-system-misc-cvars.md#cl_lerp_maxdistance) | `cvar cl_lerp_maxdistance(int, "200")` |
| [`cl_lerp_maxinterval`](../38-cvars-reference/07-system-misc-cvars.md#cl_lerp_maxinterval) | `cvar cl_lerp_maxinterval(float, "0.3")` |
| [`cl_lerp_players`](../38-cvars-reference/07-system-misc-cvars.md#cl_lerp_players) | `cvar cl_lerp_players(int, "0")` |
| [`cl_loopbackprotocol`](../38-cvars-reference/07-system-misc-cvars.md#cl_loopbackprotocol) | `cvar cl_loopbackprotocol(string, "qw")` |
| [`cl_maxfps`](../38-cvars-reference/07-system-misc-cvars.md#cl_maxfps) | `cvar cl_maxfps(int, "250")` |
| [`cl_model_bobbing`](../38-cvars-reference/07-system-misc-cvars.md#cl_model_bobbing) | `cvar cl_model_bobbing(int, "0")` |
| [`cl_muzzleflash`](../38-cvars-reference/07-system-misc-cvars.md#cl_muzzleflash) | `cvar cl_muzzleflash(int, "1")` |
| [`cl_netfps`](../38-cvars-reference/07-system-misc-cvars.md#cl_netfps) | `cvar cl_netfps(int, "150")` |
| [`cl_noblink`](../38-cvars-reference/07-system-misc-cvars.md#cl_noblink) | `cvar cl_noblink(int, "0")` |
| [`cl_nocsqc`](../38-cvars-reference/07-system-misc-cvars.md#cl_nocsqc) | `cvar cl_nocsqc(int, "0")` |
| [`cl_nodelta`](../38-cvars-reference/07-system-misc-cvars.md#cl_nodelta) | `cvar cl_nodelta(int, "0")` |
| [`cl_nofake`](../38-cvars-reference/07-system-misc-cvars.md#cl_nofake) | `cvar cl_nofake(int, "2")` |
| [`cl_nolerp`](../38-cvars-reference/07-system-misc-cvars.md#cl_nolerp) | `cvar cl_nolerp(int, "0")` |
| [`cl_nolerp_netquake`](../38-cvars-reference/07-system-misc-cvars.md#cl_nolerp_netquake) | `cvar cl_nolerp_netquake(int, "0")` |
| [`cl_nopext`](../38-cvars-reference/07-system-misc-cvars.md#cl_nopext) | `cvar cl_nopext(int, "0")` |
| [`cl_parsewhitetext`](../38-cvars-reference/07-system-misc-cvars.md#cl_parsewhitetext) | `cvar cl_parsewhitetext(int, "1")` |
| [`cl_part_density_fade`](../38-cvars-reference/07-system-misc-cvars.md#cl_part_density_fade) | `cvar cl_part_density_fade(int, "1024")` |
| [`cl_part_density_fade_start`](../38-cvars-reference/07-system-misc-cvars.md#cl_part_density_fade_start) | `cvar cl_part_density_fade_start(int, "1024")` |
| [`cl_pext_mask`](../38-cvars-reference/07-system-misc-cvars.md#cl_pext_mask) | `cvar cl_pext_mask(string, "0xffffffff")` |
| [`cl_playerclass`](../38-cvars-reference/07-system-misc-cvars.md#cl_playerclass) | `cvar cl_playerclass(string, "")` |
| [`cl_proxyaddr`](../38-cvars-reference/07-system-misc-cvars.md#cl_proxyaddr) | `cvar cl_proxyaddr(string, "")` |
| [`cl_pure`](../38-cvars-reference/07-system-misc-cvars.md#cl_pure) | `cvar cl_pure(int, "0")` |
| [`cl_queueimpulses`](../38-cvars-reference/07-system-misc-cvars.md#cl_queueimpulses) | `cvar cl_queueimpulses(int, "0")` |
| [`cl_r2g`](../38-cvars-reference/07-system-misc-cvars.md#cl_r2g) | `cvar cl_r2g(int, "0")` |
| [`cl_rollalpha`](../38-cvars-reference/07-system-misc-cvars.md#cl_rollalpha) | `cvar cl_rollalpha(int, "20")` |
| [`cl_sbar`](../38-cvars-reference/07-system-misc-cvars.md#cl_sbar) | `cvar cl_sbar(int, "0")` |
| [`cl_sbaralpha`](../38-cvars-reference/07-system-misc-cvars.md#cl_sbaralpha) | `cvar cl_sbaralpha(float, "0.75")` |
| [`cl_selfcam`](../38-cvars-reference/07-system-misc-cvars.md#cl_selfcam) | `cvar cl_selfcam(int, "1")` |
| [`cl_sendguid`](../38-cvars-reference/07-system-misc-cvars.md#cl_sendguid) | `cvar cl_sendguid(string, "")` |
| [`cl_serveraddress`](../38-cvars-reference/07-system-misc-cvars.md#cl_serveraddress) | `cvar cl_serveraddress(string, "none")` |
| [`cl_servername`](../38-cvars-reference/07-system-misc-cvars.md#cl_servername) | `cvar cl_servername(string, "")` |
| [`cl_shownet`](../38-cvars-reference/07-system-misc-cvars.md#cl_shownet) | `cvar cl_shownet(int, "0")` |
| [`cl_solid_players`](../38-cvars-reference/07-system-misc-cvars.md#cl_solid_players) | `cvar cl_solid_players(int, "1")` |
| [`cl_splitscreen`](../38-cvars-reference/07-system-misc-cvars.md#cl_splitscreen) | `cvar cl_splitscreen(int, "0")` |
| [`cl_standardmsg`](../38-cvars-reference/07-system-misc-cvars.md#cl_standardmsg) | `cvar cl_standardmsg(int, "0")` |
| [`cl_threadedphysics`](../38-cvars-reference/07-system-misc-cvars.md#cl_threadedphysics) | `cvar cl_threadedphysics(int, "0")` |
| [`cl_timeout`](../38-cvars-reference/07-system-misc-cvars.md#cl_timeout) | `cvar cl_timeout(int, "60")` |
| [`cl_truelightning`](../38-cvars-reference/07-system-misc-cvars.md#cl_truelightning) | `cvar cl_truelightning(int, "0")` |
| [`cl_verify_urischeme`](../38-cvars-reference/07-system-misc-cvars.md#cl_verify_urischeme) | `cvar cl_verify_urischeme(int, "2")` |
| [`cl_warncmd`](../38-cvars-reference/07-system-misc-cvars.md#cl_warncmd) | `cvar cl_warncmd(int, "1")` |
| [`cl_weaponforgetorder`](../38-cvars-reference/07-system-misc-cvars.md#cl_weaponforgetorder) | `cvar cl_weaponforgetorder(int, "0")` |
| [`cl_weaponhide`](../38-cvars-reference/07-system-misc-cvars.md#cl_weaponhide) | `cvar cl_weaponhide(int, "0")` |
| [`cl_weaponhide_preference`](../38-cvars-reference/07-system-misc-cvars.md#cl_weaponhide_preference) | `cvar cl_weaponhide_preference(string, "2 1")` |
| [`cl_weaponpreselect`](../38-cvars-reference/07-system-misc-cvars.md#cl_weaponpreselect) | `cvar cl_weaponpreselect(int, "0")` |
| [`cl_yieldcpu`](../38-cvars-reference/07-system-misc-cvars.md#cl_yieldcpu) | `cvar cl_yieldcpu(int, "1")` |
| [`cmd_allowaccess`](../38-cvars-reference/07-system-misc-cvars.md#cmd_allowaccess) | `cvar cmd_allowaccess(int, "0")` |
| [`cmd_gamecodelevel`](../38-cvars-reference/07-system-misc-cvars.md#cmd_gamecodelevel) | `cvar cmd_gamecodelevel(string, "")` |
| [`cmd_maxbuffersize`](../38-cvars-reference/07-system-misc-cvars.md#cmd_maxbuffersize) | `cvar cmd_maxbuffersize(int, "65536")` |
| [`d3d_hlsl`](../38-cvars-reference/07-system-misc-cvars.md#d3d_hlsl) | `cvar d3d_hlsl(int, "1")` |
| [`d_mipcap`](../38-cvars-reference/07-system-misc-cvars.md#d_mipcap) | `cvar d_mipcap(string, "0 1000")` |
| [`developer`](../38-cvars-reference/07-system-misc-cvars.md#developer) | `cvar developer(int, "1")` |
| [`dpcompat_csqcinputeventtypes`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_csqcinputeventtypes) | `cvar dpcompat_csqcinputeventtypes(int, "999999")` |
| [`dpcompat_findradiusarealinks`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_findradiusarealinks) | `cvar dpcompat_findradiusarealinks(int, "0")` |
| [`dpcompat_nofloodfill`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_nofloodfill) | `cvar dpcompat_nofloodfill(int, "0")` |
| [`dpcompat_nopremulpics`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_nopremulpics) | `cvar dpcompat_nopremulpics(int, "0")` |
| [`dpcompat_nopreparse`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_nopreparse) | `cvar dpcompat_nopreparse(int, "0")` |
| [`dpcompat_noretouchground`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_noretouchground) | `cvar dpcompat_noretouchground(int, "0")` |
| [`dpcompat_psa_ungroup`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_psa_ungroup) | `cvar dpcompat_psa_ungroup(int, "0")` |
| [`dpcompat_set`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_set) | `cvar dpcompat_set(int, "0")` |
| [`dpcompat_skinfiles`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_skinfiles) | `cvar dpcompat_skinfiles(int, "0")` |
| [`dpcompat_smallerfonts`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_smallerfonts) | `cvar dpcompat_smallerfonts(int, "0")` |
| [`dpcompat_stats`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_stats) | `cvar dpcompat_stats(int, "0")` |
| [`dpcompat_strcat_limit`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_strcat_limit) | `cvar dpcompat_strcat_limit(string, "")` |
| [`dpcompat_traceontouch`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_traceontouch) | `cvar dpcompat_traceontouch(int, "0")` |
| [`edit_addcr`](../38-cvars-reference/07-system-misc-cvars.md#edit_addcr) | `cvar edit_addcr(string, "")` |
| [`edit_stripcr`](../38-cvars-reference/07-system-misc-cvars.md#edit_stripcr) | `cvar edit_stripcr(int, "1")` |
| [`edit_tabsize`](../38-cvars-reference/07-system-misc-cvars.md#edit_tabsize) | `cvar edit_tabsize(int, "4")` |
| [`enemyforceskins`](../38-cvars-reference/07-system-misc-cvars.md#enemyforceskins) | `cvar enemyforceskins(int, "0")` |
| [`ezcompat_markup`](../38-cvars-reference/07-system-misc-cvars.md#ezcompat_markup) | `cvar ezcompat_markup(int, "1")` |
| [`fbskins`](../38-cvars-reference/07-system-misc-cvars.md#fbskins) | `cvar fbskins(string, "")` |
| [`forceqmenu`](../38-cvars-reference/07-system-misc-cvars.md#forceqmenu) | `cvar forceqmenu(int, "0")` |
| [`fraglog_details`](../38-cvars-reference/07-system-misc-cvars.md#fraglog_details) | `cvar fraglog_details(int, "1")` |
| [`gamecfg`](../38-cvars-reference/07-system-misc-cvars.md#gamecfg) | `cvar gamecfg(int, "0")` |
| [`gameversion`](../38-cvars-reference/07-system-misc-cvars.md#gameversion) | `cvar gameversion(string, "")` |
| [`gameversion_max`](../38-cvars-reference/07-system-misc-cvars.md#gameversion_max) | `cvar gameversion_max(string, "")` |
| [`gameversion_min`](../38-cvars-reference/07-system-misc-cvars.md#gameversion_min) | `cvar gameversion_min(string, "")` |
| [`hand`](../38-cvars-reference/07-system-misc-cvars.md#hand) | `cvar hand(string, "")` |
| [`ignore_flood`](../38-cvars-reference/07-system-misc-cvars.md#ignore_flood) | `cvar ignore_flood(int, "0")` |
| [`ignore_flood_duration`](../38-cvars-reference/07-system-misc-cvars.md#ignore_flood_duration) | `cvar ignore_flood_duration(int, "4")` |
| [`ignore_mode`](../38-cvars-reference/07-system-misc-cvars.md#ignore_mode) | `cvar ignore_mode(int, "0")` |
| [`ignore_opponents`](../38-cvars-reference/07-system-misc-cvars.md#ignore_opponents) | `cvar ignore_opponents(int, "0")` |
| [`ignore_qizmo_spec`](../38-cvars-reference/07-system-misc-cvars.md#ignore_qizmo_spec) | `cvar ignore_qizmo_spec(int, "0")` |
| [`ignore_spec`](../38-cvars-reference/07-system-misc-cvars.md#ignore_spec) | `cvar ignore_spec(int, "0")` |
| [`ipautodump`](../38-cvars-reference/07-system-misc-cvars.md#ipautodump) | `cvar ipautodump(int, "0")` |
| [`itburnsitburnsmakeitstop`](../38-cvars-reference/07-system-misc-cvars.md#itburnsitburnsmakeitstop) | `cvar itburnsitburnsmakeitstop(int, "0")` |
| [`joyradialdeadzone`](../38-cvars-reference/07-system-misc-cvars.md#joyradialdeadzone) | `cvar joyradialdeadzone(string, "")` |
| [`lang`](../38-cvars-reference/07-system-misc-cvars.md#lang) | `cvar lang(string, "")` |
| [`leftisright`](../38-cvars-reference/07-system-misc-cvars.md#leftisright) | `cvar leftisright(int, "0")` |
| [`log_developer`](../38-cvars-reference/07-system-misc-cvars.md#log_developer) | `cvar log_developer(int, "0")` |
| [`log_dir`](../38-cvars-reference/07-system-misc-cvars.md#log_dir) | `cvar log_dir(string, "")` |
| [`log_dosformat`](../38-cvars-reference/07-system-misc-cvars.md#log_dosformat) | `cvar log_dosformat(int, "1")` |
| [`log_readable`](../38-cvars-reference/07-system-misc-cvars.md#log_readable) | `cvar log_readable(int, "7")` |
| [`log_rotate_files`](../38-cvars-reference/07-system-misc-cvars.md#log_rotate_files) | `cvar log_rotate_files(int, "0")` |
| [`log_rotate_size`](../38-cvars-reference/07-system-misc-cvars.md#log_rotate_size) | `cvar log_rotate_size(int, "131072")` |
| [`log_timestamps`](../38-cvars-reference/07-system-misc-cvars.md#log_timestamps) | `cvar log_timestamps(int, "1")` |
| [`lookspring`](../38-cvars-reference/07-system-misc-cvars.md#lookspring) | `cvar lookspring(int, "0")` |
| [`lookstrafe`](../38-cvars-reference/07-system-misc-cvars.md#lookstrafe) | `cvar lookstrafe(int, "0")` |
| [`m_accel`](../38-cvars-reference/07-system-misc-cvars.md#m_accel) | `cvar m_accel(int, "0")` |
| [`m_accel_noforce`](../38-cvars-reference/07-system-misc-cvars.md#m_accel_noforce) | `cvar m_accel_noforce(int, "0")` |
| [`m_accel_offset`](../38-cvars-reference/07-system-misc-cvars.md#m_accel_offset) | `cvar m_accel_offset(int, "0")` |
| [`m_accel_power`](../38-cvars-reference/07-system-misc-cvars.md#m_accel_power) | `cvar m_accel_power(int, "2")` |
| [`m_accel_senscap`](../38-cvars-reference/07-system-misc-cvars.md#m_accel_senscap) | `cvar m_accel_senscap(int, "0")` |
| [`m_accel_style`](../38-cvars-reference/07-system-misc-cvars.md#m_accel_style) | `cvar m_accel_style(int, "1")` |
| [`m_fatpressthreshold`](../38-cvars-reference/07-system-misc-cvars.md#m_fatpressthreshold) | `cvar m_fatpressthreshold(float, "0.2")` |
| [`m_preset_chosen`](../38-cvars-reference/07-system-misc-cvars.md#m_preset_chosen) | `cvar m_preset_chosen(int, "0")` |
| [`m_threshold_noforce`](../38-cvars-reference/07-system-misc-cvars.md#m_threshold_noforce) | `cvar m_threshold_noforce(int, "0")` |
| [`m_touchstrafe`](../38-cvars-reference/07-system-misc-cvars.md#m_touchstrafe) | `cvar m_touchstrafe(int, "0")` |
| [`map_autoopenportals`](../38-cvars-reference/07-system-misc-cvars.md#map_autoopenportals) | `cvar map_autoopenportals(int, "0")` |
| [`map_noareas`](../38-cvars-reference/07-system-misc-cvars.md#map_noareas) | `cvar map_noareas(int, "0")` |
| [`map_noCurves`](../38-cvars-reference/07-system-misc-cvars.md#map_nocurves) | `cvar map_noCurves(int, "0")` |
| [`mapname`](../38-cvars-reference/07-system-misc-cvars.md#mapname) | `cvar mapname(string, "")` |
| [`maxpitch`](../38-cvars-reference/07-system-misc-cvars.md#maxpitch) | `cvar maxpitch(string, "")` |
| [`minpitch`](../38-cvars-reference/07-system-misc-cvars.md#minpitch) | `cvar minpitch(string, "")` |
| [`mod_h2holey_bugged`](../38-cvars-reference/07-system-misc-cvars.md#mod_h2holey_bugged) | `cvar mod_h2holey_bugged(int, "0")` |
| [`mod_halftexel`](../38-cvars-reference/07-system-misc-cvars.md#mod_halftexel) | `cvar mod_halftexel(int, "1")` |
| [`mod_lightpoint_distance`](../38-cvars-reference/07-system-misc-cvars.md#mod_lightpoint_distance) | `cvar mod_lightpoint_distance(int, "8192")` |
| [`mod_lightscale_broken`](../38-cvars-reference/07-system-misc-cvars.md#mod_lightscale_broken) | `cvar mod_lightscale_broken(int, "0")` |
| [`mod_loadmappackages`](../38-cvars-reference/07-system-misc-cvars.md#mod_loadmappackages) | `cvar mod_loadmappackages(int, "1")` |
| [`mod_md3flags`](../38-cvars-reference/07-system-misc-cvars.md#mod_md3flags) | `cvar mod_md3flags(int, "1")` |
| [`mod_md5_singleanimation`](../38-cvars-reference/07-system-misc-cvars.md#mod_md5_singleanimation) | `cvar mod_md5_singleanimation(int, "1")` |
| [`mod_nomipmap`](../38-cvars-reference/07-system-misc-cvars.md#mod_nomipmap) | `cvar mod_nomipmap(int, "0")` |
| [`mod_obj_orientation`](../38-cvars-reference/07-system-misc-cvars.md#mod_obj_orientation) | `cvar mod_obj_orientation(int, "1")` |
| [`mod_precache`](../38-cvars-reference/07-system-misc-cvars.md#mod_precache) | `cvar mod_precache(int, "1")` |
| [`mod_warnmodels`](../38-cvars-reference/07-system-misc-cvars.md#mod_warnmodels) | `cvar mod_warnmodels(int, "1")` |
| [`model`](../38-cvars-reference/07-system-misc-cvars.md#model) | `cvar model(string, "")` |
| [`msg`](../38-cvars-reference/07-system-misc-cvars.md#msg) | `cvar msg(int, "1")` |
| [`msg_filter`](../38-cvars-reference/07-system-misc-cvars.md#msg_filter) | `cvar msg_filter(int, "0")` |
| [`musicvolume`](../38-cvars-reference/07-system-misc-cvars.md#musicvolume) | `cvar musicvolume(float, "0.3")` |
| [`name`](../38-cvars-reference/07-system-misc-cvars.md#name) | `cvar name(string, "Player")` |
| [`noaim`](../38-cvars-reference/07-system-misc-cvars.md#noaim) | `cvar noaim(string, "")` |
| [`noexit`](../38-cvars-reference/07-system-misc-cvars.md#noexit) | `cvar noexit(int, "0")` |
| [`nomonsters`](../38-cvars-reference/07-system-misc-cvars.md#nomonsters) | `cvar nomonsters(int, "0")` |
| [`noskins`](../38-cvars-reference/07-system-misc-cvars.md#noskins) | `cvar noskins(int, "0")` |
| [`pext_ezquake_nochunks`](../38-cvars-reference/07-system-misc-cvars.md#pext_ezquake_nochunks) | `cvar pext_ezquake_nochunks(int, "0")` |
| [`pext_ezquake_verfortrans`](../38-cvars-reference/07-system-misc-cvars.md#pext_ezquake_verfortrans) | `cvar pext_ezquake_verfortrans(int, "7088")` |
| [`pext_predinfo`](../38-cvars-reference/07-system-misc-cvars.md#pext_predinfo) | `cvar pext_predinfo(int, "1")` |
| [`pext_replacementdeltas`](../38-cvars-reference/07-system-misc-cvars.md#pext_replacementdeltas) | `cvar pext_replacementdeltas(int, "1")` |
| [`pkg_autoupdate`](../38-cvars-reference/07-system-misc-cvars.md#pkg_autoupdate) | `cvar pkg_autoupdate(int, "1")` |
| [`plug_loaddefault`](../38-cvars-reference/07-system-misc-cvars.md#plug_loaddefault) | `cvar plug_loaddefault(int, "1")` |
| [`plug_sbar`](../38-cvars-reference/07-system-misc-cvars.md#plug_sbar) | `cvar plug_sbar(int, "3")` |
| [`prox_inmenu`](../38-cvars-reference/07-system-misc-cvars.md#prox_inmenu) | `cvar prox_inmenu(int, "0")` |
| [`q3bsp_ignorestyles`](../38-cvars-reference/07-system-misc-cvars.md#q3bsp_ignorestyles) | `cvar q3bsp_ignorestyles(int, "0")` |
| [`q3bsp_mergelightmaps`](../38-cvars-reference/07-system-misc-cvars.md#q3bsp_mergelightmaps) | `cvar q3bsp_mergelightmaps(int, "1")` |
| [`q3bsp_surf_meshcollision_flag`](../38-cvars-reference/07-system-misc-cvars.md#q3bsp_surf_meshcollision_flag) | `cvar q3bsp_surf_meshcollision_flag(string, "0x80000000")` |
| [`q3bsp_surf_meshcollision_force`](../38-cvars-reference/07-system-misc-cvars.md#q3bsp_surf_meshcollision_force) | `cvar q3bsp_surf_meshcollision_force(int, "0")` |
| [`qport_`](../38-cvars-reference/07-system-misc-cvars.md#qport_) | `cvar qport_(int, "0")` |
| [`rank_autoadd`](../38-cvars-reference/07-system-misc-cvars.md#rank_autoadd) | `cvar rank_autoadd(int, "1")` |
| [`rank_filename`](../38-cvars-reference/07-system-misc-cvars.md#rank_filename) | `cvar rank_filename(string, "")` |
| [`rank_needlogin`](../38-cvars-reference/07-system-misc-cvars.md#rank_needlogin) | `cvar rank_needlogin(int, "0")` |
| [`record_flush`](../38-cvars-reference/07-system-misc-cvars.md#record_flush) | `cvar record_flush(int, "0")` |
| [`registered`](../38-cvars-reference/07-system-misc-cvars.md#registered) | `cvar registered(int, "0")` |
| [`route_shownodes`](../38-cvars-reference/07-system-misc-cvars.md#route_shownodes) | `cvar route_shownodes(int, "0")` |
| [`ruleset`](../38-cvars-reference/07-system-misc-cvars.md#ruleset) | `cvar ruleset(string, "none")` |
| [`ruleset_allow_fbmodels`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_fbmodels) | `cvar ruleset_allow_fbmodels(int, "0")` |
| [`ruleset_allow_frj`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_frj) | `cvar ruleset_allow_frj(int, "1")` |
| [`ruleset_allow_in`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_in) | `cvar ruleset_allow_in(int, "1")` |
| [`ruleset_allow_localvolume`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_localvolume) | `cvar ruleset_allow_localvolume(int, "1")` |
| [`ruleset_allow_modified_eyes`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_modified_eyes) | `cvar ruleset_allow_modified_eyes(int, "0")` |
| [`ruleset_allow_packet`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_packet) | `cvar ruleset_allow_packet(int, "1")` |
| [`ruleset_allow_particle_lightning`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_particle_lightning) | `cvar ruleset_allow_particle_lightning(int, "1")` |
| [`ruleset_allow_playercount`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_playercount) | `cvar ruleset_allow_playercount(int, "1")` |
| [`ruleset_allow_semicheats`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_semicheats) | `cvar ruleset_allow_semicheats(int, "1")` |
| [`ruleset_allow_sensitive_texture_replacements`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_sensitive_texture_replacements) | `cvar ruleset_allow_sensitive_texture_replacements(int, "1")` |
| [`ruleset_allow_triggers`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_triggers) | `cvar ruleset_allow_triggers(int, "1")` |
| [`ruleset_allow_watervis`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_watervis) | `cvar ruleset_allow_watervis(int, "1")` |
| [`saved1`](../38-cvars-reference/07-system-misc-cvars.md#saved1) | `cvar saved1(int, "0")` |
| [`saved2`](../38-cvars-reference/07-system-misc-cvars.md#saved2) | `cvar saved2(int, "0")` |
| [`saved3`](../38-cvars-reference/07-system-misc-cvars.md#saved3) | `cvar saved3(int, "0")` |
| [`saved4`](../38-cvars-reference/07-system-misc-cvars.md#saved4) | `cvar saved4(int, "0")` |
| [`savedgamecfg`](../38-cvars-reference/07-system-misc-cvars.md#savedgamecfg) | `cvar savedgamecfg(int, "0")` |
| [`sb_alpha`](../38-cvars-reference/07-system-misc-cvars.md#sb_alpha) | `cvar sb_alpha(float, "0.7")` |
| [`sb_filtertext`](../38-cvars-reference/07-system-misc-cvars.md#sb_filtertext) | `cvar sb_filtertext(string, "")` |
| [`sb_hidedead`](../38-cvars-reference/07-system-misc-cvars.md#sb_hidedead) | `cvar sb_hidedead(int, "1")` |
| [`sb_hideempty`](../38-cvars-reference/07-system-misc-cvars.md#sb_hideempty) | `cvar sb_hideempty(int, "0")` |
| [`sb_hidefull`](../38-cvars-reference/07-system-misc-cvars.md#sb_hidefull) | `cvar sb_hidefull(int, "0")` |
| [`sb_hidenetquake`](../38-cvars-reference/07-system-misc-cvars.md#sb_hidenetquake) | `cvar sb_hidenetquake(int, "0")` |
| [`sb_hidenotempty`](../38-cvars-reference/07-system-misc-cvars.md#sb_hidenotempty) | `cvar sb_hidenotempty(int, "0")` |
| [`sb_hideproxies`](../38-cvars-reference/07-system-misc-cvars.md#sb_hideproxies) | `cvar sb_hideproxies(int, "1")` |
| [`sb_hidequakeworld`](../38-cvars-reference/07-system-misc-cvars.md#sb_hidequakeworld) | `cvar sb_hidequakeworld(int, "0")` |
| [`sb_showaddress`](../38-cvars-reference/07-system-misc-cvars.md#sb_showaddress) | `cvar sb_showaddress(int, "0")` |
| [`sb_showgamedir`](../38-cvars-reference/07-system-misc-cvars.md#sb_showgamedir) | `cvar sb_showgamedir(int, "0")` |
| [`sb_showmap`](../38-cvars-reference/07-system-misc-cvars.md#sb_showmap) | `cvar sb_showmap(int, "0")` |
| [`sb_showping`](../38-cvars-reference/07-system-misc-cvars.md#sb_showping) | `cvar sb_showping(int, "0")` |
| [`sb_showplayers`](../38-cvars-reference/07-system-misc-cvars.md#sb_showplayers) | `cvar sb_showplayers(int, "1")` |
| [`sb_sortcolumn`](../38-cvars-reference/07-system-misc-cvars.md#sb_sortcolumn) | `cvar sb_sortcolumn(int, "0")` |
| [`scratch1`](../38-cvars-reference/07-system-misc-cvars.md#scratch1) | `cvar scratch1(int, "0")` |
| [`scratch2`](../38-cvars-reference/07-system-misc-cvars.md#scratch2) | `cvar scratch2(int, "0")` |
| [`scratch3`](../38-cvars-reference/07-system-misc-cvars.md#scratch3) | `cvar scratch3(int, "0")` |
| [`scratch4`](../38-cvars-reference/07-system-misc-cvars.md#scratch4) | `cvar scratch4(int, "0")` |
| [`secure`](../38-cvars-reference/07-system-misc-cvars.md#secure) | `cvar secure(string, "")` |
| [`sensitivity`](../38-cvars-reference/07-system-misc-cvars.md#sensitivity) | `cvar sensitivity(int, "10")` |
| [`show_fps`](../38-cvars-reference/07-system-misc-cvars.md#show_fps) | `cvar show_fps(int, "0")` |
| [`show_speed_x`](../38-cvars-reference/07-system-misc-cvars.md#show_speed_x) | `cvar show_speed_x(int, "-1")` |
| [`show_speed_y`](../38-cvars-reference/07-system-misc-cvars.md#show_speed_y) | `cvar show_speed_y(int, "-9")` |
| [`showdrop`](../38-cvars-reference/07-system-misc-cvars.md#showdrop) | `cvar showdrop(int, "0")` |
| [`showpackets`](../38-cvars-reference/07-system-misc-cvars.md#showpackets) | `cvar showpackets(int, "0")` |
| [`showpause`](../38-cvars-reference/07-system-misc-cvars.md#showpause) | `cvar showpause(int, "1")` |
| [`showturtle`](../38-cvars-reference/07-system-misc-cvars.md#showturtle) | `cvar showturtle(int, "0")` |
| [`skin`](../38-cvars-reference/07-system-misc-cvars.md#skin) | `cvar skin(string, "")` |
| [`skyroom`](../38-cvars-reference/07-system-misc-cvars.md#skyroom) | `cvar skyroom(string, "")` |
| [`slist_cacheinfo`](../38-cvars-reference/07-system-misc-cvars.md#slist_cacheinfo) | `cvar slist_cacheinfo(int, "0")` |
| [`slist_writeservers`](../38-cvars-reference/07-system-misc-cvars.md#slist_writeservers) | `cvar slist_writeservers(int, "1")` |
| [`spectator`](../38-cvars-reference/07-system-misc-cvars.md#spectator) | `cvar spectator(string, "")` |
| [`sw_fthreads`](../38-cvars-reference/07-system-misc-cvars.md#sw_fthreads) | `cvar sw_fthreads(int, "0")` |
| [`sw_interlace`](../38-cvars-reference/07-system-misc-cvars.md#sw_interlace) | `cvar sw_interlace(int, "0")` |
| [`sw_vthread`](../38-cvars-reference/07-system-misc-cvars.md#sw_vthread) | `cvar sw_vthread(int, "0")` |
| [`team`](../38-cvars-reference/07-system-misc-cvars.md#team) | `cvar team(string, "")` |
| [`topcolor`](../38-cvars-reference/07-system-misc-cvars.md#topcolor) | `cvar topcolor(int, "13")` |
| [`tp_disputablemacros`](../38-cvars-reference/07-system-misc-cvars.md#tp_disputablemacros) | `cvar tp_disputablemacros(int, "1")` |
| [`utf8_enable`](../38-cvars-reference/07-system-misc-cvars.md#utf8_enable) | `cvar utf8_enable(int, "0")` |
| [`vk_amd_rasterization_order`](../38-cvars-reference/07-system-misc-cvars.md#vk_amd_rasterization_order) | `cvar vk_amd_rasterization_order(string, "")` |
| [`vk_busywait`](../38-cvars-reference/07-system-misc-cvars.md#vk_busywait) | `cvar vk_busywait(string, "")` |
| [`vk_debug`](../38-cvars-reference/07-system-misc-cvars.md#vk_debug) | `cvar vk_debug(int, "0")` |
| [`vk_dualqueue`](../38-cvars-reference/07-system-misc-cvars.md#vk_dualqueue) | `cvar vk_dualqueue(string, "")` |
| [`vk_ext_astc_decode_mode`](../38-cvars-reference/07-system-misc-cvars.md#vk_ext_astc_decode_mode) | `cvar vk_ext_astc_decode_mode(string, "")` |
| [`vk_stagingbuffers`](../38-cvars-reference/07-system-misc-cvars.md#vk_stagingbuffers) | `cvar vk_stagingbuffers(string, "")` |
| [`vk_submissionthread`](../38-cvars-reference/07-system-misc-cvars.md#vk_submissionthread) | `cvar vk_submissionthread(string, "")` |
| [`vk_usememorypools`](../38-cvars-reference/07-system-misc-cvars.md#vk_usememorypools) | `cvar vk_usememorypools(string, "")` |
| [`vk_waitfence`](../38-cvars-reference/07-system-misc-cvars.md#vk_waitfence) | `cvar vk_waitfence(string, "")` |
| [`volume`](../38-cvars-reference/07-system-misc-cvars.md#volume) | `cvar volume(float, "0.7")` |
| [`votelevel`](../38-cvars-reference/07-system-misc-cvars.md#votelevel) | `cvar votelevel(int, "0")` |
| [`voteminimum`](../38-cvars-reference/07-system-misc-cvars.md#voteminimum) | `cvar voteminimum(int, "4")` |
| [`votepercent`](../38-cvars-reference/07-system-misc-cvars.md#votepercent) | `cvar votepercent(int, "-1")` |
| [`votetime`](../38-cvars-reference/07-system-misc-cvars.md#votetime) | `cvar votetime(int, "10")` |
| [`w_switch`](../38-cvars-reference/07-system-misc-cvars.md#w_switch) | `cvar w_switch(string, "")` |
| [`watervis`](../38-cvars-reference/07-system-misc-cvars.md#watervis) | `cvar watervis(string, "")` |
| [`xinput_leftvibrator`](../38-cvars-reference/07-system-misc-cvars.md#xinput_leftvibrator) | `cvar xinput_leftvibrator(int, "0")` |
| [`xinput_rightvibrator`](../38-cvars-reference/07-system-misc-cvars.md#xinput_rightvibrator) | `cvar xinput_rightvibrator(int, "0")` |

## Ключи сущностей карты (entity keys)

Всего задокументировано: **146** ключей. Полный постатейный разбор — в разделе [«39. Ключи сущностей карты»](../39-entity-keys-reference/README.md).


### Общие ключи, worldspawn и глобальные настройки уровня

| Элемент | Сигнатура / Описание |
|---|---|
| [`classname`](../39-entity-keys-reference/01-worldspawn-common-keys.md#classname) | `тип значения: string` |
| [`origin`](../39-entity-keys-reference/01-worldspawn-common-keys.md#origin) | `тип значения: vector("x y z")` |
| [`angle`](../39-entity-keys-reference/01-worldspawn-common-keys.md#angle) | `тип значения: float` |
| [`angles`](../39-entity-keys-reference/01-worldspawn-common-keys.md#angles) | `тип значения: vector("x y z")` |
| [`targetname`](../39-entity-keys-reference/01-worldspawn-common-keys.md#targetname) | `тип значения: string` |
| [`target`](../39-entity-keys-reference/01-worldspawn-common-keys.md#target) | `тип значения: string` |
| [`target2`](../39-entity-keys-reference/01-worldspawn-common-keys.md#target2) | `тип значения: string` |
| [`target3`](../39-entity-keys-reference/01-worldspawn-common-keys.md#target3) | `тип значения: string` |
| [`killtarget`](../39-entity-keys-reference/01-worldspawn-common-keys.md#killtarget) | `тип значения: string` |
| [`spawnflags`](../39-entity-keys-reference/01-worldspawn-common-keys.md#spawnflags) | `тип значения: integer` |
| [`model`](../39-entity-keys-reference/01-worldspawn-common-keys.md#model) | `тип значения: string` |
| [`wait`](../39-entity-keys-reference/01-worldspawn-common-keys.md#wait) | `тип значения: float` |
| [`delay`](../39-entity-keys-reference/01-worldspawn-common-keys.md#delay) | `тип значения: float` |
| [`message`](../39-entity-keys-reference/01-worldspawn-common-keys.md#message) | `тип значения: string` |
| [`noise`](../39-entity-keys-reference/01-worldspawn-common-keys.md#noise) | `тип значения: string` |
| [`sounds`](../39-entity-keys-reference/01-worldspawn-common-keys.md#sounds) | `тип значения: integer` |
| [`count`](../39-entity-keys-reference/01-worldspawn-common-keys.md#count) | `тип значения: integer` |
| [`dmg`](../39-entity-keys-reference/01-worldspawn-common-keys.md#dmg) | `тип значения: float` |
| [`health`](../39-entity-keys-reference/01-worldspawn-common-keys.md#health) | `тип значения: float` |
| [`style`](../39-entity-keys-reference/01-worldspawn-common-keys.md#style) | `тип значения: integer` |
| [`skin`](../39-entity-keys-reference/01-worldspawn-common-keys.md#skin) | `тип значения: integer` |
| [`mangle`](../39-entity-keys-reference/01-worldspawn-common-keys.md#mangle) | `тип значения: vector("x y z")` |
| [`light`](../39-entity-keys-reference/01-worldspawn-common-keys.md#light) | `тип значения: integer` |
| [`speed`](../39-entity-keys-reference/01-worldspawn-common-keys.md#speed) | `тип значения: float` |
| [`map`](../39-entity-keys-reference/01-worldspawn-common-keys.md#map) | `тип значения: string` |
| [`lip`](../39-entity-keys-reference/01-worldspawn-common-keys.md#lip) | `тип значения: float` |
| [`height`](../39-entity-keys-reference/01-worldspawn-common-keys.md#height) | `тип значения: float` |
| [`message`](../39-entity-keys-reference/01-worldspawn-common-keys.md#message) | `тип значения: string` |
| [`sounds`](../39-entity-keys-reference/01-worldspawn-common-keys.md#sounds) | `тип значения: integer` |
| [`worldtype`](../39-entity-keys-reference/01-worldspawn-common-keys.md#worldtype) | `тип значения: integer` |
| [`wad`](../39-entity-keys-reference/01-worldspawn-common-keys.md#wad) | `тип значения: string` |
| [`model`](../39-entity-keys-reference/01-worldspawn-common-keys.md#model) | `тип значения: string` |
| [`gravity`](../39-entity-keys-reference/01-worldspawn-common-keys.md#gravity) | `тип значения: float` |
| [`sky`](../39-entity-keys-reference/01-worldspawn-common-keys.md#sky) | `тип значения: string` |
| [`MaxRange`](../39-entity-keys-reference/01-worldspawn-common-keys.md#maxrange) | `тип значения: float` |

### Свет и освещение

| Элемент | Сигнатура / Описание |
|---|---|
| [`classname`](../39-entity-keys-reference/02-light-entity-keys.md#classname) | `тип значения: string` |
| [`origin`](../39-entity-keys-reference/02-light-entity-keys.md#origin) | `тип значения: vector("x y z")` |
| [`light`](../39-entity-keys-reference/02-light-entity-keys.md#light) | `тип значения: integer или string из четырёх чисел "R G B brightness"` |
| [`style`](../39-entity-keys-reference/02-light-entity-keys.md#style) | `тип значения: integer` |
| [`targetname`](../39-entity-keys-reference/02-light-entity-keys.md#targetname) | `тип значения: string` |
| [`target`](../39-entity-keys-reference/02-light-entity-keys.md#target) | `тип значения: string` |
| [`spawnflags`](../39-entity-keys-reference/02-light-entity-keys.md#spawnflags) | `тип значения: integer` |
| [`angle`](../39-entity-keys-reference/02-light-entity-keys.md#angle) | `тип значения: float` |
| [`mangle`](../39-entity-keys-reference/02-light-entity-keys.md#mangle) | `тип значения: vector("pitch yaw roll")` |
| [`angles`](../39-entity-keys-reference/02-light-entity-keys.md#angles) | `тип значения: vector("pitch yaw roll")` |
| [`cone`](../39-entity-keys-reference/02-light-entity-keys.md#cone) | `тип значения: float` |
| [`color`](../39-entity-keys-reference/02-light-entity-keys.md#color) | `тип значения: vector("r g b")` |
| [`delay`](../39-entity-keys-reference/02-light-entity-keys.md#delay) | `тип значения: integer` |
| [`wait`](../39-entity-keys-reference/02-light-entity-keys.md#wait) | `тип значения: float` |
| [`fade`](../39-entity-keys-reference/02-light-entity-keys.md#fade) | `тип значения: float` |
| [`scale`](../39-entity-keys-reference/02-light-entity-keys.md#scale) | `тип значения: float` |
| [`skin`](../39-entity-keys-reference/02-light-entity-keys.md#skin) | `тип значения: integer` |
| [`pflags`](../39-entity-keys-reference/02-light-entity-keys.md#pflags) | `тип значения: integer` |
| [`light_radius`](../39-entity-keys-reference/02-light-entity-keys.md#light_radius) | `тип значения: float` |

### Триггеры и логические сущности

| Элемент | Сигнатура / Описание |
|---|---|
| [`classname`](../39-entity-keys-reference/03-trigger-logic-keys.md#classname) | `тип значения: string` |
| [`targetname`](../39-entity-keys-reference/03-trigger-logic-keys.md#targetname) | `тип значения: string` |
| [`target`](../39-entity-keys-reference/03-trigger-logic-keys.md#target) | `тип значения: string` |
| [`target2`](../39-entity-keys-reference/03-trigger-logic-keys.md#target2) | `тип значения: string` |
| [`killtarget`](../39-entity-keys-reference/03-trigger-logic-keys.md#killtarget) | `тип значения: string` |
| [`message`](../39-entity-keys-reference/03-trigger-logic-keys.md#message) | `тип значения: string` |
| [`sounds`](../39-entity-keys-reference/03-trigger-logic-keys.md#sounds) | `тип значения: integer` |
| [`noise`](../39-entity-keys-reference/03-trigger-logic-keys.md#noise) | `тип значения: string` |
| [`wait`](../39-entity-keys-reference/03-trigger-logic-keys.md#wait) | `тип значения: float` |
| [`delay`](../39-entity-keys-reference/03-trigger-logic-keys.md#delay) | `тип значения: float` |
| [`health`](../39-entity-keys-reference/03-trigger-logic-keys.md#health) | `тип значения: float` |
| [`count`](../39-entity-keys-reference/03-trigger-logic-keys.md#count) | `тип значения: integer` |
| [`dmg`](../39-entity-keys-reference/03-trigger-logic-keys.md#dmg) | `тип значения: float` |
| [`speed`](../39-entity-keys-reference/03-trigger-logic-keys.md#speed) | `тип значения: float` |
| [`height`](../39-entity-keys-reference/03-trigger-logic-keys.md#height) | `тип значения: float` |
| [`map`](../39-entity-keys-reference/03-trigger-logic-keys.md#map) | `тип значения: string` |
| [`angle`](../39-entity-keys-reference/03-trigger-logic-keys.md#angle) | `тип значения: float` |
| [`angles`](../39-entity-keys-reference/03-trigger-logic-keys.md#angles) | `тип значения: vector` |
| [`origin`](../39-entity-keys-reference/03-trigger-logic-keys.md#origin) | `тип значения: vector` |
| [`model`](../39-entity-keys-reference/03-trigger-logic-keys.md#model) | `тип значения: string` |
| [`spawnflags`](../39-entity-keys-reference/03-trigger-logic-keys.md#spawnflags) | `тип значения: integer` |
| [`spawnflags`](../39-entity-keys-reference/03-trigger-logic-keys.md#spawnflags-notouch) | `тип значения: integer` |
| [`spawnflags`](../39-entity-keys-reference/03-trigger-logic-keys.md#spawnflags-nomessage) | `тип значения: integer` |
| [`spawnflags`](../39-entity-keys-reference/03-trigger-logic-keys.md#spawnflags-player_only) | `тип значения: integer` |
| [`spawnflags`](../39-entity-keys-reference/03-trigger-logic-keys.md#spawnflags-silent) | `тип значения: integer` |
| [`spawnflags`](../39-entity-keys-reference/03-trigger-logic-keys.md#spawnflags-push_once) | `тип значения: integer` |
| [`spawnflags`](../39-entity-keys-reference/03-trigger-logic-keys.md#spawnflags-no_intermission) | `тип значения: integer` |
| [`path_corner`](../39-entity-keys-reference/03-trigger-logic-keys.md#path_corner) | `тип значения: string` |
| [`info_notnull`](../39-entity-keys-reference/03-trigger-logic-keys.md#info_notnull) | `тип значения: string` |
| [`info_null`](../39-entity-keys-reference/03-trigger-logic-keys.md#info_null) | `тип значения: string` |

### Двери, платформы и подвижная геометрия

| Элемент | Сигнатура / Описание |
|---|---|
| [`classname`](../39-entity-keys-reference/04-func-brush-entity-keys.md#classname) | `тип значения: string` |
| [`targetname`](../39-entity-keys-reference/04-func-brush-entity-keys.md#targetname) | `тип значения: string` |
| [`target`](../39-entity-keys-reference/04-func-brush-entity-keys.md#target) | `тип значения: string` |
| [`message`](../39-entity-keys-reference/04-func-brush-entity-keys.md#message) | `тип значения: string` |
| [`killtarget`](../39-entity-keys-reference/04-func-brush-entity-keys.md#killtarget) | `тип значения: string` |
| [`delay`](../39-entity-keys-reference/04-func-brush-entity-keys.md#delay) | `тип значения: float` |
| [`angle`](../39-entity-keys-reference/04-func-brush-entity-keys.md#angle) | `тип значения: float` |
| [`angles`](../39-entity-keys-reference/04-func-brush-entity-keys.md#angles) | `тип значения: vector` |
| [`speed`](../39-entity-keys-reference/04-func-brush-entity-keys.md#speed) | `тип значения: float` |
| [`wait`](../39-entity-keys-reference/04-func-brush-entity-keys.md#wait) | `тип значения: float` |
| [`lip`](../39-entity-keys-reference/04-func-brush-entity-keys.md#lip) | `тип значения: float` |
| [`dmg`](../39-entity-keys-reference/04-func-brush-entity-keys.md#dmg) | `тип значения: float` |
| [`sounds`](../39-entity-keys-reference/04-func-brush-entity-keys.md#sounds) | `тип значения: integer` |
| [`health`](../39-entity-keys-reference/04-func-brush-entity-keys.md#health) | `тип значения: float` |
| [`height`](../39-entity-keys-reference/04-func-brush-entity-keys.md#height) | `тип значения: float` |
| [`t_width`](../39-entity-keys-reference/04-func-brush-entity-keys.md#t_width) | `тип значения: float` |
| [`t_length`](../39-entity-keys-reference/04-func-brush-entity-keys.md#t_length) | `тип значения: float` |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-start_open) | `тип значения: integer` |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-door_dont_link) | `тип значения: integer` |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-gold_key) | `тип значения: integer` |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-silver_key) | `тип значения: integer` |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-toggle) | `тип значения: integer` |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-open_once) | `тип значения: integer` |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-1st_left) | `тип значения: integer` |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-1st_down) | `тип значения: integer` |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-no_shoot) | `тип значения: integer` |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-always_shoot) | `тип значения: integer` |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-plat_low_trigger) | `тип значения: integer` |

### Монстры, NPC и точки появления игрока

| Элемент | Сигнатура / Описание |
|---|---|
| [`classname`](../39-entity-keys-reference/05-monster-player-keys.md#classname) | `тип значения: string` |
| [`origin`](../39-entity-keys-reference/05-monster-player-keys.md#origin) | `тип значения: vector` |
| [`angle`](../39-entity-keys-reference/05-monster-player-keys.md#angle) | `тип значения: float` |
| [`angles`](../39-entity-keys-reference/05-monster-player-keys.md#angles) | `тип значения: vector` |
| [`health`](../39-entity-keys-reference/05-monster-player-keys.md#health) | `тип значения: float` |
| [`spawnflags`](../39-entity-keys-reference/05-monster-player-keys.md#spawnflags) | `тип значения: integer` |
| [`target`](../39-entity-keys-reference/05-monster-player-keys.md#target) | `тип значения: string` |
| [`targetname`](../39-entity-keys-reference/05-monster-player-keys.md#targetname) | `тип значения: string` |
| [`yaw_speed`](../39-entity-keys-reference/05-monster-player-keys.md#yaw_speed) | `тип значения: float` |
| [`items`](../39-entity-keys-reference/05-monster-player-keys.md#items) | `тип значения: integer` |
| [`model`](../39-entity-keys-reference/05-monster-player-keys.md#model) | `тип значения: string` |
| [`classname`](../39-entity-keys-reference/05-monster-player-keys.md#classname) | `тип значения: string` |
| [`origin`](../39-entity-keys-reference/05-monster-player-keys.md#origin) | `тип значения: vector` |
| [`angle`](../39-entity-keys-reference/05-monster-player-keys.md#angle) | `тип значения: float` |
| [`angles`](../39-entity-keys-reference/05-monster-player-keys.md#angles) | `тип значения: vector` |
| [`target`](../39-entity-keys-reference/05-monster-player-keys.md#target) | `тип значения: string` |
| [`targetname`](../39-entity-keys-reference/05-monster-player-keys.md#targetname) | `тип значения: string` |
| [`spawnflags`](../39-entity-keys-reference/05-monster-player-keys.md#spawnflags) | `тип значения: integer` |
| [`mangle`](../39-entity-keys-reference/05-monster-player-keys.md#mangle) | `тип значения: vector` |
| [`health`](../39-entity-keys-reference/05-monster-player-keys.md#health) | `тип значения: float` |
| [`model`](../39-entity-keys-reference/05-monster-player-keys.md#model) | `тип значения: string` |

### Предметы и оружие

| Элемент | Сигнатура / Описание |
|---|---|
| [`classname`](../39-entity-keys-reference/06-item-weapon-keys.md#classname) | `тип значения: string` |
| [`origin`](../39-entity-keys-reference/06-item-weapon-keys.md#origin) | `тип значения: vector` |
| [`angle`](../39-entity-keys-reference/06-item-weapon-keys.md#angle) | `тип значения: float` |
| [`angles`](../39-entity-keys-reference/06-item-weapon-keys.md#angles) | `тип значения: vector` |
| [`spawnflags`](../39-entity-keys-reference/06-item-weapon-keys.md#spawnflags) | `тип значения: integer` |
| [`target`](../39-entity-keys-reference/06-item-weapon-keys.md#target) | `тип значения: string` |
| [`killtarget`](../39-entity-keys-reference/06-item-weapon-keys.md#killtarget) | `тип значения: string` |
| [`delay`](../39-entity-keys-reference/06-item-weapon-keys.md#delay) | `тип значения: float` |
| [`message`](../39-entity-keys-reference/06-item-weapon-keys.md#message) | `тип значения: string` |
| [`targetname`](../39-entity-keys-reference/06-item-weapon-keys.md#targetname) | `тип значения: string` |
| [`wait`](../39-entity-keys-reference/06-item-weapon-keys.md#wait) | `тип значения: float` |
| [`count`](../39-entity-keys-reference/06-item-weapon-keys.md#count) | `тип значения: float` |
| [`effects`](../39-entity-keys-reference/06-item-weapon-keys.md#effects) | `тип значения: integer` |

## Директивы языка материалов (.shader)

Всего задокументировано: **75** директив. Полный постатейный разбор — в разделе [«40. Директивы языка материалов»](../40-shader-directives-reference/README.md).


### Директивы уровня материала

| Элемент | Сигнатура / Описание |
|---|---|
| [`cull`](../40-shader-directives-reference/01-shader-toplevel-directives.md#cull) | `cull disable|none|twosided|front|back|backside|backsided` |
| [`skyparms`](../40-shader-directives-reference/01-shader-toplevel-directives.md#skyparms) | `skyparms farbox height nearbox` |
| [`fogparms`](../40-shader-directives-reference/01-shader-toplevel-directives.md#fogparms) | `fogparms (r g b) depth` |
| [`surfaceparm`](../40-shader-directives-reference/01-shader-toplevel-directives.md#surfaceparm) | `surfaceparm keyword` |
| [`nomipmaps`](../40-shader-directives-reference/01-shader-toplevel-directives.md#nomipmaps) | `nomipmaps` |
| [`nopicmip`](../40-shader-directives-reference/01-shader-toplevel-directives.md#nopicmip) | `nopicmip` |
| [`polygonoffset`](../40-shader-directives-reference/01-shader-toplevel-directives.md#polygonoffset) | `polygonoffset [scale]` |
| [`sort`](../40-shader-directives-reference/01-shader-toplevel-directives.md#sort) | `sort portal|sky|opaque|decal|litdecal|seethrough|unlitdecal|banner|underwater|blend|additive|nearest|ripple|deferredlight|number` |
| [`deformvertexes`](../40-shader-directives-reference/01-shader-toplevel-directives.md#deformvertexes) | `deformvertexes type ...` |
| [`portal`](../40-shader-directives-reference/01-shader-toplevel-directives.md#portal) | `portal` |
| [`entitymergable`](../40-shader-directives-reference/01-shader-toplevel-directives.md#entitymergable) | `entitymergable` |
| [`clutter`](../40-shader-directives-reference/01-shader-toplevel-directives.md#clutter) | `clutter model spacing scalemin scalemax zofs anglemin anglemax` |
| [`deferredlight`](../40-shader-directives-reference/01-shader-toplevel-directives.md#deferredlight) | `deferredlight` |
| [`affine`](../40-shader-directives-reference/01-shader-toplevel-directives.md#affine) | `affine` |
| [`fullrate`](../40-shader-directives-reference/01-shader-toplevel-directives.md#fullrate) | `fullrate` |
| [`diffusemap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#diffusemap) | `diffusemap path` |
| [`normalmap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#normalmap) | `normalmap path` |
| [`specularmap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#specularmap) | `specularmap path` |
| [`fullbrightmap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#fullbrightmap) | `fullbrightmap path` |
| [`uppermap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#uppermap) | `uppermap path` |
| [`lowermap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#lowermap) | `lowermap path` |
| [`reflectmask`](../40-shader-directives-reference/01-shader-toplevel-directives.md#reflectmask) | `reflectmask path` |
| [`displacementmap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#displacementmap) | `displacementmap path` |
| [`transmissionmap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#transmissionmap) | `transmissionmap path` |
| [`thicknessmap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#thicknessmap) | `thicknessmap path` |
| [`program`](../40-shader-directives-reference/01-shader-toplevel-directives.md#program) | `program name` |
| [`glslprogram`](../40-shader-directives-reference/01-shader-toplevel-directives.md#glslprogram) | `glslprogram name` |
| [`hlslprogram`](../40-shader-directives-reference/01-shader-toplevel-directives.md#hlslprogram) | `hlslprogram name` |
| [`hlsl11program`](../40-shader-directives-reference/01-shader-toplevel-directives.md#hlsl11program) | `hlsl11program name` |
| [`portalfboscale`](../40-shader-directives-reference/01-shader-toplevel-directives.md#portalfboscale) | `portalfboscale scale` |

### Директивы уровня стадии

| Элемент | Сигнатура / Описание |
|---|---|
| [`map`](../40-shader-directives-reference/02-shader-stage-directives.md#map) | `map <textureOrSpecial>` |
| [`animmap`](../40-shader-directives-reference/02-shader-stage-directives.md#animmap) | `animmap <fps> <frame1> <frame2> ...` |
| [`clampmap`](../40-shader-directives-reference/02-shader-stage-directives.md#clampmap) | `clampmap <textureOrSpecial>` |
| [`videoMap`](../40-shader-directives-reference/02-shader-stage-directives.md#videomap) | `videoMap <videoFile>` |
| [`cubemap`](../40-shader-directives-reference/02-shader-stage-directives.md#cubemap) | `cubemap <cubeTexture>` |
| [`cameracubemap`](../40-shader-directives-reference/02-shader-stage-directives.md#cameracubemap) | `cameracubemap <cubeTexture>` |
| [`surroundmap`](../40-shader-directives-reference/02-shader-stage-directives.md#surroundmap) | `surroundmap <cubeTexture>` |
| [`blendfunc`](../40-shader-directives-reference/02-shader-stage-directives.md#blendfunc) | `blendfunc <preset>` |
| [`blend`](../40-shader-directives-reference/02-shader-stage-directives.md#blend) | `blend <presetOrFactors>` |
| [`rgbGen`](../40-shader-directives-reference/02-shader-stage-directives.md#rgbgen) | `rgbGen <mode> [args...]` |
| [`alphaGen`](../40-shader-directives-reference/02-shader-stage-directives.md#alphagen) | `alphaGen <mode> [args...]` |
| [`alphaShift`](../40-shader-directives-reference/02-shader-stage-directives.md#alphashift) | `alphaShift <speed> <min> <max>` |
| [`depthfunc`](../40-shader-directives-reference/02-shader-stage-directives.md#depthfunc) | `depthfunc <mode>` |
| [`depthwrite`](../40-shader-directives-reference/02-shader-stage-directives.md#depthwrite) | `depthwrite` |
| [`nodepthtest`](../40-shader-directives-reference/02-shader-stage-directives.md#nodepthtest) | `nodepthtest` |
| [`nodepth`](../40-shader-directives-reference/02-shader-stage-directives.md#nodepth) | `nodepth` |
| [`alphafunc`](../40-shader-directives-reference/02-shader-stage-directives.md#alphafunc) | `alphafunc <mode>` |
| [`alphaMask`](../40-shader-directives-reference/02-shader-stage-directives.md#alphamask) | `alphaMask` |
| [`alphaTest`](../40-shader-directives-reference/02-shader-stage-directives.md#alphatest) | `alphaTest 0.5` |
| [`tcMod`](../40-shader-directives-reference/02-shader-stage-directives.md#tcmod) | `tcMod <mode> [args...]` |
| [`scale`](../40-shader-directives-reference/02-shader-stage-directives.md#scale) | `scale <x> <y>` |
| [`scroll`](../40-shader-directives-reference/02-shader-stage-directives.md#scroll) | `scroll static <x> static <y>` |
| [`tcGen`](../40-shader-directives-reference/02-shader-stage-directives.md#tcgen) | `tcGen <mode> [args...]` |
| [`texgen`](../40-shader-directives-reference/02-shader-stage-directives.md#texgen) | `texgen <mode> [args...]` |
| [`envmap`](../40-shader-directives-reference/02-shader-stage-directives.md#envmap) | `envmap` |
| [`detail`](../40-shader-directives-reference/02-shader-stage-directives.md#detail) | `detail` |
| [`nolightmap`](../40-shader-directives-reference/02-shader-stage-directives.md#nolightmap) | `nolightmap` |
| [`program`](../40-shader-directives-reference/02-shader-stage-directives.md#program) | `program <programName>` |
| [`maskcolor`](../40-shader-directives-reference/02-shader-stage-directives.md#maskcolor) | `maskcolor` |
| [`maskred`](../40-shader-directives-reference/02-shader-stage-directives.md#maskred) | `maskred` |
| [`maskgreen`](../40-shader-directives-reference/02-shader-stage-directives.md#maskgreen) | `maskgreen` |
| [`maskblue`](../40-shader-directives-reference/02-shader-stage-directives.md#maskblue) | `maskblue` |
| [`maskalpha`](../40-shader-directives-reference/02-shader-stage-directives.md#maskalpha) | `maskalpha` |
| [`red`](../40-shader-directives-reference/02-shader-stage-directives.md#red) | `red <value>` |
| [`green`](../40-shader-directives-reference/02-shader-stage-directives.md#green) | `green <value>` |
| [`blue`](../40-shader-directives-reference/02-shader-stage-directives.md#blue) | `blue <value>` |
| [`alpha`](../40-shader-directives-reference/02-shader-stage-directives.md#alpha) | `alpha <value>` |
| [`map16`](../40-shader-directives-reference/02-shader-stage-directives.md#map16) | `map16 <textureOrSpecial>` |
| [`map32`](../40-shader-directives-reference/02-shader-stage-directives.md#map32) | `map32 <textureOrSpecial>` |
| [`mapcomp`](../40-shader-directives-reference/02-shader-stage-directives.md#mapcomp) | `mapcomp <textureOrSpecial>` |
| [`mapnocomp`](../40-shader-directives-reference/02-shader-stage-directives.md#mapnocomp) | `mapnocomp <textureOrSpecial>` |
| [`animcompmap`](../40-shader-directives-reference/02-shader-stage-directives.md#animcompmap) | `animcompmap <fps> <frame1> <frame2> ...` |
| [`animnocompmap`](../40-shader-directives-reference/02-shader-stage-directives.md#animnocompmap) | `animnocompmap <fps> <frame1> <frame2> ...` |
| [`animclampmap`](../40-shader-directives-reference/02-shader-stage-directives.md#animclampmap) | `animclampmap <fps> <frame1> <frame2> ...` |
| [`material`](../40-shader-directives-reference/02-shader-stage-directives.md#material) | `material <baseTexture> <normalMap> <specularMap>` |

## Директивы языка частиц (.particles)

Всего задокументировано: **76** директив. Полный постатейный разбор — в разделе [«41. Директивы языка частиц»](../41-particle-directives-reference/README.md).


### Директивы эффекта

| Элемент | Сигнатура / Описание |
|---|---|
| [`shader`](../41-particle-directives-reference/01-particle-effect-directives.md#shader) | `shader [shaderName]` |
| [`texture`](../41-particle-directives-reference/01-particle-effect-directives.md#texture) | `texture path` |
| [`tcoords`](../41-particle-directives-reference/01-particle-effect-directives.md#tcoords) | `tcoords s1 t1 s2 t2 [tscale] [rsmax] [rsstep]` |
| [`atlas`](../41-particle-directives-reference/01-particle-effect-directives.md#atlas) | `atlas dims firstIndex [lastIndex]` |
| [`rotation`](../41-particle-directives-reference/01-particle-effect-directives.md#rotation) | `rotation startMin [startMax] speedMin [speedMax]` |
| [`beamtexstep`](../41-particle-directives-reference/01-particle-effect-directives.md#beamtexstep) | `beamtexstep unitsPerRepeat` |
| [`beamtexspeed`](../41-particle-directives-reference/01-particle-effect-directives.md#beamtexspeed) | `beamtexspeed scrollSpeed` |
| [`scale`](../41-particle-directives-reference/01-particle-effect-directives.md#scale) | `scale minSize [maxSize]` |
| [`scalefactor`](../41-particle-directives-reference/01-particle-effect-directives.md#scalefactor) | `scalefactor factor` |
| [`scaledelta`](../41-particle-directives-reference/01-particle-effect-directives.md#scaledelta) | `scaledelta unitsPerSecond` |
| [`stretchfactor`](../41-particle-directives-reference/01-particle-effect-directives.md#stretchfactor) | `stretchfactor factor [minFactor]` |
| [`count`](../41-particle-directives-reference/01-particle-effect-directives.md#count) | `count baseCount [randCount] [absoluteExtra]` |
| [`alpha`](../41-particle-directives-reference/01-particle-effect-directives.md#alpha) | `alpha baseAlpha [maxAlpha] [delta]` |
| [`alpharand`](../41-particle-directives-reference/01-particle-effect-directives.md#alpharand) | `alpharand range` |
| [`alphadelta`](../41-particle-directives-reference/01-particle-effect-directives.md#alphadelta) | `alphadelta unitsPerSecond` |
| [`die`](../41-particle-directives-reference/01-particle-effect-directives.md#die) | `die maxTime [minTime]` |
| [`assoc`](../41-particle-directives-reference/01-particle-effect-directives.md#assoc) | `assoc effectName` |
| [`colorindex`](../41-particle-directives-reference/01-particle-effect-directives.md#colorindex) | `colorindex paletteIndex [range]` |
| [`rgb`](../41-particle-directives-reference/01-particle-effect-directives.md#rgb) | `rgb r [g b]` |
| [`rgbdelta`](../41-particle-directives-reference/01-particle-effect-directives.md#rgbdelta) | `rgbdelta rDelta [gDelta bDelta]` |
| [`rgbrand`](../41-particle-directives-reference/01-particle-effect-directives.md#rgbrand) | `rgbrand rRange [gRange bRange]` |
| [`rgbrandsync`](../41-particle-directives-reference/01-particle-effect-directives.md#rgbrandsync) | `rgbrandsync rSync [gSync bSync]` |
| [`stains`](../41-particle-directives-reference/01-particle-effect-directives.md#stains) | `stains amount` |
| [`blend`](../41-particle-directives-reference/01-particle-effect-directives.md#blend) | `blend mode` |
| [`type`](../41-particle-directives-reference/01-particle-effect-directives.md#type) | `type renderType` |
| [`clippeddecal`](../41-particle-directives-reference/01-particle-effect-directives.md#clippeddecal) | `clippeddecal mask [match]` |
| [`cliptype`](../41-particle-directives-reference/01-particle-effect-directives.md#cliptype) | `cliptype effectName` |
| [`rampmode`](../41-particle-directives-reference/01-particle-effect-directives.md#rampmode) | `rampmode mode` |
| [`rampindex`](../41-particle-directives-reference/01-particle-effect-directives.md#rampindex) | `rampindex paletteIndex [alpha] [scale]` |
| [`ramp`](../41-particle-directives-reference/01-particle-effect-directives.md#ramp) | `ramp r [g b [alpha [scale]]]` |
| [`lightradius`](../41-particle-directives-reference/01-particle-effect-directives.md#lightradius) | `lightradius minRadius [maxRadius]` |
| [`lightradiusfade`](../41-particle-directives-reference/01-particle-effect-directives.md#lightradiusfade) | `lightradiusfade unitsPerSecond` |
| [`lightrgb`](../41-particle-directives-reference/01-particle-effect-directives.md#lightrgb) | `lightrgb r g b` |
| [`lightcorona`](../41-particle-directives-reference/01-particle-effect-directives.md#lightcorona) | `lightcorona intensity scale` |
| [`lighttime`](../41-particle-directives-reference/01-particle-effect-directives.md#lighttime) | `lighttime seconds` |
| [`spawnstain`](../41-particle-directives-reference/01-particle-effect-directives.md#spawnstain) | `spawnstain radius r g b` |

### Директивы поведения и появления

| Элемент | Сигнатура / Описание |
|---|---|
| [`randomvel`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#randomvel) | `randomvel horizontal [vertical]` |
| [`veladd`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#veladd) | `veladd base [max]` |
| [`orgadd`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#orgadd) | `orgadd base [max]` |
| [`orgbias`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#orgbias) | `orgbias x y z` |
| [`velbias`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#velbias) | `velbias x y z` |
| [`orgwrand`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#orgwrand) | `orgwrand x y z` |
| [`velwrand`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#velwrand) | `velwrand x y z` |
| [`friction`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#friction) | `friction xyz` |
| [`gravity`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#gravity) | `gravity value` |
| [`flurry`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#flurry) | `flurry value` |
| [`assoc`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#assoc) | `assoc effectName` |
| [`inwater`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#inwater) | `inwater effectName` |
| [`underwater`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#underwater) | `underwater [contents ...]` |
| [`notunderwater`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#notunderwater) | `notunderwater [contents ...]` |
| [`spawnmode`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#spawnmode) | `spawnmode mode [param1] [param2]` |
| [`spawntime`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#spawntime) | `spawntime seconds` |
| [`spawnchance`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#spawnchance) | `spawnchance chance` |
| [`step`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#step) | `step distance [randomDistance] [extraCount]` |
| [`cliptype`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#cliptype) | `cliptype effectName` |
| [`clipcount`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#clipcount) | `clipcount multiplier` |
| [`clipbounce`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#clipbounce) | `clipbounce value` |
| [`bounce`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#bounce) | `bounce value` |
| [`emit`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#emit) | `emit effectName` |
| [`emitinterval`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#emitinterval) | `emitinterval seconds` |
| [`emitintervalrand`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#emitintervalrand) | `emitintervalrand seconds` |
| [`emitstart`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#emitstart) | `emitstart seconds` |
| [`spawnorg`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#spawnorg) | `spawnorg horizontal [vertical]` |
| [`spawnvel`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#spawnvel) | `spawnvel horizontal [vertical]` |
| [`stretchfactor`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#stretchfactor) | `stretchfactor factor [minLength]` |
| [`spawnparam1`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#spawnparam1) | `spawnparam1 value` |
| [`spawnparam2`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#spawnparam2) | `spawnparam2 value` |
| [`up`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#up) | `up value` |
| [`viewspace`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#viewspace) | `viewspace [fraction]` |
| [`perframe`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#perframe) | `perframe` |
| [`averageout`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#averageout) | `averageout` |
| [`nostate`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#nostate) | `nostate` |
| [`nospreadfirst`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#nospreadfirst) | `nospreadfirst` |
| [`nospreadlast`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#nospreadlast) | `nospreadlast` |
| [`rainfrequency`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#rainfrequency) | `rainfrequency multiplier` |
| [`placeholder`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#placeholder) | `placeholder` |
