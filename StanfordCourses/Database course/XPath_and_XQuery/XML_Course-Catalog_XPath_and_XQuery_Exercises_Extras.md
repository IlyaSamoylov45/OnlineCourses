XML Course-Catalog XPath and XQuery Exercises Extras
  - Q1 : Return the course number of the course that is cross-listed as "LING180".
    - XQuery
    ```XQUERY
    for $a in doc("courses.xml")//Course
    where $a/contains(Description, 'LING180')
    return $a/data(@Number)
    ```
    - XPath
    ```XPath
    doc("courses.xml")//Course[contains(Description, 'LING180')]/data(@Number)
    ```

  - Q2 : Return course numbers of courses that have the same title as some other course. (Hint: You might want to use the "preceding" and "following" navigation axes for this query, which were not covered in the video or our demo script; they match any preceding or following node, not just siblings.)
    - XQuery
    ```XQUERY
    for $a in doc("courses.xml")//Course
    let $b := $a/Title = $a/following::Course/Title
    let $c := $a/Title = $a/preceding::Course/Title
    where $b or $c
    return $a/data(@Number)
    ```
    - XPath
    ```XPath
    doc("courses.xml")//Course[Title = following::Course/Title or Title = preceding::Course/Title]/data(@Number)
    ```

  - Q3 : Return course numbers of courses taught by an instructor with first name "Daphne" or "Julie".
    - XQuery
    ```XQUERY
    for $a in doc("courses.xml")//Course
    let $b := $a//Instructors//First_Name = "Daphne"
    let $c := $a//Instructors//First_Name = "Julie"
    where $b or $c
    return $a/data(@Number)
    ```
    - XPath
    ```XPath
    doc("courses.xml")//Course[Instructors//First_Name = "Daphne" or Instructors//First_Name = "Julie"]/data(@Number)
    ```

  - Q4 : Return the number (count) of courses that have no lecturers as instructors.
    - XQuery
    ```XQUERY
    for $a in doc('courses.xml')
    let $b := $a//Course
    return sum(
      for $c in $b
        where count($c/Instructors/Lecturer) = 0
        return xs:int(count($c))
      )
    ```
    ```XQUERY
    for $a in doc('courses.xml')
    let $b := $a//Course
    return count(
      for $c in $b
      where count($c/Instructors/Lecturer) = 0
      return $c
    )
    ```
    - XPath
    ```XPath
    count(doc("courses.xml")//Course[count(Instructors/Lecturer) = 0])    
    ```

  - Q5 : Return titles of courses taught by the chair of a department. For this question, you may assume that all professors have distinct last names.
    - XQuery
    ```XQUERY
    for $a in doc("courses.xml")//Course
    let $b := $a/Instructors/Professor/Last_Name
    let $c := $a/parent::Department/Chair/Professor//Last_Name
    where $b = $c
    return $a//Title
    ```
    - XPath
    ```XPath
    doc("courses.xml")//Course[parent::Department/Chair/Professor/Last_Name = Instructors/Professor/Last_Name]/Title
    ```

  - Q6 : Return titles of courses that have both a lecturer and a professor as instructors. Return each title only once.
    - XQuery
    ```XQUERY
    for $a in doc("courses.xml")//Course
    where $a/Instructors/Professor and $a/Instructors/Lecturer
    return $a/Title
    ```
    ```XQUERY
    for $a in doc("courses.xml")//Course
    where count($a/Instructors/Professor) > 0 and count($a/Instructors/Lecturer) > 0
    return $a/Title
    ```
    - XPath
    ```XPath
    doc("courses.xml")//Course[Instructors/Professor/Last_Name and Instructors/Lecturer/Last_Name]/Title
    ```    

  - Q7 : Return titles of courses taught by a professor with the last name "Ng" but not by a professor with the last name "Thrun".
    - XQuery
    ```XQUERY
    for $a in doc("courses.xml")//Course
    let $b := $a/Instructors/Professor/Last_Name = 'Ng'
    let $c := $a[count(Instructors/Professor[Last_Name = 'Thrun']) = 0]
    where $b and $c
    return $a/Title
    ```
    - XPath
    ```XPath
    doc("courses.xml")//Course[Instructors/Professor/Last_Name = 'Ng' and count(Instructors/Professor[Last_Name = 'Thrun']) = 0]/Title
    ```

  - Q8 : Return course numbers of courses that have a course taught by Eric Roberts as a prerequisite.
    - XQuery
    ```XQUERY
    let $a := doc("courses.xml")//Course
    for $b in $a
    where $b/Prerequisites/Prereq = (
      for $c in $a
      where $c/Instructors//First_Name = 'Eric' and $c/Instructors//Last_Name = 'Roberts'
      return $c/data(@Number)
      )
    return $b/data(@Number)
    ```
    - XPath
    ```XPath
    doc("courses.xml")//Course[Prerequisites/Prereq = doc("courses.xml")//Course[Instructors//First_Name = 'Eric' and Instructors//Last_Name = 'Roberts']/data(@Number)]/data(@Number)
    ```

  - Q9 : Create a summary of CS classes: List all CS department courses in order of enrollment. For each course include only its Enrollment (as an attribute) and its Title (as a subelement).
    - XQuery
    ```XQUERY
    for $a in doc("courses.xml")
    return <Summary> {
      for $b in $a//Department[@Code = 'CS']/Course
      order by xs:int($b/@Enrollment)
      return
      <Course>
          { $b/@Enrollment }
          { $b/Title }
          </Course>
    } </Summary>
    ```

  - Q10 :
    - XQuery
    ```XQUERY
    <Professors> {
      for $LastName in distinct-values(doc("courses.xml")//Professor/Last_Name)
      let $p := doc("courses.xml")//Professor[Last_Name = $LastName]
      for $FirstName in distinct-values($p/First_Name)
      order by $LastName
      return
        <Professor>
          <First_Name>{ $FirstName }</First_Name>
          {$p/Middle_Initial}   
          <Last_Name>{ $LastName }</Last_Name>
          </Professor>
    } </Professors>
    ```

  - Q11 : Expanding on the previous question, create an inverted course listing: Return an "Inverted_Course_Catalog" element that contains as subelements professors together with the courses they teach, sorted by last name. You may still assume that all professors have distinct last names. The "Professor" subelements should have the same structure as in the original data, with an additional single "Courses" subelement under Professor, containing a further "Course" subelement for each course number taught by that professor. Professors who do not teach any courses should have no Courses subelement at all. (This problem is very challenging; extra congratulations if you get it right.)
    - XQuery
    ```XQUERY
    <Inverted_Course_Catalog> {
      for $ln in distinct-values(doc("courses.xml")//Professor/Last_Name)
      let $p := doc("courses.xml")//Professor[Last_Name = $ln]
      for $fn in distinct-values($p/First_Name)
      order by $ln
        return
          <Professor>
              <First_Name> { $fn } </First_Name>
              {$p/Middle_Initial}   
              <Last_Name> { $ln } </Last_Name>
              {
                let $b := doc("courses.xml")//Course[Instructors/Professor/Last_Name = $ln]
                    return
                      if (count($b) > 0) then
                          <Courses>{
                              for $r in $b
                              return <Course>
                                      { $r/data(@Number) }
                                     </Course>
                          }</Courses>
                      else()
             }
         </Professor>
    } </Inverted_Course_Catalog>
    ```
