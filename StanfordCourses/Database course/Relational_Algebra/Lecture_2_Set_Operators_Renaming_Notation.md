Relational Algebra : Set operators, Renaming, Notation
  - Relational Algebra query(expression) on set of relations produces relation as a result
  - Example College Admissions Database
    - College(<u>cName</u>, state, enrollment)
    - Student(<u>sID</u>, sName, GPA, sizeHS)
    - Apply (<u>sID</u>, <u>cName</u>, <u>major</u>, decision)
  - Set Operators
    1. Union
      - ex: List of college and student names.
        ```
        π(cName)College ∪ π(sName)Student
        ```
    2. Difference
      - ex: IDs of students who didn't apply anywhere
        ```
        π(sID)Student - π(sID)Apply
        ```
      - ex: IDs and names of students who didn't apply anywhere
        ```
        π(sname)((π(sID)Student - π(sID)Apply) ⋈ Student)
        ```
    3. Intersection operator
      - ex: Names that are both a college name and a student name
        ```
        π(cname)College ∩ π(sName)Student
        ```
      - Intersection doesn't add expressive power
        - E1 ∩ E2 ⇔ E1 - (E1 - E2)
        - E1 ∩ E2 ⇔ E1 ⋈ E2
  - Rename operator
    - Forms:
      - a. ρr(A1, . . . ,An)(E) general form
      - b. ρr(E)
      - c. ρ(A1, . . . ,An)(E)
    - To unify schemas for set operators
    - ex: List of college and student name
      ```
      ρc(name)(π(cname)College) ∪ ρc(name)(π(sname)Student)
      ```
    - For disambiguation in "self - joins"
    - ex: Pairs of colleges in same state
      ```
      σ(s1 = s2)(ρc1(e1,s1,e1)College x ρc2(n2,s2,e2)College)
      ```
      second way:
      ```
      σ(n1 < n2)(ρc1(n1,s1,e1)College ⋈ ρc2(n2,s1,e2)College)
      ```
      notice s = s ρc1 and ρc2
  - Alternative Notation
    1. Assignment statements
      - ex. Pairs of colleges in some states
        ```
        c1 := ρ(c1,s,e1) College
        c2 := ρ(c2,s,e2) College
        cp := c1 ⋈ c2
        ANS := σ(n1 < n2)cp
        ```
    2. Expression Tree
