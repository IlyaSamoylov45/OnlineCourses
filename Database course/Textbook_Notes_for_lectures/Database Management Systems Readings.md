-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
INTRODUCTION AND RELATIONAL DATABASES NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 1.1 MANAGING DATA
  - The gap between how users think of their data and how the data is ultimately stored is bridged through several levels of abstraction

Chapter 1.2 A HISTORICAL PERSPECTIVE
  - Users write programs as if they are to be run by themselves, and the responsibility for running them concurrently is given to the DBMS.
  - Data warehouses: consolidating data from several databases, and for carrying out specialized analysis.

Chapter 1.3 FILE SYSTEMS VERSUS A DBMS
  -  By storing data in a DBMS rather than as files, we can use the DBMS's features to manage the data in a robust and efficient way

Chapter 1.4 ADVANTAGES OF A DBMS
  - Benefits:
    1. Data Independence
    2. Efficient Data Access
    3. Data Integrity and Security
    4. Data Administration
    5. Concurrent Access and Crash Recovery
    6. Reduced Application Development Time

  - If specialized performance or data manipulation requirements are central to an application, the application may choose not to use a DBMS

Chapter 1.5 DESCRIBING AND STORING DATA IN A DBMS
  - A data model is a collection of high-level data description constructs that hide many low-level storage details.
  - Most DBMS today are based on the relational data model
  - You should not use fields whose values are constantly changing.

Chapter 1.5.1 The Relational Model
  - Relation can be thought of as a set of records.
  - A description of data in terms of a data model is called a schema.
  - The Ability to specify uniqueness of the values in a field increases the accuracy with which we can describe our data.

Chapter 1.5.2 Levels of Abstraction in a DBMS
  - The database description consists of a schema at each of these three levels of abstraction: the conceptual, physical, and external.
  - A data definition language (DDL) is used to define the external and conceptual schemas.
  - The three levels of abstraction are:
    1. Conceptual Schema
      - Describes all relations that are stored in the database.
      - The process of arriving at a good conceptual schema is called conceptual database design.
    2. Physical Schema
      - Summarizes how the relations described in the conceptual schema are actually stored on secondary storage devices
      - The process of arriving at a good physical schema is called physical database design.
    3. External Schema
      - Allow data access to be customized at the level of individual users or groups of users.

Chapter 1.5.3 Data Independence
  - Data independence: application programs are insulated from changes in the way the data is structured and stored
  - Logical data independence:  users can be shielded from changes in the logical structure of the data, or changes in the choice of relations to be stored.
  - Physical data independence: the conceptual schema insulates users from changes in physical storage details.

Chapter 1.6 QUERIES IN A DBMS
  - The query language is a specialized language in which queries can be posed.
  - A DBMS enables users to create, modify, and query data through a data manipulation language (DML).

Chapter 3.1 INTRODUCTION TO THE RELATIONAL MODEL
  - A relation consists of a relation schema and a relation instance. The relation instance is a table
  - The relation schema describes the column heads for the table.
  - The schema specifies the relation's name, the name of each field, and the domain of each field
  - Relational Schema example:
    ex: Students(sid: string, name: string, login: string, age: integer, gpa: real)
    - The field name has a domain of string.
  - An instance of a relation is a set of tuples, also called records (row in the table)
  - The domain of a field is essentially the type of that field.
  - The degree, also called arity, of a relation is the number of fields.
  - The cardinality of a relation instance is the number of tuples in it.

Chapter 3.1.1 Creating and Modifying Relations Using SQL
  - The SQL language standard uses the word table to denote relation
  - The subset of SQL that supports the creation, deletion, and modification of tables is called the Data Definition Language (DDL).
  - CREATE TABLE command creates table:
  ```SQL
      CREATE TABLE <table-name>(
      <attributes>
      )
  ```
  - Tuples inserted with INSERT command:
  ```SQL
      INSERT
      INTO <table-name>(<attributes>)
      VALUES (<values>)
  ```
    - You do not need to list of column names and just list values in appropriate order but it is better to be explicit
  - Delete Tuples with DELETE command:
  ```SQL
      DELETE
      FROM <table>
      WHERE <attribute> = <value>
  ```
  - Modify value in existing row with UPDATE command:
  ```SQL
      UPDATE <table>
      SET <attributes> = <values>
      WHERE <attribute> = <value>
  ```
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
XML DATA NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
7.4.1 Introduction to XML
  1. Elements:
     - Elements (tags) are the primary building blocks of an XML document.
     - XML elements are case sensitive.
  2. Attributes:
     - All attribute values must be enclosed in quotes.
  3. Entity References:
     - Entities are shortcuts for portions of common text or the content of external files
  - We call an XML document well-formed if it has no associated DTD but follows these structural guidelines:
    1. The document starts with an XML declaration.
    2. A root element contains all the other elements.
    3. All elements must be properly nested.

