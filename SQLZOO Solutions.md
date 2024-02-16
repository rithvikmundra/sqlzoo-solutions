# SQLZOO Solutions

### Author - Rithvik Mundra

### Date - 15 Feb 2024

This repo hosts the solutions for SQL questions posted at SQLZOO as of February 15, 2024 - [SQLZOO Tutorial](http://sqlzoo.net/wiki/SQL_Tutorial).  

## Sections:
1. [SELECT basics](#select-basics)
2. [SELECT from WORLD](#select-from-world)
3. [SELECT from NOBEL](#select-from-nobel)
4. [SELECT in SELECT](#select-in-select)
5. [SUM and COUNT](#sum-and-count)
6. [JOIN](#join)
7. [More JOIN](#more-join)
8. [Using NULL](#using-null)
9. [Self JOIN](#self-join)

## SELECT basics

1. Where Clause
```sql
SELECT population 
FROM world
WHERE name = 'Germany';
```
2. Scandinavia
```sql
SELECT name, population 
FROM world
WHERE name IN ('Sweden', 'Norway', 'Denmark');
```
3. Just the right size
```sql
SELECT name, area 
FROM world
WHERE area BETWEEN 200000 AND 250000;
```

## SELECT from WORLD
1. Introduction
```sql
SELECT name, continent, population 
FROM world;
```
2. Large Countries
```sql
SELECT name 
FROM world
WHERE population >= 200000000;
```
3. Per capita GDP
```sql
Select name, GDP/population
from world
where population >= 200000000;
```
4. South America In millions
```sql
select name, population/ 1000000
from world
where continent = 'South America';
```
5. France, Germany, Italy
```sql
select name, population 
from world
where name in ('France', 'Germany','Italy');
```
6. United
```sql
select name
from world
where lower(name) like '%united%' ;
```
7. Two ways to be big
```sql
select name, population, area
from world
where area >= 3000000 or population >= 250000000;
```
8. One or the other (but not both)
```sql
select name, population, area
from world
where name not in (
select distinct name from world
where area >= 3000000 and population >= 250000000
) 
and (area >= 3000000 or population >= 250000000);
```
9. Rounding
```sql
select name, round(population/1000000,2), round(GDP/1000000000,2)
from world
where continent = 'South America';
```
10. Trillion dollar economies
```sql
select name, round(gdp/population,-3)
from world
where gdp > 1000000000000;
```
11. Name and capital have the same length
```sql
select name, capital
from world
where length(name) = length(capital);
```
12. Matching name and capital
```sql
select name, capital
from world 
where left(upper(name),1) = left(upper(capital),1)
and (name != capital);
```
13. All the vowels

```sql
select name
from world
where lower(name) like '%a%' 
and  lower(name) like '%e%' 
and  lower(name) like '%i%' 
and  lower(name) like '%o%' 
and  lower(name) like '%u%'
and name not like '% %';
```
