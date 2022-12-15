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

Глава 3 (Задания 1-4)

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

Глава 4 (Задания 2, 4, 8, 12, 15, 21, 30, 33, 35)

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
```sql
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
                          {"куринная отбивная","рис","компот"},
                          {"сосиска","макароны","кофе"}}
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

## Домашняя работа 4

<details>
<summary>Развернуть</summary>

---

### Задание

Глава 5 (Задания 2, 9, 17, 18)

### Работа

#### Задание 2

Посмотрите, какие ограничения уже наложены на атрибуты таблицы «Успеваемость» (progress).
В качестве примера рассмотрим такой вариант. Добавьте в таблицу progress еще один атрибут — «Форма проверки знаний» (test_form), который может принимать только два значения: «экзамен» или «зачет». Тогда набор допустимых значений атрибута «Оценка» (mark) будет зависеть от того, экзамен или зачет предусмотрены по данной дисциплине. Если предусмотрен экзамен, тогда допускаются значения 3, 4, 5, если зачет — тогда 0 (не зачтено) или 1 (зачтено).

```sql
                                Таблица "public.progress"
   Столбец   |         Тип          | Правило сортировки | Допустимость NULL | По умолчанию
-------------+----------------------+--------------------+-------------------+------
 record_book | numeric(5,0)         |                    | not null          |
 subject     | text                 |                    | not null          |
 acad_year   | text                 |                    | not null          |
 term        | numeric(1,0)         |                    | not null          |
 mark        | numeric(1,0)         |                    | not null          | 5
 test_form   | character varying(7) |                    |                   |
Ограничения-проверки:
    "progress_mark_check" CHECK (mark >= 3::numeric AND mark <= 5::numeric)
    "progress_term_check" CHECK (term = 1::numeric OR term = 2::numeric)
Ограничения внешнего ключа:
    "progress_record_book_fkey" FOREIGN KEY (record_book) REFERENCES students(record_book) ON UPDATE CASCADE ON DELETE CASCADE
```
Добавим в таблицу progress еще один атрибут test_form, принимающий значения: «экзамен» или «зачет». Если предусмотрен экзамен, тогда допускаются значения 2, 3, 4, 5, если зачет — тогда 0 (не зачтено) или 1 (зачтено).
```sql
ALTER TABLE progress
 ADD COLUMN test_form text; 

ALTER TABLE progress                         
  ADD CHECK ((test_form = 'экзамен' AND mark IN (2,3,4,5))
              OR 
             (test_form = 'зачет' AND mark IN (0, 1))
);
```
Проверим, как будет работать новое ограничение в модифицированной таблице progress. Для этого выполним команды INSERT, как удовлетворяющие ограничению, так и нарушающие его
```sql
INSERT INTO students VALUES (7, 'Grigoriev', 666, 14159);

INSERT INTO progress VALUES (4, 'Mathematics', '2',2, 3, 'экзамен');

INSERT INTO progress VALUES (4, 'Mathematics', '2',2, 0, 'зачет');
ОШИБКА: новая строка в отношении “progress” нарушает ограничение-проверку "progress_mark_check"
ПОДРОБНОСТИ: Ошибочная строка содержит (4, 'Mathematics', '2',2, 0, 'зачет').
```

В таблице уже было ограничение на допустимые значения атрибута mark. Как вы думаете, не будет ли оно конфликтовать с новым ограничением? Проверьте эту гипотезу. Если ограничения конфликтуют, тогда удалите старое ограничение и снова попробуйте добавить строки в таблицу.

Они конфликтуют, в таком случае удалим ограничение, и попробуем добавить строки снова.

```sql
ALTER TABLE progress 
DROP CONSTRAINT progress_mark_check;
INSERT INTO progress 
VALUES (4, 'Mathematics', '2',2, 0, 'зачет');

