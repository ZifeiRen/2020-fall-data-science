
# SQL:  Structured Query Language  Exercise

### Getting Started
1. Go to BigQuery UI https://console.cloud.google.com/bigquery
2. Add in the public data sets. 
	3. Click the Add Data icon
	4. Add any dataset
	5. `bigquery-public-data` should become visible and populate in the BigQuery UI. 
3. Add your queries where it says [YOUR QUERY HERE].
4. Make sure you add your query in between the triple tick marks. 
---

For this section of the exercise we will be using the `bigquery-public-data.austin_311.311_service_requests`  table. 

5. Write a query that tells us how many rows are in the table. 
	```
<<<<<<< HEAD
    SELECT 
      COUNT(*) 
    FROM 
      `bigquery-public-data.austin_311.311_service_requests`
=======
	[YOUR QUERY HERE]
>>>>>>> upstream/master
	```

6. Write a query that tells us how many _distinct_ values there are in the complaint_description column.
	``` 
<<<<<<< HEAD
	SELECT count(DISTINCT complaint_description) FROM `bigquery-public-data.austin_311.311_service_requests`;
=======
	[YOUR QUERY HERE]
>>>>>>> upstream/master
	```
  
7. Write a query that counts how many times each owning_department appears in the table and orders them from highest to lowest. 
	``` 
<<<<<<< HEAD
    SELECT 
          owning_department, COUNT(*)
        FROM 
          `bigquery-public-data.austin_311.311_service_requests` 
          GROUP BY owning_department
	        ORDER BY COUNT(*) DESC;
```
	
8.Write a query that lists the top 5 complaint_description that appear most and the amount of times they appear in this table. (hint... limit)


	```
	SELECT 
	   complaint_description, COUNT(*) AS count
	 FROM 
	   `bigquery-public-data.austin_311.311_service_requests`
	   GROUP BY complaint_description
	   ORDER BY COUNT(*) DESC
	   LIMIT 5
	```
9. Write a query that lists and counts all the complaint_description, just for the where the owning_department is 'Animal Services Office'.
   ```
   WITH
   T AS(
   SELECT 
      complaint_description
    FROM 
      `bigquery-public-data.austin_311.311_service_requests`
      WHERE owning_department = 'Animal Services Office')
      
   SELECT complaint_description, COUNT(*) AS count
    FROM T
    GROUP BY complaint_description
   ```

10. Write a query to check if there are any duplicate values in the unique_key column (hint.. There are two was to do this, one is to use a temporary table for the groupby, then filter for values that have more than one count, or, using just one table but including the  `having` function). 
    ```
    SELECT unique_key , COUNT(*) c 
    FROM 
     `bigquery-public-data.austin_311.311_service_requests` 
    GROUP BY 
     unique_key HAVING c > 1;
    ```
=======
	[YOUR QUERY HERE]
	```

9. Write a query that lists the top 5 complaint_description that appear most and the amount of times they appear in this table. (hint... limit)
	```
	[YOUR QUERY HERE]
	  ```
10. Write a query that lists and counts all the complaint_description, just for the where the owning_department is 'Animal Services Office'.
	```
	[YOUR QUERY HERE]
	```

11. Write a query to check if there are any duplicate values in the unique_key column (hint.. There are two was to do this, one is to use a temporary table for the groupby, then filter for values that have more than one count, or, using just one table but including the  `having` function). 
	```
	[YOUR QUERY HERE]
	```
>>>>>>> upstream/master


### For the next question, use the `census_bureau_usa` tables.

1. Write a query that returns each zipcode and their population for 2000 and 2010. 
	```
<<<<<<< HEAD
	WITH 
	T AS (
	SELECT 
	 zipcode, COUNT(population) AS count_2000
	FROM 
	`bigquery-public-data.census_bureau_usa.population_by_zip_2000`
	GROUP BY 
	 zipcode
	),
	
	TT AS (
	SELECT
	 zipcode, COUNT(population) AS count_2010
	FROM 
	 `bigquery-public-data.census_bureau_usa.population_by_zip_2010`
	GROUP BY
	 zipcode
	)
	SELECT 
	 T.zipcode,T.count_2000,TT.count_2010 
	FROM 
	 T 
	JOIN 
	 TT  
	ON 
	 T.zipcode = TT.zipcode
