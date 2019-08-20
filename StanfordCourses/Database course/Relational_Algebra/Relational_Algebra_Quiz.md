Multiple Choice
1. [Q1] Suppose relation R(A,C) has the following tuples:
  - (A	C)
  - (3	3)
  - (6	4)
  - (2	3)
  - (3	5)
  - (7	1)
<br>and relation S(B,C,D) has the following tuples:
  - (B	C	 D)
  - (5	1	 6)
  - (1	5	 8)
  - (4	3	 9)

Compute the natural join of R and S. Which of the following tuples is in the result? Assume each tuple has schema (A,B,C,D).

a. (3, 5, 1, 6) <br>
b. (7, 1, 5, 8) <br>
c. (7, 5, 1, 6) <br>
d. (3, 3, 5, 8) <br>
Answer: (7, 5, 1, 6)

2. [Q2] Suppose relation R(A,B) has the following tuples:
- (A	B)
- (1	a)
- (7	t)
- (2	g)
- (4	c)
- (9	t)
<br> and relation S(B,C,D) has the following tuples:
- (B	C	 D)
- (c	5	 6)
- (a	7	 8)
- (t	8	 9)

Compute the theta-join of R and S with the condition R.B = S.B AND R.A < S.C Which of the following tuples is in the result? Assume each tuple has schema (A, R.B, S.B, C, D).

a. (7, t, t, 8, 9) <br>
b. (2, g, t, 8, 9) <br>
c. (9, t, t, 8, 9) <br>
d. (1, a, a, 8, 9) <br>
Answer: (7, t, t, 8, 9)

3. [Q3] Consider a relation R(A,B) with r tuples, all unique within R, and a relation S(B,C) with s tuples, all unique within S. Let t represent the number of tuples in R natural-join S. Which of the following triples of values (r,s,t) is possible?
a. (2,3,9) <br>
b. (5,10,500) <br>
c. (5,10,250) <br>
d. (5,2,10) <br>
Answer: (5,2,10)

4. [Q4] Consider a relation R(A) with r tuples, all unique within R, and a relation S(A) with s tuples, all unique within S. Let t represent the number of tuples in R minus S. Which of the following triples of values (r,s,t) is possible?
a. (10,15,0) <br>
b. (5,0,3) <br>
c. (10,5,15) <br>
d. (5,10,10) <br>
Answer: (10,15,0)

5. [Q5] Suppose relation R(A,B) has the following tuples:
- (A  B)
- (1	2)
- (3	4)
- (5	6)
<br> and relation S(B,C,D) has the following tuples:
- (B	C	 D)
- (2	4	 6)
- (4	6	 8)
- (4	7	 9)

Compute the natural join of R and S. Which of the following tuples is in the result? Assume each tuple has schema (A,B,C,D).

a. (5,6,4,6) <br>
b. (5,6,7,8) <br>
c. (5,6,7,9) <br>
d. (3,4,7,9) <br>
Answer: (3,4,7,9)

6. [Q6]  Suppose relation R(A,B) has the following tuples:
- (A  B)
- (1	2)
- (3	4)
- (5	6)
<br> and relation S(B,C,D) has the following tuples:
- (B	C	 D)
- (2	4	 6)
- (4	6	 8)
- (4	7	 9)

Compute the theta-join of R and S with the condition R.A < S.C AND R.B < S.D. Which of the following tuples is in the result? Assume each tuple has schema (A, R.B, S.B, C, D).

a. (3,4,2,4,6) <br>
b. (5,6,4,6,9) <br>
c. (3,4,4,7,8) <br>
d. (1,2,4,4,6) <br>
Answer: (3,4,2,4,6)

7. [Q7] Suppose relation R(A,B,C) has the following tuples:
- (A	B	 C)
- (1	2	 3)
- (4	2	 3)
- (4	5	 6)
- (2	5	 3)
- (1	2	 6)

Compute the projection π C,B (R). Which of the following tuples is in the result?

a. (4, 2) <br>
b. (3, 2) <br>
c. (2, 1) <br>
d. (5, 3) <br>
Answer: (3, 2)

8. [Q8] Suppose relation R(A,B,C) has the following tuples:
- (A	B	 C)
- (1	2	 3)
- (4	2	 3)
- (4	5	 6)
- (2	5	 3)
- (1	2	 6)
<br> and relation S(A,B,C) has the following tuples:
- (A	B	 C)
- (2	5	 3)
- (2	5	 4)
- (4	5	 6)
- (1	2	 3)

Compute the union of R and S. Which of the following tuples DOES NOT appear in the result?

a. (4,2,3) <br>
b. (1,5,4) <br>
c. (4,5,6) <br>
d. (2,5,3) <br>
Answer: (1,5,4)

9. [Q9] Suppose relation R(A,B,C) has the following tuples:
- (A	B	 C)
- (1	2	 3)
- (4	2	 3)
- (4	5	 6)
- (2	5	 3)
- (1	2	 6)
<br> and relation S(A,B,C) has the following tuples:
- (A	B	 C)
- (2	5	 3)
- (2	5	 4)
- (4	5	 6)
- (1	2	 3)

Compute the intersection of the relations R and S. Which of the following tuples is in the result?

a. (4,2,3) <br>
b. (1,2,6) <br>
c. (2,4,3) <br>
d. (2,5,3) <br>
Answer: (2,5,3)

10. [Q10] Suppose relation R(A,B,C) has the following tuples:
- (A	B	 C)
- (1	2	 3)
- (4	2	 3)
- (4	5	 6)
- (2	5	 3)
- (1	2	 6)
<br> and relation S(A,B,C) has the following tuples:
- (A	B	 C)
- (2	5	 3)
- (2	5	 4)
- (4	5	 6)
- (1	2	 3)

Compute (R - S) union (S - R), often called the "symmetric difference" of R and S. Which of the following tuples is in the result?

a. (1,2,3) <br>
b. (4,5,3) <br>
c. (2,2,3) <br>
d. (1,2,6) <br>
Answer: (1,2,6)
