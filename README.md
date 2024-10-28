<!-- markdownlint-disable MD024 MD040 -->

# Базы данных, контрольная работа №1

## Описание базы данных

- Отношение `S` - поставщики.
- Отношение `P` - детали.
- Отношение `J` - проекты.
- Отношение `SPJ` - поставки деталей поставщиками в рамках какого-либо проекта.

### Отношение `S` - поставщики

| S#  | SNAME | STATUS | CITY   |
| --- | ----- | ------ | ------ |
| S1  | Smith | 20     | London |
| S2  | Jones | 10     | Paris  |
| S3  | Blake | 30     | Paris  |
| S4  | Clark | 20     | London |
| S5  | Adams | 30     | Athens |

### Отношение `P` - детали

| P#  | PNAME | COLOR | WEIGHT | CITY   |
| --- | ----- | ----- | ------ | ------ |
| P1  | Nut   | Red   | 12.0   | London |
| P2  | Bolt  | Green | 17.0   | Paris  |
| P3  | Screw | Blue  | 17.0   | Oslo   |
| P4  | Screw | Red   | 14.0   | London |
| P5  | Cam   | Blue  | 12.0   | Paris  |
| P6  | Cog   | Red   | 19.0   | London |

### Отношение `J` - проекты

| J#  | JNAME   | CITY   |
| --- | ------- | ------ |
| J1  | Sorter  | Paris  |
| J2  | Display | Rome   |
| J3  | OCR     | Athens |
| J4  | Console | Athens |
| J5  | RAID    | London |
| J6  | EDS     | Oslo   |
| J7  | Tape    | London |

### Отношение `SPJ` - поставки деталей поставщиками в рамках какого-либо проекта

| S#  | P#  | J#  | QTY |
| --- | --- | --- | --- |
| S1  | P1  | J1  | 200 |
| S1  | P1  | J4  | 700 |
| S2  | P3  | J1  | 400 |
| S2  | P3  | J2  | 200 |
| S2  | P3  | J3  | 200 |
| S2  | P3  | J4  | 500 |
| S2  | P3  | J5  | 600 |
| S2  | P3  | J6  | 400 |
| S2  | P3  | J7  | 800 |
| S2  | P5  | J2  | 100 |
| S3  | P3  | J1  | 200 |
| S3  | P4  | J2  | 500 |
| S4  | P6  | J3  | 300 |
| S4  | P6  | J7  | 300 |
| S5  | P2  | J2  | 200 |
| S5  | P2  | J4  | 100 |
| S5  | P5  | J5  | 500 |
| S5  | P5  | J7  | 100 |
| S5  | P6  | J2  | 200 |
| S5  | P1  | J4  | 100 |
| S5  | P3  | J4  | 200 |
| S5  | P4  | J4  | 800 |
| S5  | P5  | J4  | 400 |
| S5  | P6  | J4  | 500 |

## Задание

Нужно составить запрос в трёх вариантах:

1. используя реляционную алгебру;
2. используя реляционное исчисление;
3. используя язык SQL и само решение (_таблица-ответ_).

```sql
S { S#, SNAME, STATUS, CITY }
    PRIMARY KEY { S# }

P { P#, PNAME, COLOR, WEIGHT, CITY }
    PRIMARY KEY { P# }

J { J#, JNAME, CITY }
    PRIMARY KEY { J# }

SPJ { S#, P#, J#, QTY }
    PRIMARY KEY { S#, P#, J# }
    FOREIGN KEY { S# } REFERENCES S
    FOREIGN KEY { P# } REFERENCES P
    FOREIGN KEY { J# } REFERENCES J
```

## Оффтоп: создание и заполнение таблиц для задания

### 1. Создание необходимых SQL таблиц

```sql
-- Creating the Suppliers table

CREATE TABLE s (
    s_id VARCHAR(2) PRIMARY KEY,
    sname VARCHAR(50),
    status INT,
    city VARCHAR(50)
);

-- Creating the Parts table

CREATE TABLE p (
    p_id VARCHAR(2) PRIMARY KEY,
    pname VARCHAR(50),
    color VARCHAR(20),
    weight DECIMAL(5, 2),
    city VARCHAR(50)
);

-- Creating the Projects table

CREATE TABLE j (
    j_id VARCHAR(2) PRIMARY KEY,
    jname VARCHAR(50),
    city VARCHAR(50)
);

-- Creating the SPJ (Suppliers-Parts-Projects) table

CREATE TABLE spj (
    s_id VARCHAR(2),
    p_id VARCHAR(2),
    j_id VARCHAR(2),
    qty INT,
    PRIMARY KEY (s_id, p_id, j_id),
    FOREIGN KEY (s_id) REFERENCES S(s_id),
    FOREIGN KEY (p_id) REFERENCES P(p_id),
    FOREIGN KEY (j_id) REFERENCES J(j_id)
);
```

### 2. Заполнение таблиц данными

