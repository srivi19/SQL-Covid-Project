# SQL-Covid-Project
# COVID-19 Data Analysis Project

## Overview

This project involves downloading COVID-19 case and death data from the [Our World in Data GitHub repository](https://github.com/owid/covid-19-data/blob/master/public/data/cases_deaths/full_data.csv) and analyzing it using PostgreSQL. The data is imported into a PostgreSQL database, transformed, and prepared for visualization in Power BI.

## Table of Contents

1. [Project Setup](#project-setup)
2. [Database Schema](#database-schema)
3. [Data Import](#data-import)
4. [Data Transformation](#data-transformation)
5. [Creating Views](#creating-views)
6. [Challenges Encountered](#challenges-encountered)
7. [Contributing](#contributing)
8. [License](#license)

## Project Setup

1. **Download CSV Data**: 
   - Download the full data CSV from [this link](https://github.com/owid/covid-19-data/blob/master/public/data/cases_deaths/full_data.csv).
   - Save the file to your local disk 

2. **PostgreSQL Setup**:
   - Ensure PostgreSQL is installed.
   - Create a new database for this project.

## Database Schema

Create a table named `covidwho` with the following schema:

```sql
CREATE TABLE covidwho (
    date DATE,
    location VARCHAR,
    new_cases BIGINT,
    new_deaths BIGINT,
    total_cases BIGINT,
    total_deaths BIGINT,
    weekly_cases BIGINT,
    weekly_deaths BIGINT,
    biweekly_cases BIGINT,
    biweekly_deaths BIGINT
);
```

## Data Import

Import the CSV file into the `covidwho` table:

```sql
COPY covidwho FROM 'Your Location.csv' DELIMITER ',' CSV HEADER;
```

## Data Transformation

### Adding Continent Column

Add a new column for continent using a `CASE` statement:

select * ,
case 
when location in ('Algeria','Angola','Benin','Botswana','Burkina Faso','Burundi',
'Cameroon','Cape Verde','Central African Republic','Democratic Republic of Congo
','Cote d''Ivoire','Djibouti','Egypt','Equatorial Guinea','Eritrea','Eswatini','Ethiopia',
'Gabon','Gambia','Ghana','Guinea','Guinea-Bissau','Kenya','Lesotho',
'Liberia','Libya','Madagascar','Malawi','Mali','Mauritania','Mauritius',
'Mayotte','Morocco','Mozambique','Namibia','Niger','Nigeria','Reunion','Rwanda',
'Saint Helena','Sao Tome and Principe','Senegal','Seychelles','Sierra Leone',
'Somalia','South Africa','South Sudan','Sudan','Tanzania','Togo','Tunisia','Uganda','Western Sahara','Zambia','Zimbabwe'
)
then 'Africa'
when location in ('Afghanistan','Armenia','Azerbaijan','Bahrain','Bangladesh','Bhutan','Brunei','Cambodia','China','Georgia',
'Hong Kong','India','Indonesia','Iran','Iraq','Israel','Japan','Jordan','Kazakhstan','Kuwait','Kyrgyzstan','Laos','Lebanon','Macao','Malaysia','Maldives','Mongolia','Myanmar','Nepal','North Korea','Northern Cyprus','Oman','Pakistan','Palestine','Philippines','Qatar','Saudi Arabia','Singapore','South Korea','Sri Lanka','Syria','Taiwan','Tajikistan','Thailand','Timor','Turkey','Turkmenistan','United Arab Emirates','Uzbekistan','Vietnam','Yemen'
) then 'Asia'
when location in ('Albania','Andorra','Austria','Belarus','Belgium','Bosnia and Herzegovina','Bulgaria','Croatia','Cyprus','Czechia','Denmark','England','Estonia','Faeroe Islands','Finland','France','Germany','Gibraltar','Greece','Guernsey','Hungary','Iceland','Ireland','Isle of Man','Italy','Jersey','Kosovo','Latvia','Liechtenstein','Lithuania','Luxembourg','Malta','Moldova','Monaco','Montenegro','Netherlands','North Macedonia','Northern Ireland','Norway','Poland','Portugal','Romania','Russia','San Marino','Scotland','Serbia','Slovakia','Slovenia','Spain','Sweden','Switzerland','Ukraine','United Kingdom','Vatican','Wales'
) then 'Europe'
when location in  ('Anguilla','Antigua and Barbuda','Aruba','Bahamas','Barbados','Belize','Bermuda','Bonaire Sint Eustatius and Saba','British Virgin Islands','Canada','Cayman Islands','Costa Rica','Cuba','Curacao','Dominica','Dominican Republic','El Salvador','Greenland','Grenada','Guadeloupe','Guatemala','Haiti','Honduras','Jamaica','Martinique','Mexico','Montserrat','Nicaragua','Panama','Puerto Rico','Saint Barthelemy','Saint Kitts and Nevis','Saint Lucia','Saint Martin (French part)','Saint Pierre and Miquelon','Saint Vincent and the Grenadines','Sint Maarten (Dutch part)','Trinidad and Tobago','Turks and Caicos Islands','United States','United States Virgin Islands'
) then 'North America'
when location in ('American Samoa','Australia','Cook Islands','Fiji','French Polynesia','Guam','Kiribati','Marshall Islands','Micronesia (country)','Nauru','New Caledonia','New Zealand','Niue','Northern Mariana Islands','Palau','Papua New Guinea','Pitcairn','Samoa','Solomon Islands','Tokelau','Tonga','Tuvalu','Vanuatu','Wallis and Futuna'
) then 'Oceania'

else 'South America'
end as continents
from covidwho


### Adding Year Column

Create an additional view to include a year column:

```sql
CREATE VIEW covidwhoc AS
SELECT *, 
    TO_CHAR(date, 'YYYY') AS year 
FROM covidwho;
```

## Creating Views

Create a view `covidwhoc1` that includes the year column:

```sql
CREATE VIEW covidwhoc1 AS
SELECT *, 
    TO_CHAR(date, 'YYYY') AS year 
FROM covidwhoc;
```

## Challenges Encountered

- **Handling Special Characters**: Ensuring that the continent names are correctly assigned, especially with locations that have special characters.
- **Data Import Issues**: Occasionally, the CSV format may not align perfectly with the table schema, necessitating data cleaning.

## Contributing

Feel free to fork the repository and submit a pull request. Contributions to improve data handling, analysis methods, and documentation are welcome!



---

This README provides a comprehensive overview of the COVID-19 data analysis project, guiding users through setup, schema design, data import, transformation, and challenges encountered.
