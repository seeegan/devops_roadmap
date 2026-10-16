### Что было до `systemd`
Чтобы разобраться чем `systemd` так хорошо, нужно разобраться что было до него и какие проблемы он решил.

До `systemd` в Linux был `SysV init`. Это `PID 1` - первый процесс ,который запускает ядро.

#### Как работал `SysV init`

1. Ядро запускает `/sbin/init`
2. `init` читает скрипты в `/etc/init.d/`.
3. Запускает их последовательно: сначала сеть, потом логи,  потом сервисы
4. Для каждого сервиса - `bash-скрипт` со `start`, `stop`, `restart`
5. Переходы между "уровнями" (`runlevels`) - через отдельные скрипты.
	- `runlevels` мы рассматривать не будем, и так слишком глубоко копаем.

##### Проблемы

- Медленная загрузка
- Нет зависимостей между сервисами
- Упал сервис - никто не поднимет
- Нет единого формата

`systemD` - решил всё это:
- Параллельный запуск
- Явные зависимости в `unit- файле`
- Автоперезапуск через `Restart=`
- Централизированные логи `journalctl`
- Единый формат - работает в любом дистрибутиве

`systemD` - это `PID 1`. Ядро запускает его первым. Всё остальное - дети `systemD`. Несмотря на то, что большинство современных дистрибутивов `Linux` используют именно `systemD` , `PID 1` почему-то все называют именно `init`. Так что если где-то услышите про `init` то знайте, это `systemD` просто называют его именно так.

### Что такое `unit`

`unit` - это любой объект, которым управляет `systemD`. Всё в `systemD` - это `unit`.

#### Основные типы `unit`

| Тип       | Расширение | Что описывает                      |
| --------- | ---------- | ---------------------------------- |
| `Service` | `.service` | Демон или разовая команда          |
| `Target`  | `.target`  | Группа `units` (аналог `runlevel`) |
| `Timer`   | `.timer`   | Расписание (замена `cron`)         |
| `Socket`  | `.socket`  | Сокет для активации                |
| `Mount`   | `.mount`   | Точка монтирования                 |
| `Path`    | `.path`    | Реакция на изменения файла         |
> 90% работы - `services`. Остальное по необходимости. 

Где лежат `unit- файлы`
- `/lib/systemd/system/` - от пакетов. **Не рекомендуется редактировать**
- `/etc/systemd/system` - твои. Приоритет выше
- `/run/systemd/system` - временные, генерируются системой
**Правило:** хочешь изменить unit от пакета — копируй его в `/etc/systemd/system/`и правь там. `/lib/` перезапишется при обновлении пакета.


### Структура `service unit`

