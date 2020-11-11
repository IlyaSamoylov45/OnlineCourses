Multiple Choice

  - Q1 : Consider the following query:
  ```SQL
  Select * From Apply, College
  Where Apply.cName = College.cName
  And Apply.major = 'CS' and College.enrollment < 5000
  ```
  - Which of the following indexes could NOT be useful in speeding up query execution?
    - a : Tree-based index on Apply.cName
    - b : Hash-based index on Apply.major
    - c : Hash-based index on College.enrollment
    - d : Hash-based index on College.cName
    - Solution : Hash-based index on College.enrollment

  - Q2 : Consider the following query:
  ```SQL
  Select * From Student, Apply, College
  Where Student.sID = Apply.sID and Apply.cName = College.cName
  And Student.GPA > 1.5 And College.cName < 'Cornell'
  ```
  - Suppose we are allowed to create two indexes, and assume all indexes are tree-based. Which two indexes do you think would be most useful for speeding up query execution?
    - a : Student.sID, College.cName
    - b : Student.sID, Student.GPA
    - c : Apply.cName, College.cName
    - d : Apply.sID, Student.GPA
    - Solution : Student.sID, College.cName
