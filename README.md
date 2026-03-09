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
