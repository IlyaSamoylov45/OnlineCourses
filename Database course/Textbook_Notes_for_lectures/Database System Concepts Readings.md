-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
INTRODUCTION AND RELATIONAL DATABASES NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 1.1 Database-System Applications
  - Lots or real world uses.

Chapter 1.2 Purpose of Database Systems
  - Before database management systems (DBMSs) were introduced, organizations usually stored information in a file-processing system.
  - Disadvantages (file-processing system):
    1. Data redundancy and inconsistency
    2. Difficulty accessing data
        -  Conventional file-processing environments do not allow needed data to be retrieved in a convenient and efﬁcient manner
    3. Data isolation
    4. Integrity problems
    5. Atomicity problems
        - atomic—it must happen in its entirety or not at all.
    6. Concurrent-access anomalies
    7. Security problems

Chapter 1.3 View of Data

Chapter 1.3.1 Data Abstraction
  - Physical level
    - How data is actually stored.
  - Logical Level
    - What data is stored in the DB and the relationships.
  - View Level
    - The highest level of abstraction describes only part of the entire database.

Chapter 1.3.2 Instances and Schemas
  - The collection of information stored in the DB at a particular moment is called an instance of the DB.
  - Overall design of DB is called database schema.
  - The physical schema is hidden beneath the logical schema, and can usually be changed easily without affecting application programs.

Chapter 1.3.3 Data Models
  - Data model: a collection of conceptual tools for describing data, data relationships, data semantics, and consistency constraints.

Chapter 1.4 Database Languages
  - A database system provides a data-deﬁnition language to specify the database schema and a data-manipulation language to express database queries and updates. They are not really two separate languages but instead form parts of a single database language.

Chapter 1.4.1 Data-Manipulation Language
  - A data-manipulation language (DML) is a language that enables users to access or manipulate data as organized by the appropriate data model.
  - Types of accesses:
    1. Retrieve information
    2. Insert information
    3. Delete information
    4. Modify information
  - Two Types:
    1. Procedural DMLs: user specify what data and how to get the data.
    2. Declarative DMLs: user specify what data are needed not how to get it.
  - The portion of a DML that involves information retrieval is called a query language.   

Chapter 1.4.2 Data-Deﬁnition Language
  - Database schema is expressed by a special language known as the DDL
  - Database systems implement integrity constraints that can be tested with minimal overhead:
    1. Domain Constraints
    2. Referential Integrity
    3. Assertions
       - An assertion is any condition that the database must always satisfy.
       - Domain constraints and referential-integrity constraints are special forms of assertions.
       -  When an assertion is created, the system tests it for validity. If the assertion is valid, then any future modiﬁcation to the database is allowed only if it does not cause that assertion to be violated.
    4. Authorization
       1. Read authorization, which allows reading, but not modiﬁcation, of data
       2. Insert authorization, which allows insertion of new data, but not modiﬁcation of existing data
       3. Update authorization, which allows modiﬁcation, but not deletion of data
       4. Delete authorization, which allows deletion of data.

Chapter 1.5 Relational Databases

Chapter 1.5.1 Tables
  - The relational model is an example of a record-based model.
  - Record-based models are so named because the database is structured in ﬁxed-format records of several types
Chapter 1.6 Database Design
  - Database design mainly involves the design of the database schema.

Chapter 1.5.2 Data-Manipulation Language
  - The SQL query language is nonprocedural. A query takes as input several tables (possibly only one) and always returns a single table.

Chapter 1.5.3 Data-Deﬁnition Language
  - SQL provides a rich DDL that allows one to deﬁne tables, integrity constraints, assertions, etc.

Chapter 1.5.4 Database Access from Application Programs
  - SQL also does not support actions such as input from users, output to displays, or communication over the network.
  - Application programs are programs that are used to interact with the database
Chapter 1.6.1 Design Process
  - The focus at this point is on describing the data and their relationships, rather than on specifying physical storage details.
  - In a speciﬁcation of functional requirements, users describe the kinds of operations (or transactions) that will be performed on the data

Chapter 1.6.2 Database Design for a University Organization

Chapter 1.6.3 The Entity-Relationship Model
  - The entity-relationship (E-R) data model uses a collection of basic objects, called entities, and relationships among these objects.
  - A relationship is an association among several entities
  - Entity sets are represented by a rectangular box with the entity set name in the header and the attributes listed below it.
  - Relationship sets are represented by a diamond connecting a pair of related entity sets. The name of the relationship is placed inside the diamond.
  - One important constraint is mapping cardinalities, which express the number of entities to which another entity can be associated via a relationship set.

Chapter 1.6.4 Normalization
  -  Among the undesirable properties that a bad design may have are:
     1. Repetition of information
     2. Inability to represent certain information
  - The goal of Normalization is to generate a set of relation schemas that allows us to store information without unnecessary redundancy, yet also allows us to retrieve information easily.

Chapter 1.7 Data Storage and Querying
  - The query processor is important because it helps the database system to simplify and facilitate access to data.

Chapter 1.7.1 Storage Manager
  - The storage manager is the component of a database system that provides the interface between the low-level data stored in the database and the application programs and queries submitted to the system.
  - The storage manager components include
    1. Authorization and integrity manager : tests for the satisfaction of integrity constraints and checks the authority of users to access data.
    2. Transaction manager : ensures that the database remains in a consistent (correct) state despite system failures, and that concurrent transaction executions proceed without conﬂicting.
    3. File manager : manages the allocation of space on disk storage and the data structures used to represent information stored on disk.
    4. Buffer manager : responsible for fetching data from disk storage into main memory, and deciding what data to cache in main memory.

Chapter 1.7.2 The Query Processor
  - The query processor components include:
    1. DDL interpreter, which interprets DDL statements and records the deﬁnitions in the data dictionary.
    2. DML compiler, which translates DML statements in a query language into an evaluation plan consisting of low-level instructions that the query evaluation engine understands.
       Also performs query optimizations.
    3. Query evaluation engine, which executes low-level instructions generated by the DML compiler.

Chapter 1.8 Transaction Management
  - all-or-none requirement : atomicity.
  - correctness requirement : consistency.
  - persistence requirement : durability.
  - A transaction is a collection of operations that performs a single logical function in a database application.
  - Database must preform failure recovery in the event that a system failure occurs that restores the DB to previous state.
  - It is the responsibility of the concurrency-control manager to control the interaction among the concurrent transactions
  - The transaction manager consists of the concurrency-control manager and the recovery manager.

Chapter 1.9 Database Architecture
  - Most users of a database system today are not present at the site of the database system, but connect to it through a network.
  - Client machines, on which remote database users work, and server machines
  - In a two-tier architecture, the application resides at the client machine, where it invokes database system functionality at the server machine
  - In two-tier architectures application program interface standards like ODBC and JDBC are used for interaction between the client and the server.
  - In a three-tier architecture, the client machine acts as merely a front end and does not contain any direct database calls.
  - Three-tier applications are more appropriate for large applications, and for applications that run on the World Wide Web.

Chapter 1.10 Data Mining and Information Retrieval
  - The term data mining refers loosely to the process of semi automatically analyzing large databases to ﬁnd useful patterns.
  - Data warehouses gather data from multiple sources under a uniﬁed schema, at a single site.

Chapter 1.11 Specialty Databases

Chapter 1.11.1 Object-Based Data Models
  - The major database vendors presently support the object-relational data model, a data model that combines features of the object-oriented data model and relational data model.

Chapter 1.11.2 Semi-structured Data Models
  -Semi-structured data models permit the speciﬁcation of data where individual data items of the same type may have different sets of attributes.
  - XML

Chapter 1.12 Database Users and Administrators

Chapter 1.12.1 Database Users and User Interfaces
  - Four Types:
    1. Naïve users
    2. Application programmers
    3. Sophisticated users
    4. Specialized users

Chapter 1.12.2 Database Administrator
  - The functions of DBA include:
    - Schema definition
    - Storage structure and access-method deﬁnition.
    - Schema and physical-organization modiﬁcation.
    - Granting of authorization for data access.
    - Routine maintenance

Chapter 1.13 History of Database Systems
  - 1950s and early 1960s: Magnetic tapes were developed for data storage
  - Late 1960s and 1970s: Wide spread use of hard disk (Allowed for direct access to data)
  - 1980s: Relational model not used widely until IBM System R
  - Early 1990s: Many database vendors introduced parallel database products in this period
  - 1990s: Explosive growth of WWW.
  - 2000s: The emerging of XML and the associated query language XQuery as a new database technology. Data-mining techniques are now widely deployed

Chapter 1.14 Summary
  - Data model: a collection of conceptual tools for describing data, data relationships, data semantics, and data constraints.

Chapter 2.1 Structure of Relational Databases
  - A relational database consists of a collection of tables, each of which is assigned a unique name
  - In the relational model the term relation is used to refer to a table, while the term tuple is used to refer to a row. Similarly, the term attribute refers to a column of a table.
  - A data-manipulation language (DML) is a language that enables users to access or manipulate data.

Chapter 2.2 Database Schema
  - A relational schema consists of a list of attributes and their corresponding domains.

Chapter 3.1 Overview of the SQL Query Language
  - Originally called Sequel but renamed to Structured Query Language
  - The SQL language has several parts:
    - Data-deﬁnition language(DDL): DDL provides commands for deﬁning relation schemas, deleting relations, and modifying relation schemas.
    - Data-manipulation language (DML): provides the ability to query information from the database and work with tuples.
    - Integrity: integrity constraints, updates that violate integrity constraints are not allowed
    - View Definition
    - Transaction control
    - Embedded SQL and dynamic SQL (define how SQL can be embedded in languages like C, Java, C++)
    - Authorization: specifying access rights to relations and views.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
