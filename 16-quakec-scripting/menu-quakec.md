# Логика игровых меню (MenuQC)

> [⬅ Предыдущая страница](client-side-quakec-csqc.md) | [Следующая страница ➡](../17-alternative-scripting-vms/server-side-lua.md)

> [⬅ Вернуться к оглавлению вики](../README.md)

## Что это даёт геймдизайнеру

MenuQC — это третий вид игровой логики на том же языке QuakeC, отвечающий исключительно за **главное меню и системные экраны игры** (выбор одиночной/сетевой игры, настройки, экран загрузки, список серверов) — то есть за всё, что происходит ещё до или вне самого игрового процесса. Это позволяет полностью переработать внешний вид и структуру меню под конкретный мод или самостоятельную игру — от косметических изменений до совершенно другой навигации — без единой строчки настроек самого движка.

Кроме полноценного MenuQC, у движка также есть отдельная, гораздо более простая альтернатива для несложных меню — без компиляции кода вообще, а прямо через конфигурационные консольные команды (описана в конце страницы).

---

## Интерфейс настройки

Логика меню компилируется в отдельный файл **`menu.dat`** и подключается движком при старте, если он присутствует в игровой папке. Исходники обычно собираются через отдельный список файлов (например, `menu.src`) встроенным или внешним компилятором QuakeC. Если `menu.dat` отсутствует, движок показывает своё стандартное встроенное меню.

### Точки входа, которые вызывает движок

