-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
INTRODUCTION AND RELATIONAL DATABASES NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
CHAPTER 1.1 The Evolution of Database Systems
  - The term database refers to a collection of data that is managed by a DBMS.
  - DBMS expected to:
    1. Allow users to create new databases / specify schemas
    2. Give users the ability to query the data
    3. Support the storage of very large amounts of data
    4. Enable durability in case database fails
    5. Control access to the data from many users at the same time

CHAPTER 1.1.1 Early Database Management Systems
  - The first important applications of DBMS’s were ones where data was composed of many small items
    ex: Banks, Airlines etc.

CHAPTER 1.1.2 Relational Database Systems
  - view of data organized as tables called relations.
  - Object-oriented features have infiltrated the relational model.

CHAPTER 1.1.3 Smaller and Smaller Systems
  -  Today, hundreds of gigabytes fit on a single disk, and it is quite feasible to run a DBMS on a personal computer.

CHAPTER 1.1.4 Bigger and Bigger Systems
  - On the other hand, a gigabyte is not that much data any more.

CHAPTER 1.1.5 Information Integration
  -  information integration: joining the information contained in many related databases into a whole.

CHAPTER 1.2 Overview of a Database Management System
  - Two distinct sources of commands to the DBMS:
    1. Conventional users and application programs that ask for data or modify data.
    2. A database administrator.

CHAPTER 1.2.1 Data-Definition Language Commands
  - schema-altering data-definition language (DDL) commands are parsed by a DDL processor and passed to the execution engine,
     which then goes through the index/file/record manager to alter the metadata

CHAPTER 1.2.2 Overview of Query Processing
  - The query is parsed and optimized by a query compiler.
  - Resulting query plan passed to the execution engine.
  - Queries and other DML actions are grouped into transactions,
    which are units that must be executed atomically and in isolation from one another.
  - They must also be durable
  - We divide the transaction processor into two major parts:
    1. A concurrency-control manager, or scheduler, responsible for assuring atomicity and isolation of transactions
    2. A logging and recovery manager, responsible for the durability of transactions.

CHAPTER 1.2.3 Storage and Buffer Management
  - The storage manager controls the placement of data on disk and its movement between disk and main memory.
  - The storage manager keeps track of the location of files on the disk and obtains the block or blocks containing a file on request from the buffer manager.
  - The buffer manager is responsible for partitioning the available main memory into buffers

CHAPTER 1.2.4 Transaction Processing
  - Transaction: a unit of work that must be executed atomically and in apparent isolation from other transactions.
  - The transaction processor performs the following tasks:
    1. Logging: every change in the database is logged separately on disk. A recovery manager will be able to examine the
              log of changes and restore the database to some consistent state.
    2. Concurrency control: Transactions must appear to execute in isolation.  A typical scheduler does its work by maintaining
              locks on certain pieces of the database. Locks are generally stored in a main-memory lock table.
    3. Deadlock resolution: cancel transaction in case of deadlock

CHAPTER 1.2.5 The Query Processor
  Query processor is represented by two components:
    1. The query compiler often using relational algebra. Query compiler consists of three major units:
        a) A query parser, which builds a tree structure from the textual form of the query.
        b) A query preprocessor, which performs semantic checks on the query
        c) A query optimizer that changes the initial query into the best available sequence of operations on the data.
    2. The execution engine which executes each of the steps in the chosen query plan It needs to interact with the scheduler
       to avoid accessing data that is locked, and with the log manager to make sure that all database changes are properly
       logged.

Chapter 2.2 Basics of the Relational Mode
  - A two-dimensional table called a relation.

Chapter 2.2.1 Attributes
  - The columns of a relation

Chapter 2.2.2 Schemas
  - The name of a relation and the set of attributes for a relation is called the schema for that relation.
  - The attributes in a relation schema are a set, not a list.

Chapter 2.2.3 Tuples
  - The rows of a relation not including the header row that specifies Attributes.

Chapter 2.2.4 Domains
  - The relational model requires that each component of each tuple be atomic meaning that it must be of some elementary type such as integer or string.
  - Atomic values cannot be record structures meaning its values cannot be broken into smaller types.
  - It is possible to include the domain, or data type, for each attribute in a relation schema.

Chapter 2.2.5 Equivalent Representations of a Relation
  - Order in which tuples are presented is not important

Chapter 2.2.6 Relation Instances
  - Relations change over time
  - It is less common for schema of a relation to change
  - A set of tuples for a given relation an instance of that relation.

Chapter 2.2.7 Keys of Relations
  - A set of attributes forms a key for a relation if we do not allow two tuples in a relation instance to have the same values in all the attributes
     of the key.
  - We indicate the attribute or attributes that form a key for a relation by underlining.

Chapter 2.2.8 An Example Database Schema
  -  It is not usual to assume names of persons are unique and therefore suitable as a key.

Chapter 2.3 Defining a Relation Schema in SQL
  - Two aspects to SQL:
    1. Data-Definition sublanguage for declaring database schemas
    2. The Data-Manipulation sublanguage for querying

Chapter 2.3.1 Relations in SQL
  - SQL makes a distinction between three kinds of relations:
    1. Stored relations, which are called tables
    2. Views, which are relations defined by a computation. Not stored but are constructed in part or whole when needed.
    3. Temporary table: constructed when SQL language processor preforms its job of executing queries and data modifications. These are not stored and thrown away.
  - The SQL CREATE TABLE statement declares the schema for a stored relation.

Chapter 2.3.2 Data Types
  - All attributes must have a data type.
  - Primitive Data types:
    1. Character strings of fixed or varying length.
      a) CHAR(n): fixed length
      b) VARCHAR(n): up to n characters
    2. Bit Strings of fixed or varying length. Their values are strings of bits rather than characters.
      a) BIT(n) : bit strings of length n
      b) BIT VARYING(n): bit strings of length up to n
    3. BOOLEAN: TRUE,FALSE, UNKNOWN
    4. INT or INTEGER. SHORTINT also but number of bits allowed may be less depending on implementation.
    5. Floating point numbers:
      a) REAL or FLOAT (synonyms)
      b) A higher precision : DOUBLE PRECIOSON
      c) DECIMAL(n, d): allow values to be n decimal digits with d assumed to be after decimal point
      d) NUMERIC is almost the same as DECIMAL but there are possible implementation differences.
    6. DATE, TIME.

