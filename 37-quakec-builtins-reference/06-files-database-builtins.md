# Файлы, буферы, хеш-таблицы и базы данных

> [⬅ Вернуться к оглавлению вики](../README.md)

> [Индекс справочника builtins](./README.md)

Эта группа builtins покрывает несколько уровней хранения и обмена данными: обычные файлы в песочнице gamedir, строковые буферы, key-value хранилища, низкоуровневую адресуемую память и SQL-подключения. Большинство файловых операций по умолчанию направляются в `data/` внутри текущего мода и не позволяют выйти наружу через абсолютные пути, `..` или Windows-style пути с `\`. Для `buf_*` и `hash_*` движок возвращает числовые хендлы, которые нужно хранить и освобождать вручную. Функции семейства `mem*` особенно опасны: неверные указатели, смещения и размеры способны повредить данные QuakeC VM или уронить выполнение скрипта.

## Функции

### fopen
`filestream(string filename, float mode, optional float mmapminsize) fopen = #110;`

* **filename** — относительный путь к файлу внутри gamedir; для записи безопаснее явно использовать путь с префиксом `data/`.
* **mode** — режим открытия: `FILE_READ`, `FILE_READNL`, `FILE_WRITE`, `FILE_APPEND`, `FILE_MMAP_READ` или `FILE_MMAP_RW`.
* **mmapminsize** — минимальный размер memory-mapped буфера для `FILE_MMAP_*`; полезно, когда файл может быть создан с нуля.

#### Описание и логика работы
`fopen` возвращает числовой `filestream`-хендл или отрицательное значение при ошибке. По умолчанию движок запрещает абсолютные пути, `..`, двоеточия в имени и обратные слэши, а запись без `data/` перенаправляет в `data/filename`; чтение при этом может иметь fallback на исходный путь внутри gamedir, если это разрешено песочницей. `FILE_READ` читает файл построчно через `fgets`, `FILE_READNL` отдаёт всё содержимое одной строкой, `FILE_WRITE` начинает новый буфер записи, `FILE_APPEND` дописывает в конец, `FILE_MMAP_READ` и `FILE_MMAP_RW` дают прямой адресуемый блок памяти. Файл физически записывается на диск при `fclose`, поэтому незакрытый хендл означает потерю данных.

#### Практические сценарии использования
```qc
void() save_match_log =
{
    local filestream f;

    f = fopen("data/match.log", FILE_APPEND);
    if (f < 0)
        return;

    fputs(f, "map=", mapname, " time=", ftos(time), "\n");
    fclose(f);
};
```

### fclose
`void(filestream fhandle) fclose = #111;`

* **fhandle** — ранее открытый файловый хендл.

#### Описание и логика работы
`fclose` завершает работу с файлом и освобождает внутренние ресурсы движка. Для `FILE_WRITE`, `FILE_APPEND` и `FILE_MMAP_RW` именно здесь происходит фактическая запись буфера на диск; для режимов чтения и mmap освобождается память или VFS-хендл. Неверный дескриптор, чужой хендл из другой VM или повторное закрытие вызывают предупреждение и не дают полезного результата. Если забыть вызвать `fclose`, можно получить утечки хендлов и несохранённые данные.

#### Практические сценарии использования
```qc
void() rewrite_stats_file =
{
    local filestream f;

    f = fopen("data/stats.txt", FILE_WRITE);
    if (f < 0)
        return;

    fputs(f, "frags ", ftos(total_secrets), "\n");
    fclose(f); // именно здесь буфер гарантированно уходит на диск
};
```

### fgets
`string(filestream fhandle) fgets = #112;`

* **fhandle** — файловый хендл, открытый для чтения.

#### Описание и логика работы
В режиме `FILE_READ` функция возвращает следующую строку без завершающего символа новой строки; пустая строка из файла остаётся обычной пустой строкой, а конец файла возвращает null string, поэтому EOF удобно проверять через `if (!line)`. В режиме `FILE_READNL` за один вызов возвращается всё содержимое файла, а в `FILE_MMAP_*` — строковое представление указателя на начало буфера. Если хендл некорректен или открыт не для чтения, движок возвращает пустой результат и печатает предупреждение. Не полагайтесь на различие между «ошибка» и EOF только по тексту строки — храните контекст режима сами.

#### Практические сценарии использования
```qc
void() dump_whitelist =
{
    local filestream f;
    local string line;

    f = fopen("data/whitelist.txt", FILE_READ);
    if (f < 0)
        return;

    while (1)
    {
        line = fgets(f);
        if (!line)
            break;
        bprint("allow: ", line, "\n");
    }

    fclose(f);
};
```

### fputs
`void(filestream fhandle, string s, optional string s2, optional string s3, optional string s4, optional string s5, optional string s6, optional string s7) fputs = #113;`

* **fhandle** — файловый хендл, открытый для записи.
* **s..s7** — одна или несколько строк, которые движок склеит и запишет подряд.

#### Описание и логика работы
`fputs` пишет текст в текущую позицию файла или в конец буфера при `FILE_APPEND`. Символ перевода строки функция сама не добавляет, поэтому для совместимости с `fgets` почти всегда нужно вручную дописывать `"\n"`. В неподходящем режиме записи, при неверном хендле или переполнении внутреннего буфера запись частично или полностью не произойдёт. Для `FILE_MMAP_RW` запись идёт прямо в выделенный буфер и ограничена его размером.

#### Практические сценарии использования
```qc
void(entity pl) append_player_score =
{
    local filestream f;

    f = fopen("data/scoreboard.csv", FILE_APPEND);
    if (f < 0)
        return;

    fputs(f, pl.netname, ";", ftos(pl.frags), "\n");
    fclose(f);
};
```

### fexists
`float(string fname) fexists = #653;`

* **fname** — путь, который нужно проверить.

#### Описание и логика работы
`fexists` проверяет наличие файла именно в стандартном writable location, а не во всём виртуальном файловом дереве движка. На практике это полезно для проверки пользовательских данных в `data/`, но не подходит как общий тест существования ресурсов в pak/pk3 — для этого надёжнее `whichpack` или `search_begin`. Возвращается ненулевое значение, если файл найден в ожидаемом writable слое, и `0` во всех прочих случаях. Если ресурс есть только в архиве, `fexists` может вернуть `0`.

#### Практические сценарии использования
```qc
void() ensure_server_note =
{
    local filestream f;

    if (fexists("data/server-note.txt"))
        return;

    f = fopen("data/server-note.txt", FILE_WRITE);
    if (f >= 0)
    {
        fputs(f, "Created on ", ftos(time), "\n");
        fclose(f);
    }
};
```

### fcopy
`float(string src, string dst) fcopy = #650;`

* **src** — исходный путь внутри песочницы мода.
* **dst** — путь назначения; результат должен быть доступен для записи.

#### Описание и логика работы
`fcopy` копирует содержимое файла, не заставляя QuakeC вручную читать и писать блоки данных. Возвращает `0` при успехе и отрицательное значение при ошибке: типичные причины — отказ песочницы, отсутствие исходного файла или невозможность создать выходной файл. Чтение идёт из игрового VFS с учётом обычного sandbox/fallback поведения, а запись — только в writable часть gamedir. Для резервных копий пользовательских данных эта функция заметно проще связки `fopen`/`fread`/`fwrite`.