XML DATA NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 23.1 Motivation
  - The term markup refers to anything in a document that is not intended to be part of the printed output.
  - Benefits of XML when it's used to exchange data between organizations, and for storing complex structured information in ﬁles
    1. The presence of the tags makes the message self-documenting
    2. the format of the document is not rigid
    3. XML allows nested structures

Chapter 23.2 Structure of XML Data
  - An element is simply a pair of matching start-tag and end-tag
  - XML documents must have a single root element that encompasses all other elements in the document
  - The ability to nest elements within other elements provides an alternative way to represent information.
  - In general, it is advisable to use attributes only to represent identiﬁers, and to store all other data as sub-elements.
  - One ﬁnal syntactic note is that an element of the form <element></element> that contains no subelements or text can be abbreviated as <element/>
  - A document can have more than one namespace, declared as part of the root element.

Chapter 23.3.1 Document Type Deﬁnition
  - The document type deﬁnition(DTD) is an optional part of an XML document.
  - The keyword #PCDATA indicates text data
  - An attribute of type ID provides a unique identiﬁer for the element
  - The type IDREFS allows a list of references, separated by spaces.
  - An attribute of type IDREF is a reference to an element; the attribute must contain a value that appears in the ID attribute of some element in the document.

Chapter 23.3.2 XML Schema
  - Made to address deficiencies in DTD XMLSchemadeﬁnesanumberofbuilt-intypessuchasstring,integer,decimal date, and boolean.
  - Schema deﬁnitions in XML Schema are themselves speciﬁed in XML syntax
  - Key Ex:
  ```xml
    <xs:key name = “deptKey”>
        <xs:selector xpath = “/university/department”/>
        <xs:ﬁeld xpath = “dept name”/>
    </xs:key>
  ```
  - Foreign Key Ex:
  ```xml
    <xs: name = “courseDeptFKey” refer=“deptKey”>
        <xs:selector xpath = “/university/course”/>
        <xs:ﬁeld xpath = “dept name”/>
    </xs:keyref>
  ```
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
JSON NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
  - NONE
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
RELATIONAL ALGEBRA NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 6.1 The Relational Algebra
  - The relational algebra is a procedural query language.
  - The fundamental operations in the relational algebra are select, project, union, set difference, Cartesian product, and rename.
  - In addition to the fundamental operations, there are several other operations—namely, set intersection, natural join, and assignment.

Chapter 6.1.1 Fundamental Operations
  - The select, project, and rename operations are called unary operations, because they operate on one relation.
  - The other three operations operate on pairs of relations and are, therefore, called binary operations.

Chapter 6.1.1.1 The Select Operation
  - The select operation selects tuples that satisfy a given predicate. We use the lowercase Greek letter sigma (σ) to denote selection.
  - In general, we allow comparisons using <,>,=,≤,≥, and ≠ in the selection predicate. Furthermore, we can combine several predicates into a larger predicate by using the connectives and (∧), or (∨), and not (¬).
  - In relational algebra, the term select corresponds to what we refer to in SQL as where.

Chapter 6.1.1.2 The Project Operation
  - The project operation is a unary operation that returns its argument relation, with certain attributes left out.
  - Since a relation is a set, any duplicate rows are eliminated.
  - Projection is denoted by the uppercase Greek letter pi (π).

Chapter 6.1.1.3 Composition of Relational Operations
  - The fact that the result of a relational operation is itself a relation is important.
  - In general, since the result of a relational-algebra operation is of the same type (relation) as its inputs, relational-algebra operations can be composed together into a relational-algebra expression.

Chapter 6.1.1.4 The Union Operation
  - In general, we must ensure that unions are taken between compatible relations.
  - For a union operation r ∪ s to be valid, we require that two conditions hold:
    1. The relations r and s must be of the same arity. That is, they must have the same number of attributes.
    2. The domains of the ith attribute of r and the ith attribute of s must be the same, for all i.

Chapter 6.1.1.5 The Set-Difference Operation
  - The set-difference operation, denoted by ¬, allows us to ﬁnd tuples that are in one relation but are not in another.
  - As with the union operation, we must ensure that set differences are taken between compatible relations.

Chapter 6.1.1.6 The Cartesian-Product Operation
  - The Cartesian-product operation, denoted by a cross (×), allows us to combine information from any two relations.
  - Assume that we have n1 tuples in instructor and n2 tuples in teaches. Then, there are n1∗n2 ways of choosing a pair of tuples — one tuple from each relation; so there are n1∗n2 tuples in r.

Chapter 6.1.1.7 The Rename Operation
  - The rename operator, denoted by the lowercase Greek letter rho(p)
    - p(x)(E) : returns the result of expression E under the name x
    - p(x)(A1,A2, . . . , An)(E) : returns the result of expression E under the name x, and with the attributes renamed to A1, A2,...,An.
  - We can name attributes of a relation implicitly by using a positional notation, where $1,$2, ... refer to the ﬁrst attribute, the second attribute, and so on.

Chapter 6.1.2 Formal Deﬁnition of the Relational Algebra
  - A basic expression in the relational algebra consists of either one of the following:
    - A relation in the database
    - A constant relation
  - Let E1 and E2 be relational-algebra expressions. Then, the following are all relational-algebra expressions:
    - E1 ∪ E2
    - E1 − E2
    - E1 × E2
    - σ(p)(E1), where P is a predicate on attributes in E1
    - π(s)(E1), where S is a list consisting of some of the attributes in E1
    - p(x) (E1), where x is the new name for the result of E1

Chapter 6.1.3 Additional Relational-Algebra Operations

Chapter 6.1.3.1 The Set-Intersection Operation
  - set intersection (∩).
  - We can rewrite any relational-algebra expression that uses set intersection by replacing the intersection operation with a pair of set-difference operations as:
    - r ∩ s = r − (r − s)

Chapter 6.1.3.2 The Natural-Join Operation
  - The natural join is a binary operation that allows us to combine certain selections and a Cartesian product into one operation. It is denoted by the join symbol ⋈.
  - Please note that if r(R) and s(S) are relations without any attributes in common, that is, R ∩ S = ∅, then r ⋈ s = r × s.

Chapter 6.1.3.3 The Assignment Operation
  - The assignment operation, denoted by ←, works like assignment in a programming language. To illustrate this operation, consider the deﬁnition of the natural-join operation.
  - The result of the expression to the right of the ← is assigned to the relation variable on the left of the ←.

Chapter 6.1.3.4 Outer join Operations
  - The outer-join operation is an extension of the join operation to deal with missing information.
  - The outer join operation works in a manner similar to the natural join operation we have already studied, but preserves those tuples that would be lost in an join by creating tuples in the result containing null values.
  - There are actually three forms of the operation: left outer join, right outer join, and full outer join.
  - The left outer join takes all tuples in the left relation that did not match with any tuple in the right relation, pads the tuples with null values for all other attributes from the right relation, and adds them to the result of the natural join.
  - The right outer join is symmetric with the left outer join: It pads tuples from the right relation that did not match any from the left relation with nulls and adds them to the result of the natural join.
  - The full outer join does both the left and right outer join operations, padding tuples from the left relation that did not match any from the right relation, as well as tuples from the right relation that did not match any from the left relation, and adding them to the result of the join.

Chapter 6.1.4 Extended Relational-Algebra Operations
  - Queries that cannot be expressed using the basic relational-algebra operations, these operations are called extended relational-algebra operations.

Chapter 6.1.4.1 Generalized Projection
  - The ﬁrst operation is the generalized-projection operation, which extends the projection operation by allowing operations such as arithmetic and string functions to be used in the projection list.
  - The generalized-projection operation has the form: π(F1,F2,...,Fn)(E)
    - where E is any relational-algebra expression, and each of F1, F2,...,Fn is an arithmetic expression involving constants and attributes in the schema of E.
  - In general, an expression can use arithmetic operations such as +, −, ∗, and ÷ on numeric valued attributes, numeric constants, and on expressions that generate a numeric result.
  - Generalized projection also permits operations on other data types, such as concatenation of strings.
    - For example, the expression:
      - π(ID,name,dept_name,salary÷12)(instructor)
      - Gives the ID, name, dept_name, and the monthly salary of each instructor.

Chapter 6.1.4.2 Aggregation
  - Permits the use of aggregate functions such as min or average, on sets of values.
  - Aggregate functions take a collection of values and return a single value as a result.
    - Example: count, sum, avg, min, max
  - The collections on which aggregate functions operate can have multiple occurrences of a value; the order in which the values appear is not relevant. Such collections are called multisets.
  - Sets are a special case of multisets where there is only one copy of each element.
  - G(sum(salary))(instructor)
    -  The relational-algebra operation G signiﬁes that aggregation is to be applied, and its subscript speciﬁes the aggregate operation to be applied.
  - There are cases where we must eliminate multiple occurrences of a value before computing an aggregate function. If we do want to eliminate duplicates, we use the same function names as before, with the addition of the hyphenated string “distinct” appended to the end of the function name.
  - The aggregate function count-distinct ensures that even if an instructor teaches more than one course, she is counted only once in the result.
  - There are circumstances where we would like to apply the aggregate function not to a single set of tuples, but instead to a group of sets of tuples.
    - (dept_name)G(average(salary))(instructor) vs G(average(salary))(instructor)
  - The general form of the aggregation operation G is as follows:
    - (G1,G2,...,Gn)G(F1(A1), F2(A2),..., Fm(Am))(E)
    - where E is any relational-algebra expression; G1, G2,..., Gn constitute a list of attributes on which to group; each Fi is an aggregate function; and each Ai is an attribute name.
    - The meaning of the operation is as follows: The tuples in the result of expression E are partitioned into groups in such a way that:
      1. All tuples in a group have the same values for G1,G2,...,Gn.
      2. Tuples in different groups have different values for G1,G2,...,Gn.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
