# 💳 E-Wallet Payment Performance & Transaction Behavior Analysis | Python

**Author:** Van Bat Phuc Tai  
**Tools Used:** Python 

---

## 📑 Table of Contents

- 📌 [Background & Overview](#-background--overview)
- 📂 [Dataset Description & Data Structure](#-dataset-description--data-structure)
- 🧹 [Data Cleaning & Preprocessing](#-data-cleaning--preprocessing)
- 🔍 [Exploratory Data Analysis (EDA)](#-exploratory-data-analysis-eda)
- 🔧 [Data Wrangling & Business Analysis](#-data-wrangling--business-analysis)
- 🔄 [Transaction Classification](#-transaction-classification)
- 📊 [Visualization & Insights](#-visualization--insights)
- 💡 [Business Insights & Recommendations](#-business-insights--recommendations)

---

## 📌 Background & Overview

### 💳 Business Context

Digital wallets have become an essential part of modern financial ecosystems, enabling users to perform financial activities such as: `Payments`, `Money transfers`, `Top-ups`, `Withdrawals`, `Refund transactions`.

Understanding **payment performance and transaction behavior** is crucial for monitoring product performance and improving operational efficiency.

This project analyzes payment reports, product metadata, and transaction data to generate **business insights about payment performance and transaction patterns**.

---

### 🎯 Project Objectives

The main objectives of this analysis are:

- Analyze **payment volume across products**
- Evaluate **team performance**
- Detect **abnormal product assignments**
- Classify **transactions into business-defined transaction types**
- Analyze **transaction volume and participant activity**

---

### 👥 Stakeholders

- Product Team
- Operations Team
- Finance Team
- Data Analytics Team

---

### ❓ Key Business Questions Answered

This project focuses on answering the following business questions:

- Which **products generate the highest payment volume**?
- Are there any **products assigned to multiple teams**, violating the one-product-one-team rule?
- Which **team has the lowest payment performance since Q2 2023**?
- Within that team, which **product category contributes the least** to payment volume?
- Which **source_id contributes the most to refund transactions**?
- What are the **transaction counts, total volume, and number of senders and receivers** for each transaction type?

---

### 📊 Project Outcomes

The analysis provides several key outcomes:

- Identified **top-performing products** by payment volume.
- Evaluated **team performance** based on payment activity.
- Detected potential **product ownership inconsistencies**.
- Analyzed **refund transaction sources**.
- Classified transactions into **business-defined transaction types**.
- Measured **transaction activity metrics**, including counts, volume, senders, and receivers.

---

## 📂 Dataset Description & Data Structure

### 📌 Data Source
- **Source:** Internal company database (e-wallet transaction records)  
- **Format:** `.csv`

### 📊 Dataset Size
- **product.csv:** 493 rows × 3 columns  
- **payment_report.csv:** 920 rows × 5 columns  
- **transactions.csv:** 1,324,002 rows × 9 columns  

### 🔗 Data Structure & Relationships
The dataset consists of three related tables:

- **product.csv** → Contains product information and product categories.  
- **payment_report.csv** → Stores aggregated payment statistics by product category and report date.  
- **transactions.csv** → Contains detailed transaction-level records including payment method, transaction status, and purchase information.

These tables are linked through **product category and transaction attributes**, enabling analysis of **transaction behavior, payment trends, and product performance**.


### 📊 Table Schema & Data Snapshot

#### 📄 Products Table (`product.csv`)

<details>
<summary>Click to expand (493 rows × 3 columns)</summary>

<br>

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| product_id | INT | Unique identifier for each product |
| category | TEXT | Product category |
| team_own | TEXT | Team responsible for the product |

</details>

---

#### 📄 Payment Report (`payment_report.csv`)

<details>
<summary>Click to expand (920 rows × 5 columns)</summary>

<br>

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| report_month | DATE | Month of the payment report |
| payment_group | TEXT | Type of payment (refund, purchase, etc.) |
| product_id | INT | Associated product ID |
| source_id | INT | Source of the transaction |
| volume | FLOAT | Total payment volume |

</details>

---

#### 📄 Transactions (`transactions.csv`)

<details>
<summary>Click to expand (1,324,002 rows × 9 columns)</summary>

<br>

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| transaction_id | INT | Unique transaction identifier |
| merchant_id | INT | Merchant involved in transaction |
| volume | FLOAT | Transaction amount |
| transType | INT | Transaction type code |
| transStatus | TEXT | Transaction status |
| sender_id | INT | Sender ID |
| receiver_id | INT | Receiver ID |
| extra_info | TEXT | Additional transaction details |
| timeStamp | TIMESTAMP | Transaction timestamp |

</details>

---

## 🧹 Data Cleaning & Preprocessing

### Import library

[In 1]:

```python
# Import Libraries
import numpy as np
import matplotlib as plt
import seaborn as sns
import pandas as pd
```

### Load dataset

[In 2]:

```python
#Load data to Colab
from google.colab import drive
drive.mount('/content/drive')

# Import file csv to Colab
import pandas as pd
payment = pd.read_csv('/content/drive/MyDrive/Transaction Payment-Performance_Python/payment_report.csv')
product = pd.read_csv('/content/drive/MyDrive/Transaction Payment-Performance_Python/product.csv')
transactions = pd.read_csv('/content/drive/MyDrive/Transaction Payment-Performance_Python/transactions.csv')
```

### Display the first 5 rows of the each table


<img width="600" alt="image" src="https://github.com/user-attachments/assets/0d56a870-1f1d-4f22-a862-7d7a1ca9acb0" />
[In 3]:

```python
# Display the first 5 rows of the payment_report table
payment_report.head()

# Display the first 5 rows of the product table
product.head()

# Display the first 5 rows of the transactions table
transactions.head()

```

[Out 3]:

<img width="600" alt="image" src="https://github.com/user-attachments/assets/38698abf-6520-4fe8-b057-6d8ebe0cefba" />

<img width="600" alt="image" src="https://github.com/user-attachments/assets/e97ff7b1-b840-4c78-9e3b-e38303ff3eca" />

### Check Dataset Structure

[In 4]:

```python
# Check the structure and data types of the payment_report dataset
payment_report.info()

# Check the structure and data types of the product dataset
product.info()

# Check the structure and data types of the transactions dataset
transactions.info()
```

[Out 4]:

<img width="600" alt="image" src="https://github.com/user-attachments/assets/a3db06e5-bb4a-45aa-9c67-5acb5e44fb29" />

<img width="600" alt="image"  src="https://github.com/user-attachments/assets/3623ee72-c51c-4708-a944-4306c791c1b3" />

<img width="600" alt="image" src="https://github.com/user-attachments/assets/568e3f8b-9b55-4da9-bb35-b5237f6e8459" />


#### 💡 Findings

1. **payment**: 919 rows, 5 columns  
   - `report_month`: Object  
   - `payment_group`: Object  
   - `product_id`: Int64  
   - `source_id`: Int64  
   - `volume`: Int64  

2. **product**: 492 rows, 3 columns  
   - `product_id`: Int64  
   - `category`: Object  
   - `team_own`: Object  

3. **transaction**: 1,324,002 rows, 9 columns  
   - `transaction_id`: Int64  
   - `merchant_id`: Int64  
   - `volume`: Int64  
   - `transType`: Int64  
   - `transStatus`: Int64  
   - `sender_id`: Float64  
   - `receiver_id`: Float64  
   - `extra_info`: Object  
   - `timeStamp`: Int64  

### Check for Missing Values

[In 5]:

```python
#Check for Missing Values in Transactions table
transactions.isna().sum()
```

[Out 5]:

<img width="600" alt="image" src="https://github.com/user-attachments/assets/579057af-419a-485d-961b-03585de6b87e" />

#### 💡 Missing Values Summary

The **transaction** dataset contains missing values in several columns:

- `sender_id` – 49,059 missing values  
- `receiver_id` – 164,795 missing values  
- `extra_info` – 1,317,907 missing values
No missing values were found in the **payment_report** or **product** tables.

### Check for Duplicates
[In 6]:

```python
# Check for Duplicates in payment_report table
payment_report.duplicated().sum()

# Check for Duplicates in product table
product.duplicated().sum()

# Check for Duplicates in transactions table
transactions.duplicated().sum()
```

- **Payment_report table:** No duplicate rows detected.
- **Product table:** No duplicate rows detected.
- **Transaction table:** 28 duplicate rows were identified.

### 💡 Summary

#### Data Type Review

1. **payment**
- Convert `report_month` from **object → datetime** if it represents date/month information.

2. **product**
- Convert `category` and `team_own` from **object → category** to reduce memory usage and improve performance.

3. **transaction**
- Convert `sender_id` and `receiver_id` from **float64 → int64** if no missing values exist.
- Convert `timeStamp` from **int64 → datetime** if it is a Unix timestamp.

#### Data Quality Observations

- **payment_report** and **product** tables contain **no missing values or duplicates**.
- **transaction** contains missing values in `sender_id`, `receiver_id`, and `extra_info`.
- **28 duplicate rows** were found in the **transaction** table and should be removed.
- **Volume outliers** in **payment** and **transaction** are considered **valid**, so no outlier removal is required.

---

## 🔍 Exploratory Data Analysis (EDA)


