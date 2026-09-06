# Скелетная анимация и модели

> [⬅ Вернуться к оглавлению вики](../README.md)

> [Индекс справочника builtins](./README.md)

Этот раздел описывает builtins FTEQW, которые работают со скелетными моделями, тегами привязки, чтением геометрии поверхностей и пользовательскими skin-объектами. В скелетной системе движка QuakeC управляет не самой моделью напрямую, а отдельным skeletal object: в него загружается поза из анимации, после чего отдельные кости можно дочитывать, копировать, домножать и переопределять процедурно. Кости в API обычно адресуются 1-based индексами, а `0` в ряде builtins означает специальный режим «весь диапазон». Для мировых координат костей и тегов важно различать три пространства: локальное относительно родителя, абсолютное относительно entity и итоговое world-space после учёта origin/angles и цепочек tag attachment.

## Функции

### skel_build
`float(float skel, entity ent, float modelindex, float retainfrac, float firstbone, float lastbone, optional float addfrac) skel_build = #264;`

* **skel** — `float`, идентификатор skeletal object; `0` разрешает движку создать новый объект автоматически.
* **ent** — `entity`, источник frame state: текущие `frame`, `frame2`, времена и lerp-состояние читаются именно у этой сущности.
* **modelindex** — `float`, модель, из которой нужно взять скелет и анимационные данные.
* **retainfrac** — `float`, доля уже накопленной позы, которую надо сохранить перед добавлением новой.
* **firstbone** — `float`, начало диапазона костей; `0` означает «с самого начала».
* **lastbone** — `float`, конец диапазона; `0` или отрицательное значение трактуется как «до конца скелета».
* **addfrac** — `float`, необязательный вес новой анимации; по умолчанию движок берёт `1 - retainfrac`.

#### Описание и логика работы
`skel_build` — основная точка входа в систему skeletal objects. Builtin берёт текущее анимационное состояние `ent`, читает позы костей из модели `modelindex` и смешивает их в объект `skel`. Типичный цикл такой: один раз создаётся skeletal object, затем в каждом кадре его сначала заполняют базовой позой, потом поверх отдельными вызовами домешивают ещё одну анимацию, например для торса или головы. Нумерация костей в пользовательском API 1-based, но значение `0` у `firstbone`/`lastbone` используется как сокращение для полного диапазона. Если `skel` равен `0`, движок может создать новый объект и вернуть его id; при неверной модели или нескелетном ресурсе builtin возвращает `0`.

Важно учитывать тип скелета: нормальный режим для смешивания — relative skeleton, где каждая кость хранится относительно родителя. Absolute skeletons допустимы, но частичное обновление и поэтапное смешивание там не рассчитаны на обычный workflow. При неверных индексах диапазон зажимается до допустимых границ, поэтому builtin скорее обрежет диапазон, чем аварийно завершится. Для корректного блендинга сумма весов всех вызовов за кадр должна давать 1; если она больше или меньше, итоговая поза может масштабироваться и визуально «плыть».

#### Практические сценарии использования
```
void() Player_RebuildSkeleton =
{
	// self.skeletonobject хранит handle, который потом использует renderer.
	if (!self.skeletonobject)
		self.skeletonobject = skel_create(self.modelindex);

	// База: полный скелет из анимации ног.
	self.frame = self.anim_legs;
	self.frame2 = self.anim_legs;
	self.lerpfrac = 0;
	skel_build(self.skeletonobject, self, self.modelindex, 0, 0, 0);

	// Поверх - половинный вес анимации стрельбы только для торса.
	self.frame = self.anim_torso_fire;
	self.frame2 = self.anim_torso_fire;
	self.lerpfrac = 0;
	skel_build(self.skeletonobject, self, self.modelindex, 1, self.torso_firstbone, self.torso_lastbone, 0.5);
};
```

### skel_copybones
`void(float skeldst, float skelsrc, float startbone, float entbone) skel_copybones = #274;`

* **skeldst** — `float`, skeletal object-приёмник.
* **skelsrc** — `float`, skeletal object-источник.
* **startbone** — `float`, начало диапазона; `0` означает копирование с первой кости.
* **entbone** — `float`, фактический верх диапазона; на практике используется как второй ограничитель диапазона, `0` копирует до конца.

#### Описание и логика работы
`skel_copybones` переносит готовые матрицы костей из одного skeletal object в другой без повторного чтения анимации из файла модели. Это удобно, когда один скелет уже собран и вы хотите сделать его основой для второго варианта позы: например, сначала скопировать базу, потом процедурно довернуть лишь пару костей. Если оба объекта одного типа, данные копируются напрямую. Движок также умеет переводить relative skeleton в absolute при копировании, но обратное преобразование в текущей реализации не доведено до полноценного рабочего пути, поэтому безопаснее держать совместимые скелеты одного типа.

Если любой из id неверен, builtin просто ничего не делает. Границы диапазона зажимаются по минимальному количеству костей обоих объектов. На практике используйте `0, 0`, когда нужно клонировать весь скелет, и только потом вносите локальные изменения в destination.

#### Практические сценарии использования
```
void() ClonePoseAndBendNeck =
{
	local float neckbone;

	if (!self.base_skel || !self.work_skel)
		return;

	// Копируем всю уже собранную позу в рабочий скелет.
	skel_copybones(self.work_skel, self.base_skel, 0, 0);

	neckbone = skel_find_bone(self.work_skel, "Bip01 Neck");
	if (neckbone > 0)
	{
		makevectors('0 25 0');
		skel_premul_bone(self.work_skel, neckbone, '0 0 0', v_forward, v_right, v_up);
	}
};
```

### skel_create
`float(float modlindex, optional float useabstransforms) skel_create = #263;`

* **modlindex** — `float`, `modelindex` скелетной модели, по которой определяется число костей и их схема.
* **useabstransforms** — `float`, необязательный режим хранения; `0` создаёт обычный relative skeleton, ненулевое значение запрашивает absolute transforms.

