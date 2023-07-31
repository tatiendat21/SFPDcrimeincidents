
# Mini project - analysing Sanfrancisco Crime Incidents from 2011 to 2018 by using BigQuery SQL

Hi, welcome to my mini-project to analyse Sanfrancisco Crime Incidents.

The data is extracted from City and County of Sanfrancisco - Sanfrancisco Polic Department reports - publicly available on BigQuery.


## Walkthrough
## 2) Data Query and Analysing
The dataset table has a total of 12 columns: unique_key, category, descript, dayofweek, pddistrict, resolution, address, longtitude, latitude, location, pdid, timestamp. To answer the objective questions, I only focus on: unique_key, category, pddistrict and timestamp.

### A) Query 1:

a) Split the year from timestamp:


```
SELECT
      unique_key,
      category,
      EXTRACT(year from timestamp) as year
FROM
      bigquery-public-data.san_francisco_sfpd_incidents.sfpd_incidents
```


b) Count the value of unique_key, group by category and year, where year is more than and equate to 2011, and FROM the subquery of table in (a):

```
SELECT
    COUNT(unique_key) AS number_of_incidents,
    category,
    year,
FROM
      (SELECT
      unique_key,
      category,
      EXTRACT(year from timestamp) as year
      FROM
      bigquery-public-data.san_francisco_sfpd_incidents.sfpd_incidents)
WHERE
    year >= 2011
GROUP BY
    category, year
ORDER BY
    number_of_incidents ASC)
```

c) Create a case statement to set condition for Low, Medium and High frequency and FROM the subquery of table (a) and (b) - together, the whole query is (c) = (a) + (b)

```
SELECT
 number_of_incidents,
 category,
 year,
 CASE
 WHEN number_of_incidents < 1000
 THEN "Low frequency"
 WHEN number_of_incidents >=1000 AND number_of_incidents <= 8000
 THEN "Medium frequency"
 ELSE "High frequency"
 END AS evaluation
FROM
    (SELECT
    COUNT(unique_key) AS number_of_incidents,
    category,
    year,
    FROM
      (SELECT
      unique_key,
      category,
      EXTRACT(year from timestamp) as year
      FROM
      bigquery-public-data.san_francisco_sfpd_incidents.sfpd_incidents)
    WHERE
    year >= 2011
    GROUP BY
    category, year
    ORDER BY
    number_of_incidents ASC)
```

### B) QUERY 2: Create the similar set of query like (a), (b) and (c) with column pddistrict

```
SELECT
 number_of_incidents,
 category,
 year,
 pddistrict,
 CASE
 WHEN number_of_incidents < 1000
 THEN "Low frequency"
 WHEN number_of_incidents >=1000 AND number_of_incidents <= 8000
 THEN "Medium frequency"
 ELSE "High frequency"
 END AS evaluation
FROM
    (SELECT
    COUNT(unique_key) AS number_of_incidents,
    category,
    year,
    pddistrict
    FROM
      (SELECT
      unique_key,
      category,
      pddistrict,
      EXTRACT(year from timestamp) as year
      FROM
      bigquery-public-data.san_francisco_sfpd_incidents.sfpd_incidents)
    WHERE
    year >= 2011
    GROUP BY
    category, year, pddistrict
    ORDER BY
    number_of_incidents ASC)
```


## 1) Objective questions
After previewing the datasets from BigQuery, I'd like to extract the data to answer the questions below:
- How is the total number of incidents changing from 2011 to 2017?
- What are the categories of crime incidents in the high frequency bracket?
- How are the number of crime incidents of the said categories in question 2 changing from 2011 to 2017?
## 4) Results
As this is a mini-project, I download the two queries as CSV and manipulate the file with Excel.

a) Overall total number of crime incidents from 2011 to 2018:

![alt text](https://ibb.co/wN8NrYN)

b) Crime incidents in the High frequency bracket:

![alt text](https://ibb.co/YBxwdtH)

![alt text](https://ibb.co/DLLQGhk)

c) Districts where crime incidents categories in the High frequency bracket happened:

![alt text](https://ibb.co/q5wRk4q)
## 🚀 About Me
I'm an Accountant who is dedicated to pursue the new career path as a Data Analyst.


## Authors

- [@tatiendat21](https://github.com/tatiendat21)

- Linkedin: https://www.linkedin.com/in/datta21/