```sql
-- Inserting data into the Suppliers table

INSERT INTO s (s_id, sname, status, city)
VALUES ('S1', 'Smith', 20, 'London'),
       ('S2', 'Jones', 10, 'Paris'),
       ('S3', 'Blake', 30, 'Paris'),
       ('S4', 'Clark', 20, 'London'),
       ('S5', 'Adams', 30, 'Athens');

-- Inserting data into the Parts table

INSERT INTO p (p_id, pname, color, weight, city)
VALUES ('P1', 'Nut', 'Red', 12.0, 'London'),
       ('P2', 'Bolt', 'Green', 17.0, 'Paris'),
       ('P3', 'Screw', 'Blue', 17.0, 'Oslo'),
       ('P4', 'Screw', 'Red', 14.0, 'London'),
       ('P5', 'Cam', 'Blue', 12.0, 'Paris'),
       ('P6', 'Cog', 'Red', 19.0, 'London');

-- Inserting data into the Projects table

INSERT INTO j (j_id, jname, city)
VALUES ('J1', 'Sorter', 'Paris'),
       ('J2', 'Display', 'Rome'),
       ('J3', 'OCR', 'Athens'),
       ('J4', 'Console', 'Athens'),
       ('J5', 'RAID', 'London'),
       ('J6', 'EDS', 'Oslo'),
       ('J7', 'Tape', 'London');

-- Inserting data into the SPJ table

INSERT INTO spj (s_id, p_id, j_id, qty)
VALUES ('S1', 'P1', 'J1', 200),
       ('S1', 'P1', 'J4', 700),
       ('S2', 'P3', 'J1', 400),
       ('S2', 'P3', 'J2', 200),
       ('S2', 'P3', 'J3', 200),
       ('S2', 'P3', 'J4', 500),
       ('S2', 'P3', 'J5', 600),
       ('S2', 'P3', 'J6', 400),
       ('S2', 'P3', 'J7', 800),
       ('S2', 'P5', 'J2', 100),
       ('S3', 'P3', 'J1', 200),
       ('S3', 'P4', 'J2', 500),
       ('S4', 'P6', 'J3', 300),
       ('S4', 'P6', 'J7', 300),
       ('S5', 'P2', 'J2', 200),
       ('S5', 'P2', 'J4', 100),
       ('S5', 'P5', 'J5', 500),
       ('S5', 'P5', 'J7', 100),
       ('S5', 'P6', 'J2', 200),
       ('S5', 'P1', 'J4', 100),
       ('S5', 'P3', 'J4', 200),
       ('S5', 'P4', 'J4', 800),
       ('S5', 'P5', 'J4', 400),
       ('S5', 'P6', 'J4', 500);
```

## Варианты

### 7.13. Получить полные сведения обо всех проектах

#### Реляционная алгебра

`J`

#### Реляционное исчисление

`JX`

#### SQL

```sql
SELECT *
FROM j;
```

### 7.14. Получить полные сведения обо всех проектах в Лондоне

#### Реляционная алгебра

`J WHERE City='London'`

#### Реляционное исчисление

`JX WHERE JX.City='London'`

#### SQL

```sql
SELECT *
FROM j
WHERE city = 'London';
```

### 7.15. Определить номера поставщиков деталей для проекта с номером J1

#### Реляционная алгебра

`(SPJ WHERE J#='J1') [S#]`

#### Реляционное исчисление

`SPJX.S# WHERE J#='J1'`

#### SQL

```sql
SELECT DISTINCT s_id
FROM spj
WHERE j_id='J1';
```

### 7.16. Определить все поставки, в которых количество деталей находится в диапазоне от 300 до 750 штук включительно

#### Реляционная алгебра

`SPJ WHERE Qty>=300 AND Qty<=750`

#### Реляционное исчисление

`SPJX WHERE SPJX.Qty>=300 AND SPJX.Qty<=750`

#### SQL

```sql
SELECT *
FROM spj
WHERE qty >=300
  AND qty <=750;
```

### 7.17. Найти все существующие сочетания вида "цвета детали - город, из которого поставляются детали"

>[!WARNING]
>
>**Примечание**. Здесь и в последующих упражнениях слово "все" используется в значении "все, представленные в настоящий момент в базе данных", а не "все возможные".

#### Реляционная алгебра

`P [Color, City]`

#### Реляционное исчисление

`(PX.Color, PX.City)`

#### SQL

```sql
SELECT DISTINCT color,
                city
FROM p;
```

### 7.18. Найти все такие тройки значений "номер поставщика – номер детали – номер проекта", для которых указанные поставщик, деталь и проект находятся в одном городе

#### Реляционная алгебра

`(S JOIN P JOIN J) [S#, P#, J#]`

>[!WARNING]
>
>**FIXME**: скорее всего неправильно.
>
>Может быть всё же?
>
>`((S JOIN P) JOIN J) WHERE S.CITY = P.CITY AND P.CITY = J.CITY [S#, P#, J#]`

#### Реляционное исчисление

```
(SX.S#, PX.P#,JX.J#) WHERE SX.City=PX.City AND
                     PX.City=JX.City AND SX.City=JX.City
```

#### SQL

```sql
SELECT s.s_id,
       p.p_id,
       j.j_id
FROM s
JOIN p ON s.city = p.city
JOIN j ON p.city = j.city
AND s.city = j.city;
```

>[!NOTE]
>
>Либо, что то же самое:
>
>```sql
>SELECT s.s_id,
>       p.p_id,
>       j.j_id
>FROM s,
>     p,
>     j
>WHERE s.city = p.city
>  AND p.city = j.city;
>```

### 7.19. Найти все такие тройки значений "номер поставщика – номер детали – номер проекта", для которых указанные поставщик, деталь и проект не находятся в одном городе

#### Реляционная алгебра

```
(((S RENAME City AS Scity) TIMES
(P RENAME City AS Pcity) TIMES
(J RENAME City AS Jcity))
WHERE SCity≠Pcity OR
    PCity≠Jcity OR
    JCity≠Scity) [S#, P#, J#]
```

#### Реляционное исчисление

```
(SX.S#, PX.P#,JX.J#) WHERE SX.City<>PX.City OR
PX.City<>JX.City OR SX.City<>JX.City
```

#### SQL

```sql
SELECT s.s_id,
       p.p_id,
       j.j_id
FROM s,
     p,
     j
WHERE NOT (s.city = p.city
           AND p.city = j.city);
```

>[!NOTE]
>
>Либо, что то же самое:
>
>```sql
>SELECT s.s_id,
>       p.p_id,
>       j.j_id
>FROM s,
>     p,
>     j
>WHERE s.city != p.city
>  OR p.city != j.city
>  OR s.city != j.city;
>```

### 7.20. Найти все такие тройки значений "номер поставщика – номер детали – номер проекта", для которых никакие из двух поставщиков, деталей и проектов не находятся в одном городе

#### Реляционная алгебра

```
(((S RENAME City AS Scity) TIMES
(P RENAME City AS Pcity) TIMES
(J RENAME City AS Jcity))
WHERE SCity≠Pcity AND
    PCity≠Jcity AND
    JCity≠Scity) [S#, P#, J#]
```

#### Реляционное исчисление

```
(SX.S#, PX.P#,JX.J#) WHERE SX.City<>PX.City AND
PX.City<>JX.City AND SX.City<>JX.City
```

#### SQL

```sql
SELECT s.s_id,
       p.p_id,
       j.j_id
FROM s,
     p,
     j
WHERE s.city <> p.city
  AND p.city <> j.city
  AND j.city <> s.city;
```

