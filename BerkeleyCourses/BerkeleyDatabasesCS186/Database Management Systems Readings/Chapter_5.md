Chapter 5 SQL: QUERIES, CONSTRNNTS, TRIGGERS
  - Structured Query Language (SQL) is the most widely used commercial relational database language.

Chapter 5.1 OVERVIEW
  - The SQL language has several aspects to it:
    - The Data Manipulation Language (DML): This subset of SQL allows users to pose queries and to insert, delete, and modify rows.
    - The Data Definition Language (DDL): This subset of SQL supports the creation, deletion, and modification of definitions for tables and views.
    - Triggers and Advanced Integrity Constraints: The new SQL:1999 standard includes support for triggers, which are actions executed by the DBMS whenever changes to the database meet conditions specified in the trigger.
    - Embedded and Dynamic SQL: Embedded SQL features allow SQL code to be called from a host language such as C or COBOL. Dynamic SQL features allow a query to be constructed (and executed) at run-time.
    - Client-Server Execution and Remote Database Access: These commands control how a client application program can connect to an SQL database server, or access data from a database over a network.
    - Transaction Management: Various commands allow a user to explicitly control aspects of how a transaction is to be executed.
    - Security: SQL provides mechanisms to control users' access to data objects such as tables and views.
    - Advanced features: The SQL:1999 standard includes object-oriented features, recursive queries, decision support queries, and also addresses emerging areas such as data mining, spatial data, and text and XML data management.

Chapter 5.1.1 Chapter Organization
  - An active database has a collection of triggers, which are specified by the DBA.
  - A trigger describes actions to be taken when certain situations arise. The DBMS monitors the database, detects these situations, and invokes the trigger.

Chapter 5.2 THE FORM OF A BASIC SQL QUERY
  - The basic form of an SQL query is as follows:
  ```SQL
  SELECT [DISTINCT] select-list
  FROM from-list
  WHERE qualification
  ```
  - Every query must have a SELECT clause, which specifies columns to be retained in the result, and a FROM clause, which specifies a cross-product of tables.
  - The optional WHERE clause specifies selection conditions on the tables mentioned in the FROM clause.
  - The close relationship between SQL and relational algebra is the basis for query optimization in a relational DBMS.
    - Ex: Find the names and ages of all sailors
    ```SQL
    SELECT DISTINCT S.sname, S.age
    FROM Sailors S
    ```
  - This query is equivalent to applying the projection operator of relational algebra.
  - If we omit the keyword DISTINCT, we would get a copy of the row (s,a) for each sailor with name s and age a; the answer would be a multiset of rows.
  - A multiset is similar to a set in that it is an unordered collection of elements
  - Two multisets could have the same elements and yet be different because the number of copies is different for some elements. For example, {a, b, b} and {b, a, b} denote the same multiset
  - Equivalent to an application of the selection operator of relational algebra.
    - Ex: Find all sailors with a rating above 7. This query uses the optional keyword AS to introduce a range variable.
    ```SQL
    SELECT S.sid, S.sname, S.rating, S.age
    FROM Sailors AS S
    WHERE S.rating > 7
    ```
  - SELECT * . This notation is useful for interactive querying, but it is poor style for queries that are intended to be reused and maintained because the schema of the result is not clear from the query itself
  - The SELECT clause is actually used to do projection, whereas selections in the relational algebra sense are expressed using the WHERE clause
  - The answer to a query is itself a relation which is a multiset of rows in SQL whose contents can be understood by considering the following conceptual evaluation strategy:
    1. Compute the cross-product of the tables in the from-list.
    2. Delete rows in the cross-product that fail the qualification conditions. (WHERE)
    3. Delete all columns that do not appear in the select-list.
    4. If DISTINCT is specified, eliminate duplicate rows.
    - Ex: Find the names of sailors who have reserved boat number 103.
    ```SQL
    SELECT S.sname
    FROM Sailors S, Reserves R
    WHERE S.sid = R.sid AND R.bid = 103
    ```
    - The first step is to construct the cross-product S x R
    - The second step is to apply the qualification S.sid = R.sid AND R.bid = 103.
    - The third step is to eliminate unwanted columns; only sname appears in the SELECT clause.

