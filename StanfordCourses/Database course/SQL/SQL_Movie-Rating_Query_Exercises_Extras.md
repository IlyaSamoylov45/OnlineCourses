- You've started a new movie-rating website, and you've been collecting data on reviewers' ratings of various movies. There's not much data yet, but you can still try out some interesting queries. Here's the schema:

```
Movie ( mID, title, year, director )
English: There is a movie with ID number mID, a title, a release year, and a director.

Reviewer ( rID, name )
English: The reviewer with ID number rID has a certain name.

Rating ( rID, mID, stars, ratingDate )
English: The reviewer rID gave the movie mID a number of stars rating (1-5) on a certain ratingDate.
```

- Q1 : Find the names of all reviewers who rated Gone with the Wind. (This wont work if two different people have the same name)
```SQL
SELECT DISTINCT name
FROM Reviewer R, Rating V, Movie M
WHERE R.rID = V.rID AND M.mID = V.mid AND M.title = 'Gone with the Wind';
```
```SQL
SELECT DISTINCT name
FROM Reviewer JOIN Rating USING(rID) JOIN Movie USING(mid)
WHERE Movie.title = 'Gone with the Wind';
```

- Q2 : For any rating where the reviewer is the same as the director of the movie, return the reviewer name, movie title, and number of stars.
```SQL
SELECT name, title, stars
FROM Movie M, Reviewer R, Rating V
WHERE M.mID = V.mID AND V.rID = R.rID AND M.director = R.name
```
```SQL
SELECT name, title, stars
FROM (Movie JOIN Rating USING(mID)) JOIN Reviewer USING(rID)
WHERE director = name;
```

- Q3 : Return all reviewer names and movie names together in a single list, alphabetized. (Sorting by the first name of the reviewer and first word in the title is fine; no need for special processing on last names or removing "The".)
```SQL
SELECT title
FROM Movie
UNION
SELECT name
FROM Reviewer
```
- Above works but shouldn't.
```SQL
SELECT *
FROM (SELECT title
      FROM Movie
      UNION
      SELECT name
      FROM Reviewer
      )
```
```SQL
SELECT title FROM Movie
UNION
SELECT name FROM Reviewer
ORDER BY name, title;
```

- Q4 : Find the titles of all movies not reviewed by Chris Jackson.
```SQL
SELECT title
FROM Movie
WHERE mId NOT IN (
  SELECT mId
  FROM Rating
  JOIN Reviewer USING(rId)
  WHERE name = "Chris Jackson"
);
```
```SQL
SELECT title
FROM Movie
EXCEPT
SELECT DISTINCT title
FROM Rating JOIN Reviewer USING(rId) JOIN Movie USING(mID)
WHERE name = "Chris Jackson"
```

- Q5 : For all pairs of reviewers such that both reviewers gave a rating to the same movie, return the names of both reviewers. Eliminate duplicates, don't pair reviewers with themselves, and include each pair only once. For each pair, return the names in the pair in alphabetical order.
```SQL
SELECT DISTINCT v1.name, v2.name
FROM Rating R1, Rating R2, Reviewer v1, Reviewer v2
WHERE R1.mID = R2.mID
AND R1.rID = v1.rID
AND R2.rID = v2.rID
AND v1.name < v2.name
ORDER BY v1.name;
```
```SQL
SELECT DISTINCT v1.name, v2.name
FROM (SELECT * FROM Rating JOIN Reviewer USING(rID)) as v1,
     (SELECT * FROM Rating JOIN Reviewer USING(rID)) as v2
WHERE v1.mID = v2.mID AND v1.name < v2.name
```

- Q6 : For each rating that is the lowest (fewest stars) currently in the database, return the reviewer name, movie title, and number of stars.
```SQL
SELECT name, title, stars
FROM Movie, Rating, Reviewer
WHERE Movie.mID = Rating.mID
      AND Rating.rID = Reviewer.rID
      AND stars NOT IN (SELECT R.stars
                        FROM Movie, Rating R, Rating R2, Reviewer
                        WHERE R.stars > R2.stars
                              AND Movie.mID = R.mID
                              AND Movie.mID = R2.mID
                              AND R.rID <> R2.rID
                        );
```
```SQL
SELECT name, title, stars
FROM Movie, Rating , Reviewer
WHERE stars = (SELECT MIN(stars) FROM Rating)
      AND Movie.mID = Rating.mID
      AND Rating.rID = Reviewer.rID;
```
```SQL
SELECT name, title, stars
FROM Movie JOIN Rating USING(mId) JOIN Reviewer USING(rId)
WHERE stars = (SELECT MIN(stars) FROM Rating);
```

- Q7 : List movie titles and average ratings, from highest-rated to lowest-rated. If two or more movies have the same average rating, list them in alphabetical order.
```SQL
SELECT title, avg(stars) as star
FROM Movie JOIN Rating USING(mID)
GROUP BY title
ORDER BY star DESC, title ASC
```

