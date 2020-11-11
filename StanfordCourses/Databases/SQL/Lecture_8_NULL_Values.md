SQL NULL values
  - NULL values : undefined or unknown
  ```SQL
  SELECT sID, sName, GPA
  FROM Student
  WHERE GPA > 3.5 OR sizeHS < 1600
  ```
  - Below has 8 tuples
  ```SQL
  SELECT DISTINCT GPA
  FROM Student
  ```
  - Below will be 7
  ```SQL
  SELECT COUNT(DISTINCT GPA)
  FROM Student
  ```
  - When we use count we don't include NULL
