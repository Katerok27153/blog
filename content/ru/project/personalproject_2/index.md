---
title: Сравнительный анализ файловых систем (FAT, NTFS, ext4)
summary: Архитектура компьютера
tags:
  - Deep Learning
date: '2024-05-9T00:00:00Z'

# Optional external URL for project (replaces project detail page).
external_link: ''

image:
  caption: Photo by rawpixel on Unsplash
  focal_point: Smart

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
---

## Введение

### Что такое файловая система?

Каждому пользователю компьютерных электронных устройств редко, но приходится сталкиваться с таким понятием, как «выбор файловой системы». Чаще всего это происходит при необходимости форматирования внешних накопителей (флешек, microSD), установке операционных систем, восстановлении данных на
проблемных носителях, в том числе жестких дисках. Пользователям Windows предлагается выбрать тип файловой системы, FAT32 или NTFS, и способ форматирования (быстрое/глубокое). Дополнительно можно установить размер кластера. При использовании ОС Linux и macOS названия файловых систем могут
отличаться. Возникает логичный вопрос: что же такое файловая система и в чем ее
предназначение? Основная функция ВЗУ – это хранение информации. Информация на внешних
носителях информации храниться в виде файлов. Упорядочиванием и обработкой этих файлов занимается файловая система. Файловая система (англ. file system) — порядок, определяющий способ
организации, хранения и именования данных во внешней памяти, и обеспечивающий пользователю удобный интерфейс при работе с такими данными. Простыми словами файловая система - это система хранения файлов и организации каталогов. От файловой системы зависит, как файлы будут кодироваться, храниться на диске и читаться компьютером. И, если диск — это массив кластеров, то файловая система — это инструкция по заполнению этих кластеров информацией. У каждой операционной системы свой тип организации
файлов, то есть своя файловая система.

### Что такое файл, кластеры и каталог?

Объектами файловой системы являются файлы и каталоги. С точки зрения пользователя, файл - это единица хранения логически связанной информации; текстовой, графической, звуковой, видео. С точки зрения организации хранения данных на диске, файл - это цепочка связанных между собой кластеров. Каждый
файл, в зависимости от размера, получает для хранения своих данных один или несколько кластеров, которые могут располагаться подряд, один за другим или же в разброс по всему диску. Обычно кластер весит несколько байт, а сколько конкретно — зависит от размера диска и от настроек. Когда человек настраивает файловую систему, он может выбрать и размер кластера:

- если кластер сделать меньше рекомендованного, на накопитель
поместится больше файлов. Ведь тогда не будет ситуаций, когда кластер
фактически заполнен лишь частично — все пространство окажется
задействовано;
- если увеличить размер кластера, доступ к файлам будет быстрее. Файлы
будут делиться на меньшее количество частей. Компьютеру придется
обращаться к меньшему количеству кластеров — это увеличит скорость.

Файловая система управляет кластерами, тем, как они будут записываться и
храниться, как между ними будут организованы связи. Она же разделяет и
распределяет файлы по кластерам, управляет записью и чтением.
Каталог (директория, справочник, папка) — объект в файловой системе,
упрощающий организацию файлов. Каталог реализован как специальный файл, где
регистрируется информация о других файлах и каталогах на носителе информации —
имена, физическое месторасположение, атрибуты и т. д. Принято говорить, что
каталог содержит в себе файлы или другие каталоги, хотя в действительности он
только ссылается на них, физическое размещение данных на диске обычно никак не
связано с размещением каталога.

### Основные функции файловой системы

Чтобы знать, что где хранится, у системы есть файловая таблица: там приведена
информация обо всех файлах. Файловая система определяет, как организовать эту
таблицу. Способы разные в зависимости от ОС, поэтому у разных операционных
систем различаются и файловые системы.

Также файловая система:
- определяет, какого размера будут кластеры — блоки информации, на которые
делится файл;
- соединяет «кусочки» информации из разных кластеров в единые файлы;
- отслеживает, какие ячейки памяти сейчас свободны, заняты или недоступны;
- обеспечивает прикладным программам доступ к файлам;
- Обеспечение работы с файлами и данными (создание, удаление, изменение
атрибутов, чтение и запись, поиск и т. д.)
- следит за целостностью и защищенностью файлов, создает точки
восстановления;
- хранит информацию о файлах, в том числе название, размер и дату создания.

## Основная часть

### Файловая система FAT

FAT (англ. File Allocation Table «таблица размещения файлов») — классическая
архитектура файловой системы, которая из-за своей простоты всё ещё широко
применяется для флеш-накопителей. Используется в дискетах, картах памяти и
некоторых других носителях информации. Ранее находила применение и на жёстких
дисках.

