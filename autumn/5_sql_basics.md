# Реляционные базы данных 

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

