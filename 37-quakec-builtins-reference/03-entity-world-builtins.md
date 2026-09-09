# Сущности и игровой мир

> [⬅ Предыдущая страница](02-string-builtins.md) | [Следующая страница ➡](04-network-messages-builtins.md)

> [⬅ Вернуться к оглавлению вики](../README.md)
> [Индекс справочника builtins](../README.md#встроенные-функции-quakec-builtins)

Эта страница собирает builtins, через которые SSQC и соседние модули QuakeC работают с entity, трассировкой, статами, userinfo и состоянием мира. Основа описаний — комментарии в `quakec\menusys\fteextensions.qc`, а спорные детали сверены по реализации движка. Хотя акцент страницы — SSQC, несколько функций из списка доступны только в CSQC или зависят от конкретной сборки/расширения FTEQW; это отмечено прямо в описаниях.

## Функции

### spawn
`entity() spawn = #14;`

* Аргументы отсутствуют.

#### Описание и логика работы
Создаёт новый edict и возвращает ссылку на него. Поля новой сущности стартуют в обычном «чистом» состоянии, поэтому после `spawn` вы обычно сразу задаёте [`classname`](../39-entity-keys-reference/01-worldspawn-common-keys.md#classname), [`origin`](../39-entity-keys-reference/01-worldspawn-common-keys.md#origin), `solid`, `movetype`, [`model`](../39-entity-keys-reference/01-worldspawn-common-keys.md#model) и колбэки. Сущность не нужно дополнительно «регистрировать»: как только вы выставите нужные поля и выполните `setorigin`/`setmodel`/`setsize`, она начнёт участвовать в коллизии, логике и сетевой репликации как обычный edict.

#### Практические сценарии использования
```qc
void() SpawnMarker =
{
    entity e;

    e = spawn();
    e.classname = "logic_marker";
    e.movetype = MOVETYPE_NONE;
    e.solid = SOLID_NOT;
    setorigin(e, '256 128 64');
};
```

---

### remove
`void(entity e) remove = #15;`

* **e** — `entity`, сущность, которую нужно удалить.

#### Описание и логика работы
Удаляет сущность из мира. По комментариям и реализации FTEQW, после вызова ссылка считается недействительной: движок очищает часть полей и позже может переиспользовать слот под другой edict, поэтому нельзя продолжать обращаться к старому указателю. В FTE есть окно до повторного использования слота, из-за чего `wasfreed` ещё некоторое время способен распознать «свежевычищенный» edict, но логика мода не должна на это опираться как на основной способ управления жизненным циклом.

#### Практические сценарии использования
```qc
void() RemoveSelfThink =
{
    remove(self);
};

void() SpawnTimedTrigger =
{
    entity e;

    e = spawn();
    e.classname = "timed_trigger";
    e.nextthink = time + 0.5;
    e.think = RemoveSelfThink;
};
```

---

### find
`entity(entity start, .string fld, string match) find = #18;`

* **start** — `entity`, точка начала поиска; обычно `world` или предыдущий найденный edict.
* **fld** — `.string`, строковое поле сущности, которое сравнивается.
* **match** — `string`, искомое строковое значение.

#### Описание и логика работы
Линейно перебирает edict'ы, начиная со следующего после `start`, и возвращает первую живую сущность, у которой содержимое строкового поля точно равно `match`. Сравнение идёт по строковому содержимому, а не по номеру tempstring. При несуществующем поле builtin выдаёт ошибку, а при отсутствии совпадений возвращает `world`. По коду движка поиск пустой строки допускается, но это почти всегда признак ошибки и в режиме разработки может сопровождаться предупреждением.

#### Практические сценарии использования
```qc
entity(entity start) FindNextMonster =
{
    return find(start, classname, "monster_demon1");
};

void() PrintAllDemons =
{
    entity e;

    for (e = find(world, classname, "monster_demon1"); e; e = find(e, classname, "monster_demon1"))
        dprint(etos(e), " is a demon\n");
};
```

---

### findchain
`entity(.string field, string match, optional .entity chainfield) findchain = #402;`

* **field** — `.string`, поле для строкового сравнения.
* **match** — `string`, искомое значение.
* **chainfield** — `.entity`, необязательное поле-связка; по умолчанию используется `.chain`.

#### Описание и логика работы
За один проход находит все подходящие сущности и возвращает голову цепочки. Остальные совпадения связываются через `chainfield`, поэтому этот builtin удобнее, чем многократные вызовы `find`, когда нужно обработать весь набор за один кадр и не платить за повторные переходы между QC и движком. По реализации FTEQW поле связи действительно перезаписывается у найденных сущностей, поэтому используйте отдельное поле, если `.chain` уже занято другой логикой.

#### Практические сценарии использования
```qc
void() WakeSleepingOgres =
{
    entity e;

    for (e = findchain(classname, "monster_ogre"); e; e = e.chain)
        if (e.spawnflags & 1)
            e.nextthink = time + 0.1;
};
```

---

### findchainflags
`entity(.float fld, float match, optional .entity chainfield) findchainflags = #450;`

* **fld** — `.float`, числовое поле, трактуемое как битовая маска.
* **match** — `float`, набор битов, из которых нужен хотя бы один.
* **chainfield** — `.entity`, поле для построения цепочки; по умолчанию `.chain`.

#### Описание и логика работы
Линейно проходит по сущностям и включает в результат все edict'ы, у которых `(field & match) != 0`. Это именно масочный поиск, а не сравнение на равенство. Как и другие chain-варианты, builtin разрушает прежнее содержимое выбранного `chainfield` у найденных сущностей, зато позволяет получить весь набор за один вызов. Неверная ссылка на поле приводит к builtin [error](12-system-debug-builtins.md#error).

#### Практические сценарии использования
```qc
void() TouchAllGroundedItems =
{
    entity e;

    for (e = findchainflags(flags, FL_ONGROUND); e; e = e.chain)
        if (e.flags & FL_ITEM)
            dprint("ground item: ", etos(e), "\n");
};
```

---

### findchainfloat
`entity(.float fld, float match, optional .entity chainfield) findchainfloat = #403;`

* **fld** — `.float`, поле для сравнения по сырому числовому значению.
* **match** — `float`, искомое значение.
* **chainfield** — `.entity`, поле для построения цепочки; по умолчанию `.chain`.

#### Описание и логика работы
Строит цепочку из всех сущностей, у которых выбранное `.float`-слот поля равен `match`. В отличие от `find`, здесь не сравниваются строки, но и «сырой slot compare для чего угодно» это не заменяет: для `.entity` и прочих нестроковых ссылочных значений правильнее `findfloat`, а для битовых масок — `findchainflags`. Как и у `findchain`, поле связи будет перезаписано у каждого совпадения.

#### Практические сценарии использования
```qc
void() FindSpecificFrame =
{
    entity e;

    for (e = findchainfloat(frame, 0); e; e = e.chain)
        if (e.model != "")
            dprint("idle entity: ", etos(e), "\n");
};
```

---

### findflags
`entity(entity start, .float field, float match) findflags = #449;`

* **start** — `entity`, начальный edict; обычно `world` или прошлый результат.
* **field** — `.float`, числовое поле-маска.
* **match** — `float`, набор требуемых битов.

#### Описание и логика работы
Ищет следующую сущность, у которой в `field` установлен хотя бы один бит из `match`. Это линейный поиск по списку edict'ов с возвратом первого совпадения; дальше перебор продолжают повторным вызовом с найденной сущностью в `start`. Если поле передано неверно, движок выдаёт ошибку. При неудаче возвращается `world`.

#### Практические сценарии использования
```qc
void() PrintFlyingThings =
{
    entity e;

    for (e = findflags(world, flags, FL_FLY); e; e = findflags(e, flags, FL_FLY))
        dprint("flying: ", etos(e), "\n");
};
```

---

### findfloat
`entity(entity start, .__variant fld, __variant match) findfloat = #98;`

* **start** — `entity`, начальный edict.
* **fld** — `.__variant`, поле сущности для сравнения по сырому значению.
* **match** — `__variant`, искомое значение того же представления.

#### Описание и логика работы
По смыслу это «нестроковый find»: движок сравнивает сырое хранимое значение поля, а не текстовую форму. Поэтому builtin подходит для `.float`, `.entity`, целочисленных флагов и похожих полей, где важно полное совпадение бита-в-бит. Поиск остаётся линейным, ошибки поля остаются фатальными для вызова, а отсутствие результата выражается через `world`.

#### Практические сценарии использования
```qc
void() PrintDoorsAtFrame5 =
{
    entity e;

    for (e = findfloat(world, frame, 5); e; e = findfloat(e, frame, 5))
        if (e.classname == "func_door")
            dprint("door frame 5: ", etos(e), "\n");
};
```

---

### findradius
`entity(vector org, float rad, optional .entity chainfield) findradius = #22;`

* **org** — `vector`, центр поиска.
* **rad** — `float`, радиус сферы.
* **chainfield** — `.entity`, необязательное поле связи; по умолчанию `.chain`.

#### Описание и логика работы
Ищет все сущности в пределах радиуса и возвращает голову цепочки совпадений. FTEQW отмечает у этого builtin проблемы реентерабельности, поэтому при вложенных поисках и сложной логике безопаснее современный `findradius_list`, но сам `findradius` остаётся совместимым и очень распространённым. По коду движка не-solid сущности обычно пропускаются, если только они не помечены `FL_FINDABLE_NONSOLID` или не включены соответствующие compatibility cvar'ы; расстояние может считаться либо до центра, либо до bbox в зависимости от настроек движка.

#### Практические сценарии использования
```qc
void() HurtNearby =
{
    entity e;

    for (e = findradius(self.origin, 128); e; e = e.chain)
        if (e.takedamage)
            e.health = e.health - 10;
};
```

---

### nextent
`entity(entity e) nextent = #47;`

* **e** — `entity`, текущая позиция в списке edict'ов.

#### Описание и логика работы
Возвращает следующую живую сущность после `e`, пропуская freed slots. Это самый низкоуровневый способ полного обхода списка edict'ов без фильтрации по полям. Когда список заканчивается, builtin возвращает `world`, поэтому цикл обычно строят как `for (e = nextent(world); e; e = nextent(e))`.

#### Практические сценарии использования
```qc
void() CountSolidEntities =
{
    entity e;
    float count;

    count = 0;
    for (e = nextent(world); e; e = nextent(e))
        if (e.solid)
            count = count + 1;

    dprint("solid ents: ", ftos(count), "\n");
};
```

---

### setmodel
`void(entity e, string m) setmodel = #3;`

* **e** — `entity`, сущность, которой назначается модель.
* **m** — `string`, имя уже известной движку модели.

#### Описание и логика работы
Назначает сущности [`model`](../39-entity-keys-reference/01-worldspawn-common-keys.md#model) и `modelindex`, а затем перелинковывает её коллизионное состояние. Для inline BSP-моделей builtin автоматически берёт `mins`/`maxs` из модели; для alias/sprite-моделей точные размеры зависят от [`sv_gameplayfix_setmodelrealbox`](../38-cvars-reference/04-network-server-cvars.md#sv_gameplayfix_setmodelrealbox), поэтому комментарии FTE прямо советуют после небрашеовых моделей явно делать `setsize`. Если модель не была precache'нута заранее, движок пытается разобраться сам, но это сопровождается предупреждениями и может иметь сетевые последствия для уже подключённых клиентов.

#### Практические сценарии использования
```qc
void() SpawnTorchModel =
{
    entity e;

    precache_model("progs/flame.mdl");
    e = spawn();
    e.classname = "torch_flame";
    setmodel(e, "progs/flame.mdl");
    setsize(e, '-8 -8 0', '8 8 56');
    setorigin(e, '320 64 24');
};
```

---

### setmodelindex
`void(entity e, float mdlindex) setmodelindex = #333;`

* **e** — `entity`, сущность, которой назначается модель.
* **mdlindex** — `float`, индекс из precache-списка модели.

#### Описание и логика работы
Документация в `fteextensions.qc` описывает builtin как вариант `setmodel`, принимающий уже готовый precache index вместо имени. Практический смысл — избежать лишнего поиска по строке, если индекс уже сохранён в данных мода. В актуальных сборках FTEQW серверная таблица builtins всё ещё помечает этот номер как compatibility stub, поэтому перед использованием разумно проверять поддержку через [`checkbuiltin`](12-system-debug-builtins.md#checkbuiltin)/`builtin_find` и иметь запасной путь через обычный `setmodel`.

#### Практические сценарии использования
```qc
float g_crate_modelindex;

void() WorldInitModels =
{
    g_crate_modelindex = getmodelindex("progs/armor.mdl");
};

void() SpawnCachedModel =
{
    entity e;

    e = spawn();
    setmodelindex(e, g_crate_modelindex);
    setorigin(e, '64 64 32');
};
```

---

### setorigin
`void(entity e, vector o) setorigin = #2;`

* **e** — `entity`, перемещаемая сущность.
* **o** — `vector`, новая точка `origin`.

#### Описание и логика работы
Корректный способ мгновенно переместить сущность без обычной физики движения. Builtin обновляет `origin`, пересчитывает `absmin`/`absmax` и перелинковывает edict в пространственных структурах мира; простое присваивание `e.origin = ...` этого не делает и оставляет сломанные коллизии и поиск по area links. Для teleport/spawn/ручной коррекции позиции используйте именно `setorigin`.

#### Практические сценарии использования
```qc
void(entity e, vector destination) TeleportEntity =
{
    e.velocity = '0 0 0';
    setorigin(e, destination);
};
```

---

### setsize
`void(entity e, vector min, vector max) setsize = #4;`

* **e** — `entity`, сущность, чей bbox меняется.
* **min** — `vector`, локальный минимум bbox.
* **max** — `vector`, локальный максимум bbox.

#### Описание и логика работы
Меняет `mins`, `maxs` и производное `size`, а затем перелинковывает сущность. Это влияет и на столкновения, и на результаты `findradius`, и на сетевую видимость через `absmin`/`absmax`. Нулевой bbox формально допустим, но превращает entity почти в точку и часто приводит к неожиданным трассировкам, поэтому для реальных игровых объектов размеры лучше задавать явно и сразу после `setmodel`.

#### Практические сценарии использования
```qc
void() SpawnTriggerBox =
{
    entity e;

    e = spawn();
    e.classname = "trigger_secret";
    e.solid = SOLID_TRIGGER;
    setsize(e, '-32 -32 -24', '32 32 24');
    setorigin(e, '512 256 64');
};
```

---

### checkbottom
`float(entity ent) checkbottom = #40;`

* **ent** — `entity`, сущность, для которой нужно проверить опору.

#### Описание и логика работы
Делает дорогую проверку того, что bbox сущности действительно опирается на твёрдую поверхность. Builtin полезен после ручных перемещений, нестандартной физики или сложных step-up/step-down сценариев. Возвращает ненулевое значение при успехе, но сам не меняет позицию и не ставит `FL_ONGROUND` — это остаётся за вашей логикой.

#### Практические сценарии использования
```qc
float(entity e) SafeGrounded =
{
    if (!(e.flags & FL_ONGROUND))
        return FALSE;

    return checkbottom(e);
};
```

---

### droptofloor
`float() droptofloor = #34;`

* Аргументы отсутствуют; builtin работает с `self`.

#### Описание и логика работы
Мгновенно сдвигает `self` вдоль направления гравитации вниз до первого твёрдого упора. По документации и коду FTEQW вызов проваливается, если объект уже находится в solid или если падать пришлось бы дальше [`pr_droptofloorunits`](../38-cvars-reference/01-video-rendering-cvars.md#pr_droptofloorunits); при успехе движок выставляет `FL_ONGROUND`, запоминает `groundentity` и обновляет линковку. Это удобный способ «усадить» предметы и декорации на пол после `spawn`.

#### Практические сценарии использования
```qc
void() PlaceAmmoOnFloor =
{
    self = spawn();
    self.classname = "item_shells";
    setmodel(self, "maps/b_shell0.bsp");
    setsize(self, '-16 -16 -24', '16 16 32');
    setorigin(self, '400 220 256');
    if (!droptofloor())
        remove(self);
};
```

---

### walkmove
`float(float yaw, float dist, optional float settraceglobals) walkmove = #32;`

* **yaw** — `float`, угол движения в градусах относительно текущей гравитационной оси.
* **dist** — `float`, длина шага.
* **settraceglobals** — `float`, если ненулевой, builtin заполняет `trace_*` результатом шага.

#### Описание и логика работы
Пытается сдвинуть `self` шагом обычной «наземной» логики Quake. По реализации вызов сработает только для сущностей с `FL_ONGROUND`, `FL_FLY` или `FL_SWIM`; результат возвращается как булево число, а при успехе/неудаче touch-события могут быть вызваны автоматически. Это хороший низкоуровневый кирпичик для монстров и NPC, которым нужно идти под своим углом, но не писать свой `World_movestep` на стороне движка.

#### Практические сценарии использования
```qc
float() StepForward16 =
{
    // Двигаем self на 16 units в сторону self.ideal_yaw.
    return walkmove(self.ideal_yaw, 16, TRUE);
};
```

---

### movetogoal
`void(float step) movetogoal = #67;`

* **step** — `float`, желаемая длина продвижения к `goalentity`.

#### Описание и логика работы
Запускает стандартную монстровую логику Quake, которая пытается продвинуть `self` к `goalentity`, учитывая ступеньки и обход локальных препятствий. Это высокоуровневее, чем `walkmove`: вы не задаёте конкретный вектор сами, а просите движок сделать «разумный шаг» по уже установленной цели. Builtin особенно удобен для классического AI, но из-за встроенных эвристик хуже подходит для детерминированной кастомной физики.

#### Практические сценарии использования
```qc
void() MonsterChaseThink =
{
    if (self.goalentity)
        movetogoal(20);

    self.nextthink = time + 0.1;
};
```

---

### touchtriggers
`void(optional entity ent, optional vector neworigin) touchtriggers = #279;`

* **ent** — `entity`, сущность, для которой нужно проверить касание триггеров; по умолчанию `self`.
* **neworigin** — `vector`, необязательная новая позиция перед проверкой.

#### Описание и логика работы
Принудительно проверяет контакт сущности со всеми `SOLID_TRIGGER`, которые её пересекают, и вызывает их `touch`-логику. Если передан `neworigin`, builtin сначала меняет origin указанной сущности и перелинковывает её, а затем уже проверяет триггеры. Это полезно после нестандартных перемещений, серверных телепортов и ручной коррекции позиций, когда обычный path физики не успел бы сам породить `touch`.

#### Практические сценарии использования
```qc
void(entity e, vector destination) TeleportAndFireTriggers =
{
    touchtriggers(e, destination);
};
```

---

### pointcontents
`float(vector pos) pointcontents = #41;`

* **pos** — `vector`, точка в мировых координатах.

#### Описание и логика работы
Проверяет, какой content находится в конкретной точке, и возвращает одно из значений `CONTENT_*`/`CONTENTS_*`. Это точечный запрос: пустая точка ещё не гарантирует, что туда поместится игрок или монстр с bbox, поэтому для проверки проходимости объёмного объекта нужен `tracebox`. Функция полезна для воды, лавы, слизи, неба, лестниц и проверки «не застрял ли мы в solid». 

#### Практические сценарии использования
```qc
float(vector p) IsWaterPoint =
{
    return pointcontents(p) == CONTENT_WATER;
};
```

---

### checkclient
`entity() checkclient = #17;`

* Аргументы отсутствуют.

#### Описание и логика работы
Возвращает «очередного» игрока-кандидата для AI-проверок, циклически перебирая клиентов во времени. По серверной реализации FTEQW движок дополнительно фильтрует кандидата по грубой видимости: если `self` слишком далеко или вообще не находится в PVS кандидата, вернётся `world`. Builtin задуман как дешёвый предварительный этап перед более дорогими `traceline`, а не как окончательный ответ «кого точно видно».

#### Практические сценарии использования
```qc
void() AcquireRoughTarget =
{
    entity e;

    e = checkclient();
    if (e)
        self.enemy = e;
};
```

---

### checkpvs
`float(vector viewpos, entity entity) checkpvs = #240;`

* **viewpos** — `vector`, позиция наблюдателя.
* **entity** — `entity`, сущность, которую проверяют на потенциальную видимость.

#### Описание и логика работы
Возвращает ненулевое значение, если указанный edict потенциально попадает в PVS/visibility set из точки `viewpos`. Это грубая пространственная проверка без точной геометрической трассировки: она хороша для сетевой фильтрации, активации AI и раннего отсечения expensive-логики. Если карта ещё не загружена или мир не даёт PVS-данных, FTEQW либо вернёт `FALSE`, либо `TRUE` в режиме, где такая проверка невозможна и отбрасывать по PVS бессмысленно.

#### Практические сценарии использования
```qc
float(entity viewer, entity target) CanPotentiallySee =
{
    return checkpvs(viewer.origin + viewer.view_ofs, target);
};
```

---

### num_for_edict
`float(entity ent) num_for_edict = #512;`

* **ent** — `entity`, edict, номер которого нужен.

#### Описание и логика работы
Возвращает числовой индекс edict'а в таблице сущностей. Это полезно для сериализации, отладочного вывода и передачи идентификаторов туда, где полноценная entity reference неудобна или не переживёт границу VM/сети. В отличие от `etof`, builtin выражает именно «номер сущности», а не неявное приведение типа.

#### Практические сценарии использования
```qc
void(entity e) PrintEntnum =
{
    dprint("entnum=", ftos(num_for_edict(e)), "\n");
};
```

---

### edict_num
`entity(float entnum) edict_num = #459;`

* **entnum** — `float`, индекс edict'а.

#### Описание и логика работы
Преобразует номер сущности обратно в `entity`. Если номер выходит за известный диапазон или не соответствует валидному edict'у, FTEQW возвращает `world`, поэтому результат стоит проверять перед использованием. Этот builtin естественно сочетается с `num_for_edict` и нужен там, где сущности временно хранятся в числовом виде.

#### Практические сценарии использования
```qc
entity(float n) ResolveSavedEnt =
{
    entity e;

    e = edict_num(n);
    if (!e)
        return world;
    return e;
};
```

---

### etof
`float(entity e) etof = #79;`

* **e** — `entity`, ссылка на сущность.

#### Описание и логика работы
Приводит entity reference к числу. Это старый low-level способ протащить ссылку на сущность через API, который ждёт `float`, но по смыслу обычно безопаснее и яснее использовать `num_for_edict`, если вам нужен именно индекс edict'а. На уровне практики `etof` полезен в коде совместимости и метапрограммировании, а не в обычной игровой логике.

#### Практические сценарии использования
```qc
float(entity e) SaveEntityAsFloat =
{
    return etof(e);
};
```

---

### ftoe
`entity(float f) ftoe = #80;`

* **f** — `float`, числовое представление entity reference.

#### Описание и логика работы
Обратное преобразование к `etof`: принимает число и трактует его как ссылку на сущность. Это не делает поиск по номеру с защитой от диапазона так явно, как `edict_num`, поэтому builtin обычно используют только вместе с `etof` или в legacy-коде, который именно так и хранит entity references. Для читаемого современного SSQC чаще подходят `num_for_edict` и `edict_num`.

#### Практические сценарии использования
```qc
entity(float token) RestoreEntityFromFloat =
{
    return ftoe(token);
};
```

---

### etos
`string(entity ent) etos = #65;`

* **ent** — `entity`, сущность для строкового представления.

#### Описание и логика работы
Возвращает временную строку с текстовым представлением entity reference. Обычно это используют для отладки, логов и [`bprint`](12-system-debug-builtins.md#bprint)/[`dprint`](12-system-debug-builtins.md#dprint), когда нужно быстро увидеть идентичность edict'а в консоли. Формат строки зависит от движка, поэтому не стоит хранить его как стабильный savegame/network-идентификатор.

#### Практические сценарии использования
```qc
void(entity e) DebugEntityRef =
{
    dprint("debug ref: ", etos(e), "\n");
};
```

---

### wasfreed
`float(entity ent) wasfreed = #353;`

* **ent** — `entity`, ссылка, которую нужно проверить.

#### Описание и логика работы
Быстро проверяет, помечен ли указанный edict как свободный. Комментарий FTE отдельно предупреждает, что надёжность ограничена коротким окном до повторного использования слота, поэтому builtin хорош как защитный отладочный барьер или мягкая страховка после `remove`, но не как полноценная ownership-модель. Вне этого окна ссылка уже может указывать на новую сущность того же слота.

#### Практические сценарии использования
```qc
float(entity e) IsReferenceStillDead =
{
    if (!e)
        return TRUE;

    return wasfreed(e);
};
```

---

### copyentity
`entity(entity from, optional entity to) copyentity = #400;`

* **from** — `entity`, источник полей.
* **to** — `entity`, необязательная сущность-получатель; если не указана, движок создаёт новую.

#### Описание и логика работы
Копирует все поля одного edict'а в другой и затем перелинковывает результат. По коду FTEQW обе сущности должны быть живыми, `to` не может быть read-only, а размеры объектов должны совпадать; при отсутствии `to` выделяется новый edict. Builtin полезен для клонов, временных снапшотов и переноса состояния, но помните, что вы копируете и ссылки на другие сущности, и все служебные поля сразу.

#### Практические сценарии использования
```qc
entity(entity source) CloneSimpleEntity =
{
    entity clone;

    clone = copyentity(source);
    clone.classname = strcat(source.classname, "_clone");
    clone.origin_z = clone.origin_z + 64;
    setorigin(clone, clone.origin);
    return clone;
};
```

---

### aim
`vector(entity player, float missilespeed) aim = #44;`

* **player** — `entity`, игрок или другой стрелок, для которого считается автоприцел.
* **missilespeed** — `float`, скорость снаряда; исторически используется интерфейсом builtin.

#### Описание и логика работы
Возвращает скорректированный вариант `v_forward` для Quake-style auto-aim. Перед вызовом нужно подготовить базовые направления через [`makevectors(player.v_angle)`](01-math-vector-builtins.md#makevectors); builtin затем подберёт врага с `takedamage == DAMAGE_AIM`, ближайшего к прицелу в пределах [`acos`](01-math-vector-builtins.md#acos)([`sv_aim`](../38-cvars-reference/04-network-server-cvars.md#sv_aim)), и скорректирует главным образом вертикальную составляющую. Это удобно для классического оружия в духе id1, особенно для клавиатурного управления без свободного вертикального прицела.

#### Практические сценарии использования
```qc
vector(entity shooter) RocketDirection =
{
    makevectors(shooter.v_angle);
    return aim(shooter, 1000);
};
```

---

### traceline
`void(vector v1, vector v2, float flags, entity ent) traceline = #16;`

* **v1** — `vector`, начало луча.
* **v2** — `vector`, конец луча.
* **flags** — `float`, режим трассировки: `MOVE_NORMAL`, `MOVE_NOMONSTERS`, `MOVE_MISSILE`, `MOVE_HITMODEL`, `MOVE_TRIGGERS`, `MOVE_EVERYTHING`, `MOVE_LAGGED` и т. д.
* **ent** — `entity`, сущность-исключение и источник дополнительных правил трассы.

#### Описание и логика работы
Трассирует тонкий луч и записывает результат в `trace_*` globals. FTEQW не бьёт лучом в `ent`, его `owner` и дочерние по owner сущности, а часть поведения берёт из самой сущности — например, contents mask и связанные настройки. Ключевые флаги описаны в комментариях `fteextensions.qc`: `MOVE_NOMONSTERS` игнорирует небрашеовые сущности, `MOVE_MISSILE` расширяет проверку против `FL_MONSTER`, `MOVE_HITMODEL` тестирует реальную геометрию модели, `MOVE_TRIGGERS` останавливается на триггерах, `MOVE_EVERYTHING` цепляет даже non-solid, `MOVE_LAGGED` включает антиреговой откат позиций для серверной компенсации лагов.

#### Практические сценарии использования
```qc
float(entity attacker, entity target) HasLineOfFire =
{
    vector start;
    vector finish;

    start = attacker.origin + attacker.view_ofs;
    finish = target.origin + target.view_ofs;
    traceline(start, finish, MOVE_NORMAL, attacker);
    return (trace_ent == target || trace_fraction == 1);
};
```

---

### tracebox
`void(vector start, vector mins, vector maxs, vector end, float [nomonsters](../38-cvars-reference/07-system-misc-cvars.md#nomonsters), entity ent) tracebox = #90;`

* **start** — `vector`, стартовая позиция bbox.
* **mins** — `vector`, локальный минимум коробки.
* **maxs** — `vector`, локальный максимум коробки.
* **end** — `vector`, конечная позиция.
* **nomonsters** — `float`, режим трассировки теми же флагами семейства `MOVE_*`.
* **ent** — `entity`, сущность-исключение/контекст трассы.

#### Описание и логика работы
То же самое, что `traceline`, но вместо математической линии используется объёмная коробка. Это основной инструмент для проверки «поместится ли сюда игрок/монстр/ящик», для нестандартных движений и teleport safety. Комментарий FTE предупреждает, что допустимые размеры коробки ограничены форматом BSP, особенно на q1bsp, поэтому экстремально большие bbox стоит тестировать осторожно.

#### Практические сценарии использования
```qc
float(vector p) CanPlayerStandHere =
{
    tracebox(p, '-16 -16 -24', '16 16 32', p, MOVE_NORMAL, world);
    return !trace_startsolid;
};
```

---

### tracetoss
`void(entity ent, entity ignore) tracetoss = #64;`

* **ent** — `entity`, сущность, для которой симулируют бросок.
* **ignore** — `entity`, сущность, которую нужно игнорировать при столкновениях.

#### Описание и логика работы
Симулирует баллистическое движение объекта с его текущими параметрами и заполняет `trace_*` глобалы местом/фактом ожидаемого столкновения. В коде FTEQW есть отдельное предупреждение, что world entity сюда передавать нельзя. Builtin полезен для grenade AI, проверки посадки предмета и предпросмотра броска до реального спавна projectile.

#### Практические сценарии использования
```qc
float(entity grenade, entity owner) WillHitSoon =
{
    tracetoss(grenade, owner);
    return trace_fraction < 1;
};
```

---

### traceon
`void() traceon = #29;`

* Аргументы отсутствуют.

#### Описание и логика работы
Включает трассировку/пошаговую диагностику выполнения QC. Это инструмент отладки: он полезен при поиске сложных runtime-проблем, но шумный и дорогой, поэтому в обычной игровой логике его не оставляют включённым. В FTE обычно удобнее использовать полноценный отладчик, а `traceon` держать как запасной консольный механизм.

#### Практические сценарии использования
```qc
void() DebugOneThink =
{
    traceon();
    self.nextthink = time + 0.1;
    traceoff();
};
```

---

### traceoff
`void() traceoff = #30;`

* Аргументы отсутствуют.

#### Описание и логика работы
Выключает режим трассировки QC, включённый через `traceon`. Сам по себе builtin ничего не возвращает и не меняет игровое состояние кроме отладочного режима VM. Полезен для локализации проблемы на коротком участке кода, чтобы не утонуть в лишнем выводе.

#### Практические сценарии использования
```qc
void() DebugMovementBlock =
{
    traceon();
    walkmove(self.ideal_yaw, 8, TRUE);
    traceoff();
};
```

---

### makestatic
`void(entity e) makestatic = #69;`

* **e** — `entity`, сущность, которую нужно превратить в static entity.

#### Описание и логика работы
Берёт render-состояние сущности, добавляет его в список static entities и тут же удаляет исходный edict. После этого объект больше не живёт как обычная серверная сущность: его нельзя дальше мутировать, думать, трогать и участвовать в solid-логике, но клиенты продолжают видеть статический визуальный слепок. Это полезно для факелов, декора, постоянных обломков и других объектов, которым не нужна динамика и регулярные updates.

#### Практические сценарии использования
```qc
void() SpawnStaticTorch =
{
    entity e;

    e = spawn();
    setmodel(e, "progs/flame.mdl");
    setorigin(e, '128 192 48');
    makestatic(e);
};
```

---

### setspawnparms
`void(entity player) setspawnparms = #78;`

* **player** — `entity`, обязательно клиентская сущность.

#### Описание и логика работы
Перезаписывает глобалы `parm1..parm16` значениями, сохранёнными для указанного клиента из предыдущей карты/респавна. FTEQW дополнительно проверяет, что вы действительно передали клиента, иначе выдаёт ошибку builtin. Это стандартный механизм переноса persistent-состояния между уровнями и при ручном повторном создании игрока.

#### Практические сценарии использования
```qc
void(entity pl) RestoreParmsBeforeRespawn =
{
    setspawnparms(pl);
    pl.health = parm1;
};
```

---

### spawnclient
`entity() spawnclient = #454;`

* Аргументы отсутствуют.

#### Описание и логика работы
Создаёт bot client в первом свободном клиентском слоте и возвращает его player edict. Если свободных клиентских слотов нет, FTEQW возвращает `world`. Это именно клиентский edict, а не обычная server-only сущность: такой объект участвует в scoreboard, userinfo и client-oriented логике как бот.

#### Практические сценарии использования
```qc
entity() SpawnPracticeBot =
{
    entity bot;

    bot = spawnclient();
    if (!bot)
        return world;

    bot.netname = "PracticeBot";
    return bot;
};
```

---

### dropclient
`void(entity player) dropclient = #453;`

* **player** — `entity`, клиент, которого нужно отключить.

#### Описание и логика работы
Помечает клиента на отключение. Для обычных сетевых клиентов это приводит к разрыву соединения; для loopback/local client FTEQW использует мягкий путь через [`disconnect`](../44-cli-commands-reference/02-client-ui-commands.md#disconnect), чтобы не ломать локальную сессию грубо. Builtin полезен для кика за отсутствие обязательного CSQC, для авторизации и админских команд.

#### Практические сценарии использования
```qc
void(entity pl) KickIfUnnamed =
{
    if (infokey(pl, "name") == "")
        dropclient(pl);
};
```

---

### runstandardplayerphysics
`void(entity ent) runstandardplayerphysics = #347;`

* **ent** — `entity`, игрок, к которому нужно применить стандартную физику.

#### Описание и логика работы
Просит движок выполнить обычную player-physics FTEQW, используя текущие `input_*` globals как вход. Именно это рекомендуют комментарии в `fteextensions.qc`, если вы в `SV_RunClientCommand` слегка изменили ввод, но не хотите полностью переписывать движение, прыжки, ступеньки и плавание. Builtin особенно полезен в prediction-sensitive режимах, где самописная физика быстро приводит к рассинхронизации с CSQC.

#### Практические сценарии использования
```qc
void() SV_RunClientCommand =
{
    if (self.flags & FL_WATERJUMP)
        input_movevalues_z = 0;

    runstandardplayerphysics(self);
};
```

---

### getstati
`int(float stnum) getstati = #330;`

* **stnum** — `float`, индекс stat.

#### Описание и логика работы
Доступно в CSQC. Возвращает целочисленное значение stat без потери точности, когда stat на сервере был зарегистрирован как `EV_INTEGER`. Это правильный выбор для packed-битов и всех случаев, где 32-битное значение нельзя безопасно пропускать через обычный float.

#### Практические сценарии использования
```qc
void() CSQC_UpdateView =
{
    int items;

    items = getstati(STAT_ITEMS);
    if (items & IT_KEY1)
        drawstring('16 16 0', "Silver key", '8 8 0', '1 1 1', 1, 0);
};
```

---

### getstatf
`float(float stnum, optional float firstbit, optional float bitcount) getstatf = #331;`

* **stnum** — `float`, индекс stat.
* **firstbit** — `float`, необязательный сдвиг первого читаемого бита.
* **bitcount** — `float`, необязательное число бит.

#### Описание и логика работы
Доступно в CSQC. В обычном режиме читает числовой stat как `float`; если передать `firstbit` и `bitcount`, builtin извлекает битовое поле из integer-представления статов, что отдельно рекомендовано для `STAT_ITEMS`. Это удобно для packed-флагов из классического протокола без лишнего ручного побитового кода.

#### Практические сценарии использования
```qc
void() CSQC_UpdateView =
{
    float runes;

    runes = getstatf(STAT_ITEMS, 23, 11);
    if (runes)
        drawstring('16 28 0', strcat("runes=", ftos(runes)), '8 8 0', '1 1 1', 1, 0);
};
```

---

### getstats
`string(float stnum) getstats = #332;`

* **stnum** — `float`, индекс string stat.

#### Описание и логика работы
Доступно в CSQC. Возвращает string stat как tempstring; в современных расширениях FTE строковые статы живут в отдельном пространстве имён и не ограничены жёстко 15 символами, как в старых packed-схемах. Это лучший способ выводить серверно-синхронизированный HUD-текст, названия режимов, таймеры и другие строки без ручной распаковки.

#### Практические сценарии использования
```qc
void() CSQC_UpdateView =
{
    string hint;

    hint = getstats(40);
    if (hint != "")
        drawstring('16 40 0', hint, '8 8 0', '1 0.8 0.2', 1, 0);
};
```

---

### clientstat
`void(float num, float type, .__variant fld) clientstat = #232;`

* **num** — `float`, номер stat; FTE рекомендует диапазон `32..127`.
* **type** — `float`, один из `EV_*`, обычно `EV_FLOAT`, `EV_STRING`, `EV_INTEGER` или `EV_ENTITY`.
* **fld** — `.__variant`, поле player entity, которое сервер должен отправлять каждому клиенту как его персональный stat.

#### Описание и логика работы
Это серверный mapping builtin: он не читает stat прямо сейчас, а настраивает, какое поле каждого клиента будет реплицироваться в указанный stat slot. Из-за персональной природы `clientstat` каждый игрок получает значение из собственного поля, а не из общего глобала. В результате builtin отлично подходит для здоровья, патронов, активного оружия и любой HUD-информации, специфичной именно для владельца client.

#### Практические сценарии использования
```qc
void() WorldInit =
{
    clientstat(32, EV_FLOAT, health);
    clientstat(33, EV_FLOAT, armorvalue);
};
```

---

### globalstat
`void(float num, float type, string name) globalstat = #233;`

* **num** — `float`, номер stat.
* **type** — `float`, тип данных stat через `EV_*`.
* **name** — `string`, имя глобальной переменной, которую нужно реплицировать.

#### Описание и логика работы
Похож на `clientstat`, но привязывает stat к одному глобальному значению для всех клиентов. Это удобно для номера волны, времени раунда, имени режима игры и других общих HUD-параметров. Имя переменной передаётся строкой, поэтому переименование глобала требует обновить и вызов `globalstat`.

#### Практические сценарии использования
```qc
float g_roundtime;
string g_gamemode;

void() WorldInit =
{
    globalstat(40, EV_FLOAT, "g_roundtime");
    globalstat(41, EV_STRING, "g_gamemode");
};
```

---

### forceinfokey
`void(entity player, string key, string value) forceinfokey = #213;`

* **player** — `entity`, клиент, чьи данные меняются.
* **key** — `string`, имя userinfo-ключа.
* **value** — `string`, новое значение.

#### Описание и логика работы
Меняет userinfo сервера напрямую, не заставляя клиента переподключаться и не трогая его локальный config. Важная особенность FTE — builtin позволяет выставлять и `*`-ключи вроде `*[spectator](../38-cvars-reference/07-system-misc-cvars.md#spectator)`, то есть работать не только с «пользовательскими» полями, но и со служебными. Это сильный инструмент админки и game-rules, но злоупотреблять им не стоит: вы меняете сетевое представление игрока на сервере, а не просто локальную переменную QC.

#### Практические сценарии использования
```qc
void(entity pl) ForceSpectator =
{
    forceinfokey(pl, "*spectator", "1");
    forceinfokey(pl, "team", "observer");
};
```

---

### serverkey
`string(string key) serverkey = #354;`

* **key** — `string`, ключ из публичного `serverinfo`.

#### Описание и логика работы
Читает значение из публичной строки `serverinfo`. Builtin доступен в CSQC и SSQC; если ключ хранит бинарные данные, строковый вариант обрежет их по первому `NUL`, поэтому для blobs нужны соседние blob-API. Полезен для чтения режима, лимитов, имени сервера и других публично объявленных параметров без прямой работы с whole infostring.

#### Практические сценарии использования
```qc
void() PrintHostname =
{
    dprint("hostname: ", serverkey("hostname"), "\n");
};
```

---

### infokey
`string(entity e, string key) infokey = #80;`

* **e** — `entity`, либо `world`, либо конкретный игрок.
* **key** — `string`, имя интересующего ключа.

#### Описание и логика работы
Если `e == world`, builtin ищет ключ в `serverinfo`, а при отсутствии — в [`localinfo`](../44-cli-commands-reference/04-server-multiplayer-commands.md#localinfo); если `e` — игрок, читается его userinfo. FTE отдельно поддерживает специальные ключи вроде `ip`, `realip`, `csqcactive` и другие, даже если формально они не лежат в обычной `\key\value` строке. Возвращаемое значение — tempstring, а отсутствие ключа обычно даёт пустую строку.

#### Практические сценарии использования
```qc
void(entity pl) WelcomePlayer =
{
    string name;

    name = infokey(pl, "name");
    sprint(pl, strcat("Welcome, ", name, "\n"));
};
```

---

### infoadd
`infostring(infostring old, string key, string value) infoadd = #226;`

* **old** — `infostring`, исходная `\key\value` строка.
* **key** — `string`, имя ключа.
* **value** — `string`, новое значение.

#### Описание и логика работы
Создаёт новую infostring, в которой значение `key` заменено или добавлено. По документации key и value не могут содержать обратный слеш, иначе формат строки ломается. Builtin не меняет serverinfo/userinfo сам по себе — он работает именно со строковым значением, поэтому хорошо подходит для промежуточной сборки пакетов метаданных.

#### Практические сценарии использования
```qc
string() BuildVoteInfo =
{
    infostring info;

    info = "";
    info = infoadd(info, "map", "dm3");
    info = infoadd(info, "mode", "ffa");
    return info;
};
```

---

### infoget
`string(infostring info, string key) infoget = #227;`

* **info** — `infostring`, строка вида `\key\value\...`.
* **key** — `string`, имя читаемого поля.

#### Описание и логика работы
Читает одно значение из уже готовой infostring и возвращает его как tempstring. Это быстрый и удобный разборщик quakeworld-style метаданных, когда вы хотите хранить компактный набор пар ключ/значение в одной строке. Если ключ отсутствует, builtin возвращает пустую строку.

#### Практические сценарии использования
```qc
void() ReadVoteInfo =
{
    infostring info;

    info = "\\map\\dm3\\mode\\ffa";
    dprint("mode=", infoget(info, "mode"), "\n");
};
```

---

### matchclientname
`entity(string match, optional float matchnum) matchclientname = #241;`

* **match** — `string`, селектор клиента: имя, его часть или другой шаблон, который понимает серверный resolver.
* **matchnum** — `float`, необязательный номер совпадения при множественных матчах.

#### Описание и логика работы
Ищет клиента тем же механизмом, которым сервер обычно резолвит имена в консольных командах. Важная деталь FTEQW: если `matchnum` не задан и совпадений больше одного, builtin возвращает `world`, чтобы не выбирать неоднозначного игрока молча. Если `matchnum` указан, совпадения можно перебирать по порядку, что удобно для команд админки и vote-логики.

#### Практические сценарии использования
```qc
entity(string fragment) FindUniquePlayer =
{
    return matchclientname(fragment);
};
```

---

### entityfieldname
`string(float fieldnum) entityfieldname = #497;`

* **fieldnum** — `float`, индекс именованного entity field.

#### Описание и логика работы
Возвращает имя поля сущности по его числовому индексу из reflection API. Полезно для отладочных дампов, generic-редакторов и инспекторов сущностей. Если индекс выходит за диапазон, движок возвращает пустое/нулевое значение.

#### Практические сценарии использования
```qc
void() PrintFirstFieldName =
{
    dprint(entityfieldname(0), "\n");
};
```

---

### entityfieldtype
`float(float fieldnum) entityfieldtype = #498;`

* **fieldnum** — `float`, индекс поля.

#### Описание и логика работы
Возвращает тип поля как одно из значений `EV_*`. Обычно builtin используют вместе с `numentityfields` и `entityfieldname`, чтобы безопасно перечислять поля или строить generic editor без жёстко прошитого списка. Для неверного индекса FTEQW возвращает `0`.

#### Практические сценарии использования
```qc
void() DescribeField0 =
{
    dprint(entityfieldname(0), ": type=", ftos(entityfieldtype(0)), "\n");
};
```

---

### numentityfields
`float() numentityfields = #496;`

* Аргументы отсутствуют.

#### Описание и логика работы
Возвращает количество именованных entity fields, известных текущей VM. Это число отражает именно набор имён, а не физический размер edict в слотах памяти; в документации отдельно отмечено, что vector-поля учитываются не так интуитивно, как кажется. Builtin нужен для безопасного перебора reflection API без хардкода верхней границы.

#### Практические сценарии использования
```qc
void() PrintFieldCount =
{
    dprint("entity fields: ", ftos(numentityfields()), "\n");
};
```

---

### getentityfieldstring
`string(float fieldnum, entity ent) getentityfieldstring = #499;`

* **fieldnum** — `float`, индекс поля.
* **ent** — `entity`, сущность, из которой читается значение.

#### Описание и логика работы
Возвращает строковую форму произвольного поля сущности, что удобно для редакторов, дампов и generic debugging UI. FTEQW намеренно отфильтровывает часть «пустых» значений и некоторых служебных defaults, поэтому для нулевых/неинтересных полей можно получить пустой результат. Это reflection-инструмент, а не быстрый hot-path API: для нормальной игровой логики прямой доступ `ent.field` почти всегда лучше.

#### Практические сценарии использования
```qc
void(entity e, float fieldnum) DumpOneField =
{
    dprint(entityfieldname(fieldnum), "=", getentityfieldstring(fieldnum, e), "\n");
};
```

---

### putentityfieldstring
`float(float fieldnum, entity ent, string s) putentityfieldstring = #500;`

* **fieldnum** — `float`, индекс поля.
* **ent** — `entity`, сущность-приёмник.
* **s** — `string`, текст, который нужно распарсить в формат поля.

#### Описание и логика работы
Парсит текст и записывает его в выбранное поле сущности по тем же правилам, по которым движок обычно восстанавливает поля из текстового entity/savegame формата. Возвращаемое число отражает успех/результат парсинга, а при неверном индексе builtin даёт `0`. Это удобный companion к `getentityfieldstring`, если вы делаете редактор сущностей или серверную консольную команду для ручного патча полей.

#### Практические сценарии использования
```qc
void(entity e, float fieldnum, string value) PatchField =
{
    if (!putentityfieldstring(fieldnum, e, value))
        dprint("failed to write ", entityfieldname(fieldnum), "\n");
};
```

---

### getentitytoken
`string(optional string resetstring) getentitytoken = #355;`

* **resetstring** — `string`, необязательный новый источник токенизации; пустая строка означает reset на entity lump карты.

#### Описание и логика работы
Доступно в CSQC. Builtin возвращает следующий токен из entity lump карты или из другой строки, если вы заранее подали её в `resetstring`. Пустой `resetstring` просит FTE снова взять исходную entity-строку BSP и начать сначала; все результаты — tempstring. Это основной primitive для CSQC-парсинга worldspawn/entity lump при клиентских редакторах, minimap metadata и custom preload logic.

#### Практические сценарии использования
```qc
void() CSQC_WorldLoaded =
{
    string tok;

    tok = getentitytoken("");
    while ((tok = getentitytoken()) != "")
        if (tok == "classname")
            dprint("next classname key found\n");
};
```

---

### parseentitydata
`float(entity e, string s, optional float offset) parseentitydata = #613;`

* **e** — `entity`, уже созданная сущность, в которую будут записаны поля.
* **s** — `string`, текст в формате entity/savegame блока.
* **offset** — `float`, необязательное смещение начала парсинга.

#### Описание и логика работы
Читает одну текстовую запись сущности и применяет её поля к уже существующему edict'у. Builtin возвращает `<= 0` при ошибке и иначе выдаёт новое смещение, откуда можно продолжить разбор следующего блока; в UTF-8 VM FTE пересчитывает смещение в символьных индексах, а не голых байтах. Это отличный инструмент для собственных сериализаторов, редакторов и live import из `.ent`-подобных строк.

#### Практические сценарии использования
```qc
void() RestoreOneEntity =
{
    entity e;
    float ofs;

    e = spawn();
    ofs = parseentitydata(e, "{\"classname\" \"item_health\" \"origin\" \"128 64 32\"}");
    if (ofs <= 0)
        remove(e);
};
```

---

### getentity
`__variant(float entnum, float fieldnum) getentity = #504;`

* **entnum** — `float`, номер сетевой/серверной сущности.
* **fieldnum** — `float`, один из `GE_*` selectors.

#### Описание и логика работы
Доступно в CSQC. Builtin позволяет читать ограниченный набор полей у сущностей, которые не представлены как обычные CSQC edict'ы, если они известны клиенту и находятся в PVS. По `GE_*` можно получить origin, bbox, frame, effects, tag attachment и другие атрибуты; специальный `GE_MAXENTS` игнорирует `entnum` и сообщает верхнюю разумную границу перебора. При неизвестной сущности FTEQW возвращает нули/нулевой вектор и пишет предупреждение только в отладочный вывод, поэтому код должен уметь жить с «данных пока нет».

#### Практические сценарии использования
```qc
void() DrawMarkerForServerEnt =
{
    vector org;

    org = getentity(128, GE_ORIGIN);
    if (getentity(128, GE_ACTIVE))
        dprint("server ent origin: ", vtos(org), "\n");
};
```

---

### resourcestatus
`float(float resourcetype, float tryload, string resourcename) resourcestatus = #286;`

* **resourcetype** — `float`, один из `RESTYPE_*`.
* **tryload** — `float`, `0` только проверяет, `1` пытается подгрузить ресурс.
* **resourcename** — `string`, имя модели/звука/эффекта/другого ресурса.

#### Описание и логика работы
Возвращает состояние ресурса как `RESSTATE_NOTKNOWN`, `RESSTATE_NOTLOADED`, `RESSTATE_LOADING`, `RESSTATE_FAILED` или `RESSTATE_LOADED`. Важный нюанс FTE: `tryload = 0` старается не трогать ресурс и не провоцировать лишний stall, а `tryload = 1` даёт движку право попытаться вернуть выгруженный ресурс в память. Builtin удобен для асинхронного UI, ленивого precache-контроля и диагностики content problems.

#### Практические сценарии использования
```qc
void() CheckRocketModel =
{
    float state;

    state = resourcestatus(RESTYPE_MODEL, 0, "progs/missile.mdl");
    dprint("rocket model state=", ftos(state), "\n");
};
```

---

### physics_addforce
`void(entity e, vector force, vector relative_ofs) physics_addforce = #541;`

* **e** — `entity`, объект с `MOVETYPE_PHYSICS`.
* **force** — `vector`, импульс силы.
* **relative_ofs** — `vector`, несмотря на историческое имя параметра, мировая точка приложения импульса.

#### Описание и логика работы
Отправляет в backend rigid-body physics команду применить импульс к физическому объекту. Важно: в официальных сборках FTEQW эта подсистема обычно вообще не собрана, поэтому builtin недоступен без отдельной сборки движка с внешними ODE/Bullet-библиотеками; см. [отдельную заметку](../43-legacy-unused-features/bullet-ode-physics-external-only.md). Если поддержка собрана, но physics backend мира не запущен, вызов просто не даст эффекта. Используйте этот API для пинков, взрывов и других эффектов, где важен именно физический импульс, а не простое присваивание `velocity`.

#### Практические сценарии использования
```qc
void(entity box, vector dir) KickPhysicsBox =
{
    physics_addforce(box, dir * 400, box.origin);
};
```

---

### physics_addtorque
`void(entity e, vector torque) physics_addtorque = #542;`

* **e** — `entity`, физическая сущность.
* **torque** — `vector`, вращающий импульс.

#### Описание и логика работы
Добавляет вращающий импульс rigid-body объекту. Как и `physics_addforce`, этот builtin имеет смысл только в сборках движка с внешней ODE/Bullet-физикой; в обычных официальных бинарях его может не быть вовсе, см. [заметку о статусе подсистемы](../43-legacy-unused-features/bullet-ode-physics-external-only.md). Если backend уже доступен, это удобный способ закрутить бочку, лопасть или обломок так, чтобы вращение считалось самим physics engine.

#### Практические сценарии использования
```qc
void(entity barrel) SpinBarrel =
{
    physics_addtorque(barrel, '0 0 120');
};
```

---

### physics_enable
`void(entity e, float physics_enabled) physics_enable = #540;`

* **e** — `entity`, объект с физическим телом.
* **physics_enabled** — `float`, ненуль включает расчёт, `0` выключает.

#### Описание и логика работы
Включает или выключает обработку physics backend для `MOVETYPE_PHYSICS` entity. Этот builtin тоже зависит от отдельной сборки движка с внешними ODE/Bullet-библиотеками и в стандартной официальной сборке обычно отсутствует; см. [заметку о статусе подсистемы](../43-legacy-unused-features/bullet-ode-physics-external-only.md). Если поддержка присутствует, это прямой способ снизить CPU cost для тел, которые временно должны «замереть» и не участвовать в симуляции; сама сущность мира при этом не удаляется.

#### Практические сценарии использования
```qc
void(entity doorpart, float active) SetPhysicsSleeping =
{
    physics_enable(doorpart, active);
};
```

---

### terrain_edit
`__variant(float action, optional vector pos, optional float radius, optional float quant, ...) terrain_edit = #278;`

* **action** — `float`, одна из констант `TEREDIT_*`.
* **pos** — `vector`, позиция редактирования, если конкретное действие её требует.
* **radius** — `float`, радиус кисти/области действия.
* **quant** — `float`, сила/шаг/квант операции.
* **...** — дополнительные параметры, зависящие от конкретного `TEREDIT_*` действия.

#### Описание и логика работы
Выполняет операции live-редактирования heightmap terrain: подъём/сглаживание высот, отверстия, текстурные операции, сохранение, reset секций и т. д. Конкретный набор параметров зависит от `action`; в `fteextensions.qc` перечислены многочисленные `TEREDIT_*`, от `TEREDIT_HEIGHT_RAISE` до `TEREDIT_ENT_SET`. Если движок собран без terrain subsystem, FTEQW возвращает ложный/нулевой результат, поэтому код редактора должен проверять поддержку заранее.

#### Практические сценарии использования
```qc
void() RaiseTerrainUnderPlayer =
{
    terrain_edit(TEREDIT_HEIGHT_RAISE, self.origin, 96, 8);
};
```

---

### setattachment
`void(entity e, entity tagentity, string tagname) setattachment = #443;`

* **e** — `entity`, объект, который будет прикреплён.
* **tagentity** — `entity`, носитель тега/кости.
* **tagname** — `string`, имя тега в модели носителя.

#### Описание и логика работы
Привязывает сущность к тегу/кости другой модели, записывая нужные значения в `tag_entity` и `tag_index`. Если тег не найден, FTEQW оставляет индекс `0` и пишет диагностическое сообщение в debug output, но сам builtin не падает. Это базовый механизм для оружия в руках, навесных предметов, HUD-моделей на костях и любых q3-style model tags.

#### Практические сценарии использования
```qc
void(entity weapon, entity player) AttachWeaponModel =
{
    setattachment(weapon, player, "tag_weapon");
};
```

---

### checkcommand
`float(string name) checkcommand = #294;`

* **name** — `string`, имя команды, alias или cvar.

#### Описание и логика работы
Проверяет, существует ли введённое имя в консольной экосистеме движка. По реализации FTEQW результат кодируется числами: `1` — команда, `2` — alias, `3` — cvar, `0` — ничего не найдено. Это удобно для UI-консолей, автодополнения, sanity-check пользовательских настроек и модов, регистрирующих свои консольные команды.

#### Практические сценарии использования
```qc
void(string s) ValidateConsoleSymbol =
{
    float t;

    t = checkcommand(s);
    dprint(s, " => ", ftos(t), "\n");
};
```

---

### registercommand
`void(string cmdname, optional string desc) registercommand = #352;`

* **cmdname** — `string`, имя новой консольной команды.
* **desc** — `string`, необязательное описание для help/подсказок.

#### Описание и логика работы
Регистрирует консольную команду, если такой ещё нет. Дальше, когда пользователь вызовет эту команду, управление будет передано в соответствующий QC entrypoint модуля: `ConsoleCmd`, [`CSQC_ConsoleCommand`](00-entry-points.md#csqc_consolecommand), [`m_consolecommand`](00-entry-points.md#m_consolecommand) и т. п. Builtin работает и в сервере, и в клиентской части FTE, поэтому это удобный мост между консолью движка и вашим QuakeC-кодом.

#### Практические сценарии использования
```qc
void() WorldInit =
{
    registercommand("bot_add", "Create one practice bot");
};

float(string cmd) ConsoleCmd =
{
    tokenize(cmd);
    if (argv(0) == "bot_add")
    {
        SpawnPracticeBot();
        return TRUE;
    }
    return FALSE;
};
```

---

### isfunction
`float(string s) isfunction = #607;`

* **s** — `string`, имя функции.

#### Описание и логика работы
Проверяет, существует ли функция с таким именем и можно ли вызвать её через `callfunction`. Это reflection-блок для late binding и модульных систем, где наличие хука необязательно. Результат — обычное булево число, без выполнения самой функции.

#### Практические сценарии использования
```qc
void() TryInitAddon =
{
    if (isfunction("Addon_Init"))
        callfunction("Addon_Init");
};
```

---

### callfunction
`void(.../*, string funcname*/) callfunction = #605;`

* **...** — произвольные аргументы, которые будут переданы вызываемой функции.
* **funcname** — `string`, имя функции; обязательно передаётся последним аргументом.

#### Описание и логика работы
Находит функцию по имени и вызывает её, передавая остальные аргументы как есть. В FTEQW последний аргумент всегда трактуется как имя функции; если самой функции нет, вызов просто ничего не сделает, а если имя не передано вовсе — builtin завершится ошибкой. Это полезно для generic dispatch, таблиц хуков, menu callbacks и систем, где имена entrypoint'ов известны только в runtime.

#### Практические сценарии использования
```qc
void(float dmg) ApplyNamedDamageHook =
{
    if (isfunction("OnDamageTaken"))
        callfunction(dmg, "OnDamageTaken");
};
```

---

### externcall
`__variant(float prnum, string funcname, ...) externcall = #201;`

* **prnum** — `float`, идентификатор progs: `0` main, `-1` current, `-2` first matching among active.
* **funcname** — `string`, имя функции в целевом progs.
* **...** — аргументы для вызываемой функции.

#### Описание и логика работы
Часть `FTE_MULTIPROGS`. Builtin вызывает функцию по имени в другом загруженном `progs.dat` и возвращает её значение как `__variant`. Это мощный механизм модульности, но он требует очень аккуратно согласовывать имена функций и ожидаемые типы аргументов между модулями, потому что движок не создаёт высокоуровневый ABI за вас.

#### Практические сценарии использования
```qc
float g_addon_progs;

void() NotifyAddonAboutMap =
{
    if (g_addon_progs > 0)
        externcall(g_addon_progs, "Addon_MapStarted", mapname);
};
```

---

### externset
`void(float prnum, __variant newval, string varname) externset = #204;`

* **prnum** — `float`, целевой progs handle.
* **newval** — `__variant`, новое значение.
* **varname** — `string`, имя глобальной переменной в целевом progs.

#### Описание и логика работы
Часть `FTE_MULTIPROGS`. Записывает значение в глобал другого progs по имени, что позволяет связывать несколько QC-модулей без жёсткой линковки на этапе компиляции. Как и у `externcall`, ответственность за совместимость типов полностью лежит на авторе мода.

#### Практические сценарии использования
```qc
void() EnableAddonDebug =
{
    if (g_addon_progs > 0)
        externset(g_addon_progs, 1, "g_debug_enabled");
};
```

---

### externvalue
`__variant(float prnum, string varname) externvalue = #203;`

* **prnum** — `float`, целевой progs handle.
* **varname** — `string`, имя глобала.

#### Описание и логика работы
Часть `FTE_MULTIPROGS`. Читает глобальную переменную из другого progs и возвращает её как `__variant`. Это низкоуровневый способ делиться состоянием между модулями, если вы готовы сами контролировать типизацию и порядок инициализации.

#### Практические сценарии использования
```qc
float() ReadAddonWave =
{
    if (g_addon_progs <= 0)
        return 0;

    return externvalue(g_addon_progs, "g_wave_number");
};
```

---

### builtin_find
`float(string builtinname) builtin_find = #100;`

* **builtinname** — `string`, имя builtin-функции.

#### Описание и логика работы
Проверяет, поддерживается ли builtin с данным именем, и возвращает его номер. Это более точечная версия capability-check, чем общий [`checkextension`](12-system-debug-builtins.md#checkextension), и полезна для compatibility-кода вокруг нестабильных или условных builtins вроде `setmodelindex`. При отсутствии builtin результатом будет `0`.

#### Практические сценарии использования
```qc
float() HasSetModelIndex =
{
    return builtin_find("setmodelindex") != 0;
};
```

---

### changelevel
`void(string [mapname](../38-cvars-reference/07-system-misc-cvars.md#mapname), optional string newmapstartspot) changelevel = #70;`

* **mapname** — `string`, имя карты, на которую будет осуществлен переход.
* **newmapstartspot** — `string` (optional), имя целевого тарджета/цели или спота появления, который заменит стандартный `startspot` на следующей карте.

#### Описание и особенности работы
В SSQC это классический builtin для перехода на другую карту. Если передан параметр `newmapstartspot`, движок запоминает текущее состояние игрока и при инициализации новой карты будет искать соответствующий целевой объект, что часто используется для хаб-систем (hub-maps); если параметр опущен, сработает стандартная инициализация. В MenuQC аналогичный функционал зарезервирован под номером `changelevel = #64`, а в CSQC-коде номер `#70` в движке FTEQW привязан как `PF_NoCSQC` и вызовет исключение при выполнении.

#### Пример использования
```qc
void() ExitHubThroughRune =
{
    changelevel("e2m1", "hub_return");
};
```

---

### chat
`void(string filename, float starttag, entity edict) chat = #214;`

* **filename** — `string`, путь к chat-файлу, из которого движок считывает реплики.
* **starttag** — `float`, стартовый тег/индекс, с которого начнется чтение диалога.
* **edict** — `entity`, целевой эдикт персонажа, который должен произносить речь или участвовать в QC-скрипте диалога.

#### Описание и особенности работы
Этот расширенный builtin активируется через константу `FTE_QC_NPCCHAT`. Он парсит структурированный chat-файл, находит узел `starttag`, инициализирует реплики актера и по ходу выполнения выводит текст, а также может вызывать QC-функции по триггерам в процессе.

#### Пример использования
```qc
void() NPC_BeginConversation =
{
    chat("npc/guard.chat", 100, self);
};
```

---

### empty
`void() empty = #245..#249;`

* Параметры отсутствуют.

#### Описание и особенности работы
`empty` не является реальным builtin'ом с зарегистрированным именем. Номера `#245..#249` помечены лишь закомментированными

#### Пример использования
```qc
void() CheckEmptyBuiltinName =
{
    if (!builtin_find("empty"))
        dprint("'empty' is only a reserved slot label\n");
};
```

---

### entityfieldref
`field_t(float fieldnum) entityfieldref = #0:entityfieldref;`

* **fieldnum** — `float`, глобальный индекс поля, обычно возвращаемый функцией `findentityfield`.

#### Описание и особенности работы
Часть Reflection API для полей сущностей (entity fields). Builtin возвращает прямую ссылку на поле в виде типа `field_t`, которую затем можно использовать для динамического чтения и записи значений в обход статического компилятора, основываясь на строковых именах. Интенсивное подмножество FTE позволяет проверять тип этого поля через `entityfieldtype`, прежде чем безопасно использовать этот метод для манипуляции данными на лету.

#### Пример использования
```qc
field_t g_health_field;

void() CacheHealthFieldRef =
{
    float idx;

    idx = findentityfield("health");
    if (entityfieldtype(idx) == EV_FLOAT)
        g_health_field = entityfieldref(idx);
};
```

---

### entityprotection
`float(entity e, float nowreadonly) entityprotection = #0:entityprotection;`

* **e** — `entity`, сущность, для которой меняется флаг защиты.
* **nowreadonly** — `float`, `0` снимает защиту, `1` делает эдикт защищенным от записи (read-only) для QC.

#### Описание и особенности работы
Builtin контролирует флаг [`readonly`](../44-cli-commands-reference/01-fteqw-startup-parameters.md#readonly) в структуре эдикта и при значениях `0` или `1` напрямую записывает их в `e->readonly`, после чего QC больше не сможет менять поля защищённой сущности. Важный нюанс именно текущей реализации FTEQW: возвращаемое значение здесь не «предыдущее состояние защиты», а просто переданный аргумент `nowreadonly`; если же передать что-то вне диапазона `0..1`, builtin тоже вернёт это число, но сам флаг менять не станет. Попытка записи в защищённый edict затем вызовет обычную ошибку интерпретатора о записи в read-only entity.

#### Пример использования
```qc
void(entity e) LockTemplateEntity =
{
    entityprotection(e, 1);
};
```

---

### eprint
`void(entity e) eprint = #31;`

* **e** — `entity`, сущность, текстовые данные которой будут выведены в консоль.

#### Описание и особенности работы
Отладочный builtin, который выводит в консоль всю доступную информацию о полях сущности. Это включает текущие значения ключевых переменных эдикта (edict), таких как `.origin`, `.velocity`, `.health`, а также времена выполнения think/touch-функций для точной диагностики. В MenuQC этот зарезервированный опкод не используется и выдает ошибку, так как контекст меню не оперирует классическими эдиктами на слоте `#31`.

#### Пример использования
```qc
void() DebugSelf =
{
    eprint(self);
};
```

---

### find_list
`entity*(.__variant fld, __variant match, int type=EV_STRING, __out int count) find_list = #0:find_list;`

* **fld** — `.__variant`, поле сущности, по значению которого идет фильтрация.
* **match** — `__variant`, целевое значение для поиска.
* **type** — `int`, тип данных для сравнения (`EV_STRING`, `EV_FLOAT`, `EV_VECTOR`, `EV_ENTITY` и т. д.).
* **count** — `int`, выходной параметр, в который запишется общее количество найденных совпадений.

#### Описание и особенности работы
В отличие от стандартной функции `find`, данный builtin не просто ищет одну сущность за раз, а возвращает в С-стиле указатель на динамический массив найденных совпадений в виде `entity*`. Для фильтрации используется явное указание типа через параметр `type`, что расширяет API поиска не только по строкам, но и по float/entity/vector-полям в рамках единого вызова. Возвращаемый массив выделяется во временной памяти (temp-память движка) и валиден только в рамках текущего кадра или функции.

#### Пример использования
```qc
void() ListAllDoors =
{
    entity *hits;
    int count;
    int i;

    hits = find_list(classname, "func_door", EV_STRING, count);
    for (i = 0; i < count; i = i + 1)
        dprint("door: ", etos(hits[i]), "\n");
};
```

---

### findentity
`findentity` — alias из `fteextensions.qc` для `entity(entity start, .__variant fld, __variant match) findfloat = #98;`

* **start** — `entity`, точка начала поиска; используйте `world` для поиска с самого первого эдикта.
* **fld** — `.__variant`, поле сущности, проверяемое на совпадение.
* **match** — `__variant`, значение поля, которое необходимо сопоставить.

#### Описание и особенности работы
В серверной таблице FTEQW отдельной записи с именем `findentity` нет: это именно alias к `findfloat`, задокументированный прямо в `fteextensions.qc` и в описании builtin `#98`. Поведение поэтому полностью совпадает с `findfloat`: движок сканирует активные эдикты, начиная со следующего за `start`, сравнивает сырое значение поля и возвращает первый найденный edict; при отсутствии совпадений возвращается `world`.

#### Пример использования
```qc
void(entity pl) PrintOwnedProjectiles =
{
    entity e;

    for (e = findentity(world, owner, pl); e; e = findentity(e, owner, pl))
        dprint("owned: ", etos(e), "\n");
};
```

---

### findentityfield
`float(string fieldname) findentityfield = #0:findentityfield;`

* **fieldname** — `string`, имя поля сущности в виде строки, например `"health"` или `"classname"`.

#### Описание и особенности работы
Часть рефлексии полей, возвращающая глобальный индекс указанного поля по его строковому имени. Этот индекс затем передается в смежные reflection-функции движка, такие как `entityfieldref`, `entityfieldname`, `entityfieldtype`, `getentityfieldstring` и `putentityfieldstring`. Если указанное поле отсутствует в глобальном списке определений прогса (progs), функция вернет `0`, что служит индикатором отсутствия переменной в текущей сборке.

#### Пример использования
```qc
void() PrintHealthFieldIndex =
{
    dprint("health field index=", ftos(findentityfield("health")), "\n");
};
```

---

### findradius_list
`entity*(vector org, float rad, __out int foundcount, int sort=0) findradius_list = #0:findradius_list;`

* **org** — `vector`, точка центра сферы поиска.
* **rad** — `float`, радиус сферы поиска.
* **foundcount** — `int`, выходной параметр, в который запишется общее количество найденных сущностей.
* **sort** — `int`, необязательный флаг сортировки результатов (например, по дистанции); по умолчанию имеет значение `0`.

#### Описание и особенности работы
Альтернатива стандартной функции `findradius`, которая возвращает temp-массив сущностей вместо создания связанного списка через поле `.chain`. На стороне игрового движка FTEQW этот builtin всегда использует area links и считает дистанцию с учётом bbox, то есть поведение здесь уже «как если бы» [`sv_gameplayfix_findradiusdistancetobox`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_findradiusdistancetobox) и [`dpcompat_findradiusarealinks`](../38-cvars-reference/07-system-misc-cvars.md#dpcompat_findradiusarealinks) были включены. В отличие от старого `findradius`, compatibility-путь [`sv_gameplayfix_blowupfallenzombies`](../38-cvars-reference/05-physics-gameplay-cvars.md#sv_gameplayfix_blowupfallenzombies) тут не учитывается: non-solid сущности без флага `FL_FINDABLE_NONSOLID` пропускаются всегда.

#### Пример использования
```qc
void() DamageNearby =
{
    entity *hits;
    int count;
    int i;

    hits = findradius_list(self.origin, 128, count);
    for (i = 0; i < count; i = i + 1)
        if (hits[i].takedamage)
            hits[i].health = hits[i].health - 10;
};
```

---

### generateentitydata
`string(entity e) generateentitydata = #0:generateentitydata;`

* **e** — `entity`, сущность, данные которой будут сериализованы.

#### Описание и особенности работы
Позволяет сдампить структуру полей и параметров сущности в текстовую строку формата Map/Entity, которую затем можно обратно пропарсить через `parseentitydata`. Это полезно для создания быстрых сохранений (save/load) отдельных эдиктов для сериализации, сетевой передачи, клонирования или сохранения в конфигурационные файлы. Возвращаемая строка выделяется в tempstring, поэтому если вы хотите сохранить эти данные надолго, сохраните строку в постоянную переменную.

#### Пример использования
```qc
void() DumpSelfState =
{
    string dump;

    dump = generateentitydata(self);
    dprint(dump, "\n");
};
```

---

### plaque_draw
`void(entity targ, float stringno) plaque_draw = #79;`

* **targ** — `entity`, эдикт-цель для вывода Hexen II-подобных табличек (plaque) или [centerprint](12-system-debug-builtins.md#centerprint)-сообщений.
* **stringno** — `float`, индекс строки в языковом файле строковых ресурсов Hexen II.

#### Описание и особенности работы
Специфический встроенный builtin для совместимости с Hexen II, который выводит текстовую плашку (plaque-окно) на экране игрока, а не стандартный текстовый centerprint. В реализации `PF_h2plaque_draw` движок извлекает строку через `T_GetString`. Индекс `0` используется для очистки интерфейса, а сами окна используют встроенные спрайты обрамления и шрифты движка. На современных движках этот builtin используется редко, в основном для поддержки старых H2-модов и карт, или если движок запущен в режиме обратной совместимости.

#### Пример использования
```qc
void() ShowHexen2Hint =
{
    plaque_draw(self, 12);
};
```

---

### pushmove
`float(entity pusher, vector move, vector amove) pushmove = #0;`

* **pusher** — `entity`, движущаяся сущность-платформа (обычно bmodel) со статусом push-объекта.
* **move** — `vector`, вектор линейного перемещения.
* **amove** — `vector`, угловое вращение (азимут).

#### Описание и особенности работы
Выполняет физическое перемещение объектов, имеющих тип движения `MOVETYPE_PUSH`. Функция сдвигает объект `pusher`, затем находит и толкает другие сущности (например, игроков или монстров) на своем пути, а при блокировке вызывает стандартную `blocked`-функцию у платформы для нанесения урона или остановки движения. Возвращаемое булево значение берется напрямую из внутренней функции `WPhys_Push`: возвращает `1`, если перемещение прошло успешно, и `0`, если движение было полностью заблокировано. Этот builtin незаменим, когда QC-код должен вручную симулировать логику лифтов или дверей без использования стандартных триггерных задержек.

#### Пример использования
```qc
void(entity plat) NudgePlatformUp =
{
    if (!pushmove(plat, '0 0 16', '0 0 0'))
        dprint("platform was blocked\n");
};
```

---

### qtest_canreach
`DEP float(vector v) qtest_canreach = #39;`

* **v** — `vector`, точка, до которой бот или сущность пытается определить потенциальную досягаемость.

#### Описание и особенности работы
Это устаревший QTest-код, который в современных сборках FTEQW заменен на заглушку `PF_Ignore`. Наличие этого builtin'а обусловлено исключительно целями совместимости на уровне QC, но реального значения функция не несет и возвращает постоянный ноль. Для определения путей рекомендуется использовать полноценную трассировку, `walkmove`, нативные builtins для навигации AI-ботов, а не полагаться на `qtest_canreach`.

#### Пример использования
```qc
void() TestLegacyReachCheck =
{
    if (!qtest_canreach(self.origin + '64 0 0'))
        dprint("qtest_canreach is a legacy no-op in FTEQW\n");
};
```

---

### readserverentitystate
`void(float flags, float simtime) readserverentitystate = #369;`

* **flags** — `float`, конфигурационные флаги чтения state-данных.
* **simtime** — `float`, симуляционное время/кадр интерполяции.

#### Описание и особенности работы
В актуальных сборках FTEQW этот слот не зарегистрирован как рабочий builtin: номер `#369` не активирован и помечен пометкой `EXT_CSQC_1`.

#### Пример использования
```qc
void() CheckReadServerEntityStateSupport =
{
    if (!builtin_find("readserverentitystate"))
        dprint("readserverentitystate is not exposed by this build\n");
};
```

---

### readsingleentitystate
`readsingleentitystate` — незарегистрированный закомментированный слот `#370` из старого `EXT_CSQC_1`.

* Параметры и возвращаемое значение не специфицированы, потому что рабочей записи builtin нет.

#### Описание и особенности работы
У `readsingleentitystate` та же судьба, что и у `readserverentitystate`: это только закомментированный placeholder, а не доступная builtin-функция.

#### Пример использования
```qc
void() CheckReadSingleEntityStateSupport =
{
    if (!builtin_find("readsingleentitystate"))
        dprint("readsingleentitystate is not exposed by this build\n");
};
```

---

### removeentity
`void(entity ent) removeentity = #0:removeentity;`

* **ent** — `entity`, CSQC-сущность, чьи уже добавленные render-entry нужно убрать из текущей сцены.

#### Описание и особенности работы
Это не синоним стандартного удаления `remove`, а специфическая CSQC-команда клиентского рендеринга. Builtin работает только со списком уже добавленных в сцену render-entity и убирает из него все записи, соответствующие тому же внутреннему ключу сущности; сам edict при этом не уничтожается. Такой вызов полезен, когда нужно сначала убрать результат [`addentity`](08-csqc-rendering-builtins.md#addentity)/[`addentities`](08-csqc-rendering-builtins.md#addentities), а потом добавить модифицированную версию заново — например, для split-screen, ручной сортировки или условного скрытия модели.

#### Пример использования
```qc
void(entity ent) RefreshSceneEntity =
{
    removeentity(ent);
    ent.angles_y = ent.angles_y + 45;
    addentity(ent);
};
```

---

### route_calculate
`void(entity ent, vector dest, int denylinkflags, void(entity ent, vector dest, int numnodes, nodeslist_t *nodelist) callback) route_calculate = #0:route_calculate;`

* **ent** — `entity`, перемещающаяся сущность (бот или монстр).
* **dest** — `vector`, целевая точка маршрута.
* **denylinkflags** — `int`, маска игнорируемых link-флагов для отсечения путей.
* **callback** — функция-колбэк, которая будет вызвана движком после завершения расчета маршрута.

#### Описание и особенности работы
Продвинутый встроенный метод для асинхронной работы с routing/nodegraph-системой FTEQW без блокировки основного потока: результат расчета передается через callback. По спецификации выделенный массив нод пути должен быть принудительно очищен через [`memfree`](06-files-database-builtins.md#memfree), а доступ к целевым координатам и результатам шагов в векторе доступен с конца массива, вплоть до конечной точки пути, лежащей в `nodelist[numnodes - 1]`. Если путь не был найден, колбэк всё равно будет вызван, но индекс количества нод будет равен нулю.

#### Пример использования
```qc
void(entity ent, vector dest, int numnodes, nodeslist_t *nodelist) OnRouteReady =
{
    if (numnodes)
        ent.ideal_yaw = vectoyaw(nodelist[numnodes - 1].dest - ent.origin);

    if (nodelist)
        memfree(nodelist);
};

void(entity ent, vector dest) RepathMonster =
{
    route_calculate(ent, dest, 0, OnRouteReady);
};
```

---

### runclientphys
`runclientphys` — это внутреннее имя реализации; в QuakeC рабочий builtin называется `void(entity ent) runstandardplayerphysics = #347;`

* **ent** — `entity`, игрок или player-like персонаж, для которого будут применены стандартные алгоритмы физики игрока (player-physics).

#### Описание и особенности работы
Отдельного builtin с именем `runclientphys` в текущих таблицах нет: так называется C-функция движка, обслуживающая builtin `runstandardplayerphysics`. Именно его и нужно вызывать из QC, чтобы запустить стандартный обсчёт player-physics на основе текущих `input_*` globals, обычно внутри `SV_RunClientCommand`. Историческое имя `runclientphys` отдельно всплывает лишь как заблокированный CSQC-слот совместимости, поэтому для реального кода используйте только `runstandardplayerphysics`.

#### Пример использования
```qc
void() SV_RunClientCommand =
{
    if (self.flags & FL_WATERJUMP)
        input_movevalues_z = 0;

    runstandardplayerphysics(self);
};
```

---

### te_gunshotquad
`void(vector org) te_gunshotquad = #412;`

* **org** — `vector`, точка, в которой будет создан временный сетевой эффект (temp-entity) выстрела.

#### Описание и особенности работы
Создает temp-entity эффект `TEDP_GUNSHOTQUAD`, то есть усиленную под Quad Damage версию стандартного пулевого попадания (gunshot-искры/декаль). Builtin реализован в соответствии с расширением `DP_TE_QUADEFFECTS1`. При вызове генерируется сетевой пакет для клиентов, а дальнейшая отрисовка зависит от клиентских настроек системы частиц ([particle](08-csqc-rendering-builtins.md#particle)-систем) движка. По сути, это удобный сокращенный метод (shorthand) для вызова спецэффекта попадания без ручной сборки заголовков temp-entity сообщений.

#### Пример использования
```qc
void() ShowQuadBulletImpact =
{
    te_gunshotquad(trace_endpos);
};
```

---

### te_lightning2
`void(entity own, vector start, vector end) te_lightning2 = #429;`

* **own** — `entity`, владелец/источник создаваемого луча.
* **start** — `vector`, начальная точка луча.
* **end** — `vector`, конечная точка луча.

#### Описание и особенности работы
Генерирует beam-эффект (луч молнии) типа `TE_LIGHTNING2` между координатами `start` и `end`. Аргумент `own` записывается в сетевую структуру как источник эффекта, что важно для отсечения звуков и правильного позиционирования луча относительно модели оружия при поворотах игрока. Это классический встроенный builtin, входящий в спецификацию стандартных эффектов `DP_TE_STANDARDEFFECTBUILTINS`, и он автоматически управляет временем жизни луча на стороне клиента.

#### Пример использования
```qc
void(entity victim) FireLightning2 =
{
    te_lightning2(self, self.origin, victim.origin);
};
```

---

### te_lightning3
`void(entity own, vector start, vector end) te_lightning3 = #430;`

* **own** — `entity`, владелец/источник создаваемого луча.
* **start** — `vector`, начальная точка луча.
* **end** — `vector`, конечная точка луча.

#### Описание и особенности работы
Выполняет ту же функцию создания beam-эффекта, что и `te_lightning2`, но использует визуальный тип `TE_LIGHTNING3` (обычно это текстура молнии другого цвета или формы). Метод упаковывает сетевые данные и отсылает клиентам temp-entity сообщение, которое автоматически интерполируется и очищается движком без необходимости ручного контроля флагов и удаления/создания объектов. Используется как нативный builtin для классических молний в Quake/DarkPlaces и совместимых движках.

#### Пример использования
```qc
void(entity victim) FireLightning3 =
{
    te_lightning3(self, self.origin, victim.origin);
};
```

---

### te_muzzleflash
`void(entity ent) te_muzzleflash = #0:te_muzzleflash;`

* **ent** — `entity`, сущность персонажа или оружия, у которой нужно отобразить вспышку выстрела.

#### Описание и особенности работы
Встроенный хелпер для создания эффекта вспышки выстрела (muzzle flash). В QuakeWorld-совместимых режимах он сразу отсылает клиентам сетевое сообщение `svc_muzzleflash`, а в стандартных условиях просто выставляет сущности флаг эффекта `EF_MUZZLEFLASH`, который заставляет движок отрисовать динамический свет и вспышку в месте крепления оружия. Использование данного метода гарантирует правильную синхронизацию анимации выстрела и эффектов освещения без накладных расходов.

#### Пример использования
```qc
void() FireShotgunVisuals =
{
    te_muzzleflash(self);
};
```

---

### te_spikequad
`void(vector org) te_spikequad = #413;`

* **org** — `vector`, точка попадания шипа.

#### Описание и особенности работы
Генерирует усиленный Quad-эффект попадания гвоздя/шипа типа `TEDP_SPIKEQUAD`. Является частью спецификации `DP_TE_QUADEFFECTS1` и заменяет собой стандартный spike impact, когда игрок находится под действием усилителя урона. Визуальное отображение полностью зависит от текущего конфига частиц (particle setup) на клиенте. Используется для быстрой отрисовки брызг и искр при попадании гвоздей супергвоздомета (Super Nailgun).

#### Пример использования
```qc
void() ShowQuadSpikeImpact =
{
    te_spikequad(trace_endpos);
};
```

---

### te_superspikequad
`void(vector org) te_superspikequad = #414;`

* **org** — `vector`, точка попадания супер-шипа (супер-гвоздя).

#### Описание и особенности работы
Генерирует временный сетевой эффект (temp-entity) `TEDP_SUPERSPIKEQUAD`, который представляет собой усиленную под действием Quad Damage версию попадания тяжелого гвоздя (superspike/supernail-эффект). Как и остальные встроенные функции семейства `te_*quad`, данный метод полагается на расширение системы частиц и автоматически передает клиентам пакет для отрисовки массивного снопа искр или осколков. Это избавляет от необходимости вручную формировать заголовки сообщений сетевого протокола при регистрации попаданий супергвоздомета.

#### Пример использования
```qc
void() ShowQuadSuperSpikeImpact =
{
    te_superspikequad(trace_endpos);
};
```

---

### undefined
`undefined` — это не рабочий builtin, а метка зарезервированных слотов под номерами `#458`, `#470`, `#505..#509` и `#539`.

* Параметры отсутствуют, так как реальной функции под этим именем в таблицах ядра движка нет.

#### Описание и особенности работы
Аналогично `empty`, метка `undefined` здесь служит только комментарием-подписью к незанятым слотам таблицы. Для номеров `#458`, `#470`, `#505..#509` и `#539` нет зарегистрированных builtin-имён

#### Пример использования
```qc
void() CheckUndefinedBuiltinName =
{
    if (!builtin_find("undefined"))
        dprint("'undefined' is only a placeholder slot name\n");
};
```

---

## Смежные страницы

- [Игровая логика: серверный QuakeC (SSQC)](../16-quakec-scripting/server-side-quakec-ssqc.md)
- [Справочник ключей сущностей](../README.md#ключи-сущностей-карты-entity-keys)
- [Индекс справочника builtins](../README.md#встроенные-функции-quakec-builtins)

> [⬅ Предыдущая страница](02-string-builtins.md) | [Следующая страница ➡](04-network-messages-builtins.md)

> [⬅ Вернуться к оглавлению вики](../README.md)