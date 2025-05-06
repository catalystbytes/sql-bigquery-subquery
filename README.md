# sql-bigquery-subquery
SQL BigQuery - Subquery


## Unlocking Sales Insights
### BigQuery Subqueries for Store & Customer Analysis
This project demonstrates how to use subqueries in Google BigQuery to analyze sales performance across multiple dimension tables. Specifically, it answers the question: "How many Lenovo products were sold per customer and per store in Metro Manila in the last 3 months?"

This project showcases practical use of subqueries, JOINs, and date filtering in a real-world analytics scenario using BigQuery

https://www.tiktok.com/@catalystbytes

```dax
-- Unlocking Sales Insights: BigQuery Subqueries for Store & Customer Analysis

-- =====================================================
-- Step 1: Subquery to filter data from the sales_fact, 
-- store_dim, product_dim, and customer_dim tables
-- =====================================================

SELECT 
    store_name,                          -- Select store name for display
    customer_name,                       -- Select customer name for display
    SUM(units_sold) AS total_units_sold   -- Sum the units sold for the selected conditions
FROM 
    (
        -- =====================================================
        -- Subquery: Filter sales data for stores in Metro Manila 
        -- and Lenovo products within the last 3 months
        -- =====================================================
        SELECT 
            sf.units_sold,                -- Units sold per transaction
            store.store_name,             -- Store name from the store_dim table
            customer.customer_name        -- Customer name from the customer_dim table
        FROM 
            catalystbytes.computer_store_inc.sales_fact sf
        JOIN 
            catalystbytes.computer_store_inc.store_dim store 
            ON sf.store_id = store.store_id  -- Join sales_fact with store_dim on store_id
        JOIN 
            catalystbytes.computer_store_inc.product_dim product 
            ON sf.product_id = product.product_id  -- Join sales_fact with product_dim on product_id
        JOIN 
            catalystbytes.computer_store_inc.customer_dim customer
            ON sf.customer_id = customer.customer_id  -- Join sales_fact with customer_dim on customer_id
        WHERE 
            store.Region = "Metro Manila"  -- Filter for stores in Metro Manila
            AND product.brand = "Lenovo"  -- Filter for Lenovo products
            AND sf.date_id >= DATE_SUB(CURRENT_DATE(), INTERVAL 3 MONTH)  -- Filter for transactions in the last 3 months
    ) AS filtered_sales  -- Alias for the subquery

-- =====================================================
-- Step 2: Group the filtered results by store name and 
-- customer name, and calculate total units sold
-- =====================================================
GROUP BY 
    store_name,         -- Group by store name to calculate total units sold per store
    customer_name       -- Group by customer name to calculate total units sold per customer in each store

-- =====================================================
-- Step 3: Order the results by store name and customer name 
-- for clarity
-- =====================================================
ORDER BY 
    store_name, 
    customer_name;

```
