# Совместимость с модулями логики Quake II / Quake III / Half-Life

> [⬅ Предыдущая страница](q1qvm-bytecode.md) | [Следующая страница ➡](../18-data-access-from-scripts/embedded-database-sql.md)

> [⬅ Вернуться к оглавлению вики](../README.md)

## Коротко

Это не обычный путь разработки под FTEQW, а режим совместимости/legacy. В типовой конфигурации движка встречаются подсистемы совместимости Quake II/Quake III, но загрузка нативного игрового кода всё равно дополнительно заблокирована переменной [`com_gamedirnativecode 0`](../38-cvars-reference/04-network-server-cvars.md#com_gamedirnativecode). Поддержка Half-Life в той же стандартной конфигурации вообще выключена по умолчанию.

Поэтому на такие модули нельзя полагаться без собственной проверенной сборки движка и без осознанного доверия к загружаемому коду. Подробности, ограничения по платформам и реальные практические оговорки вынесены на страницу [Нативные игровые модули других движков](../43-legacy-unused-features/native-game-modules-not-default.md).

Для обычного мода, который должен запускаться на типовых сборках FTEQW, ориентируйтесь прежде всего на [SSQC](../16-quakec-scripting/server-side-quakec-ssqc.md), [CSQC](../16-quakec-scripting/client-side-quakec-csqc.md) и [Q1QVM](./q1qvm-bytecode.md).

---

## Смежные страницы

- [Альтернативный байт-код игровой логики (Q1QVM)](./q1qvm-bytecode.md)
- [Нативные игровые модули других движков](../43-legacy-unused-features/native-game-modules-not-default.md)
- [Игровая логика: язык QuakeC](../README.md#игровая-логика-язык-quakec)

> [⬅ Предыдущая страница](q1qvm-bytecode.md) | [Следующая страница ➡](../18-data-access-from-scripts/embedded-database-sql.md)

> [⬅ Вернуться к оглавлению вики](../README.md)