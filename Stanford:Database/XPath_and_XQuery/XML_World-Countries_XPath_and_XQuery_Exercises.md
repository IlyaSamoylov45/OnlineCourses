XML World-Countries XPath and XQuery Exercises
  - Q1 : Return the area of Mongolia.
    - XQuery:
    ```XQUERY
    for $p in doc("countries.xml")/countries/country
    where $p/data(@name) = 'Mongolia'
    return $p/data(@area)
    ```
    - XPath:
    ```XPath
    doc("countries.xml")/countries/country[@name = 'Mongolia']/data(@area)
    ```

  - Q2 : Return the names of all cities that have the same name as the country in which they are located.
    - XQuery:
    ```XQUERY
    for $b in doc("countries.xml")//country
    where $b/city/name = $b/@name
    return <name>
           {$b/data(@name)}
           </name>
    ```
    ```XQUERY
    for $b in doc("countries.xml")//city
    where $b/name = $b/parent::country/@name
    return $b/name
    ```
    - XPath:
    ```XPath
    doc("countries.xml")/countries/country[city/name = @name]/<name>{data(@name)}</name>
    ```
    ```XPath
    doc("countries.xml")/countries/country/city[name = parent::country/@name]/name
    ```

  - Q3 : Return the average population of Russian-speaking countries.
    - XQuery:
    ```XQUERY
    for $country in doc('countries.xml')/countries
    let $a := $country//country[language = 'Russian']
    return avg($a/data(@population))
    ```
    - XPath:
    ```XPath
    avg(doc("countries.xml")//country[language='Russian']/data(@population))
    ```

  - Q4 : Return the names of all countries that have at least three cities with population greater than 3 million.
    - XQuery:
    ```XQUERY
    for $a in doc("countries.xml")/countries/country
    where $a[count(city[population>3000000]) >= 3]
    return $a/data(@name)
    ```
    - XPath:
    ```XPath
    doc("countries.xml")/countries/country[count(city[population>3000000]) >= 3]/data(@name)
    ```

  - Q5 : Create a list of French-speaking and German-speaking countries. The result should take the form:
    - XQuery
    ```XQUERY
    <result>
      <French>
        {
        for $country in doc("countries.xml")//country
        where $country/language = "French"
        return <country>{$country/data(@name)}</country>
        }
      </French>
      <German>
        {
        for $country in doc("countries.xml")//country
        where $country/language = "German"
        return <country>{$country/data(@name)}</country>
        }
      </German>
    </result>
    ```
    ```XQUERY
    let $c := doc('countries.xml')/countries/country
    let $french := $c[language = 'French']/data(@name)
    let $german := $c[language = 'German']/data(@name)
    return <result>
      <French>
      {
        if (count($french) > 0) then
        for $a in $french
        return <country>{$a}</country>
        else()
      }
      </French>
      <German>
      {
        if (count($german) > 0) then
        for $a in $german
        return <country>{$a}</country>
        else()
      }
      </German>
    </result>
    ```

  - Q6 : Return the countries with the highest and lowest population densities. Note that because the "/" operator has its own meaning in XPath and XQuery, the division operator is infix "div". To compute population density use "(@population div @area)". You can assume density values are unique. The result should take the form:
    - XQuery
    ```XQUERY
    let $a := doc("countries.xml")/countries/country
    let $highest_density := (
      for $d in $a
      where $d/data(@population div @area) = max($a/data(@population div @area))
      return $d
    )
    let $lowest_density := (
      for $d in $a
      where $d/data(@population div @area) = min($a/data(@population div @area))
      return $d
    )
    return <result>
           <highest density="{$highest_density/data(@population div @area)}">{$highest_density/data(@name)}</highest>
           <lowest density="{$lowest_density/data(@population div @area)}">{$lowest_density/data(@name)}</lowest>
           </result>
    ```
