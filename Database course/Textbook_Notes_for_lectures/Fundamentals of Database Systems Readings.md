-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
INTRODUCTION AND RELATIONAL DATABASES NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 1.1 Introduction
  - A database is a collection of related data
  - A random assortment of data cannot correctly be referred to as a database.
  - A database may be generated and maintained manually or it may be computerized.
  - A database management system (DBMS) is a collection of programs that enables users to create and maintain a database.
  - The DBMS must be able to maintain the database system by allowing the system to evolve as requirements change over time.

Chapter 1.2 An Example
  - A file is a collection of records that may or may not be ordered.

Chapter 1.3 Characteristics of the Database Approach
  - In the database approach, a single repository maintains data that is defined once and then accessed by various users.
  - In file systems, each application is free to name data elements independently.
  - In traditional file processing, each user defines and implements the files needed for a specific software application as part of programming the application.

Chapter 1.3.1 Self-Describing Nature of a Database System
  - The information stored in the catalog is called meta-data, and it describes the structure of the primary database
  - A fundamental characteristic of the database approach is that the database system contains not only the database itself but also a complete definition/description
    of the database structure and constraints.

Chapter 1.3.2 Insulation between Programs and Data and Data Abstraction
  - The characteristic that allows program-data independence and program-operation independence is called data abstraction
  - Conceptual representation of data does not include many of the details of how the data is stored or how the operations are implemented.
  - A data model is a type of data abstraction that is used to provide this conceptual representation.

Chapter 1.3.3 Support of Multiple Views of the data
  - A view may be a subset of the database or it may contain virtual data that is derived from the database files but is not explicitly stored.

Chapter 1.3.4 Sharing of Data and Multiuser Transaction Processing
  - A multiuser DBMS, must allow multiple users to access the database at the same time.
  - The DBMS must include concurrency control.
  - The isolation property ensures that each transaction appears to execute in isolation from other transactions
  - The atomicity property ensures that either all the database operations in a transaction are executed or none are.

Chapter 1.4 Actors on the Scene

Chapter 1.4.1 Database Administrators
  - Database administrator (DBA) is responsible for authorizing access to the database, coordinating and monitoring its use,
    and acquiring software and hardware resources as needed.

Chapter 1.4.2 Database Designers
  - Database designers are responsible for identifying the data to be stored in the database and for choosing appropriate structures to represent and store this data.

Chapter 1.4.3 End Users
  - End users are the people whose jobs require access to the database for querying
  - Types:
    1. Casual end users
    2. Naive or parametric end users
    3. Sophisticated end users
    4. Standalone users

Chapter 1.4.4 System Analysts and Application Programmers (Software Engineers)
  - System analysts determine the requirements of end users
  - Application programmers implement specifications as programs
  - Such analysts and programmers—commonly referred to as software developers or software engineers

Chapter 1.5 Workers behind the Scene
  - DBMS system designers and implementers : design and implement the DBMS modules and interfaces as a software package.
  - Tool developers : design and implement tools—the software packages that facilitate database modeling and design, database system design, and improved performance
  - System administration personnel : responsible for the actual running and maintenance of the hardware and software environment for the database system.

Chapter 1.6 Advantages of using the DBMS Approach

Chapter 1.6.1 Controlling Redundancy
  - Storage space is wasted when the same data is stored repeatedly
  - Duplication of effort
  - Files that represent the same data may become inconsistent
  - Data Normalization: Ideally, we should have a database design that stores each logical data item in one place
  - Data normalization ensures consistency and saves storage space
  - Sometimes necessary to use controlled redundancy to improve the performance of queries.
  - By placing all the data together, we do not have to search multiple files to collect this data. This is known as denormalization

Chapter 1.6.2 Restricting Unauthorized Access
  - A DBMS should provide a security and authorization subsystem

Chapter 1.6.3 Providing Persistent Storage for Program Objects
  - Databases can be used to provide persistent storage for program objects and data structures.

Chapter 1.6.4 Providing Storage Structures and Search Techniques for Efficient Query Processing
  - DBMS must provide specialized data structures and search techniques to speed up disk search for the desired records. Auxiliary files called indexes are
    used for this purpose.
  - The DBMS often has a buffering or caching module that maintains parts of the database in main memory buffers.
  - Most DBMSs do their own data buffering even though this is normally an OS operation.

Chapter 1.6.5 Providing Backup and Recovery
  - A DBMS must provide facilities for recovering from hardware or software failures.
  - The backup and recovery subsystem of the DBMS is responsible for recovery.

Chapter 1.6.6 Providing Multiple User Interfaces
  - Many specialized languages and environments exist for specifying GUIs.

Chapter 1.6.7 Representing Complex Relationships among Data
  - A DBMS must have the capability to represent a variety of complex relationships among the data

Chapter 1.6.8 Enforcing Integrity Constraints

Chapter 1.6.9 Permitting Inferencing and Actions Using Rules

Chapter 1.6.10 Additional Implications of Using the Database Approach
  - Potential for Enforcing Standards:
    - The DBA can enforce standards in a centralized database environment more easily than in an environment where each user group has control of its own data files and software.
  - Reduced Application Development Time
    -  Development time using a DBMS is estimated to be one-sixth to one-fourth of that for a traditional file system.
  - Flexibility
  - Availability of Up-to-Date Information
  - Economies of Scale

Chapter 1.7 A Brief History of Database Applications

Chapter 1.7.1 Early Database Applications Using Hierarchical and Network Systems
  - One of the main problems with early database systems was the intermixing of conceptual relationships with the physical storage and placement of records on disk.

Chapter 1.7.2 Providing Data Abstraction and Application Flexibility with Relational Databases
  - Relational databases were originally proposed to separate the physical storage of data from its conceptual representation

Chapter 1.7.3 Object-Oriented Applications and the Need for More Complex Databases

Chapter 1.7.4 Interchanging Data on the Web for E-Commerce Using XML
    - XML combines concepts from the models used in document systems with database modeling concepts
    - eXtended Markup Language (XML) is considered to be the primary standard for interchanging data among various types of databases and Web pages.

Chapter 1.7.5 Extending Database Capabilities for New Applications

Chapter 1.7.6 Databases versus Information Retrieval
  - IR is concerned with searching for material based on these keywords

Chapter 1.8 When Not to Use a DBMS
  - It may be more desirable to use regular files when:
    1. Simple, well-defined database applications that are not expected to change at all
    2. Stringent, real-time requirements for some application programs that may not be met because of DBMS overhead
    3. Embedded systems with limited storage capacity
    4. No multiple-user access to data

Chapter 1.9 Summary

