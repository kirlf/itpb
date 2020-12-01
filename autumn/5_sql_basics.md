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