```

#### Упражнение 9

В таблице «Студенты» (students) есть текстовый атрибут name, на который наложено ограничение NOT NULL. Как вы думаете, что будет, если при вводе новой строки в эту таблицу дать атрибуту name в качестве значения пустую строку?

```sql
INSERT INTO students VALUES (2665, '', 4242, 89655);
```

Добавим ограничение ( name <> '' )

```sql
ALTER TABLE students 
ADD CHECK ( name <> '' );
INSERT INTO students 
VALUES (2665, '', 4242, 89655);
ОШИБКА: новая строка в отношении “progress” нарушает ограничение-проверку
"students_name_check"
```

Посмотрим что теперь будет при вставке строки с пустым значением

```sql
INSERT INTO students VALUES (2665, '', 4242, 89655);
```

Добавим ограничение ( trim (name) <> '' )

```sql
ALTER TABLE students 
ADD CHECK (trim (name) <> '');
INSERT INTO students 
VALUES (2665, '', 4242, 89655);
ОШИБКА: новая строка в отношении “progress” нарушает ограничение-проверку
"students_name_check"
```

Есть ли подобные слабые места в таблице «Успеваемость» (progress)?
В поля с текстовым типом можно добавить пустые строки.

#### Упражнение 17

Подумайте, какие представления было бы целесообразно создать для нашей базы данных «Авиаперевозки». Необходимо учесть наличие различных групп пользователей, например: пилоты, диспетчеры, пассажиры, кассиры. Создайте представления и проверьте их в работе.

Время рейсов из Москвы

```sql
CREATE VIEW timeof_moscow_flights AS SELECT
flight_no,
scheduled_departure,
departure_city,
arrival_city 
FROM flights f
  WHERE bookings.airports.city = 'Москва'
LEFT JOIN aircrafts a on f.aircraft_code = a.aircraft_code;
CREATE VIEW
SELECT * FROM  dispatcher_info;
 flight_no |  scheduled_departure   |  departure_city   | arrival_city          
-----------+------------------------+-----------+---------------------
 PG0405    | 2016-09-13 08:35:00+03 | Москва            | Санкт-Петербург   
 PG0404    | 2016-10-03 18:05:00+03 | Москва            | Санкт-Петербург     
 PG0405    | 2016-10-03 08:35:00+03 | Москва            | Санкт-Петербург    
 PG0402    | 2016-11-07 11:25:00+03 | Москва            | Санкт-Петербург    
 PG0405    | 2016-10-14 08:35:00+03 | Москва            | Санкт-Петербург     
 PG0404    | 2016-10-14 18:05:00+03 | Москва            | Санкт-Петербург     
 PG0403    | 2016-10-14 10:25:00+03 | Москва            | Санкт-Петербург     
 PG0402    | 2016-10-14 11:25:00+03 | Москва            | Санкт-Петербург     
 PG0405    | 2016-10-23 08:35:00+03 | Москва            | Санкт-Петербург     
 PG0402    | 2016-10-21 11:25:00+03 | Москва            | Санкт-Петербург     
 PG0403    | 2016-10-21 10:25:00+03 | Москва            | Санкт-Петербург     
 PG0404    | 2016-10-21 18:05:00+03 | Москва            | Санкт-Петербург    
 PG0405    | 2016-10-21 08:35:00+03 | Москва            | Санкт-Петербург     
 PG0402    | 2016-10-04 11:25:00+03 | Москва            | Санкт-Петербург     
 PG0402    | 2016-09-25 11:25:00+03 | Москва            | Санкт-Петербург     
```

#### Упражнение 18

Подумайте, какие еще таблицы было бы целесообразно дополнить столбцами типа json/jsonb. Вспомните, что, например, в таблице «Билеты» (tickets) уже есть столбец такого типа — contact_data. Выполните модификации таблиц и измените в них одну-две строки для проверки правильности ваших решений.

В таблицу bookings в качестве json поля можно добавить информамцию о периоде действия брони.

```sql
ALTER TABLE bookings ADD COLUMN booking_period jsonb;
ALTER TABLE