Chapter 3.1 Domains, Attributes, Tuples, and Relations
  - A domain D is a set of atomic values.
  - By atomic we mean that each value in the domain is indivisible as far as the formal relational model is concerned
  - A relation schema is made up of a relation name R and a list of attributes (A1 ... An)
  - Attribute Ai is the name of a role played by some domain D
  - R is called the name of the relation; a relation schema is used to describe a relation.
  - degree(arity) of a relation is the number of attributes in the relation schema

Chapter 3.1.2 Characteristics of Relations
  - There is no preference for one ordering over another.
  - During database design, it is best to avoid NULL values as much as possible.
  - The relational model represents facts about both entities and relationships uniformly as relations.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
XML DATA NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 12.2 XML Hierarchical (Tree) Data Model
  - Two main structuring concepts are used to construct an XML document:
    1. elements
    2. attributes
  - Complex elements are constructed from other elements hierarchically, whereas simple elements contain data values
  - XML model is called a tree model or a hierarchical model.
  - Three main types of XML documents:
    1. Data-centric XML documents
      - Have many small data items that follow a specific structure
      - Formatted as XML documents in order to exchange them over or display them on the Web
      - Usually follow a predefined schema that defines the tag names.
    2. Document-centric XML documents
      - Documents with large amounts of text (think books or articles)
      - Few or no structured data elements in these documents.
    3. Hybrid XML documents
      - May have parts that contain structured data and other parts that are predominantly textual or unstructured.
      - May have a predefined schema.
  - XML documents that do not follow a predefined schema of element names and corresponding tree structure are known as schemaless XML documents

Chapter 12.3.1 Well-Formed and Valid XML Documents and XML DTD In Figure 12.3,we saw what a simple
  - An XML document is well formed if it follows a few conditions.
    1. Must start with an XML declaration to indicate the version of XML being used as well as any other relevant attributes
    2. It must also follow the syntactic guidelines of the tree data model.
  - A standard model with an associated set of API (application programming interface) functions called DOM (Document Object Model) allows programs to manipulate
    the resulting tree representation corresponding to a well-formed XML
  - A well-formed XML document can be schemaless meaning no predefined set of elements
  - A stronger criterion is for an XML document to be valid. In this case, the document must be well formed, and it must follow a particular schema.
  - XML DTD elements:
    - ```*``` : known as an optional multivalued (repeating) element.
    - ```+``` : required multivalued (repeating) element.
    - ? : optional single-valued (nonrepeating) element.
    - (none) :  required single-valued (nonrepeating) element.
    - If the parentheses include the keyword #PCDATA or one of the other data types available in XML DTD, the element is a leaf node. PCDATA stands for
      parsed character data, which is roughly similar to a string data type.
    - The list of attributes that can appear within an element can also be specified via the keyword !ATTLIST.
    - If the type of an attribute is ID, then it can be referenced from another attribute whose type is IDREF within another element.
    - e1 | e2 : e1 or e2
  - Example of specifying a DTD is used notice standalone is no not yes:
    <?xml version=“1.0” standalone=“no”?>
    <!DOCTYPE Projects SYSTEM “proj.dtd”>
  - DTD drawbacks although its  specifying tree structures with required, optional, and repeating elements, and with various types of attributes
    1. The data types not very general
    2. DTD has its own special syntax and thus requires specialized processors. Would be better if the same XML processor is used to evaluate the XML DTD file!
    3. All DTD elements are always forced to follow the specified ordering of the document, so unordered elements are not permitted.
  - The previous drawbacks led to the development of XML schema

Chapter 12.3.2 XML Schema
  - Uses the same syntax rules as regular XML documents
    1. Schema descriptions and XML namespaces
      - XML namespace : specific set of XML schema language elements (tags) being used by specifying a file stored at a Web site location
      - The file name is assigned to the variable xsd (XML schema description) using the attribute xmlns (XML namespace)
      - xsd is used to prefix all XML schema commands (tag names)
    2. Annotations, documentation, and language used
      - xsd:annotation and xsd:documentation are used for providing comments and other descriptions in the XML document.
    3. Elements and types
      - The name attribute of the xsd:element tag specifies the element name
    4. First-level elements in the database.
      - If a tag has only attributes and no further subelements or data within it, it can be ended with the backslash symbol (/>)
      - These are called empty elements.
    5. Specifying element type and minimum and maximum occurrences.
      - If we specify a type attribute in an xsd:element, the structure of the element must be described separately, typically using the xsd:complexType
        element of XML schema
      - if no type attribute is specified, the element structure can be defined directly
    6. Specifying keys
      - The xsd:unique tag specifies elements that correspond to unique attributes in a relational database.
      - We can give each such uniqueness constraint a name, and we must specify xsd:selector and xsd:field tags for it to identify the element type that contains the
        unique element and the element name within it that is unique via the xpath attribute
      - For specifying primary keys, the tag xsd:key is used
      - When specifying a foreign key, the attribute referof the xsd:keyref tag specifies the referenced primary key, whereas the tags xsd:selector and xsd:field
        specify the referencing element type and foreign key
    7. Specifying the structures of complex elements via complex types
      - If we were not going to specify any key constraints, we could have embedded the subelements within the parent element definitions directly without having to specify complex types.
      - However, when unique, primary key and foreign key constraints need to be specified; we must define complex types to specify the element structures.
    8. Composite (compound) attributes
      - Composite attributes from Figure 7.2 are also specified as complex types

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
JSON NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
  - NONE
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
RELATIONAL ALGEBRA NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
6.1 Unary Relational Operations: SELECT and PROJECT

6.1.1 The SELECT Operation
  - The SELECT operation is used to choose a subset of the tuples from a relation that satisfies a selection condition
  - In general, the SELECT operation is denoted by
    - ``` σ <selection condition> (R) ```
    - where the symbol σ (sigma) is used to denote the SELECT operator and the selection condition is a Boolean expression (condition) specified on the attributes of relation R.
  - The Boolean expression specified in <selection condition> is made up of a number of clauses of the form
    - ```<attribute name> <comparison op> <constant value>```
  - or
    - ```<attribute name> <comparison op> <attribute name>```
  - If the domain of an attribute is a set of unordered values, then only the comparison operators in the set {=,≠} can be used. An example of an unordered domain is the domain Color = { ‘red’, ‘blue’, ‘green’, ‘white’, ‘yellow’, ...},where no order is specified among the various colors.
  - The SELECT operator is unary; that is, it is applied to a single relation.
  - The degree of the relation resulting from a SELECT operation—its number of attributes—is the same as the degree of R.
  - The fraction of tuples selected by a selection condition is referred to as the selectivity of the condition.
  - A sequence of SELECTs can be applied in any order. In addition, we can always combine a cascade (or sequence) of SELECT operations into a single SELECT operation with a conjunctive (AND) condition