#### История создания FAT

Разработана Биллом Гейтсом и Марком МакДональдом в 1976—1977 годах.
Использовалась в качестве основной файловой системы в операционных
системах семейств MS-DOS и Windows 9x. Файловая система FAT представляет собой
таблицу размещения файлов, в которой указываются:
- непосредственно адреса участков логического диска, предназначенные
для размещения файлов;
- свободные области дискового пространства;
- дефектные области диска.
Файлы обычно организованы в цепочки кластеров на диске. Такая структура
обусловлена корневой директорией, в которой хранится информация о каждом файле,
включая его длину и расположение на диске.

#### Структура FAT

1. Boot Sector (загрузочный сектор)
Boot Sector хранит информацию о файловой системе, включая код,
необходимый для начала загрузки операционной системы, параметры BIOS и
другие важные данные. Он играет ключевую роль при запуске компьютера и
начале работы с файловой системой.
2. File Allocation Table (таблица выделения файлов)
File Allocation Table содержит записи о файлах и свободных кластерах на диске,
определяя, какие кластеры заняты и какие доступны для записи новых данных.
FAT является важной частью файловой системы, управляющей распределением
и размещением файлов на диске.
3. Root Directory (корневой каталог)
Root Directory хранит записи о каждом файле и подкаталоге на диске. Это
своего рода таблица содержимого диска, позволяющая операционной системе
находить файлы и каталоги.
4. Data Area (область данных)
Data Area представляет собой фактическое хранилище файлов и каталогов на
диске. Здесь размещаются сами данные, которые пользователь сохраняет в
файловой системе. Область данных включает в себя кластеры, которые
объединяются для хранения файлов.

Файловая система FAT всегда заполняет свободное место на диске
последовательно от начала к концу. При создании нового файла или увеличении уже
существующего она ищет самый первый свободный кластер в таблице размещения
файлов. Если в процессе работы одни файлы были удалены, а другие изменились в
размере, то появляющиеся в результате пустые кластеры будут рассеяны по диску.
Если кластеры, содержащие данные файла, расположены не подряд, то файл
оказывается фрагментированным.

#### Разные версии FAT

Существует четыре версии FAT — FAT12, FAT16, FAT32 и exFAT (FAT64).
После появления жестких дисков большого объема (в те времена большими считались
диски размером 10-20 Мбайт) количество кластеров увеличилось, и 12 разрядов стало
недостаточно для хранения их номеров. Был разработан новый 16-разрядный формат
таблицы размещения файлов, где для хранения номера одного кластера выделялось
два байта. Старая файловая система, разработанная для дискет, стала называться FAT12, а новая - FAT-16.
Увеличенная в размерах таблица FAT-16 перестала помещаться в одном секторе,
однако при больших объемах диска этот недостаток не играл существенной роли. Попрежнему для страховки на диске хранилось две копии таблицы FAT.
Однако когда объем диска стал измеряться в сотнях Мбайт и даже в гигабайтах,
файловая система FAT-16 опять стала неэффективной. Чтобы номера кластеров
помещались в 16 разрядов, при форматировании дисков большого объема приходится
увеличивать размер кластера до 16 Кбайт и даже больше. Это вызывало проблемы при
необходимости хранения на диске большого количества маленьких файлов. Так как
пространство для хранения файлов выделяется кластерами, даже для очень
маленького файла приходится отводить слишком много дисковой памяти.
В результате была предпринята еще одна, по всей видимости, последняя
попытка усовершенствования файловой системы FAT - размер ячейки таблицы
размещения файлов был увеличен до 32. Это позволило форматировать диски
размером в сотни Мбайт и единицы Гбайт с использованием относительно
небольшого размера кластера. Новая файловая система стала называться FAT-32.
Самый популярный вариант этой файловой системы - FAT32. Она довольно
старая, сегодняшняя версия появилась еще в 90-х. Тогда еще не было таких больших
файлов и накопителей, как сейчас, и это отразилось на ее особенностях.

#### Особенности FAT32

- максимальный размер файла в файловой системе FAT32 — 4 Гб. Более
крупные файлы вроде длинных видеозаписей записать в нее не получится;
- система быстро работает с большими файлами, но медленнее справляется со
множеством маленьких;
- изнутри структура системы представляет собой иерархическую таблицу с
данными. Есть три раздела — служебный для системных файлов, таблица
указателей для поиска данных и собственно пространство для данных;
- в FAT32 нет современных механизмов шифрования и защиты данных.
Для современных ОС FAT32 не подходит. При этом система быстрая, с ней 
удобно работать, ее распознают и читают почти все устройства. Поэтому сейчас ее
применяют в основном для флешек и карт памяти.
Хотя FAT имеет свои недостатки, она была широко распространена во многих
системах из-за своей простоты и универсальности. Тем не менее, с развитием
технологий появились более современные файловые системы (такие как NTFS и ext4),
которые обладают большей надежностью и функциональностью.