`Service - файл` состоит из трёх секций:
```ini
[Unit]
Description=My Application
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/myapp
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Разберём каждую.

#### `[Unit]`
Секция `[Unit]` - метаданные и зависимости
```ini
[Unit]
Description=My Application
Documentation=https://example.com/docs
After=network.target
Wants=postgresql.service
Requires=redis.service
Before=nginx.service
```
Разбор:
- `Description` - человеческое описание. Видно что `systemctl status`.
- `Documentation` - ссылка на документацию
- `After=network.target` - запустить после сети. Только порядок, не зависимость.
- `Wants=postgresql.service` - **мягкая**   зависимость. Если `posgres` не запуститься - мой сервис всё равно запуститься
- `Requires=redis.service` - **жёсткая** зависимость. Без `redis` - не запускайся
- `Before=nginx.service` - мой сервис должен запуститься до `nginx`.

**Ключевая разница**:
`After` - только порядок. Не требует чтобы зависимость была запущена
`Wants` - Мягкая зависимость. Попробуй запустить, не не критично
`Requires` - Жесткая зависимость. Без неё - не стартуй

#### `[Service]`
`[Service]` - как запускать

```ini
[Service]
Type=simple
User=myapp
Group=myapp
WorkingDirectory=/opt/myapp
Environment="LOG_LEVEL=info"
EnvironmentFile=-/etc/myapp/env
ExecStart=/usr/bin/myapp --config /etc/myapp/config.yml
ExecReload=/bin/kill -HUP $MAINPID
ExecStop=/bin/kill -TERM $MAINPID
Restart=on-failure
RestartSec=5
LimitNOFILE=65536
MemoryMax=512M
```

Разбор:
- `Type` - тип сервиса (разберём позже)
- `User`, `Group` - от кого запускать. Критично для безопасности - не запускай всё под **root**
- `WorkingDirectory` - рабочая директория
- `EnviromentFile=-/path` - файл с переменными. `-` перед указанием пути означает **если файла нет - не ругайся**
- `ExecStart` - команда запуска. Обязательное поле. `--config /path` путь до файла конфигурации приложения
- `ExecReload` - что делать при `reload`. Соглашение - `SIGHUP` , как мы уже помним, `SIGHUP` (1) - "**терминал отключился**" или "**перечитай конфиг**"
- `ExecStop` - что делать при `stop`. Соглашение - `SIGTERM`. Если не указать - `systemd` сам пошлёт `SIGTERM`.
- `Restart` - когда перезапускать (разберём дальше)
- `RestartSec` - пауза перед перезапуском
- `LimitNOFILE` - лимит открытых файлов
- `MemoryMax` - лимит памяти через `cgroups`

#### `[Install]`

`[Install]` - это секция, которая описывает, что делать при `systemctl enable`. Обычно такой командой мы добавляем нужную нам программу которая заведена как `service` в автозапуск, то есть при перезапуске системы тот же `nginx` должен будет запуститься если мы писали `systemctl enable nginx.service`.

Секция `[Install]` не влияет на работу сервиса. Она влияет на **автозапуск**

Важное различие:
`systemctl start` - запустить сейчас. Секция `[Install]` не используется.
`systemctl enable` - включить автозапуск. Секция `[Install]` читается.

Если в нужном нам сервисе не прописана секция `[Install]` то его нельзя будет включить через `systemctl enable`. Он будет работать только вручную.

У `[Install]` есть 3 основных директивы

`WantedBy=` Включить в `target` как "**мягкий**" (попробовать запустить)
`RequiredBy=` Включить в `target` как "**жёсткий**" (обязательно)
`Alias=` Дополнительное имя для `unit`
**90% случаев — только** ``WantedBy=multi-user.target``

## Часть 4. Что такое targets

**Target** — группа unit'ов. Это **аналог runlevel** в SysV init.
**Идея:** «при загрузке в определённом режиме запусти эти units».
#### Основные `targets`

|Target|SysV аналог|Что значит|
|---|---|---|
|`poweroff.target`|0|Выключение|
|`rescue.target`|1|Однопользовательский|
|`multi-user.target`|3|Многопользовательский, без GUI|
|`graphical.target`|5|С GUI|
|`reboot.target`|6|Перезагрузка|

**На сервере** обычно `multi-user.target`. На десктопе — `graphical.target`.

### Управление targets
```shell
systemctl get-default                 # какой target по умолчанию
systemctl set-default multi-user.target   # переключить default
systemctl isolate rescue.target       # переключиться сейчас
systemctl list-dependencies multi-user.target  # что входит
```

#### Пробуем написать свой собственный `service`

Вместо полноценной программы у нас будет просто `sh-скрипт` который будет писать лог в файл.

```shell
#!/bin/bash
while true; do
	echo "$(date) тик-так" >> /home/seegan/tik-tak.log
	sleep 5
