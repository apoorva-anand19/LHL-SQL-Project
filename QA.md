What are your risk areas? Identify and describe them.
QA Process:
Describe your QA process and include the SQL queries used to execute it.

- prodcut price and unit price is not same. Shouldn't they be?
  This will cause a lot of issues with calculating sales and revenue

        SELECT DISTINCT sr.product_name, sr.total_ordered, 
               als.product_name, als.product_quantity, als.item_quality, 
               als.product_price, ana.unit_price
        FROM sales_report AS sr 
        JOIN all_sessions AS als USING (sku_id)
        JOIN analytics AS ana USING (full_visitorid);

- All the missing data shows there is soemthing majorly wrong 
  with either the modeling of the ?site? or the extraaction of
  data by the data engineer.
  There also seems to be no contrains while taking data in.

- full_visitor ID has many duplication(794 of them). Dropping these duplicates will cause 
  cause a lot of loss of data. Unclear on whether this should be done or left as is.
        
        SELECTR DISTINCT full_visitorid, COUNT(full_visitorid)
        FROM all_sessions
        GROUP BY full_visitorid
        HAVING  COUNT(full_visitorid) > 1;

- Similarly, analytics table has 34526 duplciate ful;l_visitorid.
  Unlcear on whether such a large amount of data be dropped without any consequences.
       
        SELECT DISTINCT full_visitorid, COUNT(full_visitorid)
        FROM analytics
        GROUP BY full_visitorid
        HAVING  COUNT(full_visitorid) > 1;

- These multiple duplciations in full_visitorids and also sku_id from all_sessions table, 
  hinders the process of forming foreign key relations between the all_sessions table and analytics table.
         
- ordered_quanitty from products and total_ordered from sales report should be the same but they aren't.
  Another data anomoly?

- Given revenue is analytics table does not match when revenue is calculated as (units_sold * unit_price) 
        
        SELECT DISTINCT full_visitorid, revenue, (units_sold*unit_price) AS revenue_calculated
        FROM analytics
        WHERE units_sold IS NOT NULL AND unit_price IS NOT NULL;    

- In the following query we are trying to determine the commonality between product_quantity, units_sold
  and total_ordered between three tables. Name suggests that these three values should be equal across
  all tables, but the dataset doesn't give us that answer giving into mroe questions about how to even
  calculate the revenue properly without clear and distinct data.

        SELECT DISTINCT full_visitorid, sku_id, product_quantity, units_sold, total_ordered
        FROM all_sessions
        JOIN analytics USING (full_visitorid)
        JOIN sales_report USING (sku_id)
        WHERE full_visitorid IN (
                                  SELECT DISTINCT full_visitorid 
                                  FROM analytics
                                )
                AND sku_id IN (
                                SELECT DISTINCT sku_id 
                                FROM sales_report
                            );

