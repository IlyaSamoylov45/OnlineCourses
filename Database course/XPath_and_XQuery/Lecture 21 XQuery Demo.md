Lecture 18 XQuery Demo
  - Titles of books costing less than $90 where "Ullman" is an author
  ```XQuery
  for $b in doc("BookstoreQ.xml")/Bookstore/Book
  where $b/@Price < 90
    and $b/Authors/Author/Last_Name = "Ullman"
  return $b/Title
  ```
  - Titles and author first names of books whose title contains one of the author's first names
  ```XQuery
  for $b in doc("BookstoreQ.xml")/Bookstore/Book
  where some $fn in $b/Authors/Author/First_Name
           satisfies contains($b/Title, $fn)
  return <Book>
            { $b/Title }
            { $b/Authors/Author/First_Name }
         </Book>

  ```
  - Author first names of books whose title contains one of the author's first names
  ```XQuery
  for $b in doc("BookstoreQ.xml")/Bookstore/Book
  where some $fn in $b/Authors/Author/First_Name
           satisfies contains($b/Title, $fn)
  return <Book>
            { $b/Title }
            { for $fn in $b/Authors/Author/First_Name
              where contains($b/Title, $fn) return $fn }
         </Book>

  ```
  - Average book price
  ```XQuery
  <Average>
    { let $plist := doc("BookstoreQ.xml")/Bookstore/Book/@Price
      return avg($plist) }
  </Average>
  ```
  - Average book price compact.
  ```XQuery
  <Average>
    { let $a := avg(doc("BookstoreQ.xml")/Bookstore/Book/@Price)
      return $a }
  </Average>
  ```
  - Books whose price is below average
  ```XQuery
  let $a := avg(doc("BookstoreQ.xml")/Bookstore/Book/@Price)
  for $b in doc("BookstoreQ.xml")/Bookstore/Book
  where $b/@Price < $a
  return <Book>
            { $b/Title }
            <Price> { $b/data(@Price) } </Price>
         </Book>
  ```
  - Titles and prices sorted by price
  ```XQuery
  for $b in doc("BookstoreQ.xml")/Bookstore/Book
  order by xs:int($b/@Price)
  return <Book>
            { $b/Title }
            <Price> { $b/data(@Price) } </Price>
         </Book>
  ```
  - All last names (duplicates, then eliminate)
  ```XQuery
  for $n in distinct-values(doc("BookstoreQ.xml")//Last_Name)
  return <Last_Name> {$n} </Last_Name>
  ```
  - Books where every author's first name includes "J"
  ```XQuery
  for $b in doc("BookstoreQ.xml")/Bookstore/Book
  where every $fn in $b/Authors/Author/First_Name
           satisfies contains($fn, "J")
  return $b
  ```
  - All pairs of titles containing a shared author
  ```XQuery
  for $b1 in doc("BookstoreQ.xml")/Bookstore/Book
  for $b2 in doc("BookstoreQ.xml")/Bookstore/Book
  where $b1/Authors/Author/Last_Name = $b2/Authors/Author/Last_Name
  and $b1/Title < $b2/Title
  return
     <BookPair>
        <Title1> { data($b1/Title) } </Title1>
        <Title2> { data($b2/Title) } </Title2>
     </BookPair>
  ```
  - Invert data: Authors with the books they've written
  ```XQuery
  <InvertedBookstore>
    { for $ln in distinct-values(doc("BookstoreQ.xml")//Author/Last_Name)
      for $fn in distinct-values(doc("BookstoreQ.xml")//Author[
                                      Last_Name=$ln]/First_Name)
      return
         <Author>
            <First_Name> { $fn } </First_Name>
            <Last_Name> { $ln } </Last_Name>
            { for $b in doc("BookstoreQ.xml")/Bookstore/Book[
                                   Authors/Author/Last_Name = $ln]
              return <Book>
                        { $b/@ISBN } { $b/@Price } { $b/@Edition }
                        { $b/Title } {$b/Remark }
                     </Book> }
         </Author> }
  </InvertedBookstore>
  ```
