### 289. Ближние места

> Требуется найти названия всех пунктов, расстояние до которых не более 20 км. Названия выведите в алфавитном порядке.

```sql
SELECT name_point 
FROM points 
WHERE distance <= 20 
ORDER BY name_point
```
----------
### 302. Номера "Икарусов"

> Номера всех автобусов марки Икарус, вывести в лексикографическом порядке

```sql
SELECT bus_number 
FROM buses
LEFT JOIN models ON buses.cod_model = models.cod_model 
WHERE name_model = 'Икарус' 
ORDER BY bus_number
```
----------
### 303. Остановки Вологда-Череповец

> Названия всех пунктов маршрута Вологда-Череповец в порядке следования

```sql
SELECT P.name_point 
FROM points_routes PR
INNER JOIN routes R ON PR.cod_route = R.cod_route
INNER JOIN points P ON PR.cod_point = P.cod_point 
WHERE R.name_route = 'Вологда-Череповец' 
ORDER BY P.distance
```
----------
### 304. Маршруты через Шексну

> Названия всех маршрутов, которые проходят через Шексну, в алфавитном порядке

```sql
SELECT R.name_route 
FROM points_routes PR
INNER JOIN routes R ON PR.cod_route = R.cod_route
INNER JOIN points P ON PR.cod_point = P.cod_point 
WHERE P.name_point = 'Шексна' 
ORDER BY R.name_route
```
----------
### 305. Рейсы Вологда-Череповец, понедельник

> Время отправления всех рейсов по маршруту Вологда-Череповец в понедельник по возрастанию

```sql
SELECT hour, minute 
FROM trips T
INNER JOIN routes R ON T.cod_route = R.cod_route 
WHERE week_day = 1 AND name_route = 'Вологда-Череповец' 
ORDER BY hour, minute
```
----------
### 306. Пункты "Ч"

> Названия всех пунктов, которые начинаются с буквы Ч, вывести в алфавитном порядке

```sql
SELECT name_point 
FROM points 
WHERE name_point LIKE 'Ч%' 
ORDER BY name_point
```
----------
### 307. 5-пункты

> Названия всех пунктов, которые состоят из пяти букв, вывести в алфавитном порядке

```sql
SELECT name_point 
FROM points 
WHERE LENGTH(name_point) = 5 
ORDER BY name_point
```
----------
### 308. Пункты с "О"

> Названия всех пунктов, в которых есть хотя бы одна буква О (без учета регистра), вывести в алфавитном порядке

```sql
SELECT name_point 
FROM points 
WHERE UPPER(name_point) LIKE '%О%' 
ORDER BY name_point
```
----------
### 309. Автобусы в выходные

> Номера всех автобусов, которые задействованы в выходные дни (суббота и воскресенье) в лексикографическом порядке

```sql
SELECT DISTINCT bus_number 
FROM trips T
INNER JOIN buses B ON T.cod_bus = B.cod_bus 
WHERE week_day > 5 
ORDER BY bus_number
```
----------
### 310. Рейсы в воскресенье до 12

> Коды рейсов с указанием названий маршрутов и времени отправления в воскресенье до 12 часов, в порядке возрастания времени

```sql
SELECT T.cod_trip, R.name_route, T.hour, T.minute 
FROM trips T
INNER JOIN routes R ON T.cod_route = R.cod_route 
WHERE week_day = 7 AND hour < 12 
ORDER BY hour, minute
```
----------
### 311. Маршруты через Шексну - 2

> Названия и коды всех маршрутов, которые проходят через Шексну, в алфавитном порядке

```sql
SELECT R.name_route, R.cod_route 
FROM routes R
INNER JOIN points_routes PR ON R.cod_route = PR.cod_route
INNER JOIN points P ON PR.cod_point = P.cod_point 
WHERE P.name_point = 'Шексна' 
ORDER BY R.name_route
```
----------
### 312. В понедельник через Сокол

> Время отправления всех рейсов (часы, минуты) и называния маршрутов для всех рейсов в понедельник, которые проходят через Сокол,  в порядке возрастания времени

```sql
SELECT T.hour, T.minute, R.name_route 
FROM routes R
INNER JOIN points_routes PR ON R.cod_route = PR.cod_route
INNER JOIN points P ON PR.cod_point = P.cod_point
INNER JOIN trips T  ON T.cod_route = R.cod_route 
WHERE P.name_point = 'Сокол' AND T.week_day = 1 
ORDER BY T.hour, T.minute
```
----------
### 313. Свободных мест на второй рейс

> Сколько свободных мест имеется на рейс с кодом 2

```sql
SELECT M.places - T.tickets 
FROM trips T
INNER JOIN buses B ON T.cod_bus = B.cod_bus
INNER JOIN models M ON B.cod_model = M.cod_model 
WHERE T.cod_trip = 2
```
----------
### 314. Макс. цена билета на 2-й рейс

> Максимальная цена билета на рейс с номером 2

```sql
SELECT MAX(K.price * P.distance) 
FROM trips T
INNER JOIN routes R ON T.cod_route = R.cod_route
INNER JOIN points_routes PR ON PR.cod_route = R.cod_route 
INNER JOIN points P ON P.cod_point = PR.cod_point
INNER JOIN buses B ON T.cod_bus = B.cod_bus
INNER JOIN models M ON B.cod_model = M.cod_model 
INNER JOIN km_prices K ON M.class = K.class WHERE T.cod_trip = 2
```
----------
### 315. Сколько "Икарусов"?

> Количество автобусов марки Икарус

