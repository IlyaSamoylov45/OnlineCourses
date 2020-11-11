SQL Aggregation
  - Do computations over sets of values
  - min, max, sum, avg, count
  - "Aggregation" functions over values in multiple rows
  - Format
  ```SQL
  SELECT A1, A2, - - -, An
  FROM R1, R2 - - -, Rn
  WHERE condition
  GROUP BY colums
  HAVING condition
  ```
  ```SQL
  SELECT AVG(GPA)
  FROM Student;
  ```
  ```SQL
  SELECT MIN(GPA)
  FROM Student, Apply
  WHERE Student.sID = Apply.sID AND major = 'CS';
  ```
  - Example: Number of Students applying to Cornell
  ```SQL
  SELECT COUNT(*)
  FROM Apply
  WHERE cName = 'Cornell';
  ```
  - Overcounting above. Below considers overcounting
  ```SQL
  SELECT COUNT(DISTINCT sID)
  FROM Apply
  WHERE cName = 'Cornell';
  ```
  - Example: Amount by which avg GPA of students applying to CS exceeds average of students not applying to CS
  ```SQL
  SELECT CS.avgGPA - NonCS.avgGPA
  FROM (SELECT AVG(GPA) AS avgGPA
        FROM Students
        WHERE sID IN (SELECT sID
                      FROM Apply
                      WHERE major = 'CS')) AS CS,
       (SELECT AVG(GPA) AS avgGPA
        FROM Students
        WHERE sID NOT IN (SELECT sID
                          FROM Apply
                          WHERE major = 'CS')) AS NonCS;          
  ```
  - Example: Number of Applicants to each college
  ```SQL
  SELECT cName, COUNT(*)
  FROM Apply
  GROUP BY cName;
  ```
  - Example: MIN and MAX GPAs of applications to each college and major
  ```SQL
  SELECT cName, major, MIN(GPA) AS mn, MAX(GPA) AS mx
  FROM Student, Apply
  WHERE Student.sID = Apply.sID
  GROUP BY cName, major;
  ```
  - HAVING evaluated by GROUP BY
  - Example: Colleges with fewer than 5 applications
  ```SQL
  SELECT cName
  FROM Apply
  GROUP BY cName
  HAVING COUNT(*) < 5;
  ```
  - Example: Major whose applicants max GPA is below the average
  ```SQL
  SELECT Major
  FROM Student, Apply
  WHERE Student.sID = Apply.sID
  GROUP BY major
  HAVING MAX(GPA) < (SELECT AVG(GPA)
                     FROM Student);
  ```
