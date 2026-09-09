# FTEQW Wiki

Независимая вики для геймдизайнеров, левел-дизайнеров и моддеров, не читающих исходный код движка FTEQW — форк движка Quake, который вырос в отдельную мультидвижковую платформу: он умеет запускать контент Quake, QuakeWorld, Hexen II, а также (при наличии соответствующих официальных файлов) содержимое Quake II/III, Doom и других игр, добавляя поверх классической идеи id Software современные технологии рендеринга, сети и скриптинга.

> ВНИМАНИЕ! Данная вики была сгенерирована автоматически на основе исходного кода из репозитория [https://github.com/fte-team/fteqw](https://github.com/fte-team/fteqw) на основе master ветки с коммитом `f937b9d`.

> Она может быть недостоверной в мелочах, но общее представление и справочник API предоставить способна. Учитывая, что у основного проекта вообще нет полноценной документации, это вполне полезная альтернатива.

> Также подразумевалось, что эта вики станет отличным дополнением для [FTEQW Base Game](https://github.com/dron12261games/Q-MOD-FTEQW-Base-Game).

Каждая страница этой вики описывает одну конкретную возможность движка: что она даёт с точки зрения геймдизайна, из каких файлов и настроек она состоит и как её включить или настроить через файлы игры — без необходимости трогать или даже видеть исходный код движка на языке Си. Условное разделение вики на две большие части:

- **«Карта возможностей»** — рассказывает, что вообще умеет движок: какие форматы ресурсов он поддерживает, какие визуальные и сетевые технологии доступны, как устроены моды и платформы. Читать эти статьи можно в любом порядке, но новичкам лучше начать с раздела [«0. Введение»](00-introduction/README.md).
- **«Справочник API»** — построчный технический разбор конкретных элементов, с которыми моддер и картостроитель работают руками: встроенные функции QuakeC, cvar, ключи сущностей на карте, директивы файлов материалов и частиц. Каждый элемент API — это отдельная самодостаточная статья с точной сигнатурой, разбором аргументов, описанием логики работы и рабочими примерами кода. Если вам нужно быстро найти конкретную функцию/cvar/ключ, а не читать раздел целиком — воспользуйтесь [сводными таблицами быстрой навигации](42-api-quick-reference/README.md).

**Никогда не пользовались FTEQW? Начните с раздела [«0. Введение»](00-introduction/README.md)** — там простым языком объясняется, что такое FTEQW, откуда он взялся, как его установить и запустить, а также даётся словарь базовых терминов, используемых во всей остальной вики.

## Навигационная карта

### Введение

- [Что такое FTEQW](00-introduction/what-is-fteqw.md)
- [История и философия проекта](00-introduction/history-and-philosophy.md)
- [В какие игры и на чём можно играть](00-introduction/supported-games-and-platforms.md)
- [Ключевые понятия и словарь терминов](00-introduction/key-concepts-glossary.md)
- [Структура папок игры](00-introduction/directory-structure-basics.md)
- [Установка и первый запуск](00-introduction/installation-and-first-launch.md)
- [Быстрый старт: первые команды](00-introduction/quickstart-first-commands.md)
- [Как пользоваться этой вики](00-introduction/how-to-use-this-wiki.md)

## Контент, ресурсы и ассеты

### Архивы и упаковка игрового контента

- [PAK — классический архив данных](01-archives-packaging/pak-archives.md)
- [PK3/PK4 — архивы в формате ZIP](01-archives-packaging/pk3-pk4-archives.md)
- [Архив-как-папка (pk3dir)](01-archives-packaging/pk3dir-virtual-archive.md)
- [Сжатые архивы DZ/XZ/GZ](01-archives-packaging/compressed-archives-dz-xz-gz.md)
- [Архивы сторонних игр (VPK, MPQ, GMA, WAD)](01-archives-packaging/third-party-archives.md)
- [Проверка целостности и «чистые» серверные архивы](01-archives-packaging/content-integrity-pure-packages.md)

### Трёхмерные модели и анимация

- [Классические модели и спрайты Quake (MDL/SPR)](02-models-animation/classic-quake-models-mdl-spr.md)
- [Модели поздних игр серии Quake (MD2/MD3)](02-models-animation/later-quake-models-md2-md3.md)
- [Современные скелетные модели (IQM/MD5/DPM/ZYM)](02-models-animation/skeletal-models-iqm-md5-dpm-zym.md)
- [Импорт из внешних 3D-редакторов (OBJ/glTF/LWO/ASE/PSK)](02-models-animation/external-editor-import-obj-gltf.md)
- [Модели сторонних движков (Half-Life, Source, Call of Duty)](02-models-animation/third-party-engine-models.md)
- [Скелетные теги и присоединение объектов (tag attachment)](02-models-animation/skeletal-tags-attachment.md)
- [Внешние файлы анимаций (externalanim)](02-models-animation/external-animation-files.md)

### Уровни, карты и ландшафт

- [Классические карты Quake/Hexen II (BSP)](03-maps-levels-terrain/classic-quake-hexen2-bsp.md)
- [Расширенные лимиты карт (BSP2 и другие варианты формата)](03-maps-levels-terrain/extended-bsp-limits-bsp2.md)
- [Карты Quake II/III и родственных игр (включая RTCW и др.)](03-maps-levels-terrain/quake2-quake3-maps.md)
- [Карты Half-Life 2 (VBSP)](03-maps-levels-terrain/half-life-2-vbsp-maps.md)
- [Карты Doom (WAD)](03-maps-levels-terrain/doom-wad-maps.md)
- [Внешние файлы сущностей карты (.ent)](03-maps-levels-terrain/external-entity-files-ent.md)
- [Внешние патчи освещения и видимости (.lit/.vis)](03-maps-levels-terrain/external-lighting-visibility-patches.md)
- [Ландшафт на основе карт высот (heightmap terrain)](03-maps-levels-terrain/heightmap-terrain.md)
- [Совместное редактирование ландшафта в реальном времени](03-maps-levels-terrain/networked-terrain-editing.md)

### Текстуры и изображения

- [Базовые растровые форматы (TGA/PNG/JPG/BMP/PCX)](04-textures-images/basic-raster-formats.md)
- [Сжатые «видеокарточные» форматы (DDS/KTX/PKM/ASTC)](04-textures-images/gpu-compressed-formats.md)
- [HDR-изображения (HDR/EXR)](04-textures-images/hdr-images.md)
- [Кубические карты окружения (skybox/cubemap)](04-textures-images/cubemaps-skybox-images.md)
- [Автоматическая подгрузка карт рельефа и бликов (_norm/_bump/_spec)](04-textures-images/auto-normal-specular-maps.md)

### Звук и музыка

- [Стандартный звук WAV](05-audio-music/wav-sound.md)
- [Сжатая музыка и озвучка (OGG/MP3/FLAC/Opus)](05-audio-music/compressed-music-ogg-mp3-flac-opus.md)
- [Фоновый (эмбиент) звук и микширование](05-audio-music/ambient-sound-mixing.md)

### Видео и катсцены

- [Классические видео-заставки (ROQ/CIN)](06-video-cutscenes/classic-video-roq-cin.md)
- [Современные видеоформаты (OGV/WebM/MP4/AVI)](06-video-cutscenes/modern-video-ogv-webm-mp4.md)
- [Видео как текстура на поверхности (videomap)](06-video-cutscenes/videomap-video-as-texture.md)

### Шрифты и текст

- [Встроенные растровые шрифты движка](07-fonts-text/builtin-bitmap-fonts.md)
- [Подключение TTF-шрифтов](07-fonts-text/ttf-fonts.md)
- [Постобработка текста (обводка, чёткое отображение)](07-fonts-text/text-postprocessing.md)

## Рендеринг и визуальные эффекты

### Материалы и шейдеры

- [Язык описания материалов (в стиле Quake III, .shader)](08-materials-shaders/shader-script-language.md)
- [Многослойные и полупрозрачные материалы](08-materials-shaders/multilayer-transparent-materials.md)
- [Анимация текстурных координат (вращение/скролл/волны)](08-materials-shaders/texcoord-animation.md)
- [Формулы цвета и прозрачности (rgbGen/alphaGen)](08-materials-shaders/color-alpha-formulas.md)
- [Деформация геометрии по формулам (deformvertexes)](08-materials-shaders/vertex-deformation.md)
- [PBR-материалы (физически корректный рендеринг)](08-materials-shaders/pbr-materials.md)

### Освещение и тени

- [Запечённое освещение карты и лайтстили](09-lighting-shadows/baked-lightmaps-lightstyles.md)
- [Направленное запечённое освещение (deluxemap) для рельефных материалов](09-lighting-shadows/deluxemap-directional-lighting.md)
- [Динамический свет от эффектов (вспышки, взрывы)](09-lighting-shadows/dynamic-light-from-effects.md)
- [Полностью динамическое освещение сцены и тени в реальном времени](09-lighting-shadows/realtime-world-lighting-shadows.md)

### Частицы, следы и пятна

- [Скриптовый язык описания частиц](10-particles-decals-trails/particle-script-language.md)
- [Наборы эффектов разного качества](10-particles-decals-trails/particle-quality-presets.md)
- [Пятна и потёки на поверхностях (stains)](10-particles-decals-trails/surface-stains.md)

### Небо, вода, туман

- [Небесная кубокарта (skybox) и её вращение](11-sky-water-fog/skybox-rotation.md)
- [Вода с отражениями и преломлениями](11-sky-water-fog/water-reflections-refractions.md)
- [Туман (обычный и по зонам)](11-sky-water-fog/fog-zones.md)

### Постобработка изображения

- [HDR-рендеринг с автоэкспозицией](12-postprocessing/hdr-auto-exposure.md)
- [Bloom-эффект (свечение ярких объектов)](12-postprocessing/bloom-effect.md)
- [Цветокоррекция и цветовое пространство](12-postprocessing/color-correction-space.md)

### Захват изображения и видео

- [Скриншоты (включая панораму 360°, кубокарту, VR-стерео)](13-screenshots-video-capture/screenshots-360-vr-cubemap.md)

### VR и стереоскопия

- [Поддержка VR-гарнитур](14-vr-stereo/vr-headset-support.md)
- [VR-ввод (контроллеры, отслеживание рук)](14-vr-stereo/vr-input-hand-tracking.md)
- [Стереоскопические режимы вывода](14-vr-stereo/stereoscopic-output-modes.md)

### Выбор движка рендеринга

- [Переключение графического бэкенда](15-renderer-backends/renderer-backend-switch.md)

## Игровая логика и скрипты

### Игровая логика: язык QuakeC

- [Синтаксис языка QuakeC: основы](16-quakec-scripting/quakec-language-basics.md)
- [Компилятор FTEQCC](16-quakec-scripting/fteqcc-compiler.md)
- [Стандартные заголовочные файлы для QuakeC](16-quakec-scripting/standard-header-files.md)
- [Серверная игровая логика (SSQC)](16-quakec-scripting/server-side-quakec-ssqc.md)
- [Клиентская логика и интерфейс (CSQC)](16-quakec-scripting/client-side-quakec-csqc.md)
- [Логика игровых меню (MenuQC)](16-quakec-scripting/menu-quakec.md)
- [Множественные аддон-скрипты и модульные прогс-файлы](16-quakec-scripting/addon-modular-progs.md)
- [Расширенные типы данных в QuakeC](16-quakec-scripting/extended-quakec-datatypes.md)
- [Отладка и «горячая» пересборка логики](16-quakec-scripting/hot-reload-debugging.md)

### Альтернативные языки и виртуальные машины игровой логики

- [Lua-скрипты на сервере](17-alternative-scripting-vms/server-side-lua.md)
- [Альтернативный байт-код игровой логики (Q1QVM)](17-alternative-scripting-vms/q1qvm-bytecode.md)
- [Совместимость с модулями логики Quake III / Half-Life](17-alternative-scripting-vms/quake3-halflife-game-modules.md)

### Работа с данными из игровой логики

- [Встроенная база данных (SQLite/MySQL)](18-data-access-from-scripts/embedded-database-sql.md)
- [Чтение и запись файлов из игровой логики](18-data-access-from-scripts/file-read-write-from-scripts.md)
- [Разбор и создание данных в формате JSON](18-data-access-from-scripts/json-parsing-generation.md)

## Конфигурация, ввод и пользовательские данные

### Конфигурационные файлы и консоль

- [Автозагружаемые конфигурационные файлы](19-config-console/autoexec-config-files.md)
- [Настраиваемые переменные движка (cvar)](19-config-console/cvars-engine-variables.md)
- [Алиасы и командные макросы](19-config-console/aliases-macros.md)
- [Параметры запуска игры](19-config-console/startup-parameters.md)
- [Привязка клавиш и устройств ввода](19-config-console/key-bindings-input-devices.md)

### Пользовательские данные игрока

- [Ник, скин и командная принадлежность игрока (userinfo)](20-player-userinfo/player-userinfo-name-skin-team.md)

### Локализация и переводы

- [Файлы перевода интерфейса](21-localization/ui-translation-files.md)
- [Фильтрация нежелательных слов в чате](21-localization/chat-word-filtering.md)

### Сохранения игры

- [Система сохранения прогресса](22-savegames/savegame-system.md)

## Моды, сеть и мультиплеер

### Моды, сборки и манифесты

- [Файл-манифест мода](23-mods-manifests/mod-manifest-file.md)
- [Автоматическая докачка недостающих файлов мода](23-mods-manifests/automatic-content-download.md)
- [Переключение между установленными модами](23-mods-manifests/switching-between-mods.md)

### Сетевые протоколы и мультиплеер

- [Совместимость с протоколами классических игр](24-network-protocols-multiplayer/classic-protocol-compatibility.md)
- [Расширенные сетевые возможности (больше игроков, точность, разделение экрана)](24-network-protocols-multiplayer/extended-network-features.md)
- [Голосовой чат по сети](24-network-protocols-multiplayer/voice-chat.md)

### Поиск серверов и мастер-серверы

- [Браузер серверов и избранное](25-server-browser-masters/server-browser-favorites.md)
- [Поиск серверов в локальной сети (LAN)](25-server-browser-masters/lan-discovery.md)

### Загрузка контента из интернета

- [Автодокачивание недостающего контента прямо во время игры](26-content-autodownload/mid-game-content-autodownload.md)

### Встроенный веб-сервер и удалённое администрирование

- [Управление сервером через RCON и встроенный веб-сервер](27-web-server-rcon/rcon-remote-administration.md)

### Прямые соединения и обход NAT

- [Помощь в прямом соединении между игроками (ICE/STUN/TURN)](28-nat-traversal/ice-stun-turn-p2p.md)

### Ретрансляция и наблюдение за игрой (QTV)

- [Прокси-трансляция матчей для зрителей](29-qtv-relay/qtv-proxy-broadcasting.md)

### Запись и просмотр демо

- [Классические демозаписи и многоракурсные демо](30-demos-recording/demo-recording-playback.md)

## Расширяемость и интеграции

### Встроенный веб-браузер и HTML-контент в игре

- [Автоматическая веб-страница сервера и запуск игры прямо по ссылке](31-embedded-web-browser/in-game-web-pages.md)

### Интеграция со сторонними сервисами и мессенджерами

- [IRC-чат прямо из игры](32-third-party-services/irc-chat-integration.md)
- [Мгновенные сообщения через Jabber/XMPP](32-third-party-services/jabber-xmpp-messaging.md)

### Физические движки

- [Продвинутая физика объектов (ODE)](33-physics-engines/advanced-object-physics-bullet-ode.md)

### Безопасность и защита контента

- [Проверка целостности файлов (контрольные суммы/хеши)](34-security-content-protection/content-integrity-checksums.md)
- [Защищённые сетевые соединения](34-security-content-protection/secure-network-connections.md)

### Расширяемость через плагины движка

- [Подключаемые модули движка](35-engine-plugins/engine-plugin-modules.md)

### Платформы: мобильные устройства и веб-браузер

- [Сенсорное управление на мобильных устройствах](36-mobile-web-platforms/mobile-touch-controls.md)
- [Запуск игры прямо в веб-браузере](36-mobile-web-platforms/run-in-browser-webgl.md)

## Справочник API (для моддеров и картостроителей)

Отдельная группа разделов — не по возможностям движка, а по конкретным элементам, с которыми моддер и картостроитель работают руками: функции QuakeC, переменные консоли, ключи сущностей на карте, директивы файлов материалов и частиц. Каждый элемент API описан отдельной, максимально подробной статьёй-руководством.

### Быстрая навигация по API (сводные таблицы)

- [Все функции, cvar, ключи и директивы одной таблицей со ссылками](42-api-quick-reference/README.md)

### Встроенные функции QuakeC (builtins)

- [Индекс раздела](37-quakec-builtins-reference/README.md)
- [Точки входа: SSQC, CSQC, MenuQC](37-quakec-builtins-reference/00-entry-points.md)
- [Математика и работа с векторами](37-quakec-builtins-reference/01-math-vector-builtins.md)
- [Строки и текст](37-quakec-builtins-reference/02-string-builtins.md)
- [Сущности и игровой мир](37-quakec-builtins-reference/03-entity-world-builtins.md)
- [Сеть и сетевые сообщения](37-quakec-builtins-reference/04-network-messages-builtins.md)
- [Звук](37-quakec-builtins-reference/05-sound-builtins.md)
- [Файлы, буферы, хеш-таблицы и базы данных](37-quakec-builtins-reference/06-files-database-builtins.md)
- [Прекэш и игровые ресурсы](37-quakec-builtins-reference/07-precache-resources-builtins.md)
- [Рендеринг и сцена CSQC](37-quakec-builtins-reference/08-csqc-rendering-builtins.md)
- [Ввод, интерфейс и клавиатура CSQC](37-quakec-builtins-reference/09-csqc-input-ui-builtins.md)
- [Функции MenuQC (меню, экран загрузки)](37-quakec-builtins-reference/13-menuqc-builtins.md)
- [Скелетная анимация и модели](37-quakec-builtins-reference/10-skeletal-model-builtins.md)
- [Браузер серверов и мастер-сервер](37-quakec-builtins-reference/11-server-browser-builtins.md)
- [Системные функции, отладка и cvar](37-quakec-builtins-reference/12-system-debug-builtins.md)
- [Редактор карт, криптография и разные редкие builtins](37-quakec-builtins-reference/14-editor-crypto-misc-builtins.md)

### Переменные движка (cvar reference)

- [Индекс раздела](38-cvars-reference/README.md)
- [Видео, экран и общий рендеринг](38-cvars-reference/01-video-rendering-cvars.md)
- [Освещение, тени и материалы](38-cvars-reference/02-lighting-materials-cvars.md)
- [Звук](38-cvars-reference/03-audio-cvars.md)
- [Сеть, сервер и мультиплеер](38-cvars-reference/04-network-server-cvars.md)
- [Физика и игровой процесс](38-cvars-reference/05-physics-gameplay-cvars.md)
- [Интерфейс, консоль и управление](38-cvars-reference/06-ui-console-input-cvars.md)
- [Системные, отладочные и прочие cvar](38-cvars-reference/07-system-misc-cvars.md)

### Ключи сущностей карты (entity keys)

- [Индекс раздела](39-entity-keys-reference/README.md)
- [Общие ключи, worldspawn и глобальные настройки уровня](39-entity-keys-reference/01-worldspawn-common-keys.md)
- [Свет и освещение](39-entity-keys-reference/02-light-entity-keys.md)
- [Триггеры и логические сущности](39-entity-keys-reference/03-trigger-logic-keys.md)
- [Двери, платформы и подвижная геометрия](39-entity-keys-reference/04-func-brush-entity-keys.md)
- [Монстры, NPC и точки появления игрока](39-entity-keys-reference/05-monster-player-keys.md)
- [Предметы и оружие](39-entity-keys-reference/06-item-weapon-keys.md)

### Директивы языка материалов (.shader)

- [Индекс раздела](40-shader-directives-reference/README.md)
- [Директивы уровня материала](40-shader-directives-reference/01-shader-toplevel-directives.md)
- [Директивы уровня стадии](40-shader-directives-reference/02-shader-stage-directives.md)

### Директивы языка частиц (.particles)

- [Индекс раздела](41-particle-directives-reference/README.md)
- [Директивы эффекта](41-particle-directives-reference/01-particle-effect-directives.md)
- [Директивы поведения и появления](41-particle-directives-reference/02-particle-spawn-behaviour-directives.md)

## Устаревшие, экспериментальные и недоступные по умолчанию возможности

- [Индекс раздела](43-legacy-unused-features/README.md)
- [Software-рендерер и D3D9: устаревшие пути отрисовки](43-legacy-unused-features/software-and-d3d9-renderer.md)
- [Экспериментальные бэкенды D3D11/Vulkan и трассировка теней](43-legacy-unused-features/experimental-d3d11-vulkan.md)
- [Физика Bullet/ODE: только через внешние сборки](43-legacy-unused-features/bullet-ode-physics-external-only.md)
- [Lua-скрипты: недоступны без пересборки движка](43-legacy-unused-features/lua-not-default.md)
- [Устаревшие и «заглушечные» встроенные функции QuakeC](43-legacy-unused-features/deprecated-and-stub-builtins.md)
- [Совместимость с игровыми модулями Quake III / Half-Life](43-legacy-unused-features/native-game-modules-not-default.md)
- [Формат моделей Half-Life: ограничения поддержки](43-legacy-unused-features/halflife-model-format-caveats.md)
- [Игровая логика Half-Life: исключена из обычной сборки](43-legacy-unused-features/halflife-gameplay-code-removed.md)
- [Устаревшие и ненадёжные сетевые возможности](43-legacy-unused-features/unreliable-networking-features.md)
- [Устаревшее меню на основе m_script](43-legacy-unused-features/legacy-script-menu.md)
