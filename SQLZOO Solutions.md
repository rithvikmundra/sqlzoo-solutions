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

## SELECT from NOBEL
1. Winners from 1950
```sql
select *
from nobel
where yr = 1950;
```
2. 1962 Literature
```sql
select winner
from nobel
where yr = 1962
and subject = 'literature';
```
3. Albert Einstein
```sql
select yr, subject
from nobel
where winner = 'Albert Einstein';
```
4. Recent Peace Prizes
```sql
select winner
from nobel
where subject = 'peace'
and yr >= '2000';
```
5. Literature in the 1980's
```sql
select *
from nobel
where subject = 'literature'
and yr between 1980 and 1989;
```
6. Only Presidents
```sql
select *
from nobel
where winner in ('Theodore Roosevelt', 'Thomas Woodrow Wilson', 'Jimmy Carter', 'Barack Obama');
```
7. John
```sql
select winner from 
nobel
where lower(substring_index(winner, " ", 1)) = 'john';
```
8. Chemistry and Physics from different years
```sql
select *
from nobel 
where yr = 1980 and subject= 'physics'

union all

select *
from nobel 
where yr = 1984 and subject= 'chemistry';
```
9. Exclude Chemists and Medics

```sql
select *
from nobel 
where yr = 1980 and subject not in ( 'chemistry', 'medicine');
```
10. Early Medicine, Late Literature
```sql
select *
from nobel 
where yr < 1910 and subject= 'medicine'

union all

select *
from nobel 
where yr >= 2004 and subject= 'literature';
```
#### Harder Questions
11. Umlaut
```sql
select *
from nobel
where winner = 'PETER GRÜNBERG';
```
12. Apostrophe
```sql
select *
from nobel
where winner = 'EUGENE O\'NEILL';
```
13. Knights of the realm
```sql
select winner, yr, subject
from nobel
where winner like 'Sir%'
order by yr desc, winner;
```
14. Chemistry and Physics last
```sql
select winner, subject
from nobel 
where yr =1984
order by 
case when subject in ('chemistry', 'physics') then 1 else 0 end,
subject, 
winner;
```
## SELECT in SELECT
1. Bigger than Russia

```sql
select name from world 
where population > (
select population 
from world 
where name = 'Russia'
);
```
2. Richer than UK

```sql
select name from world 
where continent = 'Europe'
and gdp/population > (
select gdp/population 
from world
where name = 'United Kingdom'
);
```
3. Neighbours of Argentina and Australia
```sql
select name, continent
from world
where continent in (select continent from world where name in ('Argentina', 'Australia'))
order by name;
```
4. Between Canada and Poland
```sql
select name, population from world
where population > (select population from world where name = 'United Kingdom')
and population < (select population from world where name = 'Germany');
```
5. Percentages of Germany
```sql
select name, concat(cast(round(population * 100/(select population from world where name = 'Germany')) as char), '%')
from world
where continent='Europe';
```
6. Bigger than every country in Europe
```sql
select name
from world
where gdp >= (select max(gdp) from world where continent='Europe')
and continent !='Europe';
```
7. Largest in each continent
```sql
select continent, name, area
from world w
where area >= (select max(area) from world w2 where w2.continent = w.continent);

```
8. First country of each continent (alphabetically)
```sql
select continent,name
from
(select name, continent, dense_rank() over (partition by continent order by name ) as rnk
from world)a
where rnk=1
order by name;
```
9. Difficult Questions That Utilize Techniques Not Covered In Prior Sections
```sql
select name, continent, population
from world
where continent in
(select continent 
from world 
having max(population) <= 25000000);
```
10. Three time bigger
```sql
select name,
continent
from world w1
where population >=  ALL
(
select 3*population 
from world w2 
where w2.name <> w1.name 
and w1.continent = w2.continent);
```
## SUM and COUNT
1. Total world population
```sql
select sum(population)
from world;
```
2. List of continents
```sql
select distinct continent
from world;
```
3. GDP of Africa

```sql
select sum(gdp)
from world
where continent='Africa';
```
4. Count the big countries

```sql
select count(name)
from world
where area >= 1000000;
```
5. Baltic states population

```sql
select sum(population)
from world
where name in ('Estonia', 'Latvia', 'Lithuania');
```
6. Counting the countries of each continent