Chapter 2.3.3 Simple Table Declarations
  - CREATE TABLE followed by the name of the relation

Chapter 2.3.4 Modifying Relation Schemas
  - We can delete a relation R by the SQL statement:
    DROP TABLE R;
  - We may need to modify the schema of an existing relation.
    1. ADD followed by an attribute name and its data type.
    2. DROP followed by an attribute name.
    ALTER TABLE Star ADD birthdate CHAR(16); (creates attribute birthdate)
    ALTER TABLE Star DROP birthdate; (deletes attribute birthdate)

Chapter 2.3.5 Default Values
  - Any place we declare an attribute and its data type, we may add the keyword DEFAULT and an appropriate value.
    ex: gender CHAR(l) DEFAULT ’?’,
    ex: birthdate DATE DEFAULT DATE ’0000-00-00’

Chapter 2.3.6 Declaring Keys
  - Two ways if a set of attributes make a key must use option 2:
    1. We may declare one attribute to be a key when that attribute is listed in the relation schema.
    2. We may add to the list of items declared in the schema
  - Two ways that we can indicate keyness:
    1. PRIMARY KEY
    2. UNIQUE
  - Primary key vs unique:
    1. If PRIMARY KEY is used, then attributes in S are not allowed to have NULL as a value for their components.
    2. NULL is permitted if the set S is declared UNIQUE but duplicate values that aren't null are not allowed!
    3. A DBMS may make other distinctions between the two terms, if it wishes.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
XML DATA NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 11.2 XML
  - Tag-based notation designed originally for “marking” documents, much like the familiar HTML.
  - XML tags are intended to talk about the meanings of pieces of the document.

Chapter 11.2.1 Semantic Tags
  - A pair of matching tags and everything that comes between them is called an element.
  - A single tag, with no matched closing tag, is also permitted in XML.
    ex: <form/>
    - This can have attributes but is not allowed to have any other elements or text.

Chapter 11.2.2 XML With and Without a Schema
- XML designed to be used in two somewhat diff modes:
  1. Well-formed XML
    - No predefined schema
    - Author free to use any tag they wish
    - Of course nesting rule for tags must be obeyed or doc not well-formed
  2. Valid XML
    - Involves a DTD (Document Type Definition)
    - This form of XML is intermediate between the strict-schema models such as the relational model, and the completely schemaless world of semistructured data.
    - Allow more flexibility in the data than does a conventional schema (often allow optional fields or missing fields for example)

Chapter 11.2.3 Well-Formed XML
- The minimal requirement for well-formed XML is that the document begin with a declaration that it is XML, and that it have a root element that is the entire body of the text.
- Well-Formed will look something like this:
  ```xml
  <? xml version = "1.0" encoding = "utf-8" standalone = "yes" ?>
  <Book>
  ***************
  </Book>
  ```
- The first like says it is XML doc.
- Standalone="yes" indicates there's no DTD for this particular document.
- Initial declaration has form <? . . . . . ?>
- <Book> here is the root.

Chapter 11.2.4 Attributes
- An attribute is an alternative way to represent a leaf node of semi-structured data.
  ex: <Movie title = "Star Wars" year = 1977></Movie>

Chapter 11.2.5 Attributes That Connect Elements
  - Important use for attributes is to represent connections in a semi-structured data graph

Chapter 11.2.6 Namespaces
  - To say that an element’s tag should be interpreted as part of a certain namespace, we can use the attribute xmlns in its opening tag.
    ex: xmlns: name="URI”

Chapter 11.2.7 XML and Databases
  - Two approaches to storing XML to provide some efficiency:
    1. Store the XML data in a parsed form, and provide a library of tools to navigate the data in that form.
    2. Represent the documents and their elements as relations and use conventional relational DBMS to store them.

Chapter 11.3 Document Type Definitions
  - The description of the schema is given by a grammar-like set of rules, called a document type definition, or DTD.