6.1.2 The PROJECT Operation
  - If we think of a relation as a table, the SELECT operation chooses some of the rows from the table while discarding other rows.
  - The PROJECT operation, on the other hand, selects certain columns from the table and discards the other columns.
  - The general form of the PROJECT operation is ```π<attribute list>(R)```
  - The result of the PROJECT operation has only the attributes specified in (attribute list) in the same order as they appear in the list. Hence, its degree is equal to the number of attributes in (attribute list).
  - The PROJECT operation removes any duplicate tuples, so the result of the PROJECT operation is a set of distinct tuples, and hence a valid relation. This is known as duplicate elimination.
  - If duplicates are not eliminated, the result would be a multiset or bag of tuples rather than a set.
  - The number of tuples in a relation resulting from a PROJECT operation is always less than or equal to the number of tuples in R.
  - In SQL, the PROJECT attribute list is specified in the SELECT clause of a query.
  - ```π <Sex, Salary>(EMPLOYEE) ```
    - would correspond to the following SQL query:
    ```SQL
    SELECT DISTINCT Sex, Salary FROM EMPLOYEE
    ```
    - Notice that if we remove the keyword DISTINCT from this SQL query, then duplicates will not be eliminated. This option is not available in the formal relational algebra

6.1.3 Sequences of Operations and the RENAME Operation
  - Either we can write the operations as a single relational algebra expression by nesting the operations, or we can apply one operation at a time and create intermediate result relations.
  - It is sometimes simpler to break down a complex sequence of operations by specifying intermediate result relations than to write a single relational algebra expression.
  - Renaming in SQL is accomplished by aliasing using AS, as in the following example:
  ```SQL
  SELECT E.Fname AS First_name, E.Lname AS Last_name, E.Salary AS Salary
  FROM EMPLOYEE AS E
  WHERE E.Dno=5
  ```

6.2 Relational Algebra Operations from Set Theory

6.2.1 The UNION, INTERSECTION, and MINUS Operations
  - Several set theoretic operations are used to merge the elements of two sets in various ways, including UNION, INTERSECTION, and SET DIFFERENCE (also called MINUS or EXCEPT).
  - Two relations R(A1,A2,...,An) and S(B1,B2,...,Bn) are said to be union compatible (or type compatible) if they have the same degree n and if dom(Ai) = dom(Bi) for 1 fi fn. This means that the two relations have the same number of attributes and each corresponding pair of attributes has the same domain.
  - Three operations UNION, INTERSECTION, and SET DIFFERENCE
    1. UNION: The result of this operation, denoted by R ∪ S, is a relation that includes all tuples that are either in R or in S or in both R and S. Duplicate tuples are eliminated.
    2. INTERSECTION: The result of this operation, denoted by R ∩ S, is a relation that includes all tuples that are in both R and S.
    3. SET DIFFERENCE (or MINUS): The result of this operation, denoted by R – S, is a relation that includes all tuples that are in R but not in S.
  - Both UNION and INTERSECTION are commutative operations; that is,
  - ```R ∪ S = S ∪ R and R ∩ S = S ∩ R```
  - Both UNION and INTERSECTION can be treated as n-ary operations applicable to any number of relations because both are also associative operations;
  - ```R ∪ (S ∪ T)=(R ∪ S) ∪ T and  (R ∩ S) ∩ T = R ∩ (S ∩ T)```
  - The MINUS operation is not commutative; that is, in general,
  - ```R − S ≠ S − R```
  - Note that INTERSECTION can be expressed in terms of union and set difference as follows:
  - ```R ∩ S = ((R ∪ S) − (R − S)) − (S − R) ```
  - In SQL, there are three operations — UNION, INTERSECT, and EXCEPT—that correspond to the set operations.  

6.2.2 The CARTESIAN PRODUCT (CROSS PRODUCT) Operation
  - CARTESIAN PRODUCT operation — also known as CROSS PRODUCT or CROSS JOIN — which is denoted by ×.
  - This is also a binary set operation, but the relations on which it is applied do not have to be union compatible.
  - In general, the CARTESIAN PRODUCT operation applied by itself is generally meaningless. It is mostly useful when followed by a selection that matches values of attributes coming from the component relations.

6.3 Binary Relational Operations: JOIN and DIVISION

6.3.1 The JOIN Operation
  - The JOIN operation can be specified as a CARTESIAN PRODUCT operation followed by a SELECT operation.
  - ``` R ⋈ <join condition>S ```
  - The result of the JOIN is a relation Q with n + m attributes Q(A1, A2, ...,An, B1, B2, ...,Bm) in that order
  - In JOIN, only combinations of tuples satisfying the join condition appear in the result, whereas in the CARTESIAN PRODUCT all combinations of tuples are included in the result.
  - Each tuple combination for which the join condition evaluates to TRUE is included in the resulting relation Q as a single combined tuple.
  - In general ``` <condition> AND <condition> AND...AND <condition>  ```

6.3.2 Variations of JOIN: The EQUIJOIN and NATURAL JOIN
  - A JOIN, where the only comparison operator used is =, is called an EQUIJOIN.
  - Notice that in the result of an EQUIJOIN we always have one or more pairs of attributes that have identical values in every tuple.
  - A new operation called NATURAL JOIN—denoted by *  was created to get rid of the second (superfluous) attribute in an EQUIJOIN condition
  - The standard definition of NATURAL JOIN requires that the two join attributes (or each pair of join attributes) have the same name in both relations. If this is not the case, a renaming operation is applied first.
  - If the attributes on which the natural join is specified already have the same names in both relations, renaming is unnecessary.
  - The expected size of the join result divided by the maximum size nR * nS leads to a ratio called join selectivity, which is a property of each join condition.
  - If there is no join condition, all combinations of tuples qualify and the JOIN degenerates into a CARTESIAN PRODUCT, also called CROSS PRODUCT or CROSS JOIN.

6.3.3 A Complete Set of Relational Algebra Operations
  - It has been shown that the set of relational algebra operations {σ,π,∪,ρ, –,×} is a complete set; that is, any of the other original relational algebra operations can be expressed as a sequence of operations from this set.

6.3.4 The DIVISION Operation
  - The DIVISION operation, denoted by ÷, is useful for a special kind of query that sometimes occurs in database applications.
  - The DIVISION operation is defined for convenience for dealing with queries that involve universal quantification or the all condition. Most RDBMS implementations with SQL as the primary query language do not directly implement division. SQL has a roundabout way of dealing with the type of query.

6.3.5 Notation for Query Trees
  - A query tree is a tree data structure that corresponds to a relational algebra expression.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
SQL NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 3.3 Basic Structure of SQL Queries
  - The basic structure of an SQL query consists of three clauses: select, from, and where. The query takes as its input the relations listed in the from clause, operates on them as speciﬁed in the where and select clauses, and then produces a relation as the result.  

