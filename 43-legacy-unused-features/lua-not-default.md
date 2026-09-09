# Lua не включена по умолчанию

> [⬅ Вернуться к оглавлению вики](../README.md)

Lua-поддержка есть в исходниках FTEQW, но в стандартной конфигурации сборки она **выключена**: опция `VM_LUA` закомментирована. Поэтому на обычную сборку движка нельзя рассчитывать как на среду, которая автоматически поймёт `progs.lua`.

## Что важно знать

- Если разработчик **сам пересобрал движок** с включённой Lua-поддержкой, сервер ищет файл `qwprogs.lua`, а если его нет — `progs.lua`.
- Lua-логика запускается вместо обычного серверного `progs.dat`/`qwprogs.dat`.
- Используются почти те же точки входа, что и у [SSQC](../16-quakec-scripting/server-side-quakec-ssqc.md): `StartFrame`, `PlayerPreThink`, `PlayerPostThink`, `ClientConnect`, `PutClientInServer`, `ClientDisconnect`, `ClientKill`, `SetNewParms`, `SetChangeParms`; дополнительно поддерживается `ClientReEnter`.
- Движок специально режет опасные части стандартной Lua-библиотеки: `dofile` и `loadfile` отключены, а `require` читает файлы только через игровую файловую систему движка.
- Есть несколько Lua-специфичных удобств: `cvar_get`, `findradiuschain`, `findradiustable`, `vec3(...)`, `field(...)`.

## Когда это имеет смысл

Только если вы **контролируете свою собственную сборку движка** и готовы сами проверять совместимость на целевой платформе. Для обычного мода, который должен запускаться на типовых сборках FTEQW, безопаснее опираться на [SSQC](../16-quakec-scripting/server-side-quakec-ssqc.md) или [Q1QVM](../17-alternative-scripting-vms/q1qvm-bytecode.md).
