# SQL Query Showcase 
History of past PostgreSQL queries completed for code challenges.

## Code Challenge: Monthly Percentage Difference (StrataScratch)
Source: https://platform.stratascratch.com/coding/10319-monthly-percentage-difference?code_type=1   
   
This was one of my favorite challenges to tackle!   
   
My solution query utilizes the:  
&ensp;&ensp;(1) WITH clause to generate temporary tables,  
&ensp;&ensp;(2) SUM() function to aggregate monthly sales values,  
&ensp;&ensp;(3) '::date' casting to convert a string to date format,  
&ensp;&ensp;(4) TO_CHAR() function to re-format dates to align with final requirements,  
&ensp;&ensp;(5) LEAD() function to retrieve data from previous months,  
&ensp;&ensp;(6) GROUP BY statement to group information by month,  
&ensp;&ensp;(7) ORDER BY statement to usefully sort information, and  
&ensp;&ensp;(8) ROUND() function to format the final ouput per challenge requirements.

```
 WITH 
 month_aggregate AS
     (SELECT TO_CHAR(created_at::date,'YYYY-MM') AS month, SUM(value) AS month_sales
     FROM sf_transactions
     GROUP BY month
     ORDER BY month),
 
 month_difference as
    (SELECT month, month_sales, LEAD(month_sales, -1) OVER() as last_month_sales
    FROM month_aggregate) 
    
Select month AS year_month, ROUND(((month_sales-last_month_sales)/last_month_sales)*100,2) AS revenue_diff_pct FROM month_difference
```
