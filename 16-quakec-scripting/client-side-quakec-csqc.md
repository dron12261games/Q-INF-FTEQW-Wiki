# Клиентская логика и интерфейс (CSQC)

> [⬅ Предыдущая страница](server-side-quakec-ssqc.md) | [Следующая страница ➡](menu-quakec.md)

> [⬅ Вернуться к оглавлению вики](../README.md)

## Что это даёт геймдизайнеру

CSQC (Client-Side QuakeC) — это второй, отдельный скрипт на том же языке, что и SSQC, но выполняющийся **на компьютере каждого игрока**, а не на сервере. Он отвечает за всё, что видит и слышит именно этот игрок: отрисовку экрана и HUD (интерфейса поверх игры), реакцию на нажатия клавиш и движения мыши до того, как они уйдут на сервер, локальные визуальные эффекты (искры, частицы, кастомную анимацию), а также может полностью заменить стандартный игровой экран собственным. Это открывает возможность делать полностью кастомные интерфейсы, HUD-элементы, зрительские режимы и локальные эффекты без нагрузки на сервер и без необходимости пересылать по сети лишние данные.

Ниже — не только общая идея, но и подробный справочник по тому, какие именно функции ожидает от CSQC-скрипта движок, и какой полный набор инструментов рисования/чтения сети/управления доступен разработчику.

---

## Интерфейс настройки