Chapter 3.3.1 Queries on a Single Relation
  - Ex: “Find the names of all instructors.”
  ```SQL
  select name
  from instructor;
  ```
  - Ex: “Find the department names of all instructors”
  ```SQL
  select dept_name
  from instructor;
  ```
  - Since more than one instructor can belong to a department, a department name could appear more than once in the instructor relation.
  - In the formal, mathematical deﬁnition of the relational model, a relation is a set. Thus, duplicate tuples would never appear in relations. In practice, duplicate elimination is time-consuming. Therefore, SQL allows duplicates in relations as well as in the results of SQL expressions
  - In those cases where we want to force the elimination of duplicates, we insert the keyword distinct after select. If we want duplicates removed.
  ```SQL
  select distinct dept_name
  from instructor
  ```
  - SQL allows us to use the keyword all to specify explicitly that duplicates are not removed:
  ```SQL
  select all dept_name
  from instructor;
  ```
  - The select clause may also contain arithmetic expressions involving the operators +, −, ∗, and /operating on constants or attributes of tuples.
  ```SQL
  select ID, name, dept_name, salary * 1.1
  from instructor;
  ```
  - Note, however, that it does not result in any change to the instructor relation.
  - The where clause allows us to select only those rows in the result relation of the from clause that satisfy a speciﬁed predicate.
  - Ex: “Find the names of all instructors in the Computer Science department who have salary greater than $70,000.”
  ```SQL
  select name
  from instructor
  where dept_name = ’Comp. Sci.’ and salary > 70000;
  ```
  - SQL allows the use of the logical connectives and, or, and not in the where clause.
  - The operands of the logical connectives can be expressions involving the comparison operators <, <=, >, >=, =, and <>.

Chapter 3.3.2 Queries on Multiple Relations
  -  SQL query can contain three types of clauses, the select clause, the from clause, and the where clause. The role of each clause is as follows:
    - The select clause is used to list the attributes desired in the result of a query.
    - The from clause is a list of the relations to be accessed in the evaluation of the query.
    - The where clause is a predicate involving attributes of the relation in the from clause.
  - A typical SQL query has the form
  ```SQL
  select A1, A2,...,An
  from r1, r2,...,rm
  where P;
  ```
  - Each Ai represents an attribute, and each ri a relation. P is a predicate. If the where clause is omitted, the predicate P is true.
  - Although the clauses must be written in the order select, from, where, the easiest way to understand the operations speciﬁed by the query is to consider the clauses in operational order: ﬁrst from, then where, and then select.
  - The from clause by itself deﬁnes a Cartesian product of the relations listed in the clause.
  - The predicate in the where clause is used to restrict the combinations created by the Cartesian product to those that are meaningful for the desired answer.
  - In general, the meaning of an SQL query can be understood as follows:
    1. Generate a Cartesian product of the relations listed in the from clause
    2. Apply the predicates speciﬁed in the where clause on the result of Step 1.
    3. For each tuple in the result of Step 2, output the attributes (or results of expressions) speciﬁed in the select clause.
  - A real implementation of SQL would not execute the query in this fashion; it would instead optimize evaluation by generating(as far as possible) only elements of the Cartesian product that satisfy the where clause predicates.
  - When writing queries, you should be careful to include appropriate where clause conditions. If you omit the where clause condition in the preceding SQL query, it would output the Cartesian product, which could be a huge relation.

Chapter 3.3.3 The Natural Join
  - The natural join operation operates on two relations and produces a relation as the result.
  - Natural join considers only those pairs of tuples with the same value on those attributes that appear in the schemas of both relations.
  - Notice that we do not repeat those attributes that appear in the schemas of both relations; rather they appear only once.
  - Notice also the order in which the attributes are listed: ﬁrst the attributes common to the schemas of both relations, second those attributes unique to the schema of the ﬁrst relation, and ﬁnally, those attributes unique to the schema of the second relation.
  - Ex: “For all instructors in the university who have taught some course, ﬁnd their names and the course ID of all courses they taught”
  ```SQL
  select name, course id
  from instructor, teaches
  where instructor.ID=teaches.ID;
  ```
  - This can be rewritten more concisely using the natural-join operation:
  ```SQL
  select name, course id
  from instructor natural join teaches;
  ```
  - A from clause in an SQL query can have multiple relations combined using natural join, as shown here:
  ```SQL
  select A1, A2,...,An
  from r1 natural join r2 natural join ... natural join rm
  where P;
  ```
  - More generally, a from clause can be of the form ``` from E1, E2,...,En ```
  - Ex: “List the names of instructors along with the titles of courses that they teach.”
    - The natural join of instructor and teaches is ﬁrst computed, as we saw earlier, and a Cartesian product of this result with course is computed, from which the where clause extracts only those tuples where the course identiﬁer from the join result matches the course identiﬁer from the course relation.
    ```SQL
    select name, title
    from instructor natural join teaches, course
    where teaches.course_id = course.course_id;
    ```  
  - In contrast the following SQL query does not compute the same result:
  ```SQL
  select name, title
  from instructor natural join teaches natural join course;  
  ```

Chapter 3.4 Additional Basic Operations

Chapter 3.4.1 The Rename Operation
  - Two relations in the from clause may have attributes with the same name, in which case an attribute name is duplicated in the result.
  - If we used an arithmetic expression in the select clause, the resultant attribute does not have a name.
  - SQL provides a way of renaming the attributes of a result relation. ``` old-name as new-name ```
  - The as clause can appear in both the select and from clauses.
  ```SQL
  select name as instructor name, course id
  from instructor, teaches
  where instructor.ID=teaches.ID;
  ```
  - The as clause is particularly useful in renaming relations.
  - One reason to rename a relation is to replace along relation name with a shortened version that is more convenient to use elsewhere in the query.
  - Ex: "For all instructors in the university who have taught some course, ﬁnd their names and the course ID of all courses they taught.”
  ```SQL
  select T.name, S.course id
  from instructor as T, teaches as S
  where T.ID = S.ID;
  ```
  - Another reason to rename a relation is a case where we wish to compare tuples in the same relation. We then need to take the Cartesian product of a relation with itself and, without renaming, it becomes impossible to distinguish one tuple from the other.
  - Ex: “Find the names of all instructors whose salary is greater than at least one instructor in the Biology department.”
  ```SQL
  select distinct T.name
  from instructor as T, instructor as S
  where T.salary > S.salary and S.dept name = ’Biology’;
  ```
  - An identiﬁer, that is used to rename a relation is referred to as a correlation name in the SQL standard, but is also commonly referred to as a table alias, or a correlation variable, or a tuple variable.

