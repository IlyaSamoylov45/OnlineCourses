SQL Subqueries in FROM and SELECT
  - Example: Students whose GPA changes GPA by more than one
  ```SQL
  SELECT *
  FROM (SELECT sID, sName, GPA, GPA * (sizeHS/1000.0) AS scaledGPA
        FROM Student) AS G
  WHERE abs(G.scaledGPA - G.GPA) > 1.0;
  ```