Chapter 5.2.1 Examples ofBasic SQL Queries
  - Ex: Find the names of sailors who have reserved boat number 103. (Same as before)
  ```SQL
  SELECT sname
  FROM Sailors S, Reserves R
  WHERE S.sid = R.sid AND bid = 103
  ```
  - Ex: (Same as before)
  ```SQL
  SELECT sname
  FROM Sailors, Reserves
  WHERE Sailors.sid = Reserves.sid AND bid = 103
  ```
  - Range variables need to be introduced explicitly only when the FROM clause contains more than one occurrence of a relation
  - Ex: Find the sids of sailors who have reserved a red boat
  ```SQL
  SELECT R.sid
  FROM Boats B, Reserves R
  WHERE B.bid = R.bid AND B.color = 'red'
  ```
  - Ex: Find the names of sailors who have reserved a red boat.
  ```SQL
  SELECT S.sname
  FROM Sailors S, Reserves R, Boats B
  WHERE S.sid = R.sid AND R.bid = B.bid AND B.color = 'red'
  ```
  - Ex: Find the colors of boats reserved by Lubber
  ```SQL
  SELECT B.color
  FROM Sailors S, Reserves R, Boats B
  WHERE S.sid = R.sid AND R.bid = B.bid AND S.sname = 'Lubber'
  ```
  - Ex: Find the names of sailors who have reserved at least one boat.
  ```SQL
  SELECT S.sname
  FROM Sailors S, Reserves R
  WHERE S.sid = R.sid
  ```

Chapter 5.2.2 Expressions and Strings in the SELECT Command
  - Each item in a select-list can be of the form expression AS column name, where expression is any arithmetic or string expression over column name (possibly prefixed by range variables) and constants, and column name is a new name for this column in the output of the query.
  - Compute increments for the rating of persons who have sailed two different boats on the same day.
  ```SQL
  SELECT S.sname, S.rating+1 AS rating
  FROM Sailors S, Reserves R1, Reserves R2
  WHERE S.sid = R1.sid AND S.sid = R2.sid
        AND R1.day = R2.day AND R1.bid <> R2.bid
  ```
  - SQL provides support for pattern matching through the LIKE operator, along with the use of the wild-card symbols % (which stands for zero or more arbitrary characters) and _ (which stands for exactly one, arbitrary, character). Thus, ```'_AB%'``` denotes a pattern matching every string that contains at least three characters, with the second and third characters being A and B respectively.
  - Ex: Find the ages of sailors whose name begins and ends with B and has at least three characters.
  ```SQL
  SELECT S.age
  FROM Sailors S
  WHERE S.sname LIKE 'B_%B'
  ```

