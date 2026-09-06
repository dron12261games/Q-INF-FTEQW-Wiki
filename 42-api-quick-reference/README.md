# Быстрая навигация по API (сводные таблицы)

> [⬅ Вернуться к оглавлению вики](../README.md)

Эта статья не объясняет ничего сама по себе — это чисто справочный, максимально плотный указатель по всем элементам API движка, задокументированным в разделах 37–41. Если вы уже примерно знаете, как называется нужная функция, cvar, ключ сущности или директива, но не помните деталей — ищите имя в соответствующей таблице (Ctrl+F) и переходите по ссылке в первом столбце: она ведёт прямо на разбор этого элемента с сигнатурой, описанием логики и примерами кода. Если вы не знаете, с чего начать, а хотите изучить тему последовательно — вернитесь в [оглавление вики](../README.md) и откройте один из разделов 37–41 целиком, а не эту страницу.

## Содержание

- [Встроенные функции QuakeC (builtins)](#встроенные-функции-quakec-builtins)
- [Переменные движка (cvar)](#переменные-движка-cvar)
- [Ключи сущностей карты (entity keys)](#ключи-сущностей-карты-entity-keys)
- [Директивы языка материалов (.shader)](#директивы-языка-материалов-shader)
- [Директивы языка частиц (.particles)](#директивы-языка-частиц-particles)

## Встроенные функции QuakeC (builtins)

Всего задокументировано: **744** builtin-функций и точек входа (включая точки входа SSQC/CSQC/MenuQC и отдельно посчитанные CSQC- и MenuQC-варианты одноимённых функций). Полный постатейный разбор — в разделе [«37. Встроенные функции QuakeC»](../37-quakec-builtins-reference/README.md).

| Функция | Сигнатура | Категория |
|---|---|---|
| [`SetNewParms`](../37-quakec-builtins-reference/00-entry-points.md#setnewparms) | `void() SetNewParms` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`SetChangeParms`](../37-quakec-builtins-reference/00-entry-points.md#setchangeparms) | `void() SetChangeParms` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`ClientConnect`](../37-quakec-builtins-reference/00-entry-points.md#clientconnect) | `void() ClientConnect` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`PutClientInServer`](../37-quakec-builtins-reference/00-entry-points.md#putclientinserver) | `void() PutClientInServer` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`ClientKill`](../37-quakec-builtins-reference/00-entry-points.md#clientkill) | `void() ClientKill` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`PlayerPreThink`](../37-quakec-builtins-reference/00-entry-points.md#playerprethink) | `void() PlayerPreThink` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`PlayerPostThink`](../37-quakec-builtins-reference/00-entry-points.md#playerpostthink) | `void() PlayerPostThink` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`StartFrame`](../37-quakec-builtins-reference/00-entry-points.md#startframe) | `void() StartFrame` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`EndFrame`](../37-quakec-builtins-reference/00-entry-points.md#endframe) | `void() EndFrame` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`ClientDisconnect`](../37-quakec-builtins-reference/00-entry-points.md#clientdisconnect) | `void() ClientDisconnect` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`main`](../37-quakec-builtins-reference/00-entry-points.md#main-устаревшая-не-вызывается) | `void() main` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_Init`](../37-quakec-builtins-reference/00-entry-points.md#csqc_init) | `void(float apilevel, string enginename, float engineversion) CSQC_Init` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_WorldLoaded`](../37-quakec-builtins-reference/00-entry-points.md#csqc_worldloaded) | `void() CSQC_WorldLoaded` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_UpdateView`](../37-quakec-builtins-reference/00-entry-points.md#csqc_updateview) | `void(float vwidth, float vheight, float notmenu) CSQC_UpdateView` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_InputEvent`](../37-quakec-builtins-reference/00-entry-points.md#csqc_inputevent) | `float(float evtype, float scanx, float chary, float devid) CSQC_InputEvent` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_ConsoleCommand`](../37-quakec-builtins-reference/00-entry-points.md#csqc_consolecommand) | `float(string cmd) CSQC_ConsoleCommand` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_Parse_StuffCmd`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_stuffcmd) | `void(string msg) CSQC_Parse_StuffCmd` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_Parse_CenterPrint`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_centerprint) | `float(string msg) CSQC_Parse_CenterPrint` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_Parse_Print`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_print) | `void(string printmsg, float printlvl) CSQC_Parse_Print` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_Ent_Update`](../37-quakec-builtins-reference/00-entry-points.md#csqc_ent_update) | `void(float isnew) CSQC_Ent_Update` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_Event_Sound`](../37-quakec-builtins-reference/00-entry-points.md#csqc_event_sound) | `float(float entnum, float channel, string soundname, float vol, float attenuation, vector pos, float pitchmod, float flags) CSQC_Event_Sound` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_Ent_Remove`](../37-quakec-builtins-reference/00-entry-points.md#csqc_ent_remove) | `void() CSQC_Ent_Remove` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_Shutdown`](../37-quakec-builtins-reference/00-entry-points.md#csqc_shutdown) | `void() CSQC_Shutdown` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_UpdateViewLoading`](../37-quakec-builtins-reference/00-entry-points.md#csqc_updateviewloading) | `void(float vwidth, float vheight, float notmenu) CSQC_UpdateViewLoading` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_DrawHud`](../37-quakec-builtins-reference/00-entry-points.md#csqc_drawhud) | `void(vector viewsize, float scoresshown) CSQC_DrawHud` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_DrawScores`](../37-quakec-builtins-reference/00-entry-points.md#csqc_drawscores) | `void(vector viewsize, float scoresshown) CSQC_DrawScores` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_Parse_Event`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_event) | `void() CSQC_Parse_Event` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_Parse_Damage`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_damage) | `float(float save, float take, vector inflictororg) CSQC_Parse_Damage` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_Parse_SetAngles`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_setangles) | `float(vector angles, float isdelta) CSQC_Parse_SetAngles` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_PlayerInfoChanged`](../37-quakec-builtins-reference/00-entry-points.md#csqc_playerinfochanged) | `void(float playernum) CSQC_PlayerInfoChanged` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_ServerInfoChanged`](../37-quakec-builtins-reference/00-entry-points.md#csqc_serverinfochanged) | `void() CSQC_ServerInfoChanged` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_Input_Frame`](../37-quakec-builtins-reference/00-entry-points.md#csqc_input_frame) | `void() CSQC_Input_Frame` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_RendererRestarted`](../37-quakec-builtins-reference/00-entry-points.md#csqc_rendererrestarted) | `void(string rendererdescription) CSQC_RendererRestarted` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_GenerateMaterial`](../37-quakec-builtins-reference/00-entry-points.md#csqc_generatematerial) | `string(string shadername) CSQC_GenerateMaterial` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_ConsoleLink`](../37-quakec-builtins-reference/00-entry-points.md#csqc_consolelink) | `float(string text, string info) CSQC_ConsoleLink` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_Ent_Spawn`](../37-quakec-builtins-reference/00-entry-points.md#csqc_ent_spawn) | `void(float newentnum) CSQC_Ent_Spawn` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_ServerSound`](../37-quakec-builtins-reference/00-entry-points.md#csqc_serversound) | `float(float channel, string soundname, vector pos, float vol, float attenuation, float flags) CSQC_ServerSound` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_Parse_TempEntity`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_tempentity) | `float() CSQC_Parse_TempEntity` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`CSQC_MapEntityEdited`](../37-quakec-builtins-reference/00-entry-points.md#csqc_mapentityedited) | `void(int entidx, string newentdata) CSQC_MapEntityEdited` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`m_init`](../37-quakec-builtins-reference/00-entry-points.md#m_init) | `void() m_init` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`m_shutdown`](../37-quakec-builtins-reference/00-entry-points.md#m_shutdown) | `void() m_shutdown` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`m_toggle`](../37-quakec-builtins-reference/00-entry-points.md#m_toggle) | `void(float show) m_toggle` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`m_draw`](../37-quakec-builtins-reference/00-entry-points.md#m_draw) | `void() m_draw` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`m_drawloading`](../37-quakec-builtins-reference/00-entry-points.md#m_drawloading) | `void() m_drawloading` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`m_keydown`](../37-quakec-builtins-reference/00-entry-points.md#m_keydown) | `float(float key, float char) m_keydown` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`m_keyup`](../37-quakec-builtins-reference/00-entry-points.md#m_keyup) | `float(float key, float char) m_keyup` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`Menu_InputEvent`](../37-quakec-builtins-reference/00-entry-points.md#menu_inputevent) | `float(float evtype, float scanx, float chary, float devid) Menu_InputEvent` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`m_consolecommand`](../37-quakec-builtins-reference/00-entry-points.md#m_consolecommand) | `float(string cmd) m_consolecommand` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`m_gethostcachecategory`](../37-quakec-builtins-reference/00-entry-points.md#m_gethostcachecategory) | `float(float hostcachenum) m_gethostcachecategory` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`Menu_RendererRestarted`](../37-quakec-builtins-reference/00-entry-points.md#menu_rendererrestarted) | `void(string rendererdescription) Menu_RendererRestarted` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`GameCommand`](../37-quakec-builtins-reference/00-entry-points.md#gamecommand) | `float(string cmd) GameCommand` | Точки входа QuakeC: SSQC, CSQC и MenuQC |
| [`acos`](../37-quakec-builtins-reference/01-math-vector-builtins.md#acos) | `float(float c) acos = #472;` | Математика и работа с векторами |
| [`asin`](../37-quakec-builtins-reference/01-math-vector-builtins.md#asin) | `float(float s) asin = #471;` | Математика и работа с векторами |
| [`atan`](../37-quakec-builtins-reference/01-math-vector-builtins.md#atan) | `float(float t) atan = #473;` | Математика и работа с векторами |
| [`atan2`](../37-quakec-builtins-reference/01-math-vector-builtins.md#atan2) | `float(float c, float s) atan2 = #474;` | Математика и работа с векторами |
| [`tan`](../37-quakec-builtins-reference/01-math-vector-builtins.md#tan) | `float(float a) tan = #475;` | Математика и работа с векторами |
| [`sin`](../37-quakec-builtins-reference/01-math-vector-builtins.md#sin) | `float(float angle) sin = #60;` | Математика и работа с векторами |
| [`cos`](../37-quakec-builtins-reference/01-math-vector-builtins.md#cos) | `float(float angle) cos = #61;` | Математика и работа с векторами |
| [`sqrt`](../37-quakec-builtins-reference/01-math-vector-builtins.md#sqrt) | `float(float value) sqrt = #62;` | Математика и работа с векторами |
| [`pow`](../37-quakec-builtins-reference/01-math-vector-builtins.md#pow) | `float(float value, float exp) pow = #97;` | Математика и работа с векторами |
| [`log`](../37-quakec-builtins-reference/01-math-vector-builtins.md#log) | `float(float v, optional float base) log = #532;` | Математика и работа с векторами |
| [`rint`](../37-quakec-builtins-reference/01-math-vector-builtins.md#rint) | `float(float value) rint = #36;` | Математика и работа с векторами |
| [`ceil`](../37-quakec-builtins-reference/01-math-vector-builtins.md#ceil) | `float(float value) ceil = #38;` | Математика и работа с векторами |
| [`floor`](../37-quakec-builtins-reference/01-math-vector-builtins.md#floor) | `float(float value) floor = #37;` | Математика и работа с векторами |
| [`fabs`](../37-quakec-builtins-reference/01-math-vector-builtins.md#fabs) | `float(float value) fabs = #43;` | Математика и работа с векторами |
| [`bound`](../37-quakec-builtins-reference/01-math-vector-builtins.md#bound) | `float(float minimum, float val, float maximum) bound = #96;` | Математика и работа с векторами |
| [`min`](../37-quakec-builtins-reference/01-math-vector-builtins.md#min) | `float(float a, float b, ...) min = #94;` | Математика и работа с векторами |
| [`max`](../37-quakec-builtins-reference/01-math-vector-builtins.md#max) | `float(float a, float b, ...) max = #95;` | Математика и работа с векторами |
| [`mod`](../37-quakec-builtins-reference/01-math-vector-builtins.md#mod) | `float(float dividend, float divisor) mod = #245;` | Математика и работа с векторами |
| [`random`](../37-quakec-builtins-reference/01-math-vector-builtins.md#random) | `float() random = #7;` | Математика и работа с векторами |
| [`randomvec`](../37-quakec-builtins-reference/01-math-vector-builtins.md#randomvec) | `vector() randomvec = #91;` | Математика и работа с векторами |
| [`randomvector`](../37-quakec-builtins-reference/01-math-vector-builtins.md#randomvector) | `vector() randomvector = #41;` | Математика и работа с векторами |
| [`bitshift`](../37-quakec-builtins-reference/01-math-vector-builtins.md#bitshift) | `float(float number, float quantity) bitshift = #218;` | Математика и работа с векторами |
| [`anglemod`](../37-quakec-builtins-reference/01-math-vector-builtins.md#anglemod) | `float(float value) anglemod = #102;` | Математика и работа с векторами |
| [`changepitch`](../37-quakec-builtins-reference/01-math-vector-builtins.md#changepitch) | `void(entity ent) changepitch = #63;` | Математика и работа с векторами |
| [`changeyaw`](../37-quakec-builtins-reference/01-math-vector-builtins.md#changeyaw) | `void() changeyaw = #49;` | Математика и работа с векторами |
| [`normalize`](../37-quakec-builtins-reference/01-math-vector-builtins.md#normalize) | `vector(vector v) normalize = #9;` | Математика и работа с векторами |
| [`vlen`](../37-quakec-builtins-reference/01-math-vector-builtins.md#vlen) | `float(vector v) vlen = #12;` | Математика и работа с векторами |
| [`vtos`](../37-quakec-builtins-reference/01-math-vector-builtins.md#vtos) | `string(vector val) vtos = #27;` | Математика и работа с векторами |
| [`vectoangles`](../37-quakec-builtins-reference/01-math-vector-builtins.md#vectoangles) | `vector(vector fwd, optional vector up) vectoangles = #51;` | Математика и работа с векторами |
| [`vectoyaw`](../37-quakec-builtins-reference/01-math-vector-builtins.md#vectoyaw) | `float(vector v, optional entity reference) vectoyaw = #13;` | Математика и работа с векторами |
| [`makevectors`](../37-quakec-builtins-reference/01-math-vector-builtins.md#makevectors) | `void(vector vang) makevectors = #1;` | Математика и работа с векторами |
| [`vectorvectors`](../37-quakec-builtins-reference/01-math-vector-builtins.md#vectorvectors) | `void(vector dir) vectorvectors = #432;` | Математика и работа с векторами |
| [`rotatevectorsbyangle`](../37-quakec-builtins-reference/01-math-vector-builtins.md#rotatevectorsbyangle) | `void(vector angle) rotatevectorsbyangle = #235;` | Математика и работа с векторами |
| [`rotatevectorsbyvectors`](../37-quakec-builtins-reference/01-math-vector-builtins.md#rotatevectorsbyvectors) | `void(vector fwd, vector right, vector up) rotatevectorsbyvectors = #236;` | Математика и работа с векторами |
| [`rotatevectorsbytag`](../37-quakec-builtins-reference/01-math-vector-builtins.md#rotatevectorsbytag) | `vector(entity ent, float tagnum) rotatevectorsbytag = #244;` | Математика и работа с векторами |
| [`project`](../37-quakec-builtins-reference/01-math-vector-builtins.md#project) | `vector(vector v) project = #311;` | Математика и работа с векторами |
| [`unproject`](../37-quakec-builtins-reference/01-math-vector-builtins.md#unproject) | `vector(vector v) unproject = #310;` | Математика и работа с векторами |
| [`crc16`](../37-quakec-builtins-reference/01-math-vector-builtins.md#crc16) | `__deprecated("Use digest_hex") float(float caseinsensitive, string s, ...) crc16 = #494;` | Математика и работа с векторами |
| [`htos`](../37-quakec-builtins-reference/01-math-vector-builtins.md#htos) | `string(int value) htos = #262;` | Математика и работа с векторами |
| [`itos`](../37-quakec-builtins-reference/01-math-vector-builtins.md#itos) | `string(int value) itos = #260;` | Математика и работа с векторами |
| [`stoi`](../37-quakec-builtins-reference/01-math-vector-builtins.md#stoi) | `int(string s) stoi = #259;` | Математика и работа с векторами |
| [`stoh`](../37-quakec-builtins-reference/01-math-vector-builtins.md#stoh) | `int(string s) stoh = #261;` | Математика и работа с векторами |
| [`str2chr`](../37-quakec-builtins-reference/01-math-vector-builtins.md#str2chr) | `float(string str, float index) str2chr = #222;` | Математика и работа с векторами |
| [`chr2str`](../37-quakec-builtins-reference/01-math-vector-builtins.md#chr2str) | `string(float chr, ...) chr2str = #223;` | Математика и работа с векторами |
| [`anglesub`](../37-quakec-builtins-reference/01-math-vector-builtins.md#anglesub) | `anglesub(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Математика и работа с векторами |
| [`crossproduct`](../37-quakec-builtins-reference/01-math-vector-builtins.md#crossproduct) | `crossproduct(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Математика и работа с векторами |
| [`ftoi`](../37-quakec-builtins-reference/01-math-vector-builtins.md#ftoi) | `ftoi(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Математика и работа с векторами |
| [`ftou`](../37-quakec-builtins-reference/01-math-vector-builtins.md#ftou) | `ftou(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Математика и работа с векторами |
| [`itof`](../37-quakec-builtins-reference/01-math-vector-builtins.md#itof) | `itof(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Математика и работа с векторами |
| [`logarithm`](../37-quakec-builtins-reference/01-math-vector-builtins.md#logarithm) | `logarithm(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Математика и работа с векторами |
| [`utof`](../37-quakec-builtins-reference/01-math-vector-builtins.md#utof) | `utof(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Математика и работа с векторами |
| [`strlen`](../37-quakec-builtins-reference/02-string-builtins.md#strlen) | `float(string s) strlen = #114;` | Строки и текст |
| [`strlennocol`](../37-quakec-builtins-reference/02-string-builtins.md#strlennocol) | `float(string s) strlennocol = #476;` | Строки и текст |
| [`strcat`](../37-quakec-builtins-reference/02-string-builtins.md#strcat) | `string(string s1, optional string s2, optional string s3, optional string s4, optional string s5, optional string s6, optional string s7, optional string s8) strcat = #115;` | Строки и текст |
| [`substring`](../37-quakec-builtins-reference/02-string-builtins.md#substring) | `string(string s, float start, float length) substring = #116;` | Строки и текст |
| [`stov`](../37-quakec-builtins-reference/02-string-builtins.md#stov) | `vector(string s) stov = #117;` | Строки и текст |
| [`strzone`](../37-quakec-builtins-reference/02-string-builtins.md#strzone) | `string(string s, ...) strzone = #118;` | Строки и текст |
| [`strunzone`](../37-quakec-builtins-reference/02-string-builtins.md#strunzone) | `void(string s) strunzone = #119;` | Строки и текст |
| [`strcasecmp`](../37-quakec-builtins-reference/02-string-builtins.md#strcasecmp) | `float(string s1, string s2) strcasecmp = #229;` | Строки и текст |
| [`strncasecmp`](../37-quakec-builtins-reference/02-string-builtins.md#strncasecmp) | `float(string s1, string s2, float len, optional float s1ofs, optional float s2ofs) strncasecmp = #230;` | Строки и текст |
| [`strncmp`](../37-quakec-builtins-reference/02-string-builtins.md#strncmp) | `float(string s1, string s2, optional float len, optional float s1ofs, optional float s2ofs) strncmp = #228;` | Строки и текст |
| [`strstrofs`](../37-quakec-builtins-reference/02-string-builtins.md#strstrofs) | `float(string s1, string sub, optional float startidx) strstrofs = #221;` | Строки и текст |
| [`strtolower`](../37-quakec-builtins-reference/02-string-builtins.md#strtolower) | `string(string s) strtolower = #480;` | Строки и текст |
| [`strtoupper`](../37-quakec-builtins-reference/02-string-builtins.md#strtoupper) | `string(string s) strtoupper = #481;` | Строки и текст |
| [`strreplace`](../37-quakec-builtins-reference/02-string-builtins.md#strreplace) | `string(string search, string replace, string subject) strreplace = #484;` | Строки и текст |
| [`strireplace`](../37-quakec-builtins-reference/02-string-builtins.md#strireplace) | `string(string search, string replace, string subject) strireplace = #485;` | Строки и текст |
| [`strpad`](../37-quakec-builtins-reference/02-string-builtins.md#strpad) | `string(float pad, string str1, ...) strpad = #225;` | Строки и текст |
| [`strconv`](../37-quakec-builtins-reference/02-string-builtins.md#strconv) | `string(float ccase, float redalpha, float redchars, string str, ...) strconv = #224;` | Строки и текст |
| [`strdecolorize`](../37-quakec-builtins-reference/02-string-builtins.md#strdecolorize) | `string(string s) strdecolorize = #477;` | Строки и текст |
| [`strftime`](../37-quakec-builtins-reference/02-string-builtins.md#strftime) | `string(float uselocaltime, string format, ...) strftime = #478;` | Строки и текст |
| [`sprintf`](../37-quakec-builtins-reference/02-string-builtins.md#sprintf) | `string(string fmt, ...) sprintf = #627;` | Строки и текст |
| [`tokenize`](../37-quakec-builtins-reference/02-string-builtins.md#tokenize) | `float(string s) tokenize = #441;` | Строки и текст |
| [`tokenize_console`](../37-quakec-builtins-reference/02-string-builtins.md#tokenize_console) | `float(string str) tokenize_console = #514;` | Строки и текст |
| [`tokenizebyseparator`](../37-quakec-builtins-reference/02-string-builtins.md#tokenizebyseparator) | `float(string s, string separator1, ...) tokenizebyseparator = #479;` | Строки и текст |
| [`argv`](../37-quakec-builtins-reference/02-string-builtins.md#argv) | `string(float n) argv = #442;` | Строки и текст |
| [`argv_start_index`](../37-quakec-builtins-reference/02-string-builtins.md#argv_start_index) | `float(float idx) argv_start_index = #515;` | Строки и текст |
| [`argv_end_index`](../37-quakec-builtins-reference/02-string-builtins.md#argv_end_index) | `float(float idx) argv_end_index = #516;` | Строки и текст |
| [`validstring`](../37-quakec-builtins-reference/02-string-builtins.md#validstring) | `float(string str) validstring = #81;` | Строки и текст |
| [`ftos`](../37-quakec-builtins-reference/02-string-builtins.md#ftos) | `string(float val) ftos = #26;` | Строки и текст |
| [`stof`](../37-quakec-builtins-reference/02-string-builtins.md#stof) | `float(string s) stof = #81;` | Строки и текст |
| [`altstr_count`](../37-quakec-builtins-reference/02-string-builtins.md#altstr_count) | `float(string str) altstr_count = #82;` | Строки и текст |
| [`altstr_get`](../37-quakec-builtins-reference/02-string-builtins.md#altstr_get) | `string(string str, float num) altstr_get = #84;` | Строки и текст |
| [`altstr_prepare`](../37-quakec-builtins-reference/02-string-builtins.md#altstr_prepare) | `string(string str) altstr_prepare = #83;` | Строки и текст |
| [`altstr_set`](../37-quakec-builtins-reference/02-string-builtins.md#altstr_set) | `string(string str, float num, string setval) altstr_set = #85;` | Строки и текст |
| [`uri_escape`](../37-quakec-builtins-reference/02-string-builtins.md#uri_escape) | `string(string in) uri_escape = #510;` | Строки и текст |
| [`uri_unescape`](../37-quakec-builtins-reference/02-string-builtins.md#uri_unescape) | `string(string in) uri_unescape = #511;` | Строки и текст |
| [`argescape`](../37-quakec-builtins-reference/02-string-builtins.md#argescape) | `string(string s) argescape = #295;` | Строки и текст |
| [`stringwidth`](../37-quakec-builtins-reference/02-string-builtins.md#stringwidth) | `float(string text, float usecolours, optional vector fontsize) stringwidth = #327;` | Строки и текст |
| [`stringtokeynum`](../37-quakec-builtins-reference/02-string-builtins.md#stringtokeynum) | `float(string keyname) stringtokeynum = #341;` | Строки и текст |
| [`str2chr`](../37-quakec-builtins-reference/02-string-builtins.md#str2chr) | `float(string str, float index) str2chr = #222;` | Строки и текст |
| [`chr2str`](../37-quakec-builtins-reference/02-string-builtins.md#chr2str) | `string(float chr, ...) chr2str = #223;` | Строки и текст |
| [`altstr_ins`](../37-quakec-builtins-reference/02-string-builtins.md#altstr_ins) | `altstr_ins(...)` — сигнатура в си-таблице движка не хранится текстом (номера: MenuQC=#86).` | Строки и текст |
| [`base64decode`](../37-quakec-builtins-reference/02-string-builtins.md#base64decode) | `base64decode(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Строки и текст |
| [`base64encode`](../37-quakec-builtins-reference/02-string-builtins.md#base64encode) | `base64encode(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Строки и текст |
| [`instr`](../37-quakec-builtins-reference/02-string-builtins.md#instr) | `instr(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#206).` | Строки и текст |
| [`matchpattern`](../37-quakec-builtins-reference/02-string-builtins.md#matchpattern) | `float(string s, string pattern, float matchrule) matchpattern = #538;` | Строки и текст |
| [`strcmp`](../37-quakec-builtins-reference/02-string-builtins.md#strcmp) | `strcmp(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#228, MenuQC=#228).` | Строки и текст |
| [`strtrim`](../37-quakec-builtins-reference/02-string-builtins.md#strtrim) | `strtrim(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Строки и текст |
| [`spawn`](../37-quakec-builtins-reference/03-entity-world-builtins.md#spawn) | `entity() spawn = #14;` | Сущности и игровой мир |
| [`remove`](../37-quakec-builtins-reference/03-entity-world-builtins.md#remove) | `void(entity e) remove = #15;` | Сущности и игровой мир |
| [`find`](../37-quakec-builtins-reference/03-entity-world-builtins.md#find) | `entity(entity start, .string fld, string match) find = #18;` | Сущности и игровой мир |
| [`findchain`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findchain) | `entity(.string field, string match, optional .entity chainfield) findchain = #402;` | Сущности и игровой мир |
| [`findchainflags`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findchainflags) | `entity(.float fld, float match, optional .entity chainfield) findchainflags = #450;` | Сущности и игровой мир |
| [`findchainfloat`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findchainfloat) | `entity(.float fld, float match, optional .entity chainfield) findchainfloat = #403;` | Сущности и игровой мир |
| [`findflags`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findflags) | `entity(entity start, .float field, float match) findflags = #449;` | Сущности и игровой мир |
| [`findfloat`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findfloat) | `entity(entity start, .__variant fld, __variant match) findfloat = #98;` | Сущности и игровой мир |
| [`findradius`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findradius) | `entity(vector org, float rad, optional .entity chainfield) findradius = #22;` | Сущности и игровой мир |
| [`nextent`](../37-quakec-builtins-reference/03-entity-world-builtins.md#nextent) | `entity(entity e) nextent = #47;` | Сущности и игровой мир |
| [`setmodel`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setmodel) | `void(entity e, string m) setmodel = #3;` | Сущности и игровой мир |
| [`setmodelindex`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setmodelindex) | `void(entity e, float mdlindex) setmodelindex = #333;` | Сущности и игровой мир |
| [`setorigin`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setorigin) | `void(entity e, vector o) setorigin = #2;` | Сущности и игровой мир |
| [`setsize`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setsize) | `void(entity e, vector min, vector max) setsize = #4;` | Сущности и игровой мир |
| [`checkbottom`](../37-quakec-builtins-reference/03-entity-world-builtins.md#checkbottom) | `float(entity ent) checkbottom = #40;` | Сущности и игровой мир |
| [`droptofloor`](../37-quakec-builtins-reference/03-entity-world-builtins.md#droptofloor) | `float() droptofloor = #34;` | Сущности и игровой мир |
| [`walkmove`](../37-quakec-builtins-reference/03-entity-world-builtins.md#walkmove) | `float(float yaw, float dist, optional float settraceglobals) walkmove = #32;` | Сущности и игровой мир |
| [`movetogoal`](../37-quakec-builtins-reference/03-entity-world-builtins.md#movetogoal) | `void(float step) movetogoal = #67;` | Сущности и игровой мир |
| [`touchtriggers`](../37-quakec-builtins-reference/03-entity-world-builtins.md#touchtriggers) | `void(optional entity ent, optional vector neworigin) touchtriggers = #279;` | Сущности и игровой мир |
| [`pointcontents`](../37-quakec-builtins-reference/03-entity-world-builtins.md#pointcontents) | `float(vector pos) pointcontents = #41;` | Сущности и игровой мир |
| [`checkclient`](../37-quakec-builtins-reference/03-entity-world-builtins.md#checkclient) | `entity() checkclient = #17;` | Сущности и игровой мир |
| [`checkpvs`](../37-quakec-builtins-reference/03-entity-world-builtins.md#checkpvs) | `float(vector viewpos, entity entity) checkpvs = #240;` | Сущности и игровой мир |
| [`num_for_edict`](../37-quakec-builtins-reference/03-entity-world-builtins.md#num_for_edict) | `float(entity ent) num_for_edict = #512;` | Сущности и игровой мир |
| [`edict_num`](../37-quakec-builtins-reference/03-entity-world-builtins.md#edict_num) | `entity(float entnum) edict_num = #459;` | Сущности и игровой мир |
| [`etof`](../37-quakec-builtins-reference/03-entity-world-builtins.md#etof) | `float(entity e) etof = #79;` | Сущности и игровой мир |
| [`ftoe`](../37-quakec-builtins-reference/03-entity-world-builtins.md#ftoe) | `entity(float f) ftoe = #80;` | Сущности и игровой мир |
| [`etos`](../37-quakec-builtins-reference/03-entity-world-builtins.md#etos) | `string(entity ent) etos = #65;` | Сущности и игровой мир |
| [`wasfreed`](../37-quakec-builtins-reference/03-entity-world-builtins.md#wasfreed) | `float(entity ent) wasfreed = #353;` | Сущности и игровой мир |
| [`copyentity`](../37-quakec-builtins-reference/03-entity-world-builtins.md#copyentity) | `entity(entity from, optional entity to) copyentity = #400;` | Сущности и игровой мир |
| [`aim`](../37-quakec-builtins-reference/03-entity-world-builtins.md#aim) | `vector(entity player, float missilespeed) aim = #44;` | Сущности и игровой мир |
| [`traceline`](../37-quakec-builtins-reference/03-entity-world-builtins.md#traceline) | `void(vector v1, vector v2, float flags, entity ent) traceline = #16;` | Сущности и игровой мир |
| [`tracebox`](../37-quakec-builtins-reference/03-entity-world-builtins.md#tracebox) | `void(vector start, vector mins, vector maxs, vector end, float nomonsters, entity ent) tracebox = #90;` | Сущности и игровой мир |
| [`tracetoss`](../37-quakec-builtins-reference/03-entity-world-builtins.md#tracetoss) | `void(entity ent, entity ignore) tracetoss = #64;` | Сущности и игровой мир |
| [`traceon`](../37-quakec-builtins-reference/03-entity-world-builtins.md#traceon) | `void() traceon = #29;` | Сущности и игровой мир |
| [`traceoff`](../37-quakec-builtins-reference/03-entity-world-builtins.md#traceoff) | `void() traceoff = #30;` | Сущности и игровой мир |
| [`makestatic`](../37-quakec-builtins-reference/03-entity-world-builtins.md#makestatic) | `void(entity e) makestatic = #69;` | Сущности и игровой мир |
| [`setspawnparms`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setspawnparms) | `void(entity player) setspawnparms = #78;` | Сущности и игровой мир |
| [`spawnclient`](../37-quakec-builtins-reference/03-entity-world-builtins.md#spawnclient) | `entity() spawnclient = #454;` | Сущности и игровой мир |
| [`dropclient`](../37-quakec-builtins-reference/03-entity-world-builtins.md#dropclient) | `void(entity player) dropclient = #453;` | Сущности и игровой мир |
| [`runstandardplayerphysics`](../37-quakec-builtins-reference/03-entity-world-builtins.md#runstandardplayerphysics) | `void(entity ent) runstandardplayerphysics = #347;` | Сущности и игровой мир |
| [`getstati`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getstati) | `int(float stnum) getstati = #330;` | Сущности и игровой мир |
| [`getstatf`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getstatf) | `float(float stnum, optional float firstbit, optional float bitcount) getstatf = #331;` | Сущности и игровой мир |
| [`getstats`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getstats) | `string(float stnum) getstats = #332;` | Сущности и игровой мир |
| [`clientstat`](../37-quakec-builtins-reference/03-entity-world-builtins.md#clientstat) | `void(float num, float type, .__variant fld) clientstat = #232;` | Сущности и игровой мир |
| [`globalstat`](../37-quakec-builtins-reference/03-entity-world-builtins.md#globalstat) | `void(float num, float type, string name) globalstat = #233;` | Сущности и игровой мир |
| [`forceinfokey`](../37-quakec-builtins-reference/03-entity-world-builtins.md#forceinfokey) | `void(entity player, string key, string value) forceinfokey = #213;` | Сущности и игровой мир |
| [`serverkey`](../37-quakec-builtins-reference/03-entity-world-builtins.md#serverkey) | `string(string key) serverkey = #354;` | Сущности и игровой мир |
| [`infokey`](../37-quakec-builtins-reference/03-entity-world-builtins.md#infokey) | `string(entity e, string key) infokey = #80;` | Сущности и игровой мир |
| [`infoadd`](../37-quakec-builtins-reference/03-entity-world-builtins.md#infoadd) | `infostring(infostring old, string key, string value) infoadd = #226;` | Сущности и игровой мир |
| [`infoget`](../37-quakec-builtins-reference/03-entity-world-builtins.md#infoget) | `string(infostring info, string key) infoget = #227;` | Сущности и игровой мир |
| [`matchclientname`](../37-quakec-builtins-reference/03-entity-world-builtins.md#matchclientname) | `entity(string match, optional float matchnum) matchclientname = #241;` | Сущности и игровой мир |
| [`entityfieldname`](../37-quakec-builtins-reference/03-entity-world-builtins.md#entityfieldname) | `string(float fieldnum) entityfieldname = #497;` | Сущности и игровой мир |
| [`entityfieldtype`](../37-quakec-builtins-reference/03-entity-world-builtins.md#entityfieldtype) | `float(float fieldnum) entityfieldtype = #498;` | Сущности и игровой мир |
| [`numentityfields`](../37-quakec-builtins-reference/03-entity-world-builtins.md#numentityfields) | `float() numentityfields = #496;` | Сущности и игровой мир |
| [`getentityfieldstring`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getentityfieldstring) | `string(float fieldnum, entity ent) getentityfieldstring = #499;` | Сущности и игровой мир |
| [`putentityfieldstring`](../37-quakec-builtins-reference/03-entity-world-builtins.md#putentityfieldstring) | `float(float fieldnum, entity ent, string s) putentityfieldstring = #500;` | Сущности и игровой мир |
| [`getentitytoken`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getentitytoken) | `string(optional string resetstring) getentitytoken = #355;` | Сущности и игровой мир |
| [`parseentitydata`](../37-quakec-builtins-reference/03-entity-world-builtins.md#parseentitydata) | `float(entity e, string s, optional float offset) parseentitydata = #613;` | Сущности и игровой мир |
| [`getentity`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getentity) | `__variant(float entnum, float fieldnum) getentity = #504;` | Сущности и игровой мир |
| [`resourcestatus`](../37-quakec-builtins-reference/03-entity-world-builtins.md#resourcestatus) | `float(float resourcetype, float tryload, string resourcename) resourcestatus = #286;` | Сущности и игровой мир |
| [`physics_addforce`](../37-quakec-builtins-reference/03-entity-world-builtins.md#physics_addforce) | `void(entity e, vector force, vector relative_ofs) physics_addforce = #541;` | Сущности и игровой мир |
| [`physics_addtorque`](../37-quakec-builtins-reference/03-entity-world-builtins.md#physics_addtorque) | `void(entity e, vector torque) physics_addtorque = #542;` | Сущности и игровой мир |
| [`physics_enable`](../37-quakec-builtins-reference/03-entity-world-builtins.md#physics_enable) | `void(entity e, float physics_enabled) physics_enable = #540;` | Сущности и игровой мир |
| [`terrain_edit`](../37-quakec-builtins-reference/03-entity-world-builtins.md#terrain_edit) | `__variant(float action, optional vector pos, optional float radius, optional float quant, ...) terrain_edit = #278;` | Сущности и игровой мир |
| [`setattachment`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setattachment) | `void(entity e, entity tagentity, string tagname) setattachment = #443;` | Сущности и игровой мир |
| [`checkcommand`](../37-quakec-builtins-reference/03-entity-world-builtins.md#checkcommand) | `float(string name) checkcommand = #294;` | Сущности и игровой мир |
| [`registercommand`](../37-quakec-builtins-reference/03-entity-world-builtins.md#registercommand) | `void(string cmdname, optional string desc) registercommand = #352;` | Сущности и игровой мир |
| [`isfunction`](../37-quakec-builtins-reference/03-entity-world-builtins.md#isfunction) | `float(string s) isfunction = #607;` | Сущности и игровой мир |
| [`callfunction`](../37-quakec-builtins-reference/03-entity-world-builtins.md#callfunction) | `void(.../*, string funcname*/) callfunction = #605;` | Сущности и игровой мир |
| [`externcall`](../37-quakec-builtins-reference/03-entity-world-builtins.md#externcall) | `__variant(float prnum, string funcname, ...) externcall = #201;` | Сущности и игровой мир |
| [`externset`](../37-quakec-builtins-reference/03-entity-world-builtins.md#externset) | `void(float prnum, __variant newval, string varname) externset = #204;` | Сущности и игровой мир |
| [`externvalue`](../37-quakec-builtins-reference/03-entity-world-builtins.md#externvalue) | `__variant(float prnum, string varname) externvalue = #203;` | Сущности и игровой мир |
| [`builtin_find`](../37-quakec-builtins-reference/03-entity-world-builtins.md#builtin_find) | `float(string builtinname) builtin_find = #100;` | Сущности и игровой мир |
| [`changelevel`](../37-quakec-builtins-reference/03-entity-world-builtins.md#changelevel) | `changelevel(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#70, MenuQC=#64).` | Сущности и игровой мир |
| [`chat`](../37-quakec-builtins-reference/03-entity-world-builtins.md#chat) | `void(string filename, float starttag, entity edict) chat = #214;` | Сущности и игровой мир |
| [`empty`](../37-quakec-builtins-reference/03-entity-world-builtins.md#empty) | `void() empty = #249;` | Сущности и игровой мир |
| [`entityfieldref`](../37-quakec-builtins-reference/03-entity-world-builtins.md#entityfieldref) | `entityfieldref(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Сущности и игровой мир |
| [`entityprotection`](../37-quakec-builtins-reference/03-entity-world-builtins.md#entityprotection) | `entityprotection(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Сущности и игровой мир |
| [`eprint`](../37-quakec-builtins-reference/03-entity-world-builtins.md#eprint) | `void(entity) eprint = #33;` | Сущности и игровой мир |
| [`find_list`](../37-quakec-builtins-reference/03-entity-world-builtins.md#find_list) | `find_list(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Сущности и игровой мир |
| [`findentity`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findentity) | `findentity(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#98, MenuQC=#25).` | Сущности и игровой мир |
| [`findentityfield`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findentityfield) | `findentityfield(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Сущности и игровой мир |
| [`findradius_list`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findradius_list) | `findradius_list(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Сущности и игровой мир |
| [`generateentitydata`](../37-quakec-builtins-reference/03-entity-world-builtins.md#generateentitydata) | `generateentitydata(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Сущности и игровой мир |
| [`plaque_draw`](../37-quakec-builtins-reference/03-entity-world-builtins.md#plaque_draw) | `void(entity targ, float stringno) plaque_draw = #0;` | Сущности и игровой мир |
| [`pushmove`](../37-quakec-builtins-reference/03-entity-world-builtins.md#pushmove) | `float(entity pusher, vector move, vector amove) pushmove = #0;` | Сущности и игровой мир |
| [`qtest_canreach`](../37-quakec-builtins-reference/03-entity-world-builtins.md#qtest_canreach) | `DEP float(vector v) qtest_canreach = #0;` | Сущности и игровой мир |
| [`readserverentitystate`](../37-quakec-builtins-reference/03-entity-world-builtins.md#readserverentitystate) | `void(float flags, float simtime) readserverentitystate = #369;` | Сущности и игровой мир |
| [`readsingleentitystate`](../37-quakec-builtins-reference/03-entity-world-builtins.md#readsingleentitystate) | `readsingleentitystate(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#370).` | Сущности и игровой мир |
| [`removeentity`](../37-quakec-builtins-reference/03-entity-world-builtins.md#removeentity) | `removeentity(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Сущности и игровой мир |
| [`route_calculate`](../37-quakec-builtins-reference/03-entity-world-builtins.md#route_calculate) | `route_calculate(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Сущности и игровой мир |
| [`runclientphys`](../37-quakec-builtins-reference/03-entity-world-builtins.md#runclientphys) | `runclientphys(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#233).` | Сущности и игровой мир |
| [`te_gunshotquad`](../37-quakec-builtins-reference/03-entity-world-builtins.md#te_gunshotquad) | `void(vector org) te_gunshotquad = #412;` | Сущности и игровой мир |
| [`te_lightning2`](../37-quakec-builtins-reference/03-entity-world-builtins.md#te_lightning2) | `void(entity own, vector start, vector end) te_lightning2 = #429;` | Сущности и игровой мир |
| [`te_lightning3`](../37-quakec-builtins-reference/03-entity-world-builtins.md#te_lightning3) | `void(entity own, vector start, vector end) te_lightning3 = #430;` | Сущности и игровой мир |
| [`te_muzzleflash`](../37-quakec-builtins-reference/03-entity-world-builtins.md#te_muzzleflash) | `void(entity ent) te_muzzleflash = #0;` | Сущности и игровой мир |
| [`te_spikequad`](../37-quakec-builtins-reference/03-entity-world-builtins.md#te_spikequad) | `void(vector org) te_spikequad = #413;` | Сущности и игровой мир |
| [`te_superspikequad`](../37-quakec-builtins-reference/03-entity-world-builtins.md#te_superspikequad) | `void(vector org) te_superspikequad = #414;` | Сущности и игровой мир |
| [`undefined`](../37-quakec-builtins-reference/03-entity-world-builtins.md#undefined) | `undefined(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#539, SSQC=#509).` | Сущности и игровой мир |
| [`WriteByte`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writebyte) | `void(float to, float val) WriteByte = #52;` | Сеть и сетевые сообщения |
| [`WriteChar`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writechar) | `void(float to, float val) WriteChar = #53;` | Сеть и сетевые сообщения |
| [`WriteShort`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writeshort) | `void(float to, float val) WriteShort = #54;` | Сеть и сетевые сообщения |
| [`WriteLong`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writelong) | `void(float to, float val) WriteLong = #55;` | Сеть и сетевые сообщения |
| [`WriteAngle`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writeangle) | `void(float to, float val) WriteAngle = #57;` | Сеть и сетевые сообщения |
| [`WriteCoord`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writecoord) | `void(float to, float val) WriteCoord = #56;` | Сеть и сетевые сообщения |
| [`WriteString`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writestring) | `void(float to, string val) WriteString = #58;` | Сеть и сетевые сообщения |
| [`WriteEntity`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writeentity) | `void(float to, entity val) WriteEntity = #59;` | Сеть и сетевые сообщения |
| [`WriteFloat`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writefloat) | `void(float buf, float fl) WriteFloat = #280;` | Сеть и сетевые сообщения |
| [`WritePicture`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writepicture) | `void(float to, string s, float sz) WritePicture = #501;` | Сеть и сетевые сообщения |
| [`WriteUnterminatedString`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writeunterminatedstring) | `void(float target, string str) WriteUnterminatedString = #456;` | Сеть и сетевые сообщения |
| [`readbyte`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readbyte) | `float() readbyte = #360;` | Сеть и сетевые сообщения |
| [`readchar`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readchar) | `float() readchar = #361;` | Сеть и сетевые сообщения |
| [`readshort`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readshort) | `float() readshort = #362;` | Сеть и сетевые сообщения |
| [`readlong`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readlong) | `float() readlong = #363;` | Сеть и сетевые сообщения |
| [`readangle`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readangle) | `float() readangle = #365;` | Сеть и сетевые сообщения |
| [`readcoord`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readcoord) | `float() readcoord = #364;` | Сеть и сетевые сообщения |
| [`readfloat`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readfloat) | `float() readfloat = #367;` | Сеть и сетевые сообщения |
| [`readstring`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readstring) | `string() readstring = #366;` | Сеть и сетевые сообщения |
| [`readentitynum`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readentitynum) | `float() readentitynum = #368;` | Сеть и сетевые сообщения |
| [`ReadPicture`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readpicture) | `string() ReadPicture = #501;` | Сеть и сетевые сообщения |
| [`multicast`](../37-quakec-builtins-reference/04-network-messages-builtins.md#multicast) | `void(vector where, float set) multicast = #82;` | Сеть и сетевые сообщения |
| [`stuffcmd`](../37-quakec-builtins-reference/04-network-messages-builtins.md#stuffcmd) | `void(entity client, string s, optional string s2, optional string s3, optional string s4, optional string s5, optional string s6, optional string s7) stuffcmd = #21;` | Сеть и сетевые сообщения |
| [`clientcommand`](../37-quakec-builtins-reference/04-network-messages-builtins.md#clientcommand) | `void(entity e, string s) clientcommand = #440;` | Сеть и сетевые сообщения |
| [`sendevent`](../37-quakec-builtins-reference/04-network-messages-builtins.md#sendevent) | `void(string evname, string evargs, ...) sendevent = #359;` | Сеть и сетевые сообщения |
| [`sendpacket`](../37-quakec-builtins-reference/04-network-messages-builtins.md#sendpacket) | `float(string destaddress, string content) sendpacket = #242;` | Сеть и сетевые сообщения |
| [`deltalisten`](../37-quakec-builtins-reference/04-network-messages-builtins.md#deltalisten) | `float(string modelname, float(float isnew) updatecallback, float flags) deltalisten = #371;` | Сеть и сетевые сообщения |
| [`netaddress_resolve`](../37-quakec-builtins-reference/04-network-messages-builtins.md#netaddress_resolve) | `string(string dnsname, optional float defport) netaddress_resolve = #625;` | Сеть и сетевые сообщения |
| [`getextresponse`](../37-quakec-builtins-reference/04-network-messages-builtins.md#getextresponse) | `string() getextresponse = #624;` | Сеть и сетевые сообщения |
| [`redirectcmd`](../37-quakec-builtins-reference/04-network-messages-builtins.md#redirectcmd) | `DEP void(entity to, string str) redirectcmd = #101;` | Сеть и сетевые сообщения |
| [`isserver`](../37-quakec-builtins-reference/04-network-messages-builtins.md#isserver) | `float() isserver = #60;` | Сеть и сетевые сообщения |
| [`clientcount`](../37-quakec-builtins-reference/04-network-messages-builtins.md#clientcount) | `float() clientcount = #61;` | Сеть и сетевые сообщения |
| [`clientstate`](../37-quakec-builtins-reference/04-network-messages-builtins.md#clientstate) | `float() clientstate = #62;` | Сеть и сетевые сообщения |
| [`clienttype`](../37-quakec-builtins-reference/04-network-messages-builtins.md#clienttype) | `float(entity client) clienttype = #455;` | Сеть и сетевые сообщения |
| [`isdemo`](../37-quakec-builtins-reference/04-network-messages-builtins.md#isdemo) | `float() isdemo = #349;` | Сеть и сетевые сообщения |
| [`isbackbuffered`](../37-quakec-builtins-reference/04-network-messages-builtins.md#isbackbuffered) | `float(entity player) isbackbuffered = #234;` | Сеть и сетевые сообщения |
| [`csqc_cvar_defstring`](../37-quakec-builtins-reference/04-network-messages-builtins.md#csqc_cvar_defstring) | `csqc_cvar_defstring(...)` — сигнатура в си-таблице движка не хранится текстом (номера: MenuQC=#482).` | Сеть и сетевые сообщения |
| [`cvars_haveunsaved`](../37-quakec-builtins-reference/04-network-messages-builtins.md#cvars_haveunsaved) | `cvars_haveunsaved(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Сеть и сетевые сообщения |
| [`findkeysforcommand_dp`](../37-quakec-builtins-reference/04-network-messages-builtins.md#findkeysforcommand_dp) | `DEP string(string command, optional float bindmap) findkeysforcommand_dp = #610;` | Сеть и сетевые сообщения |
| [`findkeysforcommand_menu`](../37-quakec-builtins-reference/04-network-messages-builtins.md#findkeysforcommand_menu) | `findkeysforcommand_menu(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#610).` | Сеть и сетевые сообщения |
| [`findkeysforcommandex`](../37-quakec-builtins-reference/04-network-messages-builtins.md#findkeysforcommandex) | `findkeysforcommandex(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Сеть и сетевые сообщения |
| [`forceinfokeyblob`](../37-quakec-builtins-reference/04-network-messages-builtins.md#forceinfokeyblob) | `forceinfokeyblob(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Сеть и сетевые сообщения |
| [`getlocaluserinfo`](../37-quakec-builtins-reference/04-network-messages-builtins.md#getlocaluserinfo) | `getlocaluserinfo(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Сеть и сетевые сообщения |
| [`getlocaluserinfoblob`](../37-quakec-builtins-reference/04-network-messages-builtins.md#getlocaluserinfoblob) | `getlocaluserinfoblob(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Сеть и сетевые сообщения |
| [`getplayerkeyblob`](../37-quakec-builtins-reference/04-network-messages-builtins.md#getplayerkeyblob) | `getplayerkeyblob(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Сеть и сетевые сообщения |
| [`getplayerkeyfloat`](../37-quakec-builtins-reference/04-network-messages-builtins.md#getplayerkeyfloat) | `getplayerkeyfloat(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Сеть и сетевые сообщения |
| [`getplayerkeyvalue`](../37-quakec-builtins-reference/04-network-messages-builtins.md#getplayerkeyvalue) | `getplayerkeyvalue(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#348).` | Сеть и сетевые сообщения |
| [`getplayerstat`](../37-quakec-builtins-reference/04-network-messages-builtins.md#getplayerstat) | `getplayerstat(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Сеть и сетевые сообщения |
| [`readdouble`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readdouble) | `readdouble(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Сеть и сетевые сообщения |
| [`readint`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readint) | `readint(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Сеть и сетевые сообщения |
| [`readint64`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readint64) | `readint64(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Сеть и сетевые сообщения |
| [`readuint64`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readuint64) | `readuint64(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Сеть и сетевые сообщения |
| [`serverkeyblob`](../37-quakec-builtins-reference/04-network-messages-builtins.md#serverkeyblob) | `serverkeyblob(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Сеть и сетевые сообщения |
| [`serverkeyfloat`](../37-quakec-builtins-reference/04-network-messages-builtins.md#serverkeyfloat) | `serverkeyfloat(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Сеть и сетевые сообщения |
| [`setlocaluserinfo`](../37-quakec-builtins-reference/04-network-messages-builtins.md#setlocaluserinfo) | `setlocaluserinfo(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Сеть и сетевые сообщения |
| [`setlocaluserinfoblob`](../37-quakec-builtins-reference/04-network-messages-builtins.md#setlocaluserinfoblob) | `setlocaluserinfoblob(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Сеть и сетевые сообщения |
| [`uri_get`](../37-quakec-builtins-reference/04-network-messages-builtins.md#uri_get) | `uri_get(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#513, MenuQC=#513).` | Сеть и сетевые сообщения |
| [`uri_post`](../37-quakec-builtins-reference/04-network-messages-builtins.md#uri_post) | `uri_post(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#513, MenuQC=#513).` | Сеть и сетевые сообщения |
| [`sound`](../37-quakec-builtins-reference/05-sound-builtins.md#sound) | `void(entity e, float chan, string samp, float vol, float atten, optional float speedpct, optional float flags, optional float timeofs) sound = #8;` | Звук |
| [`ambientsound`](../37-quakec-builtins-reference/05-sound-builtins.md#ambientsound) | `void (vector pos, string samp, float vol, float atten) ambientsound = #74;` | Звук |
| [`localsound`](../37-quakec-builtins-reference/05-sound-builtins.md#localsound) | `void(string soundname, optional float channel, optional float volume) localsound = #177;` | Звук |
| [`pointsound`](../37-quakec-builtins-reference/05-sound-builtins.md#pointsound) | `void(vector origin, string sample, float volume, float attenuation) pointsound = #483;` | Звук |
| [`soundlength`](../37-quakec-builtins-reference/05-sound-builtins.md#soundlength) | `float(string sample) soundlength = #534;` | Звук |
| [`getsoundtime`](../37-quakec-builtins-reference/05-sound-builtins.md#getsoundtime) | `float(entity e, float channel) getsoundtime = #533;` | Звук |
| [`precache_sound`](../37-quakec-builtins-reference/05-sound-builtins.md#precache_sound) | `string(string s) precache_sound = #19;` | Звук |
| [`precache_sound2`](../37-quakec-builtins-reference/05-sound-builtins.md#precache_sound2) | `string(string str) precache_sound2 = #76;` | Звук |
| [`SetListener`](../37-quakec-builtins-reference/05-sound-builtins.md#setlistener) | `void(vector origin, vector forward, vector right, vector up, optional float reverbtype) SetListener = #351;` | Звук |
| [`getchannellevel`](../37-quakec-builtins-reference/05-sound-builtins.md#getchannellevel) | `getchannellevel(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Звук |
| [`getqueuedaudiotime`](../37-quakec-builtins-reference/05-sound-builtins.md#getqueuedaudiotime) | `getqueuedaudiotime(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Звук |
| [`getsoundindex`](../37-quakec-builtins-reference/05-sound-builtins.md#getsoundindex) | `getsoundindex(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Звук |
| [`queueaudio`](../37-quakec-builtins-reference/05-sound-builtins.md#queueaudio) | `queueaudio(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Звук |
| [`setup_reverb`](../37-quakec-builtins-reference/05-sound-builtins.md#setup_reverb) | `setup_reverb(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Звук |
| [`soundnameforindex`](../37-quakec-builtins-reference/05-sound-builtins.md#soundnameforindex) | `soundnameforindex(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Звук |
| [`soundupdate`](../37-quakec-builtins-reference/05-sound-builtins.md#soundupdate) | `soundupdate(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Звук |
| [`stopsound`](../37-quakec-builtins-reference/05-sound-builtins.md#stopsound) | `stopsound(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Звук |
| [`fopen`](../37-quakec-builtins-reference/06-files-database-builtins.md#fopen) | `filestream(string filename, float mode, optional float mmapminsize) fopen = #110;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`fclose`](../37-quakec-builtins-reference/06-files-database-builtins.md#fclose) | `void(filestream fhandle) fclose = #111;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`fgets`](../37-quakec-builtins-reference/06-files-database-builtins.md#fgets) | `string(filestream fhandle) fgets = #112;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`fputs`](../37-quakec-builtins-reference/06-files-database-builtins.md#fputs) | `void(filestream fhandle, string s, optional string s2, optional string s3, optional string s4, optional string s5, optional string s6, optional string s7) fputs = #113;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`fexists`](../37-quakec-builtins-reference/06-files-database-builtins.md#fexists) | `float(string fname) fexists = #653;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`fcopy`](../37-quakec-builtins-reference/06-files-database-builtins.md#fcopy) | `float(string src, string dst) fcopy = #650;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`fremove`](../37-quakec-builtins-reference/06-files-database-builtins.md#fremove) | `float(string fname) fremove = #652;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`frename`](../37-quakec-builtins-reference/06-files-database-builtins.md#frename) | `float(string src, string dst) frename = #651;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`rmtree`](../37-quakec-builtins-reference/06-files-database-builtins.md#rmtree) | `float(string path) rmtree = #654;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`writetofile`](../37-quakec-builtins-reference/06-files-database-builtins.md#writetofile) | `void(filestream fh, entity e) writetofile = #606;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`loadfromfile`](../37-quakec-builtins-reference/06-files-database-builtins.md#loadfromfile) | `void(string s) loadfromfile = #530;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`loadfromdata`](../37-quakec-builtins-reference/06-files-database-builtins.md#loadfromdata) | `void(string s) loadfromdata = #529;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`whichpack`](../37-quakec-builtins-reference/06-files-database-builtins.md#whichpack) | `string(string filename, optional enumflags:float{WP_REFERENCEPACKAGE,WP_FULLPACKAGEPATH} flags) whichpack = #503;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`search_begin`](../37-quakec-builtins-reference/06-files-database-builtins.md#search_begin) | `searchhandle(string pattern, enumflags:float{SB_CASEINSENSITIVE=1<<0,SB_FULLPACKAGEPATH=1<<1,SB_ALLOWDUPES=1<<2,SB_FORCESEARCH=1<<3,SB_MULTISEARCH=1<<4} flags, float quiet, optional string filterpackage) search_begin = #444;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`search_end`](../37-quakec-builtins-reference/06-files-database-builtins.md#search_end) | `void(searchhandle handle) search_end = #445;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`search_getsize`](../37-quakec-builtins-reference/06-files-database-builtins.md#search_getsize) | `float(searchhandle handle) search_getsize = #446;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`search_getfilename`](../37-quakec-builtins-reference/06-files-database-builtins.md#search_getfilename) | `string(searchhandle handle, float num) search_getfilename = #447;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`buf_create`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_create) | `strbuf() buf_create = #460;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`buf_del`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_del) | `void(strbuf bufhandle) buf_del = #461;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`buf_getsize`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_getsize) | `float(strbuf bufhandle) buf_getsize = #462;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`buf_copy`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_copy) | `void(strbuf bufhandle_from, strbuf bufhandle_to) buf_copy = #463;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`buf_loadfile`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_loadfile) | `float(string filename, strbuf bufhandle) buf_loadfile = #535;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`buf_writefile`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_writefile) | `float(filestream filehandle, strbuf bufhandle, optional float startpos, optional float numstrings) buf_writefile = #536;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`buf_sort`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_sort) | `void(strbuf bufhandle, float sortprefixlen, float backward) buf_sort = #464;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`buf_implode`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_implode) | `string(strbuf bufhandle, string glue) buf_implode = #465;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`buf_cvarlist`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_cvarlist) | `void(strbuf strbuf, string pattern, string antipattern) buf_cvarlist = #517;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`bufstr_add`](../37-quakec-builtins-reference/06-files-database-builtins.md#bufstr_add) | `float(strbuf bufhandle, string str, float ordered) bufstr_add = #468;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`bufstr_free`](../37-quakec-builtins-reference/06-files-database-builtins.md#bufstr_free) | `void(strbuf bufhandle, float string_index) bufstr_free = #469;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`bufstr_get`](../37-quakec-builtins-reference/06-files-database-builtins.md#bufstr_get) | `string(strbuf bufhandle, float string_index) bufstr_get = #466;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`bufstr_set`](../37-quakec-builtins-reference/06-files-database-builtins.md#bufstr_set) | `void(strbuf bufhandle, float string_index, string str) bufstr_set = #467;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`bufstr_find`](../37-quakec-builtins-reference/06-files-database-builtins.md#bufstr_find) | `float(float bufhandle, string match, float matchrule, float startpos, float step) bufstr_find = #537;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`hash_createtab`](../37-quakec-builtins-reference/06-files-database-builtins.md#hash_createtab) | `hashtable(float tabsize, optional float defaulttype) hash_createtab = #287;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`hash_destroytab`](../37-quakec-builtins-reference/06-files-database-builtins.md#hash_destroytab) | `void(hashtable table) hash_destroytab = #288;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`hash_add`](../37-quakec-builtins-reference/06-files-database-builtins.md#hash_add) | `void(hashtable table, string name, __variant value, optional float typeandflags) hash_add = #289;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`hash_delete`](../37-quakec-builtins-reference/06-files-database-builtins.md#hash_delete) | `__variant(hashtable table, string name) hash_delete = #291;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`hash_get`](../37-quakec-builtins-reference/06-files-database-builtins.md#hash_get) | `__variant(hashtable table, string name, optional __variant deflt, optional float requiretype, optional float index) hash_get = #290;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`hash_getkey`](../37-quakec-builtins-reference/06-files-database-builtins.md#hash_getkey) | `string(hashtable table, float idx) hash_getkey = #292;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`memalloc`](../37-quakec-builtins-reference/06-files-database-builtins.md#memalloc) | `__variant*(int size) memalloc = #384;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`memfree`](../37-quakec-builtins-reference/06-files-database-builtins.md#memfree) | `void(__variant *ptr) memfree = #385;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`memcpy`](../37-quakec-builtins-reference/06-files-database-builtins.md#memcpy) | `void(__variant *dst, __variant *src, int size) memcpy = #386;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`memfill8`](../37-quakec-builtins-reference/06-files-database-builtins.md#memfill8) | `void(__variant *dst, int val, int size) memfill8 = #387;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`memgetval`](../37-quakec-builtins-reference/06-files-database-builtins.md#memgetval) | `__variant(__variant *dst, float ofs) memgetval = #388;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`memsetval`](../37-quakec-builtins-reference/06-files-database-builtins.md#memsetval) | `void(__variant *dst, float ofs, __variant val) memsetval = #389;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`memptradd`](../37-quakec-builtins-reference/06-files-database-builtins.md#memptradd) | `__variant*(__variant *base, float ofs) memptradd = #390;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`sqlconnect`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlconnect) | `float(optional string host, optional string user, optional string pass, optional string defaultdb, optional string driver) sqlconnect = #250;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`sqldisconnect`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqldisconnect) | `void(float serveridx) sqldisconnect = #251;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`sqlopenquery`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlopenquery) | `float(float serveridx, void(float serveridx, float queryidx, float rows, float columns, float eof, float firstrow) callback, float querytype, string query) sqlopenquery = #252;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`sqlclosequery`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlclosequery) | `void(float serveridx, float queryidx) sqlclosequery = #253;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`sqlreadfield`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlreadfield) | `string(float serveridx, float queryidx, float row, float column) sqlreadfield = #254;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`sqlreadfloat`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlreadfloat) | `float(float serveridx, float queryidx, float row, float column) sqlreadfloat = #258;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`sqlerror`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlerror) | `string(float serveridx, optional float queryidx) sqlerror = #255;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`sqlescape`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlescape) | `string(float serveridx, string data) sqlescape = #256;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`sqlversion`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlversion) | `string(float serveridx) sqlversion = #257;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`digest_hex`](../37-quakec-builtins-reference/06-files-database-builtins.md#digest_hex) | `string(string digest, string data, ...) digest_hex = #639;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`fork`](../37-quakec-builtins-reference/06-files-database-builtins.md#fork) | `float(optional float sleeptime) fork = #210;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`sleep`](../37-quakec-builtins-reference/06-files-database-builtins.md#sleep) | `void(float sleeptime) sleep = #212;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`createbuffer`](../37-quakec-builtins-reference/06-files-database-builtins.md#createbuffer) | `createbuffer(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`digest_ptr`](../37-quakec-builtins-reference/06-files-database-builtins.md#digest_ptr) | `digest_ptr(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`fread`](../37-quakec-builtins-reference/06-files-database-builtins.md#fread) | `fread(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`fseek`](../37-quakec-builtins-reference/06-files-database-builtins.md#fseek) | `fseek(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`fseek64`](../37-quakec-builtins-reference/06-files-database-builtins.md#fseek64) | `fseek64(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`fsize`](../37-quakec-builtins-reference/06-files-database-builtins.md#fsize) | `fsize(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`fsize64`](../37-quakec-builtins-reference/06-files-database-builtins.md#fsize64) | `fsize64(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`fwrite`](../37-quakec-builtins-reference/06-files-database-builtins.md#fwrite) | `fwrite(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`hash_getcb`](../37-quakec-builtins-reference/06-files-database-builtins.md#hash_getcb) | `hash_getcb(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#293, MenuQC=#293).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`json_find_object_child`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_find_object_child) | `json_find_object_child(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`json_free`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_free) | `json_free(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`json_get_child_at_index`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_get_child_at_index) | `json_get_child_at_index(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`json_get_float`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_get_float) | `json_get_float(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`json_get_integer`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_get_integer) | `json_get_integer(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`json_get_length`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_get_length) | `json_get_length(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`json_get_name`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_get_name) | `json_get_name(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`json_get_string`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_get_string) | `json_get_string(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`json_get_value_type`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_get_value_type) | `json_get_value_type(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`json_parse`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_parse) | `json_parse(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`memcmp`](../37-quakec-builtins-reference/06-files-database-builtins.md#memcmp) | `memcmp(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`memrealloc`](../37-quakec-builtins-reference/06-files-database-builtins.md#memrealloc) | `memrealloc(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`memstrsize`](../37-quakec-builtins-reference/06-files-database-builtins.md#memstrsize) | `memstrsize(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`search_fopen`](../37-quakec-builtins-reference/06-files-database-builtins.md#search_fopen) | `search_fopen(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`search_getfilemtime`](../37-quakec-builtins-reference/06-files-database-builtins.md#search_getfilemtime) | `search_getfilemtime(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`search_getfilesize`](../37-quakec-builtins-reference/06-files-database-builtins.md#search_getfilesize) | `search_getfilesize(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`search_getpackagename`](../37-quakec-builtins-reference/06-files-database-builtins.md#search_getpackagename) | `search_getpackagename(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Файлы, буферы, хеш-таблицы и базы данных |
| [`sqlescapeblob`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlescapeblob) | `string(float serveridx, __variant *ptr, int maxsize) sqlescapeblob = #0;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`sqlreadblob`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlreadblob) | `int(float serveridx, float queryidx, float row, float column, __variant *ptr, int maxsize) sqlreadblob = #0;` | Файлы, буферы, хеш-таблицы и базы данных |
| [`precache_file`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#precache_file) | `string(string s) precache_file = #68;` | Прекэш и игровые ресурсы |
| [`precache_file2`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#precache_file2) | `string(string str) precache_file2 = #77;` | Прекэш и игровые ресурсы |
| [`precache_model`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#precache_model) | `string(string s) precache_model = #20;` | Прекэш и игровые ресурсы |
| [`precache_model2`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#precache_model2) | `string(string str) precache_model2 = #75;` | Прекэш и игровые ресурсы |
| [`precache_pic`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#precache_pic) | `string(string name, optional float flags) precache_pic = #317;` | Прекэш и игровые ресурсы |
| [`precache_vwep_model`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#precache_vwep_model) | `float(string mname) precache_vwep_model = #532;` | Прекэш и игровые ресурсы |
| [`getmodelindex`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#getmodelindex) | `float(string modelname, optional float queryonly) getmodelindex = #200;` | Прекэш и игровые ресурсы |
| [`modelnameforindex`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#modelnameforindex) | `string(float mdlindex) modelnameforindex = #334;` | Прекэш и игровые ресурсы |
| [`frameforname`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#frameforname) | `float(float modidx, string framename) frameforname = #276;` | Прекэш и игровые ресурсы |
| [`frametoname`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#frametoname) | `string(float modidx, float framenum) frametoname = #284;` | Прекэш и игровые ресурсы |
| [`frameduration`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#frameduration) | `float(float modidx, float framenum) frameduration = #277;` | Прекэш и игровые ресурсы |
| [`skinforname`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#skinforname) | `float(float mdlindex, string skinname) skinforname = #237;` | Прекэш и игровые ресурсы |
| [`skintoname`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#skintoname) | `string(float modidx, float skin) skintoname = #285;` | Прекэш и игровые ресурсы |
| [`shaderforname`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#shaderforname) | `float(string shadername, optional string defaultshader, ...) shaderforname = #238;` | Прекэш и игровые ресурсы |
| [`findfont`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#findfont) | `float(string s) findfont = #356;` | Прекэш и игровые ресурсы |
| [`loadfont`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#loadfont) | `float(string fontname, string fontmaps, string sizes, float slot, optional float fix_scale, optional float fix_voffset) loadfont = #357;` | Прекэш и игровые ресурсы |
| [`changepic`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#changepic) | `DEP_CSQC void(string slot, string picname, optional entity player) changepic = #107;` | Прекэш и игровые ресурсы |
| [`drawgetimagesize`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#drawgetimagesize) | `vector(string picname) drawgetimagesize = #318;` | Прекэш и игровые ресурсы |
| [`iscachedpic`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#iscachedpic) | `float(string name) iscachedpic = #316;` | Прекэш и игровые ресурсы |
| [`freepic`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#freepic) | `void(string name) freepic = #319;` | Прекэш и игровые ресурсы |
| [`addprogs`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#addprogs) | `addprogs(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#202, MenuQC=#202).` | Прекэш и игровые ресурсы |
| [`frameforaction`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#frameforaction) | `frameforaction(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Прекэш и игровые ресурсы |
| [`getmodeleventidx`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#getmodeleventidx) | `getmodeleventidx(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Прекэш и игровые ресурсы |
| [`getnextmodelevent`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#getnextmodelevent) | `getnextmodelevent(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Прекэш и игровые ресурсы |
| [`modelframecount`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#modelframecount) | `modelframecount(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Прекэш и игровые ресурсы |
| [`processmodelevents`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#processmodelevents) | `processmodelevents(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Прекэш и игровые ресурсы |
| [`spriteframe`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#spriteframe) | `spriteframe(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Прекэш и игровые ресурсы |
| [`addentity`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#addentity) | `void(entity ent) addentity = #302;` | Рендеринг и сцена CSQC |
| [`addentities`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#addentities) | `void(float mask) addentities = #301;` | Рендеринг и сцена CSQC |
| [`clearscene`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#clearscene) | `void() clearscene = #300;` | Рендеринг и сцена CSQC |
| [`renderscene`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#renderscene) | `void() renderscene = #304;` | Рендеринг и сцена CSQC |
| [`getproperty`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#getproperty) | `__variant(float property) getproperty = #309;` (алиас `getviewprop`)` | Рендеринг и сцена CSQC |
| [`setproperty`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#setproperty) | `float(float property, ...) setproperty = #303;` (алиас `setviewprop`)` | Рендеринг и сцена CSQC |
| [`getresolution`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#getresolution) | `vector(float vidmode, optional float forfullscreen) getresolution = #608;` | Рендеринг и сцена CSQC |
| [`R_BeginPolygon`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#r_beginpolygon) | `void(string texturename, optional float flags, optional float is2d) R_BeginPolygon = #306;` | Рендеринг и сцена CSQC |
| [`R_EndPolygon`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#r_endpolygon) | `void() R_EndPolygon = #308;` | Рендеринг и сцена CSQC |
| [`R_PolygonVertex`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#r_polygonvertex) | `void(vector org, vector texcoords, vector rgb, float alpha) R_PolygonVertex = #307;` | Рендеринг и сцена CSQC |
| [`drawcharacter`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawcharacter) | `float(vector position, float character, vector size, vector rgb, float alpha, optional float drawflag) drawcharacter = #320;` | Рендеринг и сцена CSQC |
| [`drawfill`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawfill) | `float(vector position, vector size, vector rgb, vector alpha, optional float drawflag) drawfill = #323;` | Рендеринг и сцена CSQC |
| [`drawline`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawline) | `void(float width, vector pos1, vector pos2, vector rgb, float alpha, optional float drawflag) drawline = #315;` | Рендеринг и сцена CSQC |
| [`drawpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawpic) | `float(vector position, string pic, vector size, vector rgb, float alpha, optional float drawflag) drawpic = #322;` | Рендеринг и сцена CSQC |
| [`drawrawstring`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawrawstring) | `float(vector position, string text, vector size, vector rgb, float alpha, optional float drawflag) drawrawstring = #321;` | Рендеринг и сцена CSQC |
| [`drawresetcliparea`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawresetcliparea) | `void(void) drawresetcliparea = #325;` | Рендеринг и сцена CSQC |
| [`drawsetcliparea`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawsetcliparea) | `void(float x, float y, float width, float height) drawsetcliparea = #324;` | Рендеринг и сцена CSQC |
| [`drawstring`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawstring) | `float(vector position, string text, vector size, vector rgb, float alpha, float drawflag) drawstring = #326;` | Рендеринг и сцена CSQC |
| [`drawsubpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawsubpic) | `void(vector pos, vector sz, string pic, vector srcpos, vector srcsz, vector rgb, float alpha, optional float drawflag) drawsubpic = #328;` | Рендеринг и сцена CSQC |
| [`movepic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#movepic) | `void(string slot, float x, float y, float zone, optional entity player) movepic = #106;` | Рендеринг и сцена CSQC |
| [`showpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#showpic) | `void(string slot, string picname, float x, float y, float zone, optional entity player) showpic = #104;` | Рендеринг и сцена CSQC |
| [`hidepic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#hidepic) | `void(string slot, optional entity player) hidepic = #105;` | Рендеринг и сцена CSQC |
| [`changepic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#changepic) | `void(string slot, string picname, optional entity player) changepic = #107;` | Рендеринг и сцена CSQC |
| [`iscachedpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#iscachedpic) | `float(string name) iscachedpic = #316;` | Рендеринг и сцена CSQC |
| [`freepic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#freepic) | `void(string name) freepic = #319;` | Рендеринг и сцена CSQC |
| [`drawgetimagesize`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawgetimagesize) | `vector(string picname) drawgetimagesize = #318;` (алиас `draw_getimagesize`)` | Рендеринг и сцена CSQC |
| [`stringwidth`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#stringwidth) | `float(string text, float usecolours, optional vector fontsize) stringwidth = #327;` | Рендеринг и сцена CSQC |
| [`adddecal`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#adddecal) | `void(string shadername, vector origin, vector up, vector side, vector rgb, float alpha) adddecal = #375;` | Рендеринг и сцена CSQC |
| [`boxparticles`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#boxparticles) | `void(float effectindex, entity own, vector org_from, vector org_to, vector dir_from, vector dir_to, float countmultiplier, optional float flags) boxparticles = #502;` | Рендеринг и сцена CSQC |
| [`particle`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#particle) | `void(vector pos, vector dir, float colour, float count) particle = #48;` | Рендеринг и сцена CSQC |
| [`particle2`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#particle2) | `void(vector org, vector dmin, vector dmax, float colour, float effect, float count) particle2 = #215;` | Рендеринг и сцена CSQC |
| [`particle3`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#particle3) | `void(vector org, vector box, float colour, float effect, float count) particle3 = #216;` | Рендеринг и сцена CSQC |
| [`particle4`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#particle4) | `void(vector org, float radius, float colour, float effect, float count) particle4 = #217;` | Рендеринг и сцена CSQC |
| [`particleeffectnum`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#particleeffectnum) | `float(string effectname) particleeffectnum = #335;` | Рендеринг и сцена CSQC |
| [`particleeffectquery`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#particleeffectquery) | `string(float efnum, float body) particleeffectquery = #374;` | Рендеринг и сцена CSQC |
| [`pointparticles`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#pointparticles) | `void(float effectnum, vector origin, optional vector dir, optional float count) pointparticles = #337;` | Рендеринг и сцена CSQC |
| [`trailparticles`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#trailparticles) | `void(float effectnum, entity ent, vector start, vector end) trailparticles = #336;` | Рендеринг и сцена CSQC |
| [`effect`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#effect) | `void(vector org, string modelname, float startframe, float endframe, float framerate) effect = #404;` | Рендеринг и сцена CSQC |
| [`dynamiclight_add`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#dynamiclight_add) | `float(vector org, float radius, vector lightcolours, optional float style, optional string cubemapname, optional float pflags) dynamiclight_add = #305;` | Рендеринг и сцена CSQC |
| [`dynamiclight_get`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#dynamiclight_get) | `__variant(float lno, float fld) dynamiclight_get = #372;` | Рендеринг и сцена CSQC |
| [`dynamiclight_set`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#dynamiclight_set) | `void(float lno, float fld, __variant value) dynamiclight_set = #373;` | Рендеринг и сцена CSQC |
| [`lightstyle`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#lightstyle) | `void(float lightstyle, string stylestring, optional vector rgb) lightstyle = #35;` | Рендеринг и сцена CSQC |
| [`lightstylestatic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#lightstylestatic) | `void(float style, float val, optional vector rgb) lightstylestatic = #5;` | Рендеринг и сцена CSQC |
| [`getlight`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#getlight) | `vector(vector org) getlight = #92;` | Рендеринг и сцена CSQC |
| [`con_draw`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#con_draw) | `void(string conname, vector pos, vector size, float fontsize) con_draw = #393;` | Рендеринг и сцена CSQC |
| [`con_getset`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#con_getset) | `string(string conname, string field, optional string newvalue) con_getset = #391;` | Рендеринг и сцена CSQC |
| [`con_input`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#con_input) | `float(string conname, float inevtype, float parama, float paramb, float paramc) con_input = #394;` | Рендеринг и сцена CSQC |
| [`con_printf`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#con_printf) | `void(string conname, string messagefmt, ...) con_printf = #392;` | Рендеринг и сцена CSQC |
| [`RegisterTempEnt`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#registertempent) | `float(float attributes, string effectname, ...) RegisterTempEnt = #208;` | Рендеринг и сцена CSQC |
| [`CustomTempEnt`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#customtempent) | `void(float type, vector pos, ...) CustomTempEnt = #209;` | Рендеринг и сцена CSQC |
| [`te_beam`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_beam) | `void(entity own, vector start, vector end) te_beam = #431;` | Рендеринг и сцена CSQC |
| [`te_blood`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_blood) | `void(vector org, vector dir, float count) te_blood = #405;` | Рендеринг и сцена CSQC |
| [`te_bloodqw`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_bloodqw) | `void(vector org, optional float count) te_bloodqw = #239;` | Рендеринг и сцена CSQC |
| [`te_bloodshower`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_bloodshower) | `void(vector mincorner, vector maxcorner, float explosionspeed, float howmany) te_bloodshower = #406;` | Рендеринг и сцена CSQC |
| [`te_customflash`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_customflash) | `void(vector org, float radius, float lifetime, vector color) te_customflash = #417;` | Рендеринг и сцена CSQC |
| [`te_explosion`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_explosion) | `void(vector org) te_explosion = #421;` | Рендеринг и сцена CSQC |
| [`te_explosion2`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_explosion2) | `void(vector org, float color, float colorlength) te_explosion2 = #427;` | Рендеринг и сцена CSQC |
| [`te_explosionquad`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_explosionquad) | `void(vector org) te_explosionquad = #415;` | Рендеринг и сцена CSQC |
| [`te_explosionrgb`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_explosionrgb) | `void(vector org, vector color) te_explosionrgb = #407;` | Рендеринг и сцена CSQC |
| [`te_flamejet`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_flamejet) | `void(vector org, vector vel, float howmany) te_flamejet = #457;` | Рендеринг и сцена CSQC |
| [`te_gunshot`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_gunshot) | `void(vector org, optional float count) te_gunshot = #418;` | Рендеринг и сцена CSQC |
| [`te_knightspike`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_knightspike) | `void(vector org) te_knightspike = #424;` | Рендеринг и сцена CSQC |
| [`te_lavasplash`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_lavasplash) | `void(vector org) te_lavasplash = #425;` | Рендеринг и сцена CSQC |
| [`te_lightning1`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_lightning1) | `void(entity own, vector start, vector end) te_lightning1 = #428;` | Рендеринг и сцена CSQC |
| [`te_lightningblood`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_lightningblood) | `void(vector pos) te_lightningblood = #219;` | Рендеринг и сцена CSQC |
| [`te_particlecube`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_particlecube) | `void(vector mincorner, vector maxcorner, vector vel, float howmany, float color, float gravityflag, float randomveljitter) te_particlecube = #408;` | Рендеринг и сцена CSQC |
| [`te_particlerain`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_particlerain) | `void(vector mincorner, vector maxcorner, vector vel, float howmany, float color) te_particlerain = #409;` | Рендеринг и сцена CSQC |
| [`te_particlesnow`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_particlesnow) | `void(vector mincorner, vector maxcorner, vector vel, float howmany, float color) te_particlesnow = #410;` | Рендеринг и сцена CSQC |
| [`te_plasmaburn`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_plasmaburn) | `void(vector org) te_plasmaburn = #433;` | Рендеринг и сцена CSQC |
| [`te_smallflash`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_smallflash) | `void(vector org) te_smallflash = #416;` | Рендеринг и сцена CSQC |
| [`te_spark`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_spark) | `void(vector org, vector vel, float howmany) te_spark = #411;` | Рендеринг и сцена CSQC |
| [`te_spike`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_spike) | `void(vector org) te_spike = #419;` | Рендеринг и сцена CSQC |
| [`te_superspike`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_superspike) | `void(vector org) te_superspike = #420;` | Рендеринг и сцена CSQC |
| [`te_tarexplosion`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_tarexplosion) | `void(vector org) te_tarexplosion = #422;` | Рендеринг и сцена CSQC |
| [`te_teleport`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_teleport) | `void(vector org) te_teleport = #426;` | Рендеринг и сцена CSQC |
| [`te_wizspike`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_wizspike) | `void(vector org) te_wizspike = #423;` | Рендеринг и сцена CSQC |
| [`addentity_lighting`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#addentity_lighting) | `addentity_lighting(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Рендеринг и сцена CSQC |
| [`addtrisoup_simple`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#addtrisoup_simple) | `addtrisoup_simple(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Рендеринг и сцена CSQC |
| [`customtempent`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#customtempent) | `customtempent(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#209).` | Рендеринг и сцена CSQC |
| [`drawrotpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawrotpic) | `drawrotpic(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Рендеринг и сцена CSQC |
| [`drawrotpic_dp`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawrotpic_dp) | `drawrotpic_dp(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#329, MenuQC=#470).` | Рендеринг и сцена CSQC |
| [`drawrotsubpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawrotsubpic) | `drawrotsubpic(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Рендеринг и сцена CSQC |
| [`dynamiclight_spawnstatic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#dynamiclight_spawnstatic) | `dynamiclight_spawnstatic(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Рендеринг и сцена CSQC |
| [`getlightstyle`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#getlightstyle) | `getlightstyle(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Рендеринг и сцена CSQC |
| [`getlightstylergb`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#getlightstylergb) | `getlightstylergb(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Рендеринг и сцена CSQC |
| [`getlocationname`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#getlocationname) | `getlocationname(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Рендеринг и сцена CSQC |
| [`pointcontentsmask`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#pointcontentsmask) | `pointcontentsmask(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Рендеринг и сцена CSQC |
| [`R_EndPolygonRibbon`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#r_endpolygonribbon) | `R_EndPolygonRibbon(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Рендеринг и сцена CSQC |
| [`r_readimage`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#r_readimage) | `r_readimage(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Рендеринг и сцена CSQC |
| [`r_uploadimage`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#r_uploadimage) | `r_uploadimage(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Рендеринг и сцена CSQC |
| [`registertempent`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#registertempent) | `registertempent(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#208).` | Рендеринг и сцена CSQC |
| [`remapshader`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#remapshader) | `remapshader(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Рендеринг и сцена CSQC |
| [`setcolor`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#setcolor) | `setcolor(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#401).` | Рендеринг и сцена CSQC |
| [`trailparticles_dp`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#trailparticles_dp) | `trailparticles_dp(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#336).` | Рендеринг и сцена CSQC |
| [`V_CalcRefdef`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#v_calcrefdef) | `V_CalcRefdef(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#640).` | Рендеринг и сцена CSQC |
| [`getinputstate`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getinputstate) | `float(float inputsequencenum) getinputstate = #345;` | Ввод, интерфейс и клавиатура CSQC |
| [`getkeybind`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getkeybind) | `string(float keynum) getkeybind = #342;` | Ввод, интерфейс и клавиатура CSQC |
| [`setkeybind`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setkeybind) | `float(float key, string bind, optional float bindmap, optional float modifier) setkeybind = #630;` | Ввод, интерфейс и клавиатура CSQC |
| [`getkeydest`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getkeydest) | `float() getkeydest = #602;` | Ввод, интерфейс и клавиатура CSQC |
| [`setkeydest`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setkeydest) | `void(float dest) setkeydest = #601;` | Ввод, интерфейс и клавиатура CSQC |
| [`getbindmaps`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getbindmaps) | `vector() getbindmaps = #631;` | Ввод, интерфейс и клавиатура CSQC |
| [`setbindmaps`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setbindmaps) | `float(vector bm) setbindmaps = #632;` | Ввод, интерфейс и клавиатура CSQC |
| [`getmousepos`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getmousepos) | `vector() getmousepos = #66;` | Ввод, интерфейс и клавиатура CSQC |
| [`setmousetarget`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setmousetarget) | `void(float trg) setmousetarget = #603;` | Ввод, интерфейс и клавиатура CSQC |
| [`getmousetarget`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getmousetarget) | `float() getmousetarget = #604;` | Ввод, интерфейс и клавиатура CSQC |
| [`setcursormode`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setcursormode) | `void(float usecursor, optional string cursorimage, optional vector hotspot, optional float scale) setcursormode = #343;` | Ввод, интерфейс и клавиатура CSQC |
| [`setsensitivityscaler`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setsensitivityscaler) | `void(float sens) setsensitivityscaler = #346;` | Ввод, интерфейс и клавиатура CSQC |
| [`keynumtostring`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#keynumtostring) | `string(float keynum) keynumtostring = #340;` | Ввод, интерфейс и клавиатура CSQC |
| [`keynumtostring_csqc`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#keynumtostring_csqc) | `string(float keynum) keynumtostring_csqc = #340;` | Ввод, интерфейс и клавиатура CSQC |
| [`keynumtostring_menu`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#keynumtostring_menu) | `string(float keynum) keynumtostring_menu = #609;` | Ввод, интерфейс и клавиатура CSQC |
| [`keynumtostring_omgwtf`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#keynumtostring_omgwtf) | `string(float keynum) keynumtostring_omgwtf = #520;` | Ввод, интерфейс и клавиатура CSQC |
| [`stringtokeynum`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#stringtokeynum) | `float(string keyname) stringtokeynum = #341;` | Ввод, интерфейс и клавиатура CSQC |
| [`stringtokeynum_csqc`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#stringtokeynum_csqc) | `float(string keyname) stringtokeynum_csqc = #341;` | Ввод, интерфейс и клавиатура CSQC |
| [`stringtokeynum_menu`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#stringtokeynum_menu) | `float(string key) stringtokeynum_menu = #614;` | Ввод, интерфейс и клавиатура CSQC |
| [`findkeysforcommand`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#findkeysforcommand) | `string(string command, optional float bindmap) findkeysforcommand = #521;` | Ввод, интерфейс и клавиатура CSQC |
| [`gecko_create`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_create) | `float(string name, optional string initialURI) gecko_create = #487;` | Ввод, интерфейс и клавиатура CSQC |
| [`gecko_destroy`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_destroy) | `void(string name) gecko_destroy = #488;` | Ввод, интерфейс и клавиатура CSQC |
| [`gecko_navigate`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_navigate) | `void(string name, string URI) gecko_navigate = #489;` | Ввод, интерфейс и клавиатура CSQC |
| [`gecko_keyevent`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_keyevent) | `float(string name, float key, float eventtype, optional float charcode) gecko_keyevent = #490;` | Ввод, интерфейс и клавиатура CSQC |
| [`gecko_mousemove`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_mousemove) | `void(string name, float x, float y) gecko_mousemove = #491;` | Ввод, интерфейс и клавиатура CSQC |
| [`gecko_resize`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_resize) | `void(string name, float w, float h) gecko_resize = #492;` | Ввод, интерфейс и клавиатура CSQC |
| [`gecko_get_texture_extent`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_get_texture_extent) | `vector(string name) gecko_get_texture_extent = #493;` | Ввод, интерфейс и клавиатура CSQC |
| [`CL_RotateMoves`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#cl_rotatemoves) | `CL_RotateMoves(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#638).` | Ввод, интерфейс и клавиатура CSQC |
| [`clipboard_get`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#clipboard_get) | `clipboard_get(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Ввод, интерфейс и клавиатура CSQC |
| [`clipboard_set`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#clipboard_set) | `clipboard_set(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Ввод, интерфейс и клавиатура CSQC |
| [`drawtextfield`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#drawtextfield) | `drawtextfield(...)` — сигнатура в си-таблице движка не хранится текстом (номера: MenuQC=#0).` | Ввод, интерфейс и клавиатура CSQC |
| [`gecko_getproperty`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_getproperty) | `gecko_getproperty(...)` — сигнатура в си-таблице движка не хранится текстом (номера: MenuQC=#0).` | Ввод, интерфейс и клавиатура CSQC |
| [`getcursormode`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getcursormode) | `getcursormode(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Ввод, интерфейс и клавиатура CSQC |
| [`setmousepos`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setmousepos) | `setmousepos(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Ввод, интерфейс и клавиатура CSQC |
| [`setwindowcaption`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setwindowcaption) | `setwindowcaption(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Ввод, интерфейс и клавиатура CSQC |
| [`skel_build`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_build) | `float(float skel, entity ent, float modelindex, float retainfrac, float firstbone, float lastbone, optional float addfrac) skel_build = #264;` | Скелетная анимация и модели |
| [`skel_copybones`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_copybones) | `void(float skeldst, float skelsrc, float startbone, float entbone) skel_copybones = #274;` | Скелетная анимация и модели |
| [`skel_create`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_create) | `float(float modlindex, optional float useabstransforms) skel_create = #263;` | Скелетная анимация и модели |
| [`skel_delete`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_delete) | `void(float skel) skel_delete = #275;` | Скелетная анимация и модели |
| [`skel_find_bone`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_find_bone) | `float(float skel, string tagname) skel_find_bone = #268;` | Скелетная анимация и модели |
| [`skel_get_boneabs`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_get_boneabs) | `vector(float skel, float bonenum) skel_get_boneabs = #270;` | Скелетная анимация и модели |
| [`skel_get_bonename`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_get_bonename) | `string(float skel, float bonenum) skel_get_bonename = #266;` | Скелетная анимация и модели |
| [`skel_get_boneparent`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_get_boneparent) | `float(float skel, float bonenum) skel_get_boneparent = #267;` | Скелетная анимация и модели |
| [`skel_get_bonerel`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_get_bonerel) | `vector(float skel, float bonenum) skel_get_bonerel = #269;` | Скелетная анимация и модели |
| [`skel_get_numbones`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_get_numbones) | `float(float skel) skel_get_numbones = #265;` | Скелетная анимация и модели |
| [`skel_mmap`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_mmap) | `float*(float skel) skel_mmap = #282;` | Скелетная анимация и модели |
| [`skel_premul_bone`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_premul_bone) | `void(float skel, float bonenum, vector org, optional vector fwd, optional vector right, optional vector up) skel_premul_bone = #272;` | Скелетная анимация и модели |
| [`skel_premul_bones`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_premul_bones) | `void(float skel, float startbone, float endbone, vector org, optional vector fwd, optional vector right, optional vector up) skel_premul_bones = #273;` | Скелетная анимация и модели |
| [`skel_ragupdate`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_ragupdate) | `float(entity skelent, string dollcmd, float animskel) skel_ragupdate = #281;` | Скелетная анимация и модели |
| [`skel_set_bone`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_set_bone) | `void(float skel, float bonenum, vector org, optional vector fwd, optional vector right, optional vector up) skel_set_bone = #271;` | Скелетная анимация и модели |
| [`skel_set_bone_world`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_set_bone_world) | `void(entity ent, float bonenum, vector org, optional vector angorfwd, optional vector right, optional vector up) skel_set_bone_world = #283;` | Скелетная анимация и модели |
| [`gettagindex`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#gettagindex) | `float(entity ent, string tagname) gettagindex = #451;` | Скелетная анимация и модели |
| [`gettaginfo`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#gettaginfo) | `vector(entity ent, float tagindex) gettaginfo = #452;` | Скелетная анимация и модели |
| [`getsurfaceclippedpoint`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfaceclippedpoint) | `vector(entity e, float s, vector p) getsurfaceclippedpoint = #439;` | Скелетная анимация и модели |
| [`getsurfacenearpoint`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfacenearpoint) | `float(entity e, vector p) getsurfacenearpoint = #438;` | Скелетная анимация и модели |
| [`getsurfacenormal`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfacenormal) | `vector(entity e, float s) getsurfacenormal = #436;` | Скелетная анимация и модели |
| [`getsurfacenumpoints`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfacenumpoints) | `float(entity e, float s) getsurfacenumpoints = #434;` | Скелетная анимация и модели |
| [`getsurfacenumtriangles`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfacenumtriangles) | `float(entity e, float s) getsurfacenumtriangles = #628;` | Скелетная анимация и модели |
| [`getsurfacepoint`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfacepoint) | `vector(entity e, float s, float n) getsurfacepoint = #435;` | Скелетная анимация и модели |
| [`getsurfacepointattribute`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfacepointattribute) | `vector(entity e, float s, float n, float a) getsurfacepointattribute = #486;` | Скелетная анимация и модели |
| [`getsurfacetexture`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfacetexture) | `string(entity e, float s) getsurfacetexture = #437;` | Скелетная анимация и модели |
| [`getsurfacetriangle`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfacetriangle) | `vector(entity e, float s, float n) getsurfacetriangle = #629;` | Скелетная анимация и модели |
| [`applycustomskin`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#applycustomskin) | `void(entity e, float skinobj) applycustomskin = #378;` | Скелетная анимация и модели |
| [`loadcustomskin`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#loadcustomskin) | `float(string skinfilename, optional string skindata) loadcustomskin = #377;` | Скелетная анимация и модели |
| [`setcustomskin`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#setcustomskin) | `void(entity e, string skinfilename, optional string skindata) setcustomskin = #376;` | Скелетная анимация и модели |
| [`releasecustomskin`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#releasecustomskin) | `void(float skinobj) releasecustomskin = #379;` | Скелетная анимация и модели |
| [`setcolors`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#setcolors) | `__deprecated("No RGB support.") void(entity ent, float colours) setcolors = #401;` | Скелетная анимация и модели |
| [`skel_build_ptr`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_build_ptr) | `skel_build_ptr(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Скелетная анимация и модели |
| [`skel_postmul_bone`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_postmul_bone) | `skel_postmul_bone(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Скелетная анимация и модели |
| [`skel_postmul_bones`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_postmul_bones) | `skel_postmul_bones(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Скелетная анимация и модели |
| [`addwantedhostcachekey`](../37-quakec-builtins-reference/11-server-browser-builtins.md#addwantedhostcachekey) | `void(string key) addwantedhostcachekey = #623;` | Браузер серверов и мастер-сервер |
| [`gethostcacheindexforkey`](../37-quakec-builtins-reference/11-server-browser-builtins.md#gethostcacheindexforkey) | `float(string key) gethostcacheindexforkey = #622;` | Браузер серверов и мастер-сервер |
| [`gethostcachenumber`](../37-quakec-builtins-reference/11-server-browser-builtins.md#gethostcachenumber) | `float(float fld, float hostnr) gethostcachenumber = #621;` | Браузер серверов и мастер-сервер |
| [`gethostcachestring`](../37-quakec-builtins-reference/11-server-browser-builtins.md#gethostcachestring) | `string(float type, float hostnr) gethostcachestring = #612;` | Браузер серверов и мастер-сервер |
| [`gethostcachevalue`](../37-quakec-builtins-reference/11-server-browser-builtins.md#gethostcachevalue) | `float(float type) gethostcachevalue = #611;` | Браузер серверов и мастер-сервер |
| [`refreshhostcache`](../37-quakec-builtins-reference/11-server-browser-builtins.md#refreshhostcache) | `void(optional float dopurge) refreshhostcache = #620;` | Браузер серверов и мастер-сервер |
| [`resethostcachemasks`](../37-quakec-builtins-reference/11-server-browser-builtins.md#resethostcachemasks) | `void() resethostcachemasks = #615;` | Браузер серверов и мастер-сервер |
| [`resorthostcache`](../37-quakec-builtins-reference/11-server-browser-builtins.md#resorthostcache) | `void() resorthostcache = #618;` | Браузер серверов и мастер-сервер |
| [`sethostcachemasknumber`](../37-quakec-builtins-reference/11-server-browser-builtins.md#sethostcachemasknumber) | `void(float mask, float fld, float num, float op) sethostcachemasknumber = #617;` | Браузер серверов и мастер-сервер |
| [`sethostcachemaskstring`](../37-quakec-builtins-reference/11-server-browser-builtins.md#sethostcachemaskstring) | `void(float mask, float fld, string str, float op) sethostcachemaskstring = #616;` | Браузер серверов и мастер-сервер |
| [`sethostcachesort`](../37-quakec-builtins-reference/11-server-browser-builtins.md#sethostcachesort) | `void(float fld, float descending) sethostcachesort = #619;` | Браузер серверов и мастер-сервер |
| [`getgamedirinfo`](../37-quakec-builtins-reference/11-server-browser-builtins.md#getgamedirinfo) | `string(float n, float prop) getgamedirinfo = #626;` | Браузер серверов и мастер-сервер |
| [`getextresponse`](../37-quakec-builtins-reference/11-server-browser-builtins.md#getextresponse) | `string() getextresponse = #624;` | Браузер серверов и мастер-сервер |
| [`calltimeofday`](../37-quakec-builtins-reference/11-server-browser-builtins.md#calltimeofday) | `__deprecated("Use strftime.") void() calltimeofday = #231;` | Браузер серверов и мастер-сервер |
| [`openportal`](../37-quakec-builtins-reference/11-server-browser-builtins.md#openportal) | `void(entity portal, float state) openportal = #207;` | Браузер серверов и мастер-сервер |
| [`getpackagemanagerinfo`](../37-quakec-builtins-reference/11-server-browser-builtins.md#getpackagemanagerinfo) | `getpackagemanagerinfo(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Браузер серверов и мастер-сервер |
| [`error`](../37-quakec-builtins-reference/12-system-debug-builtins.md#error) | `void(string err, ...) error = #10;` | Системные функции, отладка и cvar |
| [`objerror`](../37-quakec-builtins-reference/12-system-debug-builtins.md#objerror) | `void(string err, ...) objerror = #11;` | Системные функции, отладка и cvar |
| [`print`](../37-quakec-builtins-reference/12-system-debug-builtins.md#print) | `void(string s, ...) print = #339;` | Системные функции, отладка и cvar |
| [`bprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#bprint) | `void(float msglvl, string s, optional string s2, optional string s3, optional string s4, optional string s5, optional string s6, optional string s7) bprint = #23;` | Системные функции, отладка и cvar |
| [`msprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#msprint) | `void(float clientnum, string text, ...) msprint = #6;` | Системные функции, отладка и cvar |
| [`cprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cprint) | `void(string s, ...) cprint = #338;` | Системные функции, отладка и cvar |
| [`sprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#sprint) | `void(entity client, float msglvl, string s, optional string s2, optional string s3, optional string s4, optional string s5, optional string s6) sprint = #24;` | Системные функции, отладка и cvar |
| [`centerprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#centerprint) | `void(entity ent, string text, optional string text2, optional string text3, optional string text4, optional string text5, optional string text6, optional string text7) centerprint = #73;` | Системные функции, отладка и cvar |
| [`dprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#dprint) | `void(string s, ...) dprint = #25;` | Системные функции, отладка и cvar |
| [`coredump`](../37-quakec-builtins-reference/12-system-debug-builtins.md#coredump) | `void() coredump = #28;` | Системные функции, отладка и cvar |
| [`crash`](../37-quakec-builtins-reference/12-system-debug-builtins.md#crash) | `void() crash = #72;` | Системные функции, отладка и cvar |
| [`stackdump`](../37-quakec-builtins-reference/12-system-debug-builtins.md#stackdump) | `void() stackdump = #73;` | Системные функции, отладка и cvar |
| [`breakpoint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#breakpoint) | `void() breakpoint = #6;` | Системные функции, отладка и cvar |
| [`cvar`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar) | `float(string name) cvar = #45;` | Системные функции, отладка и cvar |
| [`cvar_set`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar_set) | `void(string cvarname, string valuetoset) cvar_set = #72;` | Системные функции, отладка и cvar |
| [`cvar_setf`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar_setf) | `void(string cvar, float val) cvar_setf = #176;` | Системные функции, отладка и cvar |
| [`cvar_string`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar_string) | `string(string cvarname) cvar_string = #448;` | Системные функции, отладка и cvar |
| [`cvar_defstring`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar_defstring) | `string(string name) cvar_defstring = #482;` | Системные функции, отладка и cvar |
| [`cvar_description`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar_description) | `string(string cvarname) cvar_description = #518;` | Системные функции, отладка и cvar |
| [`cvar_type`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar_type) | `float(string name) cvar_type = #495;` | Системные функции, отладка и cvar |
| [`registercvar`](../37-quakec-builtins-reference/12-system-debug-builtins.md#registercvar) | `float(string name, string value, float flags) registercvar = #42;` | Системные функции, отладка и cvar |
| [`checkextension`](../37-quakec-builtins-reference/12-system-debug-builtins.md#checkextension) | `float(string extname) checkextension = #99;` | Системные функции, отладка и cvar |
| [`logfrag`](../37-quakec-builtins-reference/12-system-debug-builtins.md#logfrag) | `void(entity killer, entity killee) logfrag = #79;` | Системные функции, отладка и cvar |
| [`setpause`](../37-quakec-builtins-reference/12-system-debug-builtins.md#setpause) | `void(float pause) setpause = #531;` | Системные функции, отладка и cvar |
| [`localcmd`](../37-quakec-builtins-reference/12-system-debug-builtins.md#localcmd) | `void(string s, ...) localcmd = #46;` | Системные функции, отладка и cvar |
| [`abort`](../37-quakec-builtins-reference/12-system-debug-builtins.md#abort) | `abort(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#211, MenuQC=#211).` | Системные функции, отладка и cvar |
| [`argc`](../37-quakec-builtins-reference/12-system-debug-builtins.md#argc) | `argc(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Системные функции, отладка и cvar |
| [`checkbuiltin`](../37-quakec-builtins-reference/12-system-debug-builtins.md#checkbuiltin) | `checkbuiltin(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Системные функции, отладка и cvar |
| [`externrefcall`](../37-quakec-builtins-reference/12-system-debug-builtins.md#externrefcall) | `externrefcall(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#205).` | Системные функции, отладка и cvar |
| [`addentity`](../37-quakec-builtins-reference/13-menuqc-builtins.md#addentity) | `void(entity ent) addentity = #302;` | Функции MenuQC (меню, экран загрузки) |
| [`addentities`](../37-quakec-builtins-reference/13-menuqc-builtins.md#addentities) | `void(float mask) addentities = #301;` | Функции MenuQC (меню, экран загрузки) |
| [`clearscene`](../37-quakec-builtins-reference/13-menuqc-builtins.md#clearscene) | `void() clearscene = #300;` | Функции MenuQC (меню, экран загрузки) |
| [`renderscene`](../37-quakec-builtins-reference/13-menuqc-builtins.md#renderscene) | `void() renderscene = #304;` | Функции MenuQC (меню, экран загрузки) |
| [`getproperty`](../37-quakec-builtins-reference/13-menuqc-builtins.md#getproperty) | `__variant(float property) getproperty = #309;` (алиас `getviewprop`)` | Функции MenuQC (меню, экран загрузки) |
| [`setproperty`](../37-quakec-builtins-reference/13-menuqc-builtins.md#setproperty) | `float(float property, ...) setproperty = #303;` (алиас `setviewprop`)` | Функции MenuQC (меню, экран загрузки) |
| [`getresolution`](../37-quakec-builtins-reference/13-menuqc-builtins.md#getresolution) | `vector(float vidmode, optional float forfullscreen) getresolution = #608;` | Функции MenuQC (меню, экран загрузки) |
| [`R_BeginPolygon`](../37-quakec-builtins-reference/13-menuqc-builtins.md#r_beginpolygon) | `void(string texturename, optional float flags, optional float is2d) R_BeginPolygon = #306;` | Функции MenuQC (меню, экран загрузки) |
| [`R_EndPolygon`](../37-quakec-builtins-reference/13-menuqc-builtins.md#r_endpolygon) | `void() R_EndPolygon = #308;` | Функции MenuQC (меню, экран загрузки) |
| [`R_PolygonVertex`](../37-quakec-builtins-reference/13-menuqc-builtins.md#r_polygonvertex) | `void(vector org, vector texcoords, vector rgb, float alpha) R_PolygonVertex = #307;` | Функции MenuQC (меню, экран загрузки) |
| [`drawcharacter`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawcharacter) | `float(vector position, float character, vector scale, vector rgb, float alpha, optional float flag) drawcharacter = #454;` | Функции MenuQC (меню, экран загрузки) |
| [`drawfill`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawfill) | `float(vector position, vector size, vector rgb, float alpha, optional float flag) drawfill = #457;` | Функции MenuQC (меню, экран загрузки) |
| [`drawline`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawline) | `void(float width, vector pos1, vector pos2) drawline = #466;` | Функции MenuQC (меню, экран загрузки) |
| [`drawpic`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawpic) | `float(vector position, string pic, vector size, vector rgb, float alpha, optional float flag) drawpic = #456;` | Функции MenuQC (меню, экран загрузки) |
| [`drawrawstring`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawrawstring) | `float(vector position, string text, vector scale, vector rgb, float alpha, optional float flag) drawrawstring = #455;` | Функции MenuQC (меню, экран загрузки) |
| [`drawresetcliparea`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawresetcliparea) | `void(void) drawresetcliparea = #459;` | Функции MenuQC (меню, экран загрузки) |
| [`drawsetcliparea`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawsetcliparea) | `void(float x, float y, float width, float height) drawsetcliparea = #458;` | Функции MenuQC (меню, экран загрузки) |
| [`drawstring`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawstring) | `float(vector position, string text, vector scale, vector rgb, float alpha, float flag) drawstring = #467;` | Функции MenuQC (меню, экран загрузки) |
| [`drawsubpic`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawsubpic) | `void(vector pos, vector sz, string pic, vector srcpos, vector srcsz, vector rgb, float alpha, float flag) drawsubpic = #469;` | Функции MenuQC (меню, экран загрузки) |
| [`iscachedpic`](../37-quakec-builtins-reference/13-menuqc-builtins.md#iscachedpic) | `float(string name) iscachedpic = #451;` | Функции MenuQC (меню, экран загрузки) |
| [`drawgetimagesize`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawgetimagesize) | `vector(string picname) drawgetimagesize = #460;` | Функции MenuQC (меню, экран загрузки) |
| [`stringwidth`](../37-quakec-builtins-reference/13-menuqc-builtins.md#stringwidth) | `float(string text, float usecolours, optional vector fontsize) stringwidth = #468;` | Функции MenuQC (меню, экран загрузки) |
| [`con_draw`](../37-quakec-builtins-reference/13-menuqc-builtins.md#con_draw) | `void(string conname, vector pos, vector size, float fontsize) con_draw = #393;` | Функции MenuQC (меню, экран загрузки) |
| [`con_getset`](../37-quakec-builtins-reference/13-menuqc-builtins.md#con_getset) | `string(string conname, string field, optional string newvalue) con_getset = #391;` | Функции MenuQC (меню, экран загрузки) |
| [`con_input`](../37-quakec-builtins-reference/13-menuqc-builtins.md#con_input) | `float(string conname, float inevtype, float parama, float paramb, float paramc) con_input = #394;` | Функции MenuQC (меню, экран загрузки) |
| [`con_printf`](../37-quakec-builtins-reference/13-menuqc-builtins.md#con_printf) | `void(string conname, string messagefmt, ...) con_printf = #392;` | Функции MenuQC (меню, экран загрузки) |
| [`dynamiclight_add`](../37-quakec-builtins-reference/13-menuqc-builtins.md#dynamiclight_add) | `float(vector org, float radius, vector lightcolours, optional float style, optional string cubemapname, optional float pflags) dynamiclight_add = #305;` | Функции MenuQC (меню, экран загрузки) |
| [`dynamiclight_get`](../37-quakec-builtins-reference/13-menuqc-builtins.md#dynamiclight_get) | `__variant(float lno, float fld) dynamiclight_get = #372;` | Функции MenuQC (меню, экран загрузки) |
| [`dynamiclight_set`](../37-quakec-builtins-reference/13-menuqc-builtins.md#dynamiclight_set) | `void(float lno, float fld, __variant value) dynamiclight_set = #373;` | Функции MenuQC (меню, экран загрузки) |
| [`getkeybind`](../37-quakec-builtins-reference/13-menuqc-builtins.md#getkeybind) | `string(float keynum) getkeybind = #342;` | Функции MenuQC (меню, экран загрузки) |
| [`setkeybind`](../37-quakec-builtins-reference/13-menuqc-builtins.md#setkeybind) | `float(float key, string bind, optional float bindmap, optional float modifier) setkeybind = #630;` | Функции MenuQC (меню, экран загрузки) |
| [`getkeydest`](../37-quakec-builtins-reference/13-menuqc-builtins.md#getkeydest) | `float() getkeydest = #602;` | Функции MenuQC (меню, экран загрузки) |
| [`setkeydest`](../37-quakec-builtins-reference/13-menuqc-builtins.md#setkeydest) | `void(float dest) setkeydest = #601;` | Функции MenuQC (меню, экран загрузки) |
| [`getbindmaps`](../37-quakec-builtins-reference/13-menuqc-builtins.md#getbindmaps) | `vector() getbindmaps = #631;` | Функции MenuQC (меню, экран загрузки) |
| [`setbindmaps`](../37-quakec-builtins-reference/13-menuqc-builtins.md#setbindmaps) | `float(vector bm) setbindmaps = #632;` | Функции MenuQC (меню, экран загрузки) |
| [`getmousepos`](../37-quakec-builtins-reference/13-menuqc-builtins.md#getmousepos) | `vector() getmousepos = #66;` | Функции MenuQC (меню, экран загрузки) |
| [`setmousetarget`](../37-quakec-builtins-reference/13-menuqc-builtins.md#setmousetarget) | `void(float trg) setmousetarget = #603;` | Функции MenuQC (меню, экран загрузки) |
| [`getmousetarget`](../37-quakec-builtins-reference/13-menuqc-builtins.md#getmousetarget) | `float() getmousetarget = #604;` | Функции MenuQC (меню, экран загрузки) |
| [`setcursormode`](../37-quakec-builtins-reference/13-menuqc-builtins.md#setcursormode) | `void(float usecursor, optional string cursorimage, optional vector hotspot, optional float scale) setcursormode = #343;` | Функции MenuQC (меню, экран загрузки) |
| [`keynumtostring`](../37-quakec-builtins-reference/13-menuqc-builtins.md#keynumtostring) | `string(float keynum) keynumtostring = #609;` | Функции MenuQC (меню, экран загрузки) |
| [`keynumtostring_csqc`](../37-quakec-builtins-reference/13-menuqc-builtins.md#keynumtostring_csqc) | `string(float keynum) keynumtostring_csqc = #340;` | Функции MenuQC (меню, экран загрузки) |
| [`stringtokeynum`](../37-quakec-builtins-reference/13-menuqc-builtins.md#stringtokeynum) | `float(string key) stringtokeynum = #614;` | Функции MenuQC (меню, экран загрузки) |
| [`stringtokeynum_csqc`](../37-quakec-builtins-reference/13-menuqc-builtins.md#stringtokeynum_csqc) | `float(string keyname) stringtokeynum_csqc = #341;` | Функции MenuQC (меню, экран загрузки) |
| [`findkeysforcommand`](../37-quakec-builtins-reference/13-menuqc-builtins.md#findkeysforcommand) | `string(string command, optional float bindmap) findkeysforcommand = #610;` | Функции MenuQC (меню, экран загрузки) |
| [`gecko_create`](../37-quakec-builtins-reference/13-menuqc-builtins.md#gecko_create) | `float(string name, optional string initialURI) gecko_create = #487;` | Функции MenuQC (меню, экран загрузки) |
| [`gecko_destroy`](../37-quakec-builtins-reference/13-menuqc-builtins.md#gecko_destroy) | `void(string name) gecko_destroy = #488;` | Функции MenuQC (меню, экран загрузки) |
| [`gecko_navigate`](../37-quakec-builtins-reference/13-menuqc-builtins.md#gecko_navigate) | `void(string name, string URI) gecko_navigate = #489;` | Функции MenuQC (меню, экран загрузки) |
| [`gecko_keyevent`](../37-quakec-builtins-reference/13-menuqc-builtins.md#gecko_keyevent) | `float(string name, float key, float eventtype, optional float charcode) gecko_keyevent = #490;` | Функции MenuQC (меню, экран загрузки) |
| [`gecko_mousemove`](../37-quakec-builtins-reference/13-menuqc-builtins.md#gecko_mousemove) | `void(string name, float x, float y) gecko_mousemove = #491;` | Функции MenuQC (меню, экран загрузки) |
| [`gecko_resize`](../37-quakec-builtins-reference/13-menuqc-builtins.md#gecko_resize) | `void(string name, float w, float h) gecko_resize = #492;` | Функции MenuQC (меню, экран загрузки) |
| [`gecko_get_texture_extent`](../37-quakec-builtins-reference/13-menuqc-builtins.md#gecko_get_texture_extent) | `vector(string name) gecko_get_texture_extent = #493;` | Функции MenuQC (меню, экран загрузки) |
| [`brush_calcfacepoints`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#brush_calcfacepoints) | `brush_calcfacepoints(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Редактор карт, криптография и разные редкие builtins |
| [`brush_create`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#brush_create) | `brush_create(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Редактор карт, криптография и разные редкие builtins |
| [`brush_delete`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#brush_delete) | `brush_delete(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Редактор карт, криптография и разные редкие builtins |
| [`brush_findinvolume`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#brush_findinvolume) | `brush_findinvolume(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Редактор карт, криптография и разные редкие builtins |
| [`brush_get`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#brush_get) | `brush_get(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Редактор карт, криптография и разные редкие builtins |
| [`brush_getfacepoints`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#brush_getfacepoints) | `brush_getfacepoints(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Редактор карт, криптография и разные редкие builtins |
| [`brush_selected`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#brush_selected) | `brush_selected(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Редактор карт, криптография и разные редкие builtins |
| [`bulleten`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#bulleten) | `bulleten(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#243).` | Редактор карт, криптография и разные редкие builtins |
| [`cin_close`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#cin_close) | `cin_close(...)` — сигнатура в си-таблице движка не хранится текстом (номера: MenuQC=#462).` | Редактор карт, криптография и разные редкие builtins |
| [`cin_getstate`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#cin_getstate) | `cin_getstate(...)` — сигнатура в си-таблице движка не хранится текстом (номера: MenuQC=#464).` | Редактор карт, криптография и разные редкие builtins |
| [`cin_open`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#cin_open) | `cin_open(...)` — сигнатура в си-таблице движка не хранится текстом (номера: MenuQC=#461).` | Редактор карт, криптография и разные редкие builtins |
| [`cin_restart`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#cin_restart) | `cin_restart(...)` — сигнатура в си-таблице движка не хранится текстом (номера: MenuQC=#465).` | Редактор карт, криптография и разные редкие builtins |
| [`cin_setstate`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#cin_setstate) | `cin_setstate(...)` — сигнатура в си-таблице движка не хранится текстом (номера: MenuQC=#463).` | Редактор карт, криптография и разные редкие builtins |
| [`controller_query`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#controller_query) | `controller_query(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#740, MenuQC=#740).` | Редактор карт, криптография и разные редкие builtins |
| [`controller_rumble`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#controller_rumble) | `controller_rumble(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#741, MenuQC=#741).` | Редактор карт, криптография и разные редкие builtins |
| [`controller_rumbletriggers`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#controller_rumbletriggers) | `controller_rumbletriggers(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#742, MenuQC=#742).` | Редактор карт, криптография и разные редкие builtins |
| [`crypto_getencryptlevel`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#crypto_getencryptlevel) | `crypto_getencryptlevel(...)` — сигнатура в си-таблице движка не хранится текстом (номера: MenuQC=#635).` | Редактор карт, криптография и разные редкие builtins |
| [`crypto_getidfp`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#crypto_getidfp) | `crypto_getidfp(...)` — сигнатура в си-таблице движка не хранится текстом (номера: MenuQC=#634).` | Редактор карт, криптография и разные редкие builtins |
| [`crypto_getidstatus`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#crypto_getidstatus) | `crypto_getidstatus(...)` — сигнатура в си-таблице движка не хранится текстом (номера: MenuQC=#643).` | Редактор карт, криптография и разные редкие builtins |
| [`crypto_getkeyfp`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#crypto_getkeyfp) | `crypto_getkeyfp(...)` — сигнатура в си-таблице движка не хранится текстом (номера: MenuQC=#633).` | Редактор карт, криптография и разные редкие builtins |
| [`crypto_getmyidfp`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#crypto_getmyidfp) | `crypto_getmyidfp(...)` — сигнатура в си-таблице движка не хранится текстом (номера: MenuQC=#637).` | Редактор карт, криптография и разные редкие builtins |
| [`crypto_getmyidstatus`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#crypto_getmyidstatus) | `crypto_getmyidstatus(...)` — сигнатура в си-таблице движка не хранится текстом (номера: MenuQC=#641).` | Редактор карт, криптография и разные редкие builtins |
| [`crypto_getmykeyfp`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#crypto_getmykeyfp) | `crypto_getmykeyfp(...)` — сигнатура в си-таблице движка не хранится текстом (номера: MenuQC=#636).` | Редактор карт, криптография и разные редкие builtins |
| [`free_pic`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#free_pic) | `free_pic(...)` — сигнатура в си-таблице движка не хранится текстом (номера: MenuQC=#453).` | Редактор карт, криптография и разные редкие builtins |
| [`gettime`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#gettime) | `float(optional float timetype) gettime = #519;` | Редактор карт, криптография и разные редкие builtins |
| [`gettimed`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#gettimed) | `__double(optional int timetype) gettimed = #0;` | Редактор карт, криптография и разные редкие builtins |
| [`gettimef`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#gettimef) | `gettimef(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#519).` | Редактор карт, криптография и разные редкие builtins |
| [`gp_getlayout`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#gp_getlayout) | `gp_getlayout(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Редактор карт, криптография и разные редкие builtins |
| [`gp_rumble`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#gp_rumble) | `gp_rumble(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Редактор карт, криптография и разные редкие builtins |
| [`gp_rumbletriggers`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#gp_rumbletriggers) | `gp_rumbletriggers(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Редактор карт, криптография и разные редкие builtins |
| [`gp_setledcolor`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#gp_setledcolor) | `gp_setledcolor(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Редактор карт, криптография и разные редкие builtins |
| [`gp_settriggerfx`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#gp_settriggerfx) | `gp_settriggerfx(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Редактор карт, криптография и разные редкие builtins |
| [`js_run_script`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#js_run_script) | `js_run_script(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Редактор карт, криптография и разные редкие builtins |
| [`map_builtin`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#map_builtin) | `map_builtin(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#220).` | Редактор карт, криптография и разные редкие builtins |
| [`patch_create`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#patch_create) | `patch_create(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Редактор карт, криптография и разные редкие builtins |
| [`patch_evaluate`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#patch_evaluate) | `patch_evaluate(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Редактор карт, криптография и разные редкие builtins |
| [`patch_getcp`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#patch_getcp) | `patch_getcp(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Редактор карт, криптография и разные редкие builtins |
| [`patch_getmesh`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#patch_getmesh) | `patch_getmesh(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0).` | Редактор карт, криптография и разные редкие builtins |
| [`print_csqc`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#print_csqc) | `print_csqc(...)` — сигнатура в си-таблице движка не хранится текстом (номера: MenuQC=#339).` | Редактор карт, криптография и разные редкие builtins |
| [`removeinstant`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#removeinstant) | `removeinstant(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Редактор карт, криптография и разные редкие builtins |
| [`setwatchpoint`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#setwatchpoint) | `setwatchpoint(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#0, MenuQC=#0).` | Редактор карт, криптография и разные редкие builtins |
| [`stachievement_query`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#stachievement_query) | `stachievement_query(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#731, MenuQC=#731).` | Редактор карт, криптография и разные редкие builtins |
| [`stachievement_register`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#stachievement_register) | `stachievement_register(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#735, MenuQC=#735).` | Редактор карт, криптография и разные редкие builtins |
| [`stachievement_unlock`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#stachievement_unlock) | `stachievement_unlock(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#730, MenuQC=#730).` | Редактор карт, криптография и разные редкие builtins |
| [`ststat_increment`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#ststat_increment) | `ststat_increment(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#733, MenuQC=#733).` | Редактор карт, криптография и разные редкие builtins |
| [`ststat_query`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#ststat_query) | `ststat_query(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#734, MenuQC=#734).` | Редактор карт, криптография и разные редкие builtins |
| [`ststat_register`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#ststat_register) | `ststat_register(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#736, MenuQC=#736).` | Редактор карт, криптография и разные редкие builtins |
| [`ststat_setvalue`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#ststat_setvalue) | `ststat_setvalue(...)` — сигнатура в си-таблице движка не хранится текстом (номера: CSQC=#732, MenuQC=#732).` | Редактор карт, криптография и разные редкие builtins |
| [`videoplaying`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#videoplaying) | `videoplaying(...)` — сигнатура в си-таблице движка не хранится текстом (номера: MenuQC=#355).` | Редактор карт, криптография и разные редкие builtins |

## Переменные движка (cvar)

Всего задокументировано: **1279** cvar. Полный постатейный разбор — в разделе [«38. Переменные движка»](../38-cvars-reference/README.md).

| Функция | Сигнатура | Категория |
|---|---|---|
| [`crosshair`](../38-cvars-reference/01-video-rendering-cvars.md#crosshair) | `cvar crosshair(boolean/int, "1")` | Видео, экран и общий рендеринг |
| [`crosshaircorrect`](../38-cvars-reference/01-video-rendering-cvars.md#crosshaircorrect) | `cvar crosshaircorrect(boolean/int, "0")` | Видео, экран и общий рендеринг |
| [`crosshairimage`](../38-cvars-reference/01-video-rendering-cvars.md#crosshairimage) | `cvar crosshairimage(string, "")` | Видео, экран и общий рендеринг |
| [`crosshairsize`](../38-cvars-reference/01-video-rendering-cvars.md#crosshairsize) | `cvar crosshairsize(int, "8")` | Видео, экран и общий рендеринг |
| [`d_lodbias`](../38-cvars-reference/01-video-rendering-cvars.md#d_lodbias) | `cvar d_lodbias(float, "0")` | Видео, экран и общий рендеринг |
| [`ffov`](../38-cvars-reference/01-video-rendering-cvars.md#ffov) | `cvar ffov(float/string, "")` | Видео, экран и общий рендеринг |
| [`fov`](../38-cvars-reference/01-video-rendering-cvars.md#fov) | `cvar fov(float, "90")` | Видео, экран и общий рендеринг |
| [`gl_affinemodels`](../38-cvars-reference/01-video-rendering-cvars.md#gl_affinemodels) | `cvar gl_affinemodels(boolean/int, "0")` | Видео, экран и общий рендеринг |
| [`gl_compress`](../38-cvars-reference/01-video-rendering-cvars.md#gl_compress) | `cvar gl_compress(boolean/int, "0")` | Видео, экран и общий рендеринг |
| [`gl_dither`](../38-cvars-reference/01-video-rendering-cvars.md#gl_dither) | `cvar gl_dither(boolean/int, "1")` | Видео, экран и общий рендеринг |
| [`gl_finish`](../38-cvars-reference/01-video-rendering-cvars.md#gl_finish) | `cvar gl_finish(boolean/int, "0")` | Видео, экран и общий рендеринг |
| [`gl_lerpimages`](../38-cvars-reference/01-video-rendering-cvars.md#gl_lerpimages) | `cvar gl_lerpimages(boolean/int, "1")` | Видео, экран и общий рендеринг |
| [`gl_max_size`](../38-cvars-reference/01-video-rendering-cvars.md#gl_max_size) | `cvar gl_max_size(int, "8192")` | Видео, экран и общий рендеринг |
| [`gl_motionblur`](../38-cvars-reference/01-video-rendering-cvars.md#gl_motionblur) | `cvar gl_motionblur(float, "0")` | Видео, экран и общий рендеринг |
| [`gl_motionblurscale`](../38-cvars-reference/01-video-rendering-cvars.md#gl_motionblurscale) | `cvar gl_motionblurscale(float, "1")` | Видео, экран и общий рендеринг |
| [`gl_overbright`](../38-cvars-reference/01-video-rendering-cvars.md#gl_overbright) | `cvar gl_overbright(int, "1")` | Видео, экран и общий рендеринг |
| [`gl_picmip`](../38-cvars-reference/01-video-rendering-cvars.md#gl_picmip) | `cvar gl_picmip(int, "0")` | Видео, экран и общий рендеринг |
| [`gl_picmip2d`](../38-cvars-reference/01-video-rendering-cvars.md#gl_picmip2d) | `cvar gl_picmip2d(int, "0")` | Видео, экран и общий рендеринг |
| [`gl_picmip_other`](../38-cvars-reference/01-video-rendering-cvars.md#gl_picmip_other) | `cvar gl_picmip_other(int, "0")` | Видео, экран и общий рендеринг |
| [`gl_picmip_sprites`](../38-cvars-reference/01-video-rendering-cvars.md#gl_picmip_sprites) | `cvar gl_picmip_sprites(int, "0")` | Видео, экран и общий рендеринг |
| [`gl_picmip_world`](../38-cvars-reference/01-video-rendering-cvars.md#gl_picmip_world) | `cvar gl_picmip_world(int, "0")` | Видео, экран и общий рендеринг |
| [`gl_polyblend`](../38-cvars-reference/01-video-rendering-cvars.md#gl_polyblend) | `cvar gl_polyblend(boolean/int, "1")` | Видео, экран и общий рендеринг |
| [`gl_smoothcrosshair`](../38-cvars-reference/01-video-rendering-cvars.md#gl_smoothcrosshair) | `cvar gl_smoothcrosshair(boolean/int, "1")` | Видео, экран и общий рендеринг |
| [`gl_texture_anisotropy`](../38-cvars-reference/01-video-rendering-cvars.md#gl_texture_anisotropy) | `cvar gl_texture_anisotropy(int, "4")` | Видео, экран и общий рендеринг |
| [`gl_texturemode`](../38-cvars-reference/01-video-rendering-cvars.md#gl_texturemode) | `cvar gl_texturemode(string, "GL_LINEAR_MIPMAP_LINEAR")` | Видео, экран и общий рендеринг |
| [`gl_texturemode2d`](../38-cvars-reference/01-video-rendering-cvars.md#gl_texturemode2d) | `cvar gl_texturemode2d(string, "GL_LINEAR")` | Видео, экран и общий рендеринг |
| [`mod_external_vis`](../38-cvars-reference/01-video-rendering-cvars.md#mod_external_vis) | `cvar mod_external_vis(boolean/int, "1")` | Видео, экран и общий рендеринг |
| [`r_drawflat`](../38-cvars-reference/01-video-rendering-cvars.md#r_drawflat) | `cvar r_drawflat(boolean/int, "0")` | Видео, экран и общий рендеринг |
| [`r_drawviewmodel`](../38-cvars-reference/01-video-rendering-cvars.md#r_drawviewmodel) | `cvar r_drawviewmodel(boolean/int, "1")` | Видео, экран и общий рендеринг |
| [`r_fxaa`](../38-cvars-reference/01-video-rendering-cvars.md#r_fxaa) | `cvar r_fxaa(boolean/int, "0")` | Видео, экран и общий рендеринг |
| [`r_lodbias`](../38-cvars-reference/01-video-rendering-cvars.md#r_lodbias) | `cvar r_lodbias(int, "0")` | Видео, экран и общий рендеринг |
| [`r_lodscale`](../38-cvars-reference/01-video-rendering-cvars.md#r_lodscale) | `cvar r_lodscale(float, "5")` | Видео, экран и общий рендеринг |
| [`r_noframegrouplerp`](../38-cvars-reference/01-video-rendering-cvars.md#r_noframegrouplerp) | `cvar r_noframegrouplerp(boolean/int, "0")` | Видео, экран и общий рендеринг |
| [`r_nolerp`](../38-cvars-reference/01-video-rendering-cvars.md#r_nolerp) | `cvar r_nolerp(boolean/int, "0")` | Видео, экран и общий рендеринг |
| [`r_projection`](../38-cvars-reference/01-video-rendering-cvars.md#r_projection) | `cvar r_projection(int, "0")` | Видео, экран и общий рендеринг |
| [`r_renderscale`](../38-cvars-reference/01-video-rendering-cvars.md#r_renderscale) | `cvar r_renderscale(float, "1")` | Видео, экран и общий рендеринг |
| [`r_showtris`](../38-cvars-reference/01-video-rendering-cvars.md#r_showtris) | `cvar r_showtris(boolean/int, "0")` | Видео, экран и общий рендеринг |
| [`r_viewmodel_fov`](../38-cvars-reference/01-video-rendering-cvars.md#r_viewmodel_fov) | `cvar r_viewmodel_fov(float/string, "")` | Видео, экран и общий рендеринг |
| [`r_viewmodel_quake`](../38-cvars-reference/01-video-rendering-cvars.md#r_viewmodel_quake) | `cvar r_viewmodel_quake(boolean/int, "0")` | Видео, экран и общий рендеринг |
| [`r_wateralpha`](../38-cvars-reference/01-video-rendering-cvars.md#r_wateralpha) | `cvar r_wateralpha(float, "1")` | Видео, экран и общий рендеринг |
| [`r_waterstyle`](../38-cvars-reference/01-video-rendering-cvars.md#r_waterstyle) | `cvar r_waterstyle(int, "1")` | Видео, экран и общий рендеринг |
| [`r_waterwarp`](../38-cvars-reference/01-video-rendering-cvars.md#r_waterwarp) | `cvar r_waterwarp(float, "1")` | Видео, экран и общий рендеринг |
| [`scr_fov_mode`](../38-cvars-reference/01-video-rendering-cvars.md#scr_fov_mode) | `cvar scr_fov_mode(int, "4")` | Видео, экран и общий рендеринг |
| [`vid_bpp`](../38-cvars-reference/01-video-rendering-cvars.md#vid_bpp) | `cvar vid_bpp(int, "0")` | Видео, экран и общий рендеринг |
| [`vid_conautoscale`](../38-cvars-reference/01-video-rendering-cvars.md#vid_conautoscale) | `cvar vid_conautoscale(float, "0")` | Видео, экран и общий рендеринг |
| [`vid_conheight`](../38-cvars-reference/01-video-rendering-cvars.md#vid_conheight) | `cvar vid_conheight(int, "0")` | Видео, экран и общий рендеринг |
| [`vid_conwidth`](../38-cvars-reference/01-video-rendering-cvars.md#vid_conwidth) | `cvar vid_conwidth(int, "0")` | Видео, экран и общий рендеринг |
| [`vid_depthbits`](../38-cvars-reference/01-video-rendering-cvars.md#vid_depthbits) | `cvar vid_depthbits(int, "0")` | Видео, экран и общий рендеринг |
| [`vid_desktopsettings`](../38-cvars-reference/01-video-rendering-cvars.md#vid_desktopsettings) | `cvar vid_desktopsettings(boolean/int, "0")` | Видео, экран и общий рендеринг |
| [`vid_displayfrequency`](../38-cvars-reference/01-video-rendering-cvars.md#vid_displayfrequency) | `cvar vid_displayfrequency(int, "0")` | Видео, экран и общий рендеринг |
| [`vid_fullscreen`](../38-cvars-reference/01-video-rendering-cvars.md#vid_fullscreen) | `cvar vid_fullscreen(int, "2")` | Видео, экран и общий рендеринг |
| [`vid_hardwaregamma`](../38-cvars-reference/01-video-rendering-cvars.md#vid_hardwaregamma) | `cvar vid_hardwaregamma(int, "1")` | Видео, экран и общий рендеринг |
| [`vid_height`](../38-cvars-reference/01-video-rendering-cvars.md#vid_height) | `cvar vid_height(int, "0")` | Видео, экран и общий рендеринг |
| [`vid_multisample`](../38-cvars-reference/01-video-rendering-cvars.md#vid_multisample) | `cvar vid_multisample(int, "0")` | Видео, экран и общий рендеринг |
| [`vid_renderer`](../38-cvars-reference/01-video-rendering-cvars.md#vid_renderer) | `cvar vid_renderer(string, "")` | Видео, экран и общий рендеринг |
| [`vid_srgb`](../38-cvars-reference/01-video-rendering-cvars.md#vid_srgb) | `cvar vid_srgb(int, "0")` | Видео, экран и общий рендеринг |
| [`vid_vsync`](../38-cvars-reference/01-video-rendering-cvars.md#vid_vsync) | `cvar vid_vsync(int, "0")` | Видео, экран и общий рендеринг |
| [`vid_width`](../38-cvars-reference/01-video-rendering-cvars.md#vid_width) | `cvar vid_width(int, "0")` | Видео, экран и общий рендеринг |
| [`_vid_renderer_opts`](../38-cvars-reference/01-video-rendering-cvars.md#_vid_renderer_opts) | `cvar _vid_renderer_opts(string, "The possible video renderer apis, in \\"value\\" \\"description\\" pairs, for gamecode to read.")` | Видео, экран и общий рендеринг |
| [`brightness`](../38-cvars-reference/01-video-rendering-cvars.md#brightness) | `cvar brightness(float, "0.0")` | Видео, экран и общий рендеринг |
| [`gl_ati_truform_type`](../38-cvars-reference/01-video-rendering-cvars.md#gl_ati_truform_type) | `cvar gl_ati_truform_type(int, "1")` | Видео, экран и общий рендеринг |
| [`gl_blacklist_texture_compression`](../38-cvars-reference/01-video-rendering-cvars.md#gl_blacklist_texture_compression) | `cvar gl_blacklist_texture_compression(int, "0")` | Видео, экран и общий рендеринг |
| [`gl_blend2d`](../38-cvars-reference/01-video-rendering-cvars.md#gl_blend2d) | `cvar gl_blend2d(int, "1")` | Видео, экран и общий рендеринг |
| [`gl_blendsprites`](../38-cvars-reference/01-video-rendering-cvars.md#gl_blendsprites) | `cvar gl_blendsprites(int, "0")` | Видео, экран и общий рендеринг |
| [`gl_conback`](../38-cvars-reference/01-video-rendering-cvars.md#gl_conback) | `cvar gl_conback(string, "")` | Видео, экран и общий рендеринг |
| [`gl_cshiftpercent`](../38-cvars-reference/01-video-rendering-cvars.md#gl_cshiftpercent) | `cvar gl_cshiftpercent(int, "100")` | Видео, экран и общий рендеринг |
| [`gl_detail`](../38-cvars-reference/01-video-rendering-cvars.md#gl_detail) | `cvar gl_detail(int, "0")` | Видео, экран и общий рендеринг |
| [`gl_detailscale`](../38-cvars-reference/01-video-rendering-cvars.md#gl_detailscale) | `cvar gl_detailscale(int, "5")` | Видео, экран и общий рендеринг |
| [`gl_driver`](../38-cvars-reference/01-video-rendering-cvars.md#gl_driver) | `cvar gl_driver(string, "")` | Видео, экран и общий рендеринг |
| [`gl_font`](../38-cvars-reference/01-video-rendering-cvars.md#gl_font) | `cvar gl_font(string, "")` | Видео, экран и общий рендеринг |
| [`gl_immutable_buffers`](../38-cvars-reference/01-video-rendering-cvars.md#gl_immutable_buffers) | `cvar gl_immutable_buffers(int, "1")` | Видео, экран и общий рендеринг |
| [`gl_immutable_textures`](../38-cvars-reference/01-video-rendering-cvars.md#gl_immutable_textures) | `cvar gl_immutable_textures(int, "1")` | Видео, экран и общий рендеринг |
| [`gl_lateswap`](../38-cvars-reference/01-video-rendering-cvars.md#gl_lateswap) | `cvar gl_lateswap(int, "0")` | Видео, экран и общий рендеринг |
| [`gl_lightmap_average`](../38-cvars-reference/01-video-rendering-cvars.md#gl_lightmap_average) | `cvar gl_lightmap_average(int, "0")` | Видео, экран и общий рендеринг |
| [`gl_lightmap_nearest`](../38-cvars-reference/01-video-rendering-cvars.md#gl_lightmap_nearest) | `cvar gl_lightmap_nearest(int, "0")` | Видео, экран и общий рендеринг |
| [`gl_load24bit`](../38-cvars-reference/01-video-rendering-cvars.md#gl_load24bit) | `cvar gl_load24bit(int, "1")` | Видео, экран и общий рендеринг |
| [`gl_maxdist`](../38-cvars-reference/01-video-rendering-cvars.md#gl_maxdist) | `cvar gl_maxdist(int, "0")` | Видео, экран и общий рендеринг |
| [`gl_mindist`](../38-cvars-reference/01-video-rendering-cvars.md#gl_mindist) | `cvar gl_mindist(int, "1")` | Видео, экран и общий рендеринг |
| [`gl_nocolors`](../38-cvars-reference/01-video-rendering-cvars.md#gl_nocolors) | `cvar gl_nocolors(int, "0")` | Видео, экран и общий рендеринг |
| [`gl_nohwblend`](../38-cvars-reference/01-video-rendering-cvars.md#gl_nohwblend) | `cvar gl_nohwblend(int, "1")` | Видео, экран и общий рендеринг |
| [`gl_outline`](../38-cvars-reference/01-video-rendering-cvars.md#gl_outline) | `cvar gl_outline(int, "0")` | Видео, экран и общий рендеринг |
| [`gl_outline_width`](../38-cvars-reference/01-video-rendering-cvars.md#gl_outline_width) | `cvar gl_outline_width(int, "2")` | Видео, экран и общий рендеринг |
| [`gl_overbright_all`](../38-cvars-reference/01-video-rendering-cvars.md#gl_overbright_all) | `cvar gl_overbright_all(int, "0")` | Видео, экран и общий рендеринг |
| [`gl_overbright_models`](../38-cvars-reference/01-video-rendering-cvars.md#gl_overbright_models) | `cvar gl_overbright_models(int, "0")` | Видео, экран и общий рендеринг |
| [`gl_part_flame`](../38-cvars-reference/01-video-rendering-cvars.md#gl_part_flame) | `cvar gl_part_flame(int, "1")` | Видео, экран и общий рендеринг |
| [`gl_pbolightmaps`](../38-cvars-reference/01-video-rendering-cvars.md#gl_pbolightmaps) | `cvar gl_pbolightmaps(int, "1")` | Видео, экран и общий рендеринг |
| [`gl_polyblend_edgesize`](../38-cvars-reference/01-video-rendering-cvars.md#gl_polyblend_edgesize) | `cvar gl_polyblend_edgesize(int, "128")` | Видео, экран и общий рендеринг |
| [`gl_schematics`](../38-cvars-reference/01-video-rendering-cvars.md#gl_schematics) | `cvar gl_schematics(int, "0")` | Видео, экран и общий рендеринг |
| [`gl_screenangle`](../38-cvars-reference/01-video-rendering-cvars.md#gl_screenangle) | `cvar gl_screenangle(int, "0")` | Видео, экран и общий рендеринг |
| [`gl_shadeq1_name`](../38-cvars-reference/01-video-rendering-cvars.md#gl_shadeq1_name) | `cvar gl_shadeq1_name(string, "*")` | Видео, экран и общий рендеринг |
| [`gl_shaftlight`](../38-cvars-reference/01-video-rendering-cvars.md#gl_shaftlight) | `cvar gl_shaftlight(float, "0.8")` | Видео, экран и общий рендеринг |
| [`gl_simpleitems`](../38-cvars-reference/01-video-rendering-cvars.md#gl_simpleitems) | `cvar gl_simpleitems(int, "0")` | Видео, экран и общий рендеринг |
| [`gl_specular`](../38-cvars-reference/01-video-rendering-cvars.md#gl_specular) | `cvar gl_specular(float, "0.3")` | Видео, экран и общий рендеринг |
| [`gl_specular_fallback`](../38-cvars-reference/01-video-rendering-cvars.md#gl_specular_fallback) | `cvar gl_specular_fallback(float, "0.05")` | Видео, экран и общий рендеринг |
| [`gl_specular_fallbackexp`](../38-cvars-reference/01-video-rendering-cvars.md#gl_specular_fallbackexp) | `cvar gl_specular_fallbackexp(int, "1")` | Видео, экран и общий рендеринг |
| [`gl_specular_power`](../38-cvars-reference/01-video-rendering-cvars.md#gl_specular_power) | `cvar gl_specular_power(int, "32")` | Видео, экран и общий рендеринг |
| [`msg_filter_frags`](../38-cvars-reference/01-video-rendering-cvars.md#msg_filter_frags) | `cvar msg_filter_frags(int, "0")` | Видео, экран и общий рендеринг |
| [`msg_filter_pickups`](../38-cvars-reference/01-video-rendering-cvars.md#msg_filter_pickups) | `cvar msg_filter_pickups(int, "0")` | Видео, экран и общий рендеринг |
| [`pr_allowbutton1`](../38-cvars-reference/01-video-rendering-cvars.md#pr_allowbutton1) | `cvar pr_allowbutton1(int, "1")` | Видео, экран и общий рендеринг |
| [`pr_autocreatecvars`](../38-cvars-reference/01-video-rendering-cvars.md#pr_autocreatecvars) | `cvar pr_autocreatecvars(int, "1")` | Видео, экран и общий рендеринг |
| [`pr_brokenfloatconvert`](../38-cvars-reference/01-video-rendering-cvars.md#pr_brokenfloatconvert) | `cvar pr_brokenfloatconvert(int, "0")` | Видео, экран и общий рендеринг |
| [`pr_compatabilitytest`](../38-cvars-reference/01-video-rendering-cvars.md#pr_compatabilitytest) | `cvar pr_compatabilitytest(int, "0")` | Видео, экран и общий рендеринг |
| [`pr_coreonerror`](../38-cvars-reference/01-video-rendering-cvars.md#pr_coreonerror) | `cvar pr_coreonerror(int, "1")` | Видео, экран и общий рендеринг |
| [`pr_csqc_coreonerror`](../38-cvars-reference/01-video-rendering-cvars.md#pr_csqc_coreonerror) | `cvar pr_csqc_coreonerror(int, "1")` | Видео, экран и общий рендеринг |
| [`pr_csqc_formenus`](../38-cvars-reference/01-video-rendering-cvars.md#pr_csqc_formenus) | `cvar pr_csqc_formenus(int, "1")` | Видео, экран и общий рендеринг |
| [`pr_csqc_maxedicts`](../38-cvars-reference/01-video-rendering-cvars.md#pr_csqc_maxedicts) | `cvar pr_csqc_maxedicts(int, "65536")` | Видео, экран и общий рендеринг |
| [`pr_csqc_memsize`](../38-cvars-reference/01-video-rendering-cvars.md#pr_csqc_memsize) | `cvar pr_csqc_memsize(int, "-1")` | Видео, экран и общий рендеринг |
| [`pr_debugger`](../38-cvars-reference/01-video-rendering-cvars.md#pr_debugger) | `cvar pr_debugger(string, "debugger")` | Видео, экран и общий рендеринг |
| [`pr_droptofloorunits`](../38-cvars-reference/01-video-rendering-cvars.md#pr_droptofloorunits) | `cvar pr_droptofloorunits(int, "256")` | Видео, экран и общий рендеринг |
| [`pr_enable_profiling`](../38-cvars-reference/01-video-rendering-cvars.md#pr_enable_profiling) | `cvar pr_enable_profiling(int, "0")` | Видео, экран и общий рендеринг |
| [`pr_enable_uriget`](../38-cvars-reference/01-video-rendering-cvars.md#pr_enable_uriget) | `cvar pr_enable_uriget(int, "1")` | Видео, экран и общий рендеринг |
| [`pr_engine`](../38-cvars-reference/01-video-rendering-cvars.md#pr_engine) | `cvar pr_engine(string, " -")` | Видео, экран и общий рендеринг |
| [`pr_ext_dp_qc_getsurface`](../38-cvars-reference/01-video-rendering-cvars.md#pr_ext_dp_qc_getsurface) | `cvar pr_ext_dp_qc_getsurface(string, "")` | Видео, экран и общий рендеринг |
| [`pr_fixbrokenqccarrays`](../38-cvars-reference/01-video-rendering-cvars.md#pr_fixbrokenqccarrays) | `cvar pr_fixbrokenqccarrays(int, "0")` | Видео, экран и общий рендеринг |
| [`pr_gc_threaded`](../38-cvars-reference/01-video-rendering-cvars.md#pr_gc_threaded) | `cvar pr_gc_threaded(int, "1")` | Видео, экран и общий рендеринг |
| [`pr_imitatemvdsv`](../38-cvars-reference/01-video-rendering-cvars.md#pr_imitatemvdsv) | `cvar pr_imitatemvdsv(int, "0")` | Видео, экран и общий рендеринг |
| [`pr_maxedicts`](../38-cvars-reference/01-video-rendering-cvars.md#pr_maxedicts) | `cvar pr_maxedicts(int, "131072")` | Видео, экран и общий рендеринг |
| [`pr_no_parsecommand`](../38-cvars-reference/01-video-rendering-cvars.md#pr_no_parsecommand) | `cvar pr_no_parsecommand(int, "0")` | Видео, экран и общий рендеринг |
| [`pr_no_playerphysics`](../38-cvars-reference/01-video-rendering-cvars.md#pr_no_playerphysics) | `cvar pr_no_playerphysics(int, "1")` | Видео, экран и общий рендеринг |
| [`pr_nonetaccess`](../38-cvars-reference/01-video-rendering-cvars.md#pr_nonetaccess) | `cvar pr_nonetaccess(int, "0")` | Видео, экран и общий рендеринг |
| [`pr_overridebuiltins`](../38-cvars-reference/01-video-rendering-cvars.md#pr_overridebuiltins) | `cvar pr_overridebuiltins(int, "1")` | Видео, экран и общий рендеринг |
| [`pr_precachepic_slow`](../38-cvars-reference/01-video-rendering-cvars.md#pr_precachepic_slow) | `cvar pr_precachepic_slow(int, "0")` | Видео, экран и общий рендеринг |
| [`pr_sourcedir`](../38-cvars-reference/01-video-rendering-cvars.md#pr_sourcedir) | `cvar pr_sourcedir(string, "src")` | Видео, экран и общий рендеринг |
| [`pr_ssqc_memsize`](../38-cvars-reference/01-video-rendering-cvars.md#pr_ssqc_memsize) | `cvar pr_ssqc_memsize(int, "-1")` | Видео, экран и общий рендеринг |
| [`pr_tempstringcount`](../38-cvars-reference/01-video-rendering-cvars.md#pr_tempstringcount) | `cvar pr_tempstringcount(string, "")` | Видео, экран и общий рендеринг |
| [`pr_tempstringsize`](../38-cvars-reference/01-video-rendering-cvars.md#pr_tempstringsize) | `cvar pr_tempstringsize(int, "4096")` | Видео, экран и общий рендеринг |
| [`r_bloodstains`](../38-cvars-reference/01-video-rendering-cvars.md#r_bloodstains) | `cvar r_bloodstains(int, "1")` | Видео, экран и общий рендеринг |
| [`r_bluelight_colour`](../38-cvars-reference/01-video-rendering-cvars.md#r_bluelight_colour) | `cvar r_bluelight_colour(string, "0.5 0.5 3.0 200")` | Видео, экран и общий рендеринг |
| [`r_bouncysparks`](../38-cvars-reference/01-video-rendering-cvars.md#r_bouncysparks) | `cvar r_bouncysparks(int, "1")` | Видео, экран и общий рендеринг |
| [`r_brightlight_colour`](../38-cvars-reference/01-video-rendering-cvars.md#r_brightlight_colour) | `cvar r_brightlight_colour(string, "2.0 1.0 0.5 400")` | Видео, экран и общий рендеринг |
| [`r_clear`](../38-cvars-reference/01-video-rendering-cvars.md#r_clear) | `cvar r_clear(int, "0")` | Видео, экран и общий рендеринг |
| [`r_clearcolour`](../38-cvars-reference/01-video-rendering-cvars.md#r_clearcolour) | `cvar r_clearcolour(string, "0.12 0.12 0.12")` | Видео, экран и общий рендеринг |
| [`r_clutter_density`](../38-cvars-reference/01-video-rendering-cvars.md#r_clutter_density) | `cvar r_clutter_density(int, "0")` | Видео, экран и общий рендеринг |
| [`r_clutter_distance`](../38-cvars-reference/01-video-rendering-cvars.md#r_clutter_distance) | `cvar r_clutter_distance(int, "1024")` | Видео, экран и общий рендеринг |
| [`r_coronas`](../38-cvars-reference/01-video-rendering-cvars.md#r_coronas) | `cvar r_coronas(int, "0")` | Видео, экран и общий рендеринг |
| [`r_decal_noperpendicular`](../38-cvars-reference/01-video-rendering-cvars.md#r_decal_noperpendicular) | `cvar r_decal_noperpendicular(int, "1")` | Видео, экран и общий рендеринг |
| [`r_deluxemapping`](../38-cvars-reference/01-video-rendering-cvars.md#r_deluxemapping) | `cvar r_deluxemapping(int, "1")` | Видео, экран и общий рендеринг |
| [`r_dimlight_colour`](../38-cvars-reference/01-video-rendering-cvars.md#r_dimlight_colour) | `cvar r_dimlight_colour(string, "2.0 1.0 0.5 200")` | Видео, экран и общий рендеринг |
| [`r_dodgymiptex`](../38-cvars-reference/01-video-rendering-cvars.md#r_dodgymiptex) | `cvar r_dodgymiptex(int, "1")` | Видео, экран и общий рендеринг |
| [`r_dodgypcxfiles`](../38-cvars-reference/01-video-rendering-cvars.md#r_dodgypcxfiles) | `cvar r_dodgypcxfiles(int, "0")` | Видео, экран и общий рендеринг |
| [`r_dodgytgafiles`](../38-cvars-reference/01-video-rendering-cvars.md#r_dodgytgafiles) | `cvar r_dodgytgafiles(int, "0")` | Видео, экран и общий рендеринг |
| [`r_drawentities`](../38-cvars-reference/01-video-rendering-cvars.md#r_drawentities) | `cvar r_drawentities(int, "1")` | Видео, экран и общий рендеринг |
| [`r_drawflame`](../38-cvars-reference/01-video-rendering-cvars.md#r_drawflame) | `cvar r_drawflame(int, "1")` | Видео, экран и общий рендеринг |
| [`r_drawviewmodelinvis`](../38-cvars-reference/01-video-rendering-cvars.md#r_drawviewmodelinvis) | `cvar r_drawviewmodelinvis(int, "0")` | Видео, экран и общий рендеринг |
| [`r_editlights`](../38-cvars-reference/01-video-rendering-cvars.md#r_editlights) | `cvar r_editlights(int, "0")` | Видео, экран и общий рендеринг |
| [`r_explosionlight`](../38-cvars-reference/01-video-rendering-cvars.md#r_explosionlight) | `cvar r_explosionlight(int, "1")` | Видео, экран и общий рендеринг |
| [`r_explosionlight_colour`](../38-cvars-reference/01-video-rendering-cvars.md#r_explosionlight_colour) | `cvar r_explosionlight_colour(string, "4.0 2.0 0.5")` | Видео, экран и общий рендеринг |
| [`r_explosionlight_fade`](../38-cvars-reference/01-video-rendering-cvars.md#r_explosionlight_fade) | `cvar r_explosionlight_fade(string, "0.784 0.92 0.48")` | Видео, экран и общий рендеринг |
| [`r_fastturb`](../38-cvars-reference/01-video-rendering-cvars.md#r_fastturb) | `cvar r_fastturb(int, "0")` | Видео, экран и общий рендеринг |
| [`r_fastturbcolour`](../38-cvars-reference/01-video-rendering-cvars.md#r_fastturbcolour) | `cvar r_fastturbcolour(string, "0.1 0.2 0.3")` | Видео, экран и общий рендеринг |
| [`r_fb_bmodels`](../38-cvars-reference/01-video-rendering-cvars.md#r_fb_bmodels) | `cvar r_fb_bmodels(int, "1")` | Видео, экран и общий рендеринг |
| [`r_fb_models`](../38-cvars-reference/01-video-rendering-cvars.md#r_fb_models) | `cvar r_fb_models(int, "1")` | Видео, экран и общий рендеринг |
| [`r_floorcolour`](../38-cvars-reference/01-video-rendering-cvars.md#r_floorcolour) | `cvar r_floorcolour(string, "64 64 128")` | Видео, экран и общий рендеринг |
| [`r_fog_cullentities`](../38-cvars-reference/01-video-rendering-cvars.md#r_fog_cullentities) | `cvar r_fog_cullentities(int, "1")` | Видео, экран и общий рендеринг |
| [`r_fog_exp2`](../38-cvars-reference/01-video-rendering-cvars.md#r_fog_exp2) | `cvar r_fog_exp2(int, "1")` | Видео, экран и общий рендеринг |
| [`r_fog_linear`](../38-cvars-reference/01-video-rendering-cvars.md#r_fog_linear) | `cvar r_fog_linear(int, "0")` | Видео, экран и общий рендеринг |
| [`r_fog_permutation`](../38-cvars-reference/01-video-rendering-cvars.md#r_fog_permutation) | `cvar r_fog_permutation(int, "1")` | Видео, экран и общий рендеринг |
| [`r_font_linear`](../38-cvars-reference/01-video-rendering-cvars.md#r_font_linear) | `cvar r_font_linear(int, "1")` | Видео, экран и общий рендеринг |
| [`r_graphics`](../38-cvars-reference/01-video-rendering-cvars.md#r_graphics) | `cvar r_graphics(int, "1")` | Видео, экран и общий рендеринг |
| [`r_greenlight_colour`](../38-cvars-reference/01-video-rendering-cvars.md#r_greenlight_colour) | `cvar r_greenlight_colour(string, "0.5 3.0 0.5 200")` | Видео, экран и общий рендеринг |
| [`r_grenadetrail`](../38-cvars-reference/01-video-rendering-cvars.md#r_grenadetrail) | `cvar r_grenadetrail(int, "1")` | Видео, экран и общий рендеринг |
| [`r_hdr_framebuffer`](../38-cvars-reference/01-video-rendering-cvars.md#r_hdr_framebuffer) | `cvar r_hdr_framebuffer(int, "0")` | Видео, экран и общий рендеринг |
| [`r_hdr_irisadaptation`](../38-cvars-reference/01-video-rendering-cvars.md#r_hdr_irisadaptation) | `cvar r_hdr_irisadaptation(int, "0")` | Видео, экран и общий рендеринг |
| [`r_hdr_irisadaptation_fade_down`](../38-cvars-reference/01-video-rendering-cvars.md#r_hdr_irisadaptation_fade_down) | `cvar r_hdr_irisadaptation_fade_down(float, "0.5")` | Видео, экран и общий рендеринг |
| [`r_hdr_irisadaptation_fade_up`](../38-cvars-reference/01-video-rendering-cvars.md#r_hdr_irisadaptation_fade_up) | `cvar r_hdr_irisadaptation_fade_up(float, "0.1")` | Видео, экран и общий рендеринг |
| [`r_hdr_irisadaptation_maxvalue`](../38-cvars-reference/01-video-rendering-cvars.md#r_hdr_irisadaptation_maxvalue) | `cvar r_hdr_irisadaptation_maxvalue(int, "4")` | Видео, экран и общий рендеринг |
| [`r_hdr_irisadaptation_minvalue`](../38-cvars-reference/01-video-rendering-cvars.md#r_hdr_irisadaptation_minvalue) | `cvar r_hdr_irisadaptation_minvalue(float, "0.5")` | Видео, экран и общий рендеринг |
| [`r_hdr_irisadaptation_multiplier`](../38-cvars-reference/01-video-rendering-cvars.md#r_hdr_irisadaptation_multiplier) | `cvar r_hdr_irisadaptation_multiplier(int, "2")` | Видео, экран и общий рендеринг |
| [`r_ignoreentpvs`](../38-cvars-reference/01-video-rendering-cvars.md#r_ignoreentpvs) | `cvar r_ignoreentpvs(int, "1")` | Видео, экран и общий рендеринг |
| [`r_ignoremapprefixes`](../38-cvars-reference/01-video-rendering-cvars.md#r_ignoremapprefixes) | `cvar r_ignoremapprefixes(int, "0")` | Видео, экран и общий рендеринг |
| [`r_imageextensions`](../38-cvars-reference/01-video-rendering-cvars.md#r_imageextensions) | `cvar r_imageextensions(string, "The list of image file extensions which might exist on disk (note that this does not list all supported formats, only the extensions that should be searched for).")` | Видео, экран и общий рендеринг |
| [`r_keepimages`](../38-cvars-reference/01-video-rendering-cvars.md#r_keepimages) | `cvar r_keepimages(int, "0")` | Видео, экран и общий рендеринг |
| [`r_lavaalpha`](../38-cvars-reference/01-video-rendering-cvars.md#r_lavaalpha) | `cvar r_lavaalpha(string, "")` | Видео, экран и общий рендеринг |
| [`r_lavastyle`](../38-cvars-reference/01-video-rendering-cvars.md#r_lavastyle) | `cvar r_lavastyle(int, "1")` | Видео, экран и общий рендеринг |
| [`r_lerpmuzzlehack`](../38-cvars-reference/01-video-rendering-cvars.md#r_lerpmuzzlehack) | `cvar r_lerpmuzzlehack(int, "1")` | Видео, экран и общий рендеринг |
| [`r_loadlit`](../38-cvars-reference/01-video-rendering-cvars.md#r_loadlit) | `cvar r_loadlit(int, "1")` | Видео, экран и общий рендеринг |
| [`r_loadsurfenvmaps`](../38-cvars-reference/01-video-rendering-cvars.md#r_loadsurfenvmaps) | `cvar r_loadsurfenvmaps(int, "1")` | Видео, экран и общий рендеринг |
| [`r_max_gpu_bones`](../38-cvars-reference/01-video-rendering-cvars.md#r_max_gpu_bones) | `cvar r_max_gpu_bones(string, "")` | Видео, экран и общий рендеринг |
| [`r_menutint`](../38-cvars-reference/01-video-rendering-cvars.md#r_menutint) | `cvar r_menutint(string, "0.68 0.4 0.13")` | Видео, экран и общий рендеринг |
| [`r_meshpitch`](../38-cvars-reference/01-video-rendering-cvars.md#r_meshpitch) | `cvar r_meshpitch(int, "1")` | Видео, экран и общий рендеринг |
| [`r_meshroll`](../38-cvars-reference/01-video-rendering-cvars.md#r_meshroll) | `cvar r_meshroll(int, "1")` | Видео, экран и общий рендеринг |
| [`r_mirroralpha`](../38-cvars-reference/01-video-rendering-cvars.md#r_mirroralpha) | `cvar r_mirroralpha(int, "1")` | Видео, экран и общий рендеринг |
| [`r_muzzleflash_colour`](../38-cvars-reference/01-video-rendering-cvars.md#r_muzzleflash_colour) | `cvar r_muzzleflash_colour(string, "1.5 1.3 1.0 200")` | Видео, экран и общий рендеринг |
| [`r_muzzleflash_fade`](../38-cvars-reference/01-video-rendering-cvars.md#r_muzzleflash_fade) | `cvar r_muzzleflash_fade(string, "1.5 0.75 0.375 1000")` | Видео, экран и общий рендеринг |
| [`r_netgraph`](../38-cvars-reference/01-video-rendering-cvars.md#r_netgraph) | `cvar r_netgraph(int, "0")` | Видео, экран и общий рендеринг |
| [`r_noaliasshadows`](../38-cvars-reference/01-video-rendering-cvars.md#r_noaliasshadows) | `cvar r_noaliasshadows(int, "0")` | Видео, экран и общий рендеринг |
| [`r_nolerp_list`](../38-cvars-reference/01-video-rendering-cvars.md#r_nolerp_list) | `cvar r_nolerp_list(string, "")` | Видео, экран и общий рендеринг |
| [`r_nolightdir`](../38-cvars-reference/01-video-rendering-cvars.md#r_nolightdir) | `cvar r_nolightdir(int, "0")` | Видео, экран и общий рендеринг |
| [`r_norefresh`](../38-cvars-reference/01-video-rendering-cvars.md#r_norefresh) | `cvar r_norefresh(int, "0")` | Видео, экран и общий рендеринг |
| [`r_noshadow_list`](../38-cvars-reference/01-video-rendering-cvars.md#r_noshadow_list) | `cvar r_noshadow_list(string, "r_noEntityCastShadowList")` | Видео, экран и общий рендеринг |
| [`r_novis`](../38-cvars-reference/01-video-rendering-cvars.md#r_novis) | `cvar r_novis(int, "0")` | Видео, экран и общий рендеринг |
| [`r_part_beams`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_beams) | `cvar r_part_beams(int, "1")` | Видео, экран и общий рендеринг |
| [`r_part_classic_expgrav`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_classic_expgrav) | `cvar r_part_classic_expgrav(int, "10")` | Видео, экран и общий рендеринг |
| [`r_part_classic_opaque`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_classic_opaque) | `cvar r_part_classic_opaque(int, "0")` | Видео, экран и общий рендеринг |
| [`r_part_classic_square`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_classic_square) | `cvar r_part_classic_square(int, "0")` | Видео, экран и общий рендеринг |
| [`r_part_contentswitch`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_contentswitch) | `cvar r_part_contentswitch(int, "1")` | Видео, экран и общий рендеринг |
| [`r_part_density`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_density) | `cvar r_part_density(int, "1")` | Видео, экран и общий рендеринг |
| [`r_part_maxdecals`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_maxdecals) | `cvar r_part_maxdecals(int, "8192")` | Видео, экран и общий рендеринг |
| [`r_part_maxparticles`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_maxparticles) | `cvar r_part_maxparticles(int, "65536")` | Видео, экран и общий рендеринг |
| [`r_part_rain`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_rain) | `cvar r_part_rain(int, "0")` | Видео, экран и общий рендеринг |
| [`r_part_sparks`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_sparks) | `cvar r_part_sparks(int, "1")` | Видео, экран и общий рендеринг |
| [`r_particle_tracelimit`](../38-cvars-reference/01-video-rendering-cvars.md#r_particle_tracelimit) | `cvar r_particle_tracelimit(string, "0x7fffffff")` | Видео, экран и общий рендеринг |
| [`r_particlesystem`](../38-cvars-reference/01-video-rendering-cvars.md#r_particlesystem) | `cvar r_particlesystem(string, "script")` | Видео, экран и общий рендеринг |
| [`r_polygonoffset_shadowmap_factor`](../38-cvars-reference/01-video-rendering-cvars.md#r_polygonoffset_shadowmap_factor) | `cvar r_polygonoffset_shadowmap_factor(float, "0.05")` | Видео, экран и общий рендеринг |
| [`r_polygonoffset_shadowmap_offset`](../38-cvars-reference/01-video-rendering-cvars.md#r_polygonoffset_shadowmap_offset) | `cvar r_polygonoffset_shadowmap_offset(int, "0")` | Видео, экран и общий рендеринг |
| [`r_polygonoffset_stencil_factor`](../38-cvars-reference/01-video-rendering-cvars.md#r_polygonoffset_stencil_factor) | `cvar r_polygonoffset_stencil_factor(float, "0.01")` | Видео, экран и общий рендеринг |
| [`r_polygonoffset_stencil_offset`](../38-cvars-reference/01-video-rendering-cvars.md#r_polygonoffset_stencil_offset) | `cvar r_polygonoffset_stencil_offset(int, "1")` | Видео, экран и общий рендеринг |
| [`r_polygonoffset_submodel_factor`](../38-cvars-reference/01-video-rendering-cvars.md#r_polygonoffset_submodel_factor) | `cvar r_polygonoffset_submodel_factor(int, "0")` | Видео, экран и общий рендеринг |
| [`r_polygonoffset_submodel_map`](../38-cvars-reference/01-video-rendering-cvars.md#r_polygonoffset_submodel_map) | `cvar r_polygonoffset_submodel_map(string, "e?m? r?m? hip?m?")` | Видео, экран и общий рендеринг |
| [`r_polygonoffset_submodel_offset`](../38-cvars-reference/01-video-rendering-cvars.md#r_polygonoffset_submodel_offset) | `cvar r_polygonoffset_submodel_offset(int, "0")` | Видео, экран и общий рендеринг |
| [`r_portaldrawplanes`](../38-cvars-reference/01-video-rendering-cvars.md#r_portaldrawplanes) | `cvar r_portaldrawplanes(int, "0")` | Видео, экран и общий рендеринг |
| [`r_portalonly`](../38-cvars-reference/01-video-rendering-cvars.md#r_portalonly) | `cvar r_portalonly(int, "0")` | Видео, экран и общий рендеринг |
| [`r_portalrecursion`](../38-cvars-reference/01-video-rendering-cvars.md#r_portalrecursion) | `cvar r_portalrecursion(int, "1")` | Видео, экран и общий рендеринг |
| [`r_powerupglow`](../38-cvars-reference/01-video-rendering-cvars.md#r_powerupglow) | `cvar r_powerupglow(int, "1")` | Видео, экран и общий рендеринг |
| [`r_redlight_colour`](../38-cvars-reference/01-video-rendering-cvars.md#r_redlight_colour) | `cvar r_redlight_colour(string, "3.0 0.5 0.5 200")` | Видео, экран и общий рендеринг |
| [`r_refract_fbo`](../38-cvars-reference/01-video-rendering-cvars.md#r_refract_fbo) | `cvar r_refract_fbo(int, "1")` | Видео, экран и общий рендеринг |
| [`r_refractreflect_scale`](../38-cvars-reference/01-video-rendering-cvars.md#r_refractreflect_scale) | `cvar r_refractreflect_scale(float, "0.5")` | Видео, экран и общий рендеринг |
| [`r_replacemodels`](../38-cvars-reference/01-video-rendering-cvars.md#r_replacemodels) | `cvar r_replacemodels(string, "")` | Видео, экран и общий рендеринг |
| [`r_rocketlight`](../38-cvars-reference/01-video-rendering-cvars.md#r_rocketlight) | `cvar r_rocketlight(int, "1")` | Видео, экран и общий рендеринг |
| [`r_rocketlight_colour`](../38-cvars-reference/01-video-rendering-cvars.md#r_rocketlight_colour) | `cvar r_rocketlight_colour(string, "2.0 1.0 0.25 200")` | Видео, экран и общий рендеринг |
| [`r_rockettrail`](../38-cvars-reference/01-video-rendering-cvars.md#r_rockettrail) | `cvar r_rockettrail(int, "1")` | Видео, экран и общий рендеринг |
| [`r_showbboxes`](../38-cvars-reference/01-video-rendering-cvars.md#r_showbboxes) | `cvar r_showbboxes(int, "0")` | Видео, экран и общий рендеринг |
| [`r_showfields`](../38-cvars-reference/01-video-rendering-cvars.md#r_showfields) | `cvar r_showfields(int, "0")` | Видео, экран и общий рендеринг |
| [`r_slimealpha`](../38-cvars-reference/01-video-rendering-cvars.md#r_slimealpha) | `cvar r_slimealpha(string, "")` | Видео, экран и общий рендеринг |
| [`r_slimestyle`](../38-cvars-reference/01-video-rendering-cvars.md#r_slimestyle) | `cvar r_slimestyle(string, "")` | Видео, экран и общий рендеринг |
| [`r_softwarebanding`](../38-cvars-reference/01-video-rendering-cvars.md#r_softwarebanding) | `cvar r_softwarebanding(int, "0")` | Видео, экран и общий рендеринг |
| [`r_speeds`](../38-cvars-reference/01-video-rendering-cvars.md#r_speeds) | `cvar r_speeds(int, "0")` | Видео, экран и общий рендеринг |
| [`r_sprite_backfacing`](../38-cvars-reference/01-video-rendering-cvars.md#r_sprite_backfacing) | `cvar r_sprite_backfacing(int, "0")` | Видео, экран и общий рендеринг |
| [`r_stainfadeammount`](../38-cvars-reference/01-video-rendering-cvars.md#r_stainfadeammount) | `cvar r_stainfadeammount(int, "1")` | Видео, экран и общий рендеринг |
| [`r_stainfadetime`](../38-cvars-reference/01-video-rendering-cvars.md#r_stainfadetime) | `cvar r_stainfadetime(int, "1")` | Видео, экран и общий рендеринг |
| [`r_stereo_convergence`](../38-cvars-reference/01-video-rendering-cvars.md#r_stereo_convergence) | `cvar r_stereo_convergence(int, "0")` | Видео, экран и общий рендеринг |
| [`r_stereo_method`](../38-cvars-reference/01-video-rendering-cvars.md#r_stereo_method) | `cvar r_stereo_method(int, "0")` | Видео, экран и общий рендеринг |
| [`r_stereo_separation`](../38-cvars-reference/01-video-rendering-cvars.md#r_stereo_separation) | `cvar r_stereo_separation(int, "4")` | Видео, экран и общий рендеринг |
| [`r_subdivisions`](../38-cvars-reference/01-video-rendering-cvars.md#r_subdivisions) | `cvar r_subdivisions(int, "2")` | Видео, экран и общий рендеринг |
| [`r_telealpha`](../38-cvars-reference/01-video-rendering-cvars.md#r_telealpha) | `cvar r_telealpha(string, "")` | Видео, экран и общий рендеринг |
| [`r_telestyle`](../38-cvars-reference/01-video-rendering-cvars.md#r_telestyle) | `cvar r_telestyle(int, "1")` | Видео, экран и общий рендеринг |
| [`r_temporalscenecache`](../38-cvars-reference/01-video-rendering-cvars.md#r_temporalscenecache) | `cvar r_temporalscenecache(string, "")` | Видео, экран и общий рендеринг |
| [`r_tessellation`](../38-cvars-reference/01-video-rendering-cvars.md#r_tessellation) | `cvar r_tessellation(int, "0")` | Видео, экран и общий рендеринг |
| [`r_tessellation_level`](../38-cvars-reference/01-video-rendering-cvars.md#r_tessellation_level) | `cvar r_tessellation_level(int, "5")` | Видео, экран и общий рендеринг |
| [`r_torch`](../38-cvars-reference/01-video-rendering-cvars.md#r_torch) | `cvar r_torch(int, "0")` | Видео, экран и общий рендеринг |
| [`r_tracker_fadetime`](../38-cvars-reference/01-video-rendering-cvars.md#r_tracker_fadetime) | `cvar r_tracker_fadetime(int, "1")` | Видео, экран и общий рендеринг |
| [`r_tracker_frags`](../38-cvars-reference/01-video-rendering-cvars.md#r_tracker_frags) | `cvar r_tracker_frags(int, "0")` | Видео, экран и общий рендеринг |
| [`r_tracker_lines`](../38-cvars-reference/01-video-rendering-cvars.md#r_tracker_lines) | `cvar r_tracker_lines(int, "8")` | Видео, экран и общий рендеринг |
| [`r_tracker_time`](../38-cvars-reference/01-video-rendering-cvars.md#r_tracker_time) | `cvar r_tracker_time(int, "4")` | Видео, экран и общий рендеринг |
| [`r_tracker_w`](../38-cvars-reference/01-video-rendering-cvars.md#r_tracker_w) | `cvar r_tracker_w(float, "0.5")` | Видео, экран и общий рендеринг |
| [`r_tracker_x`](../38-cvars-reference/01-video-rendering-cvars.md#r_tracker_x) | `cvar r_tracker_x(float, "0.5")` | Видео, экран и общий рендеринг |
| [`r_tracker_y`](../38-cvars-reference/01-video-rendering-cvars.md#r_tracker_y) | `cvar r_tracker_y(float, "0.333")` | Видео, экран и общий рендеринг |
| [`r_vertexdlights`](../38-cvars-reference/01-video-rendering-cvars.md#r_vertexdlights) | `cvar r_vertexdlights(int, "0")` | Видео, экран и общий рендеринг |
| [`r_viewpreselgun`](../38-cvars-reference/01-video-rendering-cvars.md#r_viewpreselgun) | `cvar r_viewpreselgun(int, "0")` | Видео, экран и общий рендеринг |
| [`r_wallcolour`](../38-cvars-reference/01-video-rendering-cvars.md#r_wallcolour) | `cvar r_wallcolour(string, "128 128 128")` | Видео, экран и общий рендеринг |
| [`r_walltexture`](../38-cvars-reference/01-video-rendering-cvars.md#r_walltexture) | `cvar r_walltexture(string, "")` | Видео, экран и общий рендеринг |
| [`r_wireframe_smooth`](../38-cvars-reference/01-video-rendering-cvars.md#r_wireframe_smooth) | `cvar r_wireframe_smooth(int, "0")` | Видео, экран и общий рендеринг |
| [`ruleset_allow_larger_models`](../38-cvars-reference/01-video-rendering-cvars.md#ruleset_allow_larger_models) | `cvar ruleset_allow_larger_models(int, "1")` | Видео, экран и общий рендеринг |
| [`v_bonusflash`](../38-cvars-reference/01-video-rendering-cvars.md#v_bonusflash) | `cvar v_bonusflash(int, "1")` | Видео, экран и общий рендеринг |
| [`v_centermove`](../38-cvars-reference/01-video-rendering-cvars.md#v_centermove) | `cvar v_centermove(float, "0.15")` | Видео, экран и общий рендеринг |
| [`v_centerspeed`](../38-cvars-reference/01-video-rendering-cvars.md#v_centerspeed) | `cvar v_centerspeed(int, "500")` | Видео, экран и общий рендеринг |
| [`v_contentblend`](../38-cvars-reference/01-video-rendering-cvars.md#v_contentblend) | `cvar v_contentblend(int, "1")` | Видео, экран и общий рендеринг |
| [`v_cshift_empty`](../38-cvars-reference/01-video-rendering-cvars.md#v_cshift_empty) | `cvar v_cshift_empty(string, "130 80 50 0")` | Видео, экран и общий рендеринг |
| [`v_cshift_lava`](../38-cvars-reference/01-video-rendering-cvars.md#v_cshift_lava) | `cvar v_cshift_lava(string, "255 80 0 150")` | Видео, экран и общий рендеринг |
| [`v_cshift_slime`](../38-cvars-reference/01-video-rendering-cvars.md#v_cshift_slime) | `cvar v_cshift_slime(string, "0 25 5 150")` | Видео, экран и общий рендеринг |
| [`v_cshift_water`](../38-cvars-reference/01-video-rendering-cvars.md#v_cshift_water) | `cvar v_cshift_water(string, "130 80 50 128")` | Видео, экран и общий рендеринг |
| [`v_damagecshift`](../38-cvars-reference/01-video-rendering-cvars.md#v_damagecshift) | `cvar v_damagecshift(int, "1")` | Видео, экран и общий рендеринг |
| [`v_deathtilt`](../38-cvars-reference/01-video-rendering-cvars.md#v_deathtilt) | `cvar v_deathtilt(int, "1")` | Видео, экран и общий рендеринг |
| [`v_depthsortentities`](../38-cvars-reference/01-video-rendering-cvars.md#v_depthsortentities) | `cvar v_depthsortentities(int, "0")` | Видео, экран и общий рендеринг |
| [`v_gunkick`](../38-cvars-reference/01-video-rendering-cvars.md#v_gunkick) | `cvar v_gunkick(int, "0")` | Видео, экран и общий рендеринг |
| [`v_gunkick_q2`](../38-cvars-reference/01-video-rendering-cvars.md#v_gunkick_q2) | `cvar v_gunkick_q2(int, "1")` | Видео, экран и общий рендеринг |
| [`v_idlescale`](../38-cvars-reference/01-video-rendering-cvars.md#v_idlescale) | `cvar v_idlescale(int, "0")` | Видео, экран и общий рендеринг |
| [`v_ipitch_cycle`](../38-cvars-reference/01-video-rendering-cvars.md#v_ipitch_cycle) | `cvar v_ipitch_cycle(int, "1")` | Видео, экран и общий рендеринг |
| [`v_ipitch_level`](../38-cvars-reference/01-video-rendering-cvars.md#v_ipitch_level) | `cvar v_ipitch_level(float, "0.3")` | Видео, экран и общий рендеринг |
| [`v_iroll_cycle`](../38-cvars-reference/01-video-rendering-cvars.md#v_iroll_cycle) | `cvar v_iroll_cycle(float, "0.5")` | Видео, экран и общий рендеринг |
| [`v_iroll_level`](../38-cvars-reference/01-video-rendering-cvars.md#v_iroll_level) | `cvar v_iroll_level(float, "0.1")` | Видео, экран и общий рендеринг |
| [`v_iyaw_cycle`](../38-cvars-reference/01-video-rendering-cvars.md#v_iyaw_cycle) | `cvar v_iyaw_cycle(int, "2")` | Видео, экран и общий рендеринг |
| [`v_iyaw_level`](../38-cvars-reference/01-video-rendering-cvars.md#v_iyaw_level) | `cvar v_iyaw_level(float, "0.3")` | Видео, экран и общий рендеринг |
| [`v_kickpitch`](../38-cvars-reference/01-video-rendering-cvars.md#v_kickpitch) | `cvar v_kickpitch(float, "0.6")` | Видео, экран и общий рендеринг |
| [`v_kickroll`](../38-cvars-reference/01-video-rendering-cvars.md#v_kickroll) | `cvar v_kickroll(float, "0.6")` | Видео, экран и общий рендеринг |
| [`v_kicktime`](../38-cvars-reference/01-video-rendering-cvars.md#v_kicktime) | `cvar v_kicktime(float, "0.5")` | Видео, экран и общий рендеринг |
| [`v_pentcshift`](../38-cvars-reference/01-video-rendering-cvars.md#v_pentcshift) | `cvar v_pentcshift(int, "1")` | Видео, экран и общий рендеринг |
| [`v_powerupshell`](../38-cvars-reference/01-video-rendering-cvars.md#v_powerupshell) | `cvar v_powerupshell(int, "0")` | Видео, экран и общий рендеринг |
| [`v_projectionmode`](../38-cvars-reference/01-video-rendering-cvars.md#v_projectionmode) | `cvar v_projectionmode(int, "0")` | Видео, экран и общий рендеринг |
| [`v_quadcshift`](../38-cvars-reference/01-video-rendering-cvars.md#v_quadcshift) | `cvar v_quadcshift(int, "1")` | Видео, экран и общий рендеринг |
| [`v_ringcshift`](../38-cvars-reference/01-video-rendering-cvars.md#v_ringcshift) | `cvar v_ringcshift(int, "1")` | Видео, экран и общий рендеринг |
| [`v_suitcshift`](../38-cvars-reference/01-video-rendering-cvars.md#v_suitcshift) | `cvar v_suitcshift(int, "1")` | Видео, экран и общий рендеринг |
| [`v_viewheight`](../38-cvars-reference/01-video-rendering-cvars.md#v_viewheight) | `cvar v_viewheight(int, "0")` | Видео, экран и общий рендеринг |
| [`vid_baseheight`](../38-cvars-reference/01-video-rendering-cvars.md#vid_baseheight) | `cvar vid_baseheight(string, "")` | Видео, экран и общий рендеринг |
| [`vid_devicename`](../38-cvars-reference/01-video-rendering-cvars.md#vid_devicename) | `cvar vid_devicename(string, "")` | Видео, экран и общий рендеринг |
| [`vid_dpi_x`](../38-cvars-reference/01-video-rendering-cvars.md#vid_dpi_x) | `cvar vid_dpi_x(int, "0")` | Видео, экран и общий рендеринг |
| [`vid_dpi_y`](../38-cvars-reference/01-video-rendering-cvars.md#vid_dpi_y) | `cvar vid_dpi_y(int, "0")` | Видео, экран и общий рендеринг |
| [`vid_minsize`](../38-cvars-reference/01-video-rendering-cvars.md#vid_minsize) | `cvar vid_minsize(string, "320 200")` | Видео, экран и общий рендеринг |
| [`vid_triplebuffer`](../38-cvars-reference/01-video-rendering-cvars.md#vid_triplebuffer) | `cvar vid_triplebuffer(int, "1")` | Видео, экран и общий рендеринг |
| [`vid_winthread`](../38-cvars-reference/01-video-rendering-cvars.md#vid_winthread) | `cvar vid_winthread(string, "")` | Видео, экран и общий рендеринг |
| [`vid_wndalpha`](../38-cvars-reference/01-video-rendering-cvars.md#vid_wndalpha) | `cvar vid_wndalpha(int, "1")` | Видео, экран и общий рендеринг |
| [`viewsize`](../38-cvars-reference/01-video-rendering-cvars.md#viewsize) | `cvar viewsize(int, "100")` | Видео, экран и общий рендеринг |
| [`vk_khr_dedicated_allocation`](../38-cvars-reference/01-video-rendering-cvars.md#vk_khr_dedicated_allocation) | `cvar vk_khr_dedicated_allocation(string, "")` | Видео, экран и общий рендеринг |
| [`vk_khr_get_memory_requirements2`](../38-cvars-reference/01-video-rendering-cvars.md#vk_khr_get_memory_requirements2) | `cvar vk_khr_get_memory_requirements2(string, "")` | Видео, экран и общий рендеринг |
| [`vk_khr_push_descriptor`](../38-cvars-reference/01-video-rendering-cvars.md#vk_khr_push_descriptor) | `cvar vk_khr_push_descriptor(string, "")` | Видео, экран и общий рендеринг |
| [`vk_khr_ray_query`](../38-cvars-reference/01-video-rendering-cvars.md#vk_khr_ray_query) | `cvar vk_khr_ray_query(string, "")` | Видео, экран и общий рендеринг |
| [`worker_count`](../38-cvars-reference/01-video-rendering-cvars.md#worker_count) | `cvar worker_count(string, "")` | Видео, экран и общий рендеринг |
| [`worker_flush`](../38-cvars-reference/01-video-rendering-cvars.md#worker_flush) | `cvar worker_flush(int, "1")` | Видео, экран и общий рендеринг |
| [`worker_sleeptime`](../38-cvars-reference/01-video-rendering-cvars.md#worker_sleeptime) | `cvar worker_sleeptime(int, "0")` | Видео, экран и общий рендеринг |
| [`gl_skyboxdist`](../38-cvars-reference/02-lighting-materials-cvars.md#gl_skyboxdist) | `cvar gl_skyboxdist(float, "0")` | Освещение, тени и материалы |
| [`mod_map_lights`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_map_lights) | `cvar mod_map_lights(int, "0")` | Освещение, тени и материалы |
| [`mod_map_texscale`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_map_texscale) | `cvar mod_map_texscale(float, "1")` | Освещение, тени и материалы |
| [`mod_terrain_ambient`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_terrain_ambient) | `cvar mod_terrain_ambient(float, "0.5")` | Освещение, тени и материалы |
| [`mod_terrain_shadow_dist`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_terrain_shadow_dist) | `cvar mod_terrain_shadow_dist(float, "2048")` | Освещение, тени и материалы |
| [`mod_terrain_shadows`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_terrain_shadows) | `cvar mod_terrain_shadows(bool, "0")` | Освещение, тени и материалы |
| [`mod_terrain_sundir`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_terrain_sundir) | `cvar mod_terrain_sundir(vector3, "0.4 0.7 2")` | Освещение, тени и материалы |
| [`r_bloom`](../38-cvars-reference/02-lighting-materials-cvars.md#r_bloom) | `cvar r_bloom(float, "0")` | Освещение, тени и материалы |
| [`r_bloom_downsize`](../38-cvars-reference/02-lighting-materials-cvars.md#r_bloom_downsize) | `cvar r_bloom_downsize(bool, "0")` | Освещение, тени и материалы |
| [`r_bloom_filter`](../38-cvars-reference/02-lighting-materials-cvars.md#r_bloom_filter) | `cvar r_bloom_filter(vector3, "0.7 0.7 0.7")` | Освещение, тени и материалы |
| [`r_bloom_initialscale`](../38-cvars-reference/02-lighting-materials-cvars.md#r_bloom_initialscale) | `cvar r_bloom_initialscale(float, "1")` | Освещение, тени и материалы |
| [`r_bloom_retain`](../38-cvars-reference/02-lighting-materials-cvars.md#r_bloom_retain) | `cvar r_bloom_retain(float, "1")` | Освещение, тени и материалы |
| [`r_bloom_size`](../38-cvars-reference/02-lighting-materials-cvars.md#r_bloom_size) | `cvar r_bloom_size(float, "4")` | Освещение, тени и материалы |
| [`r_fastsky`](../38-cvars-reference/02-lighting-materials-cvars.md#r_fastsky) | `cvar r_fastsky(bool, "0")` | Освещение, тени и материалы |
| [`r_fastskycolour`](../38-cvars-reference/02-lighting-materials-cvars.md#r_fastskycolour) | `cvar r_fastskycolour(vector3, "0")` | Освещение, тени и материалы |
| [`r_forceprogramify`](../38-cvars-reference/02-lighting-materials-cvars.md#r_forceprogramify) | `cvar r_forceprogramify(int, "0")` | Освещение, тени и материалы |
| [`r_glsl_precache`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_precache) | `cvar r_glsl_precache(bool, "0")` | Освещение, тени и материалы |
| [`r_glsl_skybox_autorotate`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_skybox_autorotate) | `cvar r_glsl_skybox_autorotate(bool, "1")` | Освещение, тени и материалы |
| [`r_glsl_skybox_orientation`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_skybox_orientation) | `cvar r_glsl_skybox_orientation(vector4, "0 0 0 0")` | Освещение, тени и материалы |
| [`r_halfrate`](../38-cvars-reference/02-lighting-materials-cvars.md#r_halfrate) | `cvar r_halfrate(bool, "0")` | Освещение, тени и материалы |
| [`r_shadow_playershadows`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_playershadows) | `cvar r_shadow_playershadows(bool, "1")` | Освещение, тени и материалы |
| [`r_shadow_raytrace`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_raytrace) | `cvar r_shadow_raytrace(bool, "0")` | Освещение, тени и материалы |
| [`r_shadow_realtime_dlight`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_dlight) | `cvar r_shadow_realtime_dlight(bool, "1")` | Освещение, тени и материалы |
| [`r_shadow_realtime_dlight_ambient`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_dlight_ambient) | `cvar r_shadow_realtime_dlight_ambient(float, "0")` | Освещение, тени и материалы |
| [`r_shadow_realtime_dlight_diffuse`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_dlight_diffuse) | `cvar r_shadow_realtime_dlight_diffuse(float, "1")` | Освещение, тени и материалы |
| [`r_shadow_realtime_dlight_shadows`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_dlight_shadows) | `cvar r_shadow_realtime_dlight_shadows(bool, "1")` | Освещение, тени и материалы |
| [`r_shadow_realtime_dlight_specular`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_dlight_specular) | `cvar r_shadow_realtime_dlight_specular(float, "4")` | Освещение, тени и материалы |
| [`r_shadow_realtime_world`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_world) | `cvar r_shadow_realtime_world(bool, "0")` | Освещение, тени и материалы |
| [`r_shadow_realtime_world_importlightentitiesfrommap`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_world_importlightentitiesfrommap) | `cvar r_shadow_realtime_world_importlightentitiesfrommap(int, "0")` | Освещение, тени и материалы |
| [`r_shadow_realtime_world_lightmaps`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_world_lightmaps) | `cvar r_shadow_realtime_world_lightmaps(float, "0")` | Освещение, тени и материалы |
| [`r_shadow_realtime_world_shadows`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_world_shadows) | `cvar r_shadow_realtime_world_shadows(bool, "1")` | Освещение, тени и материалы |
| [`r_shadow_scissor`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_scissor) | `cvar r_shadow_scissor(bool, "1")` | Освещение, тени и материалы |
| [`r_shadow_shadowmapping`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_shadowmapping) | `cvar r_shadow_shadowmapping(bool, "1")` | Освещение, тени и материалы |
| [`r_shadow_shadowmapping_bias`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_shadowmapping_bias) | `cvar r_shadow_shadowmapping_bias(float, "0.03")` | Освещение, тени и материалы |
| [`r_shadow_shadowmapping_depthbits`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_shadowmapping_depthbits) | `cvar r_shadow_shadowmapping_depthbits(int, "16")` | Освещение, тени и материалы |
| [`r_shadow_shadowmapping_nearclip`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_shadowmapping_nearclip) | `cvar r_shadow_shadowmapping_nearclip(float, "1")` | Освещение, тени и материалы |
| [`r_shadow_shadowmapping_precision`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_shadowmapping_precision) | `cvar r_shadow_shadowmapping_precision(float, "1")` | Освещение, тени и материалы |
| [`r_shadows_fakedistance`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadows_fakedistance) | `cvar r_shadows_fakedistance(float, "1024")` | Освещение, тени и материалы |
| [`r_shadows_focus`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadows_focus) | `cvar r_shadows_focus(vector3, "0 0 0")` | Освещение, тени и материалы |
| [`r_shadows_throwdirection`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadows_throwdirection) | `cvar r_shadows_throwdirection(vector3, "0 0 -1")` | Освещение, тени и материалы |
| [`r_skybox`](../38-cvars-reference/02-lighting-materials-cvars.md#r_skybox) | `cvar r_skybox(string, "")` | Освещение, тени и материалы |
| [`r_skycloudalpha`](../38-cvars-reference/02-lighting-materials-cvars.md#r_skycloudalpha) | `cvar r_skycloudalpha(float, "1")` | Освещение, тени и материалы |
| [`r_skyfog`](../38-cvars-reference/02-lighting-materials-cvars.md#r_skyfog) | `cvar r_skyfog(float, "0.5")` | Освещение, тени и материалы |
| [`r_sun_colour`](../38-cvars-reference/02-lighting-materials-cvars.md#r_sun_colour) | `cvar r_sun_colour(vector3, "0 0 0")` | Освещение, тени и материалы |
| [`r_sun_dir`](../38-cvars-reference/02-lighting-materials-cvars.md#r_sun_dir) | `cvar r_sun_dir(vector3, "0.2 0.5 0.8")` | Освещение, тени и материалы |
| [`r_vertexlight`](../38-cvars-reference/02-lighting-materials-cvars.md#r_vertexlight) | `cvar r_vertexlight(bool, "0")` | Освещение, тени и материалы |
| [`gl_flashblend`](../38-cvars-reference/02-lighting-materials-cvars.md#gl_flashblend) | `cvar gl_flashblend(int, "0")` | Освещение, тени и материалы |
| [`gl_flashblendscale`](../38-cvars-reference/02-lighting-materials-cvars.md#gl_flashblendscale) | `cvar gl_flashblendscale(float, "0.35")` | Освещение, тени и материалы |
| [`gl_menutint_shader`](../38-cvars-reference/02-lighting-materials-cvars.md#gl_menutint_shader) | `cvar gl_menutint_shader(int, "1")` | Освещение, тени и материалы |
| [`gl_workaround_ati_shadersource`](../38-cvars-reference/02-lighting-materials-cvars.md#gl_workaround_ati_shadersource) | `cvar gl_workaround_ati_shadersource(int, "1")` | Освещение, тени и материалы |
| [`mod_terrain_defaulttexture`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_terrain_defaulttexture) | `cvar mod_terrain_defaulttexture(string, "")` | Освещение, тени и материалы |
| [`mod_terrain_networked`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_terrain_networked) | `cvar mod_terrain_networked(int, "0")` | Освещение, тени и материалы |
| [`mod_terrain_savever`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_terrain_savever) | `cvar mod_terrain_savever(string, "")` | Освещение, тени и материалы |
| [`r_ambient`](../38-cvars-reference/02-lighting-materials-cvars.md#r_ambient) | `cvar r_ambient(int, "0")` | Освещение, тени и материалы |
| [`r_dynamic`](../38-cvars-reference/02-lighting-materials-cvars.md#r_dynamic) | `cvar r_dynamic(int, "0")` | Освещение, тени и материалы |
| [`r_fullbright`](../38-cvars-reference/02-lighting-materials-cvars.md#r_fullbright) | `cvar r_fullbright(int, "0")` | Освещение, тени и материалы |
| [`r_fullbrightSkins`](../38-cvars-reference/02-lighting-materials-cvars.md#r_fullbrightskins) | `cvar r_fullbrightSkins(float, "0.8")` | Освещение, тени и материалы |
| [`r_glsl_emissive`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_emissive) | `cvar r_glsl_emissive(int, "1")` | Освещение, тени и материалы |
| [`r_glsl_offsetmapping`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_offsetmapping) | `cvar r_glsl_offsetmapping(int, "0")` | Освещение, тени и материалы |
| [`r_glsl_offsetmapping_reliefmapping`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_offsetmapping_reliefmapping) | `cvar r_glsl_offsetmapping_reliefmapping(int, "0")` | Освещение, тени и материалы |
| [`r_glsl_offsetmapping_scale`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_offsetmapping_scale) | `cvar r_glsl_offsetmapping_scale(float, "0.04")` | Освещение, тени и материалы |
| [`r_glsl_pbr`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_pbr) | `cvar r_glsl_pbr(int, "0")` | Освещение, тени и материалы |
| [`r_glsl_turbscale_reflect`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_turbscale_reflect) | `cvar r_glsl_turbscale_reflect(int, "1")` | Освещение, тени и материалы |
| [`r_glsl_turbscale_refract`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_turbscale_refract) | `cvar r_glsl_turbscale_refract(int, "1")` | Освещение, тени и материалы |
| [`r_lightflicker`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightflicker) | `cvar r_lightflicker(int, "1")` | Освещение, тени и материалы |
| [`r_lightmap`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightmap) | `cvar r_lightmap(int, "0")` | Освещение, тени и материалы |
| [`r_lightmap_format`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightmap_format) | `cvar r_lightmap_format(string, "")` | Освещение, тени и материалы |
| [`r_lightmap_saturation`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightmap_saturation) | `cvar r_lightmap_saturation(int, "1")` | Освещение, тени и материалы |
| [`r_lightprepass`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightprepass) | `cvar r_lightprepass(int, "0")` | Освещение, тени и материалы |
| [`r_lightstylescale`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightstylescale) | `cvar r_lightstylescale(int, "1")` | Освещение, тени и материалы |
| [`r_lightstylesmooth`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightstylesmooth) | `cvar r_lightstylesmooth(int, "0")` | Освещение, тени и материалы |
| [`r_lightstylesmooth_limit`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightstylesmooth_limit) | `cvar r_lightstylesmooth_limit(int, "2")` | Освещение, тени и материалы |
| [`r_lightstylespeed`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightstylespeed) | `cvar r_lightstylespeed(int, "10")` | Освещение, тени и материалы |
| [`r_particledesc`](../38-cvars-reference/02-lighting-materials-cvars.md#r_particledesc) | `cvar r_particledesc(string, "")` | Освещение, тени и материалы |
| [`r_postprocshader`](../38-cvars-reference/02-lighting-materials-cvars.md#r_postprocshader) | `cvar r_postprocshader(string, "")` | Освещение, тени и материалы |
| [`r_shaderblobs`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shaderblobs) | `cvar r_shaderblobs(int, "0")` | Освещение, тени и материалы |
| [`r_shadow_bumpscale_basetexture`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_bumpscale_basetexture) | `cvar r_shadow_bumpscale_basetexture(int, "0")` | Освещение, тени и материалы |
| [`r_shadow_bumpscale_bumpmap`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_bumpscale_bumpmap) | `cvar r_shadow_bumpscale_bumpmap(int, "4")` | Освещение, тени и материалы |
| [`r_shadow_heightscale_basetexture`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_heightscale_basetexture) | `cvar r_shadow_heightscale_basetexture(int, "0")` | Освещение, тени и материалы |
| [`r_shadow_heightscale_bumpmap`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_heightscale_bumpmap) | `cvar r_shadow_heightscale_bumpmap(int, "1")` | Освещение, тени и материалы |
| [`r_shadow_realtime_nonworld_lightmaps`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_nonworld_lightmaps) | `cvar r_shadow_realtime_nonworld_lightmaps(int, "1")` | Освещение, тени и материалы |
| [`r_shadows`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadows) | `cvar r_shadows(int, "0")` | Освещение, тени и материалы |
| [`r_showshaders`](../38-cvars-reference/02-lighting-materials-cvars.md#r_showshaders) | `cvar r_showshaders(int, "0")` | Освещение, тени и материалы |
| [`r_stains`](../38-cvars-reference/02-lighting-materials-cvars.md#r_stains) | `cvar r_stains(int, "0")` | Освещение, тени и материалы |
| [`ruleset_allow_shaders`](../38-cvars-reference/02-lighting-materials-cvars.md#ruleset_allow_shaders) | `cvar ruleset_allow_shaders(int, "1")` | Освещение, тени и материалы |
| [`capturesound`](../38-cvars-reference/03-audio-cvars.md#capturesound) | `cvar capturesound(целое, "1")` | Звук |
| [`capturesoundbits`](../38-cvars-reference/03-audio-cvars.md#capturesoundbits) | `cvar capturesoundbits(целое, "16")` | Звук |
| [`capturesoundchannels`](../38-cvars-reference/03-audio-cvars.md#capturesoundchannels) | `cvar capturesoundchannels(целое, "2")` | Звук |
| [`cl_voip_capturedevice`](../38-cvars-reference/03-audio-cvars.md#cl_voip_capturedevice) | `cvar cl_voip_capturedevice(строка, "")` | Звук |
| [`cl_voip_send`](../38-cvars-reference/03-audio-cvars.md#cl_voip_send) | `cvar cl_voip_send(целое, "0")` | Звук |
| [`cl_voip_test`](../38-cvars-reference/03-audio-cvars.md#cl_voip_test) | `cvar cl_voip_test(целое, "0")` | Звук |
| [`cl_voip_vad_delay`](../38-cvars-reference/03-audio-cvars.md#cl_voip_vad_delay) | `cvar cl_voip_vad_delay(дробное, "0.3")` | Звук |
| [`cl_voip_vad_threshhold`](../38-cvars-reference/03-audio-cvars.md#cl_voip_vad_threshhold) | `cvar cl_voip_vad_threshhold(целое, "15")` | Звук |
| [`mastervolume`](../38-cvars-reference/03-audio-cvars.md#mastervolume) | `cvar mastervolume(дробное, "1")` | Звук |
| [`media_hijackwinamp`](../38-cvars-reference/03-audio-cvars.md#media_hijackwinamp) | `cvar media_hijackwinamp(целое, "0")` | Звук |
| [`media_repeat`](../38-cvars-reference/03-audio-cvars.md#media_repeat) | `cvar media_repeat(целое, "1")` | Звук |
| [`media_shuffle`](../38-cvars-reference/03-audio-cvars.md#media_shuffle) | `cvar media_shuffle(целое, "1")` | Звук |
| [`music_fade`](../38-cvars-reference/03-audio-cvars.md#music_fade) | `cvar music_fade(целое, "1")` | Звук |
| [`music_playlist_index`](../38-cvars-reference/03-audio-cvars.md#music_playlist_index) | `cvar music_playlist_index(целое, "-1")` | Звук |
| [`nosound`](../38-cvars-reference/03-audio-cvars.md#nosound) | `cvar nosound(целое, "0")` | Звук |
| [`s_al_debug`](../38-cvars-reference/03-audio-cvars.md#s_al_debug) | `cvar s_al_debug(целое, "0")` | Звук |
| [`s_al_disable`](../38-cvars-reference/03-audio-cvars.md#s_al_disable) | `cvar s_al_disable(целое, "0")` | Звук |
| [`s_al_hrtf`](../38-cvars-reference/03-audio-cvars.md#s_al_hrtf) | `cvar s_al_hrtf(строка, "")` | Звук |
| [`s_al_reference_distance`](../38-cvars-reference/03-audio-cvars.md#s_al_reference_distance) | `cvar s_al_reference_distance(дробное, "120")` | Звук |
| [`s_al_use_reverb`](../38-cvars-reference/03-audio-cvars.md#s_al_use_reverb) | `cvar s_al_use_reverb(целое, "1")` | Звук |
| [`s_al_velocityscale`](../38-cvars-reference/03-audio-cvars.md#s_al_velocityscale) | `cvar s_al_velocityscale(дробное, "1")` | Звук |
| [`snd_ignorecueloops`](../38-cvars-reference/03-audio-cvars.md#snd_ignorecueloops) | `cvar snd_ignorecueloops(целое, "0")` | Звук |
| [`snd_ignoregamespeed`](../38-cvars-reference/03-audio-cvars.md#snd_ignoregamespeed) | `cvar snd_ignoregamespeed(целое, "0")` | Звук |
| [`snd_loadasstereo`](../38-cvars-reference/03-audio-cvars.md#snd_loadasstereo) | `cvar snd_loadasstereo(целое, "0")` | Звук |
| [`snd_playbackrate`](../38-cvars-reference/03-audio-cvars.md#snd_playbackrate) | `cvar snd_playbackrate(дробное, "1")` | Звук |
| [`tts_mode`](../38-cvars-reference/03-audio-cvars.md#tts_mode) | `cvar tts_mode(целое, "1")` | Звук |
| [`wasapi_buffersize`](../38-cvars-reference/03-audio-cvars.md#wasapi_buffersize) | `cvar wasapi_buffersize(дробное, "0.01")` | Звук |
| [`wasapi_exclusive`](../38-cvars-reference/03-audio-cvars.md#wasapi_exclusive) | `cvar wasapi_exclusive(целое, "0")` | Звук |
| [`wasapi_forcechannels`](../38-cvars-reference/03-audio-cvars.md#wasapi_forcechannels) | `cvar wasapi_forcechannels(целое, "0")` | Звук |
| [`wasapi_forcerate`](../38-cvars-reference/03-audio-cvars.md#wasapi_forcerate) | `cvar wasapi_forcerate(целое, "0")` | Звук |
| [`_cl_voip_capturedevice_opts`](../38-cvars-reference/03-audio-cvars.md#_cl_voip_capturedevice_opts) | `cvar _cl_voip_capturedevice_opts(string, "")` | Звук |
| [`_s_device_opts`](../38-cvars-reference/03-audio-cvars.md#_s_device_opts) | `cvar _s_device_opts(string, "")` | Звук |
| [`cl_chatsound`](../38-cvars-reference/03-audio-cvars.md#cl_chatsound) | `cvar cl_chatsound(int, "1")` | Звук |
| [`cl_cursor_bias_x`](../38-cvars-reference/03-audio-cvars.md#cl_cursor_bias_x) | `cvar cl_cursor_bias_x(float, "0.0")` | Звук |
| [`cl_cursor_bias_y`](../38-cvars-reference/03-audio-cvars.md#cl_cursor_bias_y) | `cvar cl_cursor_bias_y(float, "0.0")` | Звук |
| [`cl_enemychatsound`](../38-cvars-reference/03-audio-cvars.md#cl_enemychatsound) | `cvar cl_enemychatsound(string, "misc/talk.wav")` | Звук |
| [`cl_maxfps_slop`](../38-cvars-reference/03-audio-cvars.md#cl_maxfps_slop) | `cvar cl_maxfps_slop(int, "3")` | Звук |
| [`cl_predict_players_frac`](../38-cvars-reference/03-audio-cvars.md#cl_predict_players_frac) | `cvar cl_predict_players_frac(float, "0.9")` | Звук |
| [`cl_predict_players_latency`](../38-cvars-reference/03-audio-cvars.md#cl_predict_players_latency) | `cvar cl_predict_players_latency(float, "1.0")` | Звук |
| [`cl_predict_players_nudge`](../38-cvars-reference/03-audio-cvars.md#cl_predict_players_nudge) | `cvar cl_predict_players_nudge(float, "0.02")` | Звук |
| [`cl_staticsounds`](../38-cvars-reference/03-audio-cvars.md#cl_staticsounds) | `cvar cl_staticsounds(int, "1")` | Звук |
| [`cl_teamchatsound`](../38-cvars-reference/03-audio-cvars.md#cl_teamchatsound) | `cvar cl_teamchatsound(string, "misc/talk.wav")` | Звук |
| [`cl_voip_autogain`](../38-cvars-reference/03-audio-cvars.md#cl_voip_autogain) | `cvar cl_voip_autogain(int, "0")` | Звук |
| [`cl_voip_bitrate`](../38-cvars-reference/03-audio-cvars.md#cl_voip_bitrate) | `cvar cl_voip_bitrate(int, "3000")` | Звук |
| [`cl_voip_capturingvol`](../38-cvars-reference/03-audio-cvars.md#cl_voip_capturingvol) | `cvar cl_voip_capturingvol(float, "0.5")` | Звук |
| [`cl_voip_codec`](../38-cvars-reference/03-audio-cvars.md#cl_voip_codec) | `cvar cl_voip_codec(string, "")` | Звук |
| [`cl_voip_ducking`](../38-cvars-reference/03-audio-cvars.md#cl_voip_ducking) | `cvar cl_voip_ducking(float, "0.5")` | Звук |
| [`cl_voip_micamp`](../38-cvars-reference/03-audio-cvars.md#cl_voip_micamp) | `cvar cl_voip_micamp(int, "2")` | Звук |
| [`cl_voip_noisefilter`](../38-cvars-reference/03-audio-cvars.md#cl_voip_noisefilter) | `cvar cl_voip_noisefilter(int, "1")` | Звук |
| [`cl_voip_play`](../38-cvars-reference/03-audio-cvars.md#cl_voip_play) | `cvar cl_voip_play(int, "1")` | Звук |
| [`cl_voip_showmeter`](../38-cvars-reference/03-audio-cvars.md#cl_voip_showmeter) | `cvar cl_voip_showmeter(int, "1")` | Звук |
| [`dpcompat_precachesoundhack`](../38-cvars-reference/03-audio-cvars.md#dpcompat_precachesoundhack) | `cvar dpcompat_precachesoundhack(int, "0")` | Звук |
| [`dtls_psk_hint`](../38-cvars-reference/03-audio-cvars.md#dtls_psk_hint) | `cvar dtls_psk_hint(string, "")` | Звук |
| [`dtls_psk_key`](../38-cvars-reference/03-audio-cvars.md#dtls_psk_key) | `cvar dtls_psk_key(string, "")` | Звук |
| [`dtls_psk_user`](../38-cvars-reference/03-audio-cvars.md#dtls_psk_user) | `cvar dtls_psk_user(string, "")` | Звук |
| [`fs_basepath`](../38-cvars-reference/03-audio-cvars.md#fs_basepath) | `cvar fs_basepath(string, "")` | Звук |
| [`fs_cache`](../38-cvars-reference/03-audio-cvars.md#fs_cache) | `cvar fs_cache(int, "2")` | Звук |
| [`fs_game`](../38-cvars-reference/03-audio-cvars.md#fs_game) | `cvar fs_game(string, "")` | Звук |
| [`fs_gamepath`](../38-cvars-reference/03-audio-cvars.md#fs_gamepath) | `cvar fs_gamepath(string, "")` | Звук |
| [`fs_hidesyspaths`](../38-cvars-reference/03-audio-cvars.md#fs_hidesyspaths) | `cvar fs_hidesyspaths(int, "0")` | Звук |
| [`fs_homepath`](../38-cvars-reference/03-audio-cvars.md#fs_homepath) | `cvar fs_homepath(string, "")` | Звук |
| [`fs_noreexec`](../38-cvars-reference/03-audio-cvars.md#fs_noreexec) | `cvar fs_noreexec(int, "0")` | Звук |
| [`fs_packageprioritisation`](../38-cvars-reference/03-audio-cvars.md#fs_packageprioritisation) | `cvar fs_packageprioritisation(int, "1")` | Звук |
| [`mod_litsprites_force`](../38-cvars-reference/03-audio-cvars.md#mod_litsprites_force) | `cvar mod_litsprites_force(int, "0")` | Звук |
| [`net_dns_ipv4`](../38-cvars-reference/03-audio-cvars.md#net_dns_ipv4) | `cvar net_dns_ipv4(int, "1")` | Звук |
| [`net_dns_ipv6`](../38-cvars-reference/03-audio-cvars.md#net_dns_ipv6) | `cvar net_dns_ipv6(int, "1")` | Звук |
| [`qws_builddate`](../38-cvars-reference/03-audio-cvars.md#qws_builddate) | `cvar qws_builddate(string, "")` | Звук |
| [`qws_buildnum`](../38-cvars-reference/03-audio-cvars.md#qws_buildnum) | `cvar qws_buildnum(string, "")` | Звук |
| [`qws_fullname`](../38-cvars-reference/03-audio-cvars.md#qws_fullname) | `cvar qws_fullname(string, "")` | Звук |
| [`qws_homepage`](../38-cvars-reference/03-audio-cvars.md#qws_homepage) | `cvar qws_homepage(string, "")` | Звук |
| [`qws_name`](../38-cvars-reference/03-audio-cvars.md#qws_name) | `cvar qws_name(string, "")` | Звук |
| [`qws_platform`](../38-cvars-reference/03-audio-cvars.md#qws_platform) | `cvar qws_platform(string, "")` | Звук |
| [`qws_version`](../38-cvars-reference/03-audio-cvars.md#qws_version) | `cvar qws_version(string, ".")` | Звук |
| [`r_coronas_fadedist`](../38-cvars-reference/03-audio-cvars.md#r_coronas_fadedist) | `cvar r_coronas_fadedist(int, "256")` | Звук |
| [`r_coronas_intensity`](../38-cvars-reference/03-audio-cvars.md#r_coronas_intensity) | `cvar r_coronas_intensity(int, "1")` | Звук |
| [`r_coronas_mindist`](../38-cvars-reference/03-audio-cvars.md#r_coronas_mindist) | `cvar r_coronas_mindist(int, "128")` | Звук |
| [`r_coronas_occlusion`](../38-cvars-reference/03-audio-cvars.md#r_coronas_occlusion) | `cvar r_coronas_occlusion(string, "")` | Звук |
| [`r_editlights_cursordistance`](../38-cvars-reference/03-audio-cvars.md#r_editlights_cursordistance) | `cvar r_editlights_cursordistance(int, "1024")` | Звук |
| [`r_editlights_cursorgrid`](../38-cvars-reference/03-audio-cvars.md#r_editlights_cursorgrid) | `cvar r_editlights_cursorgrid(int, "1")` | Звук |
| [`r_editlights_cursorpushback`](../38-cvars-reference/03-audio-cvars.md#r_editlights_cursorpushback) | `cvar r_editlights_cursorpushback(int, "0")` | Звук |
| [`r_editlights_cursorpushoff`](../38-cvars-reference/03-audio-cvars.md#r_editlights_cursorpushoff) | `cvar r_editlights_cursorpushoff(int, "4")` | Звук |
| [`r_editlights_import_ambient`](../38-cvars-reference/03-audio-cvars.md#r_editlights_import_ambient) | `cvar r_editlights_import_ambient(int, "0")` | Звук |
| [`r_editlights_import_diffuse`](../38-cvars-reference/03-audio-cvars.md#r_editlights_import_diffuse) | `cvar r_editlights_import_diffuse(int, "1")` | Звук |
| [`r_editlights_import_radius`](../38-cvars-reference/03-audio-cvars.md#r_editlights_import_radius) | `cvar r_editlights_import_radius(int, "1")` | Звук |
| [`r_editlights_import_specular`](../38-cvars-reference/03-audio-cvars.md#r_editlights_import_specular) | `cvar r_editlights_import_specular(int, "1")` | Звук |
| [`r_font_postprocess_mono`](../38-cvars-reference/03-audio-cvars.md#r_font_postprocess_mono) | `cvar r_font_postprocess_mono(int, "0")` | Звук |
| [`r_font_postprocess_outline`](../38-cvars-reference/03-audio-cvars.md#r_font_postprocess_outline) | `cvar r_font_postprocess_outline(int, "0")` | Звук |
| [`r_part_sparks_textured`](../38-cvars-reference/03-audio-cvars.md#r_part_sparks_textured) | `cvar r_part_sparks_textured(int, "1")` | Звук |
| [`r_part_sparks_trifan`](../38-cvars-reference/03-audio-cvars.md#r_part_sparks_trifan) | `cvar r_part_sparks_trifan(int, "1")` | Звук |
| [`rank_parms_first`](../38-cvars-reference/03-audio-cvars.md#rank_parms_first) | `cvar rank_parms_first(int, "0")` | Звук |
| [`rank_parms_last`](../38-cvars-reference/03-audio-cvars.md#rank_parms_last) | `cvar rank_parms_last(int, "31")` | Звук |
| [`ruleset_allow_overlong_sounds`](../38-cvars-reference/03-audio-cvars.md#ruleset_allow_overlong_sounds) | `cvar ruleset_allow_overlong_sounds(int, "1")` | Звук |
| [`s_al_distancemodel`](../38-cvars-reference/03-audio-cvars.md#s_al_distancemodel) | `cvar s_al_distancemodel(int, "2")` | Звук |
| [`s_al_dopplerfactor`](../38-cvars-reference/03-audio-cvars.md#s_al_dopplerfactor) | `cvar s_al_dopplerfactor(float, "1.0")` | Звук |
| [`s_al_max_distance`](../38-cvars-reference/03-audio-cvars.md#s_al_max_distance) | `cvar s_al_max_distance(int, "1000")` | Звук |
| [`s_al_rolloff_factor`](../38-cvars-reference/03-audio-cvars.md#s_al_rolloff_factor) | `cvar s_al_rolloff_factor(int, "1")` | Звук |
| [`s_al_speedofsound`](../38-cvars-reference/03-audio-cvars.md#s_al_speedofsound) | `cvar s_al_speedofsound(float, "343.3")` | Звук |
| [`s_al_static_listener`](../38-cvars-reference/03-audio-cvars.md#s_al_static_listener) | `cvar s_al_static_listener(int, "0")` | Звук |
| [`s_ambientfade`](../38-cvars-reference/03-audio-cvars.md#s_ambientfade) | `cvar s_ambientfade(int, "100")` | Звук |
| [`s_ambientlevel`](../38-cvars-reference/03-audio-cvars.md#s_ambientlevel) | `cvar s_ambientlevel(float, "0.3")` | Звук |
| [`s_bits`](../38-cvars-reference/03-audio-cvars.md#s_bits) | `cvar s_bits(int, "32")` | Звук |
| [`s_buffersize`](../38-cvars-reference/03-audio-cvars.md#s_buffersize) | `cvar s_buffersize(int, "0")` | Звук |
| [`s_device`](../38-cvars-reference/03-audio-cvars.md#s_device) | `cvar s_device(string, "")` | Звук |
| [`s_doppler`](../38-cvars-reference/03-audio-cvars.md#s_doppler) | `cvar s_doppler(int, "0")` | Звук |
| [`s_doppler_max`](../38-cvars-reference/03-audio-cvars.md#s_doppler_max) | `cvar s_doppler_max(int, "2")` | Звук |
| [`s_doppler_min`](../38-cvars-reference/03-audio-cvars.md#s_doppler_min) | `cvar s_doppler_min(float, "0.5")` | Звук |
| [`s_eax`](../38-cvars-reference/03-audio-cvars.md#s_eax) | `cvar s_eax(int, "0")` | Звук |
| [`s_inactive`](../38-cvars-reference/03-audio-cvars.md#s_inactive) | `cvar s_inactive(int, "1")` | Звук |
| [`s_khz`](../38-cvars-reference/03-audio-cvars.md#s_khz) | `cvar s_khz(string, "snd_khz")` | Звук |
| [`s_linearresample`](../38-cvars-reference/03-audio-cvars.md#s_linearresample) | `cvar s_linearresample(int, "1")` | Звук |
| [`s_linearresample_stream`](../38-cvars-reference/03-audio-cvars.md#s_linearresample_stream) | `cvar s_linearresample_stream(int, "0")` | Звук |
| [`s_loadas8bit`](../38-cvars-reference/03-audio-cvars.md#s_loadas8bit) | `cvar s_loadas8bit(int, "0")` | Звук |
| [`s_localvolume`](../38-cvars-reference/03-audio-cvars.md#s_localvolume) | `cvar s_localvolume(int, "1")` | Звук |
| [`s_mixahead`](../38-cvars-reference/03-audio-cvars.md#s_mixahead) | `cvar s_mixahead(float, "0.1")` | Звук |
| [`s_mixerthread`](../38-cvars-reference/03-audio-cvars.md#s_mixerthread) | `cvar s_mixerthread(int, "1")` | Звук |
| [`s_noextraupdate`](../38-cvars-reference/03-audio-cvars.md#s_noextraupdate) | `cvar s_noextraupdate(int, "0")` | Звук |
| [`s_nominaldistance`](../38-cvars-reference/03-audio-cvars.md#s_nominaldistance) | `cvar s_nominaldistance(int, "1000")` | Звук |
| [`s_numspeakers`](../38-cvars-reference/03-audio-cvars.md#s_numspeakers) | `cvar s_numspeakers(int, "2")` | Звук |
| [`s_precache`](../38-cvars-reference/03-audio-cvars.md#s_precache) | `cvar s_precache(int, "1")` | Звук |
| [`s_show`](../38-cvars-reference/03-audio-cvars.md#s_show) | `cvar s_show(int, "0")` | Звук |
| [`s_swapstereo`](../38-cvars-reference/03-audio-cvars.md#s_swapstereo) | `cvar s_swapstereo(int, "0")` | Звук |
| [`show_fps_x`](../38-cvars-reference/03-audio-cvars.md#show_fps_x) | `cvar show_fps_x(int, "-1")` | Звук |
| [`show_fps_y`](../38-cvars-reference/03-audio-cvars.md#show_fps_y) | `cvar show_fps_y(int, "-1")` | Звук |
| [`sv_cullentities_trace`](../38-cvars-reference/03-audio-cvars.md#sv_cullentities_trace) | `cvar sv_cullentities_trace(string, "")` | Звук |
| [`sv_loadentfiles_dir`](../38-cvars-reference/03-audio-cvars.md#sv_loadentfiles_dir) | `cvar sv_loadentfiles_dir(string, "")` | Звук |
| [`sv_sound_land`](../38-cvars-reference/03-audio-cvars.md#sv_sound_land) | `cvar sv_sound_land(string, "demon/dland2.wav")` | Звук |
| [`sv_sound_watersplash`](../38-cvars-reference/03-audio-cvars.md#sv_sound_watersplash) | `cvar sv_sound_watersplash(string, "misc/h2ohit1.wav")` | Звук |
| [`sv_voip`](../38-cvars-reference/03-audio-cvars.md#sv_voip) | `cvar sv_voip(int, "1")` | Звук |
| [`sv_voip_echo`](../38-cvars-reference/03-audio-cvars.md#sv_voip_echo) | `cvar sv_voip_echo(int, "0")` | Звук |
| [`sv_voip_record`](../38-cvars-reference/03-audio-cvars.md#sv_voip_record) | `cvar sv_voip_record(int, "0")` | Звук |
| [`sys_clockprecision`](../38-cvars-reference/03-audio-cvars.md#sys_clockprecision) | `cvar sys_clockprecision(int, "1")` | Звук |
| [`sys_clocktype`](../38-cvars-reference/03-audio-cvars.md#sys_clocktype) | `cvar sys_clocktype(string, "")` | Звук |
| [`sys_colorconsole`](../38-cvars-reference/03-audio-cvars.md#sys_colorconsole) | `cvar sys_colorconsole(int, "1")` | Звук |
| [`sys_disableTaskSwitch`](../38-cvars-reference/03-audio-cvars.md#sys_disabletaskswitch) | `cvar sys_disableTaskSwitch(int, "0")` | Звук |
| [`sys_disableWinKeys`](../38-cvars-reference/03-audio-cvars.md#sys_disablewinkeys) | `cvar sys_disableWinKeys(int, "0")` | Звук |
| [`sys_extrasleep`](../38-cvars-reference/03-audio-cvars.md#sys_extrasleep) | `cvar sys_extrasleep(int, "0")` | Звук |
| [`sys_highpriority`](../38-cvars-reference/03-audio-cvars.md#sys_highpriority) | `cvar sys_highpriority(int, "0")` | Звук |
| [`sys_keepscreenon`](../38-cvars-reference/03-audio-cvars.md#sys_keepscreenon) | `cvar sys_keepscreenon(int, "1")` | Звук |
| [`sys_linebuffer`](../38-cvars-reference/03-audio-cvars.md#sys_linebuffer) | `cvar sys_linebuffer(int, "1")` | Звук |
| [`sys_nostdout`](../38-cvars-reference/03-audio-cvars.md#sys_nostdout) | `cvar sys_nostdout(int, "0")` | Звук |
| [`sys_orientation`](../38-cvars-reference/03-audio-cvars.md#sys_orientation) | `cvar sys_orientation(string, "landscape")` | Звук |
| [`sys_platform`](../38-cvars-reference/03-audio-cvars.md#sys_platform) | `cvar sys_platform(string, "")` | Звук |
| [`sys_timestamps`](../38-cvars-reference/03-audio-cvars.md#sys_timestamps) | `cvar sys_timestamps(int, "0")` | Звук |
| [`sys_vibrate`](../38-cvars-reference/03-audio-cvars.md#sys_vibrate) | `cvar sys_vibrate(int, "1")` | Звук |
| [`tls_ignorecertificateerrors`](../38-cvars-reference/03-audio-cvars.md#tls_ignorecertificateerrors) | `cvar tls_ignorecertificateerrors(int, "0")` | Звук |
| [`tls_provider`](../38-cvars-reference/03-audio-cvars.md#tls_provider) | `cvar tls_provider(string, "")` | Звук |
| [`allow_download_configs`](../38-cvars-reference/04-network-server-cvars.md#allow_download_configs) | `cvar allow_download_configs(булево, "0")` | Сеть, сервер и мультиплеер |
| [`allow_download_copyrighted`](../38-cvars-reference/04-network-server-cvars.md#allow_download_copyrighted) | `cvar allow_download_copyrighted(булево, "0")` | Сеть, сервер и мультиплеер |
| [`allow_download_demos`](../38-cvars-reference/04-network-server-cvars.md#allow_download_demos) | `cvar allow_download_demos(булево, "1")` | Сеть, сервер и мультиплеер |
| [`allow_download_logs`](../38-cvars-reference/04-network-server-cvars.md#allow_download_logs) | `cvar allow_download_logs(булево, "0")` | Сеть, сервер и мультиплеер |
| [`allow_download_maps`](../38-cvars-reference/04-network-server-cvars.md#allow_download_maps) | `cvar allow_download_maps(булево, "1")` | Сеть, сервер и мультиплеер |
| [`allow_download_models`](../38-cvars-reference/04-network-server-cvars.md#allow_download_models) | `cvar allow_download_models(булево, "1")` | Сеть, сервер и мультиплеер |
| [`allow_download_pakcontents`](../38-cvars-reference/04-network-server-cvars.md#allow_download_pakcontents) | `cvar allow_download_pakcontents(целое перечисление, "0")` | Сеть, сервер и мультиплеер |
| [`allow_download_pakmaps`](../38-cvars-reference/04-network-server-cvars.md#allow_download_pakmaps) | `cvar allow_download_pakmaps(целое перечисление, "0")` | Сеть, сервер и мультиплеер |
| [`allow_download_skins`](../38-cvars-reference/04-network-server-cvars.md#allow_download_skins) | `cvar allow_download_skins(булево, "1")` | Сеть, сервер и мультиплеер |
| [`allow_download_sounds`](../38-cvars-reference/04-network-server-cvars.md#allow_download_sounds) | `cvar allow_download_sounds(булево, "1")` | Сеть, сервер и мультиплеер |
| [`coop`](../38-cvars-reference/04-network-server-cvars.md#coop) | `cvar coop(булево/целое, "")` | Сеть, сервер и мультиплеер |
| [`deathmatch`](../38-cvars-reference/04-network-server-cvars.md#deathmatch) | `cvar deathmatch(целое перечисление, "1")` | Сеть, сервер и мультиплеер |
| [`filterban`](../38-cvars-reference/04-network-server-cvars.md#filterban) | `cvar filterban(булево, "1")` | Сеть, сервер и мультиплеер |
| [`fraglimit`](../38-cvars-reference/04-network-server-cvars.md#fraglimit) | `cvar fraglimit(целое, "")` | Сеть, сервер и мультиплеер |
| [`hostname`](../38-cvars-reference/04-network-server-cvars.md#hostname) | `cvar hostname(строка, "unnamed")` | Сеть, сервер и мультиплеер |
| [`maxclients`](../38-cvars-reference/04-network-server-cvars.md#maxclients) | `cvar maxclients(целое, "8")` | Сеть, сервер и мультиплеер |
| [`maxspectators`](../38-cvars-reference/04-network-server-cvars.md#maxspectators) | `cvar maxspectators(целое, "8")` | Сеть, сервер и мультиплеер |
| [`net_compress`](../38-cvars-reference/04-network-server-cvars.md#net_compress) | `cvar net_compress(булево, "0")` | Сеть, сервер и мультиплеер |
| [`net_enabled`](../38-cvars-reference/04-network-server-cvars.md#net_enabled) | `cvar net_enabled(булево, "1")` | Сеть, сервер и мультиплеер |
| [`net_enable_http`](../38-cvars-reference/04-network-server-cvars.md#net_enable_http) | `cvar net_enable_http(булево, "1")` | Сеть, сервер и мультиплеер |
| [`net_enable_qtv`](../38-cvars-reference/04-network-server-cvars.md#net_enable_qtv) | `cvar net_enable_qtv(целое перечисление, "2")` | Сеть, сервер и мультиплеер |
| [`net_enable_rtcbroker`](../38-cvars-reference/04-network-server-cvars.md#net_enable_rtcbroker) | `cvar net_enable_rtcbroker(булево, "1")` | Сеть, сервер и мультиплеер |
| [`net_enable_tls`](../38-cvars-reference/04-network-server-cvars.md#net_enable_tls) | `cvar net_enable_tls(булево, "1")` | Сеть, сервер и мультиплеер |
| [`net_enable_websockets`](../38-cvars-reference/04-network-server-cvars.md#net_enable_websockets) | `cvar net_enable_websockets(булево, "0")` | Сеть, сервер и мультиплеер |
| [`net_hybriddualstack`](../38-cvars-reference/04-network-server-cvars.md#net_hybriddualstack) | `cvar net_hybriddualstack(булево, "1")` | Сеть, сервер и мультиплеер |
| [`net_ice_allowmdns`](../38-cvars-reference/04-network-server-cvars.md#net_ice_allowmdns) | `cvar net_ice_allowmdns(булево, "1")` | Сеть, сервер и мультиплеер |
| [`net_ice_allowstun`](../38-cvars-reference/04-network-server-cvars.md#net_ice_allowstun) | `cvar net_ice_allowstun(булево, "1")` | Сеть, сервер и мультиплеер |
| [`net_ice_allowturn`](../38-cvars-reference/04-network-server-cvars.md#net_ice_allowturn) | `cvar net_ice_allowturn(булево, "1")` | Сеть, сервер и мультиплеер |
| [`net_ice_broker`](../38-cvars-reference/04-network-server-cvars.md#net_ice_broker) | `cvar net_ice_broker(строка URL, "tls://master.frag-net.com:27950")` | Сеть, сервер и мультиплеер |
| [`net_ice_relayonly`](../38-cvars-reference/04-network-server-cvars.md#net_ice_relayonly) | `cvar net_ice_relayonly(булево, "0")` | Сеть, сервер и мультиплеер |
| [`net_ice_servers`](../38-cvars-reference/04-network-server-cvars.md#net_ice_servers) | `cvar net_ice_servers(строка списка, "")` | Сеть, сервер и мультиплеер |
| [`net_mtu`](../38-cvars-reference/04-network-server-cvars.md#net_mtu) | `cvar net_mtu(целое, "1440")` | Сеть, сервер и мультиплеер |
| [`password`](../38-cvars-reference/04-network-server-cvars.md#password) | `cvar password(строка, "")` | Сеть, сервер и мультиплеер |
| [`qtv_maxstreams`](../38-cvars-reference/04-network-server-cvars.md#qtv_maxstreams) | `cvar qtv_maxstreams(целое или пусто, "0")` | Сеть, сервер и мультиплеер |
| [`rcon_password`](../38-cvars-reference/04-network-server-cvars.md#rcon_password) | `cvar rcon_password(строка, "")` | Сеть, сервер и мультиплеер |
| [`spectator_password`](../38-cvars-reference/04-network-server-cvars.md#spectator_password) | `cvar spectator_password(строка, "")` | Сеть, сервер и мультиплеер |
| [`sv_banproxies`](../38-cvars-reference/04-network-server-cvars.md#sv_banproxies) | `cvar sv_banproxies(булево, "0")` | Сеть, сервер и мультиплеер |
| [`sv_bigcoords`](../38-cvars-reference/04-network-server-cvars.md#sv_bigcoords) | `cvar sv_bigcoords(булево, "1")` | Сеть, сервер и мультиплеер |
| [`sv_calcphs`](../38-cvars-reference/04-network-server-cvars.md#sv_calcphs) | `cvar sv_calcphs(целое перечисление, "2")` | Сеть, сервер и мультиплеер |
| [`sv_crypt_rcon`](../38-cvars-reference/04-network-server-cvars.md#sv_crypt_rcon) | `cvar sv_crypt_rcon(строка/переключатель, "")` | Сеть, сервер и мультиплеер |
| [`sv_cullplayers_trace`](../38-cvars-reference/04-network-server-cvars.md#sv_cullplayers_trace) | `cvar sv_cullplayers_trace(булево/целое, "")` | Сеть, сервер и мультиплеер |
| [`sv_demoClearOld`](../38-cvars-reference/04-network-server-cvars.md#sv_democlearold) | `cvar sv_demoClearOld(булево, "0")` | Сеть, сервер и мультиплеер |
| [`sv_demoExtensions`](../38-cvars-reference/04-network-server-cvars.md#sv_demoextensions) | `cvar sv_demoExtensions(целое перечисление, "1")` | Сеть, сервер и мультиплеер |
| [`sv_demofps`](../38-cvars-reference/04-network-server-cvars.md#sv_demofps) | `cvar sv_demofps(целое, "30")` | Сеть, сервер и мультиплеер |
| [`sv_demoMaxDirAge`](../38-cvars-reference/04-network-server-cvars.md#sv_demomaxdirage) | `cvar sv_demoMaxDirAge(время/целое, "0")` | Сеть, сервер и мультиплеер |
| [`sv_demoMaxDirCount`](../38-cvars-reference/04-network-server-cvars.md#sv_demomaxdircount) | `cvar sv_demoMaxDirCount(целое, "500")` | Сеть, сервер и мультиплеер |
| [`sv_demoMaxDirSize`](../38-cvars-reference/04-network-server-cvars.md#sv_demomaxdirsize) | `cvar sv_demoMaxDirSize(размер/строка, "100mb")` | Сеть, сервер и мультиплеер |
| [`sv_demoMaxSize`](../38-cvars-reference/04-network-server-cvars.md#sv_demomaxsize) | `cvar sv_demoMaxSize(размер/строка, "")` | Сеть, сервер и мультиплеер |
| [`sv_demoUseCache`](../38-cvars-reference/04-network-server-cvars.md#sv_demousecache) | `cvar sv_demoUseCache(булево/целое, "")` | Сеть, сервер и мультиплеер |
| [`sv_demo_write_csqc`](../38-cvars-reference/04-network-server-cvars.md#sv_demo_write_csqc) | `cvar sv_demo_write_csqc(булево/целое, "")` | Сеть, сервер и мультиплеер |
| [`sv_guidkey`](../38-cvars-reference/04-network-server-cvars.md#sv_guidkey) | `cvar sv_guidkey(строка, "")` | Сеть, сервер и мультиплеер |
| [`sv_heartbeat_checks`](../38-cvars-reference/04-network-server-cvars.md#sv_heartbeat_checks) | `cvar sv_heartbeat_checks(булево, "1")` | Сеть, сервер и мультиплеер |
| [`sv_heartbeat_interval`](../38-cvars-reference/04-network-server-cvars.md#sv_heartbeat_interval) | `cvar sv_heartbeat_interval(целое, "110")` | Сеть, сервер и мультиплеер |
| [`sv_heartbeattimeout`](../38-cvars-reference/04-network-server-cvars.md#sv_heartbeattimeout) | `cvar sv_heartbeattimeout(целое, "300")` | Сеть, сервер и мультиплеер |
| [`sv_limittics`](../38-cvars-reference/04-network-server-cvars.md#sv_limittics) | `cvar sv_limittics(целое, "3")` | Сеть, сервер и мультиплеер |
| [`sv_listen_dp`](../38-cvars-reference/04-network-server-cvars.md#sv_listen_dp) | `cvar sv_listen_dp(булево, "0")` | Сеть, сервер и мультиплеер |
| [`sv_listen_nq`](../38-cvars-reference/04-network-server-cvars.md#sv_listen_nq) | `cvar sv_listen_nq(целое перечисление, "0")` | Сеть, сервер и мультиплеер |
| [`sv_maxdrate`](../38-cvars-reference/04-network-server-cvars.md#sv_maxdrate) | `cvar sv_maxdrate(скорость/целое, "500000")` | Сеть, сервер и мультиплеер |
| [`sv_maxgames`](../38-cvars-reference/04-network-server-cvars.md#sv_maxgames) | `cvar sv_maxgames(целое, "100")` | Сеть, сервер и мультиплеер |
| [`sv_maxrate`](../38-cvars-reference/04-network-server-cvars.md#sv_maxrate) | `cvar sv_maxrate(скорость/целое, "50000")` | Сеть, сервер и мультиплеер |
| [`sv_maxservers`](../38-cvars-reference/04-network-server-cvars.md#sv_maxservers) | `cvar sv_maxservers(целое, "10000")` | Сеть, сервер и мультиплеер |
| [`sv_maxtic`](../38-cvars-reference/04-network-server-cvars.md#sv_maxtic) | `cvar sv_maxtic(дробное, "0.1")` | Сеть, сервер и мультиплеер |
| [`sv_minping`](../38-cvars-reference/04-network-server-cvars.md#sv_minping) | `cvar sv_minping(целое, "")` | Сеть, сервер и мультиплеер |
| [`sv_mintic`](../38-cvars-reference/04-network-server-cvars.md#sv_mintic) | `cvar sv_mintic(дробное, "0.013")` | Сеть, сервер и мультиплеер |
| [`sv_nailhack`](../38-cvars-reference/04-network-server-cvars.md#sv_nailhack) | `cvar sv_nailhack(булево, "1")` | Сеть, сервер и мультиплеер |
| [`sv_playerslots`](../38-cvars-reference/04-network-server-cvars.md#sv_playerslots) | `cvar sv_playerslots(целое или пусто, "")` | Сеть, сервер и мультиплеер |
| [`sv_protocol`](../38-cvars-reference/04-network-server-cvars.md#sv_protocol) | `cvar sv_protocol(строка списка, "")` | Сеть, сервер и мультиплеер |
| [`sv_public`](../38-cvars-reference/04-network-server-cvars.md#sv_public) | `cvar sv_public(целое перечисление, "0")` | Сеть, сервер и мультиплеер |
| [`sv_rconlim`](../38-cvars-reference/04-network-server-cvars.md#sv_rconlim) | `cvar sv_rconlim(целое, "4")` | Сеть, сервер и мультиплеер |
| [`sv_reconnectlimit`](../38-cvars-reference/04-network-server-cvars.md#sv_reconnectlimit) | `cvar sv_reconnectlimit(целое, "0")` | Сеть, сервер и мультиплеер |
| [`sv_reliable_sound`](../38-cvars-reference/04-network-server-cvars.md#sv_reliable_sound) | `cvar sv_reliable_sound(булево, "0")` | Сеть, сервер и мультиплеер |
| [`sv_reportheartbeats`](../38-cvars-reference/04-network-server-cvars.md#sv_reportheartbeats) | `cvar sv_reportheartbeats(целое перечисление, "2")` | Сеть, сервер и мультиплеер |
| [`sv_serverip`](../38-cvars-reference/04-network-server-cvars.md#sv_serverip) | `cvar sv_serverip(строка адреса, "")` | Сеть, сервер и мультиплеер |
| [`sv_slaverequery`](../38-cvars-reference/04-network-server-cvars.md#sv_slaverequery) | `cvar sv_slaverequery(целое, "120")` | Сеть, сервер и мультиплеер |
| [`sv_timestamplen`](../38-cvars-reference/04-network-server-cvars.md#sv_timestamplen) | `cvar sv_timestamplen(целое, "60")` | Сеть, сервер и мультиплеер |
| [`sv_use_dns`](../38-cvars-reference/04-network-server-cvars.md#sv_use_dns) | `cvar sv_use_dns(булево/строка, "")` | Сеть, сервер и мультиплеер |
| [`teamplay`](../38-cvars-reference/04-network-server-cvars.md#teamplay) | `cvar teamplay(целое, "")` | Сеть, сервер и мультиплеер |
| [`timelimit`](../38-cvars-reference/04-network-server-cvars.md#timelimit) | `cvar timelimit(целое, "")` | Сеть, сервер и мультиплеер |
| [`timeout`](../38-cvars-reference/04-network-server-cvars.md#timeout) | `cvar timeout(целое, "65")` | Сеть, сервер и мультиплеер |
| [`zombietime`](../38-cvars-reference/04-network-server-cvars.md#zombietime) | `cvar zombietime(целое, "2")` | Сеть, сервер и мультиплеер |
| [`allow_download`](../38-cvars-reference/04-network-server-cvars.md#allow_download) | `cvar allow_download(int, "1")` | Сеть, сервер и мультиплеер |
| [`allow_download_locs`](../38-cvars-reference/04-network-server-cvars.md#allow_download_locs) | `cvar allow_download_locs(int, "1")` | Сеть, сервер и мультиплеер |
| [`allow_download_other`](../38-cvars-reference/04-network-server-cvars.md#allow_download_other) | `cvar allow_download_other(int, "0")` | Сеть, сервер и мультиплеер |
| [`allow_download_packages`](../38-cvars-reference/04-network-server-cvars.md#allow_download_packages) | `cvar allow_download_packages(int, "1")` | Сеть, сервер и мультиплеер |
| [`allow_download_particles`](../38-cvars-reference/04-network-server-cvars.md#allow_download_particles) | `cvar allow_download_particles(int, "1")` | Сеть, сервер и мультиплеер |
| [`allow_download_refpackages`](../38-cvars-reference/04-network-server-cvars.md#allow_download_refpackages) | `cvar allow_download_refpackages(int, "1")` | Сеть, сервер и мультиплеер |
| [`allow_download_root`](../38-cvars-reference/04-network-server-cvars.md#allow_download_root) | `cvar allow_download_root(int, "0")` | Сеть, сервер и мультиплеер |
| [`allow_download_textures`](../38-cvars-reference/04-network-server-cvars.md#allow_download_textures) | `cvar allow_download_textures(int, "1")` | Сеть, сервер и мультиплеер |
| [`allow_download_wads`](../38-cvars-reference/04-network-server-cvars.md#allow_download_wads) | `cvar allow_download_wads(int, "1")` | Сеть, сервер и мультиплеер |
| [`capturerate`](../38-cvars-reference/04-network-server-cvars.md#capturerate) | `cvar capturerate(int, "30")` | Сеть, сервер и мультиплеер |
| [`cl_download_csprogs`](../38-cvars-reference/04-network-server-cvars.md#cl_download_csprogs) | `cvar cl_download_csprogs(int, "1")` | Сеть, сервер и мультиплеер |
| [`cl_download_mapsrc`](../38-cvars-reference/04-network-server-cvars.md#cl_download_mapsrc) | `cvar cl_download_mapsrc(string, "")` | Сеть, сервер и мультиплеер |
| [`cl_download_packages`](../38-cvars-reference/04-network-server-cvars.md#cl_download_packages) | `cvar cl_download_packages(int, "1")` | Сеть, сервер и мультиплеер |
| [`cl_download_redirection`](../38-cvars-reference/04-network-server-cvars.md#cl_download_redirection) | `cvar cl_download_redirection(int, "2")` | Сеть, сервер и мультиплеер |
| [`cl_download_wait`](../38-cvars-reference/04-network-server-cvars.md#cl_download_wait) | `cvar cl_download_wait(int, "1")` | Сеть, сервер и мультиплеер |
| [`cl_downloads`](../38-cvars-reference/04-network-server-cvars.md#cl_downloads) | `cvar cl_downloads(int, "1")` | Сеть, сервер и мультиплеер |
| [`com_fullgamename`](../38-cvars-reference/04-network-server-cvars.md#com_fullgamename) | `cvar com_fullgamename(string, "fs_gamename")` | Сеть, сервер и мультиплеер |
| [`com_gamedirnativecode`](../38-cvars-reference/04-network-server-cvars.md#com_gamedirnativecode) | `cvar com_gamedirnativecode(int, "0")` | Сеть, сервер и мультиплеер |
| [`com_highlightcolor`](../38-cvars-reference/04-network-server-cvars.md#com_highlightcolor) | `cvar com_highlightcolor(string, "ANSI colour to be used for highlighted text, used when com_parseutf8 is active.")` | Сеть, сервер и мультиплеер |
| [`com_parseutf8`](../38-cvars-reference/04-network-server-cvars.md#com_parseutf8) | `cvar com_parseutf8(int, "1")` | Сеть, сервер и мультиплеер |
| [`com_protocolname`](../38-cvars-reference/04-network-server-cvars.md#com_protocolname) | `cvar com_protocolname(string, "com_gamename")` | Сеть, сервер и мультиплеер |
| [`com_protocolversion`](../38-cvars-reference/04-network-server-cvars.md#com_protocolversion) | `cvar com_protocolversion(int, "3")` | Сеть, сервер и мультиплеер |
| [`drate`](../38-cvars-reference/04-network-server-cvars.md#drate) | `cvar drate(int, "3000000")` | Сеть, сервер и мультиплеер |
| [`fraglog_public`](../38-cvars-reference/04-network-server-cvars.md#fraglog_public) | `cvar fraglog_public(int, "1")` | Сеть, сервер и мультиплеер |
| [`gl_blacklist_generatemipmap`](../38-cvars-reference/04-network-server-cvars.md#gl_blacklist_generatemipmap) | `cvar gl_blacklist_generatemipmap(int, "1")` | Сеть, сервер и мультиплеер |
| [`host_speeds`](../38-cvars-reference/04-network-server-cvars.md#host_speeds) | `cvar host_speeds(int, "0")` | Сеть, сервер и мультиплеер |
| [`net_enable_`](../38-cvars-reference/04-network-server-cvars.md#net_enable_) | `cvar net_enable_(int, "0")` | Сеть, сервер и мультиплеер |
| [`net_enable_dtls`](../38-cvars-reference/04-network-server-cvars.md#net_enable_dtls) | `cvar net_enable_dtls(string, "")` | Сеть, сервер и мультиплеер |
| [`net_enable_qizmo`](../38-cvars-reference/04-network-server-cvars.md#net_enable_qizmo) | `cvar net_enable_qizmo(int, "1")` | Сеть, сервер и мультиплеер |
| [`net_fakeloss`](../38-cvars-reference/04-network-server-cvars.md#net_fakeloss) | `cvar net_fakeloss(int, "0")` | Сеть, сервер и мультиплеер |
| [`net_fakemtu`](../38-cvars-reference/04-network-server-cvars.md#net_fakemtu) | `cvar net_fakemtu(int, "0")` | Сеть, сервер и мультиплеер |
| [`net_ice_debug`](../38-cvars-reference/04-network-server-cvars.md#net_ice_debug) | `cvar net_ice_debug(int, "0")` | Сеть, сервер и мультиплеер |
| [`net_ice_exchangeprivateips`](../38-cvars-reference/04-network-server-cvars.md#net_ice_exchangeprivateips) | `cvar net_ice_exchangeprivateips(int, "0")` | Сеть, сервер и мультиплеер |
| [`net_ice_usewebrtc`](../38-cvars-reference/04-network-server-cvars.md#net_ice_usewebrtc) | `cvar net_ice_usewebrtc(string, "")` | Сеть, сервер и мультиплеер |
| [`net_upnpigp`](../38-cvars-reference/04-network-server-cvars.md#net_upnpigp) | `cvar net_upnpigp(int, "0")` | Сеть, сервер и мультиплеер |
| [`pausable`](../38-cvars-reference/04-network-server-cvars.md#pausable) | `cvar pausable(string, "")` | Сеть, сервер и мультиплеер |
| [`qtv_password`](../38-cvars-reference/04-network-server-cvars.md#qtv_password) | `cvar qtv_password(string, "")` | Сеть, сервер и мультиплеер |
| [`qtv_streamport`](../38-cvars-reference/04-network-server-cvars.md#qtv_streamport) | `cvar qtv_streamport(string, "")` | Сеть, сервер и мультиплеер |
| [`qtvcl_eztvextensions`](../38-cvars-reference/04-network-server-cvars.md#qtvcl_eztvextensions) | `cvar qtvcl_eztvextensions(int, "1")` | Сеть, сервер и мультиплеер |
| [`qtvcl_forceversion1`](../38-cvars-reference/04-network-server-cvars.md#qtvcl_forceversion1) | `cvar qtvcl_forceversion1(int, "0")` | Сеть, сервер и мультиплеер |
| [`r_image_downloadsizelimit`](../38-cvars-reference/04-network-server-cvars.md#r_image_downloadsizelimit) | `cvar r_image_downloadsizelimit(int, "131072")` | Сеть, сервер и мультиплеер |
| [`rate`](../38-cvars-reference/04-network-server-cvars.md#rate) | `cvar rate(int, "30000")` | Сеть, сервер и мультиплеер |
| [`samelevel`](../38-cvars-reference/04-network-server-cvars.md#samelevel) | `cvar samelevel(string, "")` | Сеть, сервер и мультиплеер |
| [`sb_showfraglimit`](../38-cvars-reference/04-network-server-cvars.md#sb_showfraglimit) | `cvar sb_showfraglimit(int, "0")` | Сеть, сервер и мультиплеер |
| [`sb_showtimelimit`](../38-cvars-reference/04-network-server-cvars.md#sb_showtimelimit) | `cvar sb_showtimelimit(int, "0")` | Сеть, сервер и мультиплеер |
| [`skill`](../38-cvars-reference/04-network-server-cvars.md#skill) | `cvar skill(string, "")` | Сеть, сервер и мультиплеер |
| [`spawn`](../38-cvars-reference/04-network-server-cvars.md#spawn) | `cvar spawn(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_aim`](../38-cvars-reference/04-network-server-cvars.md#sv_aim) | `cvar sv_aim(int, "2")` | Сеть, сервер и мультиплеер |
| [`sv_autooffload`](../38-cvars-reference/04-network-server-cvars.md#sv_autooffload) | `cvar sv_autooffload(int, "0")` | Сеть, сервер и мультиплеер |
| [`sv_autosave`](../38-cvars-reference/04-network-server-cvars.md#sv_autosave) | `cvar sv_autosave(int, "5")` | Сеть, сервер и мультиплеер |
| [`sv_chatfilter`](../38-cvars-reference/04-network-server-cvars.md#sv_chatfilter) | `cvar sv_chatfilter(int, "0")` | Сеть, сервер и мультиплеер |
| [`sv_cheatpc`](../38-cvars-reference/04-network-server-cvars.md#sv_cheatpc) | `cvar sv_cheatpc(int, "125")` | Сеть, сервер и мультиплеер |
| [`sv_cheats`](../38-cvars-reference/04-network-server-cvars.md#sv_cheats) | `cvar sv_cheats(int, "0")` | Сеть, сервер и мультиплеер |
| [`sv_cheatspeedchecktime`](../38-cvars-reference/04-network-server-cvars.md#sv_cheatspeedchecktime) | `cvar sv_cheatspeedchecktime(int, "30")` | Сеть, сервер и мультиплеер |
| [`sv_cmdlikercon`](../38-cvars-reference/04-network-server-cvars.md#sv_cmdlikercon) | `cvar sv_cmdlikercon(int, "0")` | Сеть, сервер и мультиплеер |
| [`sv_compatiblehulls`](../38-cvars-reference/04-network-server-cvars.md#sv_compatiblehulls) | `cvar sv_compatiblehulls(int, "1")` | Сеть, сервер и мультиплеер |
| [`sv_csqc_progname`](../38-cvars-reference/04-network-server-cvars.md#sv_csqc_progname) | `cvar sv_csqc_progname(string, "csprogs.dat")` | Сеть, сервер и мультиплеер |
| [`sv_csqcdebug`](../38-cvars-reference/04-network-server-cvars.md#sv_csqcdebug) | `cvar sv_csqcdebug(int, "0")` | Сеть, сервер и мультиплеер |
| [`sv_demoAutoCompress`](../38-cvars-reference/04-network-server-cvars.md#sv_demoautocompress) | `cvar sv_demoAutoCompress(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_demoAutoPrefix`](../38-cvars-reference/04-network-server-cvars.md#sv_demoautoprefix) | `cvar sv_demoAutoPrefix(string, "auto_")` | Сеть, сервер и мультиплеер |
| [`sv_demoAutoRecord`](../38-cvars-reference/04-network-server-cvars.md#sv_demoautorecord) | `cvar sv_demoAutoRecord(int, "0")` | Сеть, сервер и мультиплеер |
| [`sv_demoCacheSize`](../38-cvars-reference/04-network-server-cvars.md#sv_democachesize) | `cvar sv_demoCacheSize(string, "0x80000")` | Сеть, сервер и мультиплеер |
| [`sv_demoDir`](../38-cvars-reference/04-network-server-cvars.md#sv_demodir) | `cvar sv_demoDir(string, "demos")` | Сеть, сервер и мультиплеер |
| [`sv_demoDirAlt`](../38-cvars-reference/04-network-server-cvars.md#sv_demodiralt) | `cvar sv_demoDirAlt(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_demoExtraNames`](../38-cvars-reference/04-network-server-cvars.md#sv_demoextranames) | `cvar sv_demoExtraNames(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_demoPings`](../38-cvars-reference/04-network-server-cvars.md#sv_demopings) | `cvar sv_demoPings(int, "10")` | Сеть, сервер и мультиплеер |
| [`sv_demoPrefix`](../38-cvars-reference/04-network-server-cvars.md#sv_demoprefix) | `cvar sv_demoPrefix(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_demoSuffix`](../38-cvars-reference/04-network-server-cvars.md#sv_demosuffix) | `cvar sv_demoSuffix(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_demotxt`](../38-cvars-reference/04-network-server-cvars.md#sv_demotxt) | `cvar sv_demotxt(int, "1")` | Сеть, сервер и мультиплеер |
| [`sv_dlURL`](../38-cvars-reference/04-network-server-cvars.md#sv_dlurl) | `cvar sv_dlURL(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_floodprotect`](../38-cvars-reference/04-network-server-cvars.md#sv_floodprotect) | `cvar sv_floodprotect(int, "1")` | Сеть, сервер и мультиплеер |
| [`sv_floodprotect_interval`](../38-cvars-reference/04-network-server-cvars.md#sv_floodprotect_interval) | `cvar sv_floodprotect_interval(int, "4")` | Сеть, сервер и мультиплеер |
| [`sv_floodprotect_messages`](../38-cvars-reference/04-network-server-cvars.md#sv_floodprotect_messages) | `cvar sv_floodprotect_messages(int, "4")` | Сеть, сервер и мультиплеер |
| [`sv_floodprotect_sendmessage`](../38-cvars-reference/04-network-server-cvars.md#sv_floodprotect_sendmessage) | `cvar sv_floodprotect_sendmessage(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_floodprotect_silencetime`](../38-cvars-reference/04-network-server-cvars.md#sv_floodprotect_silencetime) | `cvar sv_floodprotect_silencetime(int, "10")` | Сеть, сервер и мультиплеер |
| [`sv_floodprotect_suicide`](../38-cvars-reference/04-network-server-cvars.md#sv_floodprotect_suicide) | `cvar sv_floodprotect_suicide(int, "1")` | Сеть, сервер и мультиплеер |
| [`sv_ftp`](../38-cvars-reference/04-network-server-cvars.md#sv_ftp) | `cvar sv_ftp(int, "0")` | Сеть, сервер и мультиплеер |
| [`sv_ftp_port`](../38-cvars-reference/04-network-server-cvars.md#sv_ftp_port) | `cvar sv_ftp_port(int, "21")` | Сеть, сервер и мультиплеер |
| [`sv_ftp_port_range`](../38-cvars-reference/04-network-server-cvars.md#sv_ftp_port_range) | `cvar sv_ftp_port_range(int, "0")` | Сеть, сервер и мультиплеер |
| [`sv_fulllevel`](../38-cvars-reference/04-network-server-cvars.md#sv_fulllevel) | `cvar sv_fulllevel(int, "51")` | Сеть, сервер и мультиплеер |
| [`sv_fullredirect`](../38-cvars-reference/04-network-server-cvars.md#sv_fullredirect) | `cvar sv_fullredirect(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_gameplayfix_honest_tracelines`](../38-cvars-reference/04-network-server-cvars.md#sv_gameplayfix_honest_tracelines) | `cvar sv_gameplayfix_honest_tracelines(int, "1")` | Сеть, сервер и мультиплеер |
| [`sv_gameplayfix_radialmaxvelocity`](../38-cvars-reference/04-network-server-cvars.md#sv_gameplayfix_radialmaxvelocity) | `cvar sv_gameplayfix_radialmaxvelocity(int, "0")` | Сеть, сервер и мультиплеер |
| [`sv_gameplayfix_setmodelrealbox`](../38-cvars-reference/04-network-server-cvars.md#sv_gameplayfix_setmodelrealbox) | `cvar sv_gameplayfix_setmodelrealbox(int, "1")` | Сеть, сервер и мультиплеер |
| [`sv_gameplayfix_setmodelsize_qw`](../38-cvars-reference/04-network-server-cvars.md#sv_gameplayfix_setmodelsize_qw) | `cvar sv_gameplayfix_setmodelsize_qw(int, "0")` | Сеть, сервер и мультиплеер |
| [`sv_gameplayfix_spawnbeforethinks`](../38-cvars-reference/04-network-server-cvars.md#sv_gameplayfix_spawnbeforethinks) | `cvar sv_gameplayfix_spawnbeforethinks(int, "0")` | Сеть, сервер и мультиплеер |
| [`sv_gameplayfix_stepdown`](../38-cvars-reference/04-network-server-cvars.md#sv_gameplayfix_stepdown) | `cvar sv_gameplayfix_stepdown(int, "0")` | Сеть, сервер и мультиплеер |
| [`sv_gamespeed`](../38-cvars-reference/04-network-server-cvars.md#sv_gamespeed) | `cvar sv_gamespeed(int, "1")` | Сеть, сервер и мультиплеер |
| [`sv_getrealip`](../38-cvars-reference/04-network-server-cvars.md#sv_getrealip) | `cvar sv_getrealip(int, "0")` | Сеть, сервер и мультиплеер |
| [`sv_hideinactivegames`](../38-cvars-reference/04-network-server-cvars.md#sv_hideinactivegames) | `cvar sv_hideinactivegames(int, "1")` | Сеть, сервер и мультиплеер |
| [`sv_highchars`](../38-cvars-reference/04-network-server-cvars.md#sv_highchars) | `cvar sv_highchars(int, "1")` | Сеть, сервер и мультиплеер |
| [`sv_http`](../38-cvars-reference/04-network-server-cvars.md#sv_http) | `cvar sv_http(int, "0")` | Сеть, сервер и мультиплеер |
| [`sv_http_port`](../38-cvars-reference/04-network-server-cvars.md#sv_http_port) | `cvar sv_http_port(int, "80")` | Сеть, сервер и мультиплеер |
| [`sv_listen_q3`](../38-cvars-reference/04-network-server-cvars.md#sv_listen_q3) | `cvar sv_listen_q3(int, "0")` | Сеть, сервер и мультиплеер |
| [`sv_listen_qw`](../38-cvars-reference/04-network-server-cvars.md#sv_listen_qw) | `cvar sv_listen_qw(int, "1")` | Сеть, сервер и мультиплеер |
| [`sv_loadentfiles`](../38-cvars-reference/04-network-server-cvars.md#sv_loadentfiles) | `cvar sv_loadentfiles(int, "1")` | Сеть, сервер и мультиплеер |
| [`sv_mapcheck`](../38-cvars-reference/04-network-server-cvars.md#sv_mapcheck) | `cvar sv_mapcheck(int, "1")` | Сеть, сервер и мультиплеер |
| [`sv_master`](../38-cvars-reference/04-network-server-cvars.md#sv_master) | `cvar sv_master(int, "0")` | Сеть, сервер и мультиплеер |
| [`sv_masterport`](../38-cvars-reference/04-network-server-cvars.md#sv_masterport) | `cvar sv_masterport(string, " ")` | Сеть, сервер и мультиплеер |
| [`sv_masterport_tcp`](../38-cvars-reference/04-network-server-cvars.md#sv_masterport_tcp) | `cvar sv_masterport_tcp(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_maxaim`](../38-cvars-reference/04-network-server-cvars.md#sv_maxaim) | `cvar sv_maxaim(int, "22")` | Сеть, сервер и мультиплеер |
| [`sv_nopvs`](../38-cvars-reference/04-network-server-cvars.md#sv_nopvs) | `cvar sv_nopvs(int, "0")` | Сеть, сервер и мультиплеер |
| [`sv_phs`](../38-cvars-reference/04-network-server-cvars.md#sv_phs) | `cvar sv_phs(int, "1")` | Сеть, сервер и мультиплеер |
| [`sv_ping_ignorepl`](../38-cvars-reference/04-network-server-cvars.md#sv_ping_ignorepl) | `cvar sv_ping_ignorepl(int, "0")` | Сеть, сервер и мультиплеер |
| [`sv_playermodelchecks`](../38-cvars-reference/04-network-server-cvars.md#sv_playermodelchecks) | `cvar sv_playermodelchecks(int, "0")` | Сеть, сервер и мультиплеер |
| [`sv_port`](../38-cvars-reference/04-network-server-cvars.md#sv_port) | `cvar sv_port(string, "Port number to list on for inbound udp-based connections (including dtls variants). Can be a list for multiple ports. If ips are included then binds to that specific interface. Whether any specific protocol is accepted depends upon other settings.")` | Сеть, сервер и мультиплеер |
| [`sv_port_ipv6`](../38-cvars-reference/04-network-server-cvars.md#sv_port_ipv6) | `cvar sv_port_ipv6(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_port_ipx`](../38-cvars-reference/04-network-server-cvars.md#sv_port_ipx) | `cvar sv_port_ipx(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_port_natpmp`](../38-cvars-reference/04-network-server-cvars.md#sv_port_natpmp) | `cvar sv_port_natpmp(string, "If set (typically to 5351), automatically configures your router's port forwarding. You can instead specify the full ip address of your router (192.168.1.1:5351 for example). Your router must have NAT-PMP supported and enabled.")` | Сеть, сервер и мультиплеер |
| [`sv_port_rtc`](../38-cvars-reference/04-network-server-cvars.md#sv_port_rtc) | `cvar sv_port_rtc(string, "/")` | Сеть, сервер и мультиплеер |
| [`sv_port_tcp`](../38-cvars-reference/04-network-server-cvars.md#sv_port_tcp) | `cvar sv_port_tcp(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_port_tcp6`](../38-cvars-reference/04-network-server-cvars.md#sv_port_tcp6) | `cvar sv_port_tcp6(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_port_unix`](../38-cvars-reference/04-network-server-cvars.md#sv_port_unix) | `cvar sv_port_unix(string, "@qsock.fte")` | Сеть, сервер и мультиплеер |
| [`sv_progs`](../38-cvars-reference/04-network-server-cvars.md#sv_progs) | `cvar sv_progs(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_protocol_nq`](../38-cvars-reference/04-network-server-cvars.md#sv_protocol_nq) | `cvar sv_protocol_nq(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_pupglow`](../38-cvars-reference/04-network-server-cvars.md#sv_pupglow) | `cvar sv_pupglow(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_pure`](../38-cvars-reference/04-network-server-cvars.md#sv_pure) | `cvar sv_pure(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_readlevel`](../38-cvars-reference/04-network-server-cvars.md#sv_readlevel) | `cvar sv_readlevel(int, "0")` | Сеть, сервер и мультиплеер |
| [`sv_realip_kick`](../38-cvars-reference/04-network-server-cvars.md#sv_realip_kick) | `cvar sv_realip_kick(int, "0")` | Сеть, сервер и мультиплеер |
| [`sv_realip_timeout`](../38-cvars-reference/04-network-server-cvars.md#sv_realip_timeout) | `cvar sv_realip_timeout(int, "10")` | Сеть, сервер и мультиплеер |
| [`sv_realiphostname_ipv4`](../38-cvars-reference/04-network-server-cvars.md#sv_realiphostname_ipv4) | `cvar sv_realiphostname_ipv4(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_realiphostname_ipv6`](../38-cvars-reference/04-network-server-cvars.md#sv_realiphostname_ipv6) | `cvar sv_realiphostname_ipv6(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_resetparms`](../38-cvars-reference/04-network-server-cvars.md#sv_resetparms) | `cvar sv_resetparms(int, "0")` | Сеть, сервер и мультиплеер |
| [`sv_savefmt`](../38-cvars-reference/04-network-server-cvars.md#sv_savefmt) | `cvar sv_savefmt(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_showconnectionlessmessages`](../38-cvars-reference/04-network-server-cvars.md#sv_showconnectionlessmessages) | `cvar sv_showconnectionlessmessages(int, "0")` | Сеть, сервер и мультиплеер |
| [`sv_showpredloss`](../38-cvars-reference/04-network-server-cvars.md#sv_showpredloss) | `cvar sv_showpredloss(int, "0")` | Сеть, сервер и мультиплеер |
| [`sv_sortlist`](../38-cvars-reference/04-network-server-cvars.md#sv_sortlist) | `cvar sv_sortlist(int, "3")` | Сеть, сервер и мультиплеер |
| [`sv_specprint`](../38-cvars-reference/04-network-server-cvars.md#sv_specprint) | `cvar sv_specprint(int, "3")` | Сеть, сервер и мультиплеер |
| [`sv_spectalk`](../38-cvars-reference/04-network-server-cvars.md#sv_spectalk) | `cvar sv_spectalk(int, "1")` | Сеть, сервер и мультиплеер |
| [`sv_sql_defaultdb`](../38-cvars-reference/04-network-server-cvars.md#sv_sql_defaultdb) | `cvar sv_sql_defaultdb(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_sql_driver`](../38-cvars-reference/04-network-server-cvars.md#sv_sql_driver) | `cvar sv_sql_driver(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_sql_host`](../38-cvars-reference/04-network-server-cvars.md#sv_sql_host) | `cvar sv_sql_host(string, "127.0.0.1")` | Сеть, сервер и мультиплеер |
| [`sv_sql_password`](../38-cvars-reference/04-network-server-cvars.md#sv_sql_password) | `cvar sv_sql_password(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_sql_username`](../38-cvars-reference/04-network-server-cvars.md#sv_sql_username) | `cvar sv_sql_username(string, "")` | Сеть, сервер и мультиплеер |
| [`sv_userinfo_bytelimit`](../38-cvars-reference/04-network-server-cvars.md#sv_userinfo_bytelimit) | `cvar sv_userinfo_bytelimit(int, "8192")` | Сеть, сервер и мультиплеер |
| [`sv_userinfo_keylimit`](../38-cvars-reference/04-network-server-cvars.md#sv_userinfo_keylimit) | `cvar sv_userinfo_keylimit(int, "128")` | Сеть, сервер и мультиплеер |
| [`sv_writelevel`](../38-cvars-reference/04-network-server-cvars.md#sv_writelevel) | `cvar sv_writelevel(int, "35")` | Сеть, сервер и мультиплеер |
| [`vK_khr_fragment_shading_rate`](../38-cvars-reference/04-network-server-cvars.md#vk_khr_fragment_shading_rate) | `cvar vK_khr_fragment_shading_rate(string, "")` | Сеть, сервер и мультиплеер |
| [`cl_anglespeedkey`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_anglespeedkey) | `cvar cl_anglespeedkey(float, "1.5")` | Физика и игровой процесс |
| [`cl_backspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_backspeed) | `cvar cl_backspeed(float/string, "")` | Физика и игровой процесс |
| [`cl_fastaccel`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_fastaccel) | `cvar cl_fastaccel(boolean/int, "1")` | Физика и игровой процесс |
| [`cl_forwardspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_forwardspeed) | `cvar cl_forwardspeed(float, "400")` | Физика и игровой процесс |
| [`cl_iDrive`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_idrive) | `cvar cl_iDrive(boolean/int, "1")` | Физика и игровой процесс |
| [`cl_instantrotate`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_instantrotate) | `cvar cl_instantrotate(boolean/int, "1")` | Физика и игровой процесс |
| [`cl_lerp_driftbias`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_lerp_driftbias) | `cvar cl_lerp_driftbias(float, "0")` | Физика и игровой процесс |
| [`cl_lerp_driftfrac`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_lerp_driftfrac) | `cvar cl_lerp_driftfrac(float, "0")` | Физика и игровой процесс |
| [`cl_lerp_smooth`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_lerp_smooth) | `cvar cl_lerp_smooth(int, "2")` | Физика и игровой процесс |
| [`cl_movement`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_movement) | `cvar cl_movement(boolean/int, "1")` | Физика и игровой процесс |
| [`cl_nopred`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_nopred) | `cvar cl_nopred(boolean/int, "0")` | Физика и игровой процесс |
| [`cl_pitchspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_pitchspeed) | `cvar cl_pitchspeed(float, "150")` | Физика и игровой процесс |
| [`cl_predict_extrapolate`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_predict_extrapolate) | `cvar cl_predict_extrapolate(int/string, "")` | Физика и игровой процесс |
| [`cl_predict_timenudge`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_predict_timenudge) | `cvar cl_predict_timenudge(float, "0")` | Физика и игровой процесс |
| [`cl_rollangle`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_rollangle) | `cvar cl_rollangle(float, "2.0")` | Физика и игровой процесс |
| [`cl_rollspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_rollspeed) | `cvar cl_rollspeed(float, "200")` | Физика и игровой процесс |
| [`cl_run`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_run) | `cvar cl_run(boolean/int, "0")` | Физика и игровой процесс |
| [`cl_sidespeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_sidespeed) | `cvar cl_sidespeed(float, "400")` | Физика и игровой процесс |
| [`cl_smartjump`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_smartjump) | `cvar cl_smartjump(boolean/int, "1")` | Физика и игровой процесс |
| [`cl_upspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_upspeed) | `cvar cl_upspeed(float, "400")` | Физика и игровой процесс |
| [`cl_yawspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_yawspeed) | `cvar cl_yawspeed(float, "140")` | Физика и игровой процесс |
| [`pm_airstep`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_airstep) | `cvar pm_airstep(boolean/int/string, "")` | Физика и игровой процесс |
| [`pm_autobunny`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_autobunny) | `cvar pm_autobunny(boolean/int/string, "")` | Физика и игровой процесс |
| [`pm_bunnyfriction`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_bunnyfriction) | `cvar pm_bunnyfriction(boolean/int/string, "")` | Физика и игровой процесс |
| [`pm_bunnyspeedcap`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_bunnyspeedcap) | `cvar pm_bunnyspeedcap(float/string, "")` | Физика и игровой процесс |
| [`pm_edgefriction`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_edgefriction) | `cvar pm_edgefriction(float/string, "")` | Физика и игровой процесс |
| [`pm_flyfriction`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_flyfriction) | `cvar pm_flyfriction(float/string, "")` | Физика и игровой процесс |
| [`pm_ktjump`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_ktjump) | `cvar pm_ktjump(float/string, "")` | Физика и игровой процесс |
| [`pm_pground`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_pground) | `cvar pm_pground(boolean/int/string, "")` | Физика и игровой процесс |
| [`pm_slidefix`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_slidefix) | `cvar pm_slidefix(boolean/int/string, "")` | Физика и игровой процесс |
| [`pm_slidyslopes`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_slidyslopes) | `cvar pm_slidyslopes(boolean/int/string, "")` | Физика и игровой процесс |
| [`pm_stepdown`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_stepdown) | `cvar pm_stepdown(boolean/int/string, "")` | Физика и игровой процесс |
| [`pm_walljump`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_walljump) | `cvar pm_walljump(int/string, "")` | Физика и игровой процесс |
| [`pm_watersinkspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_watersinkspeed) | `cvar pm_watersinkspeed(float/string, "")` | Физика и игровой процесс |
| [`pushlatency`](../38-cvars-reference/05-physics-gameplay-cvars.md#pushlatency) | `cvar pushlatency(float, "-999")` | Физика и игровой процесс |
| [`sv_accelerate`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_accelerate) | `cvar sv_accelerate(float, "10")` | Физика и игровой процесс |
| [`sv_airaccelerate`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_airaccelerate) | `cvar sv_airaccelerate(float, "0.7")` | Физика и игровой процесс |
| [`sv_antilag`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_antilag) | `cvar sv_antilag(int/string, "")` | Физика и игровой процесс |
| [`sv_antilag_frac`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_antilag_frac) | `cvar sv_antilag_frac(float/string, "")` | Физика и игровой процесс |
| [`sv_brokenmovetypes`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_brokenmovetypes) | `cvar sv_brokenmovetypes(boolean/int, "0")` | Физика и игровой процесс |
| [`sv_friction`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_friction) | `cvar sv_friction(float, "4")` | Физика и игровой процесс |
| [`sv_gameplayfix_blowupfallenzombies`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_blowupfallenzombies) | `cvar sv_gameplayfix_blowupfallenzombies(boolean/int, "0")` | Физика и игровой процесс |
| [`sv_gameplayfix_droptofloorstartsolid`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_droptofloorstartsolid) | `cvar sv_gameplayfix_droptofloorstartsolid(boolean/int, "0")` | Физика и игровой процесс |
| [`sv_gameplayfix_findradiusdistancetobox`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_findradiusdistancetobox) | `cvar sv_gameplayfix_findradiusdistancetobox(boolean/int, "0")` | Физика и игровой процесс |
| [`sv_gameplayfix_grenadebouncedownslopes`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_grenadebouncedownslopes) | `cvar sv_gameplayfix_grenadebouncedownslopes(boolean/int, "0")` | Физика и игровой процесс |
| [`sv_gameplayfix_multiplethinks`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_multiplethinks) | `cvar sv_gameplayfix_multiplethinks(boolean/int, "1")` | Физика и игровой процесс |
| [`sv_gameplayfix_noairborncorpse`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_noairborncorpse) | `cvar sv_gameplayfix_noairborncorpse(boolean/int, "0")` | Физика и игровой процесс |
| [`sv_gameplayfix_nolinknonsolid`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_nolinknonsolid) | `cvar sv_gameplayfix_nolinknonsolid(boolean/int, "1")` | Физика и игровой процесс |
| [`sv_gameplayfix_trappedwithin`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_trappedwithin) | `cvar sv_gameplayfix_trappedwithin(boolean/int, "0")` | Физика и игровой процесс |
| [`sv_gravity`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gravity) | `cvar sv_gravity(float, "800")` | Физика и игровой процесс |
| [`sv_maxspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_maxspeed) | `cvar sv_maxspeed(float, "320")` | Физика и игровой процесс |
| [`sv_maxvelocity`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_maxvelocity) | `cvar sv_maxvelocity(float, "10000")` | Физика и игровой процесс |
| [`sv_nqplayerphysics`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_nqplayerphysics) | `cvar sv_nqplayerphysics(string/int, "auto")` | Физика и игровой процесс |
| [`sv_pushplayers`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_pushplayers) | `cvar sv_pushplayers(float, "0")` | Физика и игровой процесс |
| [`sv_spectatormaxspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_spectatormaxspeed) | `cvar sv_spectatormaxspeed(float, "500")` | Физика и игровой процесс |
| [`sv_stepheight`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_stepheight) | `cvar sv_stepheight(float/string, "")` | Физика и игровой процесс |
| [`sv_stopspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_stopspeed) | `cvar sv_stopspeed(float, "100")` | Физика и игровой процесс |
| [`sv_wallfriction`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_wallfriction) | `cvar sv_wallfriction(float, "1")` | Физика и игровой процесс |
| [`sv_wateraccelerate`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_wateraccelerate) | `cvar sv_wateraccelerate(float, "10")` | Физика и игровой процесс |
| [`sv_waterfriction`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_waterfriction) | `cvar sv_waterfriction(float, "4")` | Физика и игровой процесс |
| [`chase_active`](../38-cvars-reference/05-physics-gameplay-cvars.md#chase_active) | `cvar chase_active(int, "0")` | Физика и игровой процесс |
| [`chase_back`](../38-cvars-reference/05-physics-gameplay-cvars.md#chase_back) | `cvar chase_back(int, "48")` | Физика и игровой процесс |
| [`chase_right`](../38-cvars-reference/05-physics-gameplay-cvars.md#chase_right) | `cvar chase_right(int, "0")` | Физика и игровой процесс |
| [`chase_up`](../38-cvars-reference/05-physics-gameplay-cvars.md#chase_up) | `cvar chase_up(int, "24")` | Физика и игровой процесс |
| [`cl_bob`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_bob) | `cvar cl_bob(float, "0.02")` | Физика и игровой процесс |
| [`cl_bobcycle`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_bobcycle) | `cvar cl_bobcycle(float, "0.6")` | Физика и игровой процесс |
| [`cl_bobmodel`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_bobmodel) | `cvar cl_bobmodel(int, "0")` | Физика и игровой процесс |
| [`cl_bobmodel_side`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_bobmodel_side) | `cvar cl_bobmodel_side(float, "0.15")` | Физика и игровой процесс |
| [`cl_bobmodel_speed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_bobmodel_speed) | `cvar cl_bobmodel_speed(int, "7")` | Физика и игровой процесс |
| [`cl_bobmodel_up`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_bobmodel_up) | `cvar cl_bobmodel_up(float, "0.06")` | Физика и игровой процесс |
| [`cl_bobup`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_bobup) | `cvar cl_bobup(float, "0.5")` | Физика и игровой процесс |
| [`cl_predict_players`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_predict_players) | `cvar cl_predict_players(int, "1")` | Физика и игровой процесс |
| [`pm_noround`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_noround) | `cvar pm_noround(int, "0")` | Физика и игровой процесс |
| [`pm_stepheight`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_stepheight) | `cvar pm_stepheight(string, "")` | Физика и игровой процесс |
| [`temp1`](../38-cvars-reference/05-physics-gameplay-cvars.md#temp1) | `cvar temp1(int, "0")` | Физика и игровой процесс |
| [`cl_anglespeedkey`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_anglespeedkey) | `cvar cl_anglespeedkey(дробное, "1.5")` | Интерфейс, консоль и управление |
| [`cl_backspeed`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_backspeed) | `cvar cl_backspeed(дробное/пустая строка, "")` | Интерфейс, консоль и управление |
| [`cl_chatmode`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_chatmode) | `cvar cl_chatmode(целое 0-2, "2")` | Интерфейс, консоль и управление |
| [`cl_clock`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_clock) | `cvar cl_clock(целое 0-2, "0")` | Интерфейс, консоль и управление |
| [`cl_fastaccel`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_fastaccel) | `cvar cl_fastaccel(логическое, "1")` | Интерфейс, консоль и управление |
| [`cl_forwardspeed`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_forwardspeed) | `cvar cl_forwardspeed(дробное, "400")` | Интерфейс, консоль и управление |
| [`cl_gameclock`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_gameclock) | `cvar cl_gameclock(целое 0-2, "0")` | Интерфейс, консоль и управление |
| [`cl_iDrive`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_idrive) | `cvar cl_iDrive(логическое, "1")` | Интерфейс, консоль и управление |
| [`cl_instantrotate`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_instantrotate) | `cvar cl_instantrotate(логическое, "1")` | Интерфейс, консоль и управление |
| [`cl_keypad`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_keypad) | `cvar cl_keypad(логическое, "1")` | Интерфейс, консоль и управление |
| [`cl_movespeedkey`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_movespeedkey) | `cvar cl_movespeedkey(дробное, "2.0")` | Интерфейс, консоль и управление |
| [`cl_pitchspeed`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_pitchspeed) | `cvar cl_pitchspeed(дробное, "150")` | Интерфейс, консоль и управление |
| [`cl_run`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_run) | `cvar cl_run(логическое, "0")` | Интерфейс, консоль и управление |
| [`cl_sendchatstate`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_sendchatstate) | `cvar cl_sendchatstate(логическое, "1")` | Интерфейс, консоль и управление |
| [`cl_sidespeed`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_sidespeed) | `cvar cl_sidespeed(дробное, "400")` | Интерфейс, консоль и управление |
| [`cl_smartjump`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_smartjump) | `cvar cl_smartjump(логическое, "1")` | Интерфейс, консоль и управление |
| [`cl_upspeed`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_upspeed) | `cvar cl_upspeed(дробное, "400")` | Интерфейс, консоль и управление |
| [`cl_yawspeed`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_yawspeed) | `cvar cl_yawspeed(дробное, "140")` | Интерфейс, консоль и управление |
| [`con_centernotify`](../38-cvars-reference/06-ui-console-input-cvars.md#con_centernotify) | `cvar con_centernotify(логическое, "0")` | Интерфейс, консоль и управление |
| [`con_displaypossibilities`](../38-cvars-reference/06-ui-console-input-cvars.md#con_displaypossibilities) | `cvar con_displaypossibilities(логическое, "1")` | Интерфейс, консоль и управление |
| [`con_echochat`](../38-cvars-reference/06-ui-console-input-cvars.md#con_echochat) | `cvar con_echochat(логическое, "0")` | Интерфейс, консоль и управление |
| [`con_maxlines`](../38-cvars-reference/06-ui-console-input-cvars.md#con_maxlines) | `cvar con_maxlines(целое, "1024")` | Интерфейс, консоль и управление |
| [`con_notify_w`](../38-cvars-reference/06-ui-console-input-cvars.md#con_notify_w) | `cvar con_notify_w(дробное, "1")` | Интерфейс, консоль и управление |
| [`con_notify_x`](../38-cvars-reference/06-ui-console-input-cvars.md#con_notify_x) | `cvar con_notify_x(дробное, "0")` | Интерфейс, консоль и управление |
| [`con_notify_y`](../38-cvars-reference/06-ui-console-input-cvars.md#con_notify_y) | `cvar con_notify_y(дробное, "0")` | Интерфейс, консоль и управление |
| [`con_notifylines`](../38-cvars-reference/06-ui-console-input-cvars.md#con_notifylines) | `cvar con_notifylines(целое, "4")` | Интерфейс, консоль и управление |
| [`con_notifytime`](../38-cvars-reference/06-ui-console-input-cvars.md#con_notifytime) | `cvar con_notifytime(дробное, "3")` | Интерфейс, консоль и управление |
| [`con_notifytime_chat`](../38-cvars-reference/06-ui-console-input-cvars.md#con_notifytime_chat) | `cvar con_notifytime_chat(дробное, "8")` | Интерфейс, консоль и управление |
| [`con_numnotifylines_chat`](../38-cvars-reference/06-ui-console-input-cvars.md#con_numnotifylines_chat) | `cvar con_numnotifylines_chat(целое, "8")` | Интерфейс, консоль и управление |
| [`con_savehistory`](../38-cvars-reference/06-ui-console-input-cvars.md#con_savehistory) | `cvar con_savehistory(логическое, "1")` | Интерфейс, консоль и управление |
| [`con_separatechat`](../38-cvars-reference/06-ui-console-input-cvars.md#con_separatechat) | `cvar con_separatechat(логическое, "0")` | Интерфейс, консоль и управление |
| [`con_showcompletion`](../38-cvars-reference/06-ui-console-input-cvars.md#con_showcompletion) | `cvar con_showcompletion(логическое, "1")` | Интерфейс, консоль и управление |
| [`con_stayhidden`](../38-cvars-reference/06-ui-console-input-cvars.md#con_stayhidden) | `cvar con_stayhidden(целое 0-3, "1")` | Интерфейс, консоль и управление |
| [`con_textsize`](../38-cvars-reference/06-ui-console-input-cvars.md#con_textsize) | `cvar con_textsize(целое, "8")` | Интерфейс, консоль и управление |
| [`con_timeformat`](../38-cvars-reference/06-ui-console-input-cvars.md#con_timeformat) | `cvar con_timeformat(строка формата времени, "(%H:%M:%S) ")` | Интерфейс, консоль и управление |
| [`con_timestamps`](../38-cvars-reference/06-ui-console-input-cvars.md#con_timestamps) | `cvar con_timestamps(логическое, "0")` | Интерфейс, консоль и управление |
| [`in_builtinkeymap`](../38-cvars-reference/06-ui-console-input-cvars.md#in_builtinkeymap) | `cvar in_builtinkeymap(логическое, "0")` | Интерфейс, консоль и управление |
| [`in_dinput`](../38-cvars-reference/06-ui-console-input-cvars.md#in_dinput) | `cvar in_dinput(логическое, "0")` | Интерфейс, консоль и управление |
| [`in_nonstandarddeadkeys`](../38-cvars-reference/06-ui-console-input-cvars.md#in_nonstandarddeadkeys) | `cvar in_nonstandarddeadkeys(логическое, "1")` | Интерфейс, консоль и управление |
| [`in_rawinput`](../38-cvars-reference/06-ui-console-input-cvars.md#in_rawinput) | `cvar in_rawinput(логическое, "0")` | Интерфейс, консоль и управление |
| [`in_rawinput_keyboard`](../38-cvars-reference/06-ui-console-input-cvars.md#in_rawinput_keyboard) | `cvar in_rawinput_keyboard(логическое, "0")` | Интерфейс, консоль и управление |
| [`in_simulatemultitouch`](../38-cvars-reference/06-ui-console-input-cvars.md#in_simulatemultitouch) | `cvar in_simulatemultitouch(логическое, "0")` | Интерфейс, консоль и управление |
| [`in_xflip`](../38-cvars-reference/06-ui-console-input-cvars.md#in_xflip) | `cvar in_xflip(логическое, "0")` | Интерфейс, консоль и управление |
| [`in_xinput`](../38-cvars-reference/06-ui-console-input-cvars.md#in_xinput) | `cvar in_xinput(целое 0-3, "1")` | Интерфейс, консоль и управление |
| [`joyexponent`](../38-cvars-reference/06-ui-console-input-cvars.md#joyexponent) | `cvar joyexponent(дробное, "1")` | Интерфейс, консоль и управление |
| [`joyonly`](../38-cvars-reference/06-ui-console-input-cvars.md#joyonly) | `cvar joyonly(логическое, "0")` | Интерфейс, консоль и управление |
| [`joypitchsensitivity`](../38-cvars-reference/06-ui-console-input-cvars.md#joypitchsensitivity) | `cvar joypitchsensitivity(дробное, "0.5")` | Интерфейс, консоль и управление |
| [`joypitchthreshold`](../38-cvars-reference/06-ui-console-input-cvars.md#joypitchthreshold) | `cvar joypitchthreshold(дробное, "0.19")` | Интерфейс, консоль и управление |
| [`joyrollsensitivity`](../38-cvars-reference/06-ui-console-input-cvars.md#joyrollsensitivity) | `cvar joyrollsensitivity(дробное, "1.0")` | Интерфейс, консоль и управление |
| [`joyrollthreshold`](../38-cvars-reference/06-ui-console-input-cvars.md#joyrollthreshold) | `cvar joyrollthreshold(дробное, "0.118")` | Интерфейс, консоль и управление |
| [`joyyawsensitivity`](../38-cvars-reference/06-ui-console-input-cvars.md#joyyawsensitivity) | `cvar joyyawsensitivity(дробное, "1.0")` | Интерфейс, консоль и управление |
| [`joyyawthreshold`](../38-cvars-reference/06-ui-console-input-cvars.md#joyyawthreshold) | `cvar joyyawthreshold(дробное, "0.19")` | Интерфейс, консоль и управление |
| [`m_filter`](../38-cvars-reference/06-ui-console-input-cvars.md#m_filter) | `cvar m_filter(дробное 0-2, "0")` | Интерфейс, консоль и управление |
| [`m_forcewheel`](../38-cvars-reference/06-ui-console-input-cvars.md#m_forcewheel) | `cvar m_forcewheel(целое 0-2, "1")` | Интерфейс, консоль и управление |
| [`m_forcewheel_threshold`](../38-cvars-reference/06-ui-console-input-cvars.md#m_forcewheel_threshold) | `cvar m_forcewheel_threshold(целое, "32")` | Интерфейс, консоль и управление |
| [`m_helpismedia`](../38-cvars-reference/06-ui-console-input-cvars.md#m_helpismedia) | `cvar m_helpismedia(логическое, "0")` | Интерфейс, консоль и управление |
| [`m_longpressthreshold`](../38-cvars-reference/06-ui-console-input-cvars.md#m_longpressthreshold) | `cvar m_longpressthreshold(дробное, "1")` | Интерфейс, консоль и управление |
| [`m_slidethreshold`](../38-cvars-reference/06-ui-console-input-cvars.md#m_slidethreshold) | `cvar m_slidethreshold(дробное, "10")` | Интерфейс, консоль и управление |
| [`m_touchmajoraxis`](../38-cvars-reference/06-ui-console-input-cvars.md#m_touchmajoraxis) | `cvar m_touchmajoraxis(логическое, "1")` | Интерфейс, консоль и управление |
| [`sbar_teamstatus`](../38-cvars-reference/06-ui-console-input-cvars.md#sbar_teamstatus) | `cvar sbar_teamstatus(целое 0-2, "1")` | Интерфейс, консоль и управление |
| [`scr_loadingrefresh`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_loadingrefresh) | `cvar scr_loadingrefresh(логическое, "0")` | Интерфейс, консоль и управление |
| [`scr_loadingscreen_aspect`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_loadingscreen_aspect) | `cvar scr_loadingscreen_aspect(целое -1/0/1/2, "0")` | Интерфейс, консоль и управление |
| [`scr_loadingscreen_picture`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_loadingscreen_picture) | `cvar scr_loadingscreen_picture(строка/путь, "gfx/loading")` | Интерфейс, консоль и управление |
| [`scr_loadingscreen_scale`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_loadingscreen_scale) | `cvar scr_loadingscreen_scale(дробное, "1")` | Интерфейс, консоль и управление |
| [`scr_loadingscreen_scale_limit`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_loadingscreen_scale_limit) | `cvar scr_loadingscreen_scale_limit(целое, "2")` | Интерфейс, консоль и управление |
| [`scr_scoreboard_afk`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_afk) | `cvar scr_scoreboard_afk(логическое, "1")` | Интерфейс, консоль и управление |
| [`scr_scoreboard_backgroundalpha`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_backgroundalpha) | `cvar scr_scoreboard_backgroundalpha(дробное, "0.5")` | Интерфейс, консоль и управление |
| [`scr_scoreboard_drawtitle`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_drawtitle) | `cvar scr_scoreboard_drawtitle(логическое, "1")` | Интерфейс, консоль и управление |
| [`scr_scoreboard_fillalpha`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_fillalpha) | `cvar scr_scoreboard_fillalpha(дробное, "0.7")` | Интерфейс, консоль и управление |
| [`scr_scoreboard_forcecolors`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_forcecolors) | `cvar scr_scoreboard_forcecolors(логическое, "0")` | Интерфейс, консоль и управление |
| [`scr_scoreboard_newstyle`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_newstyle) | `cvar scr_scoreboard_newstyle(логическое, "1")` | Интерфейс, консоль и управление |
| [`scr_scoreboard_ping_status`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_ping_status) | `cvar scr_scoreboard_ping_status(список из 4 чисел, "25 50 100 150")` | Интерфейс, консоль и управление |
| [`scr_scoreboard_showflags`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_showflags) | `cvar scr_scoreboard_showflags(целое 0-2, "2")` | Интерфейс, консоль и управление |
| [`scr_scoreboard_showfrags`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_showfrags) | `cvar scr_scoreboard_showfrags(логическое, "0")` | Интерфейс, консоль и управление |
| [`scr_scoreboard_showhealth`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_showhealth) | `cvar scr_scoreboard_showhealth(целое 0-3, "3")` | Интерфейс, консоль и управление |
| [`scr_scoreboard_showlocation`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_showlocation) | `cvar scr_scoreboard_showlocation(логическое, "1")` | Интерфейс, консоль и управление |
| [`scr_scoreboard_showruleset`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_showruleset) | `cvar scr_scoreboard_showruleset(целое 0-2, "1")` | Интерфейс, консоль и управление |
| [`scr_scoreboard_showweapon`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_showweapon) | `cvar scr_scoreboard_showweapon(логическое, "1")` | Интерфейс, консоль и управление |
| [`scr_scoreboard_teamscores`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_teamscores) | `cvar scr_scoreboard_teamscores(логическое, "1")` | Интерфейс, консоль и управление |
| [`scr_scoreboard_teamsort`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_teamsort) | `cvar scr_scoreboard_teamsort(логическое, "0")` | Интерфейс, консоль и управление |
| [`scr_scoreboard_titleseperator`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_titleseperator) | `cvar scr_scoreboard_titleseperator(логическое, "1")` | Интерфейс, консоль и управление |
| [`scr_showdisk`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_showdisk) | `cvar scr_showdisk(логическое, "0")` | Интерфейс, консоль и управление |
| [`scr_showloading`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_showloading) | `cvar scr_showloading(логическое, "1")` | Интерфейс, консоль и управление |
| [`scr_showobituaries`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_showobituaries) | `cvar scr_showobituaries(логическое, "0")` | Интерфейс, консоль и управление |
| [`show_speed`](../38-cvars-reference/06-ui-console-input-cvars.md#show_speed) | `cvar show_speed(логическое, "0")` | Интерфейс, консоль и управление |
| [`sys_osk`](../38-cvars-reference/06-ui-console-input-cvars.md#sys_osk) | `cvar sys_osk(логическое, "0")` | Интерфейс, консоль и управление |
| [`cl_clock_x`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_clock_x) | `cvar cl_clock_x(int, "0")` | Интерфейс, консоль и управление |
| [`cl_clock_y`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_clock_y) | `cvar cl_clock_y(int, "-1")` | Интерфейс, консоль и управление |
| [`cl_cursor`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_cursor) | `cvar cl_cursor(string, "")` | Интерфейс, консоль и управление |
| [`cl_cursor_scale`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_cursor_scale) | `cvar cl_cursor_scale(float, "1.0")` | Интерфейс, консоль и управление |
| [`cl_gameclock_x`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_gameclock_x) | `cvar cl_gameclock_x(int, "0")` | Интерфейс, консоль и управление |
| [`cl_gameclock_y`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_gameclock_y) | `cvar cl_gameclock_y(int, "-1")` | Интерфейс, консоль и управление |
| [`cl_prydoncursor`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_prydoncursor) | `cvar cl_prydoncursor(string, "")` | Интерфейс, консоль и управление |
| [`cl_standardchat`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_standardchat) | `cvar cl_standardchat(int, "0")` | Интерфейс, консоль и управление |
| [`cl_vrui_force`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_vrui_force) | `cvar cl_vrui_force(int, "0")` | Интерфейс, консоль и управление |
| [`cl_vrui_lock`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_vrui_lock) | `cvar cl_vrui_lock(int, "1")` | Интерфейс, консоль и управление |
| [`con_logcenterprint`](../38-cvars-reference/06-ui-console-input-cvars.md#con_logcenterprint) | `cvar con_logcenterprint(int, "1")` | Интерфейс, консоль и управление |
| [`con_ocranaleds`](../38-cvars-reference/06-ui-console-input-cvars.md#con_ocranaleds) | `cvar con_ocranaleds(int, "2")` | Интерфейс, консоль и управление |
| [`con_textfont`](../38-cvars-reference/06-ui-console-input-cvars.md#con_textfont) | `cvar con_textfont(string, "")` | Интерфейс, консоль и управление |
| [`con_window`](../38-cvars-reference/06-ui-console-input-cvars.md#con_window) | `cvar con_window(int, "0")` | Интерфейс, консоль и управление |
| [`contrast`](../38-cvars-reference/06-ui-console-input-cvars.md#contrast) | `cvar contrast(float, "1.0")` | Интерфейс, консоль и управление |
| [`crosshairalpha`](../38-cvars-reference/06-ui-console-input-cvars.md#crosshairalpha) | `cvar crosshairalpha(int, "1")` | Интерфейс, консоль и управление |
| [`crosshaircolor`](../38-cvars-reference/06-ui-console-input-cvars.md#crosshaircolor) | `cvar crosshaircolor(string, "255 255 255")` | Интерфейс, консоль и управление |
| [`dpcompat_console`](../38-cvars-reference/06-ui-console-input-cvars.md#dpcompat_console) | `cvar dpcompat_console(int, "0")` | Интерфейс, консоль и управление |
| [`gamma`](../38-cvars-reference/06-ui-console-input-cvars.md#gamma) | `cvar gamma(float, "1.0")` | Интерфейс, консоль и управление |
| [`in_forceseat`](../38-cvars-reference/06-ui-console-input-cvars.md#in_forceseat) | `cvar in_forceseat(int, "0")` | Интерфейс, консоль и управление |
| [`in_rawinput_rdp`](../38-cvars-reference/06-ui-console-input-cvars.md#in_rawinput_rdp) | `cvar in_rawinput_rdp(int, "0")` | Интерфейс, консоль и управление |
| [`in_skipplayerone`](../38-cvars-reference/06-ui-console-input-cvars.md#in_skipplayerone) | `cvar in_skipplayerone(int, "1")` | Интерфейс, консоль и управление |
| [`in_vraim`](../38-cvars-reference/06-ui-console-input-cvars.md#in_vraim) | `cvar in_vraim(int, "1")` | Интерфейс, консоль и управление |
| [`in_windowed_mouse`](../38-cvars-reference/06-ui-console-input-cvars.md#in_windowed_mouse) | `cvar in_windowed_mouse(int, "1")` | Интерфейс, консоль и управление |
| [`joystick`](../38-cvars-reference/06-ui-console-input-cvars.md#joystick) | `cvar joystick(int, "0")` | Интерфейс, консоль и управление |
| [`m_forward`](../38-cvars-reference/06-ui-console-input-cvars.md#m_forward) | `cvar m_forward(int, "1")` | Интерфейс, консоль и управление |
| [`m_pitch`](../38-cvars-reference/06-ui-console-input-cvars.md#m_pitch) | `cvar m_pitch(float, "0.022")` | Интерфейс, консоль и управление |
| [`m_side`](../38-cvars-reference/06-ui-console-input-cvars.md#m_side) | `cvar m_side(float, "0.8")` | Интерфейс, консоль и управление |
| [`m_yaw`](../38-cvars-reference/06-ui-console-input-cvars.md#m_yaw) | `cvar m_yaw(float, "0.022")` | Интерфейс, консоль и управление |
| [`pr_menu_coreonerror`](../38-cvars-reference/06-ui-console-input-cvars.md#pr_menu_coreonerror) | `cvar pr_menu_coreonerror(int, "1")` | Интерфейс, консоль и управление |
| [`pr_menu_memsize`](../38-cvars-reference/06-ui-console-input-cvars.md#pr_menu_memsize) | `cvar pr_menu_memsize(string, "64m")` | Интерфейс, консоль и управление |
| [`r_globalskin_count`](../38-cvars-reference/06-ui-console-input-cvars.md#r_globalskin_count) | `cvar r_globalskin_count(int, "10")` | Интерфейс, консоль и управление |
| [`r_globalskin_first`](../38-cvars-reference/06-ui-console-input-cvars.md#r_globalskin_first) | `cvar r_globalskin_first(int, "100")` | Интерфейс, консоль и управление |
| [`r_part_rain_quantity`](../38-cvars-reference/06-ui-console-input-cvars.md#r_part_rain_quantity) | `cvar r_part_rain_quantity(int, "1")` | Интерфейс, консоль и управление |
| [`r_skin_overlays`](../38-cvars-reference/06-ui-console-input-cvars.md#r_skin_overlays) | `cvar r_skin_overlays(int, "1")` | Интерфейс, консоль и управление |
| [`rcon_address`](../38-cvars-reference/06-ui-console-input-cvars.md#rcon_address) | `cvar rcon_address(string, "")` | Интерфейс, консоль и управление |
| [`rcon_level`](../38-cvars-reference/06-ui-console-input-cvars.md#rcon_level) | `cvar rcon_level(int, "20")` | Интерфейс, консоль и управление |
| [`scr_allowsnap`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_allowsnap) | `cvar scr_allowsnap(int, "0")` | Интерфейс, консоль и управление |
| [`scr_autoid`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_autoid) | `cvar scr_autoid(int, "1")` | Интерфейс, консоль и управление |
| [`scr_autoid_armor`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_autoid_armor) | `cvar scr_autoid_armor(int, "1")` | Интерфейс, консоль и управление |
| [`scr_autoid_enemycolour`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_autoid_enemycolour) | `cvar scr_autoid_enemycolour(string, "The colour for the text on the nametags of non-team members.")` | Интерфейс, консоль и управление |
| [`scr_autoid_health`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_autoid_health) | `cvar scr_autoid_health(int, "1")` | Интерфейс, консоль и управление |
| [`scr_autoid_team`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_autoid_team) | `cvar scr_autoid_team(int, "0")` | Интерфейс, консоль и управление |
| [`scr_autoid_teamcolour`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_autoid_teamcolour) | `cvar scr_autoid_teamcolour(string, "The colour for the text on the nametags of team members.")` | Интерфейс, консоль и управление |
| [`scr_autoid_weapon`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_autoid_weapon) | `cvar scr_autoid_weapon(int, "1")` | Интерфейс, консоль и управление |
| [`scr_autoid_weapon_mask`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_autoid_weapon_mask) | `cvar scr_autoid_weapon_mask(int, "126")` | Интерфейс, консоль и управление |
| [`scr_centersbar`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_centersbar) | `cvar scr_centersbar(int, "2")` | Интерфейс, консоль и управление |
| [`scr_centertime`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_centertime) | `cvar scr_centertime(int, "2")` | Интерфейс, консоль и управление |
| [`scr_conalpha`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_conalpha) | `cvar scr_conalpha(float, "0.7")` | Интерфейс, консоль и управление |
| [`scr_consize`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_consize) | `cvar scr_consize(float, "0.5")` | Интерфейс, консоль и управление |
| [`scr_conspeed`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_conspeed) | `cvar scr_conspeed(int, "2000")` | Интерфейс, консоль и управление |
| [`scr_diskicontimeout`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_diskicontimeout) | `cvar scr_diskicontimeout(float, "0.3")` | Интерфейс, консоль и управление |
| [`scr_neticontimeout`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_neticontimeout) | `cvar scr_neticontimeout(float, "0.3")` | Интерфейс, консоль и управление |
| [`scr_printspeed`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_printspeed) | `cvar scr_printspeed(int, "16")` | Интерфейс, консоль и управление |
| [`scr_showdisk_x`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_showdisk_x) | `cvar scr_showdisk_x(int, "-24")` | Интерфейс, консоль и управление |
| [`scr_showdisk_y`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_showdisk_y) | `cvar scr_showdisk_y(int, "0")` | Интерфейс, консоль и управление |
| [`scr_sshot_compression`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_sshot_compression) | `cvar scr_sshot_compression(int, "75")` | Интерфейс, консоль и управление |
| [`scr_sshot_prefix`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_sshot_prefix) | `cvar scr_sshot_prefix(string, "screenshots/fte-")` | Интерфейс, консоль и управление |
| [`scr_sshot_type`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_sshot_type) | `cvar scr_sshot_type(string, "png")` | Интерфейс, консоль и управление |
| [`scr_turtlefps`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_turtlefps) | `cvar scr_turtlefps(int, "10")` | Интерфейс, консоль и управление |
| [`scr_usekfont`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_usekfont) | `cvar scr_usekfont(int, "0")` | Интерфейс, консоль и управление |
| [`v_contrastboost`](../38-cvars-reference/06-ui-console-input-cvars.md#v_contrastboost) | `cvar v_contrastboost(float, "1.0")` | Интерфейс, консоль и управление |
| [`v_gammainverted`](../38-cvars-reference/06-ui-console-input-cvars.md#v_gammainverted) | `cvar v_gammainverted(int, "0")` | Интерфейс, консоль и управление |
| [`vid_desktopgamma`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_desktopgamma) | `cvar vid_desktopgamma(int, "0")` | Интерфейс, консоль и управление |
| [`vid_gl_context_compatibility`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_gl_context_compatibility) | `cvar vid_gl_context_compatibility(int, "1")` | Интерфейс, консоль и управление |
| [`vid_gl_context_debug`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_gl_context_debug) | `cvar vid_gl_context_debug(int, "0")` | Интерфейс, консоль и управление |
| [`vid_gl_context_es`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_gl_context_es) | `cvar vid_gl_context_es(int, "0")` | Интерфейс, консоль и управление |
| [`vid_gl_context_forwardcompatible`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_gl_context_forwardcompatible) | `cvar vid_gl_context_forwardcompatible(int, "0")` | Интерфейс, консоль и управление |
| [`vid_gl_context_noerror`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_gl_context_noerror) | `cvar vid_gl_context_noerror(string, "")` | Интерфейс, консоль и управление |
| [`vid_gl_context_robustness`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_gl_context_robustness) | `cvar vid_gl_context_robustness(int, "1")` | Интерфейс, консоль и управление |
| [`vid_gl_context_selfreset`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_gl_context_selfreset) | `cvar vid_gl_context_selfreset(int, "1")` | Интерфейс, консоль и управление |
| [`vid_gl_context_version`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_gl_context_version) | `cvar vid_gl_context_version(string, "")` | Интерфейс, консоль и управление |
| [`vid_preservegamma`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_preservegamma) | `cvar vid_preservegamma(int, "0")` | Интерфейс, консоль и управление |
| [`_cl_disconnectreason`](../38-cvars-reference/07-system-misc-cvars.md#_cl_disconnectreason) | `cvar _cl_disconnectreason(string, "")` | Системные, отладочные и прочие cvar |
| [`_pext_infoblobs`](../38-cvars-reference/07-system-misc-cvars.md#_pext_infoblobs) | `cvar _pext_infoblobs(int, "0")` | Системные, отладочные и прочие cvar |
| [`_pext_lerptime`](../38-cvars-reference/07-system-misc-cvars.md#_pext_lerptime) | `cvar _pext_lerptime(int, "0")` | Системные, отладочные и прочие cvar |
| [`_pext_vrinputs`](../38-cvars-reference/07-system-misc-cvars.md#_pext_vrinputs) | `cvar _pext_vrinputs(int, "0")` | Системные, отладочные и прочие cvar |
| [`_q3bsp_bihtraces`](../38-cvars-reference/07-system-misc-cvars.md#_q3bsp_bihtraces) | `cvar _q3bsp_bihtraces(int, "0")` | Системные, отладочные и прочие cvar |
| [`allow_f_cmdline`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_cmdline) | `cvar allow_f_cmdline(int, "0")` | Системные, отладочные и прочие cvar |
| [`allow_f_fakeshaft`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_fakeshaft) | `cvar allow_f_fakeshaft(int, "1")` | Системные, отладочные и прочие cvar |
| [`allow_f_modified`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_modified) | `cvar allow_f_modified(int, "1")` | Системные, отладочные и прочие cvar |
| [`allow_f_ruleset`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_ruleset) | `cvar allow_f_ruleset(int, "1")` | Системные, отладочные и прочие cvar |
| [`allow_f_scripts`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_scripts) | `cvar allow_f_scripts(int, "1")` | Системные, отладочные и прочие cvar |
| [`allow_f_server`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_server) | `cvar allow_f_server(int, "1")` | Системные, отладочные и прочие cvar |
| [`allow_f_skins`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_skins) | `cvar allow_f_skins(int, "1")` | Системные, отладочные и прочие cvar |
| [`allow_f_system`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_system) | `cvar allow_f_system(int, "0")` | Системные, отладочные и прочие cvar |
| [`allow_f_version`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_version) | `cvar allow_f_version(int, "1")` | Системные, отладочные и прочие cvar |
| [`allow_skybox`](../38-cvars-reference/07-system-misc-cvars.md#allow_skybox) | `cvar allow_skybox(string, "")` | Системные, отладочные и прочие cvar |
| [`allow_splitscreen`](../38-cvars-reference/07-system-misc-cvars.md#allow_splitscreen) | `cvar allow_splitscreen(string, "")` | Системные, отладочные и прочие cvar |
| [`auth_validateclients`](../38-cvars-reference/07-system-misc-cvars.md#auth_validateclients) | `cvar auth_validateclients(int, "1")` | Системные, отладочные и прочие cvar |
| [`b_switch`](../38-cvars-reference/07-system-misc-cvars.md#b_switch) | `cvar b_switch(string, "")` | Системные, отладочные и прочие cvar |
| [`baseskin`](../38-cvars-reference/07-system-misc-cvars.md#baseskin) | `cvar baseskin(string, "")` | Системные, отладочные и прочие cvar |
| [`bottomcolor`](../38-cvars-reference/07-system-misc-cvars.md#bottomcolor) | `cvar bottomcolor(int, "12")` | Системные, отладочные и прочие cvar |
| [`capturecodec`](../38-cvars-reference/07-system-misc-cvars.md#capturecodec) | `cvar capturecodec(string, "the compression/encoding codec to use.\\n")` | Системные, отладочные и прочие cvar |
| [`capturedemoheight`](../38-cvars-reference/07-system-misc-cvars.md#capturedemoheight) | `cvar capturedemoheight(int, "0")` | Системные, отладочные и прочие cvar |
| [`capturedemowidth`](../38-cvars-reference/07-system-misc-cvars.md#capturedemowidth) | `cvar capturedemowidth(int, "0")` | Системные, отладочные и прочие cvar |
| [`capturedriver`](../38-cvars-reference/07-system-misc-cvars.md#capturedriver) | `cvar capturedriver(string, "")` | Системные, отладочные и прочие cvar |
| [`capturemessage`](../38-cvars-reference/07-system-misc-cvars.md#capturemessage) | `cvar capturemessage(string, "")` | Системные, отладочные и прочие cvar |
| [`capturethrottlesize`](../38-cvars-reference/07-system-misc-cvars.md#capturethrottlesize) | `cvar capturethrottlesize(int, "0")` | Системные, отладочные и прочие cvar |
| [`cfg_reload_on_gamedir`](../38-cvars-reference/07-system-misc-cvars.md#cfg_reload_on_gamedir) | `cvar cfg_reload_on_gamedir(int, "1")` | Системные, отладочные и прочие cvar |
| [`cfg_save_aliases`](../38-cvars-reference/07-system-misc-cvars.md#cfg_save_aliases) | `cvar cfg_save_aliases(int, "1")` | Системные, отладочные и прочие cvar |
| [`cfg_save_all`](../38-cvars-reference/07-system-misc-cvars.md#cfg_save_all) | `cvar cfg_save_all(string, "")` | Системные, отладочные и прочие cvar |
| [`cfg_save_auto`](../38-cvars-reference/07-system-misc-cvars.md#cfg_save_auto) | `cvar cfg_save_auto(int, "0")` | Системные, отладочные и прочие cvar |
| [`cfg_save_binds`](../38-cvars-reference/07-system-misc-cvars.md#cfg_save_binds) | `cvar cfg_save_binds(int, "1")` | Системные, отладочные и прочие cvar |
| [`cfg_save_buttons`](../38-cvars-reference/07-system-misc-cvars.md#cfg_save_buttons) | `cvar cfg_save_buttons(int, "0")` | Системные, отладочные и прочие cvar |
| [`cfg_save_infos`](../38-cvars-reference/07-system-misc-cvars.md#cfg_save_infos) | `cvar cfg_save_infos(int, "1")` | Системные, отладочные и прочие cvar |
| [`cfg_save_name`](../38-cvars-reference/07-system-misc-cvars.md#cfg_save_name) | `cvar cfg_save_name(string, "fte")` | Системные, отладочные и прочие cvar |
| [`cl_aliasoverlap`](../38-cvars-reference/07-system-misc-cvars.md#cl_aliasoverlap) | `cvar cl_aliasoverlap(int, "1")` | Системные, отладочные и прочие cvar |
| [`cl_autotrack`](../38-cvars-reference/07-system-misc-cvars.md#cl_autotrack) | `cvar cl_autotrack(string, "auto")` | Системные, отладочные и прочие cvar |
| [`cl_autotrack_team`](../38-cvars-reference/07-system-misc-cvars.md#cl_autotrack_team) | `cvar cl_autotrack_team(string, "")` | Системные, отладочные и прочие cvar |
| [`cl_beam_alpha`](../38-cvars-reference/07-system-misc-cvars.md#cl_beam_alpha) | `cvar cl_beam_alpha(int, "1")` | Системные, отладочные и прочие cvar |
| [`cl_beam_trace`](../38-cvars-reference/07-system-misc-cvars.md#cl_beam_trace) | `cvar cl_beam_trace(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_c2sdupe`](../38-cvars-reference/07-system-misc-cvars.md#cl_c2sdupe) | `cvar cl_c2sdupe(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_c2sImpulseBackup`](../38-cvars-reference/07-system-misc-cvars.md#cl_c2simpulsebackup) | `cvar cl_c2sImpulseBackup(int, "3")` | Системные, отладочные и прочие cvar |
| [`cl_c2sMaxRedundancy`](../38-cvars-reference/07-system-misc-cvars.md#cl_c2smaxredundancy) | `cvar cl_c2sMaxRedundancy(int, "5")` | Системные, отладочные и прочие cvar |
| [`cl_c2spps`](../38-cvars-reference/07-system-misc-cvars.md#cl_c2spps) | `cvar cl_c2spps(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_chasecam`](../38-cvars-reference/07-system-misc-cvars.md#cl_chasecam) | `cvar cl_chasecam(int, "1")` | Системные, отладочные и прочие cvar |
| [`cl_countpendingpl`](../38-cvars-reference/07-system-misc-cvars.md#cl_countpendingpl) | `cvar cl_countpendingpl(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_crossx`](../38-cvars-reference/07-system-misc-cvars.md#cl_crossx) | `cvar cl_crossx(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_crossy`](../38-cvars-reference/07-system-misc-cvars.md#cl_crossy) | `cvar cl_crossy(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_crypt_rcon`](../38-cvars-reference/07-system-misc-cvars.md#cl_crypt_rcon) | `cvar cl_crypt_rcon(int, "1")` | Системные, отладочные и прочие cvar |
| [`cl_csqc_nodeprecate`](../38-cvars-reference/07-system-misc-cvars.md#cl_csqc_nodeprecate) | `cvar cl_csqc_nodeprecate(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_csqcdebug`](../38-cvars-reference/07-system-misc-cvars.md#cl_csqcdebug) | `cvar cl_csqcdebug(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_deadbodyfilter`](../38-cvars-reference/07-system-misc-cvars.md#cl_deadbodyfilter) | `cvar cl_deadbodyfilter(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_delay_packets`](../38-cvars-reference/07-system-misc-cvars.md#cl_delay_packets) | `cvar cl_delay_packets(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_demoreel`](../38-cvars-reference/07-system-misc-cvars.md#cl_demoreel) | `cvar cl_demoreel(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_demospeed`](../38-cvars-reference/07-system-misc-cvars.md#cl_demospeed) | `cvar cl_demospeed(int, "1")` | Системные, отладочные и прочие cvar |
| [`cl_dlemptyterminate`](../38-cvars-reference/07-system-misc-cvars.md#cl_dlemptyterminate) | `cvar cl_dlemptyterminate(int, "1")` | Системные, отладочные и прочие cvar |
| [`cl_expsprite`](../38-cvars-reference/07-system-misc-cvars.md#cl_expsprite) | `cvar cl_expsprite(int, "1")` | Системные, отладочные и прочие cvar |
| [`cl_fakeframes`](../38-cvars-reference/07-system-misc-cvars.md#cl_fakeframes) | `cvar cl_fakeframes(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_fullpitch`](../38-cvars-reference/07-system-misc-cvars.md#cl_fullpitch) | `cvar cl_fullpitch(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_gibfilter`](../38-cvars-reference/07-system-misc-cvars.md#cl_gibfilter) | `cvar cl_gibfilter(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_gunanglex`](../38-cvars-reference/07-system-misc-cvars.md#cl_gunanglex) | `cvar cl_gunanglex(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_gunangley`](../38-cvars-reference/07-system-misc-cvars.md#cl_gunangley) | `cvar cl_gunangley(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_gunanglez`](../38-cvars-reference/07-system-misc-cvars.md#cl_gunanglez) | `cvar cl_gunanglez(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_gunx`](../38-cvars-reference/07-system-misc-cvars.md#cl_gunx) | `cvar cl_gunx(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_guny`](../38-cvars-reference/07-system-misc-cvars.md#cl_guny) | `cvar cl_guny(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_gunz`](../38-cvars-reference/07-system-misc-cvars.md#cl_gunz) | `cvar cl_gunz(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_hightrack`](../38-cvars-reference/07-system-misc-cvars.md#cl_hightrack) | `cvar cl_hightrack(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_hudswap`](../38-cvars-reference/07-system-misc-cvars.md#cl_hudswap) | `cvar cl_hudswap(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_idlefps`](../38-cvars-reference/07-system-misc-cvars.md#cl_idlefps) | `cvar cl_idlefps(int, "60")` | Системные, отладочные и прочие cvar |
| [`cl_legacystains`](../38-cvars-reference/07-system-misc-cvars.md#cl_legacystains) | `cvar cl_legacystains(int, "1")` | Системные, отладочные и прочие cvar |
| [`cl_lerp_maxdistance`](../38-cvars-reference/07-system-misc-cvars.md#cl_lerp_maxdistance) | `cvar cl_lerp_maxdistance(int, "200")` | Системные, отладочные и прочие cvar |
| [`cl_lerp_maxinterval`](../38-cvars-reference/07-system-misc-cvars.md#cl_lerp_maxinterval) | `cvar cl_lerp_maxinterval(float, "0.3")` | Системные, отладочные и прочие cvar |
| [`cl_lerp_players`](../38-cvars-reference/07-system-misc-cvars.md#cl_lerp_players) | `cvar cl_lerp_players(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_loopbackprotocol`](../38-cvars-reference/07-system-misc-cvars.md#cl_loopbackprotocol) | `cvar cl_loopbackprotocol(string, "qw")` | Системные, отладочные и прочие cvar |
| [`cl_maxfps`](../38-cvars-reference/07-system-misc-cvars.md#cl_maxfps) | `cvar cl_maxfps(int, "250")` | Системные, отладочные и прочие cvar |
| [`cl_model_bobbing`](../38-cvars-reference/07-system-misc-cvars.md#cl_model_bobbing) | `cvar cl_model_bobbing(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_muzzleflash`](../38-cvars-reference/07-system-misc-cvars.md#cl_muzzleflash) | `cvar cl_muzzleflash(int, "1")` | Системные, отладочные и прочие cvar |
| [`cl_netfps`](../38-cvars-reference/07-system-misc-cvars.md#cl_netfps) | `cvar cl_netfps(int, "150")` | Системные, отладочные и прочие cvar |
| [`cl_noblink`](../38-cvars-reference/07-system-misc-cvars.md#cl_noblink) | `cvar cl_noblink(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_nocsqc`](../38-cvars-reference/07-system-misc-cvars.md#cl_nocsqc) | `cvar cl_nocsqc(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_nodelta`](../38-cvars-reference/07-system-misc-cvars.md#cl_nodelta) | `cvar cl_nodelta(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_nofake`](../38-cvars-reference/07-system-misc-cvars.md#cl_nofake) | `cvar cl_nofake(int, "2")` | Системные, отладочные и прочие cvar |
| [`cl_nolerp`](../38-cvars-reference/07-system-misc-cvars.md#cl_nolerp) | `cvar cl_nolerp(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_nolerp_netquake`](../38-cvars-reference/07-system-misc-cvars.md#cl_nolerp_netquake) | `cvar cl_nolerp_netquake(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_nopext`](../38-cvars-reference/07-system-misc-cvars.md#cl_nopext) | `cvar cl_nopext(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_parsewhitetext`](../38-cvars-reference/07-system-misc-cvars.md#cl_parsewhitetext) | `cvar cl_parsewhitetext(int, "1")` | Системные, отладочные и прочие cvar |
| [`cl_part_density_fade`](../38-cvars-reference/07-system-misc-cvars.md#cl_part_density_fade) | `cvar cl_part_density_fade(int, "1024")` | Системные, отладочные и прочие cvar |
| [`cl_part_density_fade_start`](../38-cvars-reference/07-system-misc-cvars.md#cl_part_density_fade_start) | `cvar cl_part_density_fade_start(int, "1024")` | Системные, отладочные и прочие cvar |
| [`cl_pext_mask`](../38-cvars-reference/07-system-misc-cvars.md#cl_pext_mask) | `cvar cl_pext_mask(string, "0xffffffff")` | Системные, отладочные и прочие cvar |
| [`cl_playerclass`](../38-cvars-reference/07-system-misc-cvars.md#cl_playerclass) | `cvar cl_playerclass(string, "")` | Системные, отладочные и прочие cvar |
| [`cl_proxyaddr`](../38-cvars-reference/07-system-misc-cvars.md#cl_proxyaddr) | `cvar cl_proxyaddr(string, "")` | Системные, отладочные и прочие cvar |
| [`cl_pure`](../38-cvars-reference/07-system-misc-cvars.md#cl_pure) | `cvar cl_pure(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_queueimpulses`](../38-cvars-reference/07-system-misc-cvars.md#cl_queueimpulses) | `cvar cl_queueimpulses(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_r2g`](../38-cvars-reference/07-system-misc-cvars.md#cl_r2g) | `cvar cl_r2g(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_rollalpha`](../38-cvars-reference/07-system-misc-cvars.md#cl_rollalpha) | `cvar cl_rollalpha(int, "20")` | Системные, отладочные и прочие cvar |
| [`cl_sbar`](../38-cvars-reference/07-system-misc-cvars.md#cl_sbar) | `cvar cl_sbar(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_sbaralpha`](../38-cvars-reference/07-system-misc-cvars.md#cl_sbaralpha) | `cvar cl_sbaralpha(float, "0.75")` | Системные, отладочные и прочие cvar |
| [`cl_selfcam`](../38-cvars-reference/07-system-misc-cvars.md#cl_selfcam) | `cvar cl_selfcam(int, "1")` | Системные, отладочные и прочие cvar |
| [`cl_sendguid`](../38-cvars-reference/07-system-misc-cvars.md#cl_sendguid) | `cvar cl_sendguid(string, "")` | Системные, отладочные и прочие cvar |
| [`cl_serveraddress`](../38-cvars-reference/07-system-misc-cvars.md#cl_serveraddress) | `cvar cl_serveraddress(string, "none")` | Системные, отладочные и прочие cvar |
| [`cl_servername`](../38-cvars-reference/07-system-misc-cvars.md#cl_servername) | `cvar cl_servername(string, "")` | Системные, отладочные и прочие cvar |
| [`cl_shownet`](../38-cvars-reference/07-system-misc-cvars.md#cl_shownet) | `cvar cl_shownet(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_solid_players`](../38-cvars-reference/07-system-misc-cvars.md#cl_solid_players) | `cvar cl_solid_players(int, "1")` | Системные, отладочные и прочие cvar |
| [`cl_splitscreen`](../38-cvars-reference/07-system-misc-cvars.md#cl_splitscreen) | `cvar cl_splitscreen(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_standardmsg`](../38-cvars-reference/07-system-misc-cvars.md#cl_standardmsg) | `cvar cl_standardmsg(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_threadedphysics`](../38-cvars-reference/07-system-misc-cvars.md#cl_threadedphysics) | `cvar cl_threadedphysics(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_timeout`](../38-cvars-reference/07-system-misc-cvars.md#cl_timeout) | `cvar cl_timeout(int, "60")` | Системные, отладочные и прочие cvar |
| [`cl_truelightning`](../38-cvars-reference/07-system-misc-cvars.md#cl_truelightning) | `cvar cl_truelightning(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_verify_urischeme`](../38-cvars-reference/07-system-misc-cvars.md#cl_verify_urischeme) | `cvar cl_verify_urischeme(int, "2")` | Системные, отладочные и прочие cvar |
| [`cl_warncmd`](../38-cvars-reference/07-system-misc-cvars.md#cl_warncmd) | `cvar cl_warncmd(int, "1")` | Системные, отладочные и прочие cvar |
| [`cl_weaponforgetorder`](../38-cvars-reference/07-system-misc-cvars.md#cl_weaponforgetorder) | `cvar cl_weaponforgetorder(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_weaponhide`](../38-cvars-reference/07-system-misc-cvars.md#cl_weaponhide) | `cvar cl_weaponhide(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_weaponhide_preference`](../38-cvars-reference/07-system-misc-cvars.md#cl_weaponhide_preference) | `cvar cl_weaponhide_preference(string, "2 1")` | Системные, отладочные и прочие cvar |
| [`cl_weaponpreselect`](../38-cvars-reference/07-system-misc-cvars.md#cl_weaponpreselect) | `cvar cl_weaponpreselect(int, "0")` | Системные, отладочные и прочие cvar |
| [`cl_yieldcpu`](../38-cvars-reference/07-system-misc-cvars.md#cl_yieldcpu) | `cvar cl_yieldcpu(int, "1")` | Системные, отладочные и прочие cvar |
| [`cmd_allowaccess`](../38-cvars-reference/07-system-misc-cvars.md#cmd_allowaccess) | `cvar cmd_allowaccess(int, "0")` | Системные, отладочные и прочие cvar |
| [`cmd_gamecodelevel`](../38-cvars-reference/07-system-misc-cvars.md#cmd_gamecodelevel) | `cvar cmd_gamecodelevel(string, "")` | Системные, отладочные и прочие cvar |
| [`cmd_maxbuffersize`](../38-cvars-reference/07-system-misc-cvars.md#cmd_maxbuffersize) | `cvar cmd_maxbuffersize(int, "65536")` | Системные, отладочные и прочие cvar |
| [`d3d_hlsl`](../38-cvars-reference/07-system-misc-cvars.md#d3d_hlsl) | `cvar d3d_hlsl(int, "1")` | Системные, отладочные и прочие cvar |
| [`d_mipcap`](../38-cvars-reference/07-system-misc-cvars.md#d_mipcap) | `cvar d_mipcap(string, "0 1000")` | Системные, отладочные и прочие cvar |
| [`developer`](../38-cvars-reference/07-system-misc-cvars.md#developer) | `cvar developer(int, "1")` | Системные, отладочные и прочие cvar |
| [`dpcompat_csqcinputeventtypes`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_csqcinputeventtypes) | `cvar dpcompat_csqcinputeventtypes(int, "999999")` | Системные, отладочные и прочие cvar |
| [`dpcompat_findradiusarealinks`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_findradiusarealinks) | `cvar dpcompat_findradiusarealinks(int, "0")` | Системные, отладочные и прочие cvar |
| [`dpcompat_nofloodfill`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_nofloodfill) | `cvar dpcompat_nofloodfill(int, "0")` | Системные, отладочные и прочие cvar |
| [`dpcompat_nopremulpics`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_nopremulpics) | `cvar dpcompat_nopremulpics(int, "0")` | Системные, отладочные и прочие cvar |
| [`dpcompat_nopreparse`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_nopreparse) | `cvar dpcompat_nopreparse(int, "0")` | Системные, отладочные и прочие cvar |
| [`dpcompat_noretouchground`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_noretouchground) | `cvar dpcompat_noretouchground(int, "0")` | Системные, отладочные и прочие cvar |
| [`dpcompat_psa_ungroup`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_psa_ungroup) | `cvar dpcompat_psa_ungroup(int, "0")` | Системные, отладочные и прочие cvar |
| [`dpcompat_set`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_set) | `cvar dpcompat_set(int, "0")` | Системные, отладочные и прочие cvar |
| [`dpcompat_skinfiles`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_skinfiles) | `cvar dpcompat_skinfiles(int, "0")` | Системные, отладочные и прочие cvar |
| [`dpcompat_smallerfonts`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_smallerfonts) | `cvar dpcompat_smallerfonts(int, "0")` | Системные, отладочные и прочие cvar |
| [`dpcompat_stats`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_stats) | `cvar dpcompat_stats(int, "0")` | Системные, отладочные и прочие cvar |
| [`dpcompat_strcat_limit`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_strcat_limit) | `cvar dpcompat_strcat_limit(string, "")` | Системные, отладочные и прочие cvar |
| [`dpcompat_traceontouch`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_traceontouch) | `cvar dpcompat_traceontouch(int, "0")` | Системные, отладочные и прочие cvar |
| [`edit_addcr`](../38-cvars-reference/07-system-misc-cvars.md#edit_addcr) | `cvar edit_addcr(string, "")` | Системные, отладочные и прочие cvar |
| [`edit_stripcr`](../38-cvars-reference/07-system-misc-cvars.md#edit_stripcr) | `cvar edit_stripcr(int, "1")` | Системные, отладочные и прочие cvar |
| [`edit_tabsize`](../38-cvars-reference/07-system-misc-cvars.md#edit_tabsize) | `cvar edit_tabsize(int, "4")` | Системные, отладочные и прочие cvar |
| [`enemyforceskins`](../38-cvars-reference/07-system-misc-cvars.md#enemyforceskins) | `cvar enemyforceskins(int, "0")` | Системные, отладочные и прочие cvar |
| [`ezcompat_markup`](../38-cvars-reference/07-system-misc-cvars.md#ezcompat_markup) | `cvar ezcompat_markup(int, "1")` | Системные, отладочные и прочие cvar |
| [`fbskins`](../38-cvars-reference/07-system-misc-cvars.md#fbskins) | `cvar fbskins(string, "")` | Системные, отладочные и прочие cvar |
| [`forceqmenu`](../38-cvars-reference/07-system-misc-cvars.md#forceqmenu) | `cvar forceqmenu(int, "0")` | Системные, отладочные и прочие cvar |
| [`fraglog_details`](../38-cvars-reference/07-system-misc-cvars.md#fraglog_details) | `cvar fraglog_details(int, "1")` | Системные, отладочные и прочие cvar |
| [`gamecfg`](../38-cvars-reference/07-system-misc-cvars.md#gamecfg) | `cvar gamecfg(int, "0")` | Системные, отладочные и прочие cvar |
| [`gameversion`](../38-cvars-reference/07-system-misc-cvars.md#gameversion) | `cvar gameversion(string, "")` | Системные, отладочные и прочие cvar |
| [`gameversion_max`](../38-cvars-reference/07-system-misc-cvars.md#gameversion_max) | `cvar gameversion_max(string, "")` | Системные, отладочные и прочие cvar |
| [`gameversion_min`](../38-cvars-reference/07-system-misc-cvars.md#gameversion_min) | `cvar gameversion_min(string, "")` | Системные, отладочные и прочие cvar |
| [`hand`](../38-cvars-reference/07-system-misc-cvars.md#hand) | `cvar hand(string, "")` | Системные, отладочные и прочие cvar |
| [`ignore_flood`](../38-cvars-reference/07-system-misc-cvars.md#ignore_flood) | `cvar ignore_flood(int, "0")` | Системные, отладочные и прочие cvar |
| [`ignore_flood_duration`](../38-cvars-reference/07-system-misc-cvars.md#ignore_flood_duration) | `cvar ignore_flood_duration(int, "4")` | Системные, отладочные и прочие cvar |
| [`ignore_mode`](../38-cvars-reference/07-system-misc-cvars.md#ignore_mode) | `cvar ignore_mode(int, "0")` | Системные, отладочные и прочие cvar |
| [`ignore_opponents`](../38-cvars-reference/07-system-misc-cvars.md#ignore_opponents) | `cvar ignore_opponents(int, "0")` | Системные, отладочные и прочие cvar |
| [`ignore_qizmo_spec`](../38-cvars-reference/07-system-misc-cvars.md#ignore_qizmo_spec) | `cvar ignore_qizmo_spec(int, "0")` | Системные, отладочные и прочие cvar |
| [`ignore_spec`](../38-cvars-reference/07-system-misc-cvars.md#ignore_spec) | `cvar ignore_spec(int, "0")` | Системные, отладочные и прочие cvar |
| [`ipautodump`](../38-cvars-reference/07-system-misc-cvars.md#ipautodump) | `cvar ipautodump(int, "0")` | Системные, отладочные и прочие cvar |
| [`itburnsitburnsmakeitstop`](../38-cvars-reference/07-system-misc-cvars.md#itburnsitburnsmakeitstop) | `cvar itburnsitburnsmakeitstop(int, "0")` | Системные, отладочные и прочие cvar |
| [`joyradialdeadzone`](../38-cvars-reference/07-system-misc-cvars.md#joyradialdeadzone) | `cvar joyradialdeadzone(string, "")` | Системные, отладочные и прочие cvar |
| [`lang`](../38-cvars-reference/07-system-misc-cvars.md#lang) | `cvar lang(string, "prvm_language")` | Системные, отладочные и прочие cvar |
| [`leftisright`](../38-cvars-reference/07-system-misc-cvars.md#leftisright) | `cvar leftisright(int, "0")` | Системные, отладочные и прочие cvar |
| [`log_developer`](../38-cvars-reference/07-system-misc-cvars.md#log_developer) | `cvar log_developer(int, "0")` | Системные, отладочные и прочие cvar |
| [`log_dir`](../38-cvars-reference/07-system-misc-cvars.md#log_dir) | `cvar log_dir(string, "")` | Системные, отладочные и прочие cvar |
| [`log_dosformat`](../38-cvars-reference/07-system-misc-cvars.md#log_dosformat) | `cvar log_dosformat(int, "1")` | Системные, отладочные и прочие cvar |
| [`log_readable`](../38-cvars-reference/07-system-misc-cvars.md#log_readable) | `cvar log_readable(int, "7")` | Системные, отладочные и прочие cvar |
| [`log_rotate_files`](../38-cvars-reference/07-system-misc-cvars.md#log_rotate_files) | `cvar log_rotate_files(int, "0")` | Системные, отладочные и прочие cvar |
| [`log_rotate_size`](../38-cvars-reference/07-system-misc-cvars.md#log_rotate_size) | `cvar log_rotate_size(int, "131072")` | Системные, отладочные и прочие cvar |
| [`log_timestamps`](../38-cvars-reference/07-system-misc-cvars.md#log_timestamps) | `cvar log_timestamps(int, "1")` | Системные, отладочные и прочие cvar |
| [`lookspring`](../38-cvars-reference/07-system-misc-cvars.md#lookspring) | `cvar lookspring(int, "0")` | Системные, отладочные и прочие cvar |
| [`lookstrafe`](../38-cvars-reference/07-system-misc-cvars.md#lookstrafe) | `cvar lookstrafe(int, "0")` | Системные, отладочные и прочие cvar |
| [`m_accel`](../38-cvars-reference/07-system-misc-cvars.md#m_accel) | `cvar m_accel(int, "0")` | Системные, отладочные и прочие cvar |
| [`m_accel_noforce`](../38-cvars-reference/07-system-misc-cvars.md#m_accel_noforce) | `cvar m_accel_noforce(int, "0")` | Системные, отладочные и прочие cvar |
| [`m_accel_offset`](../38-cvars-reference/07-system-misc-cvars.md#m_accel_offset) | `cvar m_accel_offset(int, "0")` | Системные, отладочные и прочие cvar |
| [`m_accel_power`](../38-cvars-reference/07-system-misc-cvars.md#m_accel_power) | `cvar m_accel_power(int, "2")` | Системные, отладочные и прочие cvar |
| [`m_accel_senscap`](../38-cvars-reference/07-system-misc-cvars.md#m_accel_senscap) | `cvar m_accel_senscap(int, "0")` | Системные, отладочные и прочие cvar |
| [`m_accel_style`](../38-cvars-reference/07-system-misc-cvars.md#m_accel_style) | `cvar m_accel_style(int, "1")` | Системные, отладочные и прочие cvar |
| [`m_fatpressthreshold`](../38-cvars-reference/07-system-misc-cvars.md#m_fatpressthreshold) | `cvar m_fatpressthreshold(float, "0.2")` | Системные, отладочные и прочие cvar |
| [`m_preset_chosen`](../38-cvars-reference/07-system-misc-cvars.md#m_preset_chosen) | `cvar m_preset_chosen(int, "0")` | Системные, отладочные и прочие cvar |
| [`m_threshold_noforce`](../38-cvars-reference/07-system-misc-cvars.md#m_threshold_noforce) | `cvar m_threshold_noforce(int, "0")` | Системные, отладочные и прочие cvar |
| [`m_touchstrafe`](../38-cvars-reference/07-system-misc-cvars.md#m_touchstrafe) | `cvar m_touchstrafe(int, "0")` | Системные, отладочные и прочие cvar |
| [`map_autoopenportals`](../38-cvars-reference/07-system-misc-cvars.md#map_autoopenportals) | `cvar map_autoopenportals(int, "0")` | Системные, отладочные и прочие cvar |
| [`map_noareas`](../38-cvars-reference/07-system-misc-cvars.md#map_noareas) | `cvar map_noareas(int, "0")` | Системные, отладочные и прочие cvar |
| [`map_noCurves`](../38-cvars-reference/07-system-misc-cvars.md#map_nocurves) | `cvar map_noCurves(int, "0")` | Системные, отладочные и прочие cvar |
| [`mapname`](../38-cvars-reference/07-system-misc-cvars.md#mapname) | `cvar mapname(string, "")` | Системные, отладочные и прочие cvar |
| [`maxpitch`](../38-cvars-reference/07-system-misc-cvars.md#maxpitch) | `cvar maxpitch(string, "")` | Системные, отладочные и прочие cvar |
| [`minpitch`](../38-cvars-reference/07-system-misc-cvars.md#minpitch) | `cvar minpitch(string, "")` | Системные, отладочные и прочие cvar |
| [`mod_h2holey_bugged`](../38-cvars-reference/07-system-misc-cvars.md#mod_h2holey_bugged) | `cvar mod_h2holey_bugged(int, "0")` | Системные, отладочные и прочие cvar |
| [`mod_halftexel`](../38-cvars-reference/07-system-misc-cvars.md#mod_halftexel) | `cvar mod_halftexel(int, "1")` | Системные, отладочные и прочие cvar |
| [`mod_lightpoint_distance`](../38-cvars-reference/07-system-misc-cvars.md#mod_lightpoint_distance) | `cvar mod_lightpoint_distance(int, "8192")` | Системные, отладочные и прочие cvar |
| [`mod_lightscale_broken`](../38-cvars-reference/07-system-misc-cvars.md#mod_lightscale_broken) | `cvar mod_lightscale_broken(int, "0")` | Системные, отладочные и прочие cvar |
| [`mod_loadmappackages`](../38-cvars-reference/07-system-misc-cvars.md#mod_loadmappackages) | `cvar mod_loadmappackages(int, "1")` | Системные, отладочные и прочие cvar |
| [`mod_md3flags`](../38-cvars-reference/07-system-misc-cvars.md#mod_md3flags) | `cvar mod_md3flags(int, "1")` | Системные, отладочные и прочие cvar |
| [`mod_md5_singleanimation`](../38-cvars-reference/07-system-misc-cvars.md#mod_md5_singleanimation) | `cvar mod_md5_singleanimation(int, "1")` | Системные, отладочные и прочие cvar |
| [`mod_nomipmap`](../38-cvars-reference/07-system-misc-cvars.md#mod_nomipmap) | `cvar mod_nomipmap(int, "0")` | Системные, отладочные и прочие cvar |
| [`mod_obj_orientation`](../38-cvars-reference/07-system-misc-cvars.md#mod_obj_orientation) | `cvar mod_obj_orientation(int, "1")` | Системные, отладочные и прочие cvar |
| [`mod_precache`](../38-cvars-reference/07-system-misc-cvars.md#mod_precache) | `cvar mod_precache(int, "1")` | Системные, отладочные и прочие cvar |
| [`mod_warnmodels`](../38-cvars-reference/07-system-misc-cvars.md#mod_warnmodels) | `cvar mod_warnmodels(int, "1")` | Системные, отладочные и прочие cvar |
| [`model`](../38-cvars-reference/07-system-misc-cvars.md#model) | `cvar model(string, "")` | Системные, отладочные и прочие cvar |
| [`msg`](../38-cvars-reference/07-system-misc-cvars.md#msg) | `cvar msg(int, "1")` | Системные, отладочные и прочие cvar |
| [`msg_filter`](../38-cvars-reference/07-system-misc-cvars.md#msg_filter) | `cvar msg_filter(int, "0")` | Системные, отладочные и прочие cvar |
| [`musicvolume`](../38-cvars-reference/07-system-misc-cvars.md#musicvolume) | `cvar musicvolume(float, "0.3")` | Системные, отладочные и прочие cvar |
| [`name`](../38-cvars-reference/07-system-misc-cvars.md#name) | `cvar name(string, "Player")` | Системные, отладочные и прочие cvar |
| [`noaim`](../38-cvars-reference/07-system-misc-cvars.md#noaim) | `cvar noaim(string, "")` | Системные, отладочные и прочие cvar |
| [`noexit`](../38-cvars-reference/07-system-misc-cvars.md#noexit) | `cvar noexit(int, "0")` | Системные, отладочные и прочие cvar |
| [`nomonsters`](../38-cvars-reference/07-system-misc-cvars.md#nomonsters) | `cvar nomonsters(int, "0")` | Системные, отладочные и прочие cvar |
| [`noskins`](../38-cvars-reference/07-system-misc-cvars.md#noskins) | `cvar noskins(int, "0")` | Системные, отладочные и прочие cvar |
| [`pext_ezquake_nochunks`](../38-cvars-reference/07-system-misc-cvars.md#pext_ezquake_nochunks) | `cvar pext_ezquake_nochunks(int, "0")` | Системные, отладочные и прочие cvar |
| [`pext_ezquake_verfortrans`](../38-cvars-reference/07-system-misc-cvars.md#pext_ezquake_verfortrans) | `cvar pext_ezquake_verfortrans(int, "7088")` | Системные, отладочные и прочие cvar |
| [`pext_predinfo`](../38-cvars-reference/07-system-misc-cvars.md#pext_predinfo) | `cvar pext_predinfo(int, "1")` | Системные, отладочные и прочие cvar |
| [`pext_replacementdeltas`](../38-cvars-reference/07-system-misc-cvars.md#pext_replacementdeltas) | `cvar pext_replacementdeltas(int, "1")` | Системные, отладочные и прочие cvar |
| [`pkg_autoupdate`](../38-cvars-reference/07-system-misc-cvars.md#pkg_autoupdate) | `cvar pkg_autoupdate(int, "1")` | Системные, отладочные и прочие cvar |
| [`plug_loaddefault`](../38-cvars-reference/07-system-misc-cvars.md#plug_loaddefault) | `cvar plug_loaddefault(int, "1")` | Системные, отладочные и прочие cvar |
| [`plug_sbar`](../38-cvars-reference/07-system-misc-cvars.md#plug_sbar) | `cvar plug_sbar(int, "0")` | Системные, отладочные и прочие cvar |
| [`prox_inmenu`](../38-cvars-reference/07-system-misc-cvars.md#prox_inmenu) | `cvar prox_inmenu(int, "0")` | Системные, отладочные и прочие cvar |
| [`q3bsp_ignorestyles`](../38-cvars-reference/07-system-misc-cvars.md#q3bsp_ignorestyles) | `cvar q3bsp_ignorestyles(int, "0")` | Системные, отладочные и прочие cvar |
| [`q3bsp_mergelightmaps`](../38-cvars-reference/07-system-misc-cvars.md#q3bsp_mergelightmaps) | `cvar q3bsp_mergelightmaps(int, "1")` | Системные, отладочные и прочие cvar |
| [`q3bsp_surf_meshcollision_flag`](../38-cvars-reference/07-system-misc-cvars.md#q3bsp_surf_meshcollision_flag) | `cvar q3bsp_surf_meshcollision_flag(string, "0x80000000")` | Системные, отладочные и прочие cvar |
| [`q3bsp_surf_meshcollision_force`](../38-cvars-reference/07-system-misc-cvars.md#q3bsp_surf_meshcollision_force) | `cvar q3bsp_surf_meshcollision_force(int, "0")` | Системные, отладочные и прочие cvar |
| [`qport_`](../38-cvars-reference/07-system-misc-cvars.md#qport_) | `cvar qport_(int, "0")` | Системные, отладочные и прочие cvar |
| [`rank_autoadd`](../38-cvars-reference/07-system-misc-cvars.md#rank_autoadd) | `cvar rank_autoadd(int, "1")` | Системные, отладочные и прочие cvar |
| [`rank_filename`](../38-cvars-reference/07-system-misc-cvars.md#rank_filename) | `cvar rank_filename(string, "")` | Системные, отладочные и прочие cvar |
| [`rank_needlogin`](../38-cvars-reference/07-system-misc-cvars.md#rank_needlogin) | `cvar rank_needlogin(int, "0")` | Системные, отладочные и прочие cvar |
| [`record_flush`](../38-cvars-reference/07-system-misc-cvars.md#record_flush) | `cvar record_flush(int, "0")` | Системные, отладочные и прочие cvar |
| [`registered`](../38-cvars-reference/07-system-misc-cvars.md#registered) | `cvar registered(int, "0")` | Системные, отладочные и прочие cvar |
| [`route_shownodes`](../38-cvars-reference/07-system-misc-cvars.md#route_shownodes) | `cvar route_shownodes(int, "0")` | Системные, отладочные и прочие cvar |
| [`ruleset`](../38-cvars-reference/07-system-misc-cvars.md#ruleset) | `cvar ruleset(string, "none")` | Системные, отладочные и прочие cvar |
| [`ruleset_allow_fbmodels`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_fbmodels) | `cvar ruleset_allow_fbmodels(int, "0")` | Системные, отладочные и прочие cvar |
| [`ruleset_allow_frj`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_frj) | `cvar ruleset_allow_frj(int, "1")` | Системные, отладочные и прочие cvar |
| [`ruleset_allow_in`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_in) | `cvar ruleset_allow_in(int, "1")` | Системные, отладочные и прочие cvar |
| [`ruleset_allow_localvolume`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_localvolume) | `cvar ruleset_allow_localvolume(int, "1")` | Системные, отладочные и прочие cvar |
| [`ruleset_allow_modified_eyes`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_modified_eyes) | `cvar ruleset_allow_modified_eyes(int, "0")` | Системные, отладочные и прочие cvar |
| [`ruleset_allow_packet`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_packet) | `cvar ruleset_allow_packet(int, "1")` | Системные, отладочные и прочие cvar |
| [`ruleset_allow_particle_lightning`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_particle_lightning) | `cvar ruleset_allow_particle_lightning(int, "1")` | Системные, отладочные и прочие cvar |
| [`ruleset_allow_playercount`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_playercount) | `cvar ruleset_allow_playercount(int, "1")` | Системные, отладочные и прочие cvar |
| [`ruleset_allow_semicheats`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_semicheats) | `cvar ruleset_allow_semicheats(int, "1")` | Системные, отладочные и прочие cvar |
| [`ruleset_allow_sensitive_texture_replacements`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_sensitive_texture_replacements) | `cvar ruleset_allow_sensitive_texture_replacements(int, "1")` | Системные, отладочные и прочие cvar |
| [`ruleset_allow_triggers`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_triggers) | `cvar ruleset_allow_triggers(int, "1")` | Системные, отладочные и прочие cvar |
| [`ruleset_allow_watervis`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_watervis) | `cvar ruleset_allow_watervis(int, "1")` | Системные, отладочные и прочие cvar |
| [`saved1`](../38-cvars-reference/07-system-misc-cvars.md#saved1) | `cvar saved1(int, "0")` | Системные, отладочные и прочие cvar |
| [`saved2`](../38-cvars-reference/07-system-misc-cvars.md#saved2) | `cvar saved2(int, "0")` | Системные, отладочные и прочие cvar |
| [`saved3`](../38-cvars-reference/07-system-misc-cvars.md#saved3) | `cvar saved3(int, "0")` | Системные, отладочные и прочие cvar |
| [`saved4`](../38-cvars-reference/07-system-misc-cvars.md#saved4) | `cvar saved4(int, "0")` | Системные, отладочные и прочие cvar |
| [`savedgamecfg`](../38-cvars-reference/07-system-misc-cvars.md#savedgamecfg) | `cvar savedgamecfg(int, "0")` | Системные, отладочные и прочие cvar |
| [`sb_alpha`](../38-cvars-reference/07-system-misc-cvars.md#sb_alpha) | `cvar sb_alpha(float, "0.7")` | Системные, отладочные и прочие cvar |
| [`sb_filtertext`](../38-cvars-reference/07-system-misc-cvars.md#sb_filtertext) | `cvar sb_filtertext(string, "")` | Системные, отладочные и прочие cvar |
| [`sb_hidedead`](../38-cvars-reference/07-system-misc-cvars.md#sb_hidedead) | `cvar sb_hidedead(int, "1")` | Системные, отладочные и прочие cvar |
| [`sb_hideempty`](../38-cvars-reference/07-system-misc-cvars.md#sb_hideempty) | `cvar sb_hideempty(int, "0")` | Системные, отладочные и прочие cvar |
| [`sb_hidefull`](../38-cvars-reference/07-system-misc-cvars.md#sb_hidefull) | `cvar sb_hidefull(int, "0")` | Системные, отладочные и прочие cvar |
| [`sb_hidenetquake`](../38-cvars-reference/07-system-misc-cvars.md#sb_hidenetquake) | `cvar sb_hidenetquake(int, "0")` | Системные, отладочные и прочие cvar |
| [`sb_hidenotempty`](../38-cvars-reference/07-system-misc-cvars.md#sb_hidenotempty) | `cvar sb_hidenotempty(int, "0")` | Системные, отладочные и прочие cvar |
| [`sb_hideproxies`](../38-cvars-reference/07-system-misc-cvars.md#sb_hideproxies) | `cvar sb_hideproxies(int, "1")` | Системные, отладочные и прочие cvar |
| [`sb_hidequakeworld`](../38-cvars-reference/07-system-misc-cvars.md#sb_hidequakeworld) | `cvar sb_hidequakeworld(int, "0")` | Системные, отладочные и прочие cvar |
| [`sb_showaddress`](../38-cvars-reference/07-system-misc-cvars.md#sb_showaddress) | `cvar sb_showaddress(int, "0")` | Системные, отладочные и прочие cvar |
| [`sb_showgamedir`](../38-cvars-reference/07-system-misc-cvars.md#sb_showgamedir) | `cvar sb_showgamedir(int, "0")` | Системные, отладочные и прочие cvar |
| [`sb_showmap`](../38-cvars-reference/07-system-misc-cvars.md#sb_showmap) | `cvar sb_showmap(int, "0")` | Системные, отладочные и прочие cvar |
| [`sb_showping`](../38-cvars-reference/07-system-misc-cvars.md#sb_showping) | `cvar sb_showping(int, "0")` | Системные, отладочные и прочие cvar |
| [`sb_showplayers`](../38-cvars-reference/07-system-misc-cvars.md#sb_showplayers) | `cvar sb_showplayers(int, "1")` | Системные, отладочные и прочие cvar |
| [`sb_sortcolumn`](../38-cvars-reference/07-system-misc-cvars.md#sb_sortcolumn) | `cvar sb_sortcolumn(int, "0")` | Системные, отладочные и прочие cvar |
| [`scratch1`](../38-cvars-reference/07-system-misc-cvars.md#scratch1) | `cvar scratch1(int, "0")` | Системные, отладочные и прочие cvar |
| [`scratch2`](../38-cvars-reference/07-system-misc-cvars.md#scratch2) | `cvar scratch2(int, "0")` | Системные, отладочные и прочие cvar |
| [`scratch3`](../38-cvars-reference/07-system-misc-cvars.md#scratch3) | `cvar scratch3(int, "0")` | Системные, отладочные и прочие cvar |
| [`scratch4`](../38-cvars-reference/07-system-misc-cvars.md#scratch4) | `cvar scratch4(int, "0")` | Системные, отладочные и прочие cvar |
| [`secure`](../38-cvars-reference/07-system-misc-cvars.md#secure) | `cvar secure(string, "")` | Системные, отладочные и прочие cvar |
| [`sensitivity`](../38-cvars-reference/07-system-misc-cvars.md#sensitivity) | `cvar sensitivity(int, "10")` | Системные, отладочные и прочие cvar |
| [`show_fps`](../38-cvars-reference/07-system-misc-cvars.md#show_fps) | `cvar show_fps(int, "0")` | Системные, отладочные и прочие cvar |
| [`show_speed_x`](../38-cvars-reference/07-system-misc-cvars.md#show_speed_x) | `cvar show_speed_x(int, "-1")` | Системные, отладочные и прочие cvar |
| [`show_speed_y`](../38-cvars-reference/07-system-misc-cvars.md#show_speed_y) | `cvar show_speed_y(int, "-9")` | Системные, отладочные и прочие cvar |
| [`showdrop`](../38-cvars-reference/07-system-misc-cvars.md#showdrop) | `cvar showdrop(int, "0")` | Системные, отладочные и прочие cvar |
| [`showpackets`](../38-cvars-reference/07-system-misc-cvars.md#showpackets) | `cvar showpackets(int, "0")` | Системные, отладочные и прочие cvar |
| [`showpause`](../38-cvars-reference/07-system-misc-cvars.md#showpause) | `cvar showpause(int, "1")` | Системные, отладочные и прочие cvar |
| [`showturtle`](../38-cvars-reference/07-system-misc-cvars.md#showturtle) | `cvar showturtle(int, "0")` | Системные, отладочные и прочие cvar |
| [`skin`](../38-cvars-reference/07-system-misc-cvars.md#skin) | `cvar skin(string, "")` | Системные, отладочные и прочие cvar |
| [`skyroom`](../38-cvars-reference/07-system-misc-cvars.md#skyroom) | `cvar skyroom(string, "")` | Системные, отладочные и прочие cvar |
| [`slist_cacheinfo`](../38-cvars-reference/07-system-misc-cvars.md#slist_cacheinfo) | `cvar slist_cacheinfo(int, "0")` | Системные, отладочные и прочие cvar |
| [`slist_writeservers`](../38-cvars-reference/07-system-misc-cvars.md#slist_writeservers) | `cvar slist_writeservers(int, "1")` | Системные, отладочные и прочие cvar |
| [`spectator`](../38-cvars-reference/07-system-misc-cvars.md#spectator) | `cvar spectator(string, "")` | Системные, отладочные и прочие cvar |
| [`sw_fthreads`](../38-cvars-reference/07-system-misc-cvars.md#sw_fthreads) | `cvar sw_fthreads(int, "0")` | Системные, отладочные и прочие cvar |
| [`sw_interlace`](../38-cvars-reference/07-system-misc-cvars.md#sw_interlace) | `cvar sw_interlace(int, "0")` | Системные, отладочные и прочие cvar |
| [`sw_vthread`](../38-cvars-reference/07-system-misc-cvars.md#sw_vthread) | `cvar sw_vthread(int, "0")` | Системные, отладочные и прочие cvar |
| [`team`](../38-cvars-reference/07-system-misc-cvars.md#team) | `cvar team(string, "")` | Системные, отладочные и прочие cvar |
| [`topcolor`](../38-cvars-reference/07-system-misc-cvars.md#topcolor) | `cvar topcolor(int, "13")` | Системные, отладочные и прочие cvar |
| [`tp_disputablemacros`](../38-cvars-reference/07-system-misc-cvars.md#tp_disputablemacros) | `cvar tp_disputablemacros(int, "1")` | Системные, отладочные и прочие cvar |
| [`utf8_enable`](../38-cvars-reference/07-system-misc-cvars.md#utf8_enable) | `cvar utf8_enable(int, "0")` | Системные, отладочные и прочие cvar |
| [`vk_amd_rasterization_order`](../38-cvars-reference/07-system-misc-cvars.md#vk_amd_rasterization_order) | `cvar vk_amd_rasterization_order(string, "")` | Системные, отладочные и прочие cvar |
| [`vk_busywait`](../38-cvars-reference/07-system-misc-cvars.md#vk_busywait) | `cvar vk_busywait(string, "")` | Системные, отладочные и прочие cvar |
| [`vk_debug`](../38-cvars-reference/07-system-misc-cvars.md#vk_debug) | `cvar vk_debug(int, "0")` | Системные, отладочные и прочие cvar |
| [`vk_dualqueue`](../38-cvars-reference/07-system-misc-cvars.md#vk_dualqueue) | `cvar vk_dualqueue(string, "")` | Системные, отладочные и прочие cvar |
| [`vk_ext_astc_decode_mode`](../38-cvars-reference/07-system-misc-cvars.md#vk_ext_astc_decode_mode) | `cvar vk_ext_astc_decode_mode(string, "")` | Системные, отладочные и прочие cvar |
| [`vk_stagingbuffers`](../38-cvars-reference/07-system-misc-cvars.md#vk_stagingbuffers) | `cvar vk_stagingbuffers(string, "")` | Системные, отладочные и прочие cvar |
| [`vk_submissionthread`](../38-cvars-reference/07-system-misc-cvars.md#vk_submissionthread) | `cvar vk_submissionthread(string, "")` | Системные, отладочные и прочие cvar |
| [`vk_usememorypools`](../38-cvars-reference/07-system-misc-cvars.md#vk_usememorypools) | `cvar vk_usememorypools(string, "")` | Системные, отладочные и прочие cvar |
| [`vk_waitfence`](../38-cvars-reference/07-system-misc-cvars.md#vk_waitfence) | `cvar vk_waitfence(string, "")` | Системные, отладочные и прочие cvar |
| [`volume`](../38-cvars-reference/07-system-misc-cvars.md#volume) | `cvar volume(float, "0.7")` | Системные, отладочные и прочие cvar |
| [`votelevel`](../38-cvars-reference/07-system-misc-cvars.md#votelevel) | `cvar votelevel(int, "0")` | Системные, отладочные и прочие cvar |
| [`voteminimum`](../38-cvars-reference/07-system-misc-cvars.md#voteminimum) | `cvar voteminimum(int, "4")` | Системные, отладочные и прочие cvar |
| [`votepercent`](../38-cvars-reference/07-system-misc-cvars.md#votepercent) | `cvar votepercent(int, "-1")` | Системные, отладочные и прочие cvar |
| [`votetime`](../38-cvars-reference/07-system-misc-cvars.md#votetime) | `cvar votetime(int, "10")` | Системные, отладочные и прочие cvar |
| [`w_switch`](../38-cvars-reference/07-system-misc-cvars.md#w_switch) | `cvar w_switch(string, "")` | Системные, отладочные и прочие cvar |
| [`watervis`](../38-cvars-reference/07-system-misc-cvars.md#watervis) | `cvar watervis(string, "")` | Системные, отладочные и прочие cvar |
| [`xinput_leftvibrator`](../38-cvars-reference/07-system-misc-cvars.md#xinput_leftvibrator) | `cvar xinput_leftvibrator(int, "0")` | Системные, отладочные и прочие cvar |
| [`xinput_rightvibrator`](../38-cvars-reference/07-system-misc-cvars.md#xinput_rightvibrator) | `cvar xinput_rightvibrator(int, "0")` | Системные, отладочные и прочие cvar |

## Ключи сущностей карты (entity keys)

Всего задокументировано: **146** ключей. Полный постатейный разбор — в разделе [«39. Ключи сущностей карты»](../39-entity-keys-reference/README.md).

| Функция | Сигнатура | Категория |
|---|---|---|
| [`classname`](../39-entity-keys-reference/01-worldspawn-common-keys.md#classname) | `тип значения: string` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`origin`](../39-entity-keys-reference/01-worldspawn-common-keys.md#origin) | `тип значения: vector("x y z")` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`angle`](../39-entity-keys-reference/01-worldspawn-common-keys.md#angle) | `тип значения: float` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`angles`](../39-entity-keys-reference/01-worldspawn-common-keys.md#angles) | `тип значения: vector("x y z")` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`targetname`](../39-entity-keys-reference/01-worldspawn-common-keys.md#targetname) | `тип значения: string` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`target`](../39-entity-keys-reference/01-worldspawn-common-keys.md#target) | `тип значения: string` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`target2`](../39-entity-keys-reference/01-worldspawn-common-keys.md#target2) | `тип значения: string` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`target3`](../39-entity-keys-reference/01-worldspawn-common-keys.md#target3) | `тип значения: string` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`killtarget`](../39-entity-keys-reference/01-worldspawn-common-keys.md#killtarget) | `тип значения: string` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`spawnflags`](../39-entity-keys-reference/01-worldspawn-common-keys.md#spawnflags) | `тип значения: integer` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`model`](../39-entity-keys-reference/01-worldspawn-common-keys.md#model) | `тип значения: string` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`wait`](../39-entity-keys-reference/01-worldspawn-common-keys.md#wait) | `тип значения: float` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`delay`](../39-entity-keys-reference/01-worldspawn-common-keys.md#delay) | `тип значения: float` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`message`](../39-entity-keys-reference/01-worldspawn-common-keys.md#message) | `тип значения: string` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`noise`](../39-entity-keys-reference/01-worldspawn-common-keys.md#noise) | `тип значения: string` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`sounds`](../39-entity-keys-reference/01-worldspawn-common-keys.md#sounds) | `тип значения: integer` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`count`](../39-entity-keys-reference/01-worldspawn-common-keys.md#count) | `тип значения: integer` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`dmg`](../39-entity-keys-reference/01-worldspawn-common-keys.md#dmg) | `тип значения: float` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`health`](../39-entity-keys-reference/01-worldspawn-common-keys.md#health) | `тип значения: float` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`style`](../39-entity-keys-reference/01-worldspawn-common-keys.md#style) | `тип значения: integer` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`skin`](../39-entity-keys-reference/01-worldspawn-common-keys.md#skin) | `тип значения: integer` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`mangle`](../39-entity-keys-reference/01-worldspawn-common-keys.md#mangle) | `тип значения: vector("x y z")` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`light`](../39-entity-keys-reference/01-worldspawn-common-keys.md#light) | `тип значения: integer` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`speed`](../39-entity-keys-reference/01-worldspawn-common-keys.md#speed) | `тип значения: float` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`map`](../39-entity-keys-reference/01-worldspawn-common-keys.md#map) | `тип значения: string` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`lip`](../39-entity-keys-reference/01-worldspawn-common-keys.md#lip) | `тип значения: float` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`height`](../39-entity-keys-reference/01-worldspawn-common-keys.md#height) | `тип значения: float` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`message`](../39-entity-keys-reference/01-worldspawn-common-keys.md#message) | `тип значения: string` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`sounds`](../39-entity-keys-reference/01-worldspawn-common-keys.md#sounds) | `тип значения: integer` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`worldtype`](../39-entity-keys-reference/01-worldspawn-common-keys.md#worldtype) | `тип значения: integer` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`wad`](../39-entity-keys-reference/01-worldspawn-common-keys.md#wad) | `тип значения: string` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`model`](../39-entity-keys-reference/01-worldspawn-common-keys.md#model) | `тип значения: string` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`gravity`](../39-entity-keys-reference/01-worldspawn-common-keys.md#gravity) | `тип значения: float` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`sky`](../39-entity-keys-reference/01-worldspawn-common-keys.md#sky) | `тип значения: string` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`MaxRange`](../39-entity-keys-reference/01-worldspawn-common-keys.md#maxrange) | `тип значения: float` | Общие ключи, worldspawn и глобальные настройки уровня |
| [`classname`](../39-entity-keys-reference/02-light-entity-keys.md#classname) | `тип значения: string` | Свет и освещение |
| [`origin`](../39-entity-keys-reference/02-light-entity-keys.md#origin) | `тип значения: vector("x y z")` | Свет и освещение |
| [`light`](../39-entity-keys-reference/02-light-entity-keys.md#light) | `тип значения: integer или string из четырёх чисел "R G B brightness"` | Свет и освещение |
| [`style`](../39-entity-keys-reference/02-light-entity-keys.md#style) | `тип значения: integer` | Свет и освещение |
| [`targetname`](../39-entity-keys-reference/02-light-entity-keys.md#targetname) | `тип значения: string` | Свет и освещение |
| [`target`](../39-entity-keys-reference/02-light-entity-keys.md#target) | `тип значения: string` | Свет и освещение |
| [`spawnflags`](../39-entity-keys-reference/02-light-entity-keys.md#spawnflags) | `тип значения: integer` | Свет и освещение |
| [`angle`](../39-entity-keys-reference/02-light-entity-keys.md#angle) | `тип значения: float` | Свет и освещение |
| [`mangle`](../39-entity-keys-reference/02-light-entity-keys.md#mangle) | `тип значения: vector("pitch yaw roll")` | Свет и освещение |
| [`angles`](../39-entity-keys-reference/02-light-entity-keys.md#angles) | `тип значения: vector("pitch yaw roll")` | Свет и освещение |
| [`cone`](../39-entity-keys-reference/02-light-entity-keys.md#cone) | `тип значения: float` | Свет и освещение |
| [`color`](../39-entity-keys-reference/02-light-entity-keys.md#color) | `тип значения: vector("r g b")` | Свет и освещение |
| [`delay`](../39-entity-keys-reference/02-light-entity-keys.md#delay) | `тип значения: integer` | Свет и освещение |
| [`wait`](../39-entity-keys-reference/02-light-entity-keys.md#wait) | `тип значения: float` | Свет и освещение |
| [`fade`](../39-entity-keys-reference/02-light-entity-keys.md#fade) | `тип значения: float` | Свет и освещение |
| [`scale`](../39-entity-keys-reference/02-light-entity-keys.md#scale) | `тип значения: float` | Свет и освещение |
| [`skin`](../39-entity-keys-reference/02-light-entity-keys.md#skin) | `тип значения: integer` | Свет и освещение |
| [`pflags`](../39-entity-keys-reference/02-light-entity-keys.md#pflags) | `тип значения: integer` | Свет и освещение |
| [`light_radius`](../39-entity-keys-reference/02-light-entity-keys.md#light_radius) | `тип значения: float` | Свет и освещение |
| [`classname`](../39-entity-keys-reference/03-trigger-logic-keys.md#classname) | `тип значения: string` | Триггеры и логические сущности |
| [`targetname`](../39-entity-keys-reference/03-trigger-logic-keys.md#targetname) | `тип значения: string` | Триггеры и логические сущности |
| [`target`](../39-entity-keys-reference/03-trigger-logic-keys.md#target) | `тип значения: string` | Триггеры и логические сущности |
| [`target2`](../39-entity-keys-reference/03-trigger-logic-keys.md#target2) | `тип значения: string` | Триггеры и логические сущности |
| [`killtarget`](../39-entity-keys-reference/03-trigger-logic-keys.md#killtarget) | `тип значения: string` | Триггеры и логические сущности |
| [`message`](../39-entity-keys-reference/03-trigger-logic-keys.md#message) | `тип значения: string` | Триггеры и логические сущности |
| [`sounds`](../39-entity-keys-reference/03-trigger-logic-keys.md#sounds) | `тип значения: integer` | Триггеры и логические сущности |
| [`noise`](../39-entity-keys-reference/03-trigger-logic-keys.md#noise) | `тип значения: string` | Триггеры и логические сущности |
| [`wait`](../39-entity-keys-reference/03-trigger-logic-keys.md#wait) | `тип значения: float` | Триггеры и логические сущности |
| [`delay`](../39-entity-keys-reference/03-trigger-logic-keys.md#delay) | `тип значения: float` | Триггеры и логические сущности |
| [`health`](../39-entity-keys-reference/03-trigger-logic-keys.md#health) | `тип значения: float` | Триггеры и логические сущности |
| [`count`](../39-entity-keys-reference/03-trigger-logic-keys.md#count) | `тип значения: integer` | Триггеры и логические сущности |
| [`dmg`](../39-entity-keys-reference/03-trigger-logic-keys.md#dmg) | `тип значения: float` | Триггеры и логические сущности |
| [`speed`](../39-entity-keys-reference/03-trigger-logic-keys.md#speed) | `тип значения: float` | Триггеры и логические сущности |
| [`height`](../39-entity-keys-reference/03-trigger-logic-keys.md#height) | `тип значения: float` | Триггеры и логические сущности |
| [`map`](../39-entity-keys-reference/03-trigger-logic-keys.md#map) | `тип значения: string` | Триггеры и логические сущности |
| [`angle`](../39-entity-keys-reference/03-trigger-logic-keys.md#angle) | `тип значения: float` | Триггеры и логические сущности |
| [`angles`](../39-entity-keys-reference/03-trigger-logic-keys.md#angles) | `тип значения: vector` | Триггеры и логические сущности |
| [`origin`](../39-entity-keys-reference/03-trigger-logic-keys.md#origin) | `тип значения: vector` | Триггеры и логические сущности |
| [`model`](../39-entity-keys-reference/03-trigger-logic-keys.md#model) | `тип значения: string` | Триггеры и логические сущности |
| [`spawnflags`](../39-entity-keys-reference/03-trigger-logic-keys.md#spawnflags) | `тип значения: integer` | Триггеры и логические сущности |
| [`spawnflags`](../39-entity-keys-reference/03-trigger-logic-keys.md#spawnflags-notouch) | `тип значения: integer` | Триггеры и логические сущности |
| [`spawnflags`](../39-entity-keys-reference/03-trigger-logic-keys.md#spawnflags-nomessage) | `тип значения: integer` | Триггеры и логические сущности |
| [`spawnflags`](../39-entity-keys-reference/03-trigger-logic-keys.md#spawnflags-player_only) | `тип значения: integer` | Триггеры и логические сущности |
| [`spawnflags`](../39-entity-keys-reference/03-trigger-logic-keys.md#spawnflags-silent) | `тип значения: integer` | Триггеры и логические сущности |
| [`spawnflags`](../39-entity-keys-reference/03-trigger-logic-keys.md#spawnflags-push_once) | `тип значения: integer` | Триггеры и логические сущности |
| [`spawnflags`](../39-entity-keys-reference/03-trigger-logic-keys.md#spawnflags-no_intermission) | `тип значения: integer` | Триггеры и логические сущности |
| [`path_corner`](../39-entity-keys-reference/03-trigger-logic-keys.md#path_corner) | `тип значения: string` | Триггеры и логические сущности |
| [`info_notnull`](../39-entity-keys-reference/03-trigger-logic-keys.md#info_notnull) | `тип значения: string` | Триггеры и логические сущности |
| [`info_null`](../39-entity-keys-reference/03-trigger-logic-keys.md#info_null) | `тип значения: string` | Триггеры и логические сущности |
| [`classname`](../39-entity-keys-reference/04-func-brush-entity-keys.md#classname) | `тип значения: string` | Двери, платформы и подвижная геометрия |
| [`targetname`](../39-entity-keys-reference/04-func-brush-entity-keys.md#targetname) | `тип значения: string` | Двери, платформы и подвижная геометрия |
| [`target`](../39-entity-keys-reference/04-func-brush-entity-keys.md#target) | `тип значения: string` | Двери, платформы и подвижная геометрия |
| [`message`](../39-entity-keys-reference/04-func-brush-entity-keys.md#message) | `тип значения: string` | Двери, платформы и подвижная геометрия |
| [`killtarget`](../39-entity-keys-reference/04-func-brush-entity-keys.md#killtarget) | `тип значения: string` | Двери, платформы и подвижная геометрия |
| [`delay`](../39-entity-keys-reference/04-func-brush-entity-keys.md#delay) | `тип значения: float` | Двери, платформы и подвижная геометрия |
| [`angle`](../39-entity-keys-reference/04-func-brush-entity-keys.md#angle) | `тип значения: float` | Двери, платформы и подвижная геометрия |
| [`angles`](../39-entity-keys-reference/04-func-brush-entity-keys.md#angles) | `тип значения: vector` | Двери, платформы и подвижная геометрия |
| [`speed`](../39-entity-keys-reference/04-func-brush-entity-keys.md#speed) | `тип значения: float` | Двери, платформы и подвижная геометрия |
| [`wait`](../39-entity-keys-reference/04-func-brush-entity-keys.md#wait) | `тип значения: float` | Двери, платформы и подвижная геометрия |
| [`lip`](../39-entity-keys-reference/04-func-brush-entity-keys.md#lip) | `тип значения: float` | Двери, платформы и подвижная геометрия |
| [`dmg`](../39-entity-keys-reference/04-func-brush-entity-keys.md#dmg) | `тип значения: float` | Двери, платформы и подвижная геометрия |
| [`sounds`](../39-entity-keys-reference/04-func-brush-entity-keys.md#sounds) | `тип значения: integer` | Двери, платформы и подвижная геометрия |
| [`health`](../39-entity-keys-reference/04-func-brush-entity-keys.md#health) | `тип значения: float` | Двери, платформы и подвижная геометрия |
| [`height`](../39-entity-keys-reference/04-func-brush-entity-keys.md#height) | `тип значения: float` | Двери, платформы и подвижная геометрия |
| [`t_width`](../39-entity-keys-reference/04-func-brush-entity-keys.md#t_width) | `тип значения: float` | Двери, платформы и подвижная геометрия |
| [`t_length`](../39-entity-keys-reference/04-func-brush-entity-keys.md#t_length) | `тип значения: float` | Двери, платформы и подвижная геометрия |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-start_open) | `тип значения: integer` | Двери, платформы и подвижная геометрия |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-door_dont_link) | `тип значения: integer` | Двери, платформы и подвижная геометрия |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-gold_key) | `тип значения: integer` | Двери, платформы и подвижная геометрия |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-silver_key) | `тип значения: integer` | Двери, платформы и подвижная геометрия |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-toggle) | `тип значения: integer` | Двери, платформы и подвижная геометрия |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-open_once) | `тип значения: integer` | Двери, платформы и подвижная геометрия |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-1st_left) | `тип значения: integer` | Двери, платформы и подвижная геометрия |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-1st_down) | `тип значения: integer` | Двери, платформы и подвижная геометрия |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-no_shoot) | `тип значения: integer` | Двери, платформы и подвижная геометрия |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-always_shoot) | `тип значения: integer` | Двери, платформы и подвижная геометрия |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-plat_low_trigger) | `тип значения: integer` | Двери, платформы и подвижная геометрия |
| [`classname`](../39-entity-keys-reference/05-monster-player-keys.md#classname) | `тип значения: string` | Монстры, NPC и точки появления игрока |
| [`origin`](../39-entity-keys-reference/05-monster-player-keys.md#origin) | `тип значения: vector` | Монстры, NPC и точки появления игрока |
| [`angle`](../39-entity-keys-reference/05-monster-player-keys.md#angle) | `тип значения: float` | Монстры, NPC и точки появления игрока |
| [`angles`](../39-entity-keys-reference/05-monster-player-keys.md#angles) | `тип значения: vector` | Монстры, NPC и точки появления игрока |
| [`health`](../39-entity-keys-reference/05-monster-player-keys.md#health) | `тип значения: float` | Монстры, NPC и точки появления игрока |
| [`spawnflags`](../39-entity-keys-reference/05-monster-player-keys.md#spawnflags) | `тип значения: integer` | Монстры, NPC и точки появления игрока |
| [`target`](../39-entity-keys-reference/05-monster-player-keys.md#target) | `тип значения: string` | Монстры, NPC и точки появления игрока |
| [`targetname`](../39-entity-keys-reference/05-monster-player-keys.md#targetname) | `тип значения: string` | Монстры, NPC и точки появления игрока |
| [`yaw_speed`](../39-entity-keys-reference/05-monster-player-keys.md#yaw_speed) | `тип значения: float` | Монстры, NPC и точки появления игрока |
| [`items`](../39-entity-keys-reference/05-monster-player-keys.md#items) | `тип значения: integer` | Монстры, NPC и точки появления игрока |
| [`model`](../39-entity-keys-reference/05-monster-player-keys.md#model) | `тип значения: string` | Монстры, NPC и точки появления игрока |
| [`classname`](../39-entity-keys-reference/05-monster-player-keys.md#classname) | `тип значения: string` | Монстры, NPC и точки появления игрока |
| [`origin`](../39-entity-keys-reference/05-monster-player-keys.md#origin) | `тип значения: vector` | Монстры, NPC и точки появления игрока |
| [`angle`](../39-entity-keys-reference/05-monster-player-keys.md#angle) | `тип значения: float` | Монстры, NPC и точки появления игрока |
| [`angles`](../39-entity-keys-reference/05-monster-player-keys.md#angles) | `тип значения: vector` | Монстры, NPC и точки появления игрока |
| [`target`](../39-entity-keys-reference/05-monster-player-keys.md#target) | `тип значения: string` | Монстры, NPC и точки появления игрока |
| [`targetname`](../39-entity-keys-reference/05-monster-player-keys.md#targetname) | `тип значения: string` | Монстры, NPC и точки появления игрока |
| [`spawnflags`](../39-entity-keys-reference/05-monster-player-keys.md#spawnflags) | `тип значения: integer` | Монстры, NPC и точки появления игрока |
| [`mangle`](../39-entity-keys-reference/05-monster-player-keys.md#mangle) | `тип значения: vector` | Монстры, NPC и точки появления игрока |
| [`health`](../39-entity-keys-reference/05-monster-player-keys.md#health) | `тип значения: float` | Монстры, NPC и точки появления игрока |
| [`model`](../39-entity-keys-reference/05-monster-player-keys.md#model) | `тип значения: string` | Монстры, NPC и точки появления игрока |
| [`classname`](../39-entity-keys-reference/06-item-weapon-keys.md#classname) | `тип значения: string` | Предметы и оружие |
| [`origin`](../39-entity-keys-reference/06-item-weapon-keys.md#origin) | `тип значения: vector` | Предметы и оружие |
| [`angle`](../39-entity-keys-reference/06-item-weapon-keys.md#angle) | `тип значения: float` | Предметы и оружие |
| [`angles`](../39-entity-keys-reference/06-item-weapon-keys.md#angles) | `тип значения: vector` | Предметы и оружие |
| [`spawnflags`](../39-entity-keys-reference/06-item-weapon-keys.md#spawnflags) | `тип значения: integer` | Предметы и оружие |
| [`target`](../39-entity-keys-reference/06-item-weapon-keys.md#target) | `тип значения: string` | Предметы и оружие |
| [`killtarget`](../39-entity-keys-reference/06-item-weapon-keys.md#killtarget) | `тип значения: string` | Предметы и оружие |
| [`delay`](../39-entity-keys-reference/06-item-weapon-keys.md#delay) | `тип значения: float` | Предметы и оружие |
| [`message`](../39-entity-keys-reference/06-item-weapon-keys.md#message) | `тип значения: string` | Предметы и оружие |
| [`targetname`](../39-entity-keys-reference/06-item-weapon-keys.md#targetname) | `тип значения: string` | Предметы и оружие |
| [`wait`](../39-entity-keys-reference/06-item-weapon-keys.md#wait) | `тип значения: float` | Предметы и оружие |
| [`count`](../39-entity-keys-reference/06-item-weapon-keys.md#count) | `тип значения: float` | Предметы и оружие |
| [`effects`](../39-entity-keys-reference/06-item-weapon-keys.md#effects) | `тип значения: integer` | Предметы и оружие |

## Директивы языка материалов (.shader)

Всего задокументировано: **75** директив. Полный постатейный разбор — в разделе [«40. Директивы языка материалов»](../40-shader-directives-reference/README.md).

| Функция | Сигнатура | Категория |
|---|---|---|
| [`cull`](../40-shader-directives-reference/01-shader-toplevel-directives.md#cull) | `cull disable\|none\|twosided\|front\|back\|backside\|backsided` | Директивы уровня материала |
| [`skyparms`](../40-shader-directives-reference/01-shader-toplevel-directives.md#skyparms) | `skyparms farbox height nearbox` | Директивы уровня материала |
| [`fogparms`](../40-shader-directives-reference/01-shader-toplevel-directives.md#fogparms) | `fogparms (r g b) depth` | Директивы уровня материала |
| [`surfaceparm`](../40-shader-directives-reference/01-shader-toplevel-directives.md#surfaceparm) | `surfaceparm keyword` | Директивы уровня материала |
| [`nomipmaps`](../40-shader-directives-reference/01-shader-toplevel-directives.md#nomipmaps) | `nomipmaps` | Директивы уровня материала |
| [`nopicmip`](../40-shader-directives-reference/01-shader-toplevel-directives.md#nopicmip) | `nopicmip` | Директивы уровня материала |
| [`polygonoffset`](../40-shader-directives-reference/01-shader-toplevel-directives.md#polygonoffset) | `polygonoffset [scale]` | Директивы уровня материала |
| [`sort`](../40-shader-directives-reference/01-shader-toplevel-directives.md#sort) | `sort portal\|sky\|opaque\|decal\|litdecal\|seethrough\|unlitdecal\|banner\|underwater\|blend\|additive\|nearest\|ripple\|deferredlight\|number` | Директивы уровня материала |
| [`deformvertexes`](../40-shader-directives-reference/01-shader-toplevel-directives.md#deformvertexes) | `deformvertexes type ...` | Директивы уровня материала |
| [`portal`](../40-shader-directives-reference/01-shader-toplevel-directives.md#portal) | `portal` | Директивы уровня материала |
| [`entitymergable`](../40-shader-directives-reference/01-shader-toplevel-directives.md#entitymergable) | `entitymergable` | Директивы уровня материала |
| [`clutter`](../40-shader-directives-reference/01-shader-toplevel-directives.md#clutter) | `clutter model spacing scalemin scalemax zofs anglemin anglemax` | Директивы уровня материала |
| [`deferredlight`](../40-shader-directives-reference/01-shader-toplevel-directives.md#deferredlight) | `deferredlight` | Директивы уровня материала |
| [`affine`](../40-shader-directives-reference/01-shader-toplevel-directives.md#affine) | `affine` | Директивы уровня материала |
| [`fullrate`](../40-shader-directives-reference/01-shader-toplevel-directives.md#fullrate) | `fullrate` | Директивы уровня материала |
| [`diffusemap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#diffusemap) | `diffusemap path` | Директивы уровня материала |
| [`normalmap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#normalmap) | `normalmap path` | Директивы уровня материала |
| [`specularmap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#specularmap) | `specularmap path` | Директивы уровня материала |
| [`fullbrightmap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#fullbrightmap) | `fullbrightmap path` | Директивы уровня материала |
| [`uppermap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#uppermap) | `uppermap path` | Директивы уровня материала |
| [`lowermap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#lowermap) | `lowermap path` | Директивы уровня материала |
| [`reflectmask`](../40-shader-directives-reference/01-shader-toplevel-directives.md#reflectmask) | `reflectmask path` | Директивы уровня материала |
| [`displacementmap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#displacementmap) | `displacementmap path` | Директивы уровня материала |
| [`transmissionmap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#transmissionmap) | `transmissionmap path` | Директивы уровня материала |
| [`thicknessmap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#thicknessmap) | `thicknessmap path` | Директивы уровня материала |
| [`program`](../40-shader-directives-reference/01-shader-toplevel-directives.md#program) | `program name` | Директивы уровня материала |
| [`glslprogram`](../40-shader-directives-reference/01-shader-toplevel-directives.md#glslprogram) | `glslprogram name` | Директивы уровня материала |
| [`hlslprogram`](../40-shader-directives-reference/01-shader-toplevel-directives.md#hlslprogram) | `hlslprogram name` | Директивы уровня материала |
| [`hlsl11program`](../40-shader-directives-reference/01-shader-toplevel-directives.md#hlsl11program) | `hlsl11program name` | Директивы уровня материала |
| [`portalfboscale`](../40-shader-directives-reference/01-shader-toplevel-directives.md#portalfboscale) | `portalfboscale scale` | Директивы уровня материала |
| [`map`](../40-shader-directives-reference/02-shader-stage-directives.md#map) | `map <textureOrSpecial>` | Директивы уровня стадии |
| [`animmap`](../40-shader-directives-reference/02-shader-stage-directives.md#animmap) | `animmap <fps> <frame1> <frame2> ...` | Директивы уровня стадии |
| [`clampmap`](../40-shader-directives-reference/02-shader-stage-directives.md#clampmap) | `clampmap <textureOrSpecial>` | Директивы уровня стадии |
| [`videoMap`](../40-shader-directives-reference/02-shader-stage-directives.md#videomap) | `videoMap <videoFile>` | Директивы уровня стадии |
| [`cubemap`](../40-shader-directives-reference/02-shader-stage-directives.md#cubemap) | `cubemap <cubeTexture>` | Директивы уровня стадии |
| [`cameracubemap`](../40-shader-directives-reference/02-shader-stage-directives.md#cameracubemap) | `cameracubemap <cubeTexture>` | Директивы уровня стадии |
| [`surroundmap`](../40-shader-directives-reference/02-shader-stage-directives.md#surroundmap) | `surroundmap <cubeTexture>` | Директивы уровня стадии |
| [`blendfunc`](../40-shader-directives-reference/02-shader-stage-directives.md#blendfunc) | `blendfunc <preset>` | Директивы уровня стадии |
| [`blend`](../40-shader-directives-reference/02-shader-stage-directives.md#blend) | `blend <presetOrFactors>` | Директивы уровня стадии |
| [`rgbGen`](../40-shader-directives-reference/02-shader-stage-directives.md#rgbgen) | `rgbGen <mode> [args...]` | Директивы уровня стадии |
| [`alphaGen`](../40-shader-directives-reference/02-shader-stage-directives.md#alphagen) | `alphaGen <mode> [args...]` | Директивы уровня стадии |
| [`alphaShift`](../40-shader-directives-reference/02-shader-stage-directives.md#alphashift) | `alphaShift <speed> <min> <max>` | Директивы уровня стадии |
| [`depthfunc`](../40-shader-directives-reference/02-shader-stage-directives.md#depthfunc) | `depthfunc <mode>` | Директивы уровня стадии |
| [`depthwrite`](../40-shader-directives-reference/02-shader-stage-directives.md#depthwrite) | `depthwrite` | Директивы уровня стадии |
| [`nodepthtest`](../40-shader-directives-reference/02-shader-stage-directives.md#nodepthtest) | `nodepthtest` | Директивы уровня стадии |
| [`nodepth`](../40-shader-directives-reference/02-shader-stage-directives.md#nodepth) | `nodepth` | Директивы уровня стадии |
| [`alphafunc`](../40-shader-directives-reference/02-shader-stage-directives.md#alphafunc) | `alphafunc <mode>` | Директивы уровня стадии |
| [`alphaMask`](../40-shader-directives-reference/02-shader-stage-directives.md#alphamask) | `alphaMask` | Директивы уровня стадии |
| [`alphaTest`](../40-shader-directives-reference/02-shader-stage-directives.md#alphatest) | `alphaTest 0.5` | Директивы уровня стадии |
| [`tcMod`](../40-shader-directives-reference/02-shader-stage-directives.md#tcmod) | `tcMod <mode> [args...]` | Директивы уровня стадии |
| [`scale`](../40-shader-directives-reference/02-shader-stage-directives.md#scale) | `scale <x> <y>` | Директивы уровня стадии |
| [`scroll`](../40-shader-directives-reference/02-shader-stage-directives.md#scroll) | `scroll static <x> static <y>` | Директивы уровня стадии |
| [`tcGen`](../40-shader-directives-reference/02-shader-stage-directives.md#tcgen) | `tcGen <mode> [args...]` | Директивы уровня стадии |
| [`texgen`](../40-shader-directives-reference/02-shader-stage-directives.md#texgen) | `texgen <mode> [args...]` | Директивы уровня стадии |
| [`envmap`](../40-shader-directives-reference/02-shader-stage-directives.md#envmap) | `envmap` | Директивы уровня стадии |
| [`detail`](../40-shader-directives-reference/02-shader-stage-directives.md#detail) | `detail` | Директивы уровня стадии |
| [`nolightmap`](../40-shader-directives-reference/02-shader-stage-directives.md#nolightmap) | `nolightmap` | Директивы уровня стадии |
| [`program`](../40-shader-directives-reference/02-shader-stage-directives.md#program) | `program <programName>` | Директивы уровня стадии |
| [`maskcolor`](../40-shader-directives-reference/02-shader-stage-directives.md#maskcolor) | `maskcolor` | Директивы уровня стадии |
| [`maskred`](../40-shader-directives-reference/02-shader-stage-directives.md#maskred) | `maskred` | Директивы уровня стадии |
| [`maskgreen`](../40-shader-directives-reference/02-shader-stage-directives.md#maskgreen) | `maskgreen` | Директивы уровня стадии |
| [`maskblue`](../40-shader-directives-reference/02-shader-stage-directives.md#maskblue) | `maskblue` | Директивы уровня стадии |
| [`maskalpha`](../40-shader-directives-reference/02-shader-stage-directives.md#maskalpha) | `maskalpha` | Директивы уровня стадии |
| [`red`](../40-shader-directives-reference/02-shader-stage-directives.md#red) | `red <value>` | Директивы уровня стадии |
| [`green`](../40-shader-directives-reference/02-shader-stage-directives.md#green) | `green <value>` | Директивы уровня стадии |
| [`blue`](../40-shader-directives-reference/02-shader-stage-directives.md#blue) | `blue <value>` | Директивы уровня стадии |
| [`alpha`](../40-shader-directives-reference/02-shader-stage-directives.md#alpha) | `alpha <value>` | Директивы уровня стадии |
| [`map16`](../40-shader-directives-reference/02-shader-stage-directives.md#map16) | `map16 <textureOrSpecial>` | Директивы уровня стадии |
| [`map32`](../40-shader-directives-reference/02-shader-stage-directives.md#map32) | `map32 <textureOrSpecial>` | Директивы уровня стадии |
| [`mapcomp`](../40-shader-directives-reference/02-shader-stage-directives.md#mapcomp) | `mapcomp <textureOrSpecial>` | Директивы уровня стадии |
| [`mapnocomp`](../40-shader-directives-reference/02-shader-stage-directives.md#mapnocomp) | `mapnocomp <textureOrSpecial>` | Директивы уровня стадии |
| [`animcompmap`](../40-shader-directives-reference/02-shader-stage-directives.md#animcompmap) | `animcompmap <fps> <frame1> <frame2> ...` | Директивы уровня стадии |
| [`animnocompmap`](../40-shader-directives-reference/02-shader-stage-directives.md#animnocompmap) | `animnocompmap <fps> <frame1> <frame2> ...` | Директивы уровня стадии |
| [`animclampmap`](../40-shader-directives-reference/02-shader-stage-directives.md#animclampmap) | `animclampmap <fps> <frame1> <frame2> ...` | Директивы уровня стадии |
| [`material`](../40-shader-directives-reference/02-shader-stage-directives.md#material) | `material <baseTexture> <normalMap> <specularMap>` | Директивы уровня стадии |

## Директивы языка частиц (.particles)

Всего задокументировано: **76** директив. Полный постатейный разбор — в разделе [«41. Директивы языка частиц»](../41-particle-directives-reference/README.md).

| Функция | Сигнатура | Категория |
|---|---|---|
| [`shader`](../41-particle-directives-reference/01-particle-effect-directives.md#shader) | `shader [shaderName]` | Директивы эффекта |
| [`texture`](../41-particle-directives-reference/01-particle-effect-directives.md#texture) | `texture path` | Директивы эффекта |
| [`tcoords`](../41-particle-directives-reference/01-particle-effect-directives.md#tcoords) | `tcoords s1 t1 s2 t2 [tscale] [rsmax] [rsstep]` | Директивы эффекта |
| [`atlas`](../41-particle-directives-reference/01-particle-effect-directives.md#atlas) | `atlas dims firstIndex [lastIndex]` | Директивы эффекта |
| [`rotation`](../41-particle-directives-reference/01-particle-effect-directives.md#rotation) | `rotation startMin [startMax] speedMin [speedMax]` | Директивы эффекта |
| [`beamtexstep`](../41-particle-directives-reference/01-particle-effect-directives.md#beamtexstep) | `beamtexstep unitsPerRepeat` | Директивы эффекта |
| [`beamtexspeed`](../41-particle-directives-reference/01-particle-effect-directives.md#beamtexspeed) | `beamtexspeed scrollSpeed` | Директивы эффекта |
| [`scale`](../41-particle-directives-reference/01-particle-effect-directives.md#scale) | `scale minSize [maxSize]` | Директивы эффекта |
| [`scalefactor`](../41-particle-directives-reference/01-particle-effect-directives.md#scalefactor) | `scalefactor factor` | Директивы эффекта |
| [`scaledelta`](../41-particle-directives-reference/01-particle-effect-directives.md#scaledelta) | `scaledelta unitsPerSecond` | Директивы эффекта |
| [`stretchfactor`](../41-particle-directives-reference/01-particle-effect-directives.md#stretchfactor) | `stretchfactor factor [minFactor]` | Директивы эффекта |
| [`count`](../41-particle-directives-reference/01-particle-effect-directives.md#count) | `count baseCount [randCount] [absoluteExtra]` | Директивы эффекта |
| [`alpha`](../41-particle-directives-reference/01-particle-effect-directives.md#alpha) | `alpha baseAlpha [maxAlpha] [delta]` | Директивы эффекта |
| [`alpharand`](../41-particle-directives-reference/01-particle-effect-directives.md#alpharand) | `alpharand range` | Директивы эффекта |
| [`alphadelta`](../41-particle-directives-reference/01-particle-effect-directives.md#alphadelta) | `alphadelta unitsPerSecond` | Директивы эффекта |
| [`die`](../41-particle-directives-reference/01-particle-effect-directives.md#die) | `die maxTime [minTime]` | Директивы эффекта |
| [`assoc`](../41-particle-directives-reference/01-particle-effect-directives.md#assoc) | `assoc effectName` | Директивы эффекта |
| [`colorindex`](../41-particle-directives-reference/01-particle-effect-directives.md#colorindex) | `colorindex paletteIndex [range]` | Директивы эффекта |
| [`rgb`](../41-particle-directives-reference/01-particle-effect-directives.md#rgb) | `rgb r [g b]` | Директивы эффекта |
| [`rgbdelta`](../41-particle-directives-reference/01-particle-effect-directives.md#rgbdelta) | `rgbdelta rDelta [gDelta bDelta]` | Директивы эффекта |
| [`rgbrand`](../41-particle-directives-reference/01-particle-effect-directives.md#rgbrand) | `rgbrand rRange [gRange bRange]` | Директивы эффекта |
| [`rgbrandsync`](../41-particle-directives-reference/01-particle-effect-directives.md#rgbrandsync) | `rgbrandsync rSync [gSync bSync]` | Директивы эффекта |
| [`stains`](../41-particle-directives-reference/01-particle-effect-directives.md#stains) | `stains amount` | Директивы эффекта |
| [`blend`](../41-particle-directives-reference/01-particle-effect-directives.md#blend) | `blend mode` | Директивы эффекта |
| [`type`](../41-particle-directives-reference/01-particle-effect-directives.md#type) | `type renderType` | Директивы эффекта |
| [`clippeddecal`](../41-particle-directives-reference/01-particle-effect-directives.md#clippeddecal) | `clippeddecal mask [match]` | Директивы эффекта |
| [`cliptype`](../41-particle-directives-reference/01-particle-effect-directives.md#cliptype) | `cliptype effectName` | Директивы эффекта |
| [`rampmode`](../41-particle-directives-reference/01-particle-effect-directives.md#rampmode) | `rampmode mode` | Директивы эффекта |
| [`rampindex`](../41-particle-directives-reference/01-particle-effect-directives.md#rampindex) | `rampindex paletteIndex [alpha] [scale]` | Директивы эффекта |
| [`ramp`](../41-particle-directives-reference/01-particle-effect-directives.md#ramp) | `ramp r [g b [alpha [scale]]]` | Директивы эффекта |
| [`lightradius`](../41-particle-directives-reference/01-particle-effect-directives.md#lightradius) | `lightradius minRadius [maxRadius]` | Директивы эффекта |
| [`lightradiusfade`](../41-particle-directives-reference/01-particle-effect-directives.md#lightradiusfade) | `lightradiusfade unitsPerSecond` | Директивы эффекта |
| [`lightrgb`](../41-particle-directives-reference/01-particle-effect-directives.md#lightrgb) | `lightrgb r g b` | Директивы эффекта |
| [`lightcorona`](../41-particle-directives-reference/01-particle-effect-directives.md#lightcorona) | `lightcorona intensity scale` | Директивы эффекта |
| [`lighttime`](../41-particle-directives-reference/01-particle-effect-directives.md#lighttime) | `lighttime seconds` | Директивы эффекта |
| [`spawnstain`](../41-particle-directives-reference/01-particle-effect-directives.md#spawnstain) | `spawnstain radius r g b` | Директивы эффекта |
| [`randomvel`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#randomvel) | `randomvel horizontal [vertical]` | Директивы поведения и появления |
| [`veladd`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#veladd) | `veladd base [max]` | Директивы поведения и появления |
| [`orgadd`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#orgadd) | `orgadd base [max]` | Директивы поведения и появления |
| [`orgbias`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#orgbias) | `orgbias x y z` | Директивы поведения и появления |
| [`velbias`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#velbias) | `velbias x y z` | Директивы поведения и появления |
| [`orgwrand`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#orgwrand) | `orgwrand x y z` | Директивы поведения и появления |
| [`velwrand`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#velwrand) | `velwrand x y z` | Директивы поведения и появления |
| [`friction`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#friction) | `friction xyz` | Директивы поведения и появления |
| [`gravity`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#gravity) | `gravity value` | Директивы поведения и появления |
| [`flurry`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#flurry) | `flurry value` | Директивы поведения и появления |
| [`assoc`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#assoc) | `assoc effectName` | Директивы поведения и появления |
| [`inwater`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#inwater) | `inwater effectName` | Директивы поведения и появления |
| [`underwater`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#underwater) | `underwater [contents ...]` | Директивы поведения и появления |
| [`notunderwater`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#notunderwater) | `notunderwater [contents ...]` | Директивы поведения и появления |
| [`spawnmode`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#spawnmode) | `spawnmode mode [param1] [param2]` | Директивы поведения и появления |
| [`spawntime`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#spawntime) | `spawntime seconds` | Директивы поведения и появления |
| [`spawnchance`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#spawnchance) | `spawnchance chance` | Директивы поведения и появления |
| [`step`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#step) | `step distance [randomDistance] [extraCount]` | Директивы поведения и появления |
| [`cliptype`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#cliptype) | `cliptype effectName` | Директивы поведения и появления |
| [`clipcount`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#clipcount) | `clipcount multiplier` | Директивы поведения и появления |
| [`clipbounce`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#clipbounce) | `clipbounce value` | Директивы поведения и появления |
| [`bounce`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#bounce) | `bounce value` | Директивы поведения и появления |
| [`emit`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#emit) | `emit effectName` | Директивы поведения и появления |
| [`emitinterval`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#emitinterval) | `emitinterval seconds` | Директивы поведения и появления |
| [`emitintervalrand`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#emitintervalrand) | `emitintervalrand seconds` | Директивы поведения и появления |
| [`emitstart`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#emitstart) | `emitstart seconds` | Директивы поведения и появления |
| [`spawnorg`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#spawnorg) | `spawnorg horizontal [vertical]` | Директивы поведения и появления |
| [`spawnvel`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#spawnvel) | `spawnvel horizontal [vertical]` | Директивы поведения и появления |
| [`stretchfactor`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#stretchfactor) | `stretchfactor factor [minLength]` | Директивы поведения и появления |
| [`spawnparam1`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#spawnparam1) | `spawnparam1 value` | Директивы поведения и появления |
| [`spawnparam2`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#spawnparam2) | `spawnparam2 value` | Директивы поведения и появления |
| [`up`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#up) | `up value` | Директивы поведения и появления |
| [`viewspace`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#viewspace) | `viewspace [fraction]` | Директивы поведения и появления |
| [`perframe`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#perframe) | `perframe` | Директивы поведения и появления |
| [`averageout`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#averageout) | `averageout` | Директивы поведения и появления |
| [`nostate`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#nostate) | `nostate` | Директивы поведения и появления |
| [`nospreadfirst`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#nospreadfirst) | `nospreadfirst` | Директивы поведения и появления |
| [`nospreadlast`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#nospreadlast) | `nospreadlast` | Директивы поведения и появления |
| [`rainfrequency`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#rainfrequency) | `rainfrequency multiplier` | Директивы поведения и появления |
| [`placeholder`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#placeholder) | `placeholder` | Директивы поведения и появления |
