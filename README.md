# 🍽️ Restaurant & Food Delivery Data Pipeline & Modeling - Power BI

An end-to-end Power BI project demonstrating multi-source data ingestion (Single File & Folder connectors), data cleaning in Power Query, and star schema relational data modeling.

---

## 📌 Project Overview

This repository demonstrates the foundational ETL (Extract, Transform, Load) and modeling workflow in Microsoft Power BI:
1. **Multi-Source Ingestion:** Importing dimensional sheets directly and appending folder-based transaction batches.
2. **Power Query Transformations:** Cleansing data types, standardizing dirty categorical values, and using exact-match replacement.
3. **Relational Data Modeling:** Constructing 1-to-many (`1:*`) relationships across customer, food, restaurant, and order tables.

---

## 🏗️ Data Architecture & Sources

| Table Name | Source Type | Nature | Key Attributes |
| :--- | :--- | :--- | :--- |
| **`Customer Details`** | Single Excel File | Dimension | `customer_id` (PK), `customer_name`, `member_Type` |
| **`Food_Details`** | Single Excel File | Dimension | `ItemCode` (PK), `Item_Name`, `Category`, `Price` |
| **`Resturant_Details`** | Single Excel File | Dimension | `Restaurant_ID` (PK), `Restaurant_Name`, `City`, `Cuisine` |
| **`Orders_Fact` / `Order Data`** | Folder Connector | Fact | `order_id`, `customer_id` (FK), `Restaurant_ID` (FK), `ItemCode` (FK), `Quantity` |

---

## 📸 Step-by-Step Implementation & Screenshots

### Step 1: Connecting to Data Sources
Connected to local storage using both **Excel Workbook** and **Folder** connectors to load dimensions and transactional order data.

![Connect to Data Source](screenshots/01_connect_data_source.png)
---

### Step 2: Selecting Required Sheets
Selected the target entity tables (`Customer Details`, `Food_Details`, and `Resturant_Details`) from the workbook preview.

![Choose Data](screenshots/02_choose_data.png.png)

---

### Step 3: Navigating to Power Query for Transformations
Rather than loading raw dirty data directly, opted for **Transform Data** to clean and inspect each column in Power Query Editor.

![Transform Data Option](screenshots/03_transform_data.png)

---

### Step 4: Data Profiling & Column Quality Inspection
Inspected data quality distributions (Valid 100%, Error 0%, Empty 0%) and identified inconsistent categorical entries (`G` vs `Gold`, `R` vs `Regular`) in `member_Type`.

![Data Profiling & Initial Table](screenshots/04_data_profiling.png)

---

### Step 5: Applying Replace Values
Used Power Query's **Replace Values** transformation to standardize the categorical abbreviations into descriptive labels.

![Replace Values Dialog](screenshots/05_replace_values.png)

---

### Step 6: Handling Exact Match Logic (Edge-Case Prevention)
Enabled **"Match entire cell contents"** during replacement. 
> *Note:* Without selecting this option, replacing `'R'` with `'Regular'` incorrectly converts existing `'Regular'` values into `'Regularegular'`.

![Replace Values Settings](screenshots/06_replace_dialog_setting.png)

---

### Step 7: Cleaned Dataset Output
Both `G` and `R` codes were successfully standardized into clean `Gold` and `Regular` values across all 30 rows.

![Cleaned Customer Table](screenshots/07_cleaned_customer_data.png)

---

### Step 8: Close & Apply to Data Model
Applied all query transformation steps and loaded the clean tabular data into the Power BI engine.

![Close & Apply](screenshots/08_close_and_apply.png)

# Folder Connector
---

## 🛠️ Step-by-Step Implementation & Workflow

### Step 1: Connecting Data Sources to Power BI Desktop
Initial stage me base dimension tables (`Customer Details`, `Food_Details`, aur `Resturant_Details`) load ki gayi hain.

![Power BI Desktop Canvas](screenshots/Screenshot%202026-09-09%20151705.png)

---

