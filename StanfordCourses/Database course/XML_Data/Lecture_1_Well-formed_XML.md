EXTENSIBLE MARKUP LANGUAGE (XML)

- Standard for data representation and exchange
- Document format similar to HTML
  - Tags describe the content of the data rather than how to format the data
- Also Streaming format

WELLFORMED XML

- Attributes
  - Any number of attributes as long as they're unique
  - format:
    name = attribute
- In order to be well-formed it must adhere to these rules:
  - single root elements
  - Matched tags(meaning no open tags without closing tags), proper nesting
  - unique attributes within elements
- An XML parser can be used to check if the document is well-formed
- There are various standards for how we show parsed XML
  - DOM (document object model)
  - SAX (More of a stream model for XML)

DISPLAYING XML

- We want to format XML in more intuitive way
- Use rule based language to translate HTML
  - Cascading stylesheets (CSS)
  - Extensive stylesheet language (XSL)
- XML -> CSS/XSL interpreter -> HTML document

RELATIONAL MODEL VS XML
                    RELATIONAL                XML
Structure:          Tables                    Hierarchical, Tree, Graph
Schema:             Fixed in advance          Flexible, "Self-Describing"
Queries:            Simple, nice language     Less so
Ordering:           Unordered                 Implied Ordering
Implementation:     Native                    ADD-ON

- Self describing: schema and data kind of mixed together
- Schemas in XML are optional
- In XML its acceptable to have some attributes for some elements and those attributes don't appear in other elements
- Usually XML is not the native model of the system itself
- XML is good for databases that have an unpredictable, complex, dynamic structure so the flexibility of XML is a plus.

EXCERCISES/QUESTIONS

Q1: You're creating a database to contain information about university records: students, courses, grades, etc. Should you use the relational model or XML?
A: Relational

Q2:You're creating a database to contain information for a university web site: news, academic announcements,
   admissions, events, research, etc. Should you use the relational model or XML?
A: XML

Q3:You're creating a database to contain information about family trees (ancestry). Should you use the relational model or XML?
A: Either XML or Relational
