# Реляционные базы данных

<img src="https://nbry.files.wordpress.com/2014/12/ialgochart-hft-highfrequencytrading-com.jpg" width="600px" />

Системы управления реляционными базами данных (СУРБД) и по сей день являются более чем распространенным классом СУБД. Вы, возможно, слышали такие наименования, как **MySQL, MariaDB, Oracle, PostgreSQL, SQLite** - все это яркие представители СУРБД.

В рамках представленного ниже семинара мы обсудим, что скрывается за самим понятием **реляционности**, а также рассмотрим несколько практических примеров работы с реляционными БД по средствам языка запросов **SQL** (Structured Query Language).

## Термины и понятия

Для начала определим, что такое реляционная база данных: 

> **Реляционная база данных** — база данных, основанная на реляционной модели данных. Понятие «реляционный» основано на англ. relation («отношение, зависимость, связь»).

Отлично, теперь осталось разобраться с тем, что такое отношение. Для этого дадим еще несколько раскрывающих тему терминов:

1. **Домен** - это тип данных, то есть множество допустимых значений.

2. **Отношение**:

    а) [*формальное определение*] Пусть дана совокупность типов данных <img src="https://i.upmath.me/svg/T_1%2C%20T_2%20...%20T_n" alt="T_1, T_2 ... T_n" /> (доменов), необязательно различных. Тогда *n*-арным отношением *R* (отношением степени *n*) называют [декартово произведение](https://ru.wikipedia.org/wiki/%D0%9F%D1%80%D1%8F%D0%BC%D0%BE%D0%B5_%D0%BF%D1%80%D0%BE%D0%B8%D0%B7%D0%B2%D0%B5%D0%B4%D0%B5%D0%BD%D0%B8%D0%B5) множеств <img src="https://i.upmath.me/svg/T_1%2C%20T_2%20...%20T_n" alt="T_1, T_2 ... T_n" />.
    
    б) [*определение от практического применения*] Отношение - это таблица (плоская, двумерная), состоящая из столбцов и строк.

Пример отношения:

- допустим, у нас есть ряд доменов: <img src="https://i.upmath.me/svg/D_1" alt="D_1" /> (фамилии преподавателей), <img src="https://i.upmath.me/svg/D_2" alt="D_2" /> (учебные группы), <img src="https://i.upmath.me/svg/D_3" alt="D_3" /> (учебные дисциплины), <img src="https://i.upmath.me/svg/D_4" alt="D_4" /> (аудитории),
- а *R* - множество нескольких доменов.

Тогда отношение, связывающее преподавателей и учебные дисциплины, будет выражаться следующим образом:

<p align="center"><img align="center" src="https://i.upmath.me/svg/R%20%5Csubseteq%20D_1%20%5Ctimes%20D_2" alt="R \subseteq D_1 \times D_2" /></p>

а отношение, в котором отразиться расписание занятий в группах, будет выглядеть так:

<p align="center"><img align="center" src="https://i.upmath.me/svg/R%20%5Csubseteq%20D_1%20%5Ctimes%20D_2%20%5Ctimes%20D_3%20%5Ctimes%20D_4" alt="R \subseteq D_1 \times D_2 \times D_3 \times D_4" /></p>

3. **Атрибут** - это поименованный столбец отношения.

4. **Кортеж** - это строка отношения (запись).

Термины, которые встрятятся вам в ходе использования реляционных БД:

1. **Базовое отношение** - это именованное отношения, которое не является производным других отношений (не зависит от существования других отношений).

2. **Производное отношение** - это отношение, которое определяется через другие именованные отношения.

3. **Первичный ключ** (primary key) - это уникальный идентификатор отношения (таблицы).

4. **Внешний ключ** (foreign key) - это ключ, объявленный в базовом отношении, который при этом ссылается на первичный ключ того же самого или другого базового отношения.

Итак, будем считать, что необходимый вокабуляр подготовлен.

## Подготовка БД для практических примеров

