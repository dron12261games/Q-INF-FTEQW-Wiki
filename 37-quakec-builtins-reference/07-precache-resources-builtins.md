# Прекэш и игровые ресурсы

> [⬅ Вернуться к оглавлению вики](../README.md)

> [Индекс справочника builtins](./README.md)

В Quake-подобных играх precache — это не просто оптимизация загрузки, а часть сетевого контракта между сервером и клиентами: обе стороны должны согласованно присвоить одним и тем же ресурсам одинаковые числовые индексы. Поэтому модели, звуки и другие игровые ресурсы обычно регистрируют во время загрузки уровня, до начала реального геймплея. Поздний precache уже после старта карты возможен не для всего и может потребовать дополнительных служебных сообщений, вызвать предупреждения в консоли или заметную задержку у клиентов, которым приходится срочно догружать ресурс на лету.

## Функции

### precache_file
`string(string s) precache_file = #68;`

* **s** — относительный путь к файлу, который вы хотите пометить как зависимость.

#### Описание и логика работы
В обычном SSQC/CSQC-варианте `precache_file` в FTEQW не формирует сетевой индекс и вообще ничего не загружает во время игры: это исторический builtin-подсказка для старых инструментов сборки pak-файлов. На стороне MenuQC существует отдельный builtin с тем же именем, но другим номером (`#28`): там он пытается скачать указанный файл с текущего сервера, если файла нет локально. Для серверной игровой логики `precache_file` нельзя использовать как проверку готовности ресурса, как замену `precache_model` или как способ гарантировать наличие картинки/модели у клиента.

#### Практические сценарии использования
```
void() RegisterLooseFiles =
{
// В SSQC это runtime no-op, но зависимость остаётся видимой для старых pack-скриптов.
precache_file("docs/credits.txt");
precache_file("gfx/help/episode1.lmp");
};
```

### precache_file2
`string(string str) precache_file2 = #77;`

* **str** — относительный путь к файлу-зависимости.

#### Описание и логика работы
`precache_file2` — исторический дубль `precache_file`. В рантайме FTEQW он так же ничего не загружает и не создаёт индексов ресурсов. Разница была нужна старому vanilla `qcc`, чтобы отличать файлы для упаковки в `pak1.pak` вместо `pak0.pak`; для современной игровой логики это не отдельный «мягкий» или «тихий» режим загрузки. Если нужен реальный runtime-контроль доступности ресурса, используйте builtin нужного типа (`precache_model`, `precache_pic`, `shaderforname` и т.д.), а не `precache_file2`.

#### Практические сценарии использования
```
void() RegisterRegisteredOnlyAssets =
{
// Историческая подсказка для старых pack/workflow-сценариев.
precache_file2("progs/endboss.txt");
precache_file2("maps/end_hub.ent");
};
```

### precache_model
`string(string s) precache_model = #20;`

* **s** — путь к модели или связанному с моделью ресурсу (`.mdl`, `.bsp`, `.iqm`, `.md5mesh`, `.framegroups` и т.п.).

