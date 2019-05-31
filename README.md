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

![alt text](https://github.com/khouly/exploring-weather-trends/blob/master/data.png "Data in Excel")

## 3. Preparing the formulas in Excel

* I used this formula to dynamically collect the data from each sheet

```
=IFNA(VLOOKUP($A2,INDIRECT(B$1&"!A:B"),2,FALSE), "")
```

* It looks up the value for each area and adds it to the main sheet
* I used this formula to dynamically calculate the moving average for each area using a dynamic moving window to try out different sizes and see the sweet spot that makes it smooth enough without losing important information

```
=IF(COUNT(OFFSET(B2,0,0,-$L$2,1))=$L$2, AVERAGE(OFFSET(B2,0,0,-$L$2,1)), NA())
```
* I kept the NA fields so that I can omit them in the chart as gaps in the data
* Added a new Excel sheet to the workbook for the chart
* Inserted a chart and selected the moving average data from all areas
* Added the labels, legend and the dynamic title that represents the window sized used using the formula below

```
="Moving average temperature trends"&CHAR(13)&"Years: 1743-2015"&CHAR(13)&"Window size: "&Data!L2
```

## 4. Creating the charts

* I created 6 charts based on the window size of the moving average
* A moving average of size one represents the actual averages
* The charts are listed in the next 6 slides for the following window sizes: 1, 2, 4, 6, 8 and 10
* I included the extra cities in all charts

### Moving average temperature trends for Years: 1743-2015 with a window size of 1
![alt text](https://github.com/khouly/exploring-weather-trends/blob/master/1.png "Moving average temperature trends for Years: 1743-2015 with a window size of 1")

### Moving average temperature trends for Years: 1743-2015 with a window size of 2
![alt text](https://github.com/khouly/exploring-weather-trends/blob/master/2.png "Moving average temperature trends for Years: 1743-2015 with a window size of 2")

### Moving average temperature trends for Years: 1743-2015 with a window size of 4
![alt text](https://github.com/khouly/exploring-weather-trends/blob/master/4.png "Moving average temperature trends for Years: 1743-2015 with a window size of 4")

### Moving average temperature trends for Years: 1743-2015 with a window size of 6
![alt text](https://github.com/khouly/exploring-weather-trends/blob/master/6.png "Moving average temperature trends for Years: 1743-2015 with a window size of 6")

### Moving average temperature trends for Years: 1743-2015 with a window size of 8
![alt text](https://github.com/khouly/exploring-weather-trends/blob/master/8.png "Moving average temperature trends for Years: 1743-2015 with a window size of 8")

### Moving average temperature trends for Years: 1743-2015 with a window size of 10
![alt text](https://github.com/khouly/exploring-weather-trends/blob/master/10.png "Moving average temperature trends for Years: 1743-2015 with a window size of 10")

# Analysis:

## 1. Missing data
* There are some missing data in the Cairo data set compared to the global data set
* Cairo has data from 1808 to 2013
* Global data from 1750 to 2015

![alt text](https://github.com/khouly/exploring-weather-trends/blob/master/o1.png "Missing data")

## 2. Difference in temperature
* There is a huge difference between average temperature in Cairo and the global average
* The average of this difference is 12.77 °C for the years 1808 to 2013

![alt text](https://github.com/khouly/exploring-weather-trends/blob/master/o2.png "Difference in temperature")

## 3. Rate of increase
* The chart shows that the weather in Cairo and globally has been increasing over time
* There is also increase in the steepness of the slope in the most recent years which indicates that the rate of higher temperature is increasing 

![alt text](https://github.com/khouly/exploring-weather-trends/blob/master/o3.png "Rate of increase")

## 4. Correlation
* There is a positive correlation between the global weather averages and the weather averages in Cairo as they follow a similar slope
* This correlation is more and more clear when you check the charts with higher window sizes

![alt text](https://github.com/khouly/exploring-weather-trends/blob/master/o4.png "Correlation")

## 5. Data error
* In year 1818 there is step dive in the temperature in Cairo for 1 year going down from 21.88 to 11.60 and then back up to 20.31
* In my opinion this might be a data error as it is the only outlier and the weather never reached this low again
* Also the global weather was much more stable around these years

![alt text](https://github.com/khouly/exploring-weather-trends/blob/master/o5.png "Data error")

## 6. Correlation coefficients
* There is a positive correlation between the average temperature globally and the selected cities
* Cairo has the highest correlation between the 3 cities to the global average
* As you increase the window size they get more and more correlated which indicates that they are following the same trend but with some fluctuations
* These values were calculated with the CORREL function in Excel

| Window size   | 1 | 2 | 4 | 6 | 8 | 10 |
| --- | --- | --- | --- | --- | --- | --- |
| Global - Cairo | 0.583 | 0.753 | 0.869 | 0.904 | 0.921 | 0.933 |
| Global - Amsterdam | 0.517 | 0.611 | 0.708 | 0.780 | 0.823 | 0.848 |
| Global - Berlin | 0.516 | 0.597 | 0.699 | 0.777 | 0.827 | 0.858 |

## 7. Correlation vs difference
* Even though Cairo has the highest correlation with the global average weather both Amsterdam and Berlin have much less variance in the average temperature

![alt text](https://github.com/khouly/exploring-weather-trends/blob/master/o7.png "Correlation vs difference")
