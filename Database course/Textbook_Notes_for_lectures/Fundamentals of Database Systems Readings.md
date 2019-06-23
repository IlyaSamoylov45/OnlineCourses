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
