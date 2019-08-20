Relational Algebra : Select, Project, Join
  - Relational Algebra forms the underpinnings of implemented languages like SQL
  - Example College Admissions Database
    - College(<u>cName</u>, state, enrollment)
    - Student(<u>sID</u>, sName, GPA, sizeHS)
    - Apply (<u>sID</u>, <u>cName</u>, <u>major</u>, decision)

Select, Project, Join
  - Simplest Query : relation name
    - ex:
      1. Student
  - Select operator: pick rows
    - ex:
      1. Students with GPA > 3.7
        ```
        σ(GPA > 3.7)(Students)
        ```
      2. Students with GPA > 3.7 and HS < 1000
        ```
        σ(GPA > 3.7 ^ sizeHS < 1000)(Students)
        ```
      3. Applications to Stanford CS major
        ```
        σ(cName = 'Stanford' ^ major = 'cs')(Apply)
        ```
  - Project operator: picks columns
    - ex:
      1. ID and decision of applicants
        ```
        π(sID, decision)(Apply)
        ```
  - To pick rows and columns
    - ex:
      1. ID and name of students with GPA > 3.7
        ```
        π(sID, sName)(σ(GPA > 3.7)(Students))
        ```
  - Duplicates : duplicates always eliminated
    - ex:
      1. Lists of application majors and decisions
        ```
        π(major, decision)(Apply)
        ```
    - SQL multisets, bags. Meaning not automatic duplicate elimination
  - Cross Product : combine two relations (a.k.a Cartesian Product)
    - Student x Apply
    - If student has s tuples and Apply has n tuples there will be n x s tuples
    - ex:
      1. Names and GPAs of students with HS > 1000 who applied to CS and were rejected.
        ```
        π(sName, GPA)(σ(student.sid = Apply.sid ^ HS > 1000 ^ major = 'cs' ^ dec = 'R')(Student x Apply))
        ```
  - Natural Join
    - Enforce equality on all attributes with same name
    - Eliminate one copy of duplicate attributes
    - ex:
      1. Names and GPAs of students with HS > 1000 who applied to CS and were rejected
        ```
        π(sName, GPA)(σ(HS > 1000 ^ major = 'cs' ^ dec = 'R')(Student ⋈ Apply))
        ```
      2. Names and GPAs of students with HS > 1000 who applied to CS at college with enrollment > 2000 and were rejected
        ```
        π(sName, GPA)(σ(HS > 1000 ^ major = 'cs' ^ dec = 'R' ^ enrollment > 2000)(Student ⋈ (Apply ⋈ College)))
        ```
  - Theta Join
    - Expr1 ⋈ Expr2 ⇔ σ(condition)(Expr1 x Expr2)
    - Basic operation implemented DBMS
    - Term join often means theta join
  - Query (expression) on set of relations produces relation as a result
    - Simplest query : relation name
    - Use operators to filter, slice, combine