SQL NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 3.1.1  Domains, Attributes, Tuples, and Relations
  - A domain D is a set of atomic values.
  - By atomic we mean that each value in the domain is indivisible as far as the formal relational model is concerned.
  - A common method of specifying a domain is to specify a data type from which the data values forming the domain are drawn.
  - It is also useful to specify a name for the domain, to help in interpreting its values.
  - A data type or format is also specified for each domain.
  - A relation schema R, denoted by R(A1,A2,...,An),is made up of a relation name R and a list of attributes, A1, A2, ... , An.
  - Each attribute Ai is the name of a role played by some domain D in the relation schema R. D is called the domain of Ai and is denoted by dom(Ai).
  - A relation schema is used to describe a relation; R is called the name of this relation.
  - The degree (or arity) of a relation is the number of attributes n of its relation schema.
  - It is also possible to refer to attributes of a relation schema by their position within the relation
  - A relation (or relation state) r of the relation schema R(A1, A2, ..., An), also denoted by r(R),is a set of n-tuples r= {t1,t2,...,tm}.
  - Each n-tuple t is an ordered list of n values t = ``` <v 1,v 2, ..., vn> ```, where each value vi, 1 ≤ i ≤ n, is an element of dom (Ai) or is a special NULL value.
  - A relation schema is sometimes called a relation scheme.
  - The earlier definition of a relation can be restated more formally using set theory concepts as follows. A relation (or relation state) r(R) is a mathematical relation of degree n on the domains dom(A1), dom(A2), ..., dom(An), which is a subset of the Cartesian product (denoted by ×) of the domains that define R: ``` r(R)⊆(dom(A1) × dom(A2) × ... × dom(An)) ```
  - The Cartesian product specifies all possible combinations of values from the underlying domains. Hence, if we denote the total number of values, or cardinality, in a domain D by |D| (assuming that all domains are finite), the total number of tuples in the Cartesian product is ``` |dom(A1)| × |dom(A2)| × ... × |dom(An)| ```
  - This product of cardinalities of all domains represents the total number of possible instances or tuples that can ever exist in any relation state r(R)
  - The current relation state—reflects only the valid tuples that represent a particular state of the real world. In general, as the state of the real world changes, so does the relation state, by being transformed into another relation state.
  - It is possible for several attributes to have the same domain. The attribute names indicate different roles, or interpretations, for the domain.

Chapter 4.3 Basic Retrieval Queries in SQL
  - SQL has one basic statement for retrieving information from a database: the SELECT statement.
  - The SELECT statement is not the same as the SELECT operation of relational algebra
  - SQL allows a table (relation) to have two or more tuples that are identical in all their attribute values. Hence, in general, an SQL table is not a set of tuples, because a set does not allow two identical members; rather, it is a multiset (sometimes called a bag) of tuples
  - Some SQL relations are constrained to be sets because a key constraint has been declared or because the DISTINCT option has been used with the SELECT statement

Chapter 4.3.1 The SELECT-FROM-WHERE Structure of Basic SQL Queries
  - The basic form of the SELECT statement, sometimes called a mapping or a select-from-where block, is:
  ```SQL
  SELECT <attribute list>
  FROM <table list>
  WHERE <condition>;
  ```
    - Where:
      1. ``` <attribute list> ``` is a list of attribute names whose values are to be retrieved by the query.
      2. ``` <table list> ``` is a list of the relation names required to process the query.
      3. ``` <condition> ``` is a conditional (Boolean) expression that identifies the tuples to be retrieved by the query.
  - Ex: Retrieve the birth date and address of the employee(s) whose name is ‘John B.Smith’.
  ```SQL
  SELECT Bdate, Address
  FROM EMPLOYEE
  WHERE Fname = ‘John’ AND Minit = ‘B’ AND Lname = ‘Smith’;
  ```
  - The SELECT clause of SQL specifies the attributes whose values are to be retrieved, which are called the projection attributes
  - The WHERE clause specifies the Boolean condition that must be true for any retrieved tuple, which is known as the selection condition.
  - Ex: Retrieve the name and address of all employees who work for the ‘Research’ department.
  ```SQL
  SELECT Fname, Lname, Address
  FROM EMPLOYEE, DEPARTMENT
  WHERE Dname = ‘Research’ AND Dnumber = Dno;
  ```
  - The condition Dname = ‘Research’ is a selection condition that chooses the particular tuple of interest in the DEPARTMENT table, because Dname is an attribute of DEPARTMENT.
  - The condition Dnumber = Dno is called a join condition, because it combines two tuples
  - In general, any number of selection and join conditions may be specified in a single SQL query.
  - A query that involves only selection and join conditions plus projection attributes is known as a select-project-join query.
  - Ex: For every project located in ‘Stafford’, list the project number, the controlling department number, and the department manager’s last name, address, and birth date.
  ```SQL
  SELECT Pnumber, Dnum, Lname, Address, Bdate
  FROM PROJECT, DEPARTMENT, EMPLOYEE
  WHERE Dnum = Dnumber
        AND Mgr_ssn = Ssn
        AND Plocation = ‘Stafford’;
  ```

Chapter 4.3.2 Ambiguous Attribute Names, Aliasing, Renaming, and Tuple Variables
  - In SQL, the same name can be used for two (or more) attributes as long as the attributes are in different relations.
  - If this is the case, and a multitable query refers to two or more attributes with the same name, we must qualify the attribute name with the relation name to prevent ambiguity.
  - The ambiguity of attribute names also arises in the case of queries that refer to the same relation twice
  - Ex: For each employee, retrieve the employee’s first and last name and the first and last name of his or her immediate supervisor.
  ```SQL
  SELECT E.Fname, E.Lname, S.Fname, S.Lname
  FROM EMPLOYEE AS E, EMPLOYEE AS S
  WHERE E.Super_ssn = S.Ssn;
  ```
  - An alias can follow the keyword AS, or it can directly follow the relation name—for example, by writing EMPLOYEE E, EMPLOYEE Sin the FROM clause of Q8.
  - It is also possible to rename the relation attributes within the query in SQL by giving them aliases. For example, if we write ``` EMPLOYEE AS E(Fn, Mi, Ln, Ssn, Bd, Addr, Sex, Sal, Sssn, Dno)  ``` in the FROM clause.

Chapter 4.3.3 Unspecified WHERE Clause and Use of the Asterisk
  - If more than one relation is specified in the FROM clause and there is no WHERE clause, then the CROSS PRODUCT—all possible tuple combinations—of these relations is selected.
  - Ex: Select all EMPLOYEE Ssns (Q9) and all combinations of EMPLOYEE Ssn and DEPARTMENT Dname (Q10) in the database.
  ```SQL
  Q9:  SELECT Ssn
       FROM EMPLOYEE;
  Q10: SELECT Ssn, Dname
       FROM EMPLOYEE, DEPARTMENT;
  ```
  - It is extremely important to specify every selection and join condition in the WHERE clause; if any such condition is overlooked, incorrect and very large relations may result.
  - To retrieve all the attribute values of the selected tuples, we do not have to list the attribute names explicitly in SQL; we just specify an asterisk ( * ), which stands for all the attributes.

Chapter 4.3.4 Tables as Sets in SQL
  - SQL usually treats a table not as a set but rather as a multiset; duplicate tuples can appear more than once in a table, and in the result of a query.
  - SQL does not automatically eliminate duplicate tuples in the results of queries, for the following reasons:
    - Duplicate elimination is an expensive operation. One way to implement it is to sort the tuples first and then eliminate duplicates.
    - The user may want to see duplicate tuples in the result of a query.
    - When an aggregate function is applied to tuples, in most cases we do not want to eliminate duplicates.
  - An SQL table with a key is restricted to being a set, since the key value must be distinct in each tuple.
  - If we do want to eliminate duplicate tuples from the result of an SQL query, we use the keyword DISTINCT in the SELECT clause.
  - In general, a query with SELECT DISTINCT eliminates duplicates, whereas a query with SELECT ALL does not.
  - Specifying SELECT with neither ALL nor DISTINCT is equivalent to SELECT ALL.
  - Ex: Retrieve the salary of every employee (Q11) and all distinct salary values (Q11A).
  ```SQL
  Q11:  SELECT ALL Salary
        FROM EMPLOYEE;
  Q11A: SELECT DISTINCT Salary
        FROM EMPLOYEE;
  ```
  - There are set union (UNION), set difference (EXCEPT)  and set intersection (INTERSECT) operations.
  - These set operations apply only to union-compatible relations, so we must make sure that the two relations on which we apply the operation have the same attributes and that the attributes appear in the same order in both relations.
  - Ex: Make a list of all project numbers for projects that involve an employee whose last name is ‘Smith’, either as a worker or as a manager of the department that controls the project.
  ```SQL
  (SELECT DISTINCT Pnumber
   FROM PROJECT, DEPARTMENT, EMPLOYEE
   WHERE Dnum=Dnumber
         AND Mgr_ssn=Ssn
         AND Lname=‘Smith’
  )
  UNION
  ( SELECT DISTINCT Pnumber
    FROM PROJECT, WORKS_ON, EMPLOYEE
    WHERE Pnumber=Pno
          AND Essn=Ssn
          AND Lname=‘Smith’
  );
  ```
  - SQL also has corresponding multiset operations, which are followed by the keyword ALL (UNION ALL,EXCEPT ALL,INTERSECT ALL).Their results are multisets (duplicates are not eliminated).

