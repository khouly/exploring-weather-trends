# Exploring weather trends
Analyzing local and global temperature data and compare the temperature trends where I live (Cairo) to overall global temperature trends and two other cities. To do this, I followed the steps below:

## 1. Extracted the data from the database
There are three tables in the database:
* city_list - This contains a list of cities and countries in the database. Look through them in order to find the city nearest to you.
* city_data - This contains the average temperatures for each city by year (ºC).
* global_data - This contains the average global temperatures by year (ºC).

Used the below SQL query to find the closest big city near me in the database and based on the output Cairo is the closest city to me.

```SQL
SELECT city
FROM city_list
WHERE country = 'Egypt';
```

Used the below SQL queries to get the cities weather data (Cairo, Amsterdam and Berlin) and the golbal weather data.

```SQL
SELECT year, avg_temp
FROM city_data
WHERE city = 'Cairo';

SELECT year, avg_temp
FROM city_data
WHERE city = 'Amsterdam';

SELECT year, avg_temp
FROM city_data
WHERE city = 'Berlin';

SELECT year, avg_temp
FROM global_data;
```

Exported the temperature data to CSV files.

## 2. Preparing the data in Excel

* Renamed the files to represent the appropriate location
* I noticed that there are differences in the number of records returned from my SQL queries
* After investigation I found that the starting and ending years are not always the same
* Created a new Excel sheet as a copy of the global data and added the missing years so that all files have rowws for the years 1743 to 2015
* Made a copy of each CSV file as a sheet in the main Excel file
