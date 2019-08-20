Integrity Constraints
  - Impose restrictions on allowable data, beyond those imposed by structure and types
  - Referential Integrity : Integrity of references = No "dangling pointers"

Referential Integrity from R.A to S.B
  - Each value in column A of table R must appear in column B of table S.
    - A is called the "foreign key" : Foreign Key constraints
    - B is usually required to be the primary key for table S or at least unique
    - Multi-attribute foreign keys are allowed

Referential Integrity Enforcement (R.A to S.B)
  - Potentially violating modifications
    - Insert into R
    - Delete from S
    - Update R.A
    - Update S.B
    - If there is a violation -> error
  - Special actions
    - Delete from S
      - Restrict (default), set null, cascade
    - Update S.B
      - Restrict (default), set null. cascade
      