```sql
select continent, count(name)
from world
group by continent;
```
7. Counting big countries in each continent

```sql
select continent, count(name)
from world
where population >= 10000000
group by continent;
```
8. Counting big continents

```sql
select distinct continent
from world
where population >= 100000000
order by continent;
```
## JOIN
1.
```sql
Select matchid, player
from goal
where teamid = 'GER';
```
2.
```sql
SELECT id,stadium,team1,team2
FROM game
where id = 1012;

```
3.
```sql
SELECT player,teamid, stadium, mdate
FROM game JOIN goal ON id=matchid
where teamid = 'GER';

```
4.
```sql
select team1, team2, player
from game join goal 
on id = matchid
where player like 'Mario%' ;
```
5.
```sql
SELECT player, teamid, coach, gtime
FROM goal join eteam
on teamid = id
WHERE gtime<=10;

```
6.
```sql
SELECT mdate, e1.teamname
FROM game join eteam e1
on team1 = e1.id
join eteam e3
on team2 = e3.id 
WHERE e1.coach = 'Fernando Santos' ;
```
7.
```sql
select player
from game join goal 
on id = matchid
where stadium = 'National Stadium, Warsaw';
```
#### More Difficult Questions
8.
```sql
SELECT distinct player
FROM game JOIN goal ON matchid = id 
WHERE (team1='GER' or team2='GER')
and teamid <> 'GER';
```
9.
```sql
SELECT teamname, count(gtime)
FROM eteam JOIN goal ON id=teamid
Group BY teamname;

```
10.
```sql
select stadium, count(gtime)
from game join goal
on id=matchid
group by stadium;
```
11.
```sql
select matchid, mdate, count(gtime)
from game join goal
on id=matchid
where team1 = 'POL' OR team2='POL'
group by matchid, mdate;
```
12.
```sql
select matchid, mdate, count(gtime)
from game join goal
on id=matchid
where teamid ='GER'
group by matchid, mdate;
```
13.
```sql
SELECT 
distinct
mdate,
team1,
sum(case when teamid = team1 then 1 else 0 end) as score1,
team2,
sum(case when teamid = team2 then 1 else 0 end) as score2
FROM game left JOIN goal 
ON matchid = id
group by mdate,team1, team2
order by mdate, matchid, team1, team2;
```
## More JOIN
1. 1962 movies

```sql
select id, title
from movie
where yr = 1962;
```
2. When was Citizen Kane released?

```sql
select yr
from movie
where title = 'Citizen Kane';
```
3. Star Trek movies

```sql
select id, title, yr
from movie
where title like '%star trek%'
order by yr;
```
4. id for actor Glenn Close

```sql
select id
from actor
where name = 'Glenn Close';
```
5. id for Casablanca

```sql
select id
from movie
where title = 'Casablanca';
```
6. Cast list for Casablanca

```sql
select a.name
from movie join casting
on id = movieid
join actor a
on a.id = actorid
where title = 'Casablanca';
```
7. Alien cast list

```sql
select a.name
from movie join casting
on id = movieid
join actor a
on a.id = actorid
where title = 'Alien';
```
8. Harrison Ford movies
```sql
select title
from movie join casting
on id = movieid
join actor a
on a.id = actorid
where name = 'Harrison Ford';
```
9. Harrison Ford as a supporting actor
```sql
select title
from movie join casting
on id = movieid
join actor a
on a.id = actorid
where name = 'Harrison Ford' and ord !=1;
```
10. Lead actors in 1962 movies
```sql
select title, name
from movie join casting
on id = movieid
join actor a
on a.id = actorid
where yr = 1962 and ord=1;
```
#### Harder Questions

11. Busy years for Rock Hudson
```sql
select yr, count(title)
from movie join casting
on id = movieid
join actor a
on a.id = actorid
where name = 'Rock Hudson'
group by  yr
having count(title)>2;
```
12. Lead actor in Julie Andrews movies
```sql
with tbl as (select 
m.id,
title, 
yr,
director,
budget,
gross,
movieid,
actorid,
ord,
name
from movie m left join casting c
on id = movieid
left join actor a
on a.id = actorid)

select title, name
from tbl 
where id in (
select id from tbl where name ='Julie Andrews' )
and ord=1;
```
13. Actors with 15 leading roles
```sql
select a.name
from movie m left join casting c
on id = movieid
left join actor a
on a.id = actorid
where ord=1
group by a.name
having count(m.id)>=15
order by a.name;
```
14. released in the year 1978