В рамках семинара мы будем использовать относительно легковесную СУБД [SQLite3](https://sqlite.org/index.html).

> Как установить и запустить указанную БД, можно прочитать по [следующей ссылке](https://www.servermania.com/kb/articles/install-sqlite/).

Начнем с создания таблицы:

``` sql
CREATE TABLE table_name (
    id INTEGER PRIMARY KEY,
    int_column INTEGER DEFAULT 0,
    name TEXT NOT NULL UNIQUE,
    kind TEXT DEFAULT "human"
);

```

Добавим (вставим) несколько строк:

```sql
INSERT INTO table_name (name, kind) VALUES ("Bob", "ghost");
INSERT INTO table_name (int_column, name) VALUES (5, "Cooper");
INSERT INTO table_name (int_column, name, kind) VALUES (4, "Mike", "ghost");

```

Посмотрим, что получилось следующим запросом:

```sql
SELECT * FROM table_name;
```

Вывод на экран:

```sh
1|0|Bob|ghost
2|5|Cooper|human
3|4|Mike|ghost
```

Перейдем к самим примерам.

## Реляционная алгебра

В реляционной модели аспект, затрагивающий манипуляцию с данными, напрямую связан с реляционной алгеброй. Примеры выполнения операций реляционной алгебры с помощью языка SQL представлены ниже.

> Определения даны по материалам ресурса [function-x.ru](https://function-x.ru/relation_algebra.html#paragraph7).

### 1. Выборка

Операция выборки работает с одним отношением <img src="https://i.upmath.me/svg/R_1" alt="R_1" /> и определяет результирующее отношение *R*, которое содержит только те кортежи (или строки, или записи), отношения <img src="https://i.upmath.me/svg/R_1" alt="R_1" />, которые удовлетворяют заданному условию (предикату *P*):

<p align="center"><img align="center" src="https://i.upmath.me/svg/R%20%3D%20%5Csigma_P(R_1)" alt="R = \sigma_P(R_1)" /></p>

Запрос:

```sql
SELECT * FROM table_name WHERE int_column > 0;
```

Вывод:

```sh
2|5|Cooper|human
3|4|Mike|ghost
```

### 2. Проекция

Операция проекции работает, как и операция выборки, только с одним отношением <img src="https://i.upmath.me/svg/R_1" alt="R_1" /> и определяет новое отношение *R*, в котором есть лишь те столбцы (и их значения), которые заданы в операции:

<p align="center"><img align="center" src="https://i.upmath.me/svg/R%20%3D%20%5CPi_%7Ba_1...a_n%7D(R_1)" alt="R = \Pi_{a_1...a_n}(R_1)" /></p>

Запрос:

```sql
SELECT DISTINCT name, kind FROM table_name;

```

Вывод:

```sh
Bob|ghost
Cooper|human
Mike|ghost
```

### 3. Объединение

Результатом объединения двух множеств (отношений) *А* и *В* будет такое множество (отношение) *С*, которое включает в себя те и только те элементы, которые есть или во множестве *А* или во множестве *В*:

<p align="center"><img align="center" src="https://i.upmath.me/svg/C%20%3D%20A%20%5Ccup%20B" alt="C = A \cup B" /></p>

![](https://upload.wikimedia.org/wikipedia/commons/thumb/3/30/Venn0111.svg/440px-Venn0111.svg.png)

Для операции объединения нам понадобится еще одна таблица:

```sql
CREATE TABLE table_name_2 (
    id INTEGER PRIMARY KEY,
    int_column INTEGER DEFAULT 0,
    name TEXT NOT NULL UNIQUE,
    kind TEXT DEFAULT "human"
);
```

Наполним ее данными:

```sql
INSERT INTO table_name_2 (name) VALUES ("Audrey");
INSERT INTO table_name_2 (name) VALUES ("Lora");
INSERT INTO table_name_2 (name) VALUES ("Donna");
```

Проверим содержимое:

```sql
SELECT * FROM table_name_2;
```

Видим на экране следующее:

```sh
1|0|Audrey|human
2|0|Lora|human
3|0|Donna|human
```

Теперь делаем запрос на объединение:

```sql
SELECT int_column, name, kind FROM table_name 
UNION SELECT int_column, name, kind FROM table_name_2;
```

Вывод:

```sh
0|Audrey|human
0|Bob|ghost
0|Donna|human
0|Lora|human
4|Mike|ghost
5|Cooper|human
```

### 4. Пересечение

Результатом пересечения двух множеств (отношений) *А* и *В* () будет такое множество (отношение) *С*, которое включает в себя те и только те элементы, которые есть и во множестве *А*, и во множестве *В*:

<p align="center"><img align="center" src="https://i.upmath.me/svg/C%20%3D%20A%20%5Ccap%20B" alt="C = A \cap B" /></p>

![](https://upload.wikimedia.org/wikipedia/commons/thumb/9/99/Venn0001.svg/440px-Venn0001.svg.png)

Запрос:

```sql
SELECT int_column FROM table_name 
INTERSECT SELECT int_column FROM table_name_2;
```

Вывод:

```sh
0
```

### 5. Разность

Разность двух отношений <img src="https://i.upmath.me/svg/R_1" alt="R_1" /> и <img src="https://i.upmath.me/svg/R_2" alt="R_2" /> состоит из кортежей (или записей, или строк), которые имеются в отношении <img src="https://i.upmath.me/svg/R_1" alt="R_1" />, но отсутствуют в отношении <img src="https://i.upmath.me/svg/R_2" alt="R_2" />:

<p align="center"><img align="center" src="https://i.upmath.me/svg/R%20%3D%20R_1%20-%20R_2" alt="R = R_1 - R_2" /></p>

Отношения <img src="https://i.upmath.me/svg/R_1" alt="R_1" /> и <img src="https://i.upmath.me/svg/R_2" alt="R_2" /> должны быть совместимы по объединению.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e6/Venn0100.svg/440px-Venn0100.svg.png)


Запрос:

```sql
SELECT int_column FROM table_name 
EXCEPT SELECT int_column FROM table_name_2;
```

Вывод:

```sh
4
5
```

### 6. Декартово произведение

Операция декартова произведения определяет новое отношение *R*, которое является результатом конкатенации каждого кортежа отношения <img src="https://i.upmath.me/svg/R_1" alt="R_1" /> с каждым кортежем отношения <img src="https://i.upmath.me/svg/R_2" alt="R_2" />:

<p align="center"><img align="center" src="https://i.upmath.me/svg/R%20%3D%20R_1%20%5Ctimes%20R_2" alt="R = R_1 \times R_2" /></p>

```sql
SELECT * FROM table_name, table_name_2;
```

Вывод:

```sh
1|0|Bob|ghost|1|0|Audrey|human
1|0|Bob|ghost|2|0|Lora|human
1|0|Bob|ghost|3|0|Donna|human
2|5|Cooper|human|1|0|Audrey|human
2|5|Cooper|human|2|0|Lora|human
2|5|Cooper|human|3|0|Donna|human
3|4|Mike|ghost|1|0|Audrey|human
3|4|Mike|ghost|2|0|Lora|human
3|4|Mike|ghost|3|0|Donna|human
```

### 7. Соединение (тета-соединение)

В результате этой операции получается отношение, которое содержит кортежи из декартова произведения отношений <img src="https://i.upmath.me/svg/R_1" alt="R_1" /> и <img src="https://i.upmath.me/svg/R_2" alt="R_2" /> удовлетворяющие предикату *Р*. Значением предиката Р может быть один из операторов сравнения (<, <=, >, >=, = или !=).

Запрос:

```sql

SELECT * FROM table_name, table_name_2 
WHERE table_name.kind = "human" AND table_name_2.kind = "human";

```

Вывод:

```sh
2|5|Cooper|human|1|0|Audrey|human
2|5|Cooper|human|3|0|Donna|human
2|5|Cooper|human|2|0|Lora|human
```

### 8. Деление

Результатом операции деления является набор кортежей (строк) отношения <img src="https://i.upmath.me/svg/R_1" alt="R_1" />, которые соответствуют комбинации всех кортежей отношения <img src="https://i.upmath.me/svg/R_2" alt="R_2" />:

<p align="center"><img align="center" src="https://i.upmath.me/svg/R%20%3D%20R_1%20%5Cdiv%20R_2" alt="R = R_1 \div R_2" /></p>


Для этого нужно, чтобы в отношении <img src="https://i.upmath.me/svg/R_2" alt="R_2" /> была часть атрибутов (можно и один), которые есть в отношении <img src="https://i.upmath.me/svg/R_1" alt="R_1" />. В результирующем отношении присутствуют только те атрибуты отношения <img src="https://i.upmath.me/svg/R_1" alt="R_1" />, которых нет в отношении <img src="https://i.upmath.me/svg/R_2" alt="R_2" />.

Запрос:

```sql
SELECT DISTINCT int_column, kind FROM table_name 
WHERE NOT EXISTS (SELECT * FROM table_name_2 
    WHERE table_name.name = table_name_2.name);
```

Вывод:


```sh
0|ghost
5|human
4|ghost
```

## Вопросы на самостоятельное изучение

### CRUD модель: CREATE, READ, UPDATE, DELETE

Кроме операций создания (`INSERT`) и чтения (`SELECT`) записей, современные СУРБД также поддерживают операции обновления (`UPDATE`) и удаления (`DELETE`) записей. Задание: провести операции обновления и удаления на представленных выше таблицах.

### Требования ACID

Как правило, СУРБД придерживаются требований [ACID](https://ru.wikipedia.org/wiki/ACID):
- Atomicity — Атомарность;
- Consistency — Согласованность;
- Isolation — Изолированность;
- Durability — Стойкость;

(подробнее см. по ссылке выше).

Противоположностью ACID можно назвать архитектуру [BASE](https://ru.wikipedia.org/wiki/%D0%A2%D0%B5%D0%BE%D1%80%D0%B5%D0%BC%D0%B0_CAP#BASE-%D0%B0%D1%80%D1%85%D0%B8%D1%82%D0%B5%D0%BA%D1%82%D1%83%D1%80%D0%B0), которая используется, например, в NoSQL базах данных (см. подробнее Силен, Д., Мейсман, А. and Али, М., 2017. Основы Data Science и Big Data. Python и наука о данных. СПб.: Питер, - c. 190-192). Но это уже совсем другая история...

