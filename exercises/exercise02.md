# Exercise 02: World Database – Joins, Grouping, and Data Quality

- Name: Houston Asher-Lawes
- Course: Database for Analytics
- Module: 2
- Database Used: World Database (PostgreSQL)

---

## Instructions

- Answer each question below using SQL executed against the **World database**.
- All SQL commands **must be run by you**.
- For each SQL-based question:
  - Include the SQL command in a fenced code block
  - Include a **screenshot** showing the command and its results
- Store screenshots in the `screenshots/` folder and embed them below each answer.

---

## Question 1

When importing records from `worldPGSQL.sql`, **how many cities were imported**?

### Answer
4079

### Screenshot
_Show evidence of how you determined this (for example, a COUNT query)._

```sql
Select Count(*) from city
```

![Q1 Screenshot](screenshots/Exercise02/question%201.png)

---

## Question 2

Using the World database, write the SQL command to **display each country name along with the name of each language spoken in that country**.

### SQL

```sql
SELECT c.name as country_name, 
cl.language as language
FROM country c
JOIN countrylanguage cl
  ON c.code = cl.countrycode
Order by c.name, cl.language
```

### Screenshot

![Q2 Screenshot](screenshots/Exercise02/question%202.png)

---

## Question 3

Using the World database, write the SQL command to **display each country name along with the name of each official language spoken in that country**.

### SQL

```sql
SELECT c.name as country_name, 
cl.language as official_language
FROM country c
JOIN countrylanguage cl
  ON c.code = cl.countrycode
  Where cl.isofficial = 'T'
Order by c.name, cl.language
```

### Screenshot

![Q3 Screenshot](screenshots/Exercise02/question%203.png)

---

## Question 4

Consider the following two SQL statements:

```sql
SELECT *
FROM country, countrylanguage
WHERE country.code = countrylanguage.countrycode;
```

```sql
SELECT *
FROM country
LEFT OUTER JOIN countrylanguage
ON country.code = countrylanguage.countrycode;
```

**In your own words**, describe what data the **second query returns that the first query does not**.

### Answer
It will return all the countries with no languages unlike the the first query.

## Question 5

Using the World database, write the SQL command to **list all different forms of government** found in the data.
Do **not** repeat any form of government more than once.

### SQL

```sql
Select distinct governmentform 
from Country
order by governmentform
```

### Screenshot

![Q5 Screenshot](screenshots/Exercise02/question%205.png)

---

## Question 6

Using the World database, write the SQL command to **list all names of cities and countries in one column**.
Label the column **"City or Country Name"**.

### SQL

Select name as "City or Country Name"
from city

Union

select name
from country;

### Screenshot

![Q6 Screenshot](screenshots/Exercise02/Question%206.png)

---

## Question 7

Using the World database, write the SQL command to **list all countries by name**, along with the **number of languages spoken in each country**.
Be sure to **sort by country name**.

### SQL
Select c.name as Country_name, 
Count (cl.language) as Number_Languages_Spoken
From country c
Join countrylanguage cl
ON c.code = cl.countrycode
Group by c.name
Order by c.name


### Screenshot

![Q7 Screenshot](screenshots/Exercise02/Question%207.png)

---

## Question 8

Using the World database, write the SQL command to **list all languages**, along with the **number of countries where each language is spoken**.
Be sure to **sort by language name**.

### SQL

Select cl.language as All_Languages, 
Count (c.name) as NumberOfCountries_Languages_Spoken
From country c
Join countrylanguage cl
ON c.code = cl.countrycode
Group by cl.language
Order by cl.language

### Screenshot

![Q8 Screenshot](screenshots/Exercise02/Question%208.png)

---

## Question 9

Using the World database, write the SQL command to **list countries that have more than two official languages**, along with the **number of official languages spoken**.

*Hint: There are 8 such countries in this dataset.*

### SQL

Select c.name as Countries_twoOrMore_Languages,
Count (cl.language) as Number_Languages_Spoken
From country c
Join countrylanguage cl
ON c.code = cl.countrycode
where cl.isofficial = 'T'
Group by c.name
Having Count(cl.language) > 2
Order by c.name;

### Screenshot

![Q9 Screenshot](screenshots/Exercise02/Question%209.png)

---

## Question 10

Using the World database, write the SQL command to **find cities where the district value is missing**.

*Hint: Use `LIKE` and the dash (`-`) since some rows use that instead of actual data.*

### SQL

SELECT name AS Cities, district
FROM city
WHERE district IS NULL
   OR district LIKE '-'
   OR TRIM(district) = ''
ORDER BY name;


### Screenshot

![Q10 Screenshot](screenshots/Exercise02/Question%2010.png)

---

## Question 11

Using the World database, write the SQL command to **calculate the percentage of cities with missing district values**.

*Hint: The result should be approximately 0.4%.*

### SQL

SELECT
  ROUND(
    100.0 * COUNT(
      CASE
        WHEN district IS NULL
          OR TRIM(district) LIKE '-'
          OR TRIM(district) LIKE '–'
          OR TRIM(district) LIKE '—'
        THEN 1
      END
    ) / COUNT(*),
    2
  ) AS PercentageMissingDistrict
FROM city;


### Screenshot

![Q11 Screenshot](screenshots/Exercise02/Question%2011.png)