7.4.2 XML DTDs
  - A DTD is a set of rules that allows us to specify our own set of elements, attributes, and entities.
  - We call a document valid if a DTD is associated with it and the document is structured according to the rules set by the DTD.
  - Possible element content type:
    1. Other elements
    2. The special symbol #PCDATA, which indicates (parsed) character data.
    3. The special symbol EMPTY that means element has no content. These are required to have an end tag.
    4. The special symbol ANY, which indicates that any content is permitted. This should be avoided
    5. A regular expression
  - Attributes of elements are declared outside the element.
  - The keyword ATTLIST indicates the beginning of an attribute declaration.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
JSON NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
  - NONE
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
RELATIONAL ALGEBRA NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 4 RELATIONAL ALGEBRA AND CALCULUS

Chapter 4.1 PRELIMINARIES
  - The inputs and outputs of a query are relations.

Chapter 4.2 RELATIONAL ALGEBRA

Chapter 4.2.1 Selection and Projection
  - select rows from a relation (σ)
  - project columns (π)
  - These operations allow us to manipulate data in a single relation.

Chapter 4.2.2 Set Operations
  - Union: Two relation instances are said to be union-compatible if the following conditions hold:
    1. They have the same number of fields
    2. corresponding fields taken left to right have the same domains.
  - Intersection
  - Set-difference
  - Cross-Product

Chapter 4.2.3 Renaming
  - Name conflicts can arise in some cases
  - Rename operator: ρ
  - Example:
    ρ(C(1 -> sid1, 5 -> sid2), S1 x R1)

Chapter 4.2.4 Joins
  - A join can be defined as a cross-product followed by selections and projections
  - Appears more frequently than a cross-product.
  - The result of a cross-product is typically much larger than the result of a join, and it is very important to recognize joins and implement them without materializing the underlying cross-product
  - Types of Joins:
    1. Condition Joins
      - R ⋈(condition) S = σ(condition)(R x S)
      - condition can and usually does refer to attributes of both R and S.
      - Condition can be referred to by name or position.
    2. Equijoin
      - The schema of the result of an equijoin contains the fields of R followed by the fields of S that do not appear in the join conditions.
    3. Natural Join
      - Equijoin in which equalities arc specified on all fields having the same name in R and S.
      - Natural Join has the property that the result is guaranteed not to have two fields with the same name.

Chapter 4.2.5 Division
  - The division operator is useful for expressing certain kinds of queries for example, "Find the names of sailors who have reserved all boats."
  - It is not needed as often so database systems do not try to exploit the semantics of division by implementing it as a distinct operator
  - Consider two relation instances A and B in which A has (exactly) two fields x and y and B has just one field y, with the same domain as in A. We define the division operation A/B as the set of all x values (in the form of unary tuples) such that for every y value in (a tuple of) B, there is a tuple (x,y) in A.
  -  It helps to think of A as a relation listing the parts supplied by suppliers and of the B relations as listing parts

