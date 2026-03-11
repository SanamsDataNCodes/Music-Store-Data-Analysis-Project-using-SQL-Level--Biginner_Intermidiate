# 🎵 Music Store Data Analysis using SQL

**Beginner → Advance SQL Project | Data Analyst Portfolio**

## 📌 Project Overview

This project demonstrates **SQL-based data analysis on a digital music store dataset**. The goal is to answer real-world business questions using **relational database queries**, helping stakeholders make **data-driven decisions about customers, sales performance, artists, and music genres**.

The dataset contains information about:

* Customers
* Employees
* Invoices & sales transactions
* Music tracks
* Albums & artists
* Genres & playlists
* Media types

Using SQL queries ranging from **basic aggregations to advanced analytical queries**, we extract meaningful business insights from the data.

---

# 🗂 Dataset ER Diagram

The dataset follows a **relational database structure** where multiple tables are connected through primary and foreign keys.

Key relationships include:

* **Customer → Invoice**
* **Invoice → Invoice Line**
* **Track → Album → Artist**
* **Track → Genre**
* **Playlist → Playlist Track**

This schema allows analysis across **sales, customer behavior, and music trends**.

---

# 🎯 Business Objectives

The main goal of this project is to answer business questions such as:

* Who are the **top customers**?
* Which **cities and countries generate the most revenue**?
* Which **artists and genres are most popular**?
* Which **customers spend the most per country**?
* Which **tracks perform above average**?

---

# 🛠 Tools & Technologies

* **SQL**
* Relational Database Concepts
* Data Aggregation
* Window Functions
* Common Table Expressions (CTEs)

Possible database systems used:

* PostgreSQL
* SQLite
* MySQL
* SQL Server

---

# 🔎 Data Analysis Approach

The analysis follows a **structured data analytics workflow**:

### 1️⃣ Understand the Database Schema

Review the relationships between tables such as:

* `customer`
* `invoice`
* `invoice_line`
* `track`
* `album`
* `artist`
* `genre`

Understanding these relationships helps determine **which tables must be joined** to answer each business question.

---

### 2️⃣ Explore the Data

Basic exploratory queries were used to inspect tables.

```sql
SELECT * FROM customer;
SELECT * FROM invoice;
SELECT * FROM track;
```

This helps understand:

* Data structure
* Column names
* Data distribution

---

### 3️⃣ Join Relevant Tables

Many business questions require combining multiple tables.

Example:

```sql
SELECT *
FROM customer
JOIN invoice
ON customer.customer_id = invoice.customer_id;
```

Key joins used:

* **INNER JOIN**
* **Subqueries**
* **Common Table Expressions (CTE)**

---

### 4️⃣ Apply Aggregations

Aggregation functions summarize data.

Examples:

* `COUNT()`
* `SUM()`
* `AVG()`

```sql
SELECT billing_country, COUNT(*) AS total_invoices
FROM invoice
GROUP BY billing_country
ORDER BY total_invoices DESC;
```

---

### 5️⃣ Use Advanced SQL Techniques

More complex insights required:

* **Window Functions**
* **CTEs**
* **Subqueries**

Example:

```sql
ROW_NUMBER() OVER(PARTITION BY country ORDER BY purchases DESC)
```

---

# 📊 Business Questions & SQL Solutions

---

# 🟢 Beginner Level Questions

## 1️⃣ Senior Most Employee

Identify the employee with the highest job level.

```sql
SELECT title, last_name, first_name
FROM employee
ORDER BY levels DESC
LIMIT 1;
```

💡 **Insight:**
Helps identify leadership roles within the organization.

---

## 2️⃣ Countries with Most Invoices

```sql
SELECT COUNT(*) AS invoice_count, billing_country
FROM invoice
GROUP BY billing_country
ORDER BY invoice_count DESC;
```

💡 **Insight:**
Shows which markets generate the most transactions.

---

## 3️⃣ Top 3 Highest Invoice Values

```sql
SELECT total
FROM invoice
ORDER BY total DESC
LIMIT 3;
```

💡 **Insight:**
Identifies high-value purchases.

---

## 4️⃣ City with Highest Revenue

```sql
SELECT billing_city, SUM(total) AS invoice_total
FROM invoice
GROUP BY billing_city
ORDER BY invoice_total DESC
LIMIT 1;
```

💡 **Business Use:**
Ideal location to host **music events or marketing campaigns**.

