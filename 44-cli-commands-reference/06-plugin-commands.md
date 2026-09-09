# Команды консоли: опциональные плагины

> [⬅ Предыдущая страница](05-filesystem-system-commands.md) | [Следующая страница ➡](07-fteqcc-command-line-parameters.md)

> [⬅ Вернуться к оглавлению вики](../README.md)
> [Индекс справочника команд и параметров](../README.md#команды-и-параметры-командной-строки)

Эти команды регистрируются не самим движком, а опциональными [плагинами FTEQW](../README.md#плагины-и-физические-движки) — динамически загружаемыми модулями. Команда доступна только если соответствующий плагин собран и явно подключён (см. `-noplugins` в [«Параметрах командной строки FTEQW»](./01-fteqw-startup-parameters.md) — при этом параметре плагины не загружаются вообще, и ни одна из команд ниже не будет работать).

## Плагин ezhud (расширенный HUD)

Полноценная альтернативная система расположения элементов интерфейса, отдельная от встроенного `hud`/`sbar` движка.

### show
`show <element>`

Показать элемент HUD по имени. 

---

### hide
`hide <element>`

Скрыть элемент HUD по имени.

---

### move
`move <element> <x> <y>`

Переместить элемент HUD в указанные координаты.

---

### place
`place <element>`

Начать интерактивное перетаскивание элемента мышью/клавиатурой.

---

### reset
`reset <element>`

Вернуть элемент HUD к положению по умолчанию.

---

### order
`order <element> <layer>`

Изменить порядок отрисовки (z-order) элемента HUD.

---

### togglehud
`togglehud`

Включить/выключить видимость всего HUD целиком.

---

### align
`align <element> <side>`

Привязать элемент HUD к краю экрана.

---

### hud_recalculate
`hud_recalculate`

Пересчитать расположение всех элементов HUD (например, после смены разрешения экрана).

---

### hud_export
`hud_export <file>`

Сохранить текущую раскладку HUD в файл.

---

### hud_editor
`hud_editor`

Открыть визуальный редактор раскладки HUD. 

---

### ezhud_nquake
`ezhud_nquake`

Переключить пресет HUD в стиль клиента nQuake. 

---

## Плагин hud (альтернативный статус-бар)

Более простая, встроенная в отдельный плагин система статус-бара, отличная от `ezhud`.

### hud_edit / sbar_edit
`hud_edit` (синоним `sbar_edit`)

Войти в интерактивный редактор расположения статус-бара. 

---

### hud_save / sbar_save
`hud_save <file>` (синоним `sbar_save`)

Сохранить текущую раскладку статус-бара.

---

### hud_load / sbar_load
`hud_load <file>` (синоним `sbar_load`)

Загрузить раскладку статус-бара из файла.

---

### hud_defaults / sbar_defaults
`hud_defaults` (синоним `sbar_defaults`)

Сбросить раскладку статус-бара к значениям по умолчанию.

---

### hud / sbar
`hud` (синоним `sbar`)

Вывести в консоль текущее состояние/раскладку статус-бара.

---

### tinfo
`tinfo`

Показать отладочную информацию о командном (teamplay) статус-баре.

---

## Плагин ezscript (совместимость со старыми конфигами)

Этот плагин не добавляет новую функциональность — он регистрирует старые имена cvar из клиентов линейки ezQuake/FuhQuake как псевдо-команды, чтобы конфиги, писавшиеся под эти клиенты, не выдавали `unknown command`, а прозрачно перенаправляли значение в соответствующий актуальный cvar FTEQW. 

Полный список псевдонимов: `loadsky`, `r_skyname`, `r_skycolor`, `fps_sky`, `fps_skycolor`, `gl_consolefont`, `gl_bounceparticles`, `gl_loadlitfiles`, `gl_weather_rain`, `r_farclip`, `vid_vsync`, `gl_lighting_vertex`, `scr_conback`, `cl_bonusflash`, `cl_fakeshaft`, `r_floorcolor`, `r_wallcolor`, `sw_gamma`, `sw_contrast`, `s_nosound`, `tp_triggers`, `teamskin`, `enemyskin`, `scr_menualpha`, `cl_predictPlayers`, `sshot_format`, `cl_solidPlayers`, `fps_muzzleflash`, `in_m_mwhook`, `bgmvolume`, `cl_physfps`, `vid_colorbits`, `vid_customheight`, `vid_cumstomwidth`, `vid_hwgammacontrol`, `sv_maxpitch`, `sv_minpitch`, `sv_zombietime`.

Использование: `<name> <value>` — присваивает `<value>` соответствующему актуальному cvar движка и выводит уведомление в консоль.

---

## Плагин emailnot (уведомления по почте)

### imapaccount
`imapaccount <params_account_record>`

Настроить учётную запись IMAP для проверки почты прямо из движка. 

---

### pop3account
`pop3account <params_account_record>`

Настроить учётную запись POP3 для проверки почты.

---

## Плагин spaceinv (мини-игра)

### spaceinv
`spaceinv`

Запустить встроенную мини-игру Space Invaders поверх движка (пасхалка/демонстрация плагинов). 

---

## Недоступные и закомментированные команды плагинов

### startx
`startx`

Статус: регистрация команды отключена. 

> [⬅ Предыдущая страница](05-filesystem-system-commands.md) | [Следующая страница ➡](07-fteqcc-command-line-parameters.md)

> [⬅ Вернуться к оглавлению вики](../README.md)