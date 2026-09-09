# Быстрая навигация по API (сводные таблицы)

> [⬅ Предыдущая страница](../36-mobile-web-platforms/run-in-browser-webgl.md) | [Следующая страница ➡](../37-quakec-builtins-reference/00-entry-points.md)

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
- [Параметры командной строки](#параметры-командной-строки)
  - [FTEQW (движок)](#fteqw-движок)
  - [FTEQCC (компилятор)](#fteqcc-компилятор)
- [Команды консоли](#команды-консоли)
  - [Клиент и интерфейс](#клиент-и-интерфейс)
  - [Рендер и звук](#рендер-и-звук)
  - [Сервер и мультиплеер](#сервер-и-мультиплеер)
  - [Файловая система и системные](#файловая-система-и-системные)
  - [Опциональные плагины](#опциональные-плагины)

---

## Встроенные функции QuakeC (builtins)

Всего задокументировано: **744** builtin-функций и точек входа (включая точки входа SSQC/CSQC/MenuQC и отдельно посчитанные CSQC- и MenuQC-варианты одноимённых функций). Полный постатейный разбор — в разделе [«37. Встроенные функции QuakeC»](../README.md#встроенные-функции-quakec-builtins).


### Точки входа QuakeC: SSQC

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`SetNewParms`](../37-quakec-builtins-reference/00-entry-points.md#setnewparms) | Вызывается движком ровно один раз для каждого клиента — в момент его первого подключения к серверу, причём даже раньше, чем `ClientConnect`. | `void() SetNewParms` |
| [`SetChangeParms`](../37-quakec-builtins-reference/00-entry-points.md#setchangeparms) | Вызывается для каждого клиента при переходе на следующий уровень (обычная смена карты по триггеру выхода, а не по [`changelevel`](../37-quakec-builtins-reference/03-entity-world-builtins.md#changelevel) с явным сбросом). | `void() SetChangeParms` |
| [`ClientConnect`](../37-quakec-builtins-reference/00-entry-points.md#clientconnect) | Вызывается один раз, когда подключающийся игрок полностью загрузился (принял все прекэши ресурсов) и готов получать активные объекты игрового мира. | `void() ClientConnect` |
| [`PutClientInServer`](../37-quakec-builtins-reference/00-entry-points.md#putclientinserver) | Со стороны движка эта функция вызывается сразу после `ClientConnect` — на этом этапе разница между ними чисто условная (по историческим причинам из classic Quake). | `void() PutClientInServer` |
| [`ClientKill`](../37-quakec-builtins-reference/00-entry-points.md#clientkill) | Вызывается в ответ на игровую консольную команду `kill` (игрок сам решил покончить с собой, например застряв в геометрии карты). | `void() ClientKill` |
| [`PlayerPreThink`](../37-quakec-builtins-reference/00-entry-points.md#playerprethink) | Вызывается каждый кадр физики сервера для каждого активного игрока — раньше, чем движок обработает ввод этого игрока (нажатия клавиш, движение мыши). | `void() PlayerPreThink` |
| [`PlayerPostThink`](../37-quakec-builtins-reference/00-entry-points.md#playerpostthink) | Вызывается каждый кадр физики сервера для каждого активного игрока, уже после того как обработаны команды ввода и применено перемещение. | `void() PlayerPostThink` |
| [`StartFrame`](../37-quakec-builtins-reference/00-entry-points.md#startframe) | Вызывается один раз за кадр физики сервера — до того, как обрабатываются `think`-функции каких-либо игровых объектов. | `void() StartFrame` |
| [`EndFrame`](../37-quakec-builtins-reference/00-entry-points.md#endframe) | Менее известная точка входа, вызываемая после того, как отработали `think`-функции всех не-игровых объектов в конце кадра физики. | `void() EndFrame` |
| [`ClientDisconnect`](../37-quakec-builtins-reference/00-entry-points.md#clientdisconnect) | Вызывается, когда игрок отключается от сервера штатно (закрыл игру, набрал [`disconnect`](../44-cli-commands-reference/02-client-ui-commands.md#disconnect)) или отвалился по таймауту связи. | `void() ClientDisconnect` |
| [`main`](../37-quakec-builtins-reference/00-entry-points.md#main-устаревшая-не-вызывается) | Историческое наследие ранних версий Quake: в текущем движке FTEQW эта функция объявлена в перечне известных имён, но **фактически никогда не вызывается** — это мёртвый код. Она сохранена в списке распознаваемых имён исключительно для полноты и обратной совместимости со старыми прогс-файлами, где она могла присутствовать по инерции из ранних экспериментальных версий движка id Software. Описывать её в собственном моде не нужно и не имеет никакого практического эффекта — используйте `SetNewParms`/`ClientConnect`/`StartFrame` для соответствующей инициализационной логики. | `void() main` |

### Точки входа QuakeC: CSQC

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`CSQC_Init`](../37-quakec-builtins-reference/00-entry-points.md#csqc_init) | Вызывается один раз при загрузке клиентской логики — это стартовая, самая первая точка входа CSQC, аналог конструктора всего модуля. | `void(float apilevel, string enginename, float engineversion) CSQC_Init` |
| [`CSQC_WorldLoaded`](../37-quakec-builtins-reference/00-entry-points.md#csqc_worldloaded) | Вызывается после того, как клиент завершил загрузку карты и обработал прекэши моделей и звуков, присланные сервером. | `void() CSQC_WorldLoaded` |
| [`CSQC_UpdateView`](../37-quakec-builtins-reference/00-entry-points.md#csqc_updateview) | Главная и самая часто вызываемая точка входа CSQC — вызывается каждый кадр рендера и целиком отвечает за то, что окажется нарисованным на экране: 3D-мир (через [`clearscene`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#clearscene)/[`addentities`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#addentities)/[`renderscene`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#renderscene)), HUD, таблицу результатов, элементы интерфейса. | `void(float vwidth, float vheight, float notmenu) CSQC_UpdateView` |
| [`CSQC_InputEvent`](../37-quakec-builtins-reference/00-entry-points.md#csqc_inputevent) | Позволяет логике интерфейса перехватывать и переопределять управление ещё до того, как оно будет обработано штатным образом (движение игрока, открытие консоли и т.п.). | `float(float evtype, float scanx, float chary, float devid) CSQC_InputEvent` |
| [`CSQC_ConsoleCommand`](../37-quakec-builtins-reference/00-entry-points.md#csqc_consolecommand) | Вызывается, когда игрок вводит в консоль команду, имя которой было заранее зарегистрировано через [`registercommand`](../37-quakec-builtins-reference/03-entity-world-builtins.md#registercommand). | `float(string cmd) CSQC_ConsoleCommand` |
| [`CSQC_Parse_StuffCmd`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_stuffcmd) | Даёт CSQC шанс перехватить команды, которые сервер обычно просто выполняет в консоли клиента напрямую (механизм [`stuffcmd`](../37-quakec-builtins-reference/04-network-messages-builtins.md#stuffcmd)). | `void(string msg) CSQC_Parse_StuffCmd` |
| [`CSQC_Parse_CenterPrint`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_centerprint) | Даёт CSQC шанс перехватить обычное центральное сообщение экрана ([`centerprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#centerprint)) и нарисовать его самостоятельно — например, другим шрифтом, с анимацией появления или в собственном стиле оформления мода. | `float(string msg) CSQC_Parse_CenterPrint` |
| [`CSQC_Parse_Print`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_print) | Даёт CSQC шанс перехватить обычные текстовые сообщения (чат, системные объявления), присланные сервером. | `void(string printmsg, float printlvl) CSQC_Parse_Print` |
| [`CSQC_Ent_Update`](../37-quakec-builtins-reference/00-entry-points.md#csqc_ent_update) | Ключевая точка входа для собственного клиентского сетевого протокола CSQC. | `void(float isnew) CSQC_Ent_Update` |
| [`CSQC_Event_Sound`](../37-quakec-builtins-reference/00-entry-points.md#csqc_event_sound) | Даёт CSQC шанс перехватить событие звука, которое сервер обычно проигрывает автоматически (обычные звуки выстрелов, шагов, эффектов). | `float(float entnum, float channel, string soundname, float vol, float attenuation, vector pos, float pitchmod, float flags) CSQC_Event_Sound` |
| [`CSQC_Ent_Remove`](../37-quakec-builtins-reference/00-entry-points.md#csqc_ent_remove) | Вызывается для конкретного клиентского объекта (доступного через `self`), когда соответствующая ему сетевая сущность была удалена или скрыта сервером (перестала попадать в поле зрения этого клиента). | `void() CSQC_Ent_Remove` |
| [`CSQC_Shutdown`](../37-quakec-builtins-reference/00-entry-points.md#csqc_shutdown) | Вызывается при штатной выгрузке клиентской логики — обычно при отключении от сервера или переподключении к другому серверу. | `void() CSQC_Shutdown` |
| [`CSQC_UpdateViewLoading`](../37-quakec-builtins-reference/00-entry-points.md#csqc_updateviewloading) | Вызывается вместо обычного `CSQC_UpdateView` в те кадры, когда карта ещё не полностью загружена и мир не готов к штатной отрисовке (идёт стриминг ресурсов, распаковка BSP, прекэш моделей и звуков). | `void(float vwidth, float vheight, float notmenu) CSQC_UpdateViewLoading` |
| [`CSQC_DrawHud`](../37-quakec-builtins-reference/00-entry-points.md#csqc_drawhud) | Упрощённая, «облегчённая» точка входа для минималистичных CSQC-модов, которым не нужен полный контроль над отрисовкой 3D-сцены: движок сам рисует мир и вид от первого лица штатным образом, а CSQC получает возможность нарисовать только HUD поверх готовой картинки. | `void(vector viewsize, float scoresshown) CSQC_DrawHud` |
| [`CSQC_DrawScores`](../37-quakec-builtins-reference/00-entry-points.md#csqc_drawscores) | Парная функция к `CSQC_DrawHud` в упрощённом режиме отрисовки: отвечает конкретно за таблицу результатов (список игроков и счёта), которая в полноценном режиме обычно рисуется прямо внутри `CSQC_UpdateView`. | `void(vector viewsize, float scoresshown) CSQC_DrawScores` |
| [`CSQC_Parse_Event`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_event) | Вызывается при получении сетевого сообщения `svc_csqcevent`/кастомного расширения событий, отправленного серверной стороной специально для CSQC, минуя стандартные `SVC_TempEntity`/`SVC_Print` и подобные. | `void() CSQC_Parse_Event` |
| [`CSQC_Parse_Damage`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_damage) | Вызывается, когда сервер сообщает клиенту, что локальный игрок получил урон (аналог стандартного экранного «покраснения» и рывка камеры ванильного Quake). | `float(float save, float take, vector inflictororg) CSQC_Parse_Damage` |
| [`CSQC_Parse_SetAngles`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_setangles) | Вызывается, когда сервер меняет угол обзора игрока принудительно (поле `.fixangle` серверной сущности игрока, например после телепорта или интро-камеры). | `float(vector angles, float isdelta) CSQC_Parse_SetAngles` |
| [`CSQC_PlayerInfoChanged`](../37-quakec-builtins-reference/00-entry-points.md#csqc_playerinfochanged) | Вызывается при изменении userinfo конкретного игрока — смена имени, скина, модели, команды или любого другого ключа, который сервер синхронизирует через userinfo. | `void(float playernum) CSQC_PlayerInfoChanged` |
| [`CSQC_ServerInfoChanged`](../37-quakec-builtins-reference/00-entry-points.md#csqc_serverinfochanged) | Вызывается при изменении serverinfo — например, при смене режима игры, лимита фрагов, названия карты в ротации или любого другого ключа, который сервер публикует через serverinfo. | `void() CSQC_ServerInfoChanged` |
| [`CSQC_Input_Frame`](../37-quakec-builtins-reference/00-entry-points.md#csqc_input_frame) | Вызывается каждый раз, когда движок опрашивает устройства ввода, отдельно от кадра отрисовки `CSQC_UpdateView` — частота вызова может отличаться от частоты кадров рендера (например, если рендер и обработка ввода разделены по времени для снижения задержки ввода). | `void() CSQC_Input_Frame` |
| [`CSQC_RendererRestarted`](../37-quakec-builtins-reference/00-entry-points.md#csqc_rendererrestarted) | Вызывается после смены видеорежима или полного перезапуска рендерера ([`vid_restart`](../44-cli-commands-reference/02-client-ui-commands.md#vid_restart), смена бэкенда рендеринга, потеря и восстановление устройства). | `void(string rendererdescription) CSQC_RendererRestarted` |
| [`CSQC_GenerateMaterial`](../37-quakec-builtins-reference/00-entry-points.md#csqc_generatematerial) | Позволяет CSQC программно сгенерировать текст `.shader`-материала на лету — вместо того, чтобы искать соответствующий файл в `scripts/*.shader`. | `string(string shadername) CSQC_GenerateMaterial` |
| [`CSQC_ConsoleLink`](../37-quakec-builtins-reference/00-entry-points.md#csqc_consolelink) | Вызывается, когда игрок кликает мышью по специальной кликабельной ссылке, ранее выведенной в текст консоли (движок поддерживает разметку кликабельных ссылок в некоторых текстовых сообщениях). | `float(string text, string info) CSQC_ConsoleLink` |
| [`CSQC_Ent_Spawn`](../37-quakec-builtins-reference/00-entry-points.md#csqc_ent_spawn) | Устаревшая (legacy) точка входа, добавленная изначально для совместимости с расширениями DarkPlaces. | `void(float newentnum) CSQC_Ent_Spawn` |
| [`CSQC_ServerSound`](../37-quakec-builtins-reference/00-entry-points.md#csqc_serversound) | Устаревший (deprecated) предшественник современной точки входа `CSQC_Event_Sound`. | `float(float channel, string soundname, vector pos, float vol, float attenuation, float flags) CSQC_ServerSound` |
| [`CSQC_Parse_TempEntity`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_tempentity) | Вызывается при получении классического сетевого сообщения «temp entity» (`svc_temp_entity`) — искры, взрывы, кровь, молнии и другие одноразовые визуальные эффекты ванильного протокола Quake — до того, как движок обработает его штатным образом. | `float() CSQC_Parse_TempEntity` |
| [`CSQC_MapEntityEdited`](../37-quakec-builtins-reference/00-entry-points.md#csqc_mapentityedited) | Вызывается инструментами редактирования карт «на лету», встроенными в движок, когда описание конкретной сущности карты было изменено во время работы (например, через внутриигровой редактор сущностей или внешний инструмент, подключённый к движку). | `void(int entidx, string newentdata) CSQC_MapEntityEdited` |

### Точки входа QuakeC: MenuQC

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`m_init`](../37-quakec-builtins-reference/00-entry-points.md#m_init) | Вызывается один раз при первой загрузке логики меню — до первого показа какого-либо экрана. | `void() m_init` |
| [`m_shutdown`](../37-quakec-builtins-reference/00-entry-points.md#m_shutdown) | Вызывается при выгрузке логики меню — обычно только при полном завершении работы движка. | `void() m_shutdown` |
| [`m_toggle`](../37-quakec-builtins-reference/00-entry-points.md#m_toggle) | Вызывается при открытии/закрытии меню — например, по нажатию клавиши Esc во время игры или по завершении отключения от сервера. | `void(float show) m_toggle` |
| [`m_draw`](../37-quakec-builtins-reference/00-entry-points.md#m_draw) | Вызывается каждый кадр, пока меню активно, и служит главной точкой входа отрисовки MenuQC. | `void(vector screensize) m_draw` |
| [`m_drawloading`](../37-quakec-builtins-reference/00-entry-points.md#m_drawloading) | Отдельная точка входа, вызываемая специально во время экрана загрузки карты — причём вызывается, даже если основное меню в этот момент закрыто. | `void(vector screensize, float opaque) m_drawloading` |
| [`m_keydown`](../37-quakec-builtins-reference/00-entry-points.md#m_keydown) | Устаревшая точка входа MenuQC для нажатия клавиши. | `void(float scan, float chr) m_keydown` |
| [`m_keyup`](../37-quakec-builtins-reference/00-entry-points.md#m_keyup) | Парная legacy-функция к `m_keydown` — вызывается при отпускании клавиши, пока меню открыто, если `Menu_InputEvent` не используется. | `void(float scan, float chr) m_keyup` |
| [`Menu_InputEvent`](../37-quakec-builtins-reference/00-entry-points.md#menu_inputevent) | Более новый, единый обработчик ввода меню — прямой аналог `CSQC_InputEvent`, объединяющий обработку клавиатуры, мыши и других устройств в одну функцию с общим форматом события, вместо нескольких раздельных функций (`m_keydown`, `m_keyup`, `m_draw` и т.п.). | `float(float evtype, float scanx, float chary, float devid) Menu_InputEvent` |
| [`m_consolecommand`](../37-quakec-builtins-reference/00-entry-points.md#m_consolecommand) | Прямой аналог `CSQC_ConsoleCommand`, но для команд, зарегистрированных логикой меню через `registercommand`. | `float(string cmd) m_consolecommand` |
| [`m_gethostcachecategory`](../37-quakec-builtins-reference/00-entry-points.md#m_gethostcachecategory) | Позволяет логике меню сгруппировать найденные сервера по произвольным категориям при построении собственного кастомного браузера серверов (см. [«Браузер серверов и мастер-сервер»](../37-quakec-builtins-reference/11-server-browser-builtins.md)). | `float(float hostcachenum) m_gethostcachecategory` |
| [`Menu_RendererRestarted`](../37-quakec-builtins-reference/00-entry-points.md#menu_rendererrestarted) | Прямой аналог `CSQC_RendererRestarted` для логики меню — вызывается после смены видеорежима или полного перезапуска рендерера. | `void(string rendererdescription) Menu_RendererRestarted` |
| [`GameCommand`](../37-quakec-builtins-reference/00-entry-points.md#gamecommand) | Разбирает команды, отправленные логике меню из встроенного (native) меню самого движка — той части интерфейса, которая реализована не на MenuQC, а на Си (стандартные диалоги настроек видео, звука, управления в некоторых сборках движка). | `void(string cmdtext) GameCommand` |

### Математика и работа с векторами

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`acos`](../37-quakec-builtins-reference/01-math-vector-builtins.md#acos) | Возвращает арккосинус числа в радианах. | `float(float c) acos = #472;` |
| [`asin`](../37-quakec-builtins-reference/01-math-vector-builtins.md#asin) | Возвращает арксинус числа в радианах и обычно выдаёт результат в диапазоне от `-pi/2` до `pi/2`. | `float(float s) asin = #471;` |
| [`atan`](../37-quakec-builtins-reference/01-math-vector-builtins.md#atan) | Возвращает арктангенс числа в радианах. | `float(float t) atan = #473;` |
| [`atan2`](../37-quakec-builtins-reference/01-math-vector-builtins.md#atan2) | Вычисляет угол по двум компонентам и, в отличие от `atan`, сохраняет информацию о квадранте. | `float(float c, float s) atan2 = #474;` |
| [`tan`](../37-quakec-builtins-reference/01-math-vector-builtins.md#tan) | Возвращает тангенс угла в радианах. | `float(float a) tan = #475;` |
| [`sin`](../37-quakec-builtins-reference/01-math-vector-builtins.md#sin) | Возвращает синус угла в радианах. | `float(float angle) sin = #60;` |
| [`cos`](../37-quakec-builtins-reference/01-math-vector-builtins.md#cos) | Возвращает косинус угла в радианах. | `float(float angle) cos = #61;` |
| [`sqrt`](../37-quakec-builtins-reference/01-math-vector-builtins.md#sqrt) | Возвращает квадратный корень числа. | `float(float value) sqrt = #62;` |
| [`pow`](../37-quakec-builtins-reference/01-math-vector-builtins.md#pow) | Возводит `value` в степень `exp`. | `float(float value, float exp) pow = #97;` |
| [`log`](../37-quakec-builtins-reference/01-math-vector-builtins.md#log) | Вычисляет логарифм числа. | `float(float v, optional float base) log = #532;` |
| [`rint`](../37-quakec-builtins-reference/01-math-vector-builtins.md#rint) | Округляет число к ближайшему целому значению. | `float(float value) rint = #36;` |
| [`ceil`](../37-quakec-builtins-reference/01-math-vector-builtins.md#ceil) | Возвращает наименьшее целое, не меньшее входного значения. | `float(float value) ceil = #38;` |
| [`floor`](../37-quakec-builtins-reference/01-math-vector-builtins.md#floor) | Возвращает наибольшее целое, не большее входного значения. | `float(float value) floor = #37;` |
| [`fabs`](../37-quakec-builtins-reference/01-math-vector-builtins.md#fabs) | Возвращает абсолютное значение числа. | `float(float value) fabs = #43;` |
| [`bound`](../37-quakec-builtins-reference/01-math-vector-builtins.md#bound) | Зажимает число в диапазон `[minimum, maximum]`: если `val` меньше минимума, возвращается `minimum`; если больше максимума — `maximum`; иначе возвращается само `val`. | `float(float minimum, float val, float maximum) bound = #96;` |
| [`min`](../37-quakec-builtins-reference/01-math-vector-builtins.md#min) | Возвращает наименьшее значение из переданных аргументов. | `float(float a, float b, ...) min = #94;` |
| [`max`](../37-quakec-builtins-reference/01-math-vector-builtins.md#max) | Возвращает наибольшее из переданных значений. | `float(float a, float b, ...) max = #95;` |
| [`mod`](../37-quakec-builtins-reference/01-math-vector-builtins.md#mod) | Возвращает остаток от деления. | `float(float dividend, float divisor) mod = #245;` |
| [`random`](../37-quakec-builtins-reference/01-math-vector-builtins.md#random) | Возвращает псевдослучайное число с плавающей точкой в интервале около `0..1`. | `float() random = #7;` |
| [`randomvec`](../37-quakec-builtins-reference/01-math-vector-builtins.md#randomvec) | Возвращает случайный `vector`. | `vector() randomvec = #91;` |
| [`randomvector`](../37-quakec-builtins-reference/01-math-vector-builtins.md#randomvector) | Старое имя того же семейства builtins, которое в поздних списках и в таблицах движка обычно фигурирует как `randomvec`. | `vector() randomvector = #41;` |
| [`bitshift`](../37-quakec-builtins-reference/01-math-vector-builtins.md#bitshift) | Выполняет побитовый сдвиг. | `float(float number, float quantity) bitshift = #218;` |
| [`anglemod`](../37-quakec-builtins-reference/01-math-vector-builtins.md#anglemod) | Сворачивает угол в диапазон от `0` включительно до `360` не включительно. | `float(float value) anglemod = #102;` |
| [`changepitch`](../37-quakec-builtins-reference/01-math-vector-builtins.md#changepitch) | Плавно двигает pitch к целевому значению, используя поля `self.idealpitch` и `self.pitch_speed`. | `void(entity ent) changepitch = #63;` |
| [`changeyaw`](../37-quakec-builtins-reference/01-math-vector-builtins.md#changeyaw) | Плавно поворачивает `self.angles_y` в сторону `self.ideal_yaw`, не превышая `self.[yaw_speed](../39-entity-keys-reference/05-monster-player-keys.md#yaw_speed)` за один вызов. | `void() changeyaw = #49;` |
| [`normalize`](../37-quakec-builtins-reference/01-math-vector-builtins.md#normalize) | Возвращает копию вектора длиной `1`, сохраняющую исходное направление. | `vector(vector v) normalize = #9;` |
| [`vlen`](../37-quakec-builtins-reference/01-math-vector-builtins.md#vlen) | Возвращает длину вектора в quake units, то есть квадратный корень из скалярного произведения вектора на самого себя. | `float(vector v) vlen = #12;` |
| [`vtos`](../37-quakec-builtins-reference/01-math-vector-builtins.md#vtos) | Превращает `vector` во временную строку (`tempstring`) для отладки, логирования и UI. | `string(vector val) vtos = #27;` |
| [`vectoangles`](../37-quakec-builtins-reference/01-math-vector-builtins.md#vectoangles) | Преобразует вектор направления в углы Quake. | `vector(vector fwd, optional vector up) vectoangles = #51;` |
| [`vectoyaw`](../37-quakec-builtins-reference/01-math-vector-builtins.md#vectoyaw) | Возвращает yaw-угол в градусах для заданного направления. | `float(vector v, optional entity reference) vectoyaw = #13;` |
| [`makevectors`](../37-quakec-builtins-reference/01-math-vector-builtins.md#makevectors) | Преобразует вектор углов в три глобальных вектора: `v_forward`, `v_right` и `v_up`. | `void(vector vang) makevectors = #1;` |
| [`vectorvectors`](../37-quakec-builtins-reference/01-math-vector-builtins.md#vectorvectors) | Строит ортонормированный базис по одному направлению и записывает результат в глобальные `v_forward`, `v_right` и `v_up`. | `void(vector dir) vectorvectors = #432;` |
| [`rotatevectorsbyangle`](../37-quakec-builtins-reference/01-math-vector-builtins.md#rotatevectorsbyangle) | Не создаёт базис с нуля, а вращает уже существующий глобальный базис на заданные углы. | `void(vector angle) rotatevectorsbyangle = #235;` |
| [`rotatevectorsbyvectors`](../37-quakec-builtins-reference/01-math-vector-builtins.md#rotatevectorsbyvectors) | Умножает текущий глобальный базис на другой базис, переданный явными осями. | `void(vector fwd, vector right, vector up) rotatevectorsbyvectors = #236;` |
| [`rotatevectorsbytag`](../37-quakec-builtins-reference/01-math-vector-builtins.md#rotatevectorsbytag) | Доступен только в `CSQC` и предназначен для привязки локального базиса к тегу или кости модели. | `vector(entity ent, float tagnum) rotatevectorsbytag = #244;` |
| [`project`](../37-quakec-builtins-reference/01-math-vector-builtins.md#project) | Существует только в `CSQC` и преобразует мировую точку в экранные координаты текущего view/projection setup. | `vector(vector v) project = #311;` |
| [`unproject`](../37-quakec-builtins-reference/01-math-vector-builtins.md#unproject) | Обратная операция к `project`, тоже доступная только в `CSQC`. | `vector(vector v) unproject = #310;` |
| [`crc16`](../37-quakec-builtins-reference/01-math-vector-builtins.md#crc16) | Вычисляет 16-битный CRC-хеш для одной строки или для конкатенации нескольких строковых аргументов. | `__deprecated("Use digest_hex") float(float caseinsensitive, string s, ...) crc16 = #494;` |
| [`htos`](../37-quakec-builtins-reference/01-math-vector-builtins.md#htos) | Превращает `int` в hex-строку фиксированной ширины. | `string(int value) htos = #262;` |
| [`itos`](../37-quakec-builtins-reference/01-math-vector-builtins.md#itos) | Возвращает десятичное текстовое представление целого числа. | `string(int value) itos = #260;` |
| [`stoi`](../37-quakec-builtins-reference/01-math-vector-builtins.md#stoi) | Преобразует строку в `int`. | `int(string s) stoi = #259;` |
| [`stoh`](../37-quakec-builtins-reference/01-math-vector-builtins.md#stoh) | Читает строку как base-16 и возвращает `int`. | `int(string s) stoh = #261;` |
| [`str2chr`](../37-quakec-builtins-reference/01-math-vector-builtins.md#str2chr) | Возвращает числовой код символа по указанному индексу. | `float(string str, float index) str2chr = #222;` |
| [`chr2str`](../37-quakec-builtins-reference/01-math-vector-builtins.md#chr2str) | Выполняет обратное преобразование к `str2chr`: берёт один или несколько числовых кодов и создаёт из них строку. | `string(float chr, ...) chr2str = #223;` |
| [`anglesub`](../37-quakec-builtins-reference/01-math-vector-builtins.md#anglesub) | Вычитает `oldangle` из `newangle`, а затем нормализует разницу по кратчайшему пути вокруг окружности и всегда возвращает значение в диапазоне `[-180, 180]`. | `float(float newangle, float oldangle) anglesub = #0:anglesub;` |
| [`crossproduct`](../37-quakec-builtins-reference/01-math-vector-builtins.md#crossproduct) | Вычисляет векторное произведение `v1 x v2`. | `vector(vector v1, vector v2) crossproduct = #0:crossproduct;` |
| [`ftoi`](../37-quakec-builtins-reference/01-math-vector-builtins.md#ftoi) | Переводит обычный `float` QuakeC в настоящий `int` FTEQW из расширения `FTE_QC_INTCONV`. | `int(float) ftoi = #0:ftoi;` |
| [`ftou`](../37-quakec-builtins-reference/01-math-vector-builtins.md#ftou) | Делает то же базовое числовое преобразование, что и `ftoi`, но записывает результат в беззнаковый `__uint`. | `__uint(float) ftou = #0:ftou;` |
| [`itof`](../37-quakec-builtins-reference/01-math-vector-builtins.md#itof) | Переводит настоящий `int` обратно в `float`. | `float(int, optional float shift, float mask=24) itof = #0:itof;` |
| [`logarithm`](../37-quakec-builtins-reference/01-math-vector-builtins.md#logarithm) | Официальное старое имя того же builtin, который в более новых заголовках обычно фигурирует как `log`. | `float(float v, optional float base) logarithm = #0:logarithm;` |
| [`utof`](../37-quakec-builtins-reference/01-math-vector-builtins.md#utof) | Переводит беззнаковый integer в `float`. | `float(__uint, optional float shift, float mask=24) utof = #0:utof;` |

### Строки и текст

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`strlen`](../37-quakec-builtins-reference/02-string-builtins.md#strlen) | Возвращает длину строки в символах. | `float(string s) strlen = #114;` |
| [`strlennocol`](../37-quakec-builtins-reference/02-string-builtins.md#strlennocol) | Считает видимую длину текста после разбора цветовых кодов и другого текстового markup, который не занимает место на экране. | `float(string s) strlennocol = #476;` |
| [`strcat`](../37-quakec-builtins-reference/02-string-builtins.md#strcat) | Просто склеивает до восьми строк в одну tempstring. | `string(string s1, optional string s2, optional string s3, optional string s4, optional string s5, optional string s6, optional string s7, optional string s8) strcat = #115;` |
| [`substring`](../37-quakec-builtins-reference/02-string-builtins.md#substring) | Возвращает часть строки как tempstring. | `string(string s, float start, float length) substring = #116;` |
| [`stov`](../37-quakec-builtins-reference/02-string-builtins.md#stov) | Преобразует строку в `vector`, читая до трёх чисел слева направо. | `vector(string s) stov = #117;` |
| [`strzone`](../37-quakec-builtins-reference/02-string-builtins.md#strzone) | Исторический builtin из эпохи, когда модам приходилось вручную удерживать строку в памяти дольше обычной tempstring. | `string(string s, ...) strzone = #118;` |
| [`strunzone`](../37-quakec-builtins-reference/02-string-builtins.md#strunzone) | Исторически должен был освобождать строку, выделенную `strzone`. | `void(string s) strunzone = #119;` |
| [`strcasecmp`](../37-quakec-builtins-reference/02-string-builtins.md#strcasecmp) | Сравнивает две строки без учёта регистра. | `float(string s1, string s2) strcasecmp = #229;` |
| [`strncasecmp`](../37-quakec-builtins-reference/02-string-builtins.md#strncasecmp) | Делает то же самое, что `strcasecmp`, но сравнивает только ограниченный фрагмент и умеет стартовать не с начала строк. | `float(string s1, string s2, float len, optional float s1ofs, optional float s2ofs) strncasecmp = #230;` |
| [`strncmp`](../37-quakec-builtins-reference/02-string-builtins.md#strncmp) | Выполняет регистрозависимое сравнение строк. | `float(string s1, string s2, optional float len, optional float s1ofs, optional float s2ofs) strncmp = #228;` |
| [`strstrofs`](../37-quakec-builtins-reference/02-string-builtins.md#strstrofs) | Возвращает нулевое смещение первого найденного вхождения `sub` внутри `s1` или `-1`, если подстрока не найдена. | `float(string s1, string sub, optional float startidx) strstrofs = #221;` |
| [`strtolower`](../37-quakec-builtins-reference/02-string-builtins.md#strtolower) | Возвращает tempstring, где все буквы по возможности приведены к нижнему регистру. | `string(string s) strtolower = #480;` |
| [`strtoupper`](../37-quakec-builtins-reference/02-string-builtins.md#strtoupper) | Делает обратное преобразование относительно `strtolower`: возвращает строку, в которой буквы переведены в верхний регистр настолько, насколько это позволяет таблица преобразований движка. | `string(string s) strtoupper = #481;` |
| [`strreplace`](../37-quakec-builtins-reference/02-string-builtins.md#strreplace) | Проходит по `subject` слева направо и заменяет все неперекрывающиеся вхождения `search` на `replace`. | `string(string search, string replace, string subject) strreplace = #484;` |
| [`strireplace`](../37-quakec-builtins-reference/02-string-builtins.md#strireplace) | Работает так же, как `strreplace`, но ищет совпадения без учёта регистра. | `string(string search, string replace, string subject) strireplace = #485;` |
| [`strpad`](../37-quakec-builtins-reference/02-string-builtins.md#strpad) | Сначала собирает входные строки в одну, а затем при необходимости дополняет её пробелами до заданной длины. | `string(float pad, string str1, ...) strpad = #225;` |
| [`strconv`](../37-quakec-builtins-reference/02-string-builtins.md#strconv) | Не занимается современными строковыми преобразованиями в стиле `tolower`; он работает с классическим Quake-charset и его «красными», «белыми» и специальными диапазонами символов. | `string(float ccase, float redalpha, float redchars, string str, ...) strconv = #224;` |
| [`strdecolorize`](../37-quakec-builtins-reference/02-string-builtins.md#strdecolorize) | Удаляет из строки цветовые коды и прочий текстовый markup, который интерпретируется движком при выводе. | `string(string s) strdecolorize = #477;` |
| [`strftime`](../37-quakec-builtins-reference/02-string-builtins.md#strftime) | Форматирует текущее системное время движка по шаблону в стиле стандартной C-функции `strftime`. | `string(float uselocaltime, string format, ...) strftime = #478;` |
| [`sprintf`](../37-quakec-builtins-reference/02-string-builtins.md#sprintf) | Собирает tempstring по форматной строке примерно как одноимённая функция в C, но с поправками на типовую модель QuakeC. | `string(string fmt, ...) sprintf = #627;` |
| [`tokenize`](../37-quakec-builtins-reference/02-string-builtins.md#tokenize) | Разбирает строку на токены и возвращает их количество. | `float(string s) tokenize = #441;` |
| [`tokenize_console`](../37-quakec-builtins-reference/02-string-builtins.md#tokenize_console) | Использует именно консольный токенизатор FTEQW. | `float(string str) tokenize_console = #514;` |
| [`tokenizebyseparator`](../37-quakec-builtins-reference/02-string-builtins.md#tokenizebyseparator) | Режет строку только по явно заданным разделителям и больше ни по чему. | `float(string s, string separator1, ...) tokenizebyseparator = #479;` |
| [`argv`](../37-quakec-builtins-reference/02-string-builtins.md#argv) | Возвращает текст токена по индексу из общего token-buffer. | `string(float n) argv = #442;` |
| [`argv_start_index`](../37-quakec-builtins-reference/02-string-builtins.md#argv_start_index) | Возвращает позицию в исходной строке, с которой начался соответствующий токен. | `float(float idx) argv_start_index = #515;` |
| [`argv_end_index`](../37-quakec-builtins-reference/02-string-builtins.md#argv_end_index) | Возвращает позицию в исходной строке сразу после конца указанного токена. | `float(float idx) argv_end_index = #516;` |
| [`validstring`](../37-quakec-builtins-reference/02-string-builtins.md#validstring) | Возвращает истину, если переданная строка не является null string. | `float(string str) validstring = #81;` |
| [`ftos`](../37-quakec-builtins-reference/02-string-builtins.md#ftos) | Переводит число `float` в tempstring. | `string(float val) ftos = #26;` |
| [`stof`](../37-quakec-builtins-reference/02-string-builtins.md#stof) | Преобразует строку в число `float`. | `float(string s) stof = #81;` |
| [`altstr_count`](../37-quakec-builtins-reference/02-string-builtins.md#altstr_count) | Устаревший helper для формата альтернативных строк, где элементы перечисляются как одинарно-кавыченные фрагменты, например `"'one''two''three'"` или с разделяющими пробелами между кавычёнными блоками. | `float(string str) altstr_count = #82;` |
| [`altstr_get`](../37-quakec-builtins-reference/02-string-builtins.md#altstr_get) | Извлекает `num`-й quoted-элемент из altstr-строки. | `string(string str, float num) altstr_get = #84;` |
| [`altstr_prepare`](../37-quakec-builtins-reference/02-string-builtins.md#altstr_prepare) | Экранирует в строке только одинарные кавычки, вставляя перед ними обратный слэш. | `string(string str) altstr_prepare = #83;` |
| [`altstr_set`](../37-quakec-builtins-reference/02-string-builtins.md#altstr_set) | Заменяет `num`-й quoted-элемент в altstr новой строкой и возвращает результат как tempstring. | `string(string str, float num, string setval) altstr_set = #85;` |
| [`uri_escape`](../37-quakec-builtins-reference/02-string-builtins.md#uri_escape) | Кодирует строку для использования в URI/HTTP-параметрах через percent-encoding. | `string(string in) uri_escape = #510;` |
| [`uri_unescape`](../37-quakec-builtins-reference/02-string-builtins.md#uri_unescape) | Выполняет обратное преобразование для percent-encoding и заменяет корректные `%HH` последовательности их исходными байтами. | `string(string in) uri_unescape = #511;` |
| [`argescape`](../37-quakec-builtins-reference/02-string-builtins.md#argescape) | Подготавливает строку так, чтобы позднее консольный токенизатор воспринимал её как один аргумент, даже если внутри есть пробелы, кавычки или иные специальные символы. | `string(string s) argescape = #295;` |
| [`stringwidth`](../37-quakec-builtins-reference/02-string-builtins.md#stringwidth) | Относится к клиентскому/UI слою, но часто используется вместе с обычными строковыми builtins, поэтому его удобно держать рядом. | `float(string text, float usecolours, optional vector fontsize) stringwidth = #327;` |
| [`stringtokeynum`](../37-quakec-builtins-reference/02-string-builtins.md#stringtokeynum) | Ищет код клавиши по её имени так же, как это делает система биндов. | `float(string keyname) stringtokeynum = #341;` |
| [`str2chr`](../37-quakec-builtins-reference/02-string-builtins.md#str2chr) | [`str2chr`](../37-quakec-builtins-reference/01-math-vector-builtins.md#str2chr) возвращает числовой код символа на позиции `index`. | `float(string str, float index) str2chr = #222;` |
| [`chr2str`](../37-quakec-builtins-reference/02-string-builtins.md#chr2str) | [`chr2str`](../37-quakec-builtins-reference/01-math-vector-builtins.md#chr2str) делает обратную операцию относительно `str2chr`: принимает один или несколько числовых кодов и собирает из них строку. | `string(float chr, ...) chr2str = #223;` |
| [`altstr_ins`](../37-quakec-builtins-reference/02-string-builtins.md#altstr_ins) | Числится в builtin-таблицах как устаревшая (`DEP`) совместимостная функция старого altstr-API, но в актуальном FTEQW она **не реализована**. И в `SSQC`, и в `MenuQC` имя привязано к `PF_Fixme`, то есть вызов заканчивается ошибкой вида `Builtin ... not implemented`, а не реальной вставкой элемента. | `DEP string(string str, float num, string set) altstr_ins = #86;` |
| [`base64decode`](../37-quakec-builtins-reference/02-string-builtins.md#base64decode) | Декодирует Base64-строку и выделяет новый адресуемый блок памяти внутри QCVM. | `__variant*(string base64str, __out int bytes) base64decode = #0:base64decode;` |
| [`base64encode`](../37-quakec-builtins-reference/02-string-builtins.md#base64encode) | Берёт бинарный blob из адресуемой памяти QC и возвращает его копию в виде Base64 tempstring. | `string(__variant *ptr, int bytes, optional int offset) base64encode = #0:base64encode;` |
| [`instr`](../37-quakec-builtins-reference/02-string-builtins.md#instr) | Ищет первое вхождение `token` внутри `input` и возвращает не числовой индекс, а **хвост исходной строки, начиная с найденного места**. Если совпадения нет, builtin возвращает пустую/null string. | `string(string input, string token) instr = #206;` |
| [`matchpattern`](../37-quakec-builtins-reference/02-string-builtins.md#matchpattern) | Присутствует в исходниках только как закомментированная запись в builtin-таблицах и не имеет рабочей реализации ни в `SSQC`, ни в `CSQC`, ни в `MenuQC`. | `float(string s, string pattern, float matchrule) matchpattern = #538;` |
| [`strcmp`](../37-quakec-builtins-reference/02-string-builtins.md#strcmp) | В официальных заголовках FTEQW `strcmp` — это не отдельный builtin, а препроцессорный alias к `strncmp`. | `#define strcmp strncmp` |
| [`strtrim`](../37-quakec-builtins-reference/02-string-builtins.md#strtrim) | Удаляет пробелы, табы, `\n` и `\r` только с начала и конца строки, не затрагивая внутренние разделители между словами. | `string(string s) strtrim = #0:strtrim;` |

### Сущности и игровой мир

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`spawn`](../37-quakec-builtins-reference/03-entity-world-builtins.md#spawn) | Создаёт новый edict и возвращает ссылку на него. | `entity() spawn = #14;` |
| [`remove`](../37-quakec-builtins-reference/03-entity-world-builtins.md#remove) | Удаляет сущность из мира. По комментариям и реализации FTEQW, после вызова ссылка считается недействительной: движок очищает часть полей и позже может переиспользовать слот под другой edict, поэтому нельзя продолжать обращаться к старому указателю. | `void(entity e) remove = #15;` |
| [`find`](../37-quakec-builtins-reference/03-entity-world-builtins.md#find) | Линейно перебирает edict'ы, начиная со следующего после `start`, и возвращает первую живую сущность, у которой содержимое строкового поля точно равно `match`. | `entity(entity start, .string fld, string match) find = #18;` |
| [`findchain`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findchain) | За один проход находит все подходящие сущности и возвращает голову цепочки. | `entity(.string field, string match, optional .entity chainfield) findchain = #402;` |
| [`findchainflags`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findchainflags) | Линейно проходит по сущностям и включает в результат все edict'ы, у которых `(field & match) != 0`. | `entity(.float fld, float match, optional .entity chainfield) findchainflags = #450;` |
| [`findchainfloat`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findchainfloat) | Строит цепочку из всех сущностей, у которых выбранное `.float`-слот поля равен `match`. | `entity(.float fld, float match, optional .entity chainfield) findchainfloat = #403;` |
| [`findflags`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findflags) | Ищет следующую сущность, у которой в `field` установлен хотя бы один бит из `match`. | `entity(entity start, .float field, float match) findflags = #449;` |
| [`findfloat`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findfloat) | По смыслу это «нестроковый find»: движок сравнивает сырое хранимое значение поля, а не текстовую форму. | `entity(entity start, .__variant fld, __variant match) findfloat = #98;` |
| [`findradius`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findradius) | Ищет все сущности в пределах радиуса и возвращает голову цепочки совпадений. | `entity(vector org, float rad, optional .entity chainfield) findradius = #22;` |
| [`nextent`](../37-quakec-builtins-reference/03-entity-world-builtins.md#nextent) | Возвращает следующую живую сущность после `e`, пропуская freed slots. | `entity(entity e) nextent = #47;` |
| [`setmodel`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setmodel) | Назначает сущности [`model`](../39-entity-keys-reference/01-worldspawn-common-keys.md#model) и `modelindex`, а затем перелинковывает её коллизионное состояние. | `void(entity e, string m) setmodel = #3;` |
| [`setmodelindex`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setmodelindex) | Документация в `fteextensions.qc` описывает builtin как вариант `setmodel`, принимающий уже готовый precache index вместо имени. | `void(entity e, float mdlindex) setmodelindex = #333;` |
| [`setorigin`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setorigin) | Корректный способ мгновенно переместить сущность без обычной физики движения. | `void(entity e, vector o) setorigin = #2;` |
| [`setsize`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setsize) | Меняет `mins`, `maxs` и производное `size`, а затем перелинковывает сущность. | `void(entity e, vector min, vector max) setsize = #4;` |
| [`checkbottom`](../37-quakec-builtins-reference/03-entity-world-builtins.md#checkbottom) | Делает дорогую проверку того, что bbox сущности действительно опирается на твёрдую поверхность. | `float(entity ent) checkbottom = #40;` |
| [`droptofloor`](../37-quakec-builtins-reference/03-entity-world-builtins.md#droptofloor) | Мгновенно сдвигает `self` вдоль направления гравитации вниз до первого твёрдого упора. | `float() droptofloor = #34;` |
| [`walkmove`](../37-quakec-builtins-reference/03-entity-world-builtins.md#walkmove) | Пытается сдвинуть `self` шагом обычной «наземной» логики Quake. | `float(float yaw, float dist, optional float settraceglobals) walkmove = #32;` |
| [`movetogoal`](../37-quakec-builtins-reference/03-entity-world-builtins.md#movetogoal) | Запускает стандартную монстровую логику Quake, которая пытается продвинуть `self` к `goalentity`, учитывая ступеньки и обход локальных препятствий. | `void(float step) movetogoal = #67;` |
| [`touchtriggers`](../37-quakec-builtins-reference/03-entity-world-builtins.md#touchtriggers) | Принудительно проверяет контакт сущности со всеми `SOLID_TRIGGER`, которые её пересекают, и вызывает их `touch`-логику. | `void(optional entity ent, optional vector neworigin) touchtriggers = #279;` |
| [`pointcontents`](../37-quakec-builtins-reference/03-entity-world-builtins.md#pointcontents) | Проверяет, какой content находится в конкретной точке, и возвращает одно из значений `CONTENT_*`/`CONTENTS_*`. | `float(vector pos) pointcontents = #41;` |
| [`checkclient`](../37-quakec-builtins-reference/03-entity-world-builtins.md#checkclient) | Возвращает «очередного» игрока-кандидата для AI-проверок, циклически перебирая клиентов во времени. | `entity() checkclient = #17;` |
| [`checkpvs`](../37-quakec-builtins-reference/03-entity-world-builtins.md#checkpvs) | Возвращает ненулевое значение, если указанный edict потенциально попадает в PVS/visibility set из точки `viewpos`. | `float(vector viewpos, entity entity) checkpvs = #240;` |
| [`num_for_edict`](../37-quakec-builtins-reference/03-entity-world-builtins.md#num_for_edict) | Возвращает числовой индекс edict'а в таблице сущностей. | `float(entity ent) num_for_edict = #512;` |
| [`edict_num`](../37-quakec-builtins-reference/03-entity-world-builtins.md#edict_num) | Преобразует номер сущности обратно в `entity`. | `entity(float entnum) edict_num = #459;` |
| [`etof`](../37-quakec-builtins-reference/03-entity-world-builtins.md#etof) | Приводит entity reference к числу. | `float(entity e) etof = #79;` |
| [`ftoe`](../37-quakec-builtins-reference/03-entity-world-builtins.md#ftoe) | Обратное преобразование к `etof`: принимает число и трактует его как ссылку на сущность. | `entity(float f) ftoe = #80;` |
| [`etos`](../37-quakec-builtins-reference/03-entity-world-builtins.md#etos) | Возвращает временную строку с текстовым представлением entity reference. | `string(entity ent) etos = #65;` |
| [`wasfreed`](../37-quakec-builtins-reference/03-entity-world-builtins.md#wasfreed) | Быстро проверяет, помечен ли указанный edict как свободный. | `float(entity ent) wasfreed = #353;` |
| [`copyentity`](../37-quakec-builtins-reference/03-entity-world-builtins.md#copyentity) | Копирует все поля одного edict'а в другой и затем перелинковывает результат. | `entity(entity from, optional entity to) copyentity = #400;` |
| [`aim`](../37-quakec-builtins-reference/03-entity-world-builtins.md#aim) | Возвращает скорректированный вариант `v_forward` для Quake-style auto-aim. | `vector(entity player, float missilespeed) aim = #44;` |
| [`traceline`](../37-quakec-builtins-reference/03-entity-world-builtins.md#traceline) | Трассирует тонкий луч и записывает результат в `trace_*` globals. | `void(vector v1, vector v2, float flags, entity ent) traceline = #16;` |
| [`tracebox`](../37-quakec-builtins-reference/03-entity-world-builtins.md#tracebox) | То же самое, что `traceline`, но вместо математической линии используется объёмная коробка. | `void(vector start, vector mins, vector maxs, vector end, float nomonsters, entity ent) tracebox = #90;` |
| [`tracetoss`](../37-quakec-builtins-reference/03-entity-world-builtins.md#tracetoss) | Симулирует баллистическое движение объекта с его текущими параметрами и заполняет `trace_*` глобалы местом/фактом ожидаемого столкновения. | `void(entity ent, entity ignore) tracetoss = #64;` |
| [`traceon`](../37-quakec-builtins-reference/03-entity-world-builtins.md#traceon) | Включает трассировку/пошаговую диагностику выполнения QC. | `void() traceon = #29;` |
| [`traceoff`](../37-quakec-builtins-reference/03-entity-world-builtins.md#traceoff) | Выключает режим трассировки QC, включённый через `traceon`. | `void() traceoff = #30;` |
| [`makestatic`](../37-quakec-builtins-reference/03-entity-world-builtins.md#makestatic) | Берёт render-состояние сущности, добавляет его в список static entities и тут же удаляет исходный edict. | `void(entity e) makestatic = #69;` |
| [`setspawnparms`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setspawnparms) | Перезаписывает глобалы `parm1..parm16` значениями, сохранёнными для указанного клиента из предыдущей карты/респавна. | `void(entity player) setspawnparms = #78;` |
| [`spawnclient`](../37-quakec-builtins-reference/03-entity-world-builtins.md#spawnclient) | Создаёт bot client в первом свободном клиентском слоте и возвращает его player edict. | `entity() spawnclient = #454;` |
| [`dropclient`](../37-quakec-builtins-reference/03-entity-world-builtins.md#dropclient) | Помечает клиента на отключение. | `void(entity player) dropclient = #453;` |
| [`runstandardplayerphysics`](../37-quakec-builtins-reference/03-entity-world-builtins.md#runstandardplayerphysics) | Просит движок выполнить обычную player-physics FTEQW, используя текущие `input_*` globals как вход. | `void(entity ent) runstandardplayerphysics = #347;` |
| [`getstati`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getstati) | Доступно в CSQC. Возвращает целочисленное значение stat без потери точности, когда stat на сервере был зарегистрирован как `EV_INTEGER`. Это правильный выбор для packed-битов и всех случаев, где 32-битное значение нельзя безопасно пропускать через обычный float. | `int(float stnum) getstati = #330;` |
| [`getstatf`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getstatf) | Доступно в CSQC. В обычном режиме читает числовой stat как `float`; если передать `firstbit` и `bitcount`, builtin извлекает битовое поле из integer-представления статов, что отдельно рекомендовано для `STAT_ITEMS`. Это удобно для packed-флагов из классического протокола без лишнего ручного побитового кода. | `float(float stnum, optional float firstbit, optional float bitcount) getstatf = #331;` |
| [`getstats`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getstats) | Доступно в CSQC. Возвращает string stat как tempstring; в современных расширениях FTE строковые статы живут в отдельном пространстве имён и не ограничены жёстко 15 символами, как в старых packed-схемах. Это лучший способ выводить серверно-синхронизированный HUD-текст, названия режимов, таймеры и другие строки без ручной распаковки. | `string(float stnum) getstats = #332;` |
| [`clientstat`](../37-quakec-builtins-reference/03-entity-world-builtins.md#clientstat) | Это серверный mapping builtin: он не читает stat прямо сейчас, а настраивает, какое поле каждого клиента будет реплицироваться в указанный stat slot. | `void(float num, float type, .__variant fld) clientstat = #232;` |
| [`globalstat`](../37-quakec-builtins-reference/03-entity-world-builtins.md#globalstat) | Похож на `clientstat`, но привязывает stat к одному глобальному значению для всех клиентов. | `void(float num, float type, string name) globalstat = #233;` |
| [`forceinfokey`](../37-quakec-builtins-reference/03-entity-world-builtins.md#forceinfokey) | Меняет userinfo сервера напрямую, не заставляя клиента переподключаться и не трогая его локальный config. | `void(entity player, string key, string value) forceinfokey = #213;` |
| [`serverkey`](../37-quakec-builtins-reference/03-entity-world-builtins.md#serverkey) | Читает значение из публичной строки `serverinfo`. | `string(string key) serverkey = #354;` |
| [`infokey`](../37-quakec-builtins-reference/03-entity-world-builtins.md#infokey) | Если `e == world`, builtin ищет ключ в `serverinfo`, а при отсутствии — в [`localinfo`](../44-cli-commands-reference/04-server-multiplayer-commands.md#localinfo); если `e` — игрок, читается его userinfo. | `string(entity e, string key) infokey = #80;` |
| [`infoadd`](../37-quakec-builtins-reference/03-entity-world-builtins.md#infoadd) | Создаёт новую infostring, в которой значение `key` заменено или добавлено. | `infostring(infostring old, string key, string value) infoadd = #226;` |
| [`infoget`](../37-quakec-builtins-reference/03-entity-world-builtins.md#infoget) | Читает одно значение из уже готовой infostring и возвращает его как tempstring. | `string(infostring info, string key) infoget = #227;` |
| [`matchclientname`](../37-quakec-builtins-reference/03-entity-world-builtins.md#matchclientname) | Ищет клиента тем же механизмом, которым сервер обычно резолвит имена в консольных командах. | `entity(string match, optional float matchnum) matchclientname = #241;` |
| [`entityfieldname`](../37-quakec-builtins-reference/03-entity-world-builtins.md#entityfieldname) | Возвращает имя поля сущности по его числовому индексу из reflection API. | `string(float fieldnum) entityfieldname = #497;` |
| [`entityfieldtype`](../37-quakec-builtins-reference/03-entity-world-builtins.md#entityfieldtype) | Возвращает тип поля как одно из значений `EV_*`. | `float(float fieldnum) entityfieldtype = #498;` |
| [`numentityfields`](../37-quakec-builtins-reference/03-entity-world-builtins.md#numentityfields) | Возвращает количество именованных entity fields, известных текущей VM. | `float() numentityfields = #496;` |
| [`getentityfieldstring`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getentityfieldstring) | Возвращает строковую форму произвольного поля сущности, что удобно для редакторов, дампов и generic debugging UI. | `string(float fieldnum, entity ent) getentityfieldstring = #499;` |
| [`putentityfieldstring`](../37-quakec-builtins-reference/03-entity-world-builtins.md#putentityfieldstring) | Парсит текст и записывает его в выбранное поле сущности по тем же правилам, по которым движок обычно восстанавливает поля из текстового entity/savegame формата. | `float(float fieldnum, entity ent, string s) putentityfieldstring = #500;` |
| [`getentitytoken`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getentitytoken) | Доступно в CSQC. Builtin возвращает следующий токен из entity lump карты или из другой строки, если вы заранее подали её в `resetstring`. Пустой `resetstring` просит FTE снова взять исходную entity-строку BSP и начать сначала; все результаты — tempstring. Это основной primitive для CSQC-парсинга worldspawn/entity lump при клиентских редакторах, minimap metadata и custom preload logic. | `string(optional string resetstring) getentitytoken = #355;` |
| [`parseentitydata`](../37-quakec-builtins-reference/03-entity-world-builtins.md#parseentitydata) | Читает одну текстовую запись сущности и применяет её поля к уже существующему edict'у. | `float(entity e, string s, optional float offset) parseentitydata = #613;` |
| [`getentity`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getentity) | Доступно в CSQC. Builtin позволяет читать ограниченный набор полей у сущностей, которые не представлены как обычные CSQC edict'ы, если они известны клиенту и находятся в PVS. По `GE_*` можно получить origin, bbox, frame, effects, tag attachment и другие атрибуты; специальный `GE_MAXENTS` игнорирует `entnum` и сообщает верхнюю разумную границу перебора. При неизвестной сущности FTEQW возвращает нули/нулевой вектор и пишет предупреждение только в отладочный вывод, поэтому код должен уметь жить с «данных пока нет». | `__variant(float entnum, float fieldnum) getentity = #504;` |
| [`resourcestatus`](../37-quakec-builtins-reference/03-entity-world-builtins.md#resourcestatus) | Возвращает состояние ресурса как `RESSTATE_NOTKNOWN`, `RESSTATE_NOTLOADED`, `RESSTATE_LOADING`, `RESSTATE_FAILED` или `RESSTATE_LOADED`. | `float(float resourcetype, float tryload, string resourcename) resourcestatus = #286;` |
| [`physics_addforce`](../37-quakec-builtins-reference/03-entity-world-builtins.md#physics_addforce) | Отправляет в backend rigid-body physics команду применить импульс к физическому объекту. | `void(entity e, vector force, vector relative_ofs) physics_addforce = #541;` |
| [`physics_addtorque`](../37-quakec-builtins-reference/03-entity-world-builtins.md#physics_addtorque) | Добавляет вращающий импульс rigid-body объекту. | `void(entity e, vector torque) physics_addtorque = #542;` |
| [`physics_enable`](../37-quakec-builtins-reference/03-entity-world-builtins.md#physics_enable) | Включает или выключает обработку physics backend для `MOVETYPE_PHYSICS` entity. | `void(entity e, float physics_enabled) physics_enable = #540;` |
| [`terrain_edit`](../37-quakec-builtins-reference/03-entity-world-builtins.md#terrain_edit) | Выполняет операции live-редактирования heightmap terrain: подъём/сглаживание высот, отверстия, текстурные операции, сохранение, reset секций и т. д. | `__variant(float action, optional vector pos, optional float radius, optional float quant, ...) terrain_edit = #278;` |
| [`setattachment`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setattachment) | Привязывает сущность к тегу/кости другой модели, записывая нужные значения в `tag_entity` и `tag_index`. | `void(entity e, entity tagentity, string tagname) setattachment = #443;` |
| [`checkcommand`](../37-quakec-builtins-reference/03-entity-world-builtins.md#checkcommand) | Проверяет, существует ли введённое имя в консольной экосистеме движка. | `float(string name) checkcommand = #294;` |
| [`registercommand`](../37-quakec-builtins-reference/03-entity-world-builtins.md#registercommand) | Регистрирует консольную команду, если такой ещё нет. | `void(string cmdname, optional string desc) registercommand = #352;` |
| [`isfunction`](../37-quakec-builtins-reference/03-entity-world-builtins.md#isfunction) | Проверяет, существует ли функция с таким именем и можно ли вызвать её через `callfunction`. | `float(string s) isfunction = #607;` |
| [`callfunction`](../37-quakec-builtins-reference/03-entity-world-builtins.md#callfunction) | Находит функцию по имени и вызывает её, передавая остальные аргументы как есть. | `void(.../*, string funcname*/) callfunction = #605;` |
| [`externcall`](../37-quakec-builtins-reference/03-entity-world-builtins.md#externcall) | Часть `FTE_MULTIPROGS`. Builtin вызывает функцию по имени в другом загруженном `progs.dat` и возвращает её значение как `__variant`. | `__variant(float prnum, string funcname, ...) externcall = #201;` |
| [`externset`](../37-quakec-builtins-reference/03-entity-world-builtins.md#externset) | Часть `FTE_MULTIPROGS`. Записывает значение в глобал другого progs по имени, что позволяет связывать несколько QC-модулей без жёсткой линковки на этапе компиляции. | `void(float prnum, __variant newval, string varname) externset = #204;` |
| [`externvalue`](../37-quakec-builtins-reference/03-entity-world-builtins.md#externvalue) | Часть `FTE_MULTIPROGS`. Читает глобальную переменную из другого progs и возвращает её как `__variant`. | `__variant(float prnum, string varname) externvalue = #203;` |
| [`builtin_find`](../37-quakec-builtins-reference/03-entity-world-builtins.md#builtin_find) | Проверяет, поддерживается ли builtin с данным именем, и возвращает его номер. | `float(string builtinname) builtin_find = #100;` |
| [`changelevel`](../37-quakec-builtins-reference/03-entity-world-builtins.md#changelevel) | В SSQC это классический builtin для перехода на другую карту. | `void(string mapname, optional string newmapstartspot) changelevel = #70;` |
| [`chat`](../37-quakec-builtins-reference/03-entity-world-builtins.md#chat) | Этот расширенный builtin активируется через константу `FTE_QC_NPCCHAT`. | `void(string filename, float starttag, entity edict) chat = #214;` |
| [`empty`](../37-quakec-builtins-reference/03-entity-world-builtins.md#empty) | Не является реальным builtin'ом с зарегистрированным именем. | `void() empty = #245..#249;` |
| [`entityfieldref`](../37-quakec-builtins-reference/03-entity-world-builtins.md#entityfieldref) | Часть Reflection API для полей сущностей (entity fields). | `field_t(float fieldnum) entityfieldref = #0:entityfieldref;` |
| [`entityprotection`](../37-quakec-builtins-reference/03-entity-world-builtins.md#entityprotection) | Builtin контролирует флаг [`readonly`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#readonly) в структуре эдикта и при значениях `0` или `1` напрямую записывает их в `e->readonly`, после чего QC больше не сможет менять поля защищённой сущности. | `float(entity e, float nowreadonly) entityprotection = #0:entityprotection;` |
| [`eprint`](../37-quakec-builtins-reference/03-entity-world-builtins.md#eprint) | Отладочный builtin, который выводит в консоль всю доступную информацию о полях сущности. | `void(entity e) eprint = #31;` |
| [`find_list`](../37-quakec-builtins-reference/03-entity-world-builtins.md#find_list) | В отличие от стандартной функции `find`, данный builtin не просто ищет одну сущность за раз, а возвращает в С-стиле указатель на динамический массив найденных совпадений в виде `entity*`. | `entity*(.__variant fld, __variant match, int type=EV_STRING, __out int count) find_list = #0:find_list;` |
| [`findentity`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findentity) | В серверной таблице FTEQW отдельной записи с именем `findentity` нет: это именно alias к `findfloat`, задокументированный прямо в `fteextensions.qc` и в описании builtin `#98`. | `findentity` — alias из `fteextensions.qc` для `entity(entity start, .__variant fld, __variant match) findfloat = #98;` |
| [`findentityfield`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findentityfield) | Часть рефлексии полей, возвращающая глобальный индекс указанного поля по его строковому имени. | `float(string fieldname) findentityfield = #0:findentityfield;` |
| [`findradius_list`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findradius_list) | Альтернатива стандартной функции `findradius`, которая возвращает temp-массив сущностей вместо создания связанного списка через поле `.chain`. | `entity*(vector org, float rad, __out int foundcount, int sort=0) findradius_list = #0:findradius_list;` |
| [`generateentitydata`](../37-quakec-builtins-reference/03-entity-world-builtins.md#generateentitydata) | Позволяет сдампить структуру полей и параметров сущности в текстовую строку формата Map/Entity, которую затем можно обратно пропарсить через `parseentitydata`. | `string(entity e) generateentitydata = #0:generateentitydata;` |
| [`plaque_draw`](../37-quakec-builtins-reference/03-entity-world-builtins.md#plaque_draw) | Специфический встроенный builtin для совместимости с Hexen II, который выводит текстовую плашку (plaque-окно) на экране игрока, а не стандартный текстовый centerprint. | `void(entity targ, float stringno) plaque_draw = #79;` |
| [`pushmove`](../37-quakec-builtins-reference/03-entity-world-builtins.md#pushmove) | Выполняет физическое перемещение объектов, имеющих тип движения `MOVETYPE_PUSH`. | `float(entity pusher, vector move, vector amove) pushmove = #0;` |
| [`qtest_canreach`](../37-quakec-builtins-reference/03-entity-world-builtins.md#qtest_canreach) | Это устаревший QTest-код, который в современных сборках FTEQW заменен на заглушку `PF_Ignore`. | `DEP float(vector v) qtest_canreach = #39;` |
| [`readserverentitystate`](../37-quakec-builtins-reference/03-entity-world-builtins.md#readserverentitystate) | В актуальных сборках FTEQW этот слот не зарегистрирован как рабочий builtin: номер `#369` не активирован и помечен пометкой `EXT_CSQC_1`. | `void(float flags, float simtime) readserverentitystate = #369;` |
| [`readsingleentitystate`](../37-quakec-builtins-reference/03-entity-world-builtins.md#readsingleentitystate) | У `readsingleentitystate` та же судьба, что и у `readserverentitystate`: это только закомментированный placeholder, а не доступная builtin-функция. | `readsingleentitystate` — незарегистрированный закомментированный слот `#370` из старого `EXT_CSQC_1`. |
| [`removeentity`](../37-quakec-builtins-reference/03-entity-world-builtins.md#removeentity) | Это не синоним стандартного удаления `remove`, а специфическая CSQC-команда клиентского рендеринга. | `void(entity ent) removeentity = #0:removeentity;` |
| [`route_calculate`](../37-quakec-builtins-reference/03-entity-world-builtins.md#route_calculate) | Продвинутый встроенный метод для асинхронной работы с routing/nodegraph-системой FTEQW без блокировки основного потока: результат расчета передается через callback. | `void(entity ent, vector dest, int denylinkflags, void(entity ent, vector dest, int numnodes, nodeslist_t *nodelist) callback) route_calculate = #0:route_calculate;` |
| [`runclientphys`](../37-quakec-builtins-reference/03-entity-world-builtins.md#runclientphys) | Отдельного builtin с именем `runclientphys` в текущих таблицах нет: так называется C-функция движка, обслуживающая builtin `runstandardplayerphysics`. | `runclientphys` — это внутреннее имя реализации; в QuakeC рабочий builtin называется `void(entity ent) runstandardplayerphysics = #347;` |
| [`te_gunshotquad`](../37-quakec-builtins-reference/03-entity-world-builtins.md#te_gunshotquad) | Создает temp-entity эффект `TEDP_GUNSHOTQUAD`, то есть усиленную под Quad Damage версию стандартного пулевого попадания (gunshot-искры/декаль). | `void(vector org) te_gunshotquad = #412;` |
| [`te_lightning2`](../37-quakec-builtins-reference/03-entity-world-builtins.md#te_lightning2) | Генерирует beam-эффект (луч молнии) типа `TE_LIGHTNING2` между координатами `start` и `end`. | `void(entity own, vector start, vector end) te_lightning2 = #429;` |
| [`te_lightning3`](../37-quakec-builtins-reference/03-entity-world-builtins.md#te_lightning3) | Выполняет ту же функцию создания beam-эффекта, что и `te_lightning2`, но использует визуальный тип `TE_LIGHTNING3` (обычно это текстура молнии другого цвета или формы). | `void(entity own, vector start, vector end) te_lightning3 = #430;` |
| [`te_muzzleflash`](../37-quakec-builtins-reference/03-entity-world-builtins.md#te_muzzleflash) | Встроенный хелпер для создания эффекта вспышки выстрела (muzzle flash). | `void(entity ent) te_muzzleflash = #0:te_muzzleflash;` |
| [`te_spikequad`](../37-quakec-builtins-reference/03-entity-world-builtins.md#te_spikequad) | Генерирует усиленный Quad-эффект попадания гвоздя/шипа типа `TEDP_SPIKEQUAD`. | `void(vector org) te_spikequad = #413;` |
| [`te_superspikequad`](../37-quakec-builtins-reference/03-entity-world-builtins.md#te_superspikequad) | Генерирует временный сетевой эффект (temp-entity) `TEDP_SUPERSPIKEQUAD`, который представляет собой усиленную под действием Quad Damage версию попадания тяжелого гвоздя (superspike/supernail-эффект). | `void(vector org) te_superspikequad = #414;` |
| [`undefined`](../37-quakec-builtins-reference/03-entity-world-builtins.md#undefined) | Аналогично `empty`, метка `undefined` в исходниках здесь служит только комментарием-подписью к незанятым слотам таблицы. | `undefined` — это не рабочий builtin, а метка зарезервированных слотов под номерами `#458`, `#470`, `#505..#509` и `#539`. |

### Сеть и сетевые сообщения

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`WriteByte`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writebyte) | Пишет ровно 1 байт. | `void(float to, float val) WriteByte = #52;` |
| [`WriteChar`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writechar) | Удобен для маленьких signed-дельт: откат отдачи, смещение камеры, изменение счётчика в пределах одного байта. | `void(float to, float val) WriteChar = #53;` |
| [`WriteShort`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writeshort) | Занимает 2 байта и подходит для средних по диапазону чисел: HP, урон, score delta, индексы ресурсов, таймеры в миллисекундах. | `void(float to, float val) WriteShort = #54;` |
| [`WriteLong`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writelong) | Пишет 4 байта signed integer. | `void(float to, float val) WriteLong = #55;` |
| [`WriteAngle`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writeangle) | Записывает угол в сетевом формате движка. | `void(float to, float val) WriteAngle = #57;` |
| [`WriteCoord`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writecoord) | Кодирует число в координатном сетевом формате. | `void(float to, float val) WriteCoord = #56;` |
| [`WriteString`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writestring) | Передаёт строку переменной длины вместе с завершающим нулём. | `void(float to, string val) WriteString = #58;` |
| [`WriteEntity`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writeentity) | Передаёт сетевой индекс сущности. | `void(float to, entity val) WriteEntity = #59;` |
| [`WriteFloat`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writefloat) | Всегда пишет полный IEEE-like 32-битный float без промежуточного округления в `short`, `coord` или [`angle`](../39-entity-keys-reference/01-worldspawn-common-keys.md#angle). | `void(float buf, float fl) WriteFloat = #280;` |
| [`WritePicture`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writepicture) | Формально предназначен для передачи изображения по сети, но в FTEQW реализация упрощена: builtin фактически пишет строку имени картинки и затем размер `0`. | `void(float to, string s, float sz) WritePicture = #501;` |
| [`WriteUnterminatedString`](../37-quakec-builtins-reference/04-network-messages-builtins.md#writeunterminatedstring) | Записывает байты строки подряд, но не добавляет null terminator. | `void(float target, string str) WriteUnterminatedString = #456;` |
| [`readbyte`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readbyte) | Парный reader для `WriteByte`. | `float() readbyte = #360;` |
| [`readchar`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readchar) | Считывает ровно тот signed byte, который был записан `WriteChar`. | `float() readchar = #361;` |
| [`readshort`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readshort) | Читает значения, переданные `WriteShort`. | `float() readshort = #362;` |
| [`readlong`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readlong) | Предназначен для данных из `WriteLong` или `WriteInt`. | `float() readlong = #363;` |
| [`readangle`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readangle) | Возвращает уже развёрнутый угол в градусах, но точность этого значения ограничена исходным сетевым кодированием. | `float() readangle = #365;` |
| [`readcoord`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readcoord) | Скрывает детали negotiated protocol: fixed-point это, floatcoords или другой совместимый wire format. | `float() readcoord = #364;` |
| [`readfloat`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readfloat) | Используется только вместе с `WriteFloat`. | `float() readfloat = #367;` |
| [`readstring`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readstring) | Читает строку, записанную `WriteString`. | `string() readstring = #366;` |
| [`readentitynum`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readentitynum) | Единственный корректный парный reader для `WriteEntity`. | `float() readentitynum = #368;` |
| [`ReadPicture`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readpicture) | Читает payload от `WritePicture`. | `string() ReadPicture = #501;` |
| [`multicast`](../37-quakec-builtins-reference/04-network-messages-builtins.md#multicast) | Не записывает данные сам по себе, а **диспатчит** уже заполненный буфер `MSG_MULTICAST`. | `void(vector where, float set) multicast = #82;` |
| [`stuffcmd`](../37-quakec-builtins-reference/04-network-messages-builtins.md#stuffcmd) | Отправляет строку в клиентскую консоль на исполнение. | `void(entity client, string s, optional string s2, optional string s3, optional string s4, optional string s5, optional string s6, optional string s7) stuffcmd = #21;` |
| [`clientcommand`](../37-quakec-builtins-reference/04-network-messages-builtins.md#clientcommand) | На стороне SSQC выполняет команду так, как будто её прислал указанный клиент. | `void(entity e, string s) clientcommand = #440;` |
| [`sendevent`](../37-quakec-builtins-reference/04-network-messages-builtins.md#sendevent) | Это путь **CSQC → SSQC**. Он вызывает на сервере функцию вида `CSEv_<evname>_<evargs>`. | `void(string evname, string evargs, ...) sendevent = #359;` |
| [`sendpacket`](../37-quakec-builtins-reference/04-network-messages-builtins.md#sendpacket) | Посылает connectionless UDP-пакет и автоматически добавляет в начало четыре байта `255`. | `float(string destaddress, string content) sendpacket = #242;` |
| [`deltalisten`](../37-quakec-builtins-reference/04-network-messages-builtins.md#deltalisten) | Относится к CSQC и позволяет подписаться на сетевые entity updates, которые обычно обслуживает сам движок. | `float(string modelname, float(float isnew) updatecallback, float flags) deltalisten = #371;` |
| [`netaddress_resolve`](../37-quakec-builtins-reference/04-network-messages-builtins.md#netaddress_resolve) | Синхронно вызывает системное разрешение адреса и возвращает первый результат строкой в формате движка. | `string(string dnsname, optional float defport) netaddress_resolve = #625;` |
| [`getextresponse`](../37-quakec-builtins-reference/04-network-messages-builtins.md#getextresponse) | Относится к browser/server-list API, но текущая реализация FTEQW фактически является stub: и на клиентской стороне, и в серверной таблице builtin отмечен как пустой/заглушка. | `string() getextresponse = #624;` |
| [`redirectcmd`](../37-quakec-builtins-reference/04-network-messages-builtins.md#redirectcmd) | Выполняет серверную консольную команду и перенаправляет её текстовый вывод указанному клиенту. | `DEP void(entity to, string str) redirectcmd = #101;` |
| [`isserver`](../37-quakec-builtins-reference/04-network-messages-builtins.md#isserver) | Имя одно, но builtin встречается в двух контекстах: MenuQC (`#60`) и CSQC (`#350`). | `float() isserver = #60;` |
| [`clientcount`](../37-quakec-builtins-reference/04-network-messages-builtins.md#clientcount) | Нужен прежде всего MenuQC и имеет смысл только тогда, когда движок уже держит локальный сервер. | `float() clientcount = #61;` |
| [`clientstate`](../37-quakec-builtins-reference/04-network-messages-builtins.md#clientstate) | Клиентский builtin из MenuQC. | `float() clientstate = #62;` |
| [`clienttype`](../37-quakec-builtins-reference/04-network-messages-builtins.md#clienttype) | Работает в SSQC и различает четыре случая: `CLIENTTYPE_DISCONNECTED` (`0`), `CLIENTTYPE_REAL` (`1`), `CLIENTTYPE_BOT` (`2`) и `CLIENTTYPE_NOTACLIENT` (`3`). | `float(entity client) clienttype = #455;` |
| [`isdemo`](../37-quakec-builtins-reference/04-network-messages-builtins.md#isdemo) | Клиентский builtin для CSQC/MenuQC. | `float() isdemo = #349;` |
| [`isbackbuffered`](../37-quakec-builtins-reference/04-network-messages-builtins.md#isbackbuffered) | Серверный диагностический builtin. | `float(entity player) isbackbuffered = #234;` |
| [`csqc_cvar_defstring`](../37-quakec-builtins-reference/04-network-messages-builtins.md#csqc_cvar_defstring) | MenuQC-имя для того же builtin, который в CSQC обычно объявляется как [`cvar_defstring`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar_defstring) на номере `#482`. | `string(string s) csqc_cvar_defstring = #482;` |
| [`cvars_haveunsaved`](../37-quakec-builtins-reference/04-network-messages-builtins.md#cvars_haveunsaved) | Возвращает ненулевое значение, если у любого archived cvar текущее значение изменено, но ещё не сохранено в конфиг. | `float() cvars_haveunsaved = #0:cvars_haveunsaved;` |
| [`findkeysforcommand_dp`](../37-quakec-builtins-reference/04-network-messages-builtins.md#findkeysforcommand_dp) | Это deprecated DP-совместимое имя присутствует в серверной builtin-таблице FTEQW, но реальной серверной реализации у него нет: слот привязан к `PF_Fixme`. | `DEP string(string command, optional float bindmap) findkeysforcommand_dp = #610;` |
| [`findkeysforcommand_menu`](../37-quakec-builtins-reference/04-network-messages-builtins.md#findkeysforcommand_menu) | Совместимый alias к старому builtin формата DarkPlaces/MenuQC, который возвращает не имена клавиш, а список их числовых keycode-ов. | `string(string command, optional float bindmap) findkeysforcommand_menu = #610;` |
| [`findkeysforcommandex`](../37-quakec-builtins-reference/04-network-messages-builtins.md#findkeysforcommandex) | Ищет все клавиши, на которые привязана указанная команда, и возвращает результат в человекочитаемом keyname-формате. | `string(string command, optional float bindmap) findkeysforcommandex = #0:findkeysforcommandex;` |
| [`forceinfokeyblob`](../37-quakec-builtins-reference/04-network-messages-builtins.md#forceinfokeyblob) | Серверный вариант [`forceinfokey`](../37-quakec-builtins-reference/03-entity-world-builtins.md#forceinfokey), который пишет в userinfo произвольный blob, а не только обычную строку. | `void(entity player, string key, void *data, int size) forceinfokeyblob = #0:forceinfokeyblob;` |
| [`getlocaluserinfo`](../37-quakec-builtins-reference/04-network-messages-builtins.md#getlocaluserinfo) | Читает локальный userinfo выбранного seat прямо на клиенте. | `string(float seat, string keyname) getlocaluserinfo = #0:getlocaluserinfo;` |
| [`getlocaluserinfoblob`](../37-quakec-builtins-reference/04-network-messages-builtins.md#getlocaluserinfoblob) | Возвращает полный raw blob выбранного локального userinfo-ключа. | `int(float seat, string keyname, void *outptr, int maxsize) getlocaluserinfoblob = #0:getlocaluserinfoblob;` |
| [`getplayerkeyblob`](../37-quakec-builtins-reference/04-network-messages-builtins.md#getplayerkeyblob) | Читает raw userinfo-данные другого игрока без преобразования в tempstring. | `int(float playernum, string keyname, optional void *outptr, int size) getplayerkeyblob = #0:getplayerkeyblob;` |
| [`getplayerkeyfloat`](../37-quakec-builtins-reference/04-network-messages-builtins.md#getplayerkeyfloat) | Более дешёвая числовая версия `getplayerkeyvalue`, которая не создаёт tempstring только ради последующего [`stof`](../37-quakec-builtins-reference/02-string-builtins.md#stof). | `float(float playernum, string keyname, optional float assumevalue) getplayerkeyfloat = #0:getplayerkeyfloat;` |
| [`getplayerkeyvalue`](../37-quakec-builtins-reference/04-network-messages-builtins.md#getplayerkeyvalue) | Читает строковое значение из userinfo игрока и из нескольких клиентских псевдоключей scoreboard-системы. | `string(float playernum, string keyname) getplayerkeyvalue = #348;` |
| [`getplayerstat`](../37-quakec-builtins-reference/04-network-messages-builtins.md#getplayerstat) | Возвращает конкретный stat указанного игрока в том типе, который вы запросили через `EV_*`. | `__variant(float playernum, float statnum, float stattype) getplayerstat = #0:getplayerstat;` |
| [`readdouble`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readdouble) | Читает следующее значение из входящего сетевого сообщения как полноценный 64-битный `double`. | `__double() readdouble = #0:readdouble;` |
| [`readint`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readint) | Читает следующее 32-битное целое без промежуточного преобразования к QuakeC `float`. | `int() readint = #0:readint;` |
| [`readint64`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readint64) | Читает 64-битное signed integer из текущего входящего сообщения. | `__int64() readint64 = #0:readint64;` |
| [`readuint64`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readuint64) | Unsigned-вариант для чтения 64-битного целого, парный к `WriteUInt64`. | `__uint64() readuint64 = #0;` |
| [`serverkeyblob`](../37-quakec-builtins-reference/04-network-messages-builtins.md#serverkeyblob) | Бинарный вариант [`serverkey`](../37-quakec-builtins-reference/03-entity-world-builtins.md#serverkey), предназначенный для чтения raw serverinfo-значений, которые могут содержать нули и другие служебные байты. | `int(string key, optional void *ptr, int maxsize) serverkeyblob = #0:serverkeyblob;` |
| [`serverkeyfloat`](../37-quakec-builtins-reference/04-network-messages-builtins.md#serverkeyfloat) | Читает серверный ключ сразу как число и избегает лишней tempstring-аллокации. | `float(string key, optional float assumevalue) serverkeyfloat = #0:serverkeyfloat;` |
| [`setlocaluserinfo`](../37-quakec-builtins-reference/04-network-messages-builtins.md#setlocaluserinfo) | Меняет локальный userinfo выбранного seat так же, как консольная команда [`setinfo`](../44-cli-commands-reference/02-client-ui-commands.md#setinfo). | `void(float seat, string keyname, string newvalue) setlocaluserinfo = #0:setlocaluserinfo;` |
| [`setlocaluserinfoblob`](../37-quakec-builtins-reference/04-network-messages-builtins.md#setlocaluserinfoblob) | Записывает в локальный userinfo произвольный blob вместо обычной текстовой строки. | `void(float seat, string keyname, void *outptr, int size) setlocaluserinfoblob = #0:setlocaluserinfoblob;` |
| [`uri_get`](../37-quakec-builtins-reference/04-network-messages-builtins.md#uri_get) | Запускает асинхронную HTTP-загрузку и при завершении вызывает callback `URI_Get_Callback(reqid, responsecode, resourcebody, resourcebytes)`. | `float(string uril, float id, optional string postmimetype, optional string postdata) uri_get = #513;` |
| [`uri_post`](../37-quakec-builtins-reference/04-network-messages-builtins.md#uri_post) | Использует тот же builtin, что и `uri_get` — в заголовках FTE он даже объявлен как `#define uri_post uri_get` — но вызывает его в режиме HTTP POST. | `float(string uril, float id, optional string postmimetype, optional string postdata, optional float strbuf) uri_post = #513;` |

### Звук

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`sound`](../37-quakec-builtins-reference/05-sound-builtins.md#sound) | Основной builtin для игровых 3D-звуков. | `void(entity e, float chan, string samp, float vol, float atten, optional float speedpct, optional float flags, optional float timeofs) sound = #8;` |
| [`ambientsound`](../37-quakec-builtins-reference/05-sound-builtins.md#ambientsound) | Не запускает обычный «живой» канал, а добавляет статический ambient-источник, который клиенты получают при подключении к карте. | `void (vector pos, string samp, float vol, float atten) ambientsound = #74;` |
| [`localsound`](../37-quakec-builtins-reference/05-sound-builtins.md#localsound) | Воспроизводит звук только на локальном клиенте. | `void(string soundname, optional float channel, optional float volume) localsound = #177;` |
| [`pointsound`](../37-quakec-builtins-reference/05-sound-builtins.md#pointsound) | Воспроизводит звук из конкретной точки мира, не привязывая его к управляемому entity-каналу. | `void(vector origin, string sample, float volume, float attenuation) pointsound = #483;` |
| [`soundlength`](../37-quakec-builtins-reference/05-sound-builtins.md#soundlength) | Возвращает длительность sample в секундах. | `float(string sample) soundlength = #534;` |
| [`getsoundtime`](../37-quakec-builtins-reference/05-sound-builtins.md#getsoundtime) | Возвращает текущую позицию воспроизведения sample на заданном entity-канале. | `float(entity e, float channel) getsoundtime = #533;` |
| [`precache_sound`](../37-quakec-builtins-reference/05-sound-builtins.md#precache_sound) | Регистрирует sample в sound precache list, чтобы клиент знал о ресурсе и мог воспроизвести его без гонки с первой загрузкой. | `string(string s) precache_sound = #19;` |
| [`precache_sound2`](../37-quakec-builtins-reference/05-sound-builtins.md#precache_sound2) | По runtime-поведению эквивалентен `precache_sound`: движок обрабатывает его тем же builtin-кодом, регистрирует ресурс в precache list и делает звук доступным для последующих вызовов `sound`, `pointsound`, `ambientsound` и локальных client-side проигрываний. | `string(string str) precache_sound2 = #76;` |
| [`SetListener`](../37-quakec-builtins-reference/05-sound-builtins.md#setlistener) | Не запускает sample, а перенастраивает саму точку прослушивания аудио для текущего кадра CSQC. | `void(vector origin, vector forward, vector right, vector up, optional float reverbtype) SetListener = #351;` |
| [`getchannellevel`](../37-quakec-builtins-reference/05-sound-builtins.md#getchannellevel) | Это CSQC builtin для получения амплитуды громкости или уровня звука на заданном канале конкретного эдикта. | `float(entity e, float channel) getchannellevel = #0:getchannellevel;` |
| [`getqueuedaudiotime`](../37-quakec-builtins-reference/05-sound-builtins.md#getqueuedaudiotime) | Возвращает общее время звучания (в секундах) аудиоданных, которые в данный момент находятся в очереди буфера `queueaudio` для потокового воспроизведения. | `float() getqueuedaudiotime = #0:getqueuedaudiotime;` |
| [`getsoundindex`](../37-quakec-builtins-reference/05-sound-builtins.md#getsoundindex) | Служит для получения сетевого индекса звука из глобальной таблицы предкешированных ресурсов (sound precache table). | `float(string soundname, optional float queryonly) getsoundindex = #0:getsoundindex;` |
| [`queueaudio`](../37-quakec-builtins-reference/05-sound-builtins.md#queueaudio) | Осуществляет потоковую запись и микширование пользовательских PCM-данных в обход стандартных статических триггеров звуков `sound()` / `ambientsound()`. | `float(int hz, int channels, int type, void *data, unsigned int frames) queueaudio = #0:queueaudio;` |
| [`setup_reverb`](../37-quakec-builtins-reference/05-sound-builtins.md#setup_reverb) | Инициализирует настройки параметров среды или окружения (эхо, затухание, плотность звука) для последующей активации этой зоны через встроенную функцию `SetListener`. | `void(float reverbslot, reverbinfo_t *reverbinfo, int sizeofreverbinfo_t) setup_reverb = #0:setup_reverb;` |
| [`soundnameforindex`](../37-quakec-builtins-reference/05-sound-builtins.md#soundnameforindex) | Выполняет операцию, противоположную `getsoundindex`: по переданному сетевому индексу прекеша она возвращает строковый путь к файлу (sound resource). | `string(float sndindex) soundnameforindex = #0:soundnameforindex;` |
| [`soundupdate`](../37-quakec-builtins-reference/05-sound-builtins.md#soundupdate) | Динамически изменяет параметры уже воспроизводимого звука на конкретном канале выбранной сущности. | `float(entity e, float channel, string newsample, float volume, float attenuation, float pitchpct, float flags, float timeoffset) soundupdate = #0:soundupdate;` |
| [`stopsound`](../37-quakec-builtins-reference/05-sound-builtins.md#stopsound) | [`stopsound`](../44-cli-commands-reference/03-rendering-sound-commands.md#stopsound) немедленно прерывает воспроизведение текущего звукового семпла на выбранном канале конкретной сущности. | `void(entity ent, float channel) stopsound = #0:stopsound;` |

### Файлы, буферы, хеш-таблицы и базы данных

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`fopen`](../37-quakec-builtins-reference/06-files-database-builtins.md#fopen) | Возвращает числовой `filestream`-хендл или отрицательное значение при ошибке. | `filestream(string filename, float mode, optional float mmapminsize) fopen = #110;` |
| [`fclose`](../37-quakec-builtins-reference/06-files-database-builtins.md#fclose) | Завершает работу с файлом и освобождает внутренние ресурсы движка. | `void(filestream fhandle) fclose = #111;` |
| [`fgets`](../37-quakec-builtins-reference/06-files-database-builtins.md#fgets) | В режиме `FILE_READ` функция возвращает следующую строку без завершающего символа новой строки; пустая строка из файла остаётся обычной пустой строкой, а конец файла возвращает null string, поэтому EOF удобно проверять через `if (!line)`. | `string(filestream fhandle) fgets = #112;` |
| [`fputs`](../37-quakec-builtins-reference/06-files-database-builtins.md#fputs) | Пишет текст в текущую позицию файла или в конец буфера при `FILE_APPEND`. | `void(filestream fhandle, string s, optional string s2, optional string s3, optional string s4, optional string s5, optional string s6, optional string s7) fputs = #113;` |
| [`fexists`](../37-quakec-builtins-reference/06-files-database-builtins.md#fexists) | Проверяет наличие файла именно в стандартном writable location, а не во всём виртуальном файловом дереве движка. | `float(string fname) fexists = #653;` |
| [`fcopy`](../37-quakec-builtins-reference/06-files-database-builtins.md#fcopy) | Копирует содержимое файла, не заставляя QuakeC вручную читать и писать блоки данных. | `float(string src, string dst) fcopy = #650;` |
| [`fremove`](../37-quakec-builtins-reference/06-files-database-builtins.md#fremove) | Удаляет файл и возвращает `0` при успехе. | `float(string fname) fremove = #652;` |
| [`frename`](../37-quakec-builtins-reference/06-files-database-builtins.md#frename) | Пытается переименовать или переместить файл внутри writable области и возвращает `0` при успехе. | `float(string src, string dst) frename = #651;` |
| [`rmtree`](../37-quakec-builtins-reference/06-files-database-builtins.md#rmtree) | По комментариям интерфейса `rmtree` задумывалась как опасная, но всё равно sandboxed операция для рекурсивного удаления дерева `data/`. | `float(string path) rmtree = #654;` |
| [`writetofile`](../37-quakec-builtins-reference/06-files-database-builtins.md#writetofile) | Сохраняет поля одной сущности в текстовом формате, совместимом с `.ent`/savegame-представлением движка. | `void(filestream fh, entity e) writetofile = #606;` |
| [`loadfromfile`](../37-quakec-builtins-reference/06-files-database-builtins.md#loadfromfile) | Читает файл с последовательностью entity blocks и вызывает штатный механизм `restoreent`, создавая или восстанавливая сущности по данным из файла. | `void(string s) loadfromfile = #530;` |
| [`loadfromdata`](../37-quakec-builtins-reference/06-files-database-builtins.md#loadfromdata) | Делает то же самое, что `loadfromfile`, но читает данные не с диска, а из уже готовой строки. | `void(string s) loadfromdata = #529;` |
| [`whichpack`](../37-quakec-builtins-reference/06-files-database-builtins.md#whichpack) | Сообщает, из какого pak/pk3 или другого пакета движок реально загрузил указанный ресурс. | `string(string filename, optional enumflags:float{WP_REFERENCEPACKAGE,WP_FULLPACKAGEPATH} flags) whichpack = #503;` |
| [`search_begin`](../37-quakec-builtins-reference/06-files-database-builtins.md#search_begin) | Запускает перечисление файлов и возвращает числовой `searchhandle`, который затем используется в `search_getsize`, `search_getfilename` и `search_end`. | `searchhandle(string pattern, enumflags:float{SB_CASEINSENSITIVE=1<<0,SB_FULLPACKAGEPATH=1<<1,SB_ALLOWDUPES=1<<2,SB_FORCESEARCH=1<<3,SB_MULTISEARCH=1<<4} flags, float quiet, optional string filterpackage) search_begin = #444;` |
| [`search_end`](../37-quakec-builtins-reference/06-files-database-builtins.md#search_end) | Освобождает результат файлового поиска. | `void(searchhandle handle) search_end = #445;` |
| [`search_getsize`](../37-quakec-builtins-reference/06-files-database-builtins.md#search_getsize) | Функция возвращает число найденных элементов в результате `search_begin`. | `float(searchhandle handle) search_getsize = #446;` |
| [`search_getfilename`](../37-quakec-builtins-reference/06-files-database-builtins.md#search_getfilename) | Возвращает имя файла по индексу внутри результата поиска. | `string(searchhandle handle, float num) search_getfilename = #447;` |
| [`buf_create`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_create) | Создаёт пустой строковый буфер и возвращает его числовой `strbuf`-хендл. | `strbuf() buf_create = #460;` |
| [`buf_del`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_del) | Освобождает сам буфер и все строки внутри него. | `void(strbuf bufhandle) buf_del = #461;` |
| [`buf_getsize`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_getsize) | Возвращает текущую «длину» буфера — индекс последнего используемого элемента плюс один. | `float(strbuf bufhandle) buf_getsize = #462;` |
| [`buf_copy`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_copy) | Полностью очищает буфер назначения и затем копирует в него все строки из источника. | `void(strbuf bufhandle_from, strbuf bufhandle_to) buf_copy = #463;` |
| [`buf_loadfile`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_loadfile) | Открывает файл через ту же файловую песочницу, что и `fopen`, читает его построчно и добавляет строки в конец существующего буфера. | `float(string filename, strbuf bufhandle) buf_loadfile = #535;` |
| [`buf_writefile`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_writefile) | Выгружает строки буфера в уже открытый файл, автоматически добавляя `\n` после каждой непустой записи. | `float(filestream filehandle, strbuf bufhandle, optional float startpos, optional float numstrings) buf_writefile = #536;` |
| [`buf_sort`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_sort) | Сортирует строки внутри буфера и перед этим удаляет из активной части массива все `NULL`-дыры, оставшиеся после `bufstr_free`. | `void(strbuf bufhandle, float sortprefixlen, float backward) buf_sort = #464;` |
| [`buf_implode`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_implode) | Склеивает все непустые строки буфера в одну temp string, вставляя `glue` между соседними элементами. | `string(strbuf bufhandle, string glue) buf_implode = #465;` |
| [`buf_cvarlist`](../37-quakec-builtins-reference/06-files-database-builtins.md#buf_cvarlist) | Полностью очищает целевой буфер и заполняет его именами cvar-ов, прошедших фильтрацию. | `void(strbuf strbuf, string pattern, string antipattern) buf_cvarlist = #517;` |
| [`bufstr_add`](../37-quakec-builtins-reference/06-files-database-builtins.md#bufstr_add) | Добавляет строку в буфер и возвращает индекс, куда она попала. | `float(strbuf bufhandle, string str, float ordered) bufstr_add = #468;` |
| [`bufstr_free`](../37-quakec-builtins-reference/06-files-database-builtins.md#bufstr_free) | Удаляет конкретную строку и превращает слот в пустую дыру, но не сдвигает остальные элементы. | `void(strbuf bufhandle, float string_index) bufstr_free = #469;` |
| [`bufstr_get`](../37-quakec-builtins-reference/06-files-database-builtins.md#bufstr_get) | Возвращает строку по индексу или null string, если индекс пустой, вышел за диапазон или handle неверен. | `string(strbuf bufhandle, float string_index) bufstr_get = #466;` |
| [`bufstr_set`](../37-quakec-builtins-reference/06-files-database-builtins.md#bufstr_set) | Записывает строку по конкретному индексу, при необходимости расширяя внутренний массив и заполняя промежуточные элементы пустыми слотами. | `void(strbuf bufhandle, float string_index, string str) bufstr_set = #467;` |
| [`bufstr_find`](../37-quakec-builtins-reference/06-files-database-builtins.md#bufstr_find) | Ищет первое совпадение и возвращает его индекс либо `-1`, если ничего не найдено. | `float(float bufhandle, string match, float matchrule, float startpos, float step) bufstr_find = #537;` |
| [`hash_createtab`](../37-quakec-builtins-reference/06-files-database-builtins.md#hash_createtab) | Создаёт хеш-таблицу key-value и возвращает её handle. | `hashtable(float tabsize, optional float defaulttype) hash_createtab = #287;` |
| [`hash_destroytab`](../37-quakec-builtins-reference/06-files-database-builtins.md#hash_destroytab) | Удаляет таблицу и все пары ключ-значение внутри неё. Вызывать её нужно только для обычных таблиц, созданных QuakeC; специальный `gamestate` уничтожать не нужно и не следует. | `void(hashtable table) hash_destroytab = #288;` |
| [`hash_add`](../37-quakec-builtins-reference/06-files-database-builtins.md#hash_add) | Вставляет ключ и значение в таблицу. | `void(hashtable table, string name, __variant value, optional float typeandflags) hash_add = #289;` |
| [`hash_delete`](../37-quakec-builtins-reference/06-files-database-builtins.md#hash_delete) | Удаляет запись и возвращает её значение как `__variant`. | `__variant(hashtable table, string name) hash_delete = #291;` |
| [`hash_get`](../37-quakec-builtins-reference/06-files-database-builtins.md#hash_get) | Ищет значение по имени ключа и возвращает либо найденный `__variant`, либо `deflt`. | `__variant(hashtable table, string name, optional __variant deflt, optional float requiretype, optional float index) hash_get = #290;` |
| [`hash_getkey`](../37-quakec-builtins-reference/06-files-database-builtins.md#hash_getkey) | Возвращает некоторое имя ключа по индексу, но этот порядок не считается стабильным. | `string(hashtable table, float idx) hash_getkey = #292;` |
| [`memalloc`](../37-quakec-builtins-reference/06-files-database-builtins.md#memalloc) | Выделяет адресуемый блок памяти внутри модели памяти QC и возвращает указатель. | `__variant*(int size) memalloc = #384;` |
| [`memfree`](../37-quakec-builtins-reference/06-files-database-builtins.md#memfree) | Освобождает блок адресуемой памяти. | `void(__variant *ptr) memfree = #385;` |
| [`memcpy`](../37-quakec-builtins-reference/06-files-database-builtins.md#memcpy) | Копирует байты между двумя адресуемыми областями памяти; движок использует безопасное поведение уровня `memmove`, поэтому перекрывающиеся диапазоны не страшны. | `void(__variant *dst, __variant *src, int size) memcpy = #386;` |
| [`memfill8`](../37-quakec-builtins-reference/06-files-database-builtins.md#memfill8) | Записывает один и тот же байт на всём диапазоне. | `void(__variant *dst, int val, int size) memfill8 = #387;` |
| [`memgetval`](../37-quakec-builtins-reference/06-files-database-builtins.md#memgetval) | Читает 32-битное значение по адресу `dst + ofs` и возвращает его как `__variant`. | `__variant(__variant *dst, float ofs) memgetval = #388;` |
| [`memsetval`](../37-quakec-builtins-reference/06-files-database-builtins.md#memsetval) | Записывает одно 32-битное значение по смещению от указателя. | `void(__variant *dst, float ofs, __variant val) memsetval = #389;` |
| [`memptradd`](../37-quakec-builtins-reference/06-files-database-builtins.md#memptradd) | Выполняет арифметику указателей и возвращает новый адрес `base + ofs`. | `__variant*(__variant *base, float ofs) memptradd = #390;` |
| [`sqlconnect`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlconnect) | Открывает соединение с доступным SQL-драйвером и возвращает `serveridx` либо `-1`, если драйвер недоступен или подключение не удалось. | `float(optional string host, optional string user, optional string pass, optional string defaultdb, optional string driver) sqlconnect = #250;` |
| [`sqldisconnect`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqldisconnect) | Закрывает SQL-соединение и просит рабочий поток прекратить обработку запросов. | `void(float serveridx) sqldisconnect = #251;` |
| [`sqlopenquery`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlopenquery) | Отправляет SQL-запрос асинхронно и возвращает `queryidx` либо `-1`, если запрос даже не удалось поставить в очередь. | `float(float serveridx, void(float serveridx, float queryidx, float rows, float columns, float eof, float firstrow) callback, float querytype, string query) sqlopenquery = #252;` |
| [`sqlclosequery`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlclosequery) | Освобождает результаты persistent-запроса и снимает внутренние структуры движка. | `void(float serveridx, float queryidx) sqlclosequery = #253;` |
| [`sqlreadfield`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlreadfield) | Читает ячейку результата как строку. | `string(float serveridx, float queryidx, float row, float column) sqlreadfield = #254;` |
| [`sqlreadfloat`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlreadfloat) | Делает то же, что `sqlreadfield`, но сразу конвертирует содержимое ячейки в число через движковый `atof`. | `float(float serveridx, float queryidx, float row, float column) sqlreadfloat = #258;` |
| [`sqlerror`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlerror) | Возвращает текст последней ошибки SQL-подсистемы. | `string(float serveridx, optional float queryidx) sqlerror = #255;` |
| [`sqlescape`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlescape) | Подготавливает строку для безопасной вставки внутрь SQL-литерала. | `string(float serveridx, string data) sqlescape = #256;` |
| [`sqlversion`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlversion) | Возвращает строку вида `sqlite: ...` или `mysql: ...` с версией активного драйвера/клиентской библиотеки. | `string(float serveridx) sqlversion = #257;` |
| [`digest_hex`](../37-quakec-builtins-reference/06-files-database-builtins.md#digest_hex) | Вычисляет хеш от склеенного текста и возвращает его в hex-виде строчными буквами. | `string(string digest, string data, ...) digest_hex = #639;` |
| [`fork`](../37-quakec-builtins-reference/06-files-database-builtins.md#fork) | Это не OS-level процесс, а механизм ветвления выполнения QuakeC в SSQC. | `float(optional float sleeptime) fork = #210;` |
| [`sleep`](../37-quakec-builtins-reference/06-files-database-builtins.md#sleep) | Приостанавливает текущий поток выполнения QuakeC и позволяет остальному игровому коду продолжать работу. | `void(float sleeptime) sleep = #212;` |
| [`createbuffer`](../37-quakec-builtins-reference/06-files-database-builtins.md#createbuffer) | Выполняет временное выделение памяти, возвращая сырой указатель на созданный неструктурированный буфер. | `void*(int bytes) createbuffer = #0:createbuffer;` |
| [`digest_ptr`](../37-quakec-builtins-reference/06-files-database-builtins.md#digest_ptr) | Производит расчет криптографических контрольных сумм и хешей напрямую из сырой оперативной памяти, обрабатывая произвольные бинарные блоки (binary blobs) без необходимости сохранять или преобразовывать их в формат QC-строк. | `string(string digest, void *data, int length, optional int offset) digest_ptr = #0:digest_ptr;` |
| [`fread`](../37-quakec-builtins-reference/06-files-database-builtins.md#fread) | Выполняет низкоуровневое чтение байтового потока с текущей позиции открытого файла и записывает данные непосредственно по указанному адресу в блоке оперативной памяти. | `int(filestream fhandle, void *ptr, int size, optional int offset) fread = #0:fread;` |
| [`fseek`](../37-quakec-builtins-reference/06-files-database-builtins.md#fseek) | Перемещает внутренний курсор (указатель) файла. | `int(filestream fhandle, optional int newoffset) fseek = #0:fseek;` |
| [`fseek64`](../37-quakec-builtins-reference/06-files-database-builtins.md#fseek64) | Представляет собой 64-битную версию `fseek`, созданную для корректной работы с файлами огромного размера и mmap/write-архивами. | `__int64(filestream fhandle, optional __int64 newoffset) fseek64 = #0:fseek64;` |
| [`fsize`](../37-quakec-builtins-reference/06-files-database-builtins.md#fsize) | Возвращает текущий физический размер файла в байтах. | `int(filestream fhandle, optional int newsize) fsize = #0:fsize;` |
| [`fsize64`](../37-quakec-builtins-reference/06-files-database-builtins.md#fsize64) | Выполняет те же базовые задачи, что и `fsize`, но используется в тех сценариях, где размеры данных выходят за рамки классического 32-битного ограничения. | `__int64(filestream fhandle, optional __int64 newsize) fsize64 = #0:fsize64;` |
| [`fwrite`](../37-quakec-builtins-reference/06-files-database-builtins.md#fwrite) | Производит низкоуровневую запись байтового потока из оперативной памяти в открытый файл и возвращает количество успешно записанных байт. | `int(filestream fhandle, void *ptr, int size, optional int offset) fwrite = #0:fwrite;` |
| [`hash_getcb`](../37-quakec-builtins-reference/06-files-database-builtins.md#hash_getcb) | По изначальной спецификации `hash_getcb` должна была вызывать переданный колбэк для перебора всех элементов таблицы либо для поиска элемента по конкретному ключу `name`. | `void(hashtable table, void(string keyname, __variant val) callback, optional string name) hash_getcb = #293;` |
| [`json_find_object_child`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_find_object_child) | Выполняет поиск дочернего элемента внутри JSON-объекта по его строковому имени и возвращает дескриптор найденного узла типа `jsonnode`, либо значение `__NULL__`, если свойство с таким именем отсутствует. | `jsonnode(jsonnode node, string name) json_find_object_child = #0:json_find_object_child;` |
| [`json_free`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_free) | Рекурсивно освобождает оперативную память, выделенную под хранение всего JSON-дерева, включая все вложенные объекты, массивы и значения. | `void(jsonnode node) json_free = #0:json_free;` |
| [`json_get_child_at_index`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_get_child_at_index) | Возвращает дескриптор N-го дочернего элемента внутри JSON-массива или объекта. | `jsonnode(jsonnode node, int childindex) json_get_child_at_index = #0:json_get_child_at_index;` |
| [`json_get_float`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_get_float) | Извлекает скалярное значение из JSON-узла и приводит его к типу с плавающей точкой `float`. | `float(jsonnode node) json_get_float = #0:json_get_float;` |
| [`json_get_integer`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_get_integer) | Считывает содержимое узла и возвращает его в виде целого числа. | `int(jsonnode node) json_get_integer = #0:json_get_integer;` |
| [`json_get_length`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_get_length) | Возвращает общее число дочерних элементов в массиве или полей в объекте. | `int(jsonnode node) json_get_length = #0:json_get_length;` |
| [`json_get_name`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_get_name) | Возвращает имя ключа (свойства), которому принадлежит текущий дочерний узел в JSON-объекте. | `string(jsonnode node) json_get_name = #0:json_get_name;` |
| [`json_get_string`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_get_string) | Извлекает текстовое содержимое из JSON-узла, имеющего строго тип `JSON_TYPE_STRING`. | `string(jsonnode node) json_get_string = #0:json_get_string;` |
| [`json_get_value_type`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_get_value_type) | Возвращает базовый тип данных узла в виде перечисления: `JSON_TYPE_STRING`, `JSON_TYPE_NUMBER`, `JSON_TYPE_OBJECT`, `JSON_TYPE_ARRAY`, `JSON_TYPE_TRUE`, `JSON_TYPE_FALSE` или `JSON_TYPE_NULL`. | `json_type_e(jsonnode node) json_get_value_type = #0:json_get_value_type;` |
| [`json_parse`](../37-quakec-builtins-reference/06-files-database-builtins.md#json_parse) | `json_get_value_type` парсит JSON-строку и возвращает дескриптор корневого узла `jsonnode`, либо значение `__NULL__`, если в процессе разбора произошла синтаксическая ошибка. | `jsonnode(string data) json_parse = #0:json_parse;` |
| [`memcmp`](../37-quakec-builtins-reference/06-files-database-builtins.md#memcmp) | Производит побайтовое сравнение двух областей оперативной памяти и возвращает `0`, если содержимое полностью идентично; отрицательное или положительное значение возвращается в случае несовпадения байт по аналогии с классической функцией Си `memcmp`. | `int(__variant *dst, __variant *src, int size, optional int srcoffset, optional int dstoffset) memcmp = #0:memcmp;` |
| [`memrealloc`](../37-quakec-builtins-reference/06-files-database-builtins.md#memrealloc) | Изменяет размер ранее выделенного блока памяти, стараясь расширить его по текущему адресу, либо переносит данные в новую область кучи, сохраняя старое содержимое в пределах минимального из двух размеров. | `__variant*(void *oldptr, int newsize) memrealloc = #0:memrealloc;` |
| [`memstrsize`](../37-quakec-builtins-reference/06-files-database-builtins.md#memstrsize) | Отличается от стандартной [`strlen`](../37-quakec-builtins-reference/02-string-builtins.md#strlen) тем, что она замеряет чистый объем сырых байт (raw bytes) UTF-8 строки, а не количество отображаемых графических символов. | `float(string s) memstrsize = #0:memstrsize;` |
| [`search_fopen`](../37-quakec-builtins-reference/06-files-database-builtins.md#search_fopen) | Открывает дескриптор файла напрямую из списка результатов поиска, избавляя от необходимости вручную формировать строковый путь. | `filestream(searchhandle handle, float num) search_fopen = #0:search_fopen;` |
| [`search_getfilemtime`](../37-quakec-builtins-reference/06-files-database-builtins.md#search_getfilemtime) | Возвращает дату и время последнего изменения указанного файла в виде стандартизированной строки формата `YYYY-MM-DD HH:MM:SS` по часовому поясу локального хоста. | `string(searchhandle handle, float num) search_getfilemtime = #0:search_getfilemtime;` |
| [`search_getfilesize`](../37-quakec-builtins-reference/06-files-database-builtins.md#search_getfilesize) | Возвращает физический размер найденного файла в байтах. | `float(searchhandle handle, float num) search_getfilesize = #0:search_getfilesize;` |
| [`search_getpackagename`](../37-quakec-builtins-reference/06-files-database-builtins.md#search_getpackagename) | Определяет, из какого именно архива (package), gamedir-папки или пака был загружен или обнаружен конкретный файл. | `string(searchhandle handle, float num) search_getpackagename = #0:search_getpackagename;` |
| [`sqlescapeblob`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlescapeblob) | Конвертирует бинарные данные из оперативной памяти в валидный текстовый литерал SQL-запроса вида `x'DEADBEEF'`, который можно безопасно вставлять в `INSERT` или `UPDATE` команды СУБД. | `string(float serveridx, __variant *ptr, int maxsize) sqlescapeblob = #0:sqlescapeblob;` |
| [`sqlreadblob`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlreadblob) | Считывает бинарные данные (BLOB) из указанной ячейки текущей выборки базы данных и копирует их напрямую в оперативную память QuakeC, возвращая фактическое количество полученных байт. | `int(float serveridx, float queryidx, float row, float column, __variant *ptr, int maxsize) sqlreadblob = #0:sqlreadblob;` |

### Прекэш и игровые ресурсы

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`precache_file`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#precache_file) | В обычном SSQC/CSQC-варианте `precache_file` в FTEQW не формирует сетевой индекс и вообще ничего не загружает во время игры: это исторический builtin-подсказка для старых инструментов сборки pak-файлов. | `string(string s) precache_file = #68;` |
| [`precache_file2`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#precache_file2) | Исторический дубль `precache_file`. | `string(string str) precache_file2 = #77;` |
| [`precache_model`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#precache_model) | Добавляет модель в общий precache-список и тем самым закрепляет за ней числовой индекс, который потом используют [`setmodel`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setmodel), сетевые entity update и сопутствующие builtin-lookup функции. | `string(string s) precache_model = #20;` |
| [`precache_model2`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#precache_model2) | В FTEQW `precache_model2` использует ту же runtime-логику, что и `precache_model`: модель попадает в тот же precache-поток, получает индекс и подчиняется тем же требованиям по spawn-time вызову. | `string(string str) precache_model2 = #75;` |
| [`precache_pic`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#precache_pic) | Относится не к сетевому entity precache сервера, а к клиентским/UI-ресурсам: она заставляет движок заранее найти и загрузить указанное изображение, чтобы первый [`drawpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawpic)/[`showpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#showpic) не упёрся в внезапную декомпрессию, чтение с диска или докачку. | `string(string name, optional float flags) precache_pic = #317;` |
| [`precache_vwep_model`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#precache_vwep_model) | Специализированный SSQC builtin из расширения `ZQ_VWEP`, предназначенный для регистрации **visible weapon models**: третье лицо видит оружие в руках другого игрока не через обычный `v_`-viewmodel, а через отдельную сетевую VWEP-таблицу. | `float(string mname) precache_vwep_model = #532;` |
| [`getmodelindex`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#getmodelindex) | Это удобная сокращённая форма для сценария «обеспечь precache модели и верни её числовой индекс». Документация движка прямо описывает его как альтернативу связке `precache_model(foo); setmodel(bar, foo); return bar.modelindex;`. | `float(string modelname, optional float queryonly) getmodelindex = #200;` |
| [`modelnameforindex`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#modelnameforindex) | Делает обратное преобразование: по уже известному model index возвращает строковое имя модели. | `string(float mdlindex) modelnameforindex = #334;` |
| [`frameforname`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#frameforname) | Нужен не для старых безымянных покадровых `.mdl`, а прежде всего для современных моделей, где анимации приходят как **именованные группы**: `idle`, `walk`, `run`, `attack_melee`, `death_back` и т.п. | `float(float modidx, string framename) frameforname = #276;` |
| [`frametoname`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#frametoname) | Обратная операция к `frameforname`: по номеру framegroup она возвращает его текстовое имя. | `string(float modidx, float framenum) frametoname = #284;` |
| [`frameduration`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#frameduration) | Возвращает длительность всей framegroup в секундах. | `float(float modidx, float framenum) frameduration = #277;` |
| [`skinforname`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#skinforname) | Превращает строковое имя skin-варианта в числовой индекс, который потом можно записать в поле `.skin`. | `float(float mdlindex, string skinname) skinforname = #237;` |
| [`skintoname`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#skintoname) | Делает обратный lookup для skin-индексов. | `string(float modidx, float skin) skintoname = #285;` |
| [`shaderforname`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#shaderforname) | Регистрирует shader и возвращает числовой handle. | `float(string shadername, optional string defaultshader, ...) shaderforname = #238;` |
| [`findfont`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#findfont) | Ищет уже зарегистрированный font slot по имени. | `float(string s) findfont = #356;` |
| [`loadfont`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#loadfont) | [`loadfont`](../44-cli-commands-reference/02-client-ui-commands.md#loadfont) регистрирует новый font slot или переопределяет существующий. | `float(string fontname, string fontmaps, string sizes, float slot, optional float fix_scale, optional float fix_voffset) loadfont = #357;` |
| [`changepic`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#changepic) | Относится к расширению `TEI_SHOWLMP2` и работает на сервере как команда клиентскому HUD-слою: поменять уже показанную картинку в named slot, не трогая позицию и зону привязки. | `DEP_CSQC void(string slot, string picname, optional entity player) changepic = #107;` |
| [`drawgetimagesize`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#drawgetimagesize) | Возвращает ширину и высоту изображения в виде `vector`, где значимы `x` и `y`. | `vector(string picname) drawgetimagesize = #318;` |
| [`iscachedpic`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#iscachedpic) | Даёт быстрый эвристический ответ на вопрос «знает ли движок сейчас эту картинку». Именно эвристический: комментарий в `fteextensions.qc` предупреждает, что разные движки могут «врать» или сохранять кэш между картами. | `float(string name) iscachedpic = #316;` |
| [`freepic`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#freepic) | По документации `freepic` сообщает движку, что указанное изображение больше не нужно и при следующем обращении должно выглядеть как «новое». Концептуально это симметричная пара к `precache_pic`: вы заранее подгрузили редкую картинку меню, попользовались ею и потом позволили движку освободить её. Но здесь важно не переоценивать жёсткость контракта: клиентская реализация FTEQW трактует вызов скорее как подсказку, а не как гарантированное немедленное освобождение памяти/VRAM. | `void(string name) freepic = #319;` |
| [`addprogs`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#addprogs) | Загружает ещё один progs-модуль в уже работающую виртуальную машину и возвращает handle, который затем используют [`externcall`](../37-quakec-builtins-reference/03-entity-world-builtins.md#externcall), [`externset`](../37-quakec-builtins-reference/03-entity-world-builtins.md#externset) и [`externvalue`](../37-quakec-builtins-reference/03-entity-world-builtins.md#externvalue). | `float(string progsname) addprogs = #202;` |
| [`frameforaction`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#frameforaction) | Ищет у модели анимацию, помеченную указанным `actionid`, и возвращает случайный подходящий framegroup/animation index. | `float(float modidx, int actionid) frameforaction = #0:frameforaction;` |
| [`getmodeleventidx`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#getmodeleventidx) | Обращается к событию по его индексу внутри конкретной анимации и возвращает `true`, если такое событие существует. | `float(float modidx, float framenum, int eventidx, __out float timestamp, __out int code, __out string data) getmodeleventidx = #0:getmodeleventidx;` |
| [`getnextmodelevent`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#getnextmodelevent) | Ищет ближайшее следующее событие анимации между `basetime` и `targettime` и возвращает булево значение успеха. | `float(float modidx, float framenum, __inout float basetime, float targettime, __out int code, __out string data) getnextmodelevent = #0:getnextmodelevent;` |
| [`modelframecount`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#modelframecount) | Возвращает количество кадров/анимационных групп, которые движок видит у указанной модели. | `float(float mdlidx) modelframecount = #0:modelframecount;` |
| [`processmodelevents`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#processmodelevents) | Пакетный вариант обхода model events: вместо одного ближайшего события builtin вызывает ваш callback для **каждого** события, достигнутого между `basetime` и `targettime`, а затем выставляет `basetime = targettime`. | `void(float modidx, float framenum, __inout float basetime, float targettime, void(float timestamp, int code, string data) callback) processmodelevents = #0:processmodelevents;` |
| [`spriteframe`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#spriteframe) | CSQC builtin для случаев, когда вам нужен не scene-entity, а готовое имя shader'а конкретного кадра спрайта, пригодное для [`drawpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawpic), [`R_BeginPolygon`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#r_beginpolygon) и похожих 2D/overlay-рендер путей. | `string(string modelname, int frame, float frametime) spriteframe = #0:spriteframe;` |

### Рендеринг и сцена CSQC

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`addentity`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#addentity) | Копирует рендер-поля указанной entity в новый rentity, который будет нарисован при следующем вызове `renderscene`. | `void(entity ent) addentity = #302;` |
| [`addentities`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#addentities) | Builtin проходит по CSQC entity и для каждой проверяет `ent.drawmask & mask`. | `void(float mask) addentities = #301;` |
| [`clearscene`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#clearscene) | Забывает все rentity, полигоны и временные (добавленные за кадр) динамические источники света, а также сбрасывает все свойства вида (`setproperty`) к значениям по умолчанию (полноэкранный вьюпорт, стандартный [fov](../38-cvars-reference/01-video-rendering-cvars.md#fov) и т.д.). | `void() clearscene = #300;` |
| [`renderscene`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#renderscene) | Рисует все entity, полигоны и партиклы, накопленные в списке рентити через `addentity`/`addentities`/`R_BeginPolygon`, используя свойства вида, заданные `setproperty` (позиция камеры, углы, fov, вьюпорт). | `void() renderscene = #304;` |
| [`getproperty`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#getproperty) | Возвращает текущее значение указанного свойства вида — как правило, то, что ранее было установлено `setproperty`, либо значение по умолчанию движка. | `__variant(float property) getproperty = #309;` (алиас `getviewprop`) |
| [`setproperty`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#setproperty) | Переопределяет параметры вида по умолчанию: вьюпорт (`VF_MIN`/`VF_SIZE`), позицию и углы камеры (`VF_ORIGIN`, `VF_ANGLES`), поле зрения (`VF_FOV`), а также включает или отключает отрисовку HUD движком (`VF_DRAWENGINESBAR`, `VF_DRAWCROSSHAIR` и др.). | `float(float property, ...) setproperty = #303;` (алиас `setviewprop`) |
| [`getresolution`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#getresolution) | Возвращает один режим из внутреннего списка FTEQW, а не делает прямой опрос видеодрайвера. | `vector(float vidmode, optional float forfullscreen) getresolution = #608;` |
| [`R_BeginPolygon`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#r_beginpolygon) | Открывает построение произвольного полигона: указывает шейдер и режим, в котором будут интерпретироваться последующие вызовы `R_PolygonVertex`. | `void(string texturename, optional float flags, optional float is2d) R_BeginPolygon = #306;` |
| [`R_EndPolygon`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#r_endpolygon) | Завершает построение текущего примитива, начатого `R_BeginPolygon`. | `void() R_EndPolygon = #308;` |
| [`R_PolygonVertex`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#r_polygonvertex) | Добавляет очередную вершину к полигону, открытому `R_BeginPolygon`. | `void(vector org, vector texcoords, vector rgb, float alpha) R_PolygonVertex = #307;` |
| [`drawcharacter`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawcharacter) | Рисует один символ в экранных 2D-координатах. | `float(vector position, float character, vector size, vector rgb, float alpha, optional float drawflag) drawcharacter = #320;` |
| [`drawfill`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawfill) | Рисует сплошной прямоугольник указанного цвета и прозрачности поверх экрана — базовый способ нарисовать фон под HUD-элементы или полосу здоровья. | `float(vector position, vector size, vector rgb, float alpha, optional float drawflag) drawfill = #323;` |
| [`drawline`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawline) | Рисует 2D-линию между двумя экранными точками. | `void(float width, vector pos1, vector pos2, vector rgb, float alpha, optional float drawflag) drawline = #315;` |
| [`drawpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawpic) | Рисует изображение целиком внутри заданного 2D-прямоугольника экрана, с масштабированием под указанный `size`. | `float(vector position, string pic, vector size, vector rgb, float alpha, optional float drawflag) drawpic = #322;` |
| [`drawrawstring`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawrawstring) | Рисует строку "как есть", без разбора цветовой разметки движка — если в игре глобально включён UTF-8, строка декодируется как UTF-8, иначе трактуется как обычные quake-символы. | `float(vector position, string text, vector size, vector rgb, float alpha, optional float drawflag) drawrawstring = #321;` |
| [`drawresetcliparea`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawresetcliparea) | Снимает установленную ранее `drawsetcliparea` область отсечения (scissor test), возвращая клипинг ко всему экрану. | `void(void) drawresetcliparea = #325;` |
| [`drawsetcliparea`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawsetcliparea) | Задаёт прямоугольную область отсечения (scissor test): все последующие 2D-функции рисования, включая 2D-полигоны, будут обрезаны по этой области — пиксели за её пределами не изменяются. | `void(float x, float y, float width, float height) drawsetcliparea = #324;` |
| [`drawstring`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawstring) | Рисует строку с разбором цветовой разметки, в отличие от `drawrawstring`. | `float(vector position, string text, vector size, vector rgb, float alpha, float drawflag) drawstring = #326;` |
| [`drawsubpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawsubpic) | Вырезает прямоугольный фрагмент изображения (например, одну иконку из атласа/спрайт-листа) и рисует его растянутым в заданную область экрана. | `void(vector pos, vector sz, string pic, vector srcpos, vector srcsz, vector rgb, float alpha, optional float drawflag) drawsubpic = #328;` |
| [`movepic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#movepic) | Устаревший (legacy, часть `TEI_SHOWLMP2`) 2D builtin для перемещения именованной картинки, ранее показанной `showpic`, без изменения её текстуры. | `void(string slot, float x, float y, float zone, optional entity player) movepic = #106;` |
| [`showpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#showpic) | Устаревший (legacy) способ показать именованную 2D-картинку на HUD, унаследованный от QuakeWorld `TEI_SHOWLMP2`. | `void(string slot, string picname, float x, float y, float zone, optional entity player) showpic = #104;` |
| [`hidepic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#hidepic) | Скрывает (убирает с экрана) картинку, ранее показанную через `showpic` в указанном слоте. | `void(string slot, optional entity player) hidepic = #105;` |
| [`changepic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#changepic) | Заменяет изображение уже показанного слота (`showpic`) на новое, не меняя позицию, заданную ранее через `showpic`/`movepic`. | `void(string slot, string picname, optional entity player) changepic = #107;` |
| [`iscachedpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#iscachedpic) | Проверяет, загружено ли изображение в память движка в данный момент. | `float(string name) iscachedpic = #316;` |
| [`freepic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#freepic) | Сообщает движку, что указанное изображение больше не требуется, освобождая связанные с ним ресурсы. | `void(string name) freepic = #319;` |
| [`drawgetimagesize`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawgetimagesize) | Возвращает размеры (ширина в `x`, высота в `y`) указанного изображения. | `vector(string picname) drawgetimagesize = #318;` (алиас `draw_getimagesize`) |
| [`stringwidth`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#stringwidth) | Вычисляет ширину строки в виртуальных экранных пикселях, что позволяет выравнивать текст (по центру, по правому краю) перед вызовом `drawstring`. | `float(string text, float usecolours, optional vector fontsize) stringwidth = #327;` |
| [`adddecal`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#adddecal) | Добавляет временную декаль (проекцию текстуры на ближайшую поверхность), центрированную в мировой точке `origin` с ориентацией по `up`/`side`. | `void(string shadername, vector origin, vector up, vector side, vector rgb, float alpha) adddecal = #375;` |
| [`boxparticles`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#boxparticles) | Расширенная версия `pointparticles`, позволяющая распределить партиклы по объёму (боксу), а не по одной точке, и задать диапазон направлений вместо единственного вектора. | `void(float effectindex, entity own, vector org_from, vector org_to, vector dir_from, vector dir_to, float countmultiplier, optional float flags) boxparticles = #502;` |
| [`particle`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#particle) | Базовый, самый старый способ породить классические квейковские партиклы (маленькие цветные точки-квадратики), раскрашенные по индексу палитры, а не по RGB. | `void(vector pos, vector dir, float colour, float count) particle = #48;` |
| [`particle2`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#particle2) | Расширенный вариант `particle`, унаследованный из Hexen 2 (`FTE_HEXEN2`): каждый партикл получает случайное смещение в диапазоне между `dmin` и `dmax`, а `effect` выбирает один из встроенных типов поведения (гравитация, затухание и т.п.), характерных для эффектов Hexen 2. | `void(vector org, vector dmin, vector dmax, float colour, float effect, float count) particle2 = #215;` |
| [`particle3`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#particle3) | Вариант `particle2`, где вместо раздельных `dmin`/`dmax` смещение партиклов задаётся одним боксом `box` вокруг точки `org` — удобно для равномерного заполнения прямоугольного объёма (например, кубической области взрыва). | `void(vector org, vector box, float colour, float effect, float count) particle3 = #216;` |
| [`particle4`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#particle4) | Ещё один вариант из семейства Hexen2-партиклов, где область распределения задаётся сферой радиуса `radius` вместо бокса — удобно для сферических вспышек и разлётов обломков вокруг точки. | `void(vector org, float radius, float colour, float effect, float count) particle4 = #217;` |
| [`particleeffectnum`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#particleeffectnum) | Прекэширует (регистрирует) именованный эффект партиклов и возвращает его числовой индекс, который затем передаётся в `pointparticles`, `trailparticles`, `boxparticles`. | `float(string effectname) particleeffectnum = #335;` |
| [`particleeffectquery`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#particleeffectquery) | Позволяет прочитать имя или полное текстовое описание (конфигурацию) уже зарегистрированного эффекта партиклов. | `string(float efnum, float body) particleeffectquery = #374;` |
| [`pointparticles`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#pointparticles) | Основной способ выпустить партиклы предварительно зарегистрированного именованного эффекта в одной точке — используется для вспышек выстрела, попаданий, взрывов и т.п. | `void(float effectnum, vector origin, optional vector dir, optional float count) pointparticles = #337;` |
| [`trailparticles`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#trailparticles) | Рисует след партиклов (дымовой шлейф, огненную трассу и т.п.) вдоль отрезка от `start` до `end` за текущий кадр. | `void(float effectnum, entity ent, vector start, vector end) trailparticles = #336;` |
| [`effect`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#effect) | Порождает самоанимирующийся спрайт (часть `DP_SV_EFFECT`) — временный видимый объект, который проигрывает указанный диапазон кадров модели с заданной частотой и исчезает по завершении анимации, без необходимости управлять entity вручную. | `void(vector org, string modelname, float startframe, float endframe, float framerate) effect = #404;` |
| [`dynamiclight_add`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#dynamiclight_add) | Добавляет динамический свет в список текущего кадра и возвращает его индекс. | `float(vector org, float radius, vector lightcolours, optional float style, optional string cubemapname, optional float pflags) dynamiclight_add = #305;` |
| [`dynamiclight_get`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#dynamiclight_get) | Считывает отдельное свойство динамического/RT-света. | `__variant(float lno, float fld) dynamiclight_get = #372;` |
| [`dynamiclight_set`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#dynamiclight_set) | Изменяет отдельное поле уже существующего динамического/RT-света. | `void(float lno, float fld, __variant value) dynamiclight_set = #373;` |
| [`lightstyle`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#lightstyle) | Задаёт (переопределяет) анимационную строку яркости для указанного номера lightstyle — то же самое, что и мигающие/пульсирующие светильники в оригинальном Quake (torch flicker, strobe и т.д.). | `void(float lightstyle, string stylestring, optional vector rgb) lightstyle = #35;` |
| [`lightstylestatic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#lightstylestatic) | Аналог `lightstyle`, но задаёт не анимированную строку, а фиксированный числовой уровень яркости стиля — унаследовано из Hexen 2, где стили могли задаваться напрямую числом без анимации по кадрам. | `void(float style, float val, optional vector rgb) lightstylestatic = #5;` |
| [`getlight`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#getlight) | Возвращает фактический уровень освещения (обычно как RGB-вектор яркости) в указанной точке карты, с учётом статических светов, lightstyle-анимаций и (в некоторых случаях) динамических источников — в отличие от `getlightstylergb`, который лишь возвращает текущий цвет конкретного стиля без учёта геометрии карты. | `vector(vector org) getlight = #92;` |
| [`con_draw`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#con_draw) | Рисует именованную альтернативную консоль (не основную консоль движка) в заданной 2D-области экрана — это может быть, например, отдельное окно чата или внутриигровой терминал. | `void(string conname, vector pos, vector size, float fontsize) con_draw = #393;` |
| [`con_getset`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#con_getset) | Читает или изменяет свойство именованного консольного объекта альтернативных консолей и всегда возвращает предыдущее значение свойства. | `string(string conname, string field, optional string newvalue) con_getset = #391;` |
| [`con_input`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#con_input) | Перенаправляет событие ввода (обычно полученное в [`CSQC_InputEvent`](../37-quakec-builtins-reference/00-entry-points.md#csqc_inputevent)) в указанную альтернативную консоль — например, чтобы нажатия клавиш попадали в окно чата, а не в игровое управление, пока это окно активно. | `float(string conname, float inevtype, float parama, float paramb, float paramc) con_input = #394;` |
| [`con_printf`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#con_printf) | Печатает форматированное сообщение в именованную альтернативную консоль, а не в основную консоль движка. | `void(string conname, string messagefmt, ...) con_printf = #392;` |
| [`RegisterTempEnt`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#registertempent) | Регистрирует на сервере (только SSQC) новый пользовательский тип временного эффекта (часть `FTE_PEXT_CUSTOMTENTS`), который затем можно разослать клиентам через `CustomTempEnt`. | `float(float attributes, string effectname, ...) RegisterTempEnt = #208;` |
| [`CustomTempEnt`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#customtempent) | Рассылает клиентам пользовательский temp-entity эффект, зарегистрированный через `RegisterTempEnt` (только SSQC). | `void(float type, vector pos, ...) CustomTempEnt = #209;` |
| [`te_beam`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_beam) | Рисует протяжённый прямой луч (беам) между двумя точками — визуально похож на длинную лазерную/электрическую нить, используется, например, для эффекта луча телепорта®огня в модах DarkPlaces. | `void(entity own, vector start, vector end) te_beam = #431;` |
| [`te_blood`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_blood) | Классический эффект крови DarkPlaces (`DP_TE_BLOOD`) — разлёт красных частиц крови из точки `org` в направлении `dir`, количество регулируется `count`. | `void(vector org, vector dir, float count) te_blood = #405;` |
| [`te_bloodqw`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_bloodqw) | Вариант эффекта крови в стиле QuakeWorld — брызги крови без явного направления разлёта (в отличие от `te_blood`), количество частиц регулируется необязательным `count`. | `void(vector org, optional float count) te_bloodqw = #239;` |
| [`te_bloodshower`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_bloodshower) | Массированный "дождь" из крови внутри прямоугольного объёма от `mincorner` до `maxcorner` — используется для зрелищных эффектов расчленения/сильных попаданий, где кровь разлетается по большой площади с заданной скоростью разлёта (`explosionspeed`) и числом частиц (`howmany`). | `void(vector mincorner, vector maxcorner, float explosionspeed, float howmany) te_bloodshower = #406;` |
| [`te_customflash`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_customflash) | Кратковременная вспышка динамического света заданного радиуса, времени жизни и цвета в указанной точке — аналог `dynamiclight_add`, но реализован как готовый temp-entity эффект, транслируемый по сети из ssqc и автоматически угасающий за `lifetime` секунд без необходимости обновлять его вручную каждый кадр. | `void(vector org, float radius, float lifetime, vector color) te_customflash = #417;` |
| [`te_explosion`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_explosion) | Стандартный взрыв Quake 1 — яркая шарообразная вспышка света, разлёт частиц-обломков и клубы дыма в точке `org`, сопровождаемый звуком взрыва. | `void(vector org) te_explosion = #421;` |
| [`te_explosion2`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_explosion2) | Взрыв с частицами, окрашенными по диапазону палитры ([`color`](../39-entity-keys-reference/02-light-entity-keys.md#color) до `color+colorlength`), вместо стандартного огненно-серого цвета — использовался, например, для взрывов зелёных/фиолетовых оттенков в DarkPlaces-модах. | `void(vector org, float color, float colorlength) te_explosion2 = #427;` |
| [`te_explosionquad`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_explosionquad) | Вариант взрыва для режима Quad Damage (`_DP_TE_QUADEFFECTS1`) — визуально похож на `te_explosion`, но частицы и вспышка окрашены в характерный синий оттенок квад-эффектов, сигнализируя игроку, что взрыв произошёл от усиленного оружия. | `void(vector org) te_explosionquad = #415;` |
| [`te_explosionrgb`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_explosionrgb) | Взрыв с полностью произвольным RGB-цветом вспышки и частиц (`DP_TE_EXPLOSIONRGB`), в отличие от `te_explosion2`, где цвет ограничен индексами палитры. | `void(vector org, vector color) te_explosionrgb = #407;` |
| [`te_flamejet`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_flamejet) | Струя огненных частиц (`_DP_TE_FLAMEJET`), вылетающих из точки `org` со скоростью/направлением `vel`; количество частиц регулируется `howmany`. | `void(vector org, vector vel, float howmany) te_flamejet = #457;` |
| [`te_gunshot`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_gunshot) | Стандартный эффект попадания пули в стену — небольшой сноп серых искр/пыли с меньшей силой, чем `te_spike`. | `void(vector org, optional float count) te_gunshot = #418;` |
| [`te_knightspike`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_knightspike) | Искры от попадания в стену снаряда рыцаря (Knight) — визуально похож на `te_spike`, но с другим цветом/звуком попадания (характерным для оружия этого монстра из Quake 1). | `void(vector org) te_knightspike = #424;` |
| [`te_lavasplash`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_lavasplash) | Всплеск лавы — россыпь огненных частиц, разлетающихся по площади вокруг точки `org`, как будто в лаву упал тяжёлый объект. | `void(vector org) te_lavasplash = #425;` |
| [`te_lightning1`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_lightning1) | Молниевидный луч (беам), визуально аналогичный оружию Lightning Gun из Quake 1 — потрескивающая электрическая полоса от `start` до `end`. | `void(entity own, vector start, vector end) te_lightning1 = #428;` |
| [`te_lightningblood`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_lightningblood) | Специфический эффект попадания молнии по живой цели — комбинация искр от `te_lightning*` и всплеска крови в точке `pos`, используемый, когда электрический разряд Lightning Gun поражает противника, а не стену. | `void(vector pos) te_lightningblood = #219;` |
| [`te_particlecube`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_particlecube) | Заполняет прямоугольный объём (куб/параллелепипед от `mincorner` до `maxcorner`) заданным числом частиц одного цвета палитры, летящих с общей скоростью `vel` и случайным дрожанием скорости (`randomveljitter`); `gravityflag` включает воздействие гравитации на частицы. | `void(vector mincorner, vector maxcorner, vector vel, float howmany, float color, float gravityflag, float randomveljitter) te_particlecube = #408;` |
| [`te_particlerain`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_particlerain) | Похож на `te_particlecube`, но частицы падают дождём (постоянный поток) над областью от `mincorner` до `maxcorner` с общей скоростью `vel`, без гравитации и дрожания — типичный визуальный эффект для погодных систем (дождь определённого цвета). | `void(vector mincorner, vector maxcorner, vector vel, float howmany, float color) te_particlerain = #409;` |
| [`te_particlesnow`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_particlesnow) | Аналог `te_particlerain`, но частицы падают медленнее и обычно с небольшим боковым дрейфом, имитируя снегопад над указанной областью — типичное применение для погодных эффектов зимних карт. | `void(vector mincorner, vector maxcorner, vector vel, float howmany, float color) te_particlesnow = #410;` |
| [`te_plasmaburn`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_plasmaburn) | Небольшая вспышка/подпалина в точке попадания плазменного выстрела (`_DP_TE_PLASMABURN`) — визуально яркая короткая вспышка света с характерным для плазмы цветом попадания, без разлёта крупных частиц. | `void(vector org) te_plasmaburn = #433;` |
| [`te_smallflash`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_smallflash) | Небольшая кратковременная вспышка света в указанной точке без частиц (`DP_TE_SMALLFLASH`) — используется как лёгкий индикатор события (например, слабое попадание), не требующий полноценного взрыва. | `void(vector org) te_smallflash = #416;` |
| [`te_spark`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_spark) | Сноп искр (`DP_TE_SPARK`), вылетающих из точки `org` с направлением/скоростью `vel`; число искр задаётся `howmany`. | `void(vector org, vector vel, float howmany) te_spark = #411;` |
| [`te_spike`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_spike) | Стандартный эффект попадания снаряда (спайка) в стену из Quake 1 — небольшой сноп серых искр с характерным звуком рикошета. | `void(vector org) te_spike = #419;` |
| [`te_superspike`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_superspike) | Более крупный и заметный вариант `te_spike` — используется для попадания супергвоздомёта (Super Nailgun), сноп искр гуще и заметнее обычного. | `void(vector org) te_superspike = #420;` |
| [`te_tarexplosion`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_tarexplosion) | Взрыв тарбейби (Tarbaby) из Quake 1 — чёрные вязкие частицы, разлетающиеся из точки `org`, визуально отличается от обычного огненного `te_explosion` тёмным, "смоляным" видом. | `void(vector org) te_tarexplosion = #422;` |
| [`te_teleport`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_teleport) | Эффект телепортации из Quake 1 — кольцевой всплеск частиц вокруг точки `org`, сопровождающий появление/исчезновение entity в телепорте. | `void(vector org) te_teleport = #426;` |
| [`te_wizspike`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_wizspike) | Искры от попадания снаряда виверны/wizard (Scrag) в стену — визуально похож на `te_spike`, но с зеленоватым оттенком частиц и другим звуком попадания, характерным для этого монстра. | `void(vector org) te_wizspike = #423;` |
| [`addentity_lighting`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#addentity_lighting) | Записывает кастомные параметры освещения прямо в render-state указанной сущности. | `void(entity ent, vector dir, vector ambient, vector diffuse) addentity_lighting = #0:addentity_lighting;` |
| [`addtrisoup_simple`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#addtrisoup_simple) | Добавляет в сцену заранее подготовленный индексированный triangle soup. | `void(string texturename, int flags, trisoup_simple_vert_t *verts, int *indexes, int numindexes) addtrisoup_simple = #0:addtrisoup_simple;` |
| [`customtempent`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#customtempent) | Рассылает клиентам пользовательский temp-entity эффект, зарегистрированный через `RegisterTempEnt` (только SSQC). | `void(float type, vector pos, ...) CustomTempEnt = #209;` |
| [`drawrotpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawrotpic) | Рисует 2D-картинку, повёрнутую на заданный угол вокруг точки `pivot`. | `void(vector pivot, vector mins, vector maxs, string pic, vector rgb, float alpha, float angle, optional float drawflag) drawrotpic = #0:drawrotpic;` |
| [`drawrotpic_dp`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawrotpic_dp) | Совместимый с DarkPlaces вариант вращающейся картинки. | `void(vector pivot, string pic, vector size, vector mins, float angle, vector rgb, float alpha, optional float drawflag) drawrotpic_dp = #329;` |
| [`drawrotsubpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawrotsubpic) | Совмещает идеи `drawrotpic` и `drawsubpic`: поворачивает не всю картинку, а выбранный прямоугольник из атласа. | `void(vector pivot, vector mins, vector maxs, string pic, vector txmin, vector txsize, vector rgb, vector alphaandangles) drawrotsubpic = #0:drawrotsubpic;` |
| [`dynamiclight_spawnstatic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#dynamiclight_spawnstatic) | Создаёт более долговечный динамический/RT-свет и возвращает его индекс. | `float(vector org, float radius, vector rgb) dynamiclight_spawnstatic = #0:dynamiclight_spawnstatic;` |
| [`getlightstyle`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#getlightstyle) | Возвращает именно текстовую анимационную строку style (например, `"m"` или `"mmnmmommommnonmmonqnmmo"`). | `string(float style, optional __out vector rgb) getlightstyle = #0:getlightstyle;` |
| [`getlightstylergb`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#getlightstylergb) | Вычисляет уже не сырую строку, а текущее итоговое значение style с учётом клиентского времени и интерполяции между шагами анимации. | `vector(float style) getlightstylergb = #0:getlightstylergb;` |
| [`getlocationname`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#getlocationname) | Это полностью реализованная встроенная функция клиентской части движка (CSQC) в экосистеме FTEQW. | `string(vector org) getlocationname = #0:getlocationname;` |
| [`pointcontentsmask`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#pointcontentsmask) | Похожа на расширенную [`pointcontents`](../37-quakec-builtins-reference/03-entity-world-builtins.md#pointcontents), но её второй аргумент в CSQC не задаёт произвольную маску фильтра. | `__uint(vector org, optional float worldonly) pointcontentsmask = #0:pointcontentsmask;` |
| [`R_EndPolygonRibbon`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#r_endpolygonribbon) | Завершает не обычный полигон, а текущую полилинию, превращая уже добавленные через `R_PolygonVertex` точки в экранно-ориентированную ribbon/quad strip. | `void(float radius, vector texcoordbias) R_EndPolygonRibbon = #0:R_EndPolygonRibbon;` |
| [`r_readimage`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#r_readimage) | Сам выделяет addressable-буфер в памяти QC, декодирует изображение и возвращает указатель на RGBA-данные. | `int*(string filename, __out int width, __out int height, __out int format) r_readimage = #0:r_readimage;` |
| [`r_uploadimage`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#r_uploadimage) | Загружает в движок пиксели из уже существующего буфера памяти QC. | `void(string imagename, int width, int height, void *pixeldata, optional int datasize, optional int format) r_uploadimage = #0:r_uploadimage;` |
| [`registertempent`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#registertempent) | Регистрирует на сервере (только SSQC) новый пользовательский тип временного эффекта (часть `FTE_PEXT_CUSTOMTENTS`), который затем можно разослать клиентам через `CustomTempEnt`. | `float(float attributes, string effectname, ...) RegisterTempEnt = #208;` |
| [`remapshader`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#remapshader) | Это полностью реализованная встроенная функция графической подсистемы движка FTEQW на стороне CSQC. | `void(string oldshader, string newshader) remapshader = #0:remapshader;` |
| [`setcolor`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#setcolor) | (в некоторых версиях таблиц упоминается как [`setcolors`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#setcolors)) представляет собой нереализованный опкод под номером `#401` в клиентской виртуальной машине (CSQC). | `setcolor(...)` — данная встроенная функция не реализована и является жесткой заглушкой (номера: CSQC=#401). |
| [`trailparticles_dp`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#trailparticles_dp) | Это полностью реализованная встроенная функция графической подсистемы частиц на стороне клиентской части (CSQC). | `void(float effectindex, entity ent, vector start, vector end) trailparticles_dp = #336;` |
| [`V_CalcRefdef`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#v_calcrefdef) | Представляет собой нереализованный опкод под номером `#640` в клиентской виртуальной машине (CSQC). | `V_CalcRefdef(...)` — данная встроенная функция не реализована и является жесткой заглушкой (номера: CSQC=#640). |

### Ввод, интерфейс и клавиатура CSQC

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`getinputstate`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getinputstate) | Доступна только в CSQC и нужна прежде всего для prediction-кода. | `float(float inputsequencenum, optional float seat) getinputstate = #345;` |
| [`getkeybind`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getkeybind) | Доступна в CSQC и MenuQC. | `string(float keynum) getkeybind = #342;` |
| [`setkeybind`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setkeybind) | Доступна в CSQC и MenuQC и позволяет менять привязки без генерации текстовой команды `bind`. | `float(float key, string bind, optional float bindmap, optional float modifier) setkeybind = #630;` |
| [`getkeydest`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getkeydest) | Существует в MenuQC и сообщает, куда сейчас направлена клавиатура для этого слоя UI. | `float() getkeydest = #602;` |
| [`setkeydest`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setkeydest) | Доступна только в MenuQC. | `void(float dest) setkeydest = #601;` |
| [`getbindmaps`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getbindmaps) | Доступна в CSQC и MenuQC и возвращает в `x` и `y` два активных альтернативных bindmap-слота, которые движок использует как fallback после обычных биндов. | `vector() getbindmaps = #631;` |
| [`setbindmaps`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setbindmaps) | Доступна в CSQC и MenuQC и переключает активную пару альтернативных bindmap-слоёв. | `float(vector bm) setbindmaps = #632;` |
| [`getmousepos`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getmousepos) | Имя `getmousepos` существует в двух вариантах: старый MenuQC builtin `#66` и CSQC builtin `#344`. | `vector() getmousepos = #66;` |
| [`setmousetarget`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setmousetarget) | Доступна в CSQC и MenuQC и переключает, должен ли текущий модуль получать мышь как движение-дельту или как абсолютный экранный курсор. | `void(float trg) setmousetarget = #603;` |
| [`getmousetarget`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getmousetarget) | Доступна в CSQC и MenuQC и возвращает текущее состояние той же системы, что переключает `setmousetarget`: `1` означает relative/delta поток, `2` — absolute cursor поток. | `float() getmousetarget = #604;` |
| [`setcursormode`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setcursormode) | Доступна в CSQC и MenuQC и просит движок либо отпустить мышь в абсолютный режим, либо снова захватить её для игры. | `void(float usecursor, optional string cursorimage, optional vector hotspot_and_scale) setcursormode = #343;` |
| [`setsensitivityscaler`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setsensitivityscaler) | Существует только в CSQC. | `void(float sens) setsensitivityscaler = #346;` |
| [`keynumtostring`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#keynumtostring) | Под именем `keynumtostring` скрываются две родственные точки входа: CSQC-версия `#340` и MenuQC-версия `#609`. | `string(float keynum) keynumtostring = #340;` |
| [`keynumtostring_csqc`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#keynumtostring_csqc) | Deprecated MenuQC alias для CSQC builtin `#340`. | `string(float keynum) keynumtostring_csqc = #340;` |
| [`keynumtostring_menu`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#keynumtostring_menu) | CSQC alias для MenuQC builtin `#609`. | `string(float keynum) keynumtostring_menu = #609;` |
| [`keynumtostring_omgwtf`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#keynumtostring_omgwtf) | Ещё один deprecated alias к той же базовой логике преобразования keynum в строку. | `string(float keynum) keynumtostring_omgwtf = #520;` |
| [`stringtokeynum`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#stringtokeynum) | Как и `keynumtostring`, builtin [`stringtokeynum`](../37-quakec-builtins-reference/02-string-builtins.md#stringtokeynum) существует в двух основных слотах: CSQC `#341` и MenuQC `#614`. | `float(string keyname) stringtokeynum = #341;` |
| [`stringtokeynum_csqc`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#stringtokeynum_csqc) | Deprecated MenuQC alias к CSQC-варианту `stringtokeynum`. | `float(string keyname) stringtokeynum_csqc = #341;` |
| [`stringtokeynum_menu`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#stringtokeynum_menu) | CSQC alias к MenuQC builtin `#614`. | `float(string key) stringtokeynum_menu = #614;` |
| [`findkeysforcommand`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#findkeysforcommand) | Доступна и в CSQC, и в MenuQC, но исторически существует в двух слотах: deprecated CSQC `#521` и menu/shared `#610`. | `string(string command, optional float bindmap) findkeysforcommand = #521;` |
| [`gecko_create`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_create) | Доступна в CSQC и MenuQC и создаёт встроенную браузерную поверхность, которую затем можно рисовать как обычный 2D shader через [`drawpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawpic). | `float(string name, optional string initialURI) gecko_create = #487;` |
| [`gecko_destroy`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_destroy) | Доступна в CSQC и MenuQC и освобождает созданную браузерную поверхность. | `void(string name) gecko_destroy = #488;` |
| [`gecko_navigate`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_navigate) | Доступна в CSQC и MenuQC и отправляет браузеру команду навигации. | `void(string name, string URI) gecko_navigate = #489;` |
| [`gecko_keyevent`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_keyevent) | Доступна в CSQC и MenuQC и пересылает key event во встроенный браузер. | `float(string name, float key, float eventtype, optional float charcode) gecko_keyevent = #490;` |
| [`gecko_mousemove`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_mousemove) | Доступна в CSQC и MenuQC и сообщает встроенному браузеру положение указателя относительно самой веб-поверхности, а не всего экрана. | `void(string name, float x, float y) gecko_mousemove = #491;` |
| [`gecko_resize`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_resize) | Доступна в CSQC и MenuQC и запрашивает у браузерного движка новый размер рендер-буфера. | `void(string name, float w, float h) gecko_resize = #492;` |
| [`gecko_get_texture_extent`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_get_texture_extent) | Доступна в CSQC и MenuQC и возвращает вектор, где `x` и `y` содержат текущие пиксельные размеры браузерной текстуры, а `z` — дополнительное значение aspect ratio, которое сообщает media/browser backend. | `vector(string name) gecko_get_texture_extent = #493;` |
| [`CL_RotateMoves`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#cl_rotatemoves) | Специализированный CSQC builtin для коррекции истории ещё не подтверждённых usercmd. | `void(vector anglechange, optional float seat) CL_RotateMoves = #638;` |
| [`clipboard_get`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#clipboard_get) | Инициирует асинхронный запрос текстовых данных из системного буфера обмена операционной системы, однако сам текст напрямую функция не возвращает. | `void(int cliptype) clipboard_get = #0:clipboard_get;` |
| [`clipboard_set`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#clipboard_set) | Отправляет указанный текст в системный буфер обмена ОС и стабильно поддерживается как в CSQC, так и в MenuQC. | `void(int cliptype, string text) clipboard_set = #0:clipboard_set;` |
| [`drawtextfield`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#drawtextfield) | Выполняет рендеринг сложного форматированного текста внутри заданной прямоугольной области, автоматически рассчитывая переносы слов по границам блока и возвращая итоговое количество фактически отрисованных строк. | `float(vector pos, vector size, float alignflags, string text) drawtextfield = #0:drawtextfield;` |
| [`gecko_getproperty`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_getproperty) | Запрашивает специфические для конкретного декодера свойства у мультимедийного бэкенда или встроенного веб-браузера. | `string(string shadname, string propname) gecko_getproperty = #0:gecko_getproperty;` |
| [`getcursormode`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getcursormode) | Опрашивает состояние видимости и захвата курсора мыши в рамках оконной системы движка. | `float(float effective) getcursormode = #0:getcursormode;` |
| [`setmousepos`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setmousepos) | Программно сдвигает курсор в указанную точку, но только если для текущего keydest уже активен absolute mouse mode. | `void(vector newpos) setmousepos = #0:setmousepos;` |
| [`setwindowcaption`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setwindowcaption) | Динамически переписывает строку заголовка (title) окна приложения в операционной системе, изменяя текст на панели задач, в переключателе окон (Task Switcher) и декорациях оконного менеджера. | `void(string newcaption) setwindowcaption = #0:setwindowcaption;` |

### Скелетная анимация и модели

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`skel_build`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_build) | Основная точка входа в систему skeletal objects. | `float(float skel, entity ent, float modelindex, float retainfrac, float firstbone, float lastbone, optional float addfrac) skel_build = #264;` |
| [`skel_copybones`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_copybones) | Переносит готовые матрицы костей из одного skeletal object в другой без повторного чтения анимации из файла модели. | `void(float skeldst, float skelsrc, float startbone, float entbone) skel_copybones = #274;` |
| [`skel_create`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_create) | Выделяет новый skeletal object и возвращает его числовой handle. | `float(float modlindex, optional float useabstransforms) skel_create = #263;` |
| [`skel_delete`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_delete) | Помечает skeletal object на удаление. | `void(float skel) skel_delete = #275;` |
| [`skel_find_bone`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_find_bone) | Ищет кость по имени и возвращает её числовой индекс в том же 1-based формате, который используют остальные `skel_*` builtins. | `float(float skel, string tagname) skel_find_bone = #268;` |
| [`skel_get_boneabs`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_get_boneabs) | Возвращает смещение кости относительно самой entity, а её ориентацию записывает в глобальные `v_forward`, `v_right`, `v_up`. | `vector(float skel, float bonenum) skel_get_boneabs = #270;` |
| [`skel_get_bonename`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_get_bonename) | Возвращает имя кости по её номеру. | `string(float skel, float bonenum) skel_get_bonename = #266;` |
| [`skel_get_boneparent`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_get_boneparent) | Сообщает, к какой кости привязана указанная кость. | `float(float skel, float bonenum) skel_get_boneparent = #267;` |
| [`skel_get_bonerel`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_get_bonerel) | Возвращает локальное смещение кости относительно её родителя и одновременно заполняет `v_forward`, `v_right`, `v_up` локальной ориентацией этой кости. | `vector(float skel, float bonenum) skel_get_bonerel = #269;` |
| [`skel_get_numbones`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_get_numbones) | Возвращает количество костей в skeletal object. | `float(float skel) skel_get_numbones = #265;` |
| [`skel_mmap`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_mmap) | Отдаёт указатель на блок памяти VM, где лежат матрицы костей текущего skeletal object. | `float*(float skel) skel_mmap = #282;` |
| [`skel_premul_bone`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_premul_bone) | Домножает существующую матрицу кости новой матрицей слева, то есть модификатор применяется «до» уже накопленной локальной позы. | `void(float skel, float bonenum, vector org, optional vector fwd, optional vector right, optional vector up) skel_premul_bone = #272;` |
| [`skel_premul_bones`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_premul_bones) | Делает то же самое, что `skel_premul_bone`, но сразу для последовательного диапазона костей. | `void(float skel, float startbone, float endbone, vector org, optional vector fwd, optional vector right, optional vector up) skel_premul_bones = #273;` |
| [`skel_ragupdate`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_ragupdate) | Управляет ragdoll-подсистемой FTEQW. | `float(entity skelent, string dollcmd, float animskel) skel_ragupdate = #281;` |
| [`skel_set_bone`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_set_bone) | Полностью заменяет локальную трансформацию кости в skeletal object. | `void(float skel, float bonenum, vector org, optional vector fwd, optional vector right, optional vector up) skel_set_bone = #271;` |
| [`skel_set_bone_world`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_set_bone_world) | Более высокий уровень, чем `skel_set_bone`: вы задаёте положение кости в world-space, а движок сам переводит его в локальную матрицу относительно родителя и текущего transform самой entity. | `void(entity ent, float bonenum, vector org, optional vector angorfwd, optional vector right, optional vector up) skel_set_bone_world = #283;` |
| [`gettagindex`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#gettagindex) | Ищет тег или кость по имени на модели указанной сущности и возвращает его числовой индекс. | `float(entity ent, string tagname) gettagindex = #451;` |
| [`gettaginfo`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#gettaginfo) | Вычисляет текущее world-space положение тега или кости с учётом анимации модели, transform самой entity и цепочки tag attachment. | `vector(entity ent, float tagindex) gettaginfo = #452;` |
| [`getsurfaceclippedpoint`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfaceclippedpoint) | Берёт точку `p`, проецирует её на плоскость указанной поверхности и затем зажимает внутрь фактического полигона/треугольников поверхности. | `vector(entity e, float s, vector p) getsurfaceclippedpoint = #439;` |
| [`getsurfacenearpoint`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfacenearpoint) | Перебирает поверхности brush-модели и возвращает индекс той, которая ближе всего к заданной точке. | `float(entity e, vector p) getsurfacenearpoint = #438;` |
| [`getsurfacenormal`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfacenormal) | Возвращает нормаль поверхности. | `vector(entity e, float s) getsurfacenormal = #436;` |
| [`getsurfacenumpoints`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfacenumpoints) | Возвращает число вершин указанной поверхности. | `float(entity e, float s) getsurfacenumpoints = #434;` |
| [`getsurfacenumtriangles`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfacenumtriangles) | Возвращает число треугольников в mesh-представлении поверхности. | `float(entity e, float s) getsurfacenumtriangles = #628;` |
| [`getsurfacepoint`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfacepoint) | Возвращает координаты одной вершины поверхности. | `vector(entity e, float s, float n) getsurfacepoint = #435;` |
| [`getsurfacepointattribute`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfacepointattribute) | Возвращает не только позицию вершины, но и другие связанные данные mesh-вершины. | `vector(entity e, float s, float n, float a) getsurfacepointattribute = #486;` |
| [`getsurfacetexture`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfacetexture) | Возвращает имя texture/shader, назначенного поверхности. | `string(entity e, float s) getsurfacetexture = #437;` |
| [`getsurfacetriangle`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#getsurfacetriangle) | Возвращает три vertex indices одного треугольника в виде `vector`, где каждая компонента — это индекс вершины для последующего чтения через `getsurfacepoint` или `getsurfacepointattribute`. | `vector(entity e, float s, float n) getsurfacetriangle = #629;` |
| [`applycustomskin`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#applycustomskin) | Применяет заранее загруженный custom skin к entity. | `void(entity e, float skinobj) applycustomskin = #378;` |
| [`loadcustomskin`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#loadcustomskin) | Создаёт skin object и возвращает его handle. | `float(string skinfilename, optional string skindata) loadcustomskin = #377;` |
| [`setcustomskin`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#setcustomskin) | Высокоуровневый convenience builtin: он сам создаёт skin object из `skinfilename`/`skindata`, назначает его entity и отпускает старый skin этой сущности. | `void(entity e, string skinfilename, optional string skindata) setcustomskin = #376;` |
| [`releasecustomskin`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#releasecustomskin) | Сообщает движку, что QC больше не нуждается в указанном skin object. | `void(float skinobj) releasecustomskin = #379;` |
| [`setcolors`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#setcolors) | Устаревший серверный builtin из классической Quake-парадигмы смены top/bottom colors у player model. | `__deprecated("No RGB support.") void(entity ent, float colours) setcolors = #401;` |
| [`skel_build_ptr`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_build_ptr) | Является pointer-вариантом `skel_build`, но требует уже существующий skeletal object и не создаёт его автоматически. | `float(float skel, int numblends, skelblend_t *weights, int structsize) skel_build_ptr = #0:skel_build_ptr;` |
| [`skel_postmul_bone`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_postmul_bone) | Применяет дополнительную матрицу трансформации после текущих вычислений кости, то есть выполняет пост-умножение (post-multiplication) геометрических преобразований для одной конкретной кости. | `void(float skel, float bonenum, vector org, optional vector fwd, optional vector right, optional vector up) skel_postmul_bone = #0:skel_postmul_bone;` |
| [`skel_postmul_bones`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_postmul_bones) | Существует рядом с `skel_postmul_bone` как внутренняя функция движка с той же общей идеей: пост-умножить матрицей подряд идущий диапазон костей. | Стандартного публичного объявления QuakeC для `skel_postmul_bones` в штатных defs FTEQW нет; в исходниках есть внутренняя C-реализация диапазонного post-multiply, но обычный QC-код не должен рассчитывать на неё как на доступный builtin. |

### Браузер серверов и мастер-сервер

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`addwantedhostcachekey`](../37-quakec-builtins-reference/11-server-browser-builtins.md#addwantedhostcachekey) | Исторически предназначена для предварительной регистрации пользовательского ключа из serverinfo, чтобы браузер серверов знал, какие дополнительные данные вас интересуют. | `void(string key) addwantedhostcachekey = #623;` |
| [`gethostcacheindexforkey`](../37-quakec-builtins-reference/11-server-browser-builtins.md#gethostcacheindexforkey) | Преобразует строковое имя поля в числовой handle, который потом дешевле передавать в `gethostcachestring`, `gethostcachenumber`, `sethostcachemaskstring`, `sethostcachemasknumber` и `sethostcachesort`. | `float(string key) gethostcacheindexforkey = #622;` |
| [`gethostcachenumber`](../37-quakec-builtins-reference/11-server-browser-builtins.md#gethostcachenumber) | Читает числовое значение из выбранной записи host cache. | `float(float fld, float hostnr) gethostcachenumber = #621;` |
| [`gethostcachestring`](../37-quakec-builtins-reference/11-server-browser-builtins.md#gethostcachestring) | Возвращает строковое значение поля для выбранного сервера: имя, карту, адрес, gamedir, сырой `serverinfo`, данные игрока (`player0`, `player1`, ...) или произвольный custom key. | `string(float type, float hostnr) gethostcachestring = #612;` |
| [`gethostcachevalue`](../37-quakec-builtins-reference/11-server-browser-builtins.md#gethostcachevalue) | Читает не поле отдельного сервера, а состояние всей подсистемы браузера. | `float(float type) gethostcachevalue = #611;` |
| [`refreshhostcache`](../37-quakec-builtins-reference/11-server-browser-builtins.md#refreshhostcache) | Запускает новый цикл запросов к мастер-серверам, LAN-источникам и самим игровым серверам. | `void(optional float dopurge) refreshhostcache = #620;` |
| [`resethostcachemasks`](../37-quakec-builtins-reference/11-server-browser-builtins.md#resethostcachemasks) | Очищает весь набор ранее добавленных фильтров, созданных через `sethostcachemaskstring` и `sethostcachemasknumber`. | `void() resethostcachemasks = #615;` |
| [`resorthostcache`](../37-quakec-builtins-reference/11-server-browser-builtins.md#resorthostcache) | Заново прогоняет все записи через активные маски и пересобирает видимый отсортированный список. | `void() resorthostcache = #618;` |
| [`sethostcachemasknumber`](../37-quakec-builtins-reference/11-server-browser-builtins.md#sethostcachemasknumber) | Добавляет числовое правило фильтрации к текущему списку масок. | `void(float mask, float fld, float num, float op) sethostcachemasknumber = #617;` |
| [`sethostcachemaskstring`](../37-quakec-builtins-reference/11-server-browser-builtins.md#sethostcachemaskstring) | Добавляет строковый фильтр к видимой выборке серверов. | `void(float mask, float fld, string str, float op) sethostcachemaskstring = #616;` |
| [`sethostcachesort`](../37-quakec-builtins-reference/11-server-browser-builtins.md#sethostcachesort) | Меняет активное поле сортировки, но не перестраивает список автоматически — для применения нужен отдельный `resorthostcache`. | `void(float fld, float descending) sethostcachesort = #619;` |
| [`getgamedirinfo`](../37-quakec-builtins-reference/11-server-browser-builtins.md#getgamedirinfo) | Перечисляет известные движку моды и возвращает их свойства как строки. | `string(float n, float prop) getgamedirinfo = #626;` |
| [`getextresponse`](../37-quakec-builtins-reference/11-server-browser-builtins.md#getextresponse) | [`getextresponse`](../37-quakec-builtins-reference/04-network-messages-builtins.md#getextresponse) присутствует в наборе `FTE_CSQC_SERVERBROWSER`, но в текущем FTEQW остаётся заглушкой: клиентская реализация просто возвращает пустую строку. | `string() getextresponse = #624;` |
| [`calltimeofday`](../37-quakec-builtins-reference/11-server-browser-builtins.md#calltimeofday) | Не относится к host cache и вообще не предназначена для MenuQC: в `fteextensions.qc` она объявлена для CSQC/SSQC и помечена как deprecated. | `__deprecated("Use strftime.") void() calltimeofday = #231;` |
| [`openportal`](../37-quakec-builtins-reference/11-server-browser-builtins.md#openportal) | Тоже не имеет отношения к host cache и не является MenuQC builtin: она существует в игровой QuakeC для картовых порталов Q2/Q3. | `void(entity portal, float state) openportal = #207;` |
| [`getpackagemanagerinfo`](../37-quakec-builtins-reference/11-server-browser-builtins.md#getpackagemanagerinfo) | Даёт CSQC/MenuQC доступ к списку пакетов, которые знает встроенный package manager движка. | `string(int n, int prop) getpackagemanagerinfo = #0:getpackagemanagerinfo;` |

### Системные функции, отладка и cvar

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`error`](../37-quakec-builtins-reference/12-system-debug-builtins.md#error) | Жёсткий аварийный выход из текущей QuakeC VM с понятным текстом в консоли. | `void(string err, ...) error = #10;` |
| [`objerror`](../37-quakec-builtins-reference/12-system-debug-builtins.md#objerror) | Нужен для ошибок, привязанных к конкретной entity. | `void(string err, ...) objerror = #11;` |
| [`print`](../37-quakec-builtins-reference/12-system-debug-builtins.md#print) | Выводит текст только в локальную консоль той VM, где он вызван. | `void(string s, ...) print = #339;` |
| [`bprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#bprint) | Рассылает текст всем подключённым игрокам. | `void(float msglvl, string s, optional string s2, optional string s3, optional string s4, optional string s5, optional string s6, optional string s7) bprint = #23;` |
| [`msprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#msprint) | Устаревший builtin из старого menu/client API. | `void(float clientnum, string text, ...) msprint = #6;` |
| [`cprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cprint) | Показывает сообщение в центре экрана локального клиента. | `void(string s, ...) cprint = #338;` |
| [`sprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#sprint) | Отправляет текст только одному конкретному игроку. | `void(entity client, float msglvl, string s, optional string s2, optional string s3, optional string s4, optional string s5, optional string s6) sprint = #24;` |
| [`centerprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#centerprint) | Серверный способ показать текст по центру экрана одному игроку. | `void(entity ent, string text, optional string text2, optional string text3, optional string text4, optional string text5, optional string text6, optional string text7) centerprint = #73;` |
| [`dprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#dprint) | Отладочный вывод, который в нормальном рабочем режиме не должен заспамливать консоль. | `void(string s, ...) dprint = #25;` |
| [`coredump`](../37-quakec-builtins-reference/12-system-debug-builtins.md#coredump) | Записывает снимок состояния QC VM на диск. | `void() coredump = #28;` |
| [`crash`](../37-quakec-builtins-reference/12-system-debug-builtins.md#crash) | Преднамеренная авария ради тестирования отладочной инфраструктуры. | `void() crash = #72;` |
| [`stackdump`](../37-quakec-builtins-reference/12-system-debug-builtins.md#stackdump) | В текущем FTEQW привязан только к legacy-слоту MenuQC `#73`. | `void() stackdump = #73;` |
| [`breakpoint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#breakpoint) | Генерирует событие для отладчика QC. | `void() breakpoint = #6;` |
| [`cvar`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar) | Читает текущее значение консольной переменной и возвращает его как `float`. | `float(string name) cvar = #45;` |
| [`cvar_set`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar_set) | Мгновенно записывает новое строковое значение в cvar. | `void(string cvarname, string valuetoset) cvar_set = #72;` |
| [`cvar_setf`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar_setf) | Делает то же самое, что `cvar_set`, но принимает число и сам переводит его в строковое представление движка. | `void(string cvar, float val) cvar_setf = #176;` |
| [`cvar_string`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar_string) | Возвращает текущее значение cvar как строку. | `string(string cvarname) cvar_string = #448;` |
| [`cvar_defstring`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar_defstring) | Возвращает не текущее, а дефолтное значение переменной. | `string(string name) cvar_defstring = #482;` |
| [`cvar_description`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar_description) | Возвращает человекочитаемое описание cvar, если движок или мод действительно зарегистрировали его. | `string(string cvarname) cvar_description = #518;` |
| [`cvar_type`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar_type) | Возвращает битовую маску с метаданными переменной и потому сильно отличается от `cvar`, `cvar_string` и `cvar_defstring`. | `float(string name) cvar_type = #495;` |
| [`registercvar`](../37-quakec-builtins-reference/12-system-debug-builtins.md#registercvar) | Builtin для создания НОВОЙ cvar от имени QC. | `float(string name, string value, optional float flags) registercvar = #93;` |
| [`checkextension`](../37-quakec-builtins-reference/12-system-debug-builtins.md#checkextension) | Сообщает, поддерживает ли текущий движок указанное расширение QuakeC API. | `float(string extname) checkextension = #99;` |
| [`logfrag`](../37-quakec-builtins-reference/12-system-debug-builtins.md#logfrag) | Служебный QuakeWorld-ориентированный builtin для записи события фрага в серверный лог/статистику. | `void(entity killer, entity killee) logfrag = #79;` |
| [`setpause`](../37-quakec-builtins-reference/12-system-debug-builtins.md#setpause) | Переключает состояние паузы сервера из QuakeC. | `void(float pause) setpause = #531;` |
| [`localcmd`](../37-quakec-builtins-reference/12-system-debug-builtins.md#localcmd) | Добавляет текст в очередь консольных команд движка. | `void(string s, ...) localcmd = #46;` |
| [`abort`](../37-quakec-builtins-reference/12-system-debug-builtins.md#abort) | Это не признак фатального падения программы, а специализированный механизм завершения текущего контекста выполнения QuakeC через процедуру `AbortStack`. | `void(optional __variant ret) abort = #211;` |
| [`argc`](../37-quakec-builtins-reference/12-system-debug-builtins.md#argc) | Возвращает количество активных токенов (фрагментов строк) в текущем состоянии глобального системного токенизатора QC. | `float() argc = #0:argc;` |
| [`checkbuiltin`](../37-quakec-builtins-reference/12-system-debug-builtins.md#checkbuiltin) | Осуществляет динамическую проверку (Feature Detection) того, реализована ли конкретная встроенная функция в запущенном исполняемом файле движка. | `float(__variant funcref) checkbuiltin = #0:checkbuiltin;` |
| [`externrefcall`](../37-quakec-builtins-reference/12-system-debug-builtins.md#externrefcall) | Это устаревший низкоуровневый встроенный метод для выполнения прямого вызова функции, расположенной в другом прогс-контексте (например, вызов функции MenuQC из контекста CSQC). | `__deprecated("Redundant") __variant(float prnum, void() func, ...) externrefcall = #205;` |

### Функции MenuQC (меню, экран загрузки)

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`addentity`](../37-quakec-builtins-reference/13-menuqc-builtins.md#addentity) | [`addentity`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#addentity) копирует текущее визуальное состояние обычной QuakeC-entity в список render-entity для следующего [`renderscene`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#renderscene). | `void(entity ent) addentity = #302;` |
| [`addentities`](../37-quakec-builtins-reference/13-menuqc-builtins.md#addentities) | Для MenuQC `addentities` в текущем FTEQW по умолчанию не экспортируется: `checkbuiltin(addentities)` для `menu.dat` вернёт `FALSE`. | `void(float mask) addentities = #301;` |
| [`clearscene`](../37-quakec-builtins-reference/13-menuqc-builtins.md#clearscene) | Забывает все render-entity, полигоны и временные динамические источники света, накопленные для текущей меню-сцены, а также сбрасывает свойства вида к значениям по умолчанию. | `void() clearscene = #300;` |
| [`renderscene`](../37-quakec-builtins-reference/13-menuqc-builtins.md#renderscene) | Рисует всё, что было накоплено после `clearscene`: menu-entity из `addentity`, build-dependent наборы из `addentities`, 3D-полигоны и временные lights/effects. | `void() renderscene = #304;` |
| [`getproperty`](../37-quakec-builtins-reference/13-menuqc-builtins.md#getproperty) | Возвращает текущее значение свойства вида: размер viewport, origin камеры, углы, field of view и другие флаги из семейства `VF_*`. | `__variant(float property) getproperty = #309;` (алиас `getviewprop`) |
| [`setproperty`](../37-quakec-builtins-reference/13-menuqc-builtins.md#setproperty) | Переопределяет параметры камеры и viewport для ближайшего `renderscene`: положение и углы камеры, поле зрения, область рендера, часть флагов отрисовки HUD/мира и связанные параметры. | `float(float property, ...) setproperty = #303;` (алиас `setviewprop`) |
| [`getresolution`](../37-quakec-builtins-reference/13-menuqc-builtins.md#getresolution) | [`getresolution`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#getresolution) помогает строить собственное меню видеонастроек без парсинга текстового вывода движка. | `vector(float vidmode, optional float forfullscreen) getresolution = #608;` |
| [`R_BeginPolygon`](../37-quakec-builtins-reference/13-menuqc-builtins.md#r_beginpolygon) | [`R_BeginPolygon`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#r_beginpolygon) открывает построение произвольного полигона. | `void(string texturename, optional float flags, optional float is2d) R_BeginPolygon = #306;` |
| [`R_EndPolygon`](../37-quakec-builtins-reference/13-menuqc-builtins.md#r_endpolygon) | Завершает текущий полигон, начатый `R_BeginPolygon`. | `void() R_EndPolygon = #308;` |
| [`R_PolygonVertex`](../37-quakec-builtins-reference/13-menuqc-builtins.md#r_polygonvertex) | Добавляет одну вершину в полигон, открытый `R_BeginPolygon`. | `void(vector org, vector texcoords, vector rgb, float alpha) R_PolygonVertex = #307;` |
| [`drawcharacter`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawcharacter) | [`drawcharacter`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawcharacter) рисует один символ шрифта и хорошо подходит для стрелок выбора, одиночных иконок-глифов, индикаторов биндов и курсоров каретки. | `float(vector position, float character, vector scale, vector rgb, float alpha, optional float flag) drawcharacter = #454;` |
| [`drawfill`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawfill) | [`drawfill`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawfill) — базовая прямоугольная заливка для фона меню, затемнений, полос прогресса, активных вкладок и подложек под текст. | `float(vector position, vector size, vector rgb, float alpha, optional float flag) drawfill = #457;` |
| [`drawline`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawline) | С [`drawline`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawline) у MenuQC есть важный practical-нюанс: старые menu-headers нередко показывают только три аргумента, но текущая реализация FTEQW читает тот же расширенный набор, что и CSQC — цвет, alpha и необязательный drawflag. | `void(float width, vector pos1, vector pos2, vector rgb, float alpha, optional float flag) drawline = #466;` |
| [`drawpic`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawpic) | [`drawpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawpic) рисует изображение в заданном прямоугольнике меню и почти всегда используется для логотипов, иконок, кнопок, фоновых панелей и самих браузерных текстур от [`gecko_create`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_create). | `float(vector position, string pic, vector size, vector rgb, float alpha, optional float flag) drawpic = #456;` |
| [`drawrawstring`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawrawstring) | [`drawrawstring`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawrawstring) выводит текст «как есть», не разбирая `^1`, `^xRGB` и другие цветовые escape-последовательности движка. | `float(vector position, string text, vector scale, vector rgb, float alpha, optional float flag) drawrawstring = #455;` |
| [`drawresetcliparea`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawresetcliparea) | [`drawresetcliparea`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawresetcliparea) снимает ранее установленную область отсечения и возвращает 2D-рисование ко всему экрану. | `void(void) drawresetcliparea = #459;` |
| [`drawsetcliparea`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawsetcliparea) | Включает прямоугольное отсечение: все последующие 2D draw-вызовы и 2D-полигоны будут видимы только внутри указанного прямоугольника. | `void(float x, float y, float width, float height) drawsetcliparea = #458;` |
| [`drawstring`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawstring) | Основной текстовый builtin MenuQC: он рисует строку и интерпретирует цветовую разметку, позволяя внутри одного текста смешивать белый, серый, красный и любые другие цвета. | `float(vector position, string text, vector scale, vector rgb, float alpha, float flag) drawstring = #467;` |
| [`drawsubpic`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawsubpic) | [`drawsubpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawsubpic) вырезает прямоугольный фрагмент из текстурного атласа и растягивает его в указанный экранный прямоугольник. | `void(vector pos, vector sz, string pic, vector srcpos, vector srcsz, vector rgb, float alpha, float flag) drawsubpic = #469;` |
| [`iscachedpic`](../37-quakec-builtins-reference/13-menuqc-builtins.md#iscachedpic) | [`iscachedpic`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#iscachedpic) проверяет, присутствует ли картинка в текущем графическом кэше движка. | `float(string name) iscachedpic = #451;` |
| [`drawgetimagesize`](../37-quakec-builtins-reference/13-menuqc-builtins.md#drawgetimagesize) | [`drawgetimagesize`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#drawgetimagesize) возвращает реальный размер изображения в пикселях. | `vector(string picname) drawgetimagesize = #460;` |
| [`stringwidth`](../37-quakec-builtins-reference/13-menuqc-builtins.md#stringwidth) | [`stringwidth`](../37-quakec-builtins-reference/02-string-builtins.md#stringwidth) вычисляет итоговую ширину текста в экранных пикселях и нужна почти в любом приличном MenuQC: для центрирования заголовков, выравнивания колонок, правой кромки кнопок и расчёта hover-областей под строки списка. | `float(string text, float usecolours, optional vector fontsize) stringwidth = #468;` |
| [`con_draw`](../37-quakec-builtins-reference/13-menuqc-builtins.md#con_draw) | [`con_draw`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#con_draw) рисует именованную альтернативную консоль прямо внутри интерфейса MenuQC. | `void(string conname, vector pos, vector size, float fontsize) con_draw = #393;` |
| [`con_getset`](../37-quakec-builtins-reference/13-menuqc-builtins.md#con_getset) | [`con_getset`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#con_getset) — управляющий builtin для альтернативных консолей. | `string(string conname, string field, optional string newvalue) con_getset = #391;` |
| [`con_input`](../37-quakec-builtins-reference/13-menuqc-builtins.md#con_input) | [`con_input`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#con_input) перенаправляет ввод из `Menu_InputEvent` во встроенную альтернативную консоль. | `float(string conname, float inevtype, float parama, float paramb, float paramc) con_input = #394;` |
| [`con_printf`](../37-quakec-builtins-reference/13-menuqc-builtins.md#con_printf) | [`con_printf`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#con_printf) печатает форматированный текст в именованную альтернативную консоль, а не в основную консоль движка. | `void(string conname, string messagefmt, ...) con_printf = #392;` |
| [`dynamiclight_add`](../37-quakec-builtins-reference/13-menuqc-builtins.md#dynamiclight_add) | В MenuQC по умолчанию этот builtin недоступен. | `float(vector org, float radius, vector lightcolours, optional float style, optional string cubemapname, optional float pflags) dynamiclight_add = #305;` |
| [`dynamiclight_get`](../37-quakec-builtins-reference/13-menuqc-builtins.md#dynamiclight_get) | Для MenuQC в штатной сборке FTEQW `dynamiclight_get` тоже не экспортируется. | `__variant(float lno, float fld) dynamiclight_get = #372;` |
| [`dynamiclight_set`](../37-quakec-builtins-reference/13-menuqc-builtins.md#dynamiclight_set) | Находится в той же ситуации, что и `dynamiclight_add`/`dynamiclight_get`: код реализации есть для клиентского рендера, но таблица MenuQC по умолчанию этот builtin не публикует. | `void(float lno, float fld, __variant value) dynamiclight_set = #373;` |
| [`getkeybind`](../37-quakec-builtins-reference/13-menuqc-builtins.md#getkeybind) | [`getkeybind`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getkeybind) возвращает строку команды, назначенную на указанную клавишу. | `string(float keynum) getkeybind = #342;` |
| [`setkeybind`](../37-quakec-builtins-reference/13-menuqc-builtins.md#setkeybind) | [`setkeybind`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setkeybind) переназначает клавишу без текстовой команды `bind`. | `float(float key, string bind, optional float bindmap, optional float modifier) setkeybind = #630;` |
| [`getkeydest`](../37-quakec-builtins-reference/13-menuqc-builtins.md#getkeydest) | [`getkeydest`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getkeydest) сообщает, кому сейчас принадлежит клавиатурный фокус для этого UI-слоя. | `float() getkeydest = #602;` |
| [`setkeydest`](../37-quakec-builtins-reference/13-menuqc-builtins.md#setkeydest) | [`setkeydest`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setkeydest) переключает владельца клавиатуры. | `void(float dest) setkeydest = #601;` |
| [`getbindmaps`](../37-quakec-builtins-reference/13-menuqc-builtins.md#getbindmaps) | [`getbindmaps`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getbindmaps) возвращает в `x` и `y` активную пару альтернативных карт биндов. | `vector() getbindmaps = #631;` |
| [`setbindmaps`](../37-quakec-builtins-reference/13-menuqc-builtins.md#setbindmaps) | [`setbindmaps`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setbindmaps) переключает активную пару альтернативных bindmap-слоёв. | `float(vector bm) setbindmaps = #632;` |
| [`getmousepos`](../37-quakec-builtins-reference/13-menuqc-builtins.md#getmousepos) | MenuQC использует исторический builtin `getmousepos = #66`. | `vector() getmousepos = #66;` |
| [`setmousetarget`](../37-quakec-builtins-reference/13-menuqc-builtins.md#setmousetarget) | [`setmousetarget`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setmousetarget) выбирает, как MenuQC хочет получать мышь: как дельты движения или как абсолютный экранный курсор. | `void(float trg) setmousetarget = #603;` |
| [`getmousetarget`](../37-quakec-builtins-reference/13-menuqc-builtins.md#getmousetarget) | [`getmousetarget`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getmousetarget) возвращает текущее состояние той же системы: `1` для delta-ввода и `2` для абсолютного курсора. | `float() getmousetarget = #604;` |
| [`setcursormode`](../37-quakec-builtins-reference/13-menuqc-builtins.md#setcursormode) | [`setcursormode`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setcursormode) освобождает или захватывает мышь для текущего UI-слоя и, при необходимости, назначает собственное изображение курсора. | `void(float usecursor, optional string cursorimage, optional vector hotspot, optional float scale) setcursormode = #343;` |
| [`keynumtostring`](../37-quakec-builtins-reference/13-menuqc-builtins.md#keynumtostring) | [`keynumtostring`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#keynumtostring) — основная MenuQC-версия builtin для перевода keynum в строку в стиле консольной команды `bind`: `SPACE`, `ENTER`, `MOUSE1`, `F6` и т.д. | `string(float keynum) keynumtostring = #609;` |
| [`keynumtostring_csqc`](../37-quakec-builtins-reference/13-menuqc-builtins.md#keynumtostring_csqc) | [`keynumtostring_csqc`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#keynumtostring_csqc) — deprecated MenuQC alias для CSQC-слота `#340`. | `string(float keynum) keynumtostring_csqc = #340;` |
| [`stringtokeynum`](../37-quakec-builtins-reference/13-menuqc-builtins.md#stringtokeynum) | [`stringtokeynum`](../37-quakec-builtins-reference/02-string-builtins.md#stringtokeynum) выполняет обратное преобразование: по строке вроде `ESCAPE`, `SPACE`, `MOUSE1` или `F5` возвращает numeric key code. | `float(string key) stringtokeynum = #614;` |
| [`stringtokeynum_csqc`](../37-quakec-builtins-reference/13-menuqc-builtins.md#stringtokeynum_csqc) | [`stringtokeynum_csqc`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#stringtokeynum_csqc) — deprecated MenuQC alias к CSQC-слоту `#341`. | `float(string keyname) stringtokeynum_csqc = #341;` |
| [`findkeysforcommand`](../37-quakec-builtins-reference/13-menuqc-builtins.md#findkeysforcommand) | [`findkeysforcommand`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#findkeysforcommand) возвращает список всех клавиш, выполняющих указанную команду, в строковом формате для последующего разбора через [`tokenize`](../37-quakec-builtins-reference/02-string-builtins.md#tokenize). | `string(string command, optional float bindmap) findkeysforcommand = #610;` |
| [`gecko_create`](../37-quakec-builtins-reference/13-menuqc-builtins.md#gecko_create) | Создаёт встроенную браузерную поверхность и связывает её с именованным shader-слотом. | `float(string name, optional string initialURI) gecko_create = #487;` |
| [`gecko_destroy`](../37-quakec-builtins-reference/13-menuqc-builtins.md#gecko_destroy) | [`gecko_destroy`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_destroy) освобождает браузерный instance и связанный shader-слот. | `void(string name) gecko_destroy = #488;` |
| [`gecko_navigate`](../37-quakec-builtins-reference/13-menuqc-builtins.md#gecko_navigate) | [`gecko_navigate`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_navigate) отправляет встроенному браузеру команду навигации. | `void(string name, string URI) gecko_navigate = #489;` |
| [`gecko_keyevent`](../37-quakec-builtins-reference/13-menuqc-builtins.md#gecko_keyevent) | [`gecko_keyevent`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_keyevent) пересылает клавиатурные события из `Menu_InputEvent` прямо во встроенный браузер. | `float(string name, float key, float eventtype, optional float charcode) gecko_keyevent = #490;` |
| [`gecko_mousemove`](../37-quakec-builtins-reference/13-menuqc-builtins.md#gecko_mousemove) | [`gecko_mousemove`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_mousemove) сообщает странице относительное положение мыши внутри браузерного прямоугольника. | `void(string name, float x, float y) gecko_mousemove = #491;` |
| [`gecko_resize`](../37-quakec-builtins-reference/13-menuqc-builtins.md#gecko_resize) | [`gecko_resize`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_resize) просит backend перестроить внутренний рендер-буфер под новый размер. | `void(string name, float w, float h) gecko_resize = #492;` |
| [`gecko_get_texture_extent`](../37-quakec-builtins-reference/13-menuqc-builtins.md#gecko_get_texture_extent) | [`gecko_get_texture_extent`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_get_texture_extent) возвращает текущий фактический размер браузерной текстуры. | `vector(string name) gecko_get_texture_extent = #493;` |

### Редактор карт, криптография и разные редкие builtins

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`brush_calcfacepoints`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#brush_calcfacepoints) | Не работает с `entity` и не использует callback. | `int(int faceid, brushface_t *in_faces, int numfaces, vector *points, int maxpoints) brush_calcfacepoints = #0:brush_calcfacepoints;` |
| [`brush_create`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#brush_create) | Вставляет новый brush прямо в редактируемую модель и возвращает его числовой `brushid`, а не `entity`. | `int(float modelidx, brushface_t *in_faces, int numfaces, int contents, optional int brushid) brush_create = #0:brush_create;` |
| [`brush_delete`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#brush_delete) | Удаляет brush по паре `modelidx + brushid`. | `void(float modelidx, int brushid) brush_delete = #0:brush_delete;` |
| [`brush_findinvolume`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#brush_findinvolume) | Ищет brushes/грани внутри выпуклого объёма и возвращает количество записанных результатов. | `int(float modelid, vector *planes, float *dists, int numplanes, int *out_brushes, int *out_faces, int maxresults) brush_findinvolume = #0:brush_findinvolume;` |
| [`brush_get`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#brush_get) | Выгружает описание существующего браша обратно в массив структур `brushface_t` и возвращает число реально скопированных граней. | `int(float modelidx, int brushid, brushface_t *out_faces, int maxfaces, int *out_contents) brush_get = #0:brush_get;` |
| [`brush_getfacepoints`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#brush_getfacepoints) | Возвращает вершины конкретной грани уже существующего браша. | `int(float modelid, int brushid, int faceid, vector *points, int maxpoints) brush_getfacepoints = #0:brush_getfacepoints;` |
| [`brush_selected`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#brush_selected) | Не возвращает текущий «выбранный brush». Это setter/getter transient-флага выделения для конкретного brush/face и он возвращает предыдущее состояние. | `float(float modelid, int brushid, int faceid, float selectedstate) brush_selected = #0:brush_selected;` |
| [`bulleten`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#bulleten) | В актуальном коде `bulleten` оставлен только в закомментированных строках совместимости как напоминание о старом убранном слоте. | `bulleten` — удалённый legacy-builtin; исторически упоминался у слота `#243`, но в текущих таблицах FTEQW не экспортируется. |
| [`cin_close`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#cin_close) | Menu-алиас к тому же media API, что и [`gecko_destroy`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#gecko_destroy). | `void(string id) cin_close = #462;` |
| [`cin_getstate`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#cin_getstate) | Возвращает не «булево играет / не играет», а реальное внутреннее состояние медиаплеера для указанного слота. | `float(string id) cin_getstate = #464;` |
| [`cin_open`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#cin_open) | Регистрирует 2D shader с `videomap`, привязывает к нему cinematic и сразу отправляет backend'у reset. | `float(string file, string id) cin_open = #461;` |
| [`cin_restart`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#cin_restart) | Ищет уже открытый cinematic по имени и шлёт ему reset. | `void(string id) cin_restart = #465;` |
| [`cin_setstate`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#cin_setstate) | Напрямую передаёт новое состояние в media backend. | `void(string id, float newstate) cin_setstate = #463;` |
| [`controller_query`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#controller_query) | В текущем FTEQW реализован и не является заглушкой. | `void(float device) controller_query = #740;` |
| [`controller_rumble`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#controller_rumble) | Тоже реализован: значения приводятся к 16-битным амплитудам и 32-битной длительности, после чего builtin передаёт их в системный input backend. | `void(float device, float lowmult, float highmult, float msec) controller_rumble = #741;` |
| [`controller_rumbletriggers`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#controller_rumbletriggers) | Работает аналогично `controller_rumble`, но использует отдельный путь для устройств с раздельной вибрацией триггеров. | `void(float device, float leftmult, float rightmult, float msec) controller_rumbletriggers = #742;` |
| [`crypto_getencryptlevel`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#crypto_getencryptlevel) | Builtin присутствует в MenuQC-таблице, но в текущей реализации просто возвращает пустую строку/`NULL`. | `string(string serveraddress) crypto_getencryptlevel = #635;` |
| [`crypto_getidfp`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#crypto_getidfp) | Как и остальные `crypto_*` helper'ы этой группы, сейчас builtin только возвращает пустую строку. | `string(string serveraddress) crypto_getidfp = #634;` |
| [`crypto_getidstatus`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#crypto_getidstatus) | Текущая реализация всегда возвращает `0`. | `float(string serveraddress) crypto_getidstatus = #643;` |
| [`crypto_getkeyfp`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#crypto_getkeyfp) | В текущем коде просто возвращает пустую строку/`NULL`. | `string(string serveraddress) crypto_getkeyfp = #633;` |
| [`crypto_getmyidfp`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#crypto_getmyidfp) | Совместимый stub без реальной криптологики: результатом всегда будет пустая строка. | `string(float slot) crypto_getmyidfp = #637;` |
| [`crypto_getmyidstatus`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#crypto_getmyidstatus) | Возвращает `0` и ничего не проверяет. | `float(float slot) crypto_getmyidstatus = #641;` |
| [`crypto_getmykeyfp`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#crypto_getmykeyfp) | Возвращает пустую строку/`NULL`; полноценный доступ к локальным ключам через этот API сейчас не реализован. | `string(float slot) crypto_getmykeyfp = #636;` |
| [`free_pic`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#free_pic) | Несмотря на название, `free_pic` в текущем MenuQC фактически no-op. | `void(string picname) free_pic = #453;` |
| [`gettime`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#gettime) | И `gettimef` используют одну и ту же логику. | `float(optional float timetype) gettime = #519;` |
| [`gettimed`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#gettimed) | Отличается от `gettime` в первую очередь типом результата: `double` меньше теряет точность на длинном аптайме. | `__double(optional int timetype) gettimed = #0:gettimed;` |
| [`gettimef`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#gettimef) | Это просто float-обёртка над `gettimed`, а не отдельный «чисто CSQC» источник времени. | `float(optional float timetype) gettimef = #519;` |
| [`gp_getlayout`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#gp_getlayout) | В текущем коде не заглушка: MenuQC/CSQC используют это имя для получения enum типа/раскладки контроллера. | `float(float devid) gp_getlayout = #0:gp_getlayout;` |
| [`gp_rumble`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#gp_rumble) | Name-mapped alias к тому же rumble backend'у, что и `controller_rumble`. | `void(float devid, float amp_low, float amp_high, float duration) gp_rumble = #0:gp_rumble;` |
| [`gp_rumbletriggers`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#gp_rumbletriggers) | Name-mapped alias для устройств с отдельной вибрацией триггеров. | `void(float devid, float left, float right, float duration) gp_rumbletriggers = #0:gp_rumbletriggers;` |
| [`gp_setledcolor`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#gp_setledcolor) | Реализован и передаёт RGB-вектор в системный backend подсветки контроллера. | `void(float devid, vector color) gp_setledcolor = #0:gp_setledcolor;` |
| [`gp_settriggerfx`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#gp_settriggerfx) | Тоже реализован. | `void(float devid, void *data, int size) gp_settriggerfx = #0:gp_settriggerfx;` |
| [`js_run_script`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#js_run_script) | Рабочая реализация `js_run_script` существует только в web/emscripten-сборках и возвращает строковый результат выполнения JavaScript. | `string(string javascript) js_run_script = #0:js_run_script;` |
| [`map_builtin`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#map_builtin) | Является фундаментальной частью подсистемы динамической рефлексии и обратной совместимости API в движке FTEQW. | `float(string builtinname, float opcodenum) map_builtin = #220;` |
| [`patch_create`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#patch_create) | Создаёт или заменяет patch внутри модели и возвращает числовой `patchid`. | `int(float modelidx, int oldpatchid, patchvert_t *in_controlverts, patchinfo_t in_info) patch_create = #0:patch_create;` |
| [`patch_evaluate`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#patch_evaluate) | Не принимает `entity patch` и не меняет модель сам по себе. | `int(patchvert_t *in_controlverts, patchvert_t *out_renderverts, int maxout, patchinfo_t *inout_info) patch_evaluate = #0:patch_evaluate;` |
| [`patch_getcp`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#patch_getcp) | Несмотря на имя, `patch_getcp` не возвращает одну control point по координатам `x/y`. | `int(float modelidx, int patchid, patchvert_t *out_controlverts, int maxcp, patchinfo_t *out_info) patch_getcp = #0:patch_getcp;` |
| [`patch_getmesh`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#patch_getmesh) | Выгружает уже рассчитанный tessellated mesh патча. | `int(float modelidx, int patchid, patchvert_t *out_verts, int maxverts, patchinfo_t *out_info) patch_getmesh = #0:patch_getmesh;` |
| [`print_csqc`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#print_csqc) | В MenuQC не отправляет данные в другую VM. | `void(string text, ...) print_csqc = #339;` |
| [`removeinstant`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#removeinstant) | Действительно удаляет entity сразу и разрешает немедленное повторное использование её слота. | `void(entity ent) removeinstant = #0:removeinstant;` |
| [`setwatchpoint`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#setwatchpoint) | Программный эквивалент консольных команд `watchpoint_*`. | `void(string name, float evaltype, void *ptr) setwatchpoint = #0:setwatchpoint;` |
| [`stachievement_query`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#stachievement_query) | Представляет собой нереализованный опкод подсистемы интеграции с платформой Valve. | `stachievement_query(...)` — данная встроенная функция не реализована и является жесткой заглушкой (номера: CSQC=#731, MenuQC=#731). |
| [`stachievement_register`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#stachievement_register) | Это пустой зарезервированный слот под опкодом `#735`. | `stachievement_register(...)` — данная встроенная функция не реализована и является жесткой заглушкой (номера: CSQC=#735, MenuQC=#735). |
| [`stachievement_unlock`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#stachievement_unlock) | Является нереализованным опкодом №730, который должен был отвечать за разблокировку игровых достижений. | `stachievement_unlock(...)` — данная встроенная функция не реализована и является жесткой заглушкой (номера: CSQC=#730, MenuQC=#730). |
| [`ststat_increment`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#ststat_increment) | Это пустой слот под номером `#733`, задумывавшийся для пошагового увеличения счётчиков статистики. | `ststat_increment(...)` — данная встроенная функция не реализована и является жесткой заглушкой (номера: CSQC=#733, MenuQC=#733). |
| [`ststat_query`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#ststat_query) | Задумывалась как встроенный метод для чтения числовых показателей пользовательской или глобальной статистики. | `ststat_query(...)` — данная встроенная функция не реализована и является жесткой заглушкой (номера: CSQC=#734, MenuQC=#734). |
| [`ststat_register`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#ststat_register) | Представляет собой зарезервированный под номером `#736` слот для предварительного объявления статистической переменной. | `ststat_register(...)` — данная встроенная функция не реализована и является жесткой заглушкой (номера: CSQC=#736, MenuQC=#736). |
| [`ststat_setvalue`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#ststat_setvalue) | Задумывалась как инструмент жёсткой перезаписи статистических значений новыми абсолютными числами. | `ststat_setvalue(...)` — данная встроенная функция не реализована и является жесткой заглушкой (номера: CSQC=#732, MenuQC=#732). |
| [`videoplaying`](../37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md#videoplaying) | В MenuQC — это shortcut для запроса состояния глобального media player без имени конкретного shader'а. | `float() videoplaying = #355;` |

---

## Переменные движка (cvar)

Всего задокументировано: **1279** cvar. Полный постатейный разбор — в разделе [«38. Переменные движка»](../README.md#переменные-движка-cvar-reference).


### Видео, экран и общий рендеринг

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`crosshair`](../38-cvars-reference/01-video-rendering-cvars.md#crosshair) | Включает встроенный прицел. | `crosshair(boolean/int)` |
| [`crosshaircorrect`](../38-cvars-reference/01-video-rendering-cvars.md#crosshaircorrect) | Сдвигает прицел так, чтобы он лучше совпадал с фактической точкой попадания оружия, если модель оружия расположена ниже реальной камеры. | `crosshaircorrect(boolean/int)` |
| [`crosshairimage`](../38-cvars-reference/01-video-rendering-cvars.md#crosshairimage) | Позволяет заменить стандартный прицел внешним изображением или шейдером. | `crosshairimage(string)` |
| [`crosshairsize`](../38-cvars-reference/01-video-rendering-cvars.md#crosshairsize) | Задаёт виртуальный размер встроенного прицела. | `crosshairsize(int)` |
| [`d_lodbias`](../38-cvars-reference/01-video-rendering-cvars.md#d_lodbias) | Историческое имя cvar для смещения выбора mip-уровней текстур; в коде у неё есть алиас `gl_texture_lodbias`. Ноль оставляет штатный выбор, положительные значения заставляют рендер раньше брать более грубые mip-уровни и размывают картинку, а отрицательные делают изображение резче ценой мерцания и алиасинга на дистанции. Переменная архивируется в конфиг (`CVAR_ARCHIVE`), имеет `CVAR_RENDERERCALLBACK`, поэтому обычно применяется на лету. | `d_lodbias(float)` |
| [`ffov`](../38-cvars-reference/01-video-rendering-cvars.md#ffov) | Задаёт отдельное поле зрения для нестандартных проекций, которые включаются через `r_projection`. | `ffov(float/string)` |
| [`fov`](../38-cvars-reference/01-video-rendering-cvars.md#fov) | Основное игровое поле зрения камеры. | `fov(float)` |
| [`gl_affinemodels`](../38-cvars-reference/01-video-rendering-cvars.md#gl_affinemodels) | Включает аффинную выборку текстур для моделей, имитируя искажения программного рендера Quake. | `gl_affinemodels(boolean/int)` |
| [`gl_compress`](../38-cvars-reference/01-video-rendering-cvars.md#gl_compress) | Разрешает автоматическое сжатие текстур драйвером даже для контента, который изначально не был сохранён в сжатом формате. | `gl_compress(boolean/int)` |
| [`gl_dither`](../38-cvars-reference/01-video-rendering-cvars.md#gl_dither) | Включает или отключает аппаратный dithering там, где его поддерживает OpenGL-путь. | `gl_dither(boolean/int)` |
| [`gl_finish`](../38-cvars-reference/01-video-rendering-cvars.md#gl_finish) | Принудительно вызывает `glFinish`, то есть заставляет CPU ждать полного завершения работы GPU перед продолжением кадра. | `gl_finish(boolean/int)` |
| [`gl_lerpimages`](../38-cvars-reference/01-video-rendering-cvars.md#gl_lerpimages) | Включает более мягкий ресемплинг изображений, когда движку приходится работать с NPOT-ограничениями старых драйверов. | `gl_lerpimages(boolean/int)` |
| [`gl_max_size`](../38-cvars-reference/01-video-rendering-cvars.md#gl_max_size) | Ограничивает максимальный размер текстуры, который движок разрешает себе использовать, даже если драйвер способен на большее. | `gl_max_size(int)` |
| [`gl_motionblur`](../38-cvars-reference/01-video-rendering-cvars.md#gl_motionblur) | Управляет силой простого накопительного motion blur. | `gl_motionblur(float)` |
| [`gl_motionblurscale`](../38-cvars-reference/01-video-rendering-cvars.md#gl_motionblurscale) | Масштабирует смещение, по которому строится motion blur. | `gl_motionblurscale(float)` |
| [`gl_overbright`](../38-cvars-reference/01-video-rendering-cvars.md#gl_overbright) | Управляет overbright-поведением светокарт и общей яркостью классического GL-пути. | `gl_overbright(int)` |
| [`gl_picmip`](../38-cvars-reference/01-video-rendering-cvars.md#gl_picmip) | Глобально уменьшает размер world/model-текстур по экспоненциальной схеме. | `gl_picmip(int)` |
| [`gl_picmip2d`](../38-cvars-reference/01-video-rendering-cvars.md#gl_picmip2d) | Отдельно уменьшает разрешение HUD- и menu-текстур. | `gl_picmip2d(int)` |
| [`gl_picmip_other`](../38-cvars-reference/01-video-rendering-cvars.md#gl_picmip_other) | Добавляется к `gl_picmip` именно для model-текстур. | `gl_picmip_other(int)` |
| [`gl_picmip_sprites`](../38-cvars-reference/01-video-rendering-cvars.md#gl_picmip_sprites) | Добавляется к `gl_picmip` только для sprite-текстур. | `gl_picmip_sprites(int)` |
| [`gl_picmip_world`](../38-cvars-reference/01-video-rendering-cvars.md#gl_picmip_world) | Добавляется к `gl_picmip` только для world-текстур BSP-геометрии. | `gl_picmip_world(int)` |
| [`gl_polyblend`](../38-cvars-reference/01-video-rendering-cvars.md#gl_polyblend) | Разрешает временные полноэкранные цветовые наложения: урон, бонусы, содержимое воды и другие screen tints. | `gl_polyblend(boolean/int)` |
| [`gl_smoothcrosshair`](../38-cvars-reference/01-video-rendering-cvars.md#gl_smoothcrosshair) | Сглаживает отрисовку стандартного прицела. | `gl_smoothcrosshair(boolean/int)` |
| [`gl_texture_anisotropy`](../38-cvars-reference/01-video-rendering-cvars.md#gl_texture_anisotropy) | Управляет анизотропной фильтрацией; в коде также сохранён старый алиас `gl_texture_anisotropic_filtering`. | `gl_texture_anisotropy(int)` |
| [`gl_texturemode`](../38-cvars-reference/01-video-rendering-cvars.md#gl_texturemode) | Главная переменная фильтрации world/model-текстур. | `gl_texturemode(string)` |
| [`gl_texturemode2d`](../38-cvars-reference/01-video-rendering-cvars.md#gl_texturemode2d) | То же самое, что `gl_texturemode`, но для 2D-изображений: HUD, меню, консольные панели и прочие интерфейсные текстуры. | `gl_texturemode2d(string)` |
| [`mod_external_vis`](../38-cvars-reference/01-video-rendering-cvars.md#mod_external_vis) | Разрешает загрузку внешних `.vis`-патчей для Quake-карт, что особенно важно для корректной прозрачной воды и похожих трюков видимости. | `mod_external_vis(boolean/int)` |
| [`r_drawflat`](../38-cvars-reference/01-video-rendering-cvars.md#r_drawflat) | Показывает мир в режиме плоских заливок вместо нормальных текстур; в коде есть алиас `gl_textureless`. | `r_drawflat(boolean/int)` |
| [`r_drawviewmodel`](../38-cvars-reference/01-video-rendering-cvars.md#r_drawviewmodel) | Управляет отрисовкой оружия и прочей first-person модели игрока. | `r_drawviewmodel(boolean/int)` |
| [`r_fxaa`](../38-cvars-reference/01-video-rendering-cvars.md#r_fxaa) | Включает постпроцессный FXAA, который сглаживает лесенки уже по готовому кадру. | `r_fxaa(boolean/int)` |
| [`r_lodbias`](../38-cvars-reference/01-video-rendering-cvars.md#r_lodbias) | Смещает выбор model-LOD для тех моделей, у которых вообще есть уровни детализации. | `r_lodbias(int)` |
| [`r_lodscale`](../38-cvars-reference/01-video-rendering-cvars.md#r_lodscale) | Масштабирует агрессивность model-LOD. | `r_lodscale(float)` |
| [`r_noframegrouplerp`](../38-cvars-reference/01-video-rendering-cvars.md#r_noframegrouplerp) | Отключает интерполяцию между кадрами групп анимации у классических моделей. | `r_noframegrouplerp(boolean/int)` |
| [`r_nolerp`](../38-cvars-reference/01-video-rendering-cvars.md#r_nolerp) | Глобально отключает интерполяцию моделей между кадрами и позициями там, где она доступна. | `r_nolerp(boolean/int)` |
| [`r_projection`](../38-cvars-reference/01-video-rendering-cvars.md#r_projection) | Выбирает тип геометрической проекции. | `r_projection(int)` |
| [`r_renderscale`](../38-cvars-reference/01-video-rendering-cvars.md#r_renderscale) | Заставляет движок рендерить 3D-сцену во внутреннем разрешении, отличном от итогового. | `r_renderscale(float)` |
| [`r_showtris`](../38-cvars-reference/01-video-rendering-cvars.md#r_showtris) | Рисует геометрию каркасом поверх обычной сцены; в коде это та же переменная, что привычный алиас `r_wireframe`. | `r_showtris(boolean/int)` |
| [`r_viewmodel_fov`](../38-cvars-reference/01-video-rendering-cvars.md#r_viewmodel_fov) | Позволяет задать отдельный FOV для модели оружия. | `r_viewmodel_fov(float/string)` |
| [`r_viewmodel_quake`](../38-cvars-reference/01-video-rendering-cvars.md#r_viewmodel_quake) | Возвращает «странные» движения viewmodel из оригинального Quake. | `r_viewmodel_quake(boolean/int)` |
| [`r_wateralpha`](../38-cvars-reference/01-video-rendering-cvars.md#r_wateralpha) | Управляет прозрачностью воды. | `r_wateralpha(float)` |
| [`r_waterstyle`](../38-cvars-reference/01-video-rendering-cvars.md#r_waterstyle) | Выбирает способ отрисовки воды и телепортеров. | `r_waterstyle(int)` |
| [`r_waterwarp`](../38-cvars-reference/01-video-rendering-cvars.md#r_waterwarp) | Включает полноэкранный warp-эффект под водой; встроенное описание отдельно отмечает, что `-1` принудительно заставляет использовать старый FOV-warp fallback. | `r_waterwarp(float)` |
| [`scr_fov_mode`](../38-cvars-reference/01-video-rendering-cvars.md#scr_fov_mode) | Определяет, как именно трактуется `fov` при разных соотношениях сторон. | `scr_fov_mode(int)` |
| [`vid_bpp`](../38-cvars-reference/01-video-rendering-cvars.md#vid_bpp) | Задаёт желаемую цветовую глубину видеорежима. | `vid_bpp(int)` |
| [`vid_conautoscale`](../38-cvars-reference/01-video-rendering-cvars.md#vid_conautoscale) | Масштабирует весь 2D-слой: HUD, консоль, шрифты и меню. | `vid_conautoscale(float)` |
| [`vid_conheight`](../38-cvars-reference/01-video-rendering-cvars.md#vid_conheight) | Принудительно задаёт виртуальную высоту экрана для 2D-слоя. | `vid_conheight(int)` |
| [`vid_conwidth`](../38-cvars-reference/01-video-rendering-cvars.md#vid_conwidth) | Принудительно задаёт виртуальную ширину экрана для HUD и консоли. | `vid_conwidth(int)` |
| [`vid_depthbits`](../38-cvars-reference/01-video-rendering-cvars.md#vid_depthbits) | Задаёт желаемую глубину Z-буфера. | `vid_depthbits(int)` |
| [`vid_desktopsettings`](../38-cvars-reference/01-video-rendering-cvars.md#vid_desktopsettings) | Заставляет движок игнорировать `vid_width` и `vid_height` и использовать параметры рабочего стола. | `vid_desktopsettings(boolean/int)` |
| [`vid_displayfrequency`](../38-cvars-reference/01-video-rendering-cvars.md#vid_displayfrequency) | Историческое имя cvar, которое управляет желаемой частотой обновления экрана; в коде оно хранится в переменной `vid_refreshrate`. `0` обычно означает «не запрашивать конкретную герцовку», а явные значения вроде `60`, `120` или `144` полезны только там, где драйвер и режим действительно поддерживают их. Переменная архивируется (`CVAR_ARCHIVE`), имеет `CVAR_VIDEOLATCH` и требует `vid_restart`. | `vid_displayfrequency(int)` |
| [`vid_fullscreen`](../38-cvars-reference/01-video-rendering-cvars.md#vid_fullscreen) | Определяет режим полноэкранного вывода. | `vid_fullscreen(int)` |
| [`vid_hardwaregamma`](../38-cvars-reference/01-video-rendering-cvars.md#vid_hardwaregamma) | Управляет тем, как движок применяет гамму и контраст: через аппаратные [gamma](../38-cvars-reference/06-ui-console-input-cvars.md#gamma) ramps, GLSL или комбинированный путь. | `vid_hardwaregamma(int)` |
| [`vid_height`](../38-cvars-reference/01-video-rendering-cvars.md#vid_height) | Запрашиваемая физическая высота окна или эксклюзивного режима в пикселях. | `vid_height(int)` |
| [`vid_multisample`](../38-cvars-reference/01-video-rendering-cvars.md#vid_multisample) | Запрашивает MSAA при создании видеоконтекста; у переменной также есть алиас `vid_samples`. | `vid_multisample(int)` |
| [`vid_renderer`](../38-cvars-reference/01-video-rendering-cvars.md#vid_renderer) | Выбирает backend рендера. Пустая строка позволяет движку выбрать обычный путь самой сборки, а явное значение вроде `vk`, `gl` или `egl` пытается принудить конкретный API, если он действительно скомпилирован и доступен на вашей платформе. | `vid_renderer(string)` |
| [`vid_srgb`](../38-cvars-reference/01-video-rendering-cvars.md#vid_srgb) | Управляет работой с sRGB/линейным цветовыми пространствами. | `vid_srgb(int)` |
| [`vid_vsync`](../38-cvars-reference/01-video-rendering-cvars.md#vid_vsync) | Управляет ожиданием вертикальной синхронизации; в коде у переменной есть алиас `vid_wait`. | `vid_vsync(int)` |
| [`vid_width`](../38-cvars-reference/01-video-rendering-cvars.md#vid_width) | Запрашиваемая физическая ширина окна или полноэкранного режима в пикселях. | `vid_width(int)` |
| [`_vid_renderer_opts`](../38-cvars-reference/01-video-rendering-cvars.md#_vid_renderer_opts) | Служебный, доступный только для чтения cvar (флаги `CVAR_NOSET\|CVAR_NOSAVE` — игрок не может изменить или сохранить его значение). | `_vid_renderer_opts(string)` |
| [`brightness`](../38-cvars-reference/01-video-rendering-cvars.md#brightness) | Яркость — это количество «белого», которое нужно добавить к каждому пикселю на экране. | `brightness(float)` |
| [`gl_ati_truform_type`](../38-cvars-reference/01-video-rendering-cvars.md#gl_ati_truform_type) | Выбирает режим ATI TruForm/PN Triangles для fallback-тесселяции meshes. 0 включает quadratic normal mode, любое ненулевое значение — linear normal mode; point mode остаётся cubic. | `gl_ati_truform_type(int)` |
| [`gl_blacklist_texture_compression`](../38-cvars-reference/01-video-rendering-cvars.md#gl_blacklist_texture_compression) | При включении блокирует распознавание всех форматов сжатых текстур. | `gl_blacklist_texture_compression(int)` |
| [`gl_blend2d`](../38-cvars-reference/01-video-rendering-cvars.md#gl_blend2d) | Переключатель 2D blending в GL renderer. | `gl_blend2d(int)` |
| [`gl_blendsprites`](../38-cvars-reference/01-video-rendering-cvars.md#gl_blendsprites) | Определяет способ смешивания спрайтов. | `gl_blendsprites(int)` |
| [`gl_conback`](../38-cvars-reference/01-video-rendering-cvars.md#gl_conback) | Указывает, какой шейдер/изображение conback использовать. | `gl_conback(string)` |
| [`gl_cshiftpercent`](../38-cvars-reference/01-video-rendering-cvars.md#gl_cshiftpercent) | Масштабирует интенсивность экранных цветовых сдвигов в процентах. | `gl_cshiftpercent(int)` |
| [`gl_detail`](../38-cvars-reference/01-video-rendering-cvars.md#gl_detail) | Исторический переключатель detail textures. В этой ревизии его объявление и регистрация закомментированы, так что cvar фактически не участвует в рендеринге. | `gl_detail(int)` |
| [`gl_detailscale`](../38-cvars-reference/01-video-rendering-cvars.md#gl_detailscale) | Исторический параметр масштаба detail textures. Как и gl_detail, в текущем коде закомментирован и обычно не даёт эффекта. | `gl_detailscale(int)` |
| [`gl_driver`](../38-cvars-reference/01-video-rendering-cvars.md#gl_driver) | Указывает имя графического драйвера для загрузки. | `gl_driver(string)` |
| [`gl_font`](../38-cvars-reference/01-video-rendering-cvars.md#gl_font) | Шрифты TTF можно загрузить из каталога Windows. \'gl_font cour?col=1,1,1:couri?col=0,1,0\' загружает, например: c:\\windows\\fonts\\cour.ttf, и использует курсивную версию courier для альтернативного текста с определенными цветовыми оттенками. | `gl_font(string)` |
| [`gl_immutable_buffers`](../38-cvars-reference/01-video-rendering-cvars.md#gl_immutable_buffers) | Определяет, использовать ли неизменяемые выделения памяти графического процессора для статических буферов вершин OpenGL. | `gl_immutable_buffers(int)` |
| [`gl_immutable_textures`](../38-cvars-reference/01-video-rendering-cvars.md#gl_immutable_textures) | Определяет, использовать ли неизменяемые выделения памяти графического процессора для текстур OpenGL. | `gl_immutable_textures(int)` |
| [`gl_lateswap`](../38-cvars-reference/01-video-rendering-cvars.md#gl_lateswap) | Опция, связанная с поздним swap или present кадра в GL-видеобэкендах. | `gl_lateswap(int)` |
| [`gl_lightmap_average`](../38-cvars-reference/01-video-rendering-cvars.md#gl_lightmap_average) | Определите значения карты освещения на основе центра многоугольника. | `gl_lightmap_average(int)` |
| [`gl_lightmap_nearest`](../38-cvars-reference/01-video-rendering-cvars.md#gl_lightmap_nearest) | Используйте ближайшую выборку для карт освещения. | `gl_lightmap_nearest(int)` |
| [`gl_load24bit`](../38-cvars-reference/01-video-rendering-cvars.md#gl_load24bit) | Разрешает загрузку 24-bit и hi-res replacement textures, а также связанных внешних ресурсов вместо палитровых или WAD-данных. | `gl_load24bit(int)` |
| [`gl_maxdist`](../38-cvars-reference/01-video-rendering-cvars.md#gl_maxdist) | Расстояние дальней плоскости отсечения. | `gl_maxdist(int)` |
| [`gl_mindist`](../38-cvars-reference/01-video-rendering-cvars.md#gl_mindist) | Расстояние до ближней плоскости отсечения. | `gl_mindist(int)` |
| [`gl_nocolors`](../38-cvars-reference/01-video-rendering-cvars.md#gl_nocolors) | Игнорирует цвета и скины игроков, уменьшая использование памяти текстур ценой незнания, убиваете ли вы своих товарищей по команде. | `gl_nocolors(int)` |
| [`gl_nohwblend`](../38-cvars-reference/01-video-rendering-cvars.md#gl_nohwblend) | Если 1, не используйте аппаратные гамма-рампы для переходных эффектов, которые изменяются в каждом кадре (не влияет на долгосрочные эффекты, такие как удержание четырехугольника или подводные оттенки). | `gl_nohwblend(int)` |
| [`gl_outline`](../38-cvars-reference/01-video-rendering-cvars.md#gl_outline) | Нарисуйте стилизованные контуры. | `gl_outline(int)` |
| [`gl_outline_width`](../38-cvars-reference/01-video-rendering-cvars.md#gl_outline_width) | Ширина этих контуров. | `gl_outline_width(int)` |
| [`gl_overbright_all`](../38-cvars-reference/01-video-rendering-cvars.md#gl_overbright_all) | Принудительно включает overbright-сдвиг lightmap не только для моделей с флагом MDLF_NEEDOVERBRIGHT, а для всех подходящих поверхностей. 0 оставляет overbright только там, где он явно нужен по данным модели. | `gl_overbright_all(int)` |
| [`gl_overbright_models`](../38-cvars-reference/01-video-rendering-cvars.md#gl_overbright_models) | Удваивает яркость моделей, чтобы соответствовать одноименной ошибке QuakeSpasm. | `gl_overbright_models(int)` |
| [`gl_part_flame`](../38-cvars-reference/01-video-rendering-cvars.md#gl_part_flame) | Включите излучение частиц из моделей. | `gl_part_flame(int)` |
| [`gl_pbolightmaps`](../38-cvars-reference/01-video-rendering-cvars.md#gl_pbolightmaps) | Определяет, использовать ли PBO для потоковой передачи обновлений карты освещения. | `gl_pbolightmaps(int)` |
| [`gl_polyblend_edgesize`](../38-cvars-reference/01-video-rendering-cvars.md#gl_polyblend_edgesize) | Это ограничивает сдвиг цвета к краю экрана, при этом значение определяет размер этих границ. | `gl_polyblend_edgesize(int)` |
| [`gl_schematics`](../38-cvars-reference/01-video-rendering-cvars.md#gl_schematics) | Режим рендеринга трюков, который рисует длину различных краев мира. | `gl_schematics(int)` |
| [`gl_screenangle`](../38-cvars-reference/01-video-rendering-cvars.md#gl_screenangle) | Поворачивает итоговый экран и добавляет roll к углу камеры. | `gl_screenangle(int)` |
| [`gl_shadeq1_name`](../38-cvars-reference/01-video-rendering-cvars.md#gl_shadeq1_name) | Переименуйте все поверхности из quake1 bsps, используя этот шаблон для имен шейдеров. | `gl_shadeq1_name(string)` |
| [`gl_shaftlight`](../38-cvars-reference/01-video-rendering-cvars.md#gl_shaftlight) | Задаёт абсолютную яркость beam или shaft-моделей временных эффектов под HEXEN2-кодом. | `gl_shaftlight(float)` |
| [`gl_simpleitems`](../38-cvars-reference/01-video-rendering-cvars.md#gl_simpleitems) | Замените модели более простыми спрайтами. | `gl_simpleitems(int)` |
| [`gl_specular`](../38-cvars-reference/01-video-rendering-cvars.md#gl_specular) | Множитель зеркальных эффектов. | `gl_specular(float)` |
| [`gl_specular_fallback`](../38-cvars-reference/01-video-rendering-cvars.md#gl_specular_fallback) | Базовое значение gloss/specular, которое движок подставляет, когда у материала нет отдельной specular texture. | `gl_specular_fallback(float)` |
| [`gl_specular_fallbackexp`](../38-cvars-reference/01-video-rendering-cvars.md#gl_specular_fallbackexp) | Экспонента для сгенерированного fallback specular или gloss map, кодируется в alpha служебной текстуры. | `gl_specular_fallbackexp(int)` |
| [`gl_specular_power`](../38-cvars-reference/01-video-rendering-cvars.md#gl_specular_power) | Базовая степень specular highlight для GLSL-шейдеров через SPECULAR_BASE_POW. | `gl_specular_power(int)` |
| [`msg_filter_frags`](../38-cvars-reference/01-video-rendering-cvars.md#msg_filter_frags) | Предотвращает появление сообщений о фрагментах на консоли. | `msg_filter_frags(int)` |
| [`msg_filter_pickups`](../38-cvars-reference/01-video-rendering-cvars.md#msg_filter_pickups) | Предотвращает появление сообщений о перехвате на консоли. | `msg_filter_pickups(int)` |
| [`pr_allowbutton1`](../38-cvars-reference/01-video-rendering-cvars.md#pr_allowbutton1) | Предполагается, что поле button1 предназначалось для работы с командой +use, но оно так и не было подключено. | `pr_allowbutton1(int)` |
| [`pr_autocreatecvars`](../38-cvars-reference/01-video-rendering-cvars.md#pr_autocreatecvars) | Неявно создайте любые переменные, которые не существуют при чтении. | `pr_autocreatecvars(int)` |
| [`pr_brokenfloatconvert`](../38-cvars-reference/01-video-rendering-cvars.md#pr_brokenfloatconvert) | Меняет формат QuakeC builtin [ftos](../37-quakec-builtins-reference/02-string-builtins.md#ftos) для нецелых чисел. | `pr_brokenfloatconvert(int)` |
| [`pr_compatabilitytest`](../38-cvars-reference/01-video-rendering-cvars.md#pr_compatabilitytest) | Встроенные функции активируются только в том случае, если было запрошено расширение, частью которого они являются. | `pr_compatabilitytest(int)` |
| [`pr_coreonerror`](../38-cvars-reference/01-video-rendering-cvars.md#pr_coreonerror) | Управляет созданием аварийного дампа серверного QuakeC при фатальной ошибке. | `pr_coreonerror(int)` |
| [`pr_csqc_coreonerror`](../38-cvars-reference/01-video-rendering-cvars.md#pr_csqc_coreonerror) | Переменная определяет, нужно ли сохранять дамп состояния CSQC при аварийном завершении VM. | `pr_csqc_coreonerror(int)` |
| [`pr_csqc_formenus`](../38-cvars-reference/01-video-rendering-cvars.md#pr_csqc_formenus) | Переменная определяет, разрешён ли запуск CSQC в несоединённом состоянии для меню через `CSQC_UnconnectedOkay()` и `CSQC_UnconnectedInit()`. | `pr_csqc_formenus(int)` |
| [`pr_csqc_maxedicts`](../38-cvars-reference/01-video-rendering-cvars.md#pr_csqc_maxedicts) | Переменная задаёт верхнюю границу числа CSQC edict-ов, выделяемых при `PR_InitEnts()`. | `pr_csqc_maxedicts(int)` |
| [`pr_csqc_memsize`](../38-cvars-reference/01-video-rendering-cvars.md#pr_csqc_memsize) | Строковый параметр объёма памяти для клиентской CSQC VM, который при инициализации пропускается через `PR_ReadBytesString()` и передаётся в `PR_Configure()`. | `pr_csqc_memsize(int)` |
| [`pr_debugger`](../38-cvars-reference/01-video-rendering-cvars.md#pr_debugger) | Если этот параметр включен, ошибки контроля качества и события отладки будут включать пошаговое отслеживание. | `pr_debugger(string)` |
| [`pr_droptofloorunits`](../38-cvars-reference/01-video-rendering-cvars.md#pr_droptofloorunits) | Расстояние, на которое допускается падение до пола, считается успешным. | `pr_droptofloorunits(int)` |
| [`pr_enable_profiling`](../38-cvars-reference/01-video-rendering-cvars.md#pr_enable_profiling) | Включает поддержку профилирования. | `pr_enable_profiling(int)` |
| [`pr_enable_uriget`](../38-cvars-reference/01-video-rendering-cvars.md#pr_enable_uriget) | Позволяет игровому коду выполнять прямые HTTP-запросы. | `pr_enable_uriget(int)` |
| [`pr_engine`](../38-cvars-reference/01-video-rendering-cvars.md#pr_engine) | Эта переменная существует для того, чтобы менюqc могло определить, какие настройки/значения, специфичные для движка, перечислять/предлагать. | `pr_engine(string)` |
| [`pr_ext_dp_qc_getsurface`](../38-cvars-reference/01-video-rendering-cvars.md#pr_ext_dp_qc_getsurface) | Установите значение 0, чтобы заблокировать обнаружение расширения. | `pr_ext_dp_qc_getsurface(string)` |
| [`pr_fixbrokenqccarrays`](../38-cvars-reference/01-video-rendering-cvars.md#pr_fixbrokenqccarrays) | В рамках поддержки nq/qw/h2/csqc FTE переназначает поля контроля качества в соответствии с внутренним порядком. | `pr_fixbrokenqccarrays(int)` |
| [`pr_gc_threaded`](../38-cvars-reference/01-video-rendering-cvars.md#pr_gc_threaded) | Указывает, следует ли использовать отдельный поток для сборки мусора временной строки. | `pr_gc_threaded(int)` |
| [`pr_imitatemvdsv`](../38-cvars-reference/01-video-rendering-cvars.md#pr_imitatemvdsv) | Включает встроенные функции, специфичные для mvdsv, и подделывает идентификаторы, чтобы моды, созданные для mvdsv, могли работать правильно и с полным набором функций. | `pr_imitatemvdsv(int)` |
| [`pr_maxedicts`](../38-cvars-reference/01-video-rendering-cvars.md#pr_maxedicts) | Максимальное количество объектов, которые могут появиться на карте одновременно. | `pr_maxedicts(int)` |
| [`pr_no_parsecommand`](../38-cvars-reference/01-video-rendering-cvars.md#pr_no_parsecommand) | Обеспечивает способ обойти недопустимое использование модов SV_ParseClientCommand, например xonotic. | `pr_no_parsecommand(int)` |
| [`pr_no_playerphysics`](../38-cvars-reference/01-video-rendering-cvars.md#pr_no_playerphysics) | Предотвращает поддержку функции контроля качества SV_PlayerPhysics. | `pr_no_playerphysics(int)` |
| [`pr_nonetaccess`](../38-cvars-reference/01-video-rendering-cvars.md#pr_nonetaccess) | Заблокируйте весь прямой доступ к сетевым буферам (встроенная функция writebyte и друзья проигнорируют вызов). | `pr_nonetaccess(int)` |
| [`pr_overridebuiltins`](../38-cvars-reference/01-video-rendering-cvars.md#pr_overridebuiltins) | Определяет, можно ли при подключении дополнительных builtin-функций перезаписывать уже занятые номера builtins. | `pr_overridebuiltins(int)` |
| [`pr_precachepic_slow`](../38-cvars-reference/01-video-rendering-cvars.md#pr_precachepic_slow) | Устаревшая настройка. Должно быть установлено в 0, если это поддерживается. | `pr_precachepic_slow(int)` |
| [`pr_sourcedir`](../38-cvars-reference/01-video-rendering-cvars.md#pr_sourcedir) | Подкаталог, в котором находится ваш источник контроля качества. | `pr_sourcedir(string)` |
| [`pr_ssqc_memsize`](../38-cvars-reference/01-video-rendering-cvars.md#pr_ssqc_memsize) | Объем памяти, доступный QC vm. | `pr_ssqc_memsize(int)` |
| [`pr_tempstringcount`](../38-cvars-reference/01-video-rendering-cvars.md#pr_tempstringcount) | Устарело. Установите значение 16, если вы хотите перерабатывать и повторно использовать одни и те же 16 ссылок на временные строки и сломать множество модов. | `pr_tempstringcount(string)` |
| [`pr_tempstringsize`](../38-cvars-reference/01-video-rendering-cvars.md#pr_tempstringsize) | Устарело. | `pr_tempstringsize(int)` |
| [`r_bloodstains`](../38-cvars-reference/01-video-rendering-cvars.md#r_bloodstains) | Управляет появлением и силой [stains](../41-particle-directives-reference/01-particle-effect-directives.md#stains) или decal-следов от blood и impact particles. 0 полностью отключает такие следы, значения больше 0 масштабируют их интенсивность. | `r_bloodstains(int)` |
| [`r_bluelight_colour`](../38-cvars-reference/01-video-rendering-cvars.md#r_bluelight_colour) | Значения красного, зеленого, синего и радиуса для эффектов EF_BLUE (обычно используются для четырехкратного урона в Quakeworld). | `r_bluelight_colour(string)` |
| [`r_bouncysparks`](../38-cvars-reference/01-video-rendering-cvars.md#r_bouncysparks) | Обеспечивает взаимодействие частиц с поверхностями мира, позволяя создавать отскакивающие частицы, пятна и наклейки. | `r_bouncysparks(int)` |
| [`r_brightlight_colour`](../38-cvars-reference/01-video-rendering-cvars.md#r_brightlight_colour) | Значения красного, зеленого, синего и радиуса для эффектов EF_BRIGHTLIGHT (не используются в ванильном Quake). | `r_brightlight_colour(string)` |
| [`r_clear`](../38-cvars-reference/01-video-rendering-cvars.md#r_clear) | Включает принудительную очистку color и depth buffers перед кадром. | `r_clear(int)` |
| [`r_clearcolour`](../38-cvars-reference/01-video-rendering-cvars.md#r_clearcolour) | Задаёт цвет очистки, применяемый когда r_clear включён. | `r_clearcolour(string)` |
| [`r_clutter_density`](../38-cvars-reference/01-video-rendering-cvars.md#r_clutter_density) | Скелер для подсчета помех. 0 полностью отключает беспорядок. | `r_clutter_density(int)` |
| [`r_clutter_distance`](../38-cvars-reference/01-video-rendering-cvars.md#r_clutter_distance) | Расстояние, на котором беспорядок станет незаметным. | `r_clutter_distance(int)` |
| [`r_coronas`](../38-cvars-reference/01-video-rendering-cvars.md#r_coronas) | Нарисуйте короны на источниках света в реальном времени. | `r_coronas(int)` |
| [`r_decal_noperpendicular`](../38-cvars-reference/01-video-rendering-cvars.md#r_decal_noperpendicular) | Если эта опция включена, декали не будут создаваться на плоскостях под крутым углом относительно обрезанной ориентации декалей. | `r_decal_noperpendicular(int)` |
| [`r_deluxemapping`](../38-cvars-reference/01-video-rendering-cvars.md#r_deluxemapping) | Включает рельефное отображение на основе заранее рассчитанных направлений света. | `r_deluxemapping(int)` |
| [`r_dimlight_colour`](../38-cvars-reference/01-video-rendering-cvars.md#r_dimlight_colour) | Значения красного, зеленого, синего и радиуса для эффектов EF_DIMLIGHT (используются для Quad+pent в Vanilla Quake). | `r_dimlight_colour(string)` |
| [`r_dodgymiptex`](../38-cvars-reference/01-video-rendering-cvars.md#r_dodgymiptex) | Если эта опция включена, это приведет к принудительной регенерации MIP-карт, отбрасывая mips1-4, как это сделал glquake. | `r_dodgymiptex(int)` |
| [`r_dodgypcxfiles`](../38-cvars-reference/01-video-rendering-cvars.md#r_dodgypcxfiles) | Если этот параметр включен, палитра, хранящаяся в файлах pcx, будет игнорироваться для совместимости с quake2. | `r_dodgypcxfiles(int)` |
| [`r_dodgytgafiles`](../38-cvars-reference/01-video-rendering-cvars.md#r_dodgytgafiles) | Многие старые движки glquake имели глючный загрузчик tga, который игнорировал восходящие флаги. | `r_dodgytgafiles(int)` |
| [`r_drawentities`](../38-cvars-reference/01-video-rendering-cvars.md#r_drawentities) | Определяет, рисовать объекты или нет. | `r_drawentities(int)` |
| [`r_drawflame`](../38-cvars-reference/01-video-rendering-cvars.md#r_drawflame) | Установите значение -1, чтобы отключить ВСЕ статические объекты. | `r_drawflame(int)` |
| [`r_drawviewmodelinvis`](../38-cvars-reference/01-video-rendering-cvars.md#r_drawviewmodelinvis) | Определяет, рисовать ли weapon или viewmodel при активной invisibility. | `r_drawviewmodelinvis(int)` |
| [`r_editlights`](../38-cvars-reference/01-video-rendering-cvars.md#r_editlights) | Включает режим редактирования файла .rtlights. | `r_editlights(int)` |
| [`r_explosionlight`](../38-cvars-reference/01-video-rendering-cvars.md#r_explosionlight) | Управляет созданием динамического света у взрывных временных эффектов. | `r_explosionlight(int)` |
| [`r_explosionlight_colour`](../38-cvars-reference/01-video-rendering-cvars.md#r_explosionlight_colour) | Это контролирует начальные значения RGB эффектов EF_EXPLOSION. | `r_explosionlight_colour(string)` |
| [`r_explosionlight_fade`](../38-cvars-reference/01-video-rendering-cvars.md#r_explosionlight_fade) | Это контролирует значения затухания RGB в секунду для эффектов EF_EXPLOSION. | `r_explosionlight_fade(string)` |
| [`r_fastturb`](../38-cvars-reference/01-video-rendering-cvars.md#r_fastturb) | Принудительно переводит water, lava, slime и tele surfaces в быстрый wstyle 0 вместо обычных шейдерных режимов. | `r_fastturb(int)` |
| [`r_fastturbcolour`](../38-cvars-reference/01-video-rendering-cvars.md#r_fastturbcolour) | Цвет, используемый для водных поверхностей, нарисуйте с помощью r_waterstyle 0. | `r_fastturbcolour(string)` |
| [`r_fb_bmodels`](../38-cvars-reference/01-video-rendering-cvars.md#r_fb_bmodels) | Позволяет загружать яркости на карту, а также любые внешние модели bsp. | `r_fb_bmodels(int)` |
| [`r_fb_models`](../38-cvars-reference/01-video-rendering-cvars.md#r_fb_models) | Позволяет использовать яркость на моделях. | `r_fb_models(int)` |
| [`r_floorcolour`](../38-cvars-reference/01-video-rendering-cvars.md#r_floorcolour) | RGB-цвет пола и горизонтальных поверхностей для drawflat или texturless-шейдеров. | `r_floorcolour(string)` |
| [`r_fog_cullentities`](../38-cvars-reference/01-video-rendering-cvars.md#r_fog_cullentities) | Управляет тем, будет ли движок отсекать сущности по плотному туману. | `r_fog_cullentities(int)` |
| [`r_fog_exp2`](../38-cvars-reference/01-video-rendering-cvars.md#r_fog_exp2) | Показывает, как туман исчезает с расстоянием. | `r_fog_exp2(int)` |
| [`r_fog_linear`](../38-cvars-reference/01-video-rendering-cvars.md#r_fog_linear) | Переключает математическую модель тумана. | `r_fog_linear(int)` |
| [`r_fog_permutation`](../38-cvars-reference/01-video-rendering-cvars.md#r_fog_permutation) | Рендерит туман, используя перестановку материалов. 0 лучше работает с шейдерами q3, но в остальном лучший выбор — 1. | `r_fog_permutation(int)` |
| [`r_font_linear`](../38-cvars-reference/01-video-rendering-cvars.md#r_font_linear) | Выбирает фильтрацию текстур шрифтов. 1 загружает шрифты с IF_LINEAR для сглаженного вида, 0 — с IF_NEAREST для более резких пиксельных символов. | `r_font_linear(int)` |
| [`r_graphics`](../38-cvars-reference/01-video-rendering-cvars.md#r_graphics) | Отключение этого параметра приведет к рендерингу в стиле ascii. | `r_graphics(int)` |
| [`r_greenlight_colour`](../38-cvars-reference/01-video-rendering-cvars.md#r_greenlight_colour) | Значения красного, зеленого, синего и радиуса для эффектов EF_GREEN (используются редко). | `r_greenlight_colour(string)` |
| [`r_grenadetrail`](../38-cvars-reference/01-video-rendering-cvars.md#r_grenadetrail) | Определяет эффект следа для моделей с флагом MF_GRENADE и при изменении сразу переинициализирует уже загруженные grenade-модели. | `r_grenadetrail(int)` |
| [`r_hdr_framebuffer`](../38-cvars-reference/01-video-rendering-cvars.md#r_hdr_framebuffer) | Если этот параметр включен, карта будет отображаться в высокоточном буфере кадров изображения. | `r_hdr_framebuffer(int)` |
| [`r_hdr_irisadaptation`](../38-cvars-reference/01-video-rendering-cvars.md#r_hdr_irisadaptation) | Включает авто-подстройку HDR exposure по локальному освещению вокруг игрока. | `r_hdr_irisadaptation(int)` |
| [`r_hdr_irisadaptation_fade_down`](../38-cvars-reference/01-video-rendering-cvars.md#r_hdr_irisadaptation_fade_down) | Ограничивает скорость уменьшения значения адаптации за секунду. | `r_hdr_irisadaptation_fade_down(float)` |
| [`r_hdr_irisadaptation_fade_up`](../38-cvars-reference/01-video-rendering-cvars.md#r_hdr_irisadaptation_fade_up) | Ограничивает скорость увеличения значения адаптации за секунду. | `r_hdr_irisadaptation_fade_up(float)` |
| [`r_hdr_irisadaptation_maxvalue`](../38-cvars-reference/01-video-rendering-cvars.md#r_hdr_irisadaptation_maxvalue) | Верхняя граница для автоматически рассчитанного hdr или exposure value. | `r_hdr_irisadaptation_maxvalue(int)` |
| [`r_hdr_irisadaptation_minvalue`](../38-cvars-reference/01-video-rendering-cvars.md#r_hdr_irisadaptation_minvalue) | Нижняя граница для автоматически рассчитанного hdr или exposure value. | `r_hdr_irisadaptation_minvalue(float)` |
| [`r_hdr_irisadaptation_multiplier`](../38-cvars-reference/01-video-rendering-cvars.md#r_hdr_irisadaptation_multiplier) | Множитель, из которого вычисляется целевой hdr level как multiplier / measured_light. | `r_hdr_irisadaptation_multiplier(int)` |
| [`r_ignoreentpvs`](../38-cvars-reference/01-video-rendering-cvars.md#r_ignoreentpvs) | Отключает pvs-отсечение объектов, отправленных в средство рендеринга. | `r_ignoreentpvs(int)` |
| [`r_ignoremapprefixes`](../38-cvars-reference/01-video-rendering-cvars.md#r_ignoremapprefixes) | Игнорирует загрузку текстур по путям, специфичным для карты. | `r_ignoremapprefixes(int)` |
| [`r_imageextensions`](../38-cvars-reference/01-video-rendering-cvars.md#r_imageextensions) | Определяет порядок расширений, которые движок перебирает при поиске текстур на диске. | `r_imageextensions(string)` |
| [`r_keepimages`](../38-cvars-reference/01-video-rendering-cvars.md#r_keepimages) | Сохраняйте неиспользуемые изображения в памяти для более быстрой загрузки карты. | `r_keepimages(int)` |
| [`r_lavaalpha`](../38-cvars-reference/01-video-rendering-cvars.md#r_lavaalpha) | Прозрачность lava surfaces, если материал сам не задал ALPHA и [watervis](../38-cvars-reference/07-system-misc-cvars.md#watervis) разрешён. | `r_lavaalpha(string)` |
| [`r_lavastyle`](../38-cvars-reference/01-video-rendering-cvars.md#r_lavastyle) | См. r_waterstyle, но влияет только на лаву. | `r_lavastyle(int)` |
| [`r_lerpmuzzlehack`](../38-cvars-reference/01-video-rendering-cvars.md#r_lerpmuzzlehack) | Масштабирует специальный lerp cutoff для некоторых view-моделей оружия и снарядов, у которых отдельные вершины при смене кадров могут «телепортироваться». При ненулевом значении порог активен, и вершины с слишком большим смещением не интерполируются между кадрами, а берутся из текущего кадра, что уменьшает артефакты на muzzle flash и похожих анимациях. | `r_lerpmuzzlehack(int)` |
| [`r_loadlit`](../38-cvars-reference/01-video-rendering-cvars.md#r_loadlit) | Загружать ли файлы освещения. 0: Не загружать внешние данные карты освещения RGB. 1: Загружать, но не генерировать. 2: Создать освещение ldr (если оно не найдено). 3: Создать освещение HDR (если оно не найдено). | `r_loadlit(int)` |
| [`r_loadsurfenvmaps`](../38-cvars-reference/01-video-rendering-cvars.md#r_loadsurfenvmaps) | Загрузите карты среды локального отражения, если таковые имеются. | `r_loadsurfenvmaps(int)` |
| [`r_max_gpu_bones`](../38-cvars-reference/01-video-rendering-cvars.md#r_max_gpu_bones) | Указывает максимальное количество костей, которое может обрабатываться графическим процессором. | `r_max_gpu_bones(string)` |
| [`r_menutint`](../38-cvars-reference/01-video-rendering-cvars.md#r_menutint) | RGB-множитель для fullscreen menu tint shader. | `r_menutint(string)` |
| [`r_meshpitch`](../38-cvars-reference/01-video-rendering-cvars.md#r_meshpitch) | Определяет направление угла наклона в форматах моделей сетки, также влияет на игровой код, поэтому не меняйте значение по умолчанию. | `r_meshpitch(int)` |
| [`r_meshroll`](../38-cvars-reference/01-video-rendering-cvars.md#r_meshroll) | Определяет направление угла поворота в форматах моделей сетки, также влияет на игровой код, поэтому не меняйте значение по умолчанию. | `r_meshroll(int)` |
| [`r_mirroralpha`](../38-cvars-reference/01-video-rendering-cvars.md#r_mirroralpha) | Указывает, как создается шейдер по умолчанию для текстуры «window02_1». Значения меньше 1 превратят его в зеркало. | `r_mirroralpha(int)` |
| [`r_muzzleflash_colour`](../38-cvars-reference/01-video-rendering-cvars.md#r_muzzleflash_colour) | Это контролирует начальный радиус RGB+ для эффектов EF_MUZZLEFLASH/svc_muzzleflash. | `r_muzzleflash_colour(string)` |
| [`r_muzzleflash_fade`](../38-cvars-reference/01-video-rendering-cvars.md#r_muzzleflash_fade) | Это контролирует посекундное затухание RGB + радиуса эффектов EF_MUZZLEFLASH/svc_muzzleflash. | `r_muzzleflash_fade(string)` |
| [`r_netgraph`](../38-cvars-reference/01-video-rendering-cvars.md#r_netgraph) | Отображает график задержки пакетов. | `r_netgraph(int)` |
| [`r_noaliasshadows`](../38-cvars-reference/01-video-rendering-cvars.md#r_noaliasshadows) | Отключает тени alias models в OpenGL-пути. | `r_noaliasshadows(int)` |
| [`r_nolerp_list`](../38-cvars-reference/01-video-rendering-cvars.md#r_nolerp_list) | Модели в этом списке не будут интерполироваться. | `r_nolerp_list(string)` |
| [`r_nolightdir`](../38-cvars-reference/01-video-rendering-cvars.md#r_nolightdir) | Отключает directional lighting на models и заставляет брать только усреднённый light_avg. | `r_nolightdir(int)` |
| [`r_norefresh`](../38-cvars-reference/01-video-rendering-cvars.md#r_norefresh) | Останавливает обычное обновление кадра: GL, Vulkan и D3D backends рано выходят из draw или present path. | `r_norefresh(int)` |
| [`r_noshadow_list`](../38-cvars-reference/01-video-rendering-cvars.md#r_noshadow_list) | Модели в этом списке не отбрасывают тени. | `r_noshadow_list(string)` |
| [`r_novis`](../38-cvars-reference/01-video-rendering-cvars.md#r_novis) | Отладочно отключает или ослабляет VIS и PVS culling. | `r_novis(int)` |
| [`r_part_beams`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_beams) | Разрешает отображение beam-частиц в scripted particle system. | `r_part_beams(int)` |
| [`r_part_classic_expgrav`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_classic_expgrav) | Масштабирование скорости классических частиц взрыва, которые должны ускоряться под действием силы тяжести. 1 для ванили, 10 для zquake. | `r_part_classic_expgrav(int)` |
| [`r_part_classic_opaque`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_classic_opaque) | Отключает прозрачность классических частиц для придания им олдскульного вида. | `r_part_classic_opaque(int)` |
| [`r_part_classic_square`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_classic_square) | Включает квадратные частицы для придания олдскульного вида. | `r_part_classic_square(int)` |
| [`r_part_contentswitch`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_contentswitch) | Включите изменение эффектов частиц в зависимости от содержимого (например, воды). | `r_part_contentswitch(int)` |
| [`r_part_density`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_density) | Глобальный множитель плотности частиц. | `r_part_density(int)` |
| [`r_part_maxdecals`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_maxdecals) | Задаёт размер пула decals для scripted particle system при её инициализации. | `r_part_maxdecals(int)` |
| [`r_part_maxparticles`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_maxparticles) | Задаёт размер основного пула scripted particles при инициализации системы. | `r_part_maxparticles(int)` |
| [`r_part_rain`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_rain) | Включите эффекты частиц, исходящие от поверхностей. | `r_part_rain(int)` |
| [`r_part_sparks`](../38-cvars-reference/01-video-rendering-cvars.md#r_part_sparks) | Управляет обычными spark-частицами в scripted системе. | `r_part_sparks(int)` |
| [`r_particle_tracelimit`](../38-cvars-reference/01-video-rendering-cvars.md#r_particle_tracelimit) | Количество трасс, допускаемых на кадр для физики элементарных частиц. | `r_particle_tracelimit(string)` |
| [`r_particlesystem`](../38-cvars-reference/01-video-rendering-cvars.md#r_particlesystem) | Выбирает активный backend системы частиц и при изменении полностью очищает текущее particle state, затем инициализирует выбранный движок заново. | `r_particlesystem(string)` |
| [`r_polygonoffset_shadowmap_factor`](../38-cvars-reference/01-video-rendering-cvars.md#r_polygonoffset_shadowmap_factor) | Slope-scaled depth bias для shadowmap passes. | `r_polygonoffset_shadowmap_factor(float)` |
| [`r_polygonoffset_shadowmap_offset`](../38-cvars-reference/01-video-rendering-cvars.md#r_polygonoffset_shadowmap_offset) | Постоянная составляющая depth bias для shadowmap passes. | `r_polygonoffset_shadowmap_offset(int)` |
| [`r_polygonoffset_stencil_factor`](../38-cvars-reference/01-video-rendering-cvars.md#r_polygonoffset_stencil_factor) | Slope-scaled polygon offset для stencil и shadow-related passes. | `r_polygonoffset_stencil_factor(float)` |
| [`r_polygonoffset_stencil_offset`](../38-cvars-reference/01-video-rendering-cvars.md#r_polygonoffset_stencil_offset) | Постоянный polygon offset для stencil и shadow-related passes. | `r_polygonoffset_stencil_offset(int)` |
| [`r_polygonoffset_submodel_factor`](../38-cvars-reference/01-video-rendering-cvars.md#r_polygonoffset_submodel_factor) | Избегание z-боев. Слегка смещает значения глубины подмодели к камере в зависимости от наклона поверхности. | `r_polygonoffset_submodel_factor(int)` |
| [`r_polygonoffset_submodel_map`](../38-cvars-reference/01-video-rendering-cvars.md#r_polygonoffset_submodel_map) | Список карт, на которых следует использовать уменьшение z-fighting. | `r_polygonoffset_submodel_map(string)` |
| [`r_polygonoffset_submodel_offset`](../38-cvars-reference/01-video-rendering-cvars.md#r_polygonoffset_submodel_offset) | Избегание z-боев. Слегка подталкивает значения глубины подмодели к камере на постоянное расстояние. | `r_polygonoffset_submodel_offset(int)` |
| [`r_portaldrawplanes`](../38-cvars-reference/01-video-rendering-cvars.md#r_portaldrawplanes) | Нарисуйте переднюю и заднюю плоскости порталов. | `r_portaldrawplanes(int)` |
| [`r_portalonly`](../38-cvars-reference/01-video-rendering-cvars.md#r_portalonly) | Не рисуйте вещи, не являющиеся порталами. | `r_portalonly(int)` |
| [`r_portalrecursion`](../38-cvars-reference/01-video-rendering-cvars.md#r_portalrecursion) | Количество порталов, через которые камера может пройти. | `r_portalrecursion(int)` |
| [`r_powerupglow`](../38-cvars-reference/01-video-rendering-cvars.md#r_powerupglow) | Управляет динамическим свечением объектов и игроков с эффектами EF_BLUE, EF_RED, EF_GREEN, EF_BRIGHTLIGHT и EF_DIMLIGHT. | `r_powerupglow(int)` |
| [`r_redlight_colour`](../38-cvars-reference/01-video-rendering-cvars.md#r_redlight_colour) | Значения красного, зеленого, синего и радиуса для эффектов EF_RED (обычно используются для пентаграммы в Quakeworld). | `r_redlight_colour(string)` |
| [`r_refract_fbo`](../38-cvars-reference/01-video-rendering-cvars.md#r_refract_fbo) | Используйте fbo для преломления. | `r_refract_fbo(int)` |
| [`r_refractreflect_scale`](../38-cvars-reference/01-video-rendering-cvars.md#r_refractreflect_scale) | Используйте другой масштаб для текстурных карт преломления и отражения. | `r_refractreflect_scale(float)` |
| [`r_replacemodels`](../38-cvars-reference/01-video-rendering-cvars.md#r_replacemodels) | Список расширений файлов, которые движок пытается использовать вместо `.mdl`, если рядом лежат альтернативные версии той же модели. | `r_replacemodels(string)` |
| [`r_rocketlight`](../38-cvars-reference/01-video-rendering-cvars.md#r_rocketlight) | Задаёт силу динамического света от ракет и других сущностей с флагом MF_ROCKET. | `r_rocketlight(int)` |
| [`r_rocketlight_colour`](../38-cvars-reference/01-video-rendering-cvars.md#r_rocketlight_colour) | Это управляет значениями RGB+радиуса эффектов MF_ROCKET. | `r_rocketlight_colour(string)` |
| [`r_rockettrail`](../38-cvars-reference/01-video-rendering-cvars.md#r_rockettrail) | Определяет эффект следа для моделей с флагом MF_ROCKET и при изменении сразу переинициализирует уже загруженные rocket-модели. | `r_rockettrail(int)` |
| [`r_showbboxes`](../38-cvars-reference/01-video-rendering-cvars.md#r_showbboxes) | Отладка. Показывает ограничивающие рамки. 1=ссскк, 2=ксскк. Красный = сплошной, Зеленый = шагать/подбрасывать/подпрыгивать, Синий = лежать на земле. | `r_showbboxes(int)` |
| [`r_showfields`](../38-cvars-reference/01-video-rendering-cvars.md#r_showfields) | Отладка. Показывает поля полей объекта (объект, ближайший к перекрестию). 1=ssqc, 2=csqc, 3=снимки. | `r_showfields(int)` |
| [`r_slimealpha`](../38-cvars-reference/01-video-rendering-cvars.md#r_slimealpha) | Прозрачность slime surfaces при отсутствии явного ALPHA в материале и при разрешённом watervis. | `r_slimealpha(string)` |
| [`r_slimestyle`](../38-cvars-reference/01-video-rendering-cvars.md#r_slimestyle) | См. r_waterstyle, но влияет только на слизь. | `r_slimestyle(string)` |
| [`r_softwarebanding`](../38-cvars-reference/01-video-rendering-cvars.md#r_softwarebanding) | Это стилистический переключатель для современного рендера: он использует quake-палитру, чтобы имитировать полосы и другие артефакты 8-битной картинки. | `r_softwarebanding(int)` |
| [`r_speeds`](../38-cvars-reference/01-video-rendering-cvars.md#r_speeds) | Включает экранную статистику производительности рендера. 0 отключает её, 1 показывает счётчики draw и batch, а > 1 добавляет детальные тайминги по стадиям; на высоких режимах измерение может быть тяжелее. | `r_speeds(int)` |
| [`r_sprite_backfacing`](../38-cvars-reference/01-video-rendering-cvars.md#r_sprite_backfacing) | Сделайте ориентированные спрайты лицом назад относительно их ориентации, для совместимости с q1. | `r_sprite_backfacing(int)` |
| [`r_stainfadeammount`](../38-cvars-reference/01-video-rendering-cvars.md#r_stainfadeammount) | Количество интенсивности, которое stain теряет за один шаг затухания. 0 фактически останавливает fade, большие значения заставляют следы исчезать быстрее. | `r_stainfadeammount(int)` |
| [`r_stainfadetime`](../38-cvars-reference/01-video-rendering-cvars.md#r_stainfadetime) | Интервал между шагами затухания stains в секундах игрового времени. | `r_stainfadetime(int)` |
| [`r_stereo_convergence`](../38-cvars-reference/01-video-rendering-cvars.md#r_stereo_convergence) | Смещает угол каждого глаза внутрь при использовании стереоскопического рендеринга. | `r_stereo_convergence(int)` |
| [`r_stereo_method`](../38-cvars-reference/01-video-rendering-cvars.md#r_stereo_method) | Значение 0 = Выкл. Значение 1 = Попытка аппаратного ускорения. | `r_stereo_method(int)` |
| [`r_stereo_separation`](../38-cvars-reference/01-video-rendering-cvars.md#r_stereo_separation) | Насколько далеко друг от друга находятся ваши глаза (в сейсмических единицах). | `r_stereo_separation(int)` |
| [`r_subdivisions`](../38-cvars-reference/01-video-rendering-cvars.md#r_subdivisions) | Управляет степенью тесселяции кривых patch-поверхностей в Q3 BSP. | `r_subdivisions(int)` |
| [`r_telealpha`](../38-cvars-reference/01-video-rendering-cvars.md#r_telealpha) | Прозрачность teleporter surfaces при отсутствии собственного ALPHA у материала и при разрешённом watervis. | `r_telealpha(string)` |
| [`r_telestyle`](../38-cvars-reference/01-video-rendering-cvars.md#r_telestyle) | См. r_waterstyle, но влияет только на телепорты. | `r_telestyle(int)` |
| [`r_temporalscenecache`](../38-cvars-reference/01-video-rendering-cvars.md#r_temporalscenecache) | Определяет, следует ли генерировать и повторно использовать кэш сцены для нескольких кадров. | `r_temporalscenecache(string)` |
| [`r_tessellation`](../38-cvars-reference/01-video-rendering-cvars.md#r_tessellation) | Включает и контролирует использование blinn тесселяции в резервном шейдере для сеток, эквивалентном шейдеру с «program defaultskin#TESS». Это будет выглядеть глупо, если только сетки не были специально разработаны для этого и не имеют подходящих нормалей вершин. | `r_tessellation(int)` |
| [`r_tessellation_level`](../38-cvars-reference/01-video-rendering-cvars.md#r_tessellation_level) | Задаёт уровень подразделения для активных путей tessellation. | `r_tessellation_level(int)` |
| [`r_torch`](../38-cvars-reference/01-video-rendering-cvars.md#r_torch) | Создайте динамический свет на месте игрока. | `r_torch(int)` |
| [`r_tracker_fadetime`](../38-cvars-reference/01-video-rendering-cvars.md#r_tracker_fadetime) | Сколько времени требуется, чтобы сообщения r_tracker полностью исчезли после того, как они начали исчезать. | `r_tracker_fadetime(int)` |
| [`r_tracker_frags`](../38-cvars-reference/01-video-rendering-cvars.md#r_tracker_frags) | 0: как в ванильном землетрясении 1: показывает только ваши убийства/смерти 2: показывает все убийства. | `r_tracker_frags(int)` |
| [`r_tracker_lines`](../38-cvars-reference/01-video-rendering-cvars.md#r_tracker_lines) | Количество отображаемых сообщений r_tracker. | `r_tracker_lines(int)` |
| [`r_tracker_time`](../38-cvars-reference/01-video-rendering-cvars.md#r_tracker_time) | Сколько времени нужно, чтобы сообщения r_tracker начали исчезать. | `r_tracker_time(int)` |
| [`r_tracker_w`](../38-cvars-reference/01-video-rendering-cvars.md#r_tracker_w) | Ширина сообщений r_tracker в процентах от ширины экрана, например 0,5. | `r_tracker_w(float)` |
| [`r_tracker_x`](../38-cvars-reference/01-video-rendering-cvars.md#r_tracker_x) | Левое положение сообщений r_tracker как часть ширины экрана, например 0,5. | `r_tracker_x(float)` |
| [`r_tracker_y`](../38-cvars-reference/01-video-rendering-cvars.md#r_tracker_y) | Верхняя позиция сообщений r_tracker в процентах от высоты экрана, например 0,333. | `r_tracker_y(float)` |
| [`r_vertexdlights`](../38-cvars-reference/01-video-rendering-cvars.md#r_vertexdlights) | Определите освещение модели относительно близлежащих источников света. | `r_vertexdlights(int)` |
| [`r_viewpreselgun`](../38-cvars-reference/01-video-rendering-cvars.md#r_viewpreselgun) | ХАК: отображать заранее выбранную модель оружия вместо текущей модели оружия. | `r_viewpreselgun(int)` |
| [`r_wallcolour`](../38-cvars-reference/01-video-rendering-cvars.md#r_wallcolour) | Задаёт RGB-цвет стен для встроенного шейдера drawflat_wall в режиме упрощённого отображения мира. | `r_wallcolour(string)` |
| [`r_walltexture`](../38-cvars-reference/01-video-rendering-cvars.md#r_walltexture) | В текущем коде объявление и регистрация этой переменной закомментированы, причём рядом она помечена как broken. | `r_walltexture(string)` |
| [`r_wireframe_smooth`](../38-cvars-reference/01-video-rendering-cvars.md#r_wireframe_smooth) | Включает сглаживание линий через GL_LINE_SMOOTH в OpenGL-рендерере. | `r_wireframe_smooth(int)` |
| [`ruleset_allow_larger_models`](../38-cvars-reference/01-video-rendering-cvars.md#ruleset_allow_larger_models) | Устанавливает максимальный предел границ для моделей, чтобы предотвратить использование дополнительных шипов, прикрепленных к модели, в качестве своего рода Wallhack. | `ruleset_allow_larger_models(int)` |
| [`v_bonusflash`](../38-cvars-reference/01-video-rendering-cvars.md#v_bonusflash) | Управляет силой временных вспышек экрана при подборе предметов (gl_polyblend должен быть включен). | `v_bonusflash(int)` |
| [`v_centermove`](../38-cvars-reference/01-video-rendering-cvars.md#v_centermove) | Задаёт время непрерывного движения, после которого разрешается автоматический возврат pitch к idealpitch или центру. | `v_centermove(float)` |
| [`v_centerspeed`](../38-cvars-reference/01-video-rendering-cvars.md#v_centerspeed) | Задаёт начальную и добавочную скорость автоцентрирования pitch. | `v_centerspeed(int)` |
| [`v_contentblend`](../38-cvars-reference/01-video-rendering-cvars.md#v_contentblend) | Управляет интенсивностью подводных цветовых оттенков (gl_polyblend должен быть включен). | `v_contentblend(int)` |
| [`v_cshift_empty`](../38-cvars-reference/01-video-rendering-cvars.md#v_cshift_empty) | Цветовой оттенок, используемый на открытом воздухе (дополнительно масштабируется `v_contentblend`). | `v_cshift_empty(string)` |
| [`v_cshift_lava`](../38-cvars-reference/01-video-rendering-cvars.md#v_cshift_lava) | Цветовой оттенок, используемый при захоронении в лаве (ууу!) (дополнительно масштабируется `v_contentblend`). | `v_cshift_lava(string)` |
| [`v_cshift_slime`](../38-cvars-reference/01-video-rendering-cvars.md#v_cshift_slime) | Цветовой оттенок, используемый при погружении в слизь (дополнительно масштабируется `v_contentblend`). | `v_cshift_slime(string)` |
| [`v_cshift_water`](../38-cvars-reference/01-video-rendering-cvars.md#v_cshift_water) | Цветовой оттенок, используемый под водой (дополнительно масштабируется `v_contentblend`). | `v_cshift_water(string)` |
| [`v_damagecshift`](../38-cvars-reference/01-video-rendering-cvars.md#v_damagecshift) | Управляет интенсивностью цветовых оттенков получаемых повреждений (gl_polyblend должен быть включен). | `v_damagecshift(int)` |
| [`v_deathtilt`](../38-cvars-reference/01-video-rendering-cvars.md#v_deathtilt) | Указывает, следует ли наклонять вид, когда он мертв. | `v_deathtilt(int)` |
| [`v_depthsortentities`](../38-cvars-reference/01-video-rendering-cvars.md#v_depthsortentities) | Измените порядок объектов для обеспечения прозрачности так, чтобы самые дальние объекты рисовались первыми, позволяя более близким прозрачным объектам рисоваться поверх них. | `v_depthsortentities(int)` |
| [`v_gunkick`](../38-cvars-reference/01-video-rendering-cvars.md#v_gunkick) | Управляет силой изменения угла обзора при стрельбе из оружия. | `v_gunkick(int)` |
| [`v_gunkick_q2`](../38-cvars-reference/01-video-rendering-cvars.md#v_gunkick_q2) | Управляет силой изменения угла обзора при стрельбе из оружия (в Quake2). | `v_gunkick_q2(int)` |
| [`v_idlescale`](../38-cvars-reference/01-video-rendering-cvars.md#v_idlescale) | Включите перелистывание изображения (в режиме ожидания или в противном случае). | `v_idlescale(int)` |
| [`v_ipitch_cycle`](../38-cvars-reference/01-video-rendering-cvars.md#v_ipitch_cycle) | Определяет частоту синусоидального idle-качания камеры по pitch. | `v_ipitch_cycle(int)` |
| [`v_ipitch_level`](../38-cvars-reference/01-video-rendering-cvars.md#v_ipitch_level) | Определяет амплитуду idle-качания камеры по pitch. | `v_ipitch_level(float)` |
| [`v_iroll_cycle`](../38-cvars-reference/01-video-rendering-cvars.md#v_iroll_cycle) | Определяет частоту idle-качания камеры по roll. | `v_iroll_cycle(float)` |
| [`v_iroll_level`](../38-cvars-reference/01-video-rendering-cvars.md#v_iroll_level) | Определяет амплитуду idle-качания камеры по roll. | `v_iroll_level(float)` |
| [`v_iyaw_cycle`](../38-cvars-reference/01-video-rendering-cvars.md#v_iyaw_cycle) | Определяет частоту idle-качания камеры по yaw. | `v_iyaw_cycle(int)` |
| [`v_iyaw_level`](../38-cvars-reference/01-video-rendering-cvars.md#v_iyaw_level) | Определяет амплитуду idle-качания камеры по yaw. | `v_iyaw_level(float)` |
| [`v_kickpitch`](../38-cvars-reference/01-video-rendering-cvars.md#v_kickpitch) | Это контролирует часть силы изменения угла обзора от получения урона. | `v_kickpitch(float)` |
| [`v_kickroll`](../38-cvars-reference/01-video-rendering-cvars.md#v_kickroll) | Это контролирует часть силы изменения угла обзора от получения урона. | `v_kickroll(float)` |
| [`v_kicktime`](../38-cvars-reference/01-video-rendering-cvars.md#v_kicktime) | Определяет, как долго будут сохраняться изменения угла обзора из-за получения урона. | `v_kicktime(float)` |
| [`v_pentcshift`](../38-cvars-reference/01-video-rendering-cvars.md#v_pentcshift) | Управляет интенсивностью цветовых оттенков пентаграммы защиты (gl_polyblend должен быть включен). | `v_pentcshift(int)` |
| [`v_powerupshell`](../38-cvars-reference/01-video-rendering-cvars.md#v_powerupshell) | Включает цветную аддитивную оболочку вокруг сущностей с эффектами EF_BLUE, EF_RED или EF_GREEN. | `v_powerupshell(int)` |
| [`v_projectionmode`](../38-cvars-reference/01-video-rendering-cvars.md#v_projectionmode) | Неактивный legacy-cvar: рабочего переключателя проекции не задействует, оставлен для обратной совместимости. | `v_projectionmode(int)` |
| [`v_quadcshift`](../38-cvars-reference/01-video-rendering-cvars.md#v_quadcshift) | Управляет интенсивностью цветовых оттенков четырехкратного урона (должен быть включен gl_polyblend). | `v_quadcshift(int)` |
| [`v_ringcshift`](../38-cvars-reference/01-video-rendering-cvars.md#v_ringcshift) | Управляет интенсивностью цветовых оттенков кольца невидимости (gl_polyblend должен быть включен). | `v_ringcshift(int)` |
| [`v_suitcshift`](../38-cvars-reference/01-video-rendering-cvars.md#v_suitcshift) | Управляет интенсивностью цветовых оттенков костюма (должен быть включен gl_polyblend). | `v_suitcshift(int)` |
| [`v_viewheight`](../38-cvars-reference/01-video-rendering-cvars.md#v_viewheight) | Добавляет ручную поправку к высоте камеры живого игрока вместо обычного bob, если задано ненулевое значение. | `v_viewheight(int)` |
| [`vid_baseheight`](../38-cvars-reference/01-video-rendering-cvars.md#vid_baseheight) | Определяет целевую высоту мода и используется только в том случае, если 2D-масштаб не применяется иным образом. | `vid_baseheight(string)` |
| [`vid_devicename`](../38-cvars-reference/01-video-rendering-cvars.md#vid_devicename) | Указывает, какое видеоустройство попытаться использовать. | `vid_devicename(string)` |
| [`vid_dpi_x`](../38-cvars-reference/01-video-rendering-cvars.md#vid_dpi_x) | Для модов, которым необходимо определять физический размер экрана (например, с тачскринами). 0 означает неизвестное. | `vid_dpi_x(int)` |
| [`vid_dpi_y`](../38-cvars-reference/01-video-rendering-cvars.md#vid_dpi_y) | Для модов, которым необходимо определять физический размер экрана (например, с тачскринами). 0 означает неизвестное. | `vid_dpi_y(int)` |
| [`vid_minsize`](../38-cvars-reference/01-video-rendering-cvars.md#vid_minsize) | Определяет минимальный виртуальный размер мода (устанавливается внутри файла default.cfg мода). | `vid_minsize(string)` |
| [`vid_triplebuffer`](../38-cvars-reference/01-video-rendering-cvars.md#vid_triplebuffer) | Указывает, принудительно ли аппаратное обеспечение использует тройную буферизацию. | `vid_triplebuffer(int)` |
| [`vid_winthread`](../38-cvars-reference/01-video-rendering-cvars.md#vid_winthread) | Если этот параметр включен, оконные сообщения будут обрабатываться отдельным потоком. | `vid_winthread(string)` |
| [`vid_wndalpha`](../38-cvars-reference/01-video-rendering-cvars.md#vid_wndalpha) | При работе в оконном режиме указывает уровень прозрачности окна. | `vid_wndalpha(int)` |
| [`viewsize`](../38-cvars-reference/01-video-rendering-cvars.md#viewsize) | Определяет размер 3D-вида и связанную с ним компоновку status bar. | `viewsize(int)` |
| [`vk_khr_dedicated_allocation`](../38-cvars-reference/01-video-rendering-cvars.md#vk_khr_dedicated_allocation) | Если применимо, пометьте выделение памяти Vulkan как выделенное. | `vk_khr_dedicated_allocation(string)` |
| [`vk_khr_get_memory_requirements2`](../38-cvars-reference/01-video-rendering-cvars.md#vk_khr_get_memory_requirements2) | Включите запросы информации о расширенной памяти. | `vk_khr_get_memory_requirements2(string)` |
| [`vk_khr_push_descriptor`](../38-cvars-reference/01-video-rendering-cvars.md#vk_khr_push_descriptor) | Обеспечивает лучшую потоковую передачу дескрипторов. | `vk_khr_push_descriptor(string)` |
| [`vk_khr_ray_query`](../38-cvars-reference/01-video-rendering-cvars.md#vk_khr_ray_query) | Требуется для использования аппаратной трассировки лучей. | `vk_khr_ray_query(string)` |
| [`worker_count`](../38-cvars-reference/01-video-rendering-cvars.md#worker_count) | Указывает количество используемых рабочих потоков. | `worker_count(string)` |
| [`worker_flush`](../38-cvars-reference/01-video-rendering-cvars.md#worker_flush) | Если установлено, обрабатывается вся очередь загрузки, загружаясь быстрее, но с риском остановки основного потока. | `worker_flush(int)` |
| [`worker_sleeptime`](../38-cvars-reference/01-video-rendering-cvars.md#worker_sleeptime) | Заставляет рабочих спать на некоторое время после каждой работы. | `worker_sleeptime(int)` |

### Освещение, тени и материалы

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`gl_skyboxdist`](../38-cvars-reference/02-lighting-materials-cvars.md#gl_skyboxdist) | Задаёт дистанцию, на которой рисуется skybox-геометрия. | `gl_skyboxdist(float)` |
| [`mod_map_lights`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_map_lights) | Включает расчёт освещения для brush/patch-геометрии, связанной с terrain-подсистемой. | `mod_map_lights(int)` |
| [`mod_map_texscale`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_map_texscale) | Определяет масштаб texel-плотности для brush/patch-текстур, когда terrain-подсистема рассчитывает их освещение. | `mod_map_texscale(float)` |
| [`mod_terrain_ambient`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_terrain_ambient) | Задаёт долю фонового освещения terrain-поверхностей. | `mod_terrain_ambient(float)` |
| [`mod_terrain_shadow_dist`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_terrain_shadow_dist) | Определяет, насколько далеко terrain-система бросает лучи при поиске геометрии, которая должна давать тень. | `mod_terrain_shadow_dist(float)` |
| [`mod_terrain_shadows`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_terrain_shadows) | Включает лучевой поиск самозатенения terrain при вычислении его освещения. | `mod_terrain_shadows(bool)` |
| [`mod_terrain_sundir`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_terrain_sundir) | Определяет направление солнца для terrain-подсистемы; в коде вектор нормализуется, поэтому абсолютная длина не важна, а важны только пропорции компонент. | `mod_terrain_sundir(vector3)` |
| [`r_bloom`](../38-cvars-reference/02-lighting-materials-cvars.md#r_bloom) | Включает bloom — свечение ярких областей с «подтеканием» света по соседним пикселям. | `r_bloom(float)` |
| [`r_bloom_downsize`](../38-cvars-reference/02-lighting-materials-cvars.md#r_bloom_downsize) | Переключает более «правильный» режим downsizing при подготовке bloom-буфера. | `r_bloom_downsize(bool)` |
| [`r_bloom_filter`](../38-cvars-reference/02-lighting-materials-cvars.md#r_bloom_filter) | Задаёт RGB-порог яркости, начиная с которого пиксели попадают в bloom. | `r_bloom_filter(vector3)` |
| [`r_bloom_initialscale`](../38-cvars-reference/02-lighting-materials-cvars.md#r_bloom_initialscale) | Определяет начальный масштаб буфера bloom перед дальнейшей фильтрацией. | `r_bloom_initialscale(float)` |
| [`r_bloom_retain`](../38-cvars-reference/02-lighting-materials-cvars.md#r_bloom_retain) | Указывает, сколько исходной сцены оставить видимой, когда поверх накладывается bloom. | `r_bloom_retain(float)` |
| [`r_bloom_size`](../38-cvars-reference/02-lighting-materials-cvars.md#r_bloom_size) | Задаёт целевой размер bloom-ядра из расчёта базовой ширины видео `320` пикселей. | `r_bloom_size(float)` |
| [`r_fastsky`](../38-cvars-reference/02-lighting-materials-cvars.md#r_fastsky) | Включает упрощённый режим прорисовки неба вместо полного skybox/[skyroom](../38-cvars-reference/07-system-misc-cvars.md#skyroom)-пайплайна. | `r_fastsky(bool)` |
| [`r_fastskycolour`](../38-cvars-reference/02-lighting-materials-cvars.md#r_fastskycolour) | Определяет цвет, которым заполняется упрощённое небо при включённом `r_fastsky`. | `r_fastskycolour(vector3)` |
| [`r_forceprogramify`](../38-cvars-reference/02-lighting-materials-cvars.md#r_forceprogramify) | Принудительно упрощает и «программифицирует» загруженные шейдеры, сводя их к более простому освещаемому виду. | `r_forceprogramify(int)` |
| [`r_glsl_precache`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_precache) | Заставляет движок заранее компилировать все релевантные GLSL-перестановки вместо ленивой подгрузки по мере надобности. | `r_glsl_precache(bool)` |
| [`r_glsl_skybox_autorotate`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_skybox_autorotate) | Переключает автоматическое вращение skybox, связанное с параметрами `r_glsl_skybox_orientation`. | `r_glsl_skybox_autorotate(bool)` |
| [`r_glsl_skybox_orientation`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_skybox_orientation) | Задаёт ось и скорость вращения skybox: первые три числа — вектор оси, четвёртое — скорость в градусах в секунду. | `r_glsl_skybox_orientation(vector4)` |
| [`r_halfrate`](../38-cvars-reference/02-lighting-materials-cvars.md#r_halfrate) | Включает half-rate shading там, где видеокарта и backend это поддерживают. | `r_halfrate(bool)` |
| [`r_shadow_playershadows`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_playershadows) | Управляет тем, показывать ли тени на локальном игроке. | `r_shadow_playershadows(bool)` |
| [`r_shadow_raytrace`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_raytrace) | Включает аппаратную трассировку лучей для теней, если backend и железо это поддерживают. | `r_shadow_raytrace(bool)` |
| [`r_shadow_realtime_dlight`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_dlight) | Включает realtime-динамический свет для кратковременных источников вроде взрывов, вспышек и временных огней. | `r_shadow_realtime_dlight(bool)` |
| [`r_shadow_realtime_dlight_ambient`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_dlight_ambient) | Масштабирует ambient-компонент для realtime dynamic lights. | `r_shadow_realtime_dlight_ambient(float)` |
| [`r_shadow_realtime_dlight_diffuse`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_dlight_diffuse) | Задаёт множитель diffuse-вклада для realtime dynamic lights. | `r_shadow_realtime_dlight_diffuse(float)` |
| [`r_shadow_realtime_dlight_shadows`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_dlight_shadows) | Разрешает динамическим realtime-огням отбрасывать тени во время движения. | `r_shadow_realtime_dlight_shadows(bool)` |
| [`r_shadow_realtime_dlight_specular`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_dlight_specular) | Определяет силу specular-блика от realtime dynamic lights. | `r_shadow_realtime_dlight_specular(float)` |
| [`r_shadow_realtime_world`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_world) | Включает статические/world realtime lights — основу более современного освещения уровня поверх или вместо baked lightmap. | `r_shadow_realtime_world(bool)` |
| [`r_shadow_realtime_world_importlightentitiesfrommap`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_world_importlightentitiesfrommap) | Определяет, как именно загружать map-based realtime lights. | `r_shadow_realtime_world_importlightentitiesfrommap(int)` |
| [`r_shadow_realtime_world_lightmaps`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_world_lightmaps) | Задаёт, сколько обычного baked lightmap-света сохранить при включённом world realtime lighting. | `r_shadow_realtime_world_lightmaps(float)` |
| [`r_shadow_realtime_world_shadows`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_world_shadows) | Включает отбрасывание теней для статических/world realtime lights. | `r_shadow_realtime_world_shadows(bool)` |
| [`r_shadow_scissor`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_scissor) | Ограничивает stencil/shadow-обработку экранным прямоугольником, который покрывает максимальные границы света. | `r_shadow_scissor(bool)` |
| [`r_shadow_shadowmapping`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_shadowmapping) | Переключает soft shadowmapping вместо stencil-теней. | `r_shadow_shadowmapping(bool)` |
| [`r_shadow_shadowmapping_bias`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_shadowmapping_bias) | Задаёт bias для shadowmap-сэмплинга и нужен для борьбы с self-shadowing-артефактами вроде shadow acne. | `r_shadow_shadowmapping_bias(float)` |
| [`r_shadow_shadowmapping_depthbits`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_shadowmapping_depthbits) | Выбирает глубину shadowmap-буфера: в описании из кода перечислены `16`, `24` и `32` бита. | `r_shadow_shadowmapping_depthbits(int)` |
| [`r_shadow_shadowmapping_nearclip`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_shadowmapping_nearclip) | Управляет near clip plane для проекций, используемых при построении shadowmap. | `r_shadow_shadowmapping_nearclip(float)` |
| [`r_shadow_shadowmapping_precision`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_shadowmapping_precision) | Масштабирует уровень детализации shadowmap вверх или вниз. | `r_shadow_shadowmapping_precision(float)` |
| [`r_shadows_fakedistance`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadows_fakedistance) | Определяет радиус для fake shadows — упрощённого режима, где тень не строится из полноценной геометрии реального света. | `r_shadows_fakedistance(float)` |
| [`r_shadows_focus`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadows_focus) | Смещает центр объёма fake-shadows по трём осям. | `r_shadows_focus(vector3)` |
| [`r_shadows_throwdirection`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadows_throwdirection) | Задаёт направление, в котором «бросаются» fake shadows. | `r_shadows_throwdirection(vector3)` |
| [`r_skybox`](../38-cvars-reference/02-lighting-materials-cvars.md#r_skybox) | Принудительно задаёт пользовательский skybox поверх того, что запрашивает карта. | `r_skybox(string)` |
| [`r_skycloudalpha`](../38-cvars-reference/02-lighting-materials-cvars.md#r_skycloudalpha) | Управляет непрозрачностью переднего слоя у старых scrolling sky. | `r_skycloudalpha(float)` |
| [`r_skyfog`](../38-cvars-reference/02-lighting-materials-cvars.md#r_skyfog) | Добавляет fog-альфу к skybox и складывается с обычной fog-прозрачностью сцены. | `r_skyfog(float)` |
| [`r_sun_colour`](../38-cvars-reference/02-lighting-materials-cvars.md#r_sun_colour) | Определяет цвет солнечного света, который проявляется как crepuscular rays — световые лучи в небе. | `r_sun_colour(vector3)` |
| [`r_sun_dir`](../38-cvars-reference/02-lighting-materials-cvars.md#r_sun_dir) | Задаёт направление, вдоль которого появляются crepuscular rays и которое также используется как ориентир для некоторых fake-shadow сценариев. | `r_sun_dir(vector3)` |
| [`r_vertexlight`](../38-cvars-reference/02-lighting-materials-cvars.md#r_vertexlight) | Включает режим vertex lighting, при котором загруженные шейдеры упрощаются: из них убираются detail-проходы и sampling lightmap ради более быстрого рендера. | `r_vertexlight(bool)` |
| [`gl_flashblend`](../38-cvars-reference/02-lighting-materials-cvars.md#gl_flashblend) | Включает упрощённый режим отрисовки динамических источников света в виде плоских полупрозрачных пятен-«корон» (coronas) вместо честного расчёта освещения геометрии сцены — это унаследованный режим из старого GLQuake, придуманный для экономии производительности на слабом железе. | `gl_flashblend(int)` |
| [`gl_flashblendscale`](../38-cvars-reference/02-lighting-materials-cvars.md#gl_flashblendscale) | Управляет размером corona/flashblend-оболочки у создаваемых динамических источников света. | `gl_flashblendscale(float)` |
| [`gl_menutint_shader`](../38-cvars-reference/02-lighting-materials-cvars.md#gl_menutint_shader) | Управляет использованием GLSL для обесцвечивания фона при рисовании меню, как это делал программный рендерер DOS в Quake до уродливого размытия Winquake. | `gl_menutint_shader(int)` |
| [`gl_workaround_ati_shadersource`](../38-cvars-reference/02-lighting-materials-cvars.md#gl_workaround_ati_shadersource) | Устранение ошибок драйвера ATI в функции glShaderSource. | `gl_workaround_ati_shadersource(int)` |
| [`mod_terrain_defaulttexture`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_terrain_defaulttexture) | Эту текстуру будут использовать вновь созданные тайлы местности. | `mod_terrain_defaulttexture(string)` |
| [`mod_terrain_networked`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_terrain_networked) | Редактирование ландшафта осуществляется по сети. | `mod_terrain_networked(int)` |
| [`mod_terrain_savever`](../38-cvars-reference/02-lighting-materials-cvars.md#mod_terrain_savever) | Какую версию участка местности писать, если местность редактировалась. | `mod_terrain_savever(string)` |
| [`r_ambient`](../38-cvars-reference/02-lighting-materials-cvars.md#r_ambient) | Добавляет равномерную базовую засветку в пересчитываемые lightmap мира. | `r_ambient(int)` |
| [`r_dynamic`](../38-cvars-reference/02-lighting-materials-cvars.md#r_dynamic) | Управляет обычными динамическими огнями старого типа (`dlights`), которые не являются полноценным realtime-lighting. | `r_dynamic(int)` |
| [`r_fullbright`](../38-cvars-reference/02-lighting-materials-cvars.md#r_fullbright) | Игнорируйте карты освещения мира, рисуя *все* полностью освещенным. | `r_fullbright(int)` |
| [`r_fullbrightSkins`](../38-cvars-reference/02-lighting-materials-cvars.md#r_fullbrightskins) | Заставьте других игроков использовать полносветлые скины. | `r_fullbrightSkins(float)` |
| [`r_glsl_emissive`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_emissive) | Если установлено, указывает, что текстуры _luma или _glow являются излучающими... Если установлено значение 0, они принимаются в качестве маски для той части карты освещения, которая будет применяться (для совместимости с q2e возникают проблемы с чрезмерной яркостью). | `r_glsl_emissive(int)` |
| [`r_glsl_offsetmapping`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_offsetmapping) | Позволяет использовать отображение паралакса, добавляя текстурам искусственную глубину. | `r_glsl_offsetmapping(int)` |
| [`r_glsl_offsetmapping_reliefmapping`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_offsetmapping_reliefmapping) | Изменен режим семплирования паралакса, чтобы он стал немного приятнее, но заметно дороже при высоких разрешениях. | `r_glsl_offsetmapping_reliefmapping(int)` |
| [`r_glsl_offsetmapping_scale`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_offsetmapping_scale) | Задаёт силу смещения текстурных координат в offset/parallax mapping. | `r_glsl_offsetmapping_scale(float)` |
| [`r_glsl_pbr`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_pbr) | Принудительное затенение PBR. | `r_glsl_pbr(int)` |
| [`r_glsl_turbscale_reflect`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_turbscale_reflect) | Управляет силой ряби отражения воды (используется кодом altwater glsl). | `r_glsl_turbscale_reflect(int)` |
| [`r_glsl_turbscale_refract`](../38-cvars-reference/02-lighting-materials-cvars.md#r_glsl_turbscale_refract) | Управляет силой подводной ряби (используется кодом altwater glsl). | `r_glsl_turbscale_refract(int)` |
| [`r_lightflicker`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightflicker) | Добавляет псевдослучайное мерцание к радиусу некоторых динамических источников света. | `r_lightflicker(int)` |
| [`r_lightmap`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightmap) | Переключает встроенный world-shader в режим показа lightmap без обычных базовых текстур. | `r_lightmap(int)` |
| [`r_lightmap_format`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightmap_format) | Переопределяет формат текстур по умолчанию, используемый для карт освещения. rgb9e5 — хороший выбор для HDR. | `r_lightmap_format(string)` |
| [`r_lightmap_saturation`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightmap_saturation) | Регулирует насыщенность RGB8 lightmap при загрузке и сборке карт. | `r_lightmap_saturation(int)` |
| [`r_lightprepass`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightprepass) | Экспериментальный. Попытайтесь использовать другой механизм освещения (также известный как отложенное освещение). Для вступления в силу требуется vid_reload. | `r_lightprepass(int)` |
| [`r_lightstylescale`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightstylescale) | Масштабирует итоговую яркость анимированных [lightstyle](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#lightstyle). | `r_lightstylescale(int)` |
| [`r_lightstylesmooth`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightstylesmooth) | Включает интерполяцию между соседними шагами анимированных lightstyle. | `r_lightstylesmooth(int)` |
| [`r_lightstylesmooth_limit`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightstylesmooth_limit) | Определяет, насколько сильно могут отличаться два соседних шага lightstyle, чтобы между ними ещё выполнялась плавная интерполяция. | `r_lightstylesmooth_limit(int)` |
| [`r_lightstylespeed`](../38-cvars-reference/02-lighting-materials-cvars.md#r_lightstylespeed) | Задаёт скорость проигрывания анимированных lightstyle. | `r_lightstylespeed(int)` |
| [`r_particledesc`](../38-cvars-reference/02-lighting-materials-cvars.md#r_particledesc) | Задаёт наборы scripted [particle](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#particle) definitions как строковый список токенов. | `r_particledesc(string)` |
| [`r_postprocshader`](../38-cvars-reference/02-lighting-materials-cvars.md#r_postprocshader) | Указывает пользовательский шейдер для использования в качестве шейдера постобработки. | `r_postprocshader(string)` |
| [`r_shaderblobs`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shaderblobs) | Если включено, может значительно ускорить перезапуск/загрузку видео (особенно с рендерером d3d). | `r_shaderblobs(int)` |
| [`r_shadow_bumpscale_basetexture`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_bumpscale_basetexture) | Масштабирование неровностей для создания резервных текстур карт нормалей из моделей. | `r_shadow_bumpscale_basetexture(int)` |
| [`r_shadow_bumpscale_bumpmap`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_bumpscale_bumpmap) | Масштабирование неровностей для текстур _bump. | `r_shadow_bumpscale_bumpmap(int)` |
| [`r_shadow_heightscale_basetexture`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_heightscale_basetexture) | Масштабатор для создания карт высот из содержимого устаревшей палитры. | `r_shadow_heightscale_basetexture(int)` |
| [`r_shadow_heightscale_bumpmap`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_heightscale_bumpmap) | Скалер высоты для 8-битных текстур _bump. | `r_shadow_heightscale_bumpmap(int)` |
| [`r_shadow_realtime_nonworld_lightmaps`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadow_realtime_nonworld_lightmaps) | Скейлер для карт освещения, используемый, когда не используется освещение мира в реальном времени. | `r_shadow_realtime_nonworld_lightmaps(int)` |
| [`r_shadows`](../38-cvars-reference/02-lighting-materials-cvars.md#r_shadows) | Нарисуйте базовые тени-капли под объектами, не используя освещение в реальном времени. | `r_shadows(int)` |
| [`r_showshaders`](../38-cvars-reference/02-lighting-materials-cvars.md#r_showshaders) | Отладка. Показывает имя шейдера (мировой модели), на который указывает. | `r_showshaders(int)` |
| [`r_stains`](../38-cvars-reference/02-lighting-materials-cvars.md#r_stains) | Управляет следами загрязнения на world lightmap, например кровавыми пятнами от попаданий. | `r_stains(int)` |
| [`ruleset_allow_shaders`](../38-cvars-reference/02-lighting-materials-cvars.md#ruleset_allow_shaders) | Если установлено значение 0, это полностью отключает использование внешних файлов шейдеров, предотвращая использование пользовательских шейдеров для взлома стен. | `ruleset_allow_shaders(int)` |

### Звук

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`capturesound`](../38-cvars-reference/03-audio-cvars.md#capturesound) | Включает запись звуковой дорожки при использовании подсистемы захвата. | `capturesound(int)` |
| [`capturesoundbits`](../38-cvars-reference/03-audio-cvars.md#capturesoundbits) | Задаёт разрядность записываемого звука для подсистемы захвата. | `capturesoundbits(int)` |
| [`capturesoundchannels`](../38-cvars-reference/03-audio-cvars.md#capturesoundchannels) | Определяет число каналов в аудиодорожке захвата. | `capturesoundchannels(int)` |
| [`cl_voip_capturedevice`](../38-cvars-reference/03-audio-cvars.md#cl_voip_capturedevice) | Выбирает устройство захвата для VOIP. | `cl_voip_capturedevice(string)` |
| [`cl_voip_send`](../38-cvars-reference/03-audio-cvars.md#cl_voip_send) | Управляет режимом отправки голосового трафика. | `cl_voip_send(int)` |
| [`cl_voip_test`](../38-cvars-reference/03-audio-cvars.md#cl_voip_test) | Локальный тест микрофона: при `1` движок воспроизводит ваш голос напрямую, минуя сервер и сетевую задержку. | `cl_voip_test(int)` |
| [`cl_voip_vad_delay`](../38-cvars-reference/03-audio-cvars.md#cl_voip_vad_delay) | Определяет, сколько секунд VOIP продолжает передаваться после того, как голосовая активация перестала срабатывать. | `cl_voip_vad_delay(float)` |
| [`cl_voip_vad_threshhold`](../38-cvars-reference/03-audio-cvars.md#cl_voip_vad_threshhold) | Порог voice-activation-detection для исходящего VOIP. | `cl_voip_vad_threshhold(int)` |
| [`mastervolume`](../38-cvars-reference/03-audio-cvars.md#mastervolume) | Общий дополнительный множитель для всех остальных звуков. | `mastervolume(float)` |
| [`media_hijackwinamp`](../38-cvars-reference/03-audio-cvars.md#media_hijackwinamp) | Переключатель интеграции с Winamp в сборках, где эта поддержка включена. | `media_hijackwinamp(int)` |
| [`media_repeat`](../38-cvars-reference/03-audio-cvars.md#media_repeat) | Определяет, должен ли медиаплеер продолжать воспроизведение после достижения конца списка. | `media_repeat(int)` |
| [`media_shuffle`](../38-cvars-reference/03-audio-cvars.md#media_shuffle) | Включает случайный выбор следующего трека в медиаплеере. | `media_shuffle(int)` |
| [`music_fade`](../38-cvars-reference/03-audio-cvars.md#music_fade) | Управляет плавным затуханием музыки при смене трека. | `music_fade(int)` |
| [`music_playlist_index`](../38-cvars-reference/03-audio-cvars.md#music_playlist_index) | Индекс совместимого cvar-плейлиста для музыкальной подсистемы. | `music_playlist_index(int)` |
| [`nosound`](../38-cvars-reference/03-audio-cvars.md#nosound) | Полностью отключает звук в движке. | `nosound(int)` |
| [`s_al_debug`](../38-cvars-reference/03-audio-cvars.md#s_al_debug) | Включает периодические проверки ошибок OpenAL. | `s_al_debug(int)` |
| [`s_al_disable`](../38-cvars-reference/03-audio-cvars.md#s_al_disable) | Управляет использованием OpenAL как выходного бэкенда. | `s_al_disable(int)` |
| [`s_al_hrtf`](../38-cvars-reference/03-audio-cvars.md#s_al_hrtf) | Настраивает HRTF в OpenAL. | `s_al_hrtf(string)` |
| [`s_al_reference_distance`](../38-cvars-reference/03-audio-cvars.md#s_al_reference_distance) | Опорная дистанция для моделей затухания OpenAL, при которой звук считается слышимым с «нормальной» громкостью. | `s_al_reference_distance(float)` |
| [`s_al_use_reverb`](../38-cvars-reference/03-audio-cvars.md#s_al_use_reverb) | Разрешает или блокирует использование реверберации в OpenAL. | `s_al_use_reverb(int)` |
| [`s_al_velocityscale`](../38-cvars-reference/03-audio-cvars.md#s_al_velocityscale) | Масштабирует значения скорости перед расчётом допплер-эффекта в OpenAL. | `s_al_velocityscale(float)` |
| [`snd_ignorecueloops`](../38-cvars-reference/03-audio-cvars.md#snd_ignorecueloops) | Игнорирует cue-команды в WAV-файлах ради совместимости с Quake 3-подобным контентом. | `snd_ignorecueloops(int)` |
| [`snd_ignoregamespeed`](../38-cvars-reference/03-audio-cvars.md#snd_ignoregamespeed) | Позволяет звуку не подстраиваться под игровую скорость и скорость демо. | `snd_ignoregamespeed(int)` |
| [`snd_loadasstereo`](../38-cvars-reference/03-audio-cvars.md#snd_loadasstereo) | Заставляет моно-звуки загружаться как стерео. | `snd_loadasstereo(int)` |
| [`snd_playbackrate`](../38-cvars-reference/03-audio-cvars.md#snd_playbackrate) | Отладочная cheat-переменная, которая меняет скорость воспроизведения всех новых звуков. | `snd_playbackrate(float)` |
| [`tts_mode`](../38-cvars-reference/03-audio-cvars.md#tts_mode) | Режим text-to-speech. Строка описания задаёт все основные варианты: `0` — выключено, `1` — читать только сообщения чата с префиксом `tts `, `2` — читать весь чат, `3` — читать вообще каждый console [print](../37-quakec-builtins-reference/12-system-debug-builtins.md#print). | `tts_mode(int)` |
| [`wasapi_buffersize`](../38-cvars-reference/03-audio-cvars.md#wasapi_buffersize) | Размер буфера WASAPI в секундах для обычного shared-режима. | `wasapi_buffersize(float)` |
| [`wasapi_exclusive`](../38-cvars-reference/03-audio-cvars.md#wasapi_exclusive) | Переводит WASAPI в exclusive mode. | `wasapi_exclusive(int)` |
| [`wasapi_forcechannels`](../38-cvars-reference/03-audio-cvars.md#wasapi_forcechannels) | Пытается заставить WASAPI использовать число каналов движка вместо системного значения по умолчанию. | `wasapi_forcechannels(int)` |
| [`wasapi_forcerate`](../38-cvars-reference/03-audio-cvars.md#wasapi_forcerate) | Пытается принудительно использовать частоту дискретизации движка вместо системного микшерного значения. | `wasapi_forcerate(int)` |
| [`_cl_voip_capturedevice_opts`](../38-cvars-reference/03-audio-cvars.md#_cl_voip_capturedevice_opts) | Возможные устройства захвата звука в парах «значение» «описание» для чтения игрового кода. | `_cl_voip_capturedevice_opts(string)` |
| [`_s_device_opts`](../38-cvars-reference/03-audio-cvars.md#_s_device_opts) | Возможные устройства вывода звука в парах «значение» «описание» для чтения игрового кода. | `_s_device_opts(string)` |
| [`cl_chatsound`](../38-cvars-reference/03-audio-cvars.md#cl_chatsound) | Управляет звуковым уведомлением о сообщениях чата. | `cl_chatsound(int)` |
| [`cl_cursor_bias_x`](../38-cvars-reference/03-audio-cvars.md#cl_cursor_bias_x) | Задаёт горизонтальное смещение hotspot у пользовательского курсора. | `cl_cursor_bias_x(float)` |
| [`cl_cursor_bias_y`](../38-cvars-reference/03-audio-cvars.md#cl_cursor_bias_y) | Задаёт вертикальное смещение hotspot у пользовательского курсора. | `cl_cursor_bias_y(float)` |
| [`cl_enemychatsound`](../38-cvars-reference/03-audio-cvars.md#cl_enemychatsound) | Определяет путь к локальному звуку, который воспроизводится для сообщений, не распознанных как командный чат. | `cl_enemychatsound(string)` |
| [`cl_maxfps_slop`](../38-cvars-reference/03-audio-cvars.md#cl_maxfps_slop) | Если кадр задерживается (например, из-за плохой точности системного таймера), это то, насколько раньше можно представить, что кадр произошел (в миллисекундах). | `cl_maxfps_slop(int)` |
| [`cl_predict_players_frac`](../38-cvars-reference/03-audio-cvars.md#cl_predict_players_frac) | Насколько прогнозировать других игроков. | `cl_predict_players_frac(float)` |
| [`cl_predict_players_latency`](../38-cvars-reference/03-audio-cvars.md#cl_predict_players_latency) | Отодвиньте плеер назад в соответствии с задержкой, чтобы обеспечить плавную последовательную симуляцию сервера. | `cl_predict_players_latency(float)` |
| [`cl_predict_players_nudge`](../38-cvars-reference/03-audio-cvars.md#cl_predict_players_nudge) | Дополнительный толчок времени, чтобы компенсировать задержку видео. | `cl_predict_players_nudge(float)` |
| [`cl_staticsounds`](../38-cvars-reference/03-audio-cvars.md#cl_staticsounds) | Масштабирует громкость статичных звуковых источников, приходящих из сетевых сообщений карты. | `cl_staticsounds(int)` |
| [`cl_teamchatsound`](../38-cvars-reference/03-audio-cvars.md#cl_teamchatsound) | Определяет путь к звуку, который воспроизводится для сообщений командного чата и чата наблюдаемой команды, когда активен [teamplay](../38-cvars-reference/04-network-server-cvars.md#teamplay). | `cl_teamchatsound(string)` |
| [`cl_voip_autogain`](../38-cvars-reference/03-audio-cvars.md#cl_voip_autogain) | Пытается нормализовать уровень вашего голоса до стандартного уровня. | `cl_voip_autogain(int)` |
| [`cl_voip_bitrate`](../38-cvars-reference/03-audio-cvars.md#cl_voip_bitrate) | Для кодеков с неопределенным битрейтом здесь указывается целевой битрейт, который будет использоваться. | `cl_voip_bitrate(int)` |
| [`cl_voip_capturingvol`](../38-cvars-reference/03-audio-cvars.md#cl_voip_capturingvol) | Во время записи применяется множитель громкости, чтобы ваш звук не был услышан другими. | `cl_voip_capturingvol(float)` |
| [`cl_voip_codec`](../38-cvars-reference/03-audio-cvars.md#cl_voip_codec) | 0: речь (@11khz). 1: сырой. 2: опус. 3: речь (@8khz). 4: речь (@16). 5: речь (@32). 6: ПКМА. 7: ПКМУ. | `cl_voip_codec(string)` |
| [`cl_voip_ducking`](../38-cvars-reference/03-audio-cvars.md#cl_voip_ducking) | Сильно масштабирует звук в игре, когда кто-то с вами разговаривает. | `cl_voip_ducking(float)` |
| [`cl_voip_micamp`](../38-cvars-reference/03-audio-cvars.md#cl_voip_micamp) | Усиливает микрофон при использовании VoIP. | `cl_voip_micamp(int)` |
| [`cl_voip_noisefilter`](../38-cvars-reference/03-audio-cvars.md#cl_voip_noisefilter) | Включите использование фильтра шумоподавления. | `cl_voip_noisefilter(int)` |
| [`cl_voip_play`](../38-cvars-reference/03-audio-cvars.md#cl_voip_play) | Включает воспроизведение VoIP. | `cl_voip_play(int)` |
| [`cl_voip_showmeter`](../38-cvars-reference/03-audio-cvars.md#cl_voip_showmeter) | Показывает громкость вашей речи над стандартным интерфейсом. 0=скрыть, 1=показать при передаче, 2=игнорировать отключение голосовой активации. | `cl_voip_showmeter(int)` |
| [`dpcompat_precachesoundhack`](../38-cvars-reference/03-audio-cvars.md#dpcompat_precachesoundhack) | Изменяет поведение только [precache_sound](../37-quakec-builtins-reference/05-sound-builtins.md#precache_sound) ssqc, чтобы он возвращал индекс precache. | `dpcompat_precachesoundhack(int)` |
| [`dtls_psk_hint`](../38-cvars-reference/03-audio-cvars.md#dtls_psk_hint) | Для рукопожатий DTLS-PSK. Это указывает идентификатор общедоступного сервера. | `dtls_psk_hint(string)` |
| [`dtls_psk_key`](../38-cvars-reference/03-audio-cvars.md#dtls_psk_key) | Для рукопожатий DTLS-PSK. Здесь указывается шестнадцатеричный ключ, который должен совпадать между клиентом и сервером. | `dtls_psk_key(string)` |
| [`dtls_psk_user`](../38-cvars-reference/03-audio-cvars.md#dtls_psk_user) | Для рукопожатий DTLS-PSK. Здесь указывается имя пользователя, которое будет использоваться при совпадении подсказок клиента и сервера. | `dtls_psk_user(string)` |
| [`fs_basepath`](../38-cvars-reference/03-audio-cvars.md#fs_basepath) | Предусмотрено для совместимости Q2/Q3. | `fs_basepath(string)` |
| [`fs_cache`](../38-cvars-reference/03-audio-cvars.md#fs_cache) | 0: выполнять индивидуальный поиск. | `fs_cache(int)` |
| [`fs_game`](../38-cvars-reference/03-audio-cvars.md#fs_game) | Предусмотрено для совместимости со вторым кварталом. | `fs_game(string)` |
| [`fs_gamepath`](../38-cvars-reference/03-audio-cvars.md#fs_gamepath) | Предусмотрено для совместимости Q2/Q3. | `fs_gamepath(string)` |
| [`fs_hidesyspaths`](../38-cvars-reference/03-audio-cvars.md#fs_hidesyspaths) | 0: показывать системные пути в распечатках консоли, которые могут отображаться на снимках экрана или в видеозаписях. | `fs_hidesyspaths(int)` |
| [`fs_homepath`](../38-cvars-reference/03-audio-cvars.md#fs_homepath) | Предусмотрено для совместимости Q2/Q3. | `fs_homepath(string)` |
| [`fs_noreexec`](../38-cvars-reference/03-audio-cvars.md#fs_noreexec) | Отключает автоматическое повторное выполнение конфигураций на переключателях игрового каталога. | `fs_noreexec(int)` |
| [`fs_packageprioritisation`](../38-cvars-reference/03-audio-cvars.md#fs_packageprioritisation) | Предпочитает пакет, который: **0:** Самый последний измененный; **1:** Последний в алфавитном порядке (предпочитает z перед a, 9 перед 0). | `fs_packageprioritisation(int)` |
| [`mod_litsprites_force`](../38-cvars-reference/03-audio-cvars.md#mod_litsprites_force) | Если установлено значение 1, спрайты будут освещены в соответствии с мировым освещением (включая rtlights), как Тенебра. | `mod_litsprites_force(int)` |
| [`net_dns_ipv4`](../38-cvars-reference/03-audio-cvars.md#net_dns_ipv4) | Если 0, отключает преобразование имен DNS в адреса ipv4 (удаляя все связанные сообщения об ошибках). | `net_dns_ipv4(int)` |
| [`net_dns_ipv6`](../38-cvars-reference/03-audio-cvars.md#net_dns_ipv6) | Если 0, отключает преобразование имен DNS в адреса ipv6 (удаляя все связанные сообщения об ошибках). | `net_dns_ipv6(int)` |
| [`qws_builddate`](../38-cvars-reference/03-audio-cvars.md#qws_builddate) | Служебное поле server info с датой сборки движка из SVNDATE. | `qws_builddate(string)` |
| [`qws_buildnum`](../38-cvars-reference/03-audio-cvars.md#qws_buildnum) | Служебное поле server info с номером ревизии сборки из SVNREVISION. | `qws_buildnum(string)` |
| [`qws_fullname`](../38-cvars-reference/03-audio-cvars.md#qws_fullname) | Служебное поле server info с полным названием движка из FULLENGINENAME. | `qws_fullname(string)` |
| [`qws_homepage`](../38-cvars-reference/03-audio-cvars.md#qws_homepage) | Служебное поле server info с адресом официального сайта движка из ENGINEWEBSITE. | `qws_homepage(string)` |
| [`qws_name`](../38-cvars-reference/03-audio-cvars.md#qws_name) | Служебное поле server info с кратким именем или дистрибутивом движка из DISTRIBUTION. | `qws_name(string)` |
| [`qws_platform`](../38-cvars-reference/03-audio-cvars.md#qws_platform) | Служебное поле server info с целевой платформой сборки; вне web-цели к ней добавляется суффикс архитектуры CPU. | `qws_platform(string)` |
| [`qws_version`](../38-cvars-reference/03-audio-cvars.md#qws_version) | Служебное поле server info с основной версией движка в формате major.minor. | `qws_version(string)` |
| [`r_coronas_fadedist`](../38-cvars-reference/03-audio-cvars.md#r_coronas_fadedist) | На этом расстоянии короны исчезнут. | `r_coronas_fadedist(int)` |
| [`r_coronas_intensity`](../38-cvars-reference/03-audio-cvars.md#r_coronas_intensity) | Альтернативный множитель интенсивности корон. | `r_coronas_intensity(int)` |
| [`r_coronas_mindist`](../38-cvars-reference/03-audio-cvars.md#r_coronas_mindist) | Короны, расположенные ближе к этому значению, будут невидимы, что предотвратит проблемы с близкой плоскостью отсечения. | `r_coronas_mindist(int)` |
| [`r_coronas_occlusion`](../38-cvars-reference/03-audio-cvars.md#r_coronas_occlusion) | Указывает, что короны следует закрывать более тщательно. | `r_coronas_occlusion(string)` |
| [`r_editlights_cursordistance`](../38-cvars-reference/03-audio-cvars.md#r_editlights_cursordistance) | Максимальное расстояние курсора от глаза. | `r_editlights_cursordistance(int)` |
| [`r_editlights_cursorgrid`](../38-cvars-reference/03-audio-cvars.md#r_editlights_cursorgrid) | Привязывает курсор к этому размеру сетки. | `r_editlights_cursorgrid(int)` |
| [`r_editlights_cursorpushback`](../38-cvars-reference/03-audio-cvars.md#r_editlights_cursorpushback) | Насколько далеко по какой-то причине отвести курсор назад к глазу. | `r_editlights_cursorpushback(int)` |
| [`r_editlights_cursorpushoff`](../38-cvars-reference/03-audio-cvars.md#r_editlights_cursorpushoff) | На какое расстояние отодвинуть курсор от поверхности воздействия. | `r_editlights_cursorpushoff(int)` |
| [`r_editlights_import_ambient`](../38-cvars-reference/03-audio-cvars.md#r_editlights_import_ambient) | Масштабирование окружающего света для импортных источников света. | `r_editlights_import_ambient(int)` |
| [`r_editlights_import_diffuse`](../38-cvars-reference/03-audio-cvars.md#r_editlights_import_diffuse) | Инструмент для масштабирования рассеянного света для импортных источников света. | `r_editlights_import_diffuse(int)` |
| [`r_editlights_import_radius`](../38-cvars-reference/03-audio-cvars.md#r_editlights_import_radius) | Изменяет размер световых объектов, загружаемых с карты. | `r_editlights_import_radius(int)` |
| [`r_editlights_import_specular`](../38-cvars-reference/03-audio-cvars.md#r_editlights_import_specular) | Средство масштабирования зеркального света для импортных источников света. | `r_editlights_import_specular(int)` |
| [`r_font_postprocess_mono`](../38-cvars-reference/03-audio-cvars.md#r_font_postprocess_mono) | Отключает сглаживание шрифтов. | `r_font_postprocess_mono(int)` |
| [`r_font_postprocess_outline`](../38-cvars-reference/03-audio-cvars.md#r_font_postprocess_outline) | Управляет количеством пикселей темных рамок вокруг шрифтов. | `r_font_postprocess_outline(int)` |
| [`r_part_sparks_textured`](../38-cvars-reference/03-audio-cvars.md#r_part_sparks_textured) | Управляет textured spark-рендерингом в scripted [particle](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#particle) system. | `r_part_sparks_textured(int)` |
| [`r_part_sparks_trifan`](../38-cvars-reference/03-audio-cvars.md#r_part_sparks_trifan) | Управляет trifan-вариантом spark-частиц в scripted particle system. | `r_part_sparks_trifan(int)` |
| [`rank_parms_first`](../38-cvars-reference/03-audio-cvars.md#rank_parms_first) | Настройка мода: первый параметр сохранен. | `rank_parms_first(int)` |
| [`rank_parms_last`](../38-cvars-reference/03-audio-cvars.md#rank_parms_last) | Настройка мода: индекс последнего сохраняемого параметра. | `rank_parms_last(int)` |
| [`ruleset_allow_overlong_sounds`](../38-cvars-reference/03-audio-cvars.md#ruleset_allow_overlong_sounds) | Установка значения 0 заблокирует использование сверхдлинных звуков срабатывания в качестве таймеров возрождения предметов. | `ruleset_allow_overlong_sounds(int)` |
| [`s_al_distancemodel`](../38-cvars-reference/03-audio-cvars.md#s_al_distancemodel) | Управляет тем, как звуки затухают с расстоянием. | `s_al_distancemodel(int)` |
| [`s_al_dopplerfactor`](../38-cvars-reference/03-audio-cvars.md#s_al_dopplerfactor) | Умножает силу доплеровского эффекта. | `s_al_dopplerfactor(float)` |
| [`s_al_max_distance`](../38-cvars-reference/03-audio-cvars.md#s_al_max_distance) | В текущем коде cvar объявлен и регистрация с использованием для него закомментированы. | `s_al_max_distance(int)` |
| [`s_al_rolloff_factor`](../38-cvars-reference/03-audio-cvars.md#s_al_rolloff_factor) | В текущем коде cvar полностью выключен: его объявление, регистрация и применение к AL_ROLLOFF_FACTOR закомментированы. | `s_al_rolloff_factor(int)` |
| [`s_al_speedofsound`](../38-cvars-reference/03-audio-cvars.md#s_al_speedofsound) | Настраивает скорость звука, в игровых единицах в секунду. | `s_al_speedofsound(float)` |
| [`s_al_static_listener`](../38-cvars-reference/03-audio-cvars.md#s_al_static_listener) | Отключает обновление слушателя OpenAL. | `s_al_static_listener(int)` |
| [`s_ambientfade`](../38-cvars-reference/03-audio-cvars.md#s_ambientfade) | Задаёт скорость, с которой ambient sounds подтягиваются к целевой громкости текущей зоны. | `s_ambientfade(int)` |
| [`s_ambientlevel`](../38-cvars-reference/03-audio-cvars.md#s_ambientlevel) | Это контролирует уровни громкости автоматических звуков, зависящих от области (например, воды или неба), и это очень раздражает. | `s_ambientlevel(float)` |
| [`s_bits`](../38-cvars-reference/03-audio-cvars.md#s_bits) | Запрашивает разрядность выходного аудиобуфера при инициализации [sound](../37-quakec-builtins-reference/05-sound-builtins.md#sound) device. | `s_bits(int)` |
| [`s_buffersize`](../38-cvars-reference/03-audio-cvars.md#s_buffersize) | Запрашивает размер аппаратного аудиобуфера при инициализации sound device. | `s_buffersize(int)` |
| [`s_device`](../38-cvars-reference/03-audio-cvars.md#s_device) | Это используемые звуковые устройства в формате драйвер:устройство. | `s_device(string)` |
| [`s_doppler`](../38-cvars-reference/03-audio-cvars.md#s_doppler) | Включает допплер с множителем шкалы. | `s_doppler(int)` |
| [`s_doppler_max`](../38-cvars-reference/03-audio-cvars.md#s_doppler_max) | Максимально допустимая допплеровская шкала, чтобы избежать слишком странных ситуаций. | `s_doppler_max(int)` |
| [`s_doppler_min`](../38-cvars-reference/03-audio-cvars.md#s_doppler_min) | Самая медленная допустимая допплеровская шкала. | `s_doppler_min(float)` |
| [`s_eax`](../38-cvars-reference/03-audio-cvars.md#s_eax) | Включает попытку использовать EAX/reverb в DirectSound backend-е. | `s_eax(int)` |
| [`s_inactive`](../38-cvars-reference/03-audio-cvars.md#s_inactive) | Воспроизведение звука, когда приложение неактивно (т. е. закрыто). | `s_inactive(int)` |
| [`s_khz`](../38-cvars-reference/03-audio-cvars.md#s_khz) | Задаёт частоту звукового микшера. | `s_khz(int/string)` |
| [`s_linearresample`](../38-cvars-reference/03-audio-cvars.md#s_linearresample) | Управляет режимом resampling для обычных загружаемых sound effects. | `s_linearresample(int)` |
| [`s_linearresample_stream`](../38-cvars-reference/03-audio-cvars.md#s_linearresample_stream) | Управляет режимом resampling для потокового аудио и raw streams. | `s_linearresample_stream(int)` |
| [`s_loadas8bit`](../38-cvars-reference/03-audio-cvars.md#s_loadas8bit) | Понижение качества звука при загрузке до 8-битного звука более низкого качества для экономии памяти. | `s_loadas8bit(int)` |
| [`s_localvolume`](../38-cvars-reference/03-audio-cvars.md#s_localvolume) | Уровень звука для местных звуков или звуков, исходящих от игрока, таких как звуки стрельбы и боли. | `s_localvolume(int)` |
| [`s_mixahead`](../38-cvars-reference/03-audio-cvars.md#s_mixahead) | Указывает, сколько секунд предварительно буферизовать звук. | `s_mixahead(float)` |
| [`s_mixerthread`](../38-cvars-reference/03-audio-cvars.md#s_mixerthread) | При включении микширование звука будет выполняться в отдельном потоке. | `s_mixerthread(int)` |
| [`s_noextraupdate`](../38-cvars-reference/03-audio-cvars.md#s_noextraupdate) | Отключает S_ExtraUpdate, дополнительный внекадровый проход обновления микшера. | `s_noextraupdate(int)` |
| [`s_nominaldistance`](../38-cvars-reference/03-audio-cvars.md#s_nominaldistance) | Эта переменная определяет, насколько далеко можно услышать звук с ослаблением = 1. | `s_nominaldistance(int)` |
| [`s_numspeakers`](../38-cvars-reference/03-audio-cvars.md#s_numspeakers) | Поддерживает до 6. | `s_numspeakers(int)` |
| [`s_precache`](../38-cvars-reference/03-audio-cvars.md#s_precache) | Управляет стратегией предварительной загрузки sounds. | `s_precache(int)` |
| [`s_show`](../38-cvars-reference/03-audio-cvars.md#s_show) | Включает отладочный вывод по sound channels при каждом обновлении звука. | `s_show(int)` |
| [`s_swapstereo`](../38-cvars-reference/03-audio-cvars.md#s_swapstereo) | Меняет местами левый и правый каналы на этапе микширования. | `s_swapstereo(int)` |
| [`show_fps_x`](../38-cvars-reference/03-audio-cvars.md#show_fps_x) | Задаёт горизонтальную позицию строки, которую выводит [show_fps](../38-cvars-reference/07-system-misc-cvars.md#show_fps). | `show_fps_x(int)` |
| [`show_fps_y`](../38-cvars-reference/03-audio-cvars.md#show_fps_y) | Задаёт вертикальную позицию строки, которую выводит show_fps. | `show_fps_y(int)` |
| [`sv_cullentities_trace`](../38-cvars-reference/03-audio-cvars.md#sv_cullentities_trace) | Попытайтесь отсеять неигровые объекты, используя линии трассировки, в качестве крайнего анти-wallhack. | `sv_cullentities_trace(string)` |
| [`sv_loadentfiles_dir`](../38-cvars-reference/03-audio-cvars.md#sv_loadentfiles_dir) | Задаёт дополнительный подкаталог внутри maps/, в котором движок ищет и сохраняет внешние файлы сущностей для карт. | `sv_loadentfiles_dir(string)` |
| [`sv_sound_land`](../38-cvars-reference/03-audio-cvars.md#sv_sound_land) | Определяет имя звукового файла, который сервер воспроизводит при жёстком приземлении сущности на землю. | `sv_sound_land(string)` |
| [`sv_sound_watersplash`](../38-cvars-reference/03-audio-cvars.md#sv_sound_watersplash) | Определяет звук пересечения границы воды. | `sv_sound_watersplash(string)` |
| [`sv_voip`](../38-cvars-reference/03-audio-cvars.md#sv_voip) | Включите прием голосовых пакетов. | `sv_voip(int)` |
| [`sv_voip_echo`](../38-cvars-reference/03-audio-cvars.md#sv_voip_echo) | Эхо голосовых пакетов обратно отправителю (настройка отладки/тестирования). | `sv_voip_echo(int)` |
| [`sv_voip_record`](../38-cvars-reference/03-audio-cvars.md#sv_voip_record) | Запишите голосовой чат на mvds. | `sv_voip_record(int)` |
| [`sys_clockprecision`](../38-cvars-reference/03-audio-cvars.md#sys_clockprecision) | Пытается контролировать интервал прерываний Windows в миллисекундах. | `sys_clockprecision(int)` |
| [`sys_clocktype`](../38-cvars-reference/03-audio-cvars.md#sys_clocktype) | 2: монотонный. | `sys_clocktype(string)` |
| [`sys_colorconsole`](../38-cvars-reference/03-audio-cvars.md#sys_colorconsole) | Анализ экранирования цвета с использованием цветов Ansi на стандартном выводе. | `sys_colorconsole(int)` |
| [`sys_disableTaskSwitch`](../38-cvars-reference/03-audio-cvars.md#sys_disabletaskswitch) | На Windows включает жёсткую блокировку системных сочетаний для выхода из игры. | `sys_disableTaskSwitch(int)` |
| [`sys_disableWinKeys`](../38-cvars-reference/03-audio-cvars.md#sys_disablewinkeys) | На Windows перехватывает клавиши LWIN, RWIN и Application через низкоуровневый keyboard hook. | `sys_disableWinKeys(int)` |
| [`sys_extrasleep`](../38-cvars-reference/03-audio-cvars.md#sys_extrasleep) | Добавляет дополнительную паузу в основной серверный цикл Unix-сборки. | `sys_extrasleep(int)` |
| [`sys_highpriority`](../38-cvars-reference/03-audio-cvars.md#sys_highpriority) | Контролирует приоритет процесса. | `sys_highpriority(int)` |
| [`sys_keepscreenon`](../38-cvars-reference/03-audio-cvars.md#sys_keepscreenon) | Если установлено, экран никогда не будет темнеть. | `sys_keepscreenon(int)` |
| [`sys_linebuffer`](../38-cvars-reference/03-audio-cvars.md#sys_linebuffer) | Переключает режим чтения stdin в Unix-консоли. | `sys_linebuffer(int)` |
| [`sys_nostdout`](../38-cvars-reference/03-audio-cvars.md#sys_nostdout) | Полностью отключает вывод Sys_Printf в stdout. | `sys_nostdout(int)` |
| [`sys_orientation`](../38-cvars-reference/03-audio-cvars.md#sys_orientation) | Указывает, под каким углом визуализировать землетрясение. | `sys_orientation(string)` |
| [`sys_platform`](../38-cvars-reference/03-audio-cvars.md#sys_platform) | Содержит строковый идентификатор платформы сборки, подставленный из макроса PLATFORM. | `sys_platform(string)` |
| [`sys_timestamps`](../38-cvars-reference/03-audio-cvars.md#sys_timestamps) | Показывать временные метки на распечатках стандартного вывода. | `sys_timestamps(int)` |
| [`sys_vibrate`](../38-cvars-reference/03-audio-cvars.md#sys_vibrate) | Включает системный вибратор при возникновении повреждений и тому подобном. | `sys_vibrate(int)` |
| [`tls_ignorecertificateerrors`](../38-cvars-reference/03-audio-cvars.md#tls_ignorecertificateerrors) | НИКОГДА не следует устанавливать значение 1! | `tls_ignorecertificateerrors(int)` |
| [`tls_provider`](../38-cvars-reference/03-audio-cvars.md#tls_provider) | Определяет, какой поставщик TLS использовать. | `tls_provider(string)` |

### Сеть, сервер и мультиплеер

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`allow_download_configs`](../38-cvars-reference/04-network-server-cvars.md#allow_download_configs) | Управляет выдачей конфигурационных файлов с сервера: расширение `.cfg` и каталог `configs/`. | `allow_download_configs(boolean)` |
| [`allow_download_copyrighted`](../38-cvars-reference/04-network-server-cvars.md#allow_download_copyrighted) | Разрешает или запрещает скачивание пакетов, которые движок считает потенциально защищёнными авторским правом — прежде всего архивов с префиксом `pak`. | `allow_download_copyrighted(boolean)` |
| [`allow_download_demos`](../38-cvars-reference/04-network-server-cvars.md#allow_download_demos) | Разрешает загрузку файлов из каталога `demos/` обычной серверной раздачей. | `allow_download_demos(boolean)` |
| [`allow_download_logs`](../38-cvars-reference/04-network-server-cvars.md#allow_download_logs) | Разрешает скачивание файлов `.log`. | `allow_download_logs(boolean)` |
| [`allow_download_maps`](../38-cvars-reference/04-network-server-cvars.md#allow_download_maps) | Включает раздачу файлов из каталога `maps/`. | `allow_download_maps(boolean)` |
| [`allow_download_models`](../38-cvars-reference/04-network-server-cvars.md#allow_download_models) | Управляет загрузкой содержимого `progs/` и `models/`. | `allow_download_models(boolean)` |
| [`allow_download_pakcontents`](../38-cvars-reference/04-network-server-cvars.md#allow_download_pakcontents) | Определяет, можно ли скачивать немаповые файлы, лежащие внутри серверных пакетов. | `allow_download_pakcontents(int enum)` |
| [`allow_download_pakmaps`](../38-cvars-reference/04-network-server-cvars.md#allow_download_pakmaps) | Контролирует скачивание карт, находящихся внутри пакетов сервера. | `allow_download_pakmaps(int enum)` |
| [`allow_download_skins`](../38-cvars-reference/04-network-server-cvars.md#allow_download_skins) | Разрешает выдачу файлов из каталога `skins/`. | `allow_download_skins(boolean)` |
| [`allow_download_sounds`](../38-cvars-reference/04-network-server-cvars.md#allow_download_sounds) | Включает встроенную раздачу файлов из каталога `sound/`. | `allow_download_sounds(boolean)` |
| [`coop`](../38-cvars-reference/04-network-server-cvars.md#coop) | Классическая `serverinfo`-переменная режима cooperative. | `coop(boolean/int)` |
| [`deathmatch`](../38-cvars-reference/04-network-server-cvars.md#deathmatch) | Основной режим сетевой игры, экспортируемый как `serverinfo`. | `deathmatch(int enum)` |
| [`filterban`](../38-cvars-reference/04-network-server-cvars.md#filterban) | Определяет действие по умолчанию для IP-фильтрации и команды `addip`. | `filterban(boolean)` |
| [`fraglimit`](../38-cvars-reference/04-network-server-cvars.md#fraglimit) | `serverinfo`-переменная лимита фрагов на карту или раунд. | `fraglimit(int)` |
| [`hostname`](../38-cvars-reference/04-network-server-cvars.md#hostname) | Имя сервера, публикуемое через `serverinfo`, status-ответы и мастер-листинги. | `hostname(string)` |
| [`maxclients`](../38-cvars-reference/04-network-server-cvars.md#maxclients) | Ограничивает число игровых слотов для активных игроков; переменная помечена как `serverinfo` и может меняться даже посреди карты. | `maxclients(int)` |
| [`maxspectators`](../38-cvars-reference/04-network-server-cvars.md#maxspectators) | Задаёт максимальное число зрителей на сервере и тоже объявлена как `serverinfo`. | `maxspectators(int)` |
| [`net_compress`](../38-cvars-reference/04-network-server-cvars.md#net_compress) | Легаси-переключатель Huffman-сжатия сетевых пакетов. | `net_compress(boolean)` |
| [`net_enabled`](../38-cvars-reference/04-network-server-cvars.md#net_enabled) | Глобальный рубильник сетевой подсистемы. | `net_enabled(boolean)` |
| [`net_enable_http`](../38-cvars-reference/04-network-server-cvars.md#net_enable_http) | Разрешает принимать HTTP-клиентов на TCP-портах движка. | `net_enable_http(boolean)` |
| [`net_enable_qtv`](../38-cvars-reference/04-network-server-cvars.md#net_enable_qtv) | Управляет тем, принимает ли сервер входящие QTV-запросы от прокси и зрителей. | `net_enable_qtv(int enum)` |
| [`net_enable_rtcbroker`](../38-cvars-reference/04-network-server-cvars.md#net_enable_rtcbroker) | Разрешает принимать websocket-подключения, используемые как брокер для прямых WebRTC/ICE-сеансов. | `net_enable_rtcbroker(boolean)` |
| [`net_enable_tls`](../38-cvars-reference/04-network-server-cvars.md#net_enable_tls) | Позволяет интерпретировать бинарный поток на обычном TCP-порту как TLS-handshake, то есть поднимать `https`/`wss` поверх того же порта. | `net_enable_tls(boolean)` |
| [`net_enable_websockets`](../38-cvars-reference/04-network-server-cvars.md#net_enable_websockets) | Разрешает websocket-игроков на TCP-портах движка. | `net_enable_websockets(boolean)` |
| [`net_hybriddualstack`](../38-cvars-reference/04-network-server-cvars.md#net_hybriddualstack) | Включает гибридные dual-stack сокеты IPv4+IPv6 там, где платформа это поддерживает. | `net_hybriddualstack(boolean)` |
| [`net_ice_allowmdns`](../38-cvars-reference/04-network-server-cvars.md#net_ice_allowmdns) | Разрешает использовать [multicast](../37-quakec-builtins-reference/04-network-messages-builtins.md#multicast)-DNS для ICE-кандидатов, чтобы не раскрывать частную адресацию напрямую, а публиковать псевдонимы. | `net_ice_allowmdns(boolean)` |
| [`net_ice_allowstun`](../38-cvars-reference/04-network-server-cvars.md#net_ice_allowstun) | Управляет использованием STUN для определения публичного адреса этого узла. | `net_ice_allowstun(boolean)` |
| [`net_ice_allowturn`](../38-cvars-reference/04-network-server-cvars.md#net_ice_allowturn) | Определяет, может ли движок регистрировать TURN-соединения и использовать релейные кандидаты. | `net_ice_allowturn(boolean)` |
| [`net_ice_broker`](../38-cvars-reference/04-network-server-cvars.md#net_ice_broker) | Адрес брокера, через который FTE пытается организовывать ICE/RTC-соединения для `sv_public /foo` и `connect /foo`. | `net_ice_broker(string URL)` |
| [`net_ice_relayonly`](../38-cvars-reference/04-network-server-cvars.md#net_ice_relayonly) | Если включить `1`, движок перестанет рекламировать нерелейные локальные кандидаты и будет пытаться ходить к удалённой стороне только через relay. | `net_ice_relayonly(boolean)` |
| [`net_ice_servers`](../38-cvars-reference/04-network-server-cvars.md#net_ice_servers) | Список ICE-серверов через пробел: STUN и TURN адреса вида `stun:host:port` или `turn:host:port?...`. | `net_ice_servers(string list)` |
| [`net_mtu`](../38-cvars-reference/04-network-server-cvars.md#net_mtu) | Ограничивает максимальный размер UDP-payload до фрагментации. | `net_mtu(int)` |
| [`password`](../38-cvars-reference/04-network-server-cvars.md#password) | Пароль на вход в игру. Пустая строка означает открытый доступ, непустая — что клиенту нужно знать пароль ещё до полноценного входа на сервер. | `password(string)` |
| [`qtv_maxstreams`](../38-cvars-reference/04-network-server-cvars.md#qtv_maxstreams) | Лимит прямых QTV-клиентов и прокси, которые могут одновременно висеть на сервере. | `qtv_maxstreams(int or empty)` |
| [`rcon_password`](../38-cvars-reference/04-network-server-cvars.md#rcon_password) | Пароль удалённого администрирования ([`rcon`](../44-cli-commands-reference/02-client-ui-commands.md#rcon)). | `rcon_password(string)` |
| [`spectator_password`](../38-cvars-reference/04-network-server-cvars.md#spectator_password) | Отдельный пароль для входа зрителем. | `spectator_password(string)` |
| [`sv_banproxies`](../38-cvars-reference/04-network-server-cvars.md#sv_banproxies) | Если включить, сервер будет отказывать клиентам, распознанным как известные proxy-программы. | `sv_banproxies(boolean)` |
| [`sv_bigcoords`](../38-cvars-reference/04-network-server-cvars.md#sv_bigcoords) | Переключает сетевое кодирование координат на float вместо 16-битного формата и одновременно повышает точность углов. | `sv_bigcoords(boolean)` |
| [`sv_calcphs`](../38-cvars-reference/04-network-server-cvars.md#sv_calcphs) | Управляет расчётом PHS для отсечения звуковых событий. | `sv_calcphs(int enum)` |
| [`sv_crypt_rcon`](../38-cvars-reference/04-network-server-cvars.md#sv_crypt_rcon) | Определяет, должен ли rcon-пароль передаваться в хешированном виде. | `sv_crypt_rcon(string/switch)` |
| [`sv_cullplayers_trace`](../38-cvars-reference/04-network-server-cvars.md#sv_cullplayers_trace) | Пытается отсеивать сетевую репликацию игроков трассировками как анти-wallhack меру. | `sv_cullplayers_trace(boolean/int)` |
| [`sv_demoClearOld`](../38-cvars-reference/04-network-server-cvars.md#sv_democlearold) | Разрешает автоматически удалять старые MVD-демо, чтобы каталог не разрастался бесконтрольно. | `sv_demoClearOld(boolean)` |
| [`sv_demoExtensions`](../38-cvars-reference/04-network-server-cvars.md#sv_demoextensions) | Разрешает дополнительные протокольные расширения внутри записываемых MVD. | `sv_demoExtensions(int enum)` |
| [`sv_demofps`](../38-cvars-reference/04-network-server-cvars.md#sv_demofps) | Частота фиксации данных в MVD-записи. | `sv_demofps(int)` |
| [`sv_demoMaxDirAge`](../38-cvars-reference/04-network-server-cvars.md#sv_demomaxdirage) | Максимальный возраст MVD-файлов, используемый очисткой при активном `sv_demoClearOld`. | `sv_demoMaxDirAge(time/int)` |
| [`sv_demoMaxDirCount`](../38-cvars-reference/04-network-server-cvars.md#sv_demomaxdircount) | Лимит количества записанных сервером MVD-файлов. | `sv_demoMaxDirCount(int)` |
| [`sv_demoMaxDirSize`](../38-cvars-reference/04-network-server-cvars.md#sv_demomaxdirsize) | Максимальный суммарный объём диска, который может занимать каталог серверных MVD. | `sv_demoMaxDirSize(size/string)` |
| [`sv_demoMaxSize`](../38-cvars-reference/04-network-server-cvars.md#sv_demomaxsize) | Обрезает каждое отдельное демо до указанного максимального размера. | `sv_demoMaxSize(size/string)` |
| [`sv_demoUseCache`](../38-cvars-reference/04-network-server-cvars.md#sv_demousecache) | Если переменная включена, сервер буферизует запись MVD и сбрасывает данные на диск периодически, а не после каждого малого фрагмента. | `sv_demoUseCache(boolean/int)` |
| [`sv_demo_write_csqc`](../38-cvars-reference/04-network-server-cvars.md#sv_demo_write_csqc) | Вкладывает копию `csprogs` в записываемое демо. | `sv_demo_write_csqc(boolean/int)` |
| [`sv_guidkey`](../38-cvars-reference/04-network-server-cvars.md#sv_guidkey) | Меняет базу, от которой клиент вычисляет свой GUID: вместо серверного IP используется заданная строка. | `sv_guidkey(string)` |
| [`sv_heartbeat_checks`](../38-cvars-reference/04-network-server-cvars.md#sv_heartbeat_checks) | Разрешает диагностические сообщения, когда `sv_public 1` не срабатывает из-за вероятных проблем с NAT/роутером. | `sv_heartbeat_checks(boolean)` |
| [`sv_heartbeat_interval`](../38-cvars-reference/04-network-server-cvars.md#sv_heartbeat_interval) | Интервал отправки heartbeat на мастер-серверы. | `sv_heartbeat_interval(int)` |
| [`sv_heartbeattimeout`](../38-cvars-reference/04-network-server-cvars.md#sv_heartbeattimeout) | Сколько секунд мастер-сервер держит запись о сервере после последнего heartbeat. | `sv_heartbeattimeout(int)` |
| [`sv_limittics`](../38-cvars-reference/04-network-server-cvars.md#sv_limittics) | Лимит количества физико-сетевых тиков, которые сервер может прогнать за один кадр, чтобы нагнать отставание. | `sv_limittics(int)` |
| [`sv_listen_dp`](../38-cvars-reference/04-network-server-cvars.md#sv_listen_dp) | Легаси-переключатель совместимости для старого DP-специфичного handshake. | `sv_listen_dp(boolean)` |
| [`sv_listen_nq`](../38-cvars-reference/04-network-server-cvars.md#sv_listen_nq) | Легаси-переключатель допуска NetQuake-клиентов. | `sv_listen_nq(int enum)` |
| [`sv_maxdrate`](../38-cvars-reference/04-network-server-cvars.md#sv_maxdrate) | Ограничивает скорость, с которой сервер отдаёт данные одному клиенту во время скачивания файлов. | `sv_maxdrate(speed/int)` |
| [`sv_maxgames`](../38-cvars-reference/04-network-server-cvars.md#sv_maxgames) | Переменная master-server процесса: максимум известных игр/гейм-идентификаторов, которые он хранит. | `sv_maxgames(int)` |
| [`sv_maxrate`](../38-cvars-reference/04-network-server-cvars.md#sv_maxrate) | Верхний предел байт в секунду, который сервер готов отправлять одному игроку вне режима скачивания. | `sv_maxrate(speed/int)` |
| [`sv_maxservers`](../38-cvars-reference/04-network-server-cvars.md#sv_maxservers) | Общий максимум серверов, известных мастер-серверу по всем играм сразу. | `sv_maxservers(int)` |
| [`sv_maxtic`](../38-cvars-reference/04-network-server-cvars.md#sv_maxtic) | Максимальный интервал между серверными физическими тиками. | `sv_maxtic(float)` |
| [`sv_minping`](../38-cvars-reference/04-network-server-cvars.md#sv_minping) | Искусственно добавляет задержку игрокам, у которых реальный ping ниже заданного порога. | `sv_minping(int)` |
| [`sv_mintic`](../38-cvars-reference/04-network-server-cvars.md#sv_mintic) | Минимальный интервал между физическими тиками сервера. | `sv_mintic(float)` |
| [`sv_nailhack`](../38-cvars-reference/04-network-server-cvars.md#sv_nailhack) | Переключает историческую сетевую оптимизацию nail-энтити. | `sv_nailhack(boolean)` |
| [`sv_playerslots`](../38-cvars-reference/04-network-server-cvars.md#sv_playerslots) | Общий лимит слотов игроков, зрителей и ботов; в коде отмечено, что новое значение применяется со следующей карты и может привести к кикам, а alias `maxplayers` используется для совместимости. | `sv_playerslots(int or empty)` |
| [`sv_protocol`](../38-cvars-reference/04-network-server-cvars.md#sv_protocol) | Форсирует конкретные расширения сетевого протокола: исходник перечисляет `fte1`, `fte2`, `csqc`. | `sv_protocol(string list)` |
| [`sv_public`](../38-cvars-reference/04-network-server-cvars.md#sv_public) | Главный переключатель публичности сервера. | `sv_public(int enum)` |
| [`sv_rconlim`](../38-cvars-reference/04-network-server-cvars.md#sv_rconlim) | Ограничивает повторные невалидные попытки `rcon`. | `sv_rconlim(int)` |
| [`sv_reconnectlimit`](../38-cvars-reference/04-network-server-cvars.md#sv_reconnectlimit) | Не даёт одному и тому же адресу/порту мгновенно переподключаться в течение заданного времени. | `sv_reconnectlimit(int)` |
| [`sv_reliable_sound`](../38-cvars-reference/04-network-server-cvars.md#sv_reliable_sound) | Переводит отправку звуков в надёжный канал, чтобы packet loss не съедал важные аудио-события. | `sv_reliable_sound(boolean)` |
| [`sv_reportheartbeats`](../38-cvars-reference/04-network-server-cvars.md#sv_reportheartbeats) | Управляет тем, насколько многословно сервер сообщает об отправке heartbeat на мастер. | `sv_reportheartbeats(int enum)` |
| [`sv_serverip`](../38-cvars-reference/04-network-server-cvars.md#sv_serverip) | Ручной внешний адрес сервера, который движок должен объявлять, если сам не может надёжно определить публичный IP за NAT/фаерволом. | `sv_serverip(string address)` |
| [`sv_slaverequery`](../38-cvars-reference/04-network-server-cvars.md#sv_slaverequery) | Переменная master-сервера: как часто он заново опрашивает slave-master узлы. | `sv_slaverequery(int)` |
| [`sv_timestamplen`](../38-cvars-reference/04-network-server-cvars.md#sv_timestamplen) | Хотя внутренняя переменная называется `sv_crypt_rcon_clockskew`, наружу экспортируется именно `sv_timestamplen`: она ограничивает допустимый сдвиг часов для снижения delayed replay-атак на hashed rcon. | `sv_timestamplen(int)` |
| [`sv_use_dns`](../38-cvars-reference/04-network-server-cvars.md#sv_use_dns) | Включает reverse-DNS lookup, чтобы сервер мог показывать более читаемую информацию о том, откуда подключаются клиенты. | `sv_use_dns(boolean/string)` |
| [`teamplay`](../38-cvars-reference/04-network-server-cvars.md#teamplay) | Классическая `serverinfo`-переменная командного режима. | `teamplay(int)` |
| [`timelimit`](../38-cvars-reference/04-network-server-cvars.md#timelimit) | Лимит времени на карту/раунд, экспортируемый через `serverinfo`. | `timelimit(int)` |
| [`timeout`](../38-cvars-reference/04-network-server-cvars.md#timeout) | Глобальный сетевой таймаут: если от соединения не приходит ни одного пакета дольше указанного числа секунд, оно считается умершим. | `timeout(int)` |
| [`zombietime`](../38-cvars-reference/04-network-server-cvars.md#zombietime) | Сколько секунд сервер не переиспользует клиентский слот после разрыва, пока «досасывает» поздние пакеты и хвосты состояния. | `zombietime(int)` |
| [`allow_download`](../38-cvars-reference/04-network-server-cvars.md#allow_download) | Если 1, загрузка разрешена. | `allow_download(int)` |
| [`allow_download_locs`](../38-cvars-reference/04-network-server-cvars.md#allow_download_locs) | 0 блокирует загрузку любого файла в каталоге locs/. | `allow_download_locs(int)` |
| [`allow_download_other`](../38-cvars-reference/04-network-server-cvars.md#allow_download_other) | 0 блокирует загрузку любого файла, который не был включен ни в один из блоков загрузки каталога. | `allow_download_other(int)` |
| [`allow_download_packages`](../38-cvars-reference/04-network-server-cvars.md#allow_download_packages) | Если 1, разрешается загрузка файлов (из корневого каталога или другого места) с известными расширениями пакетов (например: pak+pk3). | `allow_download_packages(int)` |
| [`allow_download_particles`](../38-cvars-reference/04-network-server-cvars.md#allow_download_particles) | 0 блокирует загрузку любого файла в каталоге частиц/. | `allow_download_particles(int)` |
| [`allow_download_refpackages`](../38-cvars-reference/04-network-server-cvars.md#allow_download_refpackages) | Если установлено значение 1, пакеты, содержащие файлы, необходимые во время функций создания, будут становиться «ссылками» и автоматически загружаться клиентам. | `allow_download_refpackages(int)` |
| [`allow_download_root`](../38-cvars-reference/04-network-server-cvars.md#allow_download_root) | Если установлено, разрешает загрузку из корня игрового каталога (а не из базового). | `allow_download_root(int)` |
| [`allow_download_textures`](../38-cvars-reference/04-network-server-cvars.md#allow_download_textures) | 0 блокирует загрузку любого файла в директорииtextures/. | `allow_download_textures(int)` |
| [`allow_download_wads`](../38-cvars-reference/04-network-server-cvars.md#allow_download_wads) | 0 блокирует загрузку любого файла, находящегося в каталоге wads/ или находящегося в корневом каталоге с расширением .[wad](../39-entity-keys-reference/01-worldspawn-common-keys.md#wad). | `allow_download_wads(int)` |
| [`capturerate`](../38-cvars-reference/04-network-server-cvars.md#capturerate) | Частота кадров записываемого видео. | `capturerate(int)` |
| [`cl_download_csprogs`](../38-cvars-reference/04-network-server-cvars.md#cl_download_csprogs) | Загрузите обновленный игровой код клиента, если он доступен. | `cl_download_csprogs(int)` |
| [`cl_download_mapsrc`](../38-cvars-reference/04-network-server-cvars.md#cl_download_mapsrc) | Указывает префикс местоположения http для загрузки карт. | `cl_download_mapsrc(string)` |
| [`cl_download_packages`](../38-cvars-reference/04-network-server-cvars.md#cl_download_packages) | 0=Не загружать пакеты просто потому, что их использует сервер. 1=Скачивайте и загружайте пакеты по мере необходимости (не влияет на игры, которые не используют этот пакет). 2=Загрузить и установить навсегда (используйте с осторожностью!). | `cl_download_packages(int)` |
| [`cl_download_redirection`](../38-cvars-reference/04-network-server-cvars.md#cl_download_redirection) | Следуйте перенаправлению загрузки, чтобы загружать пакеты вместо отдельных файлов. | `cl_download_redirection(int)` |
| [`cl_download_wait`](../38-cvars-reference/04-network-server-cvars.md#cl_download_wait) | 0=присоединиться к игре еще до завершения загрузки (может быть с задержкой). 1=дождитесь завершения всех загрузок, прежде чем присоединяться. | `cl_download_wait(int)` |
| [`cl_downloads`](../38-cvars-reference/04-network-server-cvars.md#cl_downloads) | Позволяет блокировать все автоматические загрузки. | `cl_downloads(int)` |
| [`com_fullgamename`](../38-cvars-reference/04-network-server-cvars.md#com_fullgamename) | Файловая система пытается запустить эту игру. | `com_fullgamename(string)` |
| [`com_gamedirnativecode`](../38-cvars-reference/04-network-server-cvars.md#com_gamedirnativecode) | ), который позже запускается из того же игрового каталога. | `com_gamedirnativecode(int)` |
| [`com_highlightcolor`](../38-cvars-reference/04-network-server-cvars.md#com_highlightcolor) | Задаёт ANSI-код цвета для подсвеченного текста в консольном выводе. | `com_highlightcolor(string)` |
| [`com_parseutf8`](../38-cvars-reference/04-network-server-cvars.md#com_parseutf8) | Интерпретируйте консольные сообщения/имена игроков/и т. д. как UTF-8. | `com_parseutf8(int)` |
| [`com_protocolname`](../38-cvars-reference/04-network-server-cvars.md#com_protocolname) | Имя протокола игры, используемое для запросов dpmaster. | `com_protocolname(string)` |
| [`com_protocolversion`](../38-cvars-reference/04-network-server-cvars.md#com_protocolversion) | Версия протокола, используемая для запросов dpmaster. | `com_protocolversion(int)` |
| [`drate`](../38-cvars-reference/04-network-server-cvars.md#drate) | Приблизительная оценка пропускной способности, которую можно использовать при загрузке (в байтах в секунду). | `drate(int)` |
| [`fraglog_public`](../38-cvars-reference/04-network-server-cvars.md#fraglog_public) | Включает поддержку запросов фраглогов без установления соединения. | `fraglog_public(int)` |
| [`gl_blacklist_generatemipmap`](../38-cvars-reference/04-network-server-cvars.md#gl_blacklist_generatemipmap) | Самостоятельное создание MIP-карт, вместо того, чтобы позволять это делать графическому драйверу. | `gl_blacklist_generatemipmap(int)` |
| [`host_speeds`](../38-cvars-reference/04-network-server-cvars.md#host_speeds) | Включает покадровый вывод времени работы основных стадий клиентского кадра в консоль. | `host_speeds(int)` |
| [`net_enable_`](../38-cvars-reference/04-network-server-cvars.md#net_enable_) | Если этот параметр включен, принимайте запросы на соединение от механизма quake2-remaster на наших прослушиваемых udp-портах. | `net_enable_(int)` |
| [`net_enable_dtls`](../38-cvars-reference/04-network-server-cvars.md#net_enable_dtls) | Управляет поддержкой dtls на стороне сервера. | `net_enable_dtls(string)` |
| [`net_enable_qizmo`](../38-cvars-reference/04-network-server-cvars.md#net_enable_qizmo) | Включает поддержку на стороне сервера для «connect tcp://foo» или «connect tls://foo» (с net_enable_tls), а также tcp-соединений и совместимых qizmo. | `net_enable_qizmo(int)` |
| [`net_fakeloss`](../38-cvars-reference/04-network-server-cvars.md#net_fakeloss) | Имитирует потерю пакетов при приеме и отправке по шкале от 0 до 1. | `net_fakeloss(int)` |
| [`net_fakemtu`](../38-cvars-reference/04-network-server-cvars.md#net_fakemtu) | Уменьшает размер принимаемых пакетов. | `net_fakemtu(int)` |
| [`net_ice_debug`](../38-cvars-reference/04-network-server-cvars.md#net_ice_debug) | 0: скрыть ненужные детали. | `net_ice_debug(int)` |
| [`net_ice_exchangeprivateips`](../38-cvars-reference/04-network-server-cvars.md#net_ice_exchangeprivateips) | Логическое значение. Если установлено значение 0, частные IP-адреса скрываются от ваших одноранговых узлов — будут доступны только адреса, определенные с другой стороны вашего маршрутизатора. | `net_ice_exchangeprivateips(int)` |
| [`net_ice_usewebrtc`](../38-cvars-reference/04-network-server-cvars.md#net_ice_usewebrtc) | Используйте дополнительные накладные расходы webrtc, а не простой ICE. | `net_ice_usewebrtc(string)` |
| [`net_upnpigp`](../38-cvars-reference/04-network-server-cvars.md#net_upnpigp) | Если установлено, позволяет использовать протокол upnp-igd для пробивания дыр в локальном блоке NAT. | `net_upnpigp(int)` |
| [`pausable`](../38-cvars-reference/04-network-server-cvars.md#pausable) | Управляет тем, разрешена ли команда pause на сервере. | `pausable(string)` |
| [`qtv_password`](../38-cvars-reference/04-network-server-cvars.md#qtv_password) | Переменная задаёт пароль для входящих QTV-подключений к MVD-стриму. | `qtv_password(string)` |
| [`qtv_streamport`](../38-cvars-reference/04-network-server-cvars.md#qtv_streamport) | Устаревший квар. Вместо этого используйте sv_port_tcp. | `qtv_streamport(string)` |
| [`qtvcl_eztvextensions`](../38-cvars-reference/04-network-server-cvars.md#qtvcl_eztvextensions) | Разрешает отправку заголовка QTV_EZQUAKE_EXT при qtvplay-подключении к потоку, выбранному по SOURCE. | `qtvcl_eztvextensions(int)` |
| [`qtvcl_forceversion1`](../38-cvars-reference/04-network-server-cvars.md#qtvcl_forceversion1) | Принудительно переводит начальный запрос QTV на строку VERSION: 1.0 вместо VERSION: 1.1. | `qtvcl_forceversion1(int)` |
| [`r_image_downloadsizelimit`](../38-cvars-reference/04-network-server-cvars.md#r_image_downloadsizelimit) | Максимально допустимый размер файла изображений, загружаемых с URL-адреса в Интернете. 0 полностью отключает, а пустое значение не налагает ограничений. | `r_image_downloadsizelimit(int)` |
| [`rate`](../38-cvars-reference/04-network-server-cvars.md#rate) | Приблизительная оценка пропускной способности, которую можно использовать во время игры. | `rate(int)` |
| [`samelevel`](../38-cvars-reference/04-network-server-cvars.md#samelevel) | Служебный cvar из server info, который общий C-код движка в этой ревизии сам не интерпретирует. | `samelevel(string)` |
| [`sb_showfraglimit`](../38-cvars-reference/04-network-server-cvars.md#sb_showfraglimit) | Показывает в браузере серверов отдельную колонку fraglimit. | `sb_showfraglimit(int)` |
| [`sb_showtimelimit`](../38-cvars-reference/04-network-server-cvars.md#sb_showtimelimit) | Показывает в браузере серверов колонку timelimit. | `sb_showtimelimit(int)` |
| [`skill`](../38-cvars-reference/04-network-server-cvars.md#skill) | Задаёт уровень сложности, который движок учитывает при спавне сущностей на одиночных картах. | `skill(string)` |
| [`spawn`](../38-cvars-reference/04-network-server-cvars.md#spawn) | Служебный cvar серверного управления, который общий код движка почти не использует напрямую. | `spawn(string)` |
| [`sv_aim`](../38-cvars-reference/04-network-server-cvars.md#sv_aim) | Значение должно быть [cos](../37-quakec-builtins-reference/01-math-vector-builtins.md#cos)(угол), где угол — это наибольший допустимый угол, на который можно отклониться от направления, по которому должен был идти выстрел. | `sv_aim(int)` |
| [`sv_autooffload`](../38-cvars-reference/04-network-server-cvars.md#sv_autooffload) | Автоматически запускать сервер в отдельном процессе, чтобы спорадические или постоянные замедления игрового кода не влияли на визуальную частоту кадров (аналогично команде mapcluster). | `sv_autooffload(int)` |
| [`sv_autosave`](../38-cvars-reference/04-network-server-cvars.md#sv_autosave) | Интервал автосохранений, в минутах. | `sv_autosave(int)` |
| [`sv_chatfilter`](../38-cvars-reference/04-network-server-cvars.md#sv_chatfilter) | Включает фильтрацию символов '\r' и '\n' в обычном say и team say. | `sv_chatfilter(int)` |
| [`sv_cheatpc`](../38-cvars-reference/04-network-server-cvars.md#sv_cheatpc) | Если клиент пытался потребовать больше, чем этот процент времени в течение любого периода мошенничества, клиент будет считаться обманщиком. | `sv_cheatpc(int)` |
| [`sv_cheats`](../38-cvars-reference/04-network-server-cvars.md#sv_cheats) | Главный серверный переключатель чит-режима, причём он помечен как CVAR_MAPLATCH и полноценно применяется после перезапуска/загрузки карты. | `sv_cheats(int)` |
| [`sv_cheatspeedchecktime`](../38-cvars-reference/04-network-server-cvars.md#sv_cheatspeedchecktime) | Интервал между каждой проверкой скорости. | `sv_cheatspeedchecktime(int)` |
| [`sv_cmdlikercon`](../38-cvars-reference/04-network-server-cvars.md#sv_cmdlikercon) | Разрешает специальный резервный путь для неизвестных пользовательских команд: если клиент авторизован через рейтинговую систему и код собран с SVRANKING, команда исполняется с уровнем доверия игрока почти как rcon/cmd. | `sv_cmdlikercon(int)` |
| [`sv_compatiblehulls`](../38-cvars-reference/04-network-server-cvars.md#sv_compatiblehulls) | Определяет выбор collision hull для трассировки движения на сервере. | `sv_compatiblehulls(int)` |
| [`sv_csqc_progname`](../38-cvars-reference/04-network-server-cvars.md#sv_csqc_progname) | Задаёт имя файла CSQC-программы, которую сервер пытается загрузить и анонсировать клиентам через *csprogs, *csprogssize и *csprogsname. | `sv_csqc_progname(string)` |
| [`sv_csqcdebug`](../38-cvars-reference/04-network-server-cvars.md#sv_csqcdebug) | Введите информацию о размере пакета для данных, направляемых в csqc. | `sv_csqcdebug(int)` |
| [`sv_demoAutoCompress`](../38-cvars-reference/04-network-server-cvars.md#sv_demoautocompress) | Указывает, следует ли сжимать демо-версии во время их записи. | `sv_demoAutoCompress(string)` |
| [`sv_demoAutoPrefix`](../38-cvars-reference/04-network-server-cvars.md#sv_demoautoprefix) | Префикс автоматически записываемых демок сервера. | `sv_demoAutoPrefix(string)` |
| [`sv_demoAutoRecord`](../38-cvars-reference/04-network-server-cvars.md#sv_demoautorecord) | Если установлено, автоматически записывать демо-версии. -1: запись только при подключении клиента. 1+: запись, когда на сервере находится определенное количество активных игроков (или при подключении к удаленному серверу). | `sv_demoAutoRecord(int)` |
| [`sv_demoCacheSize`](../38-cvars-reference/04-network-server-cvars.md#sv_democachesize) | Переменная задаёт размер буфера для кэшируемой или потоковой записи MVD в память. | `sv_demoCacheSize(string)` |
| [`sv_demoDir`](../38-cvars-reference/04-network-server-cvars.md#sv_demodir) | Переменная задаёт основной каталог для записи, перечисления, удаления и выдачи серверных демок. | `sv_demoDir(string)` |
| [`sv_demoDirAlt`](../38-cvars-reference/04-network-server-cvars.md#sv_demodiralt) | Предоставляет резервное имя каталога для загрузки демо-версии, если sv_demoDir не содержит запрошенную демо-версию. | `sv_demoDirAlt(string)` |
| [`sv_demoExtraNames`](../38-cvars-reference/04-network-server-cvars.md#sv_demoextranames) | Переменная влияет на автогенерацию имён демок в командном teamplay-режиме. | `sv_demoExtraNames(string)` |
| [`sv_demoPings`](../38-cvars-reference/04-network-server-cvars.md#sv_demopings) | Интервал между обновлениями пинга в mvds. | `sv_demoPings(int)` |
| [`sv_demoPrefix`](../38-cvars-reference/04-network-server-cvars.md#sv_demoprefix) | Переменная добавляет общий префикс к базовому имени демки перед расширением. | `sv_demoPrefix(string)` |
| [`sv_demoSuffix`](../38-cvars-reference/04-network-server-cvars.md#sv_demosuffix) | Переменная добавляет общий суффикс к базовому имени демки перед расширением файла. | `sv_demoSuffix(string)` |
| [`sv_demotxt`](../38-cvars-reference/04-network-server-cvars.md#sv_demotxt) | Переменная управляет созданием сопутствующего текстового файла рядом с записываемой демкой. | `sv_demotxt(int)` |
| [`sv_dlURL`](../38-cvars-reference/04-network-server-cvars.md#sv_dlurl) | Предоставляет клиентам внешний URL-адрес, по которому они могут получать pk3s/пакеты с внешнего http-сервера вместо необходимости загрузки через udp. | `sv_dlURL(string)` |
| [`sv_floodprotect`](../38-cvars-reference/04-network-server-cvars.md#sv_floodprotect) | Главный переключатель серверной защиты от флуда для чата и, при отдельной настройке, suicide. | `sv_floodprotect(int)` |
| [`sv_floodprotect_interval`](../38-cvars-reference/04-network-server-cvars.md#sv_floodprotect_interval) | Задаёт длину окна в секундах, внутри которого оценивается частота сообщений. | `sv_floodprotect_interval(int)` |
| [`sv_floodprotect_messages`](../38-cvars-reference/04-network-server-cvars.md#sv_floodprotect_messages) | Определяет, сколько сообщений можно накопить в пределах окна sv_floodprotect_interval до срабатывания блокировки. | `sv_floodprotect_messages(int)` |
| [`sv_floodprotect_sendmessage`](../38-cvars-reference/04-network-server-cvars.md#sv_floodprotect_sendmessage) | Текст, который сервер отправляет игроку в момент срабатывания защиты от флуда. | `sv_floodprotect_sendmessage(string)` |
| [`sv_floodprotect_silencetime`](../38-cvars-reference/04-network-server-cvars.md#sv_floodprotect_silencetime) | Длительность наказания в секундах после превышения лимита защиты от флуда. | `sv_floodprotect_silencetime(int)` |
| [`sv_floodprotect_suicide`](../38-cvars-reference/04-network-server-cvars.md#sv_floodprotect_suicide) | Подключает команду suicide к тому же счётчику защиты от флуда, что и чат. | `sv_floodprotect_suicide(int)` |
| [`sv_ftp`](../38-cvars-reference/04-network-server-cvars.md#sv_ftp) | Переменная включает встроенный FTP-сервер движка. | `sv_ftp(int)` |
| [`sv_ftp_port`](../38-cvars-reference/04-network-server-cvars.md#sv_ftp_port) | Переменная задаёт порт прослушивания встроенного FTP-сервера и передаётся вторым аргументом в `FTP_ServerRun()`. | `sv_ftp_port(int)` |
| [`sv_ftp_port_range`](../38-cvars-reference/04-network-server-cvars.md#sv_ftp_port_range) | Указывает диапазон портов сервера для создания сокетов прослушивания для «активных» ftp-соединений, чтобы обойти проблемы NAT/брандмауэра. | `sv_ftp_port_range(int)` |
| [`sv_fulllevel`](../38-cvars-reference/04-network-server-cvars.md#sv_fulllevel) | Учетные записи пользователей с уровнем доступа выше этого могут писать куда угодно, включая игровой каталог. | `sv_fulllevel(int)` |
| [`sv_fullredirect`](../38-cvars-reference/04-network-server-cvars.md#sv_fullredirect) | Это ip:порт для перенаправления игроков, когда сервер заполнен. | `sv_fullredirect(string)` |
| [`sv_gameplayfix_honest_tracelines`](../38-cvars-reference/04-network-server-cvars.md#sv_gameplayfix_honest_tracelines) | Исправляет старое поведение [traceline](../37-quakec-builtins-reference/03-entity-world-builtins.md#traceline)/[tracebox](../37-quakec-builtins-reference/03-entity-world-builtins.md#tracebox), когда трасса, начавшаяся внутри solid, могла вернуть trace_fraction = 1 и выглядеть как полностью свободная. | `sv_gameplayfix_honest_tracelines(int)` |
| [`sv_gameplayfix_radialmaxvelocity`](../38-cvars-reference/04-network-server-cvars.md#sv_gameplayfix_radialmaxvelocity) | Применяет максимальную скорость радиально, а не аксиально. | `sv_gameplayfix_radialmaxvelocity(int)` |
| [`sv_gameplayfix_setmodelrealbox`](../38-cvars-reference/04-network-server-cvars.md#sv_gameplayfix_setmodelrealbox) | Vanilla [setmodel](../37-quakec-builtins-reference/03-entity-world-builtins.md#setmodel) установит для объекта жестко запрограммированный размер для моделей, отличных от BSP. | `sv_gameplayfix_setmodelrealbox(int)` |
| [`sv_gameplayfix_setmodelsize_qw`](../38-cvars-reference/04-network-server-cvars.md#sv_gameplayfix_setmodelsize_qw) | Встроенная функция setmodel также будет действовать как размер набора для модов QuakeWorld. | `sv_gameplayfix_setmodelsize_qw(int)` |
| [`sv_gameplayfix_spawnbeforethinks`](../38-cvars-reference/04-network-server-cvars.md#sv_gameplayfix_spawnbeforethinks) | Исправлена ​​проблема, из-за которой игрок думал, что (включая Pre+Post) можно вызвать перед [PutClientInServer](../37-quakec-builtins-reference/00-entry-points.md#putclientinserver). | `sv_gameplayfix_spawnbeforethinks(int)` |
| [`sv_gameplayfix_stepdown`](../38-cvars-reference/04-network-server-cvars.md#sv_gameplayfix_stepdown) | Попытайтесь спускаться по ступенькам, а не только подниматься по ним. | `sv_gameplayfix_stepdown(int)` |
| [`sv_gamespeed`](../38-cvars-reference/04-network-server-cvars.md#sv_gamespeed) | Масштабирует ход серверного времени и тем самым замедляет или ускоряет весь игровой симулятор. | `sv_gamespeed(int)` |
| [`sv_getrealip`](../38-cvars-reference/04-network-server-cvars.md#sv_getrealip) | Попытайтесь получить более надежный IP-адрес для клиентов, а не только их прокси. | `sv_getrealip(int)` |
| [`sv_hideinactivegames`](../38-cvars-reference/04-network-server-cvars.md#sv_hideinactivegames) | Не показывать известные игры, у которых на данный момент нет серверов, в html-списках. | `sv_hideinactivegames(int)` |
| [`sv_highchars`](../38-cvars-reference/04-network-server-cvars.md#sv_highchars) | Разрешает ли сервер принимать в именах символы из второго набора шрифта. | `sv_highchars(int)` |
| [`sv_http`](../38-cvars-reference/04-network-server-cvars.md#sv_http) | Переменная включает встроенный HTTP-сервер движка. | `sv_http(int)` |
| [`sv_http_port`](../38-cvars-reference/04-network-server-cvars.md#sv_http_port) | Переменная задаёт TCP-порт встроенного HTTP-сервера. | `sv_http_port(int)` |
| [`sv_listen_q3`](../38-cvars-reference/04-network-server-cvars.md#sv_listen_q3) | Включает обработку Quake 3-совместимых подключений и пакетов QW-over-Q3. | `sv_listen_q3(int)` |
| [`sv_listen_qw`](../38-cvars-reference/04-network-server-cvars.md#sv_listen_qw) | Указывает, разрешено ли подключение обычным клиентам. | `sv_listen_qw(int)` |
| [`sv_loadentfiles`](../38-cvars-reference/04-network-server-cvars.md#sv_loadentfiles) | Разрешает подмену встроенного entity lump карты внешним файлом. | `sv_loadentfiles(int)` |
| [`sv_mapcheck`](../38-cvars-reference/04-network-server-cvars.md#sv_mapcheck) | Проверяет, совпадает ли контрольная сумма карты у клиента с серверной worldmodel во время этапа prespawn. | `sv_mapcheck(int)` |
| [`sv_master`](../38-cvars-reference/04-network-server-cvars.md#sv_master) | Переводит процесс в режим встроенного master server, если сборка собрана с поддержкой SV_MASTER. | `sv_master(int)` |
| [`sv_masterport`](../38-cvars-reference/04-network-server-cvars.md#sv_masterport) | Задаёт UDP-порт или список UDP-портов, которые master-сервер открывает для приёма heartbeat/serverinfo-пакетов и запросов клиентов. | `sv_masterport(string)` |
| [`sv_masterport_tcp`](../38-cvars-reference/04-network-server-cvars.md#sv_masterport_tcp) | Задаёт TCP-порт или список TCP-портов, которые master-сервер открывает для stream-подключений. | `sv_masterport_tcp(string)` |
| [`sv_maxaim`](../38-cvars-reference/04-network-server-cvars.md#sv_maxaim) | Максимально допустимый угол автоприцеливания. | `sv_maxaim(int)` |
| [`sv_nopvs`](../38-cvars-reference/04-network-server-cvars.md#sv_nopvs) | Установите значение 1, чтобы игнорировать pvs на сервере. | `sv_nopvs(int)` |
| [`sv_phs`](../38-cvars-reference/04-network-server-cvars.md#sv_phs) | Если 1, не используйте физ. | `sv_phs(int)` |
| [`sv_ping_ignorepl`](../38-cvars-reference/04-network-server-cvars.md#sv_ping_ignorepl) | Если установлено значение 1, время пинга, сообщаемое для игроков, будет игнорировать влияние потери пакетов на время пинга. 0 немного честнее, но менее полезен для диагностики соединения. | `sv_ping_ignorepl(int)` |
| [`sv_playermodelchecks`](../38-cvars-reference/04-network-server-cvars.md#sv_playermodelchecks) | Сверяет userinfo-поля pmodel и emodel с контрольными суммами серверных моделей player и eyes. | `sv_playermodelchecks(int)` |
| [`sv_port`](../38-cvars-reference/04-network-server-cvars.md#sv_port) | Задаёт UDP-порт или список портов, на которых сервер слушает входящие IPv4-подключения. | `sv_port(string)` |
| [`sv_port_ipv6`](../38-cvars-reference/04-network-server-cvars.md#sv_port_ipv6) | Порт, который будет использоваться для входящих соединений ipv6 udp. | `sv_port_ipv6(string)` |
| [`sv_port_ipx`](../38-cvars-reference/04-network-server-cvars.md#sv_port_ipx) | Если вам нужен этот параметр, значит, вы делаете что-то странное/древнее. | `sv_port_ipx(string)` |
| [`sv_port_natpmp`](../38-cvars-reference/04-network-server-cvars.md#sv_port_natpmp) | Включает попытку автоматической настройки проброса порта через NAT-PMP. | `sv_port_natpmp(string)` |
| [`sv_port_rtc`](../38-cvars-reference/04-network-server-cvars.md#sv_port_rtc) | Здесь указывается URL-адрес брокера, с которого можно получать клиентов. | `sv_port_rtc(string)` |
| [`sv_port_tcp`](../38-cvars-reference/04-network-server-cvars.md#sv_port_tcp) | Номер порта для входящих TCP-соединений (включая варианты tls). | `sv_port_tcp(string)` |
| [`sv_port_tcp6`](../38-cvars-reference/04-network-server-cvars.md#sv_port_tcp6) | Эквивалент sv_port_tcp, но предполагается, что номера портов — только ipv6. | `sv_port_tcp6(string)` |
| [`sv_port_unix`](../38-cvars-reference/04-network-server-cvars.md#sv_port_unix) | Задаёт адрес Unix-domain datagram socket для локальной связи: на Linux по умолчанию используется абстрактный адрес @qsock.fte, на других Unix-платформах — файловый путь вроде /tmp/qsock.fte. | `sv_port_unix(string)` |
| [`sv_progs`](../38-cvars-reference/04-network-server-cvars.md#sv_progs) | Указывает имя файла игрового кода, который будет использоваться на стороне сервера. | `sv_progs(string)` |
| [`sv_protocol_nq`](../38-cvars-reference/04-network-server-cvars.md#sv_protocol_nq) | Указывает протокол по умолчанию, который будет использоваться для новых клиентов NQ. | `sv_protocol_nq(string)` |
| [`sv_pupglow`](../38-cvars-reference/04-network-server-cvars.md#sv_pupglow) | Инструктирует клиентов включить импульсное включение питания в стиле hexen2. | `sv_pupglow(string)` |
| [`sv_pure`](../38-cvars-reference/04-network-server-cvars.md#sv_pure) | Самая злая переменная в мире, многие клиенты игнорируют ее. 0=стандартные правила Quake. 1=клиенты должны отдавать предпочтение файлам в пакетах, присутствующих на сервере. 2=клиенты должны использовать *только* файлы в пакетах, присутствующих на сервере. | `sv_pure(string)` |
| [`sv_readlevel`](../38-cvars-reference/04-network-server-cvars.md#sv_readlevel) | Переменная задаёт минимальный `trustlevel`, необходимый для read-only доступа к встроенным HTTP или FTP сервисам. | `sv_readlevel(int)` |
| [`sv_realip_kick`](../38-cvars-reference/04-network-server-cvars.md#sv_realip_kick) | Кикает клиентов, если их реалип не может быть проверен на уровне, указанном в sv_getrealip. | `sv_realip_kick(int)` |
| [`sv_realip_timeout`](../38-cvars-reference/04-network-server-cvars.md#sv_realip_timeout) | Лимит времени в секундах на подтверждение real IP клиента через механизм sv_getrealip. | `sv_realip_timeout(int)` |
| [`sv_realiphostname_ipv4`](../38-cvars-reference/04-network-server-cvars.md#sv_realiphostname_ipv4) | Это общедоступный IP-адрес:порт сервера. | `sv_realiphostname_ipv4(string)` |
| [`sv_realiphostname_ipv6`](../38-cvars-reference/04-network-server-cvars.md#sv_realiphostname_ipv6) | Это общедоступный IP-адрес:порт сервера. | `sv_realiphostname_ipv6(string)` |
| [`sv_resetparms`](../38-cvars-reference/04-network-server-cvars.md#sv_resetparms) | Управляет тем, сбрасывать ли spawn parms у уже известных серверу игроков. | `sv_resetparms(int)` |
| [`sv_savefmt`](../38-cvars-reference/04-network-server-cvars.md#sv_savefmt) | Указывает формат сохраненной игры. | `sv_savefmt(string)` |
| [`sv_showconnectionlessmessages`](../38-cvars-reference/04-network-server-cvars.md#sv_showconnectionlessmessages) | Отобразите строку, описывающую каждое сообщение без установления соединения, поступающее на сервер. | `sv_showconnectionlessmessages(int)` |
| [`sv_showpredloss`](../38-cvars-reference/04-network-server-cvars.md#sv_showpredloss) | Печатайте сообщения всякий раз, когда входные кадры игнорируются или форсируются на стороне сервера, чтобы предотвратить использование чит-кодов или читов при наведении курсора. | `sv_showpredloss(int)` |
| [`sv_sortlist`](../38-cvars-reference/04-network-server-cvars.md#sv_sortlist) | Управляет сортировкой вывода http: **0:** не беспокойтесь; **1:** клиенты, затем адрес; **2:** имя хоста, затем адрес. | `sv_sortlist(int)` |
| [`sv_specprint`](../38-cvars-reference/04-network-server-cvars.md#sv_specprint) | Битовое поле, которое контролирует, какие события игрока видят зрители при отслеживании этого игрока. &1: зрители будут видеть центральные отпечатки. &2: зрители будут видеть спринты (сообщения о пикапе и т. д.). &4: зрители будут получать консольные команды, это потенциально рискованно. | `sv_specprint(int)` |
| [`sv_spectalk`](../38-cvars-reference/04-network-server-cvars.md#sv_spectalk) | Определяет, могут ли наблюдатели общаться с обычными игроками в общих каналах. | `sv_spectalk(int)` |
| [`sv_sql_defaultdb`](../38-cvars-reference/04-network-server-cvars.md#sv_sql_defaultdb) | Переменная служит значением по умолчанию для четвёртого параметра подключения в QC builtin [`sqlconnect`](../37-quakec-builtins-reference/06-files-database-builtins.md#sqlconnect), если мод его не передал. | `sv_sql_defaultdb(string)` |
| [`sv_sql_driver`](../38-cvars-reference/04-network-server-cvars.md#sv_sql_driver) | Переменная задаёт драйвер SQL по умолчанию для `sqlconnect`, когда мод не указал его явно. | `sv_sql_driver(string)` |
| [`sv_sql_host`](../38-cvars-reference/04-network-server-cvars.md#sv_sql_host) | Переменная служит значением по умолчанию для первого параметра подключения в `sqlconnect`. | `sv_sql_host(string)` |
| [`sv_sql_password`](../38-cvars-reference/04-network-server-cvars.md#sv_sql_password) | Переменная служит значением по умолчанию для третьего параметра подключения в QC builtin `sqlconnect`, если мод не передал пароль явно. | `sv_sql_password(string)` |
| [`sv_sql_username`](../38-cvars-reference/04-network-server-cvars.md#sv_sql_username) | Переменная служит значением по умолчанию для второго параметра подключения в QC builtin `sqlconnect`. | `sv_sql_username(string)` |
| [`sv_userinfo_bytelimit`](../38-cvars-reference/04-network-server-cvars.md#sv_userinfo_bytelimit) | Это максимальное количество байтов, которое может храниться в информации пользователя каждого пользователя. | `sv_userinfo_bytelimit(int)` |
| [`sv_userinfo_keylimit`](../38-cvars-reference/04-network-server-cvars.md#sv_userinfo_keylimit) | Это максимальное количество ключей пользовательской информации, которые может создать каждый пользователь. | `sv_userinfo_keylimit(int)` |
| [`sv_writelevel`](../38-cvars-reference/04-network-server-cvars.md#sv_writelevel) | Указывает требуемый уровень доверия, при котором учетные записи пользователей могут записывать данные в специфичный для пользователя подкаталог /uploads/USERNAME/*. Если пусто, то загрузка не разрешена. | `sv_writelevel(int)` |
| [`vK_khr_fragment_shading_rate`](../38-cvars-reference/04-network-server-cvars.md#vk_khr_fragment_shading_rate) | Позволяет использовать переменную скорость затенения. | `vK_khr_fragment_shading_rate(string)` |

### Физика и игровой процесс

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`cl_anglespeedkey`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_anglespeedkey) | Задаёт множитель для скоростей поворота, когда активен `+speed`: он умножает базовые `cl_yawspeed`, `cl_pitchspeed` и `cl_rollspeed`. | `cl_anglespeedkey(float)` |
| [`cl_backspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_backspeed) | Определяет базовую скорость движения назад. | `cl_backspeed(float/string)` |
| [`cl_fastaccel`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_fastaccel) | Включает мгновенный выход клиентской команды на полную заданную скорость вместо короткого «разгона» длиной примерно в кадр. | `cl_fastaccel(boolean/int)` |
| [`cl_forwardspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_forwardspeed) | Задаёт базовую величину команды движения вперёд, которую клиент отправляет серверу. | `cl_forwardspeed(float)` |
| [`cl_iDrive`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_idrive) | Полу-читерская (`CVAR_SEMICHEAT`) настройка, которая при нажатии противоположной кнопки движения эффективно отпускает уже зажатую. | `cl_iDrive(boolean/int)` |
| [`cl_instantrotate`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_instantrotate) | Управляет только накопленным `in_rotate`-поворотом: при `1` такой импульс применяется сразу целиком, а при `0` расходуется по времени кадра и дополнительно масштабируется через `cl_anglespeedkey`. | `cl_instantrotate(boolean/int)` |
| [`cl_lerp_driftbias`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_lerp_driftbias) | Добавляет смещение к точке, вокруг которой клиент удерживает интерполяцию и дрейф времени. | `cl_lerp_driftbias(float)` |
| [`cl_lerp_driftfrac`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_lerp_driftfrac) | Определяет, насколько охотно интерполяция тянется к более свежему времени вместо более старого, стабильного. | `cl_lerp_driftfrac(float)` |
| [`cl_lerp_smooth`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_lerp_smooth) | Выбирает стратегию сглаживания интерполяции. | `cl_lerp_smooth(int)` |
| [`cl_movement`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_movement) | Указывает, отправлять ли последовательность кадров движения по DPP7-совместимому пути. | `cl_movement(boolean/int)` |
| [`cl_nopred`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_nopred) | Полностью отключает клиентское предсказание собственного движения. | `cl_nopred(boolean/int)` |
| [`cl_pitchspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_pitchspeed) | Задаёт базовую скорость изменения pitch для клавиатурного обзора: [`+lookup`](../44-cli-commands-reference/02-client-ui-commands.md#lookup), [`+lookdown`](../44-cli-commands-reference/02-client-ui-commands.md#lookdown) и режима `+klook`. | `cl_pitchspeed(float)` |
| [`cl_predict_extrapolate`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_predict_extrapolate) | Управляет тем, живёт ли локальное предсказание строго на завершённых кадрах ввода или разрешает экстраполяцию по частично собранным кадрам. | `cl_predict_extrapolate(int/string)` |
| [`cl_predict_timenudge`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_predict_timenudge) | Отладочная поправка ко времени локального предсказания в секундах. | `cl_predict_timenudge(float)` |
| [`cl_rollangle`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_rollangle) | Определяет максимальный визуальный наклон камеры при стрейфе. | `cl_rollangle(float)` |
| [`cl_rollspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_rollspeed) | Задаёт скорость бокового движения, при которой камера достигает максимума из `cl_rollangle`. | `cl_rollspeed(float)` |
| [`cl_run`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_run) | Включает autorun, то есть инвертирует смысл клавиши `+speed`. | `cl_run(boolean/int)` |
| [`cl_sidespeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_sidespeed) | Определяет базовую величину бокового движения, которую клиент отправляет в usercmd при стрейфе. | `cl_sidespeed(float)` |
| [`cl_smartjump`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_smartjump) | Делает кнопку прыжка контекстной: в воде она работает как [`+moveup`](../44-cli-commands-reference/02-client-ui-commands.md#moveup), а на суше остаётся обычным jump. | `cl_smartjump(boolean/int)` |
| [`cl_upspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_upspeed) | Переменная объявлена и архивируется, но в текущем `CL_BaseMove` вертикальная компонента `upmove` берётся не из `cl_upspeed`, а из `cl_backspeed` либо `cl_forwardspeed` (если `cl_backspeed` пуст). | `cl_upspeed(float)` |
| [`cl_yawspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_yawspeed) | Базовая скорость клавиатурного разворота по yaw для `+left` и `+right`. | `cl_yawspeed(float)` |
| [`pm_airstep`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_airstep) | Разрешает игроку «шагать» вверх по ступеням даже в прыжке, а не только при обычном наземном движении. | `pm_airstep(boolean/int/string)` |
| [`pm_autobunny`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_autobunny) | Позволяет продолжать прыгать без отпускания кнопки jump. | `pm_autobunny(boolean/int/string)` |
| [`pm_bunnyfriction`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_bunnyfriction) | Форсирует хотя бы один кадр трения при прыжке, имитируя характерную NetQuake-физику. | `pm_bunnyfriction(boolean/int/string)` |
| [`pm_bunnyspeedcap`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_bunnyspeedcap) | Добавляет верхнюю границу на горизонтальную скорость при повороте в воздухе: если игрок уже быстрее лимита, поворот аккуратно стягивает скорость к `sv_maxspeed * pm_bunnyspeedcap`. | `pm_bunnyspeedcap(float/string)` |
| [`pm_edgefriction`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_edgefriction) | Увеличивает трение, когда игрок почти выходит с края уступа, чтобы случайно не срываться вниз. | `pm_edgefriction(float/string)` |
| [`pm_flyfriction`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_flyfriction) | Трение для режимов полёта и 6DoF-перемещения. | `pm_flyfriction(float/string)` |
| [`pm_ktjump`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_ktjump) | Корректирует вертикальную составляющую прыжка: при положительных значениях движок подмешивает стандартную jump-скорость к текущей вертикальной скорости, а всё, что выше `1`, зажимается к `1`. | `pm_ktjump(float/string)` |
| [`pm_pground`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_pground) | Включает «постоянное» состояние onground вместо полного пересчёта каждый кадр. | `pm_pground(boolean/int/string)` |
| [`pm_slidefix`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_slidefix) | Исправляет старую проблему движения вниз по склонам и ступеням, из-за которой поверхность ощущалась как серия жёстких ступенек, а не как цельный уклон. | `pm_slidefix(boolean/int/string)` |
| [`pm_slidyslopes`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_slidyslopes) | Воспроизводит NetQuake-стиль, при котором игрок медленно съезжает вниз по наклонным поверхностям. | `pm_slidyslopes(boolean/int/string)` |
| [`pm_stepdown`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_stepdown) | Заставляет физику активнее «держаться» за землю при движении вниз по ступеням, а не терять контакт на каждом уступе. | `pm_stepdown(boolean/int/string)` |
| [`pm_walljump`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_walljump) | Разрешает отталкивание от стен в воздухе. | `pm_walljump(int/string)` |
| [`pm_watersinkspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_watersinkspeed) | Задаёт скорость пассивного погружения, когда игрок находится в воде и не даёт команд движения. | `pm_watersinkspeed(float/string)` |
| [`pushlatency`](../38-cvars-reference/05-physics-gameplay-cvars.md#pushlatency) | Старый клиентский сдвиг времени для пути предсказания, задаваемый в миллисекундах. | `pushlatency(float)` |
| [`sv_accelerate`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_accelerate) | Главный серверный коэффициент наземного разгона игрока. | `sv_accelerate(float)` |
| [`sv_airaccelerate`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_airaccelerate) | Определяет, насколько сильно игрок может менять горизонтальную скорость, уже находясь в воздухе. | `sv_airaccelerate(float)` |
| [`sv_antilag`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_antilag) | Включает лагокомпенсацию попаданий через backdate-трассировку. | `sv_antilag(int/string)` |
| [`sv_antilag_frac`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_antilag_frac) | Тонкая настройка силы lag-comp rewind. | `sv_antilag_frac(float/string)` |
| [`sv_brokenmovetypes`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_brokenmovetypes) | Эмулирует ванильный QuakeWorld, насильно переводя всех игроков в `MOVETYPE_WALK`. | `sv_brokenmovetypes(boolean/int)` |
| [`sv_friction`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_friction) | Базовое наземное трение сервера. | `sv_friction(float)` |
| [`sv_gameplayfix_blowupfallenzombies`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_blowupfallenzombies) | Разрешает [`findradius`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findradius) находить non-solid сущности, которые старые моды нередко считали «невидимыми» для такого поиска. | `sv_gameplayfix_blowupfallenzombies(boolean/int)` |
| [`sv_gameplayfix_droptofloorstartsolid`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_droptofloorstartsolid) | Чинит ситуацию, когда [`droptofloor`](../37-quakec-builtins-reference/03-entity-world-builtins.md#droptofloor) проваливается из-за старта в solid: при `1` движок делает вторую попытку, но уже через [traceline](../37-quakec-builtins-reference/03-entity-world-builtins.md#traceline)-путь. | `sv_gameplayfix_droptofloorstartsolid(boolean/int)` |
| [`sv_gameplayfix_findradiusdistancetobox`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_findradiusdistancetobox) | Меняет метрику [`findradius`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findradius): вместо проверки дистанции только до origin сущности движок начинает смотреть до ближайшей точки её bounding box. | `sv_gameplayfix_findradiusdistancetobox(boolean/int)` |
| [`sv_gameplayfix_grenadebouncedownslopes`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_grenadebouncedownslopes) | Исправляет расчёт отскока `MOVETYPE_BOUNCE` на уклонах: скорость отражается относительно поверхности, а не относительно чистой вертикали. | `sv_gameplayfix_grenadebouncedownslopes(boolean/int)` |
| [`sv_gameplayfix_multiplethinks`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_multiplethinks) | Разрешает вызывать несколько think на сущность в пределах одного серверного кадра, если у неё очень маленькие интервалы `nextthink`. | `sv_gameplayfix_multiplethinks(boolean/int)` |
| [`sv_gameplayfix_noairborncorpse`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_noairborncorpse) | Исправляет старую ситуацию, когда труп или другой объект мог продолжать «стоять в воздухе», если опора под ним уже исчезла. | `sv_gameplayfix_noairborncorpse(boolean/int)` |
| [`sv_gameplayfix_nolinknonsolid`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_nolinknonsolid) | Управляет тем, будут ли несолидные сущности всё равно линковаться в collision-ноды ради корректной смены `.solid` и лучшей совместимости с DP-стилем модов. | `sv_gameplayfix_nolinknonsolid(boolean/int)` |
| [`sv_gameplayfix_trappedwithin`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_trappedwithin) | Защищает от случая, когда сущность уже оказалась внутри другой и из-за погрешности BSP может полностью протиснуться сквозь мир или препятствие. | `sv_gameplayfix_trappedwithin(boolean/int)` |
| [`sv_gravity`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gravity) | Базовая гравитация сервера, вокруг которой строятся прыжки, падение и большая часть привычного ощущения «веса». Повышение значения делает мир тяжелее и укорачивает дуги прыжков, снижение даёт более парящую физику; отдельные сущности и моды могут дополнительно масштабировать её своим множителем, но отправной точкой всё равно остаётся именно `sv_gravity`. | `sv_gravity(float)` |
| [`sv_maxspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_maxspeed) | Основной серверный предел желаемой скорости игрока. | `sv_maxspeed(float)` |
| [`sv_maxvelocity`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_maxvelocity) | Жёсткий верхний предел на скорость сущностей, нужен как страховка от физического «взрыва», слишком больших импульсов и численной нестабильности. | `sv_maxvelocity(float)` |
| [`sv_nqplayerphysics`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_nqplayerphysics) | Переключает сервер между QuakeWorld-подобной предсказуемой физикой и NetQuake-подобной логикой совместимости, причём значение `auto` заставляет callback движка выбирать режим по типу игры, моду и наличию кастомной physics-функции. | `sv_nqplayerphysics(string/int)` |
| [`sv_pushplayers`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_pushplayers) | Задаёт силу, с которой один игрок передаёт импульс другому при физическом контакте. | `sv_pushplayers(float)` |
| [`sv_spectatormaxspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_spectatormaxspeed) | Максимальная скорость для spectator/free-fly наблюдателя. | `sv_spectatormaxspeed(float)` |
| [`sv_stepheight`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_stepheight) | Определяет высоту ступени, на которую игрок может автоматически зашагнуть вверх или вниз. | `sv_stepheight(float/string)` |
| [`sv_stopspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_stopspeed) | Минимальная контрольная скорость, от которой сервер считает торможение трением. | `sv_stopspeed(float)` |
| [`sv_wallfriction`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_wallfriction) | Дополнительное трение при упоре в стену во время step-slide-движения. | `sv_wallfriction(float)` |
| [`sv_wateraccelerate`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_wateraccelerate) | Серверный коэффициент разгона в воде. | `sv_wateraccelerate(float)` |
| [`sv_waterfriction`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_waterfriction) | Отвечает за то, как быстро вода гасит уже набранную скорость. | `sv_waterfriction(float)` |
| [`chase_active`](../38-cvars-reference/05-physics-gameplay-cvars.md#chase_active) | Включает вид от третьего лица (chase camera): при значении `1` и разрешённых читах (`cls.allow_cheats`, то есть [`sv_cheats 1`](../38-cvars-reference/04-network-server-cvars.md#sv_cheats) либо синглплеер/локальный сервер) камера отодвигается от игрока на расстояние, заданное `chase_back` и `chase_up`, показывая модель самого игрока со стороны, а не из глаз. | `chase_active(int)` |
| [`chase_back`](../38-cvars-reference/05-physics-gameplay-cvars.md#chase_back) | Задаёт смещение chase-камеры назад относительно направления взгляда. | `chase_back(int)` |
| [`chase_right`](../38-cvars-reference/05-physics-gameplay-cvars.md#chase_right) | Задаёт боковое смещение chase-камеры вдоль right-вектора. | `chase_right(int)` |
| [`chase_up`](../38-cvars-reference/05-physics-gameplay-cvars.md#chase_up) | Задаёт вертикальное смещение chase-камеры. | `chase_up(int)` |
| [`cl_bob`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_bob) | Управляет тем, насколько положение камеры должно колебаться вверх и вниз, когда игрок бегает. | `cl_bob(float)` |
| [`cl_bobcycle`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_bobcycle) | Определяет длительность одного цикла ходовой раскачки камеры. | `cl_bobcycle(float)` |
| [`cl_bobmodel`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_bobmodel) | Управляет тем, должна ли модель просмотра подпрыгивать вверх и вниз, когда игрок бегает. | `cl_bobmodel(int)` |
| [`cl_bobmodel_side`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_bobmodel_side) | Задаёт масштаб бокового смещения viewmodel при bob-анимации оружия. | `cl_bobmodel_side(float)` |
| [`cl_bobmodel_speed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_bobmodel_speed) | Задаёт частотный множитель для bob-анимации viewmodel. | `cl_bobmodel_speed(int)` |
| [`cl_bobmodel_up`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_bobmodel_up) | Задаёт масштаб вертикального смещения viewmodel при bob-анимации оружия. | `cl_bobmodel_up(float)` |
| [`cl_bobup`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_bobup) | Определяет долю периода cl_bobcycle, приходящуюся на первую половину волны bob-эффекта. | `cl_bobup(float)` |
| [`cl_predict_players`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_predict_players) | Очистите эту переменную, чтобы увидеть, как именно находятся объекты на сервере. | `cl_predict_players(int)` |
| [`pm_noround`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_noround) | Отключает clientside snapping/округление координат предсказанного игрока. | `pm_noround(int)` |
| [`pm_stepheight`](../38-cvars-reference/05-physics-gameplay-cvars.md#pm_stepheight) | Это основное имя того же serverinfo-cvar, который DarkPlaces-совместимо виден и как `sv_stepheight`. | `pm_stepheight(string)` |
| [`temp1`](../38-cvars-reference/05-physics-gameplay-cvars.md#temp1) | Свободный служебный cvar из группы progs. | `temp1(int)` |

### Интерфейс, консоль и управление

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`cl_anglespeedkey`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_anglespeedkey) | Множитель клавишной скорости поворота, который при удержании `+speed` масштабирует не только [`cl_yawspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_yawspeed) и [`cl_pitchspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_pitchspeed), но и [`cl_rollspeed`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_rollspeed). | `cl_anglespeedkey(float)` |
| [`cl_backspeed`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_backspeed) | Базовая скорость движения назад. | `cl_backspeed(float/empty string)` |
| [`cl_chatmode`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_chatmode) | Определяет, как строка ввода интерпретируется при нажатии Enter. | `cl_chatmode(int 0-2)` |
| [`cl_clock`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_clock) | Включает экранные часы реального времени. | `cl_clock(int 0-2)` |
| [`cl_fastaccel`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_fastaccel) | Управляет тем, как быстро цифровые клавиши движения выходят на полную величину usercmd. | `cl_fastaccel(boolean)` |
| [`cl_forwardspeed`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_forwardspeed) | Задаёт базовую скорость движения вперёд для клавишного ввода. | `cl_forwardspeed(float)` |
| [`cl_gameclock`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_gameclock) | Показывает игровой таймер матча. | `cl_gameclock(int 0-2)` |
| [`cl_iDrive`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_idrive) | Заставляет противоположную клавишу движения автоматически подавляться, когда вы нажимаете её антипод — например, [`+forward`](../44-cli-commands-reference/02-client-ui-commands.md#forward) против [`+back`](../44-cli-commands-reference/02-client-ui-commands.md#back). | `cl_iDrive(boolean)` |
| [`cl_instantrotate`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_instantrotate) | Определяет, как расходуется накопленный `in_rotate`-поворот: при `1` такой импульс применяется сразу, а при `0` размазывается по времени кадра и масштабируется `cl_anglespeedkey`. | `cl_instantrotate(boolean)` |
| [`cl_keypad`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_keypad) | Для Windows-клавиатуры управляет различием между цифровым блоком и основными клавишами навигации. | `cl_keypad(boolean)` |
| [`cl_movespeedkey`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_movespeedkey) | Множитель скорости движения, который включается через `+speed` и взаимодействует с [`cl_run`](../38-cvars-reference/05-physics-gameplay-cvars.md#cl_run). | `cl_movespeedkey(float)` |
| [`cl_pitchspeed`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_pitchspeed) | Базовая скорость изменения pitch при клавишном взгляде вверх и вниз. | `cl_pitchspeed(float)` |
| [`cl_run`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_run) | Включает autorun и инвертирует смысл клавиши `+speed`. | `cl_run(boolean)` |
| [`cl_sendchatstate`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_sendchatstate) | Разрешает клиенту сообщать серверу, что игрок находится в консоли, окне чата или состоянии AFK. | `cl_sendchatstate(boolean)` |
| [`cl_sidespeed`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_sidespeed) | Базовая скорость бокового движения для strafe-команд. | `cl_sidespeed(float)` |
| [`cl_smartjump`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_smartjump) | Переключает “умный” прыжок: в воде кнопка прыжка отправляет [`+moveup`](../44-cli-commands-reference/02-client-ui-commands.md#moveup) вместо обычного прыжка. | `cl_smartjump(boolean)` |
| [`cl_upspeed`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_upspeed) | Переменная объявлена и архивируется, но в текущем `CL_BaseMove` фактический `upmove` считается не из `cl_upspeed`, а из `cl_backspeed` либо `cl_forwardspeed`, если `cl_backspeed` пуст. | `cl_upspeed(float)` |
| [`cl_yawspeed`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_yawspeed) | Базовая скорость клавишного поворота по yaw. | `cl_yawspeed(float)` |
| [`con_centernotify`](../38-cvars-reference/06-ui-console-input-cvars.md#con_centernotify) | Управляет выравниванием уведомлений notify-ленты. | `con_centernotify(boolean)` |
| [`con_displaypossibilities`](../38-cvars-reference/06-ui-console-input-cvars.md#con_displaypossibilities) | Показывает список всех совпадений автодополнения под строкой ввода. | `con_displaypossibilities(boolean)` |
| [`con_echochat`](../38-cvars-reference/06-ui-console-input-cvars.md#con_echochat) | Определяет, будет ли отправленное вами чат-сообщение дополнительно печататься обратно в консольный буфер при подтверждении ввода. | `con_echochat(boolean)` |
| [`con_maxlines`](../38-cvars-reference/06-ui-console-input-cvars.md#con_maxlines) | Максимальный размер scrollback-буфера консоли в строках. | `con_maxlines(int)` |
| [`con_notify_w`](../38-cvars-reference/06-ui-console-input-cvars.md#con_notify_w) | Ширина области notify как доля доступной ширины экрана. | `con_notify_w(float)` |
| [`con_notify_x`](../38-cvars-reference/06-ui-console-input-cvars.md#con_notify_x) | Горизонтальное смещение notify-области. | `con_notify_x(float)` |
| [`con_notify_y`](../38-cvars-reference/06-ui-console-input-cvars.md#con_notify_y) | Вертикальное смещение notify-области. | `con_notify_y(float)` |
| [`con_notifylines`](../38-cvars-reference/06-ui-console-input-cvars.md#con_notifylines) | Количество строк обычной notify-ленты, показываемых поверх игры. | `con_notifylines(int)` |
| [`con_notifytime`](../38-cvars-reference/06-ui-console-input-cvars.md#con_notifytime) | Сколько секунд обычные notify-сообщения остаются на экране. | `con_notifytime(float)` |
| [`con_notifytime_chat`](../38-cvars-reference/06-ui-console-input-cvars.md#con_notifytime_chat) | Время жизни chat-notify-строк в секундах при использовании отдельного чата. | `con_notifytime_chat(float)` |
| [`con_numnotifylines_chat`](../38-cvars-reference/06-ui-console-input-cvars.md#con_numnotifylines_chat) | Количество строк в отдельной chat-notify-ленте, когда чат отделён от обычной консоли. | `con_numnotifylines_chat(int)` |
| [`con_savehistory`](../38-cvars-reference/06-ui-console-input-cvars.md#con_savehistory) | Разрешает запись и обновление `conhistory.txt`, чтобы история введённых команд переживала перезапуск клиента. | `con_savehistory(boolean)` |
| [`con_separatechat`](../38-cvars-reference/06-ui-console-input-cvars.md#con_separatechat) | Разделяет чат и обычную консоль на разные поверхности вывода. | `con_separatechat(boolean)` |
| [`con_showcompletion`](../38-cvars-reference/06-ui-console-input-cvars.md#con_showcompletion) | Включает встроенную подсказку автодополнения прямо в строке ввода, когда движок знает единственный или основной вариант продолжения. | `con_showcompletion(boolean)` |
| [`con_stayhidden`](../38-cvars-reference/06-ui-console-input-cvars.md#con_stayhidden) | Контролирует, может ли консоль автоматически всплывать в неигровых состояниях. | `con_stayhidden(int 0-3)` |
| [`con_textsize`](../38-cvars-reference/06-ui-console-input-cvars.md#con_textsize) | Высота символов консоли. Положительные значения трактуются как высота в виртуальных пикселях, отрицательные — как высота в физических пикселях экрана, а `0` внутри рендера принудительно откатывается к `8`. | `con_textsize(int)` |
| [`con_timeformat`](../38-cvars-reference/06-ui-console-input-cvars.md#con_timeformat) | Шаблон [`strftime`](../37-quakec-builtins-reference/02-string-builtins.md#strftime), который используется для меток времени при включённом `con_timestamps`. | `con_timeformat(string format time)` |
| [`con_timestamps`](../38-cvars-reference/06-ui-console-input-cvars.md#con_timestamps) | Добавляет временную метку в начало новых строк консоли. | `con_timestamps(boolean)` |
| [`in_builtinkeymap`](../38-cvars-reference/06-ui-console-input-cvars.md#in_builtinkeymap) | На Windows заставляет движок использовать собственную встроенную раскладку клавиш вместо системного преобразования ввода. | `in_builtinkeymap(boolean)` |
| [`in_dinput`](../38-cvars-reference/06-ui-console-input-cvars.md#in_dinput) | Включает использование DirectInput для обработки движения мыши в Windows. | `in_dinput(boolean)` |
| [`in_nonstandarddeadkeys`](../38-cvars-reference/06-ui-console-input-cvars.md#in_nonstandarddeadkeys) | Меняет обработку dead keys: движок отбрасывает события, которые порождают несколько символов, и использует только последний. | `in_nonstandarddeadkeys(boolean)` |
| [`in_rawinput`](../38-cvars-reference/06-ui-console-input-cvars.md#in_rawinput) | Включает Raw Input для мышей на Windows XP и новее. | `in_rawinput(boolean)` |
| [`in_rawinput_keyboard`](../38-cvars-reference/06-ui-console-input-cvars.md#in_rawinput_keyboard) | Расширяет использование Raw Input и на клавиатуры, а не только на мыши. | `in_rawinput_keyboard(boolean)` |
| [`in_simulatemultitouch`](../38-cvars-reference/06-ui-console-input-cvars.md#in_simulatemultitouch) | Разрешает трактовать некоторые абсолютные и относительные мышиные устройства как источники мультитача/мультикурсора. | `in_simulatemultitouch(boolean)` |
| [`in_xflip`](../38-cvars-reference/06-ui-console-input-cvars.md#in_xflip) | Инвертирует горизонтальную составляющую клиентского ввода: меняет знак у `sidemove`, а для части путей ввода ещё и переворачивает X-дельту мыши/тача до её превращения в yaw или strafe. | `in_xflip(boolean)` |
| [`in_xinput`](../38-cvars-reference/06-ui-console-input-cvars.md#in_xinput) | Включает поддержку XInput-контроллеров в Windows. | `in_xinput(int 0-3)` |
| [`joyexponent`](../38-cvars-reference/06-ui-console-input-cvars.md#joyexponent) | Нелинейно масштабирует отклонение стика: чем выше показатель, тем мягче центр и тем агрессивнее рост чувствительности к краям хода. | `joyexponent(float)` |
| [`joyonly`](../38-cvars-reference/06-ui-console-input-cvars.md#joyonly) | В SDL-ветке заставляет обрабатывать “game controllers” как обычные joystick-устройства, а не через специальный слой контроллеров. | `joyonly(boolean)` |
| [`joypitchsensitivity`](../38-cvars-reference/06-ui-console-input-cvars.md#joypitchsensitivity) | Максимальный множитель чувствительности для оси pitch на контроллере при полном отклонении. | `joypitchsensitivity(float)` |
| [`joypitchthreshold`](../38-cvars-reference/06-ui-console-input-cvars.md#joypitchthreshold) | Мёртвая зона для оси pitch. | `joypitchthreshold(float)` |
| [`joyrollsensitivity`](../38-cvars-reference/06-ui-console-input-cvars.md#joyrollsensitivity) | Максимальный множитель чувствительности для оси roll или связанного триггера/оси, если вы привязали её к крену. | `joyrollsensitivity(float)` |
| [`joyrollthreshold`](../38-cvars-reference/06-ui-console-input-cvars.md#joyrollthreshold) | Мёртвая зона для roll-оси или триггера. | `joyrollthreshold(float)` |
| [`joyyawsensitivity`](../38-cvars-reference/06-ui-console-input-cvars.md#joyyawsensitivity) | Максимальный множитель чувствительности для оси yaw на полном отклонении стика. | `joyyawsensitivity(float)` |
| [`joyyawthreshold`](../38-cvars-reference/06-ui-console-input-cvars.md#joyyawthreshold) | Мёртвая зона для оси yaw. Слишком маленькое значение вызывает самопроизвольный поворот, а слишком большое ухудшает точность микронаведения около центра. | `joyyawthreshold(float)` |
| [`m_filter`](../38-cvars-reference/06-ui-console-input-cvars.md#m_filter) | Смешивает текущее движение мыши с предыдущим и тем самым сглаживает резкие дельты. | `m_filter(float 0-2)` |
| [`m_forcewheel`](../38-cvars-reference/06-ui-console-input-cvars.md#m_forcewheel) | Определяет, как движок интерпретирует колесо мыши в API, где оно может приходить как третья ось. | `m_forcewheel(int 0-2)` |
| [`m_forcewheel_threshold`](../38-cvars-reference/06-ui-console-input-cvars.md#m_forcewheel_threshold) | Минимальная величина шага по оси колеса, после которой событие считается настоящей прокруткой. | `m_forcewheel_threshold(int)` |
| [`m_helpismedia`](../38-cvars-reference/06-ui-console-input-cvars.md#m_helpismedia) | Переключатель, связанный с системой меню и экранов справки. | `m_helpismedia(boolean)` |
| [`m_longpressthreshold`](../38-cvars-reference/06-ui-console-input-cvars.md#m_longpressthreshold) | Порог длительного нажатия на сенсорном экране в секундах. | `m_longpressthreshold(float)` |
| [`m_slidethreshold`](../38-cvars-reference/06-ui-console-input-cvars.md#m_slidethreshold) | Минимальная дистанция движения пальца, после которой касание превращается в slide-событие. | `m_slidethreshold(float)` |
| [`m_touchmajoraxis`](../38-cvars-reference/06-ui-console-input-cvars.md#m_touchmajoraxis) | Ограничивает сенсорное strafing-управление главной осью движения пальца. | `m_touchmajoraxis(boolean)` |
| [`sbar_teamstatus`](../38-cvars-reference/06-ui-console-input-cvars.md#sbar_teamstatus) | Показывает над статус-баром последние team-say сообщения от союзников. | `sbar_teamstatus(int 0-2)` |
| [`scr_loadingrefresh`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_loadingrefresh) | Принудительно перерисовывает экран загрузки при каждом обновлении списка ресурсов. | `scr_loadingrefresh(boolean)` |
| [`scr_loadingscreen_aspect`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_loadingscreen_aspect) | Управляет тем, как levelshot или загрузочное изображение вписывается по аспекту. | `scr_loadingscreen_aspect(int -1/0/1/2)` |
| [`scr_loadingscreen_picture`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_loadingscreen_picture) | Имя изображения, используемого как базовая картинка экрана загрузки в legacy-сценарии. | `scr_loadingscreen_picture(string/path)` |
| [`scr_loadingscreen_scale`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_loadingscreen_scale) | Дополнительный множитель масштаба для загрузочного изображения. | `scr_loadingscreen_scale(float)` |
| [`scr_loadingscreen_scale_limit`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_loadingscreen_scale_limit) | Режим ограничения масштаба загрузочного изображения после применения `scr_loadingscreen_scale`. | `scr_loadingscreen_scale_limit(int)` |
| [`scr_scoreboard_afk`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_afk) | Разрешает выводить метку `afk` в колонке packet loss на scoreboard, когда игрок помечен как отсутствующий. | `scr_scoreboard_afk(boolean)` |
| [`scr_scoreboard_backgroundalpha`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_backgroundalpha) | Дополнительный множитель прозрачности для фоновых элементов нового scoreboard. | `scr_scoreboard_backgroundalpha(float)` |
| [`scr_scoreboard_drawtitle`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_drawtitle) | Включает заголовок scoreboard. | `scr_scoreboard_drawtitle(boolean)` |
| [`scr_scoreboard_fillalpha`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_fillalpha) | Базовая прозрачность цветных заливок в новом стиле scoreboard. | `scr_scoreboard_fillalpha(float)` |
| [`scr_scoreboard_forcecolors`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_forcecolors) | Заставляет scoreboard подчиняться правилам `enemycolor` и `teamcolor`, а не исходным цветам записи. | `scr_scoreboard_forcecolors(boolean)` |
| [`scr_scoreboard_newstyle`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_newstyle) | Переключает современный стиль scoreboard с более заметными командными цветами и дополнительными визуальными элементами. | `scr_scoreboard_newstyle(boolean)` |
| [`scr_scoreboard_ping_status`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_ping_status) | Четыре порога, определяющие раскраску значения ping: от зелёного к белому, жёлтому, пурпурному и красному. | `scr_scoreboard_ping_status(list of 4 numbers)` |
| [`scr_scoreboard_showflags`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_showflags) | Управляет показом статистики флагов на scoreboard. | `scr_scoreboard_showflags(int 0-2)` |
| [`scr_scoreboard_showfrags`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_showfrags) | Добавляет на scoreboard расширенную статистику убийств, смертей и teamkill, насколько её удаётся восстановить по `fragfile.dat` и текстовым сообщениям. | `scr_scoreboard_showfrags(boolean)` |
| [`scr_scoreboard_showhealth`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_showhealth) | Показывает здоровье на scoreboard при просмотре MVD/QTV и родственных сценариях. | `scr_scoreboard_showhealth(int 0-3)` |
| [`scr_scoreboard_showlocation`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_showlocation) | При наличии данных выводит названия локаций игроков на scoreboard, прежде всего в MVD/QTV-повторах и трансляциях. | `scr_scoreboard_showlocation(boolean)` |
| [`scr_scoreboard_showruleset`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_showruleset) | Управляет колонкой [ruleset](../38-cvars-reference/07-system-misc-cvars.md#ruleset) на scoreboard. | `scr_scoreboard_showruleset(int 0-2)` |
| [`scr_scoreboard_showweapon`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_showweapon) | Показывает оружие игроков на scoreboard в режимах, где такие сведения доступны spectator-клиенту. | `scr_scoreboard_showweapon(boolean)` |
| [`scr_scoreboard_teamscores`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_teamscores) | Меняет поведение [`+showscores`](../44-cli-commands-reference/02-client-ui-commands.md#showscores), заставляя его работать как [`+showteamscores`](../44-cli-commands-reference/02-client-ui-commands.md#showteamscores). | `scr_scoreboard_teamscores(boolean)` |
| [`scr_scoreboard_teamsort`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_teamsort) | Сортирует игроков на scoreboard сначала по команде, а уже потом по личному счёту. | `scr_scoreboard_teamsort(boolean)` |
| [`scr_scoreboard_titleseperator`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_scoreboard_titleseperator) | Добавляет визуальный разделитель под заголовком колонок scoreboard, особенно заметный в классическом стиле. | `scr_scoreboard_titleseperator(boolean)` |
| [`scr_showdisk`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_showdisk) | Включает индикатор дисковой активности/дочитывания ресурсов на экране. | `scr_showdisk(boolean)` |
| [`scr_showloading`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_showloading) | Главный переключатель рисования загрузочного экрана. | `scr_showloading(boolean)` |
| [`scr_showobituaries`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_showobituaries) | Разрешает вывод obituary/killfeed-подобных [centerprint](../37-quakec-builtins-reference/12-system-debug-builtins.md#centerprint)-сообщений отдельным потоком, когда для них есть данные. | `scr_showobituaries(boolean)` |
| [`show_speed`](../38-cvars-reference/06-ui-console-input-cvars.md#show_speed) | Включает экранный индикатор скорости движения игрока. | `show_speed(boolean)` |
| [`sys_osk`](../38-cvars-reference/06-ui-console-input-cvars.md#sys_osk) | Разрешает системную экранную клавиатуру/IME-ввод там, где SDL предоставляет такой механизм. | `sys_osk(boolean)` |
| [`cl_clock_x`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_clock_x) | Определяет горизонтальную позицию экранных часов cl_clock. | `cl_clock_x(int)` |
| [`cl_clock_y`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_clock_y) | Определяет вертикальную позицию экранных часов cl_clock. | `cl_clock_y(int)` |
| [`cl_cursor`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_cursor) | Задаёт имя или путь к изображению пользовательского курсора для консольного и fallback-курсора меню. | `cl_cursor(string)` |
| [`cl_cursor_scale`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_cursor_scale) | Масштабирует пользовательский курсор. | `cl_cursor_scale(float)` |
| [`cl_gameclock_x`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_gameclock_x) | Определяет горизонтальную позицию игрового таймера cl_gameclock. | `cl_gameclock_x(int)` |
| [`cl_gameclock_y`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_gameclock_y) | Определяет вертикальную позицию игрового таймера cl_gameclock. | `cl_gameclock_y(int)` |
| [`cl_prydoncursor`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_prydoncursor) | Управляет отправкой курсора в расширения протокола Prydon или DP. | `cl_prydoncursor(string)` |
| [`cl_standardchat`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_standardchat) | Отключает автоматическое цветовое кодирование в сообщениях чата. | `cl_standardchat(int)` |
| [`cl_vrui_force`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_vrui_force) | Принудительно использовать интерфейсы виртуальной реальности, даже если гарнитура виртуальной реальности не активна. | `cl_vrui_force(int)` |
| [`cl_vrui_lock`](../38-cvars-reference/06-ui-console-input-cvars.md#cl_vrui_lock) | Управляет расположением пользовательского интерфейса при использовании VR/XR. 0: перемещается перед головой при переключении консоли/меню. 1: зафиксировано перед контрольной позицией (что может потребовать от пользователя развернуться, чтобы найти ее). | `cl_vrui_lock(int)` |
| [`con_logcenterprint`](../38-cvars-reference/06-ui-console-input-cvars.md#con_logcenterprint) | Указывает, печатать ли центральные отпечатки на консоли. | `con_logcenterprint(int)` |
| [`con_ocranaleds`](../38-cvars-reference/06-ui-console-input-cvars.md#con_ocranaleds) | Управляет добавлением набора символов Ocrana LED в консольный шрифт Quake. | `con_ocranaleds(int)` |
| [`con_textfont`](../38-cvars-reference/06-ui-console-input-cvars.md#con_textfont) | Шрифты TTF можно загрузить из каталога Windows. \'[gl_font](../38-cvars-reference/01-video-rendering-cvars.md#gl_font) cour?col=1,1,1:couri?col=0,1,0\' загружает, например: c:\\windows\\fonts\\cour.ttf, и использует курсивную версию courier для альтернативного текста с определенными цветовыми оттенками. | `con_textfont(string)` |
| [`con_window`](../38-cvars-reference/06-ui-console-input-cvars.md#con_window) | Указывает, должна ли консоль быть плавающим окном, как в играх на исходном движке, или должна располагаться только в верхней части экрана. | `con_window(int)` |
| [`contrast`](../38-cvars-reference/06-ui-console-input-cvars.md#contrast) | Линейное масштабирование значений цвета, чтобы сделать экран более удобным для просмотра. | `contrast(float)` |
| [`crosshairalpha`](../38-cvars-reference/06-ui-console-input-cvars.md#crosshairalpha) | Это альфа-канал, с которым [crosshair](../38-cvars-reference/01-video-rendering-cvars.md#crosshair) рисуется в 2D-рендерере. | `crosshairalpha(int)` |
| [`crosshaircolor`](../38-cvars-reference/06-ui-console-input-cvars.md#crosshaircolor) | Это строка RGB-цвета crosshair, разбираемая через SCR_StringToRGB и затем нормализуемая в диапазон 0..1. | `crosshaircolor(string)` |
| [`dpcompat_console`](../38-cvars-reference/06-ui-console-input-cvars.md#dpcompat_console) | Позволяет хакам эмулировать консоль DP. | `dpcompat_console(int)` |
| [`gamma`](../38-cvars-reference/06-ui-console-input-cvars.md#gamma) | Управляет яркостью экрана. | `gamma(float)` |
| [`in_forceseat`](../38-cvars-reference/06-ui-console-input-cvars.md#in_forceseat) | Переопределяет идентификаторы устройств для управления конкретным клиентом с любого устройства. | `in_forceseat(int)` |
| [`in_rawinput_rdp`](../38-cvars-reference/06-ui-console-input-cvars.md#in_rawinput_rdp) | Также активируйте устройства с протоколом удаленного рабочего стола. | `in_rawinput_rdp(int)` |
| [`in_skipplayerone`](../38-cvars-reference/06-ui-console-input-cvars.md#in_skipplayerone) | Не назначайте автоматически джойстики/игровые контроллеры первому игроку. | `in_skipplayerone(int)` |
| [`in_vraim`](../38-cvars-reference/06-ui-console-input-cvars.md#in_vraim) | Если установлено значение 1, угол обзора, отправляемый на сервер, контролируется вашей гарнитурой виртуальной реальности, а не отдельно. | `in_vraim(int)` |
| [`in_windowed_mouse`](../38-cvars-reference/06-ui-console-input-cvars.md#in_windowed_mouse) | Определяет, захватывает ли движок мышь в оконном режиме и работает ли с относительным вводом. | `in_windowed_mouse(int)` |
| [`joystick`](../38-cvars-reference/06-ui-console-input-cvars.md#joystick) | Включает инициализацию подсистемы joystick в Windows. | `joystick(int)` |
| [`m_forward`](../38-cvars-reference/06-ui-console-input-cvars.md#m_forward) | Задаёт множитель движения вперёд/назад для вертикального движения мыши в режимах, где ось Y используется не для обзора, а для перемещения. | `m_forward(int)` |
| [`m_pitch`](../38-cvars-reference/06-ui-console-input-cvars.md#m_pitch) | Задаёт коэффициент вертикального поворота камеры от движения мыши по оси Y. | `m_pitch(float)` |
| [`m_side`](../38-cvars-reference/06-ui-console-input-cvars.md#m_side) | Задаёт множитель бокового перемещения от горизонтального движения мыши, когда активен режим strafe_x вместо обычного поворота. | `m_side(float)` |
| [`m_yaw`](../38-cvars-reference/06-ui-console-input-cvars.md#m_yaw) | Задаёт коэффициент горизонтального поворота камеры от движения мыши по оси X. | `m_yaw(float)` |
| [`pr_menu_coreonerror`](../38-cvars-reference/06-ui-console-input-cvars.md#pr_menu_coreonerror) | Определяет, нужно ли сохранять дамп состояния menu.dat при аварийном завершении меню. | `pr_menu_coreonerror(int)` |
| [`pr_menu_memsize`](../38-cvars-reference/06-ui-console-input-cvars.md#pr_menu_memsize) | Задаёт объём памяти, выделяемый виртуальной машине menu.dat. | `pr_menu_memsize(string)` |
| [`r_globalskin_count`](../38-cvars-reference/06-ui-console-input-cvars.md#r_globalskin_count) | Указывает количество глобальных скинов. | `r_globalskin_count(int)` |
| [`r_globalskin_first`](../38-cvars-reference/06-ui-console-input-cvars.md#r_globalskin_first) | Указывает первое значение .skin, которое является глобальным скином. | `r_globalskin_first(int)` |
| [`r_part_rain_quantity`](../38-cvars-reference/06-ui-console-input-cvars.md#r_part_rain_quantity) | Множитель интенсивности surface-emitting weather в scripted [particle](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#particle) system. | `r_part_rain_quantity(int)` |
| [`r_skin_overlays`](../38-cvars-reference/06-ui-console-input-cvars.md#r_skin_overlays) | В текущем исходнике эта переменная целиком отключена: её объявление, регистрация и extern-ссылки закомментированы. | `r_skin_overlays(int)` |
| [`rcon_address`](../38-cvars-reference/06-ui-console-input-cvars.md#rcon_address) | Хранит адрес сервера для отправки rcon-команд, когда клиент сам не подключён к игре. | `rcon_address(string)` |
| [`rcon_level`](../38-cvars-reference/06-ui-console-input-cvars.md#rcon_level) | Переменная задаёт уровень ограничения по умолчанию для команд, alias-ов и cvar-ов, у которых собственное поле `restriction` не установлено. | `rcon_level(int)` |
| [`scr_allowsnap`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_allowsnap) | Разрешить ли запрашиваемые сервером запросы снимков экрана для проверки взлома драйверов графического процессора. | `scr_allowsnap(int)` |
| [`scr_autoid`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_autoid) | Размещайте бейджи над всеми игроками во время просмотра. | `scr_autoid(int)` |
| [`scr_autoid_armor`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_autoid_armor) | Отображать броню как часть именной бейджика (если она известна). | `scr_autoid_armor(int)` |
| [`scr_autoid_enemycolour`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_autoid_enemycolour) | Задаёт цвет текстовых nametag-ов для игроков, не состоящих в вашей команде. | `scr_autoid_enemycolour(string)` |
| [`scr_autoid_health`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_autoid_health) | Отображайте состояние здоровья в бейджах (если оно известно). | `scr_autoid_health(int)` |
| [`scr_autoid_team`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_autoid_team) | Размещайте бейджики над членами команды. 0: выкл. 1: отображение полуальфы, если оно закрыто. 2: скрываться при закрытии. | `scr_autoid_team(int)` |
| [`scr_autoid_teamcolour`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_autoid_teamcolour) | Задаёт цвет текстовых nametag-ов для членов вашей команды. | `scr_autoid_teamcolour(string)` |
| [`scr_autoid_weapon`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_autoid_weapon) | Отобразите лучшее оружие игрока на бейдже (если оно известно). | `scr_autoid_weapon(int)` |
| [`scr_autoid_weapon_mask`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_autoid_weapon_mask) | Маска битов для отображения значков оружия. +1: Дробовик. +2: Супердробовик. +4: Гвоздорез. +8: Супергвоздевик. +16: Гранатомет. +32: Ракетная установка. +64: Молния Показ только RL и GL — 96. | `scr_autoid_weapon_mask(int)` |
| [`scr_centersbar`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_centersbar) | Центрирует блок status bar внутри более широкого игрового прямоугольника. | `scr_centersbar(int)` |
| [`scr_centertime`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_centertime) | Сообщения Centerprint будут отображаться в течение этого времени, прежде чем исчезнут. | `scr_centertime(int)` |
| [`scr_conalpha`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_conalpha) | Насколько непрозрачной должна быть консоль, если за ней еще что-то происходит. | `scr_conalpha(float)` |
| [`scr_consize`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_consize) | Часть экрана, которая будет закрыта консолью в фокусе. | `scr_consize(float)` |
| [`scr_conspeed`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_conspeed) | Как быстро консоль появляется на экране. | `scr_conspeed(int)` |
| [`scr_diskicontimeout`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_diskicontimeout) | Определяет, сколько секунд после последнего доступа к файловой системе остаётся видимым индикатор дисковой активности. | `scr_diskicontimeout(float)` |
| [`scr_neticontimeout`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_neticontimeout) | Определяет задержку получения сетевых данных, после которой на экране появляется значок net. | `scr_neticontimeout(float)` |
| [`scr_printspeed`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_printspeed) | Насколько быстро каждый символ отображается во время заключительных сообщений. | `scr_printspeed(int)` |
| [`scr_showdisk_x`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_showdisk_x) | Задаёт горизонтальную позицию индикатора файловой активности, который рисуется при scr_showdisk и недавнем доступе к файловой системе. | `scr_showdisk_x(int)` |
| [`scr_showdisk_y`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_showdisk_y) | Задаёт вертикальную позицию индикатора файловой активности при включённом scr_showdisk. | `scr_showdisk_y(int)` |
| [`scr_sshot_compression`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_sshot_compression) | Требуемая степень сжатия в процентах. | `scr_sshot_compression(int)` |
| [`scr_sshot_prefix`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_sshot_prefix) | Задаёт каталог и префикс имени для файлов скриншотов. | `scr_sshot_prefix(string)` |
| [`scr_sshot_type`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_sshot_type) | Здесь указывается расширение по умолчанию (и, следовательно, формат файла) для снимков экрана. | `scr_sshot_type(string)` |
| [`scr_turtlefps`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_turtlefps) | Задаёт порог FPS для значка turtle, который сигнализирует о низкой частоте кадров. | `scr_turtlefps(int)` |
| [`scr_usekfont`](../38-cvars-reference/06-ui-console-input-cvars.md#scr_usekfont) | Существует для совместимости с переизданием Quake, изменяет поведение встроенных функций [sprint](../37-quakec-builtins-reference/12-system-debug-builtins.md#sprint)/[bprint](../37-quakec-builtins-reference/12-system-debug-builtins.md#bprint)/centerprint QC. | `scr_usekfont(int)` |
| [`v_contrastboost`](../38-cvars-reference/06-ui-console-input-cvars.md#v_contrastboost) | Усиливает контраст в темных областях. | `v_contrastboost(float)` |
| [`v_gammainverted`](../38-cvars-reference/06-ui-console-input-cvars.md#v_gammainverted) | Логическое значение, которое определяет, следует ли инвертировать гамму (например, землетрясение) или нет. | `v_gammainverted(int)` |
| [`vid_desktopgamma`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_desktopgamma) | Применяйте гамма-диапазоны к рабочему столу, а не к окну. | `vid_desktopgamma(int)` |
| [`vid_gl_context_compatibility`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_gl_context_compatibility) | Запрашивает контекст OpenGL с обратной совместимостью с фиксированной функцией. | `vid_gl_context_compatibility(int)` |
| [`vid_gl_context_debug`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_gl_context_debug) | Запрашивает отладочный контекст opengl. | `vid_gl_context_debug(int)` |
| [`vid_gl_context_es`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_gl_context_es) | Запрашивает контекст OpenGLES. | `vid_gl_context_es(int)` |
| [`vid_gl_context_forwardcompatible`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_gl_context_forwardcompatible) | Запрашивает контекст opengl без включенных устаревших функций. | `vid_gl_context_forwardcompatible(int)` |
| [`vid_gl_context_noerror`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_gl_context_noerror) | Отключает проверку ошибок OpenGL для небольшого повышения производительности. | `vid_gl_context_noerror(string)` |
| [`vid_gl_context_robustness`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_gl_context_robustness) | Попытайтесь обеспечить дополнительную защиту буфера в драйвере gl, но это может быть медленнее на оборудовании до gl3. | `vid_gl_context_robustness(int)` |
| [`vid_gl_context_selfreset`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_gl_context_selfreset) | В случае аппаратного сбоя движок должен создать новый контекст, а не зависеть от драйверов, чтобы все восстановить. | `vid_gl_context_selfreset(int)` |
| [`vid_gl_context_version`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_gl_context_version) | Указывает версию OpenGL, которую необходимо попытаться создать. | `vid_gl_context_version(string)` |
| [`vid_preservegamma`](../38-cvars-reference/06-ui-console-input-cvars.md#vid_preservegamma) | Восстанавливайте начальные аппаратные гамма-рампы при выходе. | `vid_preservegamma(int)` |

### Системные, отладочные и прочие cvar

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`_cl_disconnectreason`](../38-cvars-reference/07-system-misc-cvars.md#_cl_disconnectreason) | Эта квара содержит причину последнего отключения, чтобы меню мода могло знать, почему что-то не удалось. | `_cl_disconnectreason(string)` |
| [`_pext_infoblobs`](../38-cvars-reference/07-system-misc-cvars.md#_pext_infoblobs) | ПЕРЕИМЕНУЙТЕ МЕНЯ, КОГДА СТАБИЛЬНО. | `_pext_infoblobs(int)` |
| [`_pext_lerptime`](../38-cvars-reference/07-system-misc-cvars.md#_pext_lerptime) | ПЕРЕИМЕНУЙТЕ МЕНЯ, КОГДА СТАБИЛЬНО. | `_pext_lerptime(int)` |
| [`_pext_vrinputs`](../38-cvars-reference/07-system-misc-cvars.md#_pext_vrinputs) | ПЕРЕИМЕНУЙТЕ МЕНЯ, КОГДА СТАБИЛЬНО. | `_pext_vrinputs(int)` |
| [`_q3bsp_bihtraces`](../38-cvars-reference/07-system-misc-cvars.md#_q3bsp_bihtraces) | Использует генерируемую во время выполнения отсечку столкновений для более быстрого отслеживания. | `_q3bsp_bihtraces(int)` |
| [`allow_f_cmdline`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_cmdline) | Переменная управляет тем, отвечает ли клиент на запросы f_cmdline или q_cmdline из чата, которые обрабатываются системой validation. | `allow_f_cmdline(int)` |
| [`allow_f_fakeshaft`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_fakeshaft) | Переменная разрешает автоматический ответ на запрос f_fakeshaft. | `allow_f_fakeshaft(int)` |
| [`allow_f_modified`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_modified) | Переменная объявлена как флаг для ответов на запрос f_modified, однако в текущей реализации `Validation_Answer()` вызывает `Validation_FilesModified()` без проверки этого cvar. | `allow_f_modified(int)` |
| [`allow_f_ruleset`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_ruleset) | Переменная зарегистрирована в группе Authentication, но в текущем коде не участвует в ветке `f_ruleset`: запрос всегда обслуживается через `Validation_AllChecks()`. | `allow_f_ruleset(int)` |
| [`allow_f_scripts`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_scripts) | Переменная управляет ответом на запрос f_scripts. | `allow_f_scripts(int)` |
| [`allow_f_server`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_server) | Переменная управляет ответом на запрос f_server. | `allow_f_server(int)` |
| [`allow_f_skins`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_skins) | Переменная разрешает ответ на запрос f_skins о визуальных послаблениях клиента. | `allow_f_skins(int)` |
| [`allow_f_system`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_system) | Переменная управляет обработкой запроса f_system. | `allow_f_system(int)` |
| [`allow_f_version`](../38-cvars-reference/07-system-misc-cvars.md#allow_f_version) | Переменная управляет ответом на запрос f_version или q_version. | `allow_f_version(int)` |
| [`allow_skybox`](../38-cvars-reference/07-system-misc-cvars.md#allow_skybox) | Этот параметр определяет, должны ли клиенты пропускать запись глубины скайбокса при рендеринге скайбоксов/небесных куполов. | `allow_skybox(string)` |
| [`allow_splitscreen`](../38-cvars-reference/07-system-misc-cvars.md#allow_splitscreen) | Указывает, могут ли клиенты использовать расширения разделенного экрана для динамического добавления дополнительных клиентов. | `allow_splitscreen(string)` |
| [`auth_validateclients`](../38-cvars-reference/07-system-misc-cvars.md#auth_validateclients) | Переменная включает проверку чужих validation-ответов в `Validation_CheckIfResponse()`. | `auth_validateclients(int)` |
| [`b_switch`](../38-cvars-reference/07-system-misc-cvars.md#b_switch) | Сохраняется в userinfo и читается серверной логикой как запасной параметр предпочтений автопереключения оружия, если w_switch не задан. | `b_switch(string)` |
| [`baseskin`](../38-cvars-reference/07-system-misc-cvars.md#baseskin) | Имя скина игрока, который будет использоваться в качестве запасного. | `baseskin(string)` |
| [`bottomcolor`](../38-cvars-reference/07-system-misc-cvars.md#bottomcolor) | Определяет нижний цвет модели игрока и передаётся серверу через userinfo как индекс палитры или цветовое значение. | `bottomcolor(int)` |
| [`capturecodec`](../38-cvars-reference/07-system-misc-cvars.md#capturecodec) | По умолчанию движок пишет raw-кадры как `tga`. | `capturecodec(string)` |
| [`capturedemoheight`](../38-cvars-reference/07-system-misc-cvars.md#capturedemoheight) | При использовании `capturedemo` этот параметр задаёт высоту FBO-изображения, из которого берутся кадры для записи. | `capturedemoheight(int)` |
| [`capturedemowidth`](../38-cvars-reference/07-system-misc-cvars.md#capturedemowidth) | При использовании `capturedemo` этот параметр задаёт ширину FBO-изображения, из которого берутся кадры для записи. | `capturedemowidth(int)` |
| [`capturedriver`](../38-cvars-reference/07-system-misc-cvars.md#capturedriver) | Драйвер, который будет использоваться для записи демо-версии. | `capturedriver(string)` |
| [`capturemessage`](../38-cvars-reference/07-system-misc-cvars.md#capturemessage) | Задаёт произвольную строку, которую система видеозахвата рисует поверх кадра во время записи. | `capturemessage(string)` |
| [`capturethrottlesize`](../38-cvars-reference/07-system-misc-cvars.md#capturethrottlesize) | Если установлено, захват будет значительно замедляться, если на диске меньше свободного места, чем этот параметр (в МБ). | `capturethrottlesize(int)` |
| [`cfg_reload_on_gamedir`](../38-cvars-reference/07-system-misc-cvars.md#cfg_reload_on_gamedir) | Определяет, следует ли при смене game dir повторно выполнять конфиги и связанные перезапуски интерфейса. | `cfg_reload_on_gamedir(int)` |
| [`cfg_save_aliases`](../38-cvars-reference/07-system-misc-cvars.md#cfg_save_aliases) | Если 1, псевдонимы сохраняются в конфигах. | `cfg_save_aliases(int)` |
| [`cfg_save_all`](../38-cvars-reference/07-system-misc-cvars.md#cfg_save_all) | Если 1, cfg_save ВСЕГДА сохраняет все переменные. | `cfg_save_all(string)` |
| [`cfg_save_auto`](../38-cvars-reference/07-system-misc-cvars.md#cfg_save_auto) | Если 1, то конфиг сохранится автоматически и без подсказок. | `cfg_save_auto(int)` |
| [`cfg_save_binds`](../38-cvars-reference/07-system-misc-cvars.md#cfg_save_binds) | Если 1, все привязки клавиш сохраняются в конфигах. | `cfg_save_binds(int)` |
| [`cfg_save_buttons`](../38-cvars-reference/07-system-misc-cvars.md#cfg_save_buttons) | Если 1, сохраняется состояние таких вещей, как +mlook или +forward в конфигурации. | `cfg_save_buttons(int)` |
| [`cfg_save_infos`](../38-cvars-reference/07-system-misc-cvars.md#cfg_save_infos) | Если 1, информация о пользователе и сервере сохраняется в конфигах. | `cfg_save_infos(int)` |
| [`cfg_save_name`](../38-cvars-reference/07-system-misc-cvars.md#cfg_save_name) | Это имя конфигурации, которое сохраняется по умолчанию, если аргумент не указан. | `cfg_save_name(string)` |
| [`cl_aliasoverlap`](../38-cvars-reference/07-system-misc-cvars.md#cl_aliasoverlap) | Переименуйте новые псевдонимы, если они переопределяют имена кваров. | `cl_aliasoverlap(int)` |
| [`cl_autotrack`](../38-cvars-reference/07-system-misc-cvars.md#cl_autotrack) | Указывает режим отслеживания по умолчанию в начале карты. | `cl_autotrack(string)` |
| [`cl_autotrack_team`](../38-cvars-reference/07-system-misc-cvars.md#cl_autotrack_team) | Указывает название команды, которая должна автоматически отслеживаться (игроки других команд не будут кандидатами на автоматическое отслеживание). | `cl_autotrack_team(string)` |
| [`cl_beam_alpha`](../38-cvars-reference/07-system-misc-cvars.md#cl_beam_alpha) | Определяет прозрачность сегментов луча молнии. | `cl_beam_alpha(int)` |
| [`cl_beam_trace`](../38-cvars-reference/07-system-misc-cvars.md#cl_beam_trace) | Обрезает длину любых балок в соответствии со стенами, с которыми они могли столкнуться. | `cl_beam_trace(int)` |
| [`cl_c2sdupe`](../38-cvars-reference/07-system-misc-cvars.md#cl_c2sdupe) | Отправьте дубликаты пакетов на сервер. | `cl_c2sdupe(int)` |
| [`cl_c2sImpulseBackup`](../38-cvars-reference/07-system-misc-cvars.md#cl_c2simpulsebackup) | Предотвращает отбрасывание избыточных пакетов, содержащих импульсы, с помощью параметра cl_c2spps, чтобы сделать импульсы более надежными. | `cl_c2sImpulseBackup(int)` |
| [`cl_c2sMaxRedundancy`](../38-cvars-reference/07-system-misc-cvars.md#cl_c2smaxredundancy) | Это максимальное количество входных кадров для отправки в каждом входном пакете. | `cl_c2sMaxRedundancy(int)` |
| [`cl_c2spps`](../38-cvars-reference/07-system-misc-cvars.md#cl_c2spps) | Снижает скорость исходящих пакетов, удаляя до трети исходящих пакетов. | `cl_c2spps(int)` |
| [`cl_chasecam`](../38-cvars-reference/07-system-misc-cvars.md#cl_chasecam) | Управляет режимом камеры наблюдателя при слежении за игроком. | `cl_chasecam(int)` |
| [`cl_countpendingpl`](../38-cvars-reference/07-system-misc-cvars.md#cl_countpendingpl) | Если установлено значение 1, процент потери пакетов будет показывать пакеты, все еще находящиеся в пути, как потерянные, даже если они все еще могут быть получены. | `cl_countpendingpl(int)` |
| [`cl_crossx`](../38-cvars-reference/07-system-misc-cvars.md#cl_crossx) | Задаёт постоянное горизонтальное смещение [crosshair](../38-cvars-reference/01-video-rendering-cvars.md#crosshair) в пикселях экранного прямоугольника. | `cl_crossx(int)` |
| [`cl_crossy`](../38-cvars-reference/07-system-misc-cvars.md#cl_crossy) | Задаёт постоянное вертикальное смещение crosshair в пикселях экранного прямоугольника. | `cl_crossy(int)` |
| [`cl_crypt_rcon`](../38-cvars-reference/07-system-misc-cvars.md#cl_crypt_rcon) | Определяет, следует ли отправлять хэш вместо отправки пароля rcon в виде обычного текста. | `cl_crypt_rcon(int)` |
| [`cl_csqc_nodeprecate`](../38-cvars-reference/07-system-misc-cvars.md#cl_csqc_nodeprecate) | Если установлено, отключает предупреждения об устаревании. | `cl_csqc_nodeprecate(int)` |
| [`cl_csqcdebug`](../38-cvars-reference/07-system-misc-cvars.md#cl_csqcdebug) | Переменная включает диагностический вывод при обработке entity delta для CSQC. | `cl_csqcdebug(int)` |
| [`cl_deadbodyfilter`](../38-cvars-reference/07-system-misc-cvars.md#cl_deadbodyfilter) | Фильтрует кадры мёртвых тел игроков при рендеринге обычной модели игрока. | `cl_deadbodyfilter(int)` |
| [`cl_delay_packets`](../38-cvars-reference/07-system-misc-cvars.md#cl_delay_packets) | Дополнительная задержка в миллисекундах. | `cl_delay_packets(int)` |
| [`cl_demoreel`](../38-cvars-reference/07-system-misc-cvars.md#cl_demoreel) | Если этот параметр включен, при запуске движок начнет воспроизводить демонстрационный цикл. | `cl_demoreel(int)` |
| [`cl_demospeed`](../38-cvars-reference/07-system-misc-cvars.md#cl_demospeed) | Меняет скорость воспроизведения демо как множитель времени. | `cl_demospeed(float)` |
| [`cl_dlemptyterminate`](../38-cvars-reference/07-system-misc-cvars.md#cl_dlemptyterminate) | Прерывайте загрузку при получении пустого пакета загрузки. | `cl_dlemptyterminate(int)` |
| [`cl_expsprite`](../38-cvars-reference/07-system-misc-cvars.md#cl_expsprite) | Отобразите центральный спрайт с эффектами взрыва. | `cl_expsprite(int)` |
| [`cl_fakeframes`](../38-cvars-reference/07-system-misc-cvars.md#cl_fakeframes) | Медленный графический процессор? Хотите, чтобы сообщалось о более высокой частоте кадров! Раскройте силу лжи, чтобы увидеть гораздо более высокую частоту кадров! Многие говорили, что это невозможно, что народ этого не примет, но к черту соседей и неверующих! МЫ ХОТИМ БОЛЬШИХ ЦИФР, И МЫ ОЧЕНЬ ХОРОШО ИХ ПОЛУЧИМ!... Для достижения наилучших результатов комбинируйте их с внешними инструментами, такими как кадры с плавным движением... | `cl_fakeframes(int)` |
| [`cl_fullpitch`](../38-cvars-reference/07-system-misc-cvars.md#cl_fullpitch) | Если установлено, предпринимается попытка неограниченного шага просмотра по умолчанию. | `cl_fullpitch(int)` |
| [`cl_gibfilter`](../38-cvars-reference/07-system-misc-cvars.md#cl_gibfilter) | Скрывает gib-модели, включая фрагменты тела и модель оторванной головы, при отрисовке сетевых сущностей. | `cl_gibfilter(int)` |
| [`cl_gunanglex`](../38-cvars-reference/07-system-misc-cvars.md#cl_gunanglex) | Смещение угла модели оружия от первого лица по оси pitch. | `cl_gunanglex(int)` |
| [`cl_gunangley`](../38-cvars-reference/07-system-misc-cvars.md#cl_gunangley) | Смещение угла модели оружия от первого лица по оси yaw. | `cl_gunangley(int)` |
| [`cl_gunanglez`](../38-cvars-reference/07-system-misc-cvars.md#cl_gunanglez) | Смещение угла модели оружия от первого лица по оси roll. | `cl_gunanglez(int)` |
| [`cl_gunx`](../38-cvars-reference/07-system-misc-cvars.md#cl_gunx) | Сдвигает модель оружия от первого лица по горизонтали относительно камеры. | `cl_gunx(int)` |
| [`cl_guny`](../38-cvars-reference/07-system-misc-cvars.md#cl_guny) | Сдвигает модель оружия от первого лица по вертикали относительно камеры. | `cl_guny(int)` |
| [`cl_gunz`](../38-cvars-reference/07-system-misc-cvars.md#cl_gunz) | Сдвигает модель оружия от первого лица вперёд или назад вдоль направления взгляда. | `cl_gunz(int)` |
| [`cl_hightrack`](../38-cvars-reference/07-system-misc-cvars.md#cl_hightrack) | Устарело. Если вам нужен hightrack, используйте вместо этого «[cl_]autotrack high». | `cl_hightrack(int)` |
| [`cl_hudswap`](../38-cvars-reference/07-system-misc-cvars.md#cl_hudswap) | Меняет левую и правую раскладку элементов HUD в тех режимах, где строка состояния рисуется как накладной интерфейс. | `cl_hudswap(int)` |
| [`cl_idlefps`](../38-cvars-reference/07-system-misc-cvars.md#cl_idlefps) | Это максимальная частота кадров, которую можно достичь в режиме ожидания/паузы/несфокусировки. | `cl_idlefps(int)` |
| [`cl_legacystains`](../38-cvars-reference/07-system-misc-cvars.md#cl_legacystains) | ВНИМАНИЕ: по умолчанию эта переменная будет равна 0, а затем в какой-то момент будет удалена. | `cl_legacystains(int)` |
| [`cl_lerp_maxdistance`](../38-cvars-reference/07-system-misc-cvars.md#cl_lerp_maxdistance) | Максимальное расстояние, на которое сущность может перемещаться между снимками, не рассматриваясь как телепортировавшаяся. | `cl_lerp_maxdistance(int)` |
| [`cl_lerp_maxinterval`](../38-cvars-reference/07-system-misc-cvars.md#cl_lerp_maxinterval) | Максимальный интервал между ключевыми кадрами в секундах. | `cl_lerp_maxinterval(float)` |
| [`cl_lerp_players`](../38-cvars-reference/07-system-misc-cvars.md#cl_lerp_players) | Установите этот параметр, чтобы сделать игру других игроков более плавной, хотя это может увеличить эффективную задержку. | `cl_lerp_players(int)` |
| [`cl_loopbackprotocol`](../38-cvars-reference/07-system-misc-cvars.md#cl_loopbackprotocol) | Какой протокол использовать для одиночной игры/внутреннего клиента. | `cl_loopbackprotocol(string)` |
| [`cl_maxfps`](../38-cvars-reference/07-system-misc-cvars.md#cl_maxfps) | Устанавливает максимально допустимую частоту кадров. | `cl_maxfps(int)` |
| [`cl_model_bobbing`](../38-cvars-reference/07-system-misc-cvars.md#cl_model_bobbing) | Вращающиеся подбираемые предметы тоже подпрыгивают. | `cl_model_bobbing(int)` |
| [`cl_muzzleflash`](../38-cvars-reference/07-system-misc-cvars.md#cl_muzzleflash) | Управляет клиентскими вспышками выстрела. | `cl_muzzleflash(int)` |
| [`cl_netfps`](../38-cvars-reference/07-system-misc-cvars.md#cl_netfps) | Отправлять на сервер такое количество пакетов в секунду. | `cl_netfps(int)` |
| [`cl_noblink`](../38-cvars-reference/07-system-misc-cvars.md#cl_noblink) | Отключите функцию мигания текста ^^b. | `cl_noblink(int)` |
| [`cl_nocsqc`](../38-cvars-reference/07-system-misc-cvars.md#cl_nocsqc) | Переменная полностью запрещает запуск клиентского CSQC в [`CSQC_Init()`](../37-quakec-builtins-reference/00-entry-points.md#csqc_init). | `cl_nocsqc(int)` |
| [`cl_nodelta`](../38-cvars-reference/07-system-misc-cvars.md#cl_nodelta) | Отключает дельта-сжатие игровых обновлений. | `cl_nodelta(int)` |
| [`cl_nofake`](../38-cvars-reference/07-system-misc-cvars.md#cl_nofake) | Значение 0: разрешает символы \\r в сообщениях чата. значение 1: блокирует все символы \\r. значение 2: разрешает символы \\r, но только от товарищей по команде. | `cl_nofake(int)` |
| [`cl_nolerp`](../38-cvars-reference/07-system-misc-cvars.md#cl_nolerp) | Отключает интерполяцию. Если установлено, ракеты/монстры будут показывать именно то, что было получено последним, и это будет прерывисто. | `cl_nolerp(int)` |
| [`cl_nolerp_netquake`](../38-cvars-reference/07-system-misc-cvars.md#cl_nolerp_netquake) | Отключает интерполяцию при подключении к серверу NQ. | `cl_nolerp_netquake(int)` |
| [`cl_nopext`](../38-cvars-reference/07-system-misc-cvars.md#cl_nopext) | Отключает объявление клиентских протокольных расширений при подключении и в диагностической команде pext. | `cl_nopext(int)` |
| [`cl_parsewhitetext`](../38-cvars-reference/07-system-misc-cvars.md#cl_parsewhitetext) | При разборе сообщений чата включите поддержку сообщений типа: red{white}red. | `cl_parsewhitetext(int)` |
| [`cl_part_density_fade`](../38-cvars-reference/07-system-misc-cvars.md#cl_part_density_fade) | Указывает расстояние, на котором плотность точечных частиц ssqc снижается со всех до нулевых. | `cl_part_density_fade(int)` |
| [`cl_part_density_fade_start`](../38-cvars-reference/07-system-misc-cvars.md#cl_part_density_fade_start) | Определяет расстояние, на котором точечные частицы ssqc начнут становиться менее плотными. | `cl_part_density_fade_start(int)` |
| [`cl_pext_mask`](../38-cvars-reference/07-system-misc-cvars.md#cl_pext_mask) | Задаёт шестнадцатеричную маску для объявляемых FTE-расширений протокола первого набора. | `cl_pext_mask(string)` |
| [`cl_playerclass`](../38-cvars-reference/07-system-misc-cvars.md#cl_playerclass) | Хранит выбранный класс игрока для Hexen2 и передаётся через userinfo. | `cl_playerclass(string)` |
| [`cl_proxyaddr`](../38-cvars-reference/07-system-misc-cvars.md#cl_proxyaddr) | Задаёт адрес прокси, через который клиент пытается строить удалённые подключения и маршруты к серверам. | `cl_proxyaddr(string)` |
| [`cl_pure`](../38-cvars-reference/07-system-misc-cvars.md#cl_pure) | 0=стандартные правила Quake. 1=клиенты должны отдавать предпочтение файлам в пакетах, присутствующих на сервере. 2=клиенты должны использовать *только* файлы в пакетах, присутствующих на сервере. | `cl_pure(int)` |
| [`cl_queueimpulses`](../38-cvars-reference/07-system-misc-cvars.md#cl_queueimpulses) | Ставит неотправленные импульсы в очередь вместо их замены. | `cl_queueimpulses(int)` |
| [`cl_r2g`](../38-cvars-reference/07-system-misc-cvars.md#cl_r2g) | При 1 используется progs/grenade.mdl вместо progs/missile.mdl. | `cl_r2g(int)` |
| [`cl_rollalpha`](../38-cvars-reference/07-system-misc-cvars.md#cl_rollalpha) | Управляет скоростью, с которой обзор вращается в зависимости от бокового движения. | `cl_rollalpha(int)` |
| [`cl_sbar`](../38-cvars-reference/07-system-misc-cvars.md#cl_sbar) | Переключает способ рисования строки состояния и связанную компоновку игрового экрана. | `cl_sbar(int)` |
| [`cl_sbaralpha`](../38-cvars-reference/07-system-misc-cvars.md#cl_sbaralpha) | Определяет прозрачность строки состояния. | `cl_sbaralpha(float)` |
| [`cl_selfcam`](../38-cvars-reference/07-system-misc-cvars.md#cl_selfcam) | В текущем коде это cvar-заглушка без рабочего подключения. | `cl_selfcam(int)` |
| [`cl_sendguid`](../38-cvars-reference/07-system-misc-cvars.md#cl_sendguid) | Отправьте на серверы случайно сгенерированный «глобально уникальный» идентификатор, который может использоваться серверами для ранжирования и т. д. | `cl_sendguid(string)` |
| [`cl_serveraddress`](../38-cvars-reference/07-system-misc-cvars.md#cl_serveraddress) | Адрес последнего сервера, к которому вы подключались. | `cl_serveraddress(string)` |
| [`cl_servername`](../38-cvars-reference/07-system-misc-cvars.md#cl_servername) | Имя хоста последнего сервера, к которому вы подключались. | `cl_servername(string)` |
| [`cl_shownet`](../38-cvars-reference/07-system-misc-cvars.md#cl_shownet) | Отладка вар. 0 ничего не показывает. 1 показаны размеры входящих пакетов. 2 показаны отдельные сообщения. 3 также показаны объекты. | `cl_shownet(int)` |
| [`cl_solid_players`](../38-cvars-reference/07-system-misc-cvars.md#cl_solid_players) | Считайте других игроков надежными для прогнозирования. | `cl_solid_players(int)` |
| [`cl_splitscreen`](../38-cvars-reference/07-system-misc-cvars.md#cl_splitscreen) | Включает поддержку разделенного экрана. | `cl_splitscreen(int)` |
| [`cl_standardmsg`](../38-cvars-reference/07-system-misc-cvars.md#cl_standardmsg) | Отключает автоматическое цветовое кодирование при печати с консоли. | `cl_standardmsg(int)` |
| [`cl_threadedphysics`](../38-cvars-reference/07-system-misc-cvars.md#cl_threadedphysics) | Если установлено, входные кадры клиента генерируются и отправляются в рабочий поток. | `cl_threadedphysics(int)` |
| [`cl_timeout`](../38-cvars-reference/07-system-misc-cvars.md#cl_timeout) | Определяет, сколько секунд клиент ждёт новых пакетов от сервера перед разрывом соединения. | `cl_timeout(int)` |
| [`cl_truelightning`](../38-cvars-reference/07-system-misc-cvars.md#cl_truelightning) | Управляйте конечным положением собственных балок игрока, чтобы скрыть задержку. | `cl_truelightning(int)` |
| [`cl_verify_urischeme`](../38-cvars-reference/07-system-misc-cvars.md#cl_verify_urischeme) | 0: ничего не делать. | `cl_verify_urischeme(int)` |
| [`cl_warncmd`](../38-cvars-reference/07-system-misc-cvars.md#cl_warncmd) | Переменная включает пользовательские предупреждения консоли о спорных или неудачных командных действиях. | `cl_warncmd(int)` |
| [`cl_weaponforgetorder`](../38-cvars-reference/07-system-misc-cvars.md#cl_weaponforgetorder) | Команда «оружие» фиксирует выбор оружия вместо выбора другого оружия между выбором + стрельбой. | `cl_weaponforgetorder(int)` |
| [`cl_weaponhide`](../38-cvars-reference/07-system-misc-cvars.md#cl_weaponhide) | ХАК: попытайтесь сменить оружие на другое, чтобы обманом лишить убийцу любых возможных улучшений оружия. 0: оригинальное поведение 1: переключайтесь, когда +атака/+огонь выпущены 2: переключайтесь только в Deathmatch. | `cl_weaponhide(int)` |
| [`cl_weaponhide_preference`](../38-cvars-reference/07-system-misc-cvars.md#cl_weaponhide_preference) | Оружие, на которое вы хотели бы попробовать переключиться, когда cl_weaponhide активен. | `cl_weaponhide_preference(string)` |
| [`cl_weaponpreselect`](../38-cvars-reference/07-system-misc-cvars.md#cl_weaponpreselect) | ХАК: Управляет взаимодействием между командами ^aweapon^a и ^a+attack^a (не влияет на ^aimpulse^a). | `cl_weaponpreselect(int)` |
| [`cl_yieldcpu`](../38-cvars-reference/07-system-misc-cvars.md#cl_yieldcpu) | Попытайтесь уступить между кадрами. | `cl_yieldcpu(int)` |
| [`cmd_allowaccess`](../38-cvars-reference/07-system-misc-cvars.md#cmd_allowaccess) | Резервный переключатель разграничения доступа. | `cmd_allowaccess(int)` |
| [`cmd_gamecodelevel`](../38-cvars-reference/07-system-misc-cvars.md#cmd_gamecodelevel) | Минимальный Cmd_ExecLevel, при котором движок вообще передаёт неизвестную консольную команду в серверный игровой код через PR_ConsoleCmd и gfuncs.ConsoleCmd. | `cmd_gamecodelevel(string)` |
| [`cmd_maxbuffersize`](../38-cvars-reference/07-system-misc-cvars.md#cmd_maxbuffersize) | Переменная ограничивает максимальный размер командного буфера `cmd_text[level]` при динамическом росте через `Cbuf_AddText()`. | `cmd_maxbuffersize(int)` |
| [`d3d_hlsl`](../38-cvars-reference/07-system-misc-cvars.md#d3d_hlsl) | Переключает поддержку HLSL-программ в рендерере Direct3D 9. | `d3d_hlsl(int)` |
| [`d_mipcap`](../38-cvars-reference/07-system-misc-cvars.md#d_mipcap) | Указывает диапазон используемых MIP-уровней. | `d_mipcap(string)` |
| [`developer`](../38-cvars-reference/07-system-misc-cvars.md#developer) | Управляет уровнем отладочного вывода движка. | `developer(int)` |
| [`dpcompat_csqcinputeventtypes`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_csqcinputeventtypes) | Указывает первое событие ввода csqc, которое мод не распознает. | `dpcompat_csqcinputeventtypes(int)` |
| [`dpcompat_findradiusarealinks`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_findradiusarealinks) | Используйте информацию о столкновении мира, чтобы ускорить поиск радиуса вместо циклического перебора каждого отдельного объекта. | `dpcompat_findradiusarealinks(int)` |
| [`dpcompat_nofloodfill`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_nofloodfill) | Отключает заливку q1mdl. Установка значения 1 может привести к появлению синих швов на моделях ванильного землетрясения. | `dpcompat_nofloodfill(int)` |
| [`dpcompat_nopremulpics`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_nopremulpics) | По умолчанию FTE использует предварительно умноженную альфу для изображений hud/2d, а DP этого не делает (что приводит к появлению ореолов с контентом низкого разрешения). | `dpcompat_nopremulpics(int)` |
| [`dpcompat_nopreparse`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_nopreparse) | Xonotic использует svc_tempentity с неизвестной длиной, смешанный с другими данными, которые необходимо преобразовать. | `dpcompat_nopreparse(int)` |
| [`dpcompat_noretouchground`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_noretouchground) | Предотвращает повторное касание объектов, которые уже стоят на объекте, к этому объекту. | `dpcompat_noretouchground(int)` |
| [`dpcompat_psa_ungroup`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_psa_ungroup) | Включает совместимый с DarkPlaces режим загрузки PSA-анимаций для PSK-моделей. | `dpcompat_psa_ungroup(int)` |
| [`dpcompat_set`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_set) | Переменная включает совместимый с DarkPlaces разбор команды `set`. | `dpcompat_set(int)` |
| [`dpcompat_skinfiles`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_skinfiles) | Если установлено, используется шейдер nodraw для любых неупомянутых поверхностей. | `dpcompat_skinfiles(int)` |
| [`dpcompat_smallerfonts`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_smallerfonts) | Имитирует поведение DP, использующее меньший размер шрифта, чем было на самом деле запрошено. | `dpcompat_smallerfonts(int)` |
| [`dpcompat_stats`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_stats) | Режим совместимости со стилем статистики DarkPlaces. | `dpcompat_stats(int)` |
| [`dpcompat_strcat_limit`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_strcat_limit) | Если установлено, длина строки [strcat](../37-quakec-builtins-reference/02-string-builtins.md#strcat) (и связанной функции) сокращается до указанного значения. | `dpcompat_strcat_limit(string)` |
| [`dpcompat_traceontouch`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_traceontouch) | Сообщайте о плоскости трассировки и т. д., когда один объект касается другого. | `dpcompat_traceontouch(int)` |
| [`edit_addcr`](../38-cvars-reference/07-system-misc-cvars.md#edit_addcr) | Убедитесь, что каждая строка заканчивается символом \\r (при сохранении). | `edit_addcr(string)` |
| [`edit_stripcr`](../38-cvars-reference/07-system-misc-cvars.md#edit_stripcr) | Удалить \\r из eols (при загрузке). | `edit_stripcr(int)` |
| [`edit_tabsize`](../38-cvars-reference/07-system-misc-cvars.md#edit_tabsize) | Насколько широкое выравнивание табуляции. | `edit_tabsize(int)` |
| [`enemyforceskins`](../38-cvars-reference/07-system-misc-cvars.md#enemyforceskins) | 0: читать скин как есть. 1: использовать имя игрока вместо настроек его скина. 2: использовать его идентификатор пользователя (вероятно, слишком ненадежно).3: использовать индекс игрока для каждой команды. | `enemyforceskins(int)` |
| [`ezcompat_markup`](../38-cvars-reference/07-system-misc-cvars.md#ezcompat_markup) | Попытка совместимости с текстовой разметкой ezquake.0: отключено. | `ezcompat_markup(int)` |
| [`fbskins`](../38-cvars-reference/07-system-misc-cvars.md#fbskins) | Серверное объявление того, разрешены ли клиенту fullbright skins в стиле FuhQuake. | `fbskins(string)` |
| [`forceqmenu`](../38-cvars-reference/07-system-misc-cvars.md#forceqmenu) | Принудительно отключает загрузку menu.dat и оставляет только встроенное qmenu. | `forceqmenu(int)` |
| [`fraglog_details`](../38-cvars-reference/07-system-misc-cvars.md#fraglog_details) | Битовая маска 1: имена убийц+убийц. 2: команды убийц+убийц 4:метка времени. 8:оружие убийцы 16:убийца+гид убийцы. | `fraglog_details(int)` |
| [`gamecfg`](../38-cvars-reference/07-system-misc-cvars.md#gamecfg) | Совместимый служебный cvar для игрового кода. | `gamecfg(int)` |
| [`gameversion`](../38-cvars-reference/07-system-misc-cvars.md#gameversion) | Версия игрового кода для серверных браузеров. | `gameversion(string)` |
| [`gameversion_max`](../38-cvars-reference/07-system-misc-cvars.md#gameversion_max) | Версия игрового кода для серверных браузеров. | `gameversion_max(string)` |
| [`gameversion_min`](../38-cvars-reference/07-system-misc-cvars.md#gameversion_min) | Версия игрового кода для серверных браузеров. | `gameversion_min(string)` |
| [`hand`](../38-cvars-reference/07-system-misc-cvars.md#hand) | Чтобы игровой код знал, из какой руки стрелять. | `hand(string)` |
| [`ignore_flood`](../38-cvars-reference/07-system-misc-cvars.md#ignore_flood) | Предоставляет возможность уменьшить количество входящего спама, заполоняющего ваш чат (обманные сообщения игнорируются). | `ignore_flood(int)` |
| [`ignore_flood_duration`](../38-cvars-reference/07-system-misc-cvars.md#ignore_flood_duration) | Срок, в течение которого входящие сообщения будут считаться дубликатами. | `ignore_flood_duration(int)` |
| [`ignore_mode`](../38-cvars-reference/07-system-misc-cvars.md#ignore_mode) | Расширяет действие ignore-фильтра на командные сообщения. | `ignore_mode(int)` |
| [`ignore_opponents`](../38-cvars-reference/07-system-misc-cvars.md#ignore_opponents) | 0: Не игнорировать чат от врагов. 1: Всегда игнорировать чат от противников (примечание: можно также игнорировать проверки f_ruleset). 2: Игнорировать чат от противников только во время матча (требуются серверы, которые фактически сообщают о состоянии матча). | `ignore_opponents(int)` |
| [`ignore_qizmo_spec`](../38-cvars-reference/07-system-misc-cvars.md#ignore_qizmo_spec) | В текущей реализации эта переменная почти не влияет на фильтрацию чата. | `ignore_qizmo_spec(int)` |
| [`ignore_spec`](../38-cvars-reference/07-system-misc-cvars.md#ignore_spec) | 0: Никогда не игнорировать зрителей. | `ignore_spec(int)` |
| [`ipautodump`](../38-cvars-reference/07-system-misc-cvars.md#ipautodump) | Позволяет создавать дамп файла iplog.txt, который содержит журнал имен пользователей, замеченных для данного IP-адреса, что полезно для обнаружения поддельных ников. | `ipautodump(int)` |
| [`itburnsitburnsmakeitstop`](../38-cvars-reference/07-system-misc-cvars.md#itburnsitburnsmakeitstop) | Ой. | `itburnsitburnsmakeitstop(int)` |
| [`joyradialdeadzone`](../38-cvars-reference/07-system-misc-cvars.md#joyradialdeadzone) | Обрабатывать мертвые зоны контроллера как пару, а не по каждой оси. | `joyradialdeadzone(string)` |
| [`lang`](../38-cvars-reference/07-system-misc-cvars.md#lang) | Эта переменная содержит код языка/диалекта, используемый для поиска строк локализации. | `lang(string)` |
| [`leftisright`](../38-cvars-reference/07-system-misc-cvars.md#leftisright) | Зеркально переворачивает горизонтальную составляющую изображения и управления. | `leftisright(int)` |
| [`log_developer`](../38-cvars-reference/07-system-misc-cvars.md#log_developer) | Включает регистрацию распечаток консоли, если установлено значение 1. | `log_developer(int)` |
| [`log_dir`](../38-cvars-reference/07-system-misc-cvars.md#log_dir) | Задаёт каталог, в который движок складывает log-файлы. | `log_dir(string)` |
| [`log_dosformat`](../38-cvars-reference/07-system-misc-cvars.md#log_dosformat) | Управляет переводами строк в log-файлах. | `log_dosformat(int)` |
| [`log_readable`](../38-cvars-reference/07-system-misc-cvars.md#log_readable) | Битовое поле, описывающее, что конвертировать/удалить. | `log_readable(int)` |
| [`log_rotate_files`](../38-cvars-reference/07-system-misc-cvars.md#log_rotate_files) | Включает ротацию log-файлов и задаёт, сколько старых копий хранить. | `log_rotate_files(int)` |
| [`log_rotate_size`](../38-cvars-reference/07-system-misc-cvars.md#log_rotate_size) | Задаёт порог размера log-файла в байтах, после которого запускается ротация. | `log_rotate_size(int)` |
| [`log_timestamps`](../38-cvars-reference/07-system-misc-cvars.md#log_timestamps) | Добавляет временную метку в начало каждой новой строки log-файла. | `log_timestamps(int)` |
| [`lookspring`](../38-cvars-reference/07-system-misc-cvars.md#lookspring) | Отцентрируйте камеру, когда отпустите курсор мыши. | `lookspring(int)` |
| [`lookstrafe`](../38-cvars-reference/07-system-misc-cvars.md#lookstrafe) | Mouselook позволяет перемещать мышь. | `lookstrafe(int)` |
| [`m_accel`](../38-cvars-reference/07-system-misc-cvars.md#m_accel) | Значения >0 усиливают движение мыши пропорционально скорости. | `m_accel(int)` |
| [`m_accel_noforce`](../38-cvars-reference/07-system-misc-cvars.md#m_accel_noforce) | Запрещает движку подменять системный параметр ускорения мыши Windows. | `m_accel_noforce(int)` |
| [`m_accel_offset`](../38-cvars-reference/07-system-misc-cvars.md#m_accel_offset) | Используется, когда m_accel_style равен 1. | `m_accel_offset(int)` |
| [`m_accel_power`](../38-cvars-reference/07-system-misc-cvars.md#m_accel_power) | Используется, когда m_accel_style равен 1. | `m_accel_power(int)` |
| [`m_accel_senscap`](../38-cvars-reference/07-system-misc-cvars.md#m_accel_senscap) | Используется, когда m_accel_style равен 1. | `m_accel_senscap(int)` |
| [`m_accel_style`](../38-cvars-reference/07-system-misc-cvars.md#m_accel_style) | 1 = ускорение мыши Quake Live, 0 = ускорение по старому стилю. | `m_accel_style(int)` |
| [`m_fatpressthreshold`](../38-cvars-reference/07-system-misc-cvars.md#m_fatpressthreshold) | Насколько толстым должен быть ваш большой палец, чтобы можно было зарегистрировать толстый пресс (сенсорные экраны). | `m_fatpressthreshold(float)` |
| [`m_preset_chosen`](../38-cvars-reference/07-system-misc-cvars.md#m_preset_chosen) | Архивируемый флаг интерфейса, показывающий, выбирал ли пользователь preset производительности/настроек. | `m_preset_chosen(int)` |
| [`m_threshold_noforce`](../38-cvars-reference/07-system-misc-cvars.md#m_threshold_noforce) | Запрещает движку подменять пороги чувствительности/ускорения мыши Windows. | `m_threshold_noforce(int)` |
| [`m_touchstrafe`](../38-cvars-reference/07-system-misc-cvars.md#m_touchstrafe) | 0: меняется только угол всего экрана. | `m_touchstrafe(int)` |
| [`map_autoopenportals`](../38-cvars-reference/07-system-misc-cvars.md#map_autoopenportals) | Если установлено значение 1, принудительно открываются все порталы областей. | `map_autoopenportals(int)` |
| [`map_noareas`](../38-cvars-reference/07-system-misc-cvars.md#map_noareas) | Отключает систему area/connectivity на карте. | `map_noareas(int)` |
| [`map_noCurves`](../38-cvars-reference/07-system-misc-cvars.md#map_nocurves) | Отключает обработку Q3-кривых patch-поверхностей. | `map_noCurves(int)` |
| [`mapname`](../38-cvars-reference/07-system-misc-cvars.md#mapname) | Cvar, содержащий короткое имя текущей карты, для сценариев типа скриптов. | `mapname(string)` |
| [`maxpitch`](../38-cvars-reference/07-system-misc-cvars.md#maxpitch) | Предположительно 80. | `maxpitch(string)` |
| [`minpitch`](../38-cvars-reference/07-system-misc-cvars.md#minpitch) | Предполагается -70. | `minpitch(string)` |
| [`mod_h2holey_bugged`](../38-cvars-reference/07-system-misc-cvars.md#mod_h2holey_bugged) | Флаг дырявой модели Hexen2 использует индекс 0 как прозрачный (и дополнительно 255 в gl из-за ошибки). | `mod_h2holey_bugged(int)` |
| [`mod_halftexel`](../38-cvars-reference/07-system-misc-cvars.md#mod_halftexel) | Смещены координаты текстуры на половину текселя, для совместимости с glquake и большинством вилок движка. | `mod_halftexel(int)` |
| [`mod_lightpoint_distance`](../38-cvars-reference/07-system-misc-cvars.md#mod_lightpoint_distance) | Это максимальное расстояние, которое необходимо отслеживать при поиске поверхности земли для получения информации об освещении в форматах карт без более сложной информации об освещении. | `mod_lightpoint_distance(int)` |
| [`mod_lightscale_broken`](../38-cvars-reference/07-system-misc-cvars.md#mod_lightscale_broken) | Когда активен, копирует ошибку из ванили - радиус источников света [r_dynamic](../38-cvars-reference/02-lighting-materials-cvars.md#r_dynamic) масштабируется в соответствии с масштабом текстуры каждой поверхности, а не с использованием фактического расстояния. | `mod_lightscale_broken(int)` |
| [`mod_loadmappackages`](../38-cvars-reference/07-system-misc-cvars.md#mod_loadmappackages) | Загрузите дополнительный контент, встроенный в файлы BSP. | `mod_loadmappackages(int)` |
| [`mod_md3flags`](../38-cvars-reference/07-system-misc-cvars.md#mod_md3flags) | Поле flags в md3s никогда официально не определялось. | `mod_md3flags(int)` |
| [`mod_md5_singleanimation`](../38-cvars-reference/07-system-misc-cvars.md#mod_md5_singleanimation) | При загрузке файла md5mesh также попытайтесь загрузить файл .md5anim и распаковать его по отдельным позам. | `mod_md5_singleanimation(int)` |
| [`mod_nomipmap`](../38-cvars-reference/07-system-misc-cvars.md#mod_nomipmap) | Отключает использование MIP-карт в Quake1 MDLS, в соответствии с исходным программным рендерером. | `mod_nomipmap(int)` |
| [`mod_obj_orientation`](../38-cvars-reference/07-system-misc-cvars.md#mod_obj_orientation) | Управляет интерпретацией оси модели. | `mod_obj_orientation(int)` |
| [`mod_precache`](../38-cvars-reference/07-system-misc-cvars.md#mod_precache) | Управление загрузкой моделей. | `mod_precache(int)` |
| [`mod_warnmodels`](../38-cvars-reference/07-system-misc-cvars.md#mod_warnmodels) | Предупредите, если какие-либо модели не удалось загрузить. | `mod_warnmodels(int)` |
| [`model`](../38-cvars-reference/07-system-misc-cvars.md#model) | Хранит предпочитаемое имя модели игрока и отправляется как часть userinfo. | `model(string)` |
| [`msg`](../38-cvars-reference/07-system-misc-cvars.md#msg) | Фильтрация распечаток/сообщений консоли. | `msg(int)` |
| [`msg_filter`](../38-cvars-reference/07-system-misc-cvars.md#msg_filter) | Отфильтровать сообщения чата: 0 = ни одно. 1=трансляция чата. 2 = командный чат. 3=весь чат. | `msg_filter(int)` |
| [`musicvolume`](../38-cvars-reference/07-system-misc-cvars.md#musicvolume) | Уровень громкости фоновой музыки. | `musicvolume(float)` |
| [`name`](../38-cvars-reference/07-system-misc-cvars.md#name) | Определяет отображаемое имя игрока и сохраняется в userinfo. | `name(string)` |
| [`noaim`](../38-cvars-reference/07-system-misc-cvars.md#noaim) | Передаётся серверу через userinfo как запрос отключить автоприцеливание для данного игрока. | `noaim(string)` |
| [`noexit`](../38-cvars-reference/07-system-misc-cvars.md#noexit) | Служебный игровой флаг, который движок лишь сохраняет вместе с файлом сохранения и предоставляет игровому коду. | `noexit(int)` |
| [`nomonsters`](../38-cvars-reference/07-system-misc-cvars.md#nomonsters) | Наследуемый cvar для игрового кода, который движок регистрирует и сохраняет в файле сохранения, но сам не читает. | `nomonsters(int)` |
| [`noskins`](../38-cvars-reference/07-system-misc-cvars.md#noskins) | Управляет использованием пользовательских player skins в QuakeWorld-совместимой части клиента. | `noskins(int)` |
| [`pext_ezquake_nochunks`](../38-cvars-reference/07-system-misc-cvars.md#pext_ezquake_nochunks) | Не позволяет клиентам ezquake использовать расширение для загрузки по частям. | `pext_ezquake_nochunks(int)` |
| [`pext_ezquake_verfortrans`](../38-cvars-reference/07-system-misc-cvars.md#pext_ezquake_verfortrans) | EzQuake неправильно реализует PEXT_TRANS. | `pext_ezquake_verfortrans(int)` |
| [`pext_predinfo`](../38-cvars-reference/07-system-misc-cvars.md#pext_predinfo) | Включает некоторые дополнительные функции для поддержки прогнозирования по протоколам NQ. | `pext_predinfo(int)` |
| [`pext_replacementdeltas`](../38-cvars-reference/07-system-misc-cvars.md#pext_replacementdeltas) | Позволяет использовать альтернативные дельты объектов на основе Nack. | `pext_replacementdeltas(int)` |
| [`pkg_autoupdate`](../38-cvars-reference/07-system-misc-cvars.md#pkg_autoupdate) | Управляет автоматическими обновлениями, их можно изменить только через меню загрузок. 0: выключено. 1: включено (только стабильная версия). 2: включено (нестабильная версия). | `pkg_autoupdate(int)` |
| [`plug_loaddefault`](../38-cvars-reference/07-system-misc-cvars.md#plug_loaddefault) | 0: Загружать плагины только с помощью явных команд [`plug_load`](../44-cli-commands-reference/05-filesystem-system-commands.md#plug_load). | `plug_loaddefault(int)` |
| [`plug_sbar`](../38-cvars-reference/07-system-misc-cvars.md#plug_sbar) | Определяет, разрешено ли плагинам отрисовывать HUD вместо движка (если это также разрешено CSQC). | `plug_sbar(int)` |
| [`prox_inmenu`](../38-cvars-reference/07-system-misc-cvars.md#prox_inmenu) | Переводит часть обычных клавиш движения в команды управления proxy-меню. | `prox_inmenu(int)` |
| [`q3bsp_ignorestyles`](../38-cvars-reference/07-system-misc-cvars.md#q3bsp_ignorestyles) | Игнорирует несколько стилей освещения в варианте Raven q3bsp (и его производных) для повышения производительности пакетной обработки/рендеринга. | `q3bsp_ignorestyles(int)` |
| [`q3bsp_mergelightmaps`](../38-cvars-reference/07-system-misc-cvars.md#q3bsp_mergelightmaps) | Указывает, следует ли объединять карты освещения в атласы для повышения производительности. | `q3bsp_mergelightmaps(int)` |
| [`q3bsp_surf_meshcollision_flag`](../38-cvars-reference/07-system-misc-cvars.md#q3bsp_surf_meshcollision_flag) | Флаги Surfaceparm, которые разрешают столкновение трисоупа q3bsp. | `q3bsp_surf_meshcollision_flag(string)` |
| [`q3bsp_surf_meshcollision_force`](../38-cvars-reference/07-system-misc-cvars.md#q3bsp_surf_meshcollision_force) | Принудительные столкновения на основе сетки на всех поверхностях трисупа q3bsp. | `q3bsp_surf_meshcollision_force(int)` |
| [`qport_`](../38-cvars-reference/07-system-misc-cvars.md#qport_) | Хранит случайный клиентский qport, который движок генерирует при запуске и не сохраняет в конфиг. | `qport_(int)` |
| [`rank_autoadd`](../38-cvars-reference/07-system-misc-cvars.md#rank_autoadd) | Автоматически регистрировать игроков в рейтинговой системе. | `rank_autoadd(int)` |
| [`rank_filename`](../38-cvars-reference/07-system-misc-cvars.md#rank_filename) | Указывает, какой файл использовать в качестве базы данных рейтингов. | `rank_filename(string)` |
| [`rank_needlogin`](../38-cvars-reference/07-system-misc-cvars.md#rank_needlogin) | Если установлено значение 1, игрокам запрещено присоединяться, если они еще не зарегистрированы. | `rank_needlogin(int)` |
| [`record_flush`](../38-cvars-reference/07-system-misc-cvars.md#record_flush) | Если установлено, демонстрационные данные явно сбрасываются на диск во время записи. | `record_flush(int)` |
| [`registered`](../38-cvars-reference/07-system-misc-cvars.md#registered) | Установите, доступен ли pak1.pak Quake. | `registered(int)` |
| [`route_shownodes`](../38-cvars-reference/07-system-misc-cvars.md#route_shownodes) | Включает расширенную визуализацию сети waypoint-узлов в route debug-режиме клиента. | `route_shownodes(int)` |
| [`ruleset`](../38-cvars-reference/07-system-misc-cvars.md#ruleset) | Известные наборы правил: **none:** явных правил нет, разрешены все «незначительные читы»; **strict:** эквивалент набору правил smackdown. Обратите внимание, что это также заблокирует некоторые графические улучшения. | `ruleset(string)` |
| [`ruleset_allow_fbmodels`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_fbmodels) | Когда установлено значение 1, все модели отображаются в полной яркости, полностью игнорируя карты освещения. | `ruleset_allow_fbmodels(int)` |
| [`ruleset_allow_frj`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_frj) | Указывает, разрешены ли сценарии Forward-Rocket-Jump в текущем наборе правил. | `ruleset_allow_frj(int)` |
| [`ruleset_allow_in`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_in) | Переменная управляет доступностью команд `in` и `defer`, откладывающих выполнение команды на заданное время. | `ruleset_allow_in(int)` |
| [`ruleset_allow_localvolume`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_localvolume) | Разрешает использование переменной snd_playersoundvolume. | `ruleset_allow_localvolume(int)` |
| [`ruleset_allow_modified_eyes`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_modified_eyes) | Когда 0, полностью скрывает файл progs/eyes.mdl, если он не полностью идентичен ванильному Quake. | `ruleset_allow_modified_eyes(int)` |
| [`ruleset_allow_packet`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_packet) | Если 0, сетевые пакеты, отправленные с помощью команды «пакет», будут блокироваться. | `ruleset_allow_packet(int)` |
| [`ruleset_allow_particle_lightning`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_particle_lightning) | Установка 0 блоков с использованием системы частиц для замены следов молний. | `ruleset_allow_particle_lightning(int)` |
| [`ruleset_allow_playercount`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_playercount) | Указывает, разрешены ли в текущем наборе правил триггеры командной игры, учитывающие находящихся рядом игроков. | `ruleset_allow_playercount(int)` |
| [`ruleset_allow_semicheats`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_semicheats) | Если 0, блокируется ряд кваров, помеченных как получиты. | `ruleset_allow_semicheats(int)` |
| [`ruleset_allow_sensitive_texture_replacements`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_sensitive_texture_replacements) | Позволяет заменять некоторые текстуры моделей (как и сами модели). | `ruleset_allow_sensitive_texture_replacements(int)` |
| [`ruleset_allow_triggers`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_triggers) | Если значение равно 0, блокируется использование проверок msg_trigger. | `ruleset_allow_triggers(int)` |
| [`ruleset_allow_watervis`](../38-cvars-reference/07-system-misc-cvars.md#ruleset_allow_watervis) | Когда 0, это приводит к уродливой непрозрачности воды. | `ruleset_allow_watervis(int)` |
| [`saved1`](../38-cvars-reference/07-system-misc-cvars.md#saved1) | Архивируемый служебный cvar-слот для игрового кода. | `saved1(int)` |
| [`saved2`](../38-cvars-reference/07-system-misc-cvars.md#saved2) | Архивируемый служебный cvar-слот для игрового кода. | `saved2(int)` |
| [`saved3`](../38-cvars-reference/07-system-misc-cvars.md#saved3) | Архивируемый служебный cvar-слот для игрового кода. | `saved3(int)` |
| [`saved4`](../38-cvars-reference/07-system-misc-cvars.md#saved4) | Архивируемый служебный cvar-слот для игрового кода. | `saved4(int)` |
| [`savedgamecfg`](../38-cvars-reference/07-system-misc-cvars.md#savedgamecfg) | Архивируемая версия служебного gamecfg-флага. | `savedgamecfg(int)` |
| [`sb_alpha`](../38-cvars-reference/07-system-misc-cvars.md#sb_alpha) | Задаёт альфа-канал подложки строк в браузере серверов. | `sb_alpha(float)` |
| [`sb_filtertext`](../38-cvars-reference/07-system-misc-cvars.md#sb_filtertext) | Текстовый фильтр браузера серверов по имени сервера. | `sb_filtertext(string)` |
| [`sb_hidedead`](../38-cvars-reference/07-system-misc-cvars.md#sb_hidedead) | В этой ревизии cvar регистрируется и сохраняется, но код браузера серверов не использует его при построении фильтров или отрисовке. | `sb_hidedead(int)` |
| [`sb_hideempty`](../38-cvars-reference/07-system-misc-cvars.md#sb_hideempty) | Скрывает серверы без живых игроков-людей. | `sb_hideempty(int)` |
| [`sb_hidefull`](../38-cvars-reference/07-system-misc-cvars.md#sb_hidefull) | Скрывает полностью занятые серверы. | `sb_hidefull(int)` |
| [`sb_hidenetquake`](../38-cvars-reference/07-system-misc-cvars.md#sb_hidenetquake) | Скрывает из браузера серверы базового типа NetQuake. | `sb_hidenetquake(int)` |
| [`sb_hidenotempty`](../38-cvars-reference/07-system-misc-cvars.md#sb_hidenotempty) | В текущем коде браузера этот cvar только регистрируется и сохраняется. | `sb_hidenotempty(int)` |
| [`sb_hideproxies`](../38-cvars-reference/07-system-misc-cvars.md#sb_hideproxies) | Скрывает proxy-серверы в списке. | `sb_hideproxies(int)` |
| [`sb_hidequakeworld`](../38-cvars-reference/07-system-misc-cvars.md#sb_hidequakeworld) | Скрывает серверы базового типа QuakeWorld. | `sb_hidequakeworld(int)` |
| [`sb_showaddress`](../38-cvars-reference/07-system-misc-cvars.md#sb_showaddress) | Показывает в браузере серверов колонку с сетевым адресом, сформированным через Master_ServerToString(). | `sb_showaddress(int)` |
| [`sb_showgamedir`](../38-cvars-reference/07-system-misc-cvars.md#sb_showgamedir) | Показывает в списке серверов колонку gamedir. | `sb_showgamedir(int)` |
| [`sb_showmap`](../38-cvars-reference/07-system-misc-cvars.md#sb_showmap) | Показывает в браузере серверов колонку с именем текущей карты. | `sb_showmap(int)` |
| [`sb_showping`](../38-cvars-reference/07-system-misc-cvars.md#sb_showping) | Показывает колонку ping в браузере серверов и разрешает сортировку по ней. | `sb_showping(int)` |
| [`sb_showplayers`](../38-cvars-reference/07-system-misc-cvars.md#sb_showplayers) | Показывает колонку числа игроков в формате люди/maxplayers. | `sb_showplayers(int)` |
| [`sb_sortcolumn`](../38-cvars-reference/07-system-misc-cvars.md#sb_sortcolumn) | Хранит текущий столбец и направление сортировки браузера серверов. | `sb_sortcolumn(int)` |
| [`scratch1`](../38-cvars-reference/07-system-misc-cvars.md#scratch1) | Служебный cvar-слот для игрового кода, который сохраняется в файле сохранения, но не архивируется в обычный файл конфигурации. | `scratch1(int)` |
| [`scratch2`](../38-cvars-reference/07-system-misc-cvars.md#scratch2) | Служебный cvar-слот для игрового кода, который сохраняется в файле сохранения, но не архивируется в обычный файл конфигурации. | `scratch2(int)` |
| [`scratch3`](../38-cvars-reference/07-system-misc-cvars.md#scratch3) | Служебный cvar-слот для игрового кода, который сохраняется в файле сохранения, но не архивируется в обычный файл конфигурации. | `scratch3(int)` |
| [`scratch4`](../38-cvars-reference/07-system-misc-cvars.md#scratch4) | Служебный cvar-слот для игрового кода, который сохраняется в файле сохранения, но не архивируется в обычный файл конфигурации. | `scratch4(int)` |
| [`secure`](../38-cvars-reference/07-system-misc-cvars.md#secure) | Требует клиентскую валидацию перед прямым подключением. | `secure(string)` |
| [`sensitivity`](../38-cvars-reference/07-system-misc-cvars.md#sensitivity) | Задаёт базовую чувствительность мыши и масштабирует сырые mouse_x/mouse_y ещё до применения [m_yaw](../38-cvars-reference/06-ui-console-input-cvars.md#m_yaw), [m_pitch](../38-cvars-reference/06-ui-console-input-cvars.md#m_pitch), [m_forward](../38-cvars-reference/06-ui-console-input-cvars.md#m_forward) и [m_side](../38-cvars-reference/06-ui-console-input-cvars.md#m_side). | `sensitivity(int)` |
| [`show_fps`](../38-cvars-reference/07-system-misc-cvars.md#show_fps) | Отображает текущую частоту кадров на экране. | `show_fps(int)` |
| [`show_speed_x`](../38-cvars-reference/07-system-misc-cvars.md#show_speed_x) | Определяет горизонтальную позицию индикатора [show_speed](../38-cvars-reference/06-ui-console-input-cvars.md#show_speed). | `show_speed_x(int)` |
| [`show_speed_y`](../38-cvars-reference/07-system-misc-cvars.md#show_speed_y) | Определяет вертикальную позицию индикатора show_speed. | `show_speed_y(int)` |
| [`showdrop`](../38-cvars-reference/07-system-misc-cvars.md#showdrop) | Включает подробные сообщения о потерянных, устаревших и повреждённых сетевых пакетах. | `showdrop(int)` |
| [`showpackets`](../38-cvars-reference/07-system-misc-cvars.md#showpackets) | Включает трассировку входящих и исходящих пакетов netchan. | `showpackets(int)` |
| [`showpause`](../38-cvars-reference/07-system-misc-cvars.md#showpause) | Управляет отображением надписи или картинки паузы. | `showpause(int)` |
| [`showturtle`](../38-cvars-reference/07-system-misc-cvars.md#showturtle) | Включает индикатор низкой частоты кадров. | `showturtle(int)` |
| [`skin`](../38-cvars-reference/07-system-misc-cvars.md#skin) | Хранит предпочитаемый скин игрока и передаётся в userinfo; для части протоколов также используется алиас _cl_playerskin и команда playerskin. | `skin(string)` |
| [`skyroom`](../38-cvars-reference/07-system-misc-cvars.md#skyroom) | Определяет центральное положение обзора панорамного обзора. | `skyroom(string)` |
| [`slist_cacheinfo`](../38-cvars-reference/07-system-misc-cvars.md#slist_cacheinfo) | Управляет тем, насколько агрессивно браузер серверов хранит подробную информацию о серверах в памяти. | `slist_cacheinfo(int)` |
| [`slist_writeservers`](../38-cvars-reference/07-system-misc-cvars.md#slist_writeservers) | Разрешает запись списка избранных серверов на диск. | `slist_writeservers(int)` |
| [`spectator`](../38-cvars-reference/07-system-misc-cvars.md#spectator) | USERINFO-переменная, которую клиент добавляет к строке подключения при команде observe или при явном запросе режима наблюдателя. | `spectator(string)` |
| [`sw_fthreads`](../38-cvars-reference/07-system-misc-cvars.md#sw_fthreads) | Задаёт число рабочих потоков для spanqueue в программном рендерере. | `sw_fthreads(int)` |
| [`sw_interlace`](../38-cvars-reference/07-system-misc-cvars.md#sw_interlace) | Определяет степень построчной интерлейсной отрисовки в software renderer. | `sw_interlace(int)` |
| [`sw_vthread`](../38-cvars-reference/07-system-misc-cvars.md#sw_vthread) | Включает отдельный поток для commandqueue или viewpoint в программном рендерере. | `sw_vthread(int)` |
| [`team`](../38-cvars-reference/07-system-misc-cvars.md#team) | ARCHIVE\|USERINFO-строка с названием или идентификатором команды игрока. | `team(string)` |
| [`topcolor`](../38-cvars-reference/07-system-misc-cvars.md#topcolor) | ARCHIVE\|USERINFO-параметр верхнего цвета игрока, который участвует в команде color и передаётся в userinfo. | `topcolor(int)` |
| [`tp_disputablemacros`](../38-cvars-reference/07-system-misc-cvars.md#tp_disputablemacros) | Переменная относится к [teamplay](../38-cvars-reference/04-network-server-cvars.md#teamplay)-макросам, помеченным как `disputableintentions`. | `tp_disputablemacros(int)` |
| [`utf8_enable`](../38-cvars-reference/07-system-misc-cvars.md#utf8_enable) | Если установлено значение 1, встроенные функции qc изменяются так, чтобы они воздействовали на кодовые точки, а не на байты. | `utf8_enable(int)` |
| [`vk_amd_rasterization_order`](../38-cvars-reference/07-system-misc-cvars.md#vk_amd_rasterization_order) | Позволяет использовать смягченный порядок растеризации для небольшого ускорения с небольшим риском небольшого zfighting. | `vk_amd_rasterization_order(string)` |
| [`vk_busywait`](../38-cvars-reference/07-system-misc-cvars.md#vk_busywait) | Принудительное ожидание, пока графический процессор завершит свою работу. | `vk_busywait(string)` |
| [`vk_debug`](../38-cvars-reference/07-system-misc-cvars.md#vk_debug) | Зарегистрируйте обработчик отладки для отображения сообщений драйвера/уровня. 2 включает стандартные уровни проверки. | `vk_debug(int)` |
| [`vk_dualqueue`](../38-cvars-reference/07-system-misc-cvars.md#vk_dualqueue) | Попробуйте использовать для представления отдельную очередь. | `vk_dualqueue(string)` |
| [`vk_ext_astc_decode_mode`](../38-cvars-reference/07-system-misc-cvars.md#vk_ext_astc_decode_mode) | Позволяет уменьшить размер кэша текстур для текстур, сжатых LDR ASTC. | `vk_ext_astc_decode_mode(string)` |
| [`vk_stagingbuffers`](../38-cvars-reference/07-system-misc-cvars.md#vk_stagingbuffers) | Настраивает, какие динамические буферы копируются в память графического процессора для рендеринга вместо чтения из общей памяти. | `vk_stagingbuffers(string)` |
| [`vk_submissionthread`](../38-cvars-reference/07-system-misc-cvars.md#vk_submissionthread) | Выполняйте отправку+представление в потоке, предназначенном для их выполнения. | `vk_submissionthread(string)` |
| [`vk_usememorypools`](../38-cvars-reference/07-system-misc-cvars.md#vk_usememorypools) | Выделяет пулы памяти для дополнительных выделений. | `vk_usememorypools(string)` |
| [`vk_waitfence`](../38-cvars-reference/07-system-misc-cvars.md#vk_waitfence) | Ждет на заборах, а не на семафорах. | `vk_waitfence(string)` |
| [`volume`](../38-cvars-reference/07-system-misc-cvars.md#volume) | Уровень громкости игровых звуков (не влияет на музыку, голос и видеоролики). | `volume(float)` |
| [`votelevel`](../38-cvars-reference/07-system-misc-cvars.md#votelevel) | Это уровень ограничения команд, за которые игроки могут голосовать. | `votelevel(int)` |
| [`voteminimum`](../38-cvars-reference/07-system-misc-cvars.md#voteminimum) | По крайней мере, такое количество игроков должно проголосовать одинаково, чтобы голосование было признано успешным. | `voteminimum(int)` |
| [`votepercent`](../38-cvars-reference/07-system-misc-cvars.md#votepercent) | Чтобы голосование было успешным, как минимум этот процент игроков должен проголосовать одинаково. | `votepercent(int)` |
| [`votetime`](../38-cvars-reference/07-system-misc-cvars.md#votetime) | Голоса будут аннулированы по истечении указанного количества минут. | `votetime(int)` |
| [`w_switch`](../38-cvars-reference/07-system-misc-cvars.md#w_switch) | ARCHIVE\|USERINFO-переменная предпочтений автосмены оружия, почти не используемая на клиенте, но читаемая серверным builtin-кодом. | `w_switch(string)` |
| [`watervis`](../38-cvars-reference/07-system-misc-cvars.md#watervis) | Серверное объявление того, можно ли клиенту использовать watervis — расширенную видимость или отрисовку через воду. | `watervis(string)` |
| [`xinput_leftvibrator`](../38-cvars-reference/07-system-misc-cvars.md#xinput_leftvibrator) | Задаёт мощность левого вибромотора XInput-контроллера. | `xinput_leftvibrator(int)` |
| [`xinput_rightvibrator`](../38-cvars-reference/07-system-misc-cvars.md#xinput_rightvibrator) | Задаёт мощность правого вибромотора XInput-контроллера. | `xinput_rightvibrator(int)` |

---

## Ключи сущностей карты (entity keys)

Всего задокументировано: **146** ключей. Полный постатейный разбор — в разделе [«39. Ключи сущностей карты»](../README.md#ключи-сущностей-карты-entity-keys).


### Общие ключи, worldspawn и глобальные настройки уровня

| Элемент | Описание | Тип данных |
|---|---|---|
| [`classname`](../39-entity-keys-reference/01-worldspawn-common-keys.md#classname) | Главный идентификатор класса сущности. | `string` |
| [`origin`](../39-entity-keys-reference/01-worldspawn-common-keys.md#origin) | Координаты сущности в мире. | `vector("x y z")` |
| [`angle`](../39-entity-keys-reference/01-worldspawn-common-keys.md#angle) | Классический однокомпонентный Quake-ключ для задания yaw-направления. | `float` |
| [`angles`](../39-entity-keys-reference/01-worldspawn-common-keys.md#angles) | Полный трёхкомпонентный поворот сущности. | `vector("x y z")` |
| [`targetname`](../39-entity-keys-reference/01-worldspawn-common-keys.md#targetname) | Имя, по которому сущность может быть найдена и активирована другой сущностью через её `target`. | `string` |
| [`target`](../39-entity-keys-reference/01-worldspawn-common-keys.md#target) | Имя цели, которое активатор ищет среди чужих `targetname`. | `string` |
| [`target2`](../39-entity-keys-reference/01-worldspawn-common-keys.md#target2) | В классическом `quakec\basemod\defs.qc` поле `target2` не объявлено и стандартная логика basemod его не читает. | `string` |
| [`target3`](../39-entity-keys-reference/01-worldspawn-common-keys.md#target3) | Аналогично `target2`, поле `target3` не входит в набор читаемых полей basemod и не обслуживается его стандартными вспомогательными функциями. | `string` |
| [`killtarget`](../39-entity-keys-reference/01-worldspawn-common-keys.md#killtarget) | Имя набора сущностей, которые должны быть удалены в момент активации. | `string` |
| [`spawnflags`](../39-entity-keys-reference/01-worldspawn-common-keys.md#spawnflags) | Битовое поле флагов, которое один и тот же класс интерпретирует по-своему. | `integer` |
| [`model`](../39-entity-keys-reference/01-worldspawn-common-keys.md#model) | Путь или внутренний идентификатор модели сущности. | `string` |
| [`wait`](../39-entity-keys-reference/01-worldspawn-common-keys.md#wait) | Интервал ожидания между срабатыванием и повторной готовностью либо временем возврата в исходное состояние — точная семантика зависит от класса. | `float` |
| [`delay`](../39-entity-keys-reference/01-worldspawn-common-keys.md#delay) | Задержка перед реальным вызовом целей. | `float` |
| [`message`](../39-entity-keys-reference/01-worldspawn-common-keys.md#message) | Текстовое сообщение, показываемое игроку, либо строковое значение специального назначения для конкретного класса. | `string` |
| [`noise`](../39-entity-keys-reference/01-worldspawn-common-keys.md#noise) | Путь к звуковому файлу, который сущность проигрывает в момент активации. | `string` |
| [`sounds`](../39-entity-keys-reference/01-worldspawn-common-keys.md#sounds) | Числовой селектор звукового режима, но его конкретный смысл жёстко зависит от класса. | `integer` |
| [`count`](../39-entity-keys-reference/01-worldspawn-common-keys.md#count) | Счётчик требуемых активаций до выполнения действия. | `integer` |
| [`dmg`](../39-entity-keys-reference/01-worldspawn-common-keys.md#dmg) | Величина урона, которую сущность наносит при блокировке или касании. | `float` |
| [`health`](../39-entity-keys-reference/01-worldspawn-common-keys.md#health) | Количество здоровья или порог прочности, которое требуется снять, чтобы сущность умерла либо активировалась как «простреливаемая». В `trigger_multiple`, `func_button` и `func_door` наличие `health` меняет сам способ использования: вместо обычного касания объект приходится разрушать или простреливать. | `float` |
| [`style`](../39-entity-keys-reference/01-worldspawn-common-keys.md#style) | Номер [lightstyle](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#lightstyle)-анимации. | `integer` |
| [`skin`](../39-entity-keys-reference/01-worldspawn-common-keys.md#skin) | Номер скина модели. Поле существует у всех entvars, но практический смысл имеет только для тех сущностей, чья модель действительно содержит несколько вариантов внешнего вида. | `integer` |
| [`mangle`](../39-entity-keys-reference/01-worldspawn-common-keys.md#mangle) | Служебное поле для хранения «настоящих» углов там, где обычное `angles` потом сбрасывается или переиспользуется. | `vector("x y z")` |
| [`light`](../39-entity-keys-reference/01-worldspawn-common-keys.md#light) | Главное числовое значение яркости для map-света. | `integer` |
| [`speed`](../39-entity-keys-reference/01-worldspawn-common-keys.md#speed) | Скорость движения или импульса, но единицы и смысл зависят от класса. | `float` |
| [`map`](../39-entity-keys-reference/01-worldspawn-common-keys.md#map) | Имя следующей карты без расширения, куда должна вести сущность смены уровня. | `string` |
| [`lip`](../39-entity-keys-reference/01-worldspawn-common-keys.md#lip) | Остаток brush-геометрии, который не доходит до конечной точки хода и остаётся «снаружи». Для кнопок default `4`, для дверей `8`. | `float` |
| [`height`](../39-entity-keys-reference/01-worldspawn-common-keys.md#height) | Высота перемещения или вертикальная составляющая импульса. | `float` |
| [`message`](../39-entity-keys-reference/01-worldspawn-common-keys.md#message) | Текстовое сообщение, показываемое игроку, либо строковое значение специального назначения для конкретного класса. | `string` |
| [`sounds`](../39-entity-keys-reference/01-worldspawn-common-keys.md#sounds) | Числовой селектор звукового режима, но его конкретный смысл жёстко зависит от класса. | `integer` |
| [`worldtype`](../39-entity-keys-reference/01-worldspawn-common-keys.md#worldtype) | Тип мира, который выбирает тематические наборы ресурсов для части объектов. | `integer` |
| [`wad`](../39-entity-keys-reference/01-worldspawn-common-keys.md#wad) | Список WAD-архивов, использованных редактором карты. | `string` |
| [`model`](../39-entity-keys-reference/01-worldspawn-common-keys.md#model) | Путь или внутренний идентификатор модели сущности. | `string` |
| [`gravity`](../39-entity-keys-reference/01-worldspawn-common-keys.md#gravity) | Хотя поле `gravity` существует в `defs.qc`, в данном basemod оно является общим entvar-полем и не используется `worldspawn()` как читаемый map-key для глобальной настройки карты. | `float` |
| [`sky`](../39-entity-keys-reference/01-worldspawn-common-keys.md#sky) | В `quakec\basemod\defs.qc` и `world.qc` нет поля или отдельной обработки `sky` для `worldspawn`, поэтому стандартная SSQC-логика basemod не принимает решений на основе этого ключа. | `string` |
| [`MaxRange`](../39-entity-keys-reference/01-worldspawn-common-keys.md#maxrange) | В классическом наборе полей `basemod\defs.qc` ключ `MaxRange` не объявлен, а в `world.qc` и связанных общих скриптах он не читается. | `float` |

### Свет и освещение

| Элемент | Описание | Тип данных |
|---|---|---|
| [`classname`](../39-entity-keys-reference/02-light-entity-keys.md#classname) | [`classname`](../39-entity-keys-reference/01-worldspawn-common-keys.md#classname) выбирает сам тип сущности и тем самым определяет, какая логика спауна будет выполнена. | `string` |
| [`origin`](../39-entity-keys-reference/02-light-entity-keys.md#origin) | [`origin`](../39-entity-keys-reference/01-worldspawn-common-keys.md#origin) задаёт точку, из которой исходит свет. | `vector("x y z")` |
| [`light`](../39-entity-keys-reference/02-light-entity-keys.md#light) | Это главный ключ яркости. В классическом Quake-формате задаётся одним числом: `300`, `600` и т. п. | `integer or string of four numbers "R G B brightness"` |
| [`style`](../39-entity-keys-reference/02-light-entity-keys.md#style) | Привязывает источник к таблице lightstyle-анимации. | `integer` |
| [`targetname`](../39-entity-keys-reference/02-light-entity-keys.md#targetname) | Имя, по которому другие сущности карты могут обратиться к свету. | `string` |
| [`target`](../39-entity-keys-reference/02-light-entity-keys.md#target) | На световых сущностях `target` используется не как «кого включить», а как ссылка на отдельную entity-точку с совпадающим `targetname` — обычно [`info_null`](../39-entity-keys-reference/03-trigger-logic-keys.md#info_null). | `string` |
| [`spawnflags`](../39-entity-keys-reference/02-light-entity-keys.md#spawnflags) | Для обычного basemod-света подтверждён флаг `START_OFF = 1`. | `integer` |
| [`angle`](../39-entity-keys-reference/02-light-entity-keys.md#angle) | Одиночное числовое поле для упрощённой ориентации/угла. | `float` |
| [`mangle`](../39-entity-keys-reference/02-light-entity-keys.md#mangle) | Задаёт полную трёхосевую ориентацию света без отдельной цели-указателя. | `vector("pitch yaw roll")` |
| [`angles`](../39-entity-keys-reference/02-light-entity-keys.md#angles) | Более богатая версия ориентации, чем классический одиночный `angle`. | `vector("pitch yaw roll")` |
| [`cone`](../39-entity-keys-reference/02-light-entity-keys.md#cone) | Ключ `cone` синтаксически распознаётся, но не участвует в расчёте светового пятна: фактическая ширина spotlight по-прежнему берётся из `angle`. | `float` |
| [`color`](../39-entity-keys-reference/02-light-entity-keys.md#color) | Этот ключ задаёт цвет света отдельно от яркости. | `vector("r g b")` |
| [`delay`](../39-entity-keys-reference/02-light-entity-keys.md#delay) | [`delay`](../39-entity-keys-reference/01-worldspawn-common-keys.md#delay) в этом контексте не имеет отношения к задержке включения и не управляет миганием. | `integer` |
| [`wait`](../39-entity-keys-reference/02-light-entity-keys.md#wait) | В realtime-импорте `wait` — это множитель/делитель, влияющий на фактический радиус света, а не таймер и не скорость мигания. | `float` |
| [`fade`](../39-entity-keys-reference/02-light-entity-keys.md#fade) | В текущем импортёре работает как альтернативное имя для той же fade-scale-настройки, что и `wait`. | `float` |
| [`scale`](../39-entity-keys-reference/02-light-entity-keys.md#scale) | Дополнительный множитель радиуса света при realtime-импорте. | `float` |
| [`skin`](../39-entity-keys-reference/02-light-entity-keys.md#skin) | [`skin`](../39-entity-keys-reference/01-worldspawn-common-keys.md#skin) у световой сущности используется не для замены модели, а для выбора numbered light texture — проекционной маски светильника. | `integer` |
| [`pflags`](../39-entity-keys-reference/02-light-entity-keys.md#pflags) | Битовая маска дополнительных свойств realtime-света. | `integer` |
| [`light_radius`](../39-entity-keys-reference/02-light-entity-keys.md#light_radius) | Специальный override для realtime-импортёра. | `float` |

### Триггеры и логические сущности

| Элемент | Описание | Тип данных |
|---|---|---|
| [`classname`](../39-entity-keys-reference/03-trigger-logic-keys.md#classname) | [`classname`](../39-entity-keys-reference/01-worldspawn-common-keys.md#classname) выбирает, какую [spawn](../37-quakec-builtins-reference/03-entity-world-builtins.md#spawn)-логику выполнит SSQC. | `string` |
| [`targetname`](../39-entity-keys-reference/03-trigger-logic-keys.md#targetname) | Имя, по которому сущность могут найти другие объекты через их `target`. | `string` |
| [`target`](../39-entity-keys-reference/03-trigger-logic-keys.md#target) | Для большинства триггеров `target` — это имя всех получателей, которым будет отправлено событие через `SUB_UseTargets`. | `string` |
| [`target2`](../39-entity-keys-reference/03-trigger-logic-keys.md#target2) | В исследованных id1-подобных исходниках `quakec\basemod` поле [`target2`](../39-entity-keys-reference/01-worldspawn-common-keys.md#target2) не используется триггерной логикой как стандартный сценарный вход. | `string` |
| [`killtarget`](../39-entity-keys-reference/03-trigger-logic-keys.md#killtarget) | Если `killtarget` задан, `SUB_UseTargets` сначала ищет все сущности, у которых `targetname` совпадает с этим значением, и удаляет их из мира. | `string` |
| [`message`](../39-entity-keys-reference/03-trigger-logic-keys.md#message) | Текст, который `SUB_UseTargets` печатает в центре экрана, если активатором был игрок. | `string` |
| [`sounds`](../39-entity-keys-reference/03-trigger-logic-keys.md#sounds) | [`sounds`](../39-entity-keys-reference/01-worldspawn-common-keys.md#sounds) выбирает заранее прошитый звуковой вариант срабатывания. | `integer` |
| [`noise`](../39-entity-keys-reference/03-trigger-logic-keys.md#noise) | В basemod [`noise`](../39-entity-keys-reference/01-worldspawn-common-keys.md#noise) чаще всего заполняется самой spawn-логикой на основе `sounds`, но поле также участвует в общем поведении сообщений. | `string` |
| [`wait`](../39-entity-keys-reference/03-trigger-logic-keys.md#wait) | Для `trigger_multiple` стандартное значение — `0.2` секунды: после срабатывания триггер временно блокируется и затем активируется снова. | `float` |
| [`delay`](../39-entity-keys-reference/03-trigger-logic-keys.md#delay) | Откладывает не само касание и не вход игрока в brush, а момент вызова целей. | `float` |
| [`health`](../39-entity-keys-reference/03-trigger-logic-keys.md#health) | Если [`health`](../39-entity-keys-reference/01-worldspawn-common-keys.md#health) задан у `trigger_multiple` или `trigger_once`, триггер перестаёт реагировать на касание и становится разрушаемым объектом: его нужно «убить», чтобы он сработал. | `float` |
| [`count`](../39-entity-keys-reference/03-trigger-logic-keys.md#count) | Задаёт, сколько входных активаций должен накопить `trigger_counter`, прежде чем он действительно вызовет свои цели. | `integer` |
| [`dmg`](../39-entity-keys-reference/03-trigger-logic-keys.md#dmg) | [`dmg`](../39-entity-keys-reference/01-worldspawn-common-keys.md#dmg) определяет, сколько очков урона наносит `trigger_hurt` за одно касание-тик. | `float` |
| [`speed`](../39-entity-keys-reference/03-trigger-logic-keys.md#speed) | У `trigger_push` [`speed`](../39-entity-keys-reference/01-worldspawn-common-keys.md#speed) задаёт силу толчка; если ключ отсутствует, используется `1000`. | `float` |
| [`height`](../39-entity-keys-reference/03-trigger-logic-keys.md#height) | [`height`](../39-entity-keys-reference/01-worldspawn-common-keys.md#height) определяет вертикальную скорость, которую `trigger_monsterjump` придаёт монстру при успешном касании. | `float` |
| [`map`](../39-entity-keys-reference/03-trigger-logic-keys.md#map) | Содержит имя следующей карты, на которую должен перевести игрока `trigger_changelevel`. | `string` |
| [`angle`](../39-entity-keys-reference/03-trigger-logic-keys.md#angle) | Классический однокомпонентный Quake-ключ для задания yaw-направления. | `float` |
| [`angles`](../39-entity-keys-reference/03-trigger-logic-keys.md#angles) | Нужен там, где одних yaw-углов мало или где редактор/мод уже работает с полным вектором ориентации. | `vector` |
| [`origin`](../39-entity-keys-reference/03-trigger-logic-keys.md#origin) | [`origin`](../39-entity-keys-reference/01-worldspawn-common-keys.md#origin) задаёт мировые координаты point-entity. | `vector` |
| [`model`](../39-entity-keys-reference/03-trigger-logic-keys.md#model) | У brush-триггеров [`model`](../39-entity-keys-reference/01-worldspawn-common-keys.md#model) — это внутренний BSP-модельный объём, который компилятор карты создаёт для каждой brush-сущности. | `string` |
| [`spawnflags`](../39-entity-keys-reference/03-trigger-logic-keys.md#spawnflags) | [`spawnflags`](../39-entity-keys-reference/01-worldspawn-common-keys.md#spawnflags) — битовая маска дополнительных режимов поведения. | `integer` |
| [`spawnflags`](../39-entity-keys-reference/03-trigger-logic-keys.md#spawnflags-notouch) | Бит `NOTOUCH` имеет значение `1` и отключает реакцию на прямое касание. | `integer` |
| [`spawnflags`](../39-entity-keys-reference/03-trigger-logic-keys.md#spawnflags-nomessage) | У `trigger_counter` бит `NOMESSAGE` также равен `1`, но смысл уже другой: suppress промежуточных и финального сообщений игроку. | `integer` |
| [`spawnflags`](../39-entity-keys-reference/03-trigger-logic-keys.md#spawnflags-player_only) | У `trigger_teleport` бит `PLAYER_ONLY` имеет значение `1` и разрешает перенос только сущностям с `classname "player"`. | `integer` |
| [`spawnflags`](../39-entity-keys-reference/03-trigger-logic-keys.md#spawnflags-silent) | Бит `SILENT` у `trigger_teleport` имеет значение `2` и отключает постоянный эмбиентный гул телепорта в месте самого триггера. | `integer` |
| [`spawnflags`](../39-entity-keys-reference/03-trigger-logic-keys.md#spawnflags-push_once) | Бит `PUSH_ONCE` у `trigger_push` имеет значение `1`. | `integer` |
| [`spawnflags`](../39-entity-keys-reference/03-trigger-logic-keys.md#spawnflags-no_intermission) | У id1-style `trigger_changelevel` бит `NO_INTERMISSION` имеет значение `1`. | `integer` |
| [`path_corner`](../39-entity-keys-reference/03-trigger-logic-keys.md#path_corner) | Хотя `path_corner` — это `classname`, его стоит воспринимать и как специальный тип целевой точки. | `string` |
| [`info_notnull`](../39-entity-keys-reference/03-trigger-logic-keys.md#info_notnull) | Простая point-entity без собственной активной логики, но в отличие от `info_null` она не удаляется на спауне. | `string` |
| [`info_null`](../39-entity-keys-reference/03-trigger-logic-keys.md#info_null) | В исследованной id1-логике немедленно удаляет себя при спауне. | `string` |

### Двери, платформы и подвижная геометрия

| Элемент | Описание | Тип данных |
|---|---|---|
| [`classname`](../39-entity-keys-reference/04-func-brush-entity-keys.md#classname) | [`classname`](../39-entity-keys-reference/01-worldspawn-common-keys.md#classname) выбирает [spawn](../37-quakec-builtins-reference/03-entity-world-builtins.md#spawn)-логику сущности. | `string` |
| [`targetname`](../39-entity-keys-reference/04-func-brush-entity-keys.md#targetname) | [`targetname`](../39-entity-keys-reference/01-worldspawn-common-keys.md#targetname) делает сущность адресуемой для чужого [`target`](../39-entity-keys-reference/01-worldspawn-common-keys.md#target). | `string` |
| [`target`](../39-entity-keys-reference/04-func-brush-entity-keys.md#target) | У `func_door`, `func_door_secret` и `func_button` ключ `target` обрабатывается общей процедурой `SUB_UseTargets`: при срабатывании сущность находит все объекты с совпадающим `targetname` и вызывает их логику использования. | `string` |
| [`message`](../39-entity-keys-reference/04-func-brush-entity-keys.md#message) | [`message`](../39-entity-keys-reference/01-worldspawn-common-keys.md#message) — текстовое сообщение для игрока, но точный момент его показа зависит от класса. | `string` |
| [`killtarget`](../39-entity-keys-reference/04-func-brush-entity-keys.md#killtarget) | Если задан [`killtarget`](../39-entity-keys-reference/01-worldspawn-common-keys.md#killtarget), при срабатывании сущность сначала удаляет из мира все объекты с соответствующим `targetname`, а уже потом продолжает остальную цепочку событий. | `string` |
| [`delay`](../39-entity-keys-reference/04-func-brush-entity-keys.md#delay) | Откладывает не само движение brush-сущности, а момент передачи событий через `target` и `killtarget`. | `float` |
| [`angle`](../39-entity-keys-reference/04-func-brush-entity-keys.md#angle) | [`angle`](../39-entity-keys-reference/01-worldspawn-common-keys.md#angle) задаёт направление движения в редакторном формате Quake: обычно это yaw в градусах, а специальные значения `-1` и `-2` обозначают движение строго вверх и строго вниз. | `float` |
| [`angles`](../39-entity-keys-reference/04-func-brush-entity-keys.md#angles) | Некоторые редакторы или внешние `.ent`-файлы записывают ориентацию не через одиночный `angle`, а через полный вектор [`angles`](../39-entity-keys-reference/01-worldspawn-common-keys.md#angles). | `vector` |
| [`speed`](../39-entity-keys-reference/04-func-brush-entity-keys.md#speed) | [`speed`](../39-entity-keys-reference/01-worldspawn-common-keys.md#speed) задаёт скорость линейного перемещения в units per second. | `float` |
| [`wait`](../39-entity-keys-reference/04-func-brush-entity-keys.md#wait) | Почти везде измеряется в секундах, но смысл зависит от класса. | `float` |
| [`lip`](../39-entity-keys-reference/04-func-brush-entity-keys.md#lip) | Это часть brush-а в units, которая остаётся видимой после завершения движения. | `float` |
| [`dmg`](../39-entity-keys-reference/04-func-brush-entity-keys.md#dmg) | [`dmg`](../39-entity-keys-reference/01-worldspawn-common-keys.md#dmg) определяет, сколько урона сущность наносит при блокировке своего движения. | `float` |
| [`sounds`](../39-entity-keys-reference/04-func-brush-entity-keys.md#sounds) | [`sounds`](../39-entity-keys-reference/01-worldspawn-common-keys.md#sounds) — это числовой селектор заранее прошитых звуковых наборов, а не путь к аудиофайлу. | `integer` |
| [`health`](../39-entity-keys-reference/04-func-brush-entity-keys.md#health) | Если [`health`](../39-entity-keys-reference/01-worldspawn-common-keys.md#health) задан, сущность перестаёт быть обычным touch-активатором и становится shootable-объектом. | `float` |
| [`height`](../39-entity-keys-reference/04-func-brush-entity-keys.md#height) | [`height`](../39-entity-keys-reference/01-worldspawn-common-keys.md#height) задаёт, на сколько units платформа должна опуститься от верхней позиции к нижней. | `float` |
| [`t_width`](../39-entity-keys-reference/04-func-brush-entity-keys.md#t_width) | Переопределяет первую фазу движения секретной двери. | `float` |
| [`t_length`](../39-entity-keys-reference/04-func-brush-entity-keys.md#t_length) | Переопределяет вторую фазу движения секретной двери — боковое или продольное скольжение после первого шага. | `float` |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-start_open) | Флаг `START_OPEN` имеет числовое значение `1`. | `integer` |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-door_dont_link) | Флаг `DOOR_DONT_LINK` имеет значение `4`. | `integer` |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-gold_key) | Флаг `GOLD_KEY` имеет значение `8` и делает дверь ключевой. | `integer` |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-silver_key) | Флаг `SILVER_KEY` имеет значение `16` и работает так же, как золотой вариант, но требует серебряный ключевой предмет. | `integer` |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-toggle) | Флаг `TOGGLE` имеет значение `32`. | `integer` |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-open_once) | У `func_door_secret` флаг `open_once` имеет значение `1`. | `integer` |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-1st_left) | Флаг `1st_left` имеет значение `2` и меняет знак первого бокового смещения секретной двери. | `integer` |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-1st_down) | Флаг `1st_down` имеет значение `4` и заменяет первый боковой шаг вертикальным ходом вниз. | `integer` |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-no_shoot) | Флаг `no_shoot` имеет значение `8` и отключает открытие секретной двери уроном. | `integer` |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-always_shoot) | Флаг `always_shoot` имеет значение `16` и делает секретную дверь shootable даже в том случае, когда у неё задан `targetname`. | `integer` |
| [`spawnflags`](../39-entity-keys-reference/04-func-brush-entity-keys.md#spawnflags-plat_low_trigger) | Для `func_plat` флаг `PLAT_LOW_TRIGGER` имеет значение `1` и меняет форму внутреннего trigger-объёма платформы. | `integer` |

### Монстры, NPC и точки появления игрока

| Элемент | Описание | Тип данных |
|---|---|---|
| [`classname`](../39-entity-keys-reference/05-monster-player-keys.md#classname) | Выбирает конкретную spawn-функцию монстра: `monster_army`, `monster_ogre`, `monster_wizard` и так далее. | `string` |
| [`origin`](../39-entity-keys-reference/05-monster-player-keys.md#origin) | [`origin`](../39-entity-keys-reference/01-worldspawn-common-keys.md#origin) задаёт стартовую позицию монстра на карте. | `vector` |
| [`angle`](../39-entity-keys-reference/05-monster-player-keys.md#angle) | Классический одиночный [`angle`](../39-entity-keys-reference/01-worldspawn-common-keys.md#angle) задаёт стартовый yaw монстра. | `float` |
| [`angles`](../39-entity-keys-reference/05-monster-player-keys.md#angles) | [`angles`](../39-entity-keys-reference/01-worldspawn-common-keys.md#angles) хранит полный вектор ориентации, но для обычной monster-AI в basemod практически значима только yaw-компонента. | `vector` |
| [`health`](../39-entity-keys-reference/05-monster-player-keys.md#health) | Каждая spawn-функция монстра записывает собственное базовое здоровье: например, солдат получает `30`, Ogre `200`, Demon `300`, Zombie `60`. | `float` |
| [`spawnflags`](../39-entity-keys-reference/05-monster-player-keys.md#spawnflags) | Главное картографическое поле для особых режимов старта монстра. | `integer` |
| [`target`](../39-entity-keys-reference/05-monster-player-keys.md#target) | Если у монстра задан `target`, общая стартовая логика ищет сущность с совпадающим `targetname` и сохраняет её как `movetarget`/`goalentity`. | `string` |
| [`targetname`](../39-entity-keys-reference/05-monster-player-keys.md#targetname) | Делает монстра адресуемой сущностью для остальной логики карты. | `string` |
| [`yaw_speed`](../39-entity-keys-reference/05-monster-player-keys.md#yaw_speed) | Задаёт скорость разворота монстра. | `float` |
| [`items`](../39-entity-keys-reference/05-monster-player-keys.md#items) | Поле `items` существует в общих entvars, поэтому его технически можно записать в сущность карты. | `integer` |
| [`model`](../39-entity-keys-reference/05-monster-player-keys.md#model) | Хотя [`model`](../39-entity-keys-reference/01-worldspawn-common-keys.md#model) является обычным полем сущности, стандартные spawn-функции монстров почти всегда сами вызывают [`setmodel()`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setmodel) и жёстко назначают нужный `.mdl`. | `string` |
| [`classname`](../39-entity-keys-reference/05-monster-player-keys.md#classname) | Выбирает конкретную spawn-функцию монстра: `monster_army`, `monster_ogre`, `monster_wizard` и так далее. | `string` |
| [`origin`](../39-entity-keys-reference/05-monster-player-keys.md#origin) | [`origin`](../39-entity-keys-reference/01-worldspawn-common-keys.md#origin) задаёт стартовую позицию монстра на карте. | `vector` |
| [`angle`](../39-entity-keys-reference/05-monster-player-keys.md#angle) | Классический одиночный [`angle`](../39-entity-keys-reference/01-worldspawn-common-keys.md#angle) задаёт стартовый yaw монстра. | `float` |
| [`angles`](../39-entity-keys-reference/05-monster-player-keys.md#angles) | [`angles`](../39-entity-keys-reference/01-worldspawn-common-keys.md#angles) хранит полный вектор ориентации, но для обычной monster-AI в basemod практически значима только yaw-компонента. | `vector` |
| [`target`](../39-entity-keys-reference/05-monster-player-keys.md#target) | Если у монстра задан `target`, общая стартовая логика ищет сущность с совпадающим `targetname` и сохраняет её как `movetarget`/`goalentity`. | `string` |
| [`targetname`](../39-entity-keys-reference/05-monster-player-keys.md#targetname) | Делает монстра адресуемой сущностью для остальной логики карты. | `string` |
| [`spawnflags`](../39-entity-keys-reference/05-monster-player-keys.md#spawnflags) | Главное картографическое поле для особых режимов старта монстра. | `integer` |
| [`mangle`](../39-entity-keys-reference/05-monster-player-keys.md#mangle) | [`mangle`](../39-entity-keys-reference/01-worldspawn-common-keys.md#mangle) в basemod используется у некоторых других сущностей, например intermission-камеры, но не у обычных `info_player_*` стартов. | `vector` |
| [`health`](../39-entity-keys-reference/05-monster-player-keys.md#health) | Каждая spawn-функция монстра записывает собственное базовое здоровье: например, солдат получает `30`, Ogre `200`, Demon `300`, Zombie `60`. | `float` |
| [`model`](../39-entity-keys-reference/05-monster-player-keys.md#model) | Хотя [`model`](../39-entity-keys-reference/01-worldspawn-common-keys.md#model) является обычным полем сущности, стандартные spawn-функции монстров почти всегда сами вызывают [`setmodel()`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setmodel) и жёстко назначают нужный `.mdl`. | `string` |

### Предметы и оружие

| Элемент | Описание | Тип данных |
|---|---|---|
| [`classname`](../39-entity-keys-reference/06-item-weapon-keys.md#classname) | Выбирает конкретную логику спавна: модель, размер trigger-box, тип награды и правила подбора. | `string` |
| [`origin`](../39-entity-keys-reference/06-item-weapon-keys.md#origin) | [`origin`](../39-entity-keys-reference/01-worldspawn-common-keys.md#origin) задаёт стартовую позицию предмета на карте. | `vector` |
| [`angle`](../39-entity-keys-reference/06-item-weapon-keys.md#angle) | [`angle`](../39-entity-keys-reference/01-worldspawn-common-keys.md#angle) — редакторная сокращённая запись для одного yaw-угла. | `float` |
| [`angles`](../39-entity-keys-reference/06-item-weapon-keys.md#angles) | [`angles`](../39-entity-keys-reference/01-worldspawn-common-keys.md#angles) задаёт полный набор pitch/yaw/roll и нужен, когда одной yaw-компоненты недостаточно. | `vector` |
| [`spawnflags`](../39-entity-keys-reference/06-item-weapon-keys.md#spawnflags) | Это главный класс-специфичный битовый ключ для предметов. | `integer` |
| [`target`](../39-entity-keys-reference/06-item-weapon-keys.md#target) | Если предмет был успешно подобран, после выдачи награды вызывается стандартная цепочка `target`. | `string` |
| [`killtarget`](../39-entity-keys-reference/06-item-weapon-keys.md#killtarget) | Обрабатывается той же общей логикой, что и `target`, но сначала удаляет все сущности с совпавшим `targetname`, а уже потом продолжает остальную цепочку. | `string` |
| [`delay`](../39-entity-keys-reference/06-item-weapon-keys.md#delay) | [`delay`](../39-entity-keys-reference/01-worldspawn-common-keys.md#delay) не откладывает сам pickup: игрок получает здоровье, патроны, броню или powerup сразу. | `float` |
| [`message`](../39-entity-keys-reference/06-item-weapon-keys.md#message) | Выводится в [`centerprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#centerprint) игроку-активатору в момент выполнения общей target-цепочки. | `string` |
| [`targetname`](../39-entity-keys-reference/06-item-weapon-keys.md#targetname) | У самих предметов и оружия нет собственной use-логики для «активации по имени», поэтому `targetname` здесь в основном пассивный идентификатор для внешних сущностей. | `string` |
| [`wait`](../39-entity-keys-reference/06-item-weapon-keys.md#wait) | Хотя поле `wait` существует в общих entvars, item/weapon-spawner-ы basemod его не используют. | `float` |
| [`count`](../39-entity-keys-reference/06-item-weapon-keys.md#count) | Не управляет объёмом награды у map-предметов basemod. | `float` |
| [`effects`](../39-entity-keys-reference/06-item-weapon-keys.md#effects) | Классы powerup-артефактов частично опираются на `effects` для визуальной индикации: Pentagram добавляет красный glow-эффект, а Quad/OctaPower — синий. | `integer` |

---

## Директивы языка материалов (.shader)

Всего задокументировано: **75** директив. Полный постатейный разбор — в разделе [«40. Директивы языка материалов»](../README.md#директивы-языка-материалов-shader).


### Директивы уровня материала

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`cull`](../40-shader-directives-reference/01-shader-toplevel-directives.md#cull) | Управляет тем, будет ли поверхность видна только с лицевой стороны или с обеих сторон. | `cull disable\|none\|twosided\|front\|back\|backside\|backsided` |
| [`skyparms`](../40-shader-directives-reference/01-shader-toplevel-directives.md#skyparms) | Помечает материал как небо и автоматически переводит его в очередь `sort sky`. | `skyparms farbox height nearbox` |
| [`fogparms`](../40-shader-directives-reference/01-shader-toplevel-directives.md#fogparms) | Задаёт цвет и глубину тумана для материала. | `fogparms (r g b) depth` |
| [`surfaceparm`](../40-shader-directives-reference/01-shader-toplevel-directives.md#surfaceparm) | В FTEQW `surfaceparm` — это контейнерная директива: сам синтаксис один и тот же, а смысл определяется следующим словом. | `surfaceparm keyword` |
| [`nomipmaps`](../40-shader-directives-reference/01-shader-toplevel-directives.md#nomipmaps) | Отключает мип-уровни у материала. | `nomipmaps` |
| [`nopicmip`](../40-shader-directives-reference/01-shader-toplevel-directives.md#nopicmip) | Запрещает глобальным настройкам качества понижать разрешение текстуры материала. | `nopicmip` |
| [`polygonoffset`](../40-shader-directives-reference/01-shader-toplevel-directives.md#polygonoffset) | Просит рендер слегка сместить материал по глубине, чтобы он не конфликтовал с почти совпадающей геометрией под ним. | `polygonoffset [scale]` |
| [`sort`](../40-shader-directives-reference/01-shader-toplevel-directives.md#sort) | Задаёт, когда материал будет рисоваться относительно других поверхностей. | `sort portal\|sky\|opaque\|decal\|litdecal\|seethrough\|unlitdecal\|banner\|underwater\|blend\|additive\|nearest\|ripple\|deferredlight\|number` |
| [`deformvertexes`](../40-shader-directives-reference/01-shader-toplevel-directives.md#deformvertexes) | Включает геометрическую деформацию всего материала. | `deformvertexes type ...` |
| [`portal`](../40-shader-directives-reference/01-shader-toplevel-directives.md#portal) | Переводит материал в очередь `sort portal`. | `portal` |
| [`entitymergable`](../40-shader-directives-reference/01-shader-toplevel-directives.md#entitymergable) | Разрешает движку считать материал пригодным для объединения нескольких сущностей в более крупные batches. | `entitymergable` |
| [`clutter`](../40-shader-directives-reference/01-shader-toplevel-directives.md#clutter) | FTE-расширение для автоматического «засева» поверхности мелкими объектами вроде травы, камешков или мусора. | `clutter model spacing scalemin scalemax zofs anglemin anglemax` |
| [`deferredlight`](../40-shader-directives-reference/01-shader-toplevel-directives.md#deferredlight) | Переводит материал в специальную очередь `sort deferredlight`. | `deferredlight` |
| [`affine`](../40-shader-directives-reference/01-shader-toplevel-directives.md#affine) | Подсказка backend'у использовать affine-style interpolation hint на всех проходах материала. | `affine` |
| [`fullrate`](../40-shader-directives-reference/01-shader-toplevel-directives.md#fullrate) | Запрещает для материала half-rate shading и просит backend шейдить его в полном темпе. | `fullrate` |
| [`diffusemap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#diffusemap) | Заполняет стандартный слот базовой текстуры материала без явного развёртывания ручной стадии. | `diffusemap path` |
| [`normalmap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#normalmap) | Заполняет стандартный bump/normal-слот материала. | `normalmap path` |
| [`specularmap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#specularmap) | Заполняет слот блеска материала. | `specularmap path` |
| [`fullbrightmap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#fullbrightmap) | Задаёт карту участков, которые должны выглядеть самосветящимися независимо от обычного освещения сцены. | `fullbrightmap path` |
| [`uppermap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#uppermap) | Задаёт стандартный слот верхнего overlay-слоя, обычно связанного с top-color или подобной системой перекраски модели. | `uppermap path` |
| [`lowermap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#lowermap) | Парная директива к `uppermap`, отвечающая за нижнюю зону перекраски. | `lowermap path` |
| [`reflectmask`](../40-shader-directives-reference/01-shader-toplevel-directives.md#reflectmask) | Задаёт карту, по которой шейдер решает, какие участки поверхности отражают окружение сильнее, а какие слабее. | `reflectmask path` |
| [`displacementmap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#displacementmap) | Заполняет специальный слот карты смещения. | `displacementmap path` |
| [`transmissionmap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#transmissionmap) | Задаёт текстуру для материалов, у которых часть света должна проходить сквозь толщу и окрашиваться ею — например, листья, ткань на просвет, тонкий воск, кожа или цветное стекло в расширенных шейдерах. | `transmissionmap path` |
| [`thicknessmap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#thicknessmap) | Дополняет transmission-ориентированные материалы данными о локальной толщине. | `thicknessmap path` |
| [`program`](../40-shader-directives-reference/01-shader-toplevel-directives.md#program) | Подключает к материалу пользовательскую шейдерную программу для текущего активного renderer backend'а. | `program name` |
| [`glslprogram`](../40-shader-directives-reference/01-shader-toplevel-directives.md#glslprogram) | Специализированный вариант `program`, который жёстко ориентирован на OpenGL/GLSL-путь. | `glslprogram name` |
| [`hlslprogram`](../40-shader-directives-reference/01-shader-toplevel-directives.md#hlslprogram) | Делает то же, что `program`, но адресует Direct3D 9-совместимый HLSL backend. | `hlslprogram name` |
| [`hlsl11program`](../40-shader-directives-reference/01-shader-toplevel-directives.md#hlsl11program) | Вариант той же идеи для Direct3D 11. | `hlsl11program name` |
| [`portalfboscale`](../40-shader-directives-reference/01-shader-toplevel-directives.md#portalfboscale) | Регулирует разрешение вспомогательного буфера, который используется для порталов, зеркал, отражений и преломлений. | `portalfboscale scale` |

### Директивы уровня стадии

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`map`](../40-shader-directives-reference/02-shader-stage-directives.md#map) | Задаёт основной источник изображения для стадии. | `map <textureOrSpecial>` |
| [`animmap`](../40-shader-directives-reference/02-shader-stage-directives.md#animmap) | Создаёт кадровую анимацию за счёт последовательной подмены всей текстуры стадии. | `animmap <fps> <frame1> <frame2> ...` |
| [`clampmap`](../40-shader-directives-reference/02-shader-stage-directives.md#clampmap) | Делает то же, что и `map`, но принудительно запрещает повторение текстуры за пределами диапазона 0..1. | `clampmap <textureOrSpecial>` |
| [`videoMap`](../40-shader-directives-reference/02-shader-stage-directives.md#videomap) | Подставляет вместо обычной картинки видеопоток и обновляет его по мере проигрывания. | `videoMap <videoFile>` |
| [`cubemap`](../40-shader-directives-reference/02-shader-stage-directives.md#cubemap) | Привязывает кубическую текстуру. | `cubemap <cubeTexture>` |
| [`cameracubemap`](../40-shader-directives-reference/02-shader-stage-directives.md#cameracubemap) | Doom 3-совместимый алиас `cubemap`. | `cameracubemap <cubeTexture>` |
| [`surroundmap`](../40-shader-directives-reference/02-shader-stage-directives.md#surroundmap) | QFusion/Warsow-совместимый алиас `cubemap`. | `surroundmap <cubeTexture>` |
| [`blendfunc`](../40-shader-directives-reference/02-shader-stage-directives.md#blendfunc) | Определяет, как цвет стадии объединяется с тем, что уже находится в framebuffer. | `blendfunc <preset>` |
| [`blend`](../40-shader-directives-reference/02-shader-stage-directives.md#blend) | Классическая альфа-прозрачность: эквивалент `gl_src_alpha gl_one_minus_src_alpha`. | `blend <presetOrFactors>` |
| [`rgbGen`](../40-shader-directives-reference/02-shader-stage-directives.md#rgbgen) | Управляет тем, как стадия модифицирует цвет своей текстуры перед смешиванием. | `rgbGen <mode> [args...]` |
| [`alphaGen`](../40-shader-directives-reference/02-shader-stage-directives.md#alphagen) | Задаёт, насколько прозрачной должна быть стадия до применения `blendfunc` или `alphafunc`. | `alphaGen <mode> [args...]` |
| [`alphaShift`](../40-shader-directives-reference/02-shader-stage-directives.md#alphashift) | Совместимый shorthand из Alien Arena. | `alphaShift <speed> <min> <max>` |
| [`depthfunc`](../40-shader-directives-reference/02-shader-stage-directives.md#depthfunc) | Меняет правило, по которому стадия сравнивает свою глубину с уже существующим z-buffer. | `depthfunc <mode>` |
| [`depthwrite`](../40-shader-directives-reference/02-shader-stage-directives.md#depthwrite) | Принудительно включает запись глубины для этой стадии. | `depthwrite` |
| [`nodepthtest`](../40-shader-directives-reference/02-shader-stage-directives.md#nodepthtest) | Отключает проверку глубины для стадии. | `nodepthtest` |
| [`nodepth`](../40-shader-directives-reference/02-shader-stage-directives.md#nodepth) | Парсер FTEQW принимает `nodepth` как совместимую stage-команду, но в текущей реализации она не выставляет отдельный собственный per-pass режим сравнения глубины, как это делает `nodepthtest`. | `nodepth` |
| [`alphafunc`](../40-shader-directives-reference/02-shader-stage-directives.md#alphafunc) | Включает бинарное отсечение пикселей по alpha-каналу до обычного полупрозрачного смешивания. | `alphafunc <mode>` |
| [`alphaMask`](../40-shader-directives-reference/02-shader-stage-directives.md#alphamask) | Alien Arena-совместимый shorthand, который принудительно устанавливает тот же пороговый режим, что и `alphafunc ge128`. | `alphaMask` |
| [`alphaTest`](../40-shader-directives-reference/02-shader-stage-directives.md#alphatest) | Doom 3-совместимая форма порогового альфа-теста. | `alphaTest 0.5` |
| [`tcMod`](../40-shader-directives-reference/02-shader-stage-directives.md#tcmod) | Последовательно модифицирует текстурные координаты стадии. | `tcMod <mode> [args...]` |
| [`scale`](../40-shader-directives-reference/02-shader-stage-directives.md#scale) | Масштабирует UV по двум осям. | `scale <x> <y>` |
| [`scroll`](../40-shader-directives-reference/02-shader-stage-directives.md#scroll) | Постоянно сдвигает текстуру по U и V со скоростями `speedS` и `speedT`. | `scroll static <x> static <y>` |
| [`tcGen`](../40-shader-directives-reference/02-shader-stage-directives.md#tcgen) | Заменяет обычные UV-координаты автоматически вычисляемыми координатами другого типа. | `tcGen <mode> [args...]` |
| [`texgen`](../40-shader-directives-reference/02-shader-stage-directives.md#texgen) | Имя `texgen` в FTEQW встречается в двух совместимых традициях. | `texgen <mode> [args...]` |
| [`envmap`](../40-shader-directives-reference/02-shader-stage-directives.md#envmap) | Старый совместимый shorthand, который просто выставляет тот же режим координат, что и `tcGen environment`. | `envmap` |
| [`detail`](../40-shader-directives-reference/02-shader-stage-directives.md#detail) | Помечает стадию как detail-проход. | `detail` |
| [`nolightmap`](../40-shader-directives-reference/02-shader-stage-directives.md#nolightmap) | Alien Arena-совместимая stage-команда, которая просто переключает `rgbGen` в `identity`. | `nolightmap` |
| [`program`](../40-shader-directives-reference/02-shader-stage-directives.md#program) | [`program`](../40-shader-directives-reference/01-shader-toplevel-directives.md#program) назначает стадии программный GPU-проход по имени. | `program <programName>` |
| [`maskcolor`](../40-shader-directives-reference/02-shader-stage-directives.md#maskcolor) | Включает запись только в цветовые каналы RGB. | `maskcolor` |
| [`maskred`](../40-shader-directives-reference/02-shader-stage-directives.md#maskred) | Разрешает стадии писать только в красный канал. | `maskred` |
| [`maskgreen`](../40-shader-directives-reference/02-shader-stage-directives.md#maskgreen) | Разрешает запись только в зелёный канал. | `maskgreen` |
| [`maskblue`](../40-shader-directives-reference/02-shader-stage-directives.md#maskblue) | Разрешает запись только в синий канал. | `maskblue` |
| [`maskalpha`](../40-shader-directives-reference/02-shader-stage-directives.md#maskalpha) | Разрешает стадии записывать только альфа-канал framebuffer. | `maskalpha` |
| [`red`](../40-shader-directives-reference/02-shader-stage-directives.md#red) | Doom 3-совместимый shorthand, который переводит стадию в постоянный `rgbGen const` и задаёт красную компоненту. | `red <value>` |
| [`green`](../40-shader-directives-reference/02-shader-stage-directives.md#green) | Задаёт зелёную компоненту постоянного цвета стадии в Doom 3-совместимом стиле. | `green <value>` |
| [`blue`](../40-shader-directives-reference/02-shader-stage-directives.md#blue) | Задаёт синюю компоненту постоянного цвета стадии. | `blue <value>` |
| [`alpha`](../40-shader-directives-reference/02-shader-stage-directives.md#alpha) | Doom 3-совместимый shorthand для `alphaGen const`. | `alpha <value>` |
| [`map16`](../40-shader-directives-reference/02-shader-stage-directives.md#map16) | RTCW-совместимая условная форма `map`. | `map16 <textureOrSpecial>` |
| [`map32`](../40-shader-directives-reference/02-shader-stage-directives.md#map32) | Противоположная RTCW-ветка по сравнению с `map16`. | `map32 <textureOrSpecial>` |
| [`mapcomp`](../40-shader-directives-reference/02-shader-stage-directives.md#mapcomp) | RTCW-совместимая условная карта для случая, когда S3TC/BC3-компрессия поддерживается и включена. | `mapcomp <textureOrSpecial>` |
| [`mapnocomp`](../40-shader-directives-reference/02-shader-stage-directives.md#mapnocomp) | RTCW-совместимая альтернатива `mapcomp`: она активна только когда компрессия текстур не используется. | `mapnocomp <textureOrSpecial>` |
| [`animcompmap`](../40-shader-directives-reference/02-shader-stage-directives.md#animcompmap) | RTCW-совместимая условная версия `animmap`. | `animcompmap <fps> <frame1> <frame2> ...` |
| [`animnocompmap`](../40-shader-directives-reference/02-shader-stage-directives.md#animnocompmap) | Fallback-пара к `animcompmap`. | `animnocompmap <fps> <frame1> <frame2> ...` |
| [`animclampmap`](../40-shader-directives-reference/02-shader-stage-directives.md#animclampmap) | QFusion/Warsow-совместимая комбинация `animmap` + clamp. | `animclampmap <fps> <frame1> <frame2> ...` |
| [`material`](../40-shader-directives-reference/02-shader-stage-directives.md#material) | QFusion/Warsow-совместимая компактная форма объявления «набора PBR-подобных карт» одной строкой. | `material <baseTexture> <normalMap> <specularMap>` |

---

## Директивы языка частиц (.particles)

Всего задокументировано: **76** директив. Полный постатейный разбор — в разделе [«41. Директивы языка частиц»](../README.md#директивы-языка-частиц-particles).


### Директивы эффекта

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`shader`](../41-particle-directives-reference/01-particle-effect-directives.md#shader) | Директива назначает частице полноценный материал вместо простой текстуры. | `shader [shaderName]` |
| [`texture`](../41-particle-directives-reference/01-particle-effect-directives.md#texture) | Это базовый способ выбрать картинку частицы. | `texture path` |
| [`tcoords`](../41-particle-directives-reference/01-particle-effect-directives.md#tcoords) | Директива выбирает прямоугольник внутри уже назначенной текстуры. | `tcoords s1 t1 s2 t2 [tscale] [rsmax] [rsstep]` |
| [`atlas`](../41-particle-directives-reference/01-particle-effect-directives.md#atlas) | Упрощённая альтернатива `tcoords`, когда текстура уже равномерно разбита на сетку. | `atlas dims firstIndex [lastIndex]` |
| [`rotation`](../41-particle-directives-reference/01-particle-effect-directives.md#rotation) | Задаёт и стартовый поворот спрайта, и скорость его дальнейшего вращения. | `rotation startMin [startMax] speedMin [speedMax]` |
| [`beamtexstep`](../41-particle-directives-reference/01-particle-effect-directives.md#beamtexstep) | Директива используется для `type beam` и задаёт плотность укладки текстуры вдоль луча. | `beamtexstep unitsPerRepeat` |
| [`beamtexspeed`](../41-particle-directives-reference/01-particle-effect-directives.md#beamtexspeed) | Эта директива тоже относится к `type beam`. | `beamtexspeed scrollSpeed` |
| [`scale`](../41-particle-directives-reference/01-particle-effect-directives.md#scale) | Это основной размер спрайта, искры, декали или звена луча. | `scale minSize [maxSize]` |
| [`scalefactor`](../41-particle-directives-reference/01-particle-effect-directives.md#scalefactor) | `1` ведёт себя как обычный world-space размер, `0` — как экранный (без перспективного уменьшения). | `scalefactor factor` |
| [`scaledelta`](../41-particle-directives-reference/01-particle-effect-directives.md#scaledelta) | Положительное значение раздувает частицу со временем, отрицательное — сжимает. | `scaledelta unitsPerSecond` |
| [`stretchfactor`](../41-particle-directives-reference/01-particle-effect-directives.md#stretchfactor) | Параметр влияет прежде всего на искровые режимы (`spark`, `sparkfan`, `texturedspark`) и управляет тем, насколько частица вытягивается по направлению движения. | `stretchfactor factor [minFactor]` |
| [`count`](../41-particle-directives-reference/01-particle-effect-directives.md#count) | Хотя `count` относится к спавну, визуально он определяет плотность слоя: редкие искры, густой дым, массивная взрывная шапка. | `count baseCount [randCount] [absoluteExtra]` |
| [`alpha`](../41-particle-directives-reference/01-particle-effect-directives.md#alpha) | Это основной уровень непрозрачности частицы. | `alpha baseAlpha [maxAlpha] [delta]` |
| [`alpharand`](../41-particle-directives-reference/01-particle-effect-directives.md#alpharand) | Не задаёт максимум напрямую, а именно добавляет случайный диапазон к уже вычисленной базовой альфе. | `alpharand range` |
| [`alphadelta`](../41-particle-directives-reference/01-particle-effect-directives.md#alphadelta) | Парсер пишет это значение напрямую в скорость изменения прозрачности. | `alphadelta unitsPerSecond` |
| [`die`](../41-particle-directives-reference/01-particle-effect-directives.md#die) | Директива задаёт срок жизни частицы. | `die maxTime [minTime]` |
| [`assoc`](../41-particle-directives-reference/01-particle-effect-directives.md#assoc) | Связывает текущий слой с другим эффектом, который должен запускаться одновременно. | `assoc effectName` |
| [`colorindex`](../41-particle-directives-reference/01-particle-effect-directives.md#colorindex) | Вместо прямого RGB можно использовать индекс классической палитры Quake/Hexen II. | `colorindex paletteIndex [range]` |
| [`rgb`](../41-particle-directives-reference/01-particle-effect-directives.md#rgb) | Это основная директива прямой окраски частицы. | `rgb r [g b]` |
| [`rgbdelta`](../41-particle-directives-reference/01-particle-effect-directives.md#rgbdelta) | Директива анимирует цвет частицы во времени. | `rgbdelta rDelta [gDelta bDelta]` |
| [`rgbrand`](../41-particle-directives-reference/01-particle-effect-directives.md#rgbrand) | Добавляет каждому каналу собственное случайное отклонение и делает группу частиц менее однородной. | `rgbrand rRange [gRange bRange]` |
| [`rgbrandsync`](../41-particle-directives-reference/01-particle-effect-directives.md#rgbrandsync) | Эта директива управляет тем, насколько случайность цвета должна быть общей между каналами, а не полностью независимой. | `rgbrandsync rSync [gSync bSync]` |
| [`stains`](../41-particle-directives-reference/01-particle-effect-directives.md#stains) | Директива включает оставление следа на поверхности при ударе частицы. | `stains amount` |
| [`blend`](../41-particle-directives-reference/01-particle-effect-directives.md#blend) | Определяет, как пиксели частицы объединяются с уже нарисованной сценой. | `blend mode` |
| [`type`](../41-particle-directives-reference/01-particle-effect-directives.md#type) | Это главная директива выбора способа отрисовки. | `type renderType` |
| [`clippeddecal`](../41-particle-directives-reference/01-particle-effect-directives.md#clippeddecal) | Директива включает clipped decal и одновременно задаёт фильтр по surface flags карты. | `clippeddecal mask [match]` |
| [`cliptype`](../41-particle-directives-reference/01-particle-effect-directives.md#cliptype) | Относится к collision-логике, но используется именно ради визуального результата при ударе: вспышки рикошета, мелкого пылящего облака, брызг и вторичных искр. | `cliptype effectName` |
| [`rampmode`](../41-particle-directives-reference/01-particle-effect-directives.md#rampmode) | Рампа — это последовательность ключевых состояний цвета/альфы/размера. | `rampmode mode` |
| [`rampindex`](../41-particle-directives-reference/01-particle-effect-directives.md#rampindex) | Добавляет один шаг рампы на основе палитрового индекса. | `rampindex paletteIndex [alpha] [scale]` |
| [`ramp`](../41-particle-directives-reference/01-particle-effect-directives.md#ramp) | Строит рампу напрямую в RGB, без палитровых индексов. | `ramp r [g b [alpha [scale]]]` |
| [`lightradius`](../41-particle-directives-reference/01-particle-effect-directives.md#lightradius) | Эта директива заставляет слой порождать временный динамический источник света. | `lightradius minRadius [maxRadius]` |
| [`lightradiusfade`](../41-particle-directives-reference/01-particle-effect-directives.md#lightradiusfade) | Параметр задаёт, как быстро сжимается динамический свет после появления. | `lightradiusfade unitsPerSecond` |
| [`lightrgb`](../41-particle-directives-reference/01-particle-effect-directives.md#lightrgb) | Окрашивает именно динамический свет, а не сам спрайт частицы. | `lightrgb r g b` |
| [`lightcorona`](../41-particle-directives-reference/01-particle-effect-directives.md#lightcorona) | Помимо обычного динамического света, слой может добавить видимую «корону» — яркий экранный ореол вокруг источника. | `lightcorona intensity scale` |
| [`lighttime`](../41-particle-directives-reference/01-particle-effect-directives.md#lighttime) | Ограничивает срок существования временного источника света. | `lighttime seconds` |
| [`spawnstain`](../41-particle-directives-reference/01-particle-effect-directives.md#spawnstain) | В отличие от `stains`, эта директива создаёт stain сразу в точке появления эффекта, не дожидаясь удара частицы о поверхность. | `spawnstain radius r g b` |

### Директивы поведения и появления

| Элемент | Описание | Сигнатура |
|---|---|---|
| [`randomvel`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#randomvel) | Сокращённая запись для комбинации `velwrand` и частично `velbias`. | `randomvel horizontal [vertical]` |
| [`veladd`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#veladd) | Масштабирует входной вектор эффекта — например, направление трассы, удара или выброса. | `veladd base [max]` |
| [`orgadd`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#orgadd) | Не ускоряет частицу, а двигает саму стартовую точку спауна вдоль оси эффекта. | `orgadd base [max]` |
| [`orgbias`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#orgbias) | Добавляет фиксированное смещение уже после вычисления обычного спауна. | `orgbias x y z` |
| [`velbias`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#velbias) | Добавляет неизменную мировую скорость после всех случайных разбросов формы спауна. | `velbias x y z` |
| [`orgwrand`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#orgwrand) | Добавляет независимый от `spawnmode` случайный сдвиг в мировом пространстве. | `orgwrand x y z` |
| [`velwrand`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#velwrand) | Добавляет случайную скорость в мировых координатах, независимо от локальной оси эффекта. | `velwrand x y z` |
| [`friction`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#friction) | На каждом кадре движок умножает скорость на `1 - friction * frametime`, поэтому `friction` задаёт именно темп затухания, а не мгновенный множитель. | `friction xyz` |
| [`gravity`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#gravity) | Каждый кадр вычитает из Z-скорости величину `gravity * frametime`. | `gravity value` |
| [`flurry`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#flurry) | Не влияет на стартовый спаун, а каждый кадр добавляет случайное отклонение по X/Y к уже летящей частице. | `flurry value` |
| [`assoc`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#assoc) | [`assoc`](../41-particle-directives-reference/01-particle-effect-directives.md#assoc) связывает несколько самостоятельных описаний под одним вызовом: когда основной эффект спавнится, движок параллельно вызывает и связанный. | `assoc effectName` |
| [`inwater`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#inwater) | Это переключатель варианта эффекта по содержимому среды. | `inwater effectName` |
| [`underwater`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#underwater) | Разрешает спаун только внутри перечисленного содержимого. | `underwater [contents ...]` |
| [`notunderwater`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#notunderwater) | Работает зеркально к `underwater`: эффект пропускается, если точка находится внутри перечисленного содержимого. | `notunderwater [contents ...]` |
| [`spawnmode`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#spawnmode) | Центральная директива формы спауна. | `spawnmode mode [param1] [param2]` |
| [`spawntime`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#spawntime) | Ограничивает частоту появления эффекта, если у него есть trailstate. | `spawntime seconds` |
| [`spawnchance`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#spawnchance) | Перед созданием частиц движок сравнивает `spawnchance` со случайным числом `0..1`. | `spawnchance chance` |
| [`step`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#step) | Предназначен для trail- и beam-подобных эффектов, где частицы нужно раскладывать вдоль сегмента движения, а не порождать все в одной точке. | `step distance [randomDistance] [extraCount]` |
| [`cliptype`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#cliptype) | [`cliptype`](../41-particle-directives-reference/01-particle-effect-directives.md#cliptype) превращает столкновение частицы в триггер нового эффекта. | `cliptype effectName` |
| [`clipcount`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#clipcount) | Когда `cliptype` вызывает другой эффект, `clipcount` масштабирует его интенсивность. | `clipcount multiplier` |
| [`clipbounce`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#clipbounce) | Если `cliptype` указывает на сам эффект, `clipbounce` работает как обычный коэффициент отскока: нормальная компонента скорости отражается и масштабируется этим числом. | `clipbounce value` |
| [`bounce`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#bounce) | Короткая запись для случая «эта же частица должна отскакивать сама от себя». По сути директива выставляет `cliptype` в собственное имя эффекта и задаёт `clipbounce`. | `bounce value` |
| [`emit`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#emit) | Делает частицу мини-эмиттером. | `emit effectName` |
| [`emitinterval`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#emitinterval) | Определяет, как часто частица с `emit` создаёт вторичный эффект. | `emitinterval seconds` |
| [`emitintervalrand`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#emitintervalrand) | После каждого дочернего спауна движок берёт базовый `emitinterval` и добавляет к нему случайную величину от `0` до `emitintervalrand`. | `emitintervalrand seconds` |
| [`emitstart`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#emitstart) | Откладывает первый запуск `emit` после рождения частицы. | `emitstart seconds` |
| [`spawnorg`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#spawnorg) | Задаёт геометрию области рождения, а точный смысл зависит от `spawnmode`. | `spawnorg horizontal [vertical]` |
| [`spawnvel`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#spawnvel) | Даёт частицам локальную скорость, направленную по форме выбранного `spawnmode`: из шара наружу, по окружности, по спирали, вбок у tracer-следа и так далее. | `spawnvel horizontal [vertical]` |
| [`stretchfactor`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#stretchfactor) | Относится к поведению частицы в полёте: длина спрайта-искры начинает зависеть от скорости, а не только от статического размера. | `stretchfactor factor [minLength]` |
| [`spawnparam1`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#spawnparam1) | Устаревшая форма прямой записи первого параметра режима спауна. | `spawnparam1 value` |
| [`spawnparam2`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#spawnparam2) | Такой же устаревший алиас для третьего аргумента `spawnmode`. | `spawnparam2 value` |
| [`up`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#up) | Устаревший короткий алиас, который просто записывает значение в `orgbias[2]`. | `up value` |
| [`viewspace`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#viewspace) | Интерполирует положение и скорость частицы в координаты камеры. | `viewspace [fraction]` |
| [`perframe`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#perframe) | Включает специальный режим, при котором внутреннее количество спауна делится на `host_frametime`. | `perframe` |
| [`averageout`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#averageout) | Влияет на trail-эффекты: движок подправляет шаг между точками так, чтобы частицы в среднем распределялись от начала до конца сегмента более равномерно. | `averageout` |
| [`nostate`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#nostate) | Запрещает использовать trailstate для этого эмиттера. | `nostate` |
| [`nospreadfirst`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#nospreadfirst) | У trail-эффектов `nospreadfirst` подавляет случайный разброс для первой порождённой частицы сегмента. | `nospreadfirst` |
| [`nospreadlast`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#nospreadlast) | Делает то же самое для последней частицы trail-сегмента. | `nospreadlast` |
| [`rainfrequency`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#rainfrequency) | Используется не обычными точечными спаунами, а системой атмосферных осадков, которая распределяет частицы по поверхностям неба. | `rainfrequency multiplier` |
| [`placeholder`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#placeholder) | Переводит эффект в специальный режим-заглушку. | `placeholder` |

---

## Параметры командной строки

Полный постатейный справочник — в разделе [«44. Команды и параметры командной строки»](../README.md#команды-и-параметры-командной-строки). Таблицы намеренно не объединены — это параметры двух разных программ.

### FTEQW (движок)

| Элемент | Назначение | Сигнатура |
|---|---|---|
| [`-basedir`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#basedir) | Переопределяет корневую папку игровых данных и фиксирует базовый каталог для автоопределения манифеста. | `<path>` |
| [`-basepack`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#basepack) | Добавляет внешний pack/APK в базовый поиск данных; флаг можно повторять, а второй аргумент задаёт префикс внутри архива. | `<archive> [prefix]` |
| [`-basegame`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#basegame) | Дописывает `basegame` в активный манифест; флаг можно повторять несколько раз. | `<directory>` |
| [`-game`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#game) | Добавляет или заменяет активный `gamedir` в манифесте; флаг можно повторять. | `<directory>` |
| [`-manifest`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#manifest) | Загружает указанный манифест вместо чистого автоопределения и одновременно отключает клиентский диалог ручного поиска каталога. | `<file.fmf>` |
| [`-homedir`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#homedir) | Явно задаёт домашний каталог FTEQW для пользовательских файлов и включает его использование. | `<path>` |
| [`-usehome`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#usehome) | Принудительно включает домашний каталог, если он определим. | — |
| [`-nohome`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nohome) | Полностью отключает домашний каталог FTEQW. | — |
| [`-readonly`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#readonly) | Переводит файловую систему в режим только для чтения. | — |
| [`-allowfileuri`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#allowfileuri) | Разрешает создание системного search path для `file:` URI и прямого доступа к локальным путям ОС. | — |
| [`-allowfileurl`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#allowfileurl) | Синоним `-allowfileuri` с тем же поведением. | — |
| [`-unsafefopen`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#unsafefopen) | Ослабляет префикс `data/` для QuakeC-файловых операций, если путь не считается песочницей-обязательным. | — |
| [`-safe`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#safe) | Включает безопасный режим: при разборе argv движок автоматически дописывает `-stdvid -nolan -nosound -nocdaudio -nojoy -nomouse -nohome -window`. | — |
| [`-set`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#set) | Поздний аналог консольной команды `set`, исполняемый через буфер команд на этапе стартовых конфигов. | `<cvar> <value>` |
| [`-seta`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#seta) | Поздний аналог `seta`; команда попадает в командный буфер при стартовой загрузке. | `<cvar> <value>` |
| [`-exec`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#exec) | Добавляет раннее выполнение указанного конфига как консольной команды `exec`. | `<file.cfg>` |
| [`-watch`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#watch) | Помечает cvar как watched для отслеживания изменений после регистрации переменных. | `<cvar>` |
| [`-lang`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#lang) | Переопределяет язык интерфейса/переводов поверх переменных окружения и системной локали. | `<locale>` |
| [`-translatetoblank`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#translatetoblank) | Разрешает загружать пустые строки перевода из PO/MO-файлов, не отбрасывая их как «пустые». | — |
| [`-noworker`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#noworker) | Отключает фоновые worker threads, фиксируя `worker_count` в 0. | — |
| [`-noworkers`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#noworkers) | Полный синоним `-noworker`. | — |
| [`-nodaz`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nodaz) | Отключает SSE-режимы FTZ/DAZ и тем самым возвращает обработку денормализованных float, если моды на них полагаются. | — |
| [`-window`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#window) | Стартует в оконном режиме, сбрасывая `vid_fullscreen` в 0. | — |
| [`-startwindowed`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#startwindowed) | Ещё один стартовый способ принудить оконный режим. | — |
| [`-fullscreen`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#fullscreen) | Принудительно включает полноэкранный режим (`vid_fullscreen 1`). | — |
| [`-width`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#width) | Задаёт стартовую ширину видеорежима; если высота не задана отдельно, движок вычисляет её как 4:3. | `<pixels>` |
| [`-height`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#height) | Задаёт стартовую высоту видеорежима. | `<pixels>` |
| [`-conwidth`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#conwidth) | Задаёт ширину консольного/софтварного framebuffer; без `-conheight` высота выводится как 4:3. | `<pixels>` |
| [`-conheight`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#conheight) | Задаёт высоту консольного/софтварного framebuffer. | `<pixels>` |
| [`-bpp`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#bpp) | Переопределяет стартовую глубину цвета (`vid_bpp`). | `<bit>` |
| [`-current`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#current) | Просит использовать текущие десктопные настройки вместо подбора отдельного fullscreen-режима. | — |
| [`-particles`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#particles) | Задаёт стартовый лимит частиц; значение подхватывают и cvar-override, и классический particle backend. | `<number>` |
| [`-qmenu`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#qmenu) | Форсирует использование классического Quake-меню (`forceqmenu 1`). | — |
| [`-noenumerate`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#noenumerate) | Переводит перечисление рендереров/аудиоустройств в safe-ветку без полного опроса драйверов. | — |
| [`-no8bit`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#no8bit) | Отключает использование `GL_EXT_shared_texture_palette`, даже если драйвер её поддерживает. | — |
| [`-noamtex`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#noamtex) | Запрещает включать `GL_ARB_multitexture`. | — |
| [`-stayactive`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#stayactive) | На GLX-сборках не возвращает исходный fullscreen-режим при уходе приложения в фон. | — |
| [`-novmode`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#novmode) | Только Linux/X11: запрещает переключение полноэкранного режима через XF86VidMode, даже если функция доступна. | — |
| [`-forcevmode`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#forcevmode) | Только Linux/X11: принудительно запрашивает переключение полноэкранного режима через XF86VidMode, обходя cvar `x11_allow_vmode`; реальный результат всё равно зависит от библиотек и драйвера. | — |
| [`-noxrandr`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#noxrandr) | Только Linux/X11: запрещает обработку мониторов и режимов через XRandR, даже если функция доступна. | — |
| [`-forcexrandr`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#forcexrandr) | Только Linux/X11: принудительно запрашивает обработку мониторов и режимов через XRandR, обходя cvar `x11_allow_xrandr`; реальный результат всё равно зависит от библиотек и драйвера. | — |
| [`-noxim`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#noxim) | Только Linux/X11: запрещает поддержку X Input Method, даже если функция доступна. | — |
| [`-forcexim`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#forcexim) | Только Linux/X11: принудительно запрашивает поддержку X Input Method, обходя cvar `x11_allow_xim`; реальный результат всё равно зависит от библиотек и драйвера. | — |
| [`-noxcursor`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#noxcursor) | Только Linux/X11: запрещает аппаратный X11-курсор, даже если функция доступна. | — |
| [`-forcexcursor`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#forcexcursor) | Только Linux/X11: принудительно запрашивает аппаратный X11-курсор, обходя cvar `x11_allow_xcursor`; реальный результат всё равно зависит от библиотек и драйвера. | — |
| [`-nowmfullscreen`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nowmfullscreen) | Только Linux/X11: запрещает полноэкранный режим через window manager/EWMH, даже если функция доступна. | — |
| [`-forcewmfullscreen`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#forcewmfullscreen) | Только Linux/X11: принудительно запрашивает полноэкранный режим через window manager/EWMH, обходя cvar `x11_allow_wmfullscreen`; реальный результат всё равно зависит от библиотек и драйвера. | — |
| [`-noxi2`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#noxi2) | Только Linux/X11: запрещает ввод через XInput2, даже если функция доступна. | — |
| [`-forcexi2`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#forcexi2) | Только Linux/X11: принудительно запрашивает ввод через XInput2, обходя cvar `x11_allow_xi2`; реальный результат всё равно зависит от библиотек и драйвера. | — |
| [`-nodga`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nodga) | Только Linux/X11: запрещает мышиный ввод через DGA, даже если функция доступна. | — |
| [`-forcedga`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#forcedga) | Только Linux/X11: принудительно запрашивает мышиный ввод через DGA, обходя cvar `x11_allow_dga`; реальный результат всё равно зависит от библиотек и драйвера. | — |
| [`-nomouse`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nomouse) | Полностью пропускает инициализацию мыши. | — |
| [`-noforcemspd`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#noforcemspd) | Не форсирует системный параметр mouse speed и включает `m_accel_noforce`. | — |
| [`-noforcemaccel`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#noforcemaccel) | Не форсирует системные mouse threshold/acceleration параметры и включает `m_threshold_noforce`. | — |
| [`-noforcemparms`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#noforcemparms) | Комбинирует поведение `-noforcemspd` и `-noforcemaccel`. | — |
| [`-dinput`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#dinput) | Включает DirectInput через `in_dinput 1`. | — |
| [`-cddev`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#cddev) | Переопределяет путь к устройству CD Audio. | `<device>` |
| [`-nocdaudio`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nocdaudio) | Отключает CD Audio на старте. | — |
| [`-cdaudio`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#cdaudio) | Явно включает CD Audio, если код собран с `HAVE_CDPLAYER`. | — |
| [`-noopenal`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#noopenal) | Запрещает backend OpenAL. | — |
| [`-noalsa`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#noalsa) | Запрещает backend ALSA. | — |
| [`-nooss`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nooss) | Запрещает backend OSS и его перечисление/захват. | — |
| [`-nopulse`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nopulse) | Запрещает backend PulseAudio. | — |
| [`-nosdlsnd`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nosdlsnd) | Запрещает SDL audio backend. | — |
| [`-nosdl`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nosdl) | В контексте аудио действует как дополнительный запрет SDL audio backend. | — |
| [`-nosound`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nosound) | Полностью отключает звуковую подсистему, фиксируя `nosound 1` ещё при инициализации. | — |
| [`-soundspeed`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#soundspeed) | Переопределяет стартовую частоту микшера через `snd_khz`. | `<kHz>` |
| [`-sspeed`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#sspeed) | Синоним `-soundspeed`. | `<kHz>` |
| [`-sndspeed`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#sndspeed) | Ещё один синоним `-soundspeed`. | `<kHz>` |
| [`-snoforceformat`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#snoforceformat) | Только DirectSound: не пытается выставить формат primary buffer до выбора secondary/primary path. | — |
| [`-primarysound`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#primarysound) | Только DirectSound: разрешает предпочесть primary sound buffer вместо secondary. | — |
| [`-wavonly`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#wavonly) | Только DirectSound: полностью отключает DirectSound backend, оставляя wav/альтернативные пути. | — |
| [`-dsp`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#dsp) | Только Sound Blaster backend: вручную ограничивает версию DSP. | `<2\|3\|4>` |
| [`-ip`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#ip) | Привязывает сетевые сокеты к конкретному IPv4-адресу интерфейса; используется и обычной сетью, и встроенными HTTP/FTP-серверами. | `<address>` |
| [`-clport`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#clport) | Задаёт локальный UDP-порт клиента вместо автоматического выбора. | `<port>` |
| [`-svport`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#svport) | Переопределяет `sv_port` и `sv_port_tcp`. | `<port>` |
| [`-port`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#port) | Синоним `-svport` для серверного порта. | `<port>` |
| [`-dedicated`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#dedicated) | Запускает dedicated server path вместо клиентского режима. | — |
| [`-chroot`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#chroot) | Только Linux/Unix dedicated server: перед запуском пытается сменить корневой каталог процесса на указанный путь. | `<path>` |
| [`-uid`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#uid) | Только Linux/Unix dedicated server: после `-chroot` или SUID-сценария сбрасывает привилегии до указанного UID. | `<uid>` |
| [`-nostdin`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nostdin) | Запрещает чтение stdin/консольного ввода. | — |
| [`-noconinput`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#noconinput) | Только несdl Unix-клиент: отключает консольный stdin даже при наличии tty. | — |
| [`-nostdout`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nostdout) | Только несdl Unix-клиент: запрещает обычный stdout-спам. | — |
| [`-clusterslave`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#clusterslave) | Запускает процесс как подчинённый узел map-cluster и переводит управление на pipe/remote controller. | — |
| [`-clusterhost`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#clusterhost) | На dedicated-сервере подключается к удалённому cluster host и передаёт пароль управления. | `<addr:port> <password>` |
| [`-allowmapless`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#allowmapless) | Разрешает dedicated-серверу не падать мгновенно, если стартовая карта не загрузилась/не скачалась. | — |
| [`-cheats`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#cheats) | Поднимает `sv_cheats` в 1 при старте сервера. | — |
| [`-mysql`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#mysql) | Разрешает загружать MySQL-драйвер; без флага код SQL намеренно не трогает MySQL из соображений sandbox/security. | — |
| [`-noq2dll`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#noq2dll) | Запрещает загрузку Quake II game DLL и заставляет Q2 gamecode инициализацию завершиться неуспехом. | — |
| [`-color`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#color) | Только Unix dedicated server: принудительно включает ANSI-цвета stdout, даже если stdout не tty. | — |
| [`-colour`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#colour) | Британский синоним `-color` с тем же эффектом. | — |
| [`-nocolor`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nocolor) | Только Unix dedicated server: принудительно выключает ANSI-цвета stdout. | — |
| [`-nocolour`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nocolour) | Синоним `-nocolor`. | — |
| [`-nomutex`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nomutex) | Только Windows-клиент и только при `QUAKESPYAPI`: не создаёт именованный mutex `qwcl`, который фронтенд использует для обнаружения запущенного клиента. | — |
| [`-noreset`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#noreset) | Только Windows dedicated server: отключает автоперезапуск после фатальной ошибки и меняет crash-handling path. | — |
| [`-register`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#register) | Только Windows dedicated server с `USESERVICE`: регистрирует сервер как системную службу. | — |
| [`-unregister`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#unregister) | Только Windows dedicated server с `USESERVICE`: удаляет зарегистрированную системную службу. | — |
| [`-register_types`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#register_types) | Только Windows-клиент: сразу запускает регистрацию file associations / URI schemes и затем завершает процесс. | `[layout]` |
| [`-install`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#install) | Только Unix server path с `MANIFESTDOWNLOADS`: сначала выполняет установку пакетов, затем продолжает запуск. | — |
| [`-doinstall`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#doinstall) | Запускает установщик пакетов и после применения изменений завершает процесс. | — |
| [`-updatesrc`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#updatesrc) | Добавляет пользовательский источник package list; флаг можно повторять. | `<url\|list>` |
| [`-unsafe`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#unsafe) | Используется только вместе с `-updatesrc`: помечает добавленный источник как unsafe (`SRCFL_UNSAFE`). | — |
| [`-notlstrust`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#notlstrust) | Отключает специальное доверие к зеркалам с официального update-site и заставляет обычную проверку подписи/сертификата. | — |
| [`-noupdate`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#noupdate) | Блокирует автообновление движка и package-list phone-home. | — |
| [`-noupdates`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#noupdates) | Клиентский синоним `-noupdate` для логики package-list query. | — |
| [`-noautoupdate`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#noautoupdate) | Ещё один запрет автообновления движка. | — |
| [`-noupdate-double-dash`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#noupdate-double-dash) | Двухдефисный псевдоним `-noupdate`, используемый теми же путями обновления. | — |
| [`-noautoupdate-double-dash`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#noautoupdate-double-dash) | Двухдефисный псевдоним `-noautoupdate`. | — |
| [`-allowupdate`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#allowupdate) | Снимает защитный запрет на self-update для нестандартно именованных бинарников/сборок. | — |
| [`-noplugins`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#noplugins) | Полностью запрещает автозагрузку внешних engine plugins. | — |
| [`-nodlls`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nodlls) | Запрещает загрузку native DLL gamecode/QVM DLL path. | — |
| [`-nosos`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nosos) | Unix-ориентированный синоним `-nodlls`, запрещающий `.so` native gamecode. | — |
| [`-plugin`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#plugin) | Переводит процесс в plugin-host режим; на Windows отдельное значение `qcdebug` в следующем аргументе меняет подрежим. | `[name]` |
| [`-qcdebug`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#qcdebug) | Запускает специальный QC/plugin debug mode с урезанным stdout/особым `isPlugin`. | — |
| [`-plugwrapper-double-dash`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#plugwrapper-double-dash) | Служебный двухдефисный режим прямого запуска plugin wrapper entrypoint и немедленного выхода из обычного startup path. | `<dll/so> <entry>` |
| [`-v`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#v) | Только Windows-клиент: печатает `version:` и завершает процесс. | — |
| [`-version-double-dash`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#version-double-dash) | Только Windows-клиент: длинный вариант `-v`. | — |
| [`-outputdebugstring`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#outputdebugstring) | Только Windows-клиент: дублирует отладочный вывод через `OutputDebugString` path. | — |
| [`-crashonerror`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#crashonerror) | Только SDL entrypoint: превращает `Sys_Error` в преднамеренный крэш для отладчика/дампа. | — |
| [`-debugip`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#debugip) | Только в `CRAZYDEBUGGING`-сборках: отправляет debug log на TCP `ip:10000` вместо локального файла. | `<ip>` |
| [`-watchdog`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#watchdog) | Только Windows/MSVC-сборки с `CATCHCRASH` и `MULTITHREAD`: запускает отдельный watchdog thread. | — |
| [`-makeinstaller`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#makeinstaller) | Только Windows-клиент в сборках с `WEBCLIENT`: создаёт инсталлятор/самоупакованный exe из `<name>.fmf` и `<name>.png`. | `<name>` |
| [`-fromfrontend-double-dash`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#fromfrontend-double-dash) | Legacy-only путь обновления фронтенда: используется только в сборках с `HAVE_LEGACY` и нужен для выбора файла launcher при self-update. | `<rev> <launcher>` |
| [`-notls`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#notls) | Полностью отключает инициализацию TLS-провайдера GnuTLS. | — |
| [`-privkey`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#privkey) | Указывает путь к приватному PEM-ключу для TLS/подписи; без флага используются стандартные имена в FS_ROOT. | `<file>` |
| [`-pubkey`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#pubkey) | Указывает путь к публичному сертификату/цепочке PEM. | `<file>` |
| [`-certhost`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#certhost) | Задаёт CN/issuer для автогенерируемого сертификата и также попадает в режим подписи пакетов. | `<hostname>` |
| [`-pfx`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#pfx) | Только Windows SSPI/TLS build: задаёт PFX/PKCS#12-файл identity-сертификата вместо `identity.pfx`. | `<file.pfx>` |
| [`-prefix`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#prefix) | Только Unix meta-helper path: добавляет строковый префикс при расчёте qhash/подписи архива. | `<string>` |
| [`-sign`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#sign) | Только Unix meta-helper path: печатает старый `sign=`/`sha512=` набор для пакета. | `<file>` |
| [`-sign2`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#sign2) | Только Unix meta-helper path: печатает сырую base64-подпись файла. | `<file>` |
| [`-signraw`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#signraw) | Синоним `-sign2`. | `<file>` |
| [`-signfmfpkg`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#signfmfpkg) | Только Unix meta-helper path: печатает подпись в формате пакетов FMF (`prefix/filesize/sha512/signature`). | `<file>` |
| [`-qhash`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#qhash) | Только Unix meta-helper path: печатает Quake pure CRC/qhash указанного архива. | `<file>` |
| [`-sha1`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#sha1) | Только Unix meta-helper path: печатает SHA-1 файла. | `<file>` |
| [`-sha256`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#sha256) | Только Unix meta-helper path: печатает SHA-256 файла. | `<file>` |
| [`-sha512`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#sha512) | Только Unix meta-helper path: печатает SHA-512 файла. | `<file>` |
| [`-nodumpstack`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nodumpstack) | Отключает установку friendly crash handler / stack dump handler. | — |
| [`-fileul`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#fileul) | Статус: недоступно в сборке по умолчанию. | — |
| [`-ip6`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#ip6) | Статус: недоступно в сборке по умолчанию. | `<address>` |
| [`-nomtex`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nomtex) | Статус: недоступно в сборке по умолчанию. | — |
| [`-zone`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#zone) | Статус: legacy/inactive. | `<kb>` |
| [`-stdvid`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#stdvid) | Статус: legacy/inactive — флаг автоматически подставляется `-safe`. | — |
| [`-nolan`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nolan) | Статус: legacy/inactive — присутствует только в списке safe-аргументов. | — |
| [`-nojoy`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nojoy) | Статус: legacy/inactive — присутствует только в списке safe-аргументов. | — |
| [`-quake_rerel`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#quake_rerel) | Выбирает преднастроенный режим Quake Re-Release (`QuakeEX.kpf`). | — |
| [`-quake`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#quake) | Выбирает стандартный режим Quake/id1. | — |
| [`-afterquake`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#afterquake) | Альтернативное имя режима Quake для отдельного install-name/реестра. | — |
| [`-netquake`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#netquake) | Выбирает netquake-совместимый режим без QW-специфичных допущений. | — |
| [`-spasm`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#spasm) | Выбирает FauxSpasm/QuakeSpasm-совместимый режим. | — |
| [`-fitz`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#fitz) | Выбирает FauxFitz-режим совместимости. | — |
| [`-tenebrae`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#tenebrae) | Выбирает FauxTenebrae-режим совместимости. | — |
| [`-ezquake`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#ezquake) | Выбирает ezQuake-совместимый режим/набор настроек. | — |
| [`-quake2`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#quake2) | Выбирает режим Quake II / `baseq2`. | — |
| [`-dday`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#dday) | Выбирает преднастроенный режим D-Day: Normandy на базе Quake II. | — |
| [`-hipnotic`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#hipnotic) | Выбирает режим Quake Mission Pack 1; дополнительно форсирует hipnotic HUD/layout, если флаг задан явно. | — |
| [`-rogue`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#rogue) | Выбирает режим Quake Mission Pack 2; дополнительно форсирует rogue HUD/layout, если флаг задан явно. | — |
| [`-dopa`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#dopa) | Выбирает режим Dimensions of the Past. | — |
| [`-mg1`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#mg1) | Выбирает режим Dimension of the Machine. | — |
| [`-quoth`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#quoth) | Выбирает режим Quoth и связанные compat-хаки. | — |
| [`-nehahra`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nehahra) | Выбирает режим Seal of Nehahra. | — |
| [`-librequake`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#librequake) | Выбирает режим LibreQuake. | — |
| [`-portals`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#portals) | Выбирает режим Hexen II Mission Pack / Portal of Praevus. | — |
| [`-hexen2`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#hexen2) | Выбирает режим Hexen II. | — |
| [`-quake3`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#quake3) | Выбирает режим Quake III Arena. | — |
| [`-quake3demo`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#quake3demo) | Выбирает режим Quake III Arena Demo. | — |
| [`-cod4`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#cod4) | Выбирает режим Call of Duty 4. | — |
| [`-cod2`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#cod2) | Выбирает режим Call of Duty 2. | — |
| [`-cod`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#cod) | Выбирает режим Call of Duty. | — |
| [`-halflife`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#halflife) | Выбирает режим Half-Life / Rad-Therapy. | — |
| [`-gunman`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#gunman) | Выбирает режим Gunman Chronicles. | — |
| [`-halflife2`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#halflife2) | Выбирает режим Half-Life 2 / Rad-Therapy II. | — |
| [`-gmod9`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#gmod9) | Выбирает режим Garry's Mod 9 / Free Will. | — |
| [`-nexuiz`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nexuiz) | Выбирал бы режим Nexuiz. | — |
| [`-xonotic`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#xonotic) | Выбирал бы режим Xonotic. | — |
| [`-spark`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#spark) | Выбирал бы режим Spark. | — |
| [`-scouts`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#scouts) | Выбирал бы режим Scouts Journey. | — |
| [`-rmq`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#rmq) | Выбирал бы режим Remake Quake. | — |
| [`-quake4`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#quake4) | Выбирал бы режим Quake 4. | — |
| [`-et`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#et) | Выбирал бы режим Wolfenstein: Enemy Territory. | — |
| [`-jk2`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#jk2) | Выбирал бы режим Jedi Knight II. | — |
| [`-warsow`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#warsow) | Выбирал бы режим Warsow. | — |
| [`-doom`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#doom) | Выбирал бы режим Doom. | — |
| [`-doom2`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#doom2) | Выбирал бы режим Doom II. | — |
| [`-doom3`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#doom3) | Выбирал бы режим Doom 3. | — |
| [`-diablo2`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#diablo2) | Выбирал бы режим Diablo II. | — |

### FTEQCC (компилятор)

Основные параметры с собственным якорем — остальные флаги (`-O`, `-K`/`-F`, `-W` и их многочисленные значения) перечислены таблицами внутри статьи, без отдельных якорей на каждое имя.

| Элемент | Назначение | Сигнатура |
|---|---|---|
| [`-src`](../44-cli-commands-reference/07-fteqcc-command-line-parameters.md#src) | Папка с исходниками (там ищется `progs.src`). | `<folder>` |
| [`-srcfile`](../44-cli-commands-reference/07-fteqcc-command-line-parameters.md#srcfile) | Добавить ещё один список исходников (`.src`) к сборке. | `<file>` |
| [`-o`](../44-cli-commands-reference/07-fteqcc-command-line-parameters.md#o) | Явно задать имя итогового скомпилированного файла. | — |
| [`-D`](../44-cli-commands-reference/07-fteqcc-command-line-parameters.md#d) | Определить именованную константу препроцессора. | — |
| [`-I`](../44-cli-commands-reference/07-fteqcc-command-line-parameters.md#i) | Добавить папку в список путей поиска для `#include`. | — |

| Раздел параметров | Назначение |
|---|---|
| [Целевой формат байт-кода (`-T`)](../44-cli-commands-reference/07-fteqcc-command-line-parameters.md#целевой-формат-байт-кода--t) | `fte`, `standard`, `hexen2` и другие значения |
| [Оптимизации (`-O`)](../44-cli-commands-reference/07-fteqcc-command-line-parameters.md#уровни-и-флаги-оптимизации--o) | уровень 0-3 и отдельные оптимизации по имени |
| [Ключевые слова и флаги (`-K`/`-F`)](../44-cli-commands-reference/07-fteqcc-command-line-parameters.md#флаги-компилятора-и-ключевые-слова--k---f) | переключение ключевых слов и поведенческих флагов |
| [Предупреждения (`-W`)](../44-cli-commands-reference/07-fteqcc-command-line-parameters.md#предупреждения--w) | режим предупреждений (`-Wall`, `-Werror` и т.д.) |
| [Лимиты и информационные параметры](../44-cli-commands-reference/07-fteqcc-command-line-parameters.md#лимиты-и-информационные-параметры) | `-max_*`, `-v`, `--version`, `--help` |
| [Неактивные и неподдерживаемые параметры](../44-cli-commands-reference/07-fteqcc-command-line-parameters.md#неактивные-и-неподдерживаемые-параметры) | `-qc`, `-E`, `-progdefs`, `-copy`, `-bspmodels`, `-pak`, `-stdout`, `-h2` и другие принятые, но нерабочие флаги |

---

## Команды консоли

Консольные команды (в отличие от cvar и параметров запуска) — это разовые действия. Полный постатейный список с указанием источника в коде и статуса доступности — в разделе [«44. Команды и параметры командной строки»](../README.md#команды-и-параметры-командной-строки).

### Клиент и интерфейс

| Элемент | Назначение |
|---|---|
| [`autotrack`](../44-cli-commands-reference/02-client-ui-commands.md#autotrack) | Переключает автоматический выбор цели наблюдения по правилу или отключает его значением `off`. |
| [`track`](../44-cli-commands-reference/02-client-ui-commands.md#track) | Назначает одного или нескольких игроков для слежения в режиме наблюдателя. |
| [`track1`](../44-cli-commands-reference/02-client-ui-commands.md#track1) | Назначает цель слежения для слота `1` в мультикамерном режиме наблюдателя. |
| [`track2`](../44-cli-commands-reference/02-client-ui-commands.md#track2) | Назначает цель слежения для слота `2` в мультикамерном режиме наблюдателя. |
| [`track3`](../44-cli-commands-reference/02-client-ui-commands.md#track3) | Назначает цель слежения для слота `3` в мультикамерном режиме наблюдателя. |
| [`track4`](../44-cli-commands-reference/02-client-ui-commands.md#track4) | Назначает цель слежения для слота `4` в мультикамерном режиме наблюдателя. |
| [`cl_voip_mute`](../44-cli-commands-reference/02-client-ui-commands.md#cl_voip_mute) | Глушит голос конкретного игрока по имени или userid. |
| [`cl_voip_unmute`](../44-cli-commands-reference/02-client-ui-commands.md#cl_voip_unmute) | Снимает локальный муте для указанного игрока. |
| [`ignore`](../44-cli-commands-reference/02-client-ui-commands.md#ignore) | Добавляет игрока в список игнорирования или показывает состояние списка, если аргументы не заданы. |
| [`ignore_id`](../44-cli-commands-reference/02-client-ui-commands.md#ignore_id) | Добавляет в список игнорирования игрока по числовому userid. |
| [`ignore_team`](../44-cli-commands-reference/02-client-ui-commands.md#ignore_team) | Игнорирует чат и сообщения указанной команды в teamplay-режимах. |
| [`ignorelist`](../44-cli-commands-reference/02-client-ui-commands.md#ignorelist) | Показывает текущие локальные правила игнорирования. |
| [`unignore`](../44-cli-commands-reference/02-client-ui-commands.md#unignore) | Удаляет игрока из список игнорированияа по имени или userid. |
| [`unignore_id`](../44-cli-commands-reference/02-client-ui-commands.md#unignore_id) | Удаляет из список игнорированияа игрока по userid. |
| [`unignore_team`](../44-cli-commands-reference/02-client-ui-commands.md#unignore_team) | Снимает командное игнорирование для указанной команды. |
| [`unignoreAll`](../44-cli-commands-reference/02-client-ui-commands.md#unignoreall) | Полностью очищает список игнорирования игроков. |
| [`unignoreAll_team`](../44-cli-commands-reference/02-client-ui-commands.md#unignoreall_team) | Очищает все правила игнорирования команд. |
| [`+attack`](../44-cli-commands-reference/02-client-ui-commands.md#attack) | Активирует действие атаки. |
| [`+back`](../44-cli-commands-reference/02-client-ui-commands.md#back) | Активирует действие движения назад. |
| [`+forward`](../44-cli-commands-reference/02-client-ui-commands.md#forward) | Активирует действие движения вперёд. |
| [`+jump`](../44-cli-commands-reference/02-client-ui-commands.md#jump) | Активирует действие прыжка. |
| [`+klook`](../44-cli-commands-reference/02-client-ui-commands.md#klook) | Активирует действие режима keyboard-look. |
| [`+left`](../44-cli-commands-reference/02-client-ui-commands.md#left) | Активирует действие поворота влево. |
| [`+lookdown`](../44-cli-commands-reference/02-client-ui-commands.md#lookdown) | Активирует действие взгляда вниз. |
| [`+lookup`](../44-cli-commands-reference/02-client-ui-commands.md#lookup) | Активирует действие взгляда вверх. |
| [`+mlook`](../44-cli-commands-reference/02-client-ui-commands.md#mlook) | Активирует действие режима mouse-look. |
| [`+movedown`](../44-cli-commands-reference/02-client-ui-commands.md#movedown) | Активирует действие движения вниз. |
| [`+moveleft`](../44-cli-commands-reference/02-client-ui-commands.md#moveleft) | Активирует действие шага влево. |
| [`+moveright`](../44-cli-commands-reference/02-client-ui-commands.md#moveright) | Активирует действие шага вправо. |
| [`+moveup`](../44-cli-commands-reference/02-client-ui-commands.md#moveup) | Активирует действие движения вверх. |
| [`+p`](../44-cli-commands-reference/02-client-ui-commands.md#p) | Проксирует нажатие кнопочноы команды в указанный сплит-ссреен-слот. |
| [`+right`](../44-cli-commands-reference/02-client-ui-commands.md#right) | Активирует действие поворота вправо. |
| [`+rollleft`](../44-cli-commands-reference/02-client-ui-commands.md#rollleft) | Активирует действие крена влево. |
| [`+rollright`](../44-cli-commands-reference/02-client-ui-commands.md#rollright) | Активирует действие крена вправо. |
| [`+speed`](../44-cli-commands-reference/02-client-ui-commands.md#speed) | Активирует действие модификатора скорости. |
| [`+strafe`](../44-cli-commands-reference/02-client-ui-commands.md#strafe) | Активирует действие режима страфе. |
| [`+use`](../44-cli-commands-reference/02-client-ui-commands.md#use) | Активирует действие использования. |
| [`+weaponwheel`](../44-cli-commands-reference/02-client-ui-commands.md#weaponwheel) | Активирует действие колеса выбора оружия. |
| [`-attack`](../44-cli-commands-reference/02-client-ui-commands.md#-attack) | Снимает действие атаки, завершая `+attack`. |
| [`-back`](../44-cli-commands-reference/02-client-ui-commands.md#-back) | Снимает действие движения назад, завершая `+back`. |
| [`-forward`](../44-cli-commands-reference/02-client-ui-commands.md#-forward) | Снимает действие движения вперёд, завершая `+forward`. |
| [`-jump`](../44-cli-commands-reference/02-client-ui-commands.md#-jump) | Снимает действие прыжка, завершая `+jump`. |
| [`-klook`](../44-cli-commands-reference/02-client-ui-commands.md#-klook) | Снимает действие режима keyboard-look, завершая `+klook`. |
| [`-left`](../44-cli-commands-reference/02-client-ui-commands.md#-left) | Снимает действие поворота влево, завершая `+left`. |
| [`-lookdown`](../44-cli-commands-reference/02-client-ui-commands.md#-lookdown) | Снимает действие взгляда вниз, завершая `+lookdown`. |
| [`-lookup`](../44-cli-commands-reference/02-client-ui-commands.md#-lookup) | Снимает действие взгляда вверх, завершая `+lookup`. |
| [`-mlook`](../44-cli-commands-reference/02-client-ui-commands.md#-mlook) | Снимает действие режима mouse-look, завершая `+mlook`. |
| [`-movedown`](../44-cli-commands-reference/02-client-ui-commands.md#-movedown) | Снимает действие движения вниз, завершая `+movedown`. |
| [`-moveleft`](../44-cli-commands-reference/02-client-ui-commands.md#-moveleft) | Снимает действие шага влево, завершая `+moveleft`. |
| [`-moveright`](../44-cli-commands-reference/02-client-ui-commands.md#-moveright) | Снимает действие шага вправо, завершая `+moveright`. |
| [`-moveup`](../44-cli-commands-reference/02-client-ui-commands.md#-moveup) | Снимает действие движения вверх, завершая `+moveup`. |
| [`-p`](../44-cli-commands-reference/02-client-ui-commands.md#-p) | Проксирует отпусканиые кнопочноы команды в указанный сплит-ссреен-слот, завершая действие, начатое через `+p`. |
| [`-right`](../44-cli-commands-reference/02-client-ui-commands.md#-right) | Снимает действие поворота вправо, завершая `+right`. |
| [`-rollleft`](../44-cli-commands-reference/02-client-ui-commands.md#-rollleft) | Снимает действие крена влево, завершая `+rollleft`. |
| [`-rollright`](../44-cli-commands-reference/02-client-ui-commands.md#-rollright) | Снимает действие крена вправо, завершая `+rollright`. |
| [`-speed`](../44-cli-commands-reference/02-client-ui-commands.md#-speed) | Снимает действие модификатора скорости, завершая `+speed`. |
| [`-strafe`](../44-cli-commands-reference/02-client-ui-commands.md#-strafe) | Снимает действие режима страфе, завершая `+strafe`. |
| [`-use`](../44-cli-commands-reference/02-client-ui-commands.md#-use) | Снимает действие использования, завершая `+use`. |
| [`-weaponwheel`](../44-cli-commands-reference/02-client-ui-commands.md#-weaponwheel) | Снимает действие колеса выбора оружия, завершая `+weaponwheel`. |
| [`bestweapon`](../44-cli-commands-reference/02-client-ui-commands.md#bestweapon) | Рытаеця выбрат лучшее доступное оружие из переданного списка импулсов. |
| [`bind`](../44-cli-commands-reference/02-client-ui-commands.md#bind) | Привязывает команду к клавише или показывает текущую привязку для этой клавиши. |
| [`bindlevel`](../44-cli-commands-reference/02-client-ui-commands.md#bindlevel) | Вариант `bind` с дополнителным уровнем или контекстом привязки. |
| [`impulse`](../44-cli-commands-reference/02-client-ui-commands.md#impulse) | Отправляет числовоы импулс на сервер. |
| [`in_bind`](../44-cli-commands-reference/02-client-ui-commands.md#in_bind) | Создает привязку внутри выбранноы биндмап-раскладки. |
| [`in_deviceids`](../44-cli-commands-reference/02-client-ui-commands.md#in_deviceids) | Показывает найденные устройства ввода и позволяет закрепить им стабильные идентификаторы. |
| [`in_restart`](../44-cli-commands-reference/02-client-ui-commands.md#in_restart) | Полностью переинициализирует подсистему ввода. |
| [`p`](../44-cli-commands-reference/02-client-ui-commands.md#p) | Выполняет переданную команду от имени указанного сплит-ссреен-слота. |
| [`register_bestweapon`](../44-cli-commands-reference/02-client-ui-commands.md#register_bestweapon) | Сохраняет порядок предпочтения импулсов, который затем использует `bestweapon`. |
| [`rotate`](../44-cli-commands-reference/02-client-ui-commands.md#rotate) | Добавляет поворот взгляда на указанную величину; команда удобна для скриптов и алтернативного ввода. |
| [`sendcvar`](../44-cli-commands-reference/02-client-ui-commands.md#sendcvar) | Отправляет серверу значение разрешённоы клиентской свар. |
| [`unbind`](../44-cli-commands-reference/02-client-ui-commands.md#unbind) | Удаляет привязку у указанной клавиши. |
| [`unbindall`](../44-cli-commands-reference/02-client-ui-commands.md#unbindall) | Снимает все пользовательские привязки клавиш. |
| [`clear`](../44-cli-commands-reference/02-client-ui-commands.md#clear) | Очищает активную консол или указанную именованную консол. |
| [`conactivate`](../44-cli-commands-reference/02-client-ui-commands.md#conactivate) | Переводит фокус на выбранную именованную консол. |
| [`conclear`](../44-cli-commands-reference/02-client-ui-commands.md#conclear) | Очищает указанную именованную консол. |
| [`conclose`](../44-cli-commands-reference/02-client-ui-commands.md#conclose) | Закрывает и уничтожает указанную именованную консол. |
| [`conecho`](../44-cli-commands-reference/02-client-ui-commands.md#conecho) | Речатает строку в указанную именованную консол. |
| [`conecho_center`](../44-cli-commands-reference/02-client-ui-commands.md#conecho_center) | Речатает строку по центру указанной консоли. |
| [`me`](../44-cli-commands-reference/02-client-ui-commands.md#me) | Отправляет емоте-сообщение в формате `/me`. |
| [`messagemode`](../44-cli-commands-reference/02-client-ui-commands.md#messagemode) | Открывает строку ввода обычного чат-сообщения. |
| [`messagemode2`](../44-cli-commands-reference/02-client-ui-commands.md#messagemode2) | Открывает строку ввода командного чат-сообщения. |
| [`qterm`](../44-cli-commands-reference/02-client-ui-commands.md#qterm) | Запускает внешнюю команду в отдельной консоли QTerm, если эта возможностьь собрана в клиенте. |
| [`say`](../44-cli-commands-reference/02-client-ui-commands.md#say) | Отправляет обычное чат-сообщение. |
| [`say_team`](../44-cli-commands-reference/02-client-ui-commands.md#say_team) | Отправляет сообщение толко своеы команде. |
| [`sayone`](../44-cli-commands-reference/02-client-ui-commands.md#sayone) | Отправляет приватное сообщение одному адресату или в адресный канал мода. |
| [`toggleconsole`](../44-cli-commands-reference/02-client-ui-commands.md#toggleconsole) | Переключает видимость игровой консоли. |
| [`demo_jump`](../44-cli-commands-reference/02-client-ui-commands.md#demo_jump) | Прыгает к указанному времени внутри демо; допускаются абсолютное время и относительные смещения `+` и `-`. |
| [`demo_jump_end`](../44-cli-commands-reference/02-client-ui-commands.md#demo_jump_end) | Переходит к концу воспроизводимого демо. |
| [`demo_jump_mark`](../44-cli-commands-reference/02-client-ui-commands.md#demo_jump_mark) | Переходит к ближайшей метке `//demomark` внутри демо. |
| [`demo_nudge`](../44-cli-commands-reference/02-client-ui-commands.md#demo_nudge) | Сдвигает позицию воспроизведения демо на небольшой шаг вперёд или назад. |
| [`demo_setspeed`](../44-cli-commands-reference/02-client-ui-commands.md#demo_setspeed) | Меняет скорость воспроизведения демо в процентах. |
| [`demos`](../44-cli-commands-reference/02-client-ui-commands.md#demos) | Показывает список доступных демозаписий. |
| [`playdemo`](../44-cli-commands-reference/02-client-ui-commands.md#playdemo) | Начинает воспроизведение указанного демо. |
| [`record`](../44-cli-commands-reference/02-client-ui-commands.md#record) | Начинает запис демо текущей игры. |
| [`rerecord`](../44-cli-commands-reference/02-client-ui-commands.md#rerecord) | Перезаписывает демо с указанным именем с начала новой записи. |
| [`showpic`](../44-cli-commands-reference/02-client-ui-commands.md#showpic) | Показывает 2D-изображение поверх экрана в заданной точке и зоне. |
| [`showpic_removeall`](../44-cli-commands-reference/02-client-ui-commands.md#showpic_removeall) | Удаляет все изображения, созданные через `showpic`. |
| [`startdemos`](../44-cli-commands-reference/02-client-ui-commands.md#startdemos) | Задаёт цикл демозаписий для автопроигрывания. |
| [`stop`](../44-cli-commands-reference/02-client-ui-commands.md#stop) | Останавливает текущую запис демо. |
| [`stopdemo`](../44-cli-commands-reference/02-client-ui-commands.md#stopdemo) | Останавливает воспроизведение демо. |
| [`timedemo`](../44-cli-commands-reference/02-client-ui-commands.md#timedemo) | Прокручивает демо в тестовом режиме для измерения производительности. |
| [`6dof`](../44-cli-commands-reference/02-client-ui-commands.md#6dof) | Проксирует одноимённую игровую или чит-команду в мод или на сервер. |
| [`allskins`](../44-cli-commands-reference/02-client-ui-commands.md#allskins) | Принудительно применяет один и тот же скин ко всем моделям игроков. |
| [`changing`](../44-cli-commands-reference/02-client-ui-commands.md#changing) | Переводит клиент в состояние смены уровня или карты. |
| [`cl_status`](../44-cli-commands-reference/02-client-ui-commands.md#cl_status) | Показывает сводное состояние клиента, соединения и связанных подсистем. |
| [`cl_transfer`](../44-cli-commands-reference/02-client-ui-commands.md#cl_transfer) | Перенаправляет подключение на другой сервер по полному адресу. |
| [`color`](../44-cli-commands-reference/02-client-ui-commands.md#color) | Меняет цветовые настройки игрока; формат зависит от режима и обычно использует RGB-значения. |
| [`connect`](../44-cli-commands-reference/02-client-ui-commands.md#connect) | Подключается к серверу по адресу или URL со схемой. |
| [`connectbr`](../44-cli-commands-reference/02-client-ui-commands.md#connectbr) | Подключается к локально найденному серверу по адресу и порту. |
| [`connectirc`](../44-cli-commands-reference/02-client-ui-commands.md#connectirc) | Запускает IRC-подключение через встроенный клиент. |
| [`connectnq`](../44-cli-commands-reference/02-client-ui-commands.md#connectnq) | Подключается к серверу в стиле NetQuake. |
| [`connectq2e`](../44-cli-commands-reference/02-client-ui-commands.md#connectq2e) | Подключается к серверу Quake2Ex. |
| [`connectqe`](../44-cli-commands-reference/02-client-ui-commands.md#connectqe) | Подключается к серверу QE или NQ-совместимым способом. |
| [`connecttcp`](../44-cli-commands-reference/02-client-ui-commands.md#connecttcp) | Подключается к серверу по TCP. |
| [`crashme_endgame`](../44-cli-commands-reference/02-client-ui-commands.md#crashme_endgame) | Тестовая команда аварийного завершения или конца игры; предназначена для отладки. |
| [`crashme_error`](../44-cli-commands-reference/02-client-ui-commands.md#crashme_error) | Тестовая команда генерации ошибки; предназначена для отладки обработчиков сбоев. |
| [`curl`](../44-cli-commands-reference/02-client-ui-commands.md#curl) | Выполняет HTTP или URL-запрос в стиле DarkPlaces/Xonotic-совместимой команды `curl`. |
| [`disconnect`](../44-cli-commands-reference/02-client-ui-commands.md#disconnect) | Разрывает текущее соединение с сервером или демо-сеансом. |
| [`dlsize`](../44-cli-commands-reference/02-client-ui-commands.md#dlsize) | Показывает состояние текущей очереди загрузки и размеры файлов. |
| [`download`](../44-cli-commands-reference/02-client-ui-commands.md#download) | Запрашивает файл у сервера или скачивает ресурс по URL в локальное имя. |
| [`finishdl`](../44-cli-commands-reference/02-client-ui-commands.md#finishdl) | Принудительно завершает или финализирует ожидающие загрузки. |
| [`fly`](../44-cli-commands-reference/02-client-ui-commands.md#fly) | Проксирует одноимённую игровую или чит-команду в мод или на сервер. |
| [`fog`](../44-cli-commands-reference/02-client-ui-commands.md#fog) | Настраивает параметры обычного мирового тумана на стороне клиента. |
| [`freespace`](../44-cli-commands-reference/02-client-ui-commands.md#freespace) | Показывает, сколько свободного места доступно для кэша, скачиваний и записий. |
| [`ftp`](../44-cli-commands-reference/02-client-ui-commands.md#ftp) | Управляет встроенным FТР-клиентом. |
| [`fullinfo`](../44-cli-commands-reference/02-client-ui-commands.md#fullinfo) | Заменяет усеринфо целиком переданноы info-строкой. |
| [`give`](../44-cli-commands-reference/02-client-ui-commands.md#give) | Проксирует одноимённую игровую или чит-команду в мод или на сервер. |
| [`god`](../44-cli-commands-reference/02-client-ui-commands.md#god) | Проксирует одноимённую игровую или чит-команду в мод или на сервер. |
| [`join`](../44-cli-commands-reference/02-client-ui-commands.md#join) | Подключается к игровому серверу или выходит из режима наблюдателя на выбранном хосте. |
| [`kill`](../44-cli-commands-reference/02-client-ui-commands.md#kill) | Проксирует одноимённую игровую или чит-команду в мод или на сервер. |
| [`nextul`](../44-cli-commands-reference/02-client-ui-commands.md#nextul) | Переключает очередь на следующую выгрузку или отправку файла. |
| [`noclip`](../44-cli-commands-reference/02-client-ui-commands.md#noclip) | Проксирует одноимённую игровую или чит-команду в мод или на сервер. |
| [`notarget`](../44-cli-commands-reference/02-client-ui-commands.md#notarget) | Проксирует одноимённую игровую или чит-команду в мод или на сервер. |
| [`observe`](../44-cli-commands-reference/02-client-ui-commands.md#observe) | Подключается к серверу сразу в режиме наблюдателя. |
| [`packet`](../44-cli-commands-reference/02-client-ui-commands.md#packet) | Отправляет произвольный сетевоы пакет на указанный адрес. |
| [`pause`](../44-cli-commands-reference/02-client-ui-commands.md#pause) | Проксирует одноимённую игровую или чит-команду в мод или на сервер. |
| [`qtvdemos`](../44-cli-commands-reference/02-client-ui-commands.md#qtvdemos) | Показывает демозаписи, доступные на QTV-сервере. |
| [`qtvlist`](../44-cli-commands-reference/02-client-ui-commands.md#qtvlist) | Запрашивает список доступных QTV-потоков. |
| [`qtvplay`](../44-cli-commands-reference/02-client-ui-commands.md#qtvplay) | Подключается к QTV-потоку с опциональным паролем. |
| [`quit`](../44-cli-commands-reference/02-client-ui-commands.md#quit) | Завершает работу клиента. |
| [`qwurl`](../44-cli-commands-reference/02-client-ui-commands.md#qwurl) | совместимый с езQuake вариант команды быстрого подключения. |
| [`r_imagelist_wad`](../44-cli-commands-reference/02-client-ui-commands.md#r_imagelist_wad) | Показывает список изображениы, загруженных из WAD-ресурсов. |
| [`rcon`](../44-cli-commands-reference/02-client-ui-commands.md#rcon) | Отправляет удалённую консольную команду на сервер. |
| [`reconnect`](../44-cli-commands-reference/02-client-ui-commands.md#reconnect) | Ровторно подключается к последнему серверу. |
| [`serverinfo`](../44-cli-commands-reference/02-client-ui-commands.md#serverinfo) | Показывает или локально изменяет пары ключ и значение серверинфо. |
| [`setinfo`](../44-cli-commands-reference/02-client-ui-commands.md#setinfo) | Показывает или меняет отдельное поле усеринфо. |
| [`setinfoblob`](../44-cli-commands-reference/02-client-ui-commands.md#setinfoblob) | Схитает файл и отправляет его содержимое в блоб-поле усеринфо. |
| [`setpos`](../44-cli-commands-reference/02-client-ui-commands.md#setpos) | Проксирует одноимённую игровую или чит-команду в мод или на сервер. |
| [`skins`](../44-cli-commands-reference/02-client-ui-commands.md#skins) | Показывает или обновляет список доступных скинов игроков. |
| [`skipdl`](../44-cli-commands-reference/02-client-ui-commands.md#skipdl) | Пропускает текущую загрузку и переходит к следующей. |
| [`skygroup`](../44-cli-commands-reference/02-client-ui-commands.md#skygroup) | Связывает skybox с набором карт для автоподстановки. |
| [`skyroomfog`](../44-cli-commands-reference/02-client-ui-commands.md#skyroomfog) | Настраивает туман, применяемый к skyroom-сценам. |
| [`spiderpig`](../44-cli-commands-reference/02-client-ui-commands.md#spiderpig) | Проксирует одноимённую игровую или чит-команду в мод или на сервер. |
| [`stopul`](../44-cli-commands-reference/02-client-ui-commands.md#stopul) | Останавливает текущую выгрузку или отправку файла. |
| [`tcpconnect`](../44-cli-commands-reference/02-client-ui-commands.md#tcpconnect) | Синоним подключения к серверу по TCP. |
| [`topten`](../44-cli-commands-reference/02-client-ui-commands.md#topten) | Проксирует одноимённую игровую или чит-команду в мод или на сервер. |
| [`user`](../44-cli-commands-reference/02-client-ui-commands.md#user) | Показывает усеринфо выбранного игрока по имени или userid. |
| [`users`](../44-cli-commands-reference/02-client-ui-commands.md#users) | Показывает список игроков и их userid. |
| [`waterfog`](../44-cli-commands-reference/02-client-ui-commands.md#waterfog) | Настраивает параметры тумана под водоы. |
| [`capture`](../44-cli-commands-reference/02-client-ui-commands.md#capture) | Начинает видеозахват в указанный файл. |
| [`capturedemo`](../44-cli-commands-reference/02-client-ui-commands.md#capturedemo) | Воспроизводит демо и одновременно записывает его как видео. |
| [`capturepause`](../44-cli-commands-reference/02-client-ui-commands.md#capturepause) | Ставит видеозахват на паузу или снимает её. |
| [`capturestop`](../44-cli-commands-reference/02-client-ui-commands.md#capturestop) | Останавливает активный видеозахват. |
| [`cd`](../44-cli-commands-reference/02-client-ui-commands.md#cd) | Управляет СД-аудио или его эмуляцией: воспроизведение, пауза, информация и так далее. |
| [`cinematic`](../44-cli-commands-reference/02-client-ui-commands.md#cinematic) | Проигрывает ролик из каталога `video/` в стиле Quake III. |
| [`cprint`](../44-cli-commands-reference/02-client-ui-commands.md#cprint) | Речатает центрированное сообщение поверх экрана. |
| [`envmap`](../44-cli-commands-reference/02-client-ui-commands.md#envmap) | Устаревшиы псевдоним для снятия субемап-скриншота. |
| [`media_add`](../44-cli-commands-reference/02-client-ui-commands.md#media_add) | Добавляет трек в список `sound/media.m3u`. |
| [`media_next`](../44-cli-commands-reference/02-client-ui-commands.md#media_next) | Переключает воспроизведение на следующий медиа-трек. |
| [`media_remove`](../44-cli-commands-reference/02-client-ui-commands.md#media_remove) | Удаляет трек из списка `sound/media.m3u`. |
| [`menu_media`](../44-cli-commands-reference/02-client-ui-commands.md#menu_media) | Открывает меню управления медиатекоы. |
| [`music`](../44-cli-commands-reference/02-client-ui-commands.md#music) | Запускает воспроизведение музыкального трека, при желании с отдельным loop-треком. |
| [`music_fforward`](../44-cli-commands-reference/02-client-ui-commands.md#music_fforward) | Перематывает текущую музыку вперёд на заданное число секунд. |
| [`music_next`](../44-cli-commands-reference/02-client-ui-commands.md#music_next) | Переходит к следующему музыкальному треку. |
| [`music_rewind`](../44-cli-commands-reference/02-client-ui-commands.md#music_rewind) | Перематывает текущую музыку назад на заданное число секунд. |
| [`playclip`](../44-cli-commands-reference/02-client-ui-commands.md#playclip) | Проигрывает короткий видеоклип без очереди. |
| [`playfilm`](../44-cli-commands-reference/02-client-ui-commands.md#playfilm) | Ставит один или несколько роликов в очеред воспроизведения. |
| [`playvideo`](../44-cli-commands-reference/02-client-ui-commands.md#playvideo) | Запускает воспроизведение указанного видеофайла. |
| [`screenshot`](../44-cli-commands-reference/02-client-ui-commands.md#screenshot) | Делает обычный скриншот текущего кадра. |
| [`screenshot_360`](../44-cli-commands-reference/02-client-ui-commands.md#screenshot_360) | Делает панорамный 360-градусный скриншот. |
| [`screenshot_cubemap`](../44-cli-commands-reference/02-client-ui-commands.md#screenshot_cubemap) | Сохраняет субемап-снимок из шести гранеы окружения. |
| [`screenshot_mega`](../44-cli-commands-reference/02-client-ui-commands.md#screenshot_mega) | Делает сверхвысокое изображение с офлаын-таилингом. |
| [`screenshot_stereo`](../44-cli-commands-reference/02-client-ui-commands.md#screenshot_stereo) | Делает стереоскопическиы скриншот. |
| [`screenshot_vr`](../44-cli-commands-reference/02-client-ui-commands.md#screenshot_vr) | Делает ВР-совместимый скриншот. |
| [`stopmusic`](../44-cli-commands-reference/02-client-ui-commands.md#stopmusic) | Полностью останавливает музыкальное воспроизведение. |
| [`stt`](../44-cli-commands-reference/02-client-ui-commands.md#stt) | Запускает встроенный speech-to-text, если этот модуль доступен в сборке. |
| [`tts`](../44-cli-commands-reference/02-client-ui-commands.md#tts) | Озвучивает переданный текст через text-to-speech, если доступен бэкенд. |
| [`closemenu`](../44-cli-commands-reference/02-client-ui-commands.md#closemenu) | Закрывает активное меню и возвращает управление игре. |
| [`conmenu`](../44-cli-commands-reference/02-client-ui-commands.md#conmenu) | Открывает ссриптед-меню и задаёт саллбаск для обработки его пунктов. |
| [`fps_preset`](../44-cli-commands-reference/02-client-ui-commands.md#fps_preset) | Применяет готовый набор настроек производительности или FPS. |
| [`help`](../44-cli-commands-reference/02-client-ui-commands.md#help) | Открывает встроенное меню справки. |
| [`menu_audio`](../44-cli-commands-reference/02-client-ui-commands.md#menu_audio) | Открывает меню настроек звука. |
| [`menu_credits`](../44-cli-commands-reference/02-client-ui-commands.md#menu_credits) | Открывает меню титров и авторов. |
| [`menu_demo`](../44-cli-commands-reference/02-client-ui-commands.md#menu_demo) | Открывает меню демозаписий. |
| [`menu_download`](../44-cli-commands-reference/02-client-ui-commands.md#menu_download) | Открывает меню загрузок. |
| [`menu_fps`](../44-cli-commands-reference/02-client-ui-commands.md#menu_fps) | Открывает меню пресетов производительности и FPS. |
| [`menu_keys`](../44-cli-commands-reference/02-client-ui-commands.md#menu_keys) | Открывает меню привязок клавиш. |
| [`menu_lighting`](../44-cli-commands-reference/02-client-ui-commands.md#menu_lighting) | Открывает меню освещения. |
| [`menu_load`](../44-cli-commands-reference/02-client-ui-commands.md#menu_load) | Открывает меню загрузки. |
| [`menu_loadgame`](../44-cli-commands-reference/02-client-ui-commands.md#menu_loadgame) | Открывает меню загрузки сохранения. |
| [`menu_main`](../44-cli-commands-reference/02-client-ui-commands.md#menu_main) | Открывает меню главного меню. |
| [`menu_mediafiles`](../44-cli-commands-reference/02-client-ui-commands.md#menu_mediafiles) | Открывает меню медиафаилов. |
| [`menu_mods`](../44-cli-commands-reference/02-client-ui-commands.md#menu_mods) | Открывает меню модов. |
| [`menu_multi`](../44-cli-commands-reference/02-client-ui-commands.md#menu_multi) | Открывает меню мультиплеера. |
| [`menu_network`](../44-cli-commands-reference/02-client-ui-commands.md#menu_network) | Открывает меню сетевых настроек. |
| [`menu_newmulti`](../44-cli-commands-reference/02-client-ui-commands.md#menu_newmulti) | Открывает меню нового сетевого сеанса. |
| [`menu_options`](../44-cli-commands-reference/02-client-ui-commands.md#menu_options) | Открывает меню основных настроек. |
| [`menu_particles`](../44-cli-commands-reference/02-client-ui-commands.md#menu_particles) | Открывает меню частиц. |
| [`menu_quit`](../44-cli-commands-reference/02-client-ui-commands.md#menu_quit) | Открывает меню выхода. |
| [`menu_render`](../44-cli-commands-reference/02-client-ui-commands.md#menu_render) | Открывает меню рендерера. |
| [`menu_restart`](../44-cli-commands-reference/02-client-ui-commands.md#menu_restart) | Открывает меню перезапуска и подтверждения. |
| [`menu_save`](../44-cli-commands-reference/02-client-ui-commands.md#menu_save) | Открывает меню сохранения. |
| [`menu_servers`](../44-cli-commands-reference/02-client-ui-commands.md#menu_servers) | Открывает меню списка серверов. |
| [`menu_setup`](../44-cli-commands-reference/02-client-ui-commands.md#menu_setup) | Открывает меню профиля игрока. |
| [`menu_single`](../44-cli-commands-reference/02-client-ui-commands.md#menu_single) | Открывает меню одиночной игры. |
| [`menu_slist`](../44-cli-commands-reference/02-client-ui-commands.md#menu_slist) | Открывает меню поиска серверов. |
| [`menu_spcheats`](../44-cli-commands-reference/02-client-ui-commands.md#menu_spcheats) | Открывает меню читов одиночной игры. |
| [`menu_speakers`](../44-cli-commands-reference/02-client-ui-commands.md#menu_speakers) | Открывает меню настроек акустики. |
| [`menu_teamplay`](../44-cli-commands-reference/02-client-ui-commands.md#menu_teamplay) | Открывает меню teamplay-настроек. |
| [`menu_teamplay_ammo_health`](../44-cli-commands-reference/02-client-ui-commands.md#menu_teamplay_ammo_health) | Открывает меню teamplay-настроек боеприпасов и здоровья. |
| [`menu_teamplay_armor`](../44-cli-commands-reference/02-client-ui-commands.md#menu_teamplay_armor) | Открывает меню teamplay-настроек брони. |
| [`menu_teamplay_items`](../44-cli-commands-reference/02-client-ui-commands.md#menu_teamplay_items) | Открывает меню teamplay-настроек предметов. |
| [`menu_teamplay_locations`](../44-cli-commands-reference/02-client-ui-commands.md#menu_teamplay_locations) | Открывает меню teamplay-настроек локации. |
| [`menu_teamplay_needs`](../44-cli-commands-reference/02-client-ui-commands.md#menu_teamplay_needs) | Открывает меню teamplay-сообщений о потребностях. |
| [`menu_teamplay_powerups`](../44-cli-commands-reference/02-client-ui-commands.md#menu_teamplay_powerups) | Открывает меню teamplay-настроек поверуп-ов. |
| [`menu_teamplay_status_location_misc`](../44-cli-commands-reference/02-client-ui-commands.md#menu_teamplay_status_location_misc) | Открывает меню teamplay-статуса, локации и прочего. |
| [`menu_teamplay_team_fortress`](../44-cli-commands-reference/02-client-ui-commands.md#menu_teamplay_team_fortress) | Открывает меню teamplay-настроек Теам Fортресс. |
| [`menu_teamplay_weapons`](../44-cli-commands-reference/02-client-ui-commands.md#menu_teamplay_weapons) | Открывает меню teamplay-настроек оружия. |
| [`menu_textures`](../44-cli-commands-reference/02-client-ui-commands.md#menu_textures) | Открывает меню текстур. |
| [`menu_video`](../44-cli-commands-reference/02-client-ui-commands.md#menu_video) | Открывает меню видео и экрана. |
| [`menubind`](../44-cli-commands-reference/02-client-ui-commands.md#menubind) | Добавляет в ссриптед-меню елемент для назначения или смены бинда. |
| [`menubox`](../44-cli-commands-reference/02-client-ui-commands.md#menubox) | Рисует рамку или панел в ссриптед-меню. |
| [`menucallback`](../44-cli-commands-reference/02-client-ui-commands.md#menucallback) | Настраивает саллбаск и набор параметров для ссриптед-меню. |
| [`menucheck`](../44-cli-commands-reference/02-client-ui-commands.md#menucheck) | Добавляет флажок, связанный с свар и битовоы маскоы. |
| [`menuclear`](../44-cli-commands-reference/02-client-ui-commands.md#menuclear) | Очищает содержимое текущего ссриптед-меню. |
| [`menucomboi`](../44-cli-commands-reference/02-client-ui-commands.md#menucomboi) | Добавляет список выбора с целочисленными индексами. |
| [`menucombos`](../44-cli-commands-reference/02-client-ui-commands.md#menucombos) | Добавляет список выбора с явными строковыми значениями. |
| [`menuedit`](../44-cli-commands-reference/02-client-ui-commands.md#menuedit) | Добавляет поле ввода, привязанное к свар. |
| [`menueditpriv`](../44-cli-commands-reference/02-client-ui-commands.md#menueditpriv) | Добавляет приватное или маскируемое поле ввода. |
| [`menupic`](../44-cli-commands-reference/02-client-ui-commands.md#menupic) | Добавляет изображение в ссриптед-меню. |
| [`menupop`](../44-cli-commands-reference/02-client-ui-commands.md#menupop) | Закрывает верхниы уровен меню и возвращается на предыдущиы. |
| [`menuslider`](../44-cli-commands-reference/02-client-ui-commands.md#menuslider) | Добавляет слаыдер, связанный с свар и диапазоном значениы. |
| [`menutext`](../44-cli-commands-reference/02-client-ui-commands.md#menutext) | Добавляет текстовую кнопку или подпис с саллбаск-командой. |
| [`menutextbig`](../44-cli-commands-reference/02-client-ui-commands.md#menutextbig) | Добавляет крупную текстовую кнопку или заголовок с саллбаск-командой. |
| [`modelviewer`](../44-cli-commands-reference/02-client-ui-commands.md#modelviewer) | Открывает встроенный просмотрщик моделей. |
| [`quickconnect`](../44-cli-commands-reference/02-client-ui-commands.md#quickconnect) | Открывает быстрое подключение или сразу использует переданный адрес. |
| [`togglemenu`](../44-cli-commands-reference/02-client-ui-commands.md#togglemenu) | Переключает показ игрового меню. |
| [`listconfigs`](../44-cli-commands-reference/02-client-ui-commands.md#listconfigs) | Показывает доступные конфигурации или режимы видеосистемы. |
| [`listfonts`](../44-cli-commands-reference/02-client-ui-commands.md#listfonts) | Показывает список загруженных шрифтов. |
| [`listskins`](../44-cli-commands-reference/02-client-ui-commands.md#listskins) | Показывает список известных игроковых скинов. |
| [`r_dumpshaders`](../44-cli-commands-reference/02-client-ui-commands.md#r_dumpshaders) | Сбрасывает сведения о шейдерах для диагностики. |
| [`r_remapshader`](../44-cli-commands-reference/02-client-ui-commands.md#r_remapshader) | Временно подменяет один шейдер другим. |
| [`r_shaderlist`](../44-cli-commands-reference/02-client-ui-commands.md#r_shaderlist) | Показывает список известных шейдеров. |
| [`r_showbatches`](../44-cli-commands-reference/02-client-ui-commands.md#r_showbatches) | Выводит диагностическую информацию по рендер батч-ам. |
| [`r_showshader`](../44-cli-commands-reference/02-client-ui-commands.md#r_showshader) | Показывает диагностические сведения о выбранном шейдере. |
| [`setrenderer`](../44-cli-commands-reference/02-client-ui-commands.md#setrenderer) | Переключает активный рендерер или видеорежим. |
| [`vid_reload`](../44-cli-commands-reference/02-client-ui-commands.md#vid_reload) | Перезагружает видеоподсистему без полного рестарта клиента. |
| [`vid_restart`](../44-cli-commands-reference/02-client-ui-commands.md#vid_restart) | Полностью перезапускает видеоподсистему. |
| [`vid_toggle`](../44-cli-commands-reference/02-client-ui-commands.md#vid_toggle) | Выстро переключает режим окна или экрана. |
| [`+infoplaque`](../44-cli-commands-reference/02-client-ui-commands.md#infoplaque) | Активирует действие информационной таблички Hexen II. |
| [`+showdm`](../44-cli-commands-reference/02-client-ui-commands.md#showdm) | Активирует действие таблицы DM-статистики Hexen II. |
| [`+showinfo`](../44-cli-commands-reference/02-client-ui-commands.md#showinfo) | Активирует действие информационной панели Hexen II. |
| [`+showscores`](../44-cli-commands-reference/02-client-ui-commands.md#showscores) | Активирует действие таблицы счёта. |
| [`+showteamscores`](../44-cli-commands-reference/02-client-ui-commands.md#showteamscores) | Активирует действие командной таблицы счёта. |
| [`-infoplaque`](../44-cli-commands-reference/02-client-ui-commands.md#-infoplaque) | Снимает действие информационной таблички Hexen II, завершая `+infoplaque`. |
| [`-showdm`](../44-cli-commands-reference/02-client-ui-commands.md#-showdm) | Снимает действие таблицы DM-статистики Hexen II, завершая `+showdm`. |
| [`-showinfo`](../44-cli-commands-reference/02-client-ui-commands.md#-showinfo) | Снимает действие информационной панели Hexen II, завершая `+showinfo`. |
| [`-showscores`](../44-cli-commands-reference/02-client-ui-commands.md#-showscores) | Снимает действие таблицы счёта, завершая `+showscores`. |
| [`-showteamscores`](../44-cli-commands-reference/02-client-ui-commands.md#-showteamscores) | Снимает действие командной таблицы счёта, завершая `+showteamscores`. |
| [`bf`](../44-cli-commands-reference/02-client-ui-commands.md#bf) | Запускает бонусную вспышку экрана. |
| [`centerview`](../44-cli-commands-reference/02-client-ui-commands.md#centerview) | Возвращает взгляд к центральному положению. |
| [`colorise`](../44-cli-commands-reference/02-client-ui-commands.md#colorise) | Совместимый псевдоним команды перекраски имени или цветов игрока. |
| [`colorize`](../44-cli-commands-reference/02-client-ui-commands.md#colorize) | Совместимый псевдоним команды перекраски имени или цветов игрока. |
| [`colourise`](../44-cli-commands-reference/02-client-ui-commands.md#colourise) | Перекрашивает имя или цвета игрока в совместимом ZQTP-формате. |
| [`ctfscores`](../44-cli-commands-reference/02-client-ui-commands.md#ctfscores) | Обновляет значения CTF-счёта на HUD; обычно вызывается сервером, а не вручную. |
| [`df`](../44-cli-commands-reference/02-client-ui-commands.md#df) | Запускает тёмную вспышку экрана. |
| [`filter`](../44-cli-commands-reference/02-client-ui-commands.md#filter) | Настраивает фильтрацию teamplay-тегов и маркеров в сообщениях. |
| [`invleft`](../44-cli-commands-reference/02-client-ui-commands.md#invleft) | Сдвигает выбор в инвентаре Hexen II влево. |
| [`invnext`](../44-cli-commands-reference/02-client-ui-commands.md#invnext) | Переходит к следующему предмету инвентаря Hexen II. |
| [`invprev`](../44-cli-commands-reference/02-client-ui-commands.md#invprev) | Переходит к предыдущему предмету инвентаря Hexen II. |
| [`invright`](../44-cli-commands-reference/02-client-ui-commands.md#invright) | Сдвигает выбор в инвентаре Hexen II вправо. |
| [`invuse`](../44-cli-commands-reference/02-client-ui-commands.md#invuse) | Использует выбранный предмет инвентаря Hexen II. |
| [`loadloc`](../44-cli-commands-reference/02-client-ui-commands.md#loadloc) | Загружает `.loc`-файл с именами локации для teamplay-подсказок. |
| [`msg_trigger`](../44-cli-commands-reference/02-client-ui-commands.md#msg_trigger) | Настраивает триггер, реагирующий на входящие сообщения или шаблоны текста. |
| [`pointfile`](../44-cli-commands-reference/02-client-ui-commands.md#pointfile) | Загружает поинтфиле для визуализации утечек или проблем карты. |
| [`r_beaminfo`](../44-cli-commands-reference/02-client-ui-commands.md#r_beaminfo) | Показывает сведения о beam-эффектах. |
| [`r_converteffectinfo`](../44-cli-commands-reference/02-client-ui-commands.md#r_converteffectinfo) | Преобразует описание эффектов в формат, понятный FTEQW. |
| [`r_effect`](../44-cli-commands-reference/02-client-ui-commands.md#r_effect) | Назначает particle-эффект указанной модели или сущности. |
| [`r_exportalleffects`](../44-cli-commands-reference/02-client-ui-commands.md#r_exportalleffects) | Экспортирует все зарегистрированные эффекты в файл. |
| [`r_exportbuiltinparticles`](../44-cli-commands-reference/02-client-ui-commands.md#r_exportbuiltinparticles) | Экспортирует встроенные описания particle-эффектов в файл. |
| [`r_partinfo`](../44-cli-commands-reference/02-client-ui-commands.md#r_partinfo) | Показывает сведения о частицах и particle-эффектах. |
| [`r_trail`](../44-cli-commands-reference/02-client-ui-commands.md#r_trail) | Назначает траил-эффект указанной модели или сущности. |
| [`tp_pickup`](../44-cli-commands-reference/02-client-ui-commands.md#tp_pickup) | Настраивает набор teamplay-флагов для сообщений о поднятых предметах. |
| [`tp_point`](../44-cli-commands-reference/02-client-ui-commands.md#tp_point) | Настраивает набор teamplay-флагов для команд или сообщений `point`. |
| [`tp_took`](../44-cli-commands-reference/02-client-ui-commands.md#tp_took) | Настраивает набор teamplay-флагов для сообщений о взятых предметах. |
| [`v_cshift`](../44-cli-commands-reference/02-client-ui-commands.md#v_cshift) | Меняет цветовой сдвиг или подкраску экрана. |
| [`wf`](../44-cli-commands-reference/02-client-ui-commands.md#wf) | Запускает белую вспышку экрана. |
| [`breakpoint_csqc`](../44-cli-commands-reference/02-client-ui-commands.md#breakpoint_csqc) | Ставит брейкпоинт в CSQC по файлу и строке. |
| [`breakpoint_menuqc`](../44-cli-commands-reference/02-client-ui-commands.md#breakpoint_menuqc) | Ставит брейкпоинт в MenuQC по файлу и строке. |
| [`cl_cmd`](../44-cli-commands-reference/02-client-ui-commands.md#cl_cmd) | Передает аргументы в обработчик `GameCommand` клиентского CSQC. |
| [`coredump_csqc`](../44-cli-commands-reference/02-client-ui-commands.md#coredump_csqc) | Сохраняет отладочный дамп состояния CSQC. |
| [`coredump_menuqc`](../44-cli-commands-reference/02-client-ui-commands.md#coredump_menuqc) | Сохраняет отладочный дамп состояния MenuQC. |
| [`edit`](../44-cli-commands-reference/02-client-ui-commands.md#edit) | Открывает встроенный текстовый редактор на указанном файле и строке. |
| [`extensionlist_csqc`](../44-cli-commands-reference/02-client-ui-commands.md#extensionlist_csqc) | Показывает список расширений, видимых CSQC. |
| [`loadfont`](../44-cli-commands-reference/02-client-ui-commands.md#loadfont) | Загружает шрифт для MenuQC и интерфейса. |
| [`menu_cmd`](../44-cli-commands-reference/02-client-ui-commands.md#menu_cmd) | Передает аргументы в обработчик `GameCommand` MenuQC. |
| [`poke_csqc`](../44-cli-commands-reference/02-client-ui-commands.md#poke_csqc) | Вычисляет или изменяет выражение в отладочном контексте CSQC. |
| [`poke_menuqc`](../44-cli-commands-reference/02-client-ui-commands.md#poke_menuqc) | Вычисляет или изменяет выражение в отладочном контексте MenuQC. |
| [`profile_csqc`](../44-cli-commands-reference/02-client-ui-commands.md#profile_csqc) | Управляет профилированием CSQC. |
| [`profile_menuqc`](../44-cli-commands-reference/02-client-ui-commands.md#profile_menuqc) | Управляет профилированием MenuQC. |
| [`watchpoint_csqc`](../44-cli-commands-reference/02-client-ui-commands.md#watchpoint_csqc) | Создает точку наблюдения для выражения в CSQC. |
| [`watchpoint_menuqc`](../44-cli-commands-reference/02-client-ui-commands.md#watchpoint_menuqc) | Создает точку наблюдения для выражения в MenuQC. |
| [`globalservers`](../44-cli-commands-reference/02-client-ui-commands.md#globalservers) | Запрашивает список серверов у мастер-сервера по протоколу и ключевым словам. |
| [`localservers`](../44-cli-commands-reference/02-client-ui-commands.md#localservers) | Ишет локальные серверы широковещателным запросом в подсети. |
| [`setmaster`](../44-cli-commands-reference/02-client-ui-commands.md#setmaster) | Показывает или меняет список мастер-серверов, используемых клиентом. |
| [`force_centerview`](../44-cli-commands-reference/02-client-ui-commands.md#force_centerview) | Совместимая Win32-команда принудительного центрирования взгляда; в других сборках может отсутствовать. |
| [`windows`](../44-cli-commands-reference/02-client-ui-commands.md#windows) | Открывает или активирует Win32-список окон или панелей клиента, если он поддерживается сборкой. |
| [`sys_openfile`](../44-cli-commands-reference/02-client-ui-commands.md#sys_openfile) | Открывает системный диалог выбора файла. |
| [`sys_register_file_associations`](../44-cli-commands-reference/02-client-ui-commands.md#sys_register_file_associations) | Регистрирует файловые ассоциации через платформенный бэкенд; доступность зависит от ОС или веб-оболочки. |
| [`sys_browserredirect`](../44-cli-commands-reference/02-client-ui-commands.md#sys_browserredirect) | Перенаправляет встроенный браузер или веб-шелл на другой URL. |
| [`menu_somemenu`](../44-cli-commands-reference/02-client-ui-commands.md#menu_somemenu) | Это не реальная пользовательская команда, а демонстрационный пример-заглушка. |

### Рендер и звук

| Элемент | Назначение |
|---|---|
| [`mod_terrain_save`](../44-cli-commands-reference/03-rendering-sound-commands.md#mod_terrain_save) | Сохраняет текущую heightmap-карту или указанную карту на диск. |
| [`mod_terrain_reload`](../44-cli-commands-reference/03-rendering-sound-commands.md#mod_terrain_reload) | Перезагружает terrain-модель. |
| [`mod_terrain_create`](../44-cli-commands-reference/03-rendering-sound-commands.md#mod_terrain_create) | Создаёт новый файл `maps/<name>.hmp` с базовым `worldspawn`, стартовой точкой, небом, текстурами грунта/воды и начальными высотами. |
| [`mod_terrain_convert`](../44-cli-commands-reference/03-rendering-sound-commands.md#mod_terrain_convert) | Переписывает terrain в текущий внутренний формат, проходя по секциям и сохраняя их заново. |
| [`sky`](../44-cli-commands-reference/03-rendering-sound-commands.md#sky) | Устанавливает skybox через совместимое с Quakespasm имя команды, но по строке регистрации сам движок рекомендует использовать `r_skybox`. |
| [`loadsky`](../44-cli-commands-reference/03-rendering-sound-commands.md#loadsky) | Устанавливает skybox через совместимое с DarkPlaces имя команды; по описанию в коде это alias-совместимость, а не отдельный механизм. |
| [`listskyboxes`](../44-cli-commands-reference/03-rendering-sound-commands.md#listskyboxes) | Показывает список доступных пользовательских skybox-наборов, которые можно выбрать через `r_skybox`. |
| [`sv_saveentfile`](../44-cli-commands-reference/03-rendering-sound-commands.md#sv_saveentfile) | Сохраняет сущности текущей или указанной карты в `.ent`-файл замены для дальнейшего редактирования. |
| [`mod_showent`](../44-cli-commands-reference/03-rendering-sound-commands.md#mod_showent) | Ищет сущности карты по индексу или по вхождению текста в key/value-данные и печатает найденные записи в консоль. |
| [`mod_memlist`](../44-cli-commands-reference/03-rendering-sound-commands.md#mod_memlist) | Выводит список известных моделей и объём памяти, занятый каждой из них, а в конце — общий итог. |
| [`mod_batchlist`](../44-cli-commands-reference/03-rendering-sound-commands.md#mod_batchlist) | Печатает список батчей для загруженных brush-моделей вместе с шейдерами, lightmap-данными, envmap и числом поверхностей. |
| [`mod_texturelist`](../44-cli-commands-reference/03-rendering-sound-commands.md#mod_texturelist) | Показывает текстуры загруженных brush-моделей, число батчей на каждую и, при ненулевом `preview_size`, встроенные превью. |
| [`mod_usetexture`](../44-cli-commands-reference/03-rendering-sound-commands.md#mod_usetexture) | Подменяет базовую текстуру с указанным именем на однотонную 1×1 текстуру заданного RGB-цвета. |
| [`version_modelformats`](../44-cli-commands-reference/03-rendering-sound-commands.md#version_modelformats) | Выводит список зарегистрированных загрузчиков/форматов моделей, известных текущей сборке движка. |
| [`r_imagelist`](../44-cli-commands-reference/03-rendering-sound-commands.md#r_imagelist) | Печатает список всех текстур, о которых знает движок в текущий момент. |
| [`r_imageformats`](../44-cli-commands-reference/03-rendering-sound-commands.md#r_imageformats) | Показывает список доступных аппаратных пиксельных форматов, пригодных для загрузки/хранения текстур. |
| [`r_editlights_reload`](../44-cli-commands-reference/03-rendering-sound-commands.md#r_editlights_reload) | Перезагружает статические realtime-источники света и при желании принудительно выбирает их источник: `.rtlights`, статические сущности, BSP-сущности или пустой набор. |
| [`r_editlights_save`](../44-cli-commands-reference/03-rendering-sound-commands.md#r_editlights_save) | Сохраняет текущие realtime-источники света в `maps/FOO.rtlights` для активной карты. |
| [`r_editlights_spawn`](../44-cli-commands-reference/03-rendering-sound-commands.md#r_editlights_spawn) | Создаёт новый источник света в текущей позиции курсора редактора света и сразу делает его выбранным. |
| [`r_editlights_clone`](../44-cli-commands-reference/03-rendering-sound-commands.md#r_editlights_clone) | Дублирует выбранный источник света, копируя его параметры, но переносит новый экземпляр в текущую позицию курсора. |
| [`r_editlights_remove`](../44-cli-commands-reference/03-rendering-sound-commands.md#r_editlights_remove) | Удаляет текущий выбранный источник света из набора редактирования. |
| [`r_editlights_edit`](../44-cli-commands-reference/03-rendering-sound-commands.md#r_editlights_edit) | Показывает свойства выбранного источника света или меняет одно из именованных полей. |
| [`r_editlights_editall`](../44-cli-commands-reference/03-rendering-sound-commands.md#r_editlights_editall) | Работает как `r_editlights_edit`, но применяет изменение ко всем живым realtime-источникам света сразу. |
| [`r_editlights_toggleshadow`](../44-cli-commands-reference/03-rendering-sound-commands.md#r_editlights_toggleshadow) | Переключает флаг отбрасывания теней у выбранного источника света. |
| [`r_editlights_togglecorona`](../44-cli-commands-reference/03-rendering-sound-commands.md#r_editlights_togglecorona) | Включает или выключает corona-эффект у выбранного источника света. |
| [`r_editlights_copyinfo`](../44-cli-commands-reference/03-rendering-sound-commands.md#r_editlights_copyinfo) | Копирует свойства выбранного источника света во внутренний буфер, не меняя сцену. |
| [`r_editlights_pasteinfo`](../44-cli-commands-reference/03-rendering-sound-commands.md#r_editlights_pasteinfo) | Применяет сохранённые свойства из буфера к выбранному источнику света, сохраняя его текущую позицию. |
| [`r_editlights_lock`](../44-cli-commands-reference/03-rendering-sound-commands.md#r_editlights_lock) | Блокирует или разблокирует автоматическую смену текущего света по положению прицела/курсора. |
| [`vid_recenter`](../44-cli-commands-reference/03-rendering-sound-commands.md#vid_recenter) | Перемещает и перепривязывает оконный рендер-контекст, обновляя координаты, размер и при необходимости родительское окно Win32. |
| [`timerefresh`](../44-cli-commands-reference/03-rendering-sound-commands.md#timerefresh) | Запускает классический тест скорости рендера, многократно вращая камеру и измеряя время кадра. |
| [`r_part`](../44-cli-commands-reference/03-rendering-sound-commands.md#r_part) | Загружает/переопределяет описание particle-эффекта через многострочный блок в конфиге. |
| [`r_partredirect`](../44-cli-commands-reference/03-rendering-sound-commands.md#r_partredirect) | Управляет переадресацией имён particle-эффектов: без аргументов выводит все алиасы, с одним аргументом показывает текущую привязку, а с двумя — создаёт или заменяет перенаправление. |
| [`+voip`](../44-cli-commands-reference/03-rendering-sound-commands.md#voip) | Зажимает кнопку голосовой передачи и включает отправку микрофона, если вызов не заблокирован как insecure. |
| [`-voip`](../44-cli-commands-reference/03-rendering-sound-commands.md#-voip) | Отпускает кнопку голосовой передачи и прекращает активную VOIP-передачу. |
| [`voip`](../44-cli-commands-reference/03-rendering-sound-commands.md#voip) | Меняет параметры VOIP-предобработки; в текущем коде распознаётся подкоманда `maxgain` для настройки AGC у Speex. |
| [`play`](../44-cli-commands-reference/03-rendering-sound-commands.md#play) | Проигрывает один или несколько звуков по имени, автоматически дописывая `.wav`, если расширение не указано. |
| [`play2`](../44-cli-commands-reference/03-rendering-sound-commands.md#play2) | Регистрируется на тот же обработчик, что и `play`, и принимает тот же список имён звуков. |
| [`playvol`](../44-cli-commands-reference/03-rendering-sound-commands.md#playvol) | Проигрывает один или несколько звуков с явным указанием громкости после каждого имени. |
| [`stopsound`](../44-cli-commands-reference/03-rendering-sound-commands.md#stopsound) | Немедленно останавливает все текущие звуки. |
| [`soundlist`](../44-cli-commands-reference/03-rendering-sound-commands.md#soundlist) | Печатает список известных звуковых ресурсов, их формат, размер, длительность и признак зацикливания, а в конце — суммарный объём резидентных данных. |
| [`soundinfo`](../44-cli-commands-reference/03-rendering-sound-commands.md#soundinfo) | Показывает информацию об активных аудиоустройствах: имя, число каналов, частоту, разрядность, размер буфера и статистику по каналам. |
| [`snd_restart`](../44-cli-commands-reference/03-rendering-sound-commands.md#snd_restart) | Заново перечисляет аудиоустройства и перезапускает звуковую подсистему. |
| [`soundcontrol`](../44-cli-commands-reference/03-rendering-sound-commands.md#soundcontrol) | Служебная команда управления звуком: может полностью выключить подсистему, поменять частоту через `rate`/`speed` или перенастроить конкретную карту (`cardN`) в режимы `mono`, `stereo`/`standard`, `swap`, `front` и `back`. |
| [`xr_toggle`](../44-cli-commands-reference/03-rendering-sound-commands.md#xr_toggle) | Переключает состояние WebXR-сессии. |
| [`xr_start`](../44-cli-commands-reference/03-rendering-sound-commands.md#xr_start) | Запускает WebXR в любом доступном режиме. |
| [`xr_end`](../44-cli-commands-reference/03-rendering-sound-commands.md#xr_end) | Завершает активную WebXR-сессию. |
| [`xr_start_inline`](../44-cli-commands-reference/03-rendering-sound-commands.md#xr_start_inline) | Перезапускает WebXR и открывает inline-сеанс. |
| [`xr_start_vr`](../44-cli-commands-reference/03-rendering-sound-commands.md#xr_start_vr) | Перезапускает WebXR и пытается открыть immersive VR-сеанс. |
| [`xr_start_ar`](../44-cli-commands-reference/03-rendering-sound-commands.md#xr_start_ar) | Перезапускает WebXR и пытается открыть immersive AR-сеанс. |
| [`xr_info`](../44-cli-commands-reference/03-rendering-sound-commands.md#xr_info) | Показывает, доступны ли inline/VR/AR-режимы WebXR и активна ли текущая сессия. |
| [`mod_terrain_export`](../44-cli-commands-reference/03-rendering-sound-commands.md#mod_terrain_export) | Экспортирует terrain в сырой heightmap-формат `.r16`, разбивая данные по плиткам при необходимости. |
| [`mod_terrain_import`](../44-cli-commands-reference/03-rendering-sound-commands.md#mod_terrain_import) | Импортирует сырой heightmap из `maps/<map>.r16` обратно в terrain. |
| [`makewad`](../44-cli-commands-reference/03-rendering-sound-commands.md#makewad) | Собирает WAD2 из изображения, используя необязательный коэффициент масштабирования (по умолчанию `2`). |

### Сервер и мультиплеер

| Элемент | Назначение |
|---|---|
| [`quit`](../44-cli-commands-reference/04-server-multiplayer-commands.md#quit) | Завершает работу движка из серверной консоли. |
| [`say`](../44-cli-commands-reference/04-server-multiplayer-commands.md#say) | Отправляет сообщение от `{console}` всем подключённым игрокам сервера. |
| [`sayone`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sayone) | Отправляет адресное сообщение от серверной консоли всем клиентам, совпавшим с первым аргументом. |
| [`tell`](../44-cli-commands-reference/04-server-multiplayer-commands.md#tell) | Алиас `sayone`: шлёт приватное сообщение выбранному игроку из консоли сервера. |
| [`god`](../44-cli-commands-reference/04-server-multiplayer-commands.md#god) | Включает/выключает режим неуязвимости для текущего серверного игрока/оператора. |
| [`give`](../44-cli-commands-reference/04-server-multiplayer-commands.md#give) | Чит-команда для выдачи значений/предметов выбранному игроку; обработчик читает цель, тип и числовое значение из аргументов. |
| [`noclip`](../44-cli-commands-reference/04-server-multiplayer-commands.md#noclip) | Включает/выключает проход сквозь геометрию для текущего серверного игрока/оператора. |
| [`download`](../44-cli-commands-reference/04-server-multiplayer-commands.md#download) | Запускает встроенную загрузку файла по HTTP/HTTPS/FTP и сохраняет его в игровой ФС; при отсутствии второго аргумента имя берётся из URL. |
| [`sv_impulse`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sv_impulse) | Создаёт временного серверного клиента, прогоняет его через `ClientConnect`/`PutClientInServer` и вызывает указанный импульс в SSQC. |
| [`fraglogfile`](../44-cli-commands-reference/04-server-multiplayer-commands.md#fraglogfile) | Переключает запись фрагов в `frag_N.log`; повторный вызов закрывает текущий файл. |
| [`snap`](../44-cli-commands-reference/04-server-multiplayer-commands.md#snap) | Запрашивает удалённый скриншот у конкретного клиента по user id. |
| [`snapall`](../44-cli-commands-reference/04-server-multiplayer-commands.md#snapall) | Отправляет запрос удалённого скриншота всем активным не-наблюдателям. |
| [`kick`](../44-cli-commands-reference/04-server-multiplayer-commands.md#kick) | Удаляет игрока с сервера по имени, user id или IP. |
| [`clientkick`](../44-cli-commands-reference/04-server-multiplayer-commands.md#clientkick) | Кикает клиента по номеру слота, что используется для q3-совместимого меню управления ботами/игроками. |
| [`renameclient`](../44-cli-commands-reference/04-server-multiplayer-commands.md#renameclient) | Принудительно меняет `name` в userinfo выбранного клиента и рассылает обновление всем остальным. |
| [`mute`](../44-cli-commands-reference/04-server-multiplayer-commands.md#mute) | Переключает штраф `BAN_MUTE`: блокирует чат и голос выбранного игрока. |
| [`stealthmute`](../44-cli-commands-reference/04-server-multiplayer-commands.md#stealthmute) | Тихий вариант `mute`: игрок продолжает видеть, будто его сообщения уходят, но сервер их не ретранслирует. |
| [`cuff`](../44-cli-commands-reference/04-server-multiplayer-commands.md#cuff) | Переключает штраф, запрещающий игроку атаковать. |
| [`cripple`](../44-cli-commands-reference/04-server-multiplayer-commands.md#cripple) | Переключает штраф, блокирующий движение игрока. |
| [`ban`](../44-cli-commands-reference/04-server-multiplayer-commands.md#ban) | Добавляет IP-бан или иной penalty-запрет; без `flags` в этой команде по умолчанию используется `BAN_BAN`. |
| [`banname`](../44-cli-commands-reference/04-server-multiplayer-commands.md#banname) | Устаревший алиас `ban` для совместимости со старым серверным администрированием. |
| [`banlist`](../44-cli-commands-reference/04-server-multiplayer-commands.md#banlist) | Показывает только записи, в которых установлен собственно флаг IP-бана, с остатком времени и причиной. |
| [`unban`](../44-cli-commands-reference/04-server-multiplayer-commands.md#unban) | Алиас `removeip`: снимает ban/penalty-флаги у указанного адреса либо очищает весь список. |
| [`addip`](../44-cli-commands-reference/04-server-multiplayer-commands.md#addip) | Низкоуровневая команда добавления записи в penalty-список. |
| [`removeip`](../44-cli-commands-reference/04-server-multiplayer-commands.md#removeip) | Снимает один или несколько penalty-флагов у указанного адреса; без `flags` удаляет все флаги записи. |
| [`listip`](../44-cli-commands-reference/04-server-multiplayer-commands.md#listip) | Печатает весь список penalties, а не только баны: адрес, набор флагов и оставшееся время. |
| [`writeip`](../44-cli-commands-reference/04-server-multiplayer-commands.md#writeip) | Сохраняет текущий список penalties в `listip.cfg` как набор команд `addip ...`. |
| [`floodprot`](../44-cli-commands-reference/04-server-multiplayer-commands.md#floodprot) | Без аргументов показывает текущие параметры flood protection; с тремя аргументами меняет лимиты сообщений, интервал и время заглушения. |
| [`stuffcmd`](../44-cli-commands-reference/04-server-multiplayer-commands.md#stuffcmd) | Отправляет выбранному клиенту whitelisted-консольную команду, эмулируя серверный `stuffcmd`. |
| [`status`](../44-cli-commands-reference/04-server-multiplayer-commands.md#status) | Показывает текущее состояние сервера и клиентов. |
| [`serverinfo`](../44-cli-commands-reference/04-server-multiplayer-commands.md#serverinfo) | Без аргументов печатает public `serverinfo`; с парой `key/value` меняет запись и, если ключ соответствует cvar, синхронизирует и её. |
| [`serverinfoblob`](../44-cli-commands-reference/04-server-multiplayer-commands.md#serverinfoblob) | Загружает содержимое файла и сохраняет его как binary/blob-значение в `serverinfo`. |
| [`localinfo`](../44-cli-commands-reference/04-server-multiplayer-commands.md#localinfo) | Без аргументов печатает `svs.localinfo`; с парой значений меняет локальный ключ и вызывает `PR_LocalInfoChanged`. |
| [`user`](../44-cli-commands-reference/04-server-multiplayer-commands.md#user) | Печатает расширенную информацию о клиенте: userinfo, сетевые расширения, счётчики и другую диагностическую сводку. |
| [`sv`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sv) | Передаёт остаток строки в игровой код (`PR_ConsoleCmd`, q1qvm/q2/q3 backend'ы). |
| [`mod`](../44-cli-commands-reference/04-server-multiplayer-commands.md#mod) | Алиас `sv` с тем же поведением: проксирует строку в серверный игровой код. |
| [`precaches`](../44-cli-commands-reference/04-server-multiplayer-commands.md#precaches) | Показывает текущие серверные precache-списки. |
| [`heartbeat`](../44-cli-commands-reference/04-server-multiplayer-commands.md#heartbeat) | Форсирует повторный резолв/обновление адресов мастер-сервера, чтобы сервер снова отправил heartbeat. |
| [`gamedir`](../44-cli-commands-reference/04-server-multiplayer-commands.md#gamedir) | Без аргументов печатает текущий gamedir; с аргументами реально переключает поисковые пути сервера на новый gamedir/набор gamedir'ов. |
| [`sv_gamedir`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sv_gamedir) | Меняет только публикуемое клиентам значение `*gamedir`, не трогая реальные поисковые пути сервера. |
| [`sv_settimer`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sv_settimer) | Планирует повторный запуск серверной команды: `count` раз, через `interval` секунд; `-1` означает бесконечный повтор. |
| [`sv_meminfo`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sv_meminfo) | Печатает память моделей и приблизительную память/буферы по активным клиентам, а также размер SSQC string table. |
| [`pin_save`](../44-cli-commands-reference/04-server-multiplayer-commands.md#pin_save) | Сохраняет pinned-сообщения сервера на диск. |
| [`pin_reload`](../44-cli-commands-reference/04-server-multiplayer-commands.md#pin_reload) | Перечитывает pinned-сообщения с диска. |
| [`pin_delete`](../44-cli-commands-reference/04-server-multiplayer-commands.md#pin_delete) | Удаляет самое старое pinned-сообщение. |
| [`pin_add`](../44-cli-commands-reference/04-server-multiplayer-commands.md#pin_add) | Создаёт новое pinned-сообщение, используя первые два аргумента как автора и текст. |
| [`ssv`](../44-cli-commands-reference/04-server-multiplayer-commands.md#ssv) | В кластере без аргументов показывает список активных subserver'ов; с одним `id` в клиентской сборке открывает их подконсоль; с `id` и командой пересылает команду выбранному subserver'у. |
| [`ssv_all`](../44-cli-commands-reference/04-server-multiplayer-commands.md#ssv_all) | Шлёт одну и ту же команду всем subserver'ам кластера. |
| [`mapcluster`](../44-cli-commands-reference/04-server-multiplayer-commands.md#mapcluster) | Переводит сервер в режим cluster gateway и поднимает отдельные процессы для карт; первый аргумент задаёт стартовую карту для новых клиентов. |
| [`killserver`](../44-cli-commands-reference/04-server-multiplayer-commands.md#killserver) | Мгновенно останавливает текущий сервер и снимает спавн сервера без запуска новой карты. |
| [`map`](../44-cli-commands-reference/04-server-multiplayer-commands.md#map) | Запускает новую игру на указанной карте. |
| [`mapedit`](../44-cli-commands-reference/04-server-multiplayer-commands.md#mapedit) | Загружает карту без активного gamecode, то есть в режиме редактирования/диагностики. |
| [`spmap`](../44-cli-commands-reference/04-server-multiplayer-commands.md#spmap) | Q3-совместимая одиночная загрузка карты с очисткой spawn parameters. |
| [`spdevmap`](../44-cli-commands-reference/04-server-multiplayer-commands.md#spdevmap) | Q3-совместимая одиночная загрузка карты в developer/cheat-режиме. |
| [`devmap`](../44-cli-commands-reference/04-server-multiplayer-commands.md#devmap) | Загружает карту с принудительным чит-режимом (`sv_cheats 1`). |
| [`gamemap`](../44-cli-commands-reference/04-server-multiplayer-commands.md#gamemap) | Вариант смены карты с Quake II-семантикой сохранения состояния между уровнями; обработчик отдельно помечает его для save-to-slot-0 логики. |
| [`changelevel`](../44-cli-commands-reference/04-server-multiplayer-commands.md#changelevel) | Продолжает текущую игру на другой карте и, если указан `startspot`, позволяет вернуться к предыдущей структуре progress/hub-состояния. |
| [`map_restart`](../44-cli-commands-reference/04-server-multiplayer-commands.md#map_restart) | Перезапускает текущую карту и сервер; для некоторых игр понимает специальные аргументы вроде `restore`/`initial`. |
| [`listmaps`](../44-cli-commands-reference/04-server-multiplayer-commands.md#listmaps) | Печатает список карт, найденных в `maps/` и вложенных подпапках. |
| [`maplist`](../44-cli-commands-reference/04-server-multiplayer-commands.md#maplist) | Алиас `listmaps`: показывает установленные карты. |
| [`maps`](../44-cli-commands-reference/04-server-multiplayer-commands.md#maps) | Ещё один алиас списка карт. |
| [`savegame_legacy`](../44-cli-commands-reference/04-server-multiplayer-commands.md#savegame_legacy) | Сохраняет игру в vanilla Quake-совместимом формате с потерей всего, что он не умеет хранить. |
| [`savegame`](../44-cli-commands-reference/04-server-multiplayer-commands.md#savegame) | Сохраняет игру в штатный формат FTEQW. |
| [`loadgame`](../44-cli-commands-reference/04-server-multiplayer-commands.md#loadgame) | Загружает сохранённую игру. |
| [`save`](../44-cli-commands-reference/04-server-multiplayer-commands.md#save) | Короткий алиас `savegame`. |
| [`load`](../44-cli-commands-reference/04-server-multiplayer-commands.md#load) | Короткий алиас `loadgame`. |
| [`unsavegame`](../44-cli-commands-reference/04-server-multiplayer-commands.md#unsavegame) | Удаляет сохранение с диска. |
| [`playmvd`](../44-cli-commands-reference/04-server-multiplayer-commands.md#playmvd) | Открывает и начинает проигрывать серверную multi-view demo (`.mvd` или путь в `demos/`). |
| [`mvdplay`](../44-cli-commands-reference/04-server-multiplayer-commands.md#mvdplay) | Алиас `playmvd` с тем же multi-view playback. |
| [`svplay`](../44-cli-commands-reference/04-server-multiplayer-commands.md#svplay) | Регистрируется рядом с MVD-playback как отдельная команда проигрывания server-side demo. |
| [`svrecord`](../44-cli-commands-reference/04-server-multiplayer-commands.md#svrecord) | Регистрируется в server-demo подсистеме как команда записи server-side demo. |
| [`record`](../44-cli-commands-reference/04-server-multiplayer-commands.md#record) | Начинает MVD-запись в `sv_demoDir`, очищая имя и автоматически подставляя `.mvd`/`.mvd.gz`. |
| [`stop`](../44-cli-commands-reference/04-server-multiplayer-commands.md#stop) | Останавливает текущую MVD-запись и закрывает файл. |
| [`cancel`](../44-cli-commands-reference/04-server-multiplayer-commands.md#cancel) | Останавливает текущую MVD-запись и удаляет получившийся demo-файл. |
| [`easyrecord`](../44-cli-commands-reference/04-server-multiplayer-commands.md#easyrecord) | Автоматически создаёт удобное имя MVD по карте/командам/игрокам или использует переданный аргумент. |
| [`demolist`](../44-cli-commands-reference/04-server-multiplayer-commands.md#demolist) | Показывает демки из `sv_demoDir`, при необходимости фильтруя список по подстрокам из аргументов, и печатает размер каталога. |
| [`rmdemo`](../44-cli-commands-reference/04-server-multiplayer-commands.md#rmdemo) | Удаляет одну demo, все demo или все demo с подстрокой `token`; если удаляется активная запись, она сначала останавливается. |
| [`rmdemonum`](../44-cli-commands-reference/04-server-multiplayer-commands.md#rmdemonum) | Удаляет demo по номеру из списка `demolist`. |
| [`sv_demorecord`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sv_demorecord) | Полное server-side имя для начала MVD-записи; делает то же, что `record`. |
| [`sv_demostop`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sv_demostop) | Полное server-side имя для остановки MVD-записи. |
| [`sv_democancel`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sv_democancel) | Полное server-side имя для отмены и удаления текущей MVD-записи. |
| [`sv_demoeasyrecord`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sv_demoeasyrecord) | Полное server-side имя для `easyrecord`. |
| [`sv_demolist`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sv_demolist) | Полное server-side имя для `demolist`. |
| [`sv_demoremove`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sv_demoremove) | Полное server-side имя для `rmdemo`. |
| [`sv_demonumremove`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sv_demonumremove) | Полное server-side имя для `rmdemonum`. |
| [`mvdrecord`](../44-cli-commands-reference/04-server-multiplayer-commands.md#mvdrecord) | Старое FTE-имя начала MVD-записи, оставленное для совместимости. |
| [`mvdstop`](../44-cli-commands-reference/04-server-multiplayer-commands.md#mvdstop) | Старое FTE-имя остановки MVD-записи. |
| [`mvdcancel`](../44-cli-commands-reference/04-server-multiplayer-commands.md#mvdcancel) | Старое FTE-имя отмены и удаления MVD-записи. |
| [`mvdlist`](../44-cli-commands-reference/04-server-multiplayer-commands.md#mvdlist) | Старое FTE-имя для списка MVD-файлов. |
| [`mvdplaynum`](../44-cli-commands-reference/04-server-multiplayer-commands.md#mvdplaynum) | Берёт demo по номеру из списка и превращает её в `mvdplay <name>`. |
| [`sv_demoinfoadd`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sv_demoinfoadd) | Добавляет текстовый `.txt`-sidecar к demo: `*` означает текущую запись, `**` — загрузить содержимое из файла. |
| [`sv_demoinforemove`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sv_demoinforemove) | Удаляет текстовый `.txt`-sidecar у demo по номеру или у текущей записи. |
| [`sv_demoinfo`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sv_demoinfo) | Печатает содержимое текстового `.txt`-sidecar, связанного с demo. |
| [`qtvreverse`](../44-cli-commands-reference/04-server-multiplayer-commands.md#qtvreverse) | Открывает reverse-QTV TCP-соединение к удалённому адресу и начинает hand-shake `QTV REVERSE`. |
| [`breakpoint`](../44-cli-commands-reference/04-server-multiplayer-commands.md#breakpoint) | Переключает breakpoint в серверном QC по имени файла и номеру строки. |
| [`watchpoint`](../44-cli-commands-reference/04-server-multiplayer-commands.md#watchpoint) | Ставит или снимает watchpoint на указанную переменную/выражение в SSQC. |
| [`watchpoint_ssqc`](../44-cli-commands-reference/04-server-multiplayer-commands.md#watchpoint_ssqc) | Алиас `watchpoint` для явного SSQC-контекста. |
| [`decompile`](../44-cli-commands-reference/04-server-multiplayer-commands.md#decompile) | Декомпилирует `qwprogs.dat` или указанный файл progs через встроенный QC backend. |
| [`compile`](../44-cli-commands-reference/04-server-multiplayer-commands.md#compile) | Запускает встроенный компилятор QC: без аргументов ищет `progs.src`, с одним аргументом подменяет src-файл, а с несколькими — передаёт их компилятору как есть. |
| [`applycompile`](../44-cli-commands-reference/04-server-multiplayer-commands.md#applycompile) | Сохраняет текущее состояние сущностей, переконфигурирует серверный progs и заново загружает состояние в новый SSQC. |
| [`coredump_ssqc`](../44-cli-commands-reference/04-server-multiplayer-commands.md#coredump_ssqc) | Сбрасывает состояние SSQC-сущностей в `ssqccore.txt`. |
| [`poke_ssqc`](../44-cli-commands-reference/04-server-multiplayer-commands.md#poke_ssqc) | Вычисляет отладочную строку в SSQC и печатает результат. |
| [`profile_ssqc`](../44-cli-commands-reference/04-server-multiplayer-commands.md#profile_ssqc) | Печатает накопленный профиль времени по QC-функциям; аргумент `1` запрещает очищать счётчики после вывода. |
| [`extensionlist_ssqc`](../44-cli-commands-reference/04-server-multiplayer-commands.md#extensionlist_ssqc) | Показывает доступные/неактивные SSQC extensions и builtins; набор битов в `flags` управляет тем, что именно выводить. |
| [`pr_dumpplatform`](../44-cli-commands-reference/04-server-multiplayer-commands.md#pr_dumpplatform) | Генерирует платформенную сводку/символьный дамп для QC-целей (`-F`, `-T`, `-O` и т. п.). |
| [`sv_lightstyle`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sv_lightstyle) | Чит-команда для просмотра или подмены lightstyle на сервере; с одним аргументом печатает стиль, с дополнительными — меняет его. |
| [`sqlstatus`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sqlstatus) | Печатает доступность MySQL/SQLite backend'ов, список SQL-соединений, очереди запросов и pending results. |
| [`sqlkill`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sqlkill) | Останавливает конкретное SQL-соединение по его номеру из `sqlstatus`. |
| [`sqlkillall`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sqlkillall) | Гасит все SQL-соединения сервера. |
| [`ranklist`](../44-cli-commands-reference/04-server-multiplayer-commands.md#ranklist) | Не печатает рейтинг в консоль, а выгружает полный список игроков в `list.txt` с именами, убийствами и смертями. |
| [`ranktopten`](../44-cli-commands-reference/04-server-multiplayer-commands.md#ranktopten) | Печатает в консоль первую десятку рейтинга. |
| [`rankfind`](../44-cli-commands-reference/04-server-multiplayer-commands.md#rankfind) | Ищет игроков в рейтинговой базе по wildcard-маске имени и печатает совпавшие id/имена. |
| [`rankremove`](../44-cli-commands-reference/04-server-multiplayer-commands.md#rankremove) | Удаляет запись рейтинга по порядковому номеру из списка лидеров, а не по внутреннему id. |
| [`rankrefresh`](../44-cli-commands-reference/04-server-multiplayer-commands.md#rankrefresh) | Сбрасывает накопленные on-server kills/deaths/time в рейтинговый файл для всех активных клиентов и переоткрывает базу рейтинга. |
| [`rankrconlevel`](../44-cli-commands-reference/04-server-multiplayer-commands.md#rankrconlevel) | Меняет trust/rcon-уровень рангового пользователя по его позиции в таблице. |
| [`rankadd`](../44-cli-commands-reference/04-server-multiplayer-commands.md#rankadd) | Добавляет пользователя в рейтинговую базу, опционально задавая числовой пароль и trust-level. |
| [`adduser`](../44-cli-commands-reference/04-server-multiplayer-commands.md#adduser) | Алиас `rankadd` с тем же поведением. |
| [`setpass`](../44-cli-commands-reference/04-server-multiplayer-commands.md#setpass) | Меняет числовой пароль существующего рейтингового пользователя. |
| [`gamealias`](../44-cli-commands-reference/04-server-multiplayer-commands.md#gamealias) | Добавляет альтернативное protocol/game-имя в мастер-серверную запись указанной игры. |
| [`gamelevelshotsurl`](../44-cli-commands-reference/04-server-multiplayer-commands.md#gamelevelshotsurl) | Назначает базовый URL, из которого браузеры смогут строить адреса изображений карт для данной игры. |
| [`openroute`](../44-cli-commands-reference/04-server-multiplayer-commands.md#openroute) | Отправляет out-of-band `hello` потенциальному клиенту, чтобы пробить маршрут/туннель до нового адреса. |
| [`route_visualise`](../44-cli-commands-reference/04-server-multiplayer-commands.md#route_visualise) | Запускает асинхронный расчёт и визуализацию маршрута от текущей позиции камеры к заданной точке. |
| [`route_reload`](../44-cli-commands-reference/04-server-multiplayer-commands.md#route_reload) | Сбрасывает кэш маршрутов/waynet для доступных миров и заставляет движок построить их заново при следующем запросе. |
| [`hide`](../44-cli-commands-reference/04-server-multiplayer-commands.md#hide) | Прячет окно серверной консоли Windows. |
| [`check_maps`](../44-cli-commands-reference/04-server-multiplayer-commands.md#check_maps) | Помечена как устаревшая ktpro-специфичная команда; встроенное описание рекомендует использовать `search_begin`. |
| [`sys_select_timeout`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sys_select_timeout) | Оставлена как заглушка совместимости: современный сервер сам троттлит цикл по tickrate. |
| [`sv_downloadchunksperframe`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sv_downloadchunksperframe) | Совместимый, но объявленный flawed/избыточным переключатель старой логики загрузок; современный код опирается на `drate/rate` клиента. |
| [`sv_speedcheck`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sv_speedcheck) | Устаревшая совместимая заглушка: проверка speedhack описана как заменённая более корректным учётом `movetime`. |
| [`sv_enableprofile`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sv_enableprofile) | Совместимая debug-заглушка, помеченная в описании как не реализованная. |
| [`sv_progsname`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sv_progsname) | Совместимый псевдоним, для которого описание предлагает использовать `sv_progs`. |
| [`download_map_url`](../44-cli-commands-reference/04-server-multiplayer-commands.md#download_map_url) | Устаревшая заглушка совместимости: встроенное описание считает её избыточной по сравнению с обычной загрузкой карт. |
| [`sv_progtype`](../44-cli-commands-reference/04-server-multiplayer-commands.md#sv_progtype) | Совместимая заглушка с рекомендацией использовать `sv_progs` вместо блокировки `.dll` через старый флаг. |
| [`svtestprogs`](../44-cli-commands-reference/04-server-multiplayer-commands.md#svtestprogs) | Находится в закомментированном блоке `/* #ifdef _DEBUG ... */`, поэтому фактически недоступна даже в debug-сборке, пока блок не раскомментируют. |
| [`reallyevilhack`](../44-cli-commands-reference/04-server-multiplayer-commands.md#reallyevilhack) | Регистрация закомментирована строкой `// Cmd_AddCommand(...)`, поэтому команда намеренно отключена и не попадает в рабочую таблицу команд. |

### Файловая система и системные

| Элемент | Назначение |
|---|---|
| [`cfg_save`](../44-cli-commands-reference/05-filesystem-system-commands.md#cfg_save) | Сохраняет конфиг в `configs/<name>.cfg`, а без аргумента — в основной конфиг манифеста/игры. |
| [`cfg_save_ifmodified`](../44-cli-commands-reference/05-filesystem-system-commands.md#cfg_save_ifmodified) | Делает то же, что `cfg_save`, но молча пропускает запись, если архивируемые cvar'ы не изменялись. |
| [`saveconfig`](../44-cli-commands-reference/05-filesystem-system-commands.md#saveconfig) | DP-совместимый вариант записи `.cfg`, который умеет писать в произвольный путь внутри разрешённой игровой ФС. |
| [`cfg_load`](../44-cli-commands-reference/05-filesystem-system-commands.md#cfg_load) | Загружает `configs/<name>.cfg`; при пустом аргументе берёт `mainconfig` из manifest либо `config.cfg`. |
| [`cfg_reset`](../44-cli-commands-reference/05-filesystem-system-commands.md#cfg_reset) | Сбрасывает cvar'ы через `cvarreset *`, но не восстанавливает автоматически алиасы и прочие пользовательские сущности. |
| [`resetcfg`](../44-cli-commands-reference/05-filesystem-system-commands.md#resetcfg) | Алиас `cfg_reset` для совместимости с другими движками. |
| [`exec`](../44-cli-commands-reference/05-filesystem-system-commands.md#exec) | Выполняет script-файл из игровой ФС, аккуратно обходя опасные пути, BOM и несколько известных несовместимых конфигов чужих движков. |
| [`echo`](../44-cli-commands-reference/05-filesystem-system-commands.md#echo) | Печатает остаток строки в консоль, предварительно разворачивая макросы и funchars по правилам текущего уровня исполнения. |
| [`alias`](../44-cli-commands-reference/05-filesystem-system-commands.md#alias) | Без аргументов выводит все алиасы; с аргументами создаёт/меняет alias, включая многострочные блоки `{ ... }`. |
| [`newalias`](../44-cli-commands-reference/05-filesystem-system-commands.md#newalias) | Вариант `alias`, который создаёт алиас только если его ещё не существует. |
| [`wait`](../44-cli-commands-reference/05-filesystem-system-commands.md#wait) | Откладывает исполнение оставшегося буфера команд до следующего кадра. |
| [`cmd`](../44-cli-commands-reference/05-filesystem-system-commands.md#cmd) | Клиентская обёртка, отправляющая остаток строки подключённому серверу без самого слова `cmd`. |
| [`condump`](../44-cli-commands-reference/05-filesystem-system-commands.md#condump) | Сбрасывает содержимое текущей консоли в текстовый файл (`condump.txt` по умолчанию). |
| [`aliasedit`](../44-cli-commands-reference/05-filesystem-system-commands.md#aliasedit) | Подставляет существующий алиас обратно в строку ввода консоли для интерактивного редактирования. |
| [`restrict`](../44-cli-commands-reference/05-filesystem-system-commands.md#restrict) | Показывает или меняет уровень допуска для команды, cvar или alias. |
| [`aliaslevel`](../44-cli-commands-reference/05-filesystem-system-commands.md#aliaslevel) | Показывает или меняет exec-level конкретного алиаса, то есть уровень, от имени которого он будет выполняться. |
| [`showalias`](../44-cli-commands-reference/05-filesystem-system-commands.md#showalias) | Печатает точное определение указанного alias или сообщает, что он не существует. |
| [`toggle`](../44-cli-commands-reference/05-filesystem-system-commands.md#toggle) | Переключает cvar между двумя состояниями. |
| [`set`](../44-cli-commands-reference/05-filesystem-system-commands.md#set) | Меняет значение cvar и при необходимости создаёт новый пользовательский cvar. |
| [`setfl`](../44-cli-commands-reference/05-filesystem-system-commands.md#setfl) | То же, что `set`, но третий аргумент позволяет задать флаги создаваемого cvar (`u`, `s`, `a`). |
| [`set_calc`](../44-cli-commands-reference/05-filesystem-system-commands.md#set_calc) | Присваивает cvar результат вычисления выражения. |
| [`set_tp`](../44-cli-commands-reference/05-filesystem-system-commands.md#set_tp) | Совместимый с ezQuake алиас `set`. |
| [`seta`](../44-cli-commands-reference/05-filesystem-system-commands.md#seta) | Как `set`, но принудительно включает флаг archive, чтобы значение всегда сохранялось в конфиг. |
| [`seta_calc`](../44-cli-commands-reference/05-filesystem-system-commands.md#seta_calc) | Комбинация `set_calc` и `seta`: вычисляет значение выражением и помечает cvar как сохраняемый. |
| [`vstr`](../44-cli-commands-reference/05-filesystem-system-commands.md#vstr) | Исполняет строковое значение cvar так, будто это alias/командная строка. |
| [`inc`](../44-cli-commands-reference/05-filesystem-system-commands.md#inc) | Прибавляет к cvar указанное значение; отрицательное число уменьшает значение. |
| [`if`](../44-cli-commands-reference/05-filesystem-system-commands.md#if) | Условное выполнение консольных команд. |
| [`cmdlist`](../44-cli-commands-reference/05-filesystem-system-commands.md#cmdlist) | Показывает список зарегистрированных команд, при желании отфильтрованный wildcard-маской имени. |
| [`aliaslist`](../44-cli-commands-reference/05-filesystem-system-commands.md#aliaslist) | Печатает список alias'ов; аргумент `server` оставляет только алиасы, пришедшие от сервера. |
| [`macrolist`](../44-cli-commands-reference/05-filesystem-system-commands.md#macrolist) | Показывает все доступные `$macro`-подстановки. |
| [`cvarlist`](../44-cli-commands-reference/05-filesystem-system-commands.md#cvarlist) | Показывает список cvar'ов; подробности фильтрации берутся из встроенной реализации `Cvar_List_f`. |
| [`cvarreset`](../44-cli-commands-reference/05-filesystem-system-commands.md#cvarreset) | Сбрасывает указанный cvar к его default-значению. |
| [`cvarwatch`](../44-cli-commands-reference/05-filesystem-system-commands.md#cvarwatch) | Включает уведомления об изменениях выбранного cvar и дополнительно отмечает начало/конец загрузки конфигов. |
| [`cvar_lockdefaults`](../44-cli-commands-reference/05-filesystem-system-commands.md#cvar_lockdefaults) | Фиксирует текущие значения почти всех обычных cvar как новые defaults, чтобы последующие reset/default-операции отталкивались именно от них. |
| [`cvar_purgedefaults`](../44-cli-commands-reference/05-filesystem-system-commands.md#cvar_purgedefaults) | Возвращает default-значения cvar'ов обратно к engine defaults, не меняя их активное текущее значение. |
| [`cvar_resettodefaults_nosaveonly`](../44-cli-commands-reference/05-filesystem-system-commands.md#cvar_resettodefaults_nosaveonly) | Массово возвращает default-значения только у несохраняемых cvar'ов. |
| [`cvar_resettodefaults_saveonly`](../44-cli-commands-reference/05-filesystem-system-commands.md#cvar_resettodefaults_saveonly) | Массово возвращает default-значения только у сохраняемых cvar'ов. |
| [`cvar_resettodefaults_all`](../44-cli-commands-reference/05-filesystem-system-commands.md#cvar_resettodefaults_all) | Массово возвращает default-значения у всех обычных cvar'ов. |
| [`apropos`](../44-cli-commands-reference/05-filesystem-system-commands.md#apropos) | Ищет подстроку по именам и описаниям cvar/команд и печатает совпадения. |
| [`find`](../44-cli-commands-reference/05-filesystem-system-commands.md#find) | Алиас `apropos` с тем же поиском по именам и описаниям. |
| [`in`](../44-cli-commands-reference/05-filesystem-system-commands.md#in) | Запланированно выполняет команду через заданную задержку; при `ruleset_allow_in 0` работает только для нулевой задержки. |
| [`defer`](../44-cli-commands-reference/05-filesystem-system-commands.md#defer) | Legacy-алиас `in`. |
| [`fs_restart`](../44-cli-commands-reference/05-filesystem-system-commands.md#fs_restart) | Перечитывает pack/manifest search paths и заново инициализирует файловую подсистему. |
| [`fs_changegame`](../44-cli-commands-reference/05-filesystem-system-commands.md#fs_changegame) | Переключает активный manifest/game: без аргументов выводит известные игры, с одним аргументом принимает gamedir, имя игры, `.fmf` или URL manifest, а с двумя — скачивает manifest под заданным именем и загружает его. |
| [`fs_changemod`](../44-cli-commands-reference/05-filesystem-system-commands.md#fs_changemod) | Служебный backend transient-установщика: собирает набор пакетов/директорий, переключает gamedir и может сразу выполнить `map`, `spmap` или `restart`. |
| [`fs_showmanifest`](../44-cli-commands-reference/05-filesystem-system-commands.md#fs_showmanifest) | Печатает содержимое текущего загруженного manifest, а если его нет — сообщает об этом. |
| [`fs_flush`](../44-cli-commands-reference/05-filesystem-system-commands.md#fs_flush) | Помечает кэш ФС как изменённый, чтобы движок обновил свои файловые представления. |
| [`dir`](../44-cli-commands-reference/05-filesystem-system-commands.md#dir) | Показывает содержимое игровой ФС с цветовой разметкой, размерами и hints для карт, моделей, demo и прочих ресурсов. |
| [`ls`](../44-cli-commands-reference/05-filesystem-system-commands.md#ls) | Алиас `dir`. |
| [`path`](../44-cli-commands-reference/05-filesystem-system-commands.md#path) | Печатает текущие search paths, а в pure-режиме ещё и отсутствующие/лишние пакеты. |
| [`flocate`](../44-cli-commands-reference/05-filesystem-system-commands.md#flocate) | Ищет файл в игровой ФС и показывает, из какого архива или каталога он реально берётся. |
| [`which`](../44-cli-commands-reference/05-filesystem-system-commands.md#which) | Алиас `flocate`. |
| [`fs_hash`](../44-cli-commands-reference/05-filesystem-system-commands.md#fs_hash) | Асинхронно вычисляет размеры и хэши файла (MD5/SHA1/SHA256 в доступной сборке) через loader worker. |
| [`logfile`](../44-cli-commands-reference/05-filesystem-system-commands.md#logfile) | Legacy-переключатель записи основного консольного лога: включает/выключает `log_enable[LOG_CONSOLE]` и печатает путь файла. |
| [`logplayers`](../44-cli-commands-reference/05-filesystem-system-commands.md#logplayers) | Legacy-переключатель player-лога. |
| [`logrcon`](../44-cli-commands-reference/05-filesystem-system-commands.md#logrcon) | Legacy-переключатель журнала rcon/frags. |
| [`identify`](../44-cli-commands-reference/05-filesystem-system-commands.md#identify) | Смотрит IP игрока и позволяет увидеть, не использовалось ли оно под другими никами. |
| [`ipmerge`](../44-cli-commands-reference/05-filesystem-system-commands.md#ipmerge) | Подмешивает внешнюю iplog-базу из файла в текущую память движка. |
| [`ipdump`](../44-cli-commands-reference/05-filesystem-system-commands.md#ipdump) | Сохраняет текущий IP-log в файл; перед записью сперва подмешивает уже существующий файл для более безопасной совместной работы нескольких процессов. |
| [`flush`](../44-cli-commands-reference/05-filesystem-system-commands.md#flush) | Сбрасывает кэши моделей, звуков, изображений и ragdoll-данных, а на glibc дополнительно зовёт `malloc_trim`. |
| [`hunkprint`](../44-cli-commands-reference/05-filesystem-system-commands.md#hunkprint) | Печатает накопленную и текущую статистику zone-allocations; в CRT debug-сборках дополнительно дампит объекты MSVC runtime heap. |
| [`zonegroups`](../44-cli-commands-reference/05-filesystem-system-commands.md#zonegroups) | Отладочная команда для именованных групп аллокаций zone. |
| [`net_ice_show`](../44-cli-commands-reference/05-filesystem-system-commands.md#net_ice_show) | Печатает отладочную информацию по ICE/WebRTC-состояниям; без аргументов выводит все, с аргументом — только выбранное соединение. |
| [`tls_provider_test`](../44-cli-commands-reference/05-filesystem-system-commands.md#tls_provider_test) | Запускает self-test каждого доступного TLS provider'а и печатает результат проверки подписи. |
| [`sv_addport`](../44-cli-commands-reference/05-filesystem-system-commands.md#sv_addport) | Без аргументов показывает активные server ports, а с адресом добавляет новый UDP/listen endpoint в коллекцию серверных сокетов. |
| [`cl_addport`](../44-cli-commands-reference/05-filesystem-system-commands.md#cl_addport) | Печатает активные client ports/addresses. |
| [`dtls_untrustall`](../44-cli-commands-reference/05-filesystem-system-commands.md#dtls_untrustall) | Очищает журнал доверенных DTLS-сертификатов. |
| [`dtls_importtrust`](../44-cli-commands-reference/05-filesystem-system-commands.md#dtls_importtrust) | Импортирует доверенные сертификаты из указанного файла или из default-источника при пустом аргументе. |
| [`plug_closeall`](../44-cli-commands-reference/05-filesystem-system-commands.md#plug_closeall) | Закрывает все загруженные плагины, уважая их `mayshutdown`-коллбеки. |
| [`plug_close`](../44-cli-commands-reference/05-filesystem-system-commands.md#plug_close) | Выгружает указанный плагин по имени или пути в `plugins/`. |
| [`plug_load`](../44-cli-commands-reference/05-filesystem-system-commands.md#plug_load) | Загружает плагин, автоматически достраивая платформенный префикс/суффикс (`plugin_...x64.dll` и т. п.). |
| [`plug_list`](../44-cli-commands-reference/05-filesystem-system-commands.md#plug_list) | Показывает загруженные плагины и доступные незагруженные бинарники в каталогах плагинов. |
| [`skel_info`](../44-cli-commands-reference/05-filesystem-system-commands.md#skel_info) | Печатает список активных skeleton/ragdoll-объектов, их тип, модель и дополнительные физические параметры. |
| [`skel_generateragdoll`](../44-cli-commands-reference/05-filesystem-system-commands.md#skel_generateragdoll) | Создаёт шаблонный `.doll`-файл для модели с костями и записывает туда кости, кадры и базовые ragdoll-комментарии. |
| [`com_reloadfilter`](../44-cli-commands-reference/05-filesystem-system-commands.md#com_reloadfilter) | Перечитывает `filter.txt` и пересобирает текстовый фильтр локализации/фильтрации. |
| [`mod_findcubemaps`](../44-cli-commands-reference/05-filesystem-system-commands.md#mod_findcubemaps) | Сканирует сущности текущей карты и ищет точки `env_cubemap` для отражений. |
| [`mod_buildcubemaps`](../44-cli-commands-reference/05-filesystem-system-commands.md#mod_buildcubemaps) | Строит cubemap-скриншоты для уже известных `env_cubemap` либо сперва автоматически ищет их на загруженной карте. |
| [`mod_realign`](../44-cli-commands-reference/05-filesystem-system-commands.md#mod_realign) | Читает текущий загруженный BSP и переписывает его назад только с правками выравнивания данных. |
| [`mod_bspx_list`](../44-cli-commands-reference/05-filesystem-system-commands.md#mod_bspx_list) | Показывает список lump'ов BSP/BSPX, их размеры и checksum; без аргумента использует текущую worldmodel. |
| [`mod_bspx_strip`](../44-cli-commands-reference/05-filesystem-system-commands.md#mod_bspx_strip) | Удаляет расширенный BSPX-lump из указанного BSP и переписывает файл. |
| [`mod_bspx_injest`](../44-cli-commands-reference/05-filesystem-system-commands.md#mod_bspx_injest) | Встраивает внешние ресурсы вроде `.lit`, `.lux` и `.ent` в новый BSPX, если они были запрошены аргументами. |
| [`pkg`](../44-cli-commands-reference/05-filesystem-system-commands.md#pkg) | Пакетный менеджер движка: установка, список, отключение и очистка пакетов через консоль. |
| [`version`](../44-cli-commands-reference/05-filesystem-system-commands.md#version) | Печатает ревизию движка, тип сборки, компилятор, архитектуру CPU, renderer'ы, библиотеки, игры и сетевые backend'ы. |
| [`worker_test`](../44-cli-commands-reference/05-filesystem-system-commands.md#worker_test) | Кладёт ping-задачу в loader worker и позже печатает round-trip-время. |
| [`worker_status`](../44-cli-commands-reference/05-filesystem-system-commands.md#worker_status) | Показывает число живых worker threads и количество ожидающих задач loader-очереди. |
| [`loopme`](../44-cli-commands-reference/05-filesystem-system-commands.md#loopme) | Специальная debug-команда, намеренно зацикливающая поток навсегда. |
| [`crashme`](../44-cli-commands-reference/05-filesystem-system-commands.md#crashme) | Специальная debug-команда, намеренно вызывающая крах записи по невалидному адресу. |
| [`errorme`](../44-cli-commands-reference/05-filesystem-system-commands.md#errorme) | Специальная debug-команда, намеренно вызывающая `Sys_Error`. |
| [`sys_openfile`](../44-cli-commands-reference/05-filesystem-system-commands.md#sys_openfile) | Открывает системный file picker. |
| [`sys_browserredirect`](../44-cli-commands-reference/05-filesystem-system-commands.md#sys_browserredirect) | Меняет адрес текущей страницы браузера и годится для web-сайтов, использующих quake maps как навигацию. |
| [`sys_register_file_associations`](../44-cli-commands-reference/05-filesystem-system-commands.md#sys_register_file_associations) | Просит браузер зарегистрировать страницу как обработчик `web+scheme`-URI из manifest. |
| [`zoneprint`](../44-cli-commands-reference/05-filesystem-system-commands.md#zoneprint) | Регистрация обёрнута в `#if 0`, поэтому команда намеренно выключена и не попадает в стандартную сборку. |

### Опциональные плагины

| Элемент | Назначение |
|---|---|
| [`show`](../44-cli-commands-reference/06-plugin-commands.md#show) | Показать элемент HUD по имени. |
| [`hide`](../44-cli-commands-reference/06-plugin-commands.md#hide) | Скрыть элемент HUD по имени. |
| [`move`](../44-cli-commands-reference/06-plugin-commands.md#move) | Переместить элемент HUD в указанные координаты. |
| [`place`](../44-cli-commands-reference/06-plugin-commands.md#place) | Начать интерактивное перетаскивание элемента мышью/клавиатурой. |
| [`reset`](../44-cli-commands-reference/06-plugin-commands.md#reset) | Вернуть элемент HUD к положению по умолчанию. |
| [`order`](../44-cli-commands-reference/06-plugin-commands.md#order) | Изменить порядок отрисовки (z-order) элемента HUD. |
| [`togglehud`](../44-cli-commands-reference/06-plugin-commands.md#togglehud) | Включить/выключить видимость всего HUD целиком. |
| [`align`](../44-cli-commands-reference/06-plugin-commands.md#align) | Привязать элемент HUD к краю экрана. |
| [`hud_recalculate`](../44-cli-commands-reference/06-plugin-commands.md#hud_recalculate) | Пересчитать расположение всех элементов HUD (например, после смены разрешения экрана). |
| [`hud_export`](../44-cli-commands-reference/06-plugin-commands.md#hud_export) | Сохранить текущую раскладку HUD в файл. |
| [`hud_editor`](../44-cli-commands-reference/06-plugin-commands.md#hud_editor) | Открыть визуальный редактор раскладки HUD. |
| [`ezhud_nquake`](../44-cli-commands-reference/06-plugin-commands.md#ezhud_nquake) | Переключить пресет HUD в стиль клиента nQuake. |
| [`hud_edit`](../44-cli-commands-reference/06-plugin-commands.md#hud_edit--sbar_edit) | Войти в интерактивный редактор расположения статус-бара. |
| [`hud_save`](../44-cli-commands-reference/06-plugin-commands.md#hud_save--sbar_save) | Сохранить текущую раскладку статус-бара. |
| [`hud_load`](../44-cli-commands-reference/06-plugin-commands.md#hud_load--sbar_load) | Загрузить раскладку статус-бара из файла. |
| [`hud_defaults`](../44-cli-commands-reference/06-plugin-commands.md#hud_defaults--sbar_defaults) | Сбросить раскладку статус-бара к значениям по умолчанию. |
| [`hud`](../44-cli-commands-reference/06-plugin-commands.md#hud--sbar) | Вывести в консоль текущее состояние/раскладку статус-бара. |
| [`tinfo`](../44-cli-commands-reference/06-plugin-commands.md#tinfo) | Показать отладочную информацию о командном (teamplay) статус-баре. |
| [`imapaccount`](../44-cli-commands-reference/06-plugin-commands.md#imapaccount) | Настроить учётную запись IMAP для проверки почты прямо из движка. |
| [`pop3account`](../44-cli-commands-reference/06-plugin-commands.md#pop3account) | Настроить учётную запись POP3 для проверки почты. |
| [`spaceinv`](../44-cli-commands-reference/06-plugin-commands.md#spaceinv) | Запустить встроенную мини-игру Space Invaders поверх движка (пасхалка/демонстрация плагинов). |
| [`startx`](../44-cli-commands-reference/06-plugin-commands.md#startx) | Статус: закомментировано в исходном коде — регистрация команды отключена. |

> [⬅ Предыдущая страница](../36-mobile-web-platforms/run-in-browser-webgl.md) | [Следующая страница ➡](../37-quakec-builtins-reference/00-entry-points.md)

> [⬅ Вернуться к оглавлению вики](../README.md)