### 7.21. Получить полные сведения о деталях, поставляемых поставщиком из Лондона

#### Реляционная алгебра

`(SPJ JOIN (S WHERE City='London')) [P#]`

#### Реляционное исчисление

`SPJX.P# WHERE EXISTS SX (SX.S#=SPJX.S# AND SX.City='London')`

#### SQL

```sql
SELECT DISTINCT spj.p_id
FROM spj
WHERE
    (SELECT s.city
     FROM s
     WHERE s.s_id = spj.s_id) = 'London';
```

### 7.22. Определить номера деталей, поставляемых поставщиком из Лондона для проекта в Лондоне

#### Реляционная алгебра

```
((SPJ JOIN (S WHERE City='London')) [P#, J#]
    JOIN (J WHERE City='London')) [P#]
```

#### Реляционное исчисление

```
SPJX.P# WHERE EXISTS SX EXISTS JX (SX.S#=SPJX.S# AND
SX.City='London' AND JX.J#=SPJX.J# AND JX.City='London')
```

#### SQL

```sql
SELECT DISTINCT spj.p_id
FROM spj
WHERE
    (SELECT s.city
     FROM s
     WHERE s.s_id = spj.s_id) = 'London'
  AND
    (SELECT j.city
     FROM j
     WHERE j.j_id = spj.j_id) = 'London';
```

### 7.23. Найти все пары названий городов, для которых поставщик из первого города обеспечивает проект во втором городе

#### Реляционная алгебра

```
((S RENAME City AS Scity) JOIN SPJ JOIN
(J RENAME City AS Jcity)) [SCity, JCity]
```

#### Реляционное исчисление

```
(SX.City AS Scity, JX.City AS JCity) WHERE EXISTS SPJX
(SPJX.S#=SX.S# AND SPJX.J#=JX.J#)
```

#### SQL

```sql
SELECT DISTINCT s.city,
                j.city
FROM s,
     j
WHERE EXISTS
    (SELECT *
     FROM spj
     WHERE spj.s_id = s.s_id
       AND spj.j_id = j.j_id);
```

### 7.24. Определить номера деталей, поставляемых для всех проектов, поставляемых поставщиком из того же города, в котором разрабатывается проект

#### Реляционная алгебра

1. Находим все проекты, которые содержатся во всех городах, где находятся поставщики

    `J {J#, CITY} DEVIDEDBY S{CITY})`
2. Найдем детали, которые поставляются для всех проектов, находящихся в каждом из городов, представленных поставщиками

    `(J {J#, CITY} DEVIDEDBY S{CITY}) JOIN SPJ)`
3. Найдем номера деталей, которые поставляются для всех проектов, расположенных в каждом из городов, указанных в таблице поставщиков.

    `(SPJ {P#, J#} DEVIDEDBY (J {J#, CITY} DEVIDEDBY S{CITY}) JOIN SPJ)`
4. Выберем только номера деталей (**без повторений**)

    ```
    ((SPJ [P#, J#] DEVIDEDBY
    (J [J#, CITY]] DEVIDEDBY S[CITY]) JOIN SPJ) [P#]
    ```

#### Реляционное исчисление

```
SPJX.P# WHERE EXISTS SX EXISTS JX (SX.City=JX.City AND
SPJX.S#=SX.S# AND SPJX.J#=JX.J#)
```

#### SQL

```sql
SELECT DISTINCT spj.p_id
FROM spj
WHERE
    (SELECT s.city
     FROM s
     WHERE s.s_id = spj.s_id) =
    (SELECT j.city
     FROM j
     WHERE j.j_id = spj.j_id);
```

### 7.25. Найти все номера проектов, детали для которых поставляются по крайней мере одним поставщиком из другого города

#### Реляционная алгебра

```
(((J RENAME City AS Jcity) JOIN SPJ JOIN
(S RENAME City AS Scity))
    WHERE JCity≠Ccity) [J#]
```

#### Реляционное исчисление

```
SPJX.P# WHERE EXISTS SX EXISTS JX (SX.City<>JX.City AND
SPJX.S#=SX.S# AND SPJX.J#=JX.J#)
```

#### SQL

```sql
SELECT DISTINCT spj.j_id
FROM spj
WHERE
    (SELECT s.city
     FROM s
     WHERE s.s_id = spj.s_id) <>
    (SELECT j.city
     FROM j
     WHERE j.j_id = spj.j_id);
```

### 7.26. Определить все пары номеров деталей, в которых обе детали поставляются  одним и тем же поставщиком

#### Реляционная алгебра

```
(((SPJ [S#,P#] RENAME S# AS XS#, P# AS XP#)) TIMES
((SPJ [S#,P#] RENAME S# AS YS#, P# AS YP#)))
    WHERE XS#=YS# AND XP#<YP#) [XP#, YP#]
```

#### Реляционное исчисление

```
(SPJX.P# AS XP#, SPJY AS Y#) WHERE SPJX.S#=SPJY.S# AND
SPJX.P#<SPJY.P#)
```

#### SQL

```sql
SELECT DISTINCT spj1.p_id,
                spj2.p_id
FROM spj AS spj1,
     spj AS spj2
WHERE spj1.s_id = spj2.s_id
  AND spj1.p_id < spj2.p_id;
```

### 7.27. Определить общее число проектов, детали для которых поставляются поставщиком с номером S1

#### Реляционная алгебра

`(SUMMARIZE ((SPJ WHERE S#='S1') [J#]) BY () ADD COUNT AS N) [N]`

#### Реляционное исчисление

`COUNT(SPJX.J# WHERE SPJX.S#='S1') AS N`

#### SQL

```sql
SELECT COUNT (DISTINCT spj.j_id)
FROM spj
WHERE spj.s_id = 'S1';
```

### 7.28. Определить общее количество деталей с номером P1, поставляемых поставщиком с номером S1

#### Реляционная алгебра

```
(SUMMARIZE (SPJ WHERE S#='S1' AND P#='P1')) BY ()
    ADD SUM(Qty) AS Q) [Q] 
(EXTEND (S WHERE S#='S1')
    ADD SUM((MATCHING SPJ) WHERE P#='P1', Qty) AS Q) [Q]
```