Chapter 11.3.1 The Form of a DTD
  - The structure of a DTD is:
  ```xml
    <! DOCTYPE root-tag [
      <! ELEMENT element-name (components) >
        more elements
    ]>
  ```
  - There are two important special cases of components:
    1.(#PCDATA) (“parsed character data”) after an element name means that element has a value that is text there are no elements nested within.
      ex. <!ELEMENT Title (#PCDATA)>
      says that between <Title> and </Title> tags a character string can appear.
    2. keyword EMPTY, with no parentheses says there is no matched closing tag.

Chapter 11.3.2 Using a DTD
  - If a document is intended to conform to a certain DTD either:
    1. Include the DTD itself as a preamble to the document
    2. In the opening line, refer to the DTD

Chapter 11.3.3 Attribute Lists
  -  A declaration of the form:
    <!ATTLIST element-name attribute-name type >
    says that the named attribute can be an attribute of the named element, and that the type of this attribute is the indicated type.
  - The most common type for attributes is CDATA. This type is essentially character-string data with special characters like < escaped as in #PCDATA.
  - Following the data type there can be a keyword #REQUIRED or #IMPLIED, which means that the attribute must be present, or is optional, respectively.
  - Another option is an enumerated type, which is a list of possible strings, surrounded by parentheses and separated by |’s.

Chapter 11.3.4 Identifiers and References
  - Certain attributes can be used as identifiers for elements. In a DTD, we give these attributes the type ID.
  - Other attributes have values that are references to these element ID’s; these attributes may be declared to have type IDREF.
  - The value of an IDREF attribute must also be the value of some ID attribute of some element
  - IDREF is basically a pointer to the ID
  - An alternative is to give an attribute the type IDREFS. In that case, the value of the attribute is a string consisting of a list of ID’s

Chapter 11.4 XML Schema
  - XML Schema is an alternative way to provide a schema for XML documents.
  - Allows arbitrary restriction on the number of occurrences of sub elements.
  - Allows declaration of types.
  - Allows the ability to declare keys and foreign keys.

Chapter 11.4.1 The Form of an XML Schema
  - An XML Schema description of a schema is itself an XML document. It uses the namespace at the URL:
    http://www.w3.org/2001/XMLSchema
  - Each XML-Schema document thus has the form:
    ```xml
    <? xml version = "1.0" encoding = "utf-8" ?> <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
    . . . . . . . . . . . . . . . . . . . . . . . . .
    </xs:schema>
    ```
    - First line indicates XML
    - Second line indicates that this is the root tag for the document that is the schema.
    - The attribute xmlns (XML namespace) makes the variable xs stand for the namespace for XML Schema.

Chapter 11.4.2 Elements
  - The form of an element definition in XML Schema is:
  ```xml
  <xs: element name = element name type = element type >
    constraints and/or structure information
  </xs:element>
  ```

Chapter 11.4.3 Complex Types
- The most common is a sequence of elements. These elements are required to occur in the sequence given, but the number of
  repetitions of each element can be controlled by attributes minOccurs and maxOccurs
- xs:all indicated that the tag and its matching closing tag must occur in any order exactly once Each
- xs:choice would mean that exactly one of the elements found between the opening <xs:choice> tag and its matching closing tag will appear.

Chapter 11.4.4 Attributes
  - A complex type can have attributes
  - Notation:
  ```xml
    <xs: attribute name = attribute name type = type name
      other information about the attribute />
  ```    
  - Example:
  ```xml
    <xs:attribute name = "year" type = "xs:integer" default="0" use = "required" />
  ```

  - Attribute definitions are placed within a complex-type definition.

Chapter 11.4.5 Restricted Simple Types
  - It is possible to create a restricted version of a simple type such as integer or string by limiting the values the type can take.
  - notation:
  ```xml
    <xs: simpleType name = type name >
      <xs:restriction base = base type >
        upper and/or lower bounds
      </xs:restriction>
    </xs:simpleType>
  ```
  - Our second way to restrict a simple type is to provide an enumeration of values.
    enumeration notation (haha):
  ```xml
    <xs: enumeration value = some value />
  ```

Chapter 11.4.6 Keys in XML Schema
  - FORM:
  ```xml
  <xs:key name = key name >
    <xs: selector xpath = path description >
    <xs:field xpath = path description >
  </xs:key>
  ```

Chapter 11.4.7 Foreign Keys in XML Schema
  - We can also declare that an element has, a field or fields that serve as a reference to the key for some other element.
  - FORM:
  ```xml
    <xs:keyref name = foreign-key name refer = key name >
      <xs: selector xpath = path description >
      <xs: field xpath = path description >
    </xs:keyref>
  ```
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
JSON NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
  - NONE
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
RELATIONAL ALGEBRA NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 2.4 An Algebraic Query Language
  - Relational algebra, that consists of some simple but powerful ways to construct new relations from given relations.
  - Relational algebra is not used today as a query language in commercial DBMS’s, although some of the early prototypes did use this algebra directly.
  - The “real” query language, SQL, incorporates relational algebra at its center, and many SQL programs are really “syntactically sugared” expressions of relational algebra.

Chapter 2.4.1 Why Do We Need a Special Query Language?
  - Relational algebra is useful because it is less powerful than C or Java.
  - By limiting what we can say or do in our query language, we get two huge rewards — ease of programming and the ability of the compiler to produce highly optimized code

Chapter 2.4.2 What is an Algebra?
  - Relational algebra is another example of an algebra. Its atomic operands are:
    1. Variables that stand for relations.
    2. Constants, which are finite relations.

Chapter 2.4.3 Overview of Relational Algebra
  - Operations of traditional relational algebra:
    a) The usual set operations — union, intersection, and difference — applied to relations.
    b) Operations that remove parts of a relation: “selection” eliminates some rows, and “projection” eliminates some columns.
    c) Operations that combine the tuples of two relations
    d) An operation called “renaming” that does not affect the tuples of a relation, but changes the relation schema (names of attributes/relation name)
  - Expressions of relational algebra care referred to as queries

Chapter 2.4.4 Set Operations on Relations
  - Most common operations, where R and S are sets:
    - Union : R ⋃ S (union of R and S An element appears only once in the union even if it is present in both R and S.)
    - Intersection : R ⋂ S (elements in both R and S)
    - Difference : R - S (Elements in R but not in S. S - S /= S - R)
  - When we apply these operations to relations we must put some conditions on S and R:
    1. R and S must have schemas with identical sets of attributes, and the types (domains) for each attribute must be the same in R and S.
    2. The columns of R and S must be ordered so that the order of attributes is the same for both relations before we compute the union, intersection or difference.

Chapter 2.4.5 Projection
  - The projection operator is used to produce from a relation R a new relation that has only some of R’s columns.
  - π(A1, A2, A3 ... An)(R) is a relation that has only columns for attributes A1, A2, A3 ... An of R.  

Chapter 2.4.6 Selection
  - The selection operator, applied to a relation R, produces a new relation with a subset of R’s tuples.
  - σ(c)(R)
  - σ(length > 2 AND Color = 'red')(R)

Chapter 2.4.7 Cartesian Product
  - This product is denoted R x S.
  - The relation schema for the resulting relation is the union of the schemas for R and S.
  - If R and S should happen to have some attributes in common, then we need to invent new names for at least one of each pair of identical attributes.

Chapter 2.4.8 Natural Joins
  - Simplest join is the natural join of two relations R and S
  - R ⋈ S : we pair only those tuples from R and S that agree in whatever attributes are common to the schemas of R and S.

Chapter 2.4.9 Theta-Joins
  - The notation for a theta-join of relations R and S based on condition C is R ⋈(c) S.
    1. Take the product of R and S.
    2. Select from the product only those tuples that satisfy the condition C.
  - The theta-join contrasts with natural join, since in the latter common attributes are merged into one copy.

Chapter 2.4.10 Combining Operations to Form Queries
  - Expression trees are evaluated bottom-up by applying the operator at an interior node to the arguments, which are the results of its children.
  - There is often more than one relational algebra expression that represents the same computation.
  - An important job of the query “optimizer” is to replace one expression of relational algebra by an equivalent expression that is more efficiently evaluated.

