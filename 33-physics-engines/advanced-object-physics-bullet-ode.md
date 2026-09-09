# Продвинутая физика объектов (ODE/Bullet, не по умолчанию)

> [⬅ Предыдущая страница](../32-third-party-services/jabber-xmpp-messaging.md) | [Следующая страница ➡](../35-engine-plugins/engine-plugin-modules.md)

> [⬅ Вернуться к оглавлению вики](../README.md)

## Что это даёт геймдизайнеру

В обычной сборке FTEQW движение игроков и базовые столкновения мира остаются на стандартной Quake-физике. Дополнительно существуют специальные physics-backend'ы ODE/Bullet, которые могут брать на себя поведение отдельных объектов вроде обломков, бочек или ящиков — с падением, вращением и реакцией на импульсы.

Подробности по сборке, плагинам и ограничениям вынесены в [«Физика Bullet/ODE: только через внешние сборки»](../43-legacy-unused-features/bullet-ode-physics-external-only.md).

---

## Интерфейс настройки

Точка входа для такого режима — `MOVETYPE_PHYSICS` у конкретной entity. Если в сборке действительно есть physics backend, мод может дополнительно пользоваться builtins:

| Функция | Сигнатура | Назначение |
| :--- | :--- | :--- |
| [`physics_enable`](../37-quakec-builtins-reference/03-entity-world-builtins.md#physics_enable) | `void(entity e, float physics_enabled)` | Включает или выключает расчёт physics backend'ом для объекта с `MOVETYPE_PHYSICS` |
| [`physics_addforce`](../37-quakec-builtins-reference/03-entity-world-builtins.md#physics_addforce) | `void(entity e, vector force, vector relative_ofs)` | Прикладывает импульс силы к объекту |
| [`physics_addtorque`](../37-quakec-builtins-reference/03-entity-world-builtins.md#physics_addtorque) | `void(entity e, vector torque)` | Прикладывает вращающий импульс |

Но для portable/обычных релизов безопаснее проектировать мод так, будто этих builtins и самого backend'а может не быть, и оставлять fallback на стандартную Quake-физику.

---

## Примеры

- Разрушаемые обломки могут использовать `MOVETYPE_PHYSICS` в специальной сборке с backend'ом ODE/Bullet.
- Тот же мод в обычной сборке должен продолжать работать на стандартной Quake-физике без зависимости от ODE/Bullet.

---

## Смежные страницы

- [Физика Bullet/ODE: только через внешние сборки](../43-legacy-unused-features/bullet-ode-physics-external-only.md)
- [Мод-манифест-файл](../23-mods-manifests/mod-manifest-file.md)

> [⬅ Предыдущая страница](../32-third-party-services/jabber-xmpp-messaging.md) | [Следующая страница ➡](../35-engine-plugins/engine-plugin-modules.md)

> [⬅ Вернуться к оглавлению вики](../README.md)