Chapter 5.3 UNION, INTERSECT, AND EXCEPT
  - IN and EXISTS can be prefixed by NOT, with the obvious modification to their meaning.
  - Ex: Find the names of sailors who have reserved a red or a green boat.
  ```SQL
  SELECT S.sname
  FROM Sailors S, Reserves R, Boats B
  WHERE S.sid = R.sid AND R.bid = B.bid
        AND (B.color = 'red' OR B.color = 'green')
  ```
  - Ex: Find the names of sailor's who have reserved both a red and a green boat.
  ```SQL
  SELECT S.sname
  FROM Sailors S, Reserves R1, Boats B1, Reserves R2, Boats B2
  WHERE S.sid = R1.sid AND R1.bid = B1.bid
        AND S.sid = R2.sid AND R2.bid = B2.bid
        AND B1.color='red' AND B2.color = 'green'
  ```
  - A better solution for these two queries is to use UNION and INTERSECT.
  - Ex: Find the names of sailors who have reserved a red or a green boat. This query says that we want the union of the set of sailors who have reserved red boats and the set of sailors who have reserved green boats.
  ```SQL
  SELECT S.sname
  FROM Sailors S, Reserves R, Boats B
  WHERE S.sid = R.sid AND R.bid = B.bid AND B.color = 'red'
  UNION
  SELECT S2.sname
  FROM Sailors S2, Boats B2, Reserves R2
  WHERE S2.sid = R2.sid AND R2.bid = B2.bid AND B2.color = 'green'
  ```
  - Ex: Find the names of sailor's who have reserved both a red and a green boat. This query actually contains a subtle bug-if there are two sailors such as Horatio.
  ```SQL
  SELECT S.sname
  FROM Sailors S, Reserves R, Boats B
  WHERE S.sid = R.sid AND R.bid = B.bid AND B.color = 'red'
  INTERSECT
  SELECT S2.sname
  FROM Sailors S2, Boats B2, Reserves R2
  WHERE S2.sid = R2.sid AND R2.bid = B2.bid AND B2.color = 'green'
  ```
  - Ex: Find the sids of all sailor's who have reserved red boats but not green boats.
  ```SQL
  SELECT S.sid
  FROM Sailors S, Reserves R, Boats B
  WHERE S.sid = R.sid AND R.bid = B.bid AND B.color = 'red'
  EXCEPT
  SELECT S2.sid
  FROM Sailors S2, Reserves R2, Boats B2
  WHERE S2.sid = R2.sid AND R2.bid = B2.bid AND B2.color = 'green'
  ```
  - Ex: Find the sids of all sailor's who have reserved red boats but not green boats. ( since the Reserves relation contains sid information, there is no need to look at the Sailors relation, and we can use the following simpler query)
  ```SQL
  SELECT R.sid
  FROM Boats B, Reserves R
  WHERE R.bid = B.bid AND B.color = 'red'
  EXCEPT
  SELECT R2.sid
  FROM Boats B2, Reserves R2
  WHERE R2.bid = B2.bid AND B2.color = 'green'
  ```
  - UNION, INTERSECT, and EXCEPT can be used on any two tables that are union-compatible, that is, have the same number of columns and the columns, taken in order, have the same types.
  - Ex: Find all sids of sailors who have a rating of 10 or reserved boat 104.
  ```SQL
  SELECT S.sid
  FROM Sailors S
  WHERE S.rating = 10
  UNION
  SELECT R.sid
  FROM Reserves R
  WHERE R.bid = 104
  ```
  - In contrast to the default that duplicates are not eliminated unless DISTINCT is specified in the basic query form, the default for UNION queries is that duplicates are eliminated!
  - To retain duplicates, UNION ALL must be used; if so, the number of copies of a row in the result is always m + n, where m and n are the numbers of times that the row appears in the two parts of the union.
  - INTERSECT ALL retains duplicates the number of copies of a row in the result is min(m, n)
  - EXCEPT ALL also retains duplicates the number of copies of a row in the result is m - n, where m corresponds to the first relation.

Chapter 5.4 NESTED QUERIES
  - A nested query is a query that has another query embedded within it; the embedded query is called a subquery.
  - A subquery typically appears within the WHERE clause of a query.
  - Subqueries can sometimes appear in the FROM clause or the HAVING clause
  - Nesting of queries is a feature that is not available in relational algebra
  - Nesting in SQL is inspired more by relational calculus than algebra

Chapter 5.4.1 Introduction to Nested Queries
  - Ex: Find the names of sailors who have reserved boat 103. (Note that it is very easy to modify this query to find all sailors who have not reserved boat 103 we can just replace IN by NOT IN)
  ```SQL
  SELECT S.sname
  FROM Sailors S
  WHERE S.sid IN ( SELECT R.sid
                   FROM Reserves R
                   WHERE R.bid = 103 )
  ```
  - Ex: Find the names of sailors who have reserved a red boat.
  ```SQL
  SELECT S.sname
  FROM Sailors S
  WHERE S.sid IN ( SELECT R.sid
                   FROM Reserves R
                   WHERE R.bid IN (SELECT B.bid
                                   FROM Boats B
                                   WHERE B.color = 'red'
                                  )
                 )
  ```
  - Ex: Find the names of sailors who have not reserved a red boat.
  ```SQL
  SELECT S.sname
  FROM Sailors S
  WHERE S.sid NOT IN ( SELECT R.sid
                       FROM Reserves R
                       WHERE R.bid IN ( SELECT B.bid
                                        FROM Boats B
                                        WHERE B.color = 'red' )
                     )
  ```