Chapter 4.3.5 Substring Pattern Matching and Arithmetic Operators
  - The LIKE comparison operator can be used for string pattern matching.
  - Partial strings are specified using two reserved characters: % replaces an arbitrary number of zero or more characters, and the underscore ( _ ) replaces a single character
  - Ex: Retrieve all employees whose address is in Houston, Texas.
  ```SQL
  SELECT Fname, Lname
  FROM EMPLOYEE
  WHERE Address LIKE ‘%Houston,TX%’;
  ```
  - Ex: Find all employees who were born during the 1950s.
  ```SQL
  SELECT Fname, Lname
  FROM EMPLOYEE
  WHERE Bdate LIKE ‘_ _ 5 _ _ _ _ _ _ _’;
  ```
  - If an underscore or % is needed as a literal character in the string, the character should be preceded by an escape character, which is specified after the string using the keyword ESCAPE ( \ )
  - If an apostrophe (’) is needed, it is represented as two consecutive apostrophes (”) so that it will not be interpreted as ending the string.
  - Notice that substring comparison implies that attribute values are not atomic (indivisible) values : an atomic value cannot be broken up into smaller parts. Everything in database should be atomic.
  - Another feature allows the use of arithmetic in queries.
  - Ex: Show the resulting salaries if every employee working on the ‘ProductX’ project is given a 10 percent raise.
  ```SQL
  SELECT E.Fname, E.Lname, 1.1 * E.Salary AS Increased_sal
  FROM EMPLOYEE AS E, WORKS_ON AS W, PROJECT AS P
  WHERE E.Ssn = W.Essn AND W.Pno = P.Pnumber AND P.Pname = ‘ProductX’;
  ```
  - For string data types, the concatenate operator || can be used in a query to append two string values.
  - For date, time, timestamp, and interval data types, operators include incrementing (+) or decrementing (–) a date, time, or timestamp by an interval.
  - Another comparison operator, which can be used for convenience, is BETWEEN
  - Ex: Retrieve all employees in department 5 whose salary is between $30,000 and $40,000.
  ```SQL
  SELECT *
  FROM EMPLOYEE
  WHERE (Salary BETWEEN 30000 AND 40000) AND Dno = 5;
  ```

Chapter 4.3.6 Ordering of Query Results
  - Ex: Retrieve a list of employees and the projects they are working on, ordered by department and,within each department, ordered alphabetically by last name, then first name.
  ```SQL
  SELECT D.Dname, E.Lname, E.Fname, P.Pname
  FROM DEPARTMENT D, EMPLOYEE E, WORKS_ON W, PROJECT P
  WHERE D.Dnumber= E.Dno
        AND E.Ssn= W.Essn
        AND W.Pno= P.Pnumber
  ORDER BY D.Dname, E.Lname, E.Fname;
  ```
  - The default order is in ascending order of values. We can specify the keyword DESC if we want to see the result in a descending order of values.

Chapter 4.3.7 Discussion and Summary of Basic SQL Retrieval Queries
  - A simple retrieval query in SQL can consist of up to four clauses, but only the first two—SELECT and FROM—are mandatory.
  ```SQL
  SELECT <attribute list>
  FROM <table list>
  [ WHERE <condition> ]
  [ ORDER BY <attribute list> ];
  ```

Chapter 4.4 INSERT, DELETE, and UPDATE Statements in SQL
  - Three commands can be used to modify the database: INSERT, DELETE, and UPDATE.

Chapter 4.4.1 The INSERT Command
  - In its simplest form,INSERTis used to add a single tuple to a relation.
  ```SQL
  INSERT INTO EMPLOYEE
  VALUES ( ‘Richard’,‘K’,‘Marini’,‘653298653’,‘1962-12-30’,‘98 Oak Forest, Katy, TX’,‘M’, 37000,‘653298653’, 4 );
  ```
  - A second form of the INSERT statement allows the user to specify explicit attribute names that correspond to the values provided in the INSERT command.
  - Attributes with NULL allowed or DEFAULT values are the ones that can be left out.
  ```SQL
  INSERT INTO EMPLOYEE (Fname, Lname, Dno, Ssn)
  VALUES (‘Richard’,‘Marini’, 4,‘653298653’);
  ```
  - Attributes not specified are set to their DEFAULT or to NULL, and the values are listed in the same order as the attributes are listed in the INSERT command itself.
  - It is also possible to insert into a relation multiple tuples separated by commas in a single INSERT command. The attribute values forming each tuple are enclosed in parentheses.
  - A DBMS that fully implements SQL should support and enforce all the integrity constraints that can be specified in the DDL.
  ```SQL
  INSERT INTO EMPLOYEE (Fname, Lname, Ssn, Dno)
  VALUES (‘Robert’,‘Hatcher’,‘980760540’, 2);
  (This is rejected if referential integrity checking is provided by DBMS.)
  ```
  ```SQL
  INSERT INTO EMPLOYEE (Fname, Lname, Dno)
  VALUES (‘Robert’,‘Hatcher’, 5);
  (This is rejected if NOT NULL checking is provided by DBMS.)
  ```
  - A variation of the INSERT command inserts multiple tuples into a relation in conjunction with creating the relation and loading it with the result of a query.
  ```SQL
  CREATE TABLE WORKS_ON_INFO
  (Emp_name VARCHAR(15),
  Proj_name VARCHAR(15),
  Hours_per_week DECIMAL(3,1) );
  ```
  - Notice that the WORKS_ON_INFO table may not be up-to-date; that is, if we update any of the PROJECT, WORKS_ON, or EMPLOYEE relations after issuing U3B,the information in WORKS_ON_INFO may become outdated:
  ```SQL
  INSERT INTO WORKS_ON_INFO ( Emp_name, Proj_name, Hours_per_week )
  SELECT E.Lname, P.Pname, W.Hours
  FROM PROJECT P, WORKS_ON W, EMPLOYEE E
  WHERE P.Pnumber = W.Pno AND W.Essn = E.Ssn;
  ```

Chapter 4.4.2 The DELETE Command
  - The DELETE command removes tuples from a relation.
  - Tuples are explicitly deleted from only one table at a time. However, the deletion may propagate to tuples in other relations if referential triggered actions are specified in the referential integrity constraints of the DDL.
  - A missing WHERE clause specifies that all tuples in the relation are to be deleted; however, the table remains in the database as an empty table.
  - DROP TABLE command to remove the table definition
  ```SQL
  DELETE FROM EMPLOYEE
  WHERE Lname=‘Brown’;

  DELETE FROM EMPLOYEE
  WHERE Ssn=‘123456789’;

  DELETE FROM EMPLOYEE
  WHERE Dno=5;

  DELETE FROM EMPLOYEE;
  ```

Chapter 4.4.3 The UPDATE Command
  - The UPDATE command is used to modify attribute values of one or more selected tuples.
  - Updating a primary key value may propagate to the foreign key values of tuples in other relations if such a referential triggered action is specified in the referential integrity constraints of the DDL
  - SET clause in the UPDATE command specifies the attributes to be modified and their new values
  ```SQL
  UPDATE PROJECT
  SET Plocation = ‘Bellaire’, Dnum = 5
  WHERE Pnumber = 10;
  ```
  - Several tuples can be modified with a single UPDATE command.
  ```SQL
  UPDATE EMPLOYEE
  SET Salary = Salary * 1.1
  WHERE Dno = 5;
  ```
  - To modify multiple relations, we must issue several UPDATE commands.

Chapter 5.1 More Complex SQL Retrieval Queries

Chapter 5.1.1 Comparisons Involving NULL and Three-Valued Logic
  - NULL is used to represent a missing value, but that it usually has one of three different interpretations unknown, not available or not applicable:
    1. Unknown value. A person’s date of birth is not known, so it is represented by NULL in the database.
    2. Unavailable or withheld value. A person has a home phone but does not want it to be listed, so it is withheld and represented as NULL in the database.
    3. Not applicable attribute. An attribute LastCollegeDegree would be NULL for a person who has no college degrees because it does not apply to that person.
  - It is often not possible to determine which of the meanings is intended
  - In general, each individual NULL value is considered to be different from every other NULL value in the various database records.
  - When a NULL is involved in a comparison operation, the result is considered to be UNKNOWN
  - SQL uses a three-valued logic with values TRUE, FALSE, and UNKNOWN instead of the standard two-valued (Boolean) logic with values TRUE or FALSE.
  - Notice that in standard Boolean logic, only TRUE or FALSE values are permitted; there is no UNKNOWN value.
  - In select-project-join queries, the general rule is that only those combinations of tuples that evaluate the logical expression in the WHERE clause of the query to TRUE are selected.  However, there are exceptions to that rule for certain operations, such as outer joins
  - Rather than using = or <> to compare an attribute value to NULL,SQL uses the comparison operators IS or IS NOT.
  - SQL considers each NULL value as being distinct from every other NULL value, so equality comparison is not appropriate. It follows that when a join condition is specified, tuples with NULL values for the join attributes are not included in the result.  
  - Ex: Retrieve the names of all employees who do not have supervisors.
  ```SQL
  SELECT Fname, Lname
  FROM EMPLOYEE
  WHERE Super_ssn IS NULL;
  ```