### Step 2: Data Cleaning & Transformation in Power Query
`Customer Details` table ke andar member types ko standardise kiya gaya:
* `member_Type` column me typo/format errors ko handle karne ke liye text replacement logic apply kiya gaya (`Table.ReplaceValue`).
* Column profiling enable karke 100% data quality (valid values, 0% error, 0% empty) verify ki gayi.

![Power Query Text Replacement](screenshots/Screenshot%202026-09-09%20152214.png)

---

### Step 3: Verifying Loaded Tables in Data View
Transformations complete hone ke baad queries ko apply karke Power BI model me Import mode ke under verify kiya gaya.

![Imported Tables in Data Pane](screenshots/Screenshot%202026-09-09%20152416.png)

---

### Step 4: Ingesting Dynamic Monthly Data via Folder Connector
Single-file import ke bajaye pure folder ko connect kiya gaya taki upcoming months ka data bina manual re-import ke auto-append ho sake:
1. Power BI Desktop me **Get Data > More...** select kiya.
2. **File > Folder** option choose karke destination source path select kiya.

| Get Data Selection | Folder Connector Selection |
| :---: | :---: |
| ![Get Data Menu](screenshots/Screenshot%202026-09-09%20154039.png) | ![Folder Source Selection](screenshots/Screenshot%202026-09-09%20154142.png) |

---

### Step 5: Folder Path Configuration & Preview
Folder connector ke through `Order Data` path browse kiya gaya jisme monthly workbooks store hain:
* `January_24.xlsx`
* `February_24.xlsx`
* `March_24.xlsx`
* `April_24.xlsx`

Data load karne ke bajaye **Transform Data** par click karke Power Query Editor open kiya gaya.

| Folder Path Selection | Files Preview |
| :---: | :---: |
| ![Folder Path Config](screenshots/Screenshot%202026-09-09%20154310.png) | ![Preview Files In Folder](screenshots/Screenshot%202026-09-09%20154331.png) |

---

### Step 6: Power Query Binary Extraction & Transformation
1. **Remove Other Columns:** File metadata (`Name`, `Date modified`, `Extension`) ko drop karke sirf core `Content` column retain kiya gaya (`Table.SelectColumns(Source, {"Content"})`).
2. **Custom Column Processing:** Binary content stream se Excel workbooks ko programmatically expand karne ke liye Custom Column add kiya gaya (`Excel.Workbook([Content])`).

| Select Content Column | Custom Column Addition |
| :---: | :---: |
| ![Remove Other Columns](screenshots/Screenshot%202026-09-09%20154402.png) | ![Custom Column Power Query](screenshots/Screenshot%202026-09-09%20154609.png) |

---

## 🗂️ Repository Structure

```text
├── data/
│   ├── Order Data/
│   │   ├── January_24.xlsx
│   │   ├── February_24.xlsx
│   │   ├── March_24.xlsx
│   │   └── April_24.xlsx
│   ├── Customer_Details.xlsx
│   ├── Food_Details.xlsx
│   └── Resturant_Details.xlsx
├── screenshots/
│   ├── Screenshot 2026-09-09 151705.png
│   ├── Screenshot 2026-09-09 152214.png
│   ├── Screenshot 2026-09-09 152416.png
│   ├── Screenshot 2026-09-09 154039.png
│   ├── Screenshot 2026-09-09 154142.png
│   ├── Screenshot 2026-09-09 154310.png
│   ├── Screenshot 2026-09-09 154331.png
│   ├── Screenshot 2026-09-09 154402.png
│   └── Screenshot 2026-09-09 154609.png
├── reports/
│   └── Restaurant_Analytics.pbix
└── README.md
---

## 🔗 Data Model & Relationships (Star Schema)

After loading, relationships were established between dimension tables and the central fact table:

```text
       [ Customer Details ] (1)
               │
               │ 1:* (customer_id)
               ▼
   ┌───────────────────────┐
   │    Orders (Fact)      │ ◄─── 1:* (Restaurant_ID) ─── [ Resturant_Details ] (1)
   └───────────────────────┘
               ▲
               │ 1:* (ItemCode)
               │
        [ Food_Details ] (1)
