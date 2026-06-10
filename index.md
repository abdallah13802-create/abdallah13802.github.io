# Hi, I'm Abdallah
### Accelerated Data Science Student (ASU) & Media Buyer

I am an accelerated Data Science student at Arizona State University with a practical background in media buying and digital advertising. Instead of looking at data purely in spreadsheets, I am interested in how spatial data and GIS tools can be used to understand target audiences and regional demographics.

My goal is to build clean, functional data pipelines — taking messy, fragmented public datasets (like the US Census) and transforming them into clear visual maps that can guide real-world business and marketing decisions.

- **Skills I'm Developing:** Relational Data Joining, Data Cleaning & Text Manipulation, Basic SQL Query Filtering, Demographic Audience Segmentation
- **Tools I Use:** QGIS, SQL, Python, GitHub

---

## Project 1: Orange County Income Distribution Map
### Geospatial Demographics Analysis

**Technologies Used:** QGIS, SQL, US Census Bureau APIs, TIGER/Line Shapefiles, String Regex Engineering

---

### 🎯 Objective
Engineer a highly localized demographic heat map of Orange County, California, isolating median household income distributions by Zip Code Tabulation Areas (ZCTAs). This asset provides geographic intelligence for target market penetration and regional demographic analysis.

---

### 🔄 Step-by-Step Process

#### Step 1 — Loading Raw Geographic Boundaries
Loaded the US Census TIGER/Line shapefile containing all California zip code boundaries into QGIS, then filtered to Orange County using a SQL expression targeting 84 zip codes.

<img width="840" height="638" alt="step1_raw_boundaries" src="https://github.com/user-attachments/assets/a096c4a8-54a5-4a05-8d29-063c8014c21e" />

---

#### Step 2 — Joining Income Data
Census ACS income data was joined to the shapefile using a custom engineered ZIPCODE key extracted from the NAME field using `right("Geography", 5)`. The attribute table below shows income values successfully attached to each zip code geometry.

<img width="1512" height="961" alt="step2_attribute_table" src="https://github.com/user-attachments/assets/6f7d9a24-0077-4360-81aa-d473c824fa63" />

---

#### Step 3 — Graduated Color Classification
Applied Equal Count (Quantile) classification across 5 income tiers using a white-to-red color ramp. Used `to_real()` typecasting to convert string income values to numeric for classification.

<img width="856" height="804" alt="step3_symbology" src="https://github.com/user-attachments/assets/528491bd-4543-47dd-afc7-1fff6725c9c8" />

---

### 🗺️ Final Output

<img width="2402" height="1274" alt="OC-wealth- distrubution  " src="https://github.com/user-attachments/assets/2152526a-c047-4563-ae52-1c9eecea1296" />

---

### 🛑 Core Engineering Challenges

Government databases are notoriously fragmented. This project encountered several data architecture barriers:

1. **Missing Schema Keys:** The Census ACS table lacked a normalized 5-digit zip code column to link with TIGER geometry layers
2. **Type-Mismatch Incompatibilities:** Spreadsheet parsers read numeric IDs as integers, stripping leading zeros
3. **Bloated String Descriptions:** Geographic anchor field was locked in format `"ZCTA5 92657"` instead of `"92657"`
4. **Geographic Noise:** Dataset covered all of California, creating heavy rendering overhead

---

### 💡 Technical Solutions

**Phase 1 — Column Injection**
Injected a custom zip code column using:
` right("Geography", 5) `
Stripped text prefixes (e.g. "ZCTA5 92657" → "92657") and enforced string datatype for leading-zero integrity.

Stripped text prefixes and enforced string datatype for leading-zero integrity.

**Phase 2 — Relational Spatial Join**
Mapped the engineered zip code key to the shapefile's native `ZCTA5CE20` field, suppressing default prefix naming for clean output.

**Phase 3 — SQL Scope Filtering**
Isolated Orange County using:
Reduced geometry processing overhead by over 90%.

**Phase 4 — Cartographic Optimization**
- Equal Count Quantile classification across 5 income tiers
- `to_real()` typecasting for numeric conversion
- Null/suppressed zones handled with 0% opacity transparency

---

### 📊 Key Findings

- **Highest income zones:** Irvine, Newport Beach, Mission Viejo ($127k–$240k)
- **Lower income zones:** Santa Ana, western Anaheim corridors ($17k–$63k)
- **Clear pattern:** Income increases moving southeast from the Santa Ana urban core toward the Irvine/Mission Viejo corridor
- **84 zip codes** analyzed across Orange County

---

### 💼 Business Application

This map directly supports Meta advertising strategy by identifying premium zip codes for high-value service business targeting. Zip codes in the $100k+ income tier represent the highest-value audiences for local service businesses in Orange County.

---

### 📁 Data Sources

- US Census Bureau TIGER/Line Shapefiles 2023
- American Community Survey (ACS) 5-Year Estimates 2023, Table S1901