```sql
SELECT COUNT(B.cod_bus) 
FROM buses B
INNER JOIN models M ON B.cod_model = M.cod_model 
WHERE M.name_model = 'Икарус'
```
----------
### 316. Первоклассные автобусы

> Количество автобусов 1 класса

```sql
SELECT COUNT(B.cod_bus) 
FROM buses B
INNER JOIN models M ON B.cod_model = M.cod_model 
WHERE M.class = 1
```
----------
### 317. Число маршрутов

> Общее количество маршрутов

```sql
SELECT COUNT(cod_route) FROM routes
```
----------
### 318. Число рейсов

> Общее количество рейсов в неделю

```sql
SELECT COUNT(cod_trip) FROM trips
```
----------
### 319. Число рейсов в понедельник

> Общее количество рейсов в понедельник

```sql
SELECT COUNT(cod_trip) FROM trips WHERE week_day = 1
```
----------
### 320. Число рейсов Вологда-Череповец

> Общее количество рейсов в неделю по маршруту Вологда-Череповец

```sql
SELECT COUNT(T.cod_trip) 
FROM trips T
INNER JOIN routes R ON T.cod_route = R.cod_route 
WHERE R.name_route = 'Вологда-Череповец'
```
----------
### 321. Ближние места-2

> Количество пунктов, расстояние до которых не более 20 км

```sql
SELECT COUNT(cod_point) FROM points WHERE distance <= 20
```
----------
### 322. Пункты "Ч" - 2

> Количество пунктов, которые начинаются с буквы Ч

```sql
SELECT COUNT(cod_point) FROM points WHERE name_point LIKE 'Ч%'
```
----------
### 323. Cколько 5-пунктов?

> Количество пунктов, названия которых состоят из пяти букв

```sql
SELECT COUNT(cod_point) FROM points WHERE LENGTH(name_point) = 5
```
----------
### 324. Пункты с "О" - 2

> Количество пунктов, в названии которых есть хотя бы одна буква О (без учета регистра)

```sql
SELECT COUNT(cod_point) FROM points WHERE UPPER(name_point) LIKE '%О%'
```
----------
### 325. Сколько рейсов в понедельник, Череповец

> Количество рейсов в понедельник по маршруту Вологда-Череповец

```sql
SELECT COUNT(T.cod_trip) 
FROM trips T
INNER JOIN routes R ON T.cod_route = R.cod_route 
WHERE R.name_route = 'Вологда-Череповец' AND week_day = 1
```
----------
### 326. Понедельник, с 9  до 10

> Общее количество рейсов в понедельник между 9 и 10 часами (рейсы в 10 часов 0 минут не включаются)

```sql
SELECT COUNT(cod_trip) 
FROM trips 
WHERE hour >= 9 AND hour < 10 AND week_day = 1
```
----------
### 327. Маленькие автобусы

> Количество автобусов вместимостью более 30 мест

```sql
SELECT COUNT(B.cod_bus) 
FROM buses B
INNER JOIN models M ON B.cod_model = M.cod_model 
WHERE M.places > 30
```
----------
### 328. Число рейсов в выходные

> Общее количество рейсов в выходные дни (суббота и воскресенье)

```sql
SELECT COUNT(cod_trip) FROM trips WHERE week_day > 5
```
----------
### 329. Через Сокол в понедельник

> Количество рейсов, которые проходят через Сокол в понедельник

```sql
SELECT COUNT(T.cod_trip) 
FROM trips T
INNER JOIN routes R ON T.cod_route = R.cod_route
INNER JOIN points_routes PR ON PR.cod_route = R.cod_route
INNER JOIN points P ON P.cod_point = PR.cod_point
WHERE P.name_point = 'Сокол' AND T.week_day = 1
```
----------
### 330. До самого далёкого

> Расстояние до самого удаленного пункта, до которого можно доехать каким-нибудь рейсом

```sql
SELECT MAX(P.distance) 
FROM trips T
INNER JOIN routes R ON T.cod_route = R.cod_route
INNER JOIN points_routes PR ON PR.cod_route = R.cod_route
INNER JOIN points P ON P.cod_point = PR.cod_point
```
----------
### 331. Самый дешевый билет

> Цена самого дешевого билета

```sql
SELECT MIN(K.price * P.distance) 
FROM trips T 
INNER JOIN routes R ON T.cod_route = R.cod_route 
INNER JOIN points_routes PR ON PR.cod_route = R.cod_route
INNER JOIN points P ON P.cod_point = PR.cod_point
INNER JOIN buses B ON T.cod_bus = B.cod_bus
INNER JOIN models M ON M.cod_model = B.cod_model
INNER JOIN km_prices K ON K.class = M.class
WHERE P.distance > 0
```
----------
### 332. Самый большой автобус

> Самая большая вместимость автобуса, имеющегося в автопарке вокзала

```sql
SELECT MAX(M.places) 
FROM buses B
INNER JOIN models M ON B.cod_model = M.cod_model
```
----------
### 333. Самый маленький автобус

> Самая маленькая вместимость автобуса, имеющегося в автопарке вокзала

```sql
SELECT MIN(M.places) 
FROM buses B
INNER JOIN models M ON B.cod_model = M.cod_model
```
----------
### 334. Самый остановочный маршрут

> Самое большое количество остановок в маршруте

```sql
SELECT MAX(pointsCount) 
FROM (
    SELECT COUNT(cod_point) pointsCount 
    FROM points_routes 
    GROUP BY (cod_route)) COUNTS
```
----------
### 335. Самое раннее время

