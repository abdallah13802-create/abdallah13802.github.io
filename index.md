# Hi, I'm Abdallah
---

## 🗂️ Projects

| # | Project | Description | Status |
|---|---|---|---|
| 1 | [🗺️ OC Income Heat Map](#project-1-orange-county-income-distribution-map) | Median household income by zip code | ✅ Complete |
| 2 | [📍 OC Competitor Map](#project-2) | Nail salon density vs income zones | ✅ Complete |
| 3 | [🎯 Meta Ads Geo-Intelligence](#project-3-meta-ads-geo-intelligence-system) | Ad targeting system for 4 OC local service clients | ✅ Complete |

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

<a id="project-2"></a>
# -----------------------------------------------------------
# 📍 PROJECT 2: OC Competitor Location Mapping
# -----------------------------------------------------------

<br>

### Nail Salon Density vs Income Zones — Ad Targeting Intelligence

**Technologies Used:** QGIS, OpenStreetMap, Overpass Turbo API, 
Buffer Analysis, GeoJSON

---

### 🎯 Objective
Identify high-income zip codes in Orange County with low nail 
salon competitor density — pinpointing the highest-value 
geographic zones for local service business advertising on Meta.

> *"Where is there affluent purchasing power, but a complete structural gap in local competition?"*
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

##### Step 2 — Overcoming Proximity Translation Conflicts
Initially, the project encountered an OGR database write error due to root-level Mac file pathway security. Upon bypassing, the spatial coordinate reference system (CRS) was reprojected from global degrees to local coordinates (**EPSG:2230 - NAD83 / California Zone 6**). This translated the map elements into standard local feet, unlocking native real-world mileage calculations.

<img width="938" height="641" alt="oc-market-proximity-analysis" src="https://github.com/user-attachments/assets/7e8255c3-1c61-42ef-aed5-30f095432fd9" />

*Figure 2: Clean projection of 655 beauty business locations overlaid onto the baseline Orange County household income matrix.*
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
  a 2-mile radius
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
- American Community Survey (ACS) 5-Year Estimates 2023



<a id="project-3-meta-ads-geo-intelligence-system"></a>

---
<br>

<a id="project-2"></a>
# -----------------------------------------------------------
> # 🎯 PROJECT 3: Meta Ads Geo-Intelligence System
# -----------------------------------------------------------

<br>

## Project 3: Meta Ads Geo-Intelligence System
### Ad Targeting Intelligence for Local Service Businesses

**Technologies Used:** QGIS, Python, US Census Bureau API, TIGER/Line Shapefiles, Buffer Analysis, GeoJSON, CSV Export

---

### 🎯 Objective
Build a complete geospatial ad targeting intelligence system for 4 real local service business clients of Orange AdTech — located in Corona del Mar, Newport Beach, Coto de Caza, and Yorba Linda. Instead of guessing which zip codes to target on Meta, this system combines US Census income data, drive-time buffer analysis, and campaign performance metrics to produce a ranked targeting list ready to paste directly into Meta Ads Manager.

> *"Don't guess where to run ads. Prove it with data."*

---

### 💼 Business Context
As founder of Orange AdTech, I serve 4 local service business clients operating in Orange County's highest-opportunity zones. Each client is located in a premium income corridor — Corona del Mar ($198k), Newport Beach ($178k–$185k), Coto de Caza ($178k), and Yorba Linda ($118k–$128k).

The buffer circles on the map represent each client's 30-minute drive-time service radius — the realistic zone within which their customers will travel. This constraint is critical: there is no point running Meta ads targeting zip codes 45 minutes away if customers won't make the drive.

The geospatial analysis confirms what the income data suggests — all 4 client locations sit inside Orange County's highest-scoring opportunity zones.

---

### 🗂️ Project Layer Stack
This project uses 6 layers working together in QGIS:

<img width="308" height="237" alt="p3_layers" src="https://github.com/user-attachments/assets/303fcf2a-2582-49bc-a49a-9ec268184d61" />

- **OC_Meta_Ads_Performance** — ad performance data by zip code
- **Multi-ring buffer** — 30-minute drive-time radius per client
- **Centroids** — client business location points
- **OC_Top_Targeting_Zones** — final exported targeting list
- **tl_2023_us_zcta520** — OC zip code boundaries
- **Google Satellite** — basemap

---

### 📂 Data Sources

| Data | Source | Format |
|---|---|---|
| Zip code boundaries | US Census TIGER/Line 2023 | Shapefile |
| Income demographics | ACS 5-Year Estimates 2023 | CSV |
| Ad performance metrics | Orange AdTech performance dataset | CSV |
| Drive-time zones | Buffer analysis (QGIS) | Vector polygon |

---

### 🔄 Step-by-Step Process

#### Step 1 — Joining Performance Data to Map
Joined the ad performance CSV to the OC zip code shapefile using `ZIP_CODE` → `ZCTA5CE20` field matching. The attribute table below shows all performance metrics successfully attached to each zip code geometry — including CPL, ROAS, CTR, and composite Opportunity Score.

<img width="1496" height="878" alt="p3_attribute_table" src="https://github.com/user-attachments/assets/c51ff48e-9b4d-4bfd-a075-5aec06da68e7" />

---

#### Step 2 — Opportunity Score Heat Map
Applied Graduated symbology using the composite Opportunity Score field across 5 tiers. White = lowest opportunity (avoid), Dark Red = highest opportunity (prioritize). The score combines 3 variables:
- Income level (40 points)
- ROAS efficiency (40 points)
- CPL cost-effectiveness (20 points)

All 4 client locations — Corona del Mar, Newport Beach, Coto de Caza, and Yorba Linda — fall inside the dark red high-opportunity zones, confirming their locations are geographically optimal for Meta ad targeting.

<img width="1180" height="642" alt="p3_opportunity_map" src="https://github.com/user-attachments/assets/fe0e0520-b7d7-4b2c-9981-578620bbbc4b" />

---

#### Step 3 — Drive-Time Buffer Analysis
Generated multi-ring buffer zones around each of the 4 client business locations. The circles represent 5, 10, and 15-mile service radii — constraining the Meta ad targeting to only zip codes realistically within the client's service area.

This is a critical business constraint: running ads outside the drive-time zone wastes budget on audiences who will never convert into actual customers.

<img width="1208" height="670" alt="p3_buffer_map" src="https://github.com/user-attachments/assets/d0aaf324-81be-4eff-a46d-de4592bb91d0" />

---

#### Step 4 — Actionable CSV Export
Exported the final ranked targeting list sorted by Opportunity Score descending. Each zip code is tagged META_ACTION: INCLUDE, REVIEW, or EXCLUDE — ready to paste directly into Meta Ads Manager geographic targeting settings.

The top INCLUDE zones align perfectly with the 4 client locations — validating that their businesses are positioned in Orange County's highest-value advertising corridors.

<img width="944" height="673" alt="p3_csv_export" src="https://github.com/user-attachments/assets/552ffeee-16c2-4a45-ba64-7f95312889ef" />

---

### 📊 Key Findings

**Top 5 INCLUDE Zones — All Client Locations:**

| City | Income | ROAS | CPL | Score | Action |
|---|---|---|---|---|---|
| Corona del Mar | $198k | 6.61x | $42 | 79.7 | ✅ INCLUDE |
| Newport Coast | $198k | 6.49x | $43 | 78.7 | ✅ INCLUDE |
| Newport Beach | $185k | 6.15x | $39 | 75.2 | ✅ INCLUDE |
| Newport Beach | $178k | 5.67x | $37 | 71.4 | ✅ INCLUDE |
| Coto de Caza | $178k | 5.57x | $38 | 70.5 | ✅ INCLUDE |

**Bottom EXCLUDE Zones — Outside Client Service Areas:**

| City | Income | ROAS | CPL | Score | Action |
|---|---|---|---|---|---|
| Santa Ana | $38k | 1.60x | $8 | 26.5 | ❌ EXCLUDE |
| Santa Ana | $41k | 1.62x | $11 | 26.3 | ❌ EXCLUDE |

---

### 💡 Technical Highlights

- **Composite Scoring:** Custom 0-100 Opportunity Score combining income level (40pts), ROAS efficiency (40pts), and CPL cost-effectiveness (20pts)
- **Drive-Time Constraints:** Multi-ring buffer analysis eliminates zip codes outside realistic service radius — preventing wasted ad spend on unreachable audiences
- **Client Validation:** All 4 client locations independently score in the top INCLUDE tier — confirming their business positioning is geographically optimal
- **Actionable Output:** Final CSV exports directly into Meta Ads Manager format — zero manual translation required

---

### 💼 Business Application

This system gives Orange AdTech clients a data-backed targeting decision instead of guesswork. For a nail spa in Newport Beach running Meta ads using this output:

- **Spend budget only** in zip codes scoring above 70 (ROAS 5x+)
- **Exclude automatically** zip codes scoring below 30 (Santa Ana corridor)
- **Constrain targeting** strictly to within the 30-minute drive-time service radius
- **Prioritize** the Corona del Mar / Newport Beach corridor where households earn $178k–$198k and ROAS reaches 6.61x

The result: higher ROAS, lower wasted spend, and a geo-intelligent targeting strategy that a client can see, understand, and trust.

---

### 📁 Data Sources
- US Census Bureau TIGER/Line Shapefiles 2023
- American Community Survey (ACS) 5-Year Estimates 2023
- Orange AdTech ad performance dataset (beauty/service vertical benchmarks)
