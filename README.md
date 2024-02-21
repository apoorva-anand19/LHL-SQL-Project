# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals

- Loading and cleaning data and handling errors in the process.

- Understanding the relationships and the schema to answer the questions and quries

- Understanding the real world chalenges with inconsistant data and planning for mitigation strategies

- Creating summarized views through multiple joins and sub-queries


## Process

1. Browsed the CSVs in Excel first and then created tables accordingly and loaded the CSVs to pgAdmin

2. Explored the data some more using queries on pgAdmin to understand some relations between the tables.

3. Cleaned the data by adjusting columns and ensuring data consistancy in the columns where possible.

4. Answered the questions asked with queries and explantions and obstacles face.

5. Came up with observations about the given data set and found some extra patterns.

6. Stated the possible risk factors and anomalies present in the data set with breif explanations.


## Results

- The dataset can give a lot of information based on when and where the eCommerece site was accessed.

- By querying the time stamp, we can find out the bussiest month and the least busy month. 
    This can have varying results based on weather we filter using page visits or products purchsed.

- Querying the channel groupng attribute based on the city and country, we can get an idea of the 
    demographics of the populous that is accessing the eCommerce site.

- Querying time spent on the site based on products purchased gives us an idea of how many people
    just browse and how many actually make a purchase.

- Channel grouping also gives us information on how the visitors arrived at the eCommerece site.
    Did they click on Adds, promotions or did they arrive by searching for the site organically?

- Product stock and ordered quantities gives us the ratio between supply and demand. Running 
    queries shows us the products where sold is much higher than stock, which could lead to
    potential sales and delivery issues.


## Challenges 

- There is high volume of repetitive/duplicate data. Dropping them will cause significant changes
  in the dataset and loss of value. So, did not have a clarity in whether to drop them or not.

- Many values also don't match up. Altering this wil too cause changes in the data set and
  will give very varying results causing confusion in the true value of analysis of the date.

- The large amount of missing values like NULL or 'not set' or NA also will not give a very
  defined or reliable analysis and leaves many questions unanswered.


## Future Goals

- Given more time and most importantly, clarity and direction, would create new tables with what
  we think might be data value and do analysis based on that.