UPDATE bookings
SET booking_period='{"booking_start": "06.10.2020", "booking_end": "16.10.2020"}'
WHERE book_ref='000181';

SELECT * FROM bookings WHERE book_ref='000181';
 book_ref |       book_date        | total_amount |   booking_period
----------+------------------------+--------------+-------------------------------------------------------------
 000181   | 2016-10-08 12:28:00+03 |    131800.00 | {"booking_end": "16.10.2020", "booking_start": "06.10.2020"}
```

[Наверх](#ссылки)

---

</details>
## Домашняя работа 5

<details>
<summary>Развернуть</summary>

---

### Задание

Глава 6 (Задания 2, 7, 9, 13, 19, 21, 23)

### Работа

#### Задание 2

Этот запрос выбирает из таблицы «Билеты» (tickets) всех пассажиров с именами, состоящими из трех букв (в шаблоне присутствуют три символа «_»): 

```sql
SELECT passenger_name
FROM tickets
WHERE passenger_name LIKE '___ %';
```

Предложите шаблон поиска в операторе LIKE для выбора из этой таблицы всех пассажиров с фамилиями, состоящими из пяти букв.

```sql
SELECT passenger_name
FROM tickets
WHERE passenger_name LIKE '% _____';
```
Вывод:
```sql
   passenger_name  
------------------
 ILYA POPOV
 VLADIMIR POPOV
 PAVEL GUSEV
 LEONID ORLOV
 EVGENIY GUSEV
 NIKOLAY FOMIN
 EKATERINA ILINA
 ANTON POPOV
 ARTEM BELOV
 VLADIMIR POPOV
 ALEKSEY ISAEV
 ...
```
Проверим, сколько имён удовлетворяют запросу
```sql
SELECT count(passenger_name)
FROM tickets
WHERE passenger_name LIKE '% _____';
 count 
-------
 14272
```

#### Задание 7

Самые крупные самолеты в нашей авиакомпании — это Boeing 777-300. Выяснить, между какими парами городов они летают, поможет запрос: 

```sql
SELECT DISTINCT departure_city, arrival_city
FROM routes r
JOIN aircrafts a ON r.aircraft_code = a.aircraft_code
WHERE a.model = 'Boeing 777-300'
ORDER BY 1;
```

Модифицируйте запрос таким образом, чтобы каждая пара городов была выведена только один раз

```sql
SELECT DISTINCT r.departure_city, r.arrival_city
FROM routes r
  JOIN routes rr 
  ON r.arrival_city = rr.departure_city
  AND rr.arrival_city = r.departure_city 
  AND r.arrival_city > rr.arrival_city
  JOIN aircrafts a ON r.aircraft_code = a.aircraft_code
WHERE a.model = 'Boeing 777-300'
ORDER BY 1;
```
Получим
```sql
 departure_city | arrival_city
----------------+--------------
 Екатеринбург   | Москва
 Москва         | Новосибирск
 Москва         | Пермь
 Москва         | Сочи
(4 строки)
```

#### Задание 9

Для ответа на вопрос, сколько рейсов выполняется из Москвы в Санкт-Петербург, можно написать совсем простой запрос: 

```sql
SELECT count( * )
FROM routes
WHERE departure_city = 'Москва'
AND arrival_city = 'Санкт-Петербург'
```

А с помощью какого запроса можно получить результат в таком виде?

```sql
 departure_city |  arrival_city   | count
----------------+-----------------+-------
 Москва         | Санкт-Петербург |    12
```
Например так:
```sql
SELECT  departure_city, arrival_city, count(*)
FROM routes
WHERE departure_city = 'Москва'
AND arrival_city = 'Санкт-Петербург'
GROUP BY departure_city, arrival_city;
 departure_city |  arrival_city   | count
----------------+-----------------+-------
 Москва         | Санкт-Петербург |    12
(1 строка)
```

#### Задание 13

Ответить на вопрос о том, каковы максимальные и минимальные цены билетов на все направления, может такой запрос: 

```sql
SELECT
f.departure_city,
f.arrival_city,
max( tf.amount ),
min( tf.amount )
FROM flights_v f
JOIN ticket_flights tf 
  ON f.flight_id = tf.flight_id
