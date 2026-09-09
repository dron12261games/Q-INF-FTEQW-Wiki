# Быстрый старт: первые команды

> [⬅ Предыдущая страница](installation-and-first-launch.md) | [Следующая страница ➡](how-to-use-this-wiki.md)

> [⬅ Вернуться к оглавлению вики](../README.md)

Эта страница — короткая практическая шпаргалка для самого первого знакомства с движком. Подробное описание каждой темы вынесено в соответствующие разделы основной вики.

## Открываем консоль

Консоль открывается клавишей `` ` `` (tilde, обычно под Esc) или сочетанием `Shift+Esc`. Это текстовое поле ввода, куда можно вводить команды и переменные движка.

Две полезные команды для самостоятельного изучения того, что вообще умеет движок:

- [`cvarlist`](../44-cli-commands-reference/05-filesystem-system-commands.md#cvarlist) — выводит список вообще всех переменных движка;
- [`cmdlist`](../44-cli-commands-reference/05-filesystem-system-commands.md#cmdlist) — выводит список вообще всех доступных команд.

---

## Базовые команды

| Команда | Что делает |
|---|---|
| [`bind <key> <command>`](../44-cli-commands-reference/02-client-ui-commands.md#bind) | Привязывает команду к клавише, например `bind F12 quit` |
| [`unbindall`](../44-cli-commands-reference/02-client-ui-commands.md#unbindall) | Снимает вообще все привязки клавиш — полезно перед тем, как настроить управление с нуля |
| [`map <mapname>`](../44-cli-commands-reference/04-server-multiplayer-commands.md#map) | Запускает игру на указанной карте, например `map dm4` |
| [`changelevel <mapname>`](../44-cli-commands-reference/04-server-multiplayer-commands.md#changelevel) | Переходит на другую карту без полного пересоздания сервера (сохраняя часть состояния игры) |
| [`connect <address>`](../44-cli-commands-reference/02-client-ui-commands.md#connect) | Подключается к серверу по IP-адресу или доменному имени |
| [`reconnect`](../44-cli-commands-reference/02-client-ui-commands.md#reconnect) | Заново подключается к серверу, к которому вы были подключены последним |
| [`disconnect`](../44-cli-commands-reference/02-client-ui-commands.md#disconnect) | Закрывает текущее сетевое или локальное игровое подключение |
| [`cfg_save`](../44-cli-commands-reference/05-filesystem-system-commands.md#cfg_save) | Сохраняет все ещё не сохранённые изменения настроек на диск |
| [`echo <text>`](../44-cli-commands-reference/05-filesystem-system-commands.md#echo) | Печатает произвольный текст в консоль — полезно для отладки алиасов и конфигов |
| [`exec <file.cfg>`](../44-cli-commands-reference/05-filesystem-system-commands.md#exec) | Построчно исполняет команды из указанного текстового конфигурационного файла |
| [`alias <name> "<commands>"`](../44-cli-commands-reference/05-filesystem-system-commands.md#alias) | Создаёт собственную команду-макрос, объединяющую несколько других команд |
| [`toggle <cvar>`](../44-cli-commands-reference/05-filesystem-system-commands.md#toggle) | Переключает булеву (0/1) переменную в противоположное значение — удобно вешать на одну клавишу вместо ручного набора значений |
| [`wait`](../44-cli-commands-reference/05-filesystem-system-commands.md#wait) | Приостанавливает выполнение следующей команды в той же строке/алиасе на один кадр — нужно для последовательностей нажатий |
| [`find <substring>`](../44-cli-commands-reference/05-filesystem-system-commands.md#find) / [`apropos <substring>`](../44-cli-commands-reference/05-filesystem-system-commands.md#apropos) | Ищет команды и переменные, в имени или описании которых встречается указанное слово |
| [`quit`](../44-cli-commands-reference/02-client-ui-commands.md#quit) | Закрывает игру |

Подробнее о привязке клавиш, алиасах и командах — в разделе [«Конфигурационные файлы и консоль»](../README.md#конфигурационные-файлы-и-консоль).

---

## Переменные движка (cvar): временное и постоянное изменение

В отличие от многих других движков серии Quake, система переменных FTEQW ближе к более поздним движкам: значения можно менять как временно, так и «навсегда» (с сохранением в конфиг):

```
set sv_port 26000     ! change value only for the current engine session
seta sv_port 26000     ! change value and mark it for saving to the config
```

Если ввести в консоли просто `varname value`, не указывая [`set`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#set)/[`seta`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#seta), движок сам поймёт, что вы обращаетесь к переменной, и применит [`set`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#set) — то есть изменение будет только временным.

---

## Параметры командной строки

Переменные и команды можно передать движку сразу при запуске, через командную строку, с символом `+` перед каждой:

```
fteqw +set sv_port 26000 +map dm4
```

Другие полезные параметры запуска:

| Параметр | Назначение |
|---|---|
| [`-nohome`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#nohome) | не пытаться писать конфиги/сохранения/скриншоты в домашнюю папку пользователя |
| [`-basedir <path>`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#basedir) | указать корневую папку игровых данных |
| [`-basegame <folder>`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#basegame) | указать папку основной игры |
| [`-game <folder>`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#game) | указать папку мода поверх основной игры |
| [`-window`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#window) | запустить рендер в оконном, а не полноэкранном режиме |
| [`-manifest <file.fmf>`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#manifest) | загрузить файл-манифест мода/игры |
| [`-dedicated`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#dedicated) | запустить движок в режиме выделенного сервера, без изображения |

Подробнее о структуре папок и о том, как переключаться между играми и модами — в статье [«Структура папок игры»](./directory-structure-basics.md).

---

## Быстрый сетевой старт

Чтобы **найти и подключиться** к чужому серверу, откройте в игре встроенное меню сетевой игры (multiplayer) — там есть браузер серверов, который сам находит доступные матчи по разным поддерживаемым протоколам (подробнее — в разделе [«Поиск серверов и мастер-серверы»](../README.md#поиск-серверов-автозагрузка-контента-и-прямые-соединения)).

Чтобы **поднять свой сервер**, есть два основных варианта:
1. Запустить listen-сервер (сервер и клиент одновременно на одном компьютере) и вручную настроить проброс портов в домашнем роутере.
2. Воспользоваться сторонним сервисом `frag-net.com`, который помогает игрокам находить ваш сервер, даже если вы не настраивали проброс портов, — для этого при создании сервера достаточно выставить настройку публичности сервера в режим «Holepunch».

Пример запуска выделенного сервера сразу из командной строки, с автоматическим анонсом через `frag-net.com`:

```
fteqw +set sv_public 2 +set sv_playerslots 8 +map dm4
```

---

## Пример типичной первой сессии в консоли

Ниже — законченная последовательность команд, которую новичок может набрать подряд сразу после первого запуска, чтобы на практике почувствовать все понятия из этой шпаргалки:

```
cvarlist name
find crosshair
seta crosshair 3
bind mouse1 +attack
bind space +jump
map dm4
cfg_save
```

Построчный разбор: первая команда выводит список всех переменных, в имени которых встречается [`name`](../38-cvars-reference/07-system-misc-cvars.md#name) (так можно быстро найти [`name`](../38-cvars-reference/07-system-misc-cvars.md#name) — переменную с именем игрока). Вторая ищет все команды/переменные, связанные с прицелом. Третья постоянно (с сохранением в конфиг) включает третий вариант прицела. Четвёртая и пятая привязывают стрельбу к левой кнопке мыши и прыжок к пробелу. Шестая запускает одиночную игру на карте `dm4`. Последняя команда принудительно сохраняет всё, что было изменено, чтобы не потерять настройки при следующем запуске, даже если движок закроется аварийно.

---

## Что дальше

На этом вводная часть вики завершается. Следующий шаг — [Как пользоваться этой вики](./how-to-use-this-wiki.md), после чего можно переходить к тематическим разделам, начиная с [общего оглавления](../README.md).

> [⬅ Предыдущая страница](installation-and-first-launch.md) | [Следующая страница ➡](how-to-use-this-wiki.md)

> [⬅ Вернуться к оглавлению вики](../README.md)