Chapter 3.4.2 String Operations
  - A single quote character that is part of a string can be speciﬁed by using two single quote characters; for example, the string “It’s right” can be speciﬁed by “It's right”.
  - The SQL standard speciﬁes that the equality operation on strings is case sensitive
  - However, some database systems, such as MySQL and SQL Server, do not distinguish uppercase from lowercase when matching strings
  - SQL also permits a variety of functions on character strings, such as concatenating (using “||”), extracting substrings, ﬁnding the length of strings, upper(s), lower(s), trim(s).
  - Pattern matching can be performed on strings, using the operator like.
  - We describe patterns by using two special characters:
    - Percent ( % ): The % character matches any substring.
    - Underscore ( _ ): The character matches any character.
  - Patterns are case sensitive
  - SQL expresses patterns by using the like comparison operator
  - Ex: “Find the names of all departments whose building name includes the substring ‘Watson’.”
  ```SQL
  select dept_name
  from department
  where building like ’%Watson%’;
  ```
  - For patterns to include the special pattern characters (that is, % and _ ), SQL allows the speciﬁcation of an escape character
    - like ’ab\%cd%’ escape ’\’ matches all strings beginning with “ab%cd”.
    - like ’ab\\cd%’ escape ’\’ matches all strings beginning with “ab\cd”.
  - SQL allows us to search for mismatches instead of matches by using the not like comparison operator

Chapter 3.4.3 Attribute Speciﬁcation in Select Clause
  - The asterisk symbol “ * ” can be used in the select clause to denote “all attributes.”
  ```SQL
  select instructor.*
  from instructor, teaches
  where instructor.ID=teaches.ID;
  ```

Chapter 3.4.4 Ordering the Display of Tuples
  - The order by clause causes the tuples in the result of a query to appear in sorted order.
  ```SQL
  select name
  from instructor
  where dept_name=’Physics’
  order by name;
  ```
  - By default, the order by clause lists items in ascending order.
  ```SQL
  select *
  from instructor
  order by salary desc, name asc;
  ```

Chapter 3.4.5 Where Clause Predicates
  - SQL includes a between comparison operator to simplify where clauses that specify that a value be less than or equal to some value and greater than or equal to some other value
  ```SQL
  select name
  from instructor
  where salary between 90000 and 100000;
  ```
  - Similarly, we can use the not between comparison operator.
  - Two tuples are equal if all their attributes are equal
  ```SQL
  select name, course_id
  from instructor, teaches
  where (instructor.ID, dept_name) = (teaches.ID, ’Biology’);
  ```

Chapter 3.5 Set Operations
  - The SQL operations union, intersect, and except operate on relations and correspond to the mathematical set-theory operations ∪, ∩, and −.

Chapter 3.5.1 The Union Operation
  ```SQL
  (select course_id
   from section
   where semester=’Fall’ and year=2009)
  union
  (select course_id
   from section
   where semester=’Spring’ and year=2010);
  ```
  - The union operation automatically eliminates duplicates, unlike the select clause.
  - If we want to retain all duplicates, we must write union all in place of union:
  ```SQL
  (select course_id
   from section
   where semester=’Fall’ and year=2009)
  union all
  (select course_id
   from section
   where semester=’Spring’ and year=2010);
  ```
  - The number of duplicate tuples in the result is equal to the total number of duplicates that appear in both c1 and c2.

Chapter 3.5.2 The Intersect Operation
  ```SQL
  (select course_id
   from section
   where semester=’Fall’ and year=2009)
  intersect
  (select course_id
   from section
   where semester=’Spring’ and year=2010);
  ```
  - The intersect operation automatically eliminates duplicates.
  - If we want to retain all duplicates, we must write intersect all in place of intersect:
  ```SQL
  (select course_id
   from section
   where semester=’Fall’ and year=2009)
  intersect all
  (select course_id
   from section
   where semester=’Spring’ and year=2010);
  ```
  - The number of duplicate tuples that appear in the result is equal to the minimum number of duplicates in both c1 and c2.

Chapter 3.5.3 The Except Operation
  ```SQL
  (select course_id
   from section
   where semester=’Fall’ and year=2009)
  except
  (select course_id
   from section
   where semester=’Spring’ and year=2010);
  ```
  - The except operation7 outputs all tuples from its ﬁrst input that do not occur in the second input; that is, it performs set difference.
  - The operation automatically eliminates duplicates in the inputs before performing set difference.
  - If we want to retain duplicates, we must write except all in place of except:
  ```SQL
  (select course_id
   from section
   where semester=’Fall’ and year=2009)
  except all
  (select course_id
   from section
   where semester=’Spring’ and year=2010);
  ```

Chapter 3.6 Null Values
  - The result of an arithmetic expression (involving, for example+,−,∗,or/) is null if any of the input values is null.
  - SQL treats as unknown the result of any comparison involving a null value
  - The deﬁnitions of the Boolean operations are extended to deal with the value unknown.
    - and: The result of true and unknown is unknown, false and unknown is false, while unknown and unknown is unknown.
    - or: The result of true or unknown is true, false or unknown is unknown, while unknown or unknown is unknown.
    - not: The result of not unknown is unknown.
  - If the where clause predicate evaluates to either false or unknown for a tuple, that tuple is not added to the result.
  - SQL uses the special keyword null in a predicate to test for a null value
  ```SQL
  select name
  from instructor
  where salary is null;
  ```
  - The predicate is not null succeeds if the value on which it is applied is not null.

Chapter 3.7 Aggregate Functions
  - Aggregate functions are functions that take a collection (a set or multiset) of values as input and return a single value.
  - SQL offers ﬁve built-in aggregate functions:
    - Average: avg
    - Minimum: min
    - Maximum: max
    - Total: sum
    - Count: count
  - The input to sum and avg must be a collection of numbers, but the other operators can operate on collections of nonnumeric data types, such as strings, as well.

Chapter 3.7.1 Basic Aggregation
  - Ex: “Find the average salary of instructors in the Computer Science department.”
  ```SQL
  select avg (salary)
  from instructor
  where dept_name=’Comp. Sci.’;
  ```
  - We can give a meaningful name to the attribute by using the as clause:
  ```SQL
  select avg (salary) as avg salary
  from instructor
  where dept_name=’Comp. Sci.’;
  ```
  - If we do want to eliminate duplicates, we use the keyword distinct in the aggregate expression.
  - Ex: “Find the total number of instructors who teach a course in the Spring 2010 semester.”
  ```SQL
  select count (distinct ID)
  from teaches
  where semester=’Spring’ and year=2010;
  ```
  - SQL does not allow the use of distinct with count( * ).
  - It is legal to use distinct with max and min, even though the result does not change.