| Функция | Когда вызывается |
|---|---|
| [`m_init()`](../37-quakec-builtins-reference/00-entry-points.md#m_init) | один раз при первой загрузке логики меню — здесь создаются экраны и структуры данных |
| [`m_shutdown()`](../37-quakec-builtins-reference/00-entry-points.md#m_shutdown) | при выгрузке логики меню |
| [`m_toggle(...)`](../37-quakec-builtins-reference/00-entry-points.md#m_toggle) | при открытии/закрытии меню (например, по клавише Esc) |
| [`m_draw()`](../37-quakec-builtins-reference/00-entry-points.md#m_draw) | каждый кадр, пока меню открыто — здесь описывается вся отрисовка текущего экрана меню теми же функциями рисования, что и в CSQC ([`drawpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawpic), [`drawstring`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawstring)/[`drawcolorcodedstring`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawstring), [`drawfill`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawfill) и т.д.) |
| [`m_drawloading()`](../37-quakec-builtins-reference/00-entry-points.md#m_drawloading) | во время экрана загрузки карты — отдельная точка входа, вызываемая, даже если основное меню закрыто |
| [`Menu_InputEvent(...)`](../37-quakec-builtins-reference/00-entry-points.md#menu_inputevent) | современная точка входа для клавиатуры, мыши и прочего ввода; если она есть, старые [`m_keydown`](../37-quakec-builtins-reference/00-entry-points.md#m_keydown)/[`m_keyup`](../37-quakec-builtins-reference/00-entry-points.md#m_keyup) обычно уже не нужны |
| [`m_keydown(...)`](../37-quakec-builtins-reference/00-entry-points.md#m_keydown) / [`m_keyup(...)`](../37-quakec-builtins-reference/00-entry-points.md#m_keyup) | устаревшая совместимая пара обработчиков нажатия/отпускания клавиш |
| [`m_consolecommand("command")`](../37-quakec-builtins-reference/00-entry-points.md#m_consolecommand) | вызывается для консольных команд, зарегистрированных через [`registercommand(...)`](../37-quakec-builtins-reference/03-entity-world-builtins.md#registercommand) |
| [`m_gethostcachecategory(...)`](../37-quakec-builtins-reference/00-entry-points.md#m_gethostcachecategory) | дополнительный хук для категоризации записей браузера серверов |
| [`Menu_RendererRestarted("description")`](../37-quakec-builtins-reference/00-entry-points.md#menu_rendererrestarted) | уведомление о перезапуске видео/рендера, когда меню должно заново создать свои кэшированные ресурсы |

### Дополнительные встроенные функции, доступные именно логике меню

Помимо стандартного набора функций отрисовки и работы со строками, общего с CSQC, логике меню доступны специфичные именно для системных экранов функции:

- **[`getresolution(...)`](../37-quakec-builtins-reference/13-menuqc-builtins.md#getresolution)** — получение списка поддерживаемых видеорежимов/разрешений экрана — для построения экрана настроек видео.
- **[`getgamedirinfo(...)`](../37-quakec-builtins-reference/11-server-browser-builtins.md#getgamedirinfo)** — получение информации об установленных играх/модах в системе — для построения экрана выбора мода.
- **Работа со списком серверов**: **[`refreshhostcache()`](../37-quakec-builtins-reference/11-server-browser-builtins.md#refreshhostcache)** (запросить обновление списка публичных серверов), **[`gethostcachenumber()`](../37-quakec-builtins-reference/11-server-browser-builtins.md#gethostcachenumber)** (сколько серверов сейчас известно), **[`gethostcachevalue(...)`](../37-quakec-builtins-reference/11-server-browser-builtins.md#gethostcachevalue)** / **[`gethostcachestring(...)`](../37-quakec-builtins-reference/11-server-browser-builtins.md#gethostcachestring)** (числовое/строковое поле конкретного найденного сервера — имя, карта, число игроков и т.п.), **[`resethostcachemasks()`](../37-quakec-builtins-reference/11-server-browser-builtins.md#resethostcachemasks)** / **[`sethostcachemaskstring(...)`](../37-quakec-builtins-reference/11-server-browser-builtins.md#sethostcachemaskstring)** (задать фильтры отображаемых серверов), **[`gethostcacheindexforkey(...)`](../37-quakec-builtins-reference/11-server-browser-builtins.md#gethostcacheindexforkey)** — этот набор функций — основа для полностью кастомного экрана браузера серверов внутри меню (см. [«Браузер серверов и избранное»](../25-server-browser-masters/server-browser-favorites.md)).
- **[`cvar_set(...)`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar_set)**, **[`localsound(...)`](../37-quakec-builtins-reference/05-sound-builtins.md#localsound)**, **[`registercommand(...)`](../37-quakec-builtins-reference/03-entity-world-builtins.md#registercommand)** — доступны так же, как в CSQC, для изменения настроек, проигрывания звуков интерфейса и регистрации собственных консольных команд меню.

---

## Примеры

- Мод заменяет стандартное меню на брендированное — со своим фоном, музыкальным сопровождением и структурой разделов, соответствующей именно его режимам игры.
- Кастомный экран выбора сервера в меню использует [`refreshhostcache`](../37-quakec-builtins-reference/11-server-browser-builtins.md#refreshhostcache)/[`gethostcachevalue`](../37-quakec-builtins-reference/11-server-browser-builtins.md#gethostcachevalue), чтобы показать дополнительную информацию (например, текущий счёт турнира), которой нет в стандартном меню движка.
- Экран настроек видео построен через [`getresolution`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#getresolution), чтобы предложить игроку список реально поддерживаемых его монитором разрешений.

---

## Более простая альтернатива: меню на основе консольных команд

Если полноценный MenuQC — это слишком сложно для задачи, движок поддерживает создание простых интерактивных меню прямо в обычном конфигурационном файле, без компиляции какого-либо кода — набором специальных консольных команд, обычно оформленных в виде псевдонима ([`alias`](../44-cli-commands-reference/05-filesystem-system-commands.md#alias)):

- **[`conmenu <alias_handler>`](../44-cli-commands-reference/02-client-ui-commands.md#conmenu)** — обязательная первая команда, создающая новое меню; указанный алиас будет вызван при определённых событиях (нажатие Esc — событие [`cancel`](../44-cli-commands-reference/04-server-multiplayer-commands.md#cancel), нажатие цифровых клавиш 1–9/0 — их номер, клик по пункту [`menutext`](../44-cli-commands-reference/02-client-ui-commands.md#menutext) — команда этого пункта), при этом текст события каждый раз подставляется в переменную `$option`.
- **[`menuclear`](../44-cli-commands-reference/02-client-ui-commands.md#menuclear)** — закрывает текущее меню.
- **[`menubox <x> <y> <width> <height>`](../44-cli-commands-reference/02-client-ui-commands.md#menubox)** — рисует фоновую рамку под текстом (аналогично классическому окну выхода из игры).
- **[`menutext <x> <y> "text" [command]`](../44-cli-commands-reference/02-client-ui-commands.md#menutext)** / **[`menutextbig <x> <y> "text" [command]`](../44-cli-commands-reference/02-client-ui-commands.md#menutextbig)** — обычная (или крупным фирменным шрифтом движка) строка текста; если указана команда, пункт становится кликабельным.
- **[`menupic <x> <y> "name_image"`](../44-cli-commands-reference/02-client-ui-commands.md#menupic)** — картинка в указанной точке (координата `x`, равная `-`, центрирует картинку по горизонтали).
- **[`menucheck <x> <y> "text" <cvar> [bit_mask]`](../44-cli-commands-reference/02-client-ui-commands.md#menucheck)** — переключатель значения переменной (или отдельного бита в ней, если указана маска).
- **[`menuslider <x> <y> "text" <cvar> <min> <max>`](../44-cli-commands-reference/02-client-ui-commands.md#menuslider)** — ползунок значения переменной в заданном диапазоне.
- **[`menuedit <x> <y> "text" <cvar>`](../44-cli-commands-reference/02-client-ui-commands.md#menuedit)** — поле для ручного ввода текстового значения переменной.
- **[`menubind <x> <y> "text" <command>`](../44-cli-commands-reference/02-client-ui-commands.md#menubind)** — пункт для назначения клавиши на указанную команду (единственный тип пункта, который не привязан к переменной).

Такие меню удобно строить с использованием многострочных псевдонимов и условной команды `if`/`else`, тоже поддерживающей вложенные блоки в фигурных скобках — это позволяет писать логику меню сразу в конфигурационном файле, без отдельной компиляции.

---

## Смежные страницы

- [Клиентская логика и интерфейс (CSQC)](./client-side-quakec-csqc.md)
- [Серверная игровая логика (SSQC)](./server-side-quakec-ssqc.md)
- [Браузер серверов и избранное](../25-server-browser-masters/server-browser-favorites.md)
- [Псевдонимы команд и алиасы](../19-config-console/aliases-macros.md)

> [⬅ Предыдущая страница](client-side-quakec-csqc.md) | [Следующая страница ➡](../17-alternative-scripting-vms/server-side-lua.md)

> [⬅ Вернуться к оглавлению вики](../README.md)