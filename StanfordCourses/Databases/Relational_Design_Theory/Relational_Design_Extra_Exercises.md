Relational Design Extra Exercises
  - Q1 : Consider a relation R(A,B,C) and suppose R contains the following four tuples:

  | A  | B  | C  |
  | -- | -- | -- |                          
  | 1  | 2  | 2  |
  | 1  | 3  | 2  |
  | 1  | 4  | 2  |
  | 2  | 5  | 2  |

  - For each of the following functional dependencies, state whether or not the dependency is satisfied by this relation instance.
    - A → B : ```False```
    - A → C : ```True```
    - B → A : ```True```
    - B → C : ```True```
    - C → A : ```False```
    - C → B : ```False```
    - AB → C : ```True```
    - AC → B : ```False```
    - BC → A : ```True```

  - Q2 : Which of the following rules for functional dependencies are correct (i.e., the rule holds over all databases) and which are incorrect (i.e., the rule does not hold over some database)? For incorrect rules, give the simplest example relation instance you can come up with where the rule does not hold.
    - If A → B and BC → D, then AC → D : ```True```
    - If AB → C then A → C : ```False : (1, 2, 1) and (1, 3, 2)```
    - If A → B1,..,Bn  and  C1,..,Cm → D  and {C1,...,Cm} is a subset of {B1,..,Bn}, then A → D : ``` True```
    - If A → C and B → C and ABC → D, then A → D : ```False :  (1, 2, 3, 4) and (1, 4, 3, 1)```

  - Q3 : Consider a relation R(A,B,C,D,E) with the following functional dependencies:
    - A → B
    - CD → E
    - E → A
    - B → D
  - Specify all minimal keys for R.
    ```
    AC
    CE
    BC
    CD
    ```

  - Q4 : Consider a relation R(A,B,C,D,E,F,G,H) with the following functional dependencies:
    - A → BCD
    - AD → E
    - EFG → H
    - F → GH
  - Based on these functional dependencies, there is one minimal key for R. What is it?
    ```
    AF
    ```
  - One of the four functional dependencies can be removed without altering the key. Which one?
    ```
    EFG → H
    ```

  - Q5 : Consider a relation R(A,B,C,D,E,F) with the following set of functional dependencies:
    - A → C
    - DE → F
    - B → D
  - Based on these functional dependencies, there is one minimal key for R. What is it?
    ```
    ABE
    ```
  - Add to the above set of functional dependencies the dependency A → B. Now suppose we want A to be a key. Name one more functional dependency that, if added to the set, makes A a key. As an additional restriction, the new functional dependency must have only one attribute on the left-hand side and only one attribute on the right-hand side.
    ```
    A → E
    ```
  - Q6 :  Consider the following sets of functional dependencies over a relation R(A,B,C).
    - F1 = {A → B, B → C}
    - F2 = {A → B, A → C}
    - F3 = {A → B, AB → C}
  - Which of these sets are equivalent? (Two sets of functional dependencies (FDs) F and F' are equivalent if all FDs in F' follow from the ones in F, and all FDs in F follow from the ones in F'.)
    ```
    F2 and F3. F1 is not equivalent
    ```

  - Q7 : Consider a relation R(A,B,C) and suppose R contains the following five tuples:

  | A  | B  | C  |
  | -- | -- | -- |                          
  | 1  | 2  | 3  |
  | 1  | 3  | 2  |
  | 1  | 2  | 2  |
  | 3  | 2  | 1  |
  | 3  | 2  | 3  |

  - For each of the following multivalued dependencies, state whether or not the dependency is satisfied by this relation instance.
    - A ↠ B : ```False```
    - A ↠ C : ```False```
    - B ↠ A : ```False```
    - B ↠ C : ```False```
    - C ↠ A : ```True```
    - C ↠ B : ```True```

  - Q8 : Consider a relation R(A,B,C,D) that satisfies A ↠ B and B ↠ C. Suppose R contains the tuples (1,2,3,4) and (1,5,6,7). What other tuples must also be in R?
    ```
    (1, 2, 3, 7)
    (1, 5, 6 ,4)
    (1, 2, 6, 4)
    (1, 2, 6, 7)
    (1, 5, 3, 4)
    (1, 5, 3, 7)
    ```
  - Q9 : Which of the following rules for multivalued dependencies are correct (i.e., the rule holds over all databases) and which are incorrect (i.e., the rule does not hold over some database)? For incorrect rules, give the simplest example relation instance you can come up with where the rule does not hold.
    - If A ↠ BC and A ↠ CD then A ↠ C : ```True```
    - If AB ↠ C then A ↠ C : ```False : (1, 2, 3, 4), (1, 5, 6, 7)```
    - If A ↠ BC then A ↠ B and A ↠ C : ```False : (1, 2, 3, 4), (1, 5, 6, 7)```
    - If A ↠ BCD and A ↠ C then A ↠ BD : ```True```

  - Q10 : The relation R(A,B,C) satisfies an unknown set of functional and multivalued dependencies. All we know about R is that it allows at least the following two instances:

  | A  | B  | C  |
  | -- | -- | -- |                          
  | 1  | 2  | 3  |
  | 1  | 3  | 4  |

  | A  | B  | C  |
  | -- | -- | -- |                          
  | 1  | 3  | 3  |
  | 2  | 2  | 4  |
  | 3  | 3  | 3  |

  - Consider the following possible functional and multivalued dependencies:

    - A → B : ```False : Ruled out```
    - A → C : ```False : Ruled out```
    - B → A : ```False : Ruled out```
    - B → C : ```True```
    - C → A : ```False : Ruled out```
    - C → B : ```True```
    - AB → C : ```True```
    - AC → B : ```True```
    - BC → A : ```False : Ruled out```
    - A ↠ B : ```False : Ruled out```
    - A ↠ C : ```False : Ruled out```
    - B ↠ A : ```True```
    - B ↠ C : ```True```
    - C ↠ A : ```True```
    - C ↠ B : ```True```

  - Which of these dependencies are ruled out by the two instances of R above?

  - Q11 : Consider the following relational schema:
  - ```Car(make, model, year, color, dealer) ``` Each tuple in relation Car specifies that one or more cars of a particular make, model, and year in a particular color are available at a particular dealer. For example, the tuple ```(Honda, Civic, 2010, Blue, Fred's Friendly Folks)``` indicates that 2010 Honda Civics in blue are available at the Fred's Friendly Folks car dealer. For each of the following English statements, write one nontrivial functional or multivalued dependency that best captures the statement.
    - The model name for a car is trademarked by its make, i.e., no two makes can use the same model name.
    ```
    model → make
    ```  
    - Each dealer sells only one model of each make of car.
    ```
    dealer, make → model
    ```
    - If a particular make, model, and year of a car is available in a particular color at a particular dealer, then that color is available at all dealers carrying the same make, model, and year.
    ```
    make, model, year ↠ dealer
    ```
    - Based on your answers for (a)-(c), specify all minimal keys for relation Car.
    ```
    (model, dealer, year, color) and (make, dealer, year, color)
    ```

  - Q12 : Consider the following two relational schemas:
    ```
    Schema 1: R(A,B,C,D)
    Schema 2: R1(A,B,C), R2(B,D)
    ```
    - Consider Schema 1 and suppose that the only functional dependencies that hold on the relations in this schema are A → B, C → D, and all dependencies that follow from these. Is Schema 1 in Boyce-Codd Normal Form (BCNF)?
    ```
    No since neither A or C are keys.
    ```
    - Consider Schema 2 and suppose that the only functional dependencies that hold on the relations in this schema are A → B, A → C, B → A, A → D, and all dependencies that follow from these. Is Schema 2 in BCNF?
    ```
    Yes
    ```
    - Suppose we omit dependency A → D from the previous. Is Schema 2 in BCNF?
    ```
    Yes
    ```
    - Consider Schema 1 and suppose that the only functional and multivalued dependencies that hold on the relations in this schema are A → BC, B → D, B ↠ CD, and all dependencies that follow from these. Is Schema 1 in Fourth Normal Form (4NF)?
    ```
    No since B is not a key so therefore B ↠ C is a 4NF violation
    ```
    - Consider Schema 2 and suppose that the only functional and multivalued dependencies that hold on the relations in this schema are A → BD, D → C, A ↠ C, B ↠ D, and all dependencies that follow from these. Is Schema 2 in 4NF?
    ```
    Yes
    ```

  - Q13 : Consider a relation R(A,B,C) and suppose R contains the following four tuples:

  | A  | B  | C  |
  | -- | -- | -- |                          
  | 1  | 2  | 3  |
  | 1  | 2  | 4  |
  | 5  | 2  | 3  |
  | 5  | 2  | 6  |

  - Specify all completely nontrivial functional dependencies that hold on this instance of R.
  ```
  A → B
  AC → B
  C → B
  ```
  - Specify all nontrivial multivalued dependencies that hold on this instance of R. Do not include multivalued dependencies that are also functional dependencies.
  ```
  A ↠ C
  C ↠ A
  ```
  - Is this instance of R in Boyce-Codd Normal Form (BCNF) with respect to the dependencies you gave in part (a)? If not, specify all valid BCNF decompositions.
  ```
  No - A is not a key in A → B and C is not a key in C → B
  ```

  - Q14 : Consider a relation R(A,B,C,D,E) with the following functional dependencies: AB → C, BC → D, CD → E, DE → A
    - Specify all minimal keys for R.
    ```
    AB
    BC
    BCD
    BDE
    ```
    - Which of the given functional dependencies are Boyce-Codd Normal Form (BCNF) violations?
    ```
    CD → E
    DE → A
    ```
    - Give a decomposition of R into BCNF based on the given functional dependencies.
    ```
    (CDE), (ACD), (BCD)
    ```
    - Give a different decomposition of R into BCNF based on the given functional dependencies.
    ```
    (BCD), (ADE), (CDE)
    ```

  - Q15 : Consider the following relational schema: ```UnivInfo(studID, studName, course, profID, profOffice)``` Each tuple in relation UnivInfo encodes the fact that the student with the given ID and name took the given course from the professor with the given ID and office. Assume that students have unique IDs but not necessarily unique names, and professors have unique IDs but not necessarily unique offices. Each student has one name; each professor has one office.
    - (a) Specify a set of completely nontrivial functional dependencies for relation UnivInfo that encodes the assumptions described above and no additional assumptions.
    ```
    studID → studName, profID → profOffice
    ```
    - (b) Based on your functional dependencies in part (a), specify all minimal keys for relation UnivInfo.
    ```
    (studID,profID,course)
    ```
    - (c) Is UnivInfo in Boyce-Codd Normal Form (BCNF) according to your answers to (a) and (b)? If not, give a decomposition of UnivInfo into BCNF.
    ```
    No - neither studID nor profID is a key.
    Decomposition :
    R1(studID,studName),
    R2(profID,profOffice),
    R3(studID,profID,course)
    ```
    - (d) Now add the following two assumptions: (1) No student takes two different courses from the same professor; (2) No course is taught by more than one professor (but a professor may teach more than one course). Specify additional functional dependencies to take these new assumptions into account.
    ```
    1. studID,profID → course
    2. course → profID
    ```
    - (e) Based on your functional dependencies for parts (a) and (d) together, specify all minimal keys for relation UnivInfo.
    ```
    (studID,profID) (studID,course)
    ```
    - (f) Is UnivInfo in BCNF according to your answers to (d) and (e)? If not, give a decomposition of UnivInfo into BCNF.
    ```
    No - studID, profID, and course are not keys.
    Decomposition :
    R1(studID, studName)
    R2(profID,profOffice)
    R3(profID,course)
    R4(course,studID)
    ```

  - Q16 : Consider the following relational schema:
  ```
  Sale(clerk, store, city, date, item, size, color)   // a clerk sold an item on a particular day
  Item(item, size, color, price)   // prices and available sizes and colors for items
  ```
  - Make the following assumptions, and only these assumptions, about the real world being modeled:
    1. Each clerk works in one store.
    2. Each store is in one city.
    3. A given item always has the same price, regardless of size or color.
    4. Each item is available in one or more sizes and one or more colors, and each item is available in all combinations of sizes and colors for that item.
  - Sale does not contain duplicates: If a clerk sells more than one of a given item in a given size and color on a given day, still only one tuple appears in relation Sale to record that fact.
    - (a ) Specify a set of completely nontrivial functional dependencies for relations Sale and Item that encodes the assumptions described above and no additional assumptions.
    ```
    Sale: clerk → store, store → city
    Item: item → price
    ```
    - (b ) Based on your functional dependencies in part (a), specify all minimal keys for relations Sale and Item.
    ```
    Sale: (clerk,date,item,size,color)
    Item: (item,size,color)
    ```
    - (c ) Is the schema in Boyce-Codd Normal Form (BCNF) according to your answers to (a) and (b)? If not, give a decomposition into BCNF.
    ```
    No. BCNF decomposition:
    S1(clerk,store),
    S2(store,city),
    S3(clerk,date,item,size,color),
    I1(item,price),
    I2(item,size,color)
    ```
    - (d ) Now consider your decomposed relations from part (c), or the original relations if you did not need to decompose them for part (c). Specify a set of nontrivial multivalued dependencies for relations Sale and Item that encodes the assumptions described above and no additional assumptions. Do not include multivalued dependencies that also are functional dependencies.
    ```
    Sale: None
    Item: item ↠ size, item ↠ color
    ```
    - (e ) Are the relations you used in part (d) in Fourth Normal Form (4NF) according to your answers for (a)-(d)? If not, give a decomposition into 4NF.
    ```
    No. 4NF Decomposition:
    S1(clerk,store),
    S2(store,city),
    S3(clerk,date,item,size,color),
    I1(item,price),
    I2(item, size),
    I3(item, color)
    ```