Chapter 4.2.6 More Examples of Algebra Queries
  - Examples:
    1. (Q1) Find the names of sailors who have reserved boat 103.
         ```
         π(sname)((σ(bid=103)Reserves) ⋈ Sailors)
         ```
       - This can be broken down with rename operator:
         ```
         ρ(Temp1, σ(bid=103)Reserves)
         ρ(Temp2, Temp1 ⋈ Sailors)
         π(sname)(Temp2)
         ```
       - Queries are expressed by users in a language such as SQL.
       - The DBMS translates an SQL query into (an extended form of) relational algebra and then looks for other algebra expressions that produce the same answers but are cheaper to evaluate.

    2. (Q2) Find the names of sailors who have reserved a red boat
         ```
         π(sname)((σ(color='red')Boats) ⋈ Reserves ⋈ Sailors)
         ```
    3. (Q3) Find the colors of boats reserved by Lubber.
         ```
         π(color)((σ(sname='Lubber')Sailors) ⋈ Reserves ⋈ Boats)
         ```    
    4. (Q4) Find the names of sailors who have reserved at least one boat
         ```
         π(sname)(Reserves ⋈ Sailors)
         ```  
    5. (Q5) Find the names of sailors who have reserved a red or a green boat.
         ```
         ρ(Tempboats, σ(color='red')Boats ∪ (σ(color='green')Boats))
         π(sname)(Tempboats ⋈ Reserves ⋈ Sailors)
         ```
    6. (Q6) Find the names of sailors who have reserved a red and a green boat.
         ```
         ρ(Tempred, π(sid)(σ(color='red')Boats) ⋈ Reserves)
         ρ(Tempgreen, π(sid)(σ(color='green')Boats) ⋈ Reserves)
         π(sname)((Tempred ∩ Tempgreen) ⋈ Sailors)
         ```    
    7. (Q7) Find the names of sailors who have reserved at least two boats.
         ```
         ρ(Reservations, π(sid,sname,bid)(Sailors ⋈ Reserves)
         p(Reservationpairs(l -> sid1, 2 -> sname1,3 -> bid1, 4 -> sid2, 5 -> sname2, 6 -> bid2), Reservation x Reservations)
         π(sname1)σ(sid1 = sid2)∧(bid1 ≠ bid2)Reservationpairs
         ```
    8. (Q8) Find the sids of sailors with age over 20 who have not reserved a red boat.
         ```
         π(sid)(σ(age > 20)Sailors) - π(sid)((σ(color = 'red')Boats) ⋈ Reserves ⋈ Sailors)
         ```  
    9. (Q9) Find the names of sailors who have reserved all boats.
         ```
         ρ(Tempsids, (π(sid,bid)(Reserves))/(π(bid)Boats)
         π(sname)(Tempsids ⋈ Sailors)
         ```
    10. (Q10) Find the names of sailors who have reserved all boats called Interlake.
         ```
         ρ(Tempsids, (π(sid,bid)(Reserves))/(π(bid)(σ(bname = 'Interlake')Boats)))
         π(sname)(Tempsids ⋈ Sailors)
         ```    

Chapter 4.3 RELATIONAL CALCULUS
  - Alternative to relational algebra
  - Relation algebra is procedural while relational calculus is nonprocedural or declarative.
  - Relational Calculus allows us to describe the set of answers without being explicit about how they should be computed.
  - In <b>Tuple Relational Calculus (TRC)</b> variables take on tuples as values.
  - In another variant of Relational Calculus called <b> Domain Relational Calculus </b> the variables range over field values.

