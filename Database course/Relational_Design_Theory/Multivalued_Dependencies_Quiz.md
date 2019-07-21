Multivalued Dependencies Quiz
  - Q1 : Here is an instance of a relation R(A,B,C):

  | A  | B  | C  |
  | -- | -- | -- |                          
  | 1  | 2  | 3  |
  | 1  | 3  | 2  |
  | 1  | 2  | 2  |
  | 3  | 2  | 1  |
  | 3  | 2  | 3  |
  
  Which of the following multivalued dependencies does this instance of R not satisfy?
    - Solution B →→ C

  - Q2 : Here is an instance of a relation R(A,B,C,D):

  | A  | B  | C  | D  |
  | -- | -- | -- | -- |                      
  | 1  | 2  | 3  | 4  |
  | 1  | 3  | 3  | 3  |
  | 1  | 3  | 3  | 4  |
  | 1  | 2  | 3  | 3  |
  | 2  | 2  | 4  | 4  |
  | 2  | 4  | 2  | 4  |
  | 2  | 4  | 4  | 4  |
  | 2  | 2  | 2  | 4  |

  Which of the following multivalued dependencies does this instance of R satisfy?
    - Solution : A →→ CD

  - Q3 : Consider relation R(A,B,C,D,E) with multivalued dependencies: A →→ B, B →→ D. Suppose R contains the tuples (0,1,2,3,4) and (0,5,6,7,8). Which of the following tuples must also be in R?
    - Solution : (0,5,6,3,8)

  - Q4 : Here is an instance of a relation R(A,B,C,D):

  | A  | B  | C  | D  |
  | -- | -- | -- | -- |                      
  | 1  | 2  | 3  | 7  |
  | 1  | 2  | 3  | 8  |
  | 4  | 2  | 5  | 7  |
  | 4  | 2  | 5  | 8  |

  Consider the following three multivalued dependencies: AB →→ C, (2) CD →→ A, (3) D →→ C. The following (M,n) pairs say that to satisfy multivalued dependency M (M=1, M=2, or M=3), a minimum of n tuples must be added to the given instance of R. Only one such pair is correct; which one?
    - Solution : (3,4)