Chapter 5.4.2 Correlated Nested Queries
  - In general, the inner subquery could depend on the row currently being examined in the outer query
  - Ex: Find the names of sailors who have reserved boat number 103.
  ```SQL
  SELECT S.sname
  FROM Sailors S
  WHERE EXISTS ( SELECT *
                 FROM Reserves R
                 WHERE R.bid = 103
                       AND R.sid = S.sid )
  ```
  - Closely related to EXISTS is the UNIQUE predicate.
  - When we apply UNIQUE to a subquery, the resulting condition returns true if no row appears twice in the answer to the subquery

Chapter 5.4.3 Set-Comparison Operators
  - SQL also supports op ANY and op ALL, where op is one of the arithmetic comparison operators {<, <=,=, <>, >=, >}. (SOME is also available, but it is just a synonym for ANY.)
  - Ex: Find sailors whose rating is better than some sailor called Horatio. (If there are several sailors called Horatio, this query finds all sailors whose rating is better than that of some sailor called Horatio.)
  ```SQL
  SELECT S.sid
  FROM Sailors S
  WHERE S.rating > ANY ( SELECT S2.rating
                         FROM Sailors S2
                         WHERE S2.sname = 'Horatio'
                       )
  ```
  - Ex: Find sailors whose rating is better than every sailor called Horatio (We can obtain all such queries with a simple modification to the previous example replace ANY with ALL in the WHERE clause of the outer query. If there were no sailor called Horatio, the comparison S.rating > ALL is defined to return true)
  ```SQL
  SELECT S.sid
  FROM Sailors S
  WHERE S.rating > ALL ( SELECT S2.rating
                         FROM Sailors S2
                         WHERE S2.sname = 'Horatio'
                       )
  ```
  - Ex: Find the Sailors with the highest rating.
  ```SQL
  SELECT S.sid
  FROM Sailors S
  WHERE S.rating >= ALL ( SELECT S2.rating
                          FROM Sailors S2 )
  ```
  - IN and NOT IN are equivalent to = ANY and <> ALL

Chapter 5.4.4 More Examples of Nested Queries
  - Ex: Find the names of sailors who have reserved both a red and a green boat
  ```SQL
  SELECT S.sname
  FROM Sailors S, Reserves R, Boats B
  WHERE S.sid = R.sid AND R.bid = B.bid AND B.color = 'red'
        AND S.sid IN ( SELECT S2.sid
                       FROM Sailors S2, Boats B2, Reserves R2
                       WHERE S2.sid = R2.sid AND R2.bid = B2.bid
                       AND B2.color = 'green' )
  ```
  - INTERSECT can be rewritten using IN, which is useful to know if your system does not support INTERSECT.
  - Queries using EXCEPT can be similarly rewritten by using NOT IN.
  - Ex: Find the names of sailors who have reserved all boats. (how the division operation in relational algebra can be expressed in SQL.)
  ```SQL
  SELECT S.sname
  FROM Sailors S
  WHERE NOT EXISTS (( SELECT B.bid
                      FROM Boats B )
                      EXCEPT
                      (SELECT R.bid
                      FROM Reserves R
                      WHERE R.sid = S.sid )
                   )
  ```
  - Ex: Find the names of sailors who have reserved all boats. (An alternative way to do this query without using EXCEPT follows)
  ```SQL
  SELECT S.sname
  FROM Sailors S
  WHERE NOT EXISTS ( SELECT B.bid
                     FROM Boats B
                     WHERE NOT EXISTS ( SELECT R.bid
                                        FROM Reserves R
                                        WHERE R.bid = B.bid
                                              AND R.sid = S.sid )
                   )             
  ```

