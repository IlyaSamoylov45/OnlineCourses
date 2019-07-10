Lecture 19 XPath Demo
  - All book titles
  ```
  doc("BookstoreQ.xml")/Bookstore/Book/Title
  ```
  - All book or magazine titles
  ```
  doc("BookstoreQ.xml")/Bookstore/(Book|Magazine)/Title
  ```
  - All titles
  ```
  doc("BookstoreQ.xml")/Bookstore/*/Title
  ```
  - All titles another version
  ```
  doc("BookstoreQ.xml")//Title
  ```
  - All elements
  ```
  doc("BookstoreQ.xml")//*
  ```
  - All book ISBNs
  ```
  doc("BookstoreQ.xml")/Bookstore/Book/data(@ISBN)
  ```
  - All books costing less than $90
  ```
  doc("BookstoreQ.xml")/Bookstore/Book[@Price < 90]
  ```
  - Titles of books costing less than $90
  ```
  doc("BookstoreQ.xml")/Bookstore/Book[@Price < 90]/Title
  ```
  - Titles of books containing a remark
  ```
  doc("BookstoreQ.xml")/Bookstore/Book[Remark]/Title
  ```
  - Titles of books costing less than $90 where "Ullman" is an author
  ```
  doc("BookstoreQ.xml")/Bookstore/Book[@Price < 90 and Authors/Author/Last_Name = "Ullman"]/Title
  ```
  - Same query but "Jeffrey Ullman" is an author
  ```
  doc("BookstoreQ.xml")/Bookstore/Book[@Price < 90 and Authors/Author[Last_Name = "Ullman" and First_Name="Jeffrey"]]/Title
  ```
  - Titles of books where "Ullman" is an author and "Widom" is not an author
  ```
  doc("BookstoreQ.xml")/Bookstore/Book[Authors/Author/Last_Name = "Ullman" and Authors/Author/Last_Name != "Widom"]/Title
  ```
  - All second authors, third, tenth authors
  ```
  doc("BookstoreQ.xml")//Authors/Author[2]
  doc("BookstoreQ.xml")//Authors/Author[3]
  doc("BookstoreQ.xml")//Authors/Author[10]
  ```
  - Titles of books with a remark containing "great"
  ```
  doc("BookstoreQ.xml")//Book[contains(Remark, "great")]/Title
  ```
  - All magazines where there's a book with the same title
  ```
  doc("BookstoreQ.xml")//Magazine[Title = doc("BookstoreQ.xml")//Book/Title]
  ```
  - All elements whose parent tag is not "Bookstore" or "Book"
  ```
  doc("BookstoreQ.xml")/Bookstore//*[name(parent::*) != "Bookstore" and name(parent::*) != "Book"]
  ```
  - All books and magazines with non-unique titles
  ```
  doc("BookstoreQ.xml")/Bookstore/(Book|Magazine)[Title = following-sibling::*/Title or Title = preceding-sibling::*/Title]
  ```
  - Books where every author's first name includes "J"
  ```
  doc("BookstoreQ.xml")//Book[count(Authors/Author[contains(First_Name, "J")]) = count(Authors/Author/First_Name)]
  ```
  - Titles of books where "Ullman" is an author and "Widom" is not an author
  ```
  doc("BookstoreQ.xml")/Bookstore/Book[Authors/Author/Last_Name = "Ullman" and count(Authors/Author[Last_Name = "Widom"]) = 0]/Title
  ```
