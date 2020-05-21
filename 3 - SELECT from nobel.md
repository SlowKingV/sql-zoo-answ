# SELECT from nobel

## 1.
Change the query shown so that it displays Nobel prizes for 1950.
```SQL
SELECT yr, subject, winner
FROM nobel
WHERE yr = 1950
```

## 2.
Show who won the 1962 prize for Literature.
```SQL
SELECT winner
FROM nobel
WHERE yr = 1962
  AND subject = 'Literature'
```

## 3.
Show the year and subject that won 'Albert Einstein' his prize.
```SQL
SELECT yr, subject
FROM nobel
WHERE winner = 'Albert Einstein'
```

## 4.
Give the name of the 'Peace' winners since the year 2000, including 2000.
```SQL
SELECT winner
FROM nobel
WHERE subject = 'Peace'
  AND yr >= 2000
```

## 5.
Show all details (**yr, subject, winner**) of the Literature prize winners for 1980 to 1989 inclusive.
```SQL
SELECT yr, subject, winner
FROM nobel
WHERE subject = 'Literature'
  AND yr BETWEEN 1980 AND 1989
```

## 6.
Show all details of the presidential winners:

- Theodore Roosevelt
- Woodrow Wilson
- Jimmy Carter
- Barack Obama
```SQL
SELECT * FROM nobel
WHERE winner IN (
  'Theodore Roosevelt',
  'Woodrow Wilson',
  'Jimmy Carter',
  'Barack Obama'
)
```

## 7.
Show the winners with first name John
```SQL
SELECT winner
FROM nobel
WHERE winner LIKE 'John%'
```

## 8.
**Show the year, subject, and name of Physics winners for 1980 together with the Chemistry winners for 1984.**
```SQL
SELECT yr, subject, winner
FROM nobel
WHERE (subject = 'Physics' AND yr = 1980) OR (subject = 'Chemistry' AND yr = 1984)
```

## 9.
**Show the year, subject, and name of winners for 1980 excluding Chemistry and Medicine**
```SQL
SELECT yr, subject, winner
FROM nobel
WHERE yr = 1980 AND subject <> 'Chemistry' AND subject <> 'Medicine'
```

## 10.
Show year, subject, and name of people who won a 'Medicine' prize in an early year (before 1910, not including 1910) together with winners of a 'Literature' prize in a later year (after 2004, including 2004)
```SQL
SELECT yr, subject, winner
FROM nobel
WHERE (subject = 'Medicine' AND yr < 1910) OR (subject = 'Literature' AND yr >= 2004)
```

## 11.
Find all details of the prize won by PETER GRÜNBERG
```SQL
SELECT *
FROM nobel
WHERE winner LIKE 'PETER GRÜNBERG'
```

## 12.
Find all details of the prize won by EUGENE O'NEILL
```SQL
SELECT *
FROM nobel
WHERE winner LIKE 'EUGENE O''NEILL'
```

## 13.
Knights in order

**List the winners, year and subject where the winner starts with Sir. Show the the most recent first, then by name order.**
```SQL
SELECT winner, yr, subject
FROM nobel
WHERE winner LIKE 'Sir%'
ORDER BY yr DESC, winner
```

## 14.
The expression **subject IN ('Chemistry','Physics')** can be used as a value - it will be **0** or **1**.

**Show the 1984 winners and subject ordered by subject and winner name; but list Chemistry and Physics last.**
```SQL
SELECT winner, subject
FROM nobel
WHERE yr=1984
ORDER BY subject IN ('Physics', 'Chemistry'), subject, winner
```

# Quiz

#### 1. Pick the code which shows the name of winner's names beginning with C and ending in n

```SQL
SELECT winner FROM nobel
 WHERE winner LIKE 'C%' AND winner LIKE '%n'
```

#### 2. Select the code that shows how many Chemistry awards were given between 1950 and 1960

```SQL
SELECT COUNT(subject) FROM nobel
 WHERE subject = 'Chemistry'
   AND yr BETWEEN 1950 and 1960
```

#### 3. Pick the code that shows the amount of years where no Medicine awards were given

```SQL
SELECT COUNT(DISTINCT yr) FROM nobel
 WHERE yr NOT IN (SELECT DISTINCT yr FROM nobel WHERE subject = 'Medicine')
```

#### 4. Select the result that would be obtained from the following code:
```SQL
SELECT subject, winner FROM nobel WHERE winner LIKE 'Sir%' AND yr LIKE '196%'
```

subject | winner
------- | ------
Medicine | Sir John Eccles
Medicine | Sir Frank Macfarlane Burnet

#### 5. Select the code which would show the year when neither a Physics or Chemistry award was given

```SQL
SELECT yr FROM nobel
 WHERE yr NOT IN(SELECT yr 
                   FROM nobel
                 WHERE subject IN ('Chemistry','Physics'))
```

#### 6. Select the code which shows the years when a Medicine award was given but no Peace or Literature award was

```SQL
SELECT DISTINCT yr
  FROM nobel
 WHERE subject='Medicine' 
   AND yr NOT IN(SELECT yr FROM nobel 
                  WHERE subject='Literature')
   AND yr NOT IN (SELECT yr FROM nobel
                   WHERE subject='Peace')
```

#### 7. Pick the result that would be obtained from the following code:
```SQL
 SELECT subject, COUNT(subject) 
   FROM nobel 
  WHERE yr ='1960' 
  GROUP BY subject
```

subject | COUNT(subject)
------- | --------------
Chemistry | 1
Literature | 1
Medicine | 2
Peace | 1
Physics | 1