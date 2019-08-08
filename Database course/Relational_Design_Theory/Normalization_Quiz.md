Normalization Quiz
  - Q1: Consider relation R(A,B,C,D,E) with multivalued dependencies: A ↠ B, B ↠ D and no functional dependencies. Suppose we decompose R into 4th Normal Form. Depending on the order in which we deal with 4NF violations, we can get different final decompositions. Which one of the following relation schemas could be in the final 4NF decomposition?
    - Solution : ACE

  - Q2 : Let R(A,B,C,D,E) be a relation in Boyce-Codd Normal Form (BCNF). Suppose ABC is the only key for R. Which of the following functional dependencies is guaranteed to hold for R?
    - Solution : ABCE → D

  - Q3 : Consider a relation R(A,B,C,D). For which of the following sets of FDs is R in Boyce-Codd Normal Form (BCNF)?
    - Solution : AC → D, D → A, D → C, D → B

  - Q4 : Consider relation R(A,B,C,D) with functional dependencies: A → B, C → D, AD→  C, BC → A. Suppose we decompose R into Boyce-Codd Normal Form (BCNF). Which of the following schemas could not be in the result of the decomposition?
    - Solution : BCD

  - Q5 : Consider a relation R(A,B,C,D,E). For which of the following sets of FDs is R in Boyce-Codd Normal Form (BCNF)?
    - Solution : ABE → C, BDE → A, BE → D, CDE → B

  - Q6 : Consider relation R(A,B,C,D) with functional and multivalued dependencies: A → B, C → D, B → C. Suppose we decompose R into 4th Normal Form. Depending on the order in which we deal with 4NF violations, we can get different final decompositions. Which one of the following relation schemas could be in the final 4NF decomposition?
    - Solution : BC
