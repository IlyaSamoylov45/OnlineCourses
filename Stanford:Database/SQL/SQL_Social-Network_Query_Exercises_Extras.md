- Students at your hometown high school have decided to organize their social network using databases. So far, they have collected information about sixteen students in four grades, 9-12. Here's the schema:
```
Highschooler ( ID, name, grade )
English: There is a high school student with unique ID and a given first name in a certain grade.

Friend ( ID1, ID2 )
English: The student with ID1 is friends with the student with ID2. Friendship is mutual, so if (123, 456) is in the Friend table, so is (456, 123).

Likes ( ID1, ID2 )
English: The student with ID1 likes the student with ID2. Liking someone is not necessarily mutual, so if (123, 456) is in the Likes table, there is no guarantee that (456, 123) is also present.
```

- Q1 : For every situation where student A likes student B, but student B likes a different student C, return the names and grades of A, B, and C.
```SQL
SELECT S1.name, S1.grade, S2.name, S2.grade, S3.name, S3.grade
FROM Highschooler S1
     JOIN Likes L1 ON S1.ID = L1.ID1
     JOIN Highschooler S2 ON S2.ID = L1.ID2
     JOIN Likes L2 ON S2.ID = L2.ID1
     JOIN Highschooler S3 ON S3.ID = L2.ID2
WHERE L1.ID1 <> L2.ID2
      AND L1.ID2 = L2.ID1
```
```SQL
SELECT S1.name, S1.grade, S2.name, S2.grade, S3.name, S3.grade
FROM Highschooler S1, Likes L1, Highschooler S2, Likes L2, Highschooler S3
WHERE L1.ID1 <> L2.ID2 AND L1.ID2 = L2.ID1
      AND S1.ID = L1.ID1
      AND S2.ID = L1.ID2
      AND S2.ID = L2.ID1
      AND S3.ID = L2.ID2
```

- Q2 : Find those students for whom all of their friends are in different grades from themselves. Return the students' names and grades.
```SQL
SELECT name, grade
FROM Highschooler S1
WHERE S1.ID NOT IN ( SELECT S1.ID
                     FROM Highschooler S2, Friend F
                     WHERE S1.ID = F.ID1 AND S2.ID = F.ID2  
                           AND S1.grade = S2.grade
                   )
```

- Q3 : What is the average number of friends per student? (Your result should be just one number.)
```SQL
SELECT AVG(Temp.total_friends)
FROM (SELECT count(ID2) as total_friends
      FROM Friend F
      GROUP BY ID1
     ) AS Temp
```

- Q4 : Find the number of students who are either friends with Cassandra or are friends of friends of Cassandra. Do not count Cassandra, even though technically she is a friend of a friend.
```SQL
SELECT COUNT (*)
FROM (
    SELECT S1.name
    FROM Friend F, Highschooler S1, Highschooler S2
    WHERE S1.ID = F.ID1
          AND S2.ID = F.ID2
          AND S2.name = 'Cassandra'
    UNION
    SELECT S3.name
    FROM Friend F1, Friend F2, Highschooler S1, Highschooler S2, Highschooler S3
    WHERE S1.name = 'Cassandra'
          AND S1.ID = F1.ID1
          AND S2.ID = F1.ID2
          AND S2.ID = F2.ID1
          AND S3.ID = F2.ID2
)
```

- Q5 : Find the name and grade of the student(s) with the greatest number of friends.
```SQL
SELECT S1.name, S1.grade
FROM Highschooler S1, Friend F2, (SELECT MAX(total_friends) AS max_friends
                                  FROM ( SELECT COUNT(*) AS total_friends
                                         FROM Friend F
                                         GROUP BY F.ID1
                                       )
                                  )AS max_friends_total
WHERE S1.ID = F2.ID1
GROUP BY F2.ID1
HAVING COUNT(*) = max_friends_total.max_friends
```
