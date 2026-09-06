# Язык эффектов частиц

> [⬅ Вернуться к оглавлению вики](../README.md)

> Раздел: [Частицы, декали и следы](./README.md)

## Что это даёт геймдизайнеру

Движок предоставляет собственный текстовый **язык описания эффектов частиц**, позволяющий полностью настроить внешний вид, поведение и физику любого эффекта — от одиночной вспышки выстрела до сложного многослойного взрыва — без необходимости в программировании и без перекомпиляции игровой логики. Это один из самых развитых и детализированных языков описания во всём движке — он охватывает не только внешний вид частиц, но и их физику, звук, взаимодействие с окружением и даже порождение источников света.

## Интерфейс настройки

Эффекты описываются в текстовых конфигурационных файлах и подключаются командой **`r_part`** (первое описание в цепочке эффекта) — дополнительные слои того же эффекта добавляются последующими описаниями с именем, начинающимся с `+` (описание без `+` сбрасывает цепочку и начинает новый эффект):

```
r_part flame_puff
{
    texture textures/particles/smoke.tga
    tcoords 0 0 64 64 64
    scale 20
    scalefactor 1
    die 2
    alpha 0.5
    rgb 255 128 0
    rgbdelta 0 0 0
    spawnmode ball
    count 10
    type smoke
}
r_part +flame_puff
{
    texture textures/particles/spark.tga
    type spark
    count 4
}
```

Каждый эффект имеет собственное уникальное имя, на которое ссылается игровая логика при его вызове (например, при выстреле оружия или ударе о поверхность) — таким образом, автор мода может полностью переопределить визуальный эффект, просто прописав в конфигурационном файле новое описание с тем же именем, что уже используется игровой логикой, не трогая саму логику. Если эффект загружен из файла командой `r_particledesc`, ему присваивается внутренний префикс по имени файла (например, эффект `bar` из файла `foo.cfg` становится доступен как `foo.bar`), а игровая логика, явно обращающаяся по имени `foo.bar`, автоматически подгружает нужный файл `foo.cfg`.

### Изображение и раскадровка текстуры

