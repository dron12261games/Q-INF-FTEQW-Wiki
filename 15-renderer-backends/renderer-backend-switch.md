# Переключение графического бэкенда

> [⬅ Вернуться к оглавлению вики](../README.md)

> Раздел: [Выбор движка рендеринга](./README.md)

## Что это даёт геймдизайнеру

Движок умеет запускать несколько графических backend'ов. Для автора контента это значит, что материалы и модели обычно остаются одними и теми же, а игрок или сборка выбирает наиболее подходящий путь вывода изображения под конкретную платформу.

## Интерфейс настройки

Выбор делается через **[`vid_renderer`](../38-cvars-reference/01-video-rendering-cvars.md#vid_renderer)**. Фактический список доступных вариантов зависит от того, что реально собрано в текущем бинарнике; самый надёжный способ посмотреть его — `echo $_vid_renderer_opts`.

| Значение | Что означает |
|---|---|
| `gl` | основной OpenGL-путь |
| `egl` | вариант OpenGL ES, если он есть в сборке |
| `vk` | Vulkan backend, если он собран; экспериментальные оговорки вынесены в [отдельную legacy-страницу](../43-legacy-unused-features/experimental-d3d11-vulkan.md) |
| `d3d11` | Direct3D 11 backend, если он собран; краткие оговорки см. на [legacy-странице](../43-legacy-unused-features/experimental-d3d11-vulkan.md) |
| `d3d8` | старый Direct3D 8 путь, если он собран |
| `d3d9` | legacy-путь; см. [software/D3D9 renderer](../43-legacy-unused-features/software-and-d3d9-renderer.md) |
| `sw` / `software` / `SoftRast` | отдельный software renderer; см. [software/D3D9 renderer](../43-legacy-unused-features/software-and-d3d9-renderer.md) |
| `d3d11 warp` | software-fallback через D3D11; подробности вынесены в [software/D3D9 renderer](../43-legacy-unused-features/software-and-d3d9-renderer.md) |
| `headless` | запуск без вывода изображения |
| `sv` | выделенный сервер без клиентского рендеринга |
| [`random`](../37-quakec-builtins-reference/01-math-vector-builtins.md#random) | случайно выбирает один из реально доступных графических backend'ов и удобен для smoke-тестов совместимости |
| *(пусто, значение не задано)* | движок сам выбирает наиболее приоритетный доступный backend |

Переменная имеет видеолатч и обычно применяется через **`vid_restart`**. Команда **`setrenderer <строка>`** сразу перестраивает видеоподсистему и, в отличие от прямой записи в `vid_renderer`, не сохраняет `headless` в cvar.

> Подробные технические замечания про legacy software/D3D9 и про экспериментальные особенности D3D11/Vulkan оставлены в разделе [legacy / unused features](../43-legacy-unused-features/README.md), чтобы основной раздел не дублировал редкие или нестандартные конфигурации.

## Примеры

- Для максимально предсказуемого поведения пользователь явно задаёт `vid_renderer "gl"` и выполняет `vid_restart`.
- Автоматизированный сервер, которому не нужно окно, запускается с `vid_renderer "headless"`.
- Если сборка содержит Vulkan backend, пользователь может попробовать `vid_renderer "vk"`.
- Для быстрых smoke-тестов совместимости можно использовать `vid_renderer "random"`.

## Смежные страницы

- [Язык описания материалов (в стиле Quake III, .shader)](../08-materials-shaders/shader-script-language.md)
- [Параметры запуска игры](../19-config-console/startup-parameters.md)
- [Настраиваемые переменные движка (cvar)](../19-config-console/cvars-engine-variables.md)