Chapter 5.1.2 Nested Queries, Tuples, and Set/Multiset Comparisons
  - Nested queries, which are complete select-from-where blocks within the WHERE clause of another query.
  - That other query is called the outer query.
  - IN, which compares a value v with a set (or multiset) of values V and evaluates to TRUE if v is one of the elements in V.
  - If a nested query returns a single attribute and a single tuple, the query result will be a single (scalar) value. In such cases, it is permissible to use = instead of IN for the comparison operator. In general, the nested query will return a table (relation), which is a set or multiset of tuples.
  - The = ANY (or = SOME) operator returns TRUE if the value v is equal to some value in the set V and is hence equivalent to IN.
  - Other operators that can be combined with ANY (or SOME) include >, >=, <, <=, and <>. The keyword ALL can also be combined with each of these operators.
  ```SQL
  SELECT Lname, Fname
  FROM EMPLOYEE
  WHERE Salary > ALL ( SELECT Salary
                       FROM EMPLOYEE
                       WHERE Dno = 5 );
  ```
  - In general, we can have several levels of nested queries.
  - It is generally advisable to create tuple variables (aliases) for all the tables referenced in an SQL query to avoid potential errors and ambiguities.

Chapter 5.1.3 Correlated Nested Queries
  - Whenever a condition in the WHERE clause of a nested query references some attribute of a relation declared in the outer query, the two queries are said to be correlated.
  - In general, a query written with nested select-from-where blocks and using the = or IN comparison operators can always be expressed as a single block query.

Chapter 5.1.4 The EXISTS and UNIQUE Functions in SQL
  - The EXISTS function in SQL is used to check whether the result of a correlated nested query is empty (contains no tuples) or not. The result of EXISTS is a Boolean value TRUE if the nested query result contains at least one tuple, or FALSE if the nested query result contains no tuples.
  ```SQL
  SELECT E.Fname, E.Lname
  FROM EMPLOYEE AS E
  WHERE EXISTS ( SELECT *
                 FROM DEPENDENT AS D
                 WHERE E.Ssn=D.Essn
                       AND E.Sex=D.Sex
                       AND E.Fname=D.Dependent_name
               );
  ```
  - EXISTS and NOT EXISTS are typically used in conjunction with a correlated nested query.
  - In general, EXISTS(Q) returns TRUE if there is at least one tuple in the result of the nested query Q,and it returns FALSE otherwise.
  - On the other hand, NOT EXISTS(Q) returns TRUE if there are no tuples in the result of nested query Q, and it returns FALSE otherwise.
  - Ex: Retrieve the names of employees who have no dependents.
  ```SQL
  SELECT Fname, Lname
  FROM EMPLOYEE
  WHERE NOT EXISTS ( SELECT *
                     FROM DEPENDENT
                     WHERE Ssn=Essn );
  ```
  - Ex: List the names of managers who have at least one dependent.
  ```SQL
  SELECT Fname, Lname
  FROM EMPLOYEE
  WHERE EXISTS ( SELECT *
                 FROM DEPENDENT
                 WHERE Ssn=Essn )
        AND
        EXISTS ( SELECT *
                 FROM DEPARTMENT
                 WHERE Ssn=Mgr_ssn );
  ```
  - There is another SQL function, UNIQUE(Q), which returns TRUE if there are no duplicate tuples in the result of query Q; otherwise, it returns FALSE.

Chapter 5.1.5 Explicit Sets and Renaming of Attributes in SQL
  - It is also possible to use an explicit set of values in the WHERE clause, rather than a nested query.
  - Ex: Retrieve the Social Security numbers of all employees who work on project numbers 1,2,or 3.
  ```SQL
  SELECT DISTINCT Essn
  FROM WORKS_ON
  WHERE Pno IN (1, 2, 3);
  ```
  - In SQL, it is possible to rename any attribute that appears in the result of a query by adding the qualifier AS followed by the desired new name.

Chapter 5.1.6 Joined Tables in SQL and Outer Joins
  - The concept of a joined table (or joined relation) was incorporated into SQL to permit users to specify a table resulting from a join operation in the FROM clause of a query.
  ```SQL
  SELECT Fname, Lname, Address
  FROM (EMPLOYEE JOIN DEPARTMENT ON Dno=Dnumber)
  WHERE Dname=‘Research’;
  ```
  - The concept of a joined table also allows the user to specify different types of join, such as NATURAL JOIN and various types of OUTER JOIN.
  - In a NATURAL JOIN on two relations R and S, no join condition is specified; an implicit EQUIJOIN condition for each pair of attributes with the same name from R and S is created.
  - If the names of the join attributes are not the same in the base relations, it is possible to rename the attributes so that they match, and then to apply NATURAL JOIN.
  - The default type of join in a joined table is called an inner join, where a tuple is included in the result only if a matching tuple exists in the other relation.
  - In SQL, the options available for specifying joined tables include INNER JOIN (only pairs of tuples that match the join condition are retrieved, same as JOIN),LEFT OUTER JOIN (every tuple in the left table must appear in the result; if it does not have a matching tuple, it is padded with NULL values for the attributes of the right table), RIGHT OUTER JOIN (every tuple in the right table must appear in the result; if it does not have a matching tuple, it is padded with NULL values for the attributes of the left table), and FULL OUTER JOIN.
  - The keyword OUTER may be omitted.
  - If the join attributes have the same name, one can also specify the natural join variation of outer joins by using the keyword NATURAL before the operation
  - The keyword CROSS JOIN is used to specify the CARTESIAN PRODUCT operation
  - It is also possible to nest join specifications; that is, one of the tables in a join may itself be a joined table.
  - The specification of the join of three or more tables as a single joined table, which is called a multiway join.
  ```SQL
  SELECT Pnumber, Dnum, Lname, Address, Bdate
  FROM ((PROJECT JOIN DEPARTMENT ON Dnum=Dnumber)
       JOIN EMPLOYEE ON Mgr_ssn=Ssn)
  WHERE Plocation=‘Stafford’;
  ```
  - In some systems, a different syntax was used to specify outer joins by using the comparison operators +=, =+, and +=+ for left, right, and full outer join, respectively, when specifying the join condition.
  - In Oracle:
  ```SQL
  SELECT E.Lname, S.Lname
  FROM EMPLOYEE E, EMPLOYEE S
  WHERE E.Super_ssn += S.Ssn;
  ```

Chapter 5.1.7 Aggregate Functions in SQL
  - Aggregate functions are used to summarize information from multiple tuples into a single-tuple summary.
  - Grouping is used to create subgroups of tuples before summarization.
  - A number of built-in aggregate functions exist: COUNT, SUM, MAX, MIN, and AVG.
  - The functions MAX and MIN can also be used with attributes that have nonnumeric domains if the domain values have a total ordering among one another. Total order means that for any two values in the domain, it can be determined that one appears before the other in the defined order; for example, DATE, TIME, and TIMESTAMP domains have total orderings on their values, as do alphabetic strings.
  - Ex: Find the sum of the salaries of all employees, the maximum salary, the minimum salary, and the average salary.
  ```SQL
  SELECT SUM (Salary), MAX (Salary), MIN (Salary), AVG (Salary)
  FROM EMPLOYEE;
  ```
  - Ex: Find the sum of the salaries of all employees of the ‘Research’ department, as well as the maximum salary, the minimum salary, and the average salary in this department.
  ```SQL
  SELECT SUM (Salary), MAX (Salary), MIN (Salary), AVG (Salary)
  FROM (EMPLOYEE JOIN DEPARTMENT ON Dno=Dnumber)
  WHERE Dname=‘Research’;
  ```
  - Ex: Retrieve the total number of employees in the company (Q21) and the number of employees in the ‘Research’ department (Q22).
  ```SQL
  Q21: SELECT COUNT (*)
       FROM EMPLOYEE;
  Q22: SELECT COUNT (*)
       FROM EMPLOYEE, DEPARTMENT
       WHERE DNO=DNUMBER AND DNAME=‘Research’;
  ```
  - Ex: Count the number of distinct salary values in the database.
  ```SQL
  SELECT COUNT (DISTINCT Salary)
  FROM EMPLOYEE;
  ```
  - In general, NULL values are discarded when aggregate functions are applied to a particular column (attribute).

Chapter 5.1.8 Grouping: The GROUP BY and HAVING Clauses
  - We want to apply the aggregate functions to subgroups of tuples in a relation, where the subgroups are based on some attribute values.
  - If NULLs exist in the grouping attribute, then a separate group is created for all tuples with a NULL value in the grouping attribute.
  - Ex: For each project, retrieve the project number, the project name, and the number of employees who work on that project.
  ```SQL
  SELECT Pnumber, Pname, COUNT (*)
  FROM PROJECT, WORKS_ON
  WHERE Pnumber=Pno
  GROUP BY Pnumber, Pname;
  ```
  - HAVING provides a condition on the summary information regarding the group of tuples associated with each value of the grouping attributes.
  - Ex: For each project on which more than two employees work, retrieve the project number,the project name, and the number of employees who work on the project.
  ```SQL
  SELECT Pnumber, Pname, COUNT (*)
  FROM PROJECT, WORKS_ON
  WHERE Pnumber=Pno
  GROUP BY Pnumber, Pname
  HAVING COUNT (*)>2;
  ```
  - WHERE clause limit the tuples to which functions are applied, the HAVING clause serves to choose whole groups.
  - Ex: For each project, retrieve the project number, the project name, and the number of employees from department 5 who work on the project.
  ```SQL
  SELECT Pnumber, Pname, COUNT (*)
  FROM PROJECT, WORKS_ON, EMPLOYEE
  WHERE Pnumber=Pno AND Ssn=Essn AND Dno=5
  GROUP BY Pnumber, Pname;
  ```
  - The rule is that the WHERE clause is executed first, to select individual tuples or joined tuples; the HAVING clause is applied later, to select individual groups of tuples.
  - Ex: For each department that has more than five employees, retrieve the department number and the number of its employees who are making more than $40,000.
  ```SQL
   SELECT Dnumber, COUNT (*)
   FROM DEPARTMENT, EMPLOYEE
   WHERE Dnumber=Dno AND Salary>40000
         AND (SELECT Dno
              FROM EMPLOYEE
              GROUP BY Dno
              HAVING COUNT (*) > 5)
  ```

