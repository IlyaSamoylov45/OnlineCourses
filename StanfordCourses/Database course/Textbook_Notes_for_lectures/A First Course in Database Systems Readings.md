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
  - The first important applications of DBMS's were ones where data was composed of many small items
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
    ex: gender CHAR(l) DEFAULT '?',
    ex: birthdate DATE DEFAULT DATE '0000-00-00'

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
  - To say that an element's tag should be interpreted as part of a certain namespace, we can use the attribute xmlns in its opening tag.
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
  - Another option is an enumerated type, which is a list of possible strings, surrounded by parentheses and separated by |'s.

Chapter 11.3.4 Identifiers and References
  - Certain attributes can be used as identifiers for elements. In a DTD, we give these attributes the type ID.
  - Other attributes have values that are references to these element ID's; these attributes may be declared to have type IDREF.
  - The value of an IDREF attribute must also be the value of some ID attribute of some element
  - IDREF is basically a pointer to the ID
  - An alternative is to give an attribute the type IDREFS. In that case, the value of the attribute is a string consisting of a list of ID's

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
  - Relational algebra is not used today as a query language in commercial DBMS's, although some of the early prototypes did use this algebra directly.
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
  - The projection operator is used to produce from a relation R a new relation that has only some of R's columns.
  - π(A1, A2, A3 ... An)(R) is a relation that has only columns for attributes A1, A2, A3 ... An of R.  

Chapter 2.4.6 Selection
  - The selection operator, applied to a relation R, produces a new relation with a subset of R's tuples.
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
  - Thus, 'FROM' and 'from' are different character strings.

Chapter 6.1.2 Selection in SQL
  - The selection operator of relational algebra, and much more, is available through the WHERE clause of SQL.
  - We may build expressions by comparing values using the six common comparison operators: =, <>, <, >, <=, and >=.
  - We may apply the concatenation operator || to strings
  - AND takes precedence over OR, and NOT takes precedence over both.
  - A string of bits is represented by B followed by a quoted string of 0's and l's.
  - Hexadecimal notation may also be used, where an X is followed by a quoted string of hexadecimal digits (0 through 9, and a through f)

Chapter 6.1.3 Comparison of Strings
  - When we compare strings by one of the “less than” operators, such as < or >=, we are asking whether one precedes the other in lexicographic order
  - 'bar' < 'bargain' because the former is a proper prefix of the latter.

Chapter 6.1.4 Pattern Matching in SQL
  -  An alternative form of comparison expression is s LIKE p
  - Where s is a string and p is a pattern, that is, a string with the optional use of the two special characters %, and _ .

Chapter 6.1.5 Dates and Times
  - Implementations of SQL generally support dates and times as special data types.
  - A date constant is represented by the keyword DATE followed by a quoted string of a special form. For example, DATE '1948-05-14' follows the required form.
  - A time constant is represented similarly by the keyword TIME and a quoted string.
  - For instance, TIME '15:00:02.5' represents the time at which all students will have left a class that ends at 3 PM: two and a half seconds past three o'clock.
  - To combine dates and times we use a value of type TIMESTAMP.
  - Thus, TIMESTAMP '1948-05-14 12:00:00' represents noon on May 14, 1948.
  - We can compare dates or times using the same comparison operators we use for numbers or strings.