#### Описание и логика работы
`precache_model` добавляет модель в общий precache-список и тем самым закрепляет за ней числовой индекс, который потом используют [`setmodel`](03-entity-world-builtins.md#setmodel), сетевые entity update и сопутствующие builtin-lookup функции. Это главное отличие от простого хранения строки: сервер и клиенты должны заранее согласовать один и тот же slot для одной и той же модели, иначе сетевые обновления начнут ссылаться не на тот ресурс. Вызывать builtin следует во время загрузки уровня — обычно из `worldspawn`, общих функций `W_Precache`/`PlayerPrecache` и прочих spawn-time путей до начала матча. Если попытаться использовать модель без precache, `setmodel` не должен получать неизвестное имя: результатом будут ошибки, warnings, невидимая геометрия или рассинхронизация. Для `.bsp` FTEQW старается реально подтянуть модель с диска сразу; для прочих форматов важнее сама регистрация имени в precache-таблице. В SSQC такие model index всегда положительные; в CSQC клиентские precache-индексы могут быть отрицательными, если соответствующей модели нет в серверной таблице.

Если precache делается уже после старта карты, FTEQW помечает это как delayed precache и шлёт клиентам дополнительное надёжное сообщение о новом model index. На современных клиентах FTE-ветки это позволяет осторожно добавлять новые модели на лету, но старые клиенты или клиенты без нужного расширения могут показать warning, зависнуть на срочной подгрузке или вообще не успеть корректно использовать ресурс в том же кадре. Поэтому «правильный» precache — это precache до старта игры; поздний precache годится только для хорошо контролируемых расширенных сценариев.

#### Практические сценарии использования
```
void() Guard_Precache =
{
precache_model("models/guards/elite.framegroups");
precache_model("progs/g_guard.mdl");
};

void() monster_guard_spawn =
{
Guard_Precache();
setmodel(self, "models/guards/elite.framegroups");
setsize(self, '-16 -16 -24', '16 16 40');
self.frame = 0;
};
```

### precache_model2
`string(string str) precache_model2 = #75;`

* **str** — путь к модели, которую нужно зарегистрировать в precache-таблице.

#### Описание и логика работы
В FTEQW `precache_model2` использует ту же runtime-логику, что и `precache_model`: модель попадает в тот же precache-поток, получает индекс и подчиняется тем же требованиям по spawn-time вызову. Исторически отдельное имя было нужно старому `qcc`, чтобы при упаковке ресурсов различать контент для `pak1.pak`; именно это и есть реальная причина существования суффикса `2`. Поэтому не стоит воспринимать `precache_model2` как отдельный try-precache, quiet-precache или builtin, который «не считается ошибкой» сам по себе: если модель действительно обязательна для геймплея, правила синхронизации индексов и риски позднего precache остаются теми же. Для условного выбора между HD/SD-моделью безопаснее сочетать `getmodelindex(..., 1)` и обычный fallback.

#### Практические сценарии использования
```
void() EpisodeTwo_Precache =
{
// Исторический alias: в рантайме работает как обычный model precache.
precache_model2("progs/boss2.mdl");
precache_model2("progs/end_teleporter.mdl");
};
```

### precache_pic
`string(string name, optional float flags) precache_pic = #317;`

* **name** — путь к картинке или имени 2D-ресурса, который будет рисоваться в CSQC/MenuQC.
* **flags** — необязательная битовая маска `PRECACHE_PIC_*`, задающая режим подгрузки.

#### Описание и логика работы
`precache_pic` относится не к сетевому entity precache сервера, а к клиентским/UI-ресурсам: она заставляет движок заранее найти и загрузить указанное изображение, чтобы первый [`drawpic`](08-csqc-rendering-builtins.md#drawpic)/[`showpic`](08-csqc-rendering-builtins.md#showpic) не упёрся в внезапную декомпрессию, чтение с диска или докачку. Это полезно для HUD-иконок, экранов меню, баннеров, инвентарных картинок и других 2D-материалов. Комментарии в `fteextensions.qc` указывают, что `flags` — это набор `PRECACHE_PIC_*`; на практике чаще всего важны `PRECACHE_PIC_FROMWAD`, `PRECACHE_PIC_NOCLAMP`, `PRECACHE_PIC_DOWNLOAD` и `PRECACHE_PIC_TEST`. Последние два могут блокировать выполнение до завершения загрузки и потому потенциально медленные.

В отличие от `precache_model`, здесь нет задачи согласовать общий серверный индекс между игроками: картинка нужна локально конкретному клиенту. Но идея та же — загрузить ресурс заранее, пока вы ещё контролируете момент ожидания, а не в первом кадре показа HUD.

#### Практические сценарии использования
```
float hud_icons_ready;

void() HUD_PrecacheIcons =
{
if (hud_icons_ready)
return;

if (precache_pic("gfx/hud/ammo.tga", PRECACHE_PIC_TEST) != "")
if (precache_pic("gfx/hud/armor.tga", PRECACHE_PIC_TEST) != "")
hud_icons_ready = 1;
};
```

### precache_vwep_model
`float(string mname) precache_vwep_model = #532;`

* **mname** — путь к модели видимого оружия для legacy VWEP-таблицы.

#### Описание и логика работы
`precache_vwep_model` — специализированный SSQC builtin из расширения `ZQ_VWEP`, предназначенный для регистрации **visible weapon models**: третье лицо видит оружие в руках другого игрока не через обычный `v_`-viewmodel, а через отдельную сетевую VWEP-таблицу. Это не универсальная замена `precache_model`; обычно такую модель всё равно отдельно регистрируют обычным precache, если она ещё где-то используется. Проверка по серверной реализации показывает два важных ограничения: имя должно быть безопасной обычной строкой без мусорных символов, а вызывать builtin надо именно на spawn-time. Поздний вызов уже после загрузки карты отвергается и не должен использоваться как динамический mid-game loader.

#### Практические сценарии использования
```
void() W_PrecachePlayerWeapons =
{
precache_model("progs/p_shot.mdl");
precache_model("progs/p_nail.mdl");

// Регистрируем те же модели ещё и в legacy visible-weapon таблице.
precache_vwep_model("progs/p_shot.mdl");
precache_vwep_model("progs/p_nail.mdl");
};
```

### getmodelindex
`float(string modelname, optional float queryonly) getmodelindex = #200;`

* **modelname** — путь к модели, индекс которой нужно получить.
* **queryonly** — если не ноль, builtin только проверяет существующий precache-slot и не создаёт новый автоматически.

#### Описание и логика работы
`getmodelindex` — это удобная сокращённая форма для сценария «обеспечь precache модели и верни её числовой индекс». Документация движка прямо описывает его как альтернативу связке `precache_model(foo); setmodel(bar, foo); return bar.modelindex;`. Ключевая деталь — режим `queryonly`: когда он включён, функция не будет насильно делать late precache для ранее неизвестной модели, а просто вернёт `0`. Это критично для мягких fallback-сценариев, где вы хотите узнать «есть ли уже такой ресурс в таблице», но не хотите в середине матча внезапно разослать delayed precache всем клиентам и поймать лаг на догрузке.

На практике `getmodelindex` особенно полезен для выбора между несколькими вариантами одной и той же модели — например, HD-версией и классическим fallback. Он помогает сначала безопасно спросить про optional content, а уже потом, при необходимости, зафиксировать нормальный precache обязательной модели.

#### Практические сценарии использования
```
float() ChooseGuardModelIndex =
{
local float idx;

idx = getmodelindex("models/guards/elite_hd.framegroups", 1);
if (!idx)
idx = getmodelindex("models/guards/elite.framegroups", 0);

return idx;
};

void() SpawnEliteGuard =
{
local float idx;

idx = ChooseGuardModelIndex();
setmodel(self, modelnameforindex(idx));
};
```

### modelnameforindex
`string(float mdlindex) modelnameforindex = #334;`

* **mdlindex** — числовой индекс модели в precache-таблице.

#### Описание и логика работы
`modelnameforindex` делает обратное преобразование: по уже известному model index возвращает строковое имя модели. Это полезно там, где вы храните или пересылаете компактные числовые ссылки, а человеку или другой части логики всё же нужно исходное имя ресурса. Документация FTEQW отдельно отмечает, что такая пара `getmodelindex` ↔ `modelnameforindex` помогает уменьшать сетевой трафик CSQC: вместо длинной строки можно гонять число, если обе стороны знают одну и ту же precache-таблицу. Для некорректного или пустого индекса возвращается пустая строка.

#### Практические сценарии использования
```
void(float idx) DebugModelSlot =
{
local string name;

name = modelnameforindex(idx);
if (name != "")
dprint(sprintf("model slot %g = %s\n", idx, name));
};
```

### frameforname
`float(float modidx, string framename) frameforname = #276;`

* **modidx** — model index уже загруженной модели.
* **framename** — текстовое имя группы анимации или именованного framegroup.

#### Описание и логика работы
`frameforname` нужен не для старых безымянных покадровых `.mdl`, а прежде всего для современных моделей, где анимации приходят как **именованные группы**: `idle`, `walk`, `run`, `attack_melee`, `death_back` и т.п. Это типичный случай для `.iqm`, связок `.md5mesh` + `.md5anim`, а также для `EXTERNALANIM`/`.framegroups`, где одна геометрия собирается с несколькими отдельными анимациями. В такой модели поле `frame` у сущности указывает не «сырой двадцать третий кадр из файла», а номер логической анимационной группы. `frameforname` даёт способ найти этот номер по имени, не хардкодя магические числа в QuakeC. Если группа не найдена, builtin возвращает `-1`.

Практически это выглядит так: художник экспортирует персонажа с анимациями `idle`, `run`, `attack_melee`; сервер при spawn берёт `self.modelindex`, запрашивает номера этих групп через `frameforname`, сохраняет их в полях сущности и дальше переключает поведение монстра уже по осмысленным именам. Это делает код устойчивее к перестановке анимаций в исходном файле: пока имя группы осталось прежним, QuakeC не надо переписывать.

#### Практические сценарии использования
```
.float anim_idle;
.float anim_run;
.float anim_attack;

void() Guard_BindAnimations =
{
self.anim_idle = frameforname(self.modelindex, "idle");
self.anim_run = frameforname(self.modelindex, "run");
self.anim_attack = frameforname(self.modelindex, "attack_melee");

if (self.anim_idle >= 0)
self.frame = self.anim_idle;
};
```

### frametoname
`string(float modidx, float framenum) frametoname = #284;`

* **modidx** — model index модели, чьи анимационные группы вы опрашиваете.
* **framenum** — числовой номер framegroup/анимации.

#### Описание и логика работы
`frametoname` — обратная операция к `frameforname`: по номеру framegroup она возвращает его текстовое имя. Это особенно удобно для отладки сложных анимационных state machine, логирования сетевых событий и редакторских инструментов, где в консоль хочется вывести не число `7`, а понятное `attack_melee`. Для современных `.iqm`/`.md5`-моделей с именованными анимациями builtin позволяет быстро проверить, действительно ли сущность сейчас стоит на нужной группе. Для невалидного номера возвращается пустая строка.

#### Практические сценарии использования
```
void() Guard_DebugCurrentAnim =
{
local string animname;

animname = frametoname(self.modelindex, self.frame);
if (animname != "")
dprint(sprintf("guard anim = %s\n", animname));
};
```

### frameduration
`float(float modidx, float framenum) frameduration = #277;`

* **modidx** — model index модели.
* **framenum** — номер именованной анимационной группы.

#### Описание и логика работы
`frameduration` возвращает длительность всей framegroup в секундах. Для именованной анимации персонажа это означает полный цикл группы, а не длительность одного геометрического кадра. Например, у `run` это может быть длина полного шага, у `attack_melee` — длина всей атаки от замаха до конца recovery. Это особенно полезно вместе с `frameforname`: сначала находите группу по имени, потом задаёте `self.frame` и рассчитываете, когда переводить ИИ в следующее состояние, когда запускать hit frame или когда можно снова разрешить атаку. При невалидной модели или группе функция возвращает `0`.

#### Практические сценарии использования
```
void() Guard_FinishAttack;

void() Guard_StartAttack =
{
local float attackframe;
local float attacktime;

attackframe = frameforname(self.modelindex, "attack_melee");
if (attackframe < 0)
return;

self.frame = attackframe;
attacktime = frameduration(self.modelindex, attackframe);
self.nextthink = time + attacktime;
self.think = Guard_FinishAttack;
};
```

### skinforname
`float(float mdlindex, string skinname) skinforname = #237;`

* **mdlindex** — model index модели, внутри которой надо искать skin.
* **skinname** — строковое имя skin-варианта или skin-файла.

#### Описание и логика работы
`skinforname` превращает строковое имя skin-варианта в числовой индекс, который потом можно записать в поле `.skin`. Это полезно для моделей с несколькими наборами материалов, раскрасками команды или Quake 3-style `.skin`-файлами, где логике удобнее оперировать понятным именем (`lower_red.skin`, `head_blue.skin`), а движок в итоге рисует по номеру. Если указанный skin не найден, builtin возвращает `-1`. Как и с анимациями, смысл функции в том, чтобы отвязать QuakeC от жёстко забитых чисел и дать художнику свободу переставлять содержимое модели без правки игрового кода.

#### Практические сценарии использования
```
void(string teamname) ApplyGuardSkin =
{
local float skinidx;
local string skinfile;

skinfile = strcat("models/players/guard/lower_", teamname, ".skin");
skinidx = skinforname(self.modelindex, skinfile);
if (skinidx >= 0)
self.skin = skinidx;
};
```

### skintoname
`string(float modidx, float skin) skintoname = #285;`

* **modidx** — model index модели.
* **skin** — числовой skin index, который нужно превратить обратно в имя.

#### Описание и логика работы
`skintoname` делает обратный lookup для skin-индексов. Он удобен в редакторских меню, отладке пользовательских настроек персонажа и сериализации состояния, когда хранится число, а показывать или логировать хочется исходное имя skin-ресурса. Для несуществующего skin index builtin возвращает пустую строку.

#### Практические сценарии использования
```
void() DebugGuardSkin =
{
local string skinname;

skinname = skintoname(self.modelindex, self.skin);
if (skinname != "")
dprint(sprintf("guard skin = %s\n", skinname));
};
```

### shaderforname
`float(string shadername, optional string defaultshader, ...) shaderforname = #238;`

* **shadername** — имя shader-ресурса, который нужно получить или создать.
* **defaultshader** — fallback-описание shader script, включая внешние `{}`.
* **...** — дополнительные строковые фрагменты, которые движок склеивает в единый текст fallback shader-а.

#### Описание и логика работы
`shaderforname` регистрирует shader и возвращает числовой handle. Если shader уже существует на диске, движок использует его. Если файл не найден или загрузка shader-скриптов запрещена, FTEQW может собрать shader на лету из строки `defaultshader`; это особенно удобно для CSQC/MenuQC-инструментов, оверлеев и специальных material override-эффектов. Для сущностей типичный сценарий — записать результат в поле `.forceshader`, чтобы временно переопределить материалы модели. Если создать или найти shader не удалось, builtin возвращает `0`.

Важная практическая деталь: variadic-хвост после `defaultshader` нужен не «для красоты», а для удобной сборки длинного shader script из нескольких строковых аргументов. Это позволяет писать читабельный QuakeC без ручного [`strcat`](02-string-builtins.md#strcat) для больших блоков текста.

#### Практические сценарии использования
```
void() ApplyQuadOverlay =
{
self.forceshader = shaderforname(
"fx/quad_overlay",
"{\n",
"{\n",
"map textures/fx/quad.tga\n",
"blendfunc add\n",
"rgbgen identity\n",
"}\n",
"}\n"
);
};
```

### findfont
`float(string s) findfont = #356;`

* **s** — логическое имя font slot-а или, как запасной вариант, реальное имя шрифта.

#### Описание и логика работы
`findfont` ищет уже зарегистрированный font slot по имени. Документация подчёркивает, что в крайнем случае движок пытается сопоставить и фактическое имя шрифта, а не только alias slot-а. Это полезно, когда один кусок кода ранее вызвал `loadfont("hud", ...)`, а другой позже хочет просто найти тот же slot по имени `hud`, не создавая заново. Функция не предназначена для загрузки новых файлов шрифта с диска — она именно находит уже известный slot/handle для дальнейшего использования в `drawfont`.

#### Практические сценарии использования
```
void() RestoreHudFont =
{
drawfont = findfont("hud");
};
```

### loadfont
`float(string fontname, string fontmaps, string sizes, float slot, optional float fix_scale, optional float fix_voffset) loadfont = #357;`

* **fontname** — логическое имя slot-а, под которым шрифт потом удобно искать через `findfont`.
* **fontmaps** — имя семейства/лица шрифта, которое движок должен реально открыть.
* **sizes** — строка со списком размеров и дополнительных токенов вроде `outline=1` или `scale=1`.
* **slot** — желаемый номер slot-а; `-1` просит движок подобрать подходящий автоматически.
* **fix_scale** — дополнительный параметр совместимости; в современном коде обычно оставляют `0`.
* **fix_voffset** — дополнительный параметр совместимости для вертикального смещения; обычно оставляют `0`.

#### Описание и логика работы
`loadfont` регистрирует новый font slot или переопределяет существующий. Это один из тех builtin-ов, которые формально просты, но практически завязаны на множество деталей движка: список размеров, постобработку outline, выбор slot-а и fallback по имени. Сам комментарий в `fteextensions.qc` честно предупреждает, что интерфейс довольно запутанный, но даёт рабочий пример с `cour`. На практике правило простое: дайте slot-имя для повторного поиска, укажите реальный face name, перечислите нужные размеры через пробел и сразу сохраните возвращённый handle в `drawfont` или своей глобальной переменной. Если UI-код позже снова позовёт `loadfont` с тем же slot-именем и другим набором параметров, старое содержимое slot-а будет переинициализировано.

#### Практические сценарии использования
```
float hud_font;

void() HUD_LoadFonts =
{
hud_font = loadfont("hud", "cour", "12 16 20 outline=1 scale=1", -1, 0, 0);
drawfont = hud_font;
};
```

### changepic
`DEP_CSQC void(string slot, string picname, optional entity player) changepic = #107;`

* **slot** — строковое имя уже существующего showpic-слота.
* **picname** — новая картинка, которую надо подставить в этот слот.
* **player** — необязательный клиент, которому надо отправить замену; если аргумент не указан, обновление рассылается всем клиентам.

#### Описание и логика работы
`changepic` относится к расширению `TEI_SHOWLMP2` и работает на сервере как команда клиентскому HUD-слою: поменять уже показанную картинку в named slot, не трогая позицию и зону привязки. Это удобно для инвентарных иконок, portrait-слотов, индикаторов оружия и прочих элементов, где геометрия интерфейса остаётся прежней, а сам image asset меняется часто. Проверка реализации показывает два важных поведения: если передан `player`, это должен быть реальный клиент, иначе будет runtime error; если клиент не поддерживает расширение `PEXT_SHOWPIC`, сервер просто не пошлёт ему обновление. Иными словами, builtin хорош для FTE-расширенного HUD, но не является универсальным кросс-движковым способом смены картинок.

#### Практические сценарии использования
```
void(entity pl) InitAmmoHud =
{
showpic("hud_ammo", "gfx/hud/shells.tga", 32, 32, SL_ORG_TL, pl);
};

void(entity pl, float shells_left) UpdateAmmoHud =
{
if (shells_left > 0)
changepic("hud_ammo", "gfx/hud/shells.tga", pl);
else
changepic("hud_ammo", "gfx/hud/empty.tga", pl);
};
```

### drawgetimagesize
`vector(string picname) drawgetimagesize = #318;`

* **picname** — имя или путь картинки, размеры которой нужно узнать.

#### Описание и логика работы
`drawgetimagesize` возвращает ширину и высоту изображения в виде `vector`, где значимы `x` и `y`. Документация отдельно подчёркивает важную деталь для классических ресурсов: если вы спрашиваете `.lmp`, функция старается вернуть именно исходные размеры `.lmp`, даже когда на диске есть texture replacement более высокого разрешения. Это удобно для старых интерфейсов, у которых логика позиционирования завязана на оригинальную сетку пикселей. Ещё один важный нюанс — первый вызов может быть медленным, если картинка ещё не была загружена или вы вызываете функцию сразу после первого `precache_pic`, не дав движку спокойно завершить работу с ресурсом.

#### Практические сценарии использования
```
void() DrawCenterLogo =
{
local vector size;
local vector pos;

size = drawgetimagesize("gfx/menu/logo.lmp");
pos_x = (vid_conwidth - size_x) * 0.5;
pos_y = 32;
pos_z = 0;
drawpic(pos, "gfx/menu/logo.lmp", size, '1 1 1', 1, 0);
};
```

### iscachedpic
`float(string name) iscachedpic = #316;`

* **name** — имя картинки, наличие которой в текущем кэше нужно проверить.

#### Описание и логика работы
`iscachedpic` даёт быстрый эвристический ответ на вопрос «знает ли движок сейчас эту картинку». Именно эвристический: комментарий в `fteextensions.qc` предупреждает, что разные движки могут «врать» или сохранять кэш между картами. Поэтому использовать `iscachedpic` как жёсткую гарантию готовности ресурса не стоит. Зато builtin отлично подходит для ленивой загрузки UI: если иконка ещё не в кэше, можно один раз вызвать `precache_pic`, а если уже там — спокойно рисовать дальше без лишней работы.

#### Практические сценарии использования
```
void() EnsureScoreboardBadge =
{
if (!iscachedpic("gfx/hud/badge_gold.tga"))
precache_pic("gfx/hud/badge_gold.tga", 0);
};
```

### freepic
`void(string name) freepic = #319;`

* **name** — имя картинки, которая больше не нужна UI-логике.

#### Описание и логика работы
По документации `freepic` сообщает движку, что указанное изображение больше не нужно и при следующем обращении должно выглядеть как «новое». Концептуально это симметричная пара к `precache_pic`: вы заранее подгрузили редкую картинку меню, попользовались ею и потом позволили движку освободить её. Но здесь важно не переоценивать жёсткость контракта: клиентская реализация FTEQW трактует вызов скорее как подсказку, а не как гарантированное немедленное освобождение памяти/VRAM. Поэтому `freepic` полезно воспринимать как advisory hint для крупного интерфейсного контента, а не как точный manual memory manager.

#### Практические сценарии использования
```
void() CloseInventoryAtlas =
{
freepic("gfx/inventory/atlas_0.tga");
freepic("gfx/inventory/atlas_1.tga");
freepic("gfx/inventory/atlas_2.tga");
};
```

### addprogs
`float(string progsname) addprogs = #202;`

* **progsname** — путь к дополнительному `.dat`/progs-модулю, который нужно загрузить в текущую QCVM.

#### Описание и логика работы

`addprogs` загружает ещё один progs-модуль в уже работающую виртуальную машину и возвращает handle, который затем используют `externcall`, `externset` и `externvalue`. Это builtin расширения `FTE_MULTIPROGS`; официальный комментарий к `init` отдельно подчёркивает, что безопаснее всего вызывать его именно из `init`, когда сущности ещё не считаются валидными и вы можете спокойно связать глобалы между модулями. Если имя пустое или загрузка не удалась, движок возвращает `-1`.

#### Практические сценарии использования

```qc
void(float prevprogs) init
{
    local float addon;

    if (prevprogs)
        return;

    addon = addprogs("progs/hudmodule.dat");
    if (addon >= 0)
        externcall(addon, "module_init");
}
```
### frameforaction
`float(float modidx, int actionid) frameforaction = #0:frameforaction;`

* **modidx** — `float`, model index уже загруженной модели.
* **actionid** — `int`, model-defined action identifier.

#### Описание и логика работы

`frameforaction` ищет у модели анимацию, помеченную указанным `actionid`, и возвращает случайный подходящий framegroup/animation index. Если у модели нет такой action-метки или сам `modidx` невалиден, результатом будет `-1`. Builtin особенно полезен для форматов и пайплайнов, где анимации экспортируются не только по именам, но и по action-id; для старых моделей без такой метаинформации он обычно ничего полезного не найдёт.

#### Практические сценарии использования

```qc
void() bind_run_action
{
    local int action_run;
    local float anim;

    action_run = 4; // зависит от того, как action-id были экспортированы в модель
    anim = frameforaction(self.modelindex, action_run);
    if (anim >= 0)
        self.frame = anim;
}
```
### getmodeleventidx
`float(float modidx, float framenum, int eventidx, __out float timestamp, __out int code, __out string data) getmodeleventidx = #0:getmodeleventidx;`

* **modidx** — `float`, model index модели.
* **framenum** — `float`, номер framegroup/анимации.
* **eventidx** — `int`, порядковый индекс события внутри этой анимации.
* **timestamp** — `__out float`, сюда записывается время события внутри анимации.
* **code** — `__out int`, сюда записывается числовой код события.
* **data** — `__out string`, сюда записывается строковый payload события.

#### Описание и логика работы

`getmodeleventidx` обращается к событию по его индексу внутри конкретной анимации и возвращает `true`, если такое событие существует. На успехе builtin заполняет `timestamp`, `code` и `data`; на ошибке или выходе за диапазон возвращает `false`. В отличие от `getnextmodelevent`, эта функция не занимается интервалами времени и не разворачивает looping-анимации автоматически, зато позволяет добраться до нескольких событий с одинаковым timestamp.

#### Практические сценарии использования

```qc
void() dump_first_anim_event
{
    local float timestamp;
    local int code;
    local string data;

    if (getmodeleventidx(self.modelindex, self.frame, 0, timestamp, code, data))
        dprint(sprintf("event0 time=%g code=%d data=%s\n", timestamp, code, data));
}
```
### getnextmodelevent
`float(float modidx, float framenum, __inout float basetime, float targettime, __out int code, __out string data) getnextmodelevent = #0:getnextmodelevent;`

* **modidx** — `float`, model index модели.
* **framenum** — `float`, номер анимации/framegroup.
* **basetime** — `__inout float`, нижняя граница поиска; на выходе становится временем найденного события или `targettime`.
* **targettime** — `float`, верхняя граница поиска.
* **code** — `__out int`, числовой код найденного события.
* **data** — `__out string`, строковые данные найденного события.

#### Описание и логика работы

`getnextmodelevent` ищет ближайшее следующее событие анимации между `basetime` и `targettime` и возвращает булево значение успеха. По официальной докстроке FTEQW при успехе builtin записывает в `basetime`, `code` и `data` найденное событие, а при неудаче просто продвигает `basetime` до `targettime`; реализация фактически работает как окно `(basetime, targettime]`. Важное ограничение прямо зафиксировано в исходниках: несколько событий с **одинаковым timestamp** эта функция корректно не перебирает, для такого случая нужен `getmodeleventidx`.

#### Практические сценарии использования

```qc
.float anim_event_time;

void() poll_next_anim_event
{
    local float t;
    local int code;
    local string data;

    t = self.anim_event_time;
    if (getnextmodelevent(self.modelindex, self.frame, t, time, code, data))
    {
        self.anim_event_time = t;
        dprint(sprintf("event code=%d data=%s\n", code, data));
    }
    else
        self.anim_event_time = t;
}
```
### modelframecount
`float(float mdlidx) modelframecount = #0:modelframecount;`

* **mdlidx** — `float`, model index, для которого нужно узнать количество доступных framegroup/animation slots.

#### Описание и логика работы

`modelframecount` возвращает количество кадров/анимационных групп, которые движок видит у указанной модели. Для простых спрайтов и старых покадровых форматов это обычно число обычных кадров, а для современных моделей с именованными анимациями — количество адресуемых framegroup, которые можно ставить в `self.frame` или искать через `frameforname`. Если модель не найдена, builtin возвращает `0`.

#### Практические сценарии использования

```qc
void() clamp_model_frame
{
    local float count;

    count = modelframecount(self.modelindex);
    if (count > 0 && self.frame >= count)
        self.frame = count - 1;
}
```
### processmodelevents
`void(float modidx, float framenum, __inout float basetime, float targettime, void(float timestamp, int code, string data) callback) processmodelevents = #0:processmodelevents;`

* **modidx** — `float`, model index модели.
* **framenum** — `float`, номер анимации/framegroup.
* **basetime** — `__inout float`, стартовая временная позиция; после вызова движок продвигает её к `targettime`.
* **targettime** — `float`, до какого времени нужно обработать события.
* **callback** — функция вида `void(float timestamp, int code, string data)`, которую движок вызовет для каждого достигнутого события.

#### Описание и логика работы

`processmodelevents` — пакетный вариант обхода model events: вместо одного ближайшего события builtin вызывает ваш callback для **каждого** события, достигнутого между `basetime` и `targettime`, а затем выставляет `basetime = targettime`. Реализация в `pr_skelobj.c` специально не делает ничего при `basetime == targettime`, умеет учитывать looping-анимации и для alias/generic путей перебирает события немного по-разному, но внешний контракт у них одинаковый. На практике это самый удобный вариант, когда вы раз в кадр продвигаете анимацию и хотите автоматически проиграть все пропущенные footsteps, muzzle flashes и прочие embedded events.

#### Практические сценарии использования

```qc
.float anim_event_time;

void(float timestamp, int code, string data) Guard_AnimEventCB
{
    if (code == 1)
        sound(self, CHAN_BODY, data, 1, ATTN_NORM);
}

void() Guard_ProcessAnimEvents
{
    local float t;

    t = self.anim_event_time;
    processmodelevents(self.modelindex, self.frame, t, time, Guard_AnimEventCB);
    self.anim_event_time = t;
}
```
### spriteframe
`string(string modelname, int frame, float frametime) spriteframe = #0:spriteframe;`

* **modelname** — имя sprite-модели.
* **frame** — `int`, индекс кадра/группы внутри спрайта.
* **frametime** — `float`, время/селектор подкадра для animated sprite-group.

#### Описание и логика работы

`spriteframe` — CSQC builtin для случаев, когда вам нужен не scene-entity, а готовое имя shader'а конкретного кадра спрайта, пригодное для `drawpic`, `R_BeginPolygon` и похожих 2D/overlay-рендер путей. Движок находит модель по имени, при необходимости догружает её, убеждается, что это именно sprite, а затем выбирает нужный subframe и возвращает имя shader'а. Для невалидного имени, некорректного `frame` или не-sprite модели builtin возвращает пустую строку; для grouped/animated sprite'ов параметр `frametime` используется как селектор текущего подкадра.

#### Практические сценарии использования

```qc
void() DrawMuzzleSprite
{
    local string shader;
    local vector size;

    shader = spriteframe("sprites/muzzle.spr", 0, time);
    if (shader == "")
        return;

    size = drawgetimagesize(shader);
    drawpic('320 200 0', shader, size, '1 1 1', 1);
}
```

## Смежные страницы

- [Раздел по моделям и анимации](../02-models-animation/README.md)
- [Индекс справочника builtins](./README.md)