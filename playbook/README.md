# Playbook для установки clickhouse и vector.
## Содержание:  
[Описание Playbook](#описание-playbook)  
[Описание inventory файла](#описание-inventory-файла)  
[Описание файлов переменных](#описание-файлов-переменных)
## Описание Playbook
Playbook содержит 2 плея:
1. [Install Clickhouse](#плей-install-clickhouse)
2. [Install Vector](#плей-install-vector)
### Плей Install Clickhouse 
Плей ```Install Clickhouse``` устанавливает clickhouse на все машины в группе clickhouse в inventory файле.  
Процесс выполнения плея:
1. Таск ```Get clickhouse distrib```. Скачивает дистрибутивы перечисленные в переменных. Сначала task ищет дистрибутивы без архитектуры, если не находит, то берет дистрибутив с архитектурой x86_64.
2. Таск ```Install clickhouse packages```. Устанавливает скачанные по списку дистрибутивы. Данный таск, в случае если были внесены изменения, вызывает handler ```Start clickhouse service```.
3. Таск ```Start clickhouse service```. Убеждается что service "clickhouse-server" запущен, либо запускает его.
4. Таск ```Create database```. Создаёт базу данных "logs".

Хендлеры в порядке их выполнения:
1. Хэндлер ```Start clickhouse service``` рестартует сервис "clickhouse-server" в случае если был вызван в таске ```Install clickhouse packages```.

### Плей Install Vector
Плей ```Install Vector``` устанавливает vector на все машины в группе Vector в inventory файле.  
Процесс выполнения плея:
1. Таск ```Get vector distrib```. Скачивает дистрибутив vector версии указанной в переменных.
2. Таск ```Install vector packages```. Устанавливает скачанный в первом таске пакет. Данный таск, в случае если были внесены изменения, вызывает handler ```Start vector service```.

Хендлеры в порядке их выполнения:
1. Хэндлер ```Start vector service``` рестартует сервис "vector" в случае если был вызван в таске ```Install vector packages```.

## Описание inventory файла
Inventory файл prod.yml находится [здесь](./inventory/prod.yml).  
В inventory файле обязательно должны быть перечислены хосты для двух групп: clickhouse и vector. Плейбуком предполагается что хосты группы clickhouse будут на базе ОС CentOS, а хосты группы vector будут на базе ОС ubuntu или Debian.

## Описание файлов переменных
В данном проекте 2 файлах переменных для групп хостов [clickhouse](./group_vars/clickhouse/vars.yml) и [vector](./group_vars/vector/vars.yml).
### Файл переменных clickhouse
Файл переменных clickhouse в обязательном порядке должен содержать следующие переменные:
Переменная | Описание
---------- | --------
clickhouse_version | Требуемая к установке версия дистрибутива
clickhouse_packages | Список требуемых к установке пакетов
### Файл переменных vector
Файл переменных vector в обязательном порядке должен содержать следующие переменные:
Переменная | Описание
---------- | --------
vector_version | Требуемая к установке версия дистрибутива