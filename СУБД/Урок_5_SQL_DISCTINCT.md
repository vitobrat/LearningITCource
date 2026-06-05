---
tags:
  - СУБД
  - Урок_5
aliases:
---
Следующий урок_6: [[Урок_6_SQL_WHERE]]

# # [Исключение дубликатов, DISTINCT](https://sql-academy.org/ru/guide/distinct-operator#isklyuchenie-dublikatov-distinct)

В некоторых ситуациях SQL запрос на выборку может возвращать повторяющиеся строки данных.

Например, давайте выведем поле class из таблицы Student_in_class из базы данных, в которой организовано хранение информации о расписании занятий в школе.
![[Pasted image 20260605152239.png]]

PostgreSQL 17.5

```sql
SELECT class FROM Student_in_class;
```

|class|
|---|
|9|
|9|
|9|
|9|
|9|
|9|
|9|
|9|
|9|
|9|
|9|
|9|
|9|
|8|
|8|
|8|
|8|
|8|
|8|
|8|
|8|
|8|
|8|
|8|
|6|
|6|
|6|
|6|
|6|
|6|
|6|
|6|
|6|
|6|
|6|
|5|
|5|
|5|
|5|
|5|
|5|
|5|
|5|
|4|
|4|
|4|
|4|
|4|
|4|
|4|
|4|
|4|
|3|
|3|
|3|
|3|
|3|
|3|
|3|
|3|
|2|
|2|
|2|
|2|
|2|
|2|
|2|
|1|
|1|
|1|
|1|
|1|
|1|
|1|

Поскольку в одном классе возможно нахождение нескольких студентов, то не удивительно, что при выводе мы можем наблюдать одинаковые значения. Чтобы при выборке избежать такого дублирования, есть оператор DISTINCT.

## [Синтаксис оператора](https://sql-academy.org/ru/guide/distinct-operator#sintaksis-operatora)

PostgreSQL 17.5

```sql
SELECT [DISTINCT] поля_таблиц FROM наименование_таблицы;
```

> Квадратные скобки в описании синтаксиса обозначают необязательную часть запроса. В самом SQL-запросе их писать не нужно.

То есть в нашем случае запрос на получение уникальных классов, в которых есть хотя бы один студент, будет выглядеть следующим образом:

PostgreSQL 17.5

```sql
SELECT DISTINCT class FROM Student_in_class;
```

|class|
|---|
|9|
|8|
|7|
|6|
|5|
|4|
|3|
|2|
|1|

## [DISTINCT для нескольких колонок](https://sql-academy.org/ru/guide/distinct-operator#distinct-dlya-neskolkih-kolonok)

При использовании оператора DISTINCT для двух и более колонок будут удаляться записи, которые имеют одинаковые значения по всем полям.

То есть для такой таблицы

| first_name | last_name |
| ---------- | --------- |
| John       | Scott     |
| William    | Dawson    |
| Raul       | Hartman   |
| William    | Hartman   |
| John       | Scott     |
| John       | Hartman   |

запрос с оператором DISTINCT вернул бы все сочетания имён и фамилий, кроме дублирующихся «John Scott».

PostgreSQL 17.5

```sql
SELECT DISTINCT first_name, last_name FROM User;
```

|first_name|last_name|
|---|---|
|John|Scott|
|William|Dawson|
|Raul|Hartman|
|William|Hartman|
|John|Hartman