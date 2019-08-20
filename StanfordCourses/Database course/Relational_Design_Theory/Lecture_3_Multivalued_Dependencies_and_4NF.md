Multivalued dependencies and 4NF
  - Apply(SSN, cName, HS)
    - Redundancy, update and deletion anomalies
    - Multiplicative effect (c Colleges, h High schools, c * h tuples)
    - Not addressed by BCNF: No functional dependencies
  - Multivalued Dependency SSN ↠ cName
    - Given SSN has every combination of cName with HS
    - Should store each cName and each HS for an SSN once
  - Fourth Normal Form
    - If A ↠ B then A is a key
    - Decompose : Apply (SSN, cName), HighSchool(SSN, HS)
      - c + h instead of c * h
      