Chapter 5.1.9 Discussion and Summary of SQL Queries
  - A retrieval query in SQL can consist of up to six clauses, but only the first two— SELECT and FROM—are mandatory.
  ```SQL
  SELECT <attribute and function list>
  FROM <table list>
  [ WHERE <condition> ]
  [ GROUP BY <grouping attribute(s)> ]
  [ HAVING <group condition> ]
  [ ORDER BY <attribute list> ];
  ```
  - HAVING specifies a condition on the groups being selected rather than on the individual tuples.
  - The built-in aggregate functions COUNT, SUM, MIN, MAX, and AVG are used in conjunction with grouping
  - ORDER BY specifies an order for displaying the result of a query.
  - A query is evaluated conceptually by first applying the FROM clause (to identify all tables involved in the query or to materialize any joined tables), followed by the WHERE clause to select and join tuples, and then by GROUP BY and HAVING.
  - Conceptually, ORDER BY is applied at the end to sort the query result.
  - Each DBMS has special query optimization routines to decide on an execution plan that is efficient to execute.
  - From the programmer’s and the system’s point of view regarding query optimization, it is generally preferable to write a query with as little nesting and implied ordering as possible.
  - The DBMS should process the same query in the same way regardless of how the query is specified. But this is quite difficult in practice, since each DBMS has different methods for processing queries specified in different ways.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
XPATH AND XQUERY NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 23.4.1 Tree Model of XML
  - The terms parent, child, ancestor, descendant, and siblings are used in the tree model of XML data.

Chapter 23.4.2 XPath
  - XPath addresses parts of an XML document by means of path expressions.
  - Can be viewed as an extension of the simple path expressions in object oriented and object-relational databases
  - A path expression in XPath is a sequence of location steps separated by “/”
  - Path expressions are evaluated from left to right.
  - Like a directory hierarchy, the initial ’/’ indicates the root of the document
  - The result of a path expression is then the set of nodes after the last step of path expression evaluation.
  - The nodes returned by each step appear in the same order as their appearance in the document.
  - Attribute values may also be accessed, using the “@” symbol.
  - For instance, /university-3/course/@course_id returns a set of all values of course id attributes of course elements.
  - By default, IDREF links are not followed.
  - XPath supports a number of other features:
    1. Selection predicates : ``` /university-3/course[credits >=4] ``` returns course elements with a credits value greater than or equal to 4.
    2. XPath provides several functions that can be used as part of predicates, including testing the position of the current node in the sibling order and the aggregate function count(), which counts the number of nodes matched by the expression to which it is applied. ``` /university-2/instructor[count(./teaches/course)> 2]``` returns instructors who teach more than two courses.  Boolean connectives AND and OR can be used in predicates, while the function not( ... ) can be used for negation.
    3. The function id(“foo”) returns the node (if any) with an attribute of type ID and value “foo”. ``` /university-3/course/id(@dept name)``` returns all department elements referred to from the dept_name attribute of course elements, while: ```XPATH /university-3/course/id(@instructors)``` returns the instructor elements referred to in the instructor attribute of course elements.
    4. The | operator allows expression results to be unioned. ``` /university-3/course[@dept name=“Comp. Sci”] | /university-3/course[@dept name=“Biology”] ``` : union of Computer Science and Biology courses. The | operator cannot be nested inside other operators.
    5. An XPath expression can skip multiple levels of nodes by using “//”. For instance, the expression ``` /university-3//name``` ﬁnds all name elements anywhere under the /university-3 element, regardless of the elements in which they are contained, and regardless of how many levels of enclosing elements are present between the university-3 and name elements.
    6. A step in the path need not just select from the children of the nodes in the current node set. “//”, described above, is a short form for specifying “all descendants,” while “..” speciﬁes the parent.
    7. The built-in function doc(name) returns the root of a named document; the name could be a ﬁle name or a URL. For example, if the university data in our university example is contained in a ﬁle “university.xml”, the following path expression would return all departments at the university: ``` doc(“university.xml”)/university/department``` The function collection(name) is similar to doc, but returns a collection of documents identiﬁed by name.

Chapter 23.4.3 XQuery
  - The World Wide Web Consortium (W3C) has developed XQuery as the standard query language for XML.

Chapter 23.4.3.1 FLWOR Expressions
  - XQuery queries are modeled after SQL queries, but differ signiﬁcantly from SQL.
  - They are organized into ﬁve sections: for, let, where, order by, and return : FLWOR (pronounced “ﬂower”)
  ```XQUERY
  for $x in /university-3/course
  let $courseId := $x/@course_id
  where $x/credits > 3
  return <course_id> {$courseId} </course_id>
  ```
  - The for clause is like the from clause of SQL, and speciﬁes variables that range over the results of XPath expressions. When more than one variable is speciﬁed, the results include the Cartesian product of the possible values the variables can take, just as the SQL from clause does.
  - The let clause simply allows the results of XPath expressions to be assigned to variable names for simplicity of representation
  - The where clause, like the SQL where clause, performs additional tests on the joined tuples from the for clause.
  - The order by clause, like the SQL order by clause, allows sorting of the output.
  - The return clause allows the construction of results in XML.
  - A FLWOR query need not contain all the clauses
  - Similar Query:
  ```XQUERY
  for $x in /university-3/course[credits > 3]
  return <course id> {$x/@course id} </course id>
  ```
  - Observe the use of curly brackets (“{}”) in the return clause. When XQuery ﬁnds an element such as ```<course_id>``` starting an expression, it treats its contents as regular XML text, except for portions enclosed within curly brackets, which are evaluated as expressions.
  - XQuery provides another way of constructing elements using the element and attribute constructors.
  ```XQuery
  return element course{
      attribute course_id{$x/@course_id},
      attribute dept_name{$x/dept_name},
      element title{$x/title},
      element credits{$x/credits}
  }
  ```

Chapter 23.4.3.2 Joins
  - Joins are speciﬁed in XQuery much as they are in SQL
  ```XQUERY
  for $c in /university/course,
      $i in /university/instructor,
      $t in /university/teaches
  where $c/course_id= $t/course_id
      and $t/IID = $i/IID
  return <course_instructor> {$c $i} </course_instructor>
  ```
  - Same expression using selections specified as XPath selections:
  ```XQUERY
  for $c in /university/course,
      $i in /university/instructor,
      $t in /university/teaches[ $c/course_id= $t/course_id
            and $t/IID = $i/IID]
  return <course_instructor> {$c $i} </course_instructor>
  ```
  - XQuery has an interesting deﬁnition of comparison operations on sequences. For example, the expression ```$x/credits > 3``` would have the usual interpretation if the result of $x/credits is a single value, but if the result is a sequence containing multiple values, the expression evaluates to true if at least one of the values is greater than 3. If this behavior is not appropriate, the operators eq, ne, lt, gt, le, ge can be used instead. These raise an error if either of their inputs is a sequence with multiple values.

