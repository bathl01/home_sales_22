# Home Sales
### Module 22 Challenge
## Introduction
Use your knowledge of SparkSQL to determine key metrics about home sales data. Then you'll use Spark to create temporary views, partition the data, cache and uncache a temporary table, and verify that the table has been uncached.
## Findings
__What is the average price for a four-bedroom house sold for each year? Round off your answer to two decimal places.__

        +----+--------+-------------+
        |YEAR|bedrooms|AVERAGE_PRICE|
        +----+--------+-------------+
        |2019|       4|     300263.7|
        |2020|       4|    298353.78|
        |2021|       4|    301819.44|
        |2022|       4|    296363.88|
        +----+--------+-------------+

The average price for a 4 bedroom house was fairly stable from year to year from 2019 through 2022.  Starting in 2019 at $300,263.70 decreasing to $298,353.78 the next year.  In 2021 the price went up to 301,819.44 and finally dropped again to 296,363.88.

__What is the average price of a home for each year the home was built, that has three bedrooms and three bathrooms? Round off your answer to two decimal places.__

        +----------+--------+---------+-------------+
        |date_built|bedrooms|bathrooms|AVERAGE_PRICE|
        +----------+--------+---------+-------------+
        |      2010|       3|        3|    292859.62|
        |      2015|       3|        3|     288770.3|
        |      2012|       3|        3|    293683.19|
        |      2014|       3|        3|    290852.27|
        |      2011|       3|        3|    291117.47|
        |      2017|       3|        3|    292676.79|
        |      2013|       3|        3|    295962.27|
        |      2016|       3|        3|    290555.07|
        +----------+--------+---------+-------------+

The average price for a 3 bedroom home with 3 bathrooms was fairly stableno matter what year it was built in.  There is no linear regresion from year to year the house was built in.

__What is the average price of a home for each year the home was built, that has three bedrooms, three bathrooms, two floors, and is greater than or equal to 2,000 square feet? Round off your answer to two decimal places.__

        +----------+--------+---------+------+-------------+
        |date_built|bedrooms|bathrooms|floors|AVERAGE_PRICE|
        +----------+--------+---------+------+-------------+
        |      2014|       3|        3|     2|    298264.72|
        |      2016|       3|        3|     2|     293965.1|
        |      2013|       3|        3|     2|    303676.79|
        |      2011|       3|        3|     2|    276553.81|
        |      2010|       3|        3|     2|    285010.22|
        |      2015|       3|        3|     2|    297609.97|
        |      2012|       3|        3|     2|    307539.97|
        |      2017|       3|        3|     2|    280317.58|
        +----------+--------+---------+------+-------------+

The addition of the 2 floors and square footage to the 3 bedroom/3 bathroom data increased the Average price for each year.  Again for this period of time there is no linear increase or decrease observable.

__What is the average price of a home per "view" rating having an average home price greater than or equal to $350,000? Determine the run time for this query, and round off your answer to two decimal places.__

        +----+-------------+
        |view|AVERAGE_PRICE|
        +----+-------------+
        |  99|   1061201.42|
        |  98|   1053739.33|
        |  97|   1129040.15|
        |  96|   1017815.92|
        |  95|    1054325.6|
        |  94|    1033536.2|
        |  93|   1026006.06|
        |  92|    970402.55|
        |  91|   1137372.73|
        |  90|   1062654.16|
        |   9|    401393.34|
        |  89|   1107839.15|
        |  88|   1031719.35|
        |  87|    1072285.2|
        |  86|   1070444.25|
        |  85|   1056336.74|
        |  84|   1117233.13|
        |  83|   1033965.93|
        |  82|    1063498.0|
        |  81|   1053472.79|
        +----+-------------+
        only showing top 20 rows
        --- 0.5719707012176514 seconds ---

This shows the run time for a query computing the average home price based on the view rating using Spark SQL. This is run for comparison purposes against cashing of the data set and Parquet data frames.
        