Chapter 3.7.2 Aggregation with Grouping
  - The attribute or attributes given in the group by clause are used to form groups.
  - Tuples with the same value on all attributes in the group by clause are placed in one group.
  - Ex: “Find the average salary in each department.”
  ```SQL
  select dept_name, avg (salary) as avg salary
  from instructor
  group by dept_name;
  ```
  - Ex: “Find the number of instructors in each department who teach a course in the Spring 2010 semester.”
  ```SQL
  select dept_name, count (distinctID) as instr_count
  from instructor natural join teaches
  where semester=’Spring’ and year=2010
  group by dept_name;
  ```
  ```SQL
  /* erroneous query*/
  select dept_name, ID,
  avg (salary) from instructor
  group by dept_name;
  ```
  - Each instructor in a particular group (deﬁned by dept_name) can have a different ID, and since only one tuple is output for each group, there is no unique way of choosing which ID value to output

Chapter 3.7.3 The Having Clause
  - It is useful to state a condition that applies to groups rather than to tuples
  - To express such a query, we use the having clause of SQL
  ```SQL
  select dept_name, avg (salary) as avg_salary
  from instructor
  group by dept_name
  having avg (salary) > 42000;
  ```
  - The meaning of a query containing aggregation, group by, or having clauses is deﬁned by the following sequence of operations:
    1. As was the case for queries without aggregation, the from clause is ﬁrst evaluated to get a relation.
    2. If a where clause is present, the predicate in the where clause is applied on the result relation of the from clause.
    3. Tuples satisfying the where predicate are then placed into groups by the group by clause if it is present. If the group by clause is absent, the entire set of tuples satisfying the where predicate is treated as being in one group.
    4. The having clause, if it is present, is applied to each group; the groups that do not satisfy the having clause predicate are removed.
    5. The select clause uses the remaining groups to generate tuples of the result of the query, applying the aggregate functions to get a single result tuple for each group.
  - “For each course section offered in 2009, ﬁnd the average total credits(tot_cred) of all students enrolled in the section, if the section had at least 2 students.”
  ```SQL
  select course_id, semester, year, sec_id, avg (tot_cred)
  from takes natural join student
  where year=2009
  group by course_id, semester, year, sec_id
  having count (ID) >=2;
  ```

Chapter 3.7.4 Aggregation with Null and Boolean Values
  - Null values, when they exist, complicate the processing of aggregate operators.
  ```SQL
  select sum (salary)
  from instructor;
  ```
  - Rather than say that the overall sum is itself null, the SQL standard says that the sum operator should ignore null values in its input.
  - All aggregate functions except count ( * ) ignore null values in their input collection

Chapter 3.8 Nested Subqueries

Chapter 3.8.1 Set Membership
  - The in connective tests for set membership, where the set is a collection of values produced by a select clause.
  - The not in connective tests for the absence of set membership.
  - Ex: “Find all the courses taught in the both the Fall2009 and Spring 2010 semesters.”
  ```SQL
  select distinct course_id
  from section
  where semester=’Fall’
        and year=2009
        and course_id in (
                          select course_id
                          from section
                          where semester=’Spring’
                                and year=2010
                          );
  ```
  - Ex: to ﬁnd all the courses taught in the Fall 2009 semester but not in the Spring 2010 semester
  ```SQL
  select distinct course_id
  from section
  where semester=’Fall’
        and year=2009
        and course id not in (
                              select course id
                              from section
                              where semester=’Spring’
                                    and year=2010
                             );
  ```
  - Ex: “ﬁnd the total number of (distinct) students who have taken course sections taught by the instructor with ID 110011”
  ```SQL
  select count (distinct ID)
  from takes
  where (course_id, sec_id, semester, year) in (select course_id, sec_id, semester, year
                                                from teaches
                                                where teaches.ID= 10101
                                              );

  ```

Chapter 3.8.2 Set Comparison
  - “Find the names of all instructors whose salary is greater than at least one instructor in the Biology department.”
  ```SQL
  select name
  from instructor
  where salary > some (select salary
                       from instructor
                       where dept_name=’Biology’);
  ```
  - SQL also allows < some, <= some, >= some, = some, and <> some comparisons.
  - = some is identical to in, whereas <> some is not the same as not in.
  - The construct > all corresponds to the phrase “greater than all.”
  ```SQL
  select name
  from instructor
  where salary > all (select salary
                      from instructor
                      where dept_name=’Biology’);
  ```
  - SQL also allows < all, <= all, >= all, = all, and <> all comparisons.
  - <> all is identical to not in, whereas= all is not the same as in.
  - Ex: “Find the departments that have the highest average salary.”
  ```SQL
  select dept_name
  from instructor
  group by dept_name
  having avg (salary) >= all (select avg (salary)
                              from instructor
                              group by dept_name);
  ```

Chapter 3.8.3 Test for Empty Relations
  - The exists construct returns the value true if the argument subquery is nonempty
  - Ex: “Find all courses taught in both the Fall 2009 semester and in the Spring 2010 semester”
  ```SQL
  select course_id
  from section as S
  where semester=’Fall’
        and year=2009
        and exists (select *
                    from section as T
                    where semester=’Spring’
                          and year=2010
                          and S.course_id=T.course_id);
  ```
  - A subquery that uses a correlation name from an outer query is called a correlated subquery.
  - If a correlation name is deﬁned both locally in a subquery and globally in a containing query, the local deﬁnition applies.
  - This rule is analogous to the usual scoping rules used for variables in programming languages.
  - We can write “relation A contains relation B” as “ not exists (B except A).”
  - Ex: “Find all students who have taken all courses offered in the Biology department.”
  ```SQL
  select distinct S.ID, S.name
  from student as S
  where not exists ((select course_id
                     from course
                     where dept_name=’Biology’) except (select T.course_id
                                                        from takes as T
                                                        where S.ID=T.ID));
  ```

Chapter 3.8.4 Test for the Absence of Duplicate Tuples
  - The unique construct returns the value true if the argument subquery contains no duplicate tuples.
  - Ex: “Find all courses that were offered at most once in 2009”
    ```SQL
    select T.course_id
    from course as T
    where unique (select R.course_id
                  from section as R
                  where T.course_id = R.course_id
                        and R.year=2009
                 );
    ```
    - Note that if a course is not offered in 2009, the subquery would return an empty result, and the unique predicate would evaluate to true on the empty set.
  - Ex: Same as previous
  ```SQL
  select T.course_id
  from course as T
  where 1 <= (select count(R.course_id)
              from section as R
              where T.course_id=R.course_id
                    and R.year=2009
              );
  ```
  - We can test for the existence of duplicate tuples in a subquery by using the not unique construct.
  - Ex: “Find all courses that were offered at least twice in 2009”
  ```SQL
  select T.course_id
  from course as T
  where not unique(select R.course_id
                   from section as R
                   where T.course_id=R.course_id
                         and R.year=2009);
  ```
  - Since the test t1 = t2 fails if any of the ﬁelds of t1 or t2 are null, it is possible for unique to be true even if there are multiple copies of a tuple, as long as at least one of the attributes of the tuple is null.