Chapter 5.5 AGGREGATE OPERATORS
  - SQL supports five aggregate operations
    1. COUNT ([DISTINCT] A): The number of (unique) values in the A column.
    2. SUM ([DISTINCT] A): The sum of all (unique) values in the A column.
    3. AVG ([DISTINCT] A): The average of all (unique) values in the A column.
    4. MAX (A): The maximum value in the A column.
    5. MIN (A): The minimum value in the A column.
  - It does not make sense to specify DISTINCT in conjunction with MIN or MAX
  - Ex: Find the average age of all sailors.
  ```SQL
  SELECT AVG (S.age)
  FROM Sailors S
  ```
  - Ex: Find the average age of sailors with a rating of 10.
  ```SQL
  SELECT AVG (S.age)
  FROM Sailors S
  WHERE S.rating = 10
  ```
  - Ex: Find the name and age of the oldest sailor
  ```SQL
  SELECT S.sname, S.age  
  FROM Sailors S
  WHERE S.age = ( SELECT MAX (S2.age)
                  FROM Sailors S2 )
  ```
  - In SQL-if the SELECT clause uses an aggregate operation, then it must use only aggregate operations unless the query contains a GROUP BY clause
  - Ex: Count the number of sailors.
  ```SQL
  SELECT COUNT (*)
  FROM Sailors S
  ```
  - Ex: Count the number of different sailor names.
  ```SQL
  SELECT COUNT ( DISTINCT S.sname )
  FROM Sailors S
  ```
  - Ex: Find the names of sailors who are older than the oldest sailor with a rating of 10.
  ```SQL
  SELECT S.sname
  FROM Sailors S
  WHERE S.age > ( SELECT MAX ( S2.age )
                  FROM Sailors S2
                  WHERE S2.rating = 10 )
  ```

Chapter 5.5.1 The GROUP BY and HAVING Clauses
  - Often we want to apply aggregate operations to each of a number of groups of rows in a relation, where the number of groups depends on the relation instance.
  - Ex: Find the age of the youngest sailor for each rating level.
  ```SQL
  SELECT S.rating, MIN (S.age)
  FROM Sailors S
  GROUP BY S.rating
  ```
  - General Form:
  ```SQL
  SELECT [ DISTINCT] select-list
  FROM from-list
  WHERE 'qualification'
  GROUP BY grouping-list
  HAVING group-qualification
  ```
  - The expressions appearing in the group-qualification in the HAVING clause must have a single value per group
  - If GROUP BY is omitted, the entire table is regarded as a single group.
  - Ex: Find the age of the youngest sailor who is eligible to vote (i.e., is at least 18 years old) for each rating level with at least two such sailors.
  ```SQL
  SELECT S.rating, MIN (S.age) AS minage
  FROM Sailors S
  WHERE S.age >= 18
  GROUP BY S.rating
  HAVING COUNT (*) > 1
  ```

Chapter 5.5.2 More Examples of Aggregate Queries
  - Ex: For each red boat; find the number of reservations for this boat.
  ```SQL
  SELECT B.bid, COUNT (*) AS reservationcount
  FROM Boats B, Reserves R
  WHERE R.bid = B.bid AND B.color = 'red'
  GROUP BY B.bid
  ```
  - Only columns that appear in the GROUP BY clause can appear in the HAVING clause, unless they appear as arguments to an aggregate operator in the HAVING clause.
  - Ex: Find the average age of sailors for each rating level that has at least two sailors.
  ```SQL
  SELECT S.rating, AVG (S.age) AS avgage
  FROM Sailors S
  GROUP BY S.rating
  HAVING COUNT (*) > 1
  ```
  - Ex: Find the average age of sailors who are of voting age (i.e. at least 18 years old) for each rating level that has at least two sailors.
  ```SQL
  SELECT S.rating, AVG ( S.age ) AS avgage
  FROM Sailors S
  WHERE S.age >= 18
  GROUP BY S.rating
  HAVING 1 < ( SELECT COUNT (*)  
               FROM Sailors S2
               WHERE S.rating = S2.rating
             )
  ```
  - Ex: Find the average age of sailors who are of voting age (i.e. at least 18 years old) for each rating level that has at least two such sailors
  ```SQL
  SELECT S.rating, AVG ( S.age ) AS avgage
  FROM Sailors S
  WHERE S.age> 18
  GROUP BY S.rating
  HAVING 1 < ( SELECT COUNT (*)
        FROM Sailors S2
        WHERE S.rating = S2.rating
              AND S2.age >= 18
      )
  ```
  - Any query with a HAVING clause can be rewritten without one, but many queries are simpler to express with the HAVING clause.
  - Ex: Find those ratings for which the average age of sailors is the minimum over all ratings.
  ```SQL
  SELECT Temp.rating, Temp.avgage
  FROM ( SELECT S.rating, AVG (S.age) AS avgage,
         FROM Sailors S
         GROUP BY S.rating) AS Temp
  WHERE Temp.avgage = ( SELECT MIN (Temp.avgage) FROM Temp)
  ```