#### Практические сценарии использования
```qc
void() backup_settings =
{
    if (fcopy("data/settings.cfg", "data/settings.cfg.bak") != 0)
        bprint("backup failed\n");
};
```

### fremove
`float(string fname) fremove = #652;`

* **fname** — путь к файлу в writable части мода.

#### Описание и логика работы
`fremove` удаляет файл и возвращает `0` при успехе. Если путь не проходит песочницу, файл недоступен для записи, уже отсутствует или лежит только внутри pak/pk3, builtin возвращает отрицательное значение. Удаление касается writable слоя gamedir, поэтому ресурсов из архивов функция не касается. Для безопасности движок всё так же не позволяет выйти наружу через абсолютные пути или `..`.

#### Практические сценарии использования
```qc
void() reset_daily_log =
{
    if (fremove("data/daily.log") == 0)
        bprint("old log removed\n");
};
```

### frename
`float(string src, string dst) frename = #651;`

* **src** — текущий путь файла.
* **dst** — новый путь файла.

#### Описание и логика работы
`frename` пытается переименовать или переместить файл внутри writable области и возвращает `0` при успехе. Оба пути проходят ту же файловую песочницу, что и `fopen`, поэтому переезд наружу из gamedir заблокирован. При ошибке builtin возвращает отрицательное число — например, если источника нет, каталог назначения недоступен или файловая система только для чтения. Для атомарной схемы «записать во временное имя, затем переименовать» это основная штатная операция.

#### Практические сценарии использования
```qc
void() publish_new_config =
{
    local filestream f;

    f = fopen("data/config.new", FILE_WRITE);
    if (f < 0)
        return;

    fputs(f, "skill ", ftos(skill), "\n");
    fclose(f);

    if (frename("data/config.new", "data/config.cfg") != 0)
        bprint("rename failed\n");
};
```

### rmtree
`float(string path) rmtree = #654;`

* **path** — каталог, который предполагается удалить рекурсивно.

#### Описание и логика работы
По комментариям интерфейса `rmtree` задумывалась как опасная, но всё равно sandboxed операция для рекурсивного удаления дерева `data/`. Однако в текущей реализации движок лишь печатает сообщение о том, что функция не реализована, и не выполняет фактическое удаление. Поэтому использовать её как рабочий механизм очистки каталога нельзя. Если нужна совместимость, рассматривайте `rmtree` только как потенциальную future-proof точку расширения и обязательно закладывайте fallback через индивидуальные `fremove`.

#### Практические сценарии использования
```qc
void() try_purge_cache_tree =
{
    // В текущих сборках FTEQW это только диагностический вызов.
    if (rmtree("data/cache") != 0)
        bprint("rmtree is not implemented here\n");
};
```

### writetofile
`void(filestream fh, entity e) writetofile = #606;`

* **fh** — файловый хендл, открытый на запись.
* **e** — сущность, чьи поля нужно сериализовать.

#### Описание и логика работы
`writetofile` сохраняет поля одной сущности в текстовом формате, совместимом с `.ent`/savegame-представлением движка. Обычно это используется вместе с `loadfromfile` или `loadfromdata`, когда нужно быстро сериализовать произвольные entity state blocks без собственного формата. Запись идёт в уже открытый `filestream`, поэтому за режим файла и `fclose` отвечает вызывающий код. Если хендл неверен или сущность не может быть сериализована, полезного результата не будет.

#### Практические сценарии использования
```qc
void(entity victim) save_single_monster =
{
    local filestream f;

    f = fopen("data/monster_snapshot.ent", FILE_WRITE);
    if (f < 0)
        return;

    writetofile(f, victim);
    fclose(f);
};
```

### loadfromfile
`void(string s) loadfromfile = #530;`

* **s** — имя файла с entity-описаниями в формате `.ent` или savegame.

#### Описание и логика работы
`loadfromfile` читает файл с последовательностью entity blocks и вызывает штатный механизм `restoreent`, создавая или восстанавливая сущности по данным из файла. Это высокий уровень загрузки: QuakeC не получает поштучный контроль над разбором, зато быстро восстанавливает известный движку формат. Если файл не найден или открыть его не удалось, сущности не будут созданы. Используйте этот builtin только для данных, которые действительно соответствуют синтаксису entity lump/savegame.

#### Практические сценарии использования
```qc
void() restore_wave_from_disk =
{
    if (!fexists("data/wave1.ent"))
        return;

    loadfromfile("data/wave1.ent");
};
```

### loadfromdata
`void(string s) loadfromdata = #529;`

* **s** — текст с entity-описаниями в формате `.ent` или savegame.

#### Описание и логика работы
`loadfromdata` делает то же самое, что `loadfromfile`, но читает данные не с диска, а из уже готовой строки. Это удобно, когда entity blob приходит из сети, SQL, буфера строк или строится на лету скриптом. Строка должна быть синтаксически корректной; при пустой строке или плохом формате сущности не восстановятся полностью. Builtin не сообщает подробный журнал разбора, поэтому отладку удобнее делать на малых порциях текста.

#### Практические сценарии использования
```qc
void() spawn_bonus_box =
{
    local string entblob;

    entblob = "{\n\"classname\" \"item_health\"\n\"origin\" \"128 64 32\"\n}\n";
    loadfromdata(entblob);
};
```

### whichpack
`string(string filename, optional enumflags:float{WP_REFERENCEPACKAGE,WP_FULLPACKAGEPATH} flags) whichpack = #503;`

* **filename** — виртуальный путь к ресурсу, который нужно найти.
* **flags** — дополнительные флаги поиска и ссылки на пакет.

#### Описание и логика работы
`whichpack` сообщает, из какого pak/pk3 или другого пакета движок реально загрузил указанный ресурс. Если файл не найден, возвращается пустая строка. Флаг `WP_REFERENCEPACKAGE` может пометить найденный пакет как reference package для клиента, а `WP_FULLPACKAGEPATH` помогает убрать неоднозначность, когда одно и то же имя пакета существует в нескольких gamedir. Это диагностический builtin: он особенно полезен при конфликте одноимённых ресурсов в нескольких архивах.

#### Практические сценарии использования
```qc
void() print_model_source =
{
    local string pkg;

    pkg = whichpack("progs/player.mdl");
    if (pkg)
        bprint("player.mdl came from ", pkg, "\n");
};
```

### search_begin
`searchhandle(string pattern, enumflags:float{SB_CASEINSENSITIVE=1<<0,SB_FULLPACKAGEPATH=1<<1,SB_ALLOWDUPES=1<<2,SB_FORCESEARCH=1<<3,SB_MULTISEARCH=1<<4} flags, float quiet, optional string filterpackage) search_begin = #444;`

* **pattern** — шаблон поиска по именам файлов.
* **flags** — флаги поиска: нечувствительность к регистру, разрешение дублей, принудительный поиск в конкретном пакете и т. п.
* **quiet** — параметр совместимости; в FTEQW практически не влияет на вывод.
* **filterpackage** — необязательное ограничение конкретным пакетом/gamedir.

