XML World-Countries XPath and XQuery Exercises Extras
  - Q1 :
    - XQuery
    ```XQuery
    for $countries in doc("countries.xml")/countries/country
    where $countries/@population > 100000000
    return $countries/data(@name)
    ```
    - XPath
    ```XPath
    doc("countries.xml")/countries/country[@population > 100000000]/data(@name)
    ```

  - Q2 : Return the names of all countries where over 50% of the population speaks German. (Hint: Depending on your solution, you may want to use ".", which refers to the "current element" within an XPath expression.)
    - XQuery
    ```XQuery
    for $countries in doc("countries.xml")//country
    where $countries/language[@percentage > 50 and . = 'German']
    return $countries/data(@name)
    ```
    - XPath
    ```XPath
    doc("countries.xml")//country/language[@percentage > 50 and . = 'German']/parent::country/data(@name)
    ```

  - Q3 : Return the names of all countries where a city in that country contains more than one-third of the country's population.
    - XQuery
    ```XQuery
    for $country in doc("countries.xml")/countries/country
    let $city_population := $country/city/population
    where $city_population > $country/@population * 0.33
    return $country/data(@name)
    ```
    - XPath
    ```XPath
     doc("countries.xml")/countries/country[city/population > @population * 0.33]/data(@name)
    ```

  - Q4 : Return the population density of Qatar. Note: Since the "/" operator has its own meaning in XPath and XQuery, the division operator is "div". To compute population density use "(@population div @area)".
    - XQuery
    ```XQuery
    for $country in doc("countries.xml")/countries/country
    where $country/@name = 'Qatar'
    return $country/(@population div @area)
    ```
    - XPath
    ```XPath
    doc("countries.xml")/countries/country[@name = 'Qatar']/(@population div @area)
    ```

  - Q5 : Return the names of all countries whose population is less than one thousandth that of some city (in any country).
    - XQuery
    ```XQuery
    for $country in doc("countries.xml")/countries/country
    where $country[@population * 1000  < //city/population]
    return $country/data(@name)
    ```
    - XPath
    ```XPath
    doc("countries.xml")/countries/country[@population * 1000  < //city/population]/data(@name)
    ```
  - Q6 : Return all city names that appear more than once, i.e., there is more than one city with that name in the data. Return only one instance of each such city name. (Hint: You might want to use the "preceding" and/or "following" navigation axes for this query, which were not covered in the video or our demo script; they match any preceding or following node, not just siblings.)
    - XQuery
    ```XQUERY
    for $city in doc('countries.xml')/countries/country/city
    where $city/name = $city/following::city/name
    return $city/name
    ```
    - XPath
    ```XPath
    doc('countries.xml')/countries/country/city[(name = (./following::city/name))]/name
    ```

  - Q7 : Return the names of all countries containing a city such that some other country has a city of the same name. (Hint: You might want to use the "preceding" and/or "following" navigation axes for this query, which were not covered in the video or our demo script; they match any preceding or following node, not just siblings.)
    - XQuery
    ```XQUERY
    for $city in doc('countries.xml')/countries/country/city
    where $city/name = $city/following::city/name or $city/name = $city/preceding::city/name
    return $city/parent::country/data(@name)
    ```
    - XPath
    ```XPath
    doc('countries.xml')/countries/country/city[(name = (./following::city/name) or name = (./preceding::city/name))]/parent::country/data(@name)
    ```

  - Q8 : Return the names of all countries whose name textually contains a language spoken in that country. For instance, Uzbek is spoken in Uzbekistan, so return Uzbekistan. (Hint: You may want to use ".", which refers to the "current element" within an XPath expression.)
    - XQuery
    ```XQUERY
    for $country in doc('countries.xml')/countries/country
    where some $part in $country/language satisfies contains($country/@name, $part)
    return $country/data(@name)
    ```
    - XPath
    ```XPath
    doc("countries.xml")/countries/country[language[contains(parent::country/@name, .)]]/data(@name)
    ```

  - Q9 : Return the names of all countries in which people speak a language whose name textually contains the name of the country. For instance, Japanese is spoken in Japan, so return Japan. (Hint: You may want to use ".", which refers to the "current element" within an XPath expression.)
    - XQuery
    ```XQUERY
    for $country in doc('countries.xml')/countries/country
    where some $part in $country/language satisfies contains($part, $country/@name)
    return $country/data(@name)
    ```
    - XPath
    ```XPath
    doc("countries.xml")/countries/country[language[contains(., parent::country/@name)]]/data(@name)
    ```

  - Q10 : Return all languages spoken in a country whose name textually contains the language name. For instance, German is spoken in Germany, so return German. (Hint: Depending on your solution, may want to use data(.), which returns the text value of the "current element" within an XPath expression.)
    - XQuery
    ```XQUERY
    for $a in doc("countries.xml")/countries/country/language
    where $a[contains(parent::country/@name, .)]
    return $a/data(.)
    ```
    - XPath
    ```XPath
    doc("countries.xml")/countries/country/language[contains(parent::country/@name, .)]/data(.)
    ```

  - Q11 : Return all languages whose name textually contains the name of a country in which the language is spoken. For instance, Icelandic is spoken in Iceland, so return Icelandic. (Hint: Depending on your solution, may want to use data(.), which returns the text value of the "current element" within an XPath expression.)
    - XQuery
    ```XQUERY
    for $a in doc("countries.xml")/countries/country/language
    where $a[contains(., parent::country/@name)]
    return $a/data(.)
    ```
    - XPath
    ```XPath
    doc("countries.xml")/countries/country/language[contains(., parent::country/@name)]/data(.)
    ```

  - Q12 : Return the number of countries where Russian is spoken.
    - XQuery
    ```XQUERY
    count(for $country in doc("countries.xml")/countries/country
    where $country/language = "Russian"
    return $country)
    ```
    - XPath
    ```XPath
    count(doc("countries.xml")/countries/country[language = "Russian"])
    ```

  - Q13 : Return the names of all countries for which the data does not include any languages or cities, but the country has more than 10 million people.
    - XQuery
    ```XQUERY
    for $country in doc("countries.xml")/countries/country
    let $languages := $country/count(language) = 0
    let $cities := $country/count(city) = 0
    where $languages and $cities and $country/@population > 10000000
    return $country/data(@name)
    ```
    - XPath
    ```XPath
    doc("countries.xml")/countries/country[count(language) = 0 and count(city) = 0 and @population > 10000000]/data(@name)
    ```

  - Q14 : Return the name of the country with the highest population. (Hint: You may need to explicitly cast population numbers as integers with xs:int() to get the correct answer.)
    - XQuery
    ```XQUERY
    let $country := doc("countries.xml")/countries/country
    let $big := $country[@population = max($country/data(@population))]
    return $big/data(@name)
    ```
    - XPath
    ```XPath
    doc("countries.xml")/countries/country[@population = max(doc("countries.xml")/countries/country/@population)]/data(@name)
    ```

  - Q15 : Return the name of the country that has the city with the highest population. (Hint: You may need to explicitly cast population numbers as integers with xs:int() to get the correct answer.)
    - XQuery
    ```XQUERY
    let $country := doc("countries.xml")/countries/country
    let $big := $country[city/population = max($country/city/population)]
    return $big/data(@name)
    ```
    - XPath
    ```XPath
     doc("countries.xml")/countries/country[city/population = max(doc("countries.xml")/countries/country/city/population)]/data(@name)
    ```

  - Q16 : Return the average number of languages spoken in countries where Russian is spoken.
    - XQuery
    ```XQUERY
    avg(
        for $country in doc("countries.xml")/countries/country
        let $languageCount := $country[language = 'Russian']/count(language)
        return $languageCount
       )
    ```
    - XPath
    ```XPath
    avg(doc("countries.xml")/countries/country[language = 'Russian']/count(language))
    ```

  - Q17 : Return all country-language pairs where the language is spoken in the country and the name of the country textually contains the language name. Return each pair as a country element with language attribute, e.g.,
  ```
  <country language="French">French Guiana</country>
  ```
    - XQuery
    ```XQUERY
    for $country in doc("countries.xml")/countries/country/language
    where $country[contains(parent::country/data(@name), .)]
    return <country language = "{$country}">
           {$country/parent::country/data(@name)}
           </country>
    ```
    - XPath
    ```XPath
    doc("countries.xml")/countries/country/language[contains(parent::country/data(@name), .)]/<country language = "{.}">{parent::country/data(@name)}</country>
    ```

  - Q18 : Return all countries that have at least one city with population greater than 7 million. For each one, return the country name along with the cities greater than 7 million, in the format:
  ```
  <country name="country-name">
    <big>city-name</big>
    <big>city-name</big>
    ...
  </country>
  ```
    - XQuery
    ```XQUERY
    for $country in doc("countries.xml")/countries/country
    where $country/city[population > 7000000]
    return <country name="{$country/data(@name)}">
    {
      for $city in $country/city
      where $city[population > 7000000]
      return <big>{$city/data(name)}</big>
    }
    </country>
    ```

  - Q19 : Return all countries where at least one language is listed, but the total percentage for all listed languages is less than 90%. Return the country element with its name attribute and its language subelements, but no other attributes or subelements.
    - XQuery
    ```XQUERY
    for $country in doc('countries.xml')/countries/country
    where $country[exists(language) and sum(language/data(@percentage)) < 90]
    return <country name="{$country/@name}">{
      for $c in $country/language
      return $c
    }</country>
    ```

  - Q20 : Return all countries where at least one language is listed, and every listed language is spoken by less than 20% of the population. Return the country element with its name attribute and its language subelements, but no other attributes or subelements.
    - XQuery
    ```XQUERY
    for $country in doc("countries.xml")/countries/country[language]
    where every $l in $country/language satisfies $l/data(@percentage) < 20
    return <country name = '{$country/data(@name)}'>{
      for $c in $country
      return $c/language
    }</country>
    ```

  - Q21 : Find all situations where one country's most popular language is another country's least popular, and both countries list more than one language. (Hint: You may need to explicitly cast percentages as floating-point numbers with xs:float() to get the correct answer.) Return the name of the language and the two countries, each in the format:
  ```
  <LangPair language="lang-name">
    <MostPopular>country-name</MostPopular>
    <LeastPopular>country-name</LeastPopular>
  </LangPair>
  ```
    - XQuery
    ```XQUERY
    let $countries :=  doc("countries.xml")//country[count(language) >= 2]

    let $most :=
      for $c in $countries
      for $lan in $c/language
      where $lan/@percentage = max($c/language/@percentage)
      return $lan

    let $least :=
      for $c in $countries
      for $lan in $c/language
      where $lan/@percentage = min($c/language/@percentage)
      return $lan

    for $m in $most
    for $l in $least
    where $m = $l
    return
      <LangPair language="{ $m }">
        <MostPopular>{ $m/parent::country/data(@name) }</MostPopular>
        <LeastPopular>{ $l/parent::country/data(@name) }</LeastPopular>
      </LangPair>
    ```

  - Q22 : For each language spoken in one or more countries, create a "language" element with a "name" attribute and one "country" subelement for each country in which the language is spoken. The "country" subelements should have two attributes: the country "name", and "speakers" containing the number of speakers of that language (based on language percentage and the country's population). Order the result by language name, and enclose the entire list in a single "languages" element. For example, your result might look like:
  ```
  <languages>
    ...
    <language name="Arabic">
      <country name="Iran" speakers="660942"/>
      <country name="Saudi Arabia" speakers="19409058"/>
      <country name="Yemen" speakers="13483178"/>
    </language>
    ...
  </languages>
  ```
    - XQuery
    ```XQUERY

    ```
