SQL Basic Select
  - Basic Select:
  ```SQL
  SELECT A1, A2, - - -, An
  FROM R1, R2 - - -, Rn
  WHERE condition;
  ```
  - Example College Admissions Database
    - College(<u>cName</u>, state, enrollment)
    - Student(<u>sID</u>, sName, GPA, sizeHS)
    - Apply (<u>sID</u>, <u>cName</u>, <u>major</u>, decision)
  - Example: Display all ids and names of students with a GPA greater than 3.6
  ```SQL
  SELECT sID, sName
  FROM Student
  WHERE GPA > 3.6;
  ```
  - Example: display the name and major for each student who applied
  ```SQL
  SELECT sName, major
  FROM Student, Apply
  WHERE Student.sID = Apply.sID;
  ```
  - In relational algebra natural join would happen automatically, but in SQL we need to always write the join condition explicitly.
  - SQL follows multiset mode (allows duplicates)
  ```SQL
  SELECT sName, GPA, decision
  FROM Student, Apply
  WHERE Student.sID = Apply.sID
        AND sizeHS < 1000
        AND major = 'CS'
        AND cname = 'Stanford';
  ```
  ```SQL
  SELECT College.cName
  FROM College, Apply
  WHERE College.cName = Apply.cName
        AND enrollment > 20000
        AND major = 'CS';
  ```
  - SQL is unordered model
  ```SQL
  SELECT sID, major
  FROM Apply
  WHERE major LIKE '%Bio%';
  ```
  ```SQL
  SELECT *
  FROM Student, College;
  ```
  ```SQL
  SELECT sID, sName
  FROM Student;
  ```
  ```SQL
  SELECT sID, sName, GPA, sizeHS, GPA * (sizeHs/1000.0) as ScaledGPA
  FROM Student
  ```