#### Реляционное исчисление

`SUM ( SPJX WHERE SPJX.S#=SPJX.'S1' AND SPJX.P#=SPJX.'P1', Qty) AS Q`

#### SQL

```sql
SELECT sum(spj.qty)
FROM spj
WHERE spj.s_id = 'S1'
  AND spj.p_id = 'P1';
```

### 7.29. Для каждой детали, поставляемой для проекта, определить номер детали, номер проекта и соответствующее общее количество

#### Реляционная алгебра

`SUMMARIZE SPJ BY (P#,J#) ADD SUM(Qty) AS Q`

#### Реляционное исчисление

`(SPJX.P#, SPJX.J#, SUM(SPJY WHERE SPJY.P#=SPJX.P# AND SPJY.J#=SPJX.J#, Qty) AS Q)`

#### SQL

```sql
SELECT spj.p_id,
       spj.j_id,
       sum(spj.qty)
FROM spj
GROUP BY spj.p_id,
         spj.j_id;
```

### 7.30. Определить номера деталей, поставляемых для некоторого проекта, со средним количеством, составляющим больше 350 штук

#### Реляционная алгебра

`((SUMMARIZE SPJ BY (P#,J#) ADD AVG(Qty) AS Q) WHERE Q>350)[P#]`

#### Реляционное исчисление

`SPJX.P# WHERE AVG (SPJY WHERE SPJY.P#=SPJX.P# AND SPJY.J#=SPJX.J#, Qty) >350`

#### SQL

```sql
SELECT DISTINCT spj.p_id
FROM spj
GROUP BY spj.p_id,
         spj.j_id
HAVING avg(spj.qty) > 350;
```

### 7.31. Определить названия проектов, детали для которых поставляются поставщиком с номером S1

#### Реляционная алгебра

`(J JOIN (SPJ WHERE S#='S1')) [JName]`

#### Реляционное исчисление

`JX.Jname WHERE EXISTS SPJX (SPJX.J#=JX.J# AND SPJX.S#='S1')`

#### SQL

```sql
SELECT DISTINCT j.jname
FROM j,
     spj
WHERE j.j_id = spj.j_id
  AND spj.s_id = 'S1';
```

### 7.32. Определить цвета деталей, поставляемых поставщиком с номером S1

#### Реляционная алгебра

`(P JOIN (SPJ WHERE S#='S1')) [Color]`

#### Реляционное исчисление

`PX.Color WHERE EXISTS SPJX (SPJX.P#=PX.P# AND SPJX.S#='S1')`

#### SQL

```sql
SELECT DISTINCT p.color
FROM p,
     spj
WHERE p.p_id = spj.p_id
  AND spj.s_id = 'S1';
```

### 7.33. Установить номера деталей, поставляемых для любого проекта, разрабатываемого в Лондоне

#### Реляционная алгебра

`(SPJ JOIN (J WHERE City='London')) [P#]`

#### Реляционное исчисление

`SPJX.P# WHERE EXISTS JX (JX.City='London' AND JX.J#=SPJPX.J#)`

#### SQL

```sql
SELECT DISTINCT spj.p_id
FROM spj,
     j
WHERE spj.j_id = j.j_id
  AND j.city = 'London';
```

### 7.34. Определить номера проектов, в которых используется по крайней мере одна деталь, имеющуюся у поставщика с номером S1

#### Реляционная алгебра

`(SPJ JOIN (SPJ WHERE S#='S1') [P#]) [J#]`

#### Реляционное исчисление

`SPJX.J# WHERE EXISTS SPJY(SPJX.P#=SPJY.P# AND SPJY.S#='S1'`

#### SQL

```sql
SELECT DISTINCT spj1.j_id
FROM spj AS spj1,
     spj AS spj2
WHERE spj1.p_id = spj2.p_id
  AND spj2.s_id = 'S1';
```

### 7.35. Определить номера поставщиков по крайней мере одной детали, поставляемой по крайней мере одним поставщиком, который поставляет хотя бы одну деталь красного цвета

#### Реляционная алгебра

```
(((SPJ JOIN (P WHERE Color='Red') [P#]) [S#]
    JOIN SPJ) [P#] JOIN SPJ) [S#]
```

#### Реляционное исчисление

```
SPJX.S# WHERE EXISTS SPJY EXISTS SPJZ EXISTS PX
(SPJX.P#=SPJY.P# AND SPJX.S#=SPJZ.S# AND SPJZ.P#=PX.P# AND
PX.Color='Red')
```

#### SQL

```sql
SELECT DISTINCT spj1.s_id
FROM spj AS spj1,
     spj AS spj2,
     spj AS spj3
WHERE spj1.p_id = spj2.p_id
  AND spj2.s_id = spj3.s_id
  AND
    (SELECT p.color
     FROM p
     WHERE p.p_id = spj3.p_id) = 'Red';
```

### 7.36. Определить номера поставщиков со статусом, меньшим, чем у поставщика с номером S1

#### Реляционная алгебра

```
((S [S#, Status] RENAME S# AS XS#, Status AS XStatus) TIMES
(S [S#, Status] RENAME S# AS YS#, Status AS YStatus))
    WHERE XS#='S1' AND XStatus>YStatus) [YS#]
```

#### Реляционное исчисление

`SX.S# WHERE EXISTS SY(SY.S#='S1' AND SX>Status<SY.Status)`

#### SQL

```sql
SELECT s.s_id
FROM s
WHERE s.status <
    (SELECT s.status
     FROM s
     WHERE s.s_id = 'S1');
```

### 7.37. Определить номера проектов, разрабатываемых в городе, который находится на первом месте в алфавитном списке таких городов

#### Реляционная алгебра

```
J [J#] MINUS
((J [J#, City] RENAME City AS XCity) TIMES J [City])
    WHERE XCity>City) [J#]
```

#### Реляционное исчисление

```
JX.J# WHERE FORALL JY(JY.CityJX.City)
JX.J# WHERE JX.City=MIN(JY.City)
```

#### SQL

```sql
SELECT j.j_id
FROM j
WHERE j.city =
    (SELECT MIN (j.city)
     FROM j);
```

