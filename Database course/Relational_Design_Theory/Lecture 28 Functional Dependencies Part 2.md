Functional Dependencies Part 2
  - Student(SSN,sName,address,HSCode,HSname,HScity,GPA,priority)
    - SSN → sName
    - SSn → address
    - HScode → HSname, HScity
    - HSname, HScity → HScode
    - SSN → GPA
    - GPA → priority
    - SSN → priority
  - Apply(SSn,cName,state,date,major)
    - cName → date
    - SSN,cName → major
    - SSN → state
    
