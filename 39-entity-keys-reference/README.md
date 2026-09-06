# Справочник ключей сущностей карты (entity keys)

> [⬅ Вернуться к оглавлению вики](../README.md)

Каждая сущность (entity) на карте Quake — будь то `worldspawn`, [`light`](01-worldspawn-common-keys.md#light), `func_door` или `monster_army` — описывается в файле карты набором пар «ключ-значение» (key/value). Этот раздел — исчерпывающий справочник по всем ключам, которые понимает игровая логика (QuakeC) стандартного мода FTEQW, с описанием их формата, единиц измерения и влияния на поведение сущности.

Материал сгруппирован по классам сущностей ([`classname`](01-worldspawn-common-keys.md#classname)), чтобы можно было быстро найти нужный набор ключей при расстановке сущностей в редакторе карт (например, TrenchBroom, Radiant или встроенном редакторе FTEQW).

## Категории

- [Общие ключи, worldspawn и глобальные настройки уровня](./01-worldspawn-common-keys.md)
- [Свет и освещение (light, light_*)](./02-light-entity-keys.md)
- [Триггеры и логические сущности (trigger_*, path_corner, target_*)](./03-trigger-logic-keys.md)
- [Двери, платформы и подвижная геометрия (func_*)](./04-func-brush-entity-keys.md)
- [Монстры, NPC и точки появления игрока (monster_*, info_player_*)](./05-monster-player-keys.md)
- [Предметы и оружие (item_*, weapon_*)](./06-item-weapon-keys.md)

## Как читать эти статьи

Каждый ключ описывается по единому шаблону: имя ключа, тип значения (число, строка, вектор), список сущностей, к которым он применим, подробное описание логики и готовые к использованию примеры фрагментов `.map`/`.ent`-файла.

## Смежные разделы

- [Внешние файлы сущностей карты (.ent)](../03-maps-levels-terrain/external-entity-files-ent.md)
- [Серверная игровая логика (SSQC)](../16-quakec-scripting/server-side-quakec-ssqc.md)
- [Справочник встроенных функций QuakeC](../37-quakec-builtins-reference/README.md)
