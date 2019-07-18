XML Course-Catalog XSLT Exercises
  - Q1 : Return a list of department titles.
  ```XSTL
  <xsl:stylesheet version="2.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
  <xsl:output method="xml" indent="yes" omit-xml-declaration="yes" />

  <xsl:template match="Department/Title">
     <xsl:copy-of select="." />
  </xsl:template>

  <xsl:template match="text()" />

  </xsl:stylesheet>
  ```

  - Q2 : Return a list of department elements with no attributes and two subelements each: the department title and the entire Chair subelement structure.
  ```XSTL
  <?xml version="1.0" encoding="ISO-8859-1"?>
  <xsl:stylesheet version="2.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
      <xsl:template match="Department">
          <Department>
               <Title><xsl:value-of select="Title" /></Title>  
               <xsl:copy-of select="./Chair" />
          </Department>
      </xsl:template>
  </xsl:stylesheet>
  ```