Chapter 23.4.3.3 Nested Queries
  - XQuery FLWOR expressions can be nested in the return clause, in order to generate element nestings that do not appear in the source document.
  ```XQUERY
  <university-1>
  {
    for $d in /university/department
    return
      <department>
        {$d/*}
        {for $c in /university/course[dept_name = $d/dept_name]
            return $c}
      < /department>
  }
  {
    for $i in /university/instructor
    return
      <instructor>
        {$i/*}
        {for $c in /university/teaches[IID = $i/IID]
            return $c/course_id}
      < /instructor>
  }
  </university-1>
  ```
  - The syntax $d/* , which refers to all the children of the node (or sequence of nodes) bound to the variable $d.
  - XQuery provides a variety of aggregate functions such as sum() and count() that can be applied on sequences of elements or values.
  - While XQuery does not provide a group by construct, aggregate queries can be written by using the aggregate functions on path or FLWOR expressions nested within the return clause.
  - For example, the following query on the university XML schema ﬁnds the total salary of all instructors in each department:
  ```XQUERY
  for $d in /university/department
  return
    <department-total-salary>
      <dept_name> {$d/dept_name} </dept_name>
      <total_salary> {fn:sum(
          for $i in /university/instructor[dept_name = $d/dept_name]
          return $i/salary
      )} </total salary>
    < /department-total-salary>
  ```

Chapter 23.4.3.4 Sorting of Results
  - Results can be sorted in XQuery by using the order by clause.
  ```XQUERY
  for $i in /university/instructor
  order by $i/name
  return <instructor> {$i/*} </instructor>
  ```
  - Sorting can be done at multiple levels of nesting.
  ```XQUERY
  <university-1> {
    for $d in /university/department
    order by $d/dept_name
    return
      <department>
        {$d/*}
        {for $c in /university/course[dept_name = $d/dept_name]
        order by $c/course_id
        return <course> {$c/*} </course> }
      < /department> }
  <university-1>
  ```

Chapter 23.4.3.5 Functions and Types
  - XQuery provides a variety of built-in functions, such as numeric functions and string matching and manipulation functions.
  - XQuery supports user deﬁned functions.
  - The following user-deﬁned function takes as input an instructor identiﬁer, and returns a list of all courses offered by the department to which the instructor belongs:
  ```XQUERY
  declare function local:dept courses($iid as xs:string) as element(course)*{
     for $i in /university/instructor[IID = $iid],
        $c in /university/courses[dept_name = $i/dept_name]
     return $c
  }
  ```
  - The namespace preﬁx xs: used in the above example is predeﬁned by XQuery to be associated with the XML Schema namespace, while the namespace local: is predeﬁned to be associated with XQuery local functions.
  - XQuery performs type conversion automatically whenever required.
  - XQuery also provides functions to convert between types

Chapter 23.4.3.6 Other Features
  - XQuery offers a variety of other features, such as if-then-else constructs that can be used within return clauses, and existential and universal quantiﬁcation that can be used in predicates in where clauses.
  ```XQUERY
  some $e in path satisﬁes P
  ```
  - where path is a path expression and P is a predicate that can use $e.
  - Universal quantiﬁcation can be expressed by using every in place of some.
  - The query also introduces the syntax $d/* , which refers to all the children of the node (or sequence of nodes) bound to the variable $d.
  - Similarly, $d/text() gives the text content of an element, without the tags.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
XSLT NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
  - NONE

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
RELATIONAL DESIGN THEORY NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 8.1 Features of Good Relational Designs

Chapter 8.1.1 Design Alternative: Larger Schemas
  - Suppose that instead of having the schemas instructor and department, we have the schema:
  ```
  inst_dept (ID, name, salary, dept_name, building, budget)
  ```
  - inst_dept is a bad idea since it stores the budget amounts redundantly and runs the risk that some user might update the budget amount in one tuple but not all, and thus create inconsistency.
  - In the alternative design above, we cannot represent directly the information concerning a department(dept_name, building, budget) unless that department has at least one instructor at the university.

Chapter 8.1.2 Design Alternative: Smaller Schemas
  - By observing the contents of actual relations on schema inst_dept, we could note the repetition of information resulting from having to list the building and budget once for each instructor associated with a department. However, this is an unreliable process.
  - In other words, we need to write a rule that says “if there were a schema(dept_name, budget), then dept_name is able to serve as the primary key.” This rule is speciﬁed as a functional dependency ```dept_name → budget```
  - Observations such as these and the rules (functional dependencies in particular) that result from them allow the database designer to recognize situations where a schema ought to be split, or decomposed, into two or more schemas. It is not hard to see that the right way to decompose inst_dept is into schemas instructor and department as in the original design.
  - Not all decompositions of schemas are helpful. Consider an extreme case where all we had were schemas consisting of one attribute. No interesting relationships of any kind could be expressed.
  - Consider a less extreme case where we choose to decompose the employee schema ```employee (ID, name, street, city, salary)``` into the following two schemas ``` employee1 (ID, name) ``` and ```employee2 (name, street, city, salary)```
  - The flaw in the above decomposition arises from the possibility that the enterprise has two employees with the same name.
  - Example:
    ```
    (57766, Kim, Main, Perryridge, 75000)
    (98776, Kim, North, Hampton, 67000)
    ```
  - If we attempted to regenerate the original tuples using a natural join, the two original tuples appear in the result along with two new tuples that incorrectly mix data values pertaining to the two employees named Kim.
  - We shall refer to such decompositions as being lossy decompositions, and, conversely, to those that are not as lossless decompositions.

Chapter 8.3 Decomposition Using Functional Dependencies
  -  In general, we use Greek letters for sets of attributes (for example, ). We use a lowercase Roman letter followed by an uppercase Roman letter in parentheses to refer to a relation schema (for example, r(R)). We use the notation r(R) to show that the schema is for relation r, with R denoting the set of attributes, but at times simplify our notation to use just R when the relation name does not matter to us. Of course, a relation schema is a set of attributes, but not all sets of attributes are schemas. When we use a lowercase Greek letter, we are referring to a set of attributes that may or may not be a schema. A Roman letter is used when we wish to indicate that the set of attributes is deﬁnitely a schema.
  - When a set of attributes is a superkey, we denote it by K. A superkey pertains to a speciﬁc relation schema, so we use the terminology “K is a superkey of r(R).”
  - We use a lowercase name for relations. In our examples, these names are intended to be realistic(for example, instructor), while in our deﬁnitions and algorithms, we use single letters, like r.
  - A relation, of course, has a particular value at any given time; we refer to that as an instance and use the term “instance of r”. When it is clear that we are talking about an instance, we may uses imply the relation name (for example, r).

Chapter 8.3.1 Keys and Functional Dependencies
  - A database models a set of entities and relationships in the real world. There are usually a variety of constraints(rules) on the data in the real world. For example, some of the constraints that are expected to hold in a university database are:
    1. Students and instructors are uniquely identiﬁed by their ID.
    2. Each student and instructor has only one name.
    3. Each instructor and student is (primarily) associated with only one department.
    4. Each department has only one value for its budget, and only one associated building.
  - An instance of a relation that satisﬁes all such real-world constraints is called a legal instance of the relation; a legal instance of a database is one where all the relation instances are legal instances.
  - We deﬁned the notion of a superkey as a set of one or more attributes that, taken collectively, allows us to identify uniquely a tuple in the relation. We restate that deﬁnition here as follows: Let r (R) be a relation schema. A subset K of R is a superkey of r(R) if, in any legal instance of r (R), for all pairs t1 and t2 of tuples in the instance of r if t1 /= t2, thent1[K] /= t2[K]. That is, no two tuples in any legal instance of relation r(R) may have the same value on attribute k.
  - Whereas a superkey is a set of attributes that uniquely identiﬁes an entire tuple, a functional dependency allows us to express constraints that uniquely identify the values of certain attributes.
  - Consider a relation schema r(R), and let alpha ⊆ R and beta ⊆ R.
    - Given an instance of r(R), we say that the instance satisﬁes the functional dependency alpha → beta if for all pairs of tuplest1 and t2 in the instance such that t1[alpha] = t2[beta], it is also the case that t1[alpha] = t2[beta].
    - We say that the functional dependency alpha → beta holds on schema r(R) if, in every legal instance of r (R) it satisﬁes the functional dependency.
  - Using the functional-dependency notation, we say that K is a superkey of r(R)if the functional dependency K → R holds on r(R). In other words, K is a superkey if, for every legal instance of r(R), for every pair of tuples t1 and t2 from the instance, whenever t1[K] = t2[K], it is also the case that t1[R] = t2[R]
  - Functional dependencies allow us to express constraints that we cannot express with superkeys.
  - ``` inst_dept (ID, name, salary, dept_name, building, budget) ```
    - We denote the fact that the pair of attributes(ID, dept_name) forms a superkey for inst_dept by writing:
    - ``` ID, dept_name → name, salary, building, budget  ```
  - We shall use functional dependencies in two ways:
    1. To test instances of relations to see whether they satisfy a given set F of functional dependencies.
    2. To specify constraints on the set of legal relations. We shall thus concern ourselves with only those relation instances that satisfy a given set of functional dependencies. If we wish to constrain ourselves to relations on schema r(R) that satisfy a set F of functional dependencies, we say that F holds on r(R).
  - Some functional dependencies are said to be trivial because they are satisﬁed by all relations. For example, A → A is satisﬁed by all relations involving attribute A.
  - Similarly, AB → A is satisﬁed by all relations involving attribute A. In general, a functional dependency of the form alpha → beta is trivial if beta ⊆ alpha.
  - In the instance of the classroom relation we see that ```room_number → capacity``` is satisﬁed. However, we believe that, in the real world, two classrooms in different buildings can have the same room number but with different room capacity. Thus, it is possible, at some time, to have an instance of the classroom relation in which ```room_number → capacity``` is not satisﬁed. So, we would not include ```room_number → capacity ``` in the set of functional dependencies that hold on the schema for the classroom relation. However, we would expect the functional dependency building, ```room_number → capacity``` to hold on the classroom schema.
  - We will use the notation F+ to denote the closure of the set F, that is, the set of all functional dependencies that can be inferred given the set F. Clearly F+ contains all of the functional dependencies in F.

Chapter 8.3.2 Boyce-Codd Normal Form
  - One of the more desirable normal forms that we can obtain is Boyce–Codd normal form (BCNF). It eliminates all redundancy that can be discovered based on functional dependencies.
  - A relation schema R is in BCNF with respect to a set F of functional dependencies if, for all functional dependencies in F+ of the form alpha → beta, where alpha ⊆ R and beta ⊆ R, at least one of the following holds:
    - alpha → beta is a trivial functional dependency(that is, alpha ⊆ beta).
    - alpha is a superkey for schema R.
  - We now state a general rule for decomposing that are not in BCNF. Let R be a schema that is not in BCNF. Then there is at least one nontrivial functional dependency alpha → beta such that alpha is not a superkey for R. We replace R in our design with two schemas:
    - (alpha U beta)
    - (R - (alpha - beta))
  - In the case of inst_dept above, alpha = dept_name, beta = {building, budget}, and inst_dept is replaced by
    - (alpha U beta) = (dept_name, building, budget)
    - (R - (alpha - beta)) = (ID, name, dept_name, salary)

Chapter 8.3.3 BCNF and Dependency Preservation
  - We have seen several ways in which to express database consistency constraints: primary-key constraints, functional dependencies, check constraints, assertions, and triggers. Testing these constraints each time the database is updated can be costly and, therefore, it is useful to design the database in a way that constraints can be tested efﬁciently. In particular, if testing a functional dependency can be done by considering just one relation, then the cost of testing this constraint is low.
  - Any schema with only two attributes is in BCNF by deﬁnition
  - Because dependency preservation is usually considered desirable, we consider another normal form, weaker than BCNF, that will allow us to preserve dependencies. That normal form is called third normal form.

Chapter 8.3.4 Third Normal Form
  - A relation schema R is in third normal form with respect to a set F of functional dependencies if, for all functional dependencies in F+ of the form alpha → beta, where alpha ⊆ R and beta ⊆ R, at least one of the following holds:
    - alpha → beta is a trivial functional dependency
    - alpha is a superkey for R.
    - Each attribute A in alpha → beta is contained in a candidate key for R.
  - Example:
    - i_ID → dept_name
    - s_ID, dept_name→ i_ID
    - Since the functional dependency s_ID, dept_name → i_ID holds on dept_advisor, the attribute dept_name is contained in a candidate key and, therefore, dept_advisor is in 3NF.

Chapter 8.3.5 Higher Normal Forms
  - Using functional dependencies to decompose schemas may not be sufﬁcient to avoid unnecessary repetition of information in certain cases.

Chapter 8.5.1 BCNF Decomposition   
  - If a relation is not in BCNF, it can be decomposed to create relations that are in BCNF.

Chapter 8.5.1.1 Testing for BCNF
  - Testing of a relation schema R to see if it satisﬁes BCNF can be simplified in some cases:
    - To check if a nontrivial dependency alpha → beta causes a violation of BCNF, compute alpha+ (the attribute closure of alpha), and verify that it includes all attributes of R; that is, it is a superkey of R.
    - To check if a relation schema R is in BCNF, it sufﬁces to check only the dependencies in the given set F for violation of BCNF, rather than check all dependencies in F+. We can show that if none of the dependencies in F causes a violation of BCNF, then none of the dependencies in F+ will cause a violation of BCNF, either.
  - Unfortunately, the latter procedure does not work when a relation is decomposed. That is, it does not suffice to use F when we test a relation Ri, in a decomposition of R, for violation of BCNF
  - Consider relation schema R(A, B,C, D, E), with functional dependencies F containing A → B and BC → D. Suppose this were decomposed into R1(A, B) and R2(A,C, D, E). Now, neither of the dependencies in F contains only attributes from (A,C, D, E) so we might be misled into thinking R2 satisﬁes BCNF. In fact, there is a dependency AC → D in F+ that shows R2 is not in BCNF.
  - To check if a relation Ri in a decomposition of R is in BCNF, we apply this test:
    - For every subset  of attributes in Ri, check that alpha+ (the attribute closure of alpha under F) either includes no attribute of Ri − alpha, or includes all attributes of Ri.
  - BCNF decomposition algorithm:
  ```
  result := {R};
  done := false;
  compute F+;
  while (not done) do
    if (there is a schema Ri in result that is not in BCNF)
      then begin
        let alpha → beta be a nontrivial functional dependency that holds on Ri such that alpha → Ri is not in F+, and alpha ∩ beta =∅;
        result :=(result− Ri) ∪ (Ri −beta) ∪ (alpha,beta);
    end
    else done := true;   
  ```
  - alpha → (alpha+ - alpha) ∩ Ri.
  - The above dependencyshows that Ri violates BCNF.

Chapter 8.5.1.2 BCNF Decomposition Algorithm
  -  If R is not in BCNF, we can decompose R into a collection of BCNF schemas R1, R2,...,Rn by the algorithm. The algorithm uses dependencies that demonstrate violation of BCNF to perform the decomposition.
  - To see why our algorithm generates only lossless decompositions, we note that, when we replace a schema Ri with (Ri − beta) and (alpha,beta), the dependency alpha → beta holds, and (Ri − beta) ∩ (alpha, beta) = alpha. If we did not require alpha ∩ beta =∅, then those attributes in alpha ∩ beta would not appear in the schema(Ri − beta) and the dependency alpha → beta would no longer hold.
  - Example:
    - As a longer example of the use of the BCNF decomposition algorithm, suppose we have a database design using the class schema below:
      ```
      class (course_id, title, dept_name, credits, sec_id, semester, year, building, room_number, capacity, time_slot_id)
      ```
    - The set of functional dependencies that we require to hold on class are:
      ```
      course_id → title, dept_name, credits
      building, room_number → capacity
      course_id, sec_id, semester, year → building, room_number, time_slot_id
      ```
    - A candidate key for this schema is{course_id, sec_id, semester, year}. We can apply the algorithm of the class example as follows:
      - The functional dependency:
      ```
      course_id → title, dept_name, credits
      ```
      - holds, but course_id is not a superkey. Thus, class is not in BCNF. We replace class by:
      ```
      course(course_id, title, dept_name, credits)
      class-1 (course_id, sec_id, semester, year, building, room_number, capacity, time_slot_id)
      ```
      - The only non trivial functional dependencies that hold on course include course_id on the left side of the arrow. Since course_id is a key for course, the relation course is in BCNF.
      - A candidate key for class-1 is{course_id, sec_id, semester, year}. The functional dependency:
      ```
      building, room_number → capacity
      ```
      - holds on class-1, but{building, room_number}is not a superkey for class-1. We replace class-1 by:
      ```
      classroom (building, room_number, capacity)
      section (course_id, sec_id, semester, year, building, room_number, time_slot_id)
      ```
      - classroom and section are in BCNF.
    - Thus, the decomposition of class results in the three relation schemas course, classroom, and section, each of which is in BCNF.

Chapter 8.5.2 3NF Decomposition

Chapter 8.6 Decomposition Using Multivalued Dependencies
  - Some relation schemas, even though they are in BCNF, do not seem to be sufficiently normalized, in the sense that they still suffer from the problem of repetition of information. Consider a variation of the university organization where an instructor may be associated with multiple departments.
  ```
  inst (ID, dept_name, name, street, city)
  ```
  - The astute reader will recognize this schema as a non-BCNF schema because of the functional dependency
  ```
  ID → name, street, city
  ```
  - and because ID is not a key for inst.
  -  This normal form, called fourth normal form (4NF), is more restrictive than BCNF. We shall see that every 4NF schema is also in BCNF but there are BCNF schemas that are not in 4NF.

Chapter 8.6.1 Multivalued Dependencies
  - Multivalued dependencies do not rule out the existence of certain tuples. Instead, they require that other tuples of a certain form be present in the relation
  - Functional dependencies sometimes are referred to as equality-generating dependencies, and multivalued dependencies are referred to as tuple-generating dependencies.
  - Let r(R) be a relation schema and let alpha ⊆ R and beta ⊆ R. The multivalued dependency alpha →→ beta holds on R if, in any legal instance of relation r(R), for all pairs of tuples t1 and t2 in r such that t1[alpha]=t2[alpha], there exist tuples t3 and t4 in r such that:
  ```
  t1[alpha] = t2[alpha] = t3[alpha] = t4[alpha]
  t3[beta] = t1[beta]
  t3[R − beta] = t2[R − beta]
  t4[beta] = t2[beta]
  t4[R − beta] = t1[R − beta]
  ```
  - Intuitively, the multivalued dependency alpha →→ beta says that the relationship between alpha and beta is independent of the relationship between alpha and R−beta.
  - If the multivalued dependency alpha →→ beta is satisﬁed by all relations on schema R, then alpha →→ beta is a trivial multivalued dependency on schema R. Thus, alpha →→ beta is trivial if beta ⊆ alpha or beta U alpha = R.
  - As with functional dependencies, we shall use multivalued dependencies in two ways:
    1. To test relations to determine whether they are legal under a given set of functional and multivalued dependencies
    2. To specify constraints on the set of legal relations; we shall thus concern ourselves with only those relations that satisfy a given set of functional and multivalued dependencies
  - Let D denote a set of functional and multivalued dependencies. The closure D+ of D is the set of all functional and multivalued dependencies logically implied by D.
  - As we did for functional dependencies, we can compute D+ from D, using the formal deﬁnitions of functional dependencies and multivalued dependencies.
  - From the deﬁnition of multivalued dependency, we can derive the following rules for alpha, beta ⊆ R:
    - If alpha → beta, then alpha →→ beta. In other words, every functional dependency is also a multivalued dependency.
    -  If alpha →→ beta, then alpha →→ R − alpha − beta  

Chapter 8.6.2 Fourth Normal Form
  - Example: ```r2 (ID, dept_name, street, city)```
    - Although this schema is in BCNF, the design is not ideal, since we must repeat an instructor’s address information for each department.
  - A relation schema r(R) is in fourth normal form(4NF) with respect to a set D of functional and multivalued dependencies if, for all multivalued dependencies in D+ of the form alpha →→ beta, where alpha ⊆ R and beta ⊆ R, at least one of the following holds:
    - alpha →→ beta is a trivial multivalued dependency.
    - alpha is a superkey for R.
  - A database design is in 4NF if each member of the set of relation schemas that constitutes the design is in 4NF.
  - Note that the deﬁnition of 4NF differs from the deﬁnition of BCNF in only the use of multivalued dependencies. Every 4NF schema is in BCNF.

Chapter 8.6.3 4NF Decomposition
  - 4NF decomposition algorithm:
  ```
  result := {R};
  done := false;
  compute D+; Given schema Ri, let Di denote the restriction of D+ to Ri
  while (not done) do
    if (there is a schema Ri in result that is not in 4NF w.r.t. Di)
      then begin let alpha →→ beta be a nontrivial multivalued dependency that holds on Ri such that alpha → Ri is not in Di, and alpha ∩ beta = ∅ ;
      result := (result − Ri) ∪ (Ri − beta) ∪ (alpha, beta);
    end
    else done := true;
  ```
  - Let r(R) be a relation schema, and let D be a set of functional and multivalued dependencies on R. Let r1(R1) and r2(R2) form a decomposition of R. This decomposition is lossless of R if and only if at least one of the following multivalued dependencies is in D+:
    - R1 ∩ R2 →→ R1
    - R1 ∩ R2 →→ R2 

Chapter 8.8.4 Other Design Issues
  - There are some aspects of database design that are not addressed by normalization, and can thus lead to bad database design. Data pertaining to time or to ranges of time have several such issues.

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