Chapter 3.8.5 Subqueries in the From Clause
  - SQL allows a subquery expression to be used in the from clause
  - Ex: “Find the average instructors’ salaries of those departments where the average salary is greater than $42,000.”
  ```SQL
  select dept_name, avg_salary
  from (select dept_name, avg (salary) as avg_salary
        from instructor
        group by dept_name)
  where avg_salary > 42000;
  ```
  - We can give the subquery result relation a name, and rename the attributes, using the as claus, The subquery result relation is named dept_avg, with the attributes dept_name and avg_salary.
  ```SQL
  select dept_name, avg_salary
  from (select dept_name, avg (salary)
        from instructor
        group by dept_name)
        as dept_avg (dept_name, avg_salary)
  where avg_salary > 42000;
  ```
  - Nested subqueries in the from clause are supported by most but not all SQL implementations.
  - Some SQL implementations, notably Oracle, do not support renaming of the result relation in the from clause.
  - Find the maximum across all departments of the total salary at each department
  ```SQL
  select max (tot_salary)
  from (select dept_name, sum(salary)
        from instructor
        group by dept_name) as dept_total (dept_name, tot_salary);
  ```
  - We note that nested subqueries in the from clause cannot use correlation variables from other relations in the from clause.

Chapter 3.8.6 The with Clause
  - The with clause provides away of deﬁning a temporary relation whose deﬁnition is available only to the query in which the with clause occurs.
  - Ex: departments with the maximum budget
  ```SQL
  with max_budget (value) as
      (select max(budget)
      from department)
  select budget
  from department, max_budget
  where department.budget = max_budget.value;
  ```
  - All departments where the total salary is greater than the average of the total salary at all departments
  ```SQL
  with dept_total (dept_name, value) as
       (select dept_name, sum(salary)
       from instructor
       group by dept_name),
  dept_total_avg(value) as
      (select avg(value)
      from dept_total)
  select dept_name
  from dept_total, dept_total_avg
  where dept_total.value >=dept_total_avg.value;
  ```

Chapter 3.8.7 Scalar Subqueries
  - SQL allows subqueries to occur wherever an expression returning a value is permitted, provided the subquery returns only one tuple containing a single attribute; such subqueries are called scalar subqueries
  ```SQL
  select dept_name,
         (select count(*)
          from instructor
          where department.dept_name = instructor.dept_name)
         as num_instructors
  from department;
  ```
  - Scalar subqueries can occur in select, where, and having clauses.

Chapter 3.9 Modiﬁcation of the Database

Chapter 3.9.1 Deletion
  - SQL expresses a deletion by:
  ```SQL
  delete from r
  where P;
  ```
  - The where clause can be omitted, in which case all tuples in r are deleted.
  ```SQL
  delete from instructor;
  ```
  - The instructor relation itself still exists, but it is empty.
  - Examples of SQL delete requests:
    1. Delete all tuples in the instructor relation pertaining to instructors in the Finance department.
    ```SQL
    delete from instructor
    where dept name=’Finance’;
    ```
    2. Delete all instructors with a salary between $13,000 and $15,000.
    ```SQL
    delete from instructor
    where salary between 13000 and 15000;
    ```
    3. Delete all tuples in the instructor relation for those instructors associated with a department located in the Watson building.
    ```SQL
    delete from instructor
    where dept_name in (select dept_name
                        from department
                        where building=’Watson’);
    ```
  - Note that, although we may delete tuples from only one relation at a time, we may reference any number of relations in a select-from-where nested in the where clause of a delete.
  - Ex: Delete the records of all instructors with salary below the average at the university
  ```SQL
  delete from instructor
  where salary < (select avg (salary)
                  from instructor);
  ```

Chapter 3.9.2 Insertion
  - The simplest insert statement is a request to insert one tuple.
  - Ex: Suppose that we wish to insert the fact that there is a course CS-437 in the Computer Science department with title “Database Systems”, and 4 credit hours.
  ```SQL
  insert into course
  values (’CS-437’, ’Database Systems’, ’Comp. Sci.’, 4);
  ```
  - For the beneﬁt of users who may not remember the order of the attributes, SQL allows the attributes to be speciﬁed as part of the insert statement.
  ```SQL
  insert into course (course id, title, dept_name, credits)
  values (’CS-437’, ’Database Systems’, ’Comp. Sci.’, 4);
  ```
  ```SQL
  insert into course (title, course_id, credits, dept_name)
  values (’Database Systems’, ’CS-437’, 4, ’Comp. Sci.’);
  ```
  - Ex: Make each student in the Music department who has earned more than 144 credit hours, an instructor in the Music department, with a salary of $18,000.
  ```SQL
  insert into instructor
         select ID, name, dept_name, 18000
         from student
         where dept_name=’Music’ and tot_cred > 144;
  ```
  - It is possible for inserted tuples to be given values on only some attributes of the schema.
  - Ex:
  ```SQL
  insert into student
  values (’3003’, ’Green’, ’Finance’, null);
  ```
  - Ex: Consider the query:
    ```SQL
    select student
    from student
    where tot_cred > 45;
    ```
    - Since the tot cred value of student “3003” is not known, we cannot determine whether it is greater than 45.

Chapter 3.9.3 Updates
  - Salaries of all instructors are to be increased by 5 percent.
  ```SQL
  update instructor
  set salary=salary * 1.05;
  ```
  - Ex: If a salary increase is to be paid only to instructors with salary of less than $70,000
  ```SQL
  update instructor
  set salary = salary * 1.05
  where salary < 70000;
  ```
  - Ex: “Give a 5 percent salary raise to instructors whose salary is less than average”
  ```SQL
  update instructor
  set salary = salary * 1.05
  where salary < (select avg (salary)
                  from instructor);
  ```
  - SQL provides a case construct that we can use to perform two updates with a single update statement, avoiding the problem with the order of updates.
  ```SQL
  update instructor
  set salary = case
                   when salary <=100000 then salary * 1.05
                   else salary * 1.03
               end
  ```
  - The general form of the case statement is as follows.
  ```SQL
  case
    when pred1 then result1
    when pred2 then result2
    ...
    when predn then resultn
    else result0
  end
  ```
  - Scalar subqueries are also useful in SQL update statements, where they can be used in the set clause.
  - Ex: We assume that a course is successfully completed if the student has a grade that is not ’F’ or null.
  ```SQL
  update student S
  set tot_cred = (
      select sum(credits)
      from takes natural join course
      where S.ID = takes.ID
            and takes.grade <> ’F’
            and takes.grade is not null);
  ```

