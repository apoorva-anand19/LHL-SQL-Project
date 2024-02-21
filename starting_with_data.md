Question 1: What are some insigts we can get from the time spent on site by visitors?

SQL Queries: 

    -- most time is spent on which category 
        SELECT DISTINCT product_category, SUM(time_onsite) AS total_timeonsite
        FROM tab1
        WHERE time_onsite IS NOT NULL 
        GROUP BY product_category
        ORDER BY total_timeonsite DESC

Answer: Department or category they spent most time on is Home/Electronics

    -- most time spend onsite by country 
        SELECT DISTINCT country, SUM(time_onsite) AS total_timeonsite
        FROM tab1
        WHERE time_onsite IS NOT NULL 
        GROUP BY country
        ORDER BY total_timeonsite DESC

Answer: Visitors from country United States spent the most time on this compared to other countries

Question 2: What information can we get uing Channel Grouping?

SQL Queries:

    -- this tells us how a visitor arrived to the site.
        SELECT channel_grouping, SUM(time_onsite) AS total_timeonsite
        FROM all_sessions
        GROUP BY channel_grouping
        ORDER BY total_timeonsite DESC

Answer: Organic Search is the way most people arrived on the site.
        Not sure if page_views or time_onsite should be used to figure this.
        the highest is Organic in both, but the second highest changes depening on
        attribute used to SUM
       
        SELECT channel_grouping, SUM(page_views) AS total_pageviews
        FROM all_sessions
        GROUP BY channel_grouping
        ORDER BY total_pageviews DESC
        
Question 3: Which visitors have come back multiple times and how much have they shopped?

SQL Queries:

    -- to find which visitors are more regular shoppers
        SELECT DISTINCT full_visitorid, SUM(page_views) AS total_page_views, SUM(product_quantity) AS total_bought
        FROM all_sessions
        GROUP BY full_visitorid
        ORDER BY total_page_views DESC

Answer: The most visitor who was on the site has no data for how much they 
         actually purchased. This is very poor data collection as this information
         can help so much in figuring out demographics, most profitable season,
         pattern of buying and other informations
