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
