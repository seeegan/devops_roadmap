
В каждой системе есть самый главный пользователь root (корень). При входе на тачку мы используем какого-то пользователя

```bash
user@dnsserver:~$ pwd
/home/user
```

Мы находимся в домашней директории нашего пользователя, но можем подняться и выше, в корень /

```bash
user@dnsserver:~$ cd /
user@dnsserver:/$ pwd
/
```

А что если я скажу что этот корень мы можем поменять, для этого можно использовать команду `chroot`

>**Chroot** (от англ. change root — «сменить корень») — это операция в Unix-подобных системах, которая меняет видимый корневой каталог `/` для запущенного процесса и всех его дочерних процессов.

Будем создавать новый корень, для этого перейдем в директорию `/tmp` и создадим в ней директорию `seegan/`

```shell
user@dnsserver:/$ cd /tmp
user@dnsserver:/tmp$ mkdir seegan/
user@dnsserver:/tmp$ ls | grep "seegan"
seegan
```

`chroot` будем замещён другой программой, в нашем случае будем использовать shell, для этого нам нужно найти где находится shell
```shell
user@dnsserver:/tmp$ which sh
/usr/bin/sh
```

Теперь скопируем все вместе с поддиректориями в нашу новую директорию `seegan/`

```shell
user@dnsserver:/tmp$ cp --parents /usr/bin/sh seegan/
tree
.
└── usr
    └── bin
        └── sh
```

Но этого будет недостаточно,  если сейчас мы попробуем выполнить `chroot seegan/ sh` то получим ошибку, у нас не установлены необходимые зависимости, давай посмотрим какие зависимости нужны с помощью команды `ldd`


>Команда `ldd` в Linux показывает список разделяемых (динамических) библиотек, которые необходимы программе для запуска. Расшифровывается как _List Dynamic Dependencies_ (список динамических зависимостей).

```shell
user@dnsserver:/tmp/seegan$ ldd /usr/bin/sh
        linux-vdso.so.1 (0x00007ffda7138000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007a7d30000000)
        /lib64/ld-linux-x86-64.so.2 (0x00007a7d302bd000)
```

Нам понадобятся 2 и 3 строка

```shell
user@dnsserver:/tmp/seegan$ cp -L --parents /lib/x86_64-linux-gnu/libc.so.6 /lib64/ld-linux-x86-64.so.2 /tmp/seegan/
user@dnsserver:/tmp/seegan$ tree
.
├── lib
│   └── x86_64-linux-gnu
│       └── libc.so.6
├── lib64
│   └── ld-linux-x86-64.so.2
└── usr
    └── bin
        └── sh
```

Теперь попробуем сменить корень
```shell
root@dnsserver:/tmp# chroot seegan/ sh
# pwd
/
# cd /
# pwd
/
```
Как видно я пытался подняться выше и не смог, так как корень теперь это `/tmp/seegan/`. Большинство команд типо `ls` не будет, поэтому нам нужно будет сказать busybox.

```shell
curl -O https://github.com/xerta555/Busybox-Binaries/blob/master/busybox-x86_64
mv busybox-x86_64 busybox
chmod +x busybox
cp buxybox /tmp/seegan/bin
chroot seegan/ sh busybox

```
Скачали сам бинарник busybox, переименовали для удобства, дали ему права на execute, ну и скопировали в директорию для нового рута. Чтобы каждый раз не вводить абсюлютный путь для выполнения команды внутри нового рута нужно создать символьные ссылки.
```sh
/usr/bin/busybox --install -s
```

И так, рута мы сменили, комманды работают, давайте посмотрим какие процессы у нас сейчас есть 
```shell
~ # ps
PID   USER     TIME  COMMAND
ps: can't open '/proc': No such file or directory
```
Видим ошибку, потому что у нас нет "псевдофайловой системы `/proc`". Псевдофайловая она потому что у неё нет физического носителя, она представляет собой дерево каталогов процессов информация о которых передается от ядра.

Давай мы создаим её
```
~ # ls
bin    lib    lib64  proc   usr
```
Но нам этого будет мало, её нужно ещё и вмонтировать
```shell
mount -t proc none /proc
# none указываем потому что у proc нет физического носителя
```

И так, давай теперь посмотрим через ps наши процессы
```shell
~ # ps | head -10
PID   USER     TIME  COMMAND
    1 root      0:02 {systemd} /sbin/init
    2 root      0:00 [kthreadd]
    3 root      0:00 [pool_workqueue_]
    4 root      0:00 [kworker/R-rcu_g]
    5 root      0:00 [kworker/R-rcu_p]
    6 root      0:00 [kworker/R-slub_]
    7 root      0:00 [kworker/R-netns]
   11 root      0:01 [kworker/u2:0-ev]
   12 root      0:00 [kworker/R-mm_pe]
```
Вывод будет большим потому что мы изолировали лишь дерево каталогов, поэтому мы видим все процессы в том числе и на хостовой машине от которой мы по идее изолировались, поэтому этот способ нам не подоходит.

# Итог
`chroot` не подходит для полной изоляции хостовой машины от нашего "контейнера" так как мы сменили лишь дерево каталогов корня и всё.