- Q8: Find the names of all reviewers who have contributed three or more ratings. (As an extra challenge, try writing the query without HAVING or without COUNT.)
```SQL
SELECT name
FROM Reviewer
WHERE rID IN (SELECT DISTINCT R.rID
              FROM Rating R, Rating R2, Rating R3
              WHERE R.rID = R2.rID AND (R.mID <> R2.mID OR R.ratingDate <> R2.ratingDate)
                    AND R.rID = R3.rID AND (R.mID <> R3.mID OR R.ratingDate <> R3.ratingDate)
                    AND R3.rID = R2.rID AND (R3.mID <> R2.mID OR R3.ratingDate <> R2.ratingDate)
)
```
```SQL
SELECT name
FROM Reviewer
WHERE rID IN (SELECT DISTINCT R.rID
              FROM Rating R, Rating R2, Rating R3
              WHERE R.rID = R2.rID AND R.rID = R3.rID
                    AND (R.mID <> R2.mID OR R.ratingDate <> R2.ratingDate)
                    AND (R.mID <> R3.mID OR R.ratingDate <> R3.ratingDate)
                    AND (R3.mID <> R2.mID OR R3.ratingDate <> R2.ratingDate)
)
```
- Easier Solution :
```SQL
SELECT name
FROM Reviewer, (SELECT rID, COUNT(*) as vals
                FROM Rating
                GROUP BY rID) as curr
WHERE curr.vals >= 3 AND curr.rID = Reviewer.rID;
```

- Q9 : Some directors directed more than one movie. For all such directors, return the titles of all movies directed by them, along with the director name. Sort by director name, then movie title. (As an extra challenge, try writing the query both with and without COUNT.)
```SQL
SELECT M.title, M.director
FROM Movie M, Movie M2, Movie M3
WHERE (M.director = M2.director AND M.mID <> M2.mID) AND
      (M.director = M3.director AND M2.mID <> M3.mID)
ORDER BY M.director, M.title
```
- Easier Solution :
```SQL
SELECT M.title, M.director
FROM Movie M, Movie M2
WHERE M.director = M2.director AND M.mID <> M2.mID
ORDER BY M.director, M.title
```
```SQL
SELECT M.title, M.director
FROM (SELECT director, COUNT(title) as val
      FROM Movie
      GROUP BY director) as curr, Movie M
WHERE curr.val > 1 AND curr.director = M.director
ORDER BY M.director, M.title;
```

- Q10 : Find the movie with the highest average rating. Return the movie title(s) and average rating. (Hint: This query is more difficult to write in SQLite than other systems; you might think of it as finding the highest average rating and then choosing the movie(s) with that average rating.)
```SQL
SELECT title, AVG(stars) AS vals
FROM Rating R, Movie M
WHERE R.mID = M.mID
GROUP BY R.mID
ORDER BY vals DESC
LIMIT 1;
```
```SQL
SELECT temp.title, temp.vals
FROM (SELECT title, AVG(stars) AS vals
      FROM Rating R, Movie M
      WHERE R.mID = M.mID
      GROUP BY R.mID
) as temp
ORDER BY temp.vals DESC
LIMIT 1;
```
- The above 2 queries will only find the 1 max movie to find multiple we need:
```SQL
SELECT title, AVG(stars) AS vals
FROM Rating R, Movie M
WHERE R.mID = M.mID
GROUP BY R.mID
EXCEPT
SELECT temp1.title, temp1.vals
        FROM (SELECT title, AVG(stars) AS vals
              FROM Rating R, Movie M
              WHERE R.mID = M.mID
              GROUP BY R.mID) as temp1,
              (SELECT title, AVG(stars) AS vals
               FROM Rating R, Movie M
               WHERE R.mID = M.mID
               GROUP BY R.mID) as temp2
 WHERE temp1.vals < temp2.vals;
```
```SQL
SELECT title, AVG(stars) AS average_stars
FROM Rating JOIN movie USING(mID)
GROUP BY mID
HAVING average_stars = (SELECT MAX(temp.vals)
                        FROM (SELECT mID, AVG(stars) AS vals
                              FROM Rating
                              GROUP BY mID) AS temp)
```

- Q11 : Find the movies with the lowest average rating. Return the movie title(s) and average rating. (Hint: This query may be more difficult to write in SQLite than other systems; you might think of it as finding the lowest average rating and then choosing the movie(s) with that average rating.)
```SQL
SELECT title, AVG(stars) AS vals
FROM Rating R, Movie M
WHERE R.mID = M.mID
GROUP BY R.mID
EXCEPT
SELECT temp1.title, temp1.vals
        FROM (SELECT title, AVG(stars) AS vals
              FROM Rating R, Movie M
              WHERE R.mID = M.mID
              GROUP BY R.mID) as temp1,
              (SELECT title, AVG(stars) AS vals
               FROM Rating R, Movie M
               WHERE R.mID = M.mID
               GROUP BY R.mID) as temp2
 WHERE temp1.vals > temp2.vals;
```
```SQL
SELECT title, AVG(stars) AS average_stars
FROM Rating JOIN Movie USING(mID)
GROUP BY mID
HAVING average_stars = (SELECT MIN(temp.vals)
                        FROM (SELECT mID, AVG(stars) AS vals
                              FROM Rating
                              GROUP BY mID) AS temp)
```

- Q12 : For each director, return the director's name together with the title(s) of the movie(s) they directed that received the highest rating among all of their movies, and the value of that rating. Ignore movies whose director is NULL.
```SQL
SELECT DISTINCT director, title, stars
FROM Movie JOIN Rating USING(mID), (SELECT director as dir, MAX(stars) AS vals
                                    FROM Rating JOIN Movie USING (mID)
                                    GROUP BY dir) AS temp
WHERE director = temp.dir AND stars = temp.vals;
```
