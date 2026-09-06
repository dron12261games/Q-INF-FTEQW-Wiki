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
`bufstr_get` возвращает строку по индексу или null string, если индекс пустой, вышел за диапазон или handle неверен. Это означает, что при обходе буфера нужно различать «слот существует и содержит пустую строку» и «слот отсутствует» по контексту заполнения списка. Возвращаемое значение — temp string, подходящая для немедленного использования в [`bprint`](12-system-debug-builtins.md#bprint), `fputs`, сравнении и т. д. Для sparse-буферов всегда проверяйте `if (!value)`.

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

### createbuffer
`void*(int bytes) createbuffer = #0:createbuffer;`

* **bytes** — `int`, размер выделяемого буфера памяти в байтах.

#### Описание и особенности работы
`createbuffer` выполняет временное выделение памяти, возвращая сырой указатель на созданный неструктурированный буфер. Данный метод разработан для последующего использования со специфическими pointer-based функциями вроде `fread`, `fwrite`, `digest_ptr` и аналогичными методами. В отличие от стандартной `memalloc`, этот буфер создаётся в области быстрой temp-памяти движка и автоматически освобождается интерпретатором в конце текущего кадра (GC-цикла), что избавляет разработчика от необходимости вручную вызывать `memfree`. Запрос размера `bytes <= 0` классифицируется движком как фатальная ошибка. Метод намного безопаснее для манипуляций с бинарными данными, чем склейка тяжелых tempstrings, и исключает фрагментацию кучи при частых дисковых операциях.

#### Пример использования
```qc
void() hash_small_file =
{
    local filestream f;
    local void *buf;
    local float got;
    local string sum;

    f = fopen("data/header.bin", FILE_READ);
    if (f < 0)
        return;

    buf = createbuffer(256);
    if (!buf)
    {
        fclose(f);
        return;
    }

    got = fread(f, buf, 256);
    sum = digest_ptr("SHA256", buf, got);
    bprint(sprintf("header sha256: %s\n", sum));
    fclose(f);
};
```

### digest_ptr
`string(string digest, void *data, int length, optional int offset) digest_ptr = #0:digest_ptr;`

* **digest** — `string`, алгоритм хеширования, поддерживаются варианты `MD5`, `SHA1`, `SHA256`, `SHA512` или `CRC16`.
* **data** — `void *`, указатель на исследуемый блок памяти или буфер с данными.
* **length** — `int`, размер обрабатываемых данных в байтах.
* **offset** — `int` (optional), необязательное смещение (в байтах) от начала указателя перед расчетом хеша.

#### Описание и особенности работы
`digest_ptr` производит расчет криптографических контрольных сумм и хешей напрямую из сырой оперативной памяти, обрабатывая произвольные бинарные блоки (binary blobs) без необходимости сохранять или преобразовывать их в формат QC-строк. Этот метод критически важен для античит-систем, верификации целостности сетевых пакетов или проверки сохранений на лету. Алгоритмы оптимизированы на стороне С-ядра движка; передача невалидного имени алгоритма или выход за границы выделенной памяти вызовет немедленное падение с логом `PF_digest_ptr`. В отличие от функции `digest_hex`, данный builtin работает исключительно с указателями на память, а не со строковыми переменными, возвращая результат в виде шестнадцатеричной строки.

#### Пример использования
```qc
void() print_save_digest =
{
    local filestream f;
    local __variant *buf;
    local float got;

    f = fopen("data/save.bin", FILE_READ);
    if (f < 0)
        return;

    buf = memalloc(1024);
    if (!buf)
    {
        fclose(f);
        return;
    }

    got = fread(f, buf, 1024);
    bprint(sprintf("save sha1: %s\n", digest_ptr("SHA1", buf, got)));
    memfree(buf);
    fclose(f);
};
```

### fread
`int(filestream fhandle, void *ptr, int size, optional int offset) fread = #0:fread;`

* **fhandle** — `filestream`, дескриптор файла, ранее открытого в режиме чтения (`FILE_READ`).
* **ptr** — `void *`, целевой указатель на блок памяти, куда запишутся данные.
* **size** — `int`, количество байт, которое необходимо считать из файла.
* **offset** — `int` (optional), необязательное смещение внутри целевого буфера `ptr`, куда будет производиться запись.

#### Описание и особенности работы
`fread` выполняет низкоуровневое чтение байтового потока с текущей позиции открытого файла и записывает данные непосредственно по указанному адресу в блоке оперативной памяти. Функция возвращает реальное количество успешно прочитанных байт; если достигнут конец файла (EOF) или произошел сбой диска, это число будет меньше параметра `size`. Обратите внимание: необязательный аргумент `offset` задает смещение именно внутри буфера памяти `ptr`, а не внутри файла. Это позволяет последовательно склеивать чанки данных в едином массиве без сдвига базового указателя. Перед использованием данного встроенного метода память под `ptr` должна быть гарантированно зарезервирована с помощью функций `memalloc` или `createbuffer`.

#### Пример использования
```qc
void() read_two_chunks =
{
    local filestream f;
    local __variant *buf;
    local float got;

    f = fopen("data/blob.bin", FILE_READ);
    if (f < 0)
        return;

    buf = memalloc(512);
    if (!buf)
    {
        fclose(f);
        return;
    }

    got = fread(f, buf, 256);
    got = got + fread(f, buf, 256, 256);
    bprint(sprintf("bytes read: %s\n", ftos(got)));

    memfree(buf);
    fclose(f);
};
```

### fseek
`int(filestream fhandle, optional int newoffset) fseek = #0:fseek;`

* **fhandle** — `filestream`, дескриптор открытого файла.
* **newoffset** — `int` (optional), новое абсолютное смещение в байтах от начала файла.

#### Описание и особенности работы
`fseek` перемещает внутренний курсор (указатель) файла. Если аргумент `newoffset` опущен или не предоставлен, функция просто возвращает текущую позицию курсора в байтах от начала файла. Это крайне полезно для запоминания позиции чтения перед переходом к другим структурам данных в рамках тяжелых mmap/write-файлов. При возникновении ошибки выполнения builtin возвращает `-1`. Стоит отметить, что относительное перемещение в стиле стандартного сишного `SEEK_CUR` здесь отсутствует, поэтому все смещения рассчитываются строго от нулевого байта.

#### Пример использования
```qc
void() reread_header =
{
    local filestream f;
    local __variant *buf;
    local float oldpos;

    f = fopen("data/demo.dat", FILE_READ);
    if (f < 0)
        return;

    buf = memalloc(32);
    if (!buf)
    {
        fclose(f);
        return;
    }

    fread(f, buf, 32);
    oldpos = fseek(f, 0);
    bprint(sprintf("previous cursor: %s\n", ftos(oldpos)));
    fread(f, buf, 32);

    memfree(buf);
    fclose(f);
};
```

### fseek64
`__int64(filestream fhandle, optional __int64 newoffset) fseek64 = #0:fseek64;`

* **fhandle** — `filestream`, дескриптор открытого файла.
* **newoffset** — `__int64` (optional), новое абсолютное смещение в файле в 64-битном целочисленном формате.

#### Описание и особенности работы
`fseek64` представляет собой 64-битную версию `fseek`, созданную для корректной работы с файлами огромного размера и mmap/write-архивами. На системном уровне данный builtin оперирует и возвращает текущую позицию курсора как тип `__int64`, что решает проблему переполнения, когда размер файла превышает лимиты стандартного 32-битного знакового `int` (2 ГБ). Если необязательный параметр `newoffset` не указан, функция возвращает текущее положение указателя. Для некоторых stream-only дескрипторов (сетевые потоки) произвольное изменение позиции может быть заблокировано на уровне виртуальной файловой системы (VFS), но интерпретатор QuakeC обработает это без критического падения.

#### Пример использования
```qc
void() query_large_cursor =
{
    local filestream f;
    local __int64 oldpos;

    f = fopen("data/huge.cache", FILE_READ);
    if (f < 0)
        return;

    oldpos = fseek64(f, 0);
    bprint("old 64-bit cursor queried\n");
    fclose(f);
};
```

### fsize
`int(filestream fhandle, optional int newsize) fsize = #0:fsize;`

* **fhandle** — `filestream`, дескриптор открытого файла.
* **newsize** — `int` (optional), новый размер файла в байтах, если требуется выполнить его обрезку (truncate) или расширение (extend).

#### Описание и особенности работы
`fsize` возвращает текущий физический размер файла в байтах. Если передан необязательный параметр `newsize`, движок инициирует изменение размера для файлов, открытых в режиме buffered/mmap-write: файл будет либо обрезан, либо расширен с заполнением образовавшихся пустот нулевыми байтами. Для защищенных архивов и файлов внутри системных VFS-паков (пакеты .pak) изменение размера заблокировано, и попытка записи вызовет предупреждение. При ошибках работы с диском или отсутствии прав доступа функция возвращает `-1`.

#### Пример использования
```qc
void() trim_server_log =
{
    local filestream f;
    local float oldsize;

    f = fopen("data/server.log", FILE_APPEND);
    if (f < 0)
        return;

    oldsize = fsize(f);
    if (oldsize > 65536)
        fsize(f, 65536);

    fclose(f);
};
```

### fsize64
`__int64(filestream fhandle, optional __int64 newsize) fsize64 = #0:fsize64;`

* **fhandle** — `filestream`, дескриптор открытого файла.
* **newsize** — `__int64` (optional), новое значение размера файла в 64-битном формате.

#### Описание и особенности работы
`fsize64` выполняет те же базовые задачи, что и `fsize`, но используется в тех сценариях, где размеры данных выходят за рамки классического 32-битного ограничения. Метод незаменим при манипуляциях с кэш-файлами объемом более 2 ГБ, гарантируя точный возврат и запись без потери старших битов в числе. Если целевой дескриптор указывает на динамический stream-поток (например, входящий сетевой буфер), размер может быть динамическим или неопределенным, и в таком случае данный builtin вернет `-1`, сигнализируя о невозможности замера.

#### Пример использования
```qc
void() inspect_large_file_size =
{
    local filestream f;
    local __int64 bytes;

    f = fopen("data/huge.cache", FILE_READ);
    if (f < 0)
        return;

    bytes = fsize64(f);
    bprint("queried 64-bit file size\n");
    fclose(f);
};
```

### fwrite
`int(filestream fhandle, void *ptr, int size, optional int offset) fwrite = #0:fwrite;`

* **fhandle** — `filestream`, дескриптор файла, открытого для записи.
* **ptr** — `void *`, указатель на буфер памяти, откуда берутся данные для записи.
* **size** — `int`, размер записываемого блока данных в байтах.
* **offset** — `int` (optional), необязательное смещение внутри буфера `ptr`, с которого начнется чтение.

#### Описание и особенности работы
`fwrite` производит низкоуровневую запись байтового потока из оперативной памяти в открытый файл и возвращает количество успешно записанных байт. При работе в режимах `FILE_WRITE`, `FILE_APPEND` и `FILE_MMAP_RW` метод принудительно сдвигает внутренний курсор файла на величину записанных данных. Необязательный аргумент `offset` задает смещение именно внутри исходного буфера `ptr`, благодаря чему можно гибко отправлять на диск отдельные фрагменты одной и той же структуры без ручного сдвига базового указателя. Передача невалидного адреса памяти или некорректного размера мгновенно приводит к аварийной остановке программы с системным логом `PF_fwrite: invalid ptr / size`.

#### Пример использования
```qc
void() write_packet_blob =
{
    local filestream f;
    local __variant *buf;

    f = fopen("data/packet.bin", FILE_WRITE);
    if (f < 0)
        return;

    buf = memalloc(8);
    if (!buf)
    {
        fclose(f);
        return;
    }

    memsetval(buf, 0, 287454020);
    memsetval(buf, 4, 1432778632);
    fwrite(f, buf, 8);

    memfree(buf);
    fclose(f);
};
```

### hash_getcb
`void(hashtable table, void(string keyname, __variant val) callback, optional string name) hash_getcb = #293;`

* **table** — `hashtable`, хэш-таблица, которая будет сканироваться.
* **callback** — функция-колбэк, которую движок будет вызывать для каждой подходящей пары данных.
* **name** — `string` (optional), необязательный ключ для фильтрации поиска конкретного элемента.

#### Описание и особенности работы
По изначальной спецификации `hash_getcb` должна была вызывать переданный колбэк для перебора всех элементов таблицы либо для поиска элемента по конкретному ключу `name`. Однако в текущей реализации ядра FTEQW (функция `PF_hash_getcb` в файле `pr_bgcmd.c`) этот builtin работает некорректно: аргументы могут интерпретироваться неверно, а итоговый вызов часто приводит к критическому сбою. Из-за этого использовать данный метод в стабильных проектах настоятельно не рекомендуется. Вместо него для безопасного обхода и выборки данных из хэш-таблиц разработчикам следует применять функции прямого доступа по ключу — `hash_getkey` и `hash_get`.

#### Пример использования
```qc
void(string keyname, __variant val) on_hash_pair =
{
    bprint(sprintf("seen key: %s\n", keyname));
};

void(hashtable tab) try_hash_callback =
{
    // В текущих версиях данный вызов небезопасен и приведен лишь для демонстрации
    hash_getcb(tab, on_hash_pair);
};
```

### json_find_object_child
`jsonnode(jsonnode node, string name) json_find_object_child = #0:json_find_object_child;`

* **node** — `jsonnode`, дескриптор родительского JSON-узла, имеющего тип объекта.
* **name** — `string`, имя искомого дочернего свойства.

#### Описание и особенности работы
`json_find_object_child` выполняет поиск дочернего элемента внутри JSON-объекта по его строковому имени и возвращает дескриптор найденного узла типа `jsonnode`, либо значение `__NULL__`, если свойство с таким именем отсутствует. Сопоставление имен во внутренней хэш-карте узла происходит на основе строгого точного совпадения (exact match). Функция оптимизирована на уровне Си-кода движка и работает значительно быстрее ручного перебора элементов через `json_get_child_at_index`. Полученный дочерний узел является частью общего дерева и валиден до тех пор, пока вся корневая структура не будет уничтожена с помощью `json_free`.

#### Пример использования
```qc
void() print_player_name_from_json =
{
    local jsonnode root, player, name;

    root = json_parse("{\"player\":{\"name\":\"Ranger\"}}");
    if (!root)
        return;

    player = json_find_object_child(root, "player");
    name = json_find_object_child(player, "name");
    if (name)
        bprint(sprintf("player=%s\n", json_get_string(name)));

    json_free(root);
};
```

### json_free
`void(jsonnode node) json_free = #0:json_free;`

* **node** — `jsonnode`, дескриптор корневого или изолированного JSON-узла, ранее созданного через `json_parse`.

#### Описание и особенности работы
`json_free` рекурсивно освобождает оперативную память, выделенную под хранение всего JSON-дерева, включая все вложенные объекты, массивы и значения. В качестве аргумента функции необходимо передавать именно тот базовый дескриптор, который был возвращен функцией `json_parse`. После вызова `json_free` использование данного дескриптора и любых его дочерних узлов в коде категорически запрещено, так как память очищается полностью. Повторный вызов `json_free` для одного и того же узла приведет к фатальной ошибке деаллокации.

#### Пример использования
```qc
void() parse_and_discard_json =
{
    local jsonnode root;

    root = json_parse("[1,2,3]");
    if (!root)
        return;

    bprint(sprintf("items=%s\n", ftos(json_get_length(root))));
    json_free(root);
};
```

### json_get_child_at_index
`jsonnode(jsonnode node, int childindex) json_get_child_at_index = #0:json_get_child_at_index;`

* **node** — `jsonnode`, родительский JSON-узел типа объекта или массива.
* **childindex** — `int`, индекс искомого дочернего элемента от `0` до `json_get_length(node) - 1`.

#### Описание и особенности работы
`json_get_child_at_index` возвращает дескриптор N-го дочернего элемента внутри JSON-массива или объекта. Этот метод незаменим для последовательного итерирования по массивам, а также для обхода полей объектов, когда точные имена ключей неизвестны заранее или их количество динамически меняется. Если переданный индекс выходит за границы диапазона или узел не содержит дочерних элементов (children), функция возвращает `__NULL__`. При итерации по свойствам объекта имя конкретного ключа для полученного дочернего узла можно извлечь сопутствующим методом `json_get_name`.

#### Пример использования
```qc
void() list_object_fields =
{
    local jsonnode root, child;
    local float i, count;

    root = json_parse("{\"hp\":100,\"armor\":50}");
    if (!root)
        return;

    count = json_get_length(root);
    for (i = 0; i < count; i = i + 1)
    {
        child = json_get_child_at_index(root, i);
        bprint(sprintf("%s\n", json_get_name(child)));
    }

    json_free(root);
};
```

### json_get_float
`float(jsonnode node) json_get_float = #0:json_get_float;`

* **node** — `jsonnode`, JSON-узел, текстовое или числовое значение которого будет интерпретировано как `float`.

#### Описание и особенности работы
`json_get_float` извлекает скалярное значение из JSON-узла и приводит его к типу с плавающей точкой `float`. Для нативных числовых полей значение возвращается напрямую; булевы типы `true`/`false` автоматически конвертируются в `1.0`/`0.0`, а строковые значения парсятся через внутренний движковый аналог функции `atof`. Массивы, объекты и пустые значения `null` возвращают `0.0`. Стоит помнить, что данный метод выполняет принудительное приведение типов на лету, не генерируя ошибок при несоответствии. Если вам требуется строгая валидация структуры перед чтением, сначала проверьте исходный тип узла с помощью функции `json_get_value_type`.

#### Пример использования
```qc
void() print_spawn_delay =
{
    local jsonnode root, node;

    root = json_parse("{\"delay\":1.5}");
    if (!root)
        return;

    node = json_find_object_child(root, "delay");
    bprint(sprintf("delay=%s\n", ftos(json_get_float(node))));
    json_free(root);
};
```

### json_get_integer
`int(jsonnode node) json_get_integer = #0:json_get_integer;`

* **node** — `jsonnode`, JSON-узел, значение которого будет приведено к целочисленному типу `int`.

#### Описание и особенности работы
`json_get_integer` считывает содержимое узла и возвращает его в виде целого числа. Для стандартных JSON-чисел (number), а также булевых `true` и `false` функция возвращает их прямое математическое представление. Если узел содержит строку, движок пытается преобразовать её в число с помощью встроенного механизма `atoi`. В случае сложных структур (объекты, массивы) или значения `null` функция безопасно возвращает `0`. Этот метод удобен для быстрого получения флагов, лимитов или индексов, но из-за отбрасывания дробной части не подходит для точных физических или временных расчетов — для них лучше использовать `json_get_float`.

#### Пример использования
```qc
void() print_frag_limit =
{
    local jsonnode root, node;

    root = json_parse("{\"fraglimit\":25}");
    if (!root)
        return;

    node = json_find_object_child(root, "fraglimit");
    bprint(sprintf("fraglimit=%s\n", ftos(json_get_integer(node))));
    json_free(root);
};
```

### json_get_length
`int(jsonnode node) json_get_length = #0:json_get_length;`

* **node** — `jsonnode`, родительский JSON-узел типа объекта или массива.

#### Описание и особенности работы
`json_get_length` возвращает общее число дочерних элементов в массиве или полей в объекте. Для скалярных типов данных (строки, числа, булевы значения, null) builtin всегда возвращает `0`, поэтому его можно использовать в качестве безопасной первичной проверки структуры перед итерацией. Данная функция замеряет количество элементов только первого уровня вложенности и не рассчитывает рекурсивно внутреннее содержимое сложных дочерних узлов. Чаще всего применяется для организации циклов последовательного перебора элементов совместно с функцией `json_get_child_at_index`.

#### Пример использования
```qc
void() print_vote_count =
{
    local jsonnode root;

    root = json_parse("[\"yes\",\"no\",\"yes\"]");
    if (!root)
        return;

    bprint(sprintf("votes=%s\n", ftos(json_get_length(root))));
    json_free(root);
};
```

### json_get_name
`string(jsonnode node) json_get_name = #0:json_get_name;`

* **node** — `jsonnode`, дочерний JSON-узел, входящий в состав родительского объекта.

#### Описание и особенности работы
`json_get_name` возвращает имя ключа (свойства), которому принадлежит текущий дочерний узел в JSON-объекте. Эта функция незаменима при динамическом разборе конфигураций, когда вы последовательно извлекаете узлы через `json_get_child_at_index` и вам необходимо узнать не только значение поля, но и его название. Для элементов, находящихся внутри массивов или являющихся корнем всего дерева, функция возвращает пустую строку `""`. Память под возвращаемую строку выделяется движком автоматически в пуле временных строк и валидна на протяжении текущего кадра.

#### Пример использования
```qc
void() dump_setting_names =
{
    local jsonnode root, child;
    local float i;

    root = json_parse("{\"music\":1,\"gamma\":1.2}");
    if (!root)
        return;

    for (i = 0; i < json_get_length(root); i = i + 1)
    {
        child = json_get_child_at_index(root, i);
        bprint(sprintf("field: %s\n", json_get_name(child)));
    }

    json_free(root);
};
```

### json_get_string
`string(jsonnode node) json_get_string = #0:json_get_string;`

* **node** — `jsonnode`, JSON-узел с типом строки.

#### Описание и особенности работы
`json_get_string` извлекает текстовое содержимое из JSON-узла, имеющего строго тип `JSON_TYPE_STRING`. Для любых других типов данных этот builtin возвращает пустую строку `""`, не выполняя автоматического приведения чисел или булевых флагов в текстовый вид. Функция не содержит скрытых аллокаций памяти QuakeC, так как возвращает прямой строковый указатель, созданный и сохраненный в кэше движка в момент парсинга дерева. Содержимое возвращаемой строки полностью соответствует данным, которые были переданы в исходном JSON-файле (включая Unicode-символы).

#### Пример использования
```qc
void() print_map_rotation_entry =
{
    local jsonnode root, node;

    root = json_parse("{\"nextmap\":\"dm6\"}");
    if (!root)
        return;

    node = json_find_object_child(root, "nextmap");
    if (node)
        bprint(sprintf("nextmap=%s\n", json_get_string(node)));

    json_free(root);
};
```

### json_get_value_type
`json_type_e(jsonnode node) json_get_value_type = #0:json_get_value_type;`

* **node** — `jsonnode`, исследуемый JSON-узел.

#### Описание и особенности работы
`json_get_value_type` возвращает базовый тип данных узла в виде перечисления: `JSON_TYPE_STRING`, `JSON_TYPE_NUMBER`, `JSON_TYPE_OBJECT`, `JSON_TYPE_ARRAY`, `JSON_TYPE_TRUE`, `JSON_TYPE_FALSE` или `JSON_TYPE_NULL`. Это критически важный системный метод валидации, позволяющий определить, какую именно функцию извлечения данных (`json_get_string`, `json_get_float`, `json_get_integer`) или итерации следует вызвать дальше без риска получить некорректное значение `0` или пустую строку. Передача неинициализированного или нулевого дескриптора узла вызовет ошибку обращения в интерпретаторе.

#### Пример использования
```qc
void() inspect_json_root =
{
    local jsonnode root;

    root = json_parse("[1,2,3]");
    if (!root)
        return;

    if (json_get_value_type(root) == JSON_TYPE_ARRAY)
        bprint("root is array\n");

    json_free(root);
};
```

### json_parse
`jsonnode(string data) json_parse = #0:json_parse;`

* **data** — `string`, валидная текстовая JSON-строка документа.

#### Описание и особенности работы
`json_get_value_type` парсит JSON-строку и возвращает дескриптор корневого узла `jsonnode`, либо значение `__NULL__`, если в процессе разбора произошла синтаксическая ошибка. Ядро движка FTEQW компилирует дерево объектов в единый contiguous memory block, что гарантирует высокую скорость доступа к дочерним узлам по их дескрипторам и исключает утечки памяти в процессе итерации. Разработчик обязан принудительно освободить память, занятую корневым узлом, вызвав функцию `json_free`. Попытка вызова дочерних узлов или чтение данных после уничтожения корня вызовет критическое падение интерпретатора.

#### Пример использования
```qc
void() parse_basic_document =
{
    local jsonnode root;

    root = json_parse("{\"mode\":\"ctf\",\"teams\":2}");
    if (!root)
    {
        bprint("bad json\n");
        return;
    }

    bprint(sprintf("top-level fields=%s\n", ftos(json_get_length(root))));
    json_free(root);
};
```

### memcmp
`int(__variant *dst, __variant *src, int size, optional int srcoffset, optional int dstoffset) memcmp = #0:memcmp;`

* **dst** — `__variant *`, указатель на первый блок памяти для сравнения.
* **src** — `__variant *`, указатель на второй блок памяти для сравнения.
* **size** — `int`, объем сравниваемых данных в байтах.
* **srcoffset** — `int` (optional), необязательное смещение (в байтах) относительно указателя `src`.
* **dstoffset** — `int` (optional), необязательное смещение (в байтах) относительно указателя `dst`.

#### Описание и особенности работы
`memcmp` производит побайтовое сравнение двух областей оперативной памяти и возвращает `0`, если содержимое полностью идентично; отрицательное или положительное значение возвращается в случае несовпадения байт по аналогии с классической функцией Си `memcmp`. Архитектурная деталь FTEQW: первый необязательный оффсет применяется к `src`, а второй — к `dst`. Функция работает напрямую с сырой памятью и не осуществляет проверку типов, поэтому перед сравнением структур необходимо убедиться, что они имеют одинаковое выравнивание в памяти. Запрос размера `size` за пределами реально выделенной памяти приведет к ошибке сегментации.

#### Пример использования
```qc
void() compare_two_headers =
{
    local __variant *a, *b;

    a = memalloc(8);
    b = memalloc(8);
    if (!a || !b)
        return;

    memsetval(a, 0, 10);
    memsetval(a, 4, 20);
    bprint(sprintf("headers are equal\n"));

    memfree(a);
    memfree(b);
};
```

### memrealloc
`__variant*(void *oldptr, int newsize) memrealloc = #0:memrealloc;`

* **oldptr** — `void *`, исходный указатель на ранее выделенный блок памяти.
* **newsize** — `int`, новый требуемый размер блока памяти в байтах.

#### Описание и особенности работы
`memrealloc` изменяет размер ранее выделенного блока памяти, стараясь расширить его по текущему адресу, либо переносит данные в новую область кучи, сохраняя старое содержимое в пределах минимального из двух размеров. Передача нулевого или неинициализированного указателя `oldptr` вызовет ошибку, так как функция работает строго с существующими аллокациями. Если новый размер равен `0`, поведение функции эквивалентно вызову `memfree`. Метод возвращает новый адрес блока, который необходимо переприсвоить исходной переменной во избежание появления повисших указателей.

#### Пример использования
```qc
void() grow_snapshot_buffer =
{
    local __variant *buf;

    buf = memalloc(8);
    if (!buf)
        return;

    memsetval(buf, 0, 111);
    memsetval(buf, 4, 222);
    buf = memrealloc(buf, 16);
    if (!buf)
        return;

    memsetval(buf, 8, 333);
    memsetval(buf, 12, 444);
    memfree(buf);
};
```

### memstrsize
`float(string s) memstrsize = #0:memstrsize;`

* **s** — `string`, строка, длину которой необходимо замерить в байтах.

#### Описание и особенности работы
`memstrsize` отличается от стандартной `strlen` тем, что она замеряет чистый объем сырых байт (raw bytes) UTF-8 строки, а не количество отображаемых графических символов. Данный builtin всегда возвращает физический размер C-строки в оперативной памяти до терминального нуля `\0`. Результат для базовых ASCII-символов полностью совпадает с обычной длиной, но для кириллицы, эмодзи и других мультибайтовых UTF-8 символов он будет в несколько раз больше. Это критически важно, когда строку нужно сериализовать в буфер памяти или рассчитать точный размер выделения памяти перед отправкой данных на диск.

#### Пример использования
```qc
void() print_utf8_byte_length =
{
    local string s;

    s = "Я"; // Кириллица в UTF-8 занимает 2 байта
    bprint(sprintf("bytes=%s\n", ftos(memstrsize(s)))); // Выведет: bytes=2
};
```

### search_fopen
`filestream(searchhandle handle, float num) search_fopen = #0:search_fopen;`

* **handle** — `searchhandle`, дескриптор поисковой сессии, ранее созданный через `search_begin`.
* **num** — `float`, индекс найденного файла в списке результатов поиска.

#### Описание и особенности работы
`search_fopen` открывает дескриптор файла напрямую из списка результатов поиска, избавляя от необходимости вручную формировать строковый путь. Это гарантирует, что файл будет открыт именно из того конкретного приоритетного расположения (например, внутри pak/pk3 архива или конкретной gamedir), где поисковый алгоритм его обнаружил, даже если в других директориях существуют файлы-дубликаты. Файл открывается исключительно в режиме чтения. Если передан неверный индекс или невалидный поисковый хендл, builtin возвращает `-1`.

#### Пример использования
```qc
void() open_exact_shader =
{
    local searchhandle h;
    local filestream f;

    h = search_begin("scripts/common.shader", SB_ALLOWDUPES, 1, "");
    if (h < 0 || !search_getsize(h))
        return;

    f = search_fopen(h, 0);
    if (f >= 0)
        fclose(f);

    search_end(h);
};
```

### search_getfilemtime
`string(searchhandle handle, float num) search_getfilemtime = #0:search_getfilemtime;`

* **handle** — `searchhandle`, дескриптор активной поисковой сессии.
* **num** — `float`, индекс найденного файла.

#### Описание и особенности работы
`search_getfilemtime` возвращает дату и время последнего изменения указанного файла в виде стандартизированной строки формата `YYYY-MM-DD HH:MM:SS` по часовому поясу локального хоста. Если целевой файл находится внутри упакованного виртуального архива (.pak) и не содержит метаданных о дате изменения, builtin возвращает пустую строку `""`. Переданный `searchhandle` должен оставаться открытым до завершения вызовов функции. Метод активно применяется разработчиками для синхронизации кэша, отслеживания обновлений файлов конфигурации или организации встроенного лога обновлений мода.

#### Пример использования
```qc
void() show_first_map_mtime =
{
    local searchhandle h;

    h = search_begin("maps/*.bsp", 0, 1, "");
    if (h < 0 || !search_getsize(h))
        return;

    bprint(sprintf("%s -> %s\n", search_getfilename(h, 0), search_getfilemtime(h, 0)));
    search_end(h);
};
```

### search_getfilesize
`float(searchhandle handle, float num) search_getfilesize = #0:search_getfilesize;`

* **handle** — `searchhandle`, дескриптор активной поисковой сессии.
* **num** — `float`, индекс найденного файла.

#### Описание и особенности работы
`search_getfilesize` возвращает физический размер найденного файла в байтах. Этот метод является частью спецификации расширения `DP_QC_FS_SEARCH` и спроектирован для быстрой оптимизации: он извлекает размер прямо из метаданных файловой структуры VFS, не требуя предварительного открытия файла через `fopen`. При передаче неверного индекса или закрытого хендла функция возвращает `0`, поэтому вызов имеет смысл осуществлять в цикле, границы которого ограничены значением `search_getsize`, используя параллельно метод `search_getfilename` для вывода логов.

#### Пример использования
```qc
void() list_cfg_sizes =
{
    local searchhandle h;
    local float i;

    h = search_begin("configs/*.cfg", 0, 1, "");
    if (h < 0)
        return;

    for (i = 0; i < search_getsize(h); i = i + 1)
        bprint(sprintf("%s: %s bytes\n", search_getfilename(h, i), ftos(search_getfilesize(h, i))));

    search_end(h);
};
```

### search_getpackagename
`string(searchhandle handle, float num) search_getpackagename = #0:search_getpackagename;`

* **handle** — `searchhandle`, дескриптор поисковой сессии.
* **num** — `float`, индекс найденного файла.

#### Описание и особенности работы
`search_getpackagename` определяет, из какого именно архива (package), gamedir-папки или пака был загружен или обнаружен конкретный файл. Этот метод критически важен при использовании флагов поиска `SB_ALLOWDUPES` и `SB_FULLPACKAGEPATH`, когда вам необходимо выявить потенциальные конфликты или дубликаты ресурсов внутри pk3/pak файлов. Если целевой файл расположен в обычной директории в виде loose-файла, функция вернет имя текущего базового gamedir-каталога. Для получения самого имени файла в рамках этой же итерации по-прежнему следует использовать метод `search_getfilename`.

#### Пример использования
```qc
void() print_duplicate_origins =
{
    local searchhandle h;
    local float i;

    h = search_begin("progs.dat", SB_ALLOWDUPES, 1, "");
    if (h < 0)
        return;

    for (i = 0; i < search_getsize(h); i = i + 1)
        bprint(sprintf("%s from %s\n", search_getfilename(h, i), search_getpackagename(h, i)));

    search_end(h);
};
```

### sqlescapeblob
`string(float serveridx, __variant *ptr, int maxsize) sqlescapeblob = #0:sqlescapeblob;`

* **serveridx** — `float`, уникальный индекс SQL-сервера в системе.
* **ptr** — `__variant *`, указатель на сырой буфер памяти, который нужно экранировать.
* **maxsize** — `int`, максимальный объем данных в байтах для обработки.

#### Описание и особенности работы
`sqlescapeblob` конвертирует бинарные данные из оперативной памяти в валидный текстовый литерал SQL-запроса вида `x'DEADBEEF'`, который можно безопасно вставлять в `INSERT` или `UPDATE` команды СУБД. Поведение метода зависит от используемого SQL-драйвера: он преобразует массив в строку шестнадцатеричных символов в нижнем регистре (lowercase hex) и, если того требует специфика бекенда, применяет драйверное экранирование. Передаваемый указатель должен быть валидным и ссылаться на доступный сегмент памяти, иначе движок аварийно завершит работу с логом `PF_sqlescapeblob: invalid blob`. Для записи очень больших BLOB-массивов рекомендуется использовать параметризованные запросы вместо данного метода во избежание переполнения пула строк QuakeC.

#### Пример использования
```qc
void() print_blob_literal =
{
    local __variant *buf;
    local string blobsql;

    buf = memalloc(4);
    if (!buf)
        return;

    memfill8(buf, 255, 4);
    blobsql = sqlescapeblob(g_sql_server, buf, 4);
    bprint(sprintf("literal=%s\n", blobsql));
    memfree(buf);
};
```

### sqlreadblob
`int(float serveridx, float queryidx, float row, float column, __variant *ptr, int maxsize) sqlreadblob = #0:sqlreadblob;`

* **serveridx** — `float`, глобальный индекс SQL-соединения.
* **queryidx** — `float`, дескриптор выполненного SQL-запроса.
* **row** — `float`, индекс строки в результирующей выборке.
* **column** — `float`, индекс колонки в результирующей выборке.
* **ptr** — `__variant *`, указатель на целевой массив памяти, куда запишутся данные.
* **maxsize** — `int`, лимит байт, который разрешено скопировать в буфер `ptr`.

#### Описание и особенности работы
`sqlreadblob` считывает бинарные данные (BLOB) из указанной ячейки текущей выборки базы данных и копирует их напрямую в оперативную память QuakeC, возвращая фактическое количество полученных байт. Если размер данных в БД превышает значение параметра `maxsize`, то запись будет безопасно обрезана по этой границе. В случае невалидных индексов строк/колонок, ошибок соединения или если поле содержит значение `NULL`, builtin вернет `0`. Память, на которую ссылается указатель `ptr`, должна быть заранее зарезервирована разработчиком через `memalloc` или аналогичные функции аллокации.

#### Пример использования
```qc
void() read_avatar_blob =
{
    local __variant *buf;
    local float got;

    buf = memalloc(256);
    if (!buf)
        return;

    got = sqlreadblob(g_sql_server, g_sql_query, 0, 1, buf, 256);
    bprint(sprintf("blob bytes=%s\n", ftos(got)));
    memfree(buf);
};
```

## Смежные страницы

- [Работа с данными из игровой логики](../18-data-access-from-scripts/README.md)
- [Индекс справочника builtins](./README.md)