Chapter 4.3.1 Tuple Relational Calculus
  - A tuple variable is a variable that takes on tuples of a particular relation schema as values.
  - A tuple relational calculus query has the form { T | p(T) }, where T is a tuple variable and p(T) denotes a formula that describes T.
  - The result of this query is the set of all tuples t for which the formula p(T) evaluates to true with T = t.
  - Example:
    1. Find all sailors with a rating above 7.
       ```
       {S | S ∈ Sailors ∧ S.rating > 7}
       ```
  - Let Rel be a relation name, R and S be tuple variables, a be an attribute of R, and b be an attribute of S.   
  - Let op denote {<,>,=,≤,≥,≠}
  - An atomic function is:
    a. R ∈ Rel
    b. R.a op S.b
    c. R.a op constant, or constant op R.a
  - A formula is recursively defined to be one of the following, where p and q are themselves formulas and p(R) denotes a formula in which the variable R appears:
    a. any atomic formula
    b. ¬p, p ∧ q, p V q or p ⇒ q
    c. ∃R(p(R)), where R is a tuple variable
    d. ∀R(p(R)), where R is a tuple variable
  - The quantifiers ∃ and ∀ are said to bind the variable R.
  - A variable is said to be free in a formula or subformuia (a formula contained in a larger formula) if the (sub)formula does not contain an occurrence of a quantifier that binds it.3
  - A TRC query is defined to be expression of the form {T | p(T)}, where T is the only free variable in the formula p.
  - The answer to a TRC query {T | p(T)}, as noted earlier, is the set of all tuples t for which the formula p(T) evaluates to true with variable T assigned the tuple value t.
  - Example:
    1. Find the names and ages of sailors with a rating above 7.
       ```
       {S | ∃S ∈ Sailors (S.rating > 7 ∧ P.name = S.name ∧ P.age = S.age)}
       ```
       - The atomic formulas P.name = S.sname and Page = S.age give values to the fields of an answer tuple P
    2. Find the sailor name, boat id, and reservation date for each reservation.
       ```
       { P | ∃R ∈ Reserves ∃S ∈ Sailors
         (R.sid = S.sid ∧ P.bid = R.bid ∧ P.day = R.day ∧ P.sname = S.sname)
       }
       ```
    3. Find the names of sailors who have reserved boat 103.
       ```
       { P | ∃S ∈ Sailors ∃R ∈ Reserves
         (R.sid = S.sid ∧ R.bid = 103 ∧ P.sname = S.sname)
       }
       ```
    4. Find the names of sailors who have reserved a red boat
       ```
       { P | ∃S ∈ Sailors ∃R ∈ Reserves
         (R.sid = S.sid ∧ P.sname = S.sname ∧ ∃B ∈ Boats(
           B.bid = R.bid ∧ B.color = 'red'
           ))
       }
       ```
       - This query can be read as follows: "Retrieve all sailor tuples S for which there exist tuples R in Reserves and B in Boats such that S.sid = R.sid, R.bid = B.bid, and B.color ='red'."
       - Another way to write this:
       ```
       { P | ∃S ∈ Sailors ∃R ∈ Reserves ∃B ∈ Boats
         (R.sid = S.sid ∧ B.bid = R.bid ∧ B.color = 'red' ∧ P.sname = S.sname )
       }
       ```
    5. Find the names of sailors who have reserved at least two boats.
       ```
        {P | ∃S ∈ Sailors ∃R1 ∈ Reserves ∃R2 ∈ Reserves
          (R1.sid = S.sid ∧ R2.sid = S.sid ∧ R1.bid ≠ R2.bid ∧ P.sname = S.sname)
        }
       ```
    6. Find sailors who have reserved all red boats.
       ```
       {S | S ∈ Sailors ∧ ∀B ∈ Boats
         (B.color = 'red' ⇒ (∃R ∈ Reserves(S.sid = R.sid ∧ R.bid = B.bid)))
       }
       ```
       - This query can be read as follows: For each candidate (sailor), if a boat is red, the sailor must have reserved it. That is, for a candidate sailor, a boat being red must imply that the sailor has reserved it.
       - We can rewrite it using ¬p V q since that is logically equivalent to p ⇒ q
       ```
       {S | S ∈ Sailors ∧ ∀B ∈ Boats
         (B.color ≠ 'red' V (∃R ∈ Reserves(S.sid = R.sid ∧ R.bid = B.bid)))
       }
       ```
Chapter 4.3.2 Domain Relational Calculus
  - A domain variable is a variable that ranges over the values in the domain of some attribute
  - A DRC query has the form {⟨ X1,X2, ... ,Xn ⟩ | P(⟨ X1,X2, ... ,Xn ⟩)}, where each Xi is either a domain variable or a constant and p(⟨X1, X2, ... ,xn ⟩) denotes a DRC formula whose only free variables are the variables among the Xi, 1 <= i <= n.
  - An atomic formula in DRC is one of the following:
    1. ⟨ X1,X2, ... ,Xn ⟩ ∈ Rel, where Rel is a relation with n attributes; each xi, 1 <= i <= n is either a variable or a constant.
    2. X op Y
    3. X op constant, or constant op X
  - A formula is recursively defined to be one of the following, where P and q are themselves formulas and p(X) denotes a formula in which the variable X appears:
    1. any atomic formula
    2. ¬p, p ∧ q, p V q, or p ⇒ q
    3. ∃X(p(X)), where X is a domain variable
    4. ∀X(p(X)), where R is a domain variable
  - Examples:
    1. Find all sailors with a rating above 7.
       ```
       {⟨I, N, T, A⟩ | ⟨I, N, T, A⟩ ∈ Sailors ∧ T > 7}
       ```
      - In comparison with the TRC query, we can say T > 7 instead of S.rating > 7, but we must specify the tuple ⟨I, N, T, A⟩ in the result, rather than just S.
    2. Find the names of sailors who have reserved boat 103
       ```
       {⟨N⟩ | ∃I, T, A(⟨I, N, T, A⟩ ∈ Sailors
         ∧ ∃Ir, Br, D(⟨Ir, Br, D⟩ ∈ Reserves ∧ Ir = I ∧ Br =103)
         )}
       ```
       - A more compact notation:
       ```
       {⟨N⟩ | ∃I, T, A(⟨I, N, T, A⟩ ∈ Sailors
         ∧ ∃⟨Ir, Br, D⟩ ∈ Reserves(Ir = I ∧ Br = 103)
         )}
       ```
       - Can also be rewritten:
       ```
       {⟨N⟩ | ∃I, T, A(⟨I, N, T, A⟩ ∈ Sailors
         ∧ ∃D(⟨I, 103, D⟩ ∈ Reserves)
         )}
       ```
    3. Find the names of sailors who have reserved a red boat.
       ```
       {⟨N⟩ | ∃I, T, A(⟨I, N, T, A⟩ ∈ Sailors
         ∧ ∃⟨I, Br, D⟩ ∈ Reserves ∧ ∃⟨Br, BN, 'red'⟩ ∈ Boats)
         )}
       ```
    4. Find the names of sailors who have reserved at least two boats.
       ```
       {⟨N⟩ | ∃I, T, A(⟨I, N, T, A⟩ ∈ Sailors ∧
         ∃Br1, Br2, D1, D2(⟨I, Br1, D1⟩ ∈ Reserves
         ∧ ⟨I, Br2, D2⟩ ∈ Reserves ∧ Br1 ≠ Br2))
       }
       ```
       - Note how the repeated use of variable I ensures that the same sailor has reserved both the boats in question.
    5. Find the names of sailors who have reserved all Boats
       ```
       {⟨N⟩ | ∃I, T, A(⟨I, N, T, A⟩ ∈ Sailors ∧
         ∀B, BN, C(¬(⟨B, Bn, C⟩ ∈ Boats) V
         (∃⟨Ir, Br, D⟩ ∈ Reserves (I = Ir ∧ Br = B))))
       }
       ```
    6. Find Sailors who have reserved all red boats.
       ```
       {⟨I, N, T, A⟩ | ⟨I, N, T, A⟩ ∈ Sailors ∧ ∀⟨B, BN, C⟩ ∈ Boats
       (C = 'red' ⇒ ∃⟨Ir, Br, D⟩ ∈ Reserves(I = Ir ∧ Br = B))}
       ```

