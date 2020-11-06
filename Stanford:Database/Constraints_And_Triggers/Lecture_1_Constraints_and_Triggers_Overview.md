Constraints and Triggers Overview
  - For relational databases
  - SQL standard; systems vary considerably

(Integrity) Constraints (static)
  - Constrain allowable databases states

Triggers (dynamic)
  - Monitor database changes
  - Check conditions and initiate actions.

Integrity Constraints
  - Impose restrictions on allowable data, beyond those imposed by structure and types
  - Examples:
    - 0.0 < GPA <= 4.0
    - enrollment < 50,000 -> 75,000
    - decision : 'y', 'n', null
    - major = 'cs' -> decision = null
    - sizeHS < 200 -> not admitted enr > 30000
  - Why not use them?
    - Data-entry errors (inserts)
    - Correctness criteria (updates)
    - Enforce consistency
    - Tell system about data-store, query processing
  - Classification
    - Non-null
    - Key
    - Referential integrity (Foreign key)
    - Tuple-based
    - General Assertions

Declaring and enforcing constraints
  - Declaration
    - With original schema : checked after bulk loading
    - Or later - checked on current DB
  - Enforcement
    - Check after every "dangerous" modification
    - Deferred constraint checking (transaction)

Triggers
  - "Event-Condition-Action Rules"
    - When event occurs, check condition; if true do action.
  - Examples:
    - Enrollment > 35000 -> reject all application
    - Insert app with GPA > 3.95 -> Accept automatically
    - Update sizeHS to be 7000 -> change to "wrong" raise error
  - Why use them?
    - More logic from apps into DBMS
    - To enforce constraints
      - Expressiveness
      - Constraint "repair" logic
  - General Trigger Syntax
  ```
  Create Trigger name
  Before | After | Instead of events
  [referencing-variables]
  [For Each Row]
  when (condition)
  action
  ```

Constraints and Triggers
  - For relational databases
  - SQL standard; systems vary considerably
  - (Integrity) constraints
    - Constrain allowable database states
  - Triggers
    - Monitor database changes
    - Check conditions and initiate actions
