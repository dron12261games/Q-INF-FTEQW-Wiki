# Lua-скрипты на сервере

> [⬅ Вернуться к оглавлению вики](../README.md)

> Раздел: [Альтернативные языки и виртуальные машины игровой логики](./README.md)

## Что это даёт геймдизайнеру

Классическая игровая логика движка пишется на собственном языке QuakeC (см. [«Игровая логика: язык QuakeC»](../16-quakec-scripting/README.md)), но в сборках движка с соответствующей поддержкой серверную логику можно написать и на языке **Lua** — простом, широко распространённом скриптовом языке, который используется во множестве других игр и приложений. Это удобно тем, кто уже знаком с Lua по другим проектам или хочет обойтись без специфического синтаксиса QuakeC для небольших модификаций и утилитарных задач. При этом Lua-версия логики использует практически тот же набор стандартных команд, что и QuakeC, — так что весь опыт работы с полями объектов, событиями и стандартными функциями (см. [«Серверная игровая логика (SSQC)»](../16-quakec-scripting/server-side-quakec-ssqc.md)) переносится почти без изменений, только на другом синтаксисе.

## Интерфейс настройки

Движок ищет при старте сервера в игровой директории файл с именем **`qwprogs.lua`** или **`progs.lua`** — если такой файл найден (и движок собран с поддержкой Lua), он загружается и исполняется как основная серверная логика, вместо обычного скомпилированного файла QuakeC. Сам файл — это обычный текстовый Lua-скрипт, который можно редактировать любым текстовым редактором без отдельной компиляции: движок компилирует и запускает его прямо при загрузке.

Поддержка Lua присутствует не во всех сборках движка — если разработчик хочет опираться на эту возможность, стоит явно указать пользователям, что необходима сборка движка с включённой поддержкой Lua.

### Точки входа, которые вызывает движок