Chapter 5.6 NULL VALUES
  - We use null when the column value is either unknown or inapplicable.

Chapter 5.6.1 Comparisons Using Null Values
  - If we compare two null values using <, >, =, and so on, the result is always unknown.
  - SQL also provides a special comparison operator IS NULL to test whether a column value is null

Chapter 5.6.2 Logical Connectives AND, OR, and NOT
  - The expression NOT unknown is defined to be unknown.
  - OR of two arguments evaluates to true if either argument evaluates to true, and to unknown if one argument evaluates to false and the other evaluates to unknown.
  - AND of two arguments evaluates to false if either argument evaluates to false, and to unknown if one argument evaluates to unknown and the other evaluates to true or unknown.

Chapter 5.6.3 Impact on SQL Constructs
  - Eliminating rows that evaluate to unknown has a subtle but significant impact on queries, especially nested queries involving EXISTS or UNIQUE.
  - The SQL definition is that two rows are duplicates if corresponding columns are either equal, or both contain null.
  - As expected, the arithmetic operations +, -, * , and / all return null if one of their arguments is null.
  - COUNT( * ) handles null values just like other values; that is, they get counted.
  - All the other aggregate operations (COUNT, SUM, AVG, MIN, MAX, and variations using DISTINCT) simply discard null values
  - SUM cannot be understood as just the addition of all values in the (multi)set of values that it is applied to; a preliminary step of discarding all null values must also be accounted for.
  - As a special case, if one of these operators-other than COUNT-is applied to only null values, the result is again null.

Chapter 5.6.4 Outer Joins
  - The NATURAL keyword specifies that the join condition is equality on all common attributes

Chapter 5.6.5 Disallowing Null Values
  - We can disallow null values by specifying NOT NULL as part of the field definition
  ```
  SQL sname CHAR(20) NOT NULL
  ```
  - The fields in a primary key are not allowed to take on null values.

Chapter 5.7 COMPLEX INTEGRITY CONSTRAINTS IN SQL

Chapter 5.7.1 Constraints over a Single Table
  - We can specify complex constraints over a single table using table constraints, which have the form CHECK conditional-expression.
  - Trigger example:
  ```SQL
  CREATE TABLE Sailors (
      sid INTEGER,
      sname CHAR(10),
      rating INTEGER,
      age REAL,
      PRIMARY KEY (sid),
      CHECK (rating >= 1 AND rating <= 10 ))
  ```
  - Trigger example: To enforce the constraint that Interlake boats cannot be reserved. When a row is inserted into Reserves or an existing row is modified, the conditional expression in the CHECK constraint is evaluated. If it evaluates to false, the command is rejected.

  ```SQL
  CREATE TABLE Reserves (
      sid INTEGER,
      bid INTEGER,
      day DATE,
      FOREIGN KEY (sid) REFERENCES Sailors
      FOREIGN KEY (bid) REFERENCES Boats
      CONSTRAINT noInterlakeRes
      CHECK ( 'Interlake' <>
          ( SELECT B.bname
            FROM Boats B
            WHERE B.bid = Reserves.bid
          )
        )
      )
  ```

Chapter 5.7.2 Domain Constraints and Distinct Types
  - A user can define a new domain using the CREATE DOMAIN statement, which uses CHECK constraints.
  ```SQL
  CREATE DOMAIN ratingval INTEGER DEFAULT 1 CHECK ( VALUE >= 1 AND VALUE <= 10 )
  ```
  - INTEGER is the underlying, or source, type for the domain ratingval, and every ratingval value must be of this type. Values in ratingval are further restricted by using a CHECK constraint; in defining this constraint, we use the keyword VALUE to refer to a value in the domain.
  - Once a domain is defined, the name of the domain can be used to restrict column values in a table; we can use the following line in a schema declaration, for example: ```rating ratingval```
  - The optional DEFAULT keyword is used to associate a default value with a domain. If the domain ratingval is used for a column in some relation and no value is entered for this column in an inserted tuple, the default value 1 associated with ratingval is used.
  - ```CREATE TYPE ratingtype AS INTEGER``` This statement defines a new distinct type called ratingtype, with INTEGER as its source type.