done
```

Проверим, работает ли наш скрипт:
```shell
chmod +x tik-tak.sh
/home/seegan/tik-tak.sh &
```
Запустим его в режиме `bg` чтобы можно было смотреть наш `log-файл`
![gif](../attachments/2026-10-1700-10-04-ezgif.com-effects.gif)
> Отлично, скрипт успешно отрабатывает и записывает в файл логи.

Теперь давайте напишем свой сервис и попробуем его потом запустить через `systemd`
Сервисы которые пишем мы, хранятся по пути `/etc/systed/system`
```ini
[Unit]                                                                    
Description= My tik-tak service                                            
[Service]                                                                 
Type=simple                                                               
User=seegan                                                               
Group=seegan                                                              
ExecStart=/home/seegan/tik-tak.sh                                         
ExecReload=/bin/kill -TERM $MAINPID                                       
ExecStop=/bin/kill -TERM $MAINPID                                             
Restart=on-failure                                                       
[Install]                                                                 
WantedBy=multi-user.target
```

Перечитаем все юниты `systemd` и проверим есть ли наш новый юнит в `systemd`
```shell
systemctl daemon-reload
systemctl status tik-tak
```
![](../attachments/Pasted%20image%2020261017003223.png)
>Появился, теперь можем попробовать его запустить.

```shell
systemctl start tik-tak.service
systemctl status tik-tak.service
```
![](../attachments/Pasted%20image%2020261017003346.png)

```shell
tail -f /home/seegan/tik-tak.log
```
![](../attachments/Pasted%20image%2020261017003504.png)
В файл записываются строки.


```shell
ps -eo pid,ppid,comm,stat | grep "tik-tak"
```
![](../attachments/Pasted%20image%2020261017003913.png)

А вот и наш процесс который был запущен через `systemD`

```shell
systemctl stop tik-tak.service
ps -eo pid,ppid,comm,stat | grep "tik-tak"
```
![](../attachments/Pasted%20image%2020261017004114.png)А теперь мы остановили наш сервис.

Стоит разобрать ещё типы сервисов.
## Все типы

### `Type=simple` (по умолчанию)

**Как работает:** systemd запускает `ExecStart` и **сразу считает сервис запущенным**. Даже если программа ещё не готова принимать запросы.

**Требование:** процесс работает **на переднем плане**, не форкается.

**Когда использовать:** современные приложения — Python, Node.js, Go. Nginx в контейнерах, Prometheus, Grafana.
```ini
[Service]
Type=simple
ExecStart=/usr/bin/myapp
```

**Плюсы:** просто, работает для большинства случаев.  
**Минусы:** systemd не знает, **когда** сервис реально готов. Он просто считает «запущен» после старта процесса.

### `Type=forking`

**Как работает:** systemd запускает `ExecStart`, ждёт, пока **родитель форкнется и завершится**. Тогда считает сервис запущенным.

**Требование:** программа **демонизируется** — делает `fork()`, родитель выходит, ребёнок продолжает работать. Обычно пишет pid-файл.

**Когда использовать:** старые демоны — nginx (master форкается), sshd, Apache prefork, PHP-FPM.

```ini
[Service]
Type=forking
PIDFile=/run/myapp.pid
ExecStart=/usr/bin/myapp
```

**Зачем `PIDFile`:** после форка родитель умирает. systemd теряет связь с главным процессом. PIDFile говорит: «главный процесс — вот с таким PID». Без PIDFile systemd может **не найти**главный процесс и подумать, что сервис не запустился.

**Плюсы:** работает со старыми демонами.  
**Минусы:** сложнее настраивать, нужен PIDFile, есть задержка.

### `Type=oneshot`

**Как работает:** systemd запускает `ExecStart`, **ждёт полного завершения**. Сервис считается успешным, если exit code = 0.

**Требование:** программа **завершается** — не демон, не цикл.

**Когда использовать:** разовые действия — применить миграции БД, создать директорию, настроить сеть.

```ini
[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/usr/local/bin/setup.sh
```

**`RemainAfterExit=yes`** — после завершения **считать сервис активным**. Иначе systemd покажет `inactive (dead)`, и зависимые сервисы решат, что зависимость не работает.

**Примеры:**

- `systemd-tmpfiles-setup.service` — создаёт временные директории.
- `e2scrub_reap.service` — проверяет ФС.

**Плюсы:** идеально для скриптов.  
**Минусы:** без `RemainAfterExit` сервис сразу считается «мёртвым».

### `Type=notify`

**Как работает:** сервис **сам сообщает** systemd, что готов принимать запросы. Через специальный механизм — `sd_notify()`.

**Требование:** программа **умеет** вызывать `sd_notify()`.

**Когда использовать:** современные сервисы — PostgreSQL 9.6+, systemd-networkd, systemd-journald, Docker.

```ini
[Service]
Type=notify
ExecStart=/usr/bin/myapp
NotifyAccess=main
```

**Почему это лучше `simple`:** systemd **точно знает**, когда сервис готов. Это критично для зависимостей — если nginx запускается после PostgreSQL, он должен ждать, пока PostgreSQL **реально готов**, а не просто «запущен процесс».

**Как работает под капотом:** сервис получает переменную окружения `NOTIFY_SOCKET` от systemd. Когда он готов — вызывает `sd_notify("READY=1")`. systemd читает сокет и понимает, что можно запускать зависимые сервисы.

**Плюсы:** точное определение готовности. Идеально для оркестрации.  
**Минусы:** требует поддержки в программе.

### `Type=dbus`

**Как работает:** сервис считается запущенным, когда **займёт имя в D-Bus** — шине обмена сообщениями.

**Требование:** сервис использует D-Bus (системный демон).

**Когда использовать:** системные сервисы, которые регистрируются в D-Bus. Например, `NetworkManager`, `bluetoothd`, `systemd-logind`.
```ini
[Service]
Type=dbus
BusName=org.freedesktop.NetworkManager
ExecStart=/usr/sbin/NetworkManager
```

**`BusName`** — какое имя должен занять сервис в D-Bus.

**Плюсы:** точная синхронизация.  
**Минусы:** только для D-Bus-сервисов.

### `Type=idle`

**Как работает:** как `simple`, но systemd **подождёт**, пока все остальные задачи запуска завершатся. Только потом запустит сервис.

**Когда использовать:** сервисы, которые **не должны задерживать загрузку**. Потому что их вывод откладывается до последнего.

**Редкий тип.** Практически не используется.

### `Type=exec`

**Как работает:** как `simple`, но systemd ждёт, пока `exec()` **успешно выполнится**. Если `ExecStart`не удалось — systemd **сразу** поймает ошибку, а не подумает, что сервис запущен.

**Разница с `simple`:** `simple` считает сервис запущенным сразу после fork. `exec` — после успешного exec.

**Когда использовать:** если важно поймать ошибку запуска — например, «бинарник не найден».

**Современный рекомендуемый тип.** В некоторых дистрибутивах по умолчанию `exec`, а не `simple`.