# 🛒 Quantium Retail EDA — Chips Category Analysis

An end-to-end **Exploratory Data Analysis (EDA)** project simulating a real-world data analyst job at **Quantium**, a leading data science consultancy. The goal is to analyze customer purchasing behaviour for the chips category and surface actionable insights for the retail client.

\---

## 📌 Project Overview

This project uses two datasets:

* **`QVI\\\_purchase\\\_behaviour.csv`** — Customer loyalty card data including lifestage and premium classification
* **`QVI\\\_transaction\\\_data.csv`** — Transaction-level data including product names, quantities, and sales amounts

The analysis covers data cleaning, feature engineering, time-series exploration, customer segmentation, and brand affinity analysis — all steps that mirror a real Quantium virtual experience task.

\---

## 🔄 Main Steps

### 1\. 📥 Data Loading \& Initial Inspection

Load both datasets and perform an initial inspection using `.head()`, `.info()`, `.shape()`, and `.describe()` to understand the structure, column types, and basic statistics of each table.

### 2\. 🧹 Data Cleaning

* **Duplicates**: Detected and removed one duplicate transaction row (index 124845).
* **Date conversion**: The `DATE` column was stored as a serial number (Excel format). It was converted to a proper datetime using `origin='1899-12-30'`.
* **Outlier removal**: Two records belonging to loyalty card `226000` were removed — this customer purchased 200 units in a single transaction, indicating a reseller rather than a regular consumer.
* **Salsa filter**: Salsa products were removed since the analysis focuses exclusively on chips.

### 3\. 🔗 Merging Datasets

The transaction table and the customer behaviour table were merged on the `LYLTY\\\_CARD\\\_NBR` (loyalty card number) key using a left join, preserving all transaction records.

### 4\. 📅 Time-Series Analysis

Transactions were aggregated by date and plotted over the full fiscal year (July 2018 – June 2019). Key finding: **December 25th had zero sales** (stores closed on Christmas Day), and a clear **spike in purchases** was visible in the lead-up to Christmas.

### 5\. ⚙️ Feature Engineering

New features were derived from existing columns to enrich the analysis:

|Feature|Description|
|-|-|
|`pack\\\_size`|Extracted from product name using regex (e.g. `175g` → `175`)|
|`brand\\\_name`|First word of the product name, with `RRD` corrected to `Red` (Red Rock Deli)|
|`year`, `month`, `day`|Extracted from the `DATE` column|
|`day\\\_of\\\_week`|Day name from `DATE`|
|`month\\\_year`|Period column for monthly aggregation|
|`customer\\\_segment`|Combined `LIFESTAGE` + `PREMIUM\\\_CUSTOMER` label|

### 6\. 👥 Customer Segment Analysis

Sales, customer counts, and average units purchased were analysed across all customer segments. Key findings:

* **Top 3 segments by total sales**: Older Families (Budget), Young Singles/Couples (Mainstream), Retirees (Mainstream)
* **Older and Young Families** buy the **most units per customer**, explaining their high total sales even if their head count is lower
* **Mainstream Mid-age and Young Singles/Couples** are willing to pay a **higher price per packet**

### 7\. 📊 Statistical Testing

A **Welch's t-test** (`ttest\\\_ind` with `equal\\\_var=False`) was performed to check whether the difference in average units purchased per customer between *Mainstream Mid-age Singles/Couples* and *Mainstream Young Singles/Couples* was statistically significant.

### 8\. 🎯 Deep Dive — Mainstream Mid-age Singles/Couples

This high-value segment was analysed in detail:

* **Brand affinity**: Compared segment-level brand purchases against overall market shares. The segment shows a **strong preference for Kettle** chips — a premium brand.
* **Price per unit**: Confirmed Kettle is among the highest-priced brands, consistent with this segment's willingness to spend more.
* **Pack size**: The segment buys slightly larger packs on average (177.9g vs 175.5g across the rest of the population), suggesting a tendency toward value-for-money or premium-sized products.

\---

## 💡 Key Insights

* Budget Older Families are high-volume buyers driven by large household size
* Mainstream Retirees and Young Singles/Couples are large customer groups that drive overall category volume
* Mainstream Mid-age Singles/Couples are a **premium-leaning segment** with strong affinity for Kettle — a prime target for brand partnerships or premium promotions
* Christmas Day is a hard sales blackout; pre-Christmas is the peak sales window for the chips category

\---

## 🛠️ Tech Stack

|Library|Purpose|
|-|-|
|`pandas`|Data manipulation and aggregation|
|`numpy`|Numerical operations|
|`matplotlib`|Data visualization|
|`scipy.stats`|Statistical hypothesis testing|

## 📚 Context

This project is based on the **Quantium Data Analytics Virtual Experience Program** available on Forage. It simulates the type of analysis a graduate data analyst would conduct during the first few weeks on the job.

