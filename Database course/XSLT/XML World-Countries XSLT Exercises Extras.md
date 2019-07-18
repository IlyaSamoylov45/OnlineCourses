XML World-Countries XSLT Exercises Extras
  - Q1 : Find all country names containing the string "stan"; return each one within a "Stan" element. (Note: To specify quotes within an already-quoted XPath expression, use quot;.)
  ```XSLT
  <?xml version="1.0" encoding="ISO-8859-1"?>
  <xsl:stylesheet version="2.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
      <xsl:template match="//country[contains(@name, 'stan')]">
           <Stan>
               <xsl:value-of select='@name'/>
           </Stan>
      </xsl:template>    

      <xsl:template match="text()" />
  </xsl:stylesheet>
  ```

  - Q2 : Remove from the data all countries with area greater than 40,000 and all countries with no cities listed. Otherwise the structure of the data should be the same.
  ```XSLT
  <?xml version="1.0" encoding="ISO-8859-1"?>
  <xsl:stylesheet version="2.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
      <xsl:template match="countries">
          <countries>
               <xsl:for-each select="country[@area &lt; 40000 and count(city) &gt; 0]">
                   <xsl:copy-of select="." />
               </xsl:for-each>
          </countries>
      </xsl:template>    

  </xsl:stylesheet>
  ```
