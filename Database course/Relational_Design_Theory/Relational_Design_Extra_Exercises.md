Relational Design Extra Exercises
  - <h3>Functional Dependencies</h3>
  - Q1 : Consider a relation R(A,B,C) and suppose R contains the following four tuples:
    <table style="text-align: left; width: 61px; height: 144px;" border="1" cellpadding="2" cellspacing="2">
      <tbody>
        <tr>
          <td style="padding: 4px; font-weight: bold;">A<br> </td>
          <td style="padding: 4px; font-weight: bold;">B<br></td>
          <td style="padding: 4px; font-weight: bold;">C<br></td>
        </tr>
        <tr>
          <td style="padding: 4px;">1<br></td>
          <td style="padding: 4px;">2<br></td>
          <td style="padding: 4px;">2<br></td>
        </tr>
        <tr>
          <td style="padding: 4px;">1<br></td>
          <td style="padding: 4px;">3<br></td>
          <td style="padding: 4px;">2<br></td>
        </tr>
        <tr>  
          <td style="padding: 4px;">1<br></td>
          <td style="padding: 4px;">4<br></td>
          <td style="padding: 4px;">2<br></td>
        </tr>
        <tr>
          <td style="padding: 4px;">2<br></td>
          <td style="padding: 4px;">5<br></td>
          <td style="padding: 4px;">2<br></td>
        </tr>
      </tbody>
    </table>    
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

    ```
