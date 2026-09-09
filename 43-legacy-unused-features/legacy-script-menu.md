﻿# Устаревшее меню на основе m_script

> [⬅ Предыдущая страница](unreliable-networking-features.md)

> [⬅ Вернуться к оглавлению вики](../README.md)

## Коротко

Ещё до появления полноценной логики меню на QuakeC (MenuQC — см. [«Логика игровых меню (MenuQC)»](../16-quakec-scripting/menu-quakec.md)) в движке появился более простой способ собирать собственные экраны меню — не программированием, а обычными консольными командами внутри конфигурационных файлов и alias-скриптов (алиасов, то есть именованных наборов команд). Эта возможность по-прежнему компилируется в обычную сборку движка, но она значительно менее гибкая, чем полноценная MenuQC, и годами не получала заметного развития — рассматривать её как основной инструмент создания интерфейса не стоит.

---

## Как это устроено

Отдельные консольные команды создают на экране элементы меню один за другим, начиная с команды, которая открывает новое меню и связывает с ним функцию-аналог обработчика («callback», код, который получает управление при выборе пункта или закрытии меню):

| Команда | Назначение |
|---|---|
| [`conmenu <alias_name>`](../44-cli-commands-reference/02-client-ui-commands.md#conmenu) | Открывает новый экран меню; последующие команды `menu*` добавляют элементы именно в него. Указанный алиас будет вызван с аргументом [`cancel`](../44-cli-commands-reference/04-server-multiplayer-commands.md#cancel), когда пользователь закроет меню. |
| [`menutext x y "text" "command"`](../44-cli-commands-reference/02-client-ui-commands.md#menutext) | Добавляет строку текста, при выборе которой выполняется указанная команда. |
| [`menutextbig x y "text" "command"`](../44-cli-commands-reference/02-client-ui-commands.md#menutextbig) | То же самое, но текстом увеличенного размера. |
| [`menupic x y "name_image"`](../44-cli-commands-reference/02-client-ui-commands.md#menupic) | Добавляет изображение. |
| [`menuedit x y "signature" "name_cvar"`](../44-cli-commands-reference/02-client-ui-commands.md#menuedit) | Добавляет текстовое поле ввода, привязанное к переменной движка (cvar). |
| [`menucheck x y "signature" "name_cvar" mask_bit`](../44-cli-commands-reference/02-client-ui-commands.md#menucheck) | Добавляет флажок (чекбокс), переключающий один бит в значении cvar. |
| [`menuslider x y "signature" "name_cvar" min max`](../44-cli-commands-reference/02-client-ui-commands.md#menuslider) | Добавляет ползунок, изменяющий числовое значение cvar в заданном диапазоне. |
| [`menubind x y "signature" "bind_command"`](../44-cli-commands-reference/02-client-ui-commands.md#menubind) | Добавляет строку назначения клавиши (bind) на определённое действие. |
| [`menucomboi`](../44-cli-commands-reference/02-client-ui-commands.md#menucomboi) / [`menucombos`](../44-cli-commands-reference/02-client-ui-commands.md#menucombos) | Добавляют выпадающий список вариантов, привязанный к cvar. |
| [`menuclear`](../44-cli-commands-reference/02-client-ui-commands.md#menuclear) | Закрывает текущее скриптовое меню. |

Пример учебного меню, целиком составленного из подобных команд внутри одного alias-блока:

```
alias MenuScriptTest
{
	alias menucallback
	{
		echo callback $option
		if (($option == "cancel") or ($option == "7"))
		{
			menuclear
		}
	}
	conmenu menucallback

	menutext 0 0 "Cool test menu"
	menucheck 0 8 "Bouncy sparks!" "r_bouncy_sparks"
	menuslider 0 16 "Forward speed" "cl_forwardspeed" 20 1000
	menuedit 0 30 "name" "name"
	menubind 0 48 "+use" "use"
}
```

---

## Почему не стоит строить на этом весь интерфейс мода

- Все элементы такого меню жёстко привязаны к cvar-переменным движка — им нельзя напрямую управлять игровой логикой мода (например, посчитать что-то в коде и тут же показать результат), в отличие от полноценной MenuQC, где экран меню — это обычный код с полным доступом к возможностям движка.
- Визуальные возможности ограничены заранее заданным набором элементов (текст, картинка, ползунок, флажок, поле ввода, список) без произвольной вёрстки, анимации или собственных виджетов.
- Эта система не развивается вместе с новыми возможностями движка — новые способы вывода (например, сложные многослойные интерфейсы или адаптация под разные разрешения экрана) в первую очередь появляются в MenuQC, а не здесь.

---

## Практический вывод

Для простых служебных задач (например, быстрое тестовое меню без написания кода) этот механизм по-прежнему можно использовать. Но для полноценного пользовательского интерфейса игры или мода стоит использовать MenuQC — см. [«Логика игровых меню (MenuQC)»](../16-quakec-scripting/menu-quakec.md).

---

## Смежные страницы

- [Логика игровых меню (MenuQC)](../16-quakec-scripting/menu-quakec.md)
- [Конфигурационные файлы и консоль](../README.md#конфигурационные-файлы-и-консоль)

> [⬅ Предыдущая страница](unreliable-networking-features.md)

> [⬅ Вернуться к оглавлению вики](../README.md)