Chapter 6.1.6 Null Values and Comparisons Involving NULL
  - There are many different interpretations that can be put on null values.
    1. Value unknown
    2. Value inapplicable
    3. Value withheld
  - SQL allows outerjoins and also produces NULL's when a query involves outerjoins
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
    WHERE studioName = 'Disney' AND year = 1990
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
         WHERE gender = 'F')
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
      WHERE title = 'Star Wars'
      );
    ```

Chapter 6.3.2 Conditions Involving Relations
  1. EXISTS R is a condition that is true if and only if R is not empty.
  2. s IN R is true if and only if s is equal to one of the values in R. s NOT IN R is true if and only if s is equal to no value in R.
  3. s > ALL R is true if and only if s is greater than every value in unary relation R.
  4. s > ANY R is true if and only if s is greater than at least one value in unary relation R.
  - The EXISTS, ALL, and ANY operators can be negated by putting NOT in front of the entire expression, NOT s >= ALL R is true if and only if s is not the maximum value in R, and NOT s > ANY R is true if and only if s is the minimum value in R.

Chapter 6.3.3 Conditions Involving Tuples
  - Ex: Finding the producers of Harrison Ford's movies:
  ```SQL
  SELECT name
  FROM MovieExec
  WHERE cert# IN
    (SELECT producerC#
     FROM Movies
     WHERE (title, year) IN
        (SELECT movieTitle, movieYear
         FROM Starsln
         WHERE starName = 'Harrison Ford'
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
  - Ex: Finding the producers of Ford's movies using a subquery in the FROM clause
  ```SQL
  SELECT name
  FROM MovieExec, (SELECT producerC#
                  FROM Movies, Starsln
                  WHERE title = movieTitle AND
                        year = movieYear AND
                        starName = 'Harrison Ford'
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
  VALUES('The Maltese Falcon', 1942, 'Sydney Greenstreet');
  ```
  - If, we provide values for all attributes of the relation, then we may omit the list of attributes that follows the relation name.
  ```SQL
  INSERT INTO StarsIn
  VALUES('The Maltese Falcon', 1942, 'Sydney Greenstreet');
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
  WHERE movieTitle = 'The Maltese Falcon'
        AND movieYear = 1942
        AND starName = 'Sydney Greenstreet'
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
  - Strictly speaking, distinct-values applies to primitive types. It will strip the tags from an element that is a tagged text-string, but it won't put them back.
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
  - NONE

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
RELATIONAL DESIGN THEORY NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 3 Design Theory for Relational Databases
  - Functional dependencies are a generalization of the idea of a key for a relation.

Chapter 3.1 Functional Dependencies
  - The most common constraint is the “functional dependency,” a statement of a type that generalizes the idea of a key for a relation.

Chapter 3.1.1 Definition of Functional Dependency
  - A functional dependency (FD) on a relation R is a statement of the form “If two tuples of R agree on all of the attributes A1, A2, . . . , An (i.e., the tuples have the same values in their respective components for each of these attributes), then they must also agree on all of another list of attributes B1, B2 , . . . , Bm.
  - We write this FD formally as A1, A2, . . . , An → B1, B2, . . . , Bm and say : A1, A2, . . . , An functionally determines  B1, B2, . . . , Bm.
  - It is common for the right side of an FD to be a single attribute.

Chapter 3.1.2 Keys of Relations
  - We say a set of one or more attributes {A1, A2, . . . , An} is a key for a relation R if:
    1. Those attributes functionally determine all other attributes of the relation. That is, it is impossible for two distinct tuples of R to agree on all of A1, A2, . . ., An.
    2. No proper subset of {A1, A2, . . . , An} functionally determines all other attributes of R; i.e., a key must be minimal.
  - {year, starName} is not a key because we could have a star in two movies in the same year; therefore year, starName → title is not an FD.
  - What Is “Functional” About Functional Dependencies?
    - A1, A2, . . . , An → B is called a “functional” dependency because in principle there is a function that takes a list of values, one for each of attributes A1, A2, . . . , An and produces a unique value (or no value at all) for B.

Chapter 3.1.3 Superkeys
  - A set of attributes that contains a key is called a superkey, short for “superset of a key.”
  - Note that every superkey satisfies the first condition of a key: it functionally determines all other attributes of the relation. However, a superkey need not satisfy the second condition: minimality.

Chapter 3.2 Rules About Functional Dependencies
  - Suppose we are told of a set of FD's that a relation satisfies. Often, we can deduce that the relation must satisfy certain other FD's.

Chapter 3.2.1 Reasoning About Functional Dependencies
  - FD's often can be presented in several different ways, without changing the set of legal instances of the relation. We say:
    - Two sets of FD's 5 and T are equivalent if the set of relation instances satisfying S is exactly the same as the set of relation instances satisfying T.
    - More generally, a set of FD's S follows from a set of FD's T if every relation instance that satisfies all the FD's in T also satisfies all the FD's in S.

Chapter 3.2.2 The Splitting/Combining Rule
  - A1, A2, . . . , An → B1, B2, . . . , Bm is equivalent to the set of FD's:
  ```
  A1, A2, . . . , An → B1,
  A1, A2, . . . , An → B2,
  . . .
  A1, A2, . . . , An → Bn
  ```
  - The above is why we may split attributes on the right side so that only one attribute appears on the right of each FD.
  - The equivalence above can be used in two ways
    - We can replace an FD A1, A2, . . . , An → B1, B2, . . . , Bm by a set of FD's   A1, A2, . . . , An → Bi for i = 1,2,... ,m. This transformation we call the splitting rule.
    - We can replace a set of FD's A1, A2, . . . , An → Bi for i = 1,2,... , m by the single FD A1, A2, . . . , An → B1, B2, . . . , Bm. We call this transformation the combining rule.
  - Example:
    ```
    title year → length
    title year → genre
    title year → studioName
    ```
    - is equivalent to the single FD: ```title year → length genre studioName```
  - There is no splitting rule for left sides
  - Example:
    ```
    title year → length
    ```
    - If we try to split the left side into:
    ```
    title → length
    year → length
    ```
    - Then we get two false FD's. That is, title does not functionally determine length, since there can be several movies with the same title (e.g., King Kong) but of different lengths.

Chapter 3.2.3 Trivial Functional Dependencies
  - A trivial FD has a right side that is a subset of its left side.
  - ```title year → length``` is a trivial FD, as is ```title → title```
  - There is an intermediate situation in which some, but not all, of the attributes on the right side of an FD are also on the left. This FD is not trivial, but it can be simplifed by removing from the right side of an FD those attributes that appear on the left.
    - The FD A1, A2, . . . , An → B1, B2, . . . , Bm is equivalent to A1, A2, . . . , An → C1, C2, . . . , Ck where the C's are all those B's that are not also A's.
    - This rule is called the trivial-dependency rule.

Chapter 3.2.4 Computing the Closure of Attributes
  - The closure of {A1, A2, . . . , An} under the FD's in S is the set of attributes B such that every relation that satisfies all the FD's in set S also satisfies A1, A2, . . . , An → B.
  - That is A1, A2, . . . , An → B follows from the FDs of S.
  - Algorithm for the Closure of a Set of Attributes:
    - INPUT: A set of attributes {A1, A2, . . . , An} and a set of FD's S.
    - OUTPUT: The closure {A1, A2, . . . , An}+.  
    - METHOD:
      1. If necessary, split the FD's of S, so each FD in S has a single attribute on the right.
      2. Let X be a set of attributes that eventually will become the closure. Initialize X to be {A1, A2, . . . , An}.
      3. Repeatedly search for some FD B1, B2, . . . , Bm → C such that B1, B2, . . . , Bm are in the set of attributes X, but C is not. Add C to the set X and repeat the search. Since X can only grow, and the number of attributes of any relation schema must be finite, eventually nothing more can be added to X, and this step ends.
      4. The set X, after no more attributes can be added to it, is the correct value of {A1, A2, . . . , An}+


Chapter 3.2.5 Why the Closure Algorithm Works

Chapter 3.2.6 The Transitive Rule
  - If A1, A2, . . . , An → B1, B2, . . . , Bm and B1, B2, . . . , Bm → C1, C2, . . . , Ck hold in relation R, then A1, A2, . . . , An → C1, C2, . . . , Ck also holds in R.
  ```
  title year → studioName
  studioName → studioAddr
  ```
  - The transitive rule allows us to combine the two FD's above to get a new FD:
  ```
  title year → studioAddr
  ```

Chapter 3.2.7 Closing Sets of Functional Dependencies
  - A minimal basis for a relation is a basis B that satisfies three conditions:
    - All the FD's in B have singleton right sides.
    - If any FD is removed from B, the result is no longer a basis.
    - If for any FD in B we remove one or more attributes from the left side of F, the result is no longer a basis.
    - A Complete Set of Inference Rules: there is a set of rules, called Armstrong's axioms, from which it is possible to derive any FD that follows from a given set. These axioms are:
      1. Reflexivity. If {B1, B2, . . . , Bm} ⊆ {A1, A2, . . . , An}, then A1,A2, . . . , An → B1,B2, . . . , Bm. These are what we have called trivial FD's.
      2. Augmentation. If A1, A2, . . . , An → B1, B2, . . . , Bm then A1, A2, . . ., An, C1, C2, . . ., Ck → B1, B2, . . . ,Bm, C1, C2, . . ., Ck for any set of attributes C1 C2, . . ., Ck. Since some of the C's may also be A's or B's or both, we should eliminate from the left side duplicate attributes and do the same for the right side.
      3. Transitivity. If A1, A2, . . . , An → B1, B2, . . . , Bm andB1, B2, . . . , Bm → C1, C2, . . ., Ck then A1, A2, . . . , An → C1, C2, . . . , Ck.

Chapter 3.2.8 Projecting Functional Dependencies
  - Algorithm Projecting a Set of Functional Dependencies:
    - INPUT: A relation R and a second relation R1 computed by the projection R1 = πL( R ). Also, a set of FD's S that hold in R.
    - OUTPUT: The set of FD's that hold in R1
    - METHOD:
      1. Let T be the eventual output set of FD's. Initially, T is empty.
      2. For each set of attributes X that is a subset of the attributes of R1, compute X+. This computation is performed with respect to the set of FD's S, and may involve attributes that are in the schema of R but not R1. Add to T all nontrivial FD's X → A such that A is both in X+ and an attribute of R1.
      3. Now, T is a basis for the FD's that hold in R1, but may not be a minimal basis. We may construct a minimal basis by modifying T as follows:
        - a) If there is an FD F in T that follows from the other FD's in T, remove F from T.
        - b) Let Y → B be an FD in T, with at least two attributes in Y, and let Z be Y with one of its attributes removed. If Z → B follows from the FD's in T (including Y → B), then replace Y → B by Z → B.
        - c) Repeat the above steps in all possible ways until no more changes to T can be made

Chapter 3.3 Design of Relational Database Schemas
  - Careless selection of a relational database schema can lead to redundancy and related anomalies.

Chapter 3.3.1 Anomalies
  - Problems such as redundancy that occur when we try to cram too much into a single relation axe called anomalies. The principal kinds of anomalies that we encounter are:
    1. Redundancy. Information may be repeated unnecessarily in several tuples.
    2. Update Anomalies. We may change information in one tuple but leave the same information unchanged in another.
    3. Deletion Anomalies. If a set of values becomes empty, we may lose other information as a side effect.

Chapter 3.3.2 Decomposing Relations
  - The accepted way to eliminate these anomalies is to decompose relations.
  - Decomposition of R involves splitting the attributes of R to make the schemas of two new relations.
  - Given a relation R(A1, A2, . . . , An), we may decompose R into two relations S(B1, B2, . . . , Bm) and T(C1, C2, . . ., Ck) such that:
    1. {A1, A2, . . . , An} = {B1, B2, . . . , Bm} U {C1, C2, . . ., Ck}.
    2. S = π(B1, B2, . . . , Bm)(R)
    3. T = π(C1, C2, . . . , Ck)(R)

Chapter 3.3.3 Boyce-Codd Normal Form
  - The goal of decomposition is to replace a relation by several that do not exhibit anomalies. There is, it turns out, a simple condition under which the anomalies discussed above can be guaranteed not to exist. This condition is called Boyce-Codd normal form, or BCNF.
  - A relation R is in BCNF if and only if: whenever there is a nontrivial FD A1, A2, . . . , An → B1, B2, . . . , Bm for R, it is the case that {A1, A2, . . . , An} is a superkey for R.

Chapter 3.3.4 Decomposition into BCNF
  - By repeatedly choosing suitable decompositions, we can break any relation schema into a collection of subsets of its attributes with the following important properties:
   1. These subsets are the schemas of relations in BCNF.
   2. The data in the original relation is represented faithfully by the data in the relations that are the result of the decomposition. Roughly, we need to be able to reconstruct the original relation instance exactly from the decomposed relation instances.
  - In general, we must keep applying the decomposition rule as many times as needed, until all our relations are in BCNF. We can be sure of ultimate success, because every time we apply the decomposition rule to a relation R, the two resulting schemas each have fewer attributes than that of R.
  - Algorithm BCNF Decomposition:
    - INPUT: A relation R0 with a set of functional dependencies S0
    - OUTPUT: A decomposition of R0 into a collection of relations, all of which are in BCNF.
    - METHOD: The following steps can be applied recursively to any relation R and set of FD's S. Initially, apply them with R = R0 and S = S0.
      1. Check whether R is in BCNF. If so, nothing more needs to be done. Return {R} as the answer.  
      2. If there are BCNF violations, let one be X → Y. Compute X . Choose R1 = X+ as one relation schema and let R2 have attributes X and those attributes of R that are not in X+.
      3. Compute the sets of FD's for R1 and R2, let these be S1 and S2, respectively.
      4. Recursively decompose R1 and R2 using this algorithm. Return the union of the results of these decompositions.

Chapter 3.4 Decomposition: The Good, Bad, and Ugly
  - Three distinct properties we would like a decomposition to have:
    1. Elimination of Anomalies by decomposition
    2. Recoverability of Information. Can we recover the original relation from the tuples in its decomposition?
    3. Preservation of Dependencies. If we check the projected FD's in the relations of the decomposition, can we can be sure that when we reconstruct the original relation from the decomposition by joining, the result will satisfy the original FD's?

Chapter 3.4.1 Recovering Information from a Decomposition
  - If we decompose using the algorithm where all decompositions are motivated by a BCNF-violating FD, then the projections of the original tuples can be joined again to produce all and only the original tuples.
  - There is an algorithm called the “chase,” for testing whether the projection of a relation onto any decomposition allows us to recover the relation by rejoining.
  - Example of chase:
    - Consider a relation R{A, B, C) and an FD B → C that is a BCNF violation. The decomposition based on the FD B → C separates the attributes into relations R1(A, B) and R2 (B,C).
    - Let t be a tuple of R. We may write t = (a, b, c), where a, b, and c are the components of t for attributes A, B, and C, respectively. Tuple t projects as (a, b) in R1 (A,B) = π a,b(R) and as (b, c) in R2 (B,C) = π b,c(R).
    - When we compute the natural join Ri ix R2, these two projected tuples join, because they agree on the common B component (they both have b there). They give us t = (a. b, c). the tuple we started with, in the join.
    - However, getting back those tuples we started with is not enough to assure that the original relation R is truly represented by the decomposition.
    - Why?
      - Consider what happens if there are two tuples of R, say t = (a,b,c) and v = (d,b,e). When we project t onto R1 (A. B) we get u = (a, b), and when we project v onto R2(B,C) we get w = (b,e). These tuples also match in the natural join, and the resulting tuple is x = (a,b,e). Is it possible that a: is a bogus tuple? That is, could (a, b, e) not be a tuple of R? Since we assume the FD B → C for relation R, the answer is "no."
      - Recall that this FD says any two tuples of R that agree in their B components must also agree in their C components. Since t and v agree in their B components, they also agree on their C components. That means c = e; i.e., the two values we supposed were different are really the same. Thus, tuple (a, b, e) of R is really (a, b, c); that is, x = t.
  - If we decompose a relation then the original relation can be recovered exactly by the natural join.
  - The natural join is associative and commutative, so the order in which we perform the natural join of the decomposition components does not matter.

Chapter 3.4.2 The Chase Test for Lossless Join
  - Three important things to remember are:
    - The natural join is associative and commutative. It does not matter in what order we join the projections; we shall get the same relation as a result. In particular, the result is the set of tuples t such that for all i = 1, 2, . . ., k, t projected onto the set of attributes Si is a tuple in πS1(R).
    - Any tuple t in R is surely in πS1(R) ⋈ πS2(R) ⋈ . . . ⋈ πSk(R). The reason is that the projection of t onto Si is surely in πSi (R) for each i, and therefore by our first point above, t is in the result of the join.
    - As a consequence, πS1(R) ⋈ πS2(R) ⋈ . . . ⋈ πSk(R) = R when the FD's in F hold for R if and only if every tuple in the join is also in R. That is, the membership test is all we need to verify that the decomposition has a lossless join.
  - The chase test for a lossless join is just an organized way to see whether a tuple t in πS1(R) ⋈ πS2(R) ⋈ . . . ⋈ πSk(R) can be proved, using the FD's in F, also to be a tuple in R.
  - Example of chase algorithm:

    | A  | B  | C  | D  |
    | -- | -- | -- | -- |                           
    | a  | b1  | c1  | d  |
    | a  | b2  | c  | d2  |
    | a3  | b  | c  | d  |
    - Suppose the given FD's are A → B, B → C, and CD → A.
    - the FD A → B tells us they must also agree in their B-components. That is, b1 = b2.

    | A  | B  | C  | D  |
    | -- | -- | -- | -- |                           
    | a  | b1  | c1  | d  |
    | a  | b1  | c  | d2  |
    | a3  | b  | c  | d  |
    - FD B → C to deduce that their C-components, c1 and c, are the same.

    | A  | B  | C  | D  |
    | -- | -- | -- | -- |                           
    | a  | b1  | c  | d  |
    | a  | b1  | c  | d2  |
    | a3  | b  | c  | d  |
    - FD CD → A to deduce that these rows also have the same A-value; that is, a = a3.

    | A  | B  | C  | D  |
    | -- | -- | -- | -- |                           
    | a  | b1  | c  | d  |
    | a  | b1  | c  | d2  |
    | a  | b  | c  | d  |
    - At this point, we see that the last row has become equal to t, that is, (a,b,c,d).
    - We have proved that if R satisfies the FD's A → B, B → C, and CD → A, then whenever we project onto {A. D}, {A, C}, and {B,C,D} and rejoin, what we get must have been in R. In particular, what we get is the same as the tuple of R that we projected onto {B,C,D}.

Chapter 3.4.3 Why the Chase Works
  - There are two issues to address:
    1. When the chase results in a row that matches the tuple t (i.e., the tableau is shown to have a row with all unsubscripted variables), why must the join be lossless?
      - The chase process itself is a proof that one of the projected tuples from R must in fact be the tuple t that is produced by the join. We also know that every tuple in R is sure to come back if we project and join. Thus, the chase has proved that the result of projection and join is exactly R.
    2. When, after applying FD's whenever we can, we still find no row of all unsubscripted variables, why must the join not be lossless?
      - Suppose that we eventually derive a tableau without an unsubscripted row, and that this tableau does not allow us to apply any of the FD's to equate any symbols.
      - Then think of the tableau as an instance of the relation R. It obviously satisfies the given FD's, because none can be applied to equate symbols.
      - We know that the ith row has unsubscripted symbols in the attributes of Si, the * th relation of the decomposition. Thus, when we project this relation onto the Si's and take the natural join, we get the tuple with all unsubscripted variables. This tuple is not in R, so we conclude that the join is not lossless.
  - Example:
    - Consider the relation R(A,B,C,D) with the FD B → AD and the proposed decomposition {A, B}, {B, C}, and {C, D}. Here is the initial tableau:

      | A  | B  | C  | D  |
      | -- | -- | -- | -- |                           
      | a  | b  | c1  | d1  |
      | a2  | b  | c  | d2  |
      | a3  | b3  | c  | d  |
      - When we apply the lone FD, we deduce that a = a2 and d1 = d2. Thus, the final tableau is:

      | A  | B  | C  | D  |
      | -- | -- | -- | -- |                           
      | a  | b  | c1  | d1  |
      | a  | b  | c  | d1  |
      | a3  | b3  | c  | d  |
      - No more changes can be made because of the given FD's, and there is no row that is fully unsubscripted. Thus, this decomposition does not have a lossless join.

Chapter 3.4.4 Dependency Preservation
  - It is not possible, in some cases, to decompose a relation into BCNF relations that have both the lossless-join and dependency-preservation properties.
  - Example
    - Suppose we have a relation Bookings with attributes:
      1. ```title```, the name of a movie.
      2. ```theater```, the name of a theater where the movie is being shown.
      3. ```city```, the city where the theater is located.
    - The intent behind a tuple (m,t,c) is that the movie with title m is currently being shown at theater t in city c.
    - We might reasonably assert the following FD's:
      ```
      theater → city
      title, city → theater
      ```
    - The first says that a theater is located in one city. The second is not obvious but is based on the common practice of not booking a movie into two theaters in the same city.
    - No single attribute is a key. For example, title is not a key because a movie can play in several theaters at once and in several cities at once.
    - Also, ```theater``` is not a key, because although ```theater``` functionally determines ```city```, there are multiscreen theaters that show many movies at once. Thus, ```theater``` does not determine ```title```. Finally, ```city``` is not a key because cities usually have more than one theater and more than one movie playing.
    - On the other hand, two of the three sets of two attributes are keys. Clearly {title, city} is a key because of the given FD that says these attributes functionally determine ```theater```.
    - It is also true that {theater, title} is a key, because its closure includes city due to the given FD ```theater → city```.
    - We conclude that the only two keys are:
      ```
      {title, city}
      {theater, title}
      ```
    - Now we immediately see a BCNF violation. We were given functional dependency ```theater → city```, but its left side, theater, is not a superkey.
    - We are therefore tempted to decompose, using this BCNF-violating FD, into the two relation schemas:
      ```
      {theater, city}
      {theater, title}
      ```
    - There is a problem with this decomposition, concerning the FD
      ```
      title, city → theater
      ```
    - There could be current relations for the decomposed schemas that satisfy the FD ```theater → city``` (which can be checked in the relation {theater, city}) but that, when joined, yield a relation not satisfying title ```city → theater```.

Chapter 3.5 Third Normal Form

Chapter 3.5.1 Definition of Third Normal Form
  - A relation R is in third normal form (3NF) if:
    - Whenever A1, A2, . . . , An → B1, B2, . . . , Bm is a nontrivial FD, either {A1,A2, . . . ,An} is a superkey, or those of B1, B2, . . . , Bm that are not among the A's, are each a member of some key (not necessarily the same key).
  - An attribute that is a member of some key is often said to be prime.
  - Thus, the 3NF condition can be stated as “for each nontrivial FD, either the left side is a superkey, or the right side consists of prime attributes only.”
  - First normal form is simply the condition that every component of every tuple is an atomic value.
  - Second normal form is a less restrictive verison of 3NF.

Chapter 3.5.2 The Synthesis Algorithm for 3NF Schemas
  - How we decompose a relation R into a set of relations such that:
    - The relations of the decomposition are all in 3NF.
    - The decomposition has a lossless join.
    - The decomposition has the dependency-preservation property.
  - Algorithm : Synthesis of Third-Normal-Form Relations With a Lossless Join and Dependency Preservation.
    - INPUT:  A relation R and a set F of functional dependencies that hold for R.
    - OUTPUT: A decomposition of R into a collection of relations, each of which is in 3NF. The decomposition has the lossless-join and dependency-preservation properties.
    - METHOD:
      1. Find a minimal basis for F, say G.
      2. For each functional dependency X → A in G, use XA as the schema of one of the relations in the decomposition.
      3. If none of the relation schemas from Step 2 is a superkey for R, add another relation whose schema is a key for R.
  - Example:
    - Consider the relation R(A,B,C,D,E) with FD's AB → C, C → B, and A → D. To start, notice that the given FD's are their own minimal basis.
    - First, we need to verify that we cannot eliminate any of the given dependencies. We show that no two of the FD's imply the third. For example, we must take the closure of {A, B}, the left side of the first FD, using only the second and third FD's, C → B and A → D. This closure includes D but not C, so we conclude that the first FD AB → C is not implied by the second and third FD's.
    - We must also verify that we cannot eliminate any attributes from a left side. In this simple case, the only possibility is that we could eliminate A or B from the first FD. For example, if we eliminate A, we would be left with B → C. We must show that B → C is not implied by the three original FD's, AB → C, C → B, and A → D. With these FD's, the closure of {B} is just B, so B → C does not follow. A similar conclusion is drawn if we try to drop B from AB → C. Thus, we have our minimal basis.
    - We start the 3NF synthesis by taking the attributes of each FD as a relation schema. That is, we get relations S1(A,B,C), S2 {B, C). and S3{A,D). It is never necessary to use a relation whose schema is a proper subset of another relation's schema, so we can drop S2.
    - We must also consider whether we need to add a relation whose schema is a key. In this example, R has two keys: {A,B,E} and {A,C,E}, as you can verify. Neither of these keys is a subset of the schemas chosen so far. Thus, we must add one of them, say S4(A,B,E). The final decomposition of R is thus S1(A,B,C), S3(A,D), and S4 (A,B,E).

Chapter 3.5.3 Why the 3NF Synthesis Algorithm Works
  - These things hold:
    1. Lossless Join:
      - Consider the sequence of FD's that are used to expand K to become K+. Since K is a superkey, we know K + is all the attributes.
    2. Dependency Preservation:
      - Each FD of the minimal basis has all its attributes in some relation of the decomposition. Thus, each dependency can be checked in the decomposed relations.
    3. Third Normal Form:
      - If we have to add a relation whose schema is a key, then this relation is surely in 3NF. The reason is that all attributes of this relation are prime, and thus no violation of 3NF could be present in this relation.

Chapter 3.6 Multivalued Dependencies
  - A "multivalued dependency" is an assertion that two attributes or sets of attributes are independent of one another.
  - Every FD implies the corresponding multivalued dependency.

Chapter 3.6.1 Attribute Independence and Its Consequent Redundancy
  - There are occasional situations where we design a relation schema and find it is in BCNF, yet the relation has a kind of redundancy that is not related to FD's.
  - The most common source of redundancy in BCNF schemas is an attempt to put two or more set-valued properties of the key into a single relation.

Chapter 3.6.2 Definition of Multivalued Dependencies
  - A multivalued dependency (abbreviated MVD) is a statement about some relation R that when you fix the values for one set of attributes, then the values in certain other attributes are independent of the values of all the other attributes in the relation.
  - More precisely, we say the MVD ```A1, A2, . . . , An ↠ B1, B2, . . . , Bm``` holds for a relation R if when we restrict ourselves to the tuples of R that have particular values for each of the attributes among the A's, then the set of values we find among the B's is independent of the set of values we find among the attributes of R that are not among the A's or B's.
  - Still more precisely, we say this MVD holds if: For each pair of tuples t and u of relation R that agree on all the A's, we can find in R some tuple v that agrees:
    1. With both t and u on the A's,
    2. With t on the B's, and
    3. With u on all attributes of R that axe not among the A's or B's.
  - Example:
    ```
    name ↠ street city
    ```
    - That is, for each star's name, the set of addresses appears in conjunction with each of the star's movies.

Chapter 3.6.3 Reasoning About Multivalued Dependencies
  - There are a number of rules about MVD's that are similar to the rules we learned for FD's
  - MVD's obey:
    - Trivial MVD's. The MVD ```A1, A2, . . . , An ↠ B1, B2, . . . , Bm``` holds in any relation if ```{B1, B2, . . . , Bm} ⊆ {A1, A2, . . . , An}```.
    - The transitive rule, which says that if ```A1, A2, . . . , An ↠ B1, B2, . . . , Bm``` and ```B1, B2, . . . , Bm ↠ C1, C2, . . . , Ck``` hold for some relation, then so does ```A1, A2, . . . , An ↠ C1, C2, . . . , Ck```. Any C's that are also A's must be deleted from the right side.
  - On the other hand, MVD's do not obey the splitting part of the splitting/combining rule.
  - Example:
    - ```name ↠ street city ``` If the splitting rule applied to MVD's, we would expect ```name ↠ street``` also to be true. This MVD says that each star's street addresses are independent of the other attributes, including city. However, that statement is false.
  - There are several new rules dealing with MVD's that we can learn:
    - FD Promotion. Every FD is an MVD. That is, if ```A1, A2, . . . , An → B1, B2, . . . , Bm``` then ```A1, A2, . . . , An ↠ B1, B2, . . . , Bm```.
    - Complementation Rule. If ```A1, A2, . . . , An ↠ B1, B2, . . . , Bm``` is an MVD for relation R, then R also satisfies ```A1, A2, . . . , An ↠ C1, C2, . . . , Ck```, where the C's are all attributes of R not among the A's and B's. That is, swapping the B's between two tuples that agree in the A's has the same effect as swapping the C's.
    - More Trivial MVD's. If all the attributes of relation R are ```{A1, A2, . . . , An, B1, B2, . . . , Bm}``` then ```A1, A2, . . . , An ↠ B1, B2, . . . , Bm``` holds in R.
  - Example:
    - ```name ↠ street city``` the complementation rule says that ```name ↠ title year``` must also hold in this relation, because title and year are the attributes not mentioned in the first MVD. The second MVD intuitively means that each star has a set of movies starred in, which are independent of the star's addresses.

Chapter 3.6.4 Fourth Normal Form
  -  In this normal form, all nontrivial MVD's are eliminated, as are all FD's that violate BCNF. As a result, the decomposed relations have neither the redundancy from FD's nor the redundancy from MVD's.
  - The "fourth normal form" condition is essentially the BCNF condition, but applied to MVD's instead of FD's.
  - Formally: A relation R is in fourth normal form (4NF) if whenever ```A1, A2, . . . , An ↠ B1, B2, . . . , Bm``` is a nontrivial MVD, ```{A1, A2, . . . , An}``` is a superkey. That is, if a relation is in 4NF, then every nontrivial MVD is really an FD with a superkey on the left. Note that the notions of keys and super keys depend on FD's only; adding MVD's does not change the definition of "key".
  - Every FD is also an MVD. Thus, every BCNF violation is also a 4NF violation.
  - Every relation that is in 4NF is therefore in BCNF.

Chapter 3.6.5 Decomposition into Fourth Normal Form
  - Algorithm for Decomposition into Fourth Normal Form:
    - INPUT: A relation R0 with a set of functional and multivalued dependencies S0.
    - OUTPUT: A decomposition of R0 into relations all of which are in 4NF. The decomposition has the lossless-join property.
    - METHOD:  Do the following steps, with R = R0 and S = S0.
      1. Find a 4NF violation in R, say ```A1, A2, . . . , An ↠ B1, B2, . . . , Bm``` where ```{A1, A2, . . . , An}``` is not a superkey. Note this MVD could be a true MVD in S, or it could be derived from the corresponding FD ```A1, A2, . . . , An → B1, B2, . . . , Bm``` in S, since every FD is an MVD. If there is none, return; R by itself is a suitable decomposition.
      2. If there is such a 4NF violation, break the schema for the relation R that has the 4NF violation into two schemas:
        - R1, whose schema is A's and the B's
        - R2 , whose schema is the A's and all attributes of R that are not among the A's or B's.
      3. Find the FD's and MVD's that hold in R1 and R2. Recursively decompose R1 and R2 with respect to their projected dependencies.

Chapter 3.6.6 Relationships Among Normal Forms
  - BCNF eliminates the redundancy and other anomalies that are caused by FD's, while only 4NF eliminates the additional redundancy that is caused by the presence of MVD's that are not FD's
  - Often, 3NF is enough to eliminate this redundancy, but there are examples where it is not.
  - BCNF does not guarantee preservation of FD's, and none of the normal forms guarantee preservation of MVD's, although in typical cases the dependencies are preserved.
  - Properties of normal forms and their decompositions:

  | Property | 3NF  | BCNF  | 4NF  |
  | -- | -- | -- | -- |                           
  | Eliminates redundancy due to FD's  | NO  | YES  | YES  |
  | Eliminates redundancy due to MVD's  | NO  | NO  | YES  |
  | Preserves FD's | YES  | NO  | NO  |
  | Preserves MVD's | NO  | NO  | NO  |

Chapter 3.7 An Algorithm for Discovering MVD's
  - The closure algorithm is really the same as the chase algorithm
  - The ideas behind the chase can be extended to incorporate MVD's as well as FD's.

Chapter 3.7.1 The Closure and the Chase
  - A chase-based test for whether X → Y follows from F can be summarized as:
    1. Start with a tableau having two rows that agree only on X.
    2. Chase the tableau using the FD's of F.
    3. If the final tableau agrees in all columns of Y, then X → Y holds; otherwise it does not.
  - Example:
    - ```R(A,B,C,D,E,F)``` with FD's AB → C, BC → AD, D → E, and CF → B. We want to test whether AB → D holds. Start with the tableau:

    | A  | B  | C  | D  | E  | F  |
    | -- | -- | -- | -- | -- | -- |                          
    | a  | b  | c1  | d1  | e1  | f1  |
    | a  | b  | c2  | d2  | e2  | f2  |
    - We can apply AB → C to infer c1 = c2; say both become c1. The resulting tableau is:

    | A  | B  | C  | D  | E  | F  |
    | -- | -- | -- | -- | -- | -- |                          
    | a  | b  | c1  | d1  | e1  | f1  |
    | a  | b  | c1  | d2  | e2  | f2  |
    - Next, apply BC → AD to infer that d1 = d2, and apply D → E to infer e1 = e2. At this point, the tableau is:

    | A  | B  | C  | D  | E  | F  |
    | -- | -- | -- | -- | -- | -- |                          
    | a  | b  | c1  | d1  | e1  | f1  |
    | a  | b  | c1  | d1  | e1  | f2  |
    - and we can go no further. Since the two tuples now agree in the D column, we know that AB → D does follow from the given FD's.

Chapter 3.7.2 Extending the Chase to MVD's
  - X ↠ Y tells us that if we find two rows of the tableau that agree in X, then we can form two new tuples by swapping all their components in the attributes of Y ; the resulting two tuples must also be in the relation, and therefore in the tableau.

Chapter 3.7.3 Why the Chase Works for MVD's
  - Since the chase with MVD's adds rows to the tableau, how do we know we ever terminate the chase? Could we keep adding rows forever, never reaching our goal, but not sure that after a few more steps we would achieve that goal? Fortunately, that cannot happen.

Chapter 3.7.4 Projecting MVD's
  - Often, our search for FD's and MVD's in the projected relations does not have to be completely exhaustive. Here are some simplifications.
    1. It is surely not necessary to check the trivial FD's and MVD's.
    2. For FD's, we can restrict ourselves to looking for FD's with a singleton right side, because of the combining rule for FD's.
    3. An FD or MVD whose left side does not contain the left side of any given dependency surely cannot hold, since there is no way for its chase test to get started. That is, the two rows with which you start the test are unchanged by the given dependencies.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
UNIFIED MODELING LANGUAGE NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 4.7 Unified Modeling Language
  -  UML offers much the same capabilities as the E/R model, with the exception of multiway relationships.

Chapter 4.7.1 UML Classes
  - A class in UML is similar to an entity set in the E/R model.
  - The box for a class is divided into three parts. At the top is the name of the class. The middle has the attributes, which are like instance variables of a class. The bottom portion is for methods.
  - The UML specification doesn’t tell anything more about a method than the types of any arguments and the type of its return-value.

Chapter 4.7.2 Keys for UML classes
  - As for entity sets, we can declare one key for a UML class. To do so, we follow each attribute in the key by the letters PK, standing for “primary key.”
  - There is no convenient way to stipulate that several attributes or sets of attributes are each keys.

Chapter 4.7.3 Associations
  - A binary relationship between classes is called an association.
  - There is no analog of multiway relationships in UML. Rather, a multiway relationship has to be broken into binary relationships
  - We draw a UML association between two classes simply by drawing a line between them, and giving the line a name.
  - Every association has constraints on the number of objects from each of its classes that can be connected to an object of the other class.
  - A ```* in place of n, as in m..*```, stands for “infinity.” That is, there is no upper limit.
  - A ```* alone, in place of m..n, stands for the range 0..*```, that is, no constraint at all on the number of objects.
  - If there is no label at all at an end of an association edge, then the label is taken to be 1..1, i.e., “exactly one.”

Chapter 4.7.4 Self-Associations
  - An association can have both ends at the same class; such an association is called a self-association.

Chapter 4.7.5 Association Classes
  - We can attach attributes to an association in much the way we did in the E/R model
  - In UML, we create a new class, called an association class, and attach it to the middle of the association.

Chapter 4.7.6 Subclasses in UML
  - Any UML class can have a hierarchy of subclasses below it. The primary key comes from the root of the hierarchy, just as with E/R hierarchies. UML permits a class C to have four different kinds of subclasses below it, depending on our choices of answer to two questions
    - Complete versus Partial. Is every object in the class C a member of some subclass? If so, the subclasses are complete; otherwise they are partial or incomplete.
    - Disjoint versus Overlapping. Are the subclasses disjoint (an object cannot be in two of the subclasses)? If an object can be in two or more of the subclasses, then the subclasses are said to be overlapping.
  - In a typical object-oriented system, subclasses are disjoint. That is, no object can be in two classes. Of course they inherit properties from their parent class, so in a sense, an object also “belongs” in the parent class. However, the object may not also be in a sibling class.
  - The E/R model automatically allows overlapping subclasses.
  - Both the E/R model and object-oriented systems allow either complete or partial subclasses. That is, there is no requirement that a member of the superclass be in any subclass.
  - Subclasses are represented by rectangles, like any class.
  - To represent the class/subclass relationship in UML diagrams, we use a triangular, open arrow pointing to the superclass.

Chapter 4.7.7 Aggregations and Compositions
   - An aggregation is a line between two classes that ends in an open diamond at one end.
   - A composition is similar to an association, but the label at the diamond end must be 1..1.
   - Compositions are distinguished by making the diamond be solid black.

Chapter 4.8 From UML Diagrams to Relations
  - Many of the ideas needed to turn E/R diagrams into relations work for UML diagrams as well.

Chapter 4.8.1 UML-to-Relations Basics
  - Outline of the points
    - Classes to Relations. For each class, create a relation whose name is the name of the class, and whose attributes are the attributes of the class.
    - Associations to Relations. For each association, create a relation with the name of that association. The attributes of the relation are the key attributes of the two connected classes. If there is a coincidence of attributes between the two classes, rename them appropriately.

Chapter 4.8.2 From UML Subclasses to Relations
  - Options are “E/R style” (relations for each subclass have only the key attributes and attributes of that subclass), “object-oriented” (each entity is represented in the relation for only one subclass), and “use nulls” (one relation for all subclasses).
  - Here are some considerations:
    - If a hierarchy is disjoint at every level, then an object-oriented representation is suggested. We do not have to consider each possible tree of subclasses when forming relations, since we know that each object can belong to only one class and its ancestors in the hierarchy. Thus, there is no possibility of an exponentially exploding number of relations being created.
    - If the hierarchy is both complete and disjoint at every level, then the task is even simpler. If we use the object-oriented approach, then we have only to construct relations for the classes at the leaves of the hierarchy.
    - If the hierarchy is large and overlapping at some or all levels, then the E/R approach is indicated. We are likely to need so many relations that the relational database schema becomes unwieldy.

Chapter 4.8.3 From Aggregations and Compositions to Relations
  - We suggest that aggregations and compositions be treated routinely in this manner. Construct no relation for the aggregation or composition. Rather, add to the relation for the class at the nondiamond end the key attribute(s) of the class at the diamond end.

Chapter 4.8.4 The UML Analog of Weak Entity Sets
  - We shall use a special notation for a supporting composition: a small box attached to the weak class with “PK” in it will serve as the anchor for the supporting composition.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
INDEXES AND TRANSACTIONS NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
CONSTRAINTS AND TRIGGERS NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Chapter 7.1 Keys and Foreign Keys
  - SQL allows us to define an attribute or attributes to be a key for a relation with the keywords PRIMARY KEY or UNIQUE.

Chapter 7.1.1 Declaring Foreign-Key Constraints
  - In SQL we may declare an attribute or attributes of one relation to be a foreign key, referencing some attribute(s) of a second relation (possibly the same relation).
  - The implication of this declaration is twofold:
    - The referenced attribute(s) of the second relation must be declared UNIQUE or the PRIMARY KEY for their relation. Otherwise, we cannot make the foreign-key declaration.
    - Values of the foreign key appearing in the first relation must also appear in the referenced attributes of some tuple. More precisely, let there be a foreign-key F that references set of attributes G of some relation. Suppose a tuple t of the first relation has non-NULL values in all the attributes of F; call the list of t's values in these attributes t[F], Then in the referenced relation there must be some tuple s that agrees with t[F] on the attributes G. That is, s[G] = t[F],
  - Two ways to declare a foreign key
    - If the foreign key is a single attribute we may follow its name and type by a declaration that it “references” some attribute (which must be a key — primary or unique) of some table. The form of the declaration is ```REFERENCES <table> (<attribute>)```
    - Alternatively, we may append to the list of attributes in a CREATE TABLE statement one or more declarations stating that a set of attributes is a foreign key. We then give the table and its attributes (which must be a key) to which the foreign key refers. The form of this declaration is: ```FOREIGN KEY (<attributes>) REFERENCES <table> (<attributes>)```
  - Declare Foreign key 2 ways:
  ```SQL
  CREATE TABLE Studio (
    name CHAR(30) PRIMARY KEY,
    address VARCHAR(255),
    presC# INT REFERENCES MovieExec(cert#)
  );
  ```
  ```SQL
  CREATE TABLE Studio (
    name CHAR(30) PRIMARY KEY,
    address VARCHAR(255),
    presC# INT,
    FOREIGN KEY (presC#) REFERENCES MovieExec(cert#)
  );
  ```

Chapter 7.1.2 Maintaining Referential Integrity
  - Three alternatives to enforce a foreign-key constraint:
    - The Default Policy: Reject Violating Modifications. SQL has a default policy that any modification violating the referential integrity constraint is rejected.
    - The Cascade Policy. Under this policy, changes to the referenced attribute (s) are mimicked at the foreign key.
    - The Set-Null Policy. Here, when a modification to the referenced relation affects a foreign-key value, the latter is changed to NULL.
  - Example: preserve referential integrity
  ```SQL
  CREATE TABLE Studio (
    name CHAR(30) PRIMARY KEY,
    address VARCHAR(255),
    presC# INT REFERENCES MovieExec(cert#)
      ON DELETE SET NULL
      ON UPDATE CASCADE
  );
  ```

Chapter 7.1.3 Deferred Checking of Constraints
  - If we declare a constraint to be DEFERRABLE, then we have the option of having it wait until a transaction is complete before checking the constraint.
  - We follow the keyword DEFERRABLE by either INITIALLY DEFERRED or INITIALLY IMMEDIATE. In the former case, checking will be deferred to just before each transaction commits. In the latter case, the check will be made immediately after each statement.
  - Checking of its foreign-key constraint to be deferred until the end of each transaction:
  ```SQL
  CREATE TABLE Studio (
    name CHAR(30) PRIMARY KEY,
    address VARCHAR(255),
    presC# INT UNIQUE
      REFERENCES MovieExec(cert#)
      DEFERRABLE INITIALLY DEFERRED
  );
  ```
  - Two additional points about deferring constraints:
    - Constraints of any type can be given names.
    - If a constraint has a name, say MyConstraint, then we can change a deferrable constraint from immediate to deferred by the SQL statement ```SET CONSTRAINT MyConstraint DEFERRED;``` and we can reverse the process by replacing DEFERRED in the above to IMMEDIATE.

Chapter 7.2 Constraints on Attributes and Tuples
  - Within a SQL CREATE TABLE statement, we can declare two kinds of constraints:
    - A constraint on a single attribute.
    - A constraint on a tuple as a whole.

Chapter 7.2.1 Not-Null Constraints
  - One simple constraint to associate with an attribute is NOT NULL.
  - The constraint is declared by the keywords NOT NULL following the declaration of the attribute in a CREATE TABLE statement.

Chapter 7.2.2 Attribute-Based CHECK Constraints
  - In practice, an attribute-based CHECK constraint is likely to be a simple limit on values, such as an enumeration of legal values or an arithmetic inequality.
  - An attribute-based CHECK constraint is checked whenever any tuple gets a new value for this attribute.
  - In the case of an update, the constraint is checked on the new value, not the old value. If the constraint is violated by the new value, then the modification is rejected.

Chapter 7.2.3 Tuple-Based CHECK Constraints
  - The careful database designer will use attribute- and tuple-based checks only when there is no possibility that they will be violated, and will use another mechanism, such as assertions or triggers otherwise.
  - Like an attribute-based CHECK, a tuple-based CHECK is invisible to other relations.
  - A constraint on the table MovieStar:
  ```SQL
  CREATE TABLE MovieStar (
    name CHAR(30) PRIMARY KEY,
    address VARCHAR(255),
    gender CHAR(l),
    birthdate DATE,
    CHECK (gender = ’F’ OR name NOT LIKE ’Ms.%’)
  );
  ```

Chapter 7.2.4 Comparison of Tuple- and Attribute-Based Constraints
  - If a constraint on a tuple involves more than one attribute of that tuple, then it must be written as a tuple-based constraint. However, if the constraint involves only one attribute of the tuple, then it can be written as either a tuple- or attribute-based constraint.

Chapter 7.5 Triggers
  - Triggers, sometimes called event-condition-action rules or ECA rules, differ from the kinds of constraints discussed previously in three ways.
    - Triggers are only awakened when certain events, specified by the database programmer, occur. The sorts of events allowed are usually insert, delete, or update to a particular relation.
    - Once awakened by its triggering event, the trigger tests a condition. If the condition does not hold, then nothing else associated with the trigger happens in response to this event.
    - If the condition of the trigger is satisfied, the action associated with the trigger is performed by the DBMS. A possible action is to modify the effects of the event in some way, even aborting the transaction of which the event is part. However, the action could be any sequence of database operations, including operations not connected in any way to the triggering event.

Chapter 7.5.1 Triggers in SQL
  - Here are the principal features.
    - The check of the trigger’s condition and the action of the trigger may be executed either on the state of the database (i.e., the current instances of all the relations) that exists before the triggering event is itself executed or on the state that exists after the triggering event is executed.
    - The condition and action can refer to both old and/or new values of tuples that were updated in the triggering event.
    - It is possible to define update events that are limited to a particular attribute or set of attributes.
    - The programmer has an option of specifying that the trigger executes either:
      - Once for each modified tuple (a row-level trigger), or
      - Once for all the tuples that are changed in one SQL statement (a statement-level trigger, remember that one SQL modification statement can affect many tuples).
  - The key elements and the order in which they appear:
    - The CREATE TRIGGER statement
    - A clause indicating the triggering event and telling whether the trigger uses the database state before or after the triggering event
    - A REFERENCING clause to allow the condition and action of the trigger to refer to the tuple being modified. In the case of an update, such as this one, this clause allows us to give names to the tuple both before and after the change.
    - A clause telling whether the trigger executes once for each modified row or once for all the modifications made by one SQL statement
    - The condition, which uses the keyword WHEN and a boolean expression
    - The action, consisting of one or more SQL statements
  - Example of above:
  ```SQL
  CREATE TRIGGER NetWorthTrigger
  AFTER UPDATE OF netWorth ON MovieExec
  REFERENCING
    OLD ROW AS OldTuple,
    NEW ROW AS NewTuple
  FOR EACH ROW  
  WHEN (OldTuple.netWorth > NewTuple.netWorth)
      UPDATE MovieExec
      SET netWorth = OldTuple.netWorth
      WHERE cert# = NewTuple.cert#;
  ```
  - The phrase FOR EACH ROW, expresses the requirement that this trigger is executed once for each updated tuple.

Chapter 7.5.2 The Options for Trigger Design
  - A statement-level trigger is executed once whenever a statement of the appropriate type is executed, no matter how many rows — zero, one, or many — it actually affects.
  - Trigger example: Constraining the average net worth
  ```SQL
  CREATE TRIGGER AvgNetWorthTrigger
  AFTER UPDATE OF netWorth ON MovieExec
  REFERENCING
    OLD TABLE AS OldStuff,
    NEW TABLE AS NewStuff
  FOR EACH STATEMENT
  WHEN (500000 > (SELECT AVG(netWorth) FROM MovieExec))
  BEGIN
    DELETE FROM MovieExec
    WHERE (name, address, cert#, netWorth) IN NewStuff;
    INSERT INTO MovieExec
      (SELECT * FROM OldStuff);
  END;
  ```
  - An important use of BEFORE triggers is to fix up the inserted tuples in some way before they are inserted.
  - Trigger Example : Fixing NULL's in inserted tuples
  ```SQL
  CREATE TRIGGER FixYearTrigger
  BEFORE INSERT ON Movies
  REFERENCING
    NEW ROW AS NewRow
    NEW TABLE AS NewStuff
  FOR EACH ROW
  WHEN NewRow.year IS NULL
  UPDATE NewStuff SET year = 1915;
  ```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
VIEWS AND AUTHORIZATION NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
ON-LINE ANALYTICAL PROCESSING NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
RECURSION IN SQL NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