> Самое раннее время отправления за всю неделю (часы и минуты отдельно)

```sql
SELECT * 
FROM (
    SELECT hour, minute 
    FROM trips 
    ORDER BY hour, minute) 
WHERE ROWNUM = 1
```
----------
### 336. Самое позднее время

> Самое позднее время отправления за всю неделю (часы и минуты отдельно)

```sql
SELECT * 
FROM (
    SELECT hour, minute 
    FROM trips 
    ORDER BY hour DESC, minute DESC) 
WHERE ROWNUM = 1
```
----------
### 337. Первый рейс в понедельник

> Время отправления самого первого рейса в понедельник (часы и минуты отдельно)

```sql
SELECT * 
FROM (
    SELECT hour, minute 
    FROM trips 
    WHERE week_day = 1 
    ORDER BY hour, minute) 
WHERE ROWNUM = 1
```
----------
### 338. Самое раннее время Вологда-Череповец

> Самое раннее время отправления по маршруту "Вологда-Череповец" (часы и минуты отдельно)

```sql
SELECT * 
FROM (
    SELECT hour, minute 
    FROM trips T
    INNER JOIN routes R ON T.cod_route = R.cod_route 
    WHERE R.name_route = 'Вологда-Череповец' 
    ORDER BY hour, minute) 
WHERE ROWNUM = 1
```
----------
### 339. Первым классом и подальше

> Цена билета до самого удаленного пункта в автобусе первого класса (без учета того, ходят ли туда  на самом деле автобусы первого класса)

```sql
SELECT Distance.Value*Price.Value 
FROM 
    (SELECT MAX(distance) AS Value 
    FROM points) Distance, 
	(SELECT price AS Value 
    FROM km_prices 
    WHERE class = 1) Price
```
----------
### 340. Самые далекие

> Названия самых удаленных пунктов - их может быть несколько

```sql
SELECT P1.name_point 
FROM points P1
LEFT JOIN points P2 ON P1.distance < P2.distance 
WHERE P2.cod_point IS NULL
```
----------
### 341. Самые первые в понедельник

> Название маршрута, по которому отправляется самый перввый рейс в понедельник (их может быть несколько)

```sql
SELECT R.name_route 
FROM trips T1
LEFT JOIN trips T2 
    ON T1.hour*60+T1.minute > T2.hour*60+T2.minute 
    AND T1.week_day = T2.week_day 
INNER JOIN routes R 
    ON T1.cod_route = R.cod_route
WHERE T1.week_day = 1 
    AND T2.cod_trip IS NULL
```
----------
### 342. Самые последние в понедельник

> Название маршрута, по которому отправляется самый последний рейс в понедельник (их может быть несколько)

```sql
SELECT DISTINCT R.name_route 
FROM trips T1
LEFT JOIN trips T2 
    ON T1.hour*60+T1.minute < T2.hour*60+T2.minute 
    AND T1.week_day = T2.week_day 
INNER JOIN routes R 
    ON T1.cod_route = R.cod_route
WHERE T1.week_day = 1 
    AND T2.cod_trip IS NULL
```
----------
### 343. Самые остановочные маршруты - 2

> Название маршрута (или нескольких маршрутов), у которых самое большое количество остановок

```sql
SELECT R.name_route 
FROM (
    SELECT cod_route 
    FROM points_routes
    GROUP BY cod_route 
    HAVING COUNT(cod_point) = (
        SELECT MAX(pointsCount) 
        FROM (
            SELECT COUNT(cod_point) pointsCount 
            FROM points_routes 
            GROUP BY (cod_route)) COUNTS)) MaxPointCodRoute 
INNER JOIN routes R 
ON R.cod_route = MaxPointCodRoute.cod_route
```
----------
### 344. Самые первые в понедельник - 2

> Код самого раннего рейса в понедельник с указанием времени отправления и названия маршрута  (возможно таких рейсов несколько - выбрать все)

```sql
SELECT T1.cod_trip, T1.hour, T1.minute, R.name_route 
FROM trips T1
LEFT JOIN trips T2 
    ON T1.hour*60+T1.minute > T2.hour*60+T2.minute 
    AND T1.week_day = T2.week_day 
INNER JOIN routes R 
    ON T1.cod_route = R.cod_route
WHERE T1.week_day = 1 
    AND T2.cod_trip IS NULL
```
----------
### 345. Самые последние в понедельник - 2

> Код самого позднего рейса в понедельник с указанием времени отправления и названия маршрута  (возможно таких рейсов несколько - выбрать все)

```sql
SELECT DISTINCT T1.cod_trip, T1.hour, T1.minute, R.name_route 
FROM trips T1
LEFT JOIN trips T2 
    ON T1.hour*60+T1.minute < T2.hour*60+T2.minute 
    AND T1.week_day = T2.week_day 
INNER JOIN routes R 
    ON T1.cod_route = R.cod_route
WHERE T1.week_day = 1 
    AND T2.cod_trip IS NULL
```
----------
### 346. Самые дешевые

> Вывести названия пунктов, до которых самые дешевые билеты