#### Описание и логика работы
`search_begin` запускает перечисление файлов и возвращает числовой `searchhandle`, который затем используется в `search_getsize`, `search_getfilename` и `search_end`. Пустой шаблон, абсолютный путь, `..`, backslash или двоеточие без `SB_MULTISEARCH` считаются ошибкой и дают `-1`. Если файлов просто не нашлось, хендл всё равно может быть корректным, а размер результата будет равен нулю. Для поиска нескольких одноимённых файлов из разных пакетов включайте `SB_ALLOWDUPES`.

#### Практические сценарии использования
```qc
void() scan_maps =
{
    local searchhandle h;
    local float i, count;

    h = search_begin("maps/*.bsp", 0, 1, "");
    if (h < 0)
        return;

    count = search_getsize(h);
    for (i = 0; i < count; i = i + 1)
        bprint(search_getfilename(h, i), "\n");

    search_end(h);
};
```

### search_end
`void(searchhandle handle) search_end = #445;`

* **handle** — поисковый хендл, полученный из `search_begin`.

#### Описание и логика работы
`search_end` освобождает результат файлового поиска. После вызова все индексы и имена, связанные с этим handle, становятся недействительными. Если забыть закрыть поиск, движок очистит его при завершении VM, но держать лишние перечисления открытыми не стоит. Повторное закрытие неверного хендла пользы не даёт.

#### Практические сценарии использования
```qc
void() count_cfg_files =
{
    local searchhandle h;

    h = search_begin("configs/*.cfg", 0, 1, "");
    if (h < 0)
        return;

    bprint("cfg files: ", ftos(search_getsize(h)), "\n");
    search_end(h);
};
```

### search_getsize
`float(searchhandle handle) search_getsize = #446;`

* **handle** — активный поисковый хендл.

#### Описание и логика работы
Функция возвращает число найденных элементов в результате `search_begin`. Для пустого результата это будет `0`, а для недействительного handle движок печатает предупреждение и тоже возвращает `0`. Значение остаётся стабильным до `search_end`, если вы не открываете новый поиск поверх того же handle. Часто именно с `search_getsize` начинают цикл обхода результатов.

#### Практические сценарии использования
```qc
void() announce_available_shaders =
{
    local searchhandle h;
    local float count;

    h = search_begin("scripts/*.shader", 0, 1, "");
    if (h < 0)
        return;

    count = search_getsize(h);
    bprint("shader scripts found: ", ftos(count), "\n");
    search_end(h);
};
```

### search_getfilename
`string(searchhandle handle, float num) search_getfilename = #447;`

* **handle** — поисковый хендл.
* **num** — индекс результата от `0` до `search_getsize(handle) - 1`.

#### Описание и логика работы
`search_getfilename` возвращает имя файла по индексу внутри результата поиска. Если индекс вышел за диапазон или handle недействителен, builtin отдаёт null string. Возвращаемое имя — виртуальный путь внутри VFS, а не native filesystem path. При `SB_ALLOWDUPES` одно и то же имя может встретиться несколько раз из разных пакетов.

#### Практические сценарии использования
```qc
void() list_hud_configs =
{
    local searchhandle h;
    local float i;

    h = search_begin("huds/*.cfg", 0, 1, "");
    if (h < 0)
        return;

    for (i = 0; i < search_getsize(h); i = i + 1)
        bprint("hud cfg: ", search_getfilename(h, i), "\n");

    search_end(h);
};
```

### buf_create
`strbuf() buf_create = #460;`

* Аргументов нет.

#### Описание и логика работы
`buf_create` создаёт пустой строковый буфер и возвращает его числовой `strbuf`-хендл. Концептуально `buf_*` — это список строк, живущий внутри VM и управляемый вручную: его нужно удалять через `buf_del`, когда он больше не нужен. Если память закончилась, функция возвращает отрицательное значение. Хендл действителен только в той QuakeC VM, где был создан.

#### Практические сценарии использования
```qc
void() init_recent_maps_buffer =
{
    local strbuf b;

    b = buf_create();
    if (b < 0)
        return;

    bufstr_add(b, "start", 1);
    buf_del(b);
};
```

### buf_del
`void(strbuf bufhandle) buf_del = #461;`

* **bufhandle** — строковый буфер, созданный ранее через `buf_create`.

#### Описание и логика работы
`buf_del` освобождает сам буфер и все строки внутри него. После вызова использовать старый handle нельзя: остальные `buf_*`/`bufstr_*` вызовы с ним просто ничего полезного не сделают. Если handle неверен или принадлежит другой VM, builtin тихо игнорирует запрос. Это базовая операция завершения жизненного цикла буфера.

#### Практические сценарии использования
```qc
void() build_and_release_list =
{
    local strbuf b;

    b = buf_create();
    if (b < 0)
        return;

    bufstr_add(b, "alpha", 1);
    bufstr_add(b, "beta", 1);
    buf_del(b);
};
```

### buf_getsize
`float(strbuf bufhandle) buf_getsize = #462;`

* **bufhandle** — строковый буфер.

#### Описание и логика работы
`buf_getsize` возвращает текущую «длину» буфера — индекс последнего используемого элемента плюс один. Важно, что после `bufstr_free` внутри массива могут оставаться пустые ячейки, и `buf_getsize` их не скрывает. Для неверного handle возвращается `0`. Если нужно перебирать только существующие строки, в цикле дополнительно проверяйте результат `bufstr_get`.

#### Практические сценарии использования
```qc
void() print_buffer_size =
{
    local strbuf b;

    b = buf_create();
    if (b < 0)
        return;

    bufstr_add(b, "dm2", 1);
    bufstr_add(b, "dm4", 1);
    bprint("entries: ", ftos(buf_getsize(b)), "\n");
    buf_del(b);
};
```

### buf_copy
`void(strbuf bufhandle_from, strbuf bufhandle_to) buf_copy = #463;`

* **bufhandle_from** — буфер-источник.
* **bufhandle_to** — буфер-получатель.

#### Описание и логика работы
`buf_copy` полностью очищает буфер назначения и затем копирует в него все строки из источника. Это полноценная копия строк, а не перенос ссылок, поэтому после копирования оба буфера живут независимо. Если любой handle неверен, принадлежит другой VM или оба handle одинаковые, функция просто прекращает работу. Для подготовки snapshot-версии списка это удобнее, чем пересоздавать элементы вручную.

#### Практические сценарии использования
```qc
void() duplicate_vote_list =
{
    local strbuf a, b;

    a = buf_create();
    b = buf_create();
    if (a < 0 || b < 0)
        return;

    bufstr_add(a, "dm3", 1);
    bufstr_add(a, "e1m2", 1);
    buf_copy(a, b);

    bprint("copy[0]=", bufstr_get(b, 0), "\n");
    buf_del(a);
    buf_del(b);
};
```

### buf_loadfile
`float(string filename, strbuf bufhandle) buf_loadfile = #535;`

* **filename** — путь к текстовому файлу.
* **bufhandle** — уже созданный строковый буфер.

