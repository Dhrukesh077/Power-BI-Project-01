<!-- =================================================================== -->
<!--  DATA LEVERAGER — ADVANCED POWER QUERY ETL PROJECT                  -->
<!--  Built for: Red & White Skill Education                             -->
<!--  Author: (Your Name)                                                -->
<!--  Format: Enterprise-grade README for GitHub (Dark Mode Optimized)   -->
<!-- =================================================================== -->

# 🚀 **Data Leverager – Advanced Power Query ETL Intelligence Suite**

A next-generation Power Query transformation engine engineered to deliver *enterprise-grade data cleaning, integration, profiling,* and *refresh automation*.  

This project consolidates multi-file sales data, enriches it with employee metadata, integrates external datasets via web scraping, and prepares a fully standardized, analytics-ready dataset — all without a single DAX formula.

---

# 🎯 **Core Value Proposition**

This accelerator elevates your analytics environment by:

- 🔄 Streamlining **raw monthly files** into a unified, governed dataset  
- 🧹 Automating **data cleansing, validation, and standardization rules**  
- 🧠 Enabling **semantic enrichment** using merges, lookups, and conditional logic  
- 📈 Delivering **analytics-ready measures** like Profit, Sales Categories, Date Intelligence  
- 🔗 Integrating **external datasets** via live HTML web extraction  
- ⚙️ Ensuring **parameter-driven refresh stability** for scalable deployments  
- 📦 Providing **full M-code transparency** for audit and reproducibility  

---

# 🏗️ **Solution Architecture – Multi-Layer Model**


---

## 🍀 **1. Data Layer**

### 📌 **Data Sources**
- **Folder-based monthly sales files**  
  - Sales_Jan.xlsx  
  - Sales_Feb.xlsx  
  - Sales_Mar.xlsx  
  - Sales_Apr.xlsx (for refresh simulation)  
- **Employee master dataset**  
  - Employee_Data.xlsx  
- **Wikipedia HTML datasets**  
  - GDP by Country  
  - COVID stats  
  - Population data  

### 🧾 **Canonical Schema Extracted**
- OrderID  
- Order Date  
- Customer Name  
- Product  
- Category  
- Region  
- Quantity  
- Cost  
- Revenue  
- Profit (computed)  

### 📦 **Raw Data Characteristics**
- Heterogeneous column headers  
- Mixed casing  
- Numeric fields with currency symbols  
- Date formats varying by locale  
- Duplicates across months  
- Missing master data attributes  

---

## 🔧 **2. Processing Layer**

This is the heart of the project — Power Query transformations.

### 🧼 **2.1 Data Cleaning**
- TRIM whitespace  
- CLEAN invisible characters  
- Standardize casing  
- Promote headers  
- Remove blank rows  
- Remove null key rows  
- Remove duplicates (based on `OrderID`)  
- Track provenance with:
  - `SourceFile`
  - `Date modified`

### 🧮 **2.2 Type Enforcement**
- Change Type with Locale (US/IN/UK)  
- Safe parsing of dates using fallback logic  
- Safe numeric parsing with:
  - `Number.FromText`
  - Currency symbol stripping

### 📊 **2.3 Calculated Columns**
- Profit = Revenue – Cost  
- Date Intelligence:
  - Day  
  - Month  
  - Month Name  
  - Year  
  - Quarter  
  - Fiscal Month (April-based)  
- Sales Category segmentation:
  - High ≥ 10,000  
  - Medium 5,000–9,999  
  - Low < 5,000  
- Index Columns:
  - 0-based index  
  - 1-based index  

### 🔗 **2.4 Data Integration**
- Merge Sales with Employee dataset  
- Expand:
  - EmployeeID  
  - Name  
  - Department  
  - Region  

### 🔁 **2.5 Append & Consolidation**
- Folder ingestion automatically appends future files  
- Refresh-safe logic ensures schema stability

---

## 🧠 **3. Intelligence Layer**