#### Описание и логика работы
`skel_create` выделяет новый skeletal object и возвращает его числовой handle. Сам объект не «живёт» внутри сущности автоматически: обычно мод хранит этот id в своём поле вроде `.skeletonobject` или `.skeletonindex`, а затем передаёт его в [`addentity`](08-csqc-rendering-builtins.md#addentity)/рендерный путь конкретной entity. По умолчанию создаётся relative skeleton, где каждая кость задаётся относительно родителя — именно этот режим ожидают `skel_build`, `skel_set_bone` и большинство процедурных операций. Ненулевой `useabstransforms` просит движок выделить объект для абсолютных матриц относительно самой entity; это полезно для некоторых специализированных сценариев, но хуже подходит для обычного поэтапного блендинга.

Если модель не существует или не содержит скелета, builtin возвращает `0`. Создание лишь резервирует структуру данных; фактическую позу туда нужно загрузить отдельным вызовом `skel_build` или заполнить вручную через `skel_set_bone`. После завершения работы объект нужно освобождать через `skel_delete`.

#### Практические сценарии использования
```
float() Player_InitSkeleton =
{
	self.skeletonobject = skel_create(self.modelindex);
	if (!self.skeletonobject)
	{
		dprint("model has no skeleton or could not allocate skeletal object\n");
		return 0;
	}

	skel_build(self.skeletonobject, self, self.modelindex, 0, 0, 0);
	return 1;
};
```

### skel_delete
`void(float skel) skel_delete = #275;`

* **skel** — `float`, идентификатор skeletal object, который больше не нужен.

#### Описание и логика работы
`skel_delete` помечает skeletal object на удаление. Важный нюанс реализации FTEQW: освобождение откладывается до безопасного момента, поэтому builtin можно вызывать даже в том кадре, где объект ещё участвует в рендеринге, не ломая следующий `addentity`/[`renderscene`](08-csqc-rendering-builtins.md#renderscene). Если на этом skeleton был активен ragdoll, он тоже снимается. Повторный вызов с уже невалидным id ничего полезного не делает, но и не должен ломать VM.

Практическое правило простое: создали skeletal object — удалите его при уничтожении сущности, смене модели на нескелетную или полном пересоздании визуального представления. Оставленный handle сам по себе не «протухает» автоматически и будет занимать слот/память до освобождения.

#### Практические сценарии использования
```
void() Player_FreeSkeleton =
{
	if (!self.skeletonobject)
		return;

	skel_delete(self.skeletonobject);
	self.skeletonobject = 0;
};
```

### skel_find_bone
`float(float skel, string tagname) skel_find_bone = #268;`

* **skel** — `float`, skeletal object, по чьей исходной модели выполняется поиск.
* **tagname** — `string`, имя кости или тега в данных модели.

#### Описание и логика работы
`skel_find_bone` ищет кость по имени и возвращает её числовой индекс в том же 1-based формате, который используют остальные `skel_*` builtins. Название параметра `tagname` историческое: движок пользуется общим lookup-механизмом для bones/tags в модели. Это удобно для кода, который не хочет жёстко хардкодить номера костей при смене экспортера, версии рига или совместимой модели. Если skeletal object невалиден либо имя не найдено, результатом будет `0`.

Обычно builtin вызывают один раз при инициализации сущности и кешируют найденные номера в собственных полях entity. Это дешевле и надёжнее, чем искать кость по строке каждый кадр.

#### Практические сценарии использования
```
void() CacheImportantBones =
{
	self.bone_head = skel_find_bone(self.skeletonobject, "Bip01 Head");
	self.bone_spine = skel_find_bone(self.skeletonobject, "Bip01 Spine");
	self.bone_weapon = skel_find_bone(self.skeletonobject, "tag_weapon");

	if (!self.bone_head)
		dprint("warning: head bone was not found on model\n");
};
```

### skel_get_boneabs
`vector(float skel, float bonenum) skel_get_boneabs = #270;`

* **skel** — `float`, skeletal object, из которого нужно прочитать кость.
* **bonenum** — `float`, 1-based индекс кости.

#### Описание и логика работы
`skel_get_boneabs` возвращает смещение кости относительно самой entity, а её ориентацию записывает в глобальные `v_forward`, `v_right`, `v_up`. Это уже не поза относительно родителя, а итог внутри локального пространства модели. Для relative skeleton движок сам поднимается по цепочке родителей и перемножает матрицы; для absolute skeleton данные можно отдать сразу. Именно поэтому builtin удобен для вычислений вроде «где локально находится ладонь персонажа после всех анимаций». Для перехода в полноценные мировые координаты добавляется ещё transform самой entity — либо напрямую через `gettaginfo`, либо вручную.

При неверном `skel` или `bonenum` builtin возвращает нулевой origin и единичную ориентацию в `v_*`, то есть безопасный identity-результат. Это позволяет писать защитный код без риска получить мусор, но проверять `bonenum > 0` всё равно лучше заранее.

#### Практические сценарии использования
```
vector() Player_GetMuzzleLocalOrigin =
{
	local float bone;
	local vector org;

	bone = skel_find_bone(self.skeletonobject, "tag_weapon");
	if (!bone)
		return '0 0 0';

	org = skel_get_boneabs(self.skeletonobject, bone);
	return org;
};
```

### skel_get_bonename
`string(float skel, float bonenum) skel_get_bonename = #266;`

* **skel** — `float`, skeletal object, чья исходная модель используется как таблица имён.
* **bonenum** — `float`, 1-based индекс кости.

#### Описание и логика работы
`skel_get_bonename` возвращает имя кости по её номеру. Это builtin в первую очередь отладочный: он полезен для консольной диагностики, экранных overlay-списков, проверки экспорта модели и сопоставления номеров с костями, найденными через `skel_find_bone`. Возвращаемая строка временная, поэтому хранить её надолго нужно так же осторожно, как любые temp strings в QuakeC. Если skeletal object невалиден, вы получите пустой/нулевой строковый результат.

Для игровой логики по кадрам builtin почти никогда не нужен. Обычный практический workflow — один раз пробежаться по всем костям, вывести список и затем закешировать интересные индексы.

#### Практические сценарии использования
```
void() Debug_ListBones =
{
	local float i;
	local float count;

	count = skel_get_numbones(self.skeletonobject);
	for (i = 1; i <= count; i = i + 1)
		dprint(sprintf("bone %g = %s\n", i, skel_get_bonename(self.skeletonobject, i)));
};
```

### skel_get_boneparent
`float(float skel, float bonenum) skel_get_boneparent = #267;`

* **skel** — `float`, skeletal object, чья модель задаёт иерархию.
* **bonenum** — `float`, 1-based индекс кости.

#### Описание и логика работы
`skel_get_boneparent` сообщает, к какой кости привязана указанная кость. Возвращаемое значение использует ту же 1-based систему; `0` — это специальный случай «родитель не кость, а сама entity». Благодаря этому можно обходить дерево рига, строить собственные диагностические представления скелета или находить верхние сегменты, с которых удобно запускать процедурные деформации. Для невалидного skeletal object движок тоже возвращает `0`, поэтому интерпретировать `0` как «корень либо ошибка» нужно в контексте ваших предварительных проверок.

Builtin не пересчитывает позу, а только читает метаданные модели. Поэтому его нормально вызывать при инициализации и кешировать результат в полях entity.

#### Практические сценарии использования
```
float() FindTopmostParent =
{
	local float bone;
	local float parent;

	bone = skel_find_bone(self.skeletonobject, "Bip01 Head");
	while (bone > 0)
	{
		parent = skel_get_boneparent(self.skeletonobject, bone);
		if (!parent)
			return bone;
		bone = parent;
	}

	return 0;
};
```

### skel_get_bonerel
`vector(float skel, float bonenum) skel_get_bonerel = #269;`

* **skel** — `float`, skeletal object, откуда читается трансформация.
* **bonenum** — `float`, 1-based индекс кости.

#### Описание и логика работы
`skel_get_bonerel` возвращает локальное смещение кости относительно её родителя и одновременно заполняет `v_forward`, `v_right`, `v_up` локальной ориентацией этой кости. Для relative skeleton это прямое чтение сохранённой позы. Если skeleton хранится в absolute-виде, движок на лету вычисляет относительное преобразование через инверсию матрицы родителя и умножение на матрицу ребёнка. Это делает builtin особенно удобным в паре со `skel_set_bone`: можно считать текущую локальную позу, подправить её и записать назад.

При неверном id или индексе кости возвращается identity: origin `0 0 0`, а `v_*` становятся базовым ортонормированным набором. Для корневых костей относительное пространство фактически равно пространству entity.

#### Практические сценарии использования
```
void() RemoveRootTranslation =
{
	// Читаем текущую ориентацию корневой кости в v_forward/v_right/v_up.
	skel_get_bonerel(self.skeletonobject, 1);

	// Обнуляем только смещение, сохраняя текущий поворот.
	skel_set_bone(self.skeletonobject, 1, '0 0 0', v_forward, v_right, v_up);
};
```

### skel_get_numbones
`float(float skel) skel_get_numbones = #265;`

* **skel** — `float`, skeletal object, для которого нужно узнать размер скелета.

#### Описание и логика работы
`skel_get_numbones` возвращает количество костей в skeletal object. Это число определяет допустимый пользовательский диапазон индексов `1..n`. Builtin полезен для отладочных обходов, массовых модификаций через циклы и защитной проверки при чтении конфигураций, где номер кости пришёл из файла или cvar. Если id невалиден, вернётся `0`.

Хотя внутри движка кости хранятся 0-based, наружный QC API почти везде ожидает именно 1-based номера. Поэтому типичный цикл всегда начинается с `1` и идёт до результата `skel_get_numbones` включительно.

#### Практические сценарии использования
```
float() HasBoneRangeForSpine =
{
	local float count;

	count = skel_get_numbones(self.skeletonobject);
	if (count >= 20)
		return 1;
	return 0;
};
```

### skel_mmap
`float*(float skel) skel_mmap = #282;`

* **skel** — `float`, skeletal object, чьи данные нужно отобразить в память VM.

#### Описание и логика работы
`skel_mmap` отдаёт указатель на блок памяти VM, где лежат матрицы костей текущего skeletal object. Это низкоуровневый путь для кода, который уже использует pointer arithmetic и хочет массово читать или писать кости без отдельных builtin-вызовов на каждую из них. По комментариям FTEQW каждая кость занимает 12 `float`: это три вектора ориентации плюс смещение, уложенные как одна 3x4 matrix. Builtin не создаёт копию: вы получаете прямое отображение текущих данных объекта.

Если skeletal object невалиден либо принадлежит другой VM-world, возвращается нулевой указатель. Этот путь требует аккуратности: нужно точно знать раскладку памяти и не выходить за `skel_get_numbones(skel) * 12` элементов. В обычной игровой логике проще и безопаснее пользоваться `skel_get_bonerel`, `skel_get_boneabs` и `skel_set_bone`.

#### Практические сценарии использования
```
void() Debug_DumpFirstBoneMatrix =
{
	local float *bones;

	bones = skel_mmap(self.skeletonobject);
	if (!bones)
		return;

	dprint(sprintf("bone1 row0 = %g %g %g %g\n", bones[0], bones[1], bones[2], bones[3]));
	dprint(sprintf("bone1 row1 = %g %g %g %g\n", bones[4], bones[5], bones[6], bones[7]));
	dprint(sprintf("bone1 row2 = %g %g %g %g\n", bones[8], bones[9], bones[10], bones[11]));
};
```

### skel_premul_bone
`void(float skel, float bonenum, vector org, optional vector fwd, optional vector right, optional vector up) skel_premul_bone = #272;`

* **skel** — `float`, skeletal object, чью кость нужно домножить.
* **bonenum** — `float`, 1-based индекс кости.
* **org** — `vector`, смещение матрицы-модификатора.
* **fwd** — `vector`, forward-ось матрицы; если не указана, используется глобальный `v_forward`.
* **right** — `vector`, right-ось матрицы; если не указана, используется `v_right`.
* **up** — `vector`, up-ось матрицы; если не указана, используется `v_up`.

#### Описание и логика работы
`skel_premul_bone` домножает существующую матрицу кости новой матрицей слева, то есть модификатор применяется «до» уже накопленной локальной позы. На практике это хороший builtin для процедурных поворотов вроде доворота головы, хвоста или оружейной кости поверх анимации. Матрицу-модификатор удобно строить через [`makevectors`](01-math-vector-builtins.md#makevectors) из нужных углов, а затем передавать либо явно, либо неявно через текущие `v_forward`, `v_right`, `v_up`.

В реализации есть важный практический нюанс: premultiply вращает уже существующую позу вокруг переданного transform, поэтому при заметном `org` результат может быть не тем, что ожидается от «простого добавить угол». Для безопасного корректирующего поворота поверх анимации чаще всего передают `org = '0 0 0'`. При неверном `skel` или индексе кости builtin просто ничего не меняет.

#### Практические сценарии использования
```
void() AimHeadLeft =
{
	local float headbone;

	headbone = skel_find_bone(self.skeletonobject, "Bip01 Head");
	if (!headbone)
		return;

	makevectors('0 20 0');
	skel_premul_bone(self.skeletonobject, headbone, '0 0 0', v_forward, v_right, v_up);
};
```

### skel_premul_bones
`void(float skel, float startbone, float endbone, vector org, optional vector fwd, optional vector right, optional vector up) skel_premul_bones = #273;`

* **skel** — `float`, skeletal object, где нужно править кости.
* **startbone** — `float`, начало диапазона; `0` означает старт с первой кости.
* **endbone** — `float`, верх диапазона; `0` означает весь хвост до конца скелета.
* **org** — `vector`, смещение матрицы-модификатора.
* **fwd** — `vector`, forward-ось матрицы; при отсутствии берётся `v_forward`.
* **right** — `vector`, right-ось матрицы; при отсутствии берётся `v_right`.
* **up** — `vector`, up-ось матрицы; при отсутствии берётся `v_up`.

#### Описание и логика работы
`skel_premul_bones` делает то же самое, что `skel_premul_bone`, но сразу для последовательного диапазона костей. Это удобно для цепочек вроде хвоста, щупальца, позвоночника или верёвочной секции, когда одна и та же матрица должна применяться ко многим звеньям. Диапазон режется по реальному числу костей; `0, 0` интерпретируется как весь скелет. Как и одиночная версия, builtin использует premultiply-порядок, поэтому смещение `org` влияет на итоговую дугу движения.

Типичный приём — разделить большой угол на число сегментов и вызвать builtin один раз для всего диапазона, если каждой кости нужен одинаковый маленький поворот. При неверном skeletal object builtin тихо ничего не делает.

#### Практические сценарии использования
```
void() CurlTail =
{
	local float tail_first;
	local float tail_last;

	tail_first = skel_find_bone(self.skeletonobject, "Tail01");
	tail_last = skel_find_bone(self.skeletonobject, "Tail06");
	if (!tail_first || !tail_last)
		return;

	makevectors('0 5 0');
	skel_premul_bones(self.skeletonobject, tail_first, tail_last, '0 0 0', v_forward, v_right, v_up);
};
```

### skel_ragupdate
`float(entity skelent, string dollcmd, float animskel) skel_ragupdate = #281;`

* **skelent** — `entity`, сущность, на которой уже есть skeletal object для работы рэгдолла.
* **dollcmd** — `string`, пустая строка для обычного шага симуляции либо команда управления `.doll`.
* **animskel** — `float`, skeletal object с анимационной позой, к которой ragdoll может притягиваться.

#### Описание и логика работы
`skel_ragupdate` управляет ragdoll-подсистемой FTEQW. Если `dollcmd` пуст, builtin просто обновляет физические тела и переносит результат в skeletal object сущности; именно такой вызов обычно делается каждый кадр после смерти. Если валидного ragdoll ещё нет, движок пытается инстанцировать его из `default .doll` для модели. Третий аргумент, `animskel`, задаёт позу-ориентир: при ненулевом значении ragdoll стремится к этой анимации, а при `0` опирается на base pose модели, что часто выглядит хуже.

Команды позволяют менять режим на лету: `doll filename.doll`, `dollstring ...`, `cleardoll`, `animate weight`, `animatebody name weight`, `enablejoint name 0/1`. Builtin возвращает `float`, но как командный интерфейс важнее побочный эффект, чем численное значение. Если entity не имеет корректного skeletal object или ragdoll для неё невозможен, рассчитывать на полезный результат нельзя.

#### Практические сценарии использования
```
void() Corpse_StartRagdoll =
{
	if (!self.skeletonobject)
		self.skeletonobject = skel_create(self.modelindex);

	skel_build(self.skeletonobject, self, self.modelindex, 0, 0, 0);
	skel_ragupdate(self, "doll models/monsters/ogre.doll", self.skeletonobject);
	skel_ragupdate(self, "animate 0.35", self.skeletonobject);
};

void() Corpse_ThinkRagdoll =
{
	skel_ragupdate(self, "", self.skeletonobject);
};
```

### skel_set_bone
`void(float skel, float bonenum, vector org, optional vector fwd, optional vector right, optional vector up) skel_set_bone = #271;`

* **skel** — `float`, skeletal object, в который записывается новая поза.
* **bonenum** — `float`, 1-based индекс кости.
* **org** — `vector`, локальное смещение кости относительно родителя.
* **fwd** — `vector`, forward-ось новой ориентации; если аргумент пропущен, используется `v_forward`.
* **right** — `vector`, right-ось новой ориентации; если аргумент пропущен, используется `v_right`.
* **up** — `vector`, up-ось новой ориентации; если аргумент пропущен, используется `v_up`.

#### Описание и логика работы
`skel_set_bone` полностью заменяет локальную трансформацию кости в skeletal object. Builtin работает в пространстве родителя, поэтому обычно его используют либо после `skel_get_bonerel`, либо когда уже заранее известны нужные локальные оси и смещение. Если ориентация не передана явно, движок возьмёт текущие глобальные `v_forward`, `v_right`, `v_up`; это делает удобным шаблон «прочитал кость, чуть поправил, записал обратно».

При неверном `skel` или `bonenum` ничего не меняется. Builtin не знает о world-space позиции entity и не пытается согласовать результат с абсолютной позой; если нужна постановка кости по мировым координатам, используйте `skel_set_bone_world`.

#### Практические сценарии использования
```
void() RaiseWeaponBone =
{
	local float weaponbone;
	local vector relorg;

	weaponbone = skel_find_bone(self.skeletonobject, "tag_weapon");
	if (!weaponbone)
		return;

	relorg = skel_get_bonerel(self.skeletonobject, weaponbone);
	relorg_z = relorg_z + 2;
	skel_set_bone(self.skeletonobject, weaponbone, relorg, v_forward, v_right, v_up);
};
```

### skel_set_bone_world
`void(entity ent, float bonenum, vector org, optional vector angorfwd, optional vector right, optional vector up) skel_set_bone_world = #283;`

* **ent** — `entity`, сущность, у которой уже привязан skeletal object.
* **bonenum** — `float`, 1-based индекс кости.
* **org** — `vector`, желаемая мировая позиция кости.
* **angorfwd** — `vector`, либо углы Quake одним аргументом, либо forward-вектор новой ориентации.
* **right** — `vector`, right-ось, если ориентация задаётся тремя векторами.
* **up** — `vector`, up-ось, если ориентация задаётся тремя векторами.

#### Описание и логика работы
`skel_set_bone_world` — более высокий уровень, чем `skel_set_bone`: вы задаёте положение кости в world-space, а движок сам переводит его в локальную матрицу относительно родителя и текущего transform самой entity. Это удобно для IK-подобных задач «поставить руку на кнопку» или «прибить стопу к поверхности». Builtin понимает три режима ориентации: если передан один дополнительный аргумент, он трактуется как углы; если переданы три — как `forward/right/up`; если не передано ничего, используются текущие глобальные `v_*`.

Работает builtin только через skeletal object, уже привязанный к сущности. При невалидной entity, отсутствующем skeleton или неверном номере кости вызов тихо игнорируется. Если parent bone не удаётся получить из текущей позы, расчёт опирается на transform самой entity, поэтому результат для корневых костей обычно наиболее предсказуем.

#### Практические сценарии использования
```
void() PinHandToButton =
{
	local float handbone;
	local vector target;

	handbone = skel_find_bone(self.skeletonobject, "Bip01 R Hand");
	if (!handbone)
		return;

	target = self.enemy.origin + '0 0 24';
	makevectors(self.angles);
	skel_set_bone_world(self, handbone, target, v_forward, v_right, v_up);
};
```

### gettagindex
`float(entity ent, string tagname) gettagindex = #451;`

* **ent** — `entity`, чья текущая модель содержит теги или кости для привязки.
* **tagname** — `string`, имя тега/кости, например `"tag_weapon"` или `"tag_head"`.

#### Описание и логика работы
`gettagindex` ищет тег или кость по имени на модели указанной сущности и возвращает его числовой индекс. Именно этот индекс потом записывают в `.tag_index` дочерней сущности или передают в `gettaginfo`. В FTEQW lookup общий для классических MD3-тегов и для костей скелетных моделей, поэтому один и тот же builtin подходит как для «оружие к руке», так и для «шлем к кости головы». Если имя пустое, модель отсутствует или тег не найден, функция возвращает `0`.

Лучший практический подход — вызывать builtin один раз при создании составного объекта и сохранять номер в полях entity. Это избавляет от строкового поиска каждый кадр и уменьшает риск незаметной опечатки в имени.

#### Практические сценарии использования
```
void() AttachPlayerParts =
{
	self.torsoent.tag_entity = self.legsent;
	self.torsoent.tag_index = gettagindex(self.legsent, "tag_torso");

	self.headent.tag_entity = self.torsoent;
	self.headent.tag_index = gettagindex(self.torsoent, "tag_head");

	self.weaponent.tag_entity = self.torsoent;
	self.weaponent.tag_index = gettagindex(self.torsoent, "tag_weapon");
};
```

### gettaginfo
`vector(entity ent, float tagindex) gettaginfo = #452;`

* **ent** — `entity`, у которой нужно вычислить положение тега/кости.
* **tagindex** — `float`, индекс, ранее полученный через `gettagindex`.

#### Описание и логика работы
`gettaginfo` вычисляет текущее world-space положение тега или кости с учётом анимации модели, transform самой entity и цепочки tag attachment. Возвращаемое значение — мировая точка origin, а `v_forward`, `v_right`, `v_up` получают мировую ориентацию этого тега. Это builtin, который используют для спавна muzzle flash, выравнивания дочерней модели, расчёта камеры от bone и любых эффектов «строго из руки/головы/ствола». Если тег не найден на модели в момент вычисления, движок подставляет identity-tag, так что результатом станет transform самой entity или всей attachment-цепочки.

Внутри FTEQW builtin умеет подниматься по цепочке `.tag_entity`/`.tag_index` примерно до десяти уровней, поэтому вложенные конструкции вроде legs -> torso -> head -> helmet тоже корректно разворачиваются в мир. Для отсутствующей модели или неверного `tagindex` рассчитывайте на безопасный, но обычно бесполезный identity-результат; надёжнее заранее проверять индекс через `if (!tagindex)`.

#### Практические сценарии использования
```
void() SpawnMuzzleFlash =
{
	local float flash;
	local vector org;

	flash = gettagindex(self.weaponent, "tag_flash");
	if (!flash)
		return;

	org = gettaginfo(self.weaponent, flash);
	pointparticles(particletypeforname("muzzleflash"), org, v_forward * 32, 1);
};
```

### getsurfaceclippedpoint
`vector(entity e, float s, vector p) getsurfaceclippedpoint = #439;`

* **e** — `entity`, brush-model entity, чью поверхность нужно использовать.
* **s** — `float`, индекс поверхности в диапазоне `0 .. getsurfacenum* - 1`.
* **p** — `vector`, произвольная точка в мире, которую нужно спроецировать на эту поверхность.

#### Описание и логика работы
`getsurfaceclippedpoint` берёт точку `p`, проецирует её на плоскость указанной поверхности и затем зажимает внутрь фактического полигона/треугольников поверхности. Это очень полезно для эффекта «найти ближайшую допустимую точку на конкретной поверхности», например для декалей, курсоров на панели, искр по стене или привязки локального UI к world-геометрии. В текущей реализации FTEQW builtin работает именно с brush surfaces (`mod_brush`), то есть с worldspawn и brush entities, а не с alias/iqm-мешами. При неверной entity или индексе возвращается исходная точка `p` без изменений.

Для Quake 1 полигональных поверхностей и для более новых triangulated поверхностей движок использует разные внутренние пути, но внешнее поведение одинаковое: вы получаете ближайшую точку, лежащую на самой поверхности. Если поверхность вырождена или геометрия не загрузилась, лучше предварительно иметь резервный план в вашем QC.

#### Практические сценарии использования
```
vector() ClampDecalToHitSurface =
{
	local float surf;

	surf = getsurfacenearpoint(trace_ent, trace_endpos);
	if (surf < 0)
		return trace_endpos;

	return getsurfaceclippedpoint(trace_ent, surf, trace_endpos);
};
```

### getsurfacenearpoint
`float(entity e, vector p) getsurfacenearpoint = #438;`

* **e** — `entity`, brush-model entity, по чьим поверхностям идёт поиск.
* **p** — `vector`, точка в мире, для которой ищется ближайшая поверхность.

#### Описание и логика работы
`getsurfacenearpoint` перебирает поверхности brush-модели и возвращает индекс той, которая ближе всего к заданной точке. Это удобный первый шаг для всех задач вида «я попал лучом в мир, теперь хочу знать конкретную поверхность». Возвращаемое значение — surface index, который затем можно передать в `getsurfacetexture`, `getsurfacenormal`, `getsurfaceclippedpoint`, `getsurfacepointattribute` и другие getsurface-builtins. Если подходящей brush-модели нет, результатом будет `-1`.

Так как builtin реально сравнивает расстояния до поверхностей, он может быть дороже простого чтения уже известного surface index. В игровом коде обычно его вызывают в ответ на trace/hit, а не бесконечно в каждом think у десятков сущностей.

#### Практические сценарии использования
```
void() InspectHitSurface =
{
	local float surf;

	surf = getsurfacenearpoint(trace_ent, trace_endpos);
	if (surf < 0)
		return;

	dprint(sprintf("hit surface %g uses shader %s\n", surf, getsurfacetexture(trace_ent, surf)));
};
```

### getsurfacenormal
`vector(entity e, float s) getsurfacenormal = #436;`

* **e** — `entity`, brush-model entity.
* **s** — `float`, индекс поверхности.

#### Описание и логика работы
`getsurfacenormal` возвращает нормаль поверхности. Для планарных brush surfaces это именно геометрическая нормаль полигона, а для поверхностей без единой плоскости движок возвращает `0 0 0`, чтобы не выдавать ложный результат. Это удобно для отскоков, ориентации decals, вычисления «смотрит ли игрок на панель» и распределения частиц вдоль стены. В реализации FTEQW builtin работает только для brush-моделей; при неверной entity, неправильном индексе или непланарной поверхности результат тоже будет нулевым вектором.

Если поверхность помечена как обратная сторона плоскости, движок инвертирует нормаль так, чтобы на выходе она соответствовала фактически видимой стороне. Поэтому в обычном QC дополнительный ручной `-normal` не нужен.

#### Практические сценарии использования
```
void() BounceSparkFromSurface =
{
	local float surf;
	local vector n;
	local vector vel;

	surf = getsurfacenearpoint(trace_ent, trace_endpos);
	if (surf < 0)
		return;

	n = getsurfacenormal(trace_ent, surf);
	vel = normalize(v_forward - (2 * (v_forward * n)) * n) * 200;
	particleeffectnum(trace_endpos, vel, 0, particleeffectnum("spark"));
};
```

### getsurfacenumpoints
`float(entity e, float s) getsurfacenumpoints = #434;`

* **e** — `entity`, brush-model entity.
* **s** — `float`, индекс поверхности.

#### Описание и логика работы
`getsurfacenumpoints` возвращает число вершин указанной поверхности. При первом обращении движок при необходимости достраивает внутренний mesh поверхности, поэтому этот builtin часто полезно вызвать перед `getsurfacepoint` или `getsurfacepointattribute`. В текущей реализации это API для brush surfaces, а не для IQM/MD3-мешей. Если entity не указывает на brush model, индекс поверхности вне диапазона или mesh не смог построиться, вернётся `0`.

Число точек не обязательно совпадает с числом треугольников: одна поверхность может быть большим полигоном с несколькими triangle indices. Для триангуляции есть отдельные `getsurfacenumtriangles` и `getsurfacetriangle`.

#### Практические сценарии использования
```
void() EmitParticlesOnSurfaceVertices =
{
	local float surf;
	local float i;
	local float count;

	surf = getsurfacenearpoint(trace_ent, trace_endpos);
	if (surf < 0)
		return;

	count = getsurfacenumpoints(trace_ent, surf);
	for (i = 0; i < count; i = i + 1)
		pointparticles(particletypeforname("dust"), getsurfacepoint(trace_ent, surf, i), '0 0 0', 1);
};
```

### getsurfacenumtriangles
`float(entity e, float s) getsurfacenumtriangles = #628;`

* **e** — `entity`, brush-model entity.
* **s** — `float`, индекс поверхности.

#### Описание и логика работы
`getsurfacenumtriangles` возвращает число треугольников в mesh-представлении поверхности. Builtin полезен, когда нужно обойти поверхность по индексам треугольников, например для более точного случайного семплирования площади, собственного расчёта коллизий или UV-интерполяции внутри triangle. Как и другие getsurface-builtins, он относится к brush geometry и вернёт `0`, если поверхность недоступна или mesh не построен.

Важная практическая разница относительно `getsurfacenumpoints`: тут вы обходите уже не вершины по порядку хранения, а реально используемые triangles. Для доступа к самим индексам служит `getsurfacetriangle`.

#### Практические сценарии использования
```
float() CountHitSurfaceTriangles =
{
	local float surf;

	surf = getsurfacenearpoint(trace_ent, trace_endpos);
	if (surf < 0)
		return 0;

	return getsurfacenumtriangles(trace_ent, surf);
};
```

### getsurfacepoint
`vector(entity e, float s, float n) getsurfacepoint = #435;`

* **e** — `entity`, brush-model entity.
* **s** — `float`, индекс поверхности.
* **n** — `float`, индекс вершины внутри поверхности.

#### Описание и логика работы
`getsurfacepoint` возвращает координаты одной вершины поверхности. Это самый прямой способ прочитать геометрию полигона в QC: после `getsurfacenumpoints` можно пройти по всем вершинам и использовать их для дебага, процедурных эффектов или собственных геометрических расчётов. Реализация рассчитана на уже существующий surface mesh, поэтому на практике лучше сначала вызвать `getsurfacenumpoints` или `getsurfacepointattribute`, которые обеспечивают построение mesh при необходимости. При неверной entity или surface index builtin возвращает `0 0 0`.

Индекс вершины тоже должен быть в допустимом диапазоне. Если читать точки вслепую за пределами количества вершин, безопаснее сначала ограничить цикл числом из `getsurfacenumpoints`.

#### Практические сценарии использования
```
void() Debug_DrawSurfaceOutline =
{
	local float surf;
	local float i;
	local float count;

	surf = getsurfacenearpoint(trace_ent, trace_endpos);
	if (surf < 0)
		return;

	count = getsurfacenumpoints(trace_ent, surf);
	for (i = 0; i < count; i = i + 1)
		pointparticles(particletypeforname("debug_dot"), getsurfacepoint(trace_ent, surf, i), '0 0 0', 1);
};
```

### getsurfacepointattribute
`vector(entity e, float s, float n, float a) getsurfacepointattribute = #486;`

* **e** — `entity`, brush-model entity.
* **s** — `float`, индекс поверхности.
* **n** — `float`, индекс вершины.
* **a** — `float`, код атрибута: `SPA_POSITION`, `SPA_S_AXIS`, `SPA_T_AXIS`, `SPA_R_AXIS`, `SPA_TEXCOORDS0`, `SPA_LIGHTMAP0_TEXCOORDS`, `SPA_LIGHTMAP0_COLOR`.

#### Описание и логика работы
`getsurfacepointattribute` возвращает не только позицию вершины, но и другие связанные данные mesh-вершины. В FTEQW коды атрибутов объявлены в `fteextensions.qc`: `SPA_POSITION = 0`, `SPA_S_AXIS = 1`, `SPA_T_AXIS = 2`, `SPA_R_AXIS = 3` (то же, что normal), `SPA_TEXCOORDS0 = 4`, `SPA_LIGHTMAP0_TEXCOORDS = 5`, `SPA_LIGHTMAP0_COLOR = 6`. Это builtin для продвинутых сценариев: вычислить UV под курсором, восстановить tangent space или читать baked lightmap color на вершине. При любой ошибке движок возвращает `0 0 0`.

Заметьте, что для UV builtin использует `vector`, но meaningful-компоненты только две, а третья равна `0`. Для цвета светокарты альфа отдельно не возвращается. Как и весь getsurface API, builtin сейчас ориентирован на brush geometry.

#### Практические сценарии использования
```
vector() SampleFirstVertexUV =
{
	local float surf;

	surf = getsurfacenearpoint(trace_ent, trace_endpos);
	if (surf < 0)
		return '0 0 0';

	return getsurfacepointattribute(trace_ent, surf, 0, SPA_TEXCOORDS0);
};
```

### getsurfacetexture
`string(entity e, float s) getsurfacetexture = #437;`

* **e** — `entity`, brush-model entity.
* **s** — `float`, индекс поверхности; для отрицательных значений builtin умеет специальный режим доступа по texture slot.

#### Описание и логика работы
`getsurfacetexture` возвращает имя texture/shader, назначенного поверхности. Это основной builtin для диагностики материала под курсором, логики по имени шейдера и интеграции world UI. Если `s` обычный неотрицательный индекс, читается texture у конкретной поверхности. У FTEQW есть и дополнительный режим: отрицательные значения интерпретируются как прямой доступ к списку textures модели по формуле `-1 - s`. При неверной entity, неподходящей модели или индексе builtin возвращает пустую строку.

Для игрового кода отрицательный режим нужен редко, но полезно знать о нём при написании инструментов и отладочных меню. В большинстве случаев берите индекс из `getsurfacenearpoint` и сразу передавайте его сюда.

#### Практические сценарии использования
```
string() DescribeHitMaterial =
{
	local float surf;
	local string tex;

	surf = getsurfacenearpoint(trace_ent, trace_endpos);
	if (surf < 0)
		return "no brush surface";

	tex = getsurfacetexture(trace_ent, surf);
	if (tex == "")
		return "surface has no readable texture name";
	return tex;
};
```

### getsurfacetriangle
`vector(entity e, float s, float n) getsurfacetriangle = #629;`

* **e** — `entity`, brush-model entity.
* **s** — `float`, индекс поверхности.
* **n** — `float`, индекс треугольника внутри поверхности.

#### Описание и логика работы
`getsurfacetriangle` возвращает три vertex indices одного треугольника в виде `vector`, где каждая компонента — это индекс вершины для последующего чтения через `getsurfacepoint` или `getsurfacepointattribute`. Такой интерфейс кажется необычным, но для QuakeC он удобен: один вызов сразу отдаёт весь triangle. Если triangle index некорректен, поверхность невалидна или mesh недоступен, builtin возвращает `0 0 0`.

Вместе с `getsurfacenumtriangles` это даёт полный доступ к триангулированной версии brush surface. Дальше в QC можно самостоятельно считать площади, barycentric coordinates, собственные нормали или UV-интерполяцию внутри треугольника.

#### Практические сценарии использования
```
void() Debug_DrawFirstTriangle =
{
	local float surf;
	local vector tri;

	surf = getsurfacenearpoint(trace_ent, trace_endpos);
	if (surf < 0 || getsurfacenumtriangles(trace_ent, surf) <= 0)
		return;

	tri = getsurfacetriangle(trace_ent, surf, 0);
	pointparticles(particletypeforname("debug_dot"), getsurfacepoint(trace_ent, surf, tri_x), '0 0 0', 1);
	pointparticles(particletypeforname("debug_dot"), getsurfacepoint(trace_ent, surf, tri_y), '0 0 0', 1);
	pointparticles(particletypeforname("debug_dot"), getsurfacepoint(trace_ent, surf, tri_z), '0 0 0', 1);
};
```

### applycustomskin
`void(entity e, float skinobj) applycustomskin = #378;`

* **e** — `entity`, сущность, которой нужно назначить уже созданный skin object.
* **skinobj** — `float`, handle skin-объекта, полученный через `loadcustomskin`.

#### Описание и логика работы
`applycustomskin` применяет заранее загруженный custom skin к entity. Это путь для случаев, когда один и тот же skin object нужно переиспользовать на множестве сущностей: сначала один раз вызвать `loadcustomskin`, потом раздавать полученный handle через `applycustomskin`. В док-комментарии FTEQW skin object считается refcounted, так что после применения движок хранит skin до тех пор, пока он нужен entity. Builtin существует в CSQC.

Если `skinobj` невалиден, полезного изменения не произойдёт. Для однократной быстрой подстановки без явного handle можно использовать `setcustomskin`, но `applycustomskin` выигрывает, когда надо избежать повторного парсинга одинакового `.skin`-описания.

#### Практические сценарии использования
```
void() ApplyRedBlueSoldierSkin =
{
	local float skin;

	skin = loadcustomskin("", "q1upper 0x0000ff\nq1lower 0xff0000\nreplace \"body\" \"skins/soldier_body\"\n");
	if (!skin)
		return;

	applycustomskin(self, skin);
	releasecustomskin(skin);
};
```

### loadcustomskin
`float(string skinfilename, optional string skindata) loadcustomskin = #377;`

* **skinfilename** — `string`, имя `.skin`-файла на диске либо логическое имя skin-ресурса.
* **skindata** — `string`, необязательное встроенное содержимое `.skin`-файла.

#### Описание и логика работы
`loadcustomskin` создаёт skin object и возвращает его handle. Формат `skindata` следует синтаксису `.skin`: пары `surfacename,shadername`, команды `replace`, `qwskin`, `q1lower`, `q1upper`, а также `compose` для сборки диффузной текстуры из нескольких изображений. Если передан `skindata`, движок читает именно его; если строка пуста, skin пытается загрузиться по `skinfilename`. Этот builtin доступен в CSQC и особенно полезен там, где скин нужно подготовить один раз, а потом применять и отпускать независимо.

При ошибке загрузки или пустых аргументах функция возвращает `0`. Handle, полученный через `loadcustomskin`, нужно либо применить к entity, либо позже освободить `releasecustomskin`, чтобы не держать лишний skin object дольше необходимого.

#### Практические сценарии использования
```
float() BuildTeamSkin =
{
	local float skin;

	skin = loadcustomskin("",
		"replace \"helmet\" \"textures/team/blue_helmet\"\n"
		+ "replace \"armor\" \"textures/team/blue_armor\"\n"
		+ "q1upper 0x2020ff\n"
		+ "q1lower 0x101040\n");

	return skin;
};
```

### setcustomskin
`void(entity e, string skinfilename, optional string skindata) setcustomskin = #376;`

* **e** — `entity`, сущность, чей custom skin нужно заменить.
* **skinfilename** — `string`, имя skin-файла на диске.
* **skindata** — `string`, необязательное встроенное содержимое skin-описания.

#### Описание и логика работы
`setcustomskin` — высокоуровневый convenience builtin: он сам создаёт skin object из `skinfilename`/`skindata`, назначает его entity и отпускает старый skin этой сущности. Для MenuQC и CSQC это самый быстрый путь, когда skin нужен только одной сущности и отдельно переиспользовать handle не планируется. По смыслу builtin эквивалентен цепочке «загрузить новый skin -> заменить старый -> обновить entity skin refs», но делает это одной командой.

Если и `skinfilename`, и `skindata` пусты, builtin фактически очищает пользовательские skin overrides. При ошибке чтения `.skin` сущность просто останется без нового overrides-набора. В отличие от пары `loadcustomskin`/`applycustomskin`, явного handle вы здесь не видите.

#### Практические сценарии использования
```
void() Menu_UpdatePreviewColours =
{
	setcustomskin(self, "",
		sprintf("q1upper \"%s\"\nq1lower \"%s\"\nqwskin \"%s\"\n",
			cvar_string("topcolor"),
			cvar_string("bottomcolor"),
			cvar_string("skin")));
};
```

### releasecustomskin
`void(float skinobj) releasecustomskin = #379;`

* **skinobj** — `float`, handle, ранее полученный из `loadcustomskin`.

#### Описание и логика работы
`releasecustomskin` сообщает движку, что QC больше не нуждается в указанном skin object. Благодаря refcount-семантике это не означает мгновенное снятие скина со всех сущностей: если какой-то entity уже использует этот skin, он останется жить до тех пор, пока ссылка реально не станет не нужна. Именно поэтому безопасный шаблон в FTEQW выглядит так: `skin = loadcustomskin(...); applycustomskin(ent, skin); releasecustomskin(skin);`.

Если передан `0` или неверный handle, builtin ничего полезного не делает. Использовать `releasecustomskin` нужно только для skin handles, которыми вы сами управляете через `loadcustomskin`; skin, установленный напрямую через `setcustomskin`, отдельно отпускать не требуется.

#### Практические сценарии использования
```
void() SwapSkinForOneFrame =
{
	local float skin;

	skin = loadcustomskin("", "replace \"screen\" \"textures/ui/alert_screen\"\n");
	if (!skin)
		return;

	applycustomskin(self, skin);
	releasecustomskin(skin);
};
```

### setcolors
`__deprecated("No RGB support.") void(entity ent, float colours) setcolors = #401;`

* **ent** — `entity`, игрок-клиент, чьи legacy-цвета нужно изменить.
* **colours** — `float`, packed nibble value: биты `0-3` задают lower/trousers, биты `4-7` — upper/shirt.

#### Описание и логика работы
`setcolors` — устаревший серверный builtin из классической Quake-парадигмы смены top/bottom colors у player model. В FTEQW он помечен как deprecated и прямо сообщает, что полноценной RGB-поддержки здесь нет: речь идёт именно о старой индексной палитровой схеме. Builtin ожидает именно client entity; если попытаться вызвать его на не-клиенте, движок напечатает предупреждение и ничего полезного не сделает. Поэтому использовать его стоит только ради совместимости со старым SSQC-кодом и классическими player skins.

Для новых проектов, где нужны более богатые материалы, разные surface shaders или точные цвета, значительно гибче работает система `setcustomskin`/`loadcustomskin`. Но если мод остаётся в рамках QuakeWorld/Quake1-стиля одежды игрока, `setcolors` всё ещё даёт простой и понятный путь.

#### Практические сценарии использования
```
void() GivePlayerBlueShirtRedPants =
{
	local float colours;

	// lower = 4, upper = 13.
	colours = 4 + (13 * 16);
	setcolors(self, colours);
};
```

### skel_build_ptr
`float(float skel, int numblends, skelblend_t *weights, int structsize) skel_build_ptr = #0:skel_build_ptr;`

* **skel** — `float`, skeletal object, индекс целевого скелета; в отличие от `skel_build`, передача `0` здесь не вызовет полного пересоздания.
* **numblends** — `int`, количество передаваемых структур `skelblend_t` для смешивания.
* **weights** — `skelblend_t *`, указатель на массив blend-дескрипторов; каждая структура должна содержать `sourcemodelindex`, базовый фрейм, `prescale`, а также массивы frame/weight/time для HL-подобных костей типа controller/subblend.
* **structsize** — `int`, размер одной структуры массива; обычно передается `sizeof(skelblend_t)`.

#### Описание и особенности работы

`skel_build_ptr` является аналогом функции `skel_build` для тех, кому удобнее передавать blend-параметры в виде прямого указателя. Оригинальное описание функции гласит, что она работает *"slightly simpler"*, но на самом деле имеет ряд специфических ограничений: массив должен быть четко структурирован, содержать корректные ссылки на индексы моделей, prescale и тайминги анимации. При выполнении кода происходит цепочка внутренних проверок: skeleton должен быть инициализирован, `structsize` не должен быть меньше `sizeof(skelblend_t)`, указатель массива не должен ссылаться на нулевой адрес, а non-relative skeleton не приведет к генерации костей и вызовет немедленный `return`. Рекомендуется проверять и сам handle `skel`, если он `0`, во избежание сбоев/ошибок выполнения.

#### Пример использования

```quakec
skelblend_t player_blends[1];

void() RebuildPoseWithBlendArray =
{
    if (!self.skeletonobject)
        self.skeletonobject = skel_create(self.modelindex);

    player_blends[0].sourcemodelindex = self.modelindex;
    player_blends[0].firstbone = 0;
    player_blends[0].lastbone = 0;
    player_blends[0].prescale = 0;
    player_blends[0].scale[0] = 1;
    player_blends[0].animation[0] = self.frame;
    player_blends[0].animationtime[0] = 0;

    skel_build_ptr(self.skeletonobject, 1, &player_blends[0], sizeof(player_blends[0]));
};
```

### skel_postmul_bone
`void(float skel, float bonenum, vector org, optional vector fwd, optional vector right, optional vector up) skel_postmul_bone = #0:skel_postmul_bone;`

* **skel** — `float`, skeletal object, в котором будет изменена кость.
* **bonenum** — `float`, 1-based индекс кости (начиная с 1).
* **org** — `vector`, вектор смещения для матрицы трансформации.
* **fwd** — `vector`, вектор направления forward для матрицы.
* **right** — `vector`, вектор направления right для матрицы.
* **up** — `vector`, вектор направления up для матрицы.

#### Описание и особенности работы

`skel_postmul_bone` применяет дополнительную матрицу трансформации после текущих вычислений кости, то есть выполняет пост-умножение (post-multiplication) геометрических преобразований для одной конкретной кости. Если векторы `fwd/right/up` не переданы, builtin автоматически использует глобальные переменные `v_forward`, `v_right`, `v_up`, что упрощает рабочий процесс с использованием базовой функции `makevectors` перед вызовом `skel_postmul_bone`. В отличие от `skel_premul_bone`, где трансформация применяется до основных вычислений родительских костей, этот метод идеален для симуляции динамических эффектов вроде sway (покачивание), recoil (отдача) или twisting (скручивание) кистей. При неверном skeleton id или некорректном bone index вызов просто игнорируется движком.

#### Пример использования

```quakec
void() TwistRightHand =
{
    local float handbone;

    handbone = skel_find_bone(self.skeletonobject, "Bip01 R Hand");
    if (!handbone)
        return;

    makevectors('0 0 15');
    skel_postmul_bone(self.skeletonobject, handbone, '0 0 0', v_forward, v_right, v_up);
};
```

### skel_postmul_bones
`void(float skel, float startbone, float endbone, vector org, optional vector fwd, optional vector right, optional vector up) skel_postmul_bones = #0:skel_postmul_bones;`

* **skel** — `float`, skeletal object, скелет для массовой трансформации.
* **startbone** — `float`, индекс начала последовательного диапазона костей.
* **endbone** — `float`, индекс конца диапазона.
* **org** — `vector`, вектор смещения.
* **fwd** — `vector`, вектор направления forward.
* **right** — `vector`, вектор направления right.
* **up** — `vector`, вектор направления up.

#### Описание и особенности работы

`skel_postmul_bones` — это потенциально полезный builtin метод для групповой трансформации цепочки костей. В таблицах встроенных функций движка (server/client builtin tables) он описан как *"Transforms an entire consecutive range of bones by a matrix"*, однако в коде движка присутствует критический баг со связыванием в контекстах CSQC/MenuQC, из-за чего вызов падает. Суть проблемы: в `fteextensions.qc` объявление функции есть, однако в builtin-таблицах файлов `pr_csqc.c`/`pr_menu.c` привязка пропущена, а в `pr_common.h` макрос `PF_skel_postmul_bones` вместо вызова реальной функции указывает на `PF_Fixme`. По этой причине вызывать данный builtin напрямую нельзя, и разработчикам приходится писать обертку, которая в цикле выполняет поэлементную трансформацию с помощью стабильно работающей `skel_postmul_bone`.

#### Пример использования

```quakec
void() BendTailRange_Workaround =
{
    local float bone;

    makevectors('0 3 0');
    for (bone = self.tail_firstbone; bone <= self.tail_lastbone; bone = bone + 1)
        skel_postmul_bone(self.skeletonobject, bone, '0 0 0', v_forward, v_right, v_up);
};
```

## Смежные страницы

- [Современные скелетные модели (IQM/MD5/DPM/ZYM)](../02-models-animation/skeletal-models-iqm-md5-dpm-zym.md)
- [Скелетные теги и присоединение объектов (tag attachment)](../02-models-animation/skeletal-tags-attachment.md)
- [Индекс справочника builtins](./README.md)