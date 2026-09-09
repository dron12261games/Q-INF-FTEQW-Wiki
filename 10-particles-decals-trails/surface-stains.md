# Пятна и следы на поверхностях

> [⬅ Предыдущая страница](particle-quality-presets.md) | [Следующая страница ➡](../11-sky-water-fog/skybox-rotation.md)

> [⬅ Вернуться к оглавлению вики](../README.md)

## Что это даёт геймдизайнеру

Помимо самих частиц (искры, дым), движок умеет оставлять после эффекта **долговременный след прямо на текстуре поверхности** — например, подпалину от взрыва, кровавое пятно или след от пули, — окрашивая часть поверхности карты в цвет частицы в момент столкновения. В отличие от отдельных декалей-объектов, такое пятно как бы «впечатывается» непосредственно в саму поверхность и остаётся заметно дольше, придавая уровню ощущение следов от боя, накапливающихся по ходу игры.

---

## Интерфейс настройки

Эта возможность настраивается внутри описания эффекта частиц (см. [«Язык эффектов частиц»](./particle-script-language.md)) двумя способами:

- **[`stains <value>`](../41-particle-directives-reference/01-particle-effect-directives.md#stains)** — включает оставление пятна при каждом столкновении частицы с поверхностью; цвет пятна берётся из текущего цвета самой частицы в момент удара;
- **[`spawnstain <radius> <r> <g> <b>`](../41-particle-directives-reference/01-particle-effect-directives.md#spawnstain)** — создаёт пятно заданного радиуса и конкретного цвета сразу при появлении эффекта, независимо от того, сталкивается частица с чем-либо или нет (используется, например, для мгновенного пятна крови в точке попадания, а не только при разлёте брызг).

Также можно ограничить, на каких именно поверхностях допустимо появление отпечатков-декалей от частиц (см. [`type decal`](../41-particle-directives-reference/01-particle-effect-directives.md#type)/[`clippeddecal`](../41-particle-directives-reference/01-particle-effect-directives.md#clippeddecal) в общем языке описания эффектов) — например, разрешить кровавые декали только на определённых типах поверхностей карты.

---

## Инженерный справочник

#### Директивы .shader/.particles

| Директива | Синтаксис | Описание |
| :--- | :--- | :--- |
| [`stains`](../41-particle-directives-reference/01-particle-effect-directives.md#stains) | [`stains <value>`](../41-particle-directives-reference/01-particle-effect-directives.md#stains) | Включает оставление пятна при столкновении частицы с поверхностью, цвет берётся из цвета частицы в момент удара |
| [`spawnstain`](../41-particle-directives-reference/01-particle-effect-directives.md#spawnstain) | [`spawnstain <radius> <r> <g> <b>`](../41-particle-directives-reference/01-particle-effect-directives.md#spawnstain) | Создаёт пятно заданного радиуса и явно указанного цвета сразу при появлении эффекта |

#### Cvar

| Cvar | Сигнатура | Описание | По умолчанию |
| :--- | :--- | :--- | :--- |
| [`r_stains`](../38-cvars-reference/02-lighting-materials-cvars.md#r_stains) | [`r_stains(float, "0")`](../38-cvars-reference/02-lighting-materials-cvars.md#r_stains) | Общий переключатель-регулятор пятен на поверхностях уровня: `0` полностью отключает появление пятен, `1` — максимальная непрозрачность пятен | `0` |
| [`r_bloodstains`](../38-cvars-reference/01-video-rendering-cvars.md#r_bloodstains) | [`r_bloodstains(int, "1")`](../38-cvars-reference/01-video-rendering-cvars.md#r_bloodstains) | Отдельный переключатель именно для кровавых пятен (позволяет игроку отключить кровь отдельно от прочих пятен по личным/возрастным предпочтениям) | `1` |
| [`r_stainfadeammount`](../38-cvars-reference/01-video-rendering-cvars.md#r_stainfadeammount) | [`r_stainfadeammount(float, "1")`](../38-cvars-reference/01-video-rendering-cvars.md#r_stainfadeammount) | Множитель скорости исчезновения пятна со временем | `1` |
| [`r_stainfadetime`](../38-cvars-reference/01-video-rendering-cvars.md#r_stainfadetime) | [`r_stainfadetime(float, "1")`](../38-cvars-reference/01-video-rendering-cvars.md#r_stainfadetime) | Время, за которое пятно полностью исчезает с поверхности | `1` |

---

## Примеры

- Попадание пули оставляет на стене небольшую тёмную подпалину, использующую [`stains`](../41-particle-directives-reference/01-particle-effect-directives.md#stains), чтобы цвет отпечатка совпадал с цветом искры от рикошета.
- Ранение персонажа сразу создаёт пятно крови в точке попадания через [`spawnstain`](../41-particle-directives-reference/01-particle-effect-directives.md#spawnstain), не дожидаясь разлёта отдельных частиц брызг.

---

## Смежные страницы

- [Язык эффектов частиц](./particle-script-language.md)
- [Внешние патчи освещения (.lit/.lux)](../03-maps-levels-terrain/external-lighting-visibility-patches.md)

> [⬅ Предыдущая страница](particle-quality-presets.md) | [Следующая страница ➡](../11-sky-water-fog/skybox-rotation.md)

> [⬅ Вернуться к оглавлению вики](../README.md)