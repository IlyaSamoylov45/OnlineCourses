Functional Dependencies Quiz
  - Q1 : Consider relation R(A,B,C,D,E) with functional dependencies: ```AB → C, C → D, BD → E ``` Which of the following sets of attributes does not functionally determine E?
    - Solution : C

  - Q2 : Consider relation R(A,B,C,D,E) with functional dependencies: ```D → C, CE → A, D → A, AE → D``` Which of the following is a key?
    - Solution : BDE

  - Q3 :  Let relation R(A,B,C,D,E,F,G,H) satisfy the following functional dependencies: ```A → B, CH → A, B → E, BD → C, EG → H, DE → F``` Which of the following FDs is also guaranteed to be satisfied by R?
    - Solution : BDG → AE

  - Q4 : Consider relation R(A,B,C,D,E,F) with functional dependencies: ```CDE → B, ACD → F, BEF → C, B → D``` Which of the following is a key?
    - Solution : ABEF

  - Q5 : Consider relation R(A,B,C,D,E,F,G) with functional dependencies: ```AB → C, CD → E, EF → G, FG → E, DE → C, and BC → A``` Which of the following is a key?
    - Solution : BCDF

  - Q6 : Let relation R(A,B,C,D,E) satisfy the following functional dependencies: ```AB → C, BC → D, CD → E, DE → A, AE → B``` Which of the following FDs is also guaranteed to be satisfied by R?
    - Solution : ACE → D

  - Q7 : Let relation R(A,B,C,D) satisfy the following functional dependencies: ```A → B, B → C, C → A``` Call this set S1. A different set S2 of functional dependencies is equivalent to S1 if exactly the same FDs follow from S1 and S2. Which of the following sets of FDs is equivalent to the set above?
    - Solution : C → B, B → A, A → C

  - Q8 : Suppose relation R(A,B,C) currently has only the tuple (0,0,0), and it must always satisfy the functional dependencies ```A → B and B → C```. Which of the following tuples may be inserted into R legally?
    - Solution : (1,1,0)
