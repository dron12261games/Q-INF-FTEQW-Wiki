# Ник, скин и командная принадлежность игрока (userinfo)

> [⬅ Предыдущая страница](../19-config-console/key-bindings-input-devices.md) | [Следующая страница ➡](../21-localization/ui-translation-files.md)

> [⬅ Вернуться к оглавлению вики](../README.md)

## Что это даёт геймдизайнеру

Каждый игрок в многопользовательской игре хранит небольшой набор личных настроек, которые автоматически становятся известны серверу и остальным игрокам, — отображаемое имя (ник), выбранный скин (модель/раскраску персонажа), а также цвет верхней и нижней части модели, используемый, в частности, для обозначения команды в командных режимах. Игровая логика может свободно читать эти данные о любом игроке (например, чтобы отобразить нужный цвет на индикаторе команды или применить правильный скин при появлении персонажа), а также добавлять к ним собственные произвольные поля для нужд конкретного мода.

---

## Интерфейс настройки

Игрок задаёт свои личные данные либо через настройки в меню (которые сохраняются как обычные переменные конфигурации — [`name`](../38-cvars-reference/07-system-misc-cvars.md#name), [`topcolor`](../38-cvars-reference/07-system-misc-cvars.md#topcolor), [`bottomcolor`](../38-cvars-reference/07-system-misc-cvars.md#bottomcolor), [`skin`](../39-entity-keys-reference/01-worldspawn-common-keys.md#skin)), либо напрямую консольной командой:

```
setinfo <key> <value>
```

Например, [`setinfo name "PlayerNick"`](../44-cli-commands-reference/02-client-ui-commands.md#setinfo) меняет отображаемое имя, а [`setinfo skin "red_ranger"`](../44-cli-commands-reference/02-client-ui-commands.md#setinfo) — выбранный скин персонажа. Часть ключей (например, цвет и ник) можно менять и просто присвоив значение соответствующей переменной ([`name`](../38-cvars-reference/07-system-misc-cvars.md#name), [`topcolor`](../38-cvars-reference/07-system-misc-cvars.md#topcolor), [`bottomcolor`](../38-cvars-reference/07-system-misc-cvars.md#bottomcolor)) — движок сам синхронизирует изменение со всеми участниками игры.

Игровая логика (как серверная, так и клиентская) может прочитать любое из этих полей о любом игроке встроенной функцией **[`infokey(e, key)`](../37-quakec-builtins-reference/03-entity-world-builtins.md#infokey)**: если в качестве объекта указан `world`, функция ищет ключ среди общих серверных данных ([`serverinfo`](../44-cli-commands-reference/02-client-ui-commands.md#serverinfo)/[`localinfo`](../44-cli-commands-reference/04-server-multiplayer-commands.md#localinfo), см. также [«Настраиваемые переменные движка»](../19-config-console/cvars-engine-variables.md)); если указан конкретный игрок — среди личных данных именно этого игрока ([`name`](../38-cvars-reference/07-system-misc-cvars.md#name), [`topcolor`](../38-cvars-reference/07-system-misc-cvars.md#topcolor), [`bottomcolor`](../38-cvars-reference/07-system-misc-cvars.md#bottomcolor), [`skin`](../38-cvars-reference/07-system-misc-cvars.md#skin), [`team`](../38-cvars-reference/07-system-misc-cvars.md#team), а также служебные данные вроде пинга или времени входа в игру). Есть и несколько отдельных особых ключей, формально не входящих в обычный набор личных данных, но тоже доступных через [`infokey`](../37-quakec-builtins-reference/03-entity-world-builtins.md#infokey) — например, IP-адрес игрока. Мод не ограничен стандартным набором полей — командой [`setinfo`](../44-cli-commands-reference/02-client-ui-commands.md#setinfo) можно так же установить и прочитать любой собственный, придуманный модом ключ (например, выбранный игроком класс персонажа или уровень сложности), что превращает этот механизм в простой способ передать небольшой кусочек личных настроек игрока от клиента к серверу и другим игрокам без написания сетевого кода вручную.

Есть ограничения по объёму (сервер может ограничить количество ключей и общий размер личных данных одного игрока, чтобы не позволить забить канал связи), поэтому механизм подходит именно для небольших, редко меняющихся настроек, а не для потоковой передачи данных.

### Встроенные ключи личных данных (userinfo) движка

Помимо [`name`](../38-cvars-reference/07-system-misc-cvars.md#name)/[`skin`](../38-cvars-reference/07-system-misc-cvars.md#skin)/[`topcolor`](../38-cvars-reference/07-system-misc-cvars.md#topcolor)/[`bottomcolor`](../38-cvars-reference/07-system-misc-cvars.md#bottomcolor)/[`team`](../38-cvars-reference/07-system-misc-cvars.md#team), движок сам заводит и автоматически синхронизирует ещё целый ряд стандартных полей — любая переменная, зарегистрированная с внутренним флагом «userinfo», автоматически появляется в личных данных игрока при её изменении и передаётся серверу без какого-либо дополнительного кода игровой логики:

| Ключ (cvar) | Назначение |
| :--- | :--- |
| [`name`](../38-cvars-reference/07-system-misc-cvars.md#name) | Отображаемое имя игрока. |
| [`team`](../38-cvars-reference/07-system-misc-cvars.md#team) | Название команды игрока (используется многими командными модами наравне с [`bottomcolor`](../38-cvars-reference/07-system-misc-cvars.md#bottomcolor)). |
| [`skin`](../38-cvars-reference/07-system-misc-cvars.md#skin) | Имя выбранного скина модели персонажа. |
| [`model`](../39-entity-keys-reference/01-worldspawn-common-keys.md#model) | Имя выбранной модели персонажа (для модов, поддерживающих выбор модели, а не только скина). |
| [`topcolor`](../38-cvars-reference/07-system-misc-cvars.md#topcolor) / [`bottomcolor`](../38-cvars-reference/07-system-misc-cvars.md#bottomcolor) | Числовой код цвета верхней/нижней части модели персонажа (0-13, по классической палитре Quake). |
| [`rate`](../38-cvars-reference/04-network-server-cvars.md#rate) | Желаемая пропускная способность канала на приём игровых данных (байт/сек) — сервер учитывает её при формировании обновлений для этого клиента. |
| [`drate`](../38-cvars-reference/04-network-server-cvars.md#drate) | Желаемая пропускная способность при скачивании файлов с сервера. |
| [`spectator`](../38-cvars-reference/07-system-misc-cvars.md#spectator) | Признак того, что клиент подключился как наблюдатель, а не активный игрок. |
| [`password`](../38-cvars-reference/04-network-server-cvars.md#password) | Пароль для входа на защищённый сервер или в защищённую учётную запись — передаётся как обычное поле личных данных. |
| [`noaim`](../38-cvars-reference/07-system-misc-cvars.md#noaim) | Отключает серверный автоприцел (auto-[aim](../37-quakec-builtins-reference/03-entity-world-builtins.md#aim)) для этого игрока, если сервер вообще его поддерживает. |
| [`msg`](../38-cvars-reference/07-system-misc-cvars.md#msg) | Уровень фильтрации входящих текстовых сообщений сервера (0=только подбор предметов, 1=+сообщения о смертях, 2=+критические сообщения, 3=+чат). |
| [`hand`](../38-cvars-reference/07-system-misc-cvars.md#hand) | Из какой руки персонажа должно вестись оружие с точки зрения игровой логики (0=правая, 1=левая, 2=по центру/грудь). |
| [`cl_playerclass`](../38-cvars-reference/07-system-misc-cvars.md#cl_playerclass) | Выбранный класс персонажа (используется, например, в Hexen II). |
| [`lang`](../38-cvars-reference/07-system-misc-cvars.md#lang) | Код языка интерфейса игрока (см. [«Файлы перевода интерфейса»](../21-localization/ui-translation-files.md)) — доступен игровой логике через [`infokey`](../37-quakec-builtins-reference/03-entity-world-builtins.md#infokey), что позволяет мод-стороне присылать игроку тексты на предпочитаемом им языке. |

### Команды для массового управления личными данными

| Команда | Назначение |
| :--- | :--- |
| [`setinfo <key> <value>`](../44-cli-commands-reference/02-client-ui-commands.md#setinfo) | Установить (или создать) одно конкретное поле личных данных. |
| [`fullinfo <string>`](../44-cli-commands-reference/02-client-ui-commands.md#fullinfo) | Задать сразу весь набор личных данных одной строкой в специальном формате `\key1\value1\key2\value2\...` — используется в основном самим движком при подключении к серверу, но доступна и как явная команда. |

---

## Примеры

- Игрок в настройках выбирает синий цвет верхней части модели — соответствующее значение автоматически передаётся всем остальным игрокам и серверу через [`topcolor`](../38-cvars-reference/07-system-misc-cvars.md#topcolor).
- Командный мод при появлении игрока на карте читает его [`bottomcolor`](../38-cvars-reference/07-system-misc-cvars.md#bottomcolor), чтобы определить, к какой команде он принадлежит, и подсветить его имя соответствующим цветом на табло.
- Ролевой мод добавляет собственный ключ личных данных `class` через [`setinfo class "mage"`](../44-cli-commands-reference/02-client-ui-commands.md#setinfo), чтобы сервер знал выбранный игроком класс персонажа ещё до входа в игру.

---

## Смежные страницы

- [Настраиваемые переменные движка (cvar)](../19-config-console/cvars-engine-variables.md)
- [Игровая логика: язык QuakeC](../README.md#игровая-логика-язык-quakec)
- [Файлы перевода интерфейса](../21-localization/ui-translation-files.md)

> [⬅ Предыдущая страница](../19-config-console/key-bindings-input-devices.md) | [Следующая страница ➡](../21-localization/ui-translation-files.md)

> [⬅ Вернуться к оглавлению вики](../README.md)