# Менеджеры пакетов

## Нумерация версий ПО

В программе, состоящей из нескольких модулей (программных библиотек, пакетов), подключение этих модулей может происходить следующими способами:

* статическое связывание, которое осуществляется на этапе компиляции;
* динамическое связывание, происходящее на этапе выполнения программы.

Как программа, так и подключаемые к ней модули, обычно существуют в нескольких версиях.

Существуют различные схемы нумерации версий:

* последовательные значения;
* дата выпуска;
* хеш-сумма содержимого;
* различные экзотические схемы (например, последовательные цифры числа $\pi$ или $e$).

Проблема **ада зависимостей** (dependency hell) состоит в наличии конфликтных ситуаций между зависимостями различных модулей. Например, в системном каталоге библиотек может быть только одна версия библиотеки $X$, при этом одна программа требует $X$ в версии $A$, а другая – в несовместимой с $A$ версии $B$.

Для управления зависимостями между модулями необходима строгая и единая система нумерации версий, определяющая совместимые и несовместимые сочетания модулей.

## Семантическая нумерация версий

В спецификации SemVer (semantic versioning) версия состоит из следующих основных компонентов (см. @fig:pm3):

1. мажорная часть, означающая несовместимые с предыдущими версиями изменения;
1. минорная часть, означающая, добавление обратно совместимой функциональности;
1. патч, к которому относятся обратно совместимые исправления ошибок.

Дополнительные обозначения для предрелизных, билд и метаданных возможны как дополнения к мажорная.минорная.патч формату.