#### Описание и логика работы
`buf_loadfile` открывает файл через ту же файловую песочницу, что и `fopen`, читает его построчно и добавляет строки в конец существующего буфера. Возвращается `1`, если файл удалось прочитать, и `0` при отказе доступа, отсутствии файла или неверном handle. Буфер перед загрузкой не очищается автоматически — содержимое именно дополняется. Это удобно для объединения нескольких списков из разных файлов.

#### Практические сценарии использования
```qc
void() load_map_rotation =
{
    local strbuf b;

    b = buf_create();
    if (b < 0)
        return;

    if (buf_loadfile("data/mapcycle.txt", b))
        bprint("first map: ", bufstr_get(b, 0), "\n");

    buf_del(b);
};
```

### buf_writefile
`float(filestream filehandle, strbuf bufhandle, optional float startpos, optional float numstrings) buf_writefile = #536;`

* **filehandle** — файловый хендл, открытый на запись.
* **bufhandle** — буфер строк.
* **startpos** — индекс первой строки для записи.
* **numstrings** — сколько строк писать, начиная с `startpos`.

#### Описание и логика работы
`buf_writefile` выгружает строки буфера в уже открытый файл, автоматически добавляя `\n` после каждой непустой записи. Возвращает `1` при успешном проходе по буферу и `0`, если handle файла/буфера неверен. Параметры `startpos` и `numstrings` позволяют сохранить только поддиапазон списка — например, одну страницу таблицы или «хвост» лога. Это особенно полезно вместе с `FILE_WRITE`/`FILE_APPEND` и промежуточной обработкой через `buf_sort`.

#### Практические сценарии использования
```qc
void() write_top_three =
{
    local strbuf b;
    local filestream f;

    b = buf_create();
    if (b < 0)
        return;

    bufstr_add(b, "dm2", 1);
    bufstr_add(b, "dm4", 1);
    bufstr_add(b, "e1m2", 1);

    f = fopen("data/topmaps.txt", FILE_WRITE);
    if (f >= 0)
    {
        buf_writefile(f, b, 0, 3);
        fclose(f);
    }

    buf_del(b);
};
```

### buf_sort
`void(strbuf bufhandle, float sortprefixlen, float backward) buf_sort = #464;`

* **bufhandle** — буфер строк.
* **sortprefixlen** — сколько первых символов сравнивать; `0` или меньше означает «всю строку».
* **backward** — `0` для сортировки A→Z, неноль для Z→A.

#### Описание и логика работы
`buf_sort` сортирует строки внутри буфера и перед этим удаляет из активной части массива все `NULL`-дыры, оставшиеся после `bufstr_free`. Если `sortprefixlen > 0`, сравнение идёт только по префиксу, что удобно для строк формата `"map score"`, когда нужно сортировать по имени карты фиксированной длины. Пустые/освобождённые элементы после сортировки больше не учитываются в `buf_getsize`. Неверный handle просто игнорируется.

#### Практические сценарии использования
```qc
void() sort_recent_maps =
{
    local strbuf b;

    b = buf_create();
    if (b < 0)
        return;

    bufstr_add(b, "ztn3dm1", 1);
    bufstr_add(b, "aerowalk", 1);
    bufstr_add(b, "bloodrun", 1);
    buf_sort(b, 0, 0);

    bprint("sorted first: ", bufstr_get(b, 0), "\n");
    buf_del(b);
};
```

### buf_implode
`string(strbuf bufhandle, string glue) buf_implode = #465;`

* **bufhandle** — буфер строк.
* **glue** — разделитель между строками.

#### Описание и логика работы
`buf_implode` склеивает все непустые строки буфера в одну temp string, вставляя `glue` между соседними элементами. `NULL`-ячейки пропускаются, поэтому освобождённые через `bufstr_free` слоты не дают лишних разделителей. Для неверного handle обычно возвращается пустой результат. Это простой способ получить CSV-строку, список через запятую или готовый текст для `fputs`.

#### Практические сценарии использования
```qc
void() print_map_vote_line =
{
    local strbuf b;
    local string line;

    b = buf_create();
    if (b < 0)
        return;

    bufstr_add(b, "dm2", 1);
    bufstr_add(b, "dm4", 1);
    bufstr_add(b, "e1m2", 1);
    line = buf_implode(b, ", ");
    bprint("choices: ", line, "\n");
    buf_del(b);
};
```

### buf_cvarlist
`void(strbuf strbuf, string pattern, string antipattern) buf_cvarlist = #517;`

* **strbuf** — буфер, который будет заполнен именами cvar-ов.
* **pattern** — шаблон включения; поддерживает префиксное сравнение и wildcard-символы `*`/`?`.
* **antipattern** — шаблон исключения с теми же правилами.

#### Описание и логика работы
`buf_cvarlist` полностью очищает целевой буфер и заполняет его именами cvar-ов, прошедших фильтрацию. Если в шаблонах нет wildcard-символов, движок использует быстрые префиксные проверки; при наличии `*` или `?` — полноценное wildcard-сопоставление. Итоговый список автоматически сортируется по возрастанию. Для консольных меню, автодополнения и отладочных списков это самый удобный builtin.

#### Практические сценарии использования
```qc
void() list_server_cvars =
{
    local strbuf b;
    local float i;

    b = buf_create();
    if (b < 0)
        return;

    buf_cvarlist(b, "sv_*", "sv_*password*");
    for (i = 0; i < buf_getsize(b); i = i + 1)
        bprint(bufstr_get(b, i), "\n");

    buf_del(b);
};
```

### bufstr_add
`float(strbuf bufhandle, string str, float ordered) bufstr_add = #468;`

* **bufhandle** — буфер строк.
* **str** — строка, которую нужно добавить.
* **ordered** — при ненулевом значении строка всегда дописывается в конец; при `0` может занять первую свободную дыру.

#### Описание и логика работы
`bufstr_add` добавляет строку в буфер и возвращает индекс, куда она попала. При `ordered = 0` builtin переиспользует ранее освобождённые слоты после `bufstr_free`, что удобно для sparse-таблиц; при `ordered != 0` всегда растит хвост списка. Если handle неверен, полезного индекса вы не получите. Поскольку строки копируются внутрь буфера, temp string после вызова можно не сохранять отдельно.

#### Практические сценарии использования
```qc
void() add_vote_entry =
{
    local strbuf b;
    local float idx;

    b = buf_create();
    if (b < 0)
        return;

    idx = bufstr_add(b, "aerowalk", 1);
    bprint("stored at index ", ftos(idx), "\n");
    buf_del(b);
};
```

### bufstr_free
`void(strbuf bufhandle, float string_index) bufstr_free = #469;`

* **bufhandle** — буфер строк.
* **string_index** — индекс строки, которую нужно удалить.

#### Описание и логика работы
`bufstr_free` удаляет конкретную строку и превращает слот в пустую дыру, но не сдвигает остальные элементы. Поэтому `buf_getsize` не уменьшается автоматически, а `bufstr_add(..., 0)` позже может переиспользовать этот индекс. Если индекс вне диапазона или handle неверен, builtin молча ничего не делает. Такой режим особенно полезен для списков с устойчивыми индексами.