Chapter 5.7.3 Assertions: ICs over Several Tables
  - Table constraints are associated with a single table, although the conditional expression in the CHECK clause can refer to other tables. Table constraints are required to hold only if the associated table is nonempty.
  - SQL supports the creation of assertions, which are constraints not associated with anyone table.
  - An assertion:
  ```SQL
  CREATE ASSERTION smallClub
  CHECK (( SELECT COUNT (S.sid) FROM Sailors S )
        + ( SELECT COUNT (B.bid) FROM Boats B)
        < 100 )
  ```

Chapter 5.8 TRIGGERS AND ACTIVE DATABASES
  - A trigger is a procedure that is automatically invoked by the DBMS in response to specified changes to the database, and is typically specified by the DBA.
  - A database that has a set of associated triggers is called an active database. A trigger description contains three parts:
    - Event: A change to the database that activates the trigger.
    - Condition: A query or test that is run when the trigger is activated.
    - Action: A procedure that is executed when the trigger is activated and its condition is true.
  - A trigger can be thought of as a 'daemon' that monitors a databa.se, and is executed when the database is modified in a way that matches the event specification.
  - An insert, delete, or update statement could activate a trigger, regardless of which user or application invoked the activating statement; users may not even be aware that a trigger was executed as a side effect of their program.
  - A condition in a trigger can be a true/false statement or a query.
  - A query is interpreted as true if the answer set is nonempty and false if the query has no answers. If the condition part evaluates to true, the action associated with the trigger is executed.
  - A trigger action can examine the answers to the query in the condition part of the trigger, refer to old and new values of tuples modified by the statement activating the trigger, execute Hew queries, and make changes to the database.
  - A trigger that initializes a variable used to count the number of qualifying insertions should be executed before, and a trigger that executes once per qualifying inserted record and increments the variable should be executed after each record is inserted.

Chapter 5.8.1 Examples of Triggers in SQL
  - A trigger can also be scheduled to execute instead of the activating statement; or in deferred fashion, at the end of the transaction containing the activating statement; or in asynchronous fashion, as part of a separate transaction.
  - A user must be able to specify whether a trigger is to be executed once per modified record or once per activating statement.
  - Trigger Example 1:
  ```SQL
  CREATE TRIGGER init_count BEFORE INSERT ON Students
    DECLARE
      count INTEGER;
    BEGIN
      count := 0;
  END
  ```
  - Trigger Example 2:
  ```SQL
  CREATE TRIGGER incr_count AFTER INSERT ON Students
    WHEN (new.age < 18)
    FOR EACH ROW
    BEGIN
      count := count + 1;
    END
  ```

Chapter 5.9 DESIGNING ACTIVE DATABASES
  - Triggers offer a powerful mechanism for dealing with changes to a database, but they must be used with caution.

Chapter 5.9.1 Why Triggers Can Be Hard to Understand
  - If a statement activates more than one trigger, the DBMS typically processes all of them, in some arbitrary order. An important point is that the execution of the action part of a trigger could in turn activate another trigger.
  - The execution of the action part of a trigger could again activate the same trigger; such triggers are called recursive triggers.
  - The potential for such chain activations and the unpredictable order in which a DBMS processes activated triggers can make it difficult to understand the effect of a collection of triggers.

Chapter 5.9.2 Constraints versus Triggers
  - A common use of triggers is to maintain database consistency, and in such cases, we should always consider whether using an integrity constraint (e.g., a foreign key constraint) achieves the same goals.
  - A constraint also prevents the data from being made inconsistent by any kind of statement, whereas a trigger is activated by a specific kind of statement (INSERT, DELETE, or UPDATE).

Chapter 5.9.3 Other Uses of Triggers
  - Many potential uses of triggers go beyond integrity maintenance. Triggers can alert users to unusual events (as reflected in updates to the database).
  - Triggers can generate a log of events to support auditing and security checks