![Формат версии по спецификации SemVer](pm3.svg){#fig:pm3}

Сравнение двух версий осуществляется слева направо, до определения первого расхождения. Таким образом устанавливается порядок между версиями, а также могут быть введены интервалы версий.

С помощью символа `^` указываются совместимый интервал версий. Например `~1.2.3` означает `>=1.2.3` и `<2.0.0`. Символ `~` определяет близкие друг к другу версии. Например `~1.2.3` означает `>=1.2.3` и `<1.3.0`. 

## Управление пакетами

Менеджер пакетов предназначен для автоматического управления установкой, настройкой и обновлением специального вида модулей, называемых пакетами. 

Менеджеры пакетов бывают следующих видов:

1. уровня ОС (например, RPM в Linux);
1. уровня языка программирования (например, npm для JavaScript);
1. уровня приложения (например, Package Control для редактора Sublime Text, управление плагинами для среды Eclipse).

**Пакет** содержит в некотором файловом формате программный код и метаданные. К **метаданным** относятся:

* указание авторства, версии и описание пакета;
* список зависимостей пакета;
* хеш-значение содержимого пакета.

Для хранения пакетов используется **репозиторий**. Репозитории делятся на локальные, располагающиеся на компьютере пользователя и удаленные, размещаемые на сервере, предназначенном работы с конкретным менеджером пакетов.

## Менеджер пакетов apk

Менеджер пакетов apk входит в дистрибутив Alpine ОС Linux.

Работа c apk осуществляется из командной строки. Для добавления нового пакета необходимо использовать опцию add:

```default
/ # apk add python3
fetch https://dl-cdn.alpinelinux.org/alpine/v3.14/main/x86_64/
APKINDEX.tar.gz
fetch https://dl-cdn.alpinelinux.org/alpine/v3.14/community/x86_64/
APKINDEX.tar.gz
(1/13) Installing libbz2 (1.0.8-r1)
(2/13) Installing expat (2.4.1-r0)
(3/13) Installing libffi (3.3-r2)
(4/13) Installing gdbm (1.19-r0)
(5/13) Installing xz-libs (5.2.5-r0)
(6/13) Installing libgcc (10.3.1_git20210424-r2)
(7/13) Installing libstdc++ (10.3.1_git20210424-r2)
(8/13) Installing mpdecimal (2.5.1-r1)
(9/13) Installing ncurses-terminfo-base (6.2_p20210612-r0)
(10/13) Installing ncurses-libs (6.2_p20210612-r0)
(11/13) Installing readline (8.1.0-r0)
(12/13) Installing sqlite-libs (3.35.5-r0)
(13/13) Installing python3 (3.9.5-r1)
Executing busybox-1.33.1-r3.trigger
OK: 56 MiB in 27 packages
```

В первую очередь менеджером пакетов были загружены списки доступных пакетов из удаленного репозитория. В архиве APKINDEX.tar.gz можно найти информацию по устанавливаемому пакету и его зависимостям:

```default
C:Q1A2S5Zfy6pdKftVzGVhkE5s9r1f4=
P:python3
V:3.9.5-r1
A:x86_64
S:13405337
I:47747072
T:A high-level scripting language
U:https://www.python.org/
L:PSF-2.0
o:python3
m:Natanael Copa <ncopa@alpinelinux.org>
t:1620852262
c:2d63700fe78744e22c497d2cf7f8610828f00544
D:so:libbz2.so.1 so:libc.musl-x86_64.so.1 so:libcrypto.so.1.1 so:libexpat.so.1 so:libffi.so.7 so:libgdbm.so.6 so:libgdbm_compat.so.4 so:liblzma.so.5 so:libmpdec.so.3 so:libncursesw.so.6 so:libpanelw.so.6 so:libreadline.so.8 so:libsqlite3.so.0 so:libssl.so.1.1 so:libz.so.1
p:so:libpython3.9.so.1.0=1.0 so:libpython3.so=0 cmd:2to3 cmd:2to3-3.9 cmd:pydoc3 cmd:pydoc3.9 cmd:python3 cmd:python3.9 py3.9:README.txt=3.9.5-r1
```
Здесь `P:` определяет имя пакета, а `V:` – его версию. С помощью `D:` в последних строках определяются зависимости, а с помощью `p:` – те модули, которые пакет предоставляет после своей установки.

## Менеджер пакетов apt

В дистрибутиве Ubuntu ОС Linux используется менеджер пакетов apt.

Для установки пакета используется команда apt с опцией install:

```default
apt install instead
```

В файле Packages, расположенном на удаленном репозитории, располагается информация о доступных для установке пакетах.

Пакет instead имеет следующее описание:

```default
Package: instead
Architecture: amd64
Version: 3.2.1-1
Priority: optional
Section: universe/games
Origin: Ubuntu
Maintainer: Ubuntu Developers <ubuntu-devel-discuss@lists.ubuntu.com>
Original-Maintainer: Sam Protsenko <joe.skb7@gmail.com>
Bugs: https://bugs.launchpad.net/ubuntu/+filebug
Installed-Size: 527
Depends: libc6 (>= 2.14), libgdk-pixbuf2.0-0 (>= 2.22.0), libglib2.0-0 (>= 2.12.0), libgtk2.0-0 (>= 2.8.0), liblua5.1-0, libsdl2-2.0-0 (>= 2.0.8), libsdl2-image-2.0-0 (>= 2.0.2), libsdl2-mixer-2.0-0 (>= 2.0.2), libsdl2-ttf-2.0-0 (>= 2.0.14), zlib1g (>= 1:1.1.4), instead-data (= 3.2.1-1)
Filename: pool/universe/i/instead/instead_3.2.1-1_amd64.deb
Size: 224840
MD5sum: ae8edb2974cece3ff42ce664bcba8485
SHA1: 8e4f27d806d5822e2364c26b6a77502e6e1aada1
SHA256: f0d20dd6d7e3de3c2981bfe3a6fbab0f0be8ecc7bfd3f71736508bee0fe063df
Homepage: https://instead-hub.github.io/
Description: Simple text adventures/visual novels engine
Description-md5: ef0040d4434ac942fb089e9e171d022f
```

Как можно заметить, анализируя строку `Depends:`, помимо системных библиотек пакет instead зависит также от пакета instead-data, у которого также имеется описание:

```default
Package: instead-data
Architecture: all
Version: 3.2.1-1
Priority: optional
Section: universe/games
Source: instead
Origin: Ubuntu
Maintainer: Ubuntu Developers <ubuntu-devel-discuss@lists.ubuntu.com>
Original-Maintainer: Sam Protsenko <joe.skb7@gmail.com>
Bugs: https://bugs.launchpad.net/ubuntu/+filebug
Installed-Size: 4092
Depends: fonts-liberation
Recommends: instead
Filename: pool/universe/i/instead/instead-data_3.2.1-1_all.deb
Size: 3681604
MD5sum: e244752081374392d024ab100648213a
SHA1: b560cbc4118b95b5633be48ed40b679652dc1322
SHA256: 06582dc02515a031297dc7ae2e280795f7052266dbb4aee1e2a8406d132ea2c8
Homepage: https://instead-hub.github.io/
Description: Data files for INSTEAD
Description-md5: abbaa9f2bdb5492dca18ea9558b57a9d
```

Зависимости пакета instead приведены в графе на @fig:pm1.

![Зависимости пакета instead](pm1.svg){#fig:pm1}

## Задача разрешения зависимостей пакетов

Задача разрешения зависимостей, которую решает менеджер пакетов, в общем виде относится к труднорешаемым и может быть определена следующим образом.

Дано:

1. Состояние локального репозитория, содержащего уже установленные пакеты.
1. Состояние удаленного репозитория, в котором находятся все доступные к установке пакеты.
1. Имя устанавливаемого пакета.

Необходимо получить план установки нового пакета с разрешением всех его зависимостей. При этом желателен такой вариант решения, который требует установки минимального числа новых пакетов.

Пример задачи разрешения зависимостей показан на @fig:pm2. Обратите внимание, что из всех возможных версий пакета menu, менеджеру пакетов необходимо выбрать 1.0.0, иначе не получится разрешить зависимости пакета icons.

![Пример задачи разрешения зависимостей пакетов](pm2.svg){#fig:pm2}

Для задачи разрешения зависимостей в менеджерах пакетов сегодня все чаще используются универсальные решатели NP-полных задач: SAT-решатели @tucker2007opium, решатели для задачи программирования в ограничениях @pubgrub и другие @abate2020dependency.




