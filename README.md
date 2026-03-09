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
- 💡 [Final Conclusions & Recommendations](#-final-conclusions--recommendations)
  
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

<img width="680" alt="image" src="https://github.com/user-attachments/assets/0d56a870-1f1d-4f22-a862-7d7a1ca9acb0" />

<img width="580" alt="image" src="https://github.com/user-attachments/assets/38698abf-6520-4fe8-b057-6d8ebe0cefba" />

<img width="720" alt="image" src="https://github.com/user-attachments/assets/e97ff7b1-b840-4c78-9e3b-e38303ff3eca" />

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

<img width="550" alt="image" src="https://github.com/user-attachments/assets/a3db06e5-bb4a-45aa-9c67-5acb5e44fb29" />

<img width="550" alt="image"  src="https://github.com/user-attachments/assets/3623ee72-c51c-4708-a944-4306c791c1b3" />

<img width="560" alt="image" src="https://github.com/user-attachments/assets/568e3f8b-9b55-4da9-bb35-b5237f6e8459" />


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

<img width="400" alt="image" src="https://github.com/user-attachments/assets/579057af-419a-485d-961b-03585de6b87e" />

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

### Handle Missing Values in Transaction table

### Step 1: Explore Transaction Type Values

Identify the **distinct values** in the `transType` column to understand the **different transaction categories** present in the dataset.

[In 7]:

```python
# Explore Transaction Type Values
transactions['transType'].unique()
```
[Out 7]:

<img width="650" alt="image" src="https://github.com/user-attachments/assets/dc83b844-4e14-47e7-9c0a-69b98f2b0cb6" />

### Step 2: Transaction Type Distribution

Examine the **frequency distribution** of each **`transType`** to understand which **transaction types are most common** in the dataset.

[In 8]:

```python
# Transaction Type Distribution
transactions['transType'].value_counts()
```
[Out 8]:

<img width="750" alt="image" alt="image" src="https://github.com/user-attachments/assets/4dd22c7b-ac20-438f-a5c3-ecfe82215864" />

### Step 3: Analyze Missing Sender and Receiver by Transaction Type

Check how **missing `sender` and `receiver` values** are distributed across **transaction types (`transType`)** to detect potential **data quality issues or patterns**.

[In 9]:

```python
# Analyze Missing Sender and Receiver by Transaction Type
profile = (
    transactions
    .groupby('transType')
    .agg(
        total=('transaction_id', 'count'),
        missing_sender=('sender_id', lambda x: x.isna().sum()),
        missing_receiver=('receiver_id', lambda x: x.isna().sum())))

profile['pct_missing_sender'] = profile['missing_sender'] / profile['total']
profile['pct_missing_receiver'] = profile['missing_receiver'] / profile['total']

profile.sort_values('total', ascending=False)
```
[Out 9]:

<img width="1015" height="390" alt="image" src="https://github.com/user-attachments/assets/4a54552c-82c1-4b53-a3e5-933ad0213588" />

#### Handling Missing Values in Transaction Data

Missing values in `sender_id` and `receiver_id` were handled based on the observed patterns of each `transType`.

For **transType = 22** and **transType = 30**, `sender_id` is frequently missing while `receiver_id` is present. This suggests that these transactions are initiated by the **system** rather than by a specific user. Therefore, missing `sender_id` values will be replaced with a **system identifier**.

For **transType = 2**, the transaction represents a **User → Merchant** payment, where `sender_id` is the user and `receiver_id` is the merchant. Because `receiver_id` is sometimes missing due to logging issues, the missing values were filled using `merchant_id`, which already identifies the merchant in the transaction.

For **transType = 30**, some `receiver_id` values are also missing. Because these transactions often occur close together in time and involve the same receiver, the data was sorted by `timeStamp` and the missing values were filled using nearby transactions (forward fill and backward fill).

#### Step 3.1: Fill Missing `sender` for System Transactions (22, 30) with 0

Transaction Pattern (transType = 22 & 30)

- **transType = 22**
  - `receiver_id` is always present.
  - `sender_id` is often missing.
  - This suggests a **SYSTEM → USER** transaction.

- **transType = 30**
  - `sender_id` is always missing.
  - This indicates the transaction is **initiated by the SYSTEM**.

👉 Therefore, when `sender_id` is missing and `transType ∈ {22, 30}`,  
the missing sender can be inferred as **SYSTEM**.

[In 10]:

```python
# Handle Missing Sender for System Transactions (transType 22, 30) by Fill SYSTEM = 0
SYSTEM_ID = 0

mask = (transactions['transType'].isin([22, 30])) & transactions['sender_id'].isna()
transactions.loc[mask, 'sender_id'] = SYSTEM_ID
```

#### Step 3.2: Fill Missing `receiver` with `merchant_id` for User → Merchant Transactions (transType = 2)

Transaction Pattern (transType = 2)

- `missing_sender` = 0  
- `missing_receiver` ≈ 21%

This represents a **User → Merchant** transaction:

- **Sender:** User (always present)  
- **Receiver:** Merchant (sometimes not logged)
  
[In 11]:

```python
# Handle Missing Receiver for User → Merchant Transactions (transType = 2) by Filling with merchant_id
mask = (transactions['transType'] == 2) & transactions['receiver_id'].isna()
transactions.loc[mask, 'receiver_id'] = transactions.loc[mask, 'merchant_id']
```

#### Step 3.3: Fill Missing `receiver` for System Transactions (transType = 30) using Forward & Backward Fill

For **transType = 30**, some `receiver_id` values are missing. Since these transactions often occur close in time and typically involve the same receiver, the data was sorted by `timeStamp` and missing values were filled using nearby transactions (`ffill` and `bfill`).

[In 12]:

```python
# Handle Missing Receiver for System Transactions (transType = 30) by Filling from Nearby Transactions (ffill & bfill)

# Filter transactions with transType = 30
df = transactions[transactions['transType'] == 30].copy()

# Sort by timestamp to maintain chronological order
df = df.sort_values('timeStamp')

# Fill missing receiver_id using forward fill and backward fill
df['receiver_id'] = df['receiver_id'].ffill().bfill()

# Update the original transactions table
transactions.update(df)
```

#### Handle Duplicates

[In 13]:

```python
# Drop duplicates
transactions.drop_duplicates(subset=None, keep='first')
```
[Out 13]:

<img width="1560" height="545" alt="image" src="https://github.com/user-attachments/assets/5964074e-3f08-40ad-a3df-208b55d29721" />

#### Convert Unix Timestamp to Datetime

Convert the **Unix timestamp** into **datetime format** for easier **time-based analysis**.

[In 14]:

```python
# Convert 'timeStamp' from int64 (Unix timestamp in milliseconds) to datetime format
transactions['timeStamp'] = pd.to_datetime(transactions['timeStamp'], unit='ms')
```

---

## 🔧 Data Wrangling & Business Analysis

### Merge payment_report and product DataFrames

Join both DataFrames to add **product information** to the **payment report dataset**.

[In 15]:

```python
#Merge payment_report with product
payment_product = payment_report.merge(product, on='product_id', how='left')
payment_product.head()
```

[Out 15]:

<img width="933" height="284" alt="image" src="https://github.com/user-attachments/assets/3d045fa4-5bcd-4e2f-8ca6-f70fa27bad20" />

### 1. Find the Top 3 Products with the Highest Transaction Volume

Determine the **top 3 `product_id`s** with the **highest number of transactions** to identify the most frequently used products in the dataset.

[In 16]:

```python
# Calculate volume by product_id
volume_by_product = payment_product.groupby('product_id')['volume'].agg('sum').reset_index()

# Sort volume in descending order
volume_by_product.sort_values(by='volume', ascending=False)

# Filter the top 3 product_ids with the highest volume
top_3_productid = volume_by_product.head(3)

# Print the results
print("Top 3 product_ids with the highest volume:")
print(top_3_productid)
```

[Out 16]:

<img width="590" alt="image" src="https://github.com/user-attachments/assets/ed218dd2-0ed3-453a-9315-19cb0df0d9ee" />

### 2. Verify Whether Each Product Is Managed by Only One Team

Check whether each **`product_id`** is managed by **only one team** to ensure **clear ownership** and avoid **responsibility conflicts**.

[In 17]:

```python
# Group by product_id and count the number of unique teams
products_by_team = payment_product.groupby('product_id')['team_own'].nunique()

# Identify products owned by more than one team
abnormal_products = products_by_team[products_by_team > 1]

if abnormal_products.empty:
    print("No abnormal products found.")
else:
    print("Products owned by more than one team:")
    print(abnormal_products)

    # Retrieve detailed records for those abnormal products
    abnormal_records = payment_product[payment_product['product_id'].isin(abnormal_products.index)]

    print("\nDetailed records:")
    print(abnormal_records)
```

[Out 17]:

<img width="600" alt="image" src="https://github.com/user-attachments/assets/339cd819-c93c-44aa-a825-7c1ff7d1d3bd" />

### 3. Find the Team with the Lowest Performance Since Q2 2023 and Its Lowest-Contributing Category

Identify the **team with the lowest transaction volume** since **Q2 2023**, then determine which **product category contributes the least** to that team's performance.

[In 18]:

```python
# Convert report_month to datetime
payment_product["report_month"] = pd.to_datetime(payment_product["report_month"])

# Filter data from Q2 2023 onwards
since_Q2_2023 = payment_product[payment_product["report_month"] >= "2023-04-01"]

# 1. Find the team with the lowest performance (lowest total volume since Q2 2023)
team_volume = since_Q2_2023.groupby("team_own")["volume"].sum().sort_values()

lowest_team_name = team_volume.index[0]
lowest_volume = team_volume.iloc[0]

print(f"The lowest performance team since Q2 2023 is {lowest_team_name} with volume = {lowest_volume}")

# 2. Find the category contributing the least to that team
lowest_team_data = since_Q2_2023[since_Q2_2023["team_own"] == lowest_team_name]

category_volume = lowest_team_data.groupby("category")["volume"].sum().sort_values()

lowest_category = category_volume.index[0]
lowest_category_volume = category_volume.iloc[0]

print(f"The lowest contributing category for team {lowest_team_name} is {lowest_category} with volume = {lowest_category_volume}")

# 3. Calculate contribution of that category within the team
contribution_pct = lowest_category_volume / lowest_team_data["volume"].sum() * 100

print(f"Contribution of {lowest_category} to team {lowest_team_name}: {contribution_pct:.2f}%")
```

[Out 18]:

<img width="1000" alt="image" src="https://github.com/user-attachments/assets/09d55759-443c-4d05-a430-7ca06809b165" />

### 4. Determine the contribution of each `source_id` in refund transactions (where `payment_group` = 'refund') and identify the `source_id` with the highest contribution.

Analyze **refund transactions** (`payment_group = 'refund'`) to understand the **share of each `source_id` in the total refund volume** and find the **largest contributor**.

[In 19]:

```python
# Filter refund transactions
refund = payment_product[payment_product['payment_group'] == 'refund']

# Calculate total volume by source_id
volume_by_id = (
    refund.groupby('source_id')['volume']
    .sum()
    .reset_index())

# Calculate contribution percentage
volume_by_id['contribution_pct'] = (
    volume_by_id['volume'] / volume_by_id['volume'].sum() * 100)

# Sort by contribution
volume_by_id = volume_by_id.sort_values(by='contribution_pct', ascending=False)

# Get the highest contributor
top_source = volume_by_id.iloc[0]

print(volume_by_id)
print(f"{top_source['source_id']} is the highest contributor to refund transactions with {top_source['contribution_pct']:.2f}%")
```

[Out 19]:

<img width="900" alt="image" src="https://github.com/user-attachments/assets/07dc27a3-4bd6-4fa4-ba20-84b1de48c132" />

### 5. Define `transaction_type` Based on `transType` and `merchant_id`

Classify each transaction into a **`transaction_type`** based on the combination of **`transType`** and **`merchant_id`** according to the following rules:

- `transType = 2` & `merchant_id = 1205` → **Bank Transfer Transaction**
- `transType = 2` & `merchant_id = 2260` → **Withdraw Money Transaction**
- `transType = 2` & `merchant_id = 2270` → **Top Up Money Transaction**
- `transType = 2` & other `merchant_id` → **Payment Transaction**
- `transType = 8` & `merchant_id = 2250` → **Transfer Money Transaction**
- `transType = 8` & other `merchant_id` → **Split Bill Transaction**

All remaining cases are considered **invalid transactions**.

[In 20]:

```python
# Define a function to classify transactions based on transType and merchant_id
def classify_transaction(transType, merchant_id):
    if transType == 2:
        # Specific merchant rules for transType = 2
        if merchant_id == 1205:
            return "Bank Transfer Transaction"
        elif merchant_id == 2260:
            return "Withdraw Money Transaction"
        elif merchant_id == 2270:
            return "Top Up Money Transaction"
        else:
            # Other merchants under transType = 2 are treated as payment transactions
            return "Payment Transaction"
    elif transType == 8:
        # Specific merchant rule for transType = 8
        if merchant_id == 2250:
            return "Transfer Money Transaction"
        else:
            # Other merchants under transType = 8 correspond to split bill transactions
            return "Split Bill Transaction"
    else:
        # Transactions that do not match the defined rules
        return "Invalid Transaction"


# Apply the classification function to create the transaction_type column
# Using itertuples() improves performance compared to row-wise apply()
transactions['transaction_type'] = [
    classify_transaction(t.transType, t.merchant_id)
    for t in transactions.itertuples()
]

# Validate the classification by checking transactions with transType = 2 and merchant_id = 1205
transactions[
    (transactions['transType']==2) & (transactions['merchant_id']==1205)
][['transType','merchant_id','transaction_type']].head()
```

[Out 20]:

<img width="700" alt="image" src="https://github.com/user-attachments/assets/f54302b0-3371-4a24-97d1-6a981bf4c2ca" />

### 6. Analyze Transactions by `transaction_type` (Excluding Invalid Transactions)

Compute the **transaction count**, **total volume**, and **unique senders and receivers** for each **valid `transaction_type`**.

[In 21]:

```python
# Remove invalid transactions
valid_transactions = transactions[transactions['transaction_type'] != "Invalid Trans"]

# Group by transaction type and aggregate the required metrics
transaction_summary = valid_transactions.groupby("transaction_type").agg(
    num_transactions = ("transaction_id", "count"),
    total_volume = ("volume", "sum"),
    num_senders = ("sender_id", "nunique"),
    num_receivers = ("receiver_id", "nunique")
)

# Print the final result
transaction_summary.reset_index()
```

[Out 21]:

<img width="1000" alt="image" src="https://github.com/user-attachments/assets/e660f124-1dc8-45b4-bc37-2be8120bd188" />

---

## 🔎 Final Conclusions & Recommendations

### 🔗 Key Findings

**Top Products by Transaction Volume**
- **Product 15** leads with **4.21B** in volume.
- **Product 12** follows with **1.93B**.
- **Product 3** shows **very low activity** (6K), indicating limited usage.

**Product Ownership**
- Each **product is managed by a single team**.  
- No **ownership conflicts** were identified.

**Lowest-Performing Team (Since Q2 2023)**
- **Team APS** recorded the **lowest transaction volume** (51.14M).
- **Category PXXXXXB** contributed **no volume**, indicating potential inactivity.

**Refund Transactions**
- **Source ID 38** accounts for **59.11% of total refund volume**, making it the **dominant refund source**.

**Transaction Type Overview**
- **Top Up**: 108.6B volume (highest)
- **Payment**: 71.85B volume
- **Bank Transfer**: 50.6B volume
- **Transfer**: 37B volume
- **Withdraw**: 23.42B volume
- **Split Bill**: 4.9M volume (lowest)

---

### 📈 Recommendations

**Product Strategy**
- Continue prioritizing **Product 15**, which drives the majority of transaction volume.
- Investigate the **low adoption of Product 3** to identify growth opportunities.

**Team Performance**
- Review **Team APS performance**, particularly within **Category PXXXXXB**, to identify operational or market issues.

**Refund Monitoring**
- Investigate **Source ID 38**, as it contributes the **largest share of refunds**, which may indicate potential system or transaction issues.

**Transaction Optimization**
- Focus on **Payment** and **Top Up transactions**, as they represent the **largest transaction volumes**.
- Explore strategies to **increase adoption of Split Bill transactions**, which currently have minimal activity.

**User Engagement**
- Improve the **sender and receiver experience** for **high-volume transactions**, particularly **Payment** and **Top Up**, to sustain transaction growth.