### 7.38. Определить номера проектов, для которых среднее количество поставляемых деталей с номером P1 больше, чем наибольшее количество любых деталей, поставляемых для проекта с номером J1

#### Реляционная алгебра

```
(((SUMMARIZE (SPJ WHERE P#='P1') BY (J#) ADD AVG (Qty) AS QX) TIMES
(SUMMARIZE (SPJ WHERE J#='J1') BY ()
    ADD MAX(Qty) AS QY) [QY]
WHERE QX>QY)[J#]
```

#### Реляционное исчисление

```
SPJX.J# WHERE SPJX.P#='P1' AND
AVG(SPJY WHERE SPJY.P#='P1' AND SPJY.J#=SPJX.J#, Qty)>
MAX (SPJZ.Qty WHERE SPJZ.J#='J1')
```

#### SQL

```sql
SELECT DISTINCT spj1.j_id
FROM spj AS spj1
WHERE spj1.p_id = 'P1'
  AND
    (SELECT AVG (spj2.qty)
     FROM spj AS spj2
     WHERE spj2.j_id = spj1.j_id
       AND spj2.p_id = 'P1') >
    (SELECT max(spj3.qty)
     FROM spj AS spj3
     WHERE spj3.j_id = 'J1');
```

>[!WARNING]
>
>**(Если же для проекта J1 вообще нет поставок, то возникает проблема!)**

### 7.39. Определить номера поставщиков детали с номером Р1 для некоторого проекта в количестве, большем среднего количества деталей с номером Р1, поставляемых для этого проекта

#### Реляционная алгебра

```
(((((SPJ WHERE P#='P1') [S#,J#,Qty])
    RENAME J# AS XJ#, Qty AS XQ) TIMES
    (SUMMARIZE (SPJ WHERE P#='P1') BY (J#)
    ADD AVG (Qty) AS Q))
 WHERE XJ#=J# AND XQ>Q)[S#]
```

#### Реляционное исчисление

```
SPJX.S# WHERE SPJX.P#='P1' AND SPX.Qty>
AVG(SPJY WHERE SPJY.P#='P1' AND SPJY.J#=SPJX.J#, Qty)
```

#### SQL

```sql
SELECT DISTINCT spj1.s_id
FROM spj AS spj1
WHERE spj1.p_id = 'P1'
  AND spj1.qty >
    (SELECT avg(spj2.qty)
     FROM spj AS spj2
     WHERE spj2.p_id = 'P1'
       AND spj2.j_id = spj1.j_id);
```

### 7.40. Найти номера проектов, для которых поставщиками из Лондона не поставляется ни одна деталь красного цвета

#### Реляционная алгебра

```
J [J#] MINUS
(( S WHERE City='London') [S#]
JOIN SPJ JOIN (P WHERE Color='Red')) [J#]
```

#### Реляционное исчисление

```
JX.J# WHERE NOT EXISTS SPJX EXISTS SX EXISTS PX
(SX.City='London' AND PX.Color='Red' AND
SPJX.S#=SX.S# AND SPJX.P#=PX.P# AND SPJX.J#=JX.J#)
```

#### SQL

```sql
SELECT j.j_id
FROM j
WHERE NOT EXISTS
    (SELECT *
     FROM spj,
          p,
          s
     WHERE spj.j_id = j.j_id
       AND spj.p_id = p.p_id
       AND spj.s_id = s.s_id
       AND p.color = 'Red'
       AND s.city = 'London');
```

### 7.41. Определить номера проектов, детали для которых полностью поставляются поставщиком с номером S1

#### Реляционная алгебра

```
J [J#] MINUS
(SPJ WHERE S# = 'S1') [J#]
```

#### Реляционное исчисление

`JX.J# WHERE FORALL SPJY (IF SPJY.J=JX.J# THEN SPJY.S#='S1')`

#### SQL

```sql
SELECT j.j_id
FROM j
WHERE NOT EXISTS
    (SELECT *
     FROM spj
     WHERE spj.j_id = j.j_id
       AND NOT (spj.s_id = 'S1'));
```

### 7.42 (**ПЕРЕПРОВЕРИТЬ**) Определить номера деталей, поставляемых для лондонских проектов

#### Реляционная алгебра

`(P WHERE (MATCHING SPJ) [J#]>= (J WHERE City='London') [J#]) [P#]`

#### Реляционное исчисление

```
PX.P# WHERE FORALL JX
(IF JX.City='London' THEN
EXISTS SPJY (SPJY.P#=PX.P# AND SPJY.J#=JX.J#))
```

#### SQL

```sql
SELECT p.p_id
FROM p
WHERE NOT EXISTS
    (SELECT *
     FROM j
     WHERE j.city = 'London'
       AND NOT EXISTS
         (SELECT *
          FROM spj
          WHERE spj.p_id = p.p_id
            AND spj.j_id = j.j_id));
```

### 7.43. Установить номера поставщиков одной и той же детали для всех проектов (_SPJ не пустое отношение_)

#### Реляционная алгебра

`(SPJ [S#,P#,J#] DIVIDEBY J [J#]) [S#]`

#### Реляционное исчисление

```
SX.S# WHERE EXISTS PX FORALL JX EXISTS SPJY
(SPJY.S#=SX.S# AND SPJY.J#=JX.J# AND SPJY.P#=PX.P#)
```

#### SQL

```sql
SELECT s.s_id
FROM s
WHERE EXISTS
    (SELECT *
     FROM p
     WHERE NOT EXISTS
         (SELECT *
          FROM j
          WHERE NOT EXISTS
              (SELECT *
               FROM spj
               WHERE spj.s_id = s.s_id
                 AND spj.p_id = p.p_id
                 AND spj.j_id = j.j_id)));
```

### 7.44. Определить номера проектов, в состав которых входят, по меньшей мере, все типы деталей, поставляемых поставщиком с номером S1

#### Реляционная алгебра

`(J WHERE (MATCHING SPJ) [P#]>= (SPJ WHERE S#='S1') [P#]) [J#]`

#### Реляционное исчисление

```
JX.J# WHERE FORALL SPJY
(IF SPJY.S#='S1' THEN
EXISTS SPJZ (SPJZ.J#=JX.J# AND SPJZ.P#=SPJY.P#))
```

#### SQL

