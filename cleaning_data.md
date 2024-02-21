## Table - products
  Observations :
   - Column sku_id has some alphanumeric and some numeric only values. But they all seem to be unique.
   - The numeric only values have products that have 'not' been ordered or stocked.
   - But they do have restocking leadtime, sentiment score and magnitude.
   - Only the sku_id GGADFBSBKS42347 has null values
    No changes made.

## Table - analytics
  Observations :
   - Saw the hint where unit_price is to be divided by 1,000,000. So did that in excel before pushing it to pgAdmin.
   - Column userid is fully null column. Decided to delete the column as it held no value or relations to other tables.
        
            ALTER TABLE analytics 
            DROP COLUMN userid;

  - Change visit_starttime to timestamp. Took help from ChatGPT for the syntax.

            ALTER TABLE analytics 
            ALTER COLUMN visit_starttime TYPE timestamp without time zone 
            USING TO_TIMESTAMP(visit_starttime)::timestamp without time zone;

   - Columns visit_starttime and visit_date both have the same 'date' values. redundant data maybe? can visit_date be dropped?

## Table all_session
   Observation :
   - Columns city has entries like '(not set)' and 'not available in demoset'. Changed these to NA (not available).

            UPDATE all_sessions
            SET city = 'NA' 
            WHERE (city = '(not set)') OR (city = 'not available in demo dataset');
        
  - Column country also has entries as '(not set)'. Cleaned them to show NA

            UPDATE all_sessions
            SET country = 'NA' 
            WHERE country = '(not set)';

   - Similarly column product_variant also has values '(not set)'. Changed them to NA.
            
            UPDATE all_sessions
            SET product_variant = 'NA' 
            WHERE product_variant = '(not set)';

   - Column product_category also has '(not set)', changing that to NA. It also has this ${escCatTitle} as a value. Not sure what to do with this.
   - Seems like a variable name is entered as a column value. Possible bug while loading data.

            UPDATE all_sessions
            SET product_category = 'NA' 
            WHERE product_category = '(not set)';

   - Columns product_ref_amt, search_keyword and item_revenue are fully null and seem to have any part in any table relations.  
   - So decided to drop these two columns

            ALTER TABLE all_sessions 
            DROP COLUMN product_ref_amt,
            DROP COLUMN search_keyword,
            DROP COLUMN item_revenue;

        
   - Columns visit_date and visit_id have the same 'date' value. visit_id also has the timestamp, which makes it unique.
   - But visit_id has duplicate values. Do they need to be removed? Otherwise the column will not be unique.

   - Column product_price also looks like needs to be divided by 1,000,000.

            UPDATE all_sessions 
            SET product_price = (product_price/1000000); 
            
           -- setting the datatype for revenue to have 2 decimal spaces
            ALTER TABLE all_sessions 
            ALTER product_price TYPE NUMERIC(10,2);
        
   - trasanction_revenue has null values for all except 4 entries and they are very high values. Do they also have to be divided by 1,00,000?
   - visit_time and time_onsite are numeric. It would make sense to change it to timestamp. But it would also be easy to use the values as is 
     for any calculation later if necessary. So choosing not to change them.
   - Overall, this table, all_sessions, has too many null values and repetitive values too. But they are not redundant. 
     This makes this table very ambiguous and makes it hard to understand what relations it might have with the rest of the data provided.

## Table sales_by_sku
   Observations :
   - Has distinct sku_id values and their corresponding sales, be it zero.
   - But, honestly, there is no need for this table as the values in this table already exist in the sales_report table.
   - Should the entire table be dropped then..?
    NO changes made.

## Table sales_report
   Observation :
    - Has unique sku_id as the primary key. 
    - Column ratio had some data inconsistency when opened the csv in excel. eg: '0.057692307692307696
    - Was not allowing data conversion to numeric. Realized that 'ratio' is derived by diving total_ordered and stock_level.
    - Made that change in excel and pusehd that csv to pdAdmin.
    - Since ratio is derived from two columns of the same table, is it redundant? Can it not be calculated and viewed on the fly?
    No SQL changeS made.