=======
	[YOUR QUERY HERE]
>>>>>>> upstream/master
	```

### For the next section, use the  `bigquery-public-data.google_political_ads.advertiser_weekly_spend` table.
1. Using the `advertiser_weekly_spend` table, write a query that finds the advertiser_name that spent the most in usd. 
	```
<<<<<<< HEAD
	WITH
	 T AS (SELECT advertiser_name, SUM(spend_usd) SUM
	FROM 
	 bigquery-public-data.google_political_ads.advertiser_weekly_spend
	GROUP BY 
	 advertiser_name)
	SELECT 
	 advertiser_name, SUM 
	FROM 
	 T 
	ORDER BY 
	 SUM DESC 
	LIMIT 1
	```
	
2. Who was the 6th highest spender? (No need to insert query here, just type in the answer.)
	```
	TOM STEYER 2020
	(SPEND TOTAL: 3807625300)
```
	
3. What week_start_date had the highest spend? (No need to insert query here, just type in the answer.)
	```
	week_start_date: 2020-09-13
	HIGHEST_SPEND_SUM: 9521262300

	```
	
4. Using the `advertiser_weekly_spend` table, write a query that returns the sum of spend by week (using week_start_date) in usd for the month of August only. 
	```
	WITH
	T AS
	(SELECT
	  sum(spend_usd) sum, 
	  EXTRACT(MONTH FROM week_start_date ) AS month, 
	  week_start_date
	 FROM 
	  bigquery-public-data.google_political_ads.advertiser_weekly_spend
	 group by 
	  week_start_date)
	
	 SELECT 
	  week_start_date , sum(sum) sum
	 from T 
	 WHERE 
	  T.MONTH = 8
	 GROUP BY 
	  week_start_date
	```
	
5.  How many ads did the 'TOM STEYER 2020' campaign run? (No need to insert query here, just type in the answer.)
	```
	50
	```
	
6. Write a query that has, in the US region only, the total spend in usd for each advertiser_name and how many ads they ran. (Hint, you're going to have to join tables for this one). 
	```
	with
	t as(SELECT
	  advertiser_name, count( advertiser_name ) count_ad
	 FROM 
	  bigquery-public-data.google_political_ads.advertiser_weekly_spend
	 group by 
	  advertiser_name),
	 tt as(
	 SELECT
	  advertiser_name, sum( spend_usd ) sum_usd
	 FROM 
	  bigquery-public-data.google_political_ads.advertiser_weekly_spend
	 group by 
	  advertiser_name
	 )
	
	select  t.advertiser_name, t.count_ad, tt.sum_usd
	from  
	 t
	join  
	 tt
	on t.advertiser_name = tt.advertiser_name
	
	```
	
7. For each advertiser_name, find the average spend per ad. 
  ```
   SELECT
    advertiser_name, round(avg( spend_usd),1) avg_usd
   FROM 
    bigquery-public-data.google_political_ads.advertiser_weekly_spend
   group by advertiser_name
   
  ```

  8. Which advertiser_name had the lowest average spend per ad that was at least above 0. 

  ``` 
  select 
  * 
  from
   (SELECT
    advertiser_name, round(avg( spend_usd),1) avg_usd
   FROM 
    bigquery-public-data.google_political_ads.advertiser_weekly_spend
    group by advertiser_name)
   where
    avg_usd >0 
   order by 
    avg_usd asc
   limit 1
   
   
  [
  advertiser_name: GREG CHANEY
  avg_usd: 1.9
  ]
  
  
  ```
=======
	[YOUR QUERY HERE]
	```
2. Who was the 6th highest spender? (No need to insert query here, just type in the answer.)
	```
	[YOUR ANSWER HERE]
	```

