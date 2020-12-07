# Реляционные базы данных 

**Домен** - это тип данных, то есть множество допустимых значений.

**Отношение**:

1) Пусть дана совокупность типов данных <img src="https://i.upmath.me/svg/T_1%2C%20T_2%20...%20T_n" alt="T_1, T_2 ... T_n" /> (доменов), необязательно различных. Тогда *n*-арным отношением *R* (отношением степени *n*) называют [декартово произведение](https://ru.wikipedia.org/wiki/%D0%9F%D1%80%D1%8F%D0%BC%D0%BE%D0%B5_%D0%BF%D1%80%D0%BE%D0%B8%D0%B7%D0%B2%D0%B5%D0%B4%D0%B5%D0%BD%D0%B8%D0%B5) множеств <img src="https://i.upmath.me/svg/T_1%2C%20T_2%20...%20T_n" alt="T_1, T_2 ... T_n" />.
2) Отношение - это таблица (плоская, двумерная), состоящая из столбцов и строк.

Пример отношения:

- допустим, у нас есть ряд доменов: <img src="https://i.upmath.me/svg/D_1" alt="D_1" /> (фамилии преподавателей), <img src="https://i.upmath.me/svg/D_2" alt="D_2" /> (учебные группы), <img src="https://i.upmath.me/svg/D_3" alt="D_3" /> (учебные дисциплины), <img src="https://i.upmath.me/svg/D_4" alt="D_4" /> (аудитории),
- а *R* - множество нескольких доменов.

Тогда отношение, связывающее преподавателей и учебные дисциплины, будет выражаться следующим образом:

<p align="center"><img align="center" src="https://i.upmath.me/svg/R%20%5Csubseteq%20D_1%20%5Ctimes%20D_2" alt="R \subseteq D_1 \times D_2" /></p>

а отношение, в котором отразиться расписание занятий в группах, будет выглядеть так:

<p align="center"><img align="center" src="https://i.upmath.me/svg/R%20%5Csubseteq%20D_1%20%5Ctimes%20D_2%20%5Ctimes%20D_3%20%5Ctimes%20D_4" alt="R \subseteq D_1 \times D_2 \times D_3 \times D_4" /></p>

**Базовое отношение** - это именованное отношения, которое не является производным других отношений (не зависит от существования других отношений).

**Производное отношение** - это отношение, которое определяется через другие именованные отношения.

**Атрибут** - это поименованный столбец отношения.

**Кортеж** - это строка тношения (запись).

**Первичный ключ** (primary key) - это уникальный идентификатор отношения (таблицы).

**Внешний ключ** (foreign key) - это ключ, объявленный в базовом отношении, который при этом ссылается на первичный ключ того же самого или другого базового отношения.

[SQLite3](https://sqlite.org/index.html)

Создание таблицы:

``` sql
CREATE TABLE table_name (
    id INTEGER PRIMARY KEY,
    int_column INTEGER DEFAULT 0,
    name TEXT NOT NULL UNIQUE,
    kind TEXT DEFAULT "human"
);

```

Вставка значений:

```sql
INSERT INTO table_name (name, kind) VALUES ("Bob", "ghost");
INSERT INTO table_name (int_column, name) VALUES (5, "Cooper");
INSERT INTO table_name (int_column, name, kind) VALUES (4, "Mike", "ghost");

```

Посмотрим, что получилось следующим запросом:

```sql
SELECT * FROM table_name;
```

Вывод:

```sh
1|0|Bob|ghost
2|5|Cooper|human
3|4|Mike|ghost
```

## Реляционная алгебра

1. **Выборка**

Запрос:

```sql
SELECT * FROM table_name WHERE int_column > 0;
```

Вывод:

```sh
2|5|Cooper|human
3|4|Mike|ghost
```

2. **Проекция**

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

3. **Объединение**

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

SELECT * FROM table_name_2;
```

Вывод:

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

4. **Пересечение**

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

5. **Разность**

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

6. **Декартово произведение**

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

7. **Соединение (тета-соединение)**

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

