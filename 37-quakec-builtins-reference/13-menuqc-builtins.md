# Функции MenuQC (меню, экран загрузки)

> [⬅ Вернуться к оглавлению вики](../README.md)

> [Индекс справочника builtins](./README.md)

MenuQC — это клиентская QuakeC-VM для полноэкранных меню, загрузочных экранов, лаунчерных интерфейсов и встроенных браузерных страниц. На практике код menu.dat обычно живёт вокруг точек входа [`m_init`](00-entry-points.md#m_init), `m_draw(vector screensize)`, `m_toggle(float wantmode)` и `Menu_InputEvent`; legacy-хуки [`m_keydown`](00-entry-points.md#m_keydown)/[`m_keyup`](00-entry-points.md#m_keyup) тоже встречаются, но для новой логики ввода удобнее опираться именно на `Menu_InputEvent`.

Ниже собраны builtin-функции, которые в MenuQC используются для собственной 3D-сцены меню, 2D-рисования, работы с альтернативными консолями, курсором, привязками клавиш и встроенным веб-браузером. Важно учитывать исторические расхождения: часть имён существует одновременно в «старых» menu-слотах и в CSQC-совместимых alias-слотах, а несколько сценических/light-функций в текущих исходниках FTEQW выглядят build-dependent. Там, где `quakec\menusys\fteextensions.qc` и `engine\client\pr_menu.c` расходятся, это отдельно отмечено в описании.

## Рендеринг и сцена

### addentity
`void(entity ent) addentity = #302;`

* **ent** — entity, чьи поля рендера ([`model`](../39-entity-keys-reference/01-worldspawn-common-keys.md#model), [`origin`](../39-entity-keys-reference/01-worldspawn-common-keys.md#origin), [`angles`](../39-entity-keys-reference/01-worldspawn-common-keys.md#angles), `frame`, [`skin`](../39-entity-keys-reference/01-worldspawn-common-keys.md#skin), `colormap`, [`effects`](../39-entity-keys-reference/06-item-weapon-keys.md#effects), `alpha`, `scale`, `renderflags` и связанные с ними значения) нужно скопировать в сцену меню.

#### Описание и логика работы
[`addentity`](08-csqc-rendering-builtins.md#addentity) копирует текущее визуальное состояние обычной QuakeC-entity в список render-entity для следующего [`renderscene`](08-csqc-rendering-builtins.md#renderscene). Для MenuQC это основной способ нарисовать на фоне меню модель-логотип, вращающийся предмет, декоративную карту оружия или целую диораму за кнопками. Копирование происходит сразу: если вы хотите анимировать модель, изменяйте поля entity до вызова `addentity`, а не после него. Полезный практический шаблон — вызвать [`clearscene`](08-csqc-rendering-builtins.md#clearscene), настроить вид через `setproperty`, добавить несколько entity и только потом вызвать `renderscene`.

#### Практические сценарии использования
```qc
entity menu_logo;

void() m_init =
{
    menu_logo = spawn();
    setmodel(menu_logo, "progs/player.mdl");
    menu_logo.origin = '96 0 0';
    menu_logo.frame = 0;
};

void(vector screensize) m_draw =
{
    clearscene();
    menu_logo.angles_y = time * 45; // плавно крутим модель на фоне меню
    addentity(menu_logo);
    renderscene();
};
```

### addentities
`void(float mask) addentities = #301;`

* **mask** — битовая маска источников, которые нужно автоматически добавить в сцену.

#### Описание и логика работы
Для MenuQC с [`addentities`](08-csqc-rendering-builtins.md#addentities) есть важное расхождение. В справочном `fteextensions.qc` builtin `#301` сейчас объявлен только под `CSQC`, а в текущем `engine\client\pr_menu.c` строка регистрации для MenuQC закомментирована. Если ваш билд FTEQW всё же публикует `addentities` в menu.dat как часть общей клиентской VM, поведение совпадает с CSQC-вариантом: builtin проходит по entity-спискам и добавляет их в сцену по маске, вызывая `predraw`, если он определён. Для переносимого MenuQC-кода лучше считать `addentities` build-dependent и при жёстком требовании совместимости проверять наличие через `checkbuiltin`.

#### Практические сценарии использования
```qc
void(vector screensize) m_draw =
{
    clearscene();

    if (checkbuiltin(addentities))
        addentities(MASK_ENGINE); // если билд разрешает, рисуем мир/живые entity за меню

    renderscene();
};
```

### clearscene
`void() clearscene = #300;`

* Аргументов нет.

#### Описание и логика работы
`clearscene` забывает все render-entity, полигоны и временные динамические источники света, накопленные для текущей меню-сцены, а также сбрасывает свойства вида к значениям по умолчанию. Именно с неё обычно начинается [`m_draw`](00-entry-points.md#m_draw), если меню строит собственный 3D-фон или хотя бы 2D-полигоны через `R_BeginPolygon(..., TRUE)`. Если не очищать сцену каждый кадр, старые объекты будут продолжать жить и перерисовываться поверх новых.

#### Практические сценарии использования
```qc
void(vector screensize) m_draw =
{
    clearscene(); // всегда первым действием кадра меню
    renderscene();
};
```

### renderscene
`void() renderscene = #304;`

* Аргументов нет.

#### Описание и логика работы
`renderscene` рисует всё, что было накоплено после `clearscene`: menu-entity из `addentity`, build-dependent наборы из `addentities`, 3D-полигоны и временные lights/effects. В MenuQC её используют для живого фона главного меню, экранов настройки с моделью игрока, заставок и загрузочных экранов с отдельной камерой. Как и в CSQC, builtin читает текущие свойства вида из `setproperty`; вызывать его имеет смысл только после полной сборки кадра.

#### Практические сценарии использования
```qc
void(vector screensize) m_draw =
{
    clearscene();
    addentity(menu_logo);
    renderscene(); // только после этого 3D-фон действительно появится на экране
};
```

### getproperty
`__variant(float property) getproperty = #309;` (алиас `getviewprop`)

* **property** — константа `VF_*`, значение которой нужно прочитать.

#### Описание и логика работы
`getproperty` возвращает текущее значение свойства вида: размер viewport, origin камеры, углы, field of view и другие флаги из семейства `VF_*`. Для MenuQC здесь тоже есть расхождение исходников: `fteextensions.qc` объявляет builtin как общий для `CSQC` и `MENU`, но в текущем `pr_menu.c` его регистрация обёрнута `#ifdef CSQC_DAT`, то есть конкретный menu.dat-билд может не экспортировать его. Если builtin присутствует, он полезен для чтения уже применённого viewport или для проверки, не сбросил ли `clearscene` ваши настройки.

#### Практические сценарии использования
```qc
vector() Menu_CurrentViewport =
{
    local vector size;

    if (!checkbuiltin(getproperty))
        return '0 0 0';

    size = getproperty(VF_SIZE);
    return size;
};
```

### setproperty
`float(float property, ...) setproperty = #303;` (алиас `setviewprop`)

* **property** — одна из констант `VF_*`, определяющая, что именно вы меняете.
* **...** — дополнительные аргументы нужного типа (`float` или `vector`) для выбранного свойства.

#### Описание и логика работы
`setproperty` переопределяет параметры камеры и viewport для ближайшего `renderscene`: положение и углы камеры, поле зрения, область рендера, часть флагов отрисовки HUD/мира и связанные параметры. В документации MenuQC builtin числится как доступный, но в текущем `pr_menu.c` он тоже попадает под `#ifdef CSQC_DAT`, поэтому в конкретной сборке меню может отсутствовать. Если он есть, то это ключевой инструмент для 3D-фона меню: сначала `clearscene`, затем `setproperty(VF_MIN, ...)`, `setproperty(VF_SIZE, ...)`, `setproperty(VF_ORIGIN, ...)`, `setproperty(VF_ANGLES, ...)`, затем `addentity` и `renderscene`.

#### Практические сценарии использования
```qc
void(vector screensize) m_draw =
{
    clearscene();

    if (checkbuiltin(setproperty))
    {
        setproperty(VF_MIN, '0 0 0');
        setproperty(VF_SIZE, screensize);
        setproperty(VF_ORIGIN, '0 -160 36');
        setproperty(VF_ANGLES, '6 0 0');
        setproperty(VF_FOV, 75, 75);
    }

    addentity(menu_logo);
    renderscene();
};
```

### getresolution
`vector(float vidmode, optional float forfullscreen) getresolution = #608;`

* **vidmode** — индекс запрашиваемого видеорежима; `-1` в текущей реализации означает «текущий desktop mode».
* **forfullscreen** *(optional)* — ненулевое значение просит список полноэкранных режимов; ноль — оконные/внутренние режимы меню видео.

#### Описание и логика работы
[`getresolution`](08-csqc-rendering-builtins.md#getresolution) помогает строить собственное меню видеонастроек без парсинга текстового вывода движка. Возвращаемый вектор содержит ширину в `x`, высоту в `y`, а в `z` — признак валидности/соотношения пикселя (в текущей реализации это либо 0, либо 1, если режим найден). Для полноэкранных режимов builtin обращается к списку режимов движка, а для `vidmode = -1` берёт параметры рабочего стола. Если индекс не существует, возвращаются нули.

#### Практические сценарии использования
```qc
string(float mode) Menu_FormatResolution =
{
    local vector res;

    res = getresolution(mode, TRUE);
    if (res_x <= 0 || res_y <= 0)
        return "<нет режима>";

    return sprintf("%gx%g", res_x, res_y);
};
```

### R_BeginPolygon
`void(string texturename, optional float flags, optional float is2d) R_BeginPolygon = #306;`

* **texturename** — имя шейдера/картинки для следующих вершин.
* **flags** *(optional)* — дополнительные флаги полигона/материала.
* **is2d** *(optional)* — если ненуль, полигон трактуется как экранный и рисуется сразу после [`R_EndPolygon`](08-csqc-rendering-builtins.md#r_endpolygon).

#### Описание и логика работы
[`R_BeginPolygon`](08-csqc-rendering-builtins.md#r_beginpolygon) открывает построение произвольного полигона. В MenuQC это удобно в двух сценариях: 3D-декорации внутри собственной сцены меню и сложные 2D-подложки, рамки, диагональные панели или анимированные заливки поверх обычных `draw*`-вызовов. Когда `is2d` равен нулю, вершины живут в мировой системе координат и попадают в общий scene list для `renderscene`. Когда `is2d` ненулевой, вершины трактуются как экранные координаты меню и полигон выводится немедленно при `R_EndPolygon`.

#### Практические сценарии использования
```qc
void() Menu_DrawHeaderPlate =
{
    R_BeginPolygon("white", 0, TRUE);
    R_PolygonVertex('16 16 0', '0 0 0', '0.10 0.12 0.18', 0.90);
    R_PolygonVertex('304 16 0', '1 0 0', '0.10 0.12 0.18', 0.90);
    R_PolygonVertex('288 48 0', '1 1 0', '0.18 0.22 0.32', 0.90);
    R_PolygonVertex('16 48 0', '0 1 0', '0.18 0.22 0.32', 0.90);
    R_EndPolygon();
};
```

### R_EndPolygon
`void() R_EndPolygon = #308;`

* Аргументов нет.

#### Описание и логика работы
`R_EndPolygon` завершает текущий полигон, начатый `R_BeginPolygon`. Для корректной отрисовки должно быть задано минимум три вершины через [`R_PolygonVertex`](08-csqc-rendering-builtins.md#r_polygonvertex); иначе движок просто не нарисует фигуру. В MenuQC это типичный финальный шаг при ручной отрисовке декоративных панелей, 2D-треугольников-подсветок и 3D-плашек за пунктами меню.

#### Практические сценарии использования
```qc
void() Menu_DrawTriangle =
{
    R_BeginPolygon("white", 0, TRUE);
    R_PolygonVertex('24 96 0', '0 0 0', '1 0.6 0.2', 1);
    R_PolygonVertex('48 112 0', '1 0 0', '1 0.4 0.1', 1);
    R_PolygonVertex('24 128 0', '0 1 0', '1 0.6 0.2', 1);
    R_EndPolygon();
};
```

### R_PolygonVertex
`void(vector org, vector texcoords, vector rgb, float alpha) R_PolygonVertex = #307;`

* **org** — координаты вершины: мировые для 3D-полигона, экранные для `is2d`-полигона.
* **texcoords** — UV-координаты текстуры.
* **rgb** — цвет вершины.
* **alpha** — прозрачность вершины.

#### Описание и логика работы
`R_PolygonVertex` добавляет одну вершину в полигон, открытый `R_BeginPolygon`. Порядок вызовов задаёт обход вершин и итоговую веерную триангуляцию, поэтому для MenuQC обычно лучше добавлять точки последовательно по часовой или против часовой стрелки. Это один из немногих способов построить в меню фигуры сложнее прямоугольника без предварительно подготовленной текстуры.

#### Практические сценарии использования
```qc
void() Menu_DrawCursorGlow =
{
    local vector m;

    m = getmousepos();
    R_BeginPolygon("white", 0, TRUE);
    R_PolygonVertex(m + '-12 -12 0', '0 0 0', '0.2 0.5 1', 0.0);
    R_PolygonVertex(m + ' 12 -12 0', '1 0 0', '0.2 0.5 1', 0.6);
    R_PolygonVertex(m + ' 12  12 0', '1 1 0', '0.2 0.5 1', 0.0);
    R_PolygonVertex(m + '-12  12 0', '0 1 0', '0.2 0.5 1', 0.6);
    R_EndPolygon();
};
```

## 2D-отрисовка интерфейса

### drawcharacter
`float(vector position, float character, vector scale, vector rgb, float alpha, optional float flag) drawcharacter = #454;`

* **position** — экранная позиция символа в пикселях меню.
* **character** — код символа Quake либо, при `flag & 4`, unicode-код.
* **scale** — размер символа, обычно `'8 8 0'` или `'16 16 0'`.
* **rgb** — цвет символа.
* **alpha** — прозрачность.
* **flag** *(optional)* — режим смешивания и флаги интерпретации символа.

#### Описание и логика работы
[`drawcharacter`](08-csqc-rendering-builtins.md#drawcharacter) рисует один символ шрифта и хорошо подходит для стрелок выбора, одиночных иконок-глифов, индикаторов биндов и курсоров каретки. В MenuQC это более точечный инструмент, чем [`drawstring`](08-csqc-rendering-builtins.md#drawstring): вы полностью контролируете позицию каждого символа и можете анимировать их независимо. В текущей реализации вне диапазона стандартного ASCII движок либо использует Quake-глифы, либо ставит заглушку, если вы запросили unicode-режим.

#### Практические сценарии использования
```qc
void(float selected) Menu_DrawArrow =
{
    if (!selected)
        return;

    drawcharacter('24 80 0', '>', '16 16 0', '1 0.8 0.2', 1, 0); // стрелка у активного пункта
};
```

### drawfill
`float(vector position, vector size, vector rgb, float alpha, optional float flag) drawfill = #457;`

* **position** — верхний левый угол прямоугольника.
* **size** — ширина и высота.
* **rgb** — цвет заливки.
* **alpha** — прозрачность.
* **flag** *(optional)* — режим смешивания; младшие биты обычно выбирают обычный или additive-режим.

#### Описание и логика работы
[`drawfill`](08-csqc-rendering-builtins.md#drawfill) — базовая прямоугольная заливка для фона меню, затемнений, полос прогресса, активных вкладок и подложек под текст. Это чистый 2D builtin: он не зависит от сцены, камеры или `renderscene`, поэтому его можно вызывать в любой части `m_draw`, в том числе поверх уже нарисованного 3D-фона. При нулевой или отрицательной ширине/высоте прямоугольник просто не появится.

#### Практические сценарии использования
```qc
void(vector screensize) Menu_DrawDimmer =
{
    drawfill('0 0 0', screensize, '0 0 0', 0.65, 0); // затемняем игру под меню
};
```

### drawline
`void(float width, vector pos1, vector pos2) drawline = #466;`

* **width** — желаемая толщина линии.
* **pos1**, **pos2** — начальная и конечная точки линии.

#### Описание и логика работы
С [`drawline`](08-csqc-rendering-builtins.md#drawline) в MenuQC есть ещё одно историческое расхождение. В `fteextensions.qc` menu-объявление для слота `#466` оставляет только три аргумента, но текущая реализация FTEQW использует тот же внутренний helper, что и CSQC, и читает расширенные параметры [`rgb`](../41-particle-directives-reference/01-particle-effect-directives.md#rgb), `alpha` и `drawflag`. На практике это значит следующее: старое объявление отражает legacy-API, а современное FTE-поведение ближе к «расширенной» сигнатуре. Если вы пишете код только под FTEQW, обычно удобнее локально переобъявить builtin с цветом и прозрачностью; если же вам нужна буквальная совместимость со старым menu header, считайте эти дополнительные аргументы build-specific расширением. Отдельно важно, что текущая реализация игнорирует сам `width`, поэтому визуальная толщина линии определяется рендерером, а не переданным числом.

#### Практические сценарии использования
```qc
// Для FTEQW удобно переобъявить расширенную сигнатуру локально.
void(float width, vector pos1, vector pos2, vector rgb, float alpha, optional float drawflag) drawline = #466;

void() Menu_DrawSeparator =
{
    drawline(1, '24 72 0', '296 72 0', '0.45 0.60 1.00', 1, 0);
};
```

### drawpic
`float(vector position, string pic, vector size, vector rgb, float alpha, optional float flag) drawpic = #456;`

* **position** — позиция верхнего левого угла изображения.
* **pic** — имя шейдера/картинки.
* **size** — размер вывода.
* **rgb** — цветовой множитель.
* **alpha** — прозрачность.
* **flag** *(optional)* — режим смешивания и другие draw-флаги.

#### Описание и логика работы
[`drawpic`](08-csqc-rendering-builtins.md#drawpic) рисует изображение в заданном прямоугольнике меню и почти всегда используется для логотипов, иконок, кнопок, фоновых панелей и самих браузерных текстур от [`gecko_create`](09-csqc-input-ui-builtins.md#gecko_create). Картинка может быть заранее загружена через [`precache_pic`](07-precache-resources-builtins.md#precache_pic), но для вики-описания важнее практический факт: builtin безопасен для масштабирования как вверх, так и вниз, а цвет/alpha позволяют легко делать hover-эффекты без второй копии текстуры.

#### Практические сценарии использования
```qc
void() Menu_DrawLogo =
{
    drawpic('32 24 0', "gfx/menu/logo", '256 64 0', '1 1 1', 1, 0);
};
```

### drawrawstring
`float(vector position, string text, vector scale, vector rgb, float alpha, optional float flag) drawrawstring = #455;`

* **position** — экранная позиция начала строки.
* **text** — строка без интерпретации цветовой разметки.
* **scale** — размер шрифта.
* **rgb**, **alpha** — базовый цвет и прозрачность.
* **flag** *(optional)* — draw-флаги.

#### Описание и логика работы
[`drawrawstring`](08-csqc-rendering-builtins.md#drawrawstring) выводит текст «как есть», не разбирая `^1`, `^xRGB` и другие цветовые escape-последовательности движка. В MenuQC это удобно для имён файлов, URL-адресов, пользовательского ввода и любых текстов, где символ `^` должен остаться обычным символом, а не сменой цвета. Если глобально включён UTF-8, строка читается в UTF-8; иначе используются сырые Quake-символы.

#### Практические сценарии использования
```qc
void(string path) Menu_DrawSavePath =
{
    drawrawstring('32 128 0', path, '8 8 0', '0.85 0.85 0.85', 1, 0); // без разбора ^-кодов
};
```

### drawresetcliparea
`void(void) drawresetcliparea = #459;`

* Аргументов нет.

#### Описание и логика работы
[`drawresetcliparea`](08-csqc-rendering-builtins.md#drawresetcliparea) снимает ранее установленную область отсечения и возвращает 2D-рисование ко всему экрану. Для MenuQC это обязательная пара к [`drawsetcliparea`](08-csqc-rendering-builtins.md#drawsetcliparea), если вы рисуете скроллируемые списки серверов, журнал консоли, длинные описания модов или область встроенного браузера с отдельными границами.

#### Практические сценарии использования
```qc
void() Menu_DrawVisibleSlice =
{
    drawsetcliparea(32, 96, 256, 96);
    drawpic('32 96 0', "gfx/menu/long_panel", '256 256 0', '1 1 1', 1, 0);
    drawresetcliparea(); // дальше меню снова рисуется без клипа
};
```

### drawsetcliparea
`void(float x, float y, float width, float height) drawsetcliparea = #458;`

* **x**, **y** — верхний левый угол clip/scissor-области.
* **width**, **height** — размер видимой области.

#### Описание и логика работы
`drawsetcliparea` включает прямоугольное отсечение: все последующие 2D draw-вызовы и 2D-полигоны будут видимы только внутри указанного прямоугольника. Это особенно полезно в MenuQC, где списки, комбобоксы и виджеты браузера живут в собственных окнах. Клип действует до `drawresetcliparea`, поэтому лучше явно сбрасывать его в том же блоке кода, где он был установлен.

#### Практические сценарии использования
```qc
void() Menu_DrawScrollWindow =
{
    drawfill('32 96 0', '256 96 0', '0.06 0.08 0.12', 0.9, 0);
    drawsetcliparea(32, 96, 256, 96);
    drawstring('40 88 0', "^7Строка 1\n^7Строка 2\n^7Строка 3", '8 8 0', '1 1 1', 1, 0);
    drawresetcliparea();
};
```

### drawstring
`float(vector position, string text, vector scale, vector rgb, float alpha, float flag) drawstring = #467;`

* **position** — позиция начала строки.
* **text** — текст с поддержкой цветовой/форматной разметки движка.
* **scale** — размер символов.
* **rgb**, **alpha** — базовый цвет и прозрачность.
* **flag** — режим отрисовки.

#### Описание и логика работы
`drawstring` — основной текстовый builtin MenuQC: он рисует строку и интерпретирует цветовую разметку, позволяя внутри одного текста смешивать белый, серый, красный и любые другие цвета. В отличие от `drawrawstring`, эта функция подходит для пунктов меню, статусов загрузки, сообщений об ошибках и красивых подписей. Возвращаемое число обычно используют как измеренную ширину нарисованной строки, если нужно пристыковать следующий элемент справа.

#### Практические сценарии использования
```qc
void(float online) Menu_DrawStatus =
{
    if (online)
        drawstring('32 56 0', "^2Сеть: ^7подключено", '8 8 0', '1 1 1', 1, 0);
    else
        drawstring('32 56 0', "^1Сеть: ^7нет соединения", '8 8 0', '1 1 1', 1, 0);
};
```

### drawsubpic
`void(vector pos, vector sz, string pic, vector srcpos, vector srcsz, vector rgb, float alpha, float flag) drawsubpic = #469;`

* **pos**, **sz** — положение и размер результата на экране.
* **pic** — исходное изображение/атлас.
* **srcpos**, **srcsz** — прямоугольник внутри исходной картинки.
* **rgb**, **alpha** — цветовой множитель и прозрачность.
* **flag** — режим отрисовки.

#### Описание и логика работы
[`drawsubpic`](08-csqc-rendering-builtins.md#drawsubpic) вырезает прямоугольный фрагмент из текстурного атласа и растягивает его в указанный экранный прямоугольник. Для MenuQC это типичный путь рисовать наборы иконок, рамочные элементы из одного atlas-файла и анимированные кнопки без множества отдельных изображений. Хотя текущая реализация внутренне пишет возвращаемое значение, штатное menu-объявление считает builtin процедурой, поэтому переносимый код должен использовать её именно как `void`.

#### Практические сценарии использования
```qc
void(float frame) Menu_DrawWeaponIcon =
{
    local vector src;

    src = '0 0 0';
    src_x = frame * 32;
    drawsubpic('40 160 0', '32 32 0', "gfx/menu/icons", src, '32 32 0', '1 1 1', 1, 0);
};
```

### iscachedpic
`float(string name) iscachedpic = #451;`

* **name** — имя изображения.

#### Описание и логика работы
[`iscachedpic`](07-precache-resources-builtins.md#iscachedpic) проверяет, присутствует ли картинка в текущем графическом кэше движка. Для MenuQC это удобная подсказка перед тяжёлыми экранами с большими баннерами, галереями или набором превью-карт: можно один раз подгрузить ресурсы при открытии экрана и не делать это каждый кадр. Однако builtin нельзя считать абсолютной гарантией, потому что разные билды движка по-разному управляют кэшем между картами и видеорестартами.

#### Практические сценарии использования
```qc
void() Menu_EnsureBanner =
{
    if (!iscachedpic("gfx/menu/banner_big"))
        precache_pic("gfx/menu/banner_big");
};
```

### drawgetimagesize
`vector(string picname) drawgetimagesize = #460;`

* **picname** — имя картинки.

#### Описание и логика работы
[`drawgetimagesize`](07-precache-resources-builtins.md#drawgetimagesize) возвращает реальный размер изображения в пикселях. Это помогает MenuQC-коду аккуратно сохранять пропорции логотипа, центрировать баннер по настоящей ширине, вычислять UV-размеры для `drawsubpic` и на лету подстраивать hover-области. Для картинок, которые ещё не загружались, первый вызов может быть дороже обычного из-за обращения к ресурсной системе.

#### Практические сценарии использования
```qc
void() Menu_DrawNativeLogo =
{
    local vector size;

    size = drawgetimagesize("gfx/menu/logo");
    if (size_x <= 0 || size_y <= 0)
        return;

    drawpic('32 24 0', "gfx/menu/logo", size, '1 1 1', 1, 0);
};
```

### stringwidth
`float(string text, float usecolours, optional vector fontsize) stringwidth = #468;`

* **text** — строка, ширину которой нужно измерить.
* **usecolours** — ненулевое значение сохраняет/учитывает цветовую разметку; ноль измеряет «чистый» текст.
* **fontsize** *(optional)* — желаемый размер шрифта.

#### Описание и логика работы
[`stringwidth`](02-string-builtins.md#stringwidth) вычисляет итоговую ширину текста в экранных пикселях и нужна почти в любом приличном MenuQC: для центрирования заголовков, выравнивания колонок, правой кромки кнопок и расчёта hover-областей под строки списка. В отличие от грубого умножения «число символов × 8», builtin учитывает реальный механизм шрифта движка и его масштаб. Если цветовая разметка не должна влиять на измерение, передавайте `0` во втором аргументе.

#### Практические сценарии использования
```qc
void(vector screensize, string title) Menu_DrawCenteredTitle =
{
    local float w;
    local vector p;

    w = stringwidth(title, TRUE, '16 16 0');
    p = '0 0 0';
    p_x = (screensize_x - w) * 0.5;
    p_y = 24;
    drawstring(p, title, '16 16 0', '1 1 1', 1, 0);
};
```

## Консоль

### con_draw
`void(string conname, vector pos, vector size, float fontsize) con_draw = #393;`

* **conname** — имя альтернативной консоли.
* **pos**, **size** — позиция и размер области вывода.
* **fontsize** — размер шрифта.

#### Описание и логика работы
[`con_draw`](08-csqc-rendering-builtins.md#con_draw) рисует именованную альтернативную консоль прямо внутри интерфейса MenuQC. Это не основная drop-down консоль движка, а отдельный консольный объект из системы `FTE_CSQC_ALTCONSOLES`: вы можете использовать его как окно лога загрузки, чат-панель, отладочный терминал или внутренний shell лаунчера. Обычно builtin вызывают каждый кадр внутри `m_draw`, пока окно должно быть видно.

#### Практические сценарии использования
```qc
void() Menu_DrawConsolePane =
{
    drawfill('24 220 0', '272 96 0', '0 0 0', 0.75, 0);
    con_draw("chatlog", '32 228 0', '256 80 0', 8);
};
```

### con_getset
`string(string conname, string field, optional string newvalue) con_getset = #391;`

* **conname** — имя консоли.
* **field** — имя свойства (`title`, `name`, `next`, `unseen`, `markup`, `forceutf8`, `close`, `clear`, `hidden`, `linecount`).
* **newvalue** *(optional)* — новое значение свойства, если нужен set-режим.

#### Описание и логика работы
[`con_getset`](08-csqc-rendering-builtins.md#con_getset) — управляющий builtin для альтернативных консолей. Он либо читает текущее свойство, либо записывает новое значение, при этом возвращая предыдущее. В MenuQC это удобно для открытия/скрытия вспомогательных окон, очистки старого текста при смене вкладки и перебора всех существующих консолей через поле `next`.

#### Практические сценарии использования
```qc
void() Menu_ResetChatConsole =
{
    con_getset("chatlog", "title", "Лог меню");
    con_getset("chatlog", "clear", "1");
    con_getset("chatlog", "hidden", "0");
};
```

### con_input
`float(string conname, float inevtype, float parama, float paramb, float paramc) con_input = #394;`

* **conname** — имя консоли, которой нужно передать ввод.
* **inevtype** — тип события ввода.
* **parama**, **paramb**, **paramc** — параметры события, зависящие от его типа.

#### Описание и логика работы
[`con_input`](08-csqc-rendering-builtins.md#con_input) перенаправляет ввод из `Menu_InputEvent` во встроенную альтернативную консоль. Так можно сделать окно лога интерактивным: пока оно активно, клавиатура, вставка текста и абсолютные координаты мыши идут не в кнопки меню, а в консольный виджет. Для событий мыши документация прямо советует использовать абсолютные координаты, а не дельты.

#### Практические сценарии использования
```qc
float chat_console_active;

float(float evtype, float scanx, float chary, float devid) Menu_InputEvent =
{
    if (chat_console_active)
        return con_input("chatlog", evtype, scanx, chary, devid);
    return FALSE;
};
```

### con_printf
`void(string conname, string messagefmt, ...) con_printf = #392;`

* **conname** — имя консоли-получателя.
* **messagefmt**, **...** — форматная строка и аргументы.

#### Описание и логика работы
[`con_printf`](08-csqc-rendering-builtins.md#con_printf) печатает форматированный текст в именованную альтернативную консоль, а не в основную консоль движка. Это удобный способ журналировать действия пользователя внутри menu.dat: какие вкладки он открывал, какие URL загружал встроенный браузер, какую раскладку биндов вы активировали. Часто эта функция сочетается с `con_draw` и `con_getset`, образуя полноценную лог-панель внутри меню.

#### Практические сценарии использования
```qc
void(string page) Menu_LogOpenPage =
{
    con_printf("chatlog", "^2menu^7: opened %s\n", page);
};
```

## Динамический свет

### dynamiclight_add
`float(vector org, float radius, vector lightcolours, optional float style, optional string cubemapname, optional float pflags) dynamiclight_add = #305;`

* **org** — мировая позиция источника.
* **radius** — радиус света.
* **lightcolours** — цвет света.
* **style** *(optional)* — lightstyle-номер для мерцания.
* **cubemapname** *(optional)* — кубическая карта/проекционный материал.
* **pflags** *(optional)* — дополнительные флаги света.

#### Описание и логика работы
Для MenuQC builtin [`dynamiclight_add`](08-csqc-rendering-builtins.md#dynamiclight_add) также относится к спорным местам исходников. В `fteextensions.qc` он объявлен только для `CSQC`, а в текущем `pr_menu.c` регистрация закомментирована, хотя рядом оставлен комментарий, что builtin «should be okay to share». Если конкретный билд действительно экспонирует функцию в menu.dat, семантика такая же, как у CSQC: свет живёт только до следующего `clearscene`, добавляется между `clearscene` и `renderscene` и возвращает индекс для дальнейшего [`dynamiclight_set`](08-csqc-rendering-builtins.md#dynamiclight_set)/[`dynamiclight_get`](08-csqc-rendering-builtins.md#dynamiclight_get). Для декоративных меню это полезно при подсветке моделей, свечении курсора и прожекторных эффектах.

#### Практические сценарии использования
```qc
void(vector screensize) m_draw =
{
    local float lno;

    clearscene();
    addentity(menu_logo);

    if (checkbuiltin(dynamiclight_add))
    {
        lno = dynamiclight_add(menu_logo.origin + '0 0 24', 180, '0.3 0.6 1.0', 0);
    }

    renderscene();
};
```

### dynamiclight_get
`__variant(float lno, float fld) dynamiclight_get = #372;`

* **lno** — индекс света, возвращённый `dynamiclight_add`.
* **fld** — номер поля, которое нужно прочитать.

#### Описание и логика работы
`dynamiclight_get` читает свойство уже созданного динамического света: тип возвращаемого значения зависит от выбранного поля, поэтому builtin объявлен как `__variant`. Для MenuQC действует то же build-dependent замечание, что и для `dynamiclight_add`: в одних сборках меню builtin может отсутствовать совсем. Когда он доступен, его удобно использовать для отладки или для аккуратной модификации уже созданного света, не храня отдельную копию всех параметров в QuakeC.

#### Практические сценарии использования
```qc
void(float lno) Menu_DebugLight =
{
    local float radius;

    if (!checkbuiltin(dynamiclight_get))
        return;

    radius = dynamiclight_get(lno, 1); // конкретный номер поля зависит от набора RT-light констант
    con_printf("chatlog", "light radius = %g\n", radius);
};
```

### dynamiclight_set
`void(float lno, float fld, __variant value) dynamiclight_set = #373;`

* **lno** — индекс света.
* **fld** — номер изменяемого поля.
* **value** — новое значение нужного типа.

#### Описание и логика работы
`dynamiclight_set` меняет одно конкретное свойство ранее созданного источника света. Это особенно удобно, если в MenuQC вы создаёте свет однажды за кадр, а затем подправляете только цвет, радиус или флаги в зависимости от текущего состояния пункта меню. Как и `dynamiclight_get`, builtin стоит считать build-dependent для menu.dat и, если совместимость критична, проверять перед использованием.

#### Практические сценарии использования
```qc
void(float lno, float highlighted) Menu_TintLight =
{
    if (!checkbuiltin(dynamiclight_set))
        return;

    if (highlighted)
        dynamiclight_set(lno, 2, '1.0 0.8 0.3');
    else
        dynamiclight_set(lno, 2, '0.3 0.6 1.0');
};
```

## Ввод, клавиатура и мышь

### getkeybind
`string(float keynum) getkeybind = #342;`

* **keynum** — qscancode/Quake key number, для которого требуется текущий bind.

#### Описание и логика работы
[`getkeybind`](09-csqc-input-ui-builtins.md#getkeybind) возвращает строку команды, назначенную на указанную клавишу. Базовая сигнатура показывает только `keynum`, но текущая реализация FTEQW понимает и необязательные `bindmap` с `modifier`, поэтому MenuQC может читать не только «обычный» bind, но и альтернативные карты привязок. Если клавиша не распознана или не привязана, builtin возвращает пустую строку.

#### Практические сценарии использования
```qc
string() Menu_UseKeyLabel =
{
    local float key;
    local string bind;

    key = stringtokeynum("e");
    bind = getkeybind(key);
    if (bind == "")
        return "E: <нет бинда>";
    return "E: " + bind;
};
```

### setkeybind
`float(float key, string bind, optional float bindmap, optional float modifier) setkeybind = #630;`

* **key** — номер клавиши.
* **bind** — строка команды, которую надо назначить.
* **bindmap** *(optional)* — номер альтернативной bindmap.
* **modifier** *(optional)* — маска модификатора для variant-bind.

#### Описание и логика работы
[`setkeybind`](09-csqc-input-ui-builtins.md#setkeybind) переназначает клавишу без текстовой команды `bind`. Для обычного меню настроек управления это удобнее и безопаснее, чем собирать [`localcmd`](12-system-debug-builtins.md#localcmd) вручную. Если указан `bindmap`, запись идёт в альтернативную bindmap; это позволяет держать разные раскладки для разных контекстов меню или игры. Хотя builtin формально объявлен с `float`-возвратом, в текущем движке надёжнее проверять результат повторным вызовом `getkeybind`, а не полагаться на само возвращаемое значение.

#### Практические сценарии использования
```qc
void() Menu_AssignQuickSave =
{
    local float key;

    key = stringtokeynum("F6");
    if (key < 0)
        return;

    setkeybind(key, "save quick"); // записываем bind напрямую из меню
};
```

### getkeydest
`float() getkeydest = #602;`

* Аргументов нет.

#### Описание и логика работы
[`getkeydest`](09-csqc-input-ui-builtins.md#getkeydest) сообщает, кому сейчас принадлежит клавиатурный фокус для этого UI-слоя. В практическом MenuQC-коде лучше исходить из двух устойчивых состояний: `0` — меню не владеет фокусом, `2` — активен menu-layer. Внутри движка есть и другие destinations, но через эту пару builtin-функций MenuQC надёжно управляет именно собственным состоянием «открыто / закрыто».

#### Практические сценарии использования
```qc
float() Menu_IsOpen =
{
    return getkeydest() == 2;
};
```

### setkeydest
`void(float dest) setkeydest = #601;`

* **dest** — целевой key destination; на практике используйте `2` для активации MenuQC и `0` для возврата ввода игре.

#### Описание и логика работы
[`setkeydest`](09-csqc-input-ui-builtins.md#setkeydest) переключает владельца клавиатуры. Для MenuQC почти всегда нужны только два сценария: `setkeydest(2)` открыть/удержать меню и `setkeydest(0)` закрыть его, отдав клавиатуру обратно игре. Попытка использовать неподдерживаемые промежуточные значения в текущем движке приводит к builtin-ошибке, поэтому экспериментировать с «console/message»-режимом через этот API не стоит.

#### Практические сценарии использования
```qc
void() Menu_Close =
{
    if (getkeydest() == 2)
        setkeydest(0); // закрываем menu.dat и возвращаемся в игру
};
```

### getbindmaps
`vector() getbindmaps = #631;`

* Аргументов нет.

#### Описание и логика работы
[`getbindmaps`](09-csqc-input-ui-builtins.md#getbindmaps) возвращает в `x` и `y` активную пару альтернативных карт биндов. Для MenuQC это полезно в настройках управления, где нужно не просто показывать отдельный bind, а понимать, какая раскладка сейчас подключена целиком. Ноль означает отсутствие активной альтернативной карты в соответствующем слоте.

#### Практические сценарии использования
```qc
string() Menu_BindmapSummary =
{
    local vector maps;

    maps = getbindmaps();
    return sprintf("bindmaps: %g / %g", maps_x, maps_y);
};
```

### setbindmaps
`float(vector bm) setbindmaps = #632;`

* **bm** — вектор, где `x` и `y` задают две активные карты биндов, а `z` игнорируется.

#### Описание и логика работы
[`setbindmaps`](09-csqc-input-ui-builtins.md#setbindmaps) переключает активную пару альтернативных bindmap-слоёв. Это полезно, если ваше меню предлагает несколько готовых схем управления или временно включает особую раскладку для отдельного режима интерфейса. В текущей реализации FTEQW builtin возвращает ненуль после применения, поэтому в отличие от `setkeybind` на это значение уже можно умеренно опираться.

#### Практические сценарии использования
```qc
void(float use_alt_layout) Menu_SelectLayout =
{
    local vector maps;

    maps = '0 0 0';
    if (use_alt_layout)
        maps_x = 2;
    else
        maps_x = 1;

    if (setbindmaps(maps))
        con_printf("chatlog", "active bindmap is now %g\n", maps_x);
};
```

### getmousepos
`vector() getmousepos = #66;`

* Аргументов нет.

#### Описание и логика работы
MenuQC использует исторический builtin `getmousepos = #66`. Его поведение зависит от режима мыши: при свободном курсоре и `setmousetarget(2)` вы получаете абсолютные координаты курсора внутри виртуального экрана, а при `setmousetarget(1)` — накопленные дельты, которые сбрасываются самим чтением. Именно поэтому современный код сложного интерфейса обычно строят вокруг `Menu_InputEvent`, а [`getmousepos`](09-csqc-input-ui-builtins.md#getmousepos) используют как удобный helper для простой hover-логики и позиционирования собственного курсора.

#### Практические сценарии использования
```qc
vector menu_mouse;

void() Menu_UpdateMouseCache =
{
    setcursormode(1);
    setmousetarget(2);
    menu_mouse = getmousepos(); // абсолютная позиция курсора для hit-test'ов
};
```

### setmousetarget
`void(float trg) setmousetarget = #603;`

* **trg** — `1` для relative/delta режима, `2` для absolute/cursor режима.

#### Описание и логика работы
[`setmousetarget`](09-csqc-input-ui-builtins.md#setmousetarget) выбирает, как MenuQC хочет получать мышь: как дельты движения или как абсолютный экранный курсор. Для обычных кнопочных интерфейсов, браузеров и drag-and-drop меню почти всегда нужен режим `2`. Режим `1` полезнее там, где меню рисует собственный look/drag control или временно имитирует «игровую» обработку мыши.

#### Практические сценарии использования
```qc
void(float wants_cursor) Menu_SelectMouseMode =
{
    if (wants_cursor)
        setmousetarget(2);
    else
        setmousetarget(1);
};
```

### getmousetarget
`float() getmousetarget = #604;`

* Аргументов нет.

#### Описание и логика работы
[`getmousetarget`](09-csqc-input-ui-builtins.md#getmousetarget) возвращает текущее состояние той же системы: `1` для delta-ввода и `2` для абсолютного курсора. Builtin удобен, когда одно и то же MenuQC-окно умеет работать и в режиме «кнопки/браузер», и в режиме «прокрутка по дельтам», а вы хотите корректно восстановить состояние после закрытия вложенного виджета.

#### Практические сценарии использования
```qc
string() Menu_MouseModeLabel =
{
    if (getmousetarget() == 2)
        return "cursor mode";
    return "delta mode";
};
```

### setcursormode
`void(float usecursor, optional string cursorimage, optional vector hotspot, optional float scale) setcursormode = #343;`

* **usecursor** — `1`, если нужен свободный видимый курсор; `0`, если мышь должна быть снова захвачена движком.
* **cursorimage** *(optional)* — имя изображения курсора.
* **hotspot** *(optional)* — активная точка клика внутри картинки курсора.
* **scale** *(optional)* — масштаб курсора.

#### Описание и логика работы
[`setcursormode`](09-csqc-input-ui-builtins.md#setcursormode) освобождает или захватывает мышь для текущего UI-слоя и, при необходимости, назначает собственное изображение курсора. В MenuQC это центральный builtin для любых оконных интерфейсов: его обычно вызывают вместе с `setmousetarget(2)` и последующим чтением абсолютных координат. Если платформа умеет аппаратный курсор, FTEQW постарается использовать его; иначе движок рисует программную версию без конфликта с консолью.

#### Практические сценарии использования
```qc
void() Menu_EnablePointer =
{
    setcursormode(1, "gfx/menu/cursor", '4 2 0', 1); // свой курсор для menu.dat
    setmousetarget(2);
};
```

### keynumtostring
`string(float keynum) keynumtostring = #609;`

* **keynum** — код клавиши, который нужно превратить в читаемое имя.

#### Описание и логика работы
[`keynumtostring`](09-csqc-input-ui-builtins.md#keynumtostring) — основная MenuQC-версия builtin для перевода keynum в строку в стиле консольной команды `bind`: `SPACE`, `ENTER`, `MOUSE1`, `F6` и т.д. Это основной инструмент для экранов rebinding-а, где игроку нужно показать не команду, а именно человекочитаемое имя текущей клавиши. Для нераспознанных значений безопаснее считать пустую строку или странный текст ошибкой данных и обрабатывать отдельно.

#### Практические сценарии использования
```qc
string(float keynum) Menu_KeyCaption =
{
    local string name;

    name = keynumtostring(keynum);
    if (name == "")
        return "<unknown>";
    return name;
};
```

### keynumtostring_csqc
`string(float keynum) keynumtostring_csqc = #340;`

* **keynum** — код клавиши для legacy CSQC-совместимого alias.

#### Описание и логика работы
[`keynumtostring_csqc`](09-csqc-input-ui-builtins.md#keynumtostring_csqc) — deprecated MenuQC alias для CSQC-слота `#340`. Нового поведения он не добавляет: это тот же перевод keynum в строку, сохранённый ради старых helper-файлов и menu.dat, которые были написаны под CSQC-номер builtin. В новом коде MenuQC разумнее использовать обычный `keynumtostring = #609`, а этот alias держать только ради совместимости.

#### Практические сценарии использования
```qc
string() Menu_LegacyAcceptCaption =
{
    local float key;

    key = stringtokeynum_csqc("ENTER");
    return "Нажмите " + keynumtostring_csqc(key);
};
```

### stringtokeynum
`float(string key) stringtokeynum = #614;`

* **key** — текстовое имя клавиши в формате, понятном команде `bind`.

#### Описание и логика работы
[`stringtokeynum`](02-string-builtins.md#stringtokeynum) выполняет обратное преобразование: по строке вроде `ESCAPE`, `SPACE`, `MOUSE1` или `F5` возвращает numeric key code. В MenuQC это стандартный путь читать конфиг-значения, дефолтные клавиши меню и пользовательский текстовый ввод из полей настройки. Текущая реализация отвергает строки с модификаторами наподобие `CTRL+K` как единый key token и обычно возвращает `-1`.

#### Практические сценарии использования
```qc
float() Menu_BackKey =
{
    local float key;

    key = stringtokeynum("ESCAPE");
    if (key < 0)
        return stringtokeynum("BACKSPACE");
    return key;
};
```

### stringtokeynum_csqc
`float(string keyname) stringtokeynum_csqc = #341;`

* **keyname** — имя клавиши для legacy CSQC-совместимого alias.

#### Описание и логика работы
[`stringtokeynum_csqc`](09-csqc-input-ui-builtins.md#stringtokeynum_csqc) — deprecated MenuQC alias к CSQC-слоту `#341`. Он нужен прежде всего для старых menu helper-файлов, жёстко ожидающих старый номер builtin, и по смыслу полностью совпадает с обычным `stringtokeynum`: распознаёт стандартные имена клавиш и не предназначен для modifier-комбинаций как одной строки.

#### Практические сценарии использования
```qc
float() Menu_LegacyOpenConsoleKey =
{
    return stringtokeynum_csqc("ESCAPE");
};
```

### findkeysforcommand
`string(string command, optional float bindmap) findkeysforcommand = #610;`

* **command** — точная bind-строка, например `+jump` или `togglemenu`.
* **bindmap** *(optional)* — номер bindmap, если нужен поиск в альтернативной карте.

#### Описание и логика работы
[`findkeysforcommand`](09-csqc-input-ui-builtins.md#findkeysforcommand) возвращает список всех клавиш, выполняющих указанную команду, в строковом формате для последующего разбора через [`tokenize`](02-string-builtins.md#tokenize). Для MenuQC главным считается слот `#610`; исторический `#521` — это deprecated CSQC/DP-вариант, который в первую очередь фигурирует в другой VM. Builtin очень полезен на экранах настройки управления: можно сначала найти все связанные клавиши, потом пропустить каждый token через `keynumtostring` и красиво отрисовать список текущих назначений.

#### Практические сценарии использования
```qc
string() Menu_FirstJumpKey =
{
    local string raw;

    raw = findkeysforcommand("+jump");
    if (tokenize(raw) <= 0)
        return "<не назначено>";
    if (argv(0) == "-1")
        return "<не назначено>";
    return keynumtostring(stof(argv(0)));
};
```

## Встроенный веб-браузер

### gecko_create
`float(string name, optional string initialURI) gecko_create = #487;`

* **name** — имя shader/texture-слота, через который страница потом рисуется в меню.
* **initialURI** *(optional)* — стартовый URL или путь к локальной странице.

#### Описание и логика работы
`gecko_create` создаёт встроенную браузерную поверхность и связывает её с именованным shader-слотом. Дальше MenuQC может рисовать эту поверхность обычным `drawpic`, как любую другую картинку. Главное ограничение — наличие соответствующего browser/media backend (Gecko, CEF или совместимый плагин): если он отсутствует, builtin обычно возвращает `0`, и интерфейс должен gracefully перейти на запасной вариант без встроенной страницы.

#### Практические сценарии использования
```qc
float browser_ready;

void() m_init =
{
    browser_ready = gecko_create("browser/news", "ui/news.html");
    if (browser_ready)
        gecko_resize("browser/news", 1024, 768);
};
```

### gecko_destroy
`void(string name) gecko_destroy = #488;`

* **name** — имя ранее созданной браузерной поверхности.

#### Описание и логика работы
[`gecko_destroy`](09-csqc-input-ui-builtins.md#gecko_destroy) освобождает браузерный instance и связанный shader-слот. В MenuQC это имеет смысл делать при закрытии тяжёлых экранов, чтобы не держать лишнюю страницу, JavaScript-контекст и текстуру в памяти, пока игрок находится в другом разделе меню. Если surface уже не существует, вызов безопасно ничего не меняет.

#### Практические сценарии использования
```qc
void() Menu_CloseNews =
{
    if (browser_ready)
        gecko_destroy("browser/news");
    browser_ready = FALSE;
};
```

### gecko_navigate
`void(string name, string URI) gecko_navigate = #489;`

* **name** — имя браузерного shader.
* **URI** — адрес страницы или специальная команда вида `cmd:focus` / `cmd:unfocus`.

#### Описание и логика работы
[`gecko_navigate`](09-csqc-input-ui-builtins.md#gecko_navigate) отправляет встроенному браузеру команду навигации. Это может быть обычный URL, путь к локальному html-файлу или специальная управляющая команда: в комментариях FTEQW отдельно упомянуты `cmd:focus` и `cmd:unfocus`, которыми MenuQC явно отдаёт или забирает клавиатурный фокус у страницы. Если surface ещё не создана, вызов обычно просто игнорируется.

#### Практические сценарии использования
```qc
void() Menu_OpenPatchNotes =
{
    if (!browser_ready)
        return;

    gecko_navigate("browser/news", "https://example.org/patch-notes");
    gecko_navigate("browser/news", "cmd:focus");
};
```

### gecko_keyevent
`float(string name, float key, float eventtype, optional float charcode) gecko_keyevent = #490;`

* **name** — имя браузерного shader.
* **key** — keynum/scan code, который передаётся странице.
* **eventtype** — тип события клавиатуры.
* **charcode** *(optional)* — unicode/character code для текстового ввода.

#### Описание и логика работы
[`gecko_keyevent`](09-csqc-input-ui-builtins.md#gecko_keyevent) пересылает клавиатурные события из `Menu_InputEvent` прямо во встроенный браузер. Ненулевой результат означает, что page/backend принял событие; это удобно использовать как условие «съедания» клавиши, чтобы стрелки, enter и текстовый ввод не уходили одновременно и в HTML-форму, и в обычную логику menu.dat. Именно эта функция делает встроенный браузер по-настоящему интерактивным, а не просто видеовставкой.

#### Практические сценарии использования
```qc
float browser_has_focus;

float(float evtype, float scanx, float chary, float devid) Menu_InputEvent =
{
    if (browser_has_focus)
        return gecko_keyevent("browser/news", scanx, evtype, chary);
    return FALSE;
};
```

### gecko_mousemove
`void(string name, float x, float y) gecko_mousemove = #491;`

* **name** — имя браузерной поверхности.
* **x**, **y** — положение курсора внутри поверхности в диапазоне `0..1`.

#### Описание и логика работы
[`gecko_mousemove`](09-csqc-input-ui-builtins.md#gecko_mousemove) сообщает странице относительное положение мыши внутри браузерного прямоугольника. Здесь важен именно локальный UV-диапазон поверхности, а не координаты экрана меню, поэтому MenuQC обычно сначала читает абсолютный курсор через `getmousepos`, затем переводит его в координаты конкретной панели и уже после этого вызывает builtin. Значения вне диапазона `0..1` лучше отсекать самостоятельно.

#### Практические сценарии использования
```qc
vector browser_pos;
vector browser_size;

void() Menu_UpdateBrowserMouse =
{
    local vector mouse;
    local float u;
    local float v;

    mouse = getmousepos();
    u = (mouse_x - browser_pos_x) / browser_size_x;
    v = (mouse_y - browser_pos_y) / browser_size_y;
    if (u < 0 || v < 0 || u > 1 || v > 1)
        return;
    gecko_mousemove("browser/news", u, v);
};
```

### gecko_resize
`void(string name, float w, float h) gecko_resize = #492;`

* **name** — имя браузерной поверхности.
* **w**, **h** — новая ширина и высота browser buffer в пикселях.

#### Описание и логика работы
[`gecko_resize`](09-csqc-input-ui-builtins.md#gecko_resize) просит backend перестроить внутренний рендер-буфер под новый размер. Для MenuQC это важно не только для чёткости картинки, но и для layout самой страницы: HTML/CSS заново рассчитываются под новый viewport, так что полноэкранный и компактный режимы могут заметно отличаться. Обычно builtin вызывают при открытии окна, смене разрешения или переходе к другой компоновке UI.

#### Практические сценарии использования
```qc
void(vector screensize) Menu_ResizeBrowser =
{
    if (!browser_ready)
        return;

    gecko_resize("browser/news", screensize_x - 96, screensize_y - 128);
};
```

### gecko_get_texture_extent
`vector(string name) gecko_get_texture_extent = #493;`

* **name** — имя браузерной поверхности.

#### Описание и логика работы
[`gecko_get_texture_extent`](09-csqc-input-ui-builtins.md#gecko_get_texture_extent) возвращает текущий фактический размер браузерной текстуры. Вектор обычно содержит ширину в `x`, высоту в `y` и дополнительное backend-зависимое значение в `z` (часто aspect/helper value). Для MenuQC это удобный диагностический builtin: по нему можно понять, создался ли буфер вообще, совпадает ли он с желаемым размером и когда браузер уже готов к показу без пустого прямоугольника.

#### Практические сценарии использования
```qc
void() Menu_DebugBrowserExtent =
{
    local vector ext;

    ext = gecko_get_texture_extent("browser/news");
    con_printf("chatlog", "browser texture: %g x %g (extra %g)\n", ext_x, ext_y, ext_z);
};
```

## Смежные страницы

- [Рендеринг и сцена CSQC](./08-csqc-rendering-builtins.md)
- [Ввод, интерфейс и клавиатура CSQC](./09-csqc-input-ui-builtins.md)
- [Menu QuakeC](../16-quakec-scripting/menu-quakec.md)
- [Индекс справочника builtins](./README.md)
