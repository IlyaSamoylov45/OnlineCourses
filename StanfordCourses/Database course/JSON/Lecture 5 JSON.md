JSON

JavaScript Object Notation(JSON)
  - Standard for "serializing" data objects, usually in files.
  - Human-readable like XML, usable for data interchange
  - Also useful for representing and storing semi-structured data.
  - Semi-structured Data.
  - Newest out of RDS, XML.
  - Javascript Object Notation.
  - Good for storing data that doesn't have rigid structure.
  - No longer tied to JS
  - Parsers exist for many languages.

Basic Constructs(recursive)
  - Base values
    - number, string, boolean, ...
  - Objects : {}
    - Sets of label - value pairs
  - Arrays []
    - lists of values

JSON vs Relational

|                  | Relational                   |                    JSON                  |
| :---             |           :---:              |                                     ---: |
| Structure        | Tables                       | Nested sets Arrays                       |
| Schema           | Fixed in advance             | "Self describing flexible"               |
| Queries          | Simple Expressive languages  | Not widely used                          |
| Ordering         | None                         | Arrays                                   |
| Implementation   | Native System                | Coupled with other programming languages |

JSON vs XML

|                       |            XML              |              JSON                |
| :---                  |           :---:             |                             ---: |
| Verbosity             | More                        | Less                             |
| Complexity            | More                        | Less                             |
| Validity              | DTDs, XSDs (widely used)    | JSON schema (not widely used)    |
| Programming interface | Clunky "Impedance Mismatch" | More direct                      |
| Querying              | Xpath, XQuery, XSLT         | JSON Path, JSON Query            |

Syntactically Valid JSON
  - Adheres to basic structural requirements
    - Sets of label-value pairs
    - Arrays of values
    - Base values from predefined types
