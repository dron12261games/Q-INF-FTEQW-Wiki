# Команды консоли: сервер и мультиплеер

> [⬅ Предыдущая страница](03-rendering-sound-commands.md) | [Следующая страница ➡](05-filesystem-system-commands.md)

> [⬅ Вернуться к оглавлению вики](../README.md)
> [Индекс справочника команд и параметров](../README.md#команды-и-параметры-командной-строки)

В этой статье собраны консольные команды, которые движок регистрирует в серверных подсистемах FTEQW. Список кратко помечает сборочные ограничения там, где они есть.

## Операторские и чит-команды

### quit
`quit`

Завершает работу движка из серверной консоли. В `MASTERONLY`-сборке используется отдельный обработчик с тем же именем, но смысл команды тот же. 

---

### say
`say <message>`

Отправляет сообщение от `{console}` всем подключённым игрокам сервера. Это именно серверная операторская команда, а не клиентский чат. 

---

### sayone
`sayone <player|userid> <message>`

Отправляет адресное сообщение от серверной консоли всем клиентам, совпавшим с первым аргументом. Использует тот же обработчик, что и `tell`. 

---

### tell
`tell <player|userid> <message>`

Алиас `sayone`: шлёт приватное сообщение выбранному игроку из консоли сервера. 

---

### god
`god`

Включает/выключает режим неуязвимости для текущего серверного игрока/оператора. Требует чит-доступа (`sv_cheats 1` с перезапуском карты). 

---

### give
`give <player> <item> <value>`

Чит-команда для выдачи значений/предметов выбранному игроку; обработчик читает цель, тип и числовое значение из аргументов. Регистрируется только при `QUAKESTATS`. 

---

### noclip
`noclip`

Включает/выключает проход сквозь геометрию для текущего серверного игрока/оператора. Требует чит-доступа. 

---

### download
`download <url> [local_name]`

Запускает встроенную загрузку файла по HTTP/HTTPS/FTP и сохраняет его в игровой ФС; при отсутствии второго аргумента имя берётся из URL. Практически полезна только в сборках с `WEBCLIENT`. 

---

### sv_impulse
`sv_impulse <number>`

Создаёт временного серверного клиента, прогоняет его через `ClientConnect`/`PutClientInServer` и вызывает указанный импульс в SSQC. Полезно для отладки импульсов без реального игрока. 

---

## Администрирование и санкции

### fraglogfile
`fraglogfile`

Переключает запись фрагов в `frag_N.log`; повторный вызов закрывает текущий файл. Имя журнала подбирается автоматически по первому свободному номеру. 

---

### snap
`snap <userid>`

Запрашивает удалённый скриншот у конкретного клиента по user id. 

---

### snapall
`snapall`

Отправляет запрос удалённого скриншота всем активным не-наблюдателям. 

---

### kick
`kick <player|ip> [reason]`

Удаляет игрока с сервера по имени, user id или IP. Встроенная строка описания советует использовать имя или IP цели. 

---

### clientkick
`clientkick <slot>`

Кикает клиента по номеру слота, что используется для q3-совместимого меню управления ботами/игроками. 

---

### renameclient
`renameclient <player|userid> <new_name>`

Принудительно меняет `name` в userinfo выбранного клиента и рассылает обновление всем остальным. 

---

### mute
`mute <player|userid|ip>`

Переключает штраф `BAN_MUTE`: блокирует чат и голос выбранного игрока. 

---

### stealthmute
`stealthmute <player|userid|ip>`

Тихий вариант `mute`: игрок продолжает видеть, будто его сообщения уходят, но сервер их не ретранслирует. 

---

### cuff
`cuff <player|userid|ip>`

Переключает штраф, запрещающий игроку атаковать. 

---

### cripple
`cripple <player|userid|ip>`

Переключает штраф, блокирующий движение игрока. 

---

### ban
`ban <address/mask> [flags] [+time|unix_time] [reason]`

Добавляет IP-бан или иной penalty-запрет; без `flags` в этой команде по умолчанию используется `BAN_BAN`. Одновременно кикает цель при использовании по игроку. 

---

### banname
`banname <address/mask> [flags] [+time|unix_time] [reason]`

Устаревший алиас `ban` для совместимости со старым серверным администрированием. 

---

### banlist
`banlist`

Показывает только записи, в которых установлен собственно флаг IP-бана, с остатком времени и причиной. 

---

### unban
`unban <address/mask|all> [flags]`

Алиас `removeip`: снимает ban/penalty-флаги у указанного адреса либо очищает весь список. 

---

### addip
`addip <address/mask> [flags] [+time|unix_time] [reason]`

Низкоуровневая команда добавления записи в penalty-список. Если флаги не заданы, тип наказания зависит от `filterban`. 

---

### removeip
`removeip <address/mask|all> [flags]`

Снимает один или несколько penalty-флагов у указанного адреса; без `flags` удаляет все флаги записи. 

---

### listip
`listip`

Печатает весь список penalties, а не только баны: адрес, набор флагов и оставшееся время. 

---

### writeip
`writeip`

Сохраняет текущий список penalties в `listip.cfg` как набор команд `addip ...`. 

---

### floodprot
`floodprot [messages interval_sec silence_sec]`

Без аргументов показывает текущие параметры flood protection; с тремя аргументами меняет лимиты сообщений, интервал и время заглушения. 

---

### stuffcmd
`stuffcmd <player> <client_command>`

Отправляет выбранному клиенту whitelisted-консольную команду, эмулируя серверный `stuffcmd`. Обработчик специально режет опасные случаи вроде `;`, переводов строки и небезопасных подкоманд. 

---

## Информация сервера и конфигурация

### status
`status`

Показывает текущее состояние сервера и клиентов. В `MASTERONLY`-сборке это имя перерегистрируется на обработчик статуса мастер-сервера, поэтому вывод другой, но команда остаётся той же. 

---

### serverinfo
`serverinfo [key value...]`

Без аргументов печатает public `serverinfo`; с парой `key/value` меняет запись и, если ключ соответствует cvar, синхронизирует и её. 

---

### serverinfoblob
`serverinfoblob <key> <file>`

Загружает содержимое файла и сохраняет его как binary/blob-значение в `serverinfo`. Используется тем же обработчиком, что и `serverinfo`, но читает второй аргумент как путь к файлу. 

---

### localinfo
`localinfo [key value]`

Без аргументов печатает `svs.localinfo`; с парой значений меняет локальный ключ и вызывает `PR_LocalInfoChanged`. `*`-ключи запрещены, а `localinfo * ""` очищает таблицу. 

---

### user
`user <userid|name>`

Печатает расширенную информацию о клиенте: userinfo, сетевые расширения, счётчики и другую диагностическую сводку. 

---

### sv
`sv <command_mod...>`

Передаёт остаток строки в игровой код (`PR_ConsoleCmd`, q1qvm/q2/q3 backend'ы). Это основной шлюз для мод-специфичных серверных консольных команд. 

---

### mod
`mod <command_mod...>`

Алиас `sv` с тем же поведением: проксирует строку в серверный игровой код. 

---

### precaches
`precaches`

Показывает текущие серверные precache-списки. 

---

### heartbeat
`heartbeat`

Форсирует повторный резолв/обновление адресов мастер-сервера, чтобы сервер снова отправил heartbeat. 

---

### gamedir
`gamedir [gamedir ...]`

Без аргументов печатает текущий gamedir; с аргументами реально переключает поисковые пути сервера на новый gamedir/набор gamedir'ов. 

---

### sv_gamedir
`sv_gamedir [gamedir]`

Меняет только публикуемое клиентам значение `*gamedir`, не трогая реальные поисковые пути сервера. Без аргументов печатает текущее значение. 

---

### sv_settimer
`sv_settimer <count> <interval> <command>`

Планирует повторный запуск серверной команды: `count` раз, через `interval` секунд; `-1` означает бесконечный повтор. Вызов только с `0` снимает активный таймер. 

---

### sv_meminfo
`sv_meminfo`

Печатает память моделей и приблизительную память/буферы по активным клиентам, а также размер SSQC string table. 

---

### pin_save
`pin_save`

Сохраняет pinned-сообщения сервера на диск. 

---

### pin_reload
`pin_reload`

Перечитывает pinned-сообщения с диска. 

---

### pin_delete
`pin_delete`

Удаляет самое старое pinned-сообщение. 

---

### pin_add
`pin_add <from_whom> <message>`

Создаёт новое pinned-сообщение, используя первые два аргумента как автора и текст. 

---

## Карты, сохранения и кластеры

### ssv
`ssv [id [command...]]`

В кластере без аргументов показывает список активных subserver'ов; с одним `id` в клиентской сборке открывает их подконсоль; с `id` и командой пересылает команду выбранному subserver'у. Доступна только при `SUBSERVERS`. 

---

### ssv_all
`ssv_all <command...>`

Шлёт одну и ту же команду всем subserver'ам кластера. Доступна только при `SUBSERVERS`. 

---

### mapcluster
`mapcluster [map] [playerslots]`

Переводит сервер в режим cluster gateway и поднимает отдельные процессы для карт; первый аргумент задаёт стартовую карту для новых клиентов. Доступна только при `SUBSERVERS`. 

---

### killserver
`killserver`

Мгновенно останавливает текущий сервер и снимает спавн сервера без запуска новой карты. 

---

### map
`map <map> [startspot]`

Запускает новую игру на указанной карте. В разных gametype'ах сбрасывает разный объём состояния; при отсутствии аргументов выводит список доступных карт. 

---

### mapedit
`mapedit <map>`

Загружает карту без активного gamecode, то есть в режиме редактирования/диагностики. 

---

### spmap
`spmap <map>`

Q3-совместимая одиночная загрузка карты с очисткой spawn parameters. Регистрируется только при `Q3SERVER`. 

---

### spdevmap
`spdevmap <map>`

Q3-совместимая одиночная загрузка карты в developer/cheat-режиме. Регистрируется только при `Q3SERVER`. 

---

### devmap
`devmap <map>`

Загружает карту с принудительным чит-режимом (`sv_cheats 1`). 

---

### gamemap
`gamemap <map> [startspot]`

Вариант смены карты с Quake II-семантикой сохранения состояния между уровнями; обработчик отдельно помечает его для save-to-slot-0 логики. 

---

### changelevel
`changelevel <map> [startspot]`

Продолжает текущую игру на другой карте и, если указан `startspot`, позволяет вернуться к предыдущей структуре progress/hub-состояния. 

---

### map_restart
`map_restart [restore|initial|delay]`

Перезапускает текущую карту и сервер; для некоторых игр понимает специальные аргументы вроде `restore`/`initial`. 

---

### listmaps
`listmaps`

Печатает список карт, найденных в `maps/` и вложенных подпапках. 

---

### maplist
`maplist`

Алиас `listmaps`: показывает установленные карты. 

---

### maps
`maps`

Ещё один алиас списка карт. 

---

### savegame_legacy
`savegame_legacy <slot|name>`

Сохраняет игру в vanilla Quake-совместимом формате с потерей всего, что он не умеет хранить. Команда есть только при `SAVEDGAMES` и отсутствии `QUAKETC`. 

---

### savegame
`savegame <slot|name>`

Сохраняет игру в штатный формат FTEQW. Доступна только при `SAVEDGAMES`. 

---

### loadgame
`loadgame <slot|name>`

Загружает сохранённую игру. Доступна только при `SAVEDGAMES`. 

---

### save
`save <slot|name>`

Короткий алиас `savegame`. Доступна только при `SAVEDGAMES`. 

---

### load
`load <slot|name>`

Короткий алиас `loadgame`. Доступна только при `SAVEDGAMES`. 

---

### unsavegame
`unsavegame <slot|name>`

Удаляет сохранение с диска. Доступна только при `SAVEDGAMES`. 

---

## Демозаписи и MVD

### playmvd
`playmvd <demoname>`

Открывает и начинает проигрывать серверную multi-view demo (`.mvd` или путь в `demos/`). Команда регистрируется только при `SERVER_DEMO_PLAYBACK`. 

---

### mvdplay
`mvdplay <demoname>`

Алиас `playmvd` с тем же multi-view playback. Доступна только при `SERVER_DEMO_PLAYBACK`. 

---

### svplay
`svplay <demo>`

Регистрируется рядом с MVD-playback как отдельная команда проигрывания server-side demo. Команда доступна только при `SERVER_DEMO_PLAYBACK`. 

---

### svrecord
`svrecord <demo>`

Регистрируется в server-demo подсистеме как команда записи server-side demo. Команда доступна только при `SERVER_DEMO_PLAYBACK`. 

---

### record
`record <demoname>`

Начинает MVD-запись в `sv_demoDir`, очищая имя и автоматически подставляя `.mvd`/`.mvd.gz`. Это dedicated-only короткое имя, доступное только при `MVD_RECORDING` и `SERVERONLY`. 

---

### stop
`stop`

Останавливает текущую MVD-запись и закрывает файл. Короткое имя регистрируется только при `MVD_RECORDING` и `SERVERONLY`. 

---

### cancel
`cancel`

Останавливает текущую MVD-запись и удаляет получившийся demo-файл. Доступна только при `MVD_RECORDING`. 

---

### easyrecord
`easyrecord [demoname]`

Автоматически создаёт удобное имя MVD по карте/командам/игрокам или использует переданный аргумент. Доступна только при `MVD_RECORDING`. 

---

### demolist
`demolist [filter ...]`

Показывает демки из `sv_demoDir`, при необходимости фильтруя список по подстрокам из аргументов, и печатает размер каталога. Доступна только при `MVD_RECORDING`. 

---

### rmdemo
`rmdemo <demoname|*|*token>`

Удаляет одну demo, все demo или все demo с подстрокой `token`; если удаляется активная запись, она сначала останавливается. Доступна только при `MVD_RECORDING`. 

---

### rmdemonum
`rmdemonum <#>`

Удаляет demo по номеру из списка `demolist`. Доступна только при `MVD_RECORDING`. 

---

### sv_demorecord
`sv_demorecord <demoname>`

Полное server-side имя для начала MVD-записи; делает то же, что `record`. Доступна только при `MVD_RECORDING`. 

---

### sv_demostop
`sv_demostop`

Полное server-side имя для остановки MVD-записи. Доступна только при `MVD_RECORDING`. 

---

### sv_democancel
`sv_democancel`

Полное server-side имя для отмены и удаления текущей MVD-записи. Доступна только при `MVD_RECORDING`. 

---

### sv_demoeasyrecord
`sv_demoeasyrecord [demoname]`

Полное server-side имя для `easyrecord`. Доступна только при `MVD_RECORDING`. 

---

### sv_demolist
`sv_demolist [filter ...]`

Полное server-side имя для `demolist`. Доступна только при `MVD_RECORDING`. 

---

### sv_demoremove
`sv_demoremove <demoname|*|*token>`

Полное server-side имя для `rmdemo`. Доступна только при `MVD_RECORDING`. 

---

### sv_demonumremove
`sv_demonumremove <#>`

Полное server-side имя для `rmdemonum`. Доступна только при `MVD_RECORDING`. 

---

### mvdrecord
`mvdrecord <demoname>`

Старое FTE-имя начала MVD-записи, оставленное для совместимости. Доступна только при `MVD_RECORDING`. 

---

### mvdstop
`mvdstop`

Старое FTE-имя остановки MVD-записи. Доступна только при `MVD_RECORDING`. 

---

### mvdcancel
`mvdcancel`

Старое FTE-имя отмены и удаления MVD-записи. Доступна только при `MVD_RECORDING`. 

---

### mvdlist
`mvdlist [filter ...]`

Старое FTE-имя для списка MVD-файлов. Доступна только при `MVD_RECORDING`. 

---

### mvdplaynum
`mvdplaynum <#>`

Берёт demo по номеру из списка и превращает её в `mvdplay <name>`. Доступна только при `MVD_RECORDING` и `SERVER_DEMO_PLAYBACK`. 

---

### sv_demoinfoadd
`sv_demoinfoadd <demonum|*|**> <string|file>`

Добавляет текстовый `.txt`-sidecar к demo: `*` означает текущую запись, `**` — загрузить содержимое из файла. Доступна только при `MVD_RECORDING`. 

---

### sv_demoinforemove
`sv_demoinforemove <demonum|*>`

Удаляет текстовый `.txt`-sidecar у demo по номеру или у текущей записи. Доступна только при `MVD_RECORDING`. 

---

### sv_demoinfo
`sv_demoinfo <demonum|*>`

Печатает содержимое текстового `.txt`-sidecar, связанного с demo. Доступна только при `MVD_RECORDING`. 

---

### qtvreverse
`qtvreverse <ip[:port]>`

Открывает reverse-QTV TCP-соединение к удалённому адресу и начинает hand-shake `QTV REVERSE`. Доступна только при `MVD_RECORDING`. 

---

## SSQC, моды и отладка

### breakpoint
`breakpoint <file> <line>`

Переключает breakpoint в серверном QC по имени файла и номеру строки. Если QC ещё не запущен, команда просит сначала стартовать сервер. 

---

### watchpoint
`watchpoint [variable]`

Ставит или снимает watchpoint на указанную переменную/выражение в SSQC. При отсутствии осмысленного аргумента фактически снимает текущую точку наблюдения. 

---

### watchpoint_ssqc
`watchpoint_ssqc [variable]`

Алиас `watchpoint` для явного SSQC-контекста. 

---

### decompile
`decompile [progs.dat]`

Декомпилирует `qwprogs.dat` или указанный файл progs через встроенный QC backend. 

---

### compile
`compile [srcfile|compiler_arguments...]`

Запускает встроенный компилятор QC: без аргументов ищет `progs.src`, с одним аргументом подменяет src-файл, а с несколькими — передаёт их компилятору как есть. 

---

### applycompile
`applycompile`

Сохраняет текущее состояние сущностей, переконфигурирует серверный progs и заново загружает состояние в новый SSQC. Это горячая подмена компилированного server QC на работающем сервере. 

---

### coredump_ssqc
`coredump_ssqc`

Сбрасывает состояние SSQC-сущностей в `ssqccore.txt`. Работает только при запущенном server progs. 

---

### poke_ssqc
`poke_ssqc <expression...>`

Вычисляет отладочную строку в SSQC и печатает результат. Требует `sv_cheats 1`; в cluster-режиме может быть автоматически перенаправлена на subserver. 

---

### profile_ssqc
`profile_ssqc [1]`

Печатает накопленный профиль времени по QC-функциям; аргумент `1` запрещает очищать счётчики после вывода. Если профилирование было выключено, команда сначала включает его. 

---

### extensionlist_ssqc
`extensionlist_ssqc [flags]`

Показывает доступные/неактивные SSQC extensions и builtins; набор битов в `flags` управляет тем, что именно выводить. Без аргумента показывает активные и неактивные расширения. 

---

### pr_dumpplatform
`pr_dumpplatform [options]`

Генерирует платформенную сводку/символьный дамп для QC-целей (`-F`, `-T`, `-O` и т. п.). В dedicated `SERVERONLY` или build'ах без описаний QC команда выводит отказ и не работает. 

---

### sv_lightstyle
`sv_lightstyle [style] [pattern [r g b]]`

Чит-команда для просмотра или подмены lightstyle на сервере; с одним аргументом печатает стиль, с дополнительными — меняет его. 

---

## SQL, рейтинг и мастер-сервер

### sqlstatus
`sqlstatus`

Печатает доступность MySQL/SQLite backend'ов, список SQL-соединений, очереди запросов и pending results. Доступна только при `SQL`. 

---

### sqlkill
`sqlkill <serverid>`

Останавливает конкретное SQL-соединение по его номеру из `sqlstatus`. Доступна только при `SQL`. 

---

### sqlkillall
`sqlkillall`

Гасит все SQL-соединения сервера. Доступна только при `SQL`. 

---

### ranklist
`ranklist`

Не печатает рейтинг в консоль, а выгружает полный список игроков в `list.txt` с именами, убийствами и смертями. Доступна только при `SVRANKING`. 

---

### ranktopten
`ranktopten`

Печатает в консоль первую десятку рейтинга. Доступна только при `SVRANKING`. 

---

### rankfind
`rankfind <mask>`

Ищет игроков в рейтинговой базе по wildcard-маске имени и печатает совпавшие id/имена. Доступна только при `SVRANKING`. 

---

### rankremove
`rankremove <number_in_ranklist>`

Удаляет запись рейтинга по порядковому номеру из списка лидеров, а не по внутреннему id. Доступна только при `SVRANKING`. 

---

### rankrefresh
`rankrefresh`

Сбрасывает накопленные on-server kills/deaths/time в рейтинговый файл для всех активных клиентов и переоткрывает базу рейтинга. Доступна только при `SVRANKING`. 

---

### rankrconlevel
`rankrconlevel <number_in_ranklist> <level>`

Меняет trust/rcon-уровень рангового пользователя по его позиции в таблице. Нельзя поднять пользователя до уровня, равного уровню оператора. 

---

### rankadd
`rankadd <name> [pwd] [rights]`

Добавляет пользователя в рейтинговую базу, опционально задавая числовой пароль и trust-level. Доступна только при `SVRANKING`. 

---

### adduser
`adduser <name> [pwd] [rights]`

Алиас `rankadd` с тем же поведением. Доступна только при `SVRANKING`. 

---

### setpass
`setpass <name> <newpass>`

Меняет числовой пароль существующего рейтингового пользователя. Доступна только при `SVRANKING`. 

---

### gamealias
`gamealias <game> <alias>`

Добавляет альтернативное protocol/game-имя в мастер-серверную запись указанной игры. Доступна только при `SV_MASTER`. 

---

### gamelevelshotsurl
`gamelevelshotsurl <game> <url>`

Назначает базовый URL, из которого браузеры смогут строить адреса изображений карт для данной игры. Доступна только при `SV_MASTER`. 

---

## Сеть, маршрутизация и платформенные команды

### openroute
`openroute <ip[:port]>`

Отправляет out-of-band `hello` потенциальному клиенту, чтобы пробить маршрут/туннель до нового адреса. 

---

### route_visualise
`route_visualise <x> <y> <z>`

Запускает асинхронный расчёт и визуализацию маршрута от текущей позиции камеры к заданной точке. Команда существует только при `HAVE_CLIENT` и `CSQC_DAT`. 

---

### route_reload
`route_reload`

Сбрасывает кэш маршрутов/waynet для доступных миров и заставляет движок построить их заново при следующем запросе. 

---

### hide
`hide`

Прячет окно серверной консоли Windows. Команда привязана к Win32-платформе. 

---

## Устаревшая совместимость

### check_maps
`check_maps`

Помечена как устаревшая ktpro-специфичная команда; встроенное описание рекомендует использовать `search_begin`. Регистрируется только при `HAVE_LEGACY && HAVE_SERVER`. 

---

### sys_select_timeout
`sys_select_timeout`

Оставлена как заглушка совместимости: современный сервер сам троттлит цикл по tickrate. Регистрируется только при `HAVE_LEGACY && HAVE_SERVER`. 

---

### sv_downloadchunksperframe
`sv_downloadchunksperframe`

Совместимый, но объявленный flawed/избыточным переключатель старой логики загрузок; современный код опирается на `drate/rate` клиента. Регистрируется только при `HAVE_LEGACY && HAVE_SERVER`. 

---

### sv_speedcheck
`sv_speedcheck`

Устаревшая совместимая заглушка: проверка speedhack описана как заменённая более корректным учётом `movetime`. Регистрируется только при `HAVE_LEGACY && HAVE_SERVER`. 

---

### sv_enableprofile
`sv_enableprofile`

Совместимая debug-заглушка, помеченная в описании как не реализованная. Регистрируется только при `HAVE_LEGACY && HAVE_SERVER`. 

---

### sv_progsname
`sv_progsname`

Совместимый псевдоним, для которого описание предлагает использовать `sv_progs`. Регистрируется только при `HAVE_LEGACY && HAVE_SERVER`. 

---

### download_map_url
`download_map_url`

Устаревшая заглушка совместимости: встроенное описание считает её избыточной по сравнению с обычной загрузкой карт. Регистрируется только при `HAVE_LEGACY && HAVE_SERVER`. 

---

### sv_progtype
`sv_progtype`

Совместимая заглушка с рекомендацией использовать `sv_progs` вместо блокировки `.dll` через старый флаг. Регистрируется только при `HAVE_LEGACY && HAVE_SERVER`. 

---

## Недоступные и платформенно-ограниченные команды

Команды ниже не регистрируются в сборке по умолчанию.

### svtestprogs
`svtestprogs`

Находится в закомментированном блоке `/* #ifdef _DEBUG ... */`, поэтому фактически недоступна даже в debug-сборке, пока блок не раскомментируют. 

---

### reallyevilhack
`reallyevilhack`

Регистрация закомментирована строкой `// Cmd_AddCommand(...)`, поэтому команда намеренно отключена и не попадает в рабочую таблицу команд. 

> [⬅ Предыдущая страница](03-rendering-sound-commands.md) | [Следующая страница ➡](05-filesystem-system-commands.md)

> [⬅ Вернуться к оглавлению вики](../README.md)