SQL Table Variables and Set Operators
  - Table variables
  - Set operators : Union, Intersect, Except
  - Example College Admissions Database
    - College(<u>cName</u>, state, enrollment)
    - Student(<u>sID</u>, sName, GPA, sizeHS)
    - Apply (<u>sID</u>, <u>cName</u>, <u>major</u>, decision)
  ```SQL
  SELECT S1.sID, S1.sName, S1.GPA, S2.sID, S2.sName, S2.GPA
  FROM Student S1, Student S2
  WHERE S1.GPA = S2.GPA AND S1.sID < S2.sID;
  ```
  ```SQL
  SELECT cName as Name
  FROM College
  UNION
  SELECT cName as Name
  FROM College
  ```
  - UNION by default eliminates duplicates in its result.
  - To keep duplicates use UNION ALL
  ```SQL
  SELECT sID
  FROM Apply
  WHERE major = 'CS'
  INTERSECT
  SELECT sID
  FROM Apply
  WHERE major = 'EE'
  ```
  - Some SQL databases don't have INTERSECT
  ```SQL
  SELECT sID
  FROM Apply
  WHERE major = 'CS'
  EXCEPT
  SELECT sID
  FROM Apply
  WHERE major = 'EE'
  ```
  - Some SQL databases don't have EXCEPT
