# Hi, I'm Abdallah
---

## 🗂️ Projects

| # | Project | Description | Status |
|---|---|---|---|
| 1 | [🗺️ OC Income Heat Map](#project-1-orange-county-income-distribution-map) | Median household income by zip code | ✅ Complete |
| 2 | [📍 OC Competitor Map](#📍 PROJECT 2: OC Competitor Location Mapping) | Nail salon density vs income zones | ✅ Complete |
---
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
Injected a custom zip code column using `right("Geography", 5)` to strip text prefixes (e.g. "ZCTA5 92657" → "92657") and enforce string datatype for leading-zero integrity.

**Phase 2 — Relational Spatial Join**
Mapped the engineered zip code key to the shapefile's native `ZCTA5CE20` field, suppressing default prefix naming for clean output.

**Phase 3 — SQL Scope Filtering**
Isolated Orange County using:
"ZCTA5CE20" >= '92600' AND "ZCTA5CE20" <= '92899'

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







---

<br>

# ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
# 📍 PROJECT 2: OC Competitor Location Mapping
# ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

<br>

### Nail Salon Density vs Income Zones — Ad Targeting Intelligence

**Technologies Used:** QGIS, OpenStreetMap, Overpass Turbo API, 
Buffer Analysis, GeoJSON

---

### 🎯 Objective
Identify high-income zip codes in Orange County with low nail 
salon competitor density — pinpointing the highest-value 
geographic zones for local service business advertising on Meta.

> *"Where is there money but no competition?"*

---

### 📂 Data Sources
- **Competitor locations:** OpenStreetMap via Overpass Turbo API
- **Income zones:** Project 1 (US Census ACS 2023)
- **Zip code boundaries:** US Census TIGER/Line 2023

---

### 🔄 Step-by-Step Process

#### Step 1 — Extracting Business Location Data
Queried OpenStreetMap via Overpass Turbo API to pull all 
beauty salons and nail spas within Orange County. 
Exported as GeoJSON and loaded into QGIS as a point layer.

<img width="1512" height="799" alt="overpass-query-screenshot" src="https://github.com/user-attachments/assets/6aeafd4a-574f-4836-910d-34511e1899e1" />

---

#### Step 2 — Overlaying Competitors on Income Map
Loaded competitor points on top of the Project 1 income 
heat map. Each gray dot represents one nail salon or 
beauty business location.

<img width="938" height="641" alt="oc-market-proximity-analysis" src="https://github.com/user-attachments/assets/7e8255c3-1c61-42ef-aed5-30f095432fd9" />

---

#### Step 3 — Buffer Analysis
Applied a 2-mile radius buffer around each business location 
to visualize service coverage zones. Overlapping circles 
reveal areas of high competitor saturation.

**Saturated Zone — Northwest OC (Anaheim/Santa Ana)**
High competition, lower income — avoid for premium targeting.

<img width="814" height="642" alt="Screenshot 2026-06-11 at 13 45 32" src="https://github.com/user-attachments/assets/35fcde25-ee9d-4300-976a-40dcdf7c5e10" />

---

**Opportunity Zone — Southeast OC (Mission Viejo/Laguna Hills)**
High income, minimal competitor presence — prime advertising zone.

<img width="1015" height="637" alt="Screenshot 2026-06-11 at 13 45 18" src="https://github.com/user-attachments/assets/657f323e-eb14-469b-a11b-44f128c9e245" />

---

### 📊 Key Findings

- **Most saturated zone:** Anaheim/Santa Ana corridor — 
  heavily overlapping buffers, lower income
- **Highest opportunity zone:** Mission Viejo/Laguna Hills — 
  median income $127k–$240k with only 1 competitor within 
  a 10-mile radius
- **Strategic insight:** Southeast OC represents the highest 
  ROI zone for premium nail spa advertising — high purchasing 
  power, almost no local competition

---

### 💼 Business Application

A nail spa running Meta ads in the Mission Viejo/Laguna Hills 
corridor would face minimal local competition while reaching 
households earning $127k–$240k annually. This geospatial 
analysis transforms raw census and OpenStreetMap data into 
a direct, actionable advertising strategy.

---

### 📁 Data Sources
- OpenStreetMap contributors via Overpass Turbo API
- US Census Bureau TIGER/Line Shapefiles 2023
- American Community Survey (ACS) 5-Year Estimates 2023<img width="1512" height="799" alt="overpass-query-screenshot" src="https://github.com/user-attachments/assets/2e616add-61ed-416a-9cf5-6416d752fb68" />