```sql
SELECT DISTINCT Prices1.name_point 
FROM (
    SELECT K.price * P.distance AS price, P.name_point FROM trips T 
    INNER JOIN routes R ON T.cod_route = R.cod_route 
    INNER JOIN points_routes PR ON PR.cod_route = R.cod_route
    INNER JOIN points P ON P.cod_point = PR.cod_point
    INNER JOIN buses B ON T.cod_bus = B.cod_bus
    INNER JOIN models M ON M.cod_model = B.cod_model
    INNER JOIN km_prices K ON K.class = M.class
    WHERE P.distance > 0) Prices1
LEFT JOIN (
    SELECT K.price * P.distance AS price, P.name_point FROM trips T 
    INNER JOIN routes R ON T.cod_route = R.cod_route 
    INNER JOIN points_routes PR ON PR.cod_route = R.cod_route
    INNER JOIN points P ON P.cod_point = PR.cod_point
    INNER JOIN buses B ON T.cod_bus = B.cod_bus
    INNER JOIN models M ON M.cod_model = B.cod_model
    INNER JOIN km_prices K ON K.class = M.class
    WHERE P.distance > 0) Prices2
ON Prices1.price > Prices2.price
WHERE Prices2.price IS NULL
```
----------
### 347. В понедельник в Череповец с утра пораньше

> Количество свободных мест, часы и минуты отправления на самый первый рейс в понедельник,  который идет по маршруту "Вологда-Череповец"

```sql
SELECT * 
FROM (
    SELECT M.places - T.tickets, T.hour, T.minute 
    FROM trips T 
    INNER JOIN buses B ON T.cod_bus = B.cod_bus 
    INNER JOIN models M ON B.cod_model = M.cod_model 
    INNER JOIN routes R ON R.cod_route = T.cod_route
    WHERE week_day = 1 
        AND R.name_route = 'Вологда-Череповец'
    ORDER BY hour, minute) 
WHERE ROWNUM = 1
```
----------
### 348. В конце недели в Череповец с утра пораньше

> Вывести день недели, часы и минуты отправления, количество свободных мест на самые первые рейсы  в пятницу, субботу и воскресенье, которые идет по маршруту "Вологда-Череповец".  Можно считать, что в один день в одно и то же время не может быть двух рейсов по одному и тому же маршруту.

```sql
SELECT Trips1.week_day, Trips1.hour, Trips1.minute, Trips1.ATickets 
FROM (
    SELECT T.week_day, (M.places - T.tickets) AS ATickets, T.hour, T.minute 
    FROM trips T 
    INNER JOIN buses B ON T.cod_bus = B.cod_bus 
    INNER JOIN models M ON B.cod_model = M.cod_model 
    INNER JOIN routes R ON R.cod_route = T.cod_route
    WHERE week_day > 4 
        AND R.name_route = N'Вологда-Череповец') Trips1
LEFT JOIN (
    SELECT T.week_day, (M.places - T.tickets) AS ATickets, T.hour, T.minute 
    FROM trips T 
    INNER JOIN buses B ON T.cod_bus = B.cod_bus 
    INNER JOIN models M ON B.cod_model = M.cod_model 
    INNER JOIN routes R ON R.cod_route = T.cod_route
    WHERE week_day > 4 
        AND R.name_route = N'Вологда-Череповец') Trips2
    ON Trips1.hour*60+Trips1.minute > Trips2.hour*60+Trips2.minute 
    AND Trips1.week_day = Trips2.week_day WHERE Trips2.week_day IS NULL
```
----------
### 349. В понедельник едем в Сокол

> Самый ранний рейс в понедельник, проходящий через  Сокол. Вывести часы, минуты, название маршрута и количество сободных мест. Если таких рейсов не один, выбрать все.

```sql
SELECT T1.hour, T1.minute, T1.name_route, BP.places-T1.tickets AS free_places 
FROM (
SELECT trips.hour, trips.minute, routes.name_route, trips.tickets, trips.cod_bus 
    FROM trips
    LEFT JOIN routes ON trips.cod_route = routes.cod_route
    LEFT JOIN points_routes ON routes.cod_route = points_routes.cod_route
    LEFT JOIN points ON points_routes.cod_point = points.cod_point
    WHERE trips.week_day = 1 AND points.name_point = 'Сокол') T1
LEFT JOIN (
    SELECT trips.hour, trips.minute 
    FROM trips
    LEFT JOIN routes ON trips.cod_route = routes.cod_route
    LEFT JOIN points_routes ON routes.cod_route = points_routes.cod_route
    LEFT JOIN points ON points_routes.cod_point = points.cod_point
    WHERE trips.week_day = 1 AND points.name_point = 'Сокол') T2 
    ON T1.hour*60+T1.minute > T2.hour*60+T2.minute 
LEFT JOIN (
    SELECT buses.cod_bus, models.places 
    FROM buses
    LEFT JOIN models ON models.cod_model = buses.cod_model) BP 
    ON T1.cod_bus = BP.cod_bus
WHERE T2.hour IS NULL
```
----------
### 350. Число рейсов до каждого пункта

> Названия всех населенных пунктов и общее количество рейсов, которыми можно добраться до этого пункта

```sql
SELECT points.name_point, COUNT(trips.cod_trip) 
FROM points
LEFT JOIN points_routes ON points.cod_point = points_routes.cod_point
LEFT JOIN routes ON routes.cod_route = points_routes.cod_route
LEFT JOIN trips ON trips.cod_route = routes.cod_route
GROUP BY points.name_point

```
----------
### 351. Число рейсов для каждой модели автобуса

> Названия всех марок автобусов и общее количество рейсов, в которых задействованы автобусы каждой марки

```sql
SELECT models.name_model, COUNT(trips.cod_trip) 
FROM models
LEFT JOIN buses ON buses.cod_model = models.cod_model
LEFT JOIN trips ON trips.cod_bus = buses.cod_bus
GROUP BY models.name_model
```
----------
### 352. Число остановок для каждого маршрута

