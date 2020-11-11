Lecture 20 XQuery Introduction
  - XQuery
    - Expression language (compositional)
    - Each expression operates on and returns a sequence of elements
    - XPath is one type of expression
  - XQuery : FLWOR expression
    ```
    for $var in expr <- iterator variables
    let $var := expr <- assignment
    where condition <- filter
    order by <- sorts result
    return expr
    ```
    - All except return are optional
    - FOR and LET can be repeated and interleaved
  - Mixing Queries and XML
    - ```<Result>{ . . . QUERY GOES HERE . . . }</Result>```
