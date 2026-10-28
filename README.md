# UPI Transactions Analysis 💳

An end-to-end data analytics project on **250,000 UPI transactions from 2024** to find out **when people pay, who pays (age, state, bank, device), what they pay for, how often payments fail, and how much fraud there is**, using Power Query, DAX, and Power BI.

## 📌 Project Overview

UPI (Unified Payments Interface) is India's most used digital payment system. A payments team that looks at raw transaction files cannot easily see peak hours, failure rates, fraud, or which banks and states lead. This project cleans the raw transaction file, builds a data model with DAX measures, and presents the results in a 5-page interactive Power BI dashboard.

**Result at a glance**

| Metric | Value |
|---|---|
| Total Transactions | 250,000 |
| Total Transaction Amount | ₹327.94M (about ₹32.79 crore) |
| Average Transaction | ₹1,312 (smallest ₹10, largest ₹42,099) |
| Success Rate / Failed Rate | 95.05% / 4.95% |
| Fraud Transactions | 480 (0.19%, goal is 0.00%) |
| Peak Time | 4 PM to 8 PM (73.6K transactions, 29.5% of all) |
| Busiest Hour | 7 PM (21.2K transactions) |
| Top State | Maharashtra (14.97% of transactions) |
| Top Bank | SBI (25.08% of transactions) |
| Top Device | Android (75.11%) |

## 🛠️ Tech Stack

- **Data Cleaning:** Power BI Power Query
- **Data Modelling & Calculations:** DAX (measures and calculated columns)
- **Data Visualization:** Power BI (5-page report, slicers, navigation buttons, KPI cards)
- **Analytical Concepts:** Time Trend Analysis, Segment Analysis (age, state, bank, device), Fraud and Failure Monitoring, Merchant and Transaction Type Analysis

## ❓ Business Problems Solved

- **When is demand highest?** Found the busiest hours, days, and months, so servers and support can be planned in advance.
- **Who uses UPI?** Compared age groups, states, banks, and devices by transaction volume.
- **What do people pay for?** Compared transaction types (P2P, P2M, Bill Payment, Recharge) and merchant categories.
- **How reliable are payments?** Measured the failed rate and checked whether failures rise at peak time.
- **How big is the fraud problem?** Tracked the fraud rate against a goal of 0.00% and checked where fraud is concentrated.

**Who uses the dashboard:** product and operations teams (peak hours, failures), risk and fraud teams (fraud rate), banking partners (bank and state share), and marketing teams (age group and merchant mix).

## 🗂️ Dataset Summary

Source file: `UPI_Transactions_2024.csv` with 250,000 rows and 17 columns, covering 1 Jan 2024 to 30 Dec 2024.

| Table | What it contains |
|---|---|
| `Upi_Transactions_2024` | Main table with 250,000 transactions, 17 original columns and 11 calculated columns |
| `StateWise_Details` | Total amount, transaction count, and average amount for each state |
| `Top Categories` | Total amount, transaction count, and average amount for each merchant category |

**Original columns:** transaction id, timestamp, transaction type, merchant_category, amount (INR), transaction_status, sender_age_group, receiver_age_group, sender_state, sender_bank, receiver_bank, device_type, network_type, fraud_flag, hour_of_day, day_of_week, is_weekend

**How the amount is defined:** amount means the transaction value in INR (`amount (INR)`).

## 🚀 Project Workflow

### Phase 1: Data Cleaning (Power Query)

- Loaded the CSV file and promoted the first row to headers.
- Set the right data types (text, date and time, whole number).
- Trimmed extra spaces in `fraud_flag`.
- Removed rows with errors in `fraud_flag`, then changed it back to a whole number.
- Final result: **250,000 clean transactions** loaded into the model.

### Phase 2: Data Modelling & DAX

Added calculated columns to turn raw fields into groups that are easy to analyse:

| Calculated column | What it does |
|---|---|
| `Time_Slot` | Morning (4 AM to 11 AM), Afternoon (12 PM to 3 PM), Evening (4 PM to 9 PM), Night |
| `Peak_Hour_Flag` | "Peak Hour" for 5 PM to 10 PM, otherwise "Non-Peak Hour" |
| `Amount_Bucket` | Low (up to ₹500), Average (up to ₹2,000), High (above ₹2,000) |
| `Fraud_Label` | "Fraud" or "Not Fraud" from `fraud_flag` |
| `SameBank_Trnx` | "Same Bank" or "Different Bank" (sender bank compared with receiver bank) |
| `Year`, `Month_Number`, `Month_Name`, `Quarter`, `WeekDay_Name` | Date parts from `timestamp` |

Wrote DAX measures for the KPIs:

| Measure | Meaning |
|---|---|
| `Total Trnx` | Count of transaction ids |
| `Total Amount` | Sum of amount (INR) |
| `Average Trnx`, `Min Amt`, `Max Amt` | Average, smallest, and largest amount |
| `Fraud Count`, `Not Fraud Count` | Transactions by fraud label |
| `Fraud Percentage` | `DIVIDE([Fraud Count], [Total Trnx])` |
| `Success Trnx`, `Failed Trnx` | Transactions by status |
| `Success %`, `Failed Rate` | Share of successful and failed transactions |
| `Weekend Transactions`, `WeekendTrans Percentage` | Count and share of weekend transactions |
| `Targeted Fraud Rate` | The fraud goal (0.00%) shown next to the actual rate |

### Phase 3: Dashboard (Power BI)

An interactive report with 5 pages, navigation buttons on every page, and slicers for age group, bank, transaction status, fraud status, day of week, transaction type, and device type.

| Page | What it shows |
|---|---|
| Home | Total amount, total transactions, key review points, and buttons to every page |
| Transaction Overview | Fraud rate against the goal, and transaction volume by age group, bank, hour of day, and day of week |
| Time & Trends | Transactions by day, by month, weekend vs weekday, and a treemap of hours |
| Merchant & Type | Success vs failed share, share by merchant category, volume by merchant, volume by transaction type |
| Banks & States | Volume by state, top banks, receiver banks, share by state, and device split |

## 📸 Dashboard Preview

1. **Home** ![Home](images/dashboard_1_home.png)

2. **Transaction Overview** ![Transaction Overview](images/dashboard_2_transaction_overview.png)

3. **Time & Trends** ![Time and Trends](images/dashboard_3_time_and_trends.png)

4. **Merchant & Type** ![Merchant and Type](images/dashboard_4_merchant_and_type.png)

5. **Banks & States** ![Banks and States](images/dashboard_5_banks_and_states.png)

## 📁 Large Files (not uploaded to GitHub due to size)

| File | What it is | Link |
|---|---|---|
| `UPI_Transactions_2024.csv` | Transaction data (250,000 rows) | [Download](https://1drv.ms/x/c/f2e0a4c9963077ba/IQCMa3PswjpcR48fn-sm5d3jAQ8XXsBTTsMNlyPEKO6WZ5Y?e=26nfUC) |

## 💡 Key Insights

- **Demand peaks from 4 PM to 8 PM.** These four hours carry 73.6K transactions, about 29.5% of the day's total. 7 PM is the single busiest hour (21.2K). Early morning is the quietest time.
- **Days and months are almost equal.** Monday is slightly highest (36.5K) and Saturday lowest (35.3K). Weekends are 28.5% of transactions, which is what 2 days out of 7 would give, so there is no real weekend effect. Monthly volume stays near 20K to 21K, and February is lowest because it is shorter.
- **P2P and P2M make up 80% of transactions** (112K and 88K). Bill Payment is about 15% and Recharge about 5%.
- **Grocery has the most transactions (19.99%)**, followed by Food (14.99%) and Shopping (11.95%). By value, **Shopping leads with ₹76.9M**, then Grocery and Utilities.
- **Maharashtra, Uttar Pradesh, and Karnataka are the top states** (14.97%, 12.05%, and 11.9%). Together they carry about 39% of transactions.
- **SBI is the most used bank** for both sending and receiving. SBI, HDFC, and ICICI together carry about 52% of sent transactions.
- **Android dominates with 75.11%**, followed by iOS (19.85%) and Web (5.04%). 4G is the most used network (about 60%).
- **The 26-35 age group sends the most transactions** (about 35%). Ages 18 to 35 together send about 60%.
- **About 1 in 20 transactions fails** (4.95%). The failed rate stays between 4.5% and 5.4% in every hour, so failures do not rise at peak time.
- **The fraud rate is 0.19% (480 transactions), above the goal of 0.00%.** P2P (43%) and P2M (35%) hold most fraud cases, mainly because they have the most transactions. The fraud rate per type is similar (0.18% to 0.24%). Recharge has the highest rate (0.24%).
- **Fraud transactions are slightly larger.** The average fraud amount is ₹1,499, compared with ₹1,311 for other transactions.

## ✅ Business Recommendations

➢ **Prepare for the Evening Peak** → Make sure servers and support teams are ready between 4 PM and 8 PM, when 3 out of 10 transactions happen.

➢ **Look Into Failed Transactions** → Failures stay near 5% all day, so they are probably not caused by traffic. Check bank, network, and device causes first.

➢ **Strengthen Fraud Checks on P2P and P2M** → Add real-time alerts for these two types and for higher-value transactions, since they hold most fraud cases.

➢ **Work Closely With Top Banks** → SBI, HDFC, and ICICI carry over half of the volume, so fixing issues with them gives the biggest improvement.

➢ **Build for Android First** → Three out of four transactions come from Android, so test new features there first.

➢ **Target Young Adults** → Ages 18 to 35 send about 60% of transactions. Offers and rewards for Grocery, Food, and Shopping are likely to reach them best.

➢ **Compare Counts and Rates** → When comparing groups, show fraud counts next to fraud rates, because bigger groups (like P2P or SBI) will always have more cases.

## ⚠️ Limitations

- The data covers **one year (2024)** and **250,000 transactions**, so the results describe this dataset, not all UPI activity in India.
- The data has no customer ID, so unique users cannot be counted.
- The fraud flag comes with the data, so how a transaction was labelled as fraud is not known.
- The reason for a failed transaction is not available, so causes can only be investigated, not proven.
- The fraud goal of 0.00% is a target, not an industry benchmark.
- Amounts are in INR only and include no fees or costs.

## 📂 Repository Contents

| File | Description |
|---|---|
| `UPI_Transactions_Dashboard.pbix` | Power BI report (5 pages) |
| `Full_Report_UPI_Transactions.pdf` | Dashboard export (all 5 pages) |
| `images/` | Dashboard screenshots used in this README |

The transaction data file is linked in the **Large Files** section above.
