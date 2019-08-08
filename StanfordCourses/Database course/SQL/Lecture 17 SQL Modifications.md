SQL Modification
  - Inserting new data (2 methods)
  ```SQL
  INSERT INTO Table
  VALUES (A1, A2, . . ., An)
  ```
  ```SQL
  INSERT INTO Table
  SELECT statement
  ```
  - Deleting existing data
  ```SQL
  DELETE FROM Table
  WHERE condition
  ```
  - Updating existing data
  ```SQL
  UPDATE Table
  SET Attr = Expression
  WHERE condition
  ```
  ```SQL
  UPDATE Table
  SET A1 = Expr1, . . ., An = Exprn
  WHERE condition
  ```
  - Example: Have all student who didn't apply anywhere apply to CS at Carnegie Melon
  ```SQL
  INSERT INTO Apply
  SELECT sID, 'Carnegie Melon', 'CS', NULL
  FROM Student
  WHERE sID NOT IN (SELECT sID
                    FROM Apply);
  ```
  - Example: Delete students who applied to more than two majors
  ```SQL
  DELETE FROM Student
  SELECT sID, COUNT(DISTINCT major)
  FROM Apply
  GROUP BY sID
  HAVING COUNT(DISTINCT major) > 2;
  ```
  - We deleted from Student but not from Apply