Chapter 4.1 Join Expressions

Chapter 4.1.1 Join Conditions
  - The on condition allows a general predicate over the relations being joined.
  - Written like a where clause predicate except for the use of the keyword on rather than where.
  - Ex: Speciﬁes that a tuple from student matches a tuple from takes if their ID values are equal.
  ```SQL
  select *
  from student join takes on student.ID = takes.ID;
  ```
  - Ex: Equivalent to the above:
  ```SQL
  select *
  from student, takes
  where student.ID = takes.ID;
  ```

Chapter 4.1.2 Outer Joins
  - The outer join operation preserves those tuples that would be lost in a join, by creating tuples in the result containing null values.
  - Three forms of outer join:
    1. The left outer join preserves tuples only in the relation named before(to the left of) the left outer join operation.
    2. The right outer join preserves tuples only in the relation named after(to the right of) the right outer join operation.
    3. The full outer join preserves tuples in both relations.
  - Ex: “Find all students who have not taken a course”
  ```SQL
  select ID
  from student natural left outer join takes
  where course_id is null;
  ```
  - The right outer join is symmetric to the left outer join

Chapter 4.1.3 Join Types and Conditions
  - To distinguish normal joins from outer joins, normal joins are called inner joins in SQL
  - The keyword inner is optional
  - The default join type, when the join clause is used without the outer preﬁx is the inner join.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
XPATH AND XQUERY NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
12.5.1 XPath: Specifying Path Expressions in XML
  - The most common type of XPath expression returns a collection of element or attribute nodes that satisfy certain patterns specified in the expression.
  - Two main separators are used when specifying a path: single slash (/) and double slash (//).
  - A single slash before a tag specifies that the tag must appear as a direct child of the previous (parent) tag, whereas a double slash specifies that the tag can appear as a descendant of the previous tag at any level.
  - Examples:
  ```
  1. /company
  ```
  - Returns the company root node and all its descendant nodes, which means that it returns the whole XML document.
  ```
  2. /company/department
  ```
  - Returns all department nodes (elements) and their descendant subtrees
  ```
  3. //employee [employeeSalary gt 70000]/employeeName
  ```
  - //, is convenient to use if we do not know the full path name we are searching for, but do know the name of some tags of interest within the XML document.
  - The expression returns all employeeName nodes that are direct children of an employee node, such that the employee node has another child element employeeSalary whose value is greater than 70000.
  ```
  4. /company/employee [employeeSalary gt 70000]/employeeName
  ```
  - Return the same result as the previous one, except that we specified the full path name
  ```
  5. /company/project/projectWorker [hours ge 20.0]
  ```
  - returns all projectWorker nodes and their descendant nodes that are children under a path /company/project and have a child node hours with a value greater than 20.0 hours.

  - When we need to include attributes in an XPath expression, the attribute name is prefixed by the @ symbol to distinguish it from element (tag) names.
  - It is also possible to use the wildcard symbol * ,which stands for any element, as in the following example, which retrieves all elements that are child elements of the root, regardless of their element type.
  - The main restriction of XPath path expressions is that the path that specifies the pattern also specifies the items to be retrieved. Hence, it is difficult to specify certain conditions on the pattern while separately specifying which result items should be retrieved.

12.5.2 XQuery: Specifying Queries in XML
  - XQuery permits the specification of more general queries on one or more XML documents.
  - The typical form of a query in XQuery is known as a FLWR expression, which stands for the four main clauses of XQuery and has the following form:
  ```XQuery
  FOR <variable bindings to individual nodes (elements)>
  LET <variable bindings to collections of nodes (elements)>
  WHERE <qualifier conditions>
  RETURN <query result specification>
  ```
  - There can be zero or more instances of the FOR clause, as well as of the LET clause in a single XQuery. The WHERE clause is optional, but can appear at most once, and the RETURN clause must appear exactly once.
  ```XQuery
  LET $d := doc(www.company.com/info.xml)
  FOR $x IN $d/company/project[projectNumber = 5]/projectWorker, $y
         IN $d/company/employee
  WHERE $x/hours gt 20.0 AND $y.ssn = $x.ssn
  RETURN <res> $y/employeeName/firstName, $y/employeeName/lastName, $x/hours </res>
  ```
  1. Variables are prefixed with the $ sign. In the above example, $d, $x, and $y are variables.
  2. The LET clause assigns a variable to a particular expression for the rest of the query. In this example, $d is assigned to the document file name. It is possible to have a query that refers to multiple documents by assigning multiple variables in this way.
  3. The FOR clause assigns a variable to range over each of the individual items in a sequence. In our example, the sequences are specified by path expressions. The $x variable ranges over elements that satisfy the path expression $d/company/project[projectNumber = 5]/projectWorker. The $y variable ranges over elements that satisfy the path expression $d/company/employee. Hence, $x ranges over projectWorker elements, whereas $y ranges over employee elements
  4. The WHERE clause specifies additional conditions on the selection of items. In this example, the first condition selects only those projectWorker elements that satisfy the condition (hours gt 20.0). The second condition specifies a join condition that combines an employee with a projectWorker only if they have the same ssn value.
  5. RETURN clause specifies which elements or attributes should be retrieved from the items that satisfy the query conditions.In this example, it will return a sequence of elements each containing <firstName, lastName, hours> for employees who work more that 20 hours per week on project number 5.
  ```XQuery
  1. FOR $x IN
        doc(www.company.com/info.xml)
        //employee [employeeSalary gt 70000]/employeeName
        RETURN <res> $x/firstName, $x/lastName </res>
  ```
  - Retrieves the first and last names of employees who earn more than $70,000.The variable $x is bound to each employeeName element that is a child of an employee element, but only for employee elements that satisfy the qualifier that their employeeSalary value is greater than $70,000. The result retrieves the firstName and lastName child elements of the selected employeeName elements.
  ```XQuery
  2. FOR $x IN
        doc(www.company.com/info.xml)/company/employee
        WHERE $x/employeeSalary gt 70000
        RETURN <res> $x/employeeName/firstName, $x/employeeName/lastName </res>
  ```
  - Same as first query
  ```XQuery
  3. FOR $x IN
        doc(www.company.com/info.xml)/company/project[projectNumber = 5]/projectWorker,
        $y IN doc(www.company.com/info.xml)/company/employee
        WHERE $x/hours gt 20.0 AND $y.ssn = $x.ssn
        RETURN <res> $y/employeeName/firstName, $y/employeeName/lastName, $x/hours </res>
  ```
  - The $x variable is bound to each projectWorker element that is a child of project number 5,whereas the $yvariable is bound to each employee element.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
XSLT NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
  - NONE

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
