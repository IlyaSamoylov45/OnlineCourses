Multiple Choice

  - Q1 : Consider a table R(A) containing {(1),(2)}. Suppose transaction T1 is "update R set A = 2*A" and transaction T2 is "select avg(A) from R". If transaction T2 executes using "read uncommitted", what are the possible values it returns?
    - a) 1.5, 2, 2.5, 3
    - b) 1.5, 2, 3
    - c) 1.5, 2.5, 3
    - d) 1.5, 3
    - Solution : 1.5, 2, 2.5, 3
  - Q2 : Consider tables R(A) and S(B), both containing {(1),(2)}. Suppose transaction T1 is "update R set A = 2*A; update S set B = 2*B" and transaction T2 is "select avg(A) from R; select avg(B) from S". If transaction T2 executes using "read committed", is it possible for T2 to return two different values?
    - a) Yes
    - b) No
    - Solution : Yes
  - Q3 : Consider tables R(A) and S(B), both containing {(1),(2)}. Suppose transaction T1 is "update R set A = 2*A; update S set B = 2*B" and transaction T2 is "select avg(A) from R; select avg(B) from S". If transaction T2 executes using "read committed", is it possible for T2 to return a smaller avg(B) than avg(A)?
    - a) Yes
    - b) No
    - Solution : No
