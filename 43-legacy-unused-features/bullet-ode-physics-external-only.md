# Физика Bullet/ODE: только через внешние сборки

> [⬅ Вернуться к оглавлению вики](../README.md)

> Раздел: [Устаревшие, экспериментальные и недоступные по умолчанию возможности](./README.md)

Коротко: стандартное движение игроков и столкновения мира в FTEQW остаются на обычной Quake-физике. Builtins [`physics_enable`](../37-quakec-builtins-reference/03-entity-world-builtins.md#physics_enable), [`physics_addforce`](../37-quakec-builtins-reference/03-entity-world-builtins.md#physics_addforce) и [`physics_addtorque`](../37-quakec-builtins-reference/03-entity-world-builtins.md#physics_addtorque) относятся к отдельному physics backend'у и не должны считаться «обычно доступными» в стандартной сборке.

## Что важно

- В стандартной конфигурации `USE_INTERNAL_BULLET` и `USE_INTERNAL_ODE` отключены по умолчанию.
- В `bothdefs.h` подсистема physics backend'а (`USERBE`) включается только если сборка вообще содержит подходящий backend (`PLUGINS`, `USE_INTERNAL_BULLET` или `USE_INTERNAL_ODE`).
- В `pr_bgcmd.c` builtins физики собираются только когда этот backend-путь действительно включён.
- Даже при наличии builtin'ов они работают только если у мира поднят реальный backend; без него `MOVETYPE_PHYSICS` не должен быть единственной опорой геймплея.

## Практический вывод

Если мод опирается на `MOVETYPE_PHYSICS` и эти builtins, движок обычно нужно собирать специально: с доступными ODE/Bullet-библиотеками или с соответствующим physics-plugin/backend. Для portable/обычных релизов лучше считать эту подсистему опциональной и всегда иметь fallback на обычную Quake-физику.

## Смежные страницы

- [Продвинутая физика объектов (ODE/Bullet, не по умолчанию)](../33-physics-engines/advanced-object-physics-bullet-ode.md)
- [Сущности и игровой мир: builtins](../37-quakec-builtins-reference/03-entity-world-builtins.md)
