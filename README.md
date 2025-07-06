## 📊 KMS SQL Analysis Report
This repository contains a comprehensive SQL-based business analysis for **KMS** using two realistic case scenarios. The goal is to derive actionable insights from the dataset and recommend strategies to increase sales, optimize logistics, and improve customer relationships.

---

## 📁 Dataset Overview

The dataset is based on a business scenario of **KMS**, a retail company operating across various customer segments and regions. It contains information about:

- Customer demographics
- Product categories and sub-categories
- Sales, profit, shipping details
- Order priorities, returns, and regions

---
## 📁 Case Scenario I – Sales & Logistics Performance
1. Which product category had the highest sales?
    - SQL Query:
      
                 SELECT [Product Category],
                 SUM(Sales) AS [Total Sales]
                 FROM dbo.[KMS Sql Case Study]
                 GROUP BY [Product Category]
                 ORDER BY [Total Sales] DESC
    - Insight: This identifies the top-performing product category by total sales. Focus marketing or inventory efforts here.
2. What are the Top 3 and Bottom 3 regions in terms of sales?
    - Top 3:
    
             SELECT TOP 3 [Region], SUM([Sales]) AS [Total Sales]
             FROM dbo.[KMS Sql Case Study]
             GROUP BY [Region]
             ORDER BY [Total Sales] DESC
    - Bottom 3:
    
                SELECT TOP 3 [Region], SUM([Sales]) AS [Total Sales]
                FROM		 dbo.[KMS Sql Case Study]
                GROUP BY     [Region]
                ORDER BY     [Total Sales] ASC
    - Insight: High-performing regions can be used as benchmarks, while the bottom 3 may need attention.
3. What were the total sales of appliances in Ontario?
    - SQL Query:
    
                         SELECT  SUM([Sales]) AS [Ontario Appliance Sales]
                         FROM	dbo.[KMS Sql Case Study]
                         WHERE   [Product Sub-Category] = 'Appliances' AND [Province] = 'Ontario';
    - Insight: This provides a sales summary by province for a specific product sub-category. This information is valuable for regional planning..
4. Advise the management of KMS on what to do to increase the revenue from the bottom 10 customers
    - SQL Query:
    
               SELECT
                TOP 10 [Customer Name],
                SUM(Sales) AS [Total Sales]
               FROM
                dbo.[KMS Sql Case Study]
               GROUP BY
                [Customer Name]
               ORDER BY
                [Total Sales] ASC;
    - Insight: Identify the problem and assess personalized promotions, loyalty programs, and how poor service affects low sales.
5. KMS incurred the most shipping cost using which shipping method?
    - SQL Query:
    
               SELECT 
            		TOP 1 [Ship Mode], 
            		SUM([Shipping Cost]) AS [Total Shipping Cost]
               FROM	dbo.[KMS Sql Case Study]
               GROUP BY [Ship Mode]
               ORDER BY [Total Shipping Cost] DESC;
    - Insight: Once you find the issues, think about using personalized promotions or loyalty programs. Also, check if poor service is causing low sales. 

---

## 📁 Case Scenario II – Customer & Profitability Insights

6. Who are the most valuable customers, and what products or services do they typically purchase?
    - SQL Query:
    
                   WITH [Customers Sales] AS (
                    SELECT
                      TOP 5 [Customer Name],
                      SUM(Sales) AS [Total Sales]
                    FROM
                      dbo.[KMS Sql Case Study]
                    GROUP BY
                      [Customer Name]
                    ORDER BY
                      [Total Sales] DESC
                  )
                    SELECT
                      t.[Customer Name],
                      t.[Total Sales],
                      s.[Product Category],
                      COUNT(*) AS Purchases
                    FROM
                      [Customers Sales] t
                    JOIN
                      dbo.[KMS Sql Case Study] s
                      ON s.[Customer Name] = t.[Customer Name]
                    GROUP BY
                      t.[Customer Name], t.[Total Sales], s.[Product Category]
                    ORDER BY
                      t.[Total Sales] DESC,
                      Purchases DESC;
    - Insight: The top customers drive the highest revenue by consistently purchasing from their favorite product categories, This brings opportunities for target marketing and retention of customers.