Chapter 2.4.11 Naming and Renaming
  - It is often convenient to use an operator that explicitly renames relations.
  - Rename operator: ρ
  - ρs(A1,A2,... ,An)(R) to rename a relation R.
  - If we only want to change the name of the relation to S and leave the attributes as they are in R, we can just say ρs(R)
  - R x ρs(X ,C ,D )(S) is the relation R x S

Chapter 2.4.12 Relationships Among Operations
  - Some of the operations can be expressed in terms of other relational-algebra operations.
  - R ⋂ S = R - (R - S)
  - R ⋈c S = σc(R x S)
  - R x S : R.A1 = S.A1 AND R.A2 = S.A2 AND . . . AND R.An = S.An

Chapter 2.4.13 A Linear Notation for Algebraic Expressions
  - An alternative is to invent names for the temporary relations that correspond to the interior nodes of the tree and write a sequence of assignments that create a value for each.
  - The notation for assignment statements is:
    1. A relation name and parenthesized list of attributes for that relation. The name Answer will be used conventionally for the result of the final step; i.e., the name of the relation at the root of the expression tree.
    2. The assignment symbol :=.
    3. Any algebraic expression on the right. We can choose to use only one operator per assignment, in which case each interior node of the tree gets its own assignment statement.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
SQL NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 5.2.7 Outerjoins
  - A property of the join operator is that it is possible for certain tuples to be “dangling”; that is, they fail to match any tuple of the other relation in the common attributes.
  - Dangling tuples do not have any trace in the result of the join, so the join may not represent the data of the original relations completely.
  - The left outerjoin R ⋈l S is like the outerjoin, but only dangling tuples of the left argument R are padded with ⊥. and added to the result.
  - The right outerjoin R ⋈r S is like the outerjoin, but only the dangling tuples of the right argument S are padded with ⊥ and added to the result.
  - All three natural outerjoin operators have theta-join analogs, where first a theta-join is taken and then those tuples that failed to join with any tuple of the other relation, when the condition of the theta-join was applied, are padded with ⊥ and added to the result.

Chapter 6.1 Simple Queries in SQL
  - The FROM clause gives the relation or relations to which the query refers.
  - The WHERE clause is a condition, much like a selection-condition in relational algebra.
  - The SELECT clause tells which attributes of the tuples matching the condition are produced as part of the answer.

Chapter 6.1.1 Projection in SQL
  - SQL is case insensitive, meaning that it treats upper- and lower-case letters as the same letter.
  - Only inside quotes does SQL make a distinction between upper- and lower-case letters.
  - Thus, ’FROM’ and ’from’ are different character strings.

Chapter 6.1.2 Selection in SQL
  - The selection operator of relational algebra, and much more, is available through the WHERE clause of SQL.
  - We may build expressions by comparing values using the six common comparison operators: =, <>, <, >, <=, and >=.
  - We may apply the concatenation operator || to strings
  - AND takes precedence over OR, and NOT takes precedence over both.
  - A string of bits is represented by B followed by a quoted string of 0’s and l’s.
  - Hexadecimal notation may also be used, where an X is followed by a quoted string of hexadecimal digits (0 through 9, and a through f)

Chapter 6.1.3 Comparison of Strings
  - When we compare strings by one of the “less than” operators, such as < or >=, we are asking whether one precedes the other in lexicographic order
  - ’bar’ < ’bargain’ because the former is a proper prefix of the latter.

Chapter 6.1.4 Pattern Matching in SQL
  -  An alternative form of comparison expression is s LIKE p
  - Where s is a string and p is a pattern, that is, a string with the optional use of the two special characters %, and _ .

Chapter 6.1.5 Dates and Times
  - Implementations of SQL generally support dates and times as special data types.
  - A date constant is represented by the keyword DATE followed by a quoted string of a special form. For example, DATE ’1948-05-14’ follows the required form.
  - A time constant is represented similarly by the keyword TIME and a quoted string.
  - For instance, TIME ’15:00:02.5’ represents the time at which all students will have left a class that ends at 3 PM: two and a half seconds past three o’clock.
  - To combine dates and times we use a value of type TIMESTAMP.
  - Thus, TIMESTAMP ’1948-05-14 12:00:00’ represents noon on May 14, 1948.
  - We can compare dates or times using the same comparison operators we use for numbers or strings.

Chapter 6.1.6 Null Values and Comparisons Involving NULL
  - There are many different interpretations that can be put on null values.
    1. Value unknown
    2. Value inapplicable
    3. Value withheld
  - SQL allows outerjoins and also produces NULL’s when a query involves outerjoins
  - There are two important rules to remember when we operate upon a NULL value:
    1. When we operate on a NULL and any value, including another NULL, using an arithmetic operator like x or +, the result is NULL.
    2. When we compare a NULL value and any value, including another NULL, using a comparison operator like = or >, the result is UNKNOWN.
  - Although NULL is a value that can appear in tuples, it is not a constant.

Chapter 6.1.7 The Truth-Value UNKNOWN
  - We might reason that 0 * x surely has the value 0, since no matter what integer x is, its product with 0 is 0. However, if x has the value NULL, the product of 0 and NULL is NULL.
  -  Similarly, we might reason that x - x has the value 0, since whatever integer x is, its difference with itself is 0. However, again the rule about operations on nulls applies, and the result is NULL.

Chapter 6.1.8 Ordering the Output
  - ORDER BY ```<list of attributes> ```
  - To get the movies listed by length, shortest first, and among movies of equal length, alphabetically, we can say:
  ```SQL
    SELECT *
    FROM Movies
    WHERE studioName = ’Disney’ AND year = 1990
    ORDER BY length, title;
  ```
  - A subtlety of ordering is that all the attributes of Movies are available at the time of sorting, even if they are not part of the SELECT clause. Thus, we could replace SELECT * by SELECT producerC#, and the query would still be legal.
  - An additional option in ordering is that the list following ORDER BY can include expressions, just as the SELECT clause can. For instance, we can order the tuples of a relation R(A, B) by the sum of the two components of the tuples, highest first, with:
  ```SQL
    SELECT *
    FROM R
    ORDER BY A+B DESC;
  ```

