# More JOIN

## 1.
List the films where the **yr** is 1962 [Show **id**, **title**]
```SQL
SELECT id, title
 FROM movie
 WHERE yr=1962
```

## 2.
Give year of 'Citizen Kane'.
```SQL
SELECT yr
FROM movie
WHERE title = 'Citizen Kane'
```

## 3.
List all of the Star Trek movies, include the **id**, **title** and **yr** (all of these movies include the words Star Trek in the title). Order results by year.
```SQL
SELECT id, title, yr
FROM movie
WHERE title LIKE '%Star Trek%'
ORDER BY yr
```

## 4.
What **id** number does the actor 'Glenn Close' have?
```SQL
SELECT id
FROM actor
WHERE name = 'Glenn Close'
```

## 5.
What is the id of the film 'Casablanca'
```SQL
SELECT id
FROM movie
WHERE title = 'Casablanca'
```

## 6.
Obtain the cast list for 'Casablanca'.

what is a cast list?
Use **movieid=11768**, (or whatever value you got from the previous question)
```SQL
SELECT name
FROM casting JOIN actor on actorid = id
WHERE movieid = 11768
```

## 7.
Obtain the cast list for the film 'Alien'
```SQL
SELECT name
FROM casting JOIN actor ON casting.actorid = actor.id
JOIN movie ON movie.id = casting.movieid
WHERE title = 'Alien'
```

## 8.
List the films in which 'Harrison Ford' has appeared
```SQL
SELECT title
FROM casting JOIN actor ON casting.actorid = actor.id
JOIN movie ON movie.id = casting.movieid
WHERE name = 'Harrison Ford'
```

## 9.
List the films where 'Harrison Ford' has appeared - but not in the starring role. [Note: the **ord** field of casting gives the position of the actor. If ord=1 then this actor is in the starring role]
```SQL
SELECT title
FROM casting JOIN actor ON casting.actorid = actor.id
JOIN movie ON movie.id = casting.movieid
WHERE name = 'Harrison Ford'
AND ord != 1
```

## 10.
List the films together with the leading star for all 1962 films.
```SQL
SELECT title, name
FROM casting JOIN actor ON casting.actorid = actor.id
JOIN movie ON movie.id = casting.movieid
WHERE ord = 1
AND yr = 1962
```

## 11.
Which were the busiest years for 'Rock Hudson', show the year and the number of movies he made each year for any year in which he made more than 2 movies.
```SQL
SELECT yr,COUNT(title)
FROM movie
  JOIN casting ON movie.id=movieid
  JOIN actor   ON actorid=actor.id
WHERE name='Rock Hudson'
GROUP BY yr
HAVING COUNT(title) > 2
```

## 12.
List the film title and the leading actor for all of the films 'Julie Andrews' played in.
```SQL
SELECT title, name
FROM actor JOIN casting ON actor.id = casting.actorid
JOIN movie ON casting.movieid = movie.id
WHERE movieid IN (SELECT movieid FROM casting
WHERE actorid IN (
  SELECT id FROM actor
  WHERE name='Julie Andrews'))
AND ord = 1
```

## 13.
Obtain a list, in alphabetical order, of actors who've had at least 15 **starring** roles.
```SQL
SELECT name
FROM actor JOIN casting ON casting.actorid = actor.id
WHERE ord = 1
GROUP BY name
HAVING COUNT(movieid) >= 15
```

## 14.
List the films released in the year 1978 ordered by the number of actors in the cast, then by title.
```SQL
SELECT title, COUNT(actorid)
FROM movie JOIN casting ON casting.movieid = movie.id
WHERE yr = 1978
GROUP BY title
ORDER BY COUNT(actorid) DESC, title
```

## 15.
List all the people who have worked with 'Art Garfunkel'.
```SQL
SELECT name
FROM actor JOIN casting ON casting.actorid = actor.id
WHERE movieid IN(
  SELECT movieid
  FROM casting
  WHERE actorid = (
    SELECT id
    FROM actor
    WHERE name = 'Art Garfunkel'
  )
)
AND name <> 'Art Garfunkel';
```

# Quiz

#### 1. Select the statement which lists the unfortunate directors of the movies which have caused financial loses (gross < budget)

```SQL
SELECT name
  FROM actor INNER JOIN movie ON actor.id = director
 WHERE gross < budget
 ```

 #### 2. Select the correct example of JOINing three tables

 ```SQL
 SELECT *
  FROM actor JOIN casting ON actor.id = actorid
  JOIN movie ON movie.id = movieid
```

#### 3. Select the statement that shows the list of actors called 'John' by order of number of movies in which they acted

```SQL
SELECT name, COUNT(movieid)
  FROM casting JOIN actor ON actorid=actor.id
 WHERE name LIKE 'John %'
 GROUP BY name ORDER BY 2 DESC
```

#### 4. Select the result that would be obtained from the following code:
```SQL
 SELECT title 
   FROM movie JOIN casting ON (movieid=movie.id)
              JOIN actor   ON (actorid=actor.id)
  WHERE name='Paul Hogan' AND ord = 1
```

| title |
| ----- |
| "Crocodile" Dundee |
| Crocodile Dundee in Los Angeles |
| Flipper |
| Lightning Jack |

#### 5. Select the statement that lists all the actors that starred in movies directed by Ridley Scott who has id 351

```SQL
SELECT name
  FROM movie JOIN casting ON movie.id = movieid
  JOIN actor ON actor.id = actorid
WHERE ord = 1 AND director = 351
```

#### 6. There are two sensible ways to connect movie and actor. They are:

- link the director column in movies with the primary key in actor
- connect the primary keys of movie and actor via the casting table

#### 7. Select the result that would be obtained from the following code:
```SQL
 SELECT title, yr 
   FROM movie, casting, actor 
  WHERE name='Robert De Niro' AND movieid=movie.id AND actorid=actor.id AND ord = 3
```

title | yr
----- | --
A Bronx Tale | 1993
Bang the Drum Slowly | 1973
Limitless | 2011