> Названия всех маршрутов с указанием количества остановок в каждом маршруте

```sql
SELECT routes.name_route, COUNT(points_routes.cod_point) 
FROM routes
LEFT JOIN points_routes ON points_routes.cod_route = routes.cod_route
GROUP BY routes.name_route

```
----------
### 353. Макс. число пассажиров для каждого маршрута

> Названия всех маршрутов с указанием общего количества пассажиров, которых можно перевезти за неделю по каждому маршруту

```sql
SELECT routes.name_route, SUM(NVL(models.places,0)) 
FROM routes
LEFT JOIN trips ON trips.cod_route = routes.cod_route
LEFT JOIN buses ON buses.cod_bus = trips.cod_bus
LEFT JOIN models ON models.cod_model = buses.cod_model
GROUP BY routes.name_route
```
----------
### 354. Число маршрутов для каждого пункта

> Названия всех населенных пунктов и количество маршрутов, которые проходят через каждый пункт

```sql
SELECT points.name_point, COUNT(points_routes.cod_route) 
FROM points 
LEFT JOIN points_routes ON points_routes.cod_point = points.cod_point
GROUP BY points.name_point
```
----------
### 355. Число разных автобусов через каждый пункт

> Названия всех населенных пунктов и количество различных автобусов, которые задействованы в рейсах, проходящих через каждый из населенных пунктов

```sql
SELECT points.name_point, COUNT(DISTINCT(cod_bus)) 
FROM points
LEFT JOIN points_routes ON points_routes.cod_point = points.cod_point
LEFT JOIN routes ON routes.cod_route = points_routes.cod_route
LEFT JOIN trips ON trips.cod_route = routes.cod_route
GROUP BY points.name_point

```
----------
### 356. Число рейсов через каждый маршрут

> Названия всех маршрутов с указанием количества рейсов по каждому маршруту

```sql
SELECT routes.name_route, COUNT(trips.cod_trip) 
FROM routes
LEFT JOIN trips ON trips.cod_route = routes.cod_route
GROUP BY routes.name_route
```
----------
### 357. Число рейсов через каждый маршрут в выходные

> Названия всех маршрутов (вообще всех) с указанием количества рейсов в выходные дни по каждому маршруту

```sql
SELECT routes.name_route, COUNT(T.cod_trip) 
FROM routes
LEFT JOIN (
    SELECT * 
    FROM trips 
    WHERE trips.week_day > 5) T 
    ON T.cod_route = routes.cod_route
GROUP BY routes.name_route
```
----------
### 358. Число рейсов для каждого автобуса

> Номера всех автобусов с указанием количества рейсов, в которых задействован каждый из автобусов

```sql
SELECT buses.bus_number, COUNT(trips.cod_trip) 
FROM buses
LEFT JOIN trips ON trips.cod_bus = buses.cod_bus
GROUP BY buses.bus_number
```
----------
### 359. Число маршрутов для каждого автобуса

> Номера всех автобусов с указанием количества маршрутов, в которых задействован каждый из автобусов

```sql
SELECT buses.bus_number, COUNT(DISTINCT(trips.cod_route)) 
FROM buses
LEFT JOIN trips ON trips.cod_bus = buses.cod_bus
GROUP BY buses.bus_number
```
----------
### 360. Пункты с макс. числом рейсов

> Названия пунктов, которые принимают наибольшее количество рейсов в неделю

```sql
SELECT PT1.name_point 
FROM (
    SELECT points.name_point, COUNT(trips.cod_trip) AS trips_count 
    FROM points 
    LEFT JOIN points_routes ON points_routes.cod_point = points.cod_point
    LEFT JOIN routes ON routes.cod_route = points_routes.cod_route
    LEFT JOIN trips ON trips.cod_route = routes.cod_route
    GROUP BY points.name_point) PT1 
LEFT JOIN (
    SELECT points.name_point, COUNT(trips.cod_trip) AS trips_count 
    FROM points 
    LEFT JOIN points_routes ON points_routes.cod_point = points.cod_point
    LEFT JOIN routes ON routes.cod_route = points_routes.cod_route
    LEFT JOIN trips ON trips.cod_route = routes.cod_route
    GROUP BY points.name_point) PT2
    ON PT1.trips_count < PT2.trips_count
WHERE PT2.trips_count IS NULL
```
----------
### 361. Пункты с макс. числом рейсов в выходные

> Названия пунктов, которые принимают наибольшее количество рейсов в выходные дни  (в предположении, что в какой день автобус выехал, в такой он и приедет)

```sql
SELECT PT1.name_point 
FROM (
    SELECT points.name_point, COUNT(trips.cod_trip) AS trips_count 
    FROM points 
    LEFT JOIN points_routes ON points_routes.cod_point = points.cod_point
    LEFT JOIN routes ON routes.cod_route = points_routes.cod_route
    LEFT JOIN trips ON trips.cod_route = routes.cod_route
    WHERE trips.week_day > 5 
    GROUP BY points.name_point) PT1 
LEFT JOIN (
    SELECT points.name_point, COUNT(trips.cod_trip) AS trips_count 
    FROM points 
    LEFT JOIN points_routes ON points_routes.cod_point = points.cod_point
    LEFT JOIN routes ON routes.cod_route = points_routes.cod_route
    LEFT JOIN trips ON trips.cod_route = routes.cod_route
    WHERE trips.week_day > 5 
    GROUP BY points.name_point) PT2
    ON PT1.trips_count < PT2.trips_count
WHERE PT2.trips_count IS NULL
```
----------
### 362. Пункты с макс. числом рейсов в понедельник