### Файловая система NTFS

NTFS (англ. new technology file system — «файловая система новой
технологии») — стандартная файловая система для семейства операционных
систем Windows NT фирмы Microsoft.

#### История создания NTFS

New Technology File System разработали еще в 1993 году, однако, как и FAT32,
используют по сей день. Сходство с FAT проявляется и в том, что, пространство
делится на кластеры заданного размера. Однако высокую гибкость NTFS
обеспечивает именно структура.

#### Структура NTFS

1. Первые 12% диска под управлением NTFS отводятся под так называемую MFTзону – пространство, в которое растет метафайл MFT. Запись каких-либо
данных в эту область невозможна.
2. Остальные 88% диска представляют собой обычное пространство для хранения
файлов.
Основой структуры тома NTFS является MFT (Master File Table) – главная
таблица файлов, которая содержит, по крайней мере, одну запись для каждого файла
тома, включая одну запись для самой себя, т.е. представляет собой каталог всех
имеющихся файлов, причем файлы небольшого размера (менее 1500 байт) хранятся
прямо в MFT – это заметно ускоряет доступ к ним
Внутренняя структура каталога представляет собой бинарное дерево. Для
поиска файла с данным именем в линейном каталоге, таком, например, как у FAT,
операционной системе приходится просматривать все элементы каталога, пока она не
найдет нужный. Бинарное же дерево располагает имена файлов таким образом, чтобы
поиск файла осуществлялся более быстрым способом — с помощью получения
двухзначных ответов на вопросы о положении файла. Вопрос, на который бинарное
дерево способно дать ответ, таков: в какой группе, относительно данного элемента,
находится искомое имя — выше или ниже? Мы начинаем с такого вопроса к
среднему элементу, и каждый ответ сужает зону поиска в среднем в два раза. Файлы,
скажем, просто отсортированы по алфавиту, и ответ на вопрос осуществляется
очевидным способом — сравнением начальных букв. Область поиска, суженная в два
раза, начинает исследоваться аналогичным образом, начиная опять же со среднего
элемента. Соответственно, поисковику не приходится просматривать всю цепочку
файлов в каталоге.

#### Особенности NTFS

- В NTFS есть логирование, то есть сведения об операциях с файлами
записываются в специальный лог.
- Система может работать с большими файлами, но имя файла должно быть не
больше 255 символов.
- Изнутри ФС выглядит как бинарное дерево: древовидная структура
данных облегчает поиск нужной информации.
- Есть шифрование данных, кэширование и система защиты целостности: любые
операции с файлами либо идут до конца, либо полностью отменяются. То есть,
если посреди записи файла вдруг выключится компьютер — «битой»
информации не будет, запись просто отменится целиком.
- в файловой системе «NTFS» добавлена способность, отсутствующая в
характеристиках файловой системы «FAT», открывать файлы, в названиях
которых не используются английские буквы, позволяя использовать любые
символы стандарта кодирования юникода «UTF». Таким образом, ограничения
использования в названиях символов любых сложных языков, например, хинди
или корейский, отсутствует.

### Файловая система ext4

Ext4 (англ. fourth extended file system, ext4fs) — журналируемая файловая
система, используемая преимущественно в операционных системах с ядром Linux,
созданная на базе ext3 в 2006 году.

#### История создания ext4

