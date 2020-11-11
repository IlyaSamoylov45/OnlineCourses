- XML Schema allows us a way to give content specific specifications for our XML data.

XML SCHEMA (XSD)
  - Extensive language
  - Like DTDs can specify elements, attributes, nesting, ordering, # of occurrences
  - Also data types, keys, (typed pointers), and more (unlike in DTDs)
  - XSD is written in the XML language itself unlike DTDs

FOUR FEATURES IN XML SCHEMA NOT IN DTD's
  1. Typed values
     - In DTD all attributes are treated as strings
     - This is not the case with XSD
  2. Key declarations: similar to IDs but more powerful
    - In DTD's we were able to specify ID's which were globally unique values
      that could be used to identify specific elements
    - keys are more powerful, since we can specify that a particular attribute or 
      component must be unique within every element of the same type
  3. References: Similar to pointer data but more powerful
    - Key References
    - Allow us to have what are effectively typed pointers
  4. Occurrence Constraints
    - Specify how many times an element type is allowed to occurrences
    - Default is 1
    - minOccurs and maxOccurs
