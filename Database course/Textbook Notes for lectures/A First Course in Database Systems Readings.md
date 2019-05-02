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
      <xs: enumeration value = some value />

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
RELATIONAL ALGEBRA NOTES
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
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
