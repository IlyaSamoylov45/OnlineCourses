- Students at your hometown high school have decided to organize their social network using databases. So far, they have collected information about sixteen students in four grades, 9-12. Here's the schema:
```
Highschooler ( ID, name, grade )
English: There is a high school student with unique ID and a given first name in a certain grade.

Friend ( ID1, ID2 )
English: The student with ID1 is friends with the student with ID2. Friendship is mutual, so if (123, 456) is in the Friend table, so is (456, 123).

Likes ( ID1, ID2 )
English: The student with ID1 likes the student with ID2. Liking someone is not necessarily mutual, so if (123, 456) is in the Likes table, there is no guarantee that (456, 123) is also present.
```

- Q1 : Find the names of all students who are friends with someone named Gabriel.
```SQL
SELECT h1.name
FROM Highschooler AS h1 JOIN Friend ON h1.ID = ID1 JOIN Highschooler AS h2 ON h2.ID = ID2
WHERE h2.name = 'Gabriel'
```
```SQL
SELECT name
FROM HighSchooler
WHERE ID IN (SELECT ID1
             FROM Highschooler JOIN Friend ON ID = ID2
             WHERE name = 'Gabriel')
```

- Q2 : For every student who likes someone 2 or more grades younger than themselves, return that student's name and grade, and the name and grade of the student they like.
```SQL
SELECT S1.name, S1.grade, S2.name, S2.grade
FROM Highschooler S1, Highschooler S2, Likes L
WHERE S1.ID = L.ID1 AND S2.ID = L.ID2 AND S1.grade - S2.grade >= 2;
```

- Q3 : For every pair of students who both like each other, return the name and grade of both students. Include each pair only once, with the two names in alphabetical order.
```SQL
SELECT S1.name, S1.grade, S2.name, S2.grade  
FROM Likes L1, Likes L2, Highschooler S1, Highschooler S2
WHERE L1.ID1 = L2.ID2
      AND L2.ID1 = L1.ID2
      AND L1.ID1 = S1.ID
      AND L1.ID2 = S2.ID
      AND S1.name < S2.name;
```

- Q4 : Find all students who do not appear in the Likes table (as a student who likes or is liked) and return their names and grades. Sort by grade, then by name within each grade.
```SQL
SELECT name, grade
FROM Highschooler
WHERE ID NOT IN (
    SELECT ID
    FROM Likes, Highschooler
    WHERE ID = ID1 OR ID = ID2
)
```

- Q5 : For every situation where student A likes student B, but we have no information about whom B likes (that is, B does not appear as an ID1 in the Likes table), return A and B's names and grades.
```SQL
SELECT S1.name, S1.grade, S2.name, S2.grade
FROM  HighSchooler S1, HighSchooler S2, Likes
WHERE S1.ID = ID1 AND S2.ID = ID2 AND S2.ID IN
(SELECT ID
FROM Highschooler
WHERE ID NOT IN (
    SELECT ID1
    FROM LIKES
))
```

- Q6 : Find names and grades of students who only have friends in the same grade. Return the result sorted by grade, then by name within each grade.
```SQL
SELECT S1.name, S1.grade
FROM Highschooler S1
WHERE S1.ID NOT IN (SELECT S1.ID
                    FROM Highschooler S2, Friend F
                    WHERE S1.grade <> S2.grade
                          AND F.ID1 = S1.ID
                          AND F.ID2 = S2.ID
                   )
ORDER BY S1.grade, S1.name
```
```SQL
SELECT S1.name, S1.grade
FROM Highschooler S1, (SELECT S2.ID as val, MAX(S3.grade) - MIN(S3.grade) as total
                       FROM Highschooler S2, Highschooler S3, Friend F
                       WHERE F.ID1 = S2.ID AND F.ID2 = S3.ID
                       GROUP BY S2.ID
) as temp
WHERE S1.ID = temp.val AND total = 0
ORDER BY S1.grade, S1.name
```

- Q7 : For each student A who likes a student B where the two are not friends, find if they have a friend C in common (who can introduce them!). For all such trios, return the name and grade of A, B, and C.
```SQL
SELECT S1.name, S1.grade, S2.name, S2.grade, S3.name, S3.grade
FROM Highschooler S1, Highschooler S2, Highschooler S3, Likes L, Friend F1, Friend F2
WHERE S1.ID = L.ID1 AND S2.ID = L.ID2
      AND S1.ID = F1.ID1 AND S3.ID = F1.ID2
      AND S2.ID = F2.ID1 AND S3.ID = F2.ID2
      AND S1.ID NOT IN(SELECT ID2
                       FROM Friend
                       WHERE ID1 = S2.ID)

```

- Q8 : Find the difference between the number of students in the school and the number of different first names.
```SQL
SELECT total - different_names
FROM (SELECT COUNT(*) AS total
      FROM Highschooler),
     (SELECT COUNT(DISTINCT name) AS different_names
      FROM Highschooler)
```
```SQL
SELECT COUNT(*) - COUNT(DISTINCT name)
FROM Highschooler
```

- Q9 : Find the name and grade of all students who are liked by more than one other student.
```SQL
SELECT name, grade
FROM HighSchooler S1, (SELECT ID2, COUNT(ID2) AS well_liked
                        FROM LIKES
                        GROUP BY ID2
                      ) AS temp
WHERE S1.id = temp.ID2 AND temp.well_liked > 1
```
