# Модели Half-Life (GoldSrc)

> [⬅ Предыдущая страница](external-editor-import-obj-gltf.md) | [Следующая страница ➡](skeletal-tags-attachment.md)

> [⬅ Вернуться к оглавлению вики](../README.md)

## Что это даёт геймдизайнеру

Движок действительно умеет напрямую читать **GoldSrc/Half-Life `.mdl`**: это даёт доступ к большому архиву старых моделей оружия, персонажей и монстров без обязательной предварительной конвертации.

---

## Интерфейс настройки

Файл кладётся в игровую папку как обычный `.mdl`; движок различает Quake MDL и Half-Life MDL по внутренней сигнатуре файла. В стандартной конфигурации сборки эта поддержка включена по умолчанию.

Дальше модель подключается как обычная skeletal-совместимая модель: доступны именованные последовательности, кости, attachment-точки и hitbox'ы, а управление идёт через общий набор `skel_*`, [`frameforname`](../37-quakec-builtins-reference/07-precache-resources-builtins.md#frameforname), [`gettagindex`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#gettagindex) и [`gettaginfo`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#gettaginfo), а не через отдельный «half-life-only» API.

Подробности — в [«Формат моделей Half-Life: ограничения поддержки»](../43-legacy-unused-features/halflife-model-format-caveats.md).
>
См. [«Игровая логика Half-Life: исключена из обычной сборки»](../43-legacy-unused-features/halflife-gameplay-code-removed.md).

---

## Примеры

- Мод переиспользует готовые viewmodel/weapon-модели из Half-Life как источник ассетов.
- Персонаж на GoldSrc-модели получает процедурный поворот верхней части тела через [`skel_find_bone`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_find_bone) и [`skel_premul_bones`](../37-quakec-builtins-reference/10-skeletal-model-builtins.md#skel_premul_bones), так же как и любая другая скелетная модель.

---

## Смежные страницы

- [Классические модели и спрайты Quake (MDL/SPR)](./classic-quake-models-mdl-spr.md)
- [Современные скелетные модели (IQM/MD5/DPM/ZYM)](./skeletal-models-iqm-md5-dpm-zym.md)
- [Формат моделей Half-Life: ограничения поддержки](../43-legacy-unused-features/halflife-model-format-caveats.md)

> [⬅ Предыдущая страница](external-editor-import-obj-gltf.md) | [Следующая страница ➡](skeletal-tags-attachment.md)

> [⬅ Вернуться к оглавлению вики](../README.md)