Chapter 6.2 Queries Involving More Than One Relation
  - Much of the power of relational algebra comes from its ability to combine two or more relations through joins, products, unions, intersections, and differences.

Chapter 6.2.1 Products and Joins in SQL
  - SQL has a simple way to couple relations in one query: list each relation in the FROM clause. Then, the SELECT and WHERE clauses can refer to the attributes of any of the relations in the FROM clause.

Chapter 6.2.2 Disambiguating Attributes
  ```SQL
    SELECT MovieStar.name, MovieExec.name
    FROM MovieStar, MovieExec
    WHERE MovieStar.address = MovieExec.address;
  ```
  - The relation name, followed by a dot, is permissible even in situations where there is no ambiguity.

Chapter 6.2.3 Tuple Variables
  - SQL allows us to define, for each occurrence of R in the FROM clause, an “alias” which we shall refer to as a tuple variable.
  - In the SELECT and WHERE clauses, we can disambiguate attributes of R by preceding them by the appropriate tuple variable and a dot.
  - Two stars who share an address:
    ```SQL
      SELECT Starl.name, Star2.name
      FROM MovieStar Starl, MovieStar Star2
      WHERE Starl.address = Star2.address
            AND Starl.name < Star2.name;
    ```
    - If we used <> (not-equal) as the comparison operator, then we would produce pairs of married stars twice

Chapter 6.2.4 Interpreting Multirelation Queries