7. Which small business customer had the highest sales?
    - SQL Query:
    
                 SELECT
                  TOP 1 [Customer Name],
                  SUM(Sales) AS [Total Sales]
                 FROM
                  dbo.[KMS Sql Case Study]
                 WHERE
                  [Customer Segment] = 'Small Business'
                 GROUP BY
                  [Customer Name]
                 ORDER BY
                  [Total Sales] DESC
    - Insight: Identify our top-performing SMB client and connect with them. This is a great opportunity to gather testimonials and valuable referrals that can elevate the company's reputation and attract new customers. 
8. Which Corporate Customer placed the most number of orders in 2009 – 2012?
    - SQL Query:
    
                 SELECT
                  [Customer Name],
                  COUNT(DISTINCT [Order ID]) AS Orders
                 FROM
                  dbo.[KMS Sql Case Study]
                 WHERE
                  [Customer Segment] = 'Corporate'
                  AND [Order Date] BETWEEN '2009-01-01' AND '2012-12-31'
                 GROUP BY
                  [Customer Name]
                 ORDER BY
                  Orders DESC
    - Insight: Identify your most active corporate customer. These are strong candidates for premium support or retention strategies.
9. Which consumer customer was the most profitable one?
    - SQL Query:
    
                 SELECT
                  TOP 1 [Customer Name],
                  SUM(Profit) AS [Total Profit]
                 FROM
                  dbo.[KMS Sql Case Study]
                 WHERE
                  [Customer Segment] = 'Consumer'
                 GROUP BY
                  [Customer Name]
                 ORDER BY
                  [Total Profit] DESC
    - Insight: Identify your most profitable consumer customer. They could serve as a brand ambassador or be a good fit for loyalty and feedback programs.

10. Which customer returned items, and what segment do they belong to?
    - SQL Query:
    
                 SELECT DISTINCT
                  [Customer Name],
                  [Customer Segment]
                 FROM
                  dbo.[KMS Sql Case Study] 
                 WHERE
                  [Status] = 'Returned';
    - Insight: Identify the customers who returned items and the segment they belong to. Understand if returns are concentrated within certain segments which could indicate dissatisfaction or shipping/product issues.

11. If the delivery truck is the most economical but the slowest shipping method and Express Air is the fastest but the most expensive one, do you think the company
appropriately spent shipping costs based on the Order Priority? Explain your answer.
    - SQL Query:
  
                 SELECT 
                      [Order Priority],
                      [Ship Mode],
                      COUNT(*) AS [Number of Orders],
                      SUM([Shipping Cost]) AS [Total Shipping Cost],
                      AVG([Shipping Cost]) AS Avg_Shipping_Cost
                 FROM 
                      dbo.[KMS Sql Case Study]
                 GROUP BY 
                      [Order Priority], [Ship Mode]
                 ORDER BY 
                      CASE [Order Priority]
                          WHEN 'Critical' THEN 1
                          WHEN 'High' THEN 2
                          WHEN 'Medium' THEN 3
                          WHEN 'Low' THEN 4
                          ELSE 5
                      END,
                      [Ship Mode];
    - Insight: The company did not match its shipping costs with how urgent the orders were. Critical orders, which need fast delivery, were often sent using cheaper methods like Delivery Truck and Regular Air instead of Express Air. This shows a gap between the speed of delivery needed and the importance of these orders, which could lead to unhappy customers for critical shipments.

---

## 📈 Summary of Key Recommendations

- Leverage top-performing categories and regions for growth.
- Use personalized strategies to uplift bottom-tier customers.
- Audit and optimize shipping practices.
- Deepen relationships with high-value customers.
- Monitor return patterns to reduce dissatisfaction.
- Align shipping methods with order priorities to avoid cost inefficiencies.

---

## 📂 Files Included

- [KMS_SQL_ANALYSIS.sql](KMS_SQL_ANALYSIS.sql)
