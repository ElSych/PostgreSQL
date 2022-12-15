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

Глава 3 (Задания 1-4)

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
<summary>Развернуть</summary>

---

Глава 4 (Задания 2, 4, 8, 12, 15, 21, 30, 33, 35)

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

#### Задание 8

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

Глава 5 (Задания 2, 9, 17, 18)


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

#### Задание 9

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

#### Задание 17

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

#### Задание 18

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

Глава 6 (Задания 2, 7, 9, 13, 19, 21, 23)

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

## Домашняя работа 6

<details>
<summary>Развернуть</summary>

---

Глава 7 (упражнения 1, 2, 4)

#### Задание 1

Добавьте в определение таблицы aircrafts_log значение по умолчанию current_timestamp и соответствующим образом измените команды INSERT, приведенные в тексте главы.

```sql
CREATE TEMP TABLE aircrafts_log AS
SELECT * FROM aircrafts WITH DATA;
SELECT 9
ALTER TABLE aircrafts_log
ADD COLUMN log_timestamp TIMESTAMP DEFAULT (current_timestamp);

WITH add_row AS( 
    INSERT INTO aircrafts_tmp
    SELECT * FROM aircrafts
    RETURNING *
)
INSERT INTO aircrafts_log
     SELECT 
           add_row.aircraft_code, 
           add_row.model, 
           add_row.range
      FROM add_row;

SELECT * FROM aircrafts_log
 aircraft_code |        model        | range |       log_timestamp
---------------+---------------------+-------+----------------------------
 773           | Boeing 777-300      | 11100 | 2020-12-06 23:09:54.005097
 763           | Boeing 767-300      |  7900 | 2020-12-06 23:09:54.005097
 320           | Airbus A320-200     |  5700 | 2020-12-06 23:09:54.005097
 321           | Airbus A321-200     |  5600 | 2020-12-06 23:09:54.005097
 319           | Airbus A319-100     |  6700 | 2020-12-06 23:09:54.005097
 733           | Boeing 737-300      |  4200 | 2020-12-06 23:09:54.005097
 CN1           | Cessna 208 Caravan  |  1200 | 2020-12-06 23:09:54.005097
 CR2           | Bombardier CRJ-200  |  2700 | 2020-12-06 23:09:54.005097
 SU9           | Sukhoi SuperJet-100 |  6000 | 2020-12-06 23:09:54.005097
(9 строк)
```

#### Задание 2

В предложении RETURNING можно указывать не только символ «∗», означающий выбор всех столбцов таблицы, но и более сложные выражения, сформированные на основе этих столбцов. В тексте главы мы копировали содержимое таблицы «Самолеты» в таблицу aircrafts_tmp, используя в предложении RETURNING именно «∗». Однако возможен и другой вариант запроса:

```sql
WITH add_row AS
( INSERT INTO aircrafts_tmp
SELECT * FROM aircrafts
RETURNING aircraft_code, model, range,
current_timestamp, 'INSERT'
)
INSERT INTO aircrafts_log
SELECT ? FROM add_row;
```

Что нужно написать в этом запросе вместо вопросительного знака?

- Можно написать для вывода модели add_row.model, для вывода дальности add_row.range, для вывода кода add_row.aircraft_code,   для времени current_timestamp, ну или звёздочку, чтобы вывести всё

#### Задание 4

В тексте главы в предложениях ON CONFLICT команды INSERT мы использовали только выражения, состоящие из имени одного столбца.
Однако в таблице «Места» (seats) первичный ключ является составным и включает два столбца.
Напишите команду INSERT для вставки новой строки в эту таблицу и предусмотрите возможный конфликт добавляемой строки со строкой, уже имеющейся в таблице.
Сделайте два варианта предложения ON CONFLICT: первый — с использованием перечисления имен столбцов для проверки наличия дублирования, второй — с использованием предложения ON CONSTRAINT. Для того чтобы не изменить содержимое таблицы «Места», создайте ее копию и выполняйте все эти эксперименты с таблицей-копией.