```sql
SELECT j.j_id
FROM j
WHERE NOT EXISTS
    (SELECT *
     FROM spj AS spj1
     WHERE spj1.s_id ='S1'
       AND NOT EXISTS
         (SELECT *
          FROM spj AS spj2
          WHERE spj2.p_id = spj1.p_id
            AND spj2.j_id = j.j_id));
```

### 7.45. Установить все города, в которых находится по крайней мере один поставщик, одна деталь или один проект

#### Реляционная алгебра

`S [City] UNION P [City] UNION J [City]`

#### Реляционное исчисление

```
RANGE OF VX IS (SX.City), (PX.City), (JX.City);
VX.City
```

#### SQL

```sql
SELECT s.city
FROM s
UNION
SELECT p.city
FROM p
UNION
SELECT j.city
FROM j;
```

### 7.46. Определить номера деталей, поставляемых либо лондонским поставщиком, либо для лондонского проекта

#### Реляционная алгебра

```
(SPJ JOIN (S WHERE City='London')) [P#]
UNION
(SPJ JOIN (J WHERE City='London')) [P#]
```

#### Реляционное исчисление

```
SPJX.P# WHERE EXISTS SX(SX.S#=SPJX.S# AND SX.City='London')
OR EXISTS JX(JX.J#=SPJX.J# AND JX.City='London')
```

#### SQL

```sql
SELECT DISTINCT spj.p_id
FROM spj
WHERE
    (SELECT s.city
     FROM s
     WHERE s.s_id = spj.s_id) = 'London'
  OR
    (SELECT j.city
     FROM j
     WHERE j.j_id = spj.j_id) = 'London';
```

### 7.47. Найти все пары "номер поставщика – номер детали", причем только такие, в которых данный поставщик не поставляет данную деталь

#### Реляционная алгебра

`(S TIMES P) [S#,P#] MINUS SP [S#,P#]`

#### Реляционное исчисление

```
(SX.S#, PX.P#) WHERE NOT EXISTS SPJX
(SX.S#=SPJX.S# AND PX.P#=SPJX.P#)
```

#### Из книги : только такие пары, в которых данный поставщик не поставляет данную деталь

```sql
SELECT s.s_id,
       p.p_id
FROM s
CROSS JOIN p
EXCEPT
SELECT spj.s_id,
       spj.p_id
FROM spj;
```

### 7.48. **TODO** Определить все пары номеров поставщиков (скажем, Sx и Sy), причем такие, что оба эти поставщика поставляют в точности одно и то же множество деталей

>[!WARNING]
>
>**Примечание**. Для упрощения в данном упражнении рекомендуется использовать первоначальную версию базы данных поставщиков и деталей, а не расширенную версию базы данных поставщиков, деталей и проектов.

### 7.49. **TODO** Подготовить в виде бинарного отношения "сгруппированную" версию всех поставок, в которой для каждой пары "номер поставщика - номер детали" показан соответствующий номер проекта и количество поставленных деталей

### 7.50. **TODO** Получить "разгруппированную" версию отношения, полученного в результате выполнения упражнения 7.49

## Листинг

### SQL скрипт инициализации базы данных для задания, `init.sql`

```sql
-- Creating the Suppliers table

CREATE TABLE s (
    s_id VARCHAR(2) PRIMARY KEY,
    sname VARCHAR(50),
    status INT,
    city VARCHAR(50)
);

-- Creating the Parts table

CREATE TABLE p (
    p_id VARCHAR(2) PRIMARY KEY,
    pname VARCHAR(50),
    color VARCHAR(20),
    weight DECIMAL(5, 2),
    city VARCHAR(50)
);

-- Creating the Projects table

CREATE TABLE j (
    j_id VARCHAR(2) PRIMARY KEY,
    jname VARCHAR(50),
    city VARCHAR(50)
);

-- Creating the SPJ (Suppliers-Parts-Projects) table

CREATE TABLE spj (
    s_id VARCHAR(2),
    p_id VARCHAR(2),
    j_id VARCHAR(2),
    qty INT,
    PRIMARY KEY (s_id, p_id, j_id),
    FOREIGN KEY (s_id) REFERENCES S(s_id),
    FOREIGN KEY (p_id) REFERENCES P(p_id),
    FOREIGN KEY (j_id) REFERENCES J(j_id)
);


-- Inserting data into the Suppliers table

INSERT INTO s (s_id, sname, status, city)
VALUES ('S1', 'Smith', 20, 'London'),
       ('S2', 'Jones', 10, 'Paris'),
       ('S3', 'Blake', 30, 'Paris'),
       ('S4', 'Clark', 20, 'London'),
       ('S5', 'Adams', 30, 'Athens');

-- Inserting data into the Parts table

INSERT INTO p (p_id, pname, color, weight, city)
VALUES ('P1', 'Nut', 'Red', 12.0, 'London'),
       ('P2', 'Bolt', 'Green', 17.0, 'Paris'),
       ('P3', 'Screw', 'Blue', 17.0, 'Oslo'),
       ('P4', 'Screw', 'Red', 14.0, 'London'),
       ('P5', 'Cam', 'Blue', 12.0, 'Paris'),
       ('P6', 'Cog', 'Red', 19.0, 'London');

-- Inserting data into the Projects table

INSERT INTO j (j_id, jname, city)
VALUES ('J1', 'Sorter', 'Paris'),
       ('J2', 'Display', 'Rome'),
       ('J3', 'OCR', 'Athens'),
       ('J4', 'Console', 'Athens'),
       ('J5', 'RAID', 'London'),
       ('J6', 'EDS', 'Oslo'),
       ('J7', 'Tape', 'London');

-- Inserting data into the SPJ table

INSERT INTO spj (s_id, p_id, j_id, qty)
VALUES ('S1', 'P1', 'J1', 200),
       ('S1', 'P1', 'J4', 700),
       ('S2', 'P3', 'J1', 400),
       ('S2', 'P3', 'J2', 200),
       ('S2', 'P3', 'J3', 200),
       ('S2', 'P3', 'J4', 500),
       ('S2', 'P3', 'J5', 600),
       ('S2', 'P3', 'J6', 400),
       ('S2', 'P3', 'J7', 800),
       ('S2', 'P5', 'J2', 100),
       ('S3', 'P3', 'J1', 200),
       ('S3', 'P4', 'J2', 500),
       ('S4', 'P6', 'J3', 300),
       ('S4', 'P6', 'J7', 300),
       ('S5', 'P2', 'J2', 200),
       ('S5', 'P2', 'J4', 100),
       ('S5', 'P5', 'J5', 500),
       ('S5', 'P5', 'J7', 100),
       ('S5', 'P6', 'J2', 200),
       ('S5', 'P1', 'J4', 100),
       ('S5', 'P3', 'J4', 200),
       ('S5', 'P4', 'J4', 800),
       ('S5', 'P5', 'J4', 400),
       ('S5', 'P6', 'J4', 500);
```