В большинстве современных дистрибутивов Linux по умолчанию используется
файловая система Ext4. В предыдущих версиях использовалась Ext3, ещё раньше Ext2
и если вернуться достаточно далеко, то и Ext.
Прежде чем появилась файловая система Ext существовала файловая система
MINIX. Если вы не знакомы с историей развития Unix, раньше существовала
небольшая операционная система MINIX, которая работала на IBM PC. Эндрю
Танненбаум разработал её для обучения и выпустил исходный код в 1987 году. Эта
операционная система не была бесплатной. Она прилагалась к книге, которая стояла
69 долларов. Однако, это было не очень дорого, поэтому девяностых годах MINIX
начали внедрять повсеместно. Благодаря чему молодой Линус Торвальдс разработал
своё ядро Linux на основе MINIX и выпустил его в 1992 году.
У MINIX была своя файловая система, которую и использовали первые версии
Linux. Она могла работать с хранилищами до 64 мегабайт, а размер имён файлов не
мог превышать 14 символов. В 1991 году средний размер жестких дисков был 40-140
мегабайт, поэтому для Linux было нужно что-нибудь другое. Именно по этой причине
Реми Кард в 1992 году разработал первую файловую систему семейства Ext. Она
решала большинство проблем MINIX. Новая файловая система использовала новую
прослойку VFS в ядре Linux и теперь могла работать с дисками до 2 гигабайт, а имена
файлов могли состоять из 255 символов. Но у Ext был один недостаток. Она имела
только одну временную метку для файла, вместо теперешних трёх: даты создания,
даты доступа и даты модификации.
Реми Кард очень быстро создал Ext, и за следующий год он разработал Ext2 для
её замены. Это уже была серьёзная файловая система коммерческого уровня. Она
была быстро реализована в ядре Linux и MINIX, а затем и внешних модулях, которые
сделали её доступной для Windows и MacOS. Здесь были снова увеличены лимиты
файловой системы, однако у неё оставалась ещё одна проблема. Как и все файловые
системы того времени, при выключении питания в момент записи файловая система
становилась неработоспособной.
Чтобы это исправить была разработана Ext3. Она уже использовала
журналирование, как и файловая система от Microsoft - NTFS. Журнал, это
специальное место на диске, куда записываются все операции с файловой системой.
При чём для самой файловой системы они применяются только после того, как будут
полностью записаны на диск. Если возникает ошибка, то файловая система просто
откатывается до предыдущей версии из журнала.
Далее, в 2006 году, была анонсирована файловая система Ext4 и ею занимался
уже другой разработчик. Его имя Теодор Цо. Журналирование Ext4 тоже
поддерживается. В ядро Linux эта файловая система попала спустя два года после
анонса. Файловая система значительно расширила возможности Ext4, но по-прежнему
опиралась на старую технологию.
Для Ext4 существует три уровня работы с журналированием:
1. Журнал - самый безопасный режим. В журнал записываются данные и
метаданные, перед тем как они будут сохранены на диск. Это обеспечивает
полную сохранность записываемого файла, но снижает производительность.
2. Упорядоченный - этот режим используется по умолчанию во многих
дистрибутивах. Метаданные записываются в журнал, но данные для записи
сразу же записываются в файловую систему. Тут порядок работы такой:
сначала метаданные записываются в журнал, затем данные записываются в
файловую систему, и только после этого метаданные тоже записываются в
файловую систему. При сбое новые метаданные находятся только в журнале и
файловая система может очень просто восстановится, будут повреждены
только те файлы, которые записываются в момент сбоя, все остальные
останутся впорядке.
3. Обратная запись - менее безопасный метод журналирования. Здесь в журнал
тоже запиваются только метаданные, но в файловую систему они могут
записываться вместе с данными, для улучшения производительности. Несмотря
на то, что файлы, записываемые во время сбоя могут быть утеряны, для
файловой системы в целом этот режим гарантирует безопасность.

#### Структура ext4

1. Superblock
Superblock содержит важные метаданные о файловой системе, такие как размер
файловой системы, количество блоков, тип и версия.
2. Inode Table
Inode (Index Node) хранит метаданные файлов, кроме их фактического
содержимого. Это включает различные атрибуты файла, такие как разрешения
доступа, дата и время создания, номера блоков и т. д.
3. Block Group Descriptor
Этот блок содержит информацию о группах блоков, таких как суперблок, inodeтаблицы и блоки данных. Он обеспечивает дополнительные метаданные для
файловой системы, помогая при организации и управлении блоками и инодами.
4. Data Blocks
Область данных является местом хранения самих файлов и каталогов. Здесь
размещаются фактические данные, представляющие файлы и каталоги.
5. Directory Structure
Структура каталога обеспечивает организацию файловой системы в
иерархическом порядке, позволяя пользователям создавать и управлять
файлами и подкаталогами.
6. Journal
Журнал используется для поддержания целостности файловой системы и
восстановления после сбоев. Он записывает предстоящие операции перед их
выполнением, обеспечивая целостность данных в случае аварийного
завершения работы.

#### Особенности ext4

