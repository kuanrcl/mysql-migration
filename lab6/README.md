# Lab 6: Test drive MySQL HeatWave acceleration with more queries

## Introduction

In this lab, you will execute a few more long running queries on MySQL HeatWave to appreciate the performance gain using MySQL HeatWave
What we will do is to execute the queries against MDS and MySQL HeatWave to compare the performance, we will use the magic switch **use_secondary_engine** to specify where are we going to send the queries to. If **use_secondary_engine** is enabled, the query will be sent to  MySQL HeatWave, otherwise, the query will be sent to MDS

Estimated lab time: 10 minutes

### Task 1: Execute Query 1
Query 1: Find per-country average age of passengers from Switzerland, Italy and France

```
mysqlsh --user=admin --password=**PASSWORD** --host=<mysql_private_ip_address> --port=3306 --sql
USE airportdb;
SET SESSION use_secondary_engine=off;
```

```
SELECT
airline.airlinename,
AVG(datediff(departure,birthdate)/365.25) as avg_age,
count(*) as nb_people
FROM
booking, flight, airline, passengerdetails
WHERE
booking.flight_id=flight.flight_id AND
airline.airline_id=flight.airline_id AND
booking.passenger_id=passengerdetails.passenger_id AND
country IN ("SWITZERLAND", "FRANCE", "ITALY")
GROUP BY
airline.airlinename
ORDER BY
airline.airlinename, avg_age
LIMIT 10;
```

This query will take about 13s to execute. Record the response time for comparison.
Re-execute the query against MySQL HeatWave

```
SET SESSION use_secondary_engine=on;
```
```
SELECT
airline.airlinename,
AVG(datediff(departure,birthdate)/365.25) as avg_age,
count(*) as nb_people
FROM
booking, flight, airline, passengerdetails
WHERE
booking.flight_id=flight.flight_id AND
airline.airline_id=flight.airline_id AND
booking.passenger_id=passengerdetails.passenger_id AND
country IN ("SWITZERLAND", "FRANCE", "ITALY")
GROUP BY
airline.airlinename
ORDER BY
airline.airlinename, avg_age
LIMIT 10;
```
This query will now take less than 1s to execute, record the response time for comparison.

### Task 2: Execute Query 2

Query 2: Find top 10 airlines selling the most tickets for planes taking off from US airports. Run Pricing Summary Report Query:

```
mysqlsh --user=admin --password=**PASSWORD** --host=<mysql_private_ip_address> --port=3306 --sql
USE airportdb;
SET SESSION use_secondary_engine=off;
```
```
SELECT
airline.airlinename,
SUM(booking.price) as price_tickets,
count(*) as nb_tickets
FROM
booking, flight, airline, airport_geo
WHERE
booking.flight_id=flight.flight_id AND
airline.airline_id=flight.airline_id AND
flight.from=airport_geo.airport_id AND
airport_geo.country = "UNITED STATES"
GROUP BY
airline.airlinename
ORDER BY
nb_tickets desc, airline.airlinename
LIMIT 10;
```

This query will take about 24s to execute. Record the response time for comparison.
Re-execute the query against MySQL HeatWave

```
SET SESSION use_secondary_engine=on;
```
```
SELECT
airline.airlinename,
SUM(booking.price) as price_tickets,
count(*) as nb_tickets
FROM
booking, flight, airline, airport_geo
WHERE
booking.flight_id=flight.flight_id AND
airline.airline_id=flight.airline_id AND
flight.from=airport_geo.airport_id AND
airport_geo.country = "UNITED STATES"
GROUP BY
airline.airlinename
ORDER BY
nb_tickets desc, airline.airlinename
LIMIT 10;
```
This query will now take less than 1s to execute, record the response time for comparison.

### Task 3: Execute Query 3

Query 3: Find the number of bookings that Neil Armstrong and Buzz Aldrin made for a price of > $400.00

```
mysqlsh --user=admin --password=**PASSWORD** --host=<mysql_private_ip_address> --port=3306 --sql
USE airportdb;
SET SESSION use_secondary_engine=off;
```
```
SELECT
firstname,
lastname,
COUNT(booking.passenger_id) AS count_bookings
FROM
passenger,
booking
WHERE
booking.passenger_id = passenger.passenger_id
    AND passenger.lastname = 'Aldrin'
    OR (passenger.firstname = 'Neil'
    AND passenger.lastname = 'Armstrong')
    AND booking.price > 400.00
GROUP BY firstname , lastname;
```

This query will take about 40s to exeute, record the response time for comparison.

Re-execute the query against MySQL HeatWave

```
SET SESSION use_secondary_engine=on;
```
```
SELECT
firstname,
lastname,
COUNT(booking.passenger_id) AS count_bookings
FROM
passenger,
booking
WHERE
booking.passenger_id = passenger.passenger_id
    AND passenger.lastname = 'Aldrin'
    OR (passenger.firstname = 'Neil'
    AND passenger.lastname = 'Armstrong')
    AND booking.price > 400.00
GROUP BY firstname , lastname;
```
This query will now take less than 4s to exeute, record the response time for comparison.

## It works

You just experienced the awesome power of MySQL HeatWave engine!

## Congratulations, you have completed all the labs

[Home](../README.md)