GROUP BY 1, 2
ORDER BY 1, 2;
```

А как выявить те направления, на которые не было продано ни одного билета?

```sql
SELECT
    f.departure_city,
    f.arrival_city,
    max( tf.amount ),
    min( tf.amount )
FROM flights_v f
    LEFT JOIN ticket_flights tf 
    ON f.flight_id = tf.flight_id
GROUP BY 1, 2
ORDER BY 1, 2;
```
Получаем результат:
```sql
      departure_city      |       arrival_city       |    max    |   min
--------------------------+--------------------------+-----------+----------
 Абакан                   | Архангельск              |           |
 Абакан                   | Грозный                  |           |
 Абакан                   | Кызыл                    |           |
 Абакан                   | Москва                   | 101000.00 | 33700.00
 Абакан                   | Новосибирск              |   5800.00 |  5800.00
 Абакан                   | Томск                    |   4900.00 |  4900.00
 Анадырь                  | Москва                   | 185300.00 | 61800.00
 Анадырь                  | Хабаровск                |  92200.00 | 30700.00
 Анапа                    | Белгород                 |  18900.00 |  6300.00
 Анапа                    | Москва                   |  36600.00 | 12200.00
```

#### Задание 19

В разделе 6.4 мы использовали рекурсивный алгоритм в общем табличном выражении. Изучите этот пример, чтобы лучше понять работу рекурсивного алгоритма:

```sql
WITH RECURSIVE ranges ( min_sum, max_sum )
AS (
VALUES( 0, 100000 ),
( 100000, 200000 ),
( 200000, 300000 )
UNION ALL
SELECT min_sum + 100000, max_sum + 100000
FROM ranges
WHERE max_sum < ( SELECT max( total_amount ) FROM bookings )
)
SELECT * FROM ranges;
```

##### Подзадание 1

Модифицируйте запрос, добавив в него столбец level (можно назвать его и iteration). Этот столбец должен содержать номер текущей итерации, поэтому нужно увеличивать его значение на единицу на каждом шаге. Не забудьте задать начальное значение для добавленного столбца в предложении VALUES.

```sql
WITH RECURSIVE ranges ( min_sum, max_sum, iter )
  AS (
    VALUES( 0, 100000, 1 ), ( 100000, 200000, 1 ), ( 200000, 300000, 1 )
  UNION ALL
  SELECT min_sum + 100000, max_sum + 100000 , iter + 1
  FROM ranges
  WHERE max_sum < ( SELECT max( total_amount ) FROM bookings ))
SELECT * FROM ranges;
```
Результат
```sql
 min_sum | max_sum | iter
---------+---------+------
       0 |  100000 |    1
  100000 |  200000 |    1
  200000 |  300000 |    1
  100000 |  200000 |    2
  200000 |  300000 |    2
  300000 |  400000 |    2
  200000 |  300000 |    3
  300000 |  400000 |    3
  400000 |  500000 |    3
  300000 |  400000 |    4
  400000 |  500000 |    4
  500000 |  600000 |    4
  400000 |  500000 |    5
  500000 |  600000 |    5
  600000 |  700000 |    5
```

##### Подзадание 2

Для завершения экспериментов замените UNION ALL на UNION и выполните запрос. Сравните этот результат с предыдущим, когда мы использовали UNION ALL.

```sql
WITH RECURSIVE ranges ( min_sum, max_sum )
  AS (
    VALUES( 0, 100000 ), ( 100000, 200000 ), ( 200000, 300000 )
    UNION
    SELECT min_sum + 100000, max_sum + 100000
    FROM ranges
    WHERE max_sum < ( SELECT max( total_amount ) FROM bookings ))
SELECT * FROM ranges;
```
Результат
```sql
 min_sum | max_sum