Chapter 4.4 EXPRESSIVE POWER OF ALGEBRA AND CALCULUS
  - Imagine : {S | ¬(S ∈ Sailors)}. The set of such S tuples is obviously infinite, in the context of infinite domains such as the set of all integers.
  - It is desirable to restrict relational calculus to disallow unsafe queries.
  - We therefore define a safe TRC formula Q to be a formula such that:
    1. For any given I, the set of answers for Q contains only values that are in Dom(Q, I).
    2. For each subexpression of the form ∃R(p(R)) in Q, if a tuple r (assigned to variable R) makes the formula true, then r contains only constants in Dom(Q,I).
    3. For each subexpression of the form ∀R(p(R)) in Q, if a tuple r (assigned to variable R) contains a constant that is not in Dom(Q, I), then r must make the formula true.
  - Every query that can be expressed using a safe relational calculus query can also be expressed as a relational algebra query.
  - If a query language can express all the queries that we can express in relational algebra, it is said to be relationally complete.
  - Commercial query languages typically support features that allow us to express some queries that cannot be expressed in relational algebra.


-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
SQL NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 3.1.1 Creating and Modifying Relations Using SQL
  - The SQL language standard uses the word table to denote relation, and we often follow this convention when discussing SQL.
  - The subset of SQL that supports the creation, deletion, and modification of tables is called the Data Definition Language (DDL).
  - The CREATE TABLE statement is used to define a new table:
  ```SQL
  CREATE TABLE Students ( sid CHAR(20),
                          name CHAR(30),
                          login CHAR(20),
                          age INTEGER,
                          gpa REAL)
  ```
  - Tuples are inserted, using the INSERT command.
  ```SQL
  INSERT INTO Students (sid, name, login, age, gpa)
  VALUES (53688, 'Smith', 'smith@ee', 18, 3.2)
  ```
  - We can optionally omit the list of column names in the INTO clause and list the values in the appropriate order, but it is good style to be explicit about column names.
  - We can delete tuples using the DELETE command.
  ```SQL
  DELETE
  FROM Students S
  WHERE S.name = 'Smith'
  ```
  - We can modify the column values in an existing row using the UPDATE command.
  ```SQL
  UPDATE Students S
  SET S.age = S.age + 1, S.gpa = S.gpa - 1
  WHERE S.sid = 53688
  ```
  - The WHERE clause is applied first and determines which rows are to be modified.
  - The SET clause then determines how these rows are to be modified.
  -  If the column being modified is also used to determine the new value, the value used in the expression on the right side of equals (=) is the old value, that is, before the modification.

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

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
XPATH AND XQUERY NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
27.7 XQUERY: QUERYING XML DATA
  - XQuery is the W3C standard query language for XML data