__Using the cached data, run the last query above, that calculates 
the average price of a home per "view" rating, rounded to two decimal places,
having an average home price greater than or equal to $350,000. 
Determine the runtime and compare it to the uncached runtime.__

        +----+-------------+
        |view|AVERAGE_PRICE|
        +----+-------------+
        |  99|   1061201.42|
        |  98|   1053739.33|
        |  97|   1129040.15|
        |  96|   1017815.92|
        |  95|    1054325.6|
        |  94|    1033536.2|
        |  93|   1026006.06|
        |  92|    970402.55|
        |  91|   1137372.73|
        |  90|   1062654.16|
        |   9|    401393.34|
        |  89|   1107839.15|
        |  88|   1031719.35|
        |  87|    1072285.2|
        |  86|   1070444.25|
        |  85|   1056336.74|
        |  84|   1117233.13|
        |  83|   1033965.93|
        |  82|    1063498.0|
        |  81|   1053472.79|
        +----+-------------+
        only showing top 20 rows
        --- 0.41077494621276855 seconds ---

Caching the data, stores the data in memory. When you query the cached data, it can be retrieved directly from memory, eliminating the need for reading the data from disk.  Thus the faster run time.

__Using the parquet DataFrame, run the last query above, that calculates 
the average price of a home per "view" rating, rounded to two decimal places,
having an average home price greater than or equal to $350,000. 
Determine the runtime and compare it to the cached runtime.__

        +----+-------------+
        |view|AVERAGE_PRICE|
        +----+-------------+
        |  99|   1061201.42|
        |  98|   1053739.33|
        |  97|   1129040.15|
        |  96|   1017815.92|
        |  95|    1054325.6|
        |  94|    1033536.2|
        |  93|   1026006.06|
        |  92|    970402.55|
        |  91|   1137372.73|
        |  90|   1062654.16|
        |   9|    401393.34|
        |  89|   1107839.15|
        |  88|   1031719.35|
        |  87|    1072285.2|
        |  86|   1070444.25|
        |  85|   1056336.74|
        |  84|   1117233.13|
        |  83|   1033965.93|
        |  82|    1063498.0|
        |  81|   1053472.79|
        +----+-------------+
        only showing top 20 rows
        
        --- 0.8901317119598389 seconds ---

Parquet is a columnar storage format specifically designed for efficient data processing. Parquet organises the data into columns, which allows queries to skip irrelevant columns and partitions during query execution.

## Conclusion

Parquet time --- 0.8901317119598389 seconds --- 
Uncashed time --- 0.5719707012176514 seconds ---
Cashed time --- 0.41077494621276855 seconds ---

Parquet formatted data demonstrates the worst runtime among the three options. This data did not benefit by using this optimized storage format.  Cashing the data was the fastest which makes sense since the data is held in memory.

## Project Instructions
1. Rename the Home_Sales_starter_code.ipynb file as Home_Sales.ipynb.
2. Import the necessary PySpark SQL functions for this assignment.
3. Read the home_sales_revised.csv data in the starter code into a Spark DataFrame.
4. Create a temporary table called home_sales.
5. Answer the following questions using SparkSQL:
  * What is the average price for a four-bedroom house sold for each year? Round off your answer to two decimal places.
  * What is the average price of a home for each year the home was built, that has three bedrooms and three bathrooms? Round off your answer to two decimal places.
  * What is the average price of a home for each year the home was built, that has three bedrooms, three bathrooms, two floors, and is greater than or equal to 2,000 square feet? Round off your answer to two decimal places.
  * What is the average price of a home per "view" rating having an average home price greater than or equal to $350,000? Determine the run time for this query, and round off your answer to two decimal places.
6. Cache your temporary table home_sales.
7. Check if your temporary table is cached.
8. Using the cached data, run the last query that calculates the average price of a home per "view" rating having an average home price greater than or equal to $350,000. Determine the runtime and compare it to uncached runtime.
9. Partition by the "date_built" field on the formatted parquet home sales data.
10. Create a temporary table for the parquet data.
11. Run the last query that calculates the average price of a home per "view" rating having an average home price greater than or equal to $350,000. Determine the runtime and compare it to uncached runtime.
12. Uncache the home_sales temporary table.
13. Verify that the home_sales temporary table is uncached using PySpark.
14. Download your Home_Sales.ipynb file and upload it into your "Home_Sales" GitHub repository.


