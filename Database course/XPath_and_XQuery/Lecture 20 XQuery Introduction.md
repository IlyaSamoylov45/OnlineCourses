Lecture 20 XQuery Introduction
  - XQuery
    - Expression language (compositional)
    - Each expression operates on and returns a sequence of elements
    - XPath is one type of expression
  - XQuery : FLWOR expression
    ```
    FOR $var in expr <- iterator variables
    LET $var := expr <- assignment
    WHERE condition <- filter
    ORDER BY <- sorts result
    RETURN expr
    ```
    - All except return are optional
    - FOR and LET can be repeated and interleaved
  - Mixing Queries and XML
    - ```<Result>{ . . . QUERY GOES HERE . . . }</Result>```