27.7.1 Path Expressions
  ```XQUERY
  FOR
    $1 IN doc(www.ourbookstore.com/books.xml)//AUTHOR/LASTNAME
  RETURN <RESULT> $1 </RESULT>
  ```
  - The separator // specifies that the AUTHOR element can be nested anywhere within the document whereas the separator / constrains the LASTNAME element to be nested immediately under (in terms of the graph structure of the document) the AUTHOR element.
  -  The result would be the following XML document:
  ```
  <RESULT><LASTNAME>Feynman </LASTNAME></RESULT>
  <RESULT><LASTNAME>Narayan </LASTNAME></RESULT>
  ```

27.7.2 FLWR Expressions
  - Basic form of an XQuery consists of FLWR : FOR, LET, WHERE and RETURN clauses
  - The FOR and LET clauses bind variables to values through path expressions. These values are qualified by the WHERE clause, and the result XML fragment is constructed by the RETURN clause.
  - FOR binds a variable to each element specified by the path expression, LET binds a variable to the whole collection of element.
  ```XQUERY
  LET
    $1 IN doc(www.ourbookstore.com/books.xml)//AUTHOR/LASTNAME
  RETURN <RESULT> $1 </RESULT>
  ```
  - The result would be the following XML document:
  ```
  <RESULT>
    <LASTNAME>Feynman</LASTNAME>
    <LASTNAME>Narayan</LASTNAME>
  </RESULT>
  ```
  - Selection conditions are expressed using the WHERE clause.
  - The output of a query is not limited to a single element.
  ```XQUERY
  FOR $b IN doc(www.ourbookstore.com/books.xm1)/BOOKLIST/BOOK
  WHERE $b/PUBLISHED='1980'
  RETURN
    <RESULT> $b/AUTHOR/FIRSTNAME, $b/AUTHOR/LASTNAME </RESULT>
  ```
  - The result would be the following XML document:
  ```
  <RESULT>
    <FIRSTNAME>Richard </FIRSTNAME><LASTNAME>Feynman </LASTNAME>
  </RESULT>
  <RESULT>
    <FIRSTNAME>R.K. </FIRSTNAME><LASTNAME>Narayan </LASTNAME>
  </RESULT>
  ```
  - Can be rewritten as:
  ```XQUERY
  FOR $a IN doc(www.ourbookstore.com/books.xml)/BOOKLIST/BOOK[PUBLISHED='1980']/AUTHOR
  RETURN
    <RESULT> $a/FIRSTNAME, $a/LASTNAME </RESULT>
  ```

27.7.3 Ordering of Elements
  - XML data consists of ordered documents and so the query language must return data in some order.
  - If we desire a different order, we can explicitly order the output as shown in the following query, which returns TITLE elements sorted lexicographically.
  ```XQUERY
  FOR
    $b IN doc(www.ourbookstore.com/books.xml)/BOOKLIST/BOOK
  RETURN <BOOKTITLES> $b/TITLE </BOOKTITLES>
  SORT BY TITLE
  ```

27.7.4 Grouping and Generation of Collection Values
  - Suppose that for each year we want to find the last names of authors who wrote a book published in that year. We group by year of publication and generate a list of last names for each year:
  ```XQUERY
  FOR $p IN DISTINCT
    doc(www.ourbookstore.com/books.xml)/BOOKLIST/BOOK/PUBLISHED
    RETURN
      <RESULT>
        $p,
        FOR $a IN DISTINCT /BOOKLIST/BOOK[PUBLISHED=$p]/AUTHOR
          RETURN $a
      </RESULT>
  ```
  - The result would be the following XML document:
  ```
  <RESULT> <PUBLISHED>1980</PUBLISHED>
    <LASTNAME>Feynman</LASTNAME>
    <LASTNAME>Narayan</LASTNAME>
  </RESULT>
  <RESULT> <PUBLISHED>1981</PUBLISHED>
    <LASTNAME>Narayan</LASTNAME>
  </RESULT>
  ```
  
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
XSLT NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
RELATIONAL DESIGN THEORY NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
UNIFIED MODELING LANGUAGE NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
INDEXES AND TRANSACTIONS NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
CONSTRAINTS AND TRIGGERS NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
VIEWS AND AUTHORIZATION NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
ON-LINE ANALYTICAL PROCESSING NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
RECURSION IN SQL NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