#### Практические сценарии использования
```qc
void() clear_second_rotation_entry =
{
    local strbuf b;

    b = buf_create();
    if (b < 0)
        return;

    bufstr_add(b, "dm2", 1);
    bufstr_add(b, "dm4", 1);
    bufstr_free(b, 1);
    buf_del(b);
};
```

### bufstr_get
`string(strbuf bufhandle, float string_index) bufstr_get = #466;`

* **bufhandle** — буфер строк.
* **string_index** — индекс нужной записи.

#### Описание и логика работы
`bufstr_get` возвращает строку по индексу или null string, если индекс пустой, вышел за диапазон или handle неверен. Это означает, что при обходе буфера нужно различать «слот существует и содержит пустую строку» и «слот отсутствует» по контексту заполнения списка. Возвращаемое значение — temp string, подходящая для немедленного использования в `bprint`, `fputs`, сравнении и т. д. Для sparse-буферов всегда проверяйте `if (!value)`.

#### Практические сценарии использования
```qc
void() show_first_map =
{
    local strbuf b;

    b = buf_create();
    if (b < 0)
        return;

    bufstr_add(b, "start", 1);
    bprint("first map: ", bufstr_get(b, 0), "\n");
    buf_del(b);
};
```

### bufstr_set
`void(strbuf bufhandle, float string_index, string str) bufstr_set = #467;`

* **bufhandle** — буфер строк.
* **string_index** — индекс для записи.
* **str** — новое содержимое строки.

#### Описание и логика работы
`bufstr_set` записывает строку по конкретному индексу, при необходимости расширяя внутренний массив и заполняя промежуточные элементы пустыми слотами. Это делает builtin удобным для таблиц с адресацией по номеру строки, а не только для append-списков. Слишком большой индекс движок считает подозрительным и отвергает. Если в ячейке уже была строка, она корректно освобождается и заменяется новой.

#### Практические сценарии использования
```qc
void() patch_rotation_slot =
{
    local strbuf b;

    b = buf_create();
    if (b < 0)
        return;

    bufstr_set(b, 3, "end");
    bprint("slot 3 = ", bufstr_get(b, 3), "\n");
    buf_del(b);
};
```

### bufstr_find
`float(float bufhandle, string match, float matchrule, float startpos, float step) bufstr_find = #537;`

* **bufhandle** — буфер строк.
* **match** — искомый текст или шаблон.
* **matchrule** — правило поиска: `0`/`5` wildcard pattern, `1` exact, `2` prefix, `3` suffix, `4` substring.
* **startpos** — индекс, с которого начинается поиск.
* **step** — шаг обхода, обычно `1`.

#### Описание и логика работы
`bufstr_find` ищет первое совпадение и возвращает его индекс либо `-1`, если ничего не найдено. Функция пропускает `NULL`-слоты, поэтому после `bufstr_free` они не мешают поиску. Неверный handle, отрицательный `startpos` или `step <= 0` также приводят к `-1`. Это основной инструмент, когда буфер используется как небольшой in-memory индекс строк.

#### Практические сценарии использования
```qc
void() find_vote_by_prefix =
{
    local strbuf b;
    local float idx;

    b = buf_create();
    if (b < 0)
        return;

    bufstr_add(b, "dm2", 1);
    bufstr_add(b, "dm4", 1);
    bufstr_add(b, "e1m2", 1);
    idx = bufstr_find(b, "dm", 2, 0, 1);

    bprint("first dm entry index: ", ftos(idx), "\n");
    buf_del(b);
};
```

### hash_createtab
`hashtable(float tabsize, optional float defaulttype) hash_createtab = #287;`

* **tabsize** — ожидаемое число элементов; это подсказка для числа bucket-ов.
* **defaulttype** — тип значений по умолчанию, обычно один из `EV_*`.

#### Описание и логика работы
`hash_createtab` создаёт хеш-таблицу key-value и возвращает её handle. `tabsize` влияет на производительность, а не на жёсткий лимит: слишком маленькая таблица будет работать, но с большим числом сравнений строк. Если `defaulttype` не задан, движок использует векторный вариант хранения; для строк почти всегда лучше явно выбирать `EV_STRING`. Отдельно существует специальная таблица `gamestate`, которая переживает смену карты и не создаётся этим builtin.

#### Практические сценарии использования
```qc
void() create_runtime_cache =
{
    local hashtable tab;

    tab = hash_createtab(128, EV_STRING);
    if (!tab)
        return;

    hash_add(tab, "motd", "Welcome", EV_STRING | HASH_REPLACE);
    hash_destroytab(tab);
};
```

### hash_destroytab
`void(hashtable table) hash_destroytab = #288;`

* **table** — таблица, созданная через `hash_createtab`.

#### Описание и логика работы
`hash_destroytab` удаляет таблицу и все пары ключ-значение внутри неё. Вызывать её нужно только для обычных таблиц, созданных QuakeC; специальный `gamestate` уничтожать не нужно и не следует. После удаления любой `hash_*` доступ через старый handle недействителен. Если таблица принадлежит другой VM, движок её не тронет.

#### Практические сценарии использования
```qc
void() dispose_round_cache =
{
    local hashtable tab;

    tab = hash_createtab(16, EV_FLOAT);
    if (!tab)
        return;

    hash_add(tab, "wave", 3, EV_FLOAT | HASH_REPLACE);
    hash_destroytab(tab);
};
```

### hash_add
`void(hashtable table, string name, __variant value, optional float typeandflags) hash_add = #289;`

* **table** — таблица или специальный `gamestate`.
* **name** — ключ.
* **value** — сохраняемое значение.
* **typeandflags** — комбинация типа `EV_*` и флагов `HASH_REPLACE`/`HASH_ADD`.

#### Описание и логика работы
`hash_add` вставляет ключ и значение в таблицу. Без специальных флагов новое значение заменяет старое; `HASH_REPLACE` заставляет сперва удалить существующую запись, а `HASH_ADD` разрешает хранить несколько значений под одним ключом и получать их через параметр `index` в `hash_get`. Тип данных важен не только для чтения, но и для сохранений: строки надо явно помечать как `EV_STRING`, иначе можно потерять tempstring или получить некорректную сериализацию. Пустой ключ движок не принимает.

#### Практические сценарии использования
```qc
void() remember_player_note =
{
    hash_add(gamestate, "last_winner", self.netname, EV_STRING | HASH_REPLACE);
    hash_add(gamestate, "recent_map", mapname, EV_STRING | HASH_ADD);
};
```

### hash_delete
`__variant(hashtable table, string name) hash_delete = #291;`

* **table** — таблица, из которой нужно удалить ключ.
* **name** — имя ключа.

#### Описание и логика работы
`hash_delete` удаляет запись и возвращает её значение как `__variant`. Если ключа нет, builtin отдаёт нулевое значение соответствующего варианта. При наличии дубликатов с одинаковым ключом удаляется одна запись, которую движок нашёл первой. Для строк безопаснее сразу присваивать результат строковой переменной, чтобы не потерять tempstring в дальнейшем коде.

#### Практические сценарии использования
```qc
void() consume_saved_message =
{
    local string msg;

    msg = hash_delete(gamestate, "pending_broadcast");
    if (msg)
        bprint(msg, "\n");
};
```