> Названия пунктов, которые принимают наибольшее количество рейсов в понедельник  (в предположении, что в какой день автобус выехал, в такой он и приедет)

```sql
SELECT PT1.name_point 
FROM (
    SELECT points.name_point, COUNT(trips.cod_trip) AS trips_count 
    FROM points 
    LEFT JOIN points_routes ON points_routes.cod_point = points.cod_point
    LEFT JOIN routes ON routes.cod_route = points_routes.cod_route
    LEFT JOIN trips ON trips.cod_route = routes.cod_route
    WHERE trips.week_day = 1 
    GROUP BY points.name_point) PT1 
LEFT JOIN (
    SELECT points.name_point, COUNT(trips.cod_trip) AS trips_count 
    FROM points 
    LEFT JOIN points_routes ON points_routes.cod_point = points.cod_point
    LEFT JOIN routes ON routes.cod_route = points_routes.cod_route
    LEFT JOIN trips ON trips.cod_route = routes.cod_route
    WHERE trips.week_day = 1 
    GROUP BY points.name_point) PT2
    ON PT1.trips_count < PT2.trips_count
WHERE PT2.trips_count IS NULL
```
----------
### 363. Популярные модели

> Марки автобусов, которые задействованы в наибольшем общем количестве рейсов

```sql
SELECT MT1.name_model 
FROM (
    SELECT models.name_model, COUNT(trips.cod_trip) AS trips_count 
    FROM trips
    LEFT JOIN buses ON buses.cod_bus = trips.cod_bus
    LEFT JOIN models ON models.cod_model = buses.cod_model
    GROUP BY models.name_model) MT1
LEFT JOIN (
    SELECT models.name_model, COUNT(trips.cod_trip) AS trips_count 
    FROM trips
    LEFT JOIN buses ON buses.cod_bus = trips.cod_bus
    LEFT JOIN models ON models.cod_model = buses.cod_model
    GROUP BY models.name_model) MT2
    ON MT1.trips_count < MT2.trips_count
WHERE MT2.trips_count IS NULL
```
----------
### 364. Самые используемые автобусы

> Номера автобусов, которые задействованы в наибольшем общем количестве рейсов

```sql
SELECT BT1.bus_number 
FROM (
    SELECT buses.bus_number, COUNT(trips.cod_trip) AS trips_count 
    FROM trips
    LEFT JOIN buses ON buses.cod_bus = trips.cod_bus
    GROUP BY buses.bus_number) BT1
LEFT JOIN (
    SELECT buses.bus_number, COUNT(trips.cod_trip) AS trips_count 
    FROM trips
    LEFT JOIN buses ON buses.cod_bus = trips.cod_bus
    GROUP BY buses.bus_number) BT2
    ON BT1.trips_count < BT2.trips_count
WHERE BT2.trips_count IS NULL
```
----------
### 365. Самые дальние авотбусы

> Номера автобусов, которые ходят в наиболее удаленные пункты

```sql
SELECT DISTINCT buses.bus_number 
FROM buses 
INNER JOIN trips ON trips.cod_bus = buses.cod_bus
INNER JOIN routes ON routes.cod_route = trips.cod_route
INNER JOIN points_routes ON points_routes.cod_route = routes.cod_route
INNER JOIN (
    SELECT P1.cod_point FROM points P1
    LEFT JOIN points P2 ON P1.distance < P2.distance
    WHERE P2.distance IS NULL) P
    ON P.cod_point = points_routes.cod_point
```
----------
### 366. Марки самых дальних автобусов

> Марки автобусов, которые ходят в наиболее удаленные пункты

```sql
SELECT DISTINCT models.name_model 
FROM models 
INNER JOIN buses ON buses.cod_model = models.cod_model
INNER JOIN trips ON trips.cod_bus = buses.cod_bus
INNER JOIN routes ON routes.cod_route = trips.cod_route
INNER JOIN points_routes ON points_routes.cod_route = routes.cod_route
INNER JOIN (
    SELECT P1.cod_point FROM points P1
    LEFT JOIN points P2 ON P1.distance < P2.distance
    WHERE P2.distance IS NULL) P
    ON P.cod_point = points_routes.cod_point
```
----------
### 367. Самые незадействованные автобусы

> Номера автобусов, которые задействованы в наименьшем количестве рейсов

```sql
SELECT BT1.bus_number 
FROM (
    SELECT buses.bus_number, COUNT(trips.cod_trip) AS trips_count 
    FROM buses
    LEFT JOIN trips ON trips.cod_bus = buses.cod_bus
    GROUP BY buses.bus_number) BT1
LEFT JOIN (
    SELECT buses.bus_number, COUNT(trips.cod_trip) AS trips_count 
    FROM buses
    LEFT JOIN trips ON trips.cod_bus = buses.cod_bus
    GROUP BY buses.bus_number) BT2
    ON BT1.trips_count > BT2.trips_count
WHERE BT2.trips_count IS NULL
```
----------
### 368. Рейсы в самые дальние пункты

> Номера рейсов с указанием имен маршрутов, часов и минут отправления в самые отдаленные пункты

```sql
SELECT trips.cod_trip, routes.name_route, trips.hour, trips.minute 
FROM trips
INNER JOIN routes ON routes.cod_route = trips.cod_route
INNER JOIN points_routes ON points_routes.cod_route = routes.cod_route
INNER JOIN (
    SELECT P1.cod_point FROM points P1
    LEFT JOIN points P2 ON P1.distance < P2.distance
    WHERE P2.distance IS NULL) P
    ON P.cod_point = points_routes.cod_point
```
----------
### 369. Самые популярные модели