Chapter 6.2.5 Union, Intersection, and Difference of Queries
  - UNION, INTERSECT, and EXCEPT for U, ∩, and —, respectively.
    - Ex: Suppose we wanted the names and addresses of all female movie stars who are also movie executives with a net worth over $10,000,000. Using the following two relations:
      - MovieStar(name, address, gender, birthdate)
      - MovieExec(name, address, cert#, netWorth)
      ```SQL
        (SELECT name, address  
         FROM MovieStar
         WHERE gender = ’F’)
            INTERSECT  
        (SELECT name, address  
         FROM MovieExec  
         WHERE netWorth > 10000000);
      ```
      ```SQL
        (SELECT name, address FROM MovieStar)
          EXCEPT
        (SELECT name, address FROM MovieExec);
      ```
      - gives the names and addresses of movie stars who are not also movie executives, regardless of gender or net worth.
    - Ex: All the titles and years of movies that appeared in either the Movies or StarsIn relation:
      - Movies(title, year, length, genre, studioName, producerC#)
      - StarsIn(movieTitle, movieYear, starName)
      ```SQL
      (SELECT title, year FROM Movie)
        UNION
      (SELECT movieTitle AS title, movieYear AS year FROM Starsln);  
      ```

Chapter 6.3 Subqueries
  - A query that is part of another is called a subquery.
    1. Subqueries can return a single constant, and this constant can be compared with another value in a WHERE clause.
    2. Subqueries can return relations that can be used in various ways in WHERE clauses.
    3. Subqueries can appear in FROM clauses, followed by a tuple variable that represents the tuples in the result of the subquery.

Chapter 6.3.1 Subqueries that Produce Scalar Values
  - An atomic value that can appear as one component of a tuple is referred to as a scalar.
  - Ex: producer of Star Wars. We had to query the two relations
    - Movies(title, year, length, genre, studioName, producerC#)
    - MovieExec(name, address, cert#, netWorth)
    ```SQL
    SELECT name
    FROM MovieExec
    WHERE cert# =
      (SELECT producerC#
      FROM Movies  
      WHERE title = ’Star Wars’
      );
    ```

Chapter 6.3.2 Conditions Involving Relations
  1. EXISTS R is a condition that is true if and only if R is not empty.
  2. s IN R is true if and only if s is equal to one of the values in R. s NOT IN R is true if and only if s is equal to no value in R.
  3. s > ALL R is true if and only if s is greater than every value in unary relation R.
  4. s > ANY R is true if and only if s is greater than at least one value in unary relation R.
  - The EXISTS, ALL, and ANY operators can be negated by putting NOT in front of the entire expression, NOT s >= ALL R is true if and only if s is not the maximum value in R, and NOT s > ANY R is true if and only if s is the minimum value in R.

Chapter 6.3.3 Conditions Involving Tuples
  - Ex: Finding the producers of Harrison Ford’s movies:
  ```SQL
  SELECT name
  FROM MovieExec
  WHERE cert# IN
    (SELECT producerC#
     FROM Movies
     WHERE (title, year) IN
        (SELECT movieTitle, movieYear
         FROM Starsln
         WHERE starName = ’Harrison Ford’
        )
    );
  ```

Chapter 6.3.4 Correlated Subqueries
  - Ex: Finding movie titles that appear more than once
  ```SQL
  SELECT title
  FROM Movies Old
  WHERE year < ANY
    (SELECT year
      FROM Movies
      WHERE title = Old.title
    );
  ```

Chapter 6.3.5 Subqueries in FROM Clauses   
  - Another use for subqueries is as relations in a FROM clause. In a FROM list, instead of a stored relation, we may use a parenthesized subquery.
  - Ex: Finding the producers of Ford’s movies using a subquery in the FROM clause
  ```SQL
  SELECT name
  FROM MovieExec, (SELECT producerC#
                  FROM Movies, Starsln
                  WHERE title = movieTitle AND
                        year = movieYear AND
                        starName = ’Harrison Ford’
                  ) Prod
  WHERE cert# = Prod.producerC#;      
  ```

Chapter 6.3.6 SQL Join Expressions
  - Variants include products, natural joins, theta- joins, and outerjoins.
  - A more conventional theta-join is obtained with the keyword ON.
  ```SQL
  SELECT title, year, length, genre, studioName,
    producerC#, starName
  FROM Movies JOIN Starsln ON
    title = movieTitle AND year = movieYear;
  ```

Chapter 6.3.7 Natural Joins
  - A natural join differs from a theta-join in that:
    1. The join condition is that all pairs of attributes from the two relations having a common name are equated, and there are no other conditions.
    2. One of each pair of equated attributes is projected out.

Chapter 6.3.8 Outerjoins
  - SQL refers to the standard outerjoin, which pads dangling tuples from both of its arguments, as a full outerjoin.
    ```SQL
    MovieStar NATURAL FULL OUTER JOIN MovieExec;
    ```

Chapter 6.4 Full-Relation Operations
  - SQL uses relations that are bags rather than sets, and a tuple can appear more than once in a relation.

Chapter 6.4.1 Eliminating Duplicates
  - A relation, being a set, cannot have more than one copy of any given tuple.
  - When a SQL query creates a new relation, the SQL system does not ordinarily eliminate duplicates.
  - If we do not wish duplicates in the result, then we may follow the keyword SELECT by the keyword DISTINCT.

Chapter 6.4.2 Duplicates in Unions, Intersections, and Differences
  - The union, intersection, and difference operations, normally delete duplicates
  - In order to prevent the elimination of duplicates, we must follow the operator UNION, INTERSECT, or EXCEPT by the keyword ALL.
  ```SQL
  (SELECT title, year FROM Movies)
    UNION ALL
  (SELECT movieTitle AS title, movieYear AS year FROM Starsln);
  ```
  - Now, a title and year will appear as many times in the result as it appears in each of the relations Movies and Starsln put together.

Chapter 6.4.3 Grouping and Aggregation in SQL
  - SQL provides all the capability of the 7 operator through the use of aggregation operators in SELECT clauses and a special GROUP BY clause.

Chapter 6.4.4 Aggregation Operators
  - SQL uses the five aggregation operators SUM, AVG, MIN, MAX, and COUNT
  - We have the option of eliminating duplicates from the column before applying the aggregation operator by using the keyword DISTINCT.
  - COUNT (DISTINCT x) counts the number of distinct values in column x.
  - We could use any of the other operators in place of COUNT here, but expressions such as SUM (DISTINCT x) rarely make sense
  - Ex: the average net worth of all movie executives
    ```SQL
    SELECT AVG(netWorth)
    FROM MovieExec;
    ```
  - Ex: Count the number of tuples in the StarsIn relation
    ```SQL
    SELECT COUNT(*)
    FROM Starsln;
    ```
  - Ex: counts the number of values in the starName column of the relation.
    ```SQL
    SELECT COUNT(starName)
    FROM Starsln;
    ```
  - Since duplicate values are not eliminated when we project onto the starName column in SQL, this count should be the same as the count produced by the query with COUNT( * ).
  - Ex: count each star once:
    ```SQL
    SELECT COUNT(DISTINCT starName)
    FROM Starsln;
    ```

Chapter 6.4.5 Grouping
  - Ex: the sum of the lengths of all movies for each studio
  ```SQL
  SELECT studioName, SUM(length)
  FROM Movies
  GROUP BY studioName;
  ```
  - Ex: Same queries
  ```SQL
  SELECT studioName
  FROM Movies
  GROUP BY studioName;
  ```
  ```SQL
  SELECT DISTINCT studioName
  FROM Movies;
  ```
  - It is also possible to use a GROUP BY clause in a query about several relations interpreted in following sequence of steps.
    1. Evaluate the relation R expressed by the FROM and WHERE clauses. That is, relation R is the Cartesian product of the relations mentioned in the FROM clause, to which the selection of the WHERE clause is applied.
    2. Group the tuples of R according to the attributes in the GROUP BY clause.
    3. Produce as a result the attributes and aggregations of the SELECT clause, as if the query were about a stored relation R.
  - Ex: Computing the length of movies for each producer
  ```SQL
  SELECT name, SUM(length)
  FROM MovieExec, Movies
  WHERE producerC# = cert#
  GROUP BY name;
  ```

Chapter 6.4.6 Grouping, Aggregation, and Nulls
  - When tuples have nulls, there are a few rules we must remember:
    - The value NULL is ignored in any aggregation. It does not contribute to a sum, average, or count of an attribute, nor can it be the minimum or maximum in its column.
    - On the other hand, NULL is treated as an ordinary value when forming groups. That is, we can have a group in which one or more of the grouping attributes are assigned the value NULL.
    - When we perform any aggregation except count over an empty bag of values, the result is NULL. The count of an empty bag is 0.
  - Suppose we have a relation R(A, B) with one tuple, both of whose components are NULL
    - Then the result of the following query is the one tuple (NULL, 0).
    ```SQL
    SELECT A, COUNT(B)
    FROM R
    GROUP BY A;
    ```
    - The result of is the one tuple (NULL, NULL)
    ```SQL
    SELECT A, SUM(B)
    FROM R
    GROUP BY A;
    ```
    - Thus, we are summing an empty bag of values, and this sum is defined to be NULL.
    - Order of Clauses in SQL Queries: SELECT, FROM, WHERE, GROUP BY, HAVING, and ORDER BY. Only the SELECT and FROM clauses are required. Whichever additional clauses appear must be in the order listed above.

Chapter 6.4.7 HAVING Clauses
  - The keyword HAVING followed by a condition about the group.
  - Ex: Suppose we want to print the total film length for only those producers who made at least one film prior to 1930.
  ```SQL
  HAVING MIN(year) < 1930
  ```
  - There are several rules we must remember about HAVING clauses:
    1. An aggregation in a HAVING clause applies only to the tuples of the group being tested.
    2. Any attribute of relations in the FROM clause may be aggregated in the HAVING clause, but only those attributes that are in the GROUP BY list may appear unaggregated in the HAVING clause.
  - Ex: Computing the total length of film for early producers
  ```SQL
  SELECT name, SUM(length)
  FROM MovieExec, Movies
  WHERE producerC# = cert#
  GROUP BY name
  HAVING MIN(year) < 1930;
  ```

Chapter 6.5 Database Modifications
  - Three types of modification statements:
    1. Insert tuples into a relation.
    2. Delete certain tuples from a relation.
    3. Update values of certain components of certain existing tuples.

Chapter 6.5.1 Insertion
  - The basic form of insertion statement is:
  ```SQL
  INSERT INTO R(A1,... , An) VALUES (vi,... , vn);
  ```
  - If the list of attributes does not include all attributes of the relation R, then the tuple created has default values for all missing attributes.
  - Ex: add Sydney Greenstreet to the list of stars of The Maltese Falcon.
  ```SQL
  INSERT INTO StarsIn(movieTitle, movieYear, starName)
  VALUES(’The Maltese Falcon’, 1942, ’Sydney Greenstreet’);
  ```
  - If, we provide values for all attributes of the relation, then we may omit the list of attributes that follows the relation name.
  ```SQL
  INSERT INTO StarsIn
  VALUES(’The Maltese Falcon’, 1942, ’Sydney Greenstreet’);
  ```
  - If we take the option above, we must be sure that the order of the values is the same as the standard order of attributes for the relation.
  - If you are not sure of the declared order for the attributes, it is best to list them in the INSERT clause in the order you choose for their values in the VALUES clause.

Chapter 6.5.2 Deletion
  - The form of a deletion is
  ```SQL
  DELETE FROM R WHERE <condition>
  ```
  - The effect of executing this statement is that every tuple satisfying the condition will be deleted from relation R.
  - Unlike the insertion statement we cannot simply specify a tuple to be deleted. Rather, we must describe the tuple exactly by a WHERE clause.
  ```SQL
  DELETE FROM Starsln
  WHERE movieTitle = ’The Maltese Falcon’
        AND movieYear = 1942
        AND starName = ’Sydney Greenstreet’
  ```

Chapter 6.5.3 Updates
  - The general form of an update statement is:
  ```SQL
  UPDATE R SET <new-value assignments> WHERE <condition>;
  ```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
XPATH AND XQUERY NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 12 Programming Languages for XML
  - XPath, is a simple language for describing sets of similar paths in a graph of semistructured data.
  - XQuery is an extension of XPath that adopts something of the style of SQL
  - XSLT: this language was developed originally as a transformation language, capable of restructuring XML documents or turning them into printable (HTML) documents.

Chapter 12.1 XPath

Chapter 12.1.1 The XPath Data Model
  - In XPath, the analogous “shape” is sequence of items. An item is either:
    1. A value of primitive type: integer, real, boolean, or string, for example.
    2. A node. There are many kinds of nodes, for now :
      - Documents. These are files containing an XML document
      - Elements. These are XML elements, including their opening tags, their matched closing tag if there is one, and everything in between
      - Attributes. These are found inside opening tags

Chapter 12.1.2 Document Nodes
  -  We can name a file either by giving its local name or a URL if it is remote.
  -  examples of document nodes include:
  ```XPath
  doc("movies.xml")
  doc("/usr/sally/data/movies. xml")
  doc("infolab.Stanford.edu/~hector/movies.xml")
  ```
  - Every XPath query refers to a document.

Chapter 12.1.3 Path Expressions
  - Typically, an XPath expression starts at the root of a document and gives a sequence of tags and slashes (/)
  - As a special case, the root tag T1 for the document is considered a “subelement” of the document node. Thus, the expression /T1 produces a sequence of one item, which is an element node consisting of the entire contents of the document.
  - The difference may appear subtle; before we applied the expression /T1, we had a document node representing the file, and after applying /T1 to that node we have an element node representing the text in the file.

Chapter 12.1.4 Relative Path Expressions
  - Relative expressions do not start with a slash.

Chapter 12.1.5 Attributes in Path Expressions
  - The path expression returns the attribute starID
  ```
  /StarMovieData/Star/@starID
  ```

Chapter 12.1.6 Axes
  - XPath in fact provides a large number of axes, which are modes of navigation.
  - Some of the other axes are parent, ancestor (really a proper ancestor), descendant (a proper descendant), next-sibling (any sibling to the right), previous- sibling (any sibling to the left), self, and descendant-or-self.

Chapter 12.1.7 Context of Expressions
  - Results of expressions are sequences of elements or primitive values.

Chapter 12.1.8 Wildcards
  - Instead of specifying a tag along every step of a path, we can use a * to say “any tag.”

Chapter 12.1.9 Conditions in Path Expressions
  - As we evaluate a path expression, we can restrict ourselves to follow only a subset of the paths whose tags match the tags in the expression.
  - Several other useful forms of condition are:
    - An integer [i] by itself is true only when applied the ith child of its parent.
    - A tag [T] by itself is true only for elements that have one or more subelements with tag T.
    - Similarly, an attribute [A] by itself is true only for elements that have a value for the attribute A.
  - Interesting : ```/Movies/Movie/Version[1]/@year ``` asks for the year in which the first version of each movie was made, and the result is the sequence "1933" "1984".
  - ```/Movies/Movie/Version[Star]``` applied to the document returns three Version elements. The condition [Star] is interpreted as “has at least one Star subelement.”

Chapter 12.2 XQuery
  - XQuery is an extension of XPath that has become a standard for high-level querying of databases containing data in XML form.
  - XQuery is case sensitive. Therefore let and for need to be written in lower case.

Chapter 12.2.1 XQuery Basics
   - XQuery uses the same model for values as XPath
   - XQuery is a functional language, which implies that any XQuery expression can be used in any place that an expression is expected.

Chapter 12.2.2 FLWR Expressions
  - Clauses of four types, called for-, let-, where-, and return- (FLWR) clauses.
  - There are options in the order and occurrences of these clauses.
    1. The query begins with zero or more for- and let-clauses. There can be more than one of each kind, and they can be interlaced in any order.
    2. Then comes an optional where-clause
    3. Finally, there is exactly one return-clause.
  - The simple form of a let-clause is: ```let variable := expression```
  - The simple form of a for-clause is: ```for variable in expression```
  - The form of a where-clause is: ```where condition```
  - The form of the return clause is: ```return expression```
    - We should not think of the return-clause as a “return-statement,” since it does not end processing of the query.

Chapter 12.2.3 Replacement of Variables by Their Values
  - Example :  Putting tags around a sequence
  ```XQUERY
  let $starSeq := (
      let $movies := doc("movies.xml")
      for $m in $movies/Movies/Movie
      return $m/Version/Star
  )
  return <Stars>{$starSeq}</Stars>
  ```

Chapter 12.2.4 Joins in XQuery
  - In SQL, we use a from-clause to introduce the needed tuple variables while in XQuery we use a for-clause.
  - An element is not equal to a different element, even if it looks the same, character-by-character.
  - There is a built-in function data(E) that extracts the value of an element E.

Chapter 12.2.5 XQuery Comparison Operators
  - XQuery provides a set of comparison operators that only compare sequences consisting of a single item, and fail if either operand is a sequence of more than one item.
  - These operators are two-letter abbreviations for the comparisons: eq, ne, It, gt, le, and ge. We could use eq in place of = to catch the case where we are actually comparing a string with several streets or cities.
  - Example
  ```
  let $stars := doc ("stairs, xml")
  for $s in $stars/Stars/Star
  where $s/Address/Street eq "123 Maple St." and
        $s/Address/City eq "Malibu"
  return $s/Name
  ```
  - Unfortunately, the above will not report any star with two or more addresses, even if one of those addresses is 123 Maple St., Malibu because the left sides of the eq operator are not single items, and therefore the comparison fails.  

Chapter 12.2.6 Elimination of Duplicates
  - Strictly speaking, distinct-values applies to primitive types. It will strip the tags from an element that is a tagged text-string, but it won’t put them back.
  - The input to distinct-values can be a list of elements and the result a list of strings.

Chapter 12.2.7 Quantification in XQuery
  - “for all” and “there exists”:
  ```XQUERY
  every variable in expressionl satisfies expression2
  some variable in expressionl satisfies expression2
  ```
  - It is rarely necessary to use the “some” version, since most tests in XQuery are existentially quantified anyway.

Chapter 12.2.8 Aggregations
  - They can be applied to the result of any XQuery expression.

Chapter 12.2.9 Branching in XQuery Expressions
  - if-then-else expression in XQuery of the form :
  ```XQUERY
  if (expressionl) then expression2 else expression3
  ```
  - To evaluate this expression, first evaluate expressionl; if it is true, evaluate expression2, which becomes the result of the whole expression. If expressionl is false, the result of the whole expression is expressions.
  - This expression is not a statement — there are no statements in XQuery, only expressions.
  - Like the ?: expression in C, there is no way to omit the “else” part. However, we can use as expressions the empty sequence, which is denoted ().
  - Example : Tagging the versions of King Kong
  ```XQUERY
  let $kk := doc("movies.xml")/Movies/Movie[@title = "King Kong"]
  for $v in $kk/Version
  return
        if ($v/@year = max($kk/Version/@year))
        then <Latest>{$v}</Latest>
        else <01d>{$v}</01d>
  ```

Chapter 12.2.10 Ordering the Result of a Query
  - It is possible to sort the results as part of a FLWR query, if we add an order- clause before the return-clause. ```order list of expressions ```

Chapter 12.3 Extensible Stylesheet Language
  - XSLT (Extensible Stylesheet Language for Transformations) is a standard of the World-Wide-Web Consortium.
  - Like XPath or XQuery, we can use XSLT to extract data from documents or turn one document form into another form.

Chapter 12.3.1 XSLT Basics
  - Like XML Schema, XSLT specifications are XML documents; these specifications are usually called stylesheets.
  - The tags used in XSLT are found in a namespace, which is http://www.w3.org/1999/XSL/Transform.
  - Example:
  ```XSLT
  <? xml version = "1.0" encoding = "utf-8" ?>
  <xsl:stylesheet xmlns:xsl = "http://www.w3.org/1999/XSL/Transform">
  ...
  ...
  ...
  </xsl:stylesheet>
  ```

Chapter 12.3.2 Templates
  - A stylesheet will have one or more templates. To apply a stylesheet to an XML document, we go down the list of templates until we find one that matches the root.
  - Simplest Template : ``` <xsl:template match = "XPath expression">  ```
  - The XPath expression, which can be either rooted (beginning with a slash) or relative, describes the elements of an XML document to which this template is applied.
  -  Relative expressions are applied when a template T has within it a tag ```<xsl:apply-templates>```.

Chapter 12.3.3 Obtaining Values From XML Data
  - The simplest way to extract data from the input is with the value-of tag. ```<xsl:value-of select = "expression" / > ```

Chapter 12.3.4 Recursive Use of Templates
  - We can ask that a template be applied to each of its subelements, by using the apply-templates tag.
  - If we want to apply a certain template to only some subset of the subelements, e.g., those with a certain tag, we can use a select expression, as: ```<xsl:apply-templates select = "expression" /> ```
   - Example :
   ```XSLT
   <? xml version = "1.0" encoding = "utf-8" ?>
   <xsl:stylesheet xmlns:xsl = "http://www.w3.org/1999/XSL/Transform">
      <xsl:template match = "/Movies">
        <Movies>
          <xsl:apply-templates />
        </Movies>
      </xsl:template>

      <xsl:template match = "Movie">
        <Movie title = " <xsl:value-of select = "@title” /> ">
          <xsl:apply-templates />
        </Movie>
      </xsl:template>

      <xsl:template match = "Version">
        <xsl:apply-templates />
       </xsl:template>

      <xsl:template match = "Star">
        <Star name = " <xsl:value-of select = "." /> " />
      </xsl:template>
   </xsl:stylesheet>
   ```
   - Output:
   ```XSLT
   <Movies>
    <Movie title = "King Kong">
      <Star name = "Fay Wray" />
      <Star name = "Jeff Bridges" />
      <Star name = "Jessica Lange" />
    </Movie>
    <Movie title = "Footloose">
      <Star name = "Kevin Bacon" />
      <Star name = "John Lithgow" />
      <Star name = "Sarah Jessica Parker" />
    </Movie>
        ... more movies
    </Movies>
   ```

Chapter 12.3.5 Iteration in XSLT
   - We can put a loop within a template that gives us freedom over the order in which we visit certain subelements of the element to which the template is being applied.
   - The for-each tag creates the loop, with a form: ```<xsl:for-each select = "expression">```

Chapter 12.3.6 Conditionals in XSLT
  -  The form of this tag is: ```<xsl:if test = "boolean expression">```

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