---------+---------
       0 |  100000
  100000 |  200000
  200000 |  300000
  300000 |  400000
  400000 |  500000
  500000 |  600000
  600000 |  700000
  700000 |  800000
  800000 |  900000
  900000 | 1000000
 1000000 | 1100000
 1100000 | 1200000
 1200000 | 1300000
(13 строк)
```

#### Задание 21

В тексте главы был приведен запрос, выводящий список городов, в которые нет рейсов из Москвы.

```sql
SELECT DISTINCT a.city
FROM airports a
WHERE NOT EXISTS (
SELECT * FROM routes r
WHERE r.departure_city = 'Москва'
AND r.arrival_city = a.city
)
AND a.city <> 'Москва'
ORDER BY city;
```

Можно предложить другой вариант, в котором используется одна из операций над множествами строк: объединение, пересечение или разность.
Вместо знака «?» поставьте в приведенном ниже запросе нужное ключевое слово — UNION, INTERSECT или EXCEPT — и обоснуйте ваше решение.

```sql
SELECT city
FROM airports
WHERE city <> 'Москва'
?
SELECT arrival_city
FROM routes
WHERE departure_city = 'Москва'
ORDER BY city;
```

Используем EXCEPT, потому что это исключение, в нашем случае городов, в которые нет рейсов из Москвы.

#### Задание 22

В тексте главы мы рассматривали такой запрос: получить перечень аэропортов в тех городах, в которых больше одного аэропорта.

```sql
SELECT aa.city, aa.airport_code, aa.airport_name
FROM (
SELECT city, count( * )
FROM airports
GROUP BY city
HAVING count( * ) > 1
) AS a
JOIN airports AS aa ON a.city = aa.city
ORDER BY aa.city, aa.airport_name;
```

Как вы думаете, обязательно ли наличие функции count в подзапросе в предложении SELECT или можно написать просто SELECT city FROM airports ?

 - Обязательно, поскольку без count выведутся города, с одним аэропортом.

```sql
SELECT aa.city, aa.airport_code, aa.airport_name, count
  FROM (
    SELECT city, count( * ) as count
    FROM airports
    GROUP BY city
  ) AS a
JOIN airports AS aa ON a.city = aa.city
ORDER BY aa.city, aa.airport_name;
           city           | airport_code |     airport_name     | count
--------------------------+--------------+----------------------+-------
 Абакан                   | ABA          | Абакан               |     1
 Анадырь                  | DYR          | Анадырь              |     1
 Анапа                    | AAQ          | Витязево             |     1
 Архангельск              | ARH          | Талаги               |     1
 Астрахань                | ASF          | Астрахань            |     1
 Барнаул                  | BAX          | Барнаул              |     1
 Белгород                 | EGO          | Белгород             |     1
 Белоярский               | EYK          | Белоярский           |     1
 Благовещенск             | BQS          | Игнатьево            |     1
 Братск                   | BTK          | Братск               |     1
 Брянск                   | BZK          | Брянск               |     1
 Бугульма                 | UUA          | Бугульма             |     1
```

#### Задание 23

Предположим, что департамент развития нашей авиакомпании задался вопросом: каким будет общее число различных маршрутов, которые теоретически можно проложить между всеми городами? Если в каком-то городе имеется более одного аэропорта, то это учитывать не будем, т. е. маршрутом будем считать путь между городами, а не между аэропортами. Здесь мы используем соединение таблицы с самой собой на основе неравенства значений атрибутов.

```sql
SELECT count( * )
FROM ( SELECT DISTINCT city FROM airports ) AS a1
JOIN ( SELECT DISTINCT city FROM airports ) AS a2
ON a1.city <> a2.city;
```

Перепишите этот запрос с общим табличным выражением.

```sql
WITH city_from 
  AS
( SELECT DISTINCT city FROM airports )
SELECT count( * )
FROM city_from f
JOIN ( SELECT DISTINCT city FROM airports ) AS a2
ON f.city <> a2.city;
```

[Наверх](#ссылки)

---

</details>