### hash_get
`__variant(hashtable table, string name, optional __variant deflt, optional float requiretype, optional float index) hash_get = #290;`

* **table** — таблица, в которой выполняется поиск.
* **name** — ключ.
* **deflt** — значение по умолчанию, если ключ не найден.
* **requiretype** — фильтр по типу `EV_*`.
* **index** — номер дубликата для ключей, добавленных через `HASH_ADD`.

#### Описание и логика работы
`hash_get` ищет значение по имени ключа и возвращает либо найденный `__variant`, либо `deflt`. Параметр `requiretype` полезен, когда под одним и тем же ключом хранятся значения разных типов, а `index` позволяет пройти по нескольким записям с одним именем. Для таблицы `gamestate` это удобный способ держать межкарточное состояние без собственного формата файлов. Обычно результат сразу приводят типом принимающей переменной: `string`, `float`, `vector` и т. п.

#### Практические сценарии использования
```qc
void() show_saved_winner =
{
    local string winner;

    winner = hash_get(gamestate, "last_winner", "nobody", EV_STRING, 0);
    bprint("last winner: ", winner, "\n");
};
```

### hash_getkey
`string(hashtable table, float idx) hash_getkey = #292;`

* **table** — таблица, ключи которой нужно перебирать.
* **idx** — индекс ключа в текущем внутреннем порядке.

#### Описание и логика работы
`hash_getkey` возвращает некоторое имя ключа по индексу, но этот порядок не считается стабильным. Добавление и удаление записей могут менять распределение по bucket-ам, поэтому нельзя безопасно удалять элементы, просто увеличивая `idx` вслепую. Если индекс вне диапазона, функция возвращает null string. Для отладочного перечисления или экспорта содержимого таблицы builtin подходит отлично, а для детерминированного порядка — нет.

#### Практические сценарии использования
```qc
void(hashtable tab) dump_hash_keys =
{
    local float i;
    local string key;

    for (i = 0; ; i = i + 1)
    {
        key = hash_getkey(tab, i);
        if (!key)
            break;
        bprint("key: ", key, "\n");
    }
};
```

### memalloc
`__variant*(int size) memalloc = #384;`

* **size** — размер блока в байтах.

#### Описание и логика работы
`memalloc` выделяет адресуемый блок памяти внутри модели памяти QC и возвращает указатель. Блок сразу обнуляется; запрос размера `0` всё равно даёт минимально освобождаемый блок, а слишком большие или отрицательные размеры отвергаются. Это низкоуровневый интерфейс: движок не знает вашего формата данных и не защищает от логических ошибок. Неправильное использование указателя, смещения или размера легко заканчивается повреждением данных VM, предупреждениями `PF_mem*` и нестабильной работой скрипта.

#### Практические сценарии использования
```qc
void() allocate_four_cells =
{
    local __variant *ptr;

    ptr = memalloc(16);
    if (!ptr)
        return;

    memsetval(ptr, 0, 10);
    memsetval(ptr, 4, 20);
    memsetval(ptr, 8, 30);
    memsetval(ptr, 12, 40);
    memfree(ptr);
};
```

### memfree
`void(__variant *ptr) memfree = #385;`

* **ptr** — указатель, ранее возвращённый `memalloc`.

#### Описание и логика работы
`memfree` освобождает блок адресуемой памяти. Освобождать нужно только то, что действительно было выделено соответствующим механизмом движка; double free или работа со случайным указателем создают крайне опасную ситуацию для VM. Нулевой указатель builtin просто игнорирует. После освобождения обязательно зануляйте собственную переменную, чтобы не использовать висячий адрес дальше.

#### Практические сценарии использования
```qc
void() release_temp_block =
{
    local __variant *ptr;

    ptr = memalloc(32);
    if (!ptr)
        return;

    memfill8(ptr, 0, 32);
    memfree(ptr);
    ptr = __NULL__;
};
```

### memcpy
`void(__variant *dst, __variant *src, int size) memcpy = #386;`

* **dst** — адрес назначения.
* **src** — адрес источника.
* **size** — число байтов для копирования.

#### Описание и логика работы
`memcpy` копирует байты между двумя адресуемыми областями памяти; движок использует безопасное поведение уровня `memmove`, поэтому перекрывающиеся диапазоны не страшны. Однако builtin всё равно требует корректные указатели и допустимый размер, иначе выдаёт ошибку VM. Это инструмент для двоичных структур, больших массивов или ручного сериализатора, а не для обычных строк QuakeC. Слишком доверять ему нельзя: одна ошибка в размере может затронуть соседние данные.

#### Практические сценарии использования
```qc
void() clone_two_numbers =
{
    local __variant *src, *dst;

    src = memalloc(8);
    dst = memalloc(8);
    if (!src || !dst)
        return;

    memsetval(src, 0, 111);
    memsetval(src, 4, 222);
    memcpy(dst, src, 8);
    bprint(ftos(memgetval(dst, 4)), "\n");

    memfree(src);
    memfree(dst);
};
```

### memfill8
`void(__variant *dst, int val, int size) memfill8 = #387;`

* **dst** — адрес начала блока.
* **val** — байтовое значение, которым заполняется память.
* **size** — сколько байтов заполнить.

#### Описание и логика работы
`memfill8` записывает один и тот же байт на всём диапазоне. Чаще всего эту функцию используют для обнуления памяти (`val = 0`), потому что для произвольных шаблонов по байтам легко получить значения, которые не имеют осмысленного представления как `float` или `string`. Как и остальные `mem*`, builtin требует корректный указатель и размер. Ошибка в границах здесь столь же опасна, как и в `memcpy`.

#### Практические сценарии использования
```qc
void() clear_packet_buffer =
{
    local __variant *ptr;

    ptr = memalloc(64);
    if (!ptr)
        return;

    memfill8(ptr, 0, 64);
    memfree(ptr);
};
```

### memgetval
`__variant(__variant *dst, float ofs) memgetval = #388;`

* **dst** — базовый указатель.
* **ofs** — смещение относительно базы.

#### Описание и логика работы
`memgetval` читает 32-битное значение по адресу `dst + ofs` и возвращает его как `__variant`. Практически всегда разумно работать со смещениями, кратными 4 байтам, и заранее знать, как вы сами разложили данные в блоке памяти. Эта функция не занимается проверкой семантики содержимого: если там лежит «битый» float или число, которое вы потом интерпретируете как строку, ошибка будет на стороне QuakeC-кода. Используйте builtin только в парах с предсказуемым `memsetval`/`memcpy`.

#### Практические сценарии использования
```qc
void() read_second_cell =
{
    local __variant *ptr;
    local float value;

    ptr = memalloc(8);
    if (!ptr)
        return;

    memsetval(ptr, 0, 7);
    memsetval(ptr, 4, 99);
    value = memgetval(ptr, 4);
    bprint("cell[1]=", ftos(value), "\n");
    memfree(ptr);
};
```

### memsetval
`void(__variant *dst, float ofs, __variant val) memsetval = #389;`

* **dst** — базовый указатель.
* **ofs** — смещение до 32-битной ячейки.
* **val** — записываемое значение.

