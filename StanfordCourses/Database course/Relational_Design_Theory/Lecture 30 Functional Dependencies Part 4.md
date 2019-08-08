Functional Dependencies Part 4
  - Specifying FDs for a Relation
    - S1 and S2 sets of FDs
    - S2 "follows" S1 if every relation instance satisfying S1 also satisfies S2
      - S2 : {SSN → Priority}
      - S1 : {SSN → GPA, GPA → priority}
    - How to test?
      - Does A → B follow S
      - A+ based on S check if B in set