3. What week_start_date had the highest spend? (No need to insert query here, just type in the answer.)
	```
	[YOUR ANSWER HERE]
	```

4. Using the `advertiser_weekly_spend` table, write a query that returns the sum of spend by week (using week_start_date) in usd for the month of August only. 
	```
	[YOUR QUERY HERE]
	```
6.  How many ads did the 'TOM STEYER 2020' campaign run? (No need to insert query here, just type in the answer.)
	```
	[YOUR ANSWER HERE]
	```
7. Write a query that has, in the US region only, the total spend in usd for each advertiser_name and how many ads they ran. (Hint, you're going to have to join tables for this one). 
	```
		[YOUR QUERY HERE]
	```
8. For each advertiser_name, find the average spend per ad. 
	```
	[YOUR QUERY HERE]
	```
10. Which advertiser_name had the lowest average spend per ad that was at least above 0. 
	``` 
	[YOUR QUERY HERE]
	```
>>>>>>> upstream/master
## For this next section, use the `new_york_citibike` datasets.

1. Who went on more bike trips, Males or Females?
	```
<<<<<<< HEAD
	SELECT
	  gender,
	  count(gender) count
	 FROM 
	  bigquery-public-data.new_york_citibike.citibike_trips
	group by gender
	
	[
	male (count:35611787) > female(count:11376412)
	]
	```
2. What was the average, shortest, and longest bike trip taken in minutes?
	```
	WITH
	T AS
(SELECT
	   ROUND( tripduration / 60.0, 2) minite
	 FROM 
	  bigquery-public-data.new_york_citibike.citibike_trips)
	  
	SELECT 
	 ROUND(AVG(minite),2) avg_trip_time,
	 min(minite) min_trip_time,
	 max(minite) max_trip_time
	from 
	 T
	```
	
3. Write a query that, for every station_name, has the amount of trips that started there and the amount of trips that ended there. (Hint, use two temporary tables, one that counts the amount of starts, the other that counts the number of ends, and then join the two.) 
	```
	WITH
	T AS
	(SELECT
	    start_station_name ,COUNT( start_station_name ) start_station_count
	 FROM 
	  bigquery-public-data.new_york_citibike.citibike_trips
	GROUP BY
	  start_station_name),
	
	
	TT AS
	(SELECT
	    end_station_name ,COUNT( end_station_name ) end_station_count
	 FROM 
	  bigquery-public-data.new_york_citibike.citibike_trips
	GROUP BY
	  end_station_name)
	 
	 SELECT
	 T.start_station_name, 
	 T.start_station_count,
	 TT.end_station_count
	 FROM
	 T
	 JOIN
	 TT
	 ON
	 T. start_station_name = TT. end_station_name
	 
=======
	[YOUR QUERY HERE]
	```
2. What was the average, shortest, and longest bike trip taken in minutes?
	```
	[YOUR QUERY HERE]
	```

3. Write a query that, for every station_name, has the amount of trips that started there and the amount of trips that ended there. (Hint, use two temporary tables, one that counts the amount of starts, the other that counts the number of ends, and then join the two.) 
	```
	[YOUR QUERY HERE]
>>>>>>> upstream/master
	```
# The next section is the Google Colab section.  
1. Open up this [this Colab notebook](https://colab.research.google.com/drive/1kHdTtuHTPEaMH32GotVum41YVdeyzQ74?usp=sharing).
2. Save a copy of it in your drive. 
3. Rename your saved version with your initials. 
4. Click the 'Share' button on the top right.  
5. Change the permissions so anyone with link can view. 
6. Copy the link and paste it right below this line. 
<<<<<<< HEAD
	* YOUR LINK: https://colab.research.google.com/drive/1fVISdPsE4UWmqnR3YYOd3dgemc9vr3fc?usp=sharing
7. Complete the two questions in the colab notebook file. 

=======
	* YOUR LINK:  ________________________________
9. Complete the two questions in the colab notebook file. 
>>>>>>> upstream/master
