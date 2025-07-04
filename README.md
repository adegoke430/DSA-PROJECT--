# DSA-PROJECT.
#  Data Analytics Projects – DSA Portfolio

This repository contains two major analytics projects completed during the **Digital Skillup Africa (DSA)** program. The goal of both projects was to apply data analytics skills to real-world business scenarios using tools like **Microsoft Excel** and **SQL Server**.

---

##  Amazon Product Review Analysis

###  Overview

This project involved analyzing a dataset of Amazon product reviews to extract insights on pricing, product categories, customer behavior, and sales potential. The analysis was done entirely in **Microsoft Excel**, including data cleaning, visualizations, and an interactive dashboard.

###  Dataset Summary

- **Records:** 1,465 products  
- **Features:** 16 columns  
  - Product Name  
  - Category  
  - Price (Original & Discounted)  
  - Customer Rating  
  - Number of Reviews  
  - Discount %

---

###  Analysis Objectives

- Average discount % per category  
- Product count per category  
- Total reviews by category  
- Highest rated products  
- Price comparisons (actual vs discounted)  
- Top reviewed products  
- Products with 50%+ discount  
- Rating distribution  
- Revenue potential by category  
- Price range segmentation  
- Ratings vs discount trends  
- Products with <1,000 reviews  
- Highest discounting categories  
- Top 5 products (ratings + reviews)

---

###  Tools Used

- Microsoft Excel  
  - Formulas  
  - Pivot Tables  
  - Bar, Column & Pie Charts  
  - Slicers  
  - Conditional Formatting  
  - Dashboard Design

---

###  Dashboard Features

- KPI Cards  
- Dynamic bar and column charts  
- Pie chart for rating distribution  
- Interactive slicers for category filtering  
- Clean layout for easy decision-making

---

###  Key Insights

- ₹200–₹500 priced products with high ratings are optimal for promotion  
- Best visibility is achieved with **30–60% discounts**  
- Low-rated but heavily discounted products may require quality improvements  
- Under-reviewed products should be targeted for review campaigns  

---

###  Files

- [Amazon_Analysis.xlsx](./Amazon_Analysis.xlsx)-Cleaned data, pivot tables, and dashboard  
- [dashboard_screenshot.png](./dashboard_screenshot.png) – Dashboard preview image

---
## Dashboard
![dashboard_screenshot.png](dashboard_screenshot.png)

---

##  KMS SQL Case Study (2009–2012)

### Business Context

Kultra Mega Stores (KMS) is a Nigerian office equipment retailer. This case study focused on the **Abuja division**, examining operations and sales from **2009 to 2012** using **SQL** to generate insights for management decision-making.

### Tool Used

- SQL Server Management Studio (SSMS)

---

### Business Questions Answered

1. Highest-grossing product category  
2. Top 3 and bottom 3 regions by sales  
3. Appliance sales in Ontario  
4. Revenue performance of bottom 10 customers  
5. Shipping method with the highest logistics cost  
6. Most valuable customers and their purchase trends  
7. Highest-spending small business customer  
8. Corporate customer with the most orders  
9. Most profitable consumer customer  
10. Customers with returned orders  
11. Alignment between shipping cost and order priority


---

## Analysis Task


### 1. Which product category had the highest sales?
```sql
SELECT Product_Category, 
       SUM(sales) AS TotalSales
FROM KMS_Sql
GROUP BY Product_Category
ORDER BY TotalSales DESC;
```

### 2. What are the Top 3 and Bottom 3 regions in terms of sales?
```sql
-- Top 3
SELECT TOP 3 Region, SUM(sales) AS TotalSales
FROM KMS_Sql
GROUP BY Region
ORDER BY TotalSales DESC;

-- Bottom 3
SELECT TOP 3 Region, SUM(sales) AS TotalSales
FROM KMS_Sql
GROUP BY Region
ORDER BY TotalSales ASC;
```

### 3. What were the total sales of appliances in Ontario?
```sql
SELECT SUM(sales) AS TotalAppliancesSales
FROM KMS_Sql
WHERE product_category = 'Appliances' AND region = 'Ontario';
```