| Ключевое слово | Синтаксис | Описание |
| :--- | :--- | :--- |
| [`texture`](../41-particle-directives-reference/01-particle-effect-directives.md#texture) | `<имя>` | Какое изображение использовать для частицы |
| [`shader`](../41-particle-directives-reference/01-particle-effect-directives.md#shader) | `<имя> [{ ... }]` | Использовать вместо простой текстуры полноценный материал (см. [«Язык описания материалов»](../08-materials-shaders/shader-script-language.md)); если сразу же в фигурных скобках указано тело материала, оно будет использовано для создания такого материала, если он ещё не существует нигде — при этом настройки смешивания (`blend`) в самом эффекте частиц теряют смысл, так как их задаёт материал |
| [`tcoords`](../41-particle-directives-reference/01-particle-effect-directives.md#tcoords) | `<s1> <t1> <s2> <t2> [tscale] [rsmax] [rsstep]` | Выбор конкретного прямоугольного участка текстуры (например, при хранении нескольких мелких картинок частиц на одном общем изображении — «атласе»). Если указан `rsmax`, каждая частица будет случайно брать одну из нескольких картинок, расположенных в один ряд на текстуре (с шагом `rsstep`) |
| [`atlas`](../41-particle-directives-reference/01-particle-effect-directives.md#atlas) | `<кол-во_в_ряд> <первый_индекс> [последний]` | Альтернативный, более простой способ задать «атлас»: текстура делится на равномерную сетку заданного размера, и для частицы случайно выбирается одна из картинок в указанном диапазоне индексов |

### Размер и вращение

| Ключевое слово | Синтаксис | Описание |
| :--- | :--- | :--- |
| `scale` | `<мин> [макс]` | Начальный диаметр частицы в игровых единицах (случайно выбирается в этом диапазоне) |
| [`scalefactor`](../41-particle-directives-reference/01-particle-effect-directives.md#scalefactor) | `<доля>` | Насколько размер частицы меняется с расстоянием до камеры (1 — как обычный объект, 0 — размер не зависит от удалённости) |
| [`scaledelta`](../41-particle-directives-reference/01-particle-effect-directives.md#scaledelta) | `<значение>` | Как меняется размер частицы со временем (изменение диаметра в секунду) |
| [`stretchfactor`](../41-particle-directives-reference/01-particle-effect-directives.md#stretchfactor) | `<множитель>` | Насколько частицы типа «искра» вытягиваются по направлению своей скорости (отрицательные значения дают искры фиксированной длины) |
| [`rotation`](../41-particle-directives-reference/01-particle-effect-directives.md#rotation) | `<мин_старт> <макс_старт> <мин_скорость> <макс_скорость>` | Начальный угол поворота частицы и скорость дальнейшего вращения (не стоит использовать на частицах-«лучах») |

### Цвет и прозрачность

| Ключевое слово | Синтаксис | Описание |
| :--- | :--- | :--- |
| `alpha` / [`alpharand`](../41-particle-directives-reference/01-particle-effect-directives.md#alpharand) / [`alphadelta`](../41-particle-directives-reference/01-particle-effect-directives.md#alphadelta) | `<значение>` | Начальная прозрачность, случайная добавка к ней, и скорость изменения прозрачности со временем (вычитается) |
| `red` / [`green`](../40-shader-directives-reference/02-shader-stage-directives.md#green) / `blue` | `<значение>` | Начальный цвет частицы по отдельному каналу (255 и выше — полностью непрозрачный/яркий канал) |
| [`rgb`](../41-particle-directives-reference/01-particle-effect-directives.md#rgb) | `<r> <g> <b>` (или одно число) | Начальный цвет частицы по всем каналам сразу, шкала 0–255 |
| `rgbf` | `<r> <g> <b>` | Та же запись, что и `rgb`, но в долях от 1 вместо шкалы 0–255 |
| `redrand` / `greenrand` / `bluerand`, [`rgbrand`](../41-particle-directives-reference/01-particle-effect-directives.md#rgbrand) | значение(-я) | Случайная прибавка к каждому цветовому каналу независимо друг от друга |
| `redrandsync` / `greenrandsync` / `bluerandsync`, [`rgbrandsync`](../41-particle-directives-reference/01-particle-effect-directives.md#rgbrandsync) | значение(-я) | То же самое, но случайное значение общее и синхронное для всех каналов сразу (не независимое) |
| `reddelta` / `greendelta` / `bluedelta`, [`rgbdelta`](../41-particle-directives-reference/01-particle-effect-directives.md#rgbdelta) | значение(-я) | Как цвет меняется со временем (значение 255 означает переход из полностью непрозрачного в полностью прозрачный ровно за 1 секунду) |
| `rgbdeltatime` | `<значение>` | В течение скольких секунд происходит изменение цвета, заданное через `rgbdelta` |
| [`colorindex`](../41-particle-directives-reference/01-particle-effect-directives.md#colorindex) | `<индекс> [разброс]` | Задаёт цвет через индекс классической цветовой палитры Quake вместо чисел RGB (удобно для точного воспроизведения классических цветовых эффектов) |
| `citracer` | без аргументов | Дополнительно раскрашивает частицы «трассера» по индексу их позиции в цепочке |
| [`rampmode`](../41-particle-directives-reference/01-particle-effect-directives.md#rampmode) | `<режим>` | Режим цветовой рампы — продвинутого способа анимировать сразу и цвет, и размер частицы по заранее заданной последовательности «ключевых кадров». Режимы: `none` (обычные формулы rgb/delta), `nearest`/`absolute` (резкая смена ключевых значений по возрасту частицы), `lerp` (плавная интерполяция между соседними ключевыми значениями), `delta` (использует рампу только для скорости изменения, остальное — как обычно) |
| [`ramp`](../41-particle-directives-reference/01-particle-effect-directives.md#ramp) / [`rampindex`](../41-particle-directives-reference/01-particle-effect-directives.md#rampindex) / `rampindexlist` | список значений | Задаёт саму последовательность ключевых кадров цветовой рампы (значения цвета/размера и индексы палитры соответственно) |

### Физика движения

| Ключевое слово | Синтаксис | Описание |
| :--- | :--- | :--- |
| `gravity` | `<значение>` | Насколько скорость частицы меняется под гравитацией за секунду |
| [`friction`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#friction) | `<xyz \| xy z \| x y z>` | Какая доля скорости теряется из-за трения (можно указать отдельно для горизонтали/вертикали или по каждой оси) |
| [`randomvel`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#randomvel) | `<гориз> [верт]` | Случайная начальная скорость частицы при появлении |
| [`veladd`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#veladd) | `<значение>` | Какая доля скорости эффекта передаётся частице (может быть больше 1 или отрицательной) |
| [`orgadd`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#orgadd) / [`orgbias`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#orgbias) / [`velbias`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#velbias) / [`orgwrand`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#orgwrand) / [`velwrand`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#velwrand) | значение(-я) | Различные способы сдвинуть или рандомизировать начальное положение и скорость частицы в мировых координатах |
| [`clipbounce`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#clipbounce) | `<значение>` (по умолчанию `0.8`) | Насколько сильно гасится скорость частицы при отскоке от столкновения с миром |
| [`cliptype`](../41-particle-directives-reference/01-particle-effect-directives.md#cliptype) | `<имя_эффекта>` | Эффект, порождаемый при ударе о поверхность (или сама частица просто отскакивает, если указано её собственное имя) |
| [`clipcount`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#clipcount) | `<число>` | Множитель числа частиц, порождаемых при ударе |

### Порождение и время жизни

| Ключевое слово | Синтаксис | Описание |
| :--- | :--- | :--- |
| `die` | `<макс_возраст> [мин_возраст]` | Сколько живёт частица (случайно в этом диапазоне) |
| `count` | `<мин> [макс]` | Сколько частиц спавнить за раз (только для точечных/объёмных эффектов, не для трасс и лучей) |
| `step` | `<мин> <макс>` | Расстояние между соседними частицами вдоль траектории (только для трасс/лучей) |
| [`spawnmode`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#spawnmode) | `<режим> [аргументы]` | Форма области появления частиц. Для точечных эффектов: `box` (равномерный параллелепипед, по умолчанию), `circle` (кольцо на равном расстоянии от центра), `ball` (случайно внутри шара), `telebox` (классический эффект телепорта Quake), `lavasplash` (брызги лавы, как у босса Chthon), `uniformcircle` (равномерное кольцо), `syncfield` (детерминированные позиции по времени). Для трасс: `spiral` (частицы получают скорость перпендикулярно направлению движения), `tracer` (чередующиеся боковые скорости — эффект «кильватерного следа») |
| [`assoc`](../41-particle-directives-reference/01-particle-effect-directives.md#assoc) | `<имя_эффекта>` | Дополнительный эффект, порождаемый одновременно с этим (позволяет комбинировать несколько независимых наборов частиц под одним именем) |
| [`notunderwater`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#notunderwater) / [`underwater`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#underwater) | `[список сред]` | Ограничивает появление частиц только вне воды или только под водой (список сред: `water`, `slime`, `lava`, `sky`, `solid`, `fluid`) |
| [`emit`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#emit) | `<имя_эффекта>` | Превращает частицу в источник периодического порождения другого эффекта (например, тлеющий уголёк, который время от времени выпускает искру) |
| [`emitinterval`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#emitinterval) / [`emitintervalrand`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#emitintervalrand) / [`emitstart`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#emitstart) | значение (секунды) | Интервал между порождениями (`emitinterval`), случайная добавка к интервалу (`emitintervalrand`) и задержка первого порождения (`emitstart`) |

### Звук

| Ключевое слово | Синтаксис | Описание |
| :--- | :--- | :--- |
| [`sound`](../37-quakec-builtins-reference/05-sound-builtins.md#sound) (простая форма) | `<имя> <громкость> <затухание> [высота_тона] [задержка]` | При появлении эффекта проигрывается указанный звук в точке его центра |
| `sound` (расширенная форма) | `<имя> [vol=][attn=][pitch=][delay=][weight=]` | Позволяет перечислить сразу несколько возможных звуков с разными «весами» — при срабатывании эффекта случайно выбирается один звук из списка согласно весам |

### Динамический источник света от эффекта

| Ключевое слово | Синтаксис | Описание |
| :--- | :--- | :--- |
| [`lightradius`](../41-particle-directives-reference/01-particle-effect-directives.md#lightradius) | `<радиус>` | Эффект порождает временный источник света (см. [«Динамическое освещение от игровых эффектов»](../09-lighting-shadows/dynamic-light-from-effects.md)), исчезающий, когда радиус падает до нуля или истекает время жизни |
| [`lightradiusfade`](../41-particle-directives-reference/01-particle-effect-directives.md#lightradiusfade) | `<радиус/сек>` | Как быстро уменьшается радиус этого света со временем |
| [`lightrgb`](../41-particle-directives-reference/01-particle-effect-directives.md#lightrgb) | `<r> <g> <b>` | Цвет источника света (1 = белый, значения выше могут пересвечивать сцену) |
| `lightrgbfade` | `<r/с> <g/с> <b/с>` | Скорость изменения цвета источника света со временем |
| [`lighttime`](../41-particle-directives-reference/01-particle-effect-directives.md#lighttime) | `<макс_возраст>` | Максимальное время жизни источника света |
| `lightcubemap` | `<номер>` | Использовать кубическую карту окружения для источника света (файлы вида `cubemaps/5ft.tga`, `cubemaps/5bk.tga` и т.д.) |
| `lightscales` | `<ambient> <diffuse> <specular>` | Множители для разных составляющих освещения от этого источника |
| `lightshadows` | `<0\|1>` | Отбрасывает ли этот временный свет тени (без теней — быстрее) |
| [`lightcorona`](../41-particle-directives-reference/01-particle-effect-directives.md#lightcorona) | `<интенсивность> <масштаб>` | Добавляет к источнику света видимый блик-«корону» |

### Пятна на поверхностях

| Ключевое слово | Синтаксис | Описание |
| :--- | :--- | :--- |
| [`stains`](../41-particle-directives-reference/01-particle-effect-directives.md#stains) | `<значение>` | Оставляет цветное пятно-загрязнение на поверхности при ударе частицы, цвет пятна берётся из цвета самой частицы (подробнее в статье [«Пятна и следы на поверхностях»](./surface-stains.md)) |
| [`spawnstain`](../41-particle-directives-reference/01-particle-effect-directives.md#spawnstain) | `<радиус> <r> <g> <b>` | То же самое, но с явно заданным цветом пятна, не зависящим от цвета частицы |

### Тип отображения частицы (`type`)

Явное указание типа частицы настоятельно рекомендуется — без него движок пытается угадать тип по текстуре и другим признакам, что не всегда даёт ожидаемый результат.

| Значение `type` | Описание |
| :--- | :--- |
| `normal` (по умолчанию) | Обычное вращающееся плоское изображение |
| `spark` | Частица рисуется как линия, длина которой зависит от скорости |
| `sparkfan` | Похоже на `spark`, но веерная неплоская форма |
| `texturedspark` | Текстурированная искра, вытянутая по направлению движения, ширина равна размеру частицы |
| `beam` | Только для трасс — частицы образуют один цельный текстурированный луч, используя узлы вдоль него, настраивается через [`beamtexstep`](../41-particle-directives-reference/01-particle-effect-directives.md#beamtexstep)/[`beamtexspeed`](../41-particle-directives-reference/01-particle-effect-directives.md#beamtexspeed) |
| `decal` / `cdecal` | Наклейка, появляющаяся только на геометрии карты и обрезаемая по её границам |
| `udecal` | Необрезаемая наклейка |

Дополнительно директива `clippeddecal <маска> [совпадение]` ограничивает появление декалей только поверхностями с подходящими флагами.

### Порождение моделей/спрайтов вместо частиц

| Ключевое слово | Синтаксис | Описание |
| :--- | :--- | :--- |
| [`model`](../39-entity-keys-reference/01-worldspawn-common-keys.md#model) | `<имя> [опции]` | Вместо простой частицы порождает разлетающийся во все стороны объект (модель или спрайт), проигрывающий последовательность кадров анимации — удобно для простых эффектов разлетающихся кусков (гибсов) |
| опции `model`: `frame=`/`framestart=`/`framecount=`/`frameend=`/`frames=`/`framerate=` | значения | Какие кадры анимации проигрывать и с какой скоростью |
| опции `model`: `skin=` | номер | Номер скина модели |
| опции `model`: `alpha=`, `scalemin=`/`scalemax=` | значения | Прозрачность и диапазон масштаба порождённого объекта |
| опции `model`: `trail=` | имя эффекта | Оставляет ли объект за собой трассу частиц |
| опции `model`: `orient`, `additive`/`transparent`/`fullbright`, `shadow`/`noshadow` | флаги | Ориентировать по направлению полёта; особые режимы отрисовки (аддитивный/прозрачный/самосветящийся); отбрасывание тени |

### Разное

| Ключевое слово | Синтаксис | Описание |
| :--- | :--- | :--- |
| [`viewspace`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#viewspace) | `[доля]` | Частица двигается относительно камеры игрока, а не относительно мира (не поддерживает разделение экрана, не стоит совмещать со столкновениями) |
| [`perframe`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#perframe) | без аргументов | Учитывает время кадра при подсчёте количества спавнимых частиц (для эффектов, испускающих частицы каждый кадр) |
| [`averageout`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#averageout) | без аргументов | Усредняет точки трассы от начала до конца (полезно для эффектов вроде молнии) |
| [`nostate`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#nostate) | без аргументов | Заставляет систему частиц игнорировать любую сохранённую информацию о состоянии эффекта |
| [`nospreadfirst`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#nospreadfirst) / [`nospreadlast`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#nospreadlast) | без аргументов | Не применять случайный разброс положения/скорости к первой/последней порождаемой частице в серии |
| [`rainfrequency`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#rainfrequency) | `<множитель>` | Интервал между новыми всплесками частиц на поверхности (эффект дождя) |
| [`flurry`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#flurry) | `<величина>` | Периодически меняет направление всех частиц эффекта, приблизительно имитируя порывы метели/снежной круговерти |
| `blend` | `<режим>` | Режим смешивания частицы с фоном, если не используется полноценный материал через `shader` (доступны как обычные, так и «премультиплицированные» режимы смешивания — рекомендуется использовать именно премультиплицированные режимы вместе с атласами текстур для лучшего визуального качества) |

## Практический пример: полный многослойный эффект взрыва гранаты

```
r_part grenade_explosion
{
	texture textures/particles/explosion_flash.tga
	type normal
	scale 40 60
	scalefactor 0
	scaledelta 80
	die 0.25
	alpha 1
	alphadelta 4
	rgb 255 200 120
	blend add
	count 1
	lightradius 350
	lightradiusfade 900
	lightrgb 1.6 1.1 0.5
	lighttime 0.3
	lightshadows 0
	sound weapons/explode.wav 1 0 0 0
}
r_part +grenade_explosion
{
	texture textures/particles/smoke_puff.tga
	type normal
	scale 15 25
	scalefactor 1
	scaledelta 30
	die 3 2
	alpha 0.6
	alpharand 0.2
	alphadelta 0.3
	rgb 60 60 60
	rgbrand 20 20 20
	gravity -20
	friction 0.4
	randomvel 60 40
	rotation 0 360 -30 30
	count 12
	spawnmode ball
}
r_part +grenade_explosion
{
	texture textures/particles/spark.tga
	type spark
	stretchfactor 0.08
	scale 4 8
	die 0.6 0.4
	rgb 255 180 60
	rgbdelta 0 60 60
	rgbdeltatime 0.6
	gravity 400
	clipbounce 0.4
	cliptype spark_hit
	randomvel 500 350
	count 20
	spawnmode ball
}
```

Построчный разбор: первый блок `r_part grenade_explosion` (без `+`) начинает совершенно новую цепочку эффекта и описывает саму яркую вспышку взрыва — она быстро (`scaledelta 80` — рост диаметра на 80 единиц в секунду) раздувается из небольшого пятна (`scale 40 60`) в большое яркое пятно, при этом `scalefactor 0` означает, что размер вспышки не зависит от расстояния до камеры (важно для больших ярких эффектов, которые не должны «сжиматься» на экране при удалении). Вспышка живёт всего четверть секунды (`die 0.25`) и быстро гаснет (`alphadelta 4` — теряет всю непрозрачность за четверть секунды), рисуется аддитивно (`blend add` — складывается с фоном, ярко высвечивая всё вокруг). Тот же блок одновременно порождает временный источник света радиусом 350 единиц (`lightradius`), быстро угасающий (`lightradiusfade 900` — теряет 900 единиц радиуса в секунду, то есть фактически гаснет почти сразу после вспышки), тёплого оранжевого оттенка (`lightrgb 1.6 1.1 0.5` — значения выше 1 дают пересвет), без теней (`lightshadows 0` — экономия производительности для короткой вспышки) и со звуком взрыва, проигрываемым один раз в полную громкость без затухания по расстоянию (`sound weapons/explode.wav 1 0 0 0`). Второй блок `r_part +grenade_explosion` (с `+`, продолжение цепочки) добавляет клубы дыма: 12 частиц (`count 12`), появляющихся внутри сферы (`spawnmode ball`), медленно поднимающихся вверх за счёт отрицательной гравитации (`gravity -20`), теряющих скорость из-за трения воздуха (`friction 0.4`), с разным серым оттенком у каждой частицы (`rgb 60 60 60` + `rgbrand 20 20 20` — случайный разброс делает дым не однородным), медленно вращающихся (`rotation 0 360 -30 30` — случайный начальный угол и небольшая случайная скорость вращения) и живущих от 2 до 3 секунд. Третий блок добавляет разлетающиеся искры: они рисуются как вытянутые по направлению движения линии (`type spark`, `stretchfactor 0.08`), падают под сильной гравитацией (`gravity 400`), отскакивают от поверхностей с потерей 60% скорости (`clipbounce 0.4`) и при ударе порождают отдельный эффект искры от удара (`cliptype spark_hit`), при этом их цвет по мере затухания смещается от ярко-оранжевого к более тёмному (`rgbdelta 0 60 60` за `rgbdeltatime 0.6` секунды).

## Примеры

- Эффект выстрела дробовика описывается как комбинация вспышки света (`lightradius`), короткого облака дыма (`type smoke`) и нескольких разлетающихся искр (`type spark`) — три слоя одного эффекта, описанные подряд через `r_part`/`r_part +`.
- Мод полностью переопределяет визуальный стиль всех эффектов крови, заменяя реалистичные красные частицы на стилизованные звёздочки, — не трогая ни одной строчки игровой логики, только переопределяя имена уже существующих эффектов в собственном конфигурационном файле.
- Эффект тлеющего фитиля бомбы использует `emit`/`emitinterval`, чтобы периодически испускать одиночные искры, и `lightradius`/`lightrgb`, чтобы отбрасывать мерцающий оранжевый свет на окружение.
- Эффект молнии реализован как `type beam` с `averageout` для сглаживания точек трассы и `beamtexspeed` для эффекта бегущей по лучу текстуры.

## Смежные страницы

- [Настройки качества частиц](./particle-quality-presets.md)
- [Пятна и следы на поверхностях](./surface-stains.md)
- [Язык описания материалов (в стиле Quake III, .shader)](../08-materials-shaders/shader-script-language.md)
- [Динамическое освещение от игровых эффектов](../09-lighting-shadows/dynamic-light-from-effects.md)
