# Скриншоты (включая панораму 360°, кубокарту, VR-стерео) и захват видео

> [⬅ Предыдущая страница](../12-postprocessing/color-correction-space.md) | [Следующая страница ➡](../14-vr-stereo/vr-headset-support.md)

> [⬅ Вернуться к оглавлению вики](../README.md)

## Что это даёт геймдизайнеру

Движок умеет не только делать обычные скриншоты, но и создавать специализированные виды снимков, полезные для презентации проекта, создания рекламных материалов или неигровых технических нужд (например, запечь окружение в кубокарту, см. [«Кубические карты и изображения неба»](../04-textures-images/cubemaps-skybox-images.md)): равнопрямоугольную 360-градусную панораму, полноценную стереоскопическую VR-панораму для просмотра в шлеме виртуальной реальности, шестигранную кубокарту, снимок произвольного разрешения, не привязанного к экрану, и обычную стерео-пару. Помимо статичных изображений, движок умеет напрямую записывать происходящее на экране в видеофайл — либо в реальном времени во время игры, либо оффлайн по уже записанной демозаписи, что даёт возможность получить куда более плавный и качественный ролик, чем при обычной записи экрана сторонней программой.

---

## Интерфейс настройки

Все виды захвата вызываются консольными командами:

| Команда | Аргументы | Что делает |
|---|---|---|
| [`screenshot`](../44-cli-commands-reference/02-client-ui-commands.md#screenshot) | без аргументов | Обычный скриншот текущего кадра в разрешении экрана |
| [`screenshot_mega <name> [width] [height]`](../44-cli-commands-reference/02-client-ui-commands.md#screenshot_mega) | строка + два необязательных целых числа (пикселей) | Скриншот произвольного, не привязанного к разрешению монитора размера — позволяет получить снимок куда большего разрешения, чем сам экран |
| [`screenshot_stereo <name> [width] [height]`](../44-cli-commands-reference/02-client-ui-commands.md#screenshot_stereo) | строка + два необязательных целых числа | Простой стерео-скриншот (отдельно для левого и правого глаза, либо в формате `.pns`, либо в режиме quad-buffer) |
| [`screenshot_360 <name> [width] [height]`](../44-cli-commands-reference/02-client-ui-commands.md#screenshot_360) | строка + два необязательных целых числа | Равнопрямоугольная (equirectangular) 360-градусная панорама всего окружения |
| [`screenshot_vr <name> [width]`](../44-cli-commands-reference/02-client-ui-commands.md#screenshot_vr) | строка + необязательное целое число | Сферическая стереоскопическая панорама для просмотра в VR-гарнитуре |
| [`screenshot_cubemap <name> [size]`](../44-cli-commands-reference/02-client-ui-commands.md#screenshot_cubemap) | строка + необязательное целое число (сторона грани в пикселях) | Шесть снимков, формирующих полную кубокарту окружения |

Для захвата видео используются отдельные команды и группа настроечных переменных:

| Команда / переменная | Тип / формат | Описание | Значение по умолчанию |
|---|---|---|---|
| [`capture <filename>`](../44-cli-commands-reference/02-client-ui-commands.md#capture) | Строка (без аргумента — выводит список доступных драйверов захвата) | Начинает запись происходящего на экране прямо во время игры в реальном времени | — |
| [`capturedemo <demo file> <video file>`](../44-cli-commands-reference/02-client-ui-commands.md#capturedemo) | Две строки | Конвертирует уже существующую демозапись матча (см. [«Демозаписи»](../README.md#администрирование-трансляция-и-запись-матчей)) в готовый видеофайл покадрово «за кулисами» (не в реальном времени), что позволяет получить идеально плавное видео произвольного размера даже на слабом компьютере | — |
| [`capturepause`](../44-cli-commands-reference/02-client-ui-commands.md#capturepause) | без аргументов | Приостанавливает текущую запись видео (повторный вызов возобновляет её) — удобно, чтобы вырезать неинтересные моменты прямо во время записи | — |
| [`capturestop`](../44-cli-commands-reference/02-client-ui-commands.md#capturestop) | без аргументов | Прерывает текущий захват видео | — |
| [`capturerate`](../38-cvars-reference/04-network-server-cvars.md#capturerate) | Целое число (кадров/сек) | Частота кадров захватываемого видео | `30` |
| [`capturedemowidth`](../38-cvars-reference/07-system-misc-cvars.md#capturedemowidth) / [`capturedemoheight`](../38-cvars-reference/07-system-misc-cvars.md#capturedemoheight) | Целое число (пикселей) | При использовании [`capturedemo`](../44-cli-commands-reference/02-client-ui-commands.md#capturedemo) задаёт размер внутреннего изображения, в которое рендерится видео — может превышать физическое разрешение монитора | `0` (использовать текущее разрешение экрана) |
| [`capturedriver`](../38-cvars-reference/07-system-misc-cvars.md#capturedriver) | Строка, имя драйвера | Драйвер, используемый для захвата демозаписи в видео (список доступных выводится командой [`capture`](../44-cli-commands-reference/02-client-ui-commands.md#capture) без аргументов) | пусто (автовыбор) |
| [`capturecodec`](../38-cvars-reference/07-system-misc-cvars.md#capturecodec) | Строка, имя кодека/контейнера | Кодек сжатия/кодирования для итогового видео; при «сыром» захвате (raw) сюда указывается одно из расширений скриншотов (`tga`,`png`,`jpg`,`pcx`) | зависит от сборки движка |
| [`capturesound`](../38-cvars-reference/03-audio-cvars.md#capturesound) | `0`/`1` | Включает захват игрового звука (голоса) в видео; при обычном [`capture`](../44-cli-commands-reference/02-client-ui-commands.md#capture) (не [`capturedemo`](../44-cli-commands-reference/02-client-ui-commands.md#capturedemo)) можно совместить с [`cl_voip_test`](../38-cvars-reference/03-audio-cvars.md#cl_voip_test), чтобы записать и собственный голос игрока | `1` |
| [`capturesoundchannels`](../38-cvars-reference/03-audio-cvars.md#capturesoundchannels) | Целое число | Количество звуковых каналов захватываемой аудиодорожки | `2` |
| [`capturesoundbits`](../38-cvars-reference/03-audio-cvars.md#capturesoundbits) | Целое число | Битность захватываемого звука | `16` |
| [`capturemessage`](../38-cvars-reference/07-system-misc-cvars.md#capturemessage) | Строка | Произвольный текст, который может выводиться поверх процесса записи | пусто |
| [`capturethrottlesize`](../38-cvars-reference/07-system-misc-cvars.md#capturethrottlesize) | Целое число (мегабайт) | Если задано, запись существенно замедляется при нехватке свободного места на диске меньше указанного значения — полезно при записи потока на RAM-диск, который параллельно обрабатывается другой программой | `0` (не ограничено) |

---

## Примеры

- Автор мода делает панораму [`screenshot_360 promo_shot 4096 2048`](../44-cli-commands-reference/02-client-ui-commands.md#screenshot_360) с высокой точки уровня для использования на странице мода в магазине или на форуме, указав удвоенное по сравнению с экраном разрешение для лучшей чёткости.
- Разработчик конвертирует записанную демонстрацию удачного прохождения командой [`capturedemo showcase.dem showcase.avi`](../44-cli-commands-reference/02-client-ui-commands.md#capturedemo), предварительно подняв [`capturedemowidth 1920`](../38-cvars-reference/07-system-misc-cvars.md#capturedemowidth)/[`capturedemoheight 1080`](../38-cvars-reference/07-system-misc-cvars.md#capturedemoheight) и [`capturerate 60`](../38-cvars-reference/04-network-server-cvars.md#capturerate), получая идеально плавный рекламный ролик без просадок частоты кадров, характерных для записи экрана в реальном времени.
- Стример записывает собственный игровой процесс командой [`capture stream.avi`](../44-cli-commands-reference/02-client-ui-commands.md#capture) с включённым [`capturesound 1`](../38-cvars-reference/03-audio-cvars.md#capturesound) и [`cl_voip_test 1`](../38-cvars-reference/03-audio-cvars.md#cl_voip_test), чтобы в итоговое видео попал и его голос из микрофона.
- Перед долгой записью на медленный внешний диск задаётся [`capturethrottlesize 500`](../38-cvars-reference/07-system-misc-cvars.md#capturethrottlesize), чтобы движок автоматически замедлял захват при падении свободного места ниже 500 мегабайт, избегая обрыва записи.

---

## Смежные страницы

- [Кубические карты и изображения неба](../04-textures-images/cubemaps-skybox-images.md)
- [VR и стереоскопия](../README.md#экран-vr-и-выбор-рендерера)
- [Демозаписи](../README.md#администрирование-трансляция-и-запись-матчей)

> [⬅ Предыдущая страница](../12-postprocessing/color-correction-space.md) | [Следующая страница ➡](../14-vr-stereo/vr-headset-support.md)

> [⬅ Вернуться к оглавлению вики](../README.md)