> Названия марок с наибольшим количеством автобусов

```sql
SELECT MB1.name_model 
FROM (
    SELECT models.name_model, COUNT(buses.cod_bus) AS buses_count 
    FROM models
    LEFT JOIN buses ON buses.cod_model = models.cod_model
    GROUP BY models.name_model) MB1
LEFT JOIN (
    SELECT models.name_model, COUNT(buses.cod_bus) AS buses_count 
    FROM models
    LEFT JOIN buses ON buses.cod_model = models.cod_model
    GROUP BY models.name_model) MB2
    ON MB1.buses_count < MB2.buses_count
WHERE MB2.buses_count IS NULL
```
----------
### 1296. Пункты с 'C' из более чем 4 букв

>  Количество пунктов, начинающихся на букву «С» (английская) и имеющих в названии более 4 букв 

```sql
SELECT COUNT(cod_point) 
FROM points 
WHERE name_point LIKE 'C%' AND LENGTH(name_point) > 4
```
----------
### 1297. Средняя стоимость билета

>  Среднее значение стоимости билета 

```sql
SELECT AVG(K.price * P.distance) 
FROM trips T 
INNER JOIN routes R ON T.cod_route = R.cod_route 
INNER JOIN points_routes PR ON PR.cod_route = R.cod_route
INNER JOIN points P ON P.cod_point = PR.cod_point
INNER JOIN buses B ON T.cod_bus = B.cod_bus
INNER JOIN models M ON M.cod_model = B.cod_model
INNER JOIN km_prices K ON K.class = M.class
WHERE P.distance > 0
```
----------
### 1298. Некоторые остановки

>  Вывести названия остановок, начинающихся на буквы «Ч», «Ф», «М» и «Ц» в порядке, обратном алфавитному  

```sql
SELECT name_point 
FROM points 
WHERE name_point LIKE 'Ч%'
OR name_point LIKE 'Ф%'
OR name_point LIKE 'М%'
OR name_point LIKE 'Ц%'
ORDER BY name_point DESC
```
----------
### 1299. Рейсы до Сокола

>  Вывести номера рейсов, время (часы и минуты отдельно) и день недели по-русски (понедельник, вторник, …, воскресенье) всех рейсов, отправляющихся до Сокола (или проходящих через Сокол) 

```sql
SELECT trips.cod_trip, trips.hour, trips.minute, 
CASE trips.week_day
    WHEN 1 THEN 'понедельник' 
    WHEN 2 THEN 'вторник' 
    WHEN 3 THEN 'среда' 
    WHEN 4 THEN 'четверг'
    WHEN 5 THEN 'пятница'
    WHEN 6 THEN 'суббота'
    WHEN 7 THEN 'воскресенье' 
END AS day_of_week
FROM trips
LEFT JOIN routes ON routes.cod_route = trips.cod_route
LEFT JOIN points_routes ON points_routes.cod_route = routes.cod_route
LEFT JOIN points ON points.cod_point = points_routes.cod_point
WHERE points.name_point LIKE 'Сокол'
```
----------
### 1300. Максимальная выручка с рейса

>  Возможная максимальная выручка от проданных билетов рейса №21 

```sql
SELECT NVL(MAX(km_prices.price * models.places * points.distance), 0) AS max_sum 
FROM trips
LEFT JOIN routes ON routes.cod_route = trips.cod_route
LEFT JOIN points_routes ON points_routes.cod_route = routes.cod_route
LEFT JOIN points ON points.cod_point = points_routes.cod_point
LEFT JOIN buses ON buses.cod_bus = trips.cod_bus
LEFT JOIN models ON models.cod_model = buses.cod_model
LEFT JOIN km_prices ON km_prices.class = models.class
WHERE trips.cod_trip = 21
```
----------
### 1301. Вечером до Сокола недорого

>  Вывести номера автобусов, кол-во свободных мест и цену билета для тех рейсов, которые отправляются в пятницу вечером с 18:00 до 22:00 до Сокола и стоимость билета не превосходит 200 рублей 

```sql
SELECT 
    buses.bus_number, 
    models.places - trips.tickets AS free_places, 
    points.distance * km_prices.price AS ticket_price 
FROM trips
LEFT JOIN routes ON routes.cod_route = trips.cod_route
LEFT JOIN points_routes ON points_routes.cod_route = routes.cod_route
LEFT JOIN points ON points.cod_point = points_routes.cod_point
LEFT JOIN buses ON buses.cod_bus = trips.cod_bus
LEFT JOIN models ON models.cod_model = buses.cod_model
LEFT JOIN km_prices ON km_prices.class = models.class
WHERE (trips.hour >= 18)
    AND (trips.hour*60+trips.minute <= 22*60)
    AND points.name_point = 'Сокол'
    AND points.distance * km_prices.price <= 200
```
----------
### 1303. Поймайте нарушителя

>  Поступила жалоба на водителя автобуса, нарушившего ПДД, но свидетель помнит только то, что в гос. номере были цифры 5 и 7. Найдите номера автобусов, водители которых потенциальные нарушители. 

```sql
SELECT bus_number 
FROM buses
WHERE bus_number LIKE '%5%' AND bus_number LIKE '%7%'
```
----------
### 1304. Какое-то Новое

>  Пассажиру нужно попасть в село, название которого начинается со слова «Новое». Точное название он не помнит, но знает, что до нужного пункта не более 20 км. Выведите названия и расстояние до подходящих населенных пунктов в порядке возрастания расстояния (без учёта того, ходят ли туда какие-то рейсы). 

