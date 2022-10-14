# PMG-SQL-Assessment

Question #1 Generate a query to get the sum of the clicks of the marketing data

SQL query: SELECT TO_VARCHAR(SUM(clicks),'999,999') AS "TOTAL CLICKS" FROM marketing_data;

Question #2 Generate a query to gather the sum of revenue by store_location from the store_revenue table

SQL query: SELECT store_location, CONCAT('$', TO_VARCHAR(SUM(revenue),'999,999,999.00')) AS "TOTAL REVENUE" FROM store_revenue GROUP BY store_location ORDER BY "TOTAL REVENUE" DESC;

Question #3 Merge these two datasets so we can see impressions, clicks, and revenue together by date and geo. Please ensure all records from each table are accounted for.

SQL query: SELECT a.date, a.geo, TO_VARCHAR(SUM(a.impressions),'999,999') AS "IMPRESSIONS", TO_VARCHAR(SUM(a.clicks),'999,999') AS "CLICKS", CONCAT('$',TO_VARCHAR(SUM(b.revenue),'999,999.00')) AS "REVENUE" FROM marketing_data a FULL JOIN store_revenue b ON a.date = b.date AND a.geo = RIGHT(b.store_location,2) GROUP BY a.date, a.geo ORDER BY a.date, a.geo;

Question #4 In your opinion, what is the most efficient store and why?

Answer: My answer is California (CA) because it has the highest revenue/clicks ratio (about 5.21 efficiency as shown from running the query below). Although other states might have a higher click/impression conversion rate, the most important aspect to consider in this problem is how much you get out (revenue in dollars) of what you put in (impressions).

SQL query: SELECT a.geo, TO_VARCHAR(SUM(a.impressions),'999,999') AS "IMPRESSIONS", TO_VARCHAR(SUM(a.clicks),'999,999') AS "CLICKS", CONCAT('$',TO_VARCHAR(SUM(b.revenue),'999,999.00')) AS "REVENUE", CONCAT('%',TO_VARCHAR(SUM(a.clicks)/SUM(a.impressions)*100,'999.00')) AS "click conversion rate", ROUND(SUM(b.revenue)/SUM(a.impressions),2) AS "efficiency" FROM marketing_data a FULL JOIN store_revenue b ON a.date = b.date AND a.geo = RIGHT(b.store_location,2) GROUP BY a.geo ORDER BY "efficiency" DESC;

Question #5 (Challenge) Generate a query to rank in order the top 10 revenue producing states

SQL query: SELECT * FROM (SELECT store_location, CONCAT('$',TO_VARCHAR(SUM(revenue),'999,999.00')) AS "REVENUE", dense_rank() over (order by SUM(revenue) desc) as rank FROM store_revenue GROUP BY store_location) WHERE rank <= 10;

Note: There are only 3 states (CA, NY and TX) with revenue data between the two datasets. Therefore, it's impossible to rank more than 3 states by revenue.





