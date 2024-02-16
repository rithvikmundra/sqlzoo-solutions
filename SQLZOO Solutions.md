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