### 4. Advice to increase revenue from bottom 10 customers
```sql
SELECT TOP 10 Customer_Name, SUM(sales) AS TotalSales
FROM KMS_Sql
GROUP BY Customer_Name
ORDER BY TotalSales ASC;
```
Advice: 
- Send targeted offers such as discounts, product bundles, or loyalty rewardd to stimulate new purchase
- Use personalized email campaigns to re-engage inactive customers
- Conduct customer feedback surveys to understand their experience and possible reasons for low enagagement
- Promote complementary products based on previous purchases to increase average order value

### 5. KMS incurred the most shipping cost using which shipping method?
```sql
SELECT Ship_Mode, SUM(Sales - Profit) AS EstimatedShippingCost
FROM KMS_Sql
GROUP BY Ship_Mode
ORDER BY EstimatedShippingCost DESC;
```

---

##  Case Scenerio II

### 6. Who are the most valuable customers and what do they typically purchase?
```sql
WITH TopCustomers AS (
    SELECT TOP 10 Customer_Name
    FROM KMS_Sql
    GROUP BY Customer_Name
    ORDER BY SUM(Sales) DESC
)
SELECT K.Customer_name, K.Product_Category, SUM(K.Sales) AS TotalSales
FROM KMS_Sql K
JOIN TopCustomers T
ON K.Customer_Name = T.Customer_Name
GROUP BY K.Customer_Name, K.Product_Category
ORDER BY K.Customer_Name, TotalSales DESC;
```

### 7. Which small business customer had the highest sales?
```sql
SELECT Customer_Name, SUM(Sales) AS TotalSales
FROM KMS_Sql
WHERE Segment = 'Small Business'
GROUP BY Customer_Name
ORDER BY TotalSales DESC;
```

### 8. Which corporate customer placed the most orders?
```sql
SELECT Customer_Name, COUNT(Order_ID) AS Order_Count
FROM KMS_Sql
WHERE segment = 'Corporate' AND Order_Date BETWEEN '2009-01-01' AND '2012-12-31'
GROUP BY Customer_Name
ORDER BY Order_Count DESC;
```

### 9. Which consumer customer was the most profitable?
```sql
SELECT Customer_Name, SUM(Profit) AS TotalProfit
FROM KMS_Sql
WHERE Segment = 'Consumer'
GROUP BY Customer_Name
ORDER BY TotalProfit DESC;
```

### 10. Which customer returned items, and what segment do they belong to?
```sql
SELECT DISTINCT K.Customer_Name, K.Segment
FROM KMS_Sql K
JOIN Order_Status O
ON K.Order_ID = O.Order_ID
WHERE O.Status = 'Returned';
```

### 11. Was shipping cost aligned with order priority?
```sql
SELECT 
    Order_Priority,
    Ship_Mode,
    COUNT(*) AS OrderCount,
    ROUND(SUM(Sales - Profit), 2) AS EstimatedShippingCost,
    AVG(DATEDIFF(day, Order_Date,Ship_Date)) AS AvgShipDays
FROM KMS_Sql
GROUP BY 
    Order_Priority, Ship_Mode
ORDER BY 
    Order_Priority, Ship_Mode;
```
Insight: 
- The data revealed that somehigh-priority orders correctly use fast shipping like Express Air
- Several low-priority orders also used the same expensive shipping method, resulting in unnecessary logistics costs.
- KMS should assure shipping modes align with order urgency eg using delivery truck for non urgent orders to save cost and regulary monitor shipping trends to ensurecompliance and cost efficiency


---


### Recommendations

- Use targeted campaigns to re-engage low-performing customers  
- Align shipping modes with order urgency to reduce unnecessary costs  
- Promote best-performing products by category and region  
- Continuously monitor shipping trends and enforce cost control policies

---

### 📁 Files

- [DSA_SQL_Project_Query.sql](./DSA_SQL_Project_Query.sql)

---