Сделаем такую же таблицу

```sql
CREATE TEMP TABLE seats_tmp
AS SELECT * FROM SEATS;
ALTER TABLE seats_tmp
ADD PRIMARY KEY (aircraft_code, seat_no);
```
Вот она:
```sql
                                  Таблица "pg_temp_3.seats_tmp"
     Столбец     |          Тип          | Правило сортировки | Допустимость NULL | По умолчанию
-----------------+-----------------------+--------------------+-------------------+
 aircraft_code   | character(3)          |                    | not null          |
 seat_no         | character varying(4)  |                    | not null          |
 fare_conditions | character varying(10) |                    |                   |
Индексы:
    "seats_tmp_pkey" PRIMARY KEY, btree (aircraft_code, seat_no)

INSERT INTO seats_tmp
SELECT aircraft_code, seat_no, fare_conditions
FROM seats ON CONFLICT DO NOTHING;

INSERT INTO seats_tmp
VALUES ( 319, '2A', 'Business' )
ON CONFLICT ON CONSTRAINT seats_tmp_pkey
DO UPDATE SET aircraft_code = excluded.aircraft_code,
seat_no = excluded.seat_no
RETURNING *;
```

```sql
 aircraft_code | seat_no | fare_conditions
---------------+---------+-----------------
 319           | 2A      | Business
(1 строка)
```

