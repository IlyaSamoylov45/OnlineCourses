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
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
XPATH AND XQUERY NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
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
