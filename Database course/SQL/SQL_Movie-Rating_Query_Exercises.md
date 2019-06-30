- Exercises: You've started a new movie-rating website, and you've been collecting data on reviewers' ratings of various movies. There's not much data yet, but you can still try out some interesting queries. Here's the schema:
```
Movie ( mID, title, year, director )
English: There is a movie with ID number mID, a title, a release year, and a director.

Reviewer ( rID, name )
English: The reviewer with ID number rID has a certain name.

Rating ( rID, mID, stars, ratingDate )
English: The reviewer rID gave the movie mID a number of stars rating (1-5) on a certain ratingDate.
```

- Q1 : Find the titles of all movies directed by Steven Spielberg.
```SQL
SELECT title
FROM Movie
WHERE director = 'Steven Spielberg'
```

- Q2 : Find all years that have a movie that received a rating of 4 or 5, and sort them in increasing order.
```SQL
SELECT DISTINCT year
FROM Movie JOIN Rating USING(mID)
WHERE stars = 4 OR stars = 5;
```

- Q3 : Find the titles of all movies that have no ratings.
```SQL
SELECT DISTINCT title
FROM Movie
WHERE title NOT IN (SELECT title FROM Movie NATURAL JOIN Rating)
```

- Q4 : Some reviewers didn't provide a date with their rating. Find the names of all reviewers who have ratings with a NULL value for the date.
```SQL
SELECT name
FROM Reviewer LEFT JOIN Rating USING(rID)
WHERE ratingDate IS NULL;
```

- Q5 : Write a query to return the ratings data in a more readable format: reviewer name, movie title, stars, and ratingDate. Also, sort the data, first by reviewer name, then by movie title, and lastly by number of stars.
```SQL
SELECT name, title, stars, ratingDate
FROM Movie JOIN Rating USING(mID) JOIN Reviewer USING(rID)
ORDER BY name, title, stars;
```

- Q6 : For all cases where the same reviewer rated the same movie twice and gave it a higher rating the second time, return the reviewer's name and the title of the movie.
```SQL
SELECT name, title
FROM Movie M, Rating V, Reviewer R, Rating V2
WHERE R.rID = V.rID
      AND V.mID = M.mID
      AND R.rId = V2.rID
      AND V.stars < V2.stars
      AND V.ratingDate < V2.ratingDate
      AND V2.mID = M.mID
```
```SQL
SELECT name, title
FROM Movie JOIN Rating R1 USING(mId)
           JOIN Rating R2 USING(rId, mId)
           JOIN Reviewer USING(rId)
WHERE R1.ratingDate < R2.ratingDate AND R1.stars < R2.stars;
```

- Q7 : For each movie that has at least one rating, find the highest number of stars that movie received. Return the movie title and number of stars. Sort by movie title.
```SQL
SELECT title, MAX(stars)
FROM Movie JOIN Rating USING(mID)
GROUP BY mID
ORDER BY title
```

- Q8 : For each movie, return the title and the 'rating spread', that is, the difference between highest and lowest ratings given to that movie. Sort by rating spread from highest to lowest, then by movie title.
```SQL
SELECT title, MAX(stars) - MIN(Stars)
FROM Movie JOIN Rating USING(mID)
GROUP BY mID
ORDER BY MAX(stars) - MIN(Stars) DESC, title
```
```SQL
SELECT title, (MAX(stars) - MIN(Stars)) as spread
FROM Movie JOIN Rating USING(mID)
GROUP BY mID
ORDER BY spread DESC, title
```
```SQL
SELECT title, MAX(stars) - MIN(Stars) as spread
FROM Movie JOIN Rating USING(mID)
GROUP BY mID
ORDER BY spread DESC, title
```

- Q9 : Find the difference between the average rating of movies released before 1980 and the average rating of movies released after 1980. (Make sure to calculate the average rating for each movie, then the average of those averages for movies before 1980 and movies after. Don't just calculate the overall average rating before and after 1980.)
```SQL
SELECT AVG(vals.avg) - AVG(vals2.avg)
FROM (
  SELECT AVG(stars) AS avg
  FROM Movie
  JOIN Rating USING(mId)
  WHERE year < 1980
  GROUP BY mId
) AS vals, (
  SELECT AVG(stars) AS avg
  FROM Movie
  JOIN Rating USING(mId)
  WHERE year > 1980
  GROUP BY mId
) AS vals2;
```
