# Домашняя работа по дисциплине "Проектирование и сопровождение баз данных"
## Домашняя работа 1

<details>
<summary>Развернуть</summary>

---

### Задание

Подумать над выбором предметной области для выполнения финальной (экзаменационной) работы.
Выбирайте предметную область, которая вам интересна
и в которой вы разбираетесь или хотите разобраться.

Сделать краткое описание выбранной предметной области (1-2 страницы).
Если описание получится более объемным, не беда.
Ведь это описание затем войдет в финальный отчет.

Попытаться сформулировать требования к будущей базе данных.

### Работа

#### Цель работы

Описать словесно выбранную предметную область – "Риэлторская контора".

#### Описание предметной области

База предложений: район и адрес, характеристика дома и квартиры, запрашиваемая стоимость, координаты заявителя. База спроса: требования покупателя к жилью (возможно несколько вариантов, допустимые диапазоны), финансовые возможности, координаты заявителя. Подбор вариантов для той и другой стороны, автоматизированный поиск взаимоприемлемых вариантов, фиксация сделки. Пример запроса покупателя: однокомнатная, до 200 тыс. р., центр не предлагать.

[Наверх](#ссылки)

---

</details>

## Домашняя работа 2

<details>
<summary>Развернуть</summary>

---

### Задание

Глава 3 (задания 1-4)

### Работа

#### Задание 1

Попробуйте ввести в таблицу aircrafts строку с таким значением атрибута
«Код самолета» (aircraft_code), которое вы уже вводили, возьмём пример из книги:

```sql
INSERT INTO aircraft
  VALUES ('SU9', 'Sukhoi SUperJet-100', 300);
```
Получаем ошибку:
```sql
ERROR:  0A000: cannot insert into column "model" of view "aircrafts"
DETAIL:  View columns that are not columns of their base relation are not updatable.
```

Ошибка нам говорит о том, что данная операция не может быть выполнена, потому что "aircraft_code" является первичным ключом и должен быть уникальным, "aircraft_code" равный SU9 уже существует

#### Задание 2

  Самостоятельно напишите команду для выборки всех строк из таблицы aircrafts, чтобы строки были упорядочены по убыванию значения атрибута «Максимальная дальность полета, км» (range).

```sql
SELECT * 
FROM aircrafts 
ORDER BY range DESC;
```
Полученный результат:
```sql
 aircraft_code |        model        | range
---------------+---------------------+-------
 773           | Боинг 777-300       | 11100
 763           | Боинг 767-300       |  7900
 319           | Аэробус A319-100    |  6700
 320           | Аэробус A320-200    |  5700
 321           | Аэробус A321-200    |  5600
 733           | Боинг 737-300       |  4200
 SU9           | Сухой Суперджет-100 |  3000
 CR2           | Бомбардье CRJ-200   |  2700
 CN1           | Сессна 208 Караван  |  1200
```

#### Задание 3

Самостоятельно напишите команду UPDATE полностью, при
этом не забудьте, что увеличить дальность полета нужно только у одной модели — Sukhoi SuperJet, поэтому необходимо использовать условие WHERE. Затем
с помощью команды SELECT проверьте полученный результат.
Пример такого запроса:

```sql
UPDATE aircrafts SET range = range * 2
  WHERE model = 'Sukhoi SuperJet-100';
SELECT range 
  FROM aircrafts 
  WHERE model = 'Sukhoi SuperJet-100';
 ```

#### Задание 4

Самостоятельно смоделируйте описанную ситуацию, подобрав условие, которому гарантированно не соответствует ни одна строка в таблице «Самолеты»
(aircrafts).

```sql
DELETE FROM aircrafts WHERE model = 'Kukuruznik';
```

[Наверх](#ссылки)

---

</details>

## Домашняя работа 3

<details>
<summary>Сама работа</summary>

---

### Задание

Глава 4 (упражнения 2, 4, 8, 12, 15, 21, 30, 33, 35)

### Работа

#### Задание 2

Сделаем выборку из таблицы и посмотрим, что все эти разнообразные
значения сохранены именно в том виде, как мы их вводили.

```sql
CREATE TABLE test_numeric (measurement numeric, description text);
INSERT INTO test_numeric 
VALUES (1234567890.0987654321, 'Точность 20 знаков, масштаб 10 знаков'),
       (1.5, 'Точность 2 знака, масштаб 1 знак'),
       (0.12345678901234567890, 'Точность 21 знак, масштаб 20 знаков'),
       (1234567890, 'Точность 10 знаков, масштаб 0 знаков (целое число)');
```
Проверим, что у нас сохранилось в таблице
```sql
SELECT * 
  FROM test_numeric;
DROP TABLE test_numeric;
```
Ниже приведена таблица:
sql```
      measurement       |                    description
------------------------+----------------------------------------------------
  1234567890.0987654321 | Точность 20 знаков, масштаб 10 знаков
                    1.5 | Точность 2 знака, масштаб 1 знак
 0.12345678901234567890 | Точность 21 знак, масштаб 20 знаков
             1234567890 | Точность 10 знаков, масштаб 0 знаков (целое число)
```

#### Задание 4

Необходимо самостоятельно провести аналогичные эксперименты с очень большими числами, находящимися на границе допустимого диапазона для чисел типов real
и double precision

```sql
SELECT 2e-38::real > 1e-38::real;
 ?column?
----------
 false

SELECT 4e+310::double precision < 3e+310::double precision;
 ?column?
----------
 false
```

#### Упражнение 8

Выполним некоторые команды для добавления строк в таблицу и удаления одной строки из нее
                                                
```sql
CREATE TABLE test_serial (PRIMARY KEY(id), id serial, name text);

INSERT INTO test_serial (name) VALUES ('Вишневая');
```

Явно зададим значение столбца id:

```sql
INSERT INTO test_serial (id, name) VALUES (2, 'Прохладная');
```

Таким образом мы нарушаем мы условие уникальности первичного ключа, ведь мы указываем id явно0, но последовательность для id не обновляется и при добавлении следующей записи id всё ещё равно 2 

```sql
INSERT INTO test_serial (name) VALUES ('Грушевая');
```
Нам выдаёт ошибку:
```sql
ERROR:  23505: duplicate key value violates unique constraint "test_serial_pkey"
DETAIL:  Key (id)=(2) already exists.
```
То есть запись с id = 2 уже существует
```sql                                          
INSERT INTO test_serial (name) VALUES ('Грушевая');
INSERT INTO test_serial (name) VALUES ('Зеленая');

DELETE FROM test_serial WHERE id = 4;
DELETE 1
INSERT INTO test_serial (name) VALUES ('Луговая');

```
Выведем получившуюся таблицу

```sql
SELECT * 
 FROM test_serial;
DROP TABLE test_serial;
 id |    name
----+------------
  1 | Вишневая
  2 | Прохладная
  3 | Грушевая
  5 | Луговая
```

#### Задание 12

Самостоятельно выполним команды SELECT, приведенные в учебнике, как для значения типа date, так и для значения типа timestamp.
```sql
SHOW datestyle;
 DateStyle
-----------
 ISO, DMY

SET datestyle to 'MDY';

SHOW datestyle;
 DateStyle
-----------
 ISO, MDY

SELECT '18-05-2016'::date;
```
Поскольку мы поменяли формат на месяц-день-год, а 18 месяца не существует, нам выдало соответствующую ошибку:
```sql
ERROR:  22008: date/time field value out of range: "18-05-2016"
LINE 1: SELECT '18-05-2016'::date;
               ^
HINT:  Perhaps you need a different "datestyle" setting.
```
Выберем иную дату
```sql
SELECT '05-18-2016'::date;
    date    
------------
 2016-05-18
```
Вернём нормальный формат и посмотрим отображение в различных стилях
```sql
SET datestyle TO 'Postgres, DMY';

SHOW datestyle;
   DateStyle
---------------
 Postgres, DMY

SELECT current_date;
 current_date
--------------
 14-12-2022

SET datestyle to 'SQL, DMY';
SELECT current_date;
 current_date
--------------
 14/12/2022

SET datestyle to 'German, DMY';
SELECT current_date;
 current_date
--------------
 14.12.2022
```

#### Задание 15

Проэкспериментируем с функцией to_char.

```sql
SELECT to_char(current_timestamp, 'dd.mm');
---------
 14.12

SELECT to_char(current_timestamp, 'MM/DD/YYYY');
------------
 12/14/2022

SELECT to_char(current_timestamp, 'yyyy MONTHdd' ); 
------------
 2022 DECEMBER 14
```

#### Задание 21

Сначала надо сделать обоснованные предположения о результатах следующих двух команд, а затем проверить предположения на практике и проанализируйте полученные результаты. SQL Учитывает, сколько дней в месяце, 2016 год високосный, поэтому
```sql
SELECT ( '2016-01-31'::date + '1 mon'::interval ) AS new_date;
```
Выведет 29 февраля, а
```sql
SELECT ( '2016-02-29'::date + '1 mon'::interval ) AS new_date;
```
29 марта соответственно

```sql
SELECT ('2016-01-31'::date +'1 mon'::interval) AS new_date;
      new_date
---------------------
 2016-02-29 00:00:00

SELECT ('2016-02-29'::date +'1 mon'::interval) AS new_date;
      new_date
---------------------
 2016-03-29 00:00:00
```

#### Задание 30

Нужно предположить, какие из приведенных ниже команд содержат ошибку?
                                                
Предположения:

```sql
INSERT INTO test_bool VALUES (TRUE, 'yes');
--TRUE - ключевое слово для булевого типа +
INSERT INTO test_bool VALUES (yes, 'yes');
--А "yes" — уже нет -
INSERT INTO test_bool VALUES ('yes', true);
--Второй аргумент неявно преобразуется в строку +
INSERT INTO test_bool VALUES ('yes', TRUE);
-- В целом, как и выше +

INSERT INTO test_bool VALUES ('1', 'true');
-- '1' преобразуется в 1 тот в true +

INSERT INTO test_bool VALUES (1, 'true');
-- 1 не зарезервирован под boolean - 

INSERT INTO test_bool VALUES ('t', 'true');
-- 't' зарезервирована под тип boolean и неявным образом преобразуется в true +

INSERT INTO test_bool VALUES ('t', truth);
-- truth не зарезервирован под boolean -

INSERT INTO test_bool VALUES (true, true);
-- true неявным образом преобразуется в строку +

INSERT INTO test_bool VALUES (1::boolean, 'true');
-- Конвертация любого числа, кроме 0, в boolean дает TRUE +

INSERT INTO test_bool VALUES (111::boolean, 'true');
-- Как в предыдущем +
```

#### Задание 33

Для учета пожеланий пилотов необходимо модифицировать
таблицу pilots
                                                
```sql
CREATE TABLE pilots(pilot_name text, schedule integer[], meal text[][]);
INSERT INTO pilots 
VALUES( 'Ivan', '{ 1, 3, 5, 6, 7 }'::integer[],
        '{ 
            { "сосиска", "макароны", "кофе" }, 
            { "куриное филе", "пюре", "какао" }, 
            { "рагу", "сэндвич с семгой", "морс ягодный" }, 
            { "шарлотка яблочная", "гречка", "компот вишевый" }, 
            { "омлет с овощами", "бекон", "кофе" } 
        }'::text[][]
        ),
        ( 
        'Petr', '{ 1, 2, 5, 7 }'::integer[],
        '{ 
            { "котлета", "каша", "кофе" },
            { "куринная отбивная", "рис", "компот" },
            { "манная каша", "билины с мясом", "компот" },
            { "мясо запеченное", "пюре", "какао" } 
        }'::text[][]
        ),
        ( 
            'Pavel', '{ 2, 5 }'::integer[],
            '{ 
                { "сосиска", "каша", "кофе" },
                { "мясо запеченное", "пюре", "какао" }
            }'::text[][]
        ),
        ( 
            'Boris', '{ 3, 5, 6 }'::integer[],
            '{ 
                { "котлета", "каша", "чай" },
                { "куринная отбивная", "рис", "компот" },
                { "сосиска", "макароны", "кофе" }
            }'::text[][]
        );
```
Изменим пилота Pavel
```sql                                       
UPDATE pilots 
   SET schedule[2] = 7, 
       meal[1][:] = '{ "котлета", "каша", "какао" }' :: text[]
 WHERE pilot_name='Pavel';

SELECT * 
  FROM pilots 
 WHERE pilot_name='Pavel';


DROP TABLE pilots;
 pilot_name | schedule |           meal                                           
 Pavel      | {2,7}  | {{ "котлета", "каша", "какао" },
                          {"куринная отбивная",рис,компот},
                          {сосиска,макароны,кофе}}
```
#### Задание 35

```sql
SELECT '[{"метро":"поезд"},{"такси":"машина"}]'::json -> 1;
      ?column?
---------------------
{"такси":"машина"}

SELECT '["рука","нога","голова"]'::json ->> 0;
 ?column?
----------
 рука

SELECT to_json('Hello world!'::text);
/* 
       to_json       
---------------------
 "Hello world!"
(1 row)*/

SELECT json_build_object('ключ', 'значение', 'ещёключ', 'ещёзначение');
/* 
   json_build_object    
------------------------
 {"ключ":"значение","ещёключ":'ещёзначение'}
*/
```

[Наверх](#ссылки)

---

</details>
