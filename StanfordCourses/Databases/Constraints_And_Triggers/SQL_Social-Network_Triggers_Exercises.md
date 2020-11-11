SQL Social-Network Triggers Exercises
  - Students at your hometown high school have decided to organize their social network using databases. So far, they have collected information about sixteen students in four grades, 9-12.
  - Here's the schema:  
    - ```Highschooler ( ID, name, grade ) ```
      - English: There is a high school student with unique ID and a given first name in a certain grade.  
    - ```Friend ( ID1, ID2 ) ```
      - English: The student with ID1 is friends with the student with ID2. Friendship is mutual, so if (123, 456) is in the Friend table, so is (456, 123).
    - ```Likes ( ID1, ID2 ) ```
      - English: The student with ID1 likes the student with ID2. Liking someone is not necessarily mutual, so if (123, 456) is in the Likes table, there is no guarantee that (456, 123) is also present.

  - Q1 : Write a trigger that makes new students named 'Friendly' automatically like everyone else in their grade. That is, after the trigger runs, we should have ('Friendly', A) in the Likes table for every other Highschooler A in the same grade as 'Friendly'.
  ```SQL
  CREATE TRIGGER FriendlyAdd
  AFTER INSERT ON Highschooler
  FOR EACH ROW
  WHEN new.name='Friendly'
  BEGIN
      INSERT INTO Likes SELECT new.ID, h.ID FROM highschooler as h WHERE new.grade = h.grade AND NOT (new.ID=h.ID);
  END
  ```

  - Q2 : Write one or more triggers to manage the grade attribute of new Highschoolers. If the inserted tuple has a value less than 9 or greater than 12, change the value to NULL. On the other hand, if the inserted tuple has a null value for grade, change it to 9.
  ```SQL
  CREATE TRIGGER FriendlyAdd
  AFTER INSERT ON Highschooler
  FOR EACH ROW
  WHEN new.grade < 9 OR new.grade > 12
  BEGIN
      UPDATE highschooler SET grade = NULL WHERE ID = new.ID;
  END

  |

  CREATE TRIGGER FriendlyAddTwo
  AFTER INSERT ON Highschooler
  FOR EACH ROW
  WHEN new.grade IS NULL
  BEGIN
      UPDATE highschooler SET grade = 9 WHERE ID = new.ID;
  END
  ```

  - Q3 : Write one or more triggers to maintain symmetry in friend relationships. Specifically, if (A,B) is deleted from Friend, then (B,A) should be deleted too. If (A,B) is inserted into Friend then (B,A) should be inserted too. Don't worry about updates to the Friend table.
  ```SQL
  CREATE TRIGGER AddFriend
  AFTER INSERT ON Friend
  FOR EACH ROW
  BEGIN
      INSERT INTO Friend VALUES(new.ID2, new.ID1);
  END

  |

  CREATE TRIGGER DeleteFriend
  BEFORE DELETE ON Friend
  FOR EACH ROW
  BEGIN
      DELETE FROM Friend WHERE ID1=old.ID2 AND ID2=old.ID1;
  END
  ```

  - Q4 : Write a trigger that automatically deletes students when they graduate, i.e., when their grade is updated to exceed 12.
  ```SQL
  CREATE TRIGGER removeGraduate
  AFTER UPDATE ON Highschooler
  FOR EACH ROW
  WHEN new.grade > 12
  BEGIN
      DELETE FROM Highschooler WHERE new.id = id;
  END
  ```

  - Q5 : Write a trigger that automatically deletes students when they graduate, i.e., when their grade is updated to exceed 12 (same as Question 4). In addition, write a trigger so when a student is moved ahead one grade, then so are all of his or her friends.
  ```SQL
  CREATE TRIGGER removeGraduate
  AFTER UPDATE ON Highschooler
  FOR EACH ROW
  WHEN new.grade > 12
  BEGIN
      DELETE FROM Highschooler WHERE new.ID = ID;
  END

  |

  CREATE TRIGGER gradeUp
  AFTER UPDATE ON Highschooler
  FOR EACH ROW
  WHEN old.grade < new.grade
  BEGIN
      UPDATE highschooler SET grade = grade+1 WHERE ID IN (SELECT ID2 FROM friend WHERE ID1=new.ID);
  END
  ```

  - Q6 : Write a trigger to enforce the following behavior: If A liked B but is updated to A liking C instead, and B and C were friends, make B and C no longer friends. Don't forget to delete the friendship in both directions, and make sure the trigger only runs when the "liked" (ID2) person is changed but the "liking" (ID1) person is not changed.
  ```SQL
  CREATE TRIGGER gradeUp
  AFTER UPDATE ON Likes
  FOR EACH ROW
  WHEN old.ID2 <> new.ID2 and old.ID1 = new.ID1
  BEGIN
      DELETE FROM Friend WHERE ID1 = old.ID2 AND ID2 = new.ID2;
      DELETE FROM Friend WHERE ID2 = old.ID2 AND ID1 = new.ID2;
  END
  ```
