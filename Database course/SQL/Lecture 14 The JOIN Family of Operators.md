SQL The JOIN Family of Operators
  - Types:
    - INNER JOIN on condition
    - NATURAL JOIN
    - INNER JOIN using attributes
    - LEFT, RIGHT, FULL OUTER JOIN
  ```SQL
  SELECT DISTINCT sName, major
  FROM Student INNER JOIN Apply
  ON Student.sID = Apply.sID;
  ```
  - Don't need INNER clause JOIN is an abbreviation of INNER JOIN
  ```SQL
  SELECT *
  FROM Student NATURAL JOIN Apply;
  ```
  - Example: Better practice because explicit compared to natural join
  ```SQL
  SELECT sName, GPA
  FROM Student JOIN Apply USING(sID)
  WHERE sizeHS < 1000 AND major = 'CS' AND cName = 'Stanford';
  ```
  - SQL : FULL OUTER JOIN
  ```SQL
  SELECT sName, sID, cName, major
  FROM Student FULL OUTER JOIN Apply USING(sID);
  ```
  - Same as above:
  ```SQL
  SELECT sName, sID, cName, major
  FROM Student LEFT OUTER JOIN Apply USING(sID)
  UNION
  SELECT sName, sID, cName, major
  FROM Student RIGHT OUTER JOIN Apply USING(sID);
  ```
  - Same as above:
  ```SQL
  SELECT sName, Student.sID, cName, major
  FROM Student, Apply
  WHERE Student.sID = Apply.sID
  UNION
  SELECT sName, sID, NULL, NULL
  FROM Student
  WHERE sID NOT IN (SELECT sID
                    FROM Apply)
  UNION
  SELECT NULL, sID, cName, major
  FROM Apply
  WHERE sID NOT IN (SELECT sID
                    FROM Student);
  ```
  - Commutativity (A op B) = (B op A)
  - Associativity (A op B) op C = A op (B op C)
  - OUTER JOIN is not associative
  - LEFT and RIGHT OUTER JOIN are also not associative
