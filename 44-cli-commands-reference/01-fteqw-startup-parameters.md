# Параметры командной строки FTEQW (полный справочник)

> [⬅ Предыдущая страница](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md) | [Следующая страница ➡](02-client-ui-commands.md)

> [⬅ Вернуться к оглавлению вики](../README.md)
> [Индекс справочника команд и параметров](../README.md#команды-и-параметры-командной-строки)

Это исчерпывающий справочник по параметрам командной строки движка FTEQW. Параметры сгруппированы по подсистемам. Вводное объяснение синтаксиса и практических примеров — в статье [«Параметры запуска игры»](../19-config-console/startup-parameters.md).

## Файловая система, манифесты и игровые данные

### basedir
`-basedir <path>`

Переопределяет корневую папку игровых данных и фиксирует базовый каталог для автоопределения манифеста.

---

### basepack
`-basepack <archive> [prefix]`

Добавляет внешний pack/APK в базовый поиск данных; флаг можно повторять, а второй аргумент задаёт префикс внутри архива.

---

### basegame
`-basegame <directory>`

Дописывает `basegame` в активный манифест; флаг можно повторять несколько раз.

---

### game
`-game <directory>`

Добавляет или заменяет активный `gamedir` в манифесте; флаг можно повторять, а его отсутствие влияет на выбор `progs.dat`/`qwprogs.dat`.

---

### manifest
`-manifest <file.fmf>`

Загружает указанный манифест вместо чистого автоопределения и одновременно отключает клиентский диалог ручного поиска каталога.

---

### homedir
`-homedir <path>`

Явно задаёт домашний каталог FTEQW для пользовательских файлов и включает его использование.

---

### usehome
`-usehome`

Принудительно включает домашний каталог, если он определим.

---

### nohome
`-nohome`

Полностью отключает домашний каталог FTEQW.

---

### readonly
`-readonly`

Переводит файловую систему в режим только для чтения.

---

### allowfileuri
`-allowfileuri`

Разрешает создание системного search path для `file:` URI и прямого доступа к локальным путям ОС.

---

### allowfileurl
`-allowfileurl`

Синоним `-allowfileuri` с тем же поведением.

---

### unsafefopen
`-unsafefopen`

Ослабляет префикс `data/` для QuakeC-файловых операций, если путь не считается песочницей-обязательным.

---

## Ранние настройки, конфиги и локализация

### safe
`-safe`

Включает безопасный режим: при разборе argv движок автоматически дописывает `-stdvid -nolan -nosound -nocdaudio -nojoy -nomouse -nohome -window`; часть добавляемых флагов сейчас уже историческая и отдельно отмечена ниже.

---

### set
`-set <cvar> <value>`

Поздний аналог консольной команды `set`, исполняемый через буфер команд на этапе стартовых конфигов.

---

### seta
`-seta <cvar> <value>`

Поздний аналог `seta`; команда попадает в командный буфер при стартовой загрузке.

---

### exec
`-exec <file.cfg>`

Добавляет раннее выполнение указанного конфига как консольной команды `exec`.

---

### watch
`-watch <cvar>`

Помечает cvar как watched для отслеживания изменений после регистрации переменных.

---

### lang
`-lang <locale>`

Переопределяет язык интерфейса/переводов поверх переменных окружения и системной локали.

---

### translatetoblank
`-translatetoblank`

Разрешает загружать пустые строки перевода из PO/MO-файлов, не отбрасывая их как «пустые».

---

### noworker
`-noworker`

Отключает фоновые worker threads, фиксируя `worker_count` в 0.

---

### noworkers
`-noworkers`

Полный синоним `-noworker`.

---

### nodaz
`-nodaz`

Отключает SSE-режимы FTZ/DAZ и тем самым возвращает обработку денормализованных float, если моды на них полагаются.

---

## Видео, рендер и оконная система

### window
`-window`

Стартует в оконном режиме, сбрасывая `vid_fullscreen` в 0; также автоматически подставляется `-safe`.

---

### startwindowed
`-startwindowed`

Ещё один стартовый способ принудить оконный режим.

---

### fullscreen
`-fullscreen`

Принудительно включает полноэкранный режим (`vid_fullscreen 1`).

---

### width
`-width <pixels>`

Задаёт стартовую ширину видеорежима; если высота не задана отдельно, движок вычисляет её как 4:3.

---

### height
`-height <pixels>`

Задаёт стартовую высоту видеорежима.

---

### conwidth
`-conwidth <pixels>`

Задаёт ширину консольного/софтварного framebuffer; без `-conheight` высота выводится как 4:3.

---

### conheight
`-conheight <pixels>`

Задаёт высоту консольного/софтварного framebuffer.

---

### bpp
`-bpp <bit>`

Переопределяет стартовую глубину цвета (`vid_bpp`).

---

### current
`-current`

Просит использовать текущие десктопные настройки вместо подбора отдельного fullscreen-режима.

---

### particles
`-particles <number>`

Задаёт стартовый лимит частиц; значение подхватывают и cvar-override, и классический particle backend.

---

### qmenu
`-qmenu`

Форсирует использование классического Quake-меню (`forceqmenu 1`).

---

### noenumerate
`-noenumerate`

Переводит перечисление рендереров/аудиоустройств в safe-ветку без полного опроса драйверов.

---

### no8bit
`-no8bit`

Отключает использование `GL_EXT_shared_texture_palette`, даже если драйвер её поддерживает.

---

### noamtex
`-noamtex`

Запрещает включать `GL_ARB_multitexture`.

---

### stayactive
`-stayactive`

На GLX-сборках не возвращает исходный fullscreen-режим при уходе приложения в фон.

---

### novmode
`-novmode`

Только Linux/X11: запрещает переключение полноэкранного режима через XF86VidMode, даже если функция доступна.

---

### forcevmode
`-forcevmode`

Только Linux/X11: принудительно запрашивает переключение полноэкранного режима через XF86VidMode, обходя cvar `x11_allow_vmode`; реальный результат всё равно зависит от библиотек и драйвера.

---

### noxrandr
`-noxrandr`

Только Linux/X11: запрещает обработку мониторов и режимов через XRandR, даже если функция доступна.

---

### forcexrandr
`-forcexrandr`

Только Linux/X11: принудительно запрашивает обработку мониторов и режимов через XRandR, обходя cvar `x11_allow_xrandr`; реальный результат всё равно зависит от библиотек и драйвера.

---

### noxim
`-noxim`

Только Linux/X11: запрещает поддержку X Input Method, даже если функция доступна.

---

### forcexim
`-forcexim`

Только Linux/X11: принудительно запрашивает поддержку X Input Method, обходя cvar `x11_allow_xim`; реальный результат всё равно зависит от библиотек и драйвера.

---

### noxcursor
`-noxcursor`

Только Linux/X11: запрещает аппаратный X11-курсор, даже если функция доступна.

---

### forcexcursor
`-forcexcursor`

Только Linux/X11: принудительно запрашивает аппаратный X11-курсор, обходя cvar `x11_allow_xcursor`; реальный результат всё равно зависит от библиотек и драйвера.

---

### nowmfullscreen
`-nowmfullscreen`

Только Linux/X11: запрещает полноэкранный режим через window manager/EWMH, даже если функция доступна.

---

### forcewmfullscreen
`-forcewmfullscreen`

Только Linux/X11: принудительно запрашивает полноэкранный режим через window manager/EWMH, обходя cvar `x11_allow_wmfullscreen`; реальный результат всё равно зависит от библиотек и драйвера.

---

### noxi2
`-noxi2`

Только Linux/X11: запрещает ввод через XInput2, даже если функция доступна.

---

### forcexi2
`-forcexi2`

Только Linux/X11: принудительно запрашивает ввод через XInput2, обходя cvar `x11_allow_xi2`; реальный результат всё равно зависит от библиотек и драйвера.

---

### nodga
`-nodga`

Только Linux/X11: запрещает мышиный ввод через DGA, даже если функция доступна.

---

### forcedga
`-forcedga`

Только Linux/X11: принудительно запрашивает мышиный ввод через DGA, обходя cvar `x11_allow_dga`; реальный результат всё равно зависит от библиотек и драйвера.

---

## Ввод, CD и звук

### nomouse
`-nomouse`

Полностью пропускает инициализацию мыши.

---

### noforcemspd
`-noforcemspd`

Не форсирует системный параметр mouse speed и включает `m_accel_noforce`. Только Windows.

---

### noforcemaccel
`-noforcemaccel`

Не форсирует системные mouse threshold/acceleration параметры и включает `m_threshold_noforce`. Только Windows.

---

### noforcemparms
`-noforcemparms`

Комбинирует поведение `-noforcemspd` и `-noforcemaccel`. Только Windows.

---

### dinput
`-dinput`

Включает DirectInput через `in_dinput 1`. Только Windows и только в сборках с `AVAIL_DINPUT`.

---

### cddev
`-cddev <device>`

Переопределяет путь к устройству CD Audio. Только Linux-ветка CD.

---

### nocdaudio
`-nocdaudio`

Отключает CD Audio на старте; также автоматически подставляется `-safe`.

---

### cdaudio
`-cdaudio`

Явно включает CD Audio, если код собран с `HAVE_CDPLAYER`.

---

### noopenal
`-noopenal`

Запрещает backend OpenAL.

---

### noalsa
`-noalsa`

Запрещает backend ALSA. Только Linux/Unix-сборки с ALSA.

---

### nooss
`-nooss`

Запрещает backend OSS и его перечисление/захват.

---

### nopulse
`-nopulse`

Запрещает backend PulseAudio.

---

### nosdlsnd
`-nosdlsnd`

Запрещает SDL audio backend.

---

### nosdl
`-nosdl`

В контексте аудио действует как дополнительный запрет SDL audio backend.

---

### nosound
`-nosound`

Полностью отключает звуковую подсистему, фиксируя `nosound 1` ещё при инициализации; также автоматически подставляется `-safe`.

---

### soundspeed
`-soundspeed <kHz>`

Переопределяет стартовую частоту микшера через `snd_khz`.

---

### sspeed
`-sspeed <kHz>`

Синоним `-soundspeed`.

---

### sndspeed
`-sndspeed <kHz>`

Ещё один синоним `-soundspeed`.

---

### snoforceformat
`-snoforceformat`

Только DirectSound: не пытается выставить формат primary buffer до выбора secondary/primary path.

---

### primarysound
`-primarysound`

Только DirectSound: разрешает предпочесть primary sound buffer вместо secondary.

---

### wavonly
`-wavonly`

Только DirectSound: полностью отключает DirectSound backend, оставляя wav/альтернативные пути.

---

### dsp
`-dsp <2|3|4>`

Только Sound Blaster backend: вручную ограничивает версию DSP.

---

## Сеть, сервер и процессы

### ip
`-ip <address>`

Привязывает сетевые сокеты к конкретному IPv4-адресу интерфейса; используется и обычной сетью, и встроенными HTTP/FTP-серверами.

---

### clport
`-clport <port>`

Задаёт локальный UDP-порт клиента вместо автоматического выбора.

---

### svport
`-svport <port>`

Переопределяет `sv_port` и `sv_port_tcp`.

---

### port
`-port <port>`

Синоним `-svport` для серверного порта.

---

### dedicated
`-dedicated`

Запускает dedicated server path вместо клиентского режима.

---

### chroot
`-chroot <path>`

Только Linux/Unix dedicated server: перед запуском пытается сменить корневой каталог процесса на указанный путь.

---

### uid
`-uid <uid>`

Только Linux/Unix dedicated server: после `-chroot` или SUID-сценария сбрасывает привилегии до указанного UID.

---

### nostdin
`-nostdin`

Запрещает чтение stdin/консольного ввода.

---

### noconinput
`-noconinput`

Только несdl Unix-клиент: отключает консольный stdin даже при наличии tty.

---

### nostdout
`-nostdout`

Только несdl Unix-клиент: запрещает обычный stdout-спам.

---

### clusterslave
`-clusterslave`

Запускает процесс как подчинённый узел map-cluster и переводит управление на pipe/remote controller.

---

### clusterhost
`-clusterhost <addr:port> <password>`

На dedicated-сервере подключается к удалённому cluster host и передаёт пароль управления.

---

### allowmapless
`-allowmapless`

Разрешает dedicated-серверу не падать мгновенно, если стартовая карта не загрузилась/не скачалась.

---

### cheats
`-cheats`

Поднимает `sv_cheats` в 1 при старте сервера.

---

### mysql
`-mysql`

Разрешает загружать MySQL-драйвер; без флага код SQL намеренно не трогает MySQL из соображений sandbox/security.

---

### noq2dll
`-noq2dll`

Запрещает загрузку Quake II game DLL и заставляет Q2 gamecode инициализацию завершиться неуспехом.

---

### color
`-color`

Только Unix dedicated server: принудительно включает ANSI-цвета stdout, даже если stdout не tty.

---

### colour
`-colour`

Британский синоним `-color` с тем же эффектом.

---

### nocolor
`-nocolor`

Только Unix dedicated server: принудительно выключает ANSI-цвета stdout.

---

### nocolour
`-nocolour`

Синоним `-nocolor`.

---

### nomutex
`-nomutex`

Только Windows-клиент и только при `QUAKESPYAPI`: не создаёт именованный mutex `qwcl`, который фронтенд использует для обнаружения запущенного клиента.

---

### noreset
`-noreset`

Только Windows dedicated server: отключает автоперезапуск после фатальной ошибки и меняет crash-handling path.

---

### register
`-register`

Только Windows dedicated server с `USESERVICE`: регистрирует сервер как системную службу.

---

### unregister
`-unregister`

Только Windows dedicated server с `USESERVICE`: удаляет зарегистрированную системную службу.

---

### register_types
`-register_types [layout]`

Только Windows-клиент: сразу запускает регистрацию file associations / URI schemes и затем завершает процесс.

---

### install
`-install`

Только Unix server path с `MANIFESTDOWNLOADS`: сначала выполняет установку пакетов, затем продолжает запуск.

---

### doinstall
`-doinstall`

Запускает установщик пакетов и после применения изменений завершает процесс.

---

## Обновления и источники пакетов

### updatesrc
`-updatesrc <url|list>`

Добавляет пользовательский источник package list; флаг можно повторять.

---

### unsafe
`-unsafe`

Используется только вместе с `-updatesrc`: помечает добавленный источник как unsafe (`SRCFL_UNSAFE`).

---

### notlstrust
`-notlstrust`

Отключает специальное доверие к зеркалам с официального update-site и заставляет обычную проверку подписи/сертификата.

---

### noupdate
`-noupdate`

Блокирует автообновление движка и package-list phone-home.

---

### noupdates
`-noupdates`

Клиентский синоним `-noupdate` для логики package-list query.

---

### noautoupdate
`-noautoupdate`

Ещё один запрет автообновления движка.

---

### noupdate-double-dash
`--noupdate`

Двухдефисный псевдоним `-noupdate`, используемый теми же путями обновления.

---

### noautoupdate-double-dash
`--noautoupdate`

Двухдефисный псевдоним `-noautoupdate`.

---

### allowupdate
`-allowupdate`

Снимает защитный запрет на self-update для нестандартно именованных бинарников/сборок.

---

## Плагины, нативный код, служебные и отладочные режимы

### noplugins
`-noplugins`

Полностью запрещает автозагрузку внешних engine plugins.

---

### nodlls
`-nodlls`

Запрещает загрузку native DLL gamecode/QVM DLL path.

---

### nosos
`-nosos`

Unix-ориентированный синоним `-nodlls`, запрещающий `.so` native gamecode.

---

### plugin
`-plugin [name]`

Переводит процесс в plugin-host режим; на Windows отдельное значение `qcdebug` в следующем аргументе меняет подрежим.

---

### qcdebug
`-qcdebug`

Запускает специальный QC/plugin debug mode с урезанным stdout/особым `isPlugin`.

---

### plugwrapper-double-dash
`--plugwrapper <dll/so> <entry>`

Служебный двухдефисный режим прямого запуска plugin wrapper entrypoint и немедленного выхода из обычного startup path.

---

### v
`-v`

Только Windows-клиент: печатает `version:` и завершает процесс.

---

### version-double-dash
`--version`

Только Windows-клиент: длинный вариант `-v`.

---

### outputdebugstring
`-outputdebugstring`

Только Windows-клиент: дублирует отладочный вывод через `OutputDebugString` path.

---

### crashonerror
`-crashonerror`

Только SDL entrypoint: превращает `Sys_Error` в преднамеренный крэш для отладчика/дампа.

---

### debugip
`-debugip <ip>`

Только в `CRAZYDEBUGGING`-сборках: отправляет debug log на TCP `ip:10000` вместо локального файла.

---

### watchdog
`-watchdog`

Только Windows/MSVC-сборки с `CATCHCRASH` и `MULTITHREAD`: запускает отдельный watchdog thread.

---

### makeinstaller
`-makeinstaller <name>`

Только Windows-клиент в сборках с `WEBCLIENT`: создаёт инсталлятор/самоупакованный exe из `<name>.fmf` и `<name>.png`.

---

### fromfrontend-double-dash
`--fromfrontend <rev> <launcher>`

Legacy-only путь обновления фронтенда: используется только в сборках с `HAVE_LEGACY` и нужен для выбора файла launcher при self-update.

---

## TLS, сертификаты, подписи и аварийная диагностика

### notls
`-notls`

Полностью отключает инициализацию TLS-провайдера GnuTLS.

---

### privkey
`-privkey <file>`

Указывает путь к приватному PEM-ключу для TLS/подписи; без флага используются стандартные имена в FS_ROOT.

---

### pubkey
`-pubkey <file>`

Указывает путь к публичному сертификату/цепочке PEM.

---

### certhost
`-certhost <hostname>`

Задаёт CN/issuer для автогенерируемого сертификата и также попадает в режим подписи пакетов.

---

### pfx
`-pfx <file.pfx>`

Только Windows SSPI/TLS build: задаёт PFX/PKCS#12-файл identity-сертификата вместо `identity.pfx`.

---

### prefix
`-prefix <string>`

Только Unix meta-helper path: добавляет строковый префикс при расчёте qhash/подписи архива.

---

### sign
`-sign <file>`

Только Unix meta-helper path: печатает старый `sign=`/`sha512=` набор для пакета.

---

### sign2
`-sign2 <file>`

Только Unix meta-helper path: печатает сырую base64-подпись файла.

---

### signraw
`-signraw <file>`

Синоним `-sign2`.

---

### signfmfpkg
`-signfmfpkg <file>`

Только Unix meta-helper path: печатает подпись в формате пакетов FMF (`prefix/filesize/sha512/signature`).

---

### qhash
`-qhash <file>`

Только Unix meta-helper path: печатает Quake pure CRC/qhash указанного архива.

---

### sha1
`-sha1 <file>`

Только Unix meta-helper path: печатает SHA-1 файла.

---

### sha256
`-sha256 <file>`

Только Unix meta-helper path: печатает SHA-256 файла.

---

### sha512
`-sha512 <file>`

Только Unix meta-helper path: печатает SHA-512 файла.

---

### nodumpstack
`-nodumpstack`

Отключает установку friendly crash handler / stack dump handler.

---

## Недоступные, устаревшие и платформенно-ограниченные параметры

### fileul
`-fileul`

Статус: недоступно в сборке по умолчанию.

---

### ip6
`-ip6 <address>`

Статус: недоступно в сборке по умолчанию.

---

### nomtex
`-nomtex`

Статус: недоступно в сборке по умолчанию.

---

### zone
`-zone <kb>`

Статус: legacy/inactive.

---

### stdvid
`-stdvid`

Статус: legacy/inactive — флаг автоматически подставляется `-safe`.

---

### nolan
`-nolan`

Статус: legacy/inactive — присутствует только в списке safe-аргументов.

---

### nojoy
`-nojoy`

Статус: legacy/inactive — присутствует только в списке safe-аргументов.

---

### quake_rerel
`-quake_rerel`

Выбирает преднастроенный режим Quake Re-Release (`QuakeEX.kpf`). Статус: только сборки с `HAVE_LEGACY`. 

---

### quake
`-quake`

Выбирает стандартный режим Quake/id1. Статус: только сборки с `HAVE_LEGACY`; также учитывается в SUID/chroot логике Unix-сервера. 

---

### afterquake
`-afterquake`

Альтернативное имя режима Quake для отдельного install-name/реестра. Статус: только сборки с `HAVE_LEGACY`. 

---

### netquake
`-netquake`

Выбирает netquake-совместимый режим без QW-специфичных допущений. Статус: только сборки с `HAVE_LEGACY`. 

---

### spasm
`-spasm`

Выбирает FauxSpasm/QuakeSpasm-совместимый режим. Статус: только сборки с `HAVE_LEGACY`. 

---

### fitz
`-fitz`

Выбирает FauxFitz-режим совместимости. Статус: только сборки с `HAVE_LEGACY`. 

---

### tenebrae
`-tenebrae`

Выбирает FauxTenebrae-режим совместимости. Статус: только сборки с `HAVE_LEGACY`. 

---

### ezquake
`-ezquake`

Выбирает ezQuake-совместимый режим/набор настроек. Статус: только сборки с `HAVE_LEGACY`. 

---

### quake2
`-quake2`

Выбирает режим Quake II / `baseq2`. Статус: только сборки с `Q2CLIENT` или `Q2SERVER`; учитывается и в SUID/chroot логике Unix-сервера. 

---

### dday
`-dday`

Выбирает преднастроенный режим D-Day: Normandy на базе Quake II. Статус: только сборки с `Q2CLIENT` или `Q2SERVER`. 

---

### hipnotic
`-hipnotic`

Выбирает режим Quake Mission Pack 1; дополнительно форсирует hipnotic HUD/layout, если флаг задан явно. Статус: только legacy/Quake-сборки с этим gamemode. 

---

### rogue
`-rogue`

Выбирает режим Quake Mission Pack 2; дополнительно форсирует rogue HUD/layout, если флаг задан явно. Статус: только legacy/Quake-сборки с этим gamemode. 

---

### dopa
`-dopa`

Выбирает режим Dimensions of the Past. Статус: только сборки с `HAVE_LEGACY`. 

---

### mg1
`-mg1`

Выбирает режим Dimension of the Machine. Статус: только сборки с `HAVE_LEGACY`. 

---

### quoth
`-quoth`

Выбирает режим Quoth и связанные compat-хаки. Статус: только сборки с `HAVE_LEGACY`. 

---

### nehahra
`-nehahra`

Выбирает режим Seal of Nehahra. Статус: только сборки с `HAVE_LEGACY`. 

---

### librequake
`-librequake`

Выбирает режим LibreQuake. Статус: зависит от наличия соответствующих данных; запись активна в `gamemode_info`. 

---

### portals
`-portals`

Выбирает режим Hexen II Mission Pack / Portal of Praevus. Статус: только сборки с `HEXEN2`; также учитывается в SUID/chroot логике Unix-сервера. 

---

### hexen2
`-hexen2`

Выбирает режим Hexen II. Статус: только сборки с `HEXEN2`; также учитывается в SUID/chroot логике Unix-сервера. 

---

### quake3
`-quake3`

Выбирает режим Quake III Arena. Статус: только сборки с `Q3CLIENT` или `Q3SERVER`; требует plugin `fteplug_quake3`. 

---

### quake3demo
`-quake3demo`

Выбирает режим Quake III Arena Demo. Статус: только сборки с `Q3CLIENT` или `Q3SERVER`; требует plugin `fteplug_quake3`. 

---

### cod4
`-cod4`

Выбирает режим Call of Duty 4. Статус: только Q3-ветки; требует plugin `fteplug_cod`. 

---

### cod2
`-cod2`

Выбирает режим Call of Duty 2. Статус: только Q3-ветки; требует plugin `fteplug_cod`. 

---

### cod
`-cod`

Выбирает режим Call of Duty. Статус: только Q3-ветки; требует plugin `fteplug_cod`. 

---

### halflife
`-halflife`

Выбирает режим Half-Life / Rad-Therapy. Статус: требует соответствующие данные и пакет `fteplug_ffmpeg` из описания gamemode. 

---

### gunman
`-gunman`

Выбирает режим Gunman Chronicles. Статус: требует соответствующие данные и пакет `fteplug_ffmpeg`. 

---

### halflife2
`-halflife2`

Выбирает режим Half-Life 2 / Rad-Therapy II. Статус: требует пакеты `fteplug_ffmpeg`, `fteplug_ode`, `fteplug_hl2`. 

---

### gmod9
`-gmod9`

Выбирает режим Garry's Mod 9 / Free Will. Статус: требует пакеты `fteplug_ffmpeg`, `fteplug_ode`, `fteplug_hl2` и соответствующие данные HL2/CSS. 

---

### nexuiz
`-nexuiz`

Выбирал бы режим Nexuiz. Статус: недоступно в сборке по умолчанию.
---

### xonotic
`-xonotic`

Выбирал бы режим Xonotic. Статус: недоступно в сборке по умолчанию.
---

### spark
`-spark`

Выбирал бы режим Spark. Статус: недоступно в сборке по умолчанию.
---

### scouts
`-scouts`

Выбирал бы режим Scouts Journey. Статус: недоступно в сборке по умолчанию.
---

### rmq
`-rmq`

Выбирал бы режим Remake Quake. Статус: недоступно в сборке по умолчанию.
---

### quake4
`-quake4`

Выбирал бы режим Quake 4. Статус: недоступно в сборке по умолчанию.
---

### et
`-et`

Выбирал бы режим Wolfenstein: Enemy Territory. Статус: недоступно в сборке по умолчанию.
---

### jk2
`-jk2`

Выбирал бы режим Jedi Knight II. Статус: недоступно в сборке по умолчанию.
---

### warsow
`-warsow`

Выбирал бы режим Warsow. Статус: недоступно в сборке по умолчанию.
---

### doom
`-doom`

Выбирал бы режим Doom. Статус: недоступно в сборке по умолчанию.
---

### doom2
`-doom2`

Выбирал бы режим Doom II. Статус: недоступно в сборке по умолчанию.
---

### doom3
`-doom3`

Выбирал бы режим Doom 3. Статус: недоступно в сборке по умолчанию.
---

### diablo2
`-diablo2`

Выбирал бы режим Diablo II. Статус: недоступно в сборке по умолчанию.
> [⬅ Предыдущая страница](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md) | [Следующая страница ➡](02-client-ui-commands.md)

> [⬅ Вернуться к оглавлению вики](../README.md)