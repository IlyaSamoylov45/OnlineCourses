Relational Design Theory
  - Q1 : Consider a database containing information about courses taken by students. Students have a unique student ID and (possibly not unique) name; courses have a unique course number and (possibly not unique) title; students take a course in a given year and receive a grade. Which of the following schemas would you recommend?
    - Solution : Student(sID, name), Course(courseNum, title), Took(sID, courseNum, year, grade)

  - Q2 : Consider the relation Took(sID, name, courseNum, title). Students have a unique student ID and (possibly not unique) name; courses have a unique course number and (possibly not unique) title. Each tuple in the relation encodes the fact that the given student took the given course. What are all of the functional dependencies for relation Took?
    - Solution : sID → name, courseNum → title

  - Q3 : Consider the relation Took(sID, name, courseNum, title) with functional dependencies sID → name and courseNum → title. Is Took in Boyce-Codd Normal Form?
    - Solution : No, BCNF requires the left-hand side of functional dependencies to be a key for the relation. Neither sID nor courseNum are keys for Took.

  - Q4 : Consider the relation StudentInfo(sID, dorm, courseNum). Students typically live in several dorms and take many courses during college. Suppose the data does not capture which dorm(s) a student lived in when taking a specific course, i.e., all dorm-course combinations are recorded for each student. What are all of the multivalued dependencies for relation StudentInfo?
    - Solution sID ↠ dorm, sID ↠ courseNum

  - Q5 : Consider the relation StudentInfo(sID, dorm, courseNum) with multivalued dependencies sID ↠ dorm and sID ↠ courseNum. Is StudentInfo in Fourth Normal Form?
    - Solution : No, 4NF requires the left-hand side of multivalued dependencies to be a key for the relation. sID is not a key for StudentInfo.

  - Q6 : Consider a relation R(A,B,C,D) with functional dependencies A,B → C and C,D → E. Suppose there are at most 3 different values for each of A, B, and D. What's the maximum number of different values for E?
    - Solution : 27, There are at most 3*3=9 combinations of A,B values, so by A,B → C at most 9 different values for C. With at most 3 different values for D, by C,D → E there are at most 9*3=27 different values for E.

  - Q7 : For the relation Apply(SSN,cName,state,date,major), what real-world constraint is captured by SSN,cName → date
    - Solution : Every application from a student to a specific college must be on the same date, Any two tuples with the same SSN-cName combination must also have the same date. So if a student (SSN) applies to a college (cName) more than once, it must be on the same date.

  - Q8 : Consider the relation R(A,B,C,D,E) and suppose we have the functional dependencies AB → C, AE → D, and D → B. Which of the following attribute pairs is a key for R?
    - Solution : AE

  - Q9 : Consider the relation R(A,B,C,D,E) and the set of functional dependencies S1 = {AB → C, AE → D, D → B}. Which of the following sets S2 of FDs does NOT follow from S1?
    - Solution : S2 = {ABC → D, D → B}

  - Q10 : For the relation Apply(SSN,cName,state,date,major), suppose college names are unique and students may apply to each college only once, so we have two FDs: cName → state and SSN,cName → date,major. Is Apply in BCNF?
    - Solution : No, {SSN,cName} is a key so only cName → state is a BCNF violation. Based on this violation we decompose into A1(cName,state), A2(SSN,cName,date,major). Now both FDs have keys on their left-hand-side so we're done.

  - Q11 : Consider relation Apply(SSN,cName,state,date,major) with FDs cName → state and SSN,cName → date,major. What schema would be produced by the BCNF decomposition algorithm?
    - Solution : A1(cName,state), A2(SSN,cName,date,major), {SSN,cName} is a key so only cName → state is a BCNF violation. Based on this violation we decompose into A1(cName,state), A2(SSN,cName,date,major). Now both FDs have keys on their left-hand-side so we're done.

  - Q12 : Consider a relation R(A,B,C) with multivalued dependency A ↠ B. Suppose there at least 3 different values for A, and each value of A is associated with at least 4 different B values and at least 5 different C values. What is the minimum number of tuples in R?
    - Solution : 60, Multivalued dependency A ↠ B says that for each value of A, we must have every combination of B and C values. So for each of the 3 values of A we must have at least 4*5=20 different tuples.

  - Q13 : For the relation Apply(SSN,cName,date,major) with functional dependency SSN,cName → date, what real-world constraint is captured by SSN ↠ cName,date?
    - Solution : A student must apply to the same set of majors at all colleges. SSN ↠ cName,date says that (cName,date) and major are independent, i.e., all combinations of (cName,date) and major must be present for any college or major a student applies for.

  - Q14 : Consider relation StudentInfo(sID, name, dorm, major) with functional dependency sID → name and multivalued dependency sID ↠ dorm. What schema would be produced by the 4NF decomposition algorithm?
    - Solution : S1(sID,name), S2(sID,dorm), S3(sID,major), There is no key for the relation. Decomposing on the violating FD separates (sID,name) from (sID,dorm,major). Decomposing on the violating MVD separates (sID,dorm) from (sID,major). Now there are no violating FDs or nontrivial MVDs.