---

## 5️⃣ Best Customer (Highest Spending)

```sql
SELECT first_name, last_name, SUM(total) AS total_spending
FROM customer
JOIN invoice
ON customer.customer_id = invoice.customer_id
GROUP BY customer.customer_id
ORDER BY total_spending DESC
LIMIT 1;
```

💡 **Insight:**
Identifies **VIP customers** for loyalty programs.

---

# 🟡 Intermediate Level Questions

## 1️⃣ Rock Music Listeners

```sql
SELECT DISTINCT
email, first_name, last_name
FROM customer
JOIN invoice ON customer.customer_id = invoice.customer_id
JOIN invoice_line ON invoice.invoice_id = invoice_line.invoice_id
WHERE track_id IN (
    SELECT track_id
    FROM track
    JOIN genre ON track.genre_id = genre.genre_id
    WHERE genre.name = 'Rock'
)
ORDER BY email;
```

💡 **Insight:**
Helps target **genre-based marketing campaigns**.

---

## 2️⃣ Top 10 Rock Artists

```sql
SELECT artist.name, COUNT(*) AS track_count
FROM track
JOIN genre ON track.genre_id = genre.genre_id
JOIN album ON track.album_id = album.album_id
JOIN artist ON album.artist_id = artist.artist_id
WHERE genre.name = 'Rock'
GROUP BY artist.artist_id
ORDER BY track_count DESC
LIMIT 10;
```

💡 **Insight:**
Identifies **top-performing artists**.

---

## 3️⃣ Tracks Longer Than Average

```sql
SELECT name, milliseconds
FROM track
WHERE milliseconds > (
    SELECT AVG(milliseconds)
    FROM track
)
ORDER BY milliseconds DESC;
```

💡 **Insight:**
Finds **longer tracks compared to the dataset average**.

---

# 🔴 Advanced SQL Analysis

---

# 1️⃣ Customer Spending on Top Artist

Uses **CTE + joins across 6 tables**.

```sql
WITH best_selling_artist AS (
SELECT artist.artist_id, artist.name,
SUM(invoice_line.unit_price*invoice_line.quantity) AS total_sales
FROM invoice_line
JOIN track ON track.track_id = invoice_line.track_id
JOIN album ON album.album_id = track.album_id
JOIN artist ON artist.artist_id = album.artist_id
GROUP BY artist.artist_id
ORDER BY total_sales DESC
LIMIT 1
)
```

💡 **Insight:**
Shows **which customers spend the most on the top artist**.

---

# 2️⃣ Most Popular Genre per Country

Uses **Window Functions**.

```sql
ROW_NUMBER() OVER(
PARTITION BY customer.country
ORDER BY COUNT(invoice_line.quantity) DESC
)
```

💡 **Insight:**
Reveals **music preferences by geography**.

---

# 3️⃣ Top Customer per Country

```sql
ROW_NUMBER() OVER(
PARTITION BY billing_country
ORDER BY SUM(total) DESC
)
```

💡 **Insight:**
Identifies **top-spending customers in each country**.

---

# 📈 Key Analytical Skills Demonstrated

✔ SQL Joins
✔ Aggregations
✔ Subqueries
✔ Window Functions
✔ Common Table Expressions (CTE)
✔ Business Insight Extraction

---

# 💡 Key Business Insights

From the analysis we can discover:

* Top revenue-generating **cities and countries**
* **VIP customers** driving the most revenue
* **Popular genres** by region
* **Top artists** driving music sales
* Customer purchase behavior patterns

These insights can help:

* Improve **marketing strategy**
* Target **high-value customers**
* Promote **popular music genres**
* Support **business expansion decisions**

---

# 📁 Repository Structure

```
Music-SQL-Analysis
│
├── SQL-Music-Beginner2Advance.sql
├── Dataset
├── ERD Diagram
└── README.md
```

---

# 🚀 Future Improvements

Possible next steps:

* Build **Power BI / Tableau dashboards**
* Perform **customer segmentation**
* Create **genre popularity visualizations**
* Develop **predictive models for music trends**

---

# 👨‍💻 Author

**A Sanam Bhaila**

Business Systems Analyst | Data Analyst

Skills:

* SQL
* Data Analysis
* Business Intelligence
* Process Analysis

---

⭐ If you found this project helpful, feel free to **star the repository**.