[Наверх](#ссылки)

---

</details>

## Домашняя работа 7

<details>
<summary>Развернуть</summary>

---

Глава 8 (упражнения 1, 3)

#### Задание 1

Предположим, что для какой-то таблицы создан уникальный индекс по двум столбцам: column1 и column2. В таблице есть строка, у которой значение атрибута column1 равно ABC, а значение атрибута column2 - NULL. Мы решили добавить в таблицу еще одну строку с такими же значениями ключевых атрибутов, т.е. column1 - ABC, а column2 - NULL.

Как вы думаете, будет ли операция вставки новой строки успешной или завершится с ошибкой? Объясните ваше решение.

Завершится с ошибкой, NULL не будут распознаны как соответствующие каким либо существующим значениям.

#### Задание 3

Обратимся к таблице «Перелеты» (ticket_flights). В ней имеется столбец «Класс обслуживания» (fare_conditions), который отличается от остальных тем, что в нем могут присутствовать лишь три различных значения: Comfort, Business и Economy. 

Выполните запросы, подсчитывающие количество строк, в которых атрибут fare_conditions принимает одно из трех возможных значений. Каждый из запросов выполните три-четыре раза, поскольку время может немного изменяться, и подсчитайте среднее время.

```sql
SELECT count( * )
FROM ticket_flights
WHERE fare_conditions = 'Comfort';
17291 строка, время выполнения:
1.	50 мс
2.	44 мс
3.	44 мс
4.	47 мс

SELECT count( * )
FROM ticket_flights
WHERE fare_conditions = 'Business';
107642 строки, время выполнения:
1.	49 мс
2.	45 мс
3.	54 мс
4.	52 мс

SELECT count( * )
FROM ticket_flights
WHERE fare_conditions = 'Economy';
920793 строк, время выполнения:
1.	52 мс
2.	43 мс
3.	49 мс
4.	50 мс
```

Проделайте те же эксперименты с таблицей ticket_flights. Будет ли различаться среднее время выполнения запросов для различных значений атрибута fare_conditions? Почему это имеет место?

Сделаем индекс к полю fare_conditions.

```sql
SELECT count( * )
FROM ticket_flights
WHERE fare_conditions = 'Comfort';
17291 строка, время выполнения:
1.	2 мс
2.	1 мс
3.	1 мс
4.	2 мс

SELECT count( * )
FROM ticket_flights
WHERE fare_conditions = 'Business';
107642 строки, время выполнения:
1.	2 мс
2.	4 мс
3.	3 мс
4.	2 мс

SELECT count( * )
FROM ticket_flights
WHERE fare_conditions = 'Economy';
920793 строк, время выполнения:
1.	40 мс
2.	42 мс
3.	45 мс
4.	41 мс
```

Время для выборки Comfort и Business при добавлении индекса значительно сократилось, но время для Economy практически не изменилось. Это может быть связано с тем, что строк Economy больше, чем других строк, и индекс имеет преимущество только при выборе небольшой части от общего числа строк в таблице.

[Наверх](#ссылки)

---

</details>

## Домашняя работа 8

<details>
<summary>Развернуть</summary>

---

Глава 9 (упражнения 2, 3)

#### Задание 2

Модифицируйте сценарий выполнения транзакций: в первой транзакции вместо фиксации изменений выполните их отмену с помощью команды ROLLBACK и посмотрите, будет ли удалена строка и какая конкретно.

```sql
DELETE FROM aircrafts_tmp WHERE range < 2000;
SELECT * FROM aircrafts_tmp;

DELETE 
   FROM aircrafts_tmp
   WHERE range < 2000;
SELECT * FROM aircrafts_tmp;
 aircraft_code |        model        | range
---------------+---------------------+-------
 773           | Boeing 777-300      | 11100
 763           | Boeing 767-300      |  7900
 SU9           | Sukhoi SuperJet-100 |  3000
 320           | Airbus A320-200     |  5700
 321           | Airbus A321-200     |  5600
 319           | Airbus A319-100     |  6700
 733           | Boeing 737-300      |  4200
 CR2           | Bombardier CRJ-200  |  2700
 773           | Boeing 777-300      | 11100
 763           | Boeing 767-300      |  7900
 320           | Airbus A320-200     |  5700
 321           | Airbus A321-200     |  5600
 319           | Airbus A319-100     |  6700
 733           | Boeing 737-300      |  4200
 CR2           | Bombardier CRJ-200  |  2700
 SU9           | Sukhoi SuperJet-100 |  6000
(16 строк)
```

Измененияпервой транзакции не сохранились, и вторая транзакция произошла независимо от первой. Из-за чего удалилась строка, подходящая условию, которая была в изначальном состоянии таблицы. 

#### Задание 3

Когда говорят о таком феномене, как потерянное обновление, то зачастую в качестве примера приводится операция UPDATE, в которой значение какого-то атрибута изменяется с применением одного из действий арифметики. Например: 

```sql
UPDATE aircrafts_tmp SET range = range + 200 WHERE aircraft_code = 'CR2';
```

При выполнении двух и более подобных обновлений в рамках параллельных транзакций, использующих, например, уровень изоляции Read Committed, будут учтены все такие изменения (что и было показано в тексте главы). Очевидно, что потерянного обновления не происходит. Предположим, что в одной транзакции будет просто присваиваться новое значение.

Очевидно, что сохранится только одно из значений атрибута range. Можно ли говорить, что в такой ситуации имеет место потерянное обновление? Если оно имеет место, то что можно предпринять для его недопущения? Обоснуйте ваш ответ.

- С пользовательской стороны, потерянные обновления происходят, хотя то, что происходит на самом деле, эквивалентно последовательным транзакциям. Чтобы избежать потерь, можно использовать более высокий уровень изоляции.

[Наверх](#ссылки)

---

</details>

## Домашняя работа 9

<details>
<summary>Развернуть</summary>

---
Глава 10 (упражнения 3, 6, 8)

#### Задание 3

Самостоятельно выполните команду EXPLAIN для запроса, содержащего общее табличное выражение (CTE). Посмотрите, на каком уровне находится узел плана, отвечающий за это выражение, как он оформляется. Учтите, что общие табличные выражения всегда материализуются, т. е. вычисляются однократно и результат их вычисления сохраняется в памяти, а затем все последующие обращения в рамках запроса направляются уже к этому материализованному результату.

```sql
EXPLAIN WITH a AS
(SELECT DISTINCT city FROM airports)
SELECT count(*) 
  FROM a AS a1 
  JOIN a AS a2 
  ON a1.city<>a2.city;
```
Результат:
```sql
                               QUERY PLAN
-------------------------------------------------------------------------
 Aggregate  (cost=262.11..262.12 rows=1 width=8)
   CTE a
     ->  HashAggregate  (cost=3.30..4.31 rows=101 width=17)
           Group Key: airports.city
           ->  Seq Scan on airports  (cost=0.00..3.04 rows=104 width=17)
   ->  Nested Loop  (cost=0.00..232.55 rows=10100 width=0)
         Join Filter: (a1.city <> a2.city)
         ->  CTE Scan on a a1  (cost=0.00..2.02 rows=101 width=32)
         ->  CTE Scan on a a2  (cost=0.00..2.02 rows=101 width=32)
(9 строк)
```

#### Задание 6

Выполните команду EXPLAIN для запроса, в котором использована какая-нибудь из оконных функций. Найдите в плане выполнения запроса узел с именем WindowAgg. Попробуйте объяснить, почему он занимает именно этот уровень в плане.

```sql
EXPLAIN
SELECT book_ref, total_amount, avg(total_amount) OVER()
FROM bookings
ORDER BY 1
LIMIT 10;
```
Результат:
```sql
                                            QUERY PLAN
---------------------------------------------------------------------------------------------------
 Limit  (cost=0.42..0.87 rows=10 width=45)
   ->  WindowAgg  (cost=0.42..11796.09 rows=262788 width=45)
         ->  Index Scan using bookings_pkey on bookings  (cost=0.42..8511.24 rows=262788 width=13)
(3 строки)
```

#### Задание 8

Замена коррелированного подзапроса соединением таблиц является одним из способов повышения производительности. Предположим, что мы задались вопросом: сколько маршрутов обслуживают самолеты каждого типа? При этом нужно учитывать, что может иметь место такая ситуация, когда самолеты какого-либо типа не обслуживают ни одного маршрута. Поэтому необходимо использовать не только представление «Маршруты» (routes), но и таблицу «Самолеты» (aircrafts). Это первый вариант запроса, в нем используется коррелированный подзапрос. 

```sql
EXPLAIN ANALYZE 
SELECT f.flight_id, avg(tf.amount)
FROM flights_v f
LEFT OUTER JOIN ticket_flights tf ON f.flight_id = tf.flight_id
WHERE f.flight_id<100
GROUP BY 1
ORDER BY 1;
```
Результат:
```sql
                                                                           QUERY PLAN
----------------------------------------------------------------------------------------------------------------------------------------------------------------
 GroupAggregate  (cost=22132.61..22156.80 rows=97 width=36) (actual time=213.767..214.664 rows=99 loops=1)
   Group Key: f.flight_id
   ->  Sort  (cost=22132.61..22140.27 rows=3063 width=10) (actual time=213.734..213.912 rows=3988 loops=1)
         Sort Key: f.flight_id
         Sort Method: quicksort  Memory: 283kB
         ->  Hash Join  (cost=20.88..21955.25 rows=3063 width=10) (actual time=1.362..212.614 rows=3988 loops=1)
               Hash Cond: (f.arrival_airport = arr.airport_code)
               ->  Hash Join  (cost=16.54..21942.55 rows=3063 width=14) (actual time=1.303..211.537 rows=3988 loops=1)
                     Hash Cond: (f.departure_airport = dep.airport_code)
                     ->  Hash Right Join  (cost=12.20..21929.85 rows=3063 width=18) (actual time=1.257..210.285 rows=3988 loops=1)
                           Hash Cond: (tf.flight_id = f.flight_id)
                           ->  Seq Scan on ticket_flights tf  (cost=0.00..19172.26 rows=1045726 width=10) (actual time=0.045..109.140 rows=1045726 loops=1)
                           ->  Hash  (cost=10.99..10.99 rows=97 width=12) (actual time=0.061..0.062 rows=99 loops=1)
                                 Buckets: 1024  Batches: 1  Memory Usage: 13kB
                                 ->  Index Scan using flights_pkey on flights f  (cost=0.29..10.99 rows=97 width=12) (actual time=0.012..0.041 rows=99 loops=1)
                                       Index Cond: (flight_id < 100)
                     ->  Hash  (cost=3.04..3.04 rows=104 width=4) (actual time=0.037..0.038 rows=104 loops=1)
                           Buckets: 1024  Batches: 1  Memory Usage: 12kB
                           ->  Seq Scan on airports dep  (cost=0.00..3.04 rows=104 width=4) (actual time=0.006..0.017 rows=104 loops=1)
               ->  Hash  (cost=3.04..3.04 rows=104 width=4) (actual time=0.050..0.051 rows=104 loops=1)
                     Buckets: 1024  Batches: 1  Memory Usage: 12kB
                     ->  Seq Scan on airports arr  (cost=0.00..3.04 rows=104 width=4) (actual time=0.013..0.026 rows=104 loops=1)
 Planning Time: 1.548 ms
 Execution Time: 214.862 ms
(24 строки)
```

А в этом варианте коррелированный подзапрос раскрыт и заменен внешним соединением:

```sql
EXPLAIN ANALYZE
SELECT f.flight_id, (SELECT avg(tf.amount)
FROM ticket_flights tf
WHERE f.flight_id = tf.flight_id)
FROM flights_v f WHERE f.flight_id<100
GROUP BY 1
ORDER BY 1;
                                                                     QUERY PLAN
----------------------------------------------------------------------------------------------------------------------------------------------------
 Group  (cost=0.57..2113389.87 rows=97 width=36) (actual time=122.058..12766.754 rows=99 loops=1)
   Group Key: f.flight_id
   ->  Nested Loop  (cost=0.57..73.67 rows=97 width=4) (actual time=0.056..3.468 rows=99 loops=1)
         ->  Nested Loop  (cost=0.43..42.33 rows=97 width=8) (actual time=0.046..2.753 rows=99 loops=1)
               ->  Index Scan using flights_pkey on flights f  (cost=0.29..10.99 rows=97 width=12) (actual time=0.011..0.385 rows=99 loops=1)
                     Index Cond: (flight_id < 100)
               ->  Index Only Scan using airports_pkey on airports dep  (cost=0.14..0.32 rows=1 width=4) (actual time=0.018..0.018 rows=1 loops=99)
                     Index Cond: (airport_code = f.departure_airport)
                     Heap Fetches: 99
         ->  Index Only Scan using airports_pkey on airports arr  (cost=0.14..0.32 rows=1 width=4) (actual time=0.005..0.005 rows=1 loops=99)
               Index Cond: (airport_code = f.arrival_airport)
               Heap Fetches: 99
   SubPlan 1
     ->  Aggregate  (cost=21786.75..21786.76 rows=1 width=32) (actual time=128.915..128.915 rows=1 loops=99)
           ->  Seq Scan on ticket_flights tf  (cost=0.00..21786.58 rows=70 width=6) (actual time=99.973..128.884 rows=40 loops=99)
                 Filter: (f.flight_id = flight_id)
                 Rows Removed by Filter: 1045686
 Planning Time: 1.167 ms
 Execution Time: 12810.241 ms
(19 строк)
```

Исследуйте планы выполнения обоих запросов. Попытайтесь найти объяснение различиям в эффективности их выполнения. Чтобы получить усредненную картину, выполните каждый запрос несколько раз. 

Из планов выполнения запросов видно, что операция JOIN выполняется значительно быстрее, чем подзапрос

[Наверх](#ссылки)

---

</details>

## Домашняя работа 10

<details>
<summary>Развернуть</summary>

---

Задания 12-18 учебного пособия "Администрирование информационных систем".

#### Задание 12

```sql
SELECT * FROM personnel;
 emp_nbr | emp_name |         address         | birth_date 
---------+----------+-------------------------+------------
       0 | вакансия |                         | 2014-05-19
       1 | Иван     | ул. Любителей языка C   | 1962-12-01
       2 | Петр     | ул. UNIX гуру           | 1965-10-21
       3 | Антон    | ул. Ассемблерная        | 1964-04-17
       4 | Захар    | ул. им. СУБД PostgreSQL | 1963-09-27
       5 | Ирина    | просп. Программистов    | 1968-05-12
       6 | Анна     | пер. Перловый           | 1969-03-20
       7 | Андрей   | пл. Баз данных          | 1945-11-07
       8 | Николай  | наб. ОС Linux           | 1944-12-01

SELECT * FROM personnel_org_chart;
 emp_nbr |   emp   | boss_emp_nbr | boss  
---------+---------+--------------+-------
       1 | Иван    |            ø | ø
       2 | Петр    |            1 | Иван
       3 | Антон   |            1 | Иван
       4 | Захар   |            3 | Антон
       5 | Ирина   |            3 | Антон
       6 | Анна    |            3 | Антон
       7 | Андрей  |            5 | Ирина
       8 | Николай |            5 | Ирина

SELECT * FROM create_paths;
 level1 | level2 | level3 | level4  
--------+--------+--------+---------
 Иван   | Антон  | Ирина  | Андрей
 Иван   | Антон  | Ирина  | Николай
 Иван   | Петр   | ø      | ø
 Иван   | Антон  | Захар  | ø
 Иван   | Антон  | Анна   | ø

SELECT * FROM org_chart;
      job_title      | emp_nbr | boss_emp_nbr |  salary   
---------------------+---------+--------------+-----------
 Президент           |       1 |            ø | 1000.0000
 Вице-президент 1    |       2 |            1 |  900.0000
 Вице-президент 2    |       3 |            1 |  800.0000
 Архитектор          |       4 |            3 |  700.0000
 Ведущий программист |       5 |            3 |  600.0000
 Программист C       |       6 |            3 |  500.0000
 Программист Perl    |       7 |            5 |  450.0000
 Оператор            |       8 |            5 |  400.0000
```

#### Задание 13

Проверка структуры дерева на предмет отсутствия циклов.

```sql
UPDATE org_chart 
  SET boss_emp_nbr = 4 WHERE emp_nbr = 3;

SELECT * FROM tree_test();
 tree_test 
-----------
 Cycles
(1 row)

UPDATE org_chart 
SET boss_emp_nbr = 8 WHERE emp_nbr = 3;

SELECT * FROM tree_test();
 tree_test 
-----------
 Cycles
(1 row)
```

#### Задание 14

Обход дерева снизу вверх.

```sql
SELECT * FROM up_tree_traversal(6);
 emp_nbr | boss_emp_nbr 
---------+--------------
       6 |            3
       3 |            1
       1 |            ø
SELECT * FROM up_tree_traversal2(6)
AS (emp int, boss int);
 emp | boss 
-----+------
   6 |    3
   3 |    1
   1 |    ø
SELECT * FROM up_tree_traversal (( select emp_nbr from personnel where emp_name = 'Захар' ));
 emp_nbr | boss_emp_nbr 
---------+--------------
       4 |            3
       3 |            1
       1 |            ø
```

#### Задание 15

Удаление поддерева

```sql
SELECT * FROM create_paths;
 level1 | level2 | level3 | level4  
--------+--------+--------+---------
 Иван   | Антон  | Ирина  | Андрей
 Иван   | Антон  | Ирина  | Николай
 Иван   | Петр   | ø      | ø
 Иван   | Антон  | Захар  | ø
 Иван   | Антон  | Анна   | ø

SELECT * FROM personnel_org_chart;
 emp_nbr |   emp   | boss_emp_nbr | boss  
---------+---------+--------------+-------
       1 | Иван    |            ø | ø
       2 | Петр    |            1 | Иван
       3 | Антон   |            1 | Иван
       4 | Захар   |            3 | Антон
       5 | Ирина   |            3 | Антон
       6 | Анна    |            3 | Антон
       7 | Андрей  |            5 | Ирина
       8 | Николай |            5 | Ирина

SELECT * FROM delete_subtree(4);
 delete_subtree 
----------------
 
(1 row)
SELECT * FROM create_paths;
 level1 | level2 | level3 | level4  
--------+--------+--------+---------
 Иван   | Антон  | Ирина  | Андрей
 Иван   | Антон  | Ирина  | Николай
 Иван   | Петр   | ø      | ø
 Иван   | Антон  | Анна   | ø

SELECT * FROM personnel_org_chart;
 emp_nbr |   emp   | boss_emp_nbr | boss  
---------+---------+--------------+-------
       1 | Иван    |            ø | ø
       2 | Петр    |            1 | Иван
       3 | Антон   |            1 | Иван
       5 | Ирина   |            3 | Антон
       6 | Анна    |            3 | Антон
       7 | Андрей  |            5 | Ирина
       8 | Николай |            5 | Ирина

SELECT * FROM delete_subtree(( select emp_nbr from personnel where emp_name = 'Анна' )); delete_subtree 
----------------
 
(1 row)
SELECT * FROM create_paths;
 level1 | level2 | level3 | level4  
--------+--------+--------+---------
 Иван   | Антон  | Ирина  | Андрей
 Иван   | Антон  | Ирина  | Николай
 Иван   | Петр   | ø      | ø
 
SELECT * FROM personnel_org_chart;
 emp_nbr |   emp   | boss_emp_nbr | boss  
---------+---------+--------------+-------
       1 | Иван    |            ø | ø
       2 | Петр    |            1 | Иван
       3 | Антон   |            1 | Иван
       5 | Ирина   |            3 | Антон
       7 | Андрей  |            5 | Ирина
       8 | Николай |            5 | Ирина
 
```

#### Задание 16

```sql
SELECT * FROM delete_and_promote_subtree(( select emp_nbr from personnel where emp_name='Антон' ));
 delete_and_promote_subtree 
----------------------------
 
(1 row)
SELECT * FROM create_paths;
 level1 | level2 | level3  | level4 
--------+--------+---------+--------
 Иван   | Петр   | ø       | ø
 Иван   | Ирина  | Николай | ø
 Иван   | Ирина  | Андрей  | ø
 
SELECT * FROM personnel_org_chart;
 emp_nbr |   emp   | boss_emp_nbr | boss  
---------+---------+--------------+-------
       1 | Иван    |            ø | ø
       2 | Петр    |            1 | Иван
       7 | Андрей  |            5 | Ирина
       8 | Николай |            5 | Ирина
       5 | Ирина   |            1 | Иван
 
```

#### Задание 17

```sql
CREATE VIEW create_paths_5 (level1, level2, level3, level4, level5) AS
SELECT a1.emp AS e1, a2.emp AS e2, a3.emp AS e3, a4.emp AS e4, a5.emp AS e5
FROM personnel_org_chart as a1
  LEFT OUTER JOIN personnel_org_chart as a2
               ON a1.emp = a2.boss
  LEFT OUTER JOIN personnel_org_chart as a3
               ON a2.emp = a3.boss
  LEFT OUTER JOIN  personnel_org_chart AS a4
               ON a3.emp = a4.boss
  LEFT OUTER JOIN  personnel_org_chart as a5
               ON a4.emp = a5.boss 
  WHERE a1.emp = 'Иван';
SELECT * FROM create_paths_5;
 level1 | level2 | level3  | level4 | level5 
--------+--------+---------+--------+--------
 Иван   | Ирина  | Андрей  | ø      | ø
 Иван   | Ирина  | Николай | ø      | ø
 Иван   | Петр   | ø       | ø      | ø

```

#### Задание 18

```sql
ALTER TABLE personnel ADD COLUMN ne_goden text;

CREATE OR replace FUNCTION pensia() 
  RETURNS void AS $$
DECLARE curs CURSOR
  FOR SELECT * 
  FROM personnel WHERE (cast(current_date as date)-cast(birth_date as date))/365 > 65;
BEGIN
  OPEN curs;
  MOVE curs;
  WHILE FOUND LOOP
    UPDATE personnel
    SET pensioneer='yes'
    WHERE current of curs;
    MOVE curs;
  END loop;
  CLOSE curs;
END
$$
LANGUAGE plpgsql;
CREATE FUNCTION

SELECT * FROM pensia();
 pensia 
------
 
(1 row)

SELECT * FROM personnel;
 emp_nbr | emp_name |         address         | birth_date | pensioneer 
---------+----------+-------------------------+------------+----------
       0 | вакансия |                         | 2014-05-19 | ø
       1 | Иван     | ул. Любителей языка C   | 1962-12-01 | NULL
       2 | Петр     | ул. UNIX гуру           | 1965-10-21 | NULL
       3 | Антон    | ул. Ассемблерная        | 1964-04-17 | NULL
       4 | Захар    | ул. им. СУБД PostgreSQL | 1963-09-27 | NULL
       5 | Ирина    | просп. Программистов    | 1968-05-12 | NULL
       6 | Анна     | пер. Перловый           | 1969-03-20 | NULL
       7 | Андрей   | пл. Баз данных          | 1945-11-07 | yes
       8 | Николай  | наб. ОС Linux           | 1944-12-01 | yes

```

[Наверх](#ссылки)

---

</details>

## Домашняя работа 11

<details>
<summary>Развернуть</summary>

---

Задание выполняется на основе презентации 10 "Полнотекстовый поиск"
и главы 12 документации на [Постгрес](https://postgrespro.ru/docs/postgresql/12/textsearch)

Придумать и реализовать пример использования полнотекстового поиска,
аналогичный (можно более простой или более сложный) тому примеру с библиотечным каталогом,
который был приведен в презентации.
Можно использовать исходные тексты, приведенные в [презентации](https://edu.postgrespro.ru/sqlprimer/sqlprimer-2019-msu-10.tgz).

```sql
CREATE TABLE books
(book_id integer primary key, book_description text);
\copy books from '/home/Downloads/books3.txt';

ALTER TABLE books 
ADD COLUMN ts_description tsvector;
UPDATE books 
  SET ts_description = to_tsvector('russian', book_description);
```
Найдём книги, связанные с МАИ
```sql
SELECT book_id, book_description FROM books WHERE ts_description @@ plainto_tsquery('МАИ');
book_id  | book_description
---------+----------------------------------------------------------------------------------------------------------------------------------------
746	     | Ефимочкина, Евгения Петровна. Математические методы в экономике [Текст] : пособие по решению задач линейного программирования : для дневной и вечерней форм обучения / Е. П. Ефимочкина. - Москва : МАИ, 1975. - 79 с.
747	     | Задачи линейного программирования [Текст] : (учебное пособие) : для дневной и вечерней форм обучения / Моск. авиац. ин-т им. С. Орджоникидзе. - Москва : МАИ, 1975. - 70, [3] с.
748	     | Бунякин, Сергей Васильевич. Программирование и вычислительная математика при алгоритмизации процессов обработки информации [Текст] : для дневной и вечерней формы обучения. - Москва : МАИ, 1975 - . Ч. 1 : Метод Калмана-Бьюси. - 1975. - 87, [1] с.
(33 rows)
```
Теперь книги про математический анализ
```sql
SELECT book_id, book_description FROM books WHERE ts_description @@ plainto_tsquery('Математический анализ');
book_id  | book_description
---------+----------------------------------------------------------------------------------------------------------------------------------------
262      | Высшая математика для экономистов: сборник задач [Текст] : аналитическая геометрия, линейная алгебра, математический анализ, теория вероятностей, математическая статистика, линейное программирование : учебное пособие для студентов высших учебных заведений, обучающихся по направлению 38.03.01 "Экономика" и экономическим специальностям / [Г. И. Бобрик и др.]. - Москва : ИНФРА-М, 2015. - 537, [1] с.
```
Количество книг про питон
```sql
SELECT count(*) FROM books WHERE ts_description @@ to_tsquery('Python');
 count 
-------
    4
(1 row)
```

[Наверх](#ссылки)

---

</details>