```sql
select title, count(name)
from movie m join casting c
on id = movieid join actor a
on a.id = actorid
where yr = 1978
group by title
order by count(name) desc, title;


```
15. with 'Art Garfunkel'

```sql
select distinct a.name 
from movie m join casting c
on id = movieid join actor a
on a.id = actorid
where title in 
 (select title
from movie m join casting c
on id = movieid join actor a
on a.id = actorid
where a.name = 'Art Garfunkel')
and a.name != 'Art Garfunkel';
```

## Using NULL
1.
```sql
select name
from teacher 
where dept is null;
```
2.
```sql
SELECT teacher.name, dept.name
 FROM teacher INNER JOIN dept
           ON (teacher.dept=dept.id);

```
3.
```sql
select t.name, d.name
from teacher t left join dept d 
on t.dept = d.id;
```
4.
```sql
select t.name, d.name
from dept d  left join teacher t
on t.dept = d.id;
```
5.
```sql
select name, coalesce(mobile, '07986 444 2266')
from teacher;
```
6.
```sql
select t.name, coalesce(d.name, 'None')
from teacher t left join dept d 
on t.dept = d.id;
```
7.
```sql
select count(name), count(mobile)
from teacher;
```
8.
```sql
select d.name, count(t.name)
from teacher t right join dept d 
on t.dept = d.id
group by d.name;
```
9.
```sql
select t.name,
case when dept in (1,2) then 'Sci'
else 'Art' end as new_name
from teacher t left join dept d 
on t.dept = d.id;
```
10.
```sql
select t.name,
case when dept in (1,2) then 'Sci'
when dept = 3 then 'Art' 
else 'None' end as new_name
from teacher t left join dept d 
on t.dept = d.id;
```
## Self JOIN
1.
```sql
select count(id) 
from stops;
```
2.
```sql
select id 
from stops 
where name = 'Craiglockhart';
```
3.
```sql
SELECT id, name 
FROM stops, route
WHERE stops.id = route.stop
AND num = '4'
AND company = 'LRT';

```
4.
```sql
SELECT company, num, COUNT(*)
FROM route WHERE stop=149 OR stop=53
GROUP BY company, num
having  COUNT(*)>=2;

```
5.
```sql
SELECT a.company, a.num, a.stop, b.stop
FROM route a JOIN route b ON
  (a.company=b.company AND a.num=b.num)
WHERE a.stop=53 and b.stop= 149;

```
6.
```sql
SELECT a.company, a.num, stopa.name, stopb.name
FROM route a JOIN route b ON
  (a.company=b.company AND a.num=b.num)
  JOIN stops stopa ON (a.stop=stopa.id)
  JOIN stops stopb ON (b.stop=stopb.id)
WHERE stopa.name='Craiglockhart'
and stopb.name = 'London Road';

```
7.
```sql
select distinct  a.company, a.num
from route a join route b
on a.num = b.num and a.company = b.company
where a.stop = 115 and b.stop = 137;
```
8.
```sql
select distinct  a.company, a.num
from route a join route b
on a.num = b.num and a.company = b.company
join stops s1 on s1.id = a.stop
join stops s2 on s2.id = b.stop
where s1.name = 'Craiglockhart'  and s2.name = 'Tollcross';
```
9.
```sql
select distinct  s2.name, a.company, a.num
from route a join route b
on a.num = b.num and a.company = b.company
join stops s1 on s1.id = a.stop
join stops s2 on s2.id = b.stop
where s1.name = 'Craiglockhart';
```
10.
```sql
select r1.num, r1.company,s2.name, r3.num, r3.company 
from route r1 join route r2
on r1.num=r2.num and r1.company = r2.company 
join stops s1 on r1.stop = s1.id 
join stops s2 on r2.stop=s2.id
join route r3 on r3.stop = r2.stop
join route r4 on r4.num = r3.num and r4.company = r3.company
join stops s3 on r4.stop = s3.id
where s1.name = 'Craiglockhart' and s3.name = 'Lochend'
order by r1.num, r1.company, s2.name, r3.num, r3.company;
```
