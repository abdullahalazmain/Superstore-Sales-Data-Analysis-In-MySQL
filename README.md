# Superstore Sales Data Analysis | EDA | RFM Segmentation

![MySQL](https://img.shields.io/badge/MySQL-8.0-blue)
![Google Sheets](https://img.shields.io/badge/Google_Sheets-Data_Cleaning-green)
![CSV-to-SQL](https://img.shields.io/badge/Rebasedata-CSV_to_SQL_Tool-orange)

This repository contains an end-to-end data analysis project using **Superstore Sales Data**. The project focuses on setting up a **MySQL** database, cleaning and transforming raw sales data, performing exploratory analysis **(EDA)**, and implementing **RFM (Recency, Frequency, Monetary) segmentation** to categorize customers based on their purchasing behavior.

**❓ Why This Project?**  
This project demonstrates skills in **database management**, **data wrangling**, and **customer behavior analysis**. It provides a template for handling real-world sales data and actionable insights through RFM segmentation. 

---

## 📋 Overview

The goal of this project is to:
- Clean and format raw sales data.
- Convert the cleaned CSV to SQL.
- Create a database and table under that database.
- Import the SQL file into MySQL.
- Clean Formatting and Handling of Imported Data.
- Perform exploratory Data analysis.(**EDA**)
- Segment customers using **RFM (Recency, Frequency, Monetary)** analysis.

---

## 🚀 Features

- **Data Cleaning**: Handled blank cells, renamed columns, and fixed date formats in Google Sheets or Microsoft Excel.
- **CSV-to-SQL Conversion**: Automated SQL schema creation and bulk insertion using CSV to SQL Converter Tools.
- **RFM Segmentation**: SQL queries to classify customers into actionable groups (e.g., "Loyal Customers", "At-Risk").
- **Exploratory Analysis**: Analyzed sales trends, regional performance, and product profitability.

---

## 🛠️ Setup & Queries

### Prerequisites
- MySQL Workbench
- Google Sheets (or Excel) for data cleaning
- [Rebasedata](https://www.rebasedata.com/) (free CSV-to-SQL converter)

### Setup

#### 1. **Data Cleaning in Google Sheets**:
   - Open [superstore_raw.xlsx](https://github.com/abdullahalazmain/Superstore-Sales-Data-Analysis/blob/42f4d5ca168bb9a02a098f2a6e8271a85738d6e5/superstore_raw.xlsx) in Google Sheets. 
   - Perform cleaning:
     - Fill blank cells (e.g., using `CTRL + Click` or `Go To > Special > Blanks`).
     - Update column names for consistency (e.g., `Order ID` → `order_id`).
     - Fix date formats (e.g., `MM/DD/YYYY` → MySQL-friendly `YYYY-MM-DD`).
   - Export cleaned data as [superstore_clean.csv](https://github.com/abdullahalazmain/Superstore-Sales-Data-Analysis/blob/e2050a62d85c0c426128e1dac51a5cabe805bea6/superstore_clean.csv) . 

#### 2. **Convert CSV to SQL**:
   - Upload [superstore_clean.csv](https://github.com/abdullahalazmain/Superstore-Sales-Data-Analysis/blob/e2050a62d85c0c426128e1dac51a5cabe805bea6/superstore_clean.csv) to **[Rebasedata](https://www.rebasedata.com/)**.
   - Download the generated `.sql` file (includes table schema and data).
#### 3. **Create Database in MySQL**:
   - Open your server in `MySQL Workbench`.
   - If the `superstore` database doesn’t exist, create it first:  
  ```sql
  CREATE DATABASE superstore;
  ```
     
   ![image](https://github.com/abdullahalazmain/Superstore-Sales-Data-Analysis/blob/36eb3ff56f5fb32ec1bb385ad04580e474260c25/Images/Create_Database%20Result%20Screenshot.jpg)
     
#### 4. **Import SQL File into MySQL**:
   - **Steps**:  
      1. **Connect to Server**:  
            Open MySQL Workbench and connect to your MySQL server.  

      2. **Open Data Import**:  
            Go to `Server` > `Data Import` in the top menu.
         ![image](https://github.com/abdullahalazmain/Superstore-Sales-Data-Analysis/blob/4094bd9da417b742fea5bfcb4f5220fc9c0b101f/Images/Open_data_import%20Screenshot.jpg)

      4. **Select Import Options**:  
          - Choose `Import from Self-Contained File`.  
         - Browse and select your `superstore_clean.sql` file.  
         - Under `Default Target Schema`, select the `superstore` database.  

      5. **Start Import**:  
            Click `Start Import` at the bottom-right.  
   
   ![image](https://github.com/abdullahalazmain/Superstore-Sales-Data-Analysis/blob/071f320cdc15936f8f5260c10000b0a0a56cea8a/Images/My%20SQL%20workbench%20data%20import%20Screenshot.jpg)

      **Why Use This?**  
      - **Beginner-Friendly**: No command-line commands required.  
      - **Progress Tracking**: Visual progress bar for large files.  
      - **Error Logging**: Detailed error messages if the import fails.  

### **Queries**

#### Data Cleaning , Handling & Standardized
   - **Steps**:  
       1. **Rename the Table**
          
      ```sql
         RENAME TABLE Superstore_clean TO Sales_data;
      ```
        
      ![image](https://github.com/abdullahalazmain/Superstore-Sales-Data-Analysis/blob/50f63cf9aa2c2e51ab0a8736d938d4c7e3c60c0d/Images/Rename_table%20Screenshot.jpg)
   
       2. **Get the Row Count of the Table**
          
      ```sql
         SELECT COUNT(*) AS row_count FROM Sales_data;
      ```
        
      ![image](https://github.com/abdullahalazmain/Superstore-Sales-Data-Analysis/blob/f3716fc88c22d0ef661f2278bb76e0c71608749e/Images/Raw_count%20Result%20Screenshot.jpg)

      3. **Describe the structure**
          
      ```sql
         DESCRIBE sales_data;
      ```
        
      ![image](https://github.com/abdullahalazmain/Superstore-Sales-Data-Analysis/blob/ac411994429204e61416f4a470453464cf5a9d6c/Images/Describe_the_structure_Screenshot.jpg)
     
      4. **Modify Date column types**
        ```sql
         ALTER TABLE sales_data
         MODIFY order_date DATE,
         MODIFY ship_date DATE;
        ```

       ![image](https://github.com/abdullahalazmain/Superstore-Sales-Data-Analysis/blob/c94439a82acb7b5e91f6c714609c320387d3a7e7/Images/Modify_Date_types%20Screenshot.jpg)

     5. **Fix Date Columns & Handle numeric fields**
         - When you update Table , You need to disable safe update mode.
        ```sql
         -- Disable safe update mode
         SET SQL_SAFE_UPDATES = 0;
         
         -- Fix Date Columns
           UPDATE sales_data
           SET Order_Date = STR_TO_DATE(Order_Date, '%Y-%m-%d'),
               Ship_Date = STR_TO_DATE(Ship_Date, '%Y-%m-%d');
         
         -- Handle numeric fields
         UPDATE sales_data 
         SET Product_Base_Margin = NULL 
         WHERE Product_Base_Margin = 'null' 
            OR Product_Base_Margin = '';
            
         -- Re-enable safe update mode
         SET SQL_SAFE_UPDATES = 1;
        ```

       ![image](https://github.com/abdullahalazmain/Superstore-Sales-Data-Analysis/blob/c94439a82acb7b5e91f6c714609c320387d3a7e7/Images/Fix%20date%20and%20Numeric%20fields%20Screenshot.jpg)


#### EDA
   - **Steps**:
        1. **Total Sales & Profit**
        ```sql
         SELECT 
             ROUND(SUM(Sales)) AS Total_Sales,
             ROUND(SUM(Profit)) AS Total_Profit
         FROM sales_data;
        ```

       ![image](https://github.com/abdullahalazmain/Superstore-Sales-Data-Analysis/blob/56bb62986cc7c611c1f2de5d9a02ce639e63cd6e/Images/Total_sales_%26_Profit%20Screenshot.jpg)

        2. **Sales by City**
        ```sql
         SELECT 
             City,
             ROUND(SUM(Sales)) AS City_Sales
         FROM sales_data
         GROUP BY City
         ORDER BY City_Sales DESC;
        ```

       ![image](https://github.com/abdullahalazmain/Superstore-Sales-Data-Analysis/blob/55f00d8d0ae792c13e87299099583d0c109a2a70/Images/Sales%20by%20City%20Screenshot.jpg)

        3. **Top 10 Products**
        ```sql
         SELECT 
             Product_Name,
             ROUND(SUM(Sales)) AS Total_Sales
         FROM sales_data
         GROUP BY Product_Name
         ORDER BY Total_Sales DESC
         LIMIT 10;
        ```

       ![image](https://github.com/abdullahalazmain/Superstore-Sales-Data-Analysis/blob/68007f451620f6fd785ec8ff0df18b17baccc206/Images/Top10%20Products%20Result%20Screenshot.jpg)

        4. **Top 10 Most Profitable Products**
        ```sql
         SELECT 
             Product_Name,
             ROUND(SUM(Profit)) AS Total_Profit
         FROM sales_data
         GROUP BY Product_Name
         ORDER BY Total_Profit DESC
         LIMIT 10; 
        ```

       ![image](https://github.com/abdullahalazmain/Superstore-Sales-Data-Analysis-In-MySQL/blob/e778c9362d50aaa7b454466d5fa2d4000892e517/Images/Top%2010%20most%20profitable%20products%20Result.jpg)

        5. **Monthly Sales Trends**
        ```sql
         SELECT 
             DATE_FORMAT(Order_Date, '%Y-%m') AS Month,
             ROUND(SUM(Sales)) AS Monthly_Sales
         FROM sales_data
         GROUP BY Month
         ORDER BY Month;
        ```

       ![image](https://github.com/abdullahalazmain/Superstore-Sales-Data-Analysis-In-MySQL/blob/e778c9362d50aaa7b454466d5fa2d4000892e517/Images/Monthly%20Sales%20Trends%20Results%20Screenshot.jpg)

        6. **Return Rate Analysis**
        ```sql
         SELECT 
             Return_Status,
             COUNT(*) AS Return_Count,
             ROUND(COUNT(*) * 100.0 / (SELECT COUNT(*) FROM sales_data), 2) AS Return_Percentage
         FROM sales_data
         GROUP BY Return_Status;
        ```
       ![image](https://github.com/abdullahalazmain/Superstore-Sales-Data-Analysis-In-MySQL/blob/ceafa7bac5bd7ee4a25eada64b5a819f4c29d735/Images/Return%20Rate%20Analysis%20Result%20Screenshot.jpg)

#### **RFM Segmentation**
   - **Steps**:
        1. **Create/Replace View for RFM Raw Metrics**
        ```sql
         CREATE OR REPLACE VIEW RFM_RAW_DATA AS
         SELECT
             Customer_ID,
             DATEDIFF('2014-01-01', MAX(Order_Date)) AS Recency,
             COUNT(DISTINCT Order_ID) AS Frequency,
             SUM(Sales) AS Monetary
         FROM sales_data
         GROUP BY Customer_ID;
        ```

       ![image](https://github.com/abdullahalazmain/Superstore-Sales-Data-Analysis-In-MySQL/blob/983065f39094db1c5c1b60a4dfa5ab4a5c7422ce/Images/Create%20or%20Replace%20View%20for%20RFM%20Raw%20Metrics.jpg)

        2. **RFM Segmentation Using the View**
        ```sql
         CREATE OR REPLACE VIEW RFM_SCORES AS
         WITH rfm_scores AS (
             SELECT
                 Customer_ID,
                 Recency,
                 Frequency,
                 Monetary,
                 NTILE(3) OVER (ORDER BY Recency DESC) AS R_Score,
                 NTILE(3) OVER (ORDER BY Frequency) AS F_Score,
                 NTILE(3) OVER (ORDER BY Monetary) AS M_Score
             FROM RFM_RAW_DATA
         )
         SELECT
             Customer_ID,
             Recency,
             Frequency,
             Monetary,
             R_Score,
             F_Score,
             M_Score,
             CONCAT(R_Score, F_Score, M_Score) AS RFM_Cell,
             CASE
                 WHEN CONCAT(R_Score, F_Score, M_Score) IN ('333','323','332') THEN 'Champions'
                 WHEN CONCAT(R_Score, F_Score, M_Score) LIKE '2__' THEN 'Potential Loyalists'
                 WHEN CONCAT(R_Score, F_Score, M_Score) LIKE '1__' THEN 'At-Risk Customers'
                 ELSE 'Needs Attention'
             END AS RFM_Segment
         FROM rfm_scores;
        ```

       ![image](https://github.com/abdullahalazmain/Superstore-Sales-Data-Analysis-In-MySQL/blob/6e4a0dac5c6e677b0047d3e4c03734786f0eb292/Images/RFM%20Segmentation%20Using%20the%20View.jpg)
     
---

## 🔄 Alternative Way of Setup & Queries
1. **Database Setup**: Created a MySQL database and table schema tailored for the Superstore dataset.
           ```sql
         -- Create database
         CREATE DATABASE IF NOT EXISTS Superstore;
         USE Superstore;
         
         -- Create table with corrected schema
         DROP TABLE IF EXISTS sales_data;
         CREATE TABLE sales_data (
             Row_ID INT,
             Order_Priority VARCHAR(13),
             Discount DECIMAL(3,2),
             Unit_Price DECIMAL(6,2),
             Shipping_Cost DECIMAL(5,2),
             Customer_ID INT,
             Customer_Name VARCHAR(28),
             Ship_Mode VARCHAR(14),
             Customer_Segment VARCHAR(14),
             Product_Category VARCHAR(15),
             Product_Sub_Category VARCHAR(30),
             Product_Container VARCHAR(10),
             Product_Name VARCHAR(98),
             Product_Base_Margin DECIMAL(4,2), -- Changed from VARCHAR
             Region VARCHAR(7),
             Manager VARCHAR(7),
             State_or_Province VARCHAR(20),
             City VARCHAR(19),
             Postal_Code INT,
             Order_Date DATE, -- Changed from VARCHAR
             Ship_Date DATE, -- Changed from VARCHAR
             Profit DECIMAL(7,2),
             Quantity_ordered_new INT,
             Sales DECIMAL(8,2),
             Order_ID INT,
             Return_Status VARCHAR(12)
         );
        ```
3. **Bulk Data Insertion**: Efficiently imported the dataset into MySQL using `Table Data Import Wizard` or `LOAD DATA INFILE`.
4. **Data Cleaning & Validation**:  
   - Standardized date formats.  
   - Handled missing/duplicate values.  
   - Ensured consistent data types (e.g., sales, profit as `DECIMAL`, dates as `DATE`).  
   - Updated table schema for optimization (e.g., indexing, constraints).  
5. **Exploratory Data Analysis (EDA)**:  
   - Analyzed sales trends, regional performance, and product category insights.  
   - Identified top customers and profitable products.  
6. **RFM Customer Segmentation**:  
   - Calculated **Recency**, **Frequency**, and **Monetary Value** metrics.  
   - Segmented customers into groups (e.g., "High-Value", "At-Risk", "Champions") for targeted strategies.

---

## ❓ Why Google Sheets & Rebasedata?

I chose **Google Sheets** and **Rebasedata** for data cleaning and SQL conversion due to challenges faced with direct CSV imports in MySQL:

### Issues with Direct MySQL CSV Import:
1. **Formatting Errors**:  
   MySQL often failed to parse dates, strings with special characters (e.g., `'` in names), or numeric fields with inconsistencies (e.g., `$1,000` as text).
2. **Data Loss**:  
   Columns with mixed data types (e.g., postal codes stored as numbers) caused incomplete imports or truncated values.
3. **Time-Consuming Fixes**:  
   Manually editing schemas, handling missing values, and debugging import errors took hours.
4. **Bulk Insertion Failures**:  
   Large datasets (e.g., 10,000+ rows) frequently caused timeouts or incomplete imports.

### How Google Sheets & Rebasedata Solved These:
- **Google Sheets**:  
  - ✅ **Visual Data Cleaning**: Easily fill blank cells, rename columns, and fix date formats without coding.  
  - ✅ **Prevent Data Loss**: Ensure consistency (e.g., postal codes as text, sales as numbers) before conversion.  
  - ✅ **User-Friendly**: No SQL expertise needed for initial cleanup.  

- **Rebasedata**:  
  - ✅ **Automated Schema Generation**: Converts CSV columns to correct MySQL data types (e.g., `DATE`, `DECIMAL`).  
  - ✅ **Bulk Insertion Made Easy**: Generates optimized `.sql` files for seamless imports, avoiding timeouts.  
  - ✅ **Error-Free**: Handles special characters, quotes, and formatting issues automatically.  

### The Result:  
A streamlined workflow that saved time, reduced errors, and ensured **100% data integrity** during migration.  

--- 

## 🛠️ Tools Used
- **Data Cleaning**: Google Sheets
- **CSV-to-SQL Conversion**: [Rebasedata](https://www.rebasedata.com/)
- **Database**: MySQL
- **Analysis**: Pure SQL queries

---

## 📄 License

MIT License. See [LICENSE](LICENSE) for details.

---

## 🙏 Credits

- Tool: [MySQL](https://www.mysql.com/) for Data analysis.
- Tool: [Google Sheets](https://workspace.google.com/products/sheets/) for data cleaning.
- Tool: [Rebasedata](https://www.rebasedata.com/) for CSV-to-SQL conversion.

