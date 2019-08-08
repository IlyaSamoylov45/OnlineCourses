SQL Subqueries in WHERE clause
  ```SQL
  SELECT sID, sName
  FROM Student
  WHERE sID IN (SELECT sID
                FROM Apply
                WHERE major = 'CS');
  ```
  ```SQL
  SELECT GPA
  FROM Student
  WHERE sID IN (SELECT sID
                FROM Apply
                WHERE major = 'CS');
  ```
  - Duplicates matter
  ```SQL
  SELECT sID, sName
  FROM Student
  WHERE sID IN (SELECT sID
                FROM Apply
                WHERE major = 'CS')
        AND sID NOT IN(SELECT sID
                       FROM Apply
                       WHERE major = 'EE');
  ```
  ```SQL
  SELECT cName, state
  FROM College C1
  WHERE EXISTS (SELECT *
                FROM College C2
                WHERE C2.state = C1.state
                      AND C1.cName <> C2.cName);
  ```
  - Example: College with highest enrollment
  ```SQL
  SELECT cName
  FROM College C1
  WHERE NOT EXISTS (SELECT *
                FROM College C2
                WHERE C2.enrollment > C1.enrollment);
  ```
  - Example: Student with highest GPA
  ```SQL
  SELECT sName, GPA
  FROM Student
  WHERE GPA >= ALL(SELECT GPA
                   FROM Student);
  ```
  - Example: College with highest enrollment
  ```SQL
  SELECT cName
  FROM College C1
  WHERE NOT enrollment <= ANY(SELECT enrollment
                              FROM College C2
                              WHERE S2.cName <> S1.cName);
  ```
  - Example: Students not from the smallest high school
  ```SQL
  SELECT sID, sName, sizeHS
  FROM Student
  WHERE sizeHS >= ANY(SELECT sizeHS
                      FROM Student);
  ```
  - Some databases don't allow ANY 