- EXT начиная с EXT3 — журналируемые системы, это значит, что все
происходящие на накопителе изменения записываются в специальный
журнал. Поэтому система считается довольно стабильной.
- Информация в EXT хранится в битовых картах, то есть
последовательностях из битов. А содержимое папок представлено в виде
древовидных структур и связных списков.
- В EXT4 добавили экстенты — новый способ записи информации в
непрерывные области на диске. Этот способ повышает производительность
и скорость.
- Большая масштабируемость: ext4 может поддерживать до 1 ЭБ (эксабайт)
объёма диска и до 16 ТБ (терабайт) размера файла, что значительно
превосходит ограничения других файловых систем.
- Высокая производительность: ext4 использует различные технологии для
ускорения чтения и записи данных, такие как многопоточный ввод-вывод,
предварительное выделение, отложенное выделение, расширяемые блоки и
т. д., что позволяет ей эффективно работать с большими файлами и высокой
нагрузкой.
- Надёжная целостность данных: ext4 обеспечивает защиту данных от
повреждений при сбоях или отключении питания с помощью
журналирования или контрольных сумм, что позволяет ей быстро
восстанавливать данные без потерь или ошибок.
- Гибкая настройка: ext4 позволяет пользователю выбирать различные
параметры и опции при создании или монтировании файловой системы,
такие как размер блока, тип журналирования, включение или отключение
отдельных особенностей и т. д., что позволяет ей адаптироваться к разным
потребностям и условиям.

### Сравнение файловых систем

Поддержка операционных систем:
- Файловая система FAT (File Allocation Table) была разработана в 1977 году и
используется в операционных системах Windows и MS-DOS.
- Файловая система NTFS (New Technology File System) была разработана
компанией Microsoft для операционных систем Windows NT и выше.
- Файловая система ext4 (Fourth Extended Filesystem) является стандартной
файловой системой для большинства дистрибутивов Linux.
Масштабируемость и размер файлов:
- Файловая система FAT использует таблицу размещения файлов (File Allocation
Table), которая хранит информацию о том, какие блоки на жестком диске
заняты файлами. Размер файлов в FAT ограничен 4 ГБ, а размер раздела - 2 ТБ.
- Файловая система NTFS использует более сложную структуру, которая
включает таблицы размещения файлов, журнал файловой системы и другие
элементы. Размер файлов в NTFS ограничен 16 ЭБ (экзабайтами), а размер
раздела - до 256 ТБ.
- Файловая система ext4 использует индексированную структуру, которая
позволяет быстро находить файлы на диске. Размер файлов в ext4 ограничен 16
ТБ, а размер раздела - до 1 ЭБ (экзабайта).
Функциональные возможности и сложность структуры:
- У FAT простая структура, возможности ограничены по сравнению с
современными файловыми системами. Нет поддержки многопользовательского
режима и надежной защиты данных.
- У NTFS сложная структура, обеспечивает расширенные функциональные
возможности, включают поддержку многопользовательского режима, защиту
данных и другие функции.
- Ext4 обладает большей сложностью по сравнению с FAT, хорошо поддерживает
большие файлы и объемы, подходит для современных систем.
Журналирование и восстановление:
- FAT: Не имеет механизмов журналирования и, следовательно, менее устойчива
к сбоям.
- NTFS: Включает журналирование для обеспечения целостности данных и
восстановления.
- Ext4: Также включает механизмы журналирования для обеспечения
надежности и восстановления после сбоев.
Безопасность и доступ к данным:
- FAT: Ограниченные возможности в обеспечении безопасности и контроля
доступа.
- NTFS: Включает разветвленную систему доступа, шифрование файлов и папок,
аудит доступа к файлам.
- Ext4: Имеет ограниченные средства для управления доступом и безопасности
по сравнению с NTFS.
В сравнении файловых систем FAT, NTFS и ext4 можно сделать вывод, что NTFS и
ext4 предоставляют более продвинутые возможности по сравнению с FAT. NTFS
имеет более сложную структуру, чем FAT и ext4, и поддерживает большие размеры
файлов и разделов, многопользовательский режим и надежную защиту данных. Ext4
также поддерживает большие размеры файлов и разделов, а также
многопользовательский режим и надежную защиту данных, но является стандартной
файловой системой для большинства дистрибутивов Linux. В целом, выбор файловой
системы зависит от конкретных требований пользователя и операционной системы, на
которой она будет использоваться.

## Заключение

Файловые системы играют важную роль в организации и хранении данных на
компьютерах и других устройствах. Каждая из рассмотренных файловых систем
имеет свои особенности и применение в различных операционных системах. У всех
систем есть свои сильные и слабые места. Идеальной файловой системы нет, но есть
те, которые лучше или хуже подходят для определенных ОС, целей и технологий.
Понимание и выбор правильной файловой системы помогут обеспечить
надежность, безопасность и эффективное использование данных.
