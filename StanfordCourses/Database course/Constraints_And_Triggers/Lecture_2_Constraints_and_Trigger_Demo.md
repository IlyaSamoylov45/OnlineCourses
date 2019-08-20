Integrity Constraints
  - Impose restrictions on allowable data, beyond those imposed by structure and type.
    - Non-null constraints
    - Key constraints
    - Attribute based and tuple based constraints
    - General assertions (not implemented)

Constraints Demo
  - Simple college admissions database
    - College(cName, state, enrollment)
    - Student(sID, sName, GPA, sizeHS)
    - Apply(sID, cName, major, decision)

More info
  - Immediate constraint checking and deferred constraint checking
  - Currently no SQL system actually supports subqueries in check constraints
  