#### Описание и логика работы
`memsetval` записывает одно 32-битное значение по смещению от указателя. Это удобнее, чем `memfill8`, когда вы строите массив из целых или float-ячеек вручную. Но здесь особенно важно поддерживать собственную схему выравнивания: если смешать разные типы без дисциплины, последующее чтение через `memgetval` станет неочевидным. Неверный адрес или диапазон приводят к ошибкам VM.

#### Практические сценарии использования
```qc
void() store_health_snapshot =
{
    local __variant *ptr;

    ptr = memalloc(12);
    if (!ptr)
        return;

    memsetval(ptr, 0, self.health);
    memsetval(ptr, 4, self.armorvalue);
    memsetval(ptr, 8, self.currentammo);
    memfree(ptr);
};
```

### memptradd
`__variant*(__variant *base, float ofs) memptradd = #390;`

* **base** — базовый указатель.
* **ofs** — смещение от базы.

#### Описание и логика работы
`memptradd` выполняет арифметику указателей и возвращает новый адрес `base + ofs`. Смещение должно быть целым и 32-битно выровненным; движок отдельно ругается на нецелые значения, оффсеты не кратные четырём и попытки сместить специальные указатели. Это мощная, но очень опасная операция: один неправильный шаг переводит дальнейшие `memsetval`/`memgetval` на чужую память. Используйте её только там, где действительно нужна pointer math, а не обычная адресация через смещение параметром.

#### Практические сценарии использования
```qc
void() write_third_cell_via_pointer =
{
    local __variant *base, *cell;

    base = memalloc(12);
    if (!base)
        return;

    cell = memptradd(base, 8);
    memsetval(cell, 0, 1234);
    bprint(ftos(memgetval(base, 8)), "\n");
    memfree(base);
};
```

### sqlconnect
`float(optional string host, optional string user, optional string pass, optional string defaultdb, optional string driver) sqlconnect = #250;`

* **host** — адрес сервера БД или пустая строка для значения из cvar/драйвера.
* **user** — имя пользователя БД.
* **pass** — пароль пользователя БД.
* **defaultdb** — база по умолчанию; для SQLite это имя файла базы.
* **driver** — `sqlite`, `mysql` или пустая строка для выбора по умолчанию.

#### Описание и логика работы
`sqlconnect` открывает соединение с доступным SQL-драйвером и возвращает `serveridx` либо `-1`, если драйвер недоступен или подключение не удалось. Если аргументы пустые, движок берёт значения из `sv_sql_host`, `sv_sql_username`, `sv_sql_password`, `sv_sql_defaultdb` и `sv_sql_driver`. Для SQLite имя базы, кроме `:memory:`, sandbox-ится в путь вида `data/sqlite/<имя>.db`, так что QC не получает произвольный доступ к файловой системе. Ошибки конкретного backend-а затем читаются через `sqlerror`.

#### Практические сценарии использования
```qc
float g_sql_server;

void() open_stats_db =
{
    g_sql_server = sqlconnect("", "", "", "aom_stats", "sqlite");
    if (g_sql_server < 0)
        bprint("sqlconnect failed\n");
};
```

### sqldisconnect
`void(float serveridx) sqldisconnect = #251;`

* **serveridx** — идентификатор соединения, полученный из `sqlconnect`.

#### Описание и логика работы
`sqldisconnect` закрывает SQL-соединение и просит рабочий поток прекратить обработку запросов. Если индекс недействителен или соединение уже закрыто, полезного действия не произойдёт. Закрывать соединение нужно тогда, когда мод точно закончил пользоваться БД и освободил persistent query results. Не держите бесполезные соединения открытыми между сценариями, если они не нужны постоянно.

#### Практические сценарии использования
```qc
void() close_stats_db =
{
    if (g_sql_server >= 0)
    {
        sqldisconnect(g_sql_server);
        g_sql_server = -1;
    }
};
```

### sqlopenquery
`float(float serveridx, void(float serveridx, float queryidx, float rows, float columns, float eof, float firstrow) callback, float querytype, string query) sqlopenquery = #252;`

* **serveridx** — соединение с БД.
* **callback** — QuakeC-функция, которую движок вызовет по готовности данных.
* **querytype** — режим жизни запроса; на практике `0` подходит для одноразового результата, ненулевое значение делает результат persistent, а бит `2` связывают с ожиданием QC-thread.
* **query** — текст SQL-запроса.

#### Описание и логика работы
`sqlopenquery` отправляет SQL-запрос асинхронно и возвращает `queryidx` либо `-1`, если запрос даже не удалось поставить в очередь. Когда данные готовы, движок вызывает callback с количеством строк в текущем фрагменте, числом столбцов, флагом конца потока и индексом первой строки чанка; это позволяет обрабатывать большие результаты порциями. Для persistent-запросов вызывающий код должен позже освободить результат через `sqlclosequery`. Если соединение отсутствует, builtin не выполняет INSERT/SELECT и просто возвращает ошибку.

#### Практические сценарии использования
```qc
void(float serveridx, float queryidx, float rows, float columns, float eof, float firstrow) on_top_players =
{
    local float i;

    for (i = 0; i < rows; i = i + 1)
        bprint(sqlreadfield(serveridx, queryidx, firstrow + i, 0), "\n");

    if (eof)
        sqlclosequery(serveridx, queryidx);
};

void() request_top_players =
{
    sqlopenquery(g_sql_server, on_top_players, 1,
        "SELECT name FROM players ORDER BY frags DESC LIMIT 10");
};
```

### sqlclosequery
`void(float serveridx, float queryidx) sqlclosequery = #253;`

* **serveridx** — соединение с БД.
* **queryidx** — номер запроса, ранее возвращённый `sqlopenquery`.

#### Описание и логика работы
`sqlclosequery` освобождает результаты persistent-запроса и снимает внутренние структуры движка. Для одноразовых запросов движок может очистить данные сам, но persistent queries следует закрывать явно после чтения всех нужных строк. Попытка закрыть неверный `queryidx` приводит лишь к предупреждению в консоли. Если забыть эту функцию в циклическом игровом сценарии, можно накапливать лишние результаты в памяти.

#### Практические сценарии использования
```qc
void(float serveridx, float queryidx, float rows, float columns, float eof, float firstrow) on_count_ready =
{
    if (eof)
    {
        bprint("rows chunk done\n");
        sqlclosequery(serveridx, queryidx);
    }
};
```

### sqlreadfield
`string(float serveridx, float queryidx, float row, float column) sqlreadfield = #254;`

* **serveridx** — соединение с БД.
* **queryidx** — идентификатор запроса.
* **row** — номер строки внутри результата.
* **column** — номер столбца.

#### Описание и логика работы
`sqlreadfield` читает ячейку результата как строку. Для обычных строк `row` задаётся от `0` вверх; при invalid row/column, пустой ячейке SQL `NULL` или неверном запросе функция возвращает пустой результат. Внутренний API движка также поддерживает чтение имён столбцов через отрицательный `row`, но в прикладном QuakeC чаще всего используют только положительные индексы. Если запрос не persistent и callback уже отработал, не откладывайте чтение на потом.

