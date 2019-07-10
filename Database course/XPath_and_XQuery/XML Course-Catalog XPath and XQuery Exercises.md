XML Course-Catalog XPath and XQuery Exercises

  - Q1 : Return all Title elements (of both departments and courses).
    - XQuery:
    ```XQuery
    for $b in doc("courses.xml")
    return $b//Title
    ```
    - XPath:
    ```XPath
    doc("courses.xml")//Title
    ```

  - Q2 : Return last names of all department chairs.  
    - XQuery:
    ```XQuery
    for $b in doc("courses.xml")//Chair/Professor
    return $b/Last_Name
    ```
    - XPath:
    ```XPath
    doc("courses.xml")//Chair//Last_Name
    ```
    ```XPath
    doc("courses.xml")//Chair/Professor/Last_Name
    ```
  - Q3 : Return titles of courses with enrollment greater than 500.  
    - XQuery:
    ```XQuery
    for $b in doc("courses.xml")//Course
      where $b/@Enrollment > 500
    return $b/Title
    ```
    - XPath:
    ```XPath
    doc("courses.xml")//Course[@Enrollment > 500]/Title
    ```

  - Q4 : Return titles of departments that have some course that takes "CS106B" as a prerequisite.
    - XQuery:
    ```XQuery
    for $b in doc("courses.xml")//Department
    where $b/Course//Prereq = "CS106B"
    return $b/Title
    ```
    - XPath:
    ```XPath
    doc("courses.xml")//Department[Course//Prereq="CS106B"]/Title
    ```

  - Q5 : Return last names of all professors or lecturers who use a middle initial. Don't worry about eliminating duplicates.
    - XQuery:
    ```XQuery
    for $b in doc("courses.xml")//Department
    let $a := $b//Professor/Middle_Initial
    let $c := $b//Lecturer/Middle_Initial
    return $a/parent::*/Last_Name union $c/parent::*/Last_Name
    ```
    ```XQuery
    for $b in doc("courses.xml")//Department
    let $a := $b//(Professor | Lecturer)/Middle_Initial
    return $a/parent::*/Last_Name
    ```
    - XPath:
    ```XPath
    doc("courses.xml")//(Professor | Lecturer)[Middle_Initial]/Last_Name
    ```

  - Q6 : Return the count of courses that have a cross-listed course (i.e., that have "Cross-listed" in their description).
    - XQuery:
    ```XQuery
    for $a in doc("courses.xml")
    let $b := count($a//Course[contains(Description, "Cross-listed")])
    return $b
    ```
    - XPath:
    ```XPath
    count(doc("courses.xml")//Course[contains(Description, "Cross-listed")])
    ```

  - Q7 : Return the average enrollment of all courses in the CS department.
    - XQuery:
    ```XQuery
    for $a in doc("courses.xml")
    let $b := $a//Department[@Code="CS"]/avg(Course/@Enrollment)
    return $b
    ```
    ```XQuery
    for $a in doc("courses.xml")
    let $b := $a//Department[@Code="CS"]/Course[@Enrollment]
    return avg($b/@Enrollment)
    ```
    - XPath:
    ```XPath
    doc("courses.xml")//Department[@Code="CS"]/avg(Course/@Enrollment)
    ```

  - Q8 : Return last names of instructors teaching at least one course that has "system" in its description and enrollment greater than 100.
    - XQuery:
    ```XQuery
    for $a in doc("courses.xml")
    let $b := $a//Course[@Enrollment > 100]
    let $c := $a//Course[contains(Description, "system")]
    return  ($b intersect $c)//Last_Name
    ```
    ```XQuery
    for $a in doc("courses.xml")
    let $b := $a//Course[@Enrollment > 100]//Last_Name
    let $c := $a//Course[contains(Description, "system")]//Last_Name
    return $b intersect $c
    ```
    ```XQuery
    for $a in doc("courses.xml")//Course
    let $b := $a[@Enrollment > 100]
    let $c := $a[contains(Description, "system")]
    return ($b intersect $c)//Last_Name
    ```
    - XPath:
    ```XPath
    doc("courses.xml")//Course[@Enrollment>100 and contains(Description,"system")]//Last_Name
    ```

  - Q9 :Return the title of the course with the largest enrollment.
    - XQuery:
    ```XQuery
    for $a in doc("courses.xml")
    let $b := $a//Course[@Enrollment = max($a//Course/@Enrollment)]/Title
    return $b
    ```
    - XPath:
    ```XPath
    doc("courses.xml")//Course[@Enrollment = max(doc("courses.xml")//Course/@Enrollment)]/Title
    ```