CSQC обычно компилируется в отдельный бинарный файл **`csprogs.dat`** — именно это имя по умолчанию публикует серверная переменная [`sv_csqc_progname`](../38-cvars-reference/04-network-server-cvars.md#sv_csqc_progname). Если сервер использует полную клиентскую логику, клиент загружает указанный файл. Если такой логики нет, движок обычно остаётся на встроенном интерфейсе; отдельно существует ещё и более ограниченный режим **SimpleCSQC**, где вместо полноценного `csprogs.dat` может использоваться обычный `progs.dat` только для HUD и таблицы результатов. В одиночной игре, при проигрывании демо или при включённых читах движок также может подхватывать локальный `csaddon.dat` как клиентский аддон.

Как и SSQC, клиентский скрипт можно собрать заранее и просто положить `csprogs.dat` в папку мода, либо прямо в движке командой `compile csprogs.src`.

### Точки входа, которые вызывает движок

| Функция | Когда вызывается |
|---|---|
| [`CSQC_Init()`](../37-quakec-builtins-reference/00-entry-points.md#csqc_init) | один раз при загрузке клиентской логики — стартовая точка для инициализации |
| [`CSQC_WorldLoaded()`](../37-quakec-builtins-reference/00-entry-points.md#csqc_worldloaded) | когда клиент загрузил новую карту — здесь удобно через [`getentitytoken`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getentitytoken) вручную прочитать список объектов карты и создать чисто клиентские объекты (например, декоративные телепорты, не существующие на сервере) |
| [`CSQC_UpdateView(width, height, notmenu)`](../37-quakec-builtins-reference/00-entry-points.md#csqc_updateview) | основной полнофункциональный кадр CSQC: рисование мира, HUD и интерфейса |
| [`CSQC_UpdateViewLoading(width, height, notmenu)`](../37-quakec-builtins-reference/00-entry-points.md#csqc_updateviewloading) | отдельная точка входа для собственного экрана загрузки, если мод хочет заменить стандартный экран движка |
| [`CSQC_InputEvent(event_type, param_a, param_b)`](../37-quakec-builtins-reference/00-entry-points.md#csqc_inputevent) | при нажатии клавиш, кнопок мыши или движении
| [`CSQC_Input_Frame()`](../37-quakec-builtins-reference/00-entry-points.md#csqc_input_frame) | вызывается перед отправкой очередного `usercmd`; здесь можно править глобальные `input_*` для собственного клиентского ввода или предсказания |
| [`CSQC_ConsoleCommand("command")`](../37-quakec-builtins-reference/00-entry-points.md#csqc_consolecommand) | когда игрок вводит команду
| [`CSQC_Parse_StuffCmd("text")`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_stuffcmd) | когда сервер присылает команду на выполнение в консоли клиента — вместо автоматического исполнения решение об этом отдаётся на откуп скрипту |
| [`CSQC_Parse_CenterPrint("text")`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_centerprint) | при получении от сервера текста для центрального сообщения на экране |
| [`CSQC_Parse_Print("text", message_type)`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_print) | при получении обычного текста в консоль/чат от сервера |
| [`CSQC_Ent_Update(is_new)`](../37-quakec-builtins-reference/00-entry-points.md#csqc_ent_update) | при получении обновления сетевого объекта, отправленного специально для CSQC (см. ниже про `SendEntity`) |
| [`CSQC_Ent_Remove()`](../37-quakec-builtins-reference/00-entry-points.md#csqc_ent_remove) | когда ранее известный CSQC-объект удалён или перестал отправляться сервером |
| [`CSQC_Event_Sound(...)`](../37-quakec-builtins-reference/00-entry-points.md#csqc_event_sound) | при получении события звука с сервера |
| [`CSQC_Shutdown()`](../37-quakec-builtins-reference/00-entry-points.md#csqc_shutdown) | при штатной выгрузке CSQC |

Все перечисленные функции необязательны — если какая-то из них не описана в коде мода, движок просто её не вызывает. Для ограниченного режима SimpleCSQC вместо [`CSQC_UpdateView`](../37-quakec-builtins-reference/00-entry-points.md#csqc_updateview) обычно используются [`CSQC_DrawHud`](../37-quakec-builtins-reference/00-entry-points.md#csqc_drawhud) и [`CSQC_DrawScores`](../37-quakec-builtins-reference/00-entry-points.md#csqc_drawscores).

### Управление 3D-сценой

- **[`clearscene()`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#clearscene)** — очищает список объектов для отрисовки и сбрасывает параметры вида к значениям по умолчанию; вызывается в начале каждого кадра.
- **[`addentities(mask)`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#addentities)** — добавляет в сцену все «известные» объекты, отфильтрованные по битовой маске: `MASK_ENGINE` (обычные объекты, присланные не через CSQC-канал, включая классические временные эффекты), `MASK_ENGINEVIEWMODEL` (стандартная модель оружия от первого лица). Перед добавлением каждого объекта для него вызывается функция `predraw`.
- **[`addentity(entity)`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#addentity)** — добавляет в сцену ровно один конкретный объект немедленно (без вызова его `predraw`) — удобно для добавления «дополнительных копий» объекта (например, эффект щита вокруг игрока).
- **[`setviewprop(property, value)`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#setproperty)** / **[`getviewpropf(property)`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#getproperty)** / **[`getviewpropv(property)`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#getproperty)** — читают и меняют параметры текущего вида камеры.
- **[`adddynamiclight(position, radius, color)`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#dynamiclight_add)** — добавляет в сцену динамический источник света на этот кадр.
- **[`renderscene()`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#renderscene)** — фактически выполняет отрисовку всего, что было добавлено с момента [`clearscene`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#clearscene), согласно текущим параметрам вида; список объектов при этом не очищается автоматически.
- **[`project(vector)`](../37-quakec-builtins-reference/01-math-vector-builtins.md#project)** / **[`unproject(vector)`](../37-quakec-builtins-reference/01-math-vector-builtins.md#unproject)** — переводят координаты между трёхмерным игровым пространством и двумерным пространством экрана (с глубиной в диапазоне 0…1) с учётом текущих параметров вида.

### Двумерная отрисовка интерфейса

- **[`drawpic(position, "name image", size, color, alpha)`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawpic)** — отрисовывает изображение.
- **[`drawsubpic(...)`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawsubpic)** (расширение) — отрисовывает часть изображения (фрагмент текстуры) — полезно для атласов иконок.
- **[`drawfillrgb(position, size, color, alpha)`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawfill)** — закрашивает прямоугольник сплошным цветом (могут не поддерживать простые программные рендеры).
- **[`drawfillpal(position, size, index_palette)`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawfillpal)** — закрашивает прямоугольник цветом из палитры Quake.
- **[`drawcharacter(position, code_character, size, color, alpha)`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawcharacter)** — рисует один символ стандартным шрифтом консоли.
- **[`drawrawstring(position, "text", size, color, alpha)`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawrawstring)** — рисует строку текста стандартным шрифтом консоли без разметки цвета и без переноса строк.
- **[`drawcolorcodedstring(position, "text", scale, alpha)`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawstring)** — рисует текст с поддержкой цветовой разметки консоли (включая цветовые коды вида `^4`), с возможностью переменной ширины шрифта.
- **[`drawstring(...)`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawstring)** — упрощённый аналог для быстрой печати строки на экране, широко используемый для HUD.
- **[`precache_pic("name")`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#precache_pic)** / **[`iscachedpic("name")`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#iscachedpic)** / **[`freepic("name")`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#freepic)** — предзагружает изображение, проверяет, что оно уже загружено, и выгружает его из памяти соответственно.
- **[`drawgetimagesize("name")`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#drawgetimagesize)** — возвращает размеры изображения (или «0 0 0», если оно не загрузилось).

### Модели, частицы и звук

- **[`setmodelindex(entity, index_model)`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setmodelindex)** — присваивает объекту модель по числовому индексу.
- **[`modelnameforindex(index_model)`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#modelnameforindex)** — возвращает имя файла модели по её индексу.
- **[`particleeffectnum("effectname")`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#particleeffectnum)** — получает номер именованного эффекта частиц (в т.ч. стандартных: [`te_explosion`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_explosion), [`te_tarexplosion`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_tarexplosion), [`te_gunshot`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_gunshot), [`te_wizspike`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_wizspike), [`te_knightspike`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_knightspike), [`te_spike`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_spike), [`te_superspike`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_superspike), [`te_teleport`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_teleport), [`te_lavasplash`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#te_lavasplash)) для дальнейшей передачи в функции ниже.
- **[`pointparticles(number_effect, position, speed, multiplier_count)`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#pointparticles)** — проигрывает частицы точечного эффекта (без звука и без динамического света).
- **[`trailparticles(number_effect, entity, start, end)`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#trailparticles)** — проигрывает частицы эффекта-«шлейфа» вдоль отрезка (объект используется, чтобы шлейф оставался связным между кадрами при высоком fps; можно передать `world` для независимого одноразового луча).
- **[`setlistener(position, forward, right, up)`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#setlistener)** — задаёт положение и ориентацию «слушателя» для объёмного звука

### Управление вводом, вид и предсказание движения

- **[`setsensitivityscaler(multiplier)`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setsensitivityscaler)** — задаёт текущий множитель чувствительности мыши игрока (удобно для приближения оптического прицела); значение остаётся в силе, пока CSQC не поменяет его снова или пока логика не перезагрузится.
- **[`getinputstate(number_frame)`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getinputstate)** — считывает угол обзора и другие параметры ввода конкретного кадра предсказания в глобальные переменные `input_*`; возвращает ложь, если кадр устарел. Работает совместно с глобальными переменными `servercommandframe` (последний подтверждённый сервером кадр) и `clientcommandframe` (последний сгенерированный клиентом кадр) — кадры между ними нужно «доиграть» самостоятельно для правильного предсказания.
- **[`runstandardplayerphysics(entity)`](../37-quakec-builtins-reference/03-entity-world-builtins.md#runstandardplayerphysics)** — прогоняет через объект встроенный алгоритм предсказания движения движка (читает `input_*`, меняет [`origin`](../39-entity-keys-reference/01-worldspawn-common-keys.md#origin)/`velocity`/`pmove_flags`, вызывает функции столкновений).
- **[`keynumtostring(code_keys)`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#keynumtostring)** / **[`stringtokeynum("name")`](../37-quakec-builtins-reference/02-string-builtins.md#stringtokeynum)** — преобразуют код клавиши в её текстовое имя (как в команде [`bind`](../44-cli-commands-reference/02-client-ui-commands.md#bind)) и обратно.
- **[`getkeybind(code_keys)`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#getkeybind)** — возвращает полную команду, привязанную к клавише.
- **[`deltalisten("name model", update_function, flags)`](../37-quakec-builtins-reference/04-network-messages-builtins.md#deltalisten)** — регистрирует функцию, вызываемую при получении любого сетевого объекта с указанной моделью — способ выборочно «подписаться» на серверные объекты определённого типа. Флаги: `RSES_NOLERP` (отключить интерполяцию положения — критично для собственного игрока при предсказании), `RSES_NOROTATE` (не применять автоповорот по эффектам), `RSES_NOTRAILS` (не добавлять автоматический шлейф), `RSES_NOLIGHTS` (не добавлять автоматический динамический свет).

### Чтение сетевых сообщений

Когда движок передаёт CSQC на разбор специально адресованное сетевое сообщение, доступны builtin-функции последовательного чтения (действительны только в момент разбора): **[`readbyte`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readbyte)**, **[`readchar`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readchar)**, **[`readshort`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readshort)**, **[`readlong`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readlong)**, **[`readcoord`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readcoord)**, **[`readangle`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readangle)**, **[`readstring`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readstring)**, **[`readfloat`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readfloat)**, **[`readentitynum`](../37-quakec-builtins-reference/04-network-messages-builtins.md#readentitynum)** (последняя — читает номер объекта, который нужно сопоставить с полем `entnum`).

### Статистика игрока (stat'ы) и данные других игроков

- **[`getstatf(number)`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getstatf)** / **[`getstati(number)`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getstati)** / **[`getstati_bits(number, start_bit, count_bit)`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getstati)** / **[`getstats(number)`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getstats)**
- **[`getplayerkey(number_player, "key")`](../37-quakec-builtins-reference/04-network-messages-builtins.md#getplayerkeyvalue)** — получает данные другого игрока
- **[`serverkey("key")`](../37-quakec-builtins-reference/03-entity-world-builtins.md#serverkey)** — получает информацию о самом сервере (например, `ip` — адрес, использованный для подключения).
- **[`getentitytoken()`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getentitytoken)** — доступна только внутри

### Разное

- **[`registercommand("name")`](../37-quakec-builtins-reference/03-entity-world-builtins.md#registercommand)** — регистрирует консольную команду
- **[`wasfreed(entity)`](../37-quakec-builtins-reference/03-entity-world-builtins.md#wasfreed)** — сообщает, был ли объект недавно удалён и ещё не был переиспользован повторно (полезно после того, как код «поспал» — см. кооперативную многопоточность — и не уверен, жив ли ещё объект, который он помнит).
- **[`sendevent("name", "types_arguments", ...)`](../37-quakec-builtins-reference/04-network-messages-builtins.md#sendevent)** — отправляет на сервер именованное событие, которое там обрабатывается функцией `Cmd_name_types` (поддерживаемые типы аргументов: `f`-число, `e`-объект, `v`-вектор, `s`-строка; максимум 6 дополнительных аргументов).
- **[`cprint("text", ...)`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cprint)** — быстрый способ показать центральное сообщение на экране, тем же способом, что и стандартный ответ сервера (в отличие от прихода настоящего сетевого сообщения, не вызывает [`CSQC_Parse_CenterPrint`](../37-quakec-builtins-reference/00-entry-points.md#csqc_parse_centerprint)).
- **[`print("text", ...)`](../37-quakec-builtins-reference/12-system-debug-builtins.md#print)** — выводит текст в консоль и в область уведомлений (аналог [`dprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#dprint), но без необходимости включённого режима разработчика).
- **[`isdemo()`](../37-quakec-builtins-reference/04-network-messages-builtins.md#isdemo)** — сообщает, идёт ли сейчас проигрывание демозаписи.
- **[`isserver()`](../37-quakec-builtins-reference/04-network-messages-builtins.md#isserver)** — сообщает, может ли мод напрямую взаимодействовать с сервером через переменные и консольные команды (актуально в одиночной игре/при администрировании, но не должно использоваться, если на сервере есть настоящее голосование/античит).

### Как устроена отправка объектов именно на CSQC (для тех, кто хочет разобраться глубже)

На стороне SSQC для полностью кастомных сетевых объектов используется отдельная пара полей:

- **`.SendEntity(viewer, send_flags)`** — функция, которую сервер вызывает для каждого потенциального зрителя объекта: она сама решает, отправлять ли объект (вернув ложь — не отправлять, что удалит уже известную зрителю копию), и сама формирует содержимое сообщения через стандартные функции записи данных.
- **`.SendFlags`** — битовая маска «что изменилось с прошлой отправки»; логика должна лишь добавлять биты (через `|`), а не затирать их — движок сам периодически сбрасывает это поле после копирования.
- **`.pvsflags`** — управляет тем, как строго учитывается видимость объекта: обычная видимость, видимость с учётом «зоны слышимости», без учёта видимости вовсе, либо (`PVSF_NOREMOVE`) не удалять объект у клиента даже после того, как он скрылся из видимости.
- **[`clientstat(index, type, field)`](../37-quakec-builtins-reference/03-entity-world-builtins.md#clientstat)** / **[`globalstat(index, type, "name")`](../37-quakec-builtins-reference/03-entity-world-builtins.md#globalstat)** — регистрируют автоматическую отправку каждому игроку значения указанного поля (индивидуально для каждого) либо глобальной переменной (одинаково для всех) в виде стандартного «стата», без необходимости вручную писать сетевой код.

---

## Примеры

- Полностью кастомный HUD мода рисует свою полосу здоровья и счётчик патронов через [`drawpic`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawpic)/[`drawstring`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#drawstring), читая нужные значения через [`getstatf`](../37-quakec-builtins-reference/03-entity-world-builtins.md#getstatf), вместо использования стандартного интерфейса движка.
- Мод перехватывает нажатие определённой клавиши в [`CSQC_InputEvent`](../37-quakec-builtins-reference/00-entry-points.md#csqc_inputevent), чтобы локально, без задержки на отправку данных серверу, открыть кастомное меню инвентаря.
- Оружие со снайперским прицелом вызывает [`setsensitivityscaler(0.3)`](../37-quakec-builtins-reference/09-csqc-input-ui-builtins.md#setsensitivityscaler) при удержании кнопки прицеливания, возвращая обычную чувствительность при отпускании.
- Зрительский (наблюдательский) режим полностью реализован в CSQC, используя [`getplayerkey`](../37-quakec-builtins-reference/04-network-messages-builtins.md#getplayerkeyvalue) для построения табло результатов без лишней нагрузки на сервер.
- Полностью кастомный сетевой объект (например, индикатор здоровья союзника у него над головой) реализован через `.SendEntity`/`.SendFlags`, минуя стандартный протокол видимых объектов.

---

## Смежные страницы

- [Серверная игровая логика (SSQC)](./server-side-quakec-ssqc.md)
- [Логика игровых меню (MenuQC)](./menu-quakec.md)
- [Расширенные возможности движка для QuakeC](./quakec-language-basics.md#расширенные-возможности-движка-для-quakec)
- [Встроенные растровые шрифты](../07-fonts-text/builtin-bitmap-fonts.md)

> [⬅ Предыдущая страница](server-side-quakec-ssqc.md) | [Следующая страница ➡](menu-quakec.md)

> [⬅ Вернуться к оглавлению вики](../README.md)