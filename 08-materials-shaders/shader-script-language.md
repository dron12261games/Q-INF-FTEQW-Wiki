# Язык описания материалов (в стиле Quake III, .shader)

> [⬅ Предыдущая страница](../07-fonts-text/text-postprocessing.md) | [Следующая страница ➡](multilayer-transparent-materials.md)

> [⬅ Вернуться к оглавлению вики](../README.md)

## Что это даёт геймдизайнеру

Обычно текстура — это просто картинка. Но игровому материалу часто нужно намного больше: он должен светиться, менять цвет со временем, состоять из нескольких прозрачных слоёв, скроллить текстуру воды или мерцать. **Язык описания материалов** — это текстовый формат, унаследованный из Quake III Arena, который позволяет описать, как именно текстура должна вести себя на экране, не трогая саму картинку и не требуя от геймдизайнера ничего, кроме текстового редактора. Это главный, наиболее гибкий инструмент управления внешним видом поверхностей в игре.

---

## Интерфейс настройки

Материалы описываются в текстовых файлах с расширением **`.shader`**, которые кладутся в подпапку **`scripts/`** игровой папки — движок автоматически подгружает все файлы `scripts/*.shader` при запуске. Один файл может содержать описания сразу для многих текстур.

Общая структура одного материала:

```
texture_name/material
{
    surfaceparm nolightmap   // общие свойства всей поверхности
    cull none                // как отбрасывать невидимые стороны
    sort additive            // порядок отрисовки относительно других материалов

    {                        // это один "слой" (проход) материала
        map textures/foo.tga
        blendfunc add
        rgbGen identity
    }
}
```

Имя материала — это обычно тот же путь, что указывается на текстуру в игровой логике или на карте; если материал с таким именем описан в `.shader`-файле, движок применяет все его настройки поверх (или вместо) обычной текстуры с таким именем. Если подходящего материала не найдено, текстура используется как обычная плоская картинка без особых эффектов.

Материал может состоять из нескольких слоёв («проходов») — например, базовая текстура, поверх которой рисуется полупрозрачный блеск, а поверх него — светящаяся деталь; каждый слой настраивается отдельно и подробно описан в статьях этого раздела (смешивание слоёв, формулы цвета, анимация координат, деформация геометрии).

### Ключевые слова уровня всего материала (пишутся вне фигурных скобок слоя)