### 📈 **Aggregations (Region Level)**
- Total Sales  
- Average Order Value  
- Transaction Count  
- Profitability indicators  

### 🌐 **External Intelligence**
- Import GDP table from Wikipedia  
- Enrich regional analysis with global metrics  

### 📚 **Metadata Outputs**
- Row counts  
- Distinct value distribution  
- Column profiling  
- Error detection  
- Data quality flags  

---

## 📊 **4. Presentation Layer (Optional)**

Although this project is ETL-only, the transformed dataset supports:

- Executive dashboards  
- KPI cards  
- Interactive slicers  
- Regional profitability heatmaps  
- Time-series trendlines  

---

# ✨ **Key Features (Executive Summary)**

### 🟢 Automated ETL  
- Zero manual intervention  
- New month = automatic ingestion

### 🧩 Modular Architecture  
- Replace or add datasets without redesign

### 🛡️ Strong Data Governance  
- Profiling  
- Quality metrics  
- Schema stability

### 🌍 External Intelligence  
- Live web extraction via HTML tables

### 🔌 Zero Dependencies  
- 100% inside Power Query M  
- No macros  
- No DAX  

---

# 📁 **Repository Structure**


---

# 🛠 **Full Transformation Checklist**

### **Extraction**
✔ Folder.Files ingestion  
✔ Excel.Workbook parser  
✔ Web.Page HTML extraction  

### **Cleaning**
✔ TRIM/CLEAN/REPLACE  
✔ Normalize casing  
✔ Remove blank rows  
✔ Remove duplicate keys  

### **Standardization**
✔ Canonical column mapping  
✔ Add missing columns  
✔ Enforce consistent schema  

### **Transformation**
✔ Numeric correction  
✔ Date parsing  
✔ Fiscal logic  
✔ Derived columns  
✔ Sales segmentation  

### **Integration**
✔ Left Outer merge  
✔ Expand master attributes  

### **Profiling**
✔ Column Quality  
✔ Column Distribution  
✔ Column Profile  

### **Aggregation**
✔ Region-level rollups  
✔ KPI computation  

### **Refresh**
✔ Auto-detect new files  
✔ Reapply all steps  

---



# 📚 **Transformations Explained (Detailed)**

### **1. Header Normalization**
Ensures all files align regardless of header naming differences.

### **2. Numeric Normalization**
Removes:
- commas  
- currency symbols  
- trailing spaces  

Then safely converts to number.

### **3. Date Intelligence**
Extracts all time-based KPIs and adds fiscal logic.

### **4. Conditional Segmentation**
Automatically classifies customers/sales into tiers.

### **5. Merge Enrichment**
Enriches sales rows with:
- Employee name  
- Department  
- Region  

### **6. Grouping**
Provides analytics-level metrics without DAX.

---

# 🧪 **Quality Assurance & Validation**

### ✔ Mandatory Fields Check
- OrderID  
- Order Date  
- Revenue  

### ✔ Referential Integrity
- Region mappings  
- Employee join consistency  

### ✔ Outlier Detection
- Revenue spikes  
- Zero or negative profits  

### ✔ Profile-based Validation
- Distinct counts  
- Missing %  
- Error %  

---

# 📄 **Challenges & Solutions**

### ❌ Challenge: Mixed header names  
✔ Solution: Canonical header mapping via `RenameColumns`

### ❌ Challenge: Currency symbols in numeric fields  
✔ Solution: Strip unwanted characters using `Text.Remove`

### ❌ Challenge: Repeated OrderIDs  
✔ Solution: Deduplicate based on latest file timestamp

### ❌ Challenge: Date parsing issues  
✔ Solution: Locale-aware parsing with fallback logic

---

# 🧑‍💻 **Author**
**(Your Name)**  
Prepared for: **Red & White Skill Education**  
Project: **Data Leverager – Power Query ETL Pipeline**  
Date: **2025**

---

# 🏁 **END OF README**
