
# Mini project - analysing San Francisco Crime Incidents from 2011 to 2017 by using BigQuery SQL

Hi, welcome to my mini-project to analyse San Francisco Crime Incidents.

The data is extracted from City and County of San Francisco - San Francisco Police Department reports - publicly available on BigQuery.


## 1) Objective questions
After previewing the datasets from BigQuery, I'd like to extract the data to answer the questions below:
- What is the overall total number of incidents from 2011 to 2017?
- What are the categories of crime incidents in the High frequency bracket?
- Where do the crime incidents of the said categories in the High frequency braket take place?

## 2) Data Query and Analysing
The dataset table has a total of 12 columns: unique_key, category, descript, dayofweek, pddistrict, resolution, address, longtitude, latitude, location, pdid, timestamp. To answer the objective questions, I only focus on: unique_key, category, pddistrict and timestamp.

### A) Query 1:

a) Split the year from timestamp:


```
SELECT
      unique_key,
      category,
      EXTRACT(year from timestamp) AS year
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
      EXTRACT(year from timestamp) AS year
      FROM
      bigquery-public-data.san_francisco_sfpd_incidents.sfpd_incidents)
WHERE
    year >= 2011
GROUP BY
    category, year
ORDER BY
    number_of_incidents ASC
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
      EXTRACT(year from timestamp) AS year
      FROM
      bigquery-public-data.san_francisco_sfpd_incidents.sfpd_incidents)
    WHERE
    year >= 2011
    GROUP BY
    category, year
    ORDER BY
    number_of_incidents ASC)
```

### B) Query 2: Create the similar set of query like (a), (b) and (c) with column pddistrict

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
      EXTRACT(year from timestamp) AS year
      FROM
      bigquery-public-data.san_francisco_sfpd_incidents.sfpd_incidents)
    WHERE
    year >= 2011
    GROUP BY
    category, year, pddistrict
    ORDER BY
    number_of_incidents ASC)
```


## 3) Results
As this is a mini-project, I download the two queries as CSV and manipulate the file with Excel.

*Note: this dataset from San Francisco Police Department has not been updated from 2018 onwards; hence, the number in here is not correctly reflecting for 2018.

a) Overall total number of crime incidents from 2011 to 2017:

![alt text](https://github.com/tatiendat21/SFPDcrimeincidents/blob/main/Overall%20total%20number%20of%20crime%20incidents.png?raw=true)

b) Crime incidents in the High frequency bracket:

![alt text](https://github.com/tatiendat21/SFPDcrimeincidents/blob/main/Crime%20incidents%20in%20the%20high%20frequency%20bracket.png?raw=true)

![alt text](https://github.com/tatiendat21/SFPDcrimeincidents/blob/main/Crime%20incidents%20in%20the%20high%20frequency%20bracket%20(2).png?raw=true)

c) Districts where crime incidents categories in the High frequency bracket happened:

![alt text](https://github.com/tatiendat21/SFPDcrimeincidents/blob/main/District%20where%20crime%20incident%20categories%20happened%20in%20the%20high%20frequency%20bracket.png?raw=true)

Please also check out my work-in-progress data visualisation on Tableau: https://public.tableau.com/views/BriefcrimeincidentsstatsinSanfranciscofrom2011to2017/Dashboard1?:language=en-US&:display_count=n&:origin=viz_share_link

## 🚀 About Me
A data enthusiast.

## Authors

- [@tatiendat21](https://github.com/tatiendat21)

- Linkedin: https://www.linkedin.com/in/datta21/