- **[`cull none`](../40-shader-directives-reference/01-shader-toplevel-directives.md#cull) / [`cull front`](../40-shader-directives-reference/01-shader-toplevel-directives.md#cull) / [`cull back`](../40-shader-directives-reference/01-shader-toplevel-directives.md#cull)** — управляет отбрасыванием невидимых («обратных») сторон поверхности: `none` (он же `disable`/`twosided`) рисует поверхность с обеих сторон (полезно для листвы, флагов, тонких плоскостей), `back` рисует только заднюю сторону, по умолчанию рисуется только «лицевая» сторона.
`decal`/`litdecal` (наклейки на поверхности), `seethrough`, `banner`, [`underwater`](../41-particle-directives-reference/02-particle-spawn-behaviour-directives.md#underwater) (подводные эффекты и туман), `blend`
- **[`surfaceparm <param>`](../40-shader-directives-reference/01-shader-toplevel-directives.md#surfaceparm)** — присваивает поверхности одно из специальных общих свойств: например, [`nolightmap`](../40-shader-directives-reference/02-shader-stage-directives.md#nolightmap) (не использовать запечённое освещение карты для этой поверхности), а также другие параметры, распознаваемые генератором карт и физикой поверхности (см. смежные страницы о свойствах поверхностей и коллизий).
- **[`nomipmaps`](../40-shader-directives-reference/01-shader-toplevel-directives.md#nomipmaps)** — отключает генерацию уменьшенных копий текстуры (мип-уровней), из-за чего текстура не будет смягчаться при отдалении камеры — полезно для чётких интерфейсных элементов или пиксель-арта.
- **[`nopicmip`](../40-shader-directives-reference/01-shader-toplevel-directives.md#nopicmip)** — запрещает автоматическое понижение качества текстуры настройками производительности (`picmip`), гарантируя, что материал всегда рисуется в полном разрешении.
- **[`polygonoffset`](../40-shader-directives-reference/01-shader-toplevel-directives.md#polygonoffset)** — слегка сдвигает поверхность материала «к камере» в системе отрисовки, чтобы предотвратить мерцание («z-fighting») с другой поверхностью, находящейся точно на том же месте (например, декаль поверх стены).
- **[`deformvertexes ...`](../40-shader-directives-reference/01-shader-toplevel-directives.md#deformvertexes)** — деформирует геометрию поверхности по формуле (волны, автоспрайты и т.д.) — подробно описано на отдельной странице [«Деформация геометрии по формулам»](./vertex-deformation.md).
- **[`portal`](../40-shader-directives-reference/01-shader-toplevel-directives.md#portal)** — помечает поверхность как портал (то, что видно сквозь неё, рассчитывается отдельным проходом рендера — например, зеркало или окно в другую часть уровня).
- **[`skyparms ...`](../40-shader-directives-reference/01-shader-toplevel-directives.md#skyparms)** / **[`fogparms ...`](../40-shader-directives-reference/01-shader-toplevel-directives.md#fogparms)** — настройка неба и тумана для материала, см. [«Небо, вода и туман»](../README.md#небо-вода-туман).
- **[`entitymergable`](../40-shader-directives-reference/01-shader-toplevel-directives.md#entitymergable)** — разрешает движку объединять («сливать») в одну операцию отрисовки несколько объектов, использующих этот материал, ради производительности.
- **[`program <name>`](../40-shader-directives-reference/01-shader-toplevel-directives.md#program) / [`glslprogram <name>`](../40-shader-directives-reference/01-shader-toplevel-directives.md#glslprogram) / [`hlslprogram <name>`](../40-shader-directives-reference/01-shader-toplevel-directives.md#hlslprogram)** — подключает к материалу собственную шейдерную программу, написанную на языке GLSL или HLSL, вместо использования встроенных формул движка — см. [«Собственные графические шейдеры (GLSL/HLSL)»](../README.md#экран-vr-и-выбор-рендерера).

### Дополнительные ключевые слова уровня материала для PBR и продвинутых карт текстур

Движок поддерживает набор «умных» ключевых слов, которые сами создают нужный внутренний слой материала под указанную карту текстуры, что удобнее, чем расписывать слой вручную: **[`diffusemap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#diffusemap)** (базовый цвет), **[`normalmap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#normalmap)** (карта нормалей рельефа), **[`specularmap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#specularmap)** (карта блеска), **[`fullbrightmap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#fullbrightmap)** (карта самосвечения), **[`uppermap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#uppermap)**/**[`lowermap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#lowermap)** (карты цветовых зон команды/скина, аналогично классическим top/bottom-цветам), **[`reflectmask`](../40-shader-directives-reference/01-shader-toplevel-directives.md#reflectmask)** (маска отражений), **[`displacementmap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#displacementmap)**/**[`transmissionmap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#transmissionmap)**/**[`thicknessmap`](../40-shader-directives-reference/01-shader-toplevel-directives.md#thicknessmap)** (карты для продвинутых материалов вроде полупрозрачных/просвечивающих поверхностей). Подробнее о физически корректном рендеринге — на странице [«PBR-материалы»](./pbr-materials.md).

### Ключевые слова внутри слоя (пишутся внутри фигурных скобок прохода)

- **[`map <path>`](../40-shader-directives-reference/02-shader-stage-directives.md#map)** — какая картинка используется как текстура этого слоя (либо специальное значение `$lightmap` для запечённой карты освещения, или `$whiteimage` для сплошной заливки).
- **[`animmap <frequency> <path1> <path2> ...`](../40-shader-directives-reference/02-shader-stage-directives.md#animmap)** — покадровая анимация текстуры слоя, переключение между несколькими картинками с заданной частотой в кадрах в секунду.
- **[`clampmap <path>`](../40-shader-directives-reference/02-shader-stage-directives.md#clampmap)** — то же самое, что [`map`](../40-shader-directives-reference/02-shader-stage-directives.md#map), но без повторения текстуры за границами координат (полезно для текстур, которые не должны «зацикливаться»).
- **[`videomap <path>`](../40-shader-directives-reference/02-shader-stage-directives.md#videomap)** — использует видеофайл как источник текстуры слоя (см. [«Видео и заставки»](../README.md#видео-и-катсцены)).
- **[`blendfunc ...`](../40-shader-directives-reference/02-shader-stage-directives.md#blendfunc)** — как этот слой смешивается с уже нарисованным изображением позади него. Есть именованные пресеты: [`add`](../40-shader-directives-reference/02-shader-stage-directives.md#add) (аддитивно, светлее фона), [`filter`](../44-cli-commands-reference/02-client-ui-commands.md#filter) (умножение, темнее фона), [`blend`](../40-shader-directives-reference/02-shader-stage-directives.md#blend) (обычная прозрачность по альфа-каналу), [`premul`](../40-shader-directives-reference/02-shader-stage-directives.md#premul) (прозрачность без эффекта «окантовки» на краях), [`replace`](../40-shader-directives-reference/02-shader-stage-directives.md#replace) (полностью перекрывает фон, без смешивания) — либо можно указать пару низкоуровневых констант смешивания напрямую для полного контроля.
- **[`rgbGen ...`](../40-shader-directives-reference/02-shader-stage-directives.md#rgbgen)** / **[`alphaGen ...`](../40-shader-directives-reference/02-shader-stage-directives.md#alphagen)** — формулы, определяющие цвет и прозрачность слоя (постоянный цвет, мерцание, затухание по расстоянию и т.д.) — полностью описаны на странице [«Формулы цвета и прозрачности»](./color-alpha-formulas.md).
- **[`tcMod ...`](../40-shader-directives-reference/02-shader-stage-directives.md#tcmod)** / **[`tcGen ...`](../40-shader-directives-reference/02-shader-stage-directives.md#tcgen)** (он же [`texgen`](../40-shader-directives-reference/02-shader-stage-directives.md#texgen)) — анимация и генерация текстурных координат слоя (скролл, вращение, волны, эффект окружения) — полностью описаны на странице [«Анимация текстурных координат»](./texcoord-animation.md).
- **[`depthFunc ...`](../40-shader-directives-reference/02-shader-stage-directives.md#depthfunc)** / **[`depthWrite`](../40-shader-directives-reference/02-shader-stage-directives.md#depthwrite)** — управление тестом глубины (проверкой, что рисуется поверх чего) для этого конкретного слоя — тонкая настройка для сложных многослойных эффектов.
- **[`alphaFunc`](../40-shader-directives-reference/02-shader-stage-directives.md#alphafunc)** — отбрасывание полностью прозрачных пикселей слоя (например, для текстур с «дырками», листвы или решёток) без включения полноценной прозрачности. Допустимые значения: **`gt0`** (отбросить пиксель, если прозрачность равна ровно нулю), **`lt128`** (отбросить пиксель, если прозрачность меньше половины — 128 из 255), **`ge128`** (отбросить пиксель, если прозрачность меньше половины, оставить только достаточно непрозрачные — обратная логика к `lt128`).
- **[`detail`](../40-shader-directives-reference/02-shader-stage-directives.md#detail)** — помечает слой как деталь-текстуру (дополнительный мелкий узор, добавляемый только при близком рассмотрении поверхности).
- **[`nodepthtest`](../40-shader-directives-reference/02-shader-stage-directives.md#nodepthtest)** / **[`nodepth`](../40-shader-directives-reference/02-shader-stage-directives.md#nodepth)** — отключает проверку глубины для слоя, из-за чего он рисуется поверх всего независимо от реального расстояния (используется для специальных эффектов вроде значков сквозь стены).

---

## Практический пример: полный материал текущей мутной воды

```
textures/liquids/murky_water
{
	qer_editorimage textures/liquids/murky_water_preview.tga
	surfaceparm water
	surfaceparm trans
	surfaceparm nonsolid
	cull disable
	deformVertexes wave 100 sin 0 3 0 0.1
	{
		map textures/liquids/murky_water_base.tga
		tcMod scroll 0.05 0.08
		tcMod turb 0 0.05 0 0.4
		blendfunc blend
		rgbGen identity
	}
	{
		map textures/liquids/murky_water_foam.tga
		blendfunc add
		tcMod scroll -0.02 0.03
		rgbGen wave sin 0.15 0.1 0 0.3
	}
}
```

Построчный разбор: `qer_editorimage` задаёт превью-картинку, которую видит редактор уровней при выборе текстуры (сам движок в игре её не использует). [`surfaceparm water`](../40-shader-directives-reference/01-shader-toplevel-directives.md#surfaceparm) помечает поверхность как воду для игровой логики (плавание, звуки погружения, эффект экрана под водой). [`surfaceparm trans`](../40-shader-directives-reference/01-shader-toplevel-directives.md#surfaceparm) сообщает генератору уровня, что поверхность полупрозрачна и требует особой обработки видимости. [`surfaceparm nonsolid`](../40-shader-directives-reference/01-shader-toplevel-directives.md#surfaceparm) убирает физическую коллизию с этой гранью — игрок проваливается сквозь поверхность воды, а не стоит на ней как на полу. [`cull disable`](../40-shader-directives-reference/01-shader-toplevel-directives.md#cull) рисует поверхность воды с обеих сторон, что важно, если камера может оказаться под водой и смотреть на неё снизу. [`deformVertexes wave`](../40-shader-directives-reference/01-shader-toplevel-directives.md#deformvertexes) `100` [`sin`](../37-quakec-builtins-reference/01-math-vector-builtins.md#sin) `0 3 0 0.1` слегка покачивает саму геометрию поверхности воды синусоидальной волной с пространственным периодом 100 единиц, амплитудой 3 единицы и частотой 0.1 колебания в секунду — вода выглядит как настоящая рябь, а не плоское стекло. Первый слой рисует базовую текстуру воды, которая одновременно медленно плывёт вбок ([`tcMod scroll`](../40-shader-directives-reference/02-shader-stage-directives.md#tcmod)) и «дрожит» волнообразным искажением координат ([`tcMod turb`](../40-shader-directives-reference/02-shader-stage-directives.md#tcmod)), после чего накладывается на нижележащую геометрию с обычной альфа-прозрачностью ([`blendfunc blend`](../40-shader-directives-reference/02-shader-stage-directives.md#blendfunc)). Второй слой аддитивно ([`blendfunc add`](../40-shader-directives-reference/02-shader-stage-directives.md#blendfunc)) добавляет текстуру пены, плывущую в противоположном направлении для визуального контраста, при этом её яркость плавно пульсирует по синусоиде — создавая эффект бликов солнца на неспокойной воде.

### Совместимость с диалектами других движков

Помимо основного набора ключевых слов из Quake III, движок распознаёт (в ограниченном объёме, в основном ради удобства портирования готового контента) отдельные ключевые слова из шейдерных языков других похожих движков — например, DarkPlaces (`camera`, `water`, [`reflect`](../40-shader-directives-reference/02-shader-stage-directives.md#reflect), `refract`, `offsetmapping`), Doom 3 ([`bumpmap`](../40-shader-directives-reference/02-shader-stage-directives.md#bumpmap), `translucent`), RTCW/Return to Castle Wolfenstein и Call of Duty. Ориентироваться на них стоит только при переносе уже готового чужого контента — для собственных материалов рекомендуется основной FTE/Q3-совместимый набор ключевых слов, описанный выше.

---

## Примеры

- Материал воды: базовый слой с текстурой, анимированной прокруткой координат ([`tcMod scroll`](../40-shader-directives-reference/02-shader-stage-directives.md#tcmod)), и полупрозрачным смешиванием ([`blendfunc blend`](../40-shader-directives-reference/02-shader-stage-directives.md#blendfunc)), создающим эффект мерцающей поверхности.
- Светящаяся вывеска: слой с текстурой вывески, поверх которого добавляется слой аддитивного ([`blendfunc add`](../40-shader-directives-reference/02-shader-stage-directives.md#blendfunc)) свечения того же изображения, отсортированный как [`sort additive`](../40-shader-directives-reference/01-shader-toplevel-directives.md#sort).
- Материал листвы дерева: [`cull none`](../40-shader-directives-reference/01-shader-toplevel-directives.md#cull) (видна с обеих сторон), [`alphaFunc`](../40-shader-directives-reference/02-shader-stage-directives.md#alphafunc) для вырезания прозрачных участков текстуры без полноценной прозрачности, [`deformvertexes wave`](../40-shader-directives-reference/01-shader-toplevel-directives.md#deformvertexes) для лёгкого покачивания на ветру.

---

## Смежные страницы

- [Многослойные и полупрозрачные материалы](./multilayer-transparent-materials.md)
- [Формулы цвета и прозрачности (rgbGen/alphaGen)](./color-alpha-formulas.md)
- [Анимация текстурных координат (вращение/скролл/волны)](./texcoord-animation.md)
- [Деформация геометрии по формулам (deformvertexes)](./vertex-deformation.md)
- [PBR-материалы (физически корректный рендеринг)](./pbr-materials.md)

> [⬅ Предыдущая страница](../07-fonts-text/text-postprocessing.md) | [Следующая страница ➡](multilayer-transparent-materials.md)

> [⬅ Вернуться к оглавлению вики](../README.md)