Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**

SQL Queries:

        SELECT country, city, SUM(transaction_revenue) AS sum_tran_revenue
        FROM all_sessions
        WHERE transaction_revenue IS NOT NULL --because so many of transaction_reenue values are NULL
        GROUP BY country, city
        ORDER BY SUM(transaction_revenue) DESC;

Answer: 
        - Country is United States and City is NA, because of missing data. 
        - Total trasanction revenue = 2190950000.
        - The final result has only two rows. Since the data for the city is not avbailable,
          the result seems ambiguous. 
        - Also, when a SELECT * query is run where transaction_revenue is not null, it returns 
          only 4 rows, so again, there is a lot of missing data, and I do no think this 
          analysis gives a result of any value.

**Question 2: What is the average number of products ordered from visitors in each city and country?**

SQL Queries:

        SELECT DISTINCT country, city, product_name, AVG(units_sold) AS avg_units_sold
        FROM all_sessions
        JOIN analytics USING (full_visitorid)
        WHERE units_sold IS NOT NULL --since there are so many NULL values in units_sold
        GROUP BY country, city, product_name
		ORDER BY avg_units_sold DESC;

Answer: 
        - The porduct with the highest avg_units_sold is 'Waze Baby on Board Window Decal'
          and the county is United Stated and city is MountainView.
        - I also notice by running some more exploratory quries that these sales happened
          in a Spring Sale event.

**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**

SQL Queries: 

        -- created a temp table with entities that seem to suggest any pattern/behavior 
            
            CREATE TEMP TABLE tb1 AS
	                SELECT als.visit_id, als.country, als.city, 
		                   als.time_onsite, als.visit_date, als.product_name, 
		                   als.product_category, ana.units_sold, ana.times_onsite
	                FROM all_sessions AS als
	                INNER JOIN analytics AS ana USING (full_visitorid);
	
         -- extracted month to see which month has most sales by summing the no.of orderes sold

            SELECT DISTINCT country, EXTRACT(MONTH FROM visit_date) AS visit_month, 
                    SUM(units_sold) AS total_untissold
            FROM tab1
            WHERE units_sold IS NOT null -- many values here are null
            GROUP BY country, EXTRACT(MONTH FROM visit_date)
            ORDER BY visit_month;
 
        -- comapred the time_spent on the site to see if that has any impact on sales  
        
            SELECT DISTINCT country, city , time_onsite, product_category, 
                    SUM (units_sold) AS total_units_sold
            FROM tab1
            WHERE time_onsite IS NOT NULL -- there are a few values here that are null
            GROUP BY country, city, time_onsite, product_category
            ORDER BY time_onsite DESC;

Answer: 
        - From the extract month query, the pattern noticed is that most orders are placed in the months of March (3) and November (11)
        - The time spent onsite query shows us that even though most time was spent on site from the city
            Sunnyvale in United States, they have very low sales voplumes.

**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**

     - By Country

            SELECT *
            FROM (
                    SELECT country, product_name, 
                           RANK () OVER (PARTITION BY country ORDER BY SUM(units_sold) DESC) AS most_products_bought
                    FROM all_sessions 
                    JOIN analytics USING (full_visitorid)
                    WHERE units_sold IS NOT NULL
                    GROUP BY country, product_name
                    )
             WHERE most_products_bought = 1;

     - By City 

            SELECT *
            FROM (
                    SELECT country, product_name, 
                            RANK () OVER (PARTITION BY city ORDER BY SUM(units_sold) DESC) AS most_products_bought
                    FROM all_sessions 
                    JOIN analytics USING (full_visitorid)
                    WHERE units_sold IS NOT NULL
                    GROUP BY city, product_name
                )
             WHERE most_products_bought = 1;
             
Answer: These queries will give the most product bought firsst by country and second by city

**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries: 

            SELECT DISTINCT EXTTRACT (month from visit_date) AS month_visited,
                    country, city, product_name,
                    SUM(product_quantity) AS total_sold,
                    SUM(product_quantity*product_price) AS total_revenue
            FROM all_sessions 
            WHERE product_quantity IS NOT NULL
            GROUP BY country, city, month_visited, product_name
            ORDER BY total_revenue desc;

Answer: - We can infer from this the max products bought by city and it's country and also the month