#### Практические сценарии использования
```qc
void(float serveridx, float queryidx, float rows, float columns, float eof, float firstrow) on_read_names =
{
    local float i;

    for (i = 0; i < rows; i = i + 1)
        bprint("name=", sqlreadfield(serveridx, queryidx, firstrow + i, 0), "\n");

    if (eof)
        sqlclosequery(serveridx, queryidx);
};
```

### sqlreadfloat
`float(float serveridx, float queryidx, float row, float column) sqlreadfloat = #258;`

* **serveridx** — соединение с БД.
* **queryidx** — идентификатор запроса.
* **row** — номер строки результата.
* **column** — номер столбца.

#### Описание и логика работы
`sqlreadfloat` делает то же, что `sqlreadfield`, но сразу конвертирует содержимое ячейки в число через движковый `atof`. Если строка пуста, поле содержит нечисловой текст или доступ к запросу невозможен, builtin возвращает `0`. Это удобно для счетчиков, рейтингов, времени и прочих numeric columns, которые не хочется каждый раз преобразовывать вручную. Ошибка соединения сама по себе не генерирует исключение — вы просто получите нулевой результат и должны проверить `sqlerror`, если контекст важен.

#### Практические сценарии использования
```qc
void(float serveridx, float queryidx, float rows, float columns, float eof, float firstrow) on_read_scores =
{
    local float frags;

    if (rows > 0)
    {
        frags = sqlreadfloat(serveridx, queryidx, firstrow, 1);
        bprint("top frags=", ftos(frags), "\n");
    }

    if (eof)
        sqlclosequery(serveridx, queryidx);
};
```

### sqlerror
`string(float serveridx, optional float queryidx) sqlerror = #255;`

* **serveridx** — соединение с БД.
* **queryidx** — необязательный номер конкретного запроса.

#### Описание и логика работы
`sqlerror` возвращает текст последней ошибки SQL-подсистемы. Без `queryidx` обычно читается ошибка уровня соединения/driver-а, а с идентификатором запроса — ошибка конкретного SELECT/INSERT, если она была зафиксирована. Для невалидного индекса или отсутствующей ошибки функция возвращает пустую строку. Это главный builtin для диагностики синтаксических ошибок SQL, отсутствующих таблиц и проблем с backend-ом.

#### Практические сценарии использования
```qc
void(float serveridx, float queryidx, float rows, float columns, float eof, float firstrow) on_bad_query =
{
    local string err;

    err = sqlerror(serveridx, queryidx);
    if (err)
        bprint("sql error: ", err, "\n");

    if (eof)
        sqlclosequery(serveridx, queryidx);
};
```

### sqlescape
`string(float serveridx, string data) sqlescape = #256;`

* **serveridx** — соединение с БД, которое определяет правила escaping.
* **data** — пользовательская строка, которую нужно безопасно встроить в SQL.

#### Описание и логика работы
`sqlescape` подготавливает строку для безопасной вставки внутрь SQL-литерала. Для SQLite движок удваивает одиночные кавычки, для MySQL использует native escape API; если соединение отсутствует, builtin возвращает пустую строку. Эта функция не добавляет внешние `'...'` автоматически — их дописывает сам запрос. Любой пользовательский ввод, имя игрока или текст чата стоит пропускать через `sqlescape`, иначе запрос легко сломать кавычками.

#### Практические сценарии использования
```qc
void() save_player_name =
{
    local string safe_name;

    safe_name = sqlescape(g_sql_server, self.netname);
    sqlopenquery(g_sql_server, on_top_players, 0,
        strcat("INSERT INTO players(name) VALUES ('", safe_name, "')"));
};
```

### sqlversion
`string(float serveridx) sqlversion = #257;`

* **serveridx** — соединение с БД.

#### Описание и логика работы
`sqlversion` возвращает строку вида `sqlite: ...` или `mysql: ...` с версией активного драйвера/клиентской библиотеки. Если индекс соединения неверен, результатом будет пустая строка. Эта функция особенно полезна в диагностике серверной сборки, когда мод хочет понять, на каком backend-е реально работает. На логику запросов она не влияет, но помогает печатать понятные служебные сообщения.

#### Практические сценарии использования
```qc
void() print_sql_backend =
{
    local string v;

    v = sqlversion(g_sql_server);
    if (v)
        bprint("SQL backend: ", v, "\n");
};
```

### digest_hex
`string(string digest, string data, ...) digest_hex = #639;`

* **digest** — имя алгоритма хеширования, например `MD5`, `SHA1`, `SHA256`, `SHA512` или `CRC16`.
* **data, ...** — одна или несколько строк, которые движок склеит перед вычислением digest.

#### Описание и логика работы
`digest_hex` вычисляет хеш от склеенного текста и возвращает его в hex-виде строчными буквами. Если алгоритм не поддерживается текущей сборкой, builtin возвращает пустую строку. Это не файловая функция сама по себе, но отлично дополняет работу с файлами и БД: можно строить контрольные суммы, ключи кэша, подписи пакетов и проверочные идентификаторы. Помните, что builtin работает со строковыми данными, а не с произвольными binary blobs.

#### Практические сценарии использования
```qc
void() print_save_checksum =
{
    local string sum;

    sum = digest_hex("SHA256", mapname, ":", ftos(time));
    bprint("save id: ", sum, "\n");
};
```

### fork
`float(optional float sleeptime) fork = #210;`

* **sleeptime** — задержка перед тем, как «дочерний» QC-thread продолжит выполнение.

#### Описание и логика работы
`fork` — это не OS-level процесс, а механизм ветвления выполнения QuakeC в SSQC. Родительский поток немедленно возвращается из builtin со значением `0`, а дочерний — продолжает ту же точку выполнения позже и видит возвращаемое значение `1`. Из-за этого один и тот же вызывающий код может «вернуться дважды», поэтому в дочернем сценарии часто завершают выполнение через `abort()`. Неправильное понимание модели `fork` — прямой путь к дублирующимся эффектам, повторным начислениям и сложным race-like багам в логике мода.

#### Практические сценарии использования
```qc
void() delayed_match_banner =
{
    if (!fork(3))
        return; // родительский вызов заканчивается сразу

    bprint("Three seconds have passed since match start\n");
    abort(); // не возвращаемся в вызывающий код второй раз
};
```

### sleep
`void(float sleeptime) sleep = #212;`

* **sleeptime** — число секунд, на которое нужно приостановить текущий QC-thread.

#### Описание и логика работы
`sleep` приостанавливает текущий поток выполнения QuakeC и позволяет остальному игровому коду продолжать работу. После пробуждения локальные переменные сохраняются, а `self`/`other` восстанавливаются, если соответствующие сущности ещё существуют; если нет, движок подставит `world`. Нельзя рассчитывать, что глобалы или состояние полей за время сна останутся прежними. Поэтому `sleep` удобен для таймеров, но опасен для долгих критических секций и кода, который ожидает синхронный возврат значения немедленно.

#### Практические сценарии использования
```qc
void() restart_in_five_seconds =
{
    bprint("Restart in 5 seconds\n");
    sleep(5);
    localcmd("restart\n");
};
```

## Смежные страницы

- [Работа с данными из игровой логики](../18-data-access-from-scripts/README.md)
- [Индекс справочника builtins](./README.md)