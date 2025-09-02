# gtc-ml-project1-hotel-bookings
# 🏨 Hotel Booking Cancellations – Data Cleaning & Preprocessing

## 📌 Project Overview
This project analyzes a hotel booking dataset with the goal of **predicting booking cancellations**.  
The workflow follows three main phases:

1. **Exploratory Data Analysis (EDA) & Data Quality Report**  
2. **Data Cleaning**  
3. **Feature Engineering & Preprocessing**  

---

## 🔎 Phase 1: Exploratory Data Analysis (EDA)
- **Summary statistics:** Used `.info()` and `.describe()` to understand data structure and distributions.  
- **Missing values:**  
  - `children` → 4 missing  
  - `country` → 488 missing  
  - `agent` → ~16K missing  
  - `company` → ~112K missing (~94%)  
- **Outliers:**  
  - `adr` contained negative values and extreme outliers (up to 5400).  
  - `lead_time` had values up to 737 days.  
  - `stays_in_week_nights` had unrealistic long stays (50 nights).  
- **Data quality issues:**  
  - Invalid rows with `adults = 0` and no children/babies.  
  - Columns `company` and `agent` had structural missing values.  

---

## 🛠️ Phase 2: Data Cleaning
- **Handled Missing Values:**  
  - `company`, `agent` → replaced missing with **0** (meaning no company/agent).  
  - `country` → imputed with **mode** (most frequent country).  
  - `children` → imputed with **median**.  
- **Removed Duplicates:** Dropped exact duplicates.  
- **Handled Outliers:**  
  - Removed negative ADR values.  
  - Capped `adr` at **1000** to reduce skew.  
  - Capped `lead_time` at **365 days**.  
  - Capped `stays_in_week_nights` and `stays_in_weekend_nights` at **30 nights**.  
  - Dropped rows where `adults=0` and `children=babies=0`.  
- **Fixed Data Types:**  
  - Converted `reservation_status_date` → `datetime`.  
  - Converted `agent`, `company`, `children` → integers.  

---

## 🏗️ Phase 3: Feature Engineering & Preprocessing
- **Created New Features:**  
  - `total_guests = adults + children + babies`  
  - `total_nights = stays_in_weekend_nights + stays_in_week_nights`  
  - `is_family` → binary flag if children or babies > 0  
- **Encoded Categorical Variables:**  
  - **Low-cardinality (meal, market_segment, etc.)** → One-Hot Encoding.  
  - **High-cardinality (`country`)** → Frequency Encoding (rare countries grouped into `"Other"`).  
- **Removed Data Leakage:**  
  - Dropped `reservation_status` and `reservation_status_date`.  
- **Train/Test Split:**  
  - 80/20 split with `random_state=42` and stratification on target `is_canceled`.  

---

## 📂 Project Structure
```bash
├── data/
│   └── hotel_bookings.csv          # raw dataset
├── notebooks/
│   └── hotel_cancellations.ipynb   # main notebook
├── README.md                       # project documentation
```

## 🚀 Next Steps

**Phase 4: Modeling & Evaluation**

- Scale numerical features if needed (for logistic regression, neural nets).

- Train baseline models (Logistic Regression, Random Forest, XGBoost).

- Evaluate with accuracy, precision, recall, F1, and ROC-AUC.
