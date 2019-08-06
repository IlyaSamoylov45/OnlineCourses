Multivalued dependencies & 4th Normal Form Part 2
  - Apply(SSN, cName, hobby)
    - SSN ↠ cName

    | -- | SSN  | cName  | hobby  |
    | -- | -- | -- | -- |                          
    | t | 123  | Stanford  | Trumpet  |
    | u | 123  | Berkley  | Tennis  |
    | v | 123  | Stanford  | Tennis  |
    | w | 123  | Berkley  | Trumpet  |
    | ... | ...  | ...  | ...  |
  - Modified example
    - Apply(SSN, cName, hobby)
      - Reveal hobbies to college selectively
      - MVDs None
      - Good Design? Yes
  - Expanded example :
    - Apply(SSN, cName, date, major, hobby)
      - Reveal hobbies to Colleges selectively
      - Apply once to each college per day
      - May apply to multiple majors
      - SSN, cName → date
      - SSN, cName, date ↠ major