Точно так же, как и в обычной SSQC-логике, движок сам вызывает по имени определённые функции в нужные моменты: [`main`](../37-quakec-builtins-reference/00-entry-points.md#main-устаревшая-не-вызывается) (аналог `init`), [`SetNewParms`](../37-quakec-builtins-reference/00-entry-points.md#setnewparms), [`SetChangeParms`](../37-quakec-builtins-reference/00-entry-points.md#setchangeparms), [`ClientConnect`](../37-quakec-builtins-reference/00-entry-points.md#clientconnect), [`PutClientInServer`](../37-quakec-builtins-reference/00-entry-points.md#putclientinserver), [`ClientDisconnect`](../37-quakec-builtins-reference/00-entry-points.md#clientdisconnect), [`ClientKill`](../37-quakec-builtins-reference/00-entry-points.md#clientkill), [`PlayerPreThink`](../37-quakec-builtins-reference/00-entry-points.md#playerprethink), [`PlayerPostThink`](../37-quakec-builtins-reference/00-entry-points.md#playerpostthink), [`StartFrame`](../37-quakec-builtins-reference/00-entry-points.md#startframe), `ClientReEnter` — назначение каждой из них полностью совпадает с одноимёнными функциями SSQC (см. таблицу точек входа на странице про серверную логику).

### Песочница: что в Lua-скрипте нарочно недоступно

В целях безопасности сервера стандартная библиотека Lua урезана: из неё убраны прямой доступ к файловой системе операционной системы и запуску внешних программ (`dofile`, `loadfile`, `os.execute` и подобные — недоступны). Вместо этого доступна урезанная замена **`require`**, читающая файлы через собственную файловую систему движка (то есть только из папки самой игры/архивов мода), а не с произвольных путей компьютера.

### Доступные функции — практически весь стандартный набор QuakeC

Подавляющее большинство стандартных функций движка доступно в Lua-скрипте под теми же самыми именами, что и в QuakeC, поэтому вся документация по стандартным builtin-функциям SSQC применима и здесь:

- **Объекты мира**: [`spawn`](../37-quakec-builtins-reference/03-entity-world-builtins.md#spawn), [`remove`](../37-quakec-builtins-reference/03-entity-world-builtins.md#remove), [`nextent`](../37-quakec-builtins-reference/03-entity-world-builtins.md#nextent), `nextclient`, [`makestatic`](../37-quakec-builtins-reference/03-entity-world-builtins.md#makestatic), [`setorigin`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setorigin), [`setsize`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setsize), [`setmodel`](../37-quakec-builtins-reference/03-entity-world-builtins.md#setmodel), [`findradius`](../37-quakec-builtins-reference/03-entity-world-builtins.md#findradius) (плюс два Lua-специфичных дополнения — `findradiuschain` — то же самое, но без раздражающих особенностей поведения классической версии — и `findradiustable`, возвращающий результат сразу в виде таблицы/массива).
- **Предзагрузка ресурсов**: [`precache_model`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#precache_model), [`precache_sound`](../37-quakec-builtins-reference/05-sound-builtins.md#precache_sound).
- **Сообщения игрокам**: [`bprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#bprint) (всем), [`sprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#sprint) (одному), [`dprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#dprint)/`conprint` (в консоль/лог), [`centerprint`](../37-quakec-builtins-reference/12-system-debug-builtins.md#centerprint) (по центру экрана) — с некоторыми различиями в наборе аргументов между QuakeWorld и NetQuake режимом работы сервера.
- **Звук**: [`sound`](../37-quakec-builtins-reference/05-sound-builtins.md#sound), [`ambientsound`](../37-quakec-builtins-reference/05-sound-builtins.md#ambientsound).
- **Служебные функции**: [`random`](../37-quakec-builtins-reference/01-math-vector-builtins.md#random), [`checkclient`](../37-quakec-builtins-reference/03-entity-world-builtins.md#checkclient), [`stuffcmd`](../37-quakec-builtins-reference/04-network-messages-builtins.md#stuffcmd), [`localcmd`](../37-quakec-builtins-reference/12-system-debug-builtins.md#localcmd), [`cvar`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar) (только число), `cvar_get` (Lua-дополнение, может вернуть исходную строку), [`cvar_set`](../37-quakec-builtins-reference/12-system-debug-builtins.md#cvar_set), [`lightstyle`](../37-quakec-builtins-reference/08-csqc-rendering-builtins.md#lightstyle), `error`, [`objerror`](../37-quakec-builtins-reference/12-system-debug-builtins.md#objerror).
- **Математика**: полноценная таблица `math` с функциями `abs`, [`rint`](../37-quakec-builtins-reference/01-math-vector-builtins.md#rint), [`ceil`](../37-quakec-builtins-reference/01-math-vector-builtins.md#ceil), [`floor`](../37-quakec-builtins-reference/01-math-vector-builtins.md#floor), [`sin`](../37-quakec-builtins-reference/01-math-vector-builtins.md#sin), [`cos`](../37-quakec-builtins-reference/01-math-vector-builtins.md#cos), [`tan`](../37-quakec-builtins-reference/01-math-vector-builtins.md#tan), [`asin`](../37-quakec-builtins-reference/01-math-vector-builtins.md#asin), [`acos`](../37-quakec-builtins-reference/01-math-vector-builtins.md#acos), [`atan`](../37-quakec-builtins-reference/01-math-vector-builtins.md#atan), [`atan2`](../37-quakec-builtins-reference/01-math-vector-builtins.md#atan2), [`sqrt`](../37-quakec-builtins-reference/01-math-vector-builtins.md#sqrt), [`pow`](../37-quakec-builtins-reference/01-math-vector-builtins.md#pow) и константой `math.pi`.
- **Вспомогательные функции для типов, которых нет в самом Lua**: `vec3(...)` — создание вектора, `field(...)` — обращение к полю объекта по имени (поскольку в обычном Lua нет отдельного типа «поле объекта QuakeC», как в самом QuakeC).

## Примеры

- Небольшой одиночный мод-головоломка целиком реализован на `progs.lua`, без единой строчки QuakeC, что упростило его разработку для автора, ранее не знакомого с движком.
- Утилитарный серверный скрипт (например, автоматическая ротация карт с дополнительными условиями) написан на Lua и подключается к обычной QuakeC-логике игры как вспомогательный модуль.
- Автор, ранее писавший игровые скрипты для других движков на Lua, переносит знакомые практики прямо в SSQC-подобные точки входа (`PlayerPreThink`, `ClientConnect`), не изучая синтаксис QuakeC.

## Смежные страницы

- [Серверная игровая логика (SSQC)](../16-quakec-scripting/server-side-quakec-ssqc.md)
- [Игровая логика: язык QuakeC](../16-quakec-scripting/README.md)
- [Альтернативный байт-код игровой логики (Q1QVM)](./q1qvm-bytecode.md)
- [Совместимость с модулями логики Quake III / Half-Life](./quake3-halflife-game-modules.md)