```sql
SELECT name_point, distance 
FROM points
WHERE name_point LIKE 'Новое%' and distance <= 20
ORDER BY distance
```
----------
### 1306. Билеты до Череповца

>  Вывести код рейса и количество оставшихся билетов до Череповца на все рейсы в пятницу 

```sql
SELECT trips.cod_trip, models.places - trips.tickets AS free_places 
FROM trips 
LEFT JOIN routes ON routes.cod_route = trips.cod_route
LEFT JOIN points_routes ON points_routes.cod_route = routes.cod_route
LEFT JOIN points ON points.cod_point = points_routes.cod_point
LEFT JOIN buses ON buses.cod_bus = trips.cod_bus
LEFT JOIN models ON models.cod_model = buses.cod_model
WHERE points.name_point = 'Череповец' AND trips.week_day = 5
```
----------
### 1307. Убыточные рейсы

>  Вывести коды  рейсов и названия маршрутов, на рейсы которых во вторник не продано ни одного билета 

```sql
SELECT trips.cod_trip, routes.name_route 
FROM trips 
LEFT JOIN routes ON routes.cod_route = trips.cod_route
WHERE trips.tickets = 0 AND trips.week_day = 2
```
----------
### 1308. Продажи по часам

>  Выведите по порядку часы от 6 утра до 21 вечера и количество проданных билетов за каждый час (например для 6 часов учитываются билеты, проданные на рейсы с отправлением с 6:00 до 6:59:59) 

```sql
SELECT H.hour, NVL(SUM(trips.tickets),0) 
FROM trips 
RIGHT JOIN (
    SELECT 6 AS hour FROM dual UNION ALL
    SELECT 7 FROM dual UNION ALL
    SELECT 8 FROM dual UNION ALL
    SELECT 9 FROM dual UNION ALL
    SELECT 10 FROM dual UNION ALL
    SELECT 11 FROM dual UNION ALL
    SELECT 12 FROM dual UNION ALL
    SELECT 13 FROM dual UNION ALL
    SELECT 14 FROM dual UNION ALL
    SELECT 15 FROM dual UNION ALL
    SELECT 16 FROM dual UNION ALL
    SELECT 17 FROM dual UNION ALL
    SELECT 18 FROM dual UNION ALL
    SELECT 19 FROM dual UNION ALL
    SELECT 20 FROM dual UNION ALL
    SELECT 21 FROM dual) H
    ON H.hour = trips.hour
GROUP BY H.hour
```
----------
### 1310. Куда уехать за 100 рублей?

>  Выведите названия всех пунктов, до которых можно добраться, имея в кармане 100 рублей 

```sql
SELECT DISTINCT points.name_point 
FROM trips
LEFT JOIN routes ON routes.cod_route = trips.cod_route
LEFT JOIN points_routes ON points_routes.cod_route = routes.cod_route
LEFT JOIN points ON points.cod_point = points_routes.cod_point
LEFT JOIN buses ON buses.cod_bus = trips.cod_bus
LEFT JOIN models ON models.cod_model = buses.cod_model
LEFT JOIN km_prices ON km_prices.class = models.class
WHERE km_prices.price * points.distance BETWEEN 1 AND 100
```
----------
### 1312. Сколько нужно топлива?

>  Определите, сколько нужно литров топлива, чтобы добраться до самого дальнего пункта и вернуться обратно (неважно, есть ли туда рейсы). Известно, что средний расход топлива составляет 30 литров на 100 километров. Ответ округлите в большую сторону до целого числа литров. 

```sql
SELECT CEIL(MAX(distance)*2*0.3) FROM points
```
----------
### 1313. Задействованные автобусы

>  Вывести все задействованные в рейсах автобусы (название модели и количество), отсортировав в порядке убывания количества 

```sql
SELECT models.name_model, COUNT(trips.cod_trip) AS trips_count 
FROM trips
LEFT JOIN buses ON buses.cod_bus = trips.cod_bus
LEFT JOIN models ON models.cod_model = buses.cod_model
GROUP BY models.name_model
ORDER BY trips_count DESC
```
----------
### 1314. Самый загруженный день

>  Определите, в какой день (или дни) недели отправляется больше всего рейсов. Выведите день и количество. 

```sql
SELECT T1.week_day, T1.count_trip 
FROM (
    SELECT week_day, COUNT(cod_trip) AS count_trip 
    FROM trips
    GROUP BY week_day) T1
LEFT JOIN (
    SELECT week_day, COUNT(cod_trip) AS count_trip 
    FROM trips
    GROUP BY week_day) T2
    ON T1.count_trip < T2.count_trip
WHERE T2.week_day IS NULL
```
----------
### 1315. Самый распространённый класс

>  Определите, автобусов какого класса (или классов) больше всего в автопарке. Выведите количество и класс. 

```sql
SELECT BC1.count_buses, BC1.class 
FROM (
    SELECT models.class, COUNT(buses.cod_bus) AS count_buses 
    FROM buses
    LEFT JOIN models ON models.cod_model = buses.cod_model
    GROUP BY models.class) BC1
LEFT JOIN (
    SELECT models.class, COUNT(buses.cod_bus) AS count_buses 
    FROM buses
    LEFT JOIN models ON models.cod_model = buses.cod_model
    GROUP BY models.class) BC2
    ON BC1.count_buses < BC2.count_buses
WHERE BC2.class IS NULL
```