### SQL скрипт со всеми вариантами запросов

```sql
-- 7.13. Получить полные сведения обо всех проектах

SELECT *
FROM j;

-- 7.14. Получить полные сведения обо всех проектах в Лондоне

SELECT *
FROM j
WHERE city = 'London';

-- 7.15. Определить номера поставщиков деталей для проекта с номером J1

SELECT DISTINCT s_id
FROM spj
WHERE j_id='J1';

-- 7.16. Определить все поставки, в которых количество деталей находится в диапазоне от 300 до 750 штук включительно

SELECT *
FROM spj
WHERE qty >=300
  AND qty <=750;

-- 7.17. Найти все существующие сочетания вида "цвета детали - город, из которого поставляются детали"

SELECT DISTINCT color,
                city
FROM p;

-- 7.18. Найти все такие тройки значений "номер поставщика – номер детали – номер проекта", для которых указанные поставщик, деталь и проект находятся в одном городе

SELECT s.s_id,
       p.p_id,
       j.j_id
FROM s
JOIN p ON s.city = p.city
JOIN j ON p.city = j.city
AND s.city = j.city;

-- Или, может быть?

SELECT s.s_id,
       p.p_id,
       j.j_id
FROM s,
     p,
     j
WHERE s.city = p.city
  AND p.city = j.city;

-- 7.19. Найти все такие тройки значений "номер поставщика – номер детали – номер проекта", для которых указанные поставщик, деталь и проект не находятся в одном городе

SELECT s.s_id,
       p.p_id,
       j.j_id
FROM s,
     p,
     j
WHERE NOT (s.city = p.city
           AND p.city = j.city);

-- 7.20. Найти все такие тройки значений "номер поставщика – номер детали – номер проекта", для которых никакие из двух поставщиков, деталей и проектов не находятся в одном городе

SELECT s.s_id,
       p.p_id,
       j.j_id
FROM s,
     p,
     j
WHERE s.city <> p.city
  AND p.city <> j.city
  AND j.city <> s.city;

-- 7.21. Получить полные сведения о деталях, поставляемых поставщиком из Лондона

SELECT DISTINCT spj.p_id
FROM spj
WHERE
    (SELECT s.city
     FROM s
     WHERE s.s_id = spj.s_id) = 'London';

-- 7.22. Определить номера деталей, поставляемых поставщиком из Лондона для проекта в Лондоне

SELECT DISTINCT spj.p_id
FROM spj
WHERE
    (SELECT s.city
     FROM s
     WHERE s.s_id = spj.s_id) = 'London'
  AND
    (SELECT j.city
     FROM j
     WHERE j.j_id = spj.j_id) = 'London';

-- 7.23. Найти все пары названий городов, для которых поставщик из первого города обеспечивает проект во втором городе

SELECT DISTINCT s.city,
                j.city
FROM s,
     j
WHERE EXISTS
    (SELECT *
     FROM spj
     WHERE spj.s_id = s.s_id
       AND spj.j_id = j.j_id);

-- 7.24. Определить номера деталей, поставляемых для всех проектов, поставляемых поставщиком из того же города, в котором разрабатывается проект

SELECT DISTINCT spj.p_id
FROM spj
WHERE
    (SELECT s.city
     FROM s
     WHERE s.s_id = spj.s_id) =
    (SELECT j.city
     FROM j
     WHERE j.j_id = spj.j_id);

-- 7.25. Найти все номера проектов, детали для которых поставляются по крайней мере одним поставщиком из другого города

SELECT DISTINCT spj.j_id
FROM spj
WHERE
    (SELECT s.city
     FROM s
     WHERE s.s_id = spj.s_id) <>
    (SELECT j.city
     FROM j
     WHERE j.j_id = spj.j_id);

-- 7.26. Определить все пары номеров деталей, в которых обе детали поставляются  одним и тем же поставщиком

SELECT DISTINCT spj1.p_id,
                spj2.p_id
FROM spj AS spj1,
     spj AS spj2
WHERE spj1.s_id = spj2.s_id
  AND spj1.p_id < spj2.p_id;

-- 7.27. Определить общее число проектов, детали для которых поставляются поставщиком с номером S1

SELECT COUNT (DISTINCT spj.j_id)
FROM spj
WHERE spj.s_id = 'S1';

-- 7.28. Определить общее количество деталей с номером P1, поставляемых поставщиком с номером S1

SELECT sum(spj.qty)
FROM spj
WHERE spj.s_id = 'S1'
  AND spj.p_id = 'P1';

-- 7.29. Для каждой детали, поставляемой для проекта, определить номер детали, номер проекта и соответствующее общее количество

SELECT spj.p_id,
       spj.j_id,
       sum(spj.qty)
FROM spj
GROUP BY spj.p_id,
         spj.j_id;

-- 7.30. Определить номера деталей, поставляемых для некоторого проекта, со средним количеством, составляющим больше 350 штук

SELECT DISTINCT spj.p_id
FROM spj
GROUP BY spj.p_id,
         spj.j_id
HAVING avg(spj.qty) > 350;

-- 7.31. Определить названия проектов, детали для которых поставляются поставщиком с номером S1

SELECT DISTINCT j.jname
FROM j,
     spj
WHERE j.j_id = spj.j_id
  AND spj.s_id = 'S1';

-- 7.32. Определить цвета деталей, поставляемых поставщиком с номером S1

SELECT DISTINCT p.color
FROM p,
     spj
WHERE p.p_id = spj.p_id
  AND spj.s_id = 'S1';

-- 7.33. Установить номера деталей, поставляемых для любого проекта, разрабатываемого в Лондоне

SELECT DISTINCT spj.p_id
FROM spj,
     j
WHERE spj.j_id = j.j_id
  AND j.city = 'London';

-- 7.34. Определить номера проектов, в которых используется по крайней мере одна деталь, имеющуюся у поставщика с номером S1

SELECT DISTINCT spj1.j_id
FROM spj AS spj1,
     spj AS spj2
WHERE spj1.p_id = spj2.p_id
  AND spj2.s_id = 'S1';

-- 7.35. Определить номера поставщиков по крайней мере одной детали, поставляемой по крайней мере одним поставщиком, который поставляет хотя бы одну деталь красного цвета

SELECT DISTINCT spj1.s_id
FROM spj AS spj1,
     spj AS spj2,
     spj AS spj3
WHERE spj1.p_id = spj2.p_id
  AND spj2.s_id = spj3.s_id
  AND
    (SELECT p.color
     FROM p
     WHERE p.p_id = spj3.p_id) = 'Red';

-- 7.36. Определить номера поставщиков со статусом, меньшим, чем у поставщика с номером S1

SELECT s.s_id
FROM s
WHERE s.status <
    (SELECT s.status
     FROM s
     WHERE s.s_id = 'S1');

-- 7.37. Определить номера проектов, разрабатываемых в городе, который находится на первом месте в алфавитном списке таких городов

SELECT j.j_id
FROM j
WHERE j.city =
    (SELECT MIN (j.city)
     FROM j);

-- 7.38. Определить номера проектов, для которых среднее количество поставляемых деталей с номером P1 больше, чем наибольшее количество любых деталей, поставляемых для проекта с номером J1

SELECT DISTINCT spj1.j_id
FROM spj AS spj1
WHERE spj1.p_id = 'P1'
  AND
    (SELECT AVG (spj2.qty)
     FROM spj AS spj2
     WHERE spj2.j_id = spj1.j_id
       AND spj2.p_id = 'P1') >
    (SELECT max(spj3.qty)
     FROM spj AS spj3
     WHERE spj3.j_id = 'J1');

-- 7.39. Определить номера поставщиков детали с номером Р1 для некоторого проекта в количестве, большем среднего количества деталей с номером Р1, поставляемых для этого проекта

SELECT DISTINCT spj1.s_id
FROM spj AS spj1
WHERE spj1.p_id = 'P1'
  AND spj1.qty >
    (SELECT avg(spj2.qty)
     FROM spj AS spj2
     WHERE spj2.p_id = 'P1'
       AND spj2.j_id = spj1.j_id);

-- 7.40. Найти номера проектов, для которых поставщиками из Лондона не поставляется ни одна деталь красного цвета

SELECT j.j_id
FROM j
WHERE NOT EXISTS
    (SELECT *
     FROM spj,
          p,
          s
     WHERE spj.j_id = j.j_id
       AND spj.p_id = p.p_id
       AND spj.s_id = s.s_id
       AND p.color = 'Red'
       AND s.city = 'London');

-- 7.41. Определить номера проектов, детали для которых полностью поставляются поставщиком с номером S1

SELECT j.j_id
FROM j
WHERE NOT EXISTS
    (SELECT *
     FROM spj
     WHERE spj.j_id = j.j_id
       AND NOT (spj.s_id = 'S1'));

-- 7.42 (**ПЕРЕПРОВЕРИТЬ**) Определить номера деталей, поставляемых для лондонских проектов

SELECT p.p_id
FROM p
WHERE NOT EXISTS
    (SELECT *
     FROM j
     WHERE j.city = 'London'
       AND NOT EXISTS
         (SELECT *
          FROM spj
          WHERE spj.p_id = p.p_id
            AND spj.j_id = j.j_id));

-- 7.43. Установить номера поставщиков одной и той же детали для всех проектов (*SPJ не пустое отношение*)

SELECT s.s_id
FROM s
WHERE EXISTS
    (SELECT *
     FROM p
     WHERE NOT EXISTS
         (SELECT *
          FROM j
          WHERE NOT EXISTS
              (SELECT *
               FROM spj
               WHERE spj.s_id = s.s_id
                 AND spj.p_id = p.p_id
                 AND spj.j_id = j.j_id)));

-- 7.44. Определить номера проектов, в состав которых входят, по меньшей мере, все типы деталей, поставляемых поставщиком с номером S1

SELECT j.j_id
FROM j
WHERE NOT EXISTS
    (SELECT *
     FROM spj AS spj1
     WHERE spj1.s_id ='S1'
       AND NOT EXISTS
         (SELECT *
          FROM spj AS spj2
          WHERE spj2.p_id = spj1.p_id
            AND spj2.j_id = j.j_id));

-- 7.45. Установить все города, в которых находится по крайней мере один поставщик, одна деталь или один проект

SELECT s.city
FROM s
UNION
SELECT p.city
FROM p
UNION
SELECT j.city
FROM j;

-- 7.46. Определить номера деталей, поставляемых либо лондонским поставщиком, либо для лондонского проекта

SELECT DISTINCT spj.p_id
FROM spj
WHERE
    (SELECT s.city
     FROM s
     WHERE s.s_id = spj.s_id) = 'London'
  OR
    (SELECT j.city
     FROM j
     WHERE j.j_id = spj.j_id) = 'London';

-- 7.47. Найти все пары "номер поставщика – номер детали", причем только такие, в которых данный поставщик не поставляет данную деталь

SELECT s.s_id,
       p.p_id
FROM s
CROSS JOIN p
EXCEPT
SELECT spj.s_id,
       spj.p_id
FROM spj;

-- 7.48. **TODO** Определить все пары номеров поставщиков (скажем, Sx и Sy), причем такие, что оба эти поставщика поставляют в точности одно и то же множество деталей

-- 7.49. **TODO** Подготовить в виде бинарного отношения "сгруппированную" версию всех поставок, в которой для каждой пары "номер поставщика - номер детали" показан соответствующий номер проекта и количество поставленных деталей

-- 7.50. **TODO** Получить "разгруппированную" версию отношения, полученного в результате выполнения упражнения 7.49
```
