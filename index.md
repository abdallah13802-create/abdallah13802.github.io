# Hi, I'm Abdallah
 
### Accelerated Data Science Student (ASU) & Media Buyer
 
I am an accelerated Data Science student at Arizona State University with a practical background in media buying and digital advertising. Instead of looking at data purely in spreadsheets, I am interested in how spatial data and GIS tools can be used to understand target audiences and regional demographics.
 
My goal is to build clean, functional data pipelines — taking messy, fragmented public datasets (like the US Census) and transforming them into clear visual maps that can guide real-world business and marketing decisions.
 
- **Skills I'm Developing:** Relational Data Joining, Data Cleaning & Field Engineering, SQL Query Filtering, Demographic Audience Segmentation, Geospatial Buffer Analysis
- **Tools I Use:** QGIS, SQL, GitHub
---
 
## 🗂️ Projects
 
| # | Project | Description | Status |
|---|---|---|---|
| 1 | [🗺️ OC Income Heat Map](#project-1-orange-county-income-distribution-map) | Median household income by zip code | ✅ Complete |
| 2 | [📍 OC Competitor Map](#project-2-oc-competitor-location-mapping) | Nail salon density vs income zones | ✅ Complete |
| 3 | [🎯 Meta Ads Geo-Intelligence](#project-3-meta-ads-geo-intelligence-system) | Ad targeting system for 4 OC local service clients | ✅ Complete |
 
---
 
<a id="project-1-orange-county-income-distribution-map"></a>
 
# 🗺️ PROJECT 1: Orange County Income Distribution Map
 
### Geospatial Demographics Analysis
 
**Technologies Used:** QGIS, SQL, US Census Bureau APIs, TIGER/Line Shapefiles, Field Calculator (QGIS)
 
---
 
### 🎯 Objective
 
Engineer a highly localized demographic heat map of Orange County, California, isolating median household income distributions by Zip Code Tabulation Areas (ZCTAs). This asset provides geographic intelligence for target market penetration and regional demographic analysis.
 
> *"Understand where the money is before deciding where to advertise."*
 
---
 
### 🔄 Step-by-Step Process
 
#### Step 1 — Loading Raw Geographic Boundaries
 
Loaded the US Census TIGER/Line shapefile containing all California zip code boundaries into QGIS, then filtered to Orange County's primary zip code range using a SQL expression.
 
> **Scope note:** This analysis covers zip codes in the `92600–92899` range, which captures the majority of Orange County. A small number of northwest OC zip codes in the `906xx` range (Buena Park, Cypress, La Palma) fall outside this filter and were not included in this analysis.
 
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
 
<img width="2402" height="1274" alt="OC-wealth-distribution" src="https://github.com/user-attachments/assets/2152526a-c047-4563-ae52-1c9eecea1296" />
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
Injected a custom zip code column using `right("Geography", 5)` to strip text prefixes (e.g. `"ZCTA5 92657"` → `"92657"`) and enforce string datatype for leading-zero integrity.
 
**Phase 2 — Relational Spatial Join**
Mapped the engineered zip code key to the shapefile's native `ZCTA5CE20` field, suppressing default prefix naming for clean output.
 
**Phase 3 — SQL Scope Filtering**
Isolated the primary Orange County zip code range using:
```sql
"ZCTA5CE20" >= '92600' AND "ZCTA5CE20" <= '92899'
```
 
**Phase 4 — Cartographic Optimization**
- Equal Count Quantile classification across 5 income tiers
- `to_real()` typecasting for numeric conversion
- Null/suppressed zones handled with 0% opacity transparency
---
 
### 📊 Key Findings
 
- **Highest income zones:** Newport Beach and Corona del Mar corridor ($178k–$198k), with Irvine and Mission Viejo in the upper-mid tier ($115k–$127k)
- **Lower income zones:** Santa Ana and western Anaheim corridors ($17k–$63k)
- **Clear pattern:** Income increases moving southeast from the Santa Ana urban core toward the Newport Beach / Irvine corridor
- **84 zip codes** analyzed across the primary Orange County zip code range
---
 
### 💼 Business Application
 
This map directly supports Meta advertising strategy by identifying premium zip codes for high-value service business targeting. Zip codes in the $100k+ income tier represent the highest-value audiences for local service businesses in Orange County.
 
---
 
### 📁 Data Sources
 
- US Census Bureau TIGER/Line Shapefiles 2023
- American Community Survey (ACS) 5-Year Estimates 2023, Table S1901
---
 
<br>
<a id="project-2-oc-competitor-location-mapping"></a>
 
# 📍 PROJECT 2: OC Competitor Location Mapping
 
### Nail Salon Density vs Income Zones — Ad Targeting Intelligence
 
**Technologies Used:** QGIS, OpenStreetMap, Overpass Turbo API, Buffer Analysis, GeoJSON
 
---
 
### 🎯 Objective
 
Identify high-income zip codes in Orange County with low nail salon competitor density — pinpointing the highest-value geographic zones for local service business advertising on Meta.
 
> *"Where is there affluent purchasing power, but a complete structural gap in local competition?"*
 
---
 
### 📂 Data Sources
 
- **Competitor locations:** OpenStreetMap via Overpass Turbo API
- **Income zones:** Project 1 (US Census ACS 2023)
- **Zip code boundaries:** US Census TIGER/Line 2023
---
 
### 🔄 Step-by-Step Process
 
#### Step 1 — Extracting Business Location Data
 
Queried OpenStreetMap via Overpass Turbo API to pull all beauty salons and nail spas within Orange County. Exported as GeoJSON and loaded into QGIS as a point layer.
 
<img width="1512" height="799" alt="overpass-query-screenshot" src="https://github.com/user-attachments/assets/6aeafd4a-574f-4836-910d-34511e1899e1" />
---
 
#### Step 2 — Overcoming Proximity Translation Conflicts
 
Initially, the project encountered an OGR database write error due to root-level Mac file pathway security. Upon bypassing, the spatial coordinate reference system (CRS) was reprojected from global degrees to local coordinates (**EPSG:2230 - NAD83 / California Zone 6**). This translated the map elements into standard local feet, unlocking native real-world mileage calculations.
 
<img width="938" height="641" alt="oc-market-proximity-analysis" src="https://github.com/user-attachments/assets/7e8255c3-1c61-42ef-aed5-30f095432fd9" />
*Figure 2: Clean projection of 655 beauty business locations overlaid onto the baseline Orange County household income matrix.*
 
---
 
#### Step 3 — Buffer Analysis
 
Applied a 2-mile radius buffer around each business location to visualize service coverage zones. Overlapping circles reveal areas of high competitor saturation.
 
**Saturated Zone — Northwest OC (Anaheim/Santa Ana)**
High competition, lower income — avoid for premium targeting.
 
<img width="814" height="642" alt="Screenshot 2026-06-11 at 13 45 32" src="https://github.com/user-attachments/assets/35fcde25-ee9d-4300-976a-40dcdf7c5e10" />
---
 
**Opportunity Zone — Southeast OC (Mission Viejo/Laguna Hills)**
Higher income, minimal competitor presence — prime advertising zone.
 
<img width="1015" height="637" alt="Screenshot 2026-06-11 at 13 45 18" src="https://github.com/user-attachments/assets/657f323e-eb14-469b-a11b-44f128c9e245" />
---
 
### 📊 Key Findings
 
- **Most saturated zone:** Anaheim/Santa Ana corridor — heavily overlapping buffers, lower income
- **Highest opportunity zone:** Mission Viejo/Laguna Hills — median income $115k–$127k with only 1 competitor within a 2-mile radius
- **Strategic insight:** Southeast OC represents the highest ROI zone for premium nail spa advertising — solid purchasing power with almost no local competition
---
 
### 💼 Business Application
 
A nail spa running Meta ads in the Mission Viejo/Laguna Hills corridor would face minimal local competition while reaching households earning $115k–$127k annually. This geospatial analysis transforms raw census and OpenStreetMap data into a direct, actionable advertising strategy.
 
---
 
### 📁 Data Sources
 
- OpenStreetMap contributors via Overpass Turbo API
- US Census Bureau TIGER/Line Shapefiles 2023
- American Community Survey (ACS) 5-Year Estimates 2023
---
 
<br>
<a id="project-3-meta-ads-geo-intelligence-system"></a>
 
# 🎯 PROJECT 3: Meta Ads Geo-Intelligence System

## Ad Targeting Intelligence for Local Service Businesses

**Technologies Used:** QGIS, Field Calculator (QGIS), US Census Bureau API, TIGER/Line Shapefiles, Buffer Analysis, Network Isochrone Analysis (OpenRouteService), GeoJSON, CSV Export

---

### 🎯 Objective

Build a complete geospatial ad targeting intelligence system for 4 real local service business clients of Orange AdTech — located in Corona del Mar, Newport Beach, Coto de Caza, and Yorba Linda. Instead of guessing which zip codes to target on Meta, this system combines US Census income data, network-based drive-time analysis, and campaign performance metrics to produce a ranked targeting list ready to paste directly into Meta Ads Manager.

> *"Don't guess where to run ads. Prove it with data."*

---

### 💼 Business Context

As founder of Orange AdTech, I serve 4 local service business clients operating in Orange County's highest-opportunity zones. Each client is located in a strong income corridor — Corona del Mar ($198k), Newport Beach ($168k–$185k), Coto de Caza ($178k), and Yorba Linda ($118k–$128k).

The service area zones on the map represent each client's real **5, 10, and 15-minute drive-time isochrones** — the realistic zone within which their customers will travel by road. This constraint is critical: there is no point running Meta ads targeting zip codes far outside the client's actual driving range if customers won't make the trip.

The geospatial analysis confirms strong positioning for 3 of the 4 client locations — Corona del Mar, Newport Beach, and Coto de Caza all score in the top INCLUDE tier. Yorba Linda scores in the REVIEW tier, sitting well above the EXCLUDE threshold and still representing a viable targeting zone.

---

### 🗂️ Project Layer Stack

This project uses 7 layers working together in QGIS:

<img width="308" height="237" alt="p3_layers" src="https://github.com/user-attachments/assets/303fcf2a-2582-49bc-a49a-9ec268184d61" />

- **OC_Meta_Ads_Performance** — ad performance data by zip code
- **OC_Drivetime_Isochrones** — 5, 10, and 15-minute network drive-time zones per client
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
| Drive-time service zones | Network isochrone analysis (OpenRouteService) | GeoJSON / Vector polygon |

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

Corona del Mar, Newport Beach, and Coto de Caza all fall inside the dark red high-opportunity zones, confirming strong geographic positioning for Meta ad targeting. Yorba Linda scores in the REVIEW tier — a viable zone above the exclusion threshold.

<img width="1180" height="642" alt="p3_opportunity_map" src="https://github.com/user-attachments/assets/fe0e0520-b7d7-4b2c-9981-578620bbbc4b" />

---

#### Step 3 — Service Area Analysis: From Buffer to Network Isochrone

**Phase 1 — Distance Buffer (Initial Approach)**
The first version of this project used multi-ring distance buffers (5, 10, and 15-mile radii) around each client location as a quick proxy for service area. This was fast to produce but had a known limitation: a straight-line distance buffer doesn't account for actual road networks, traffic patterns, or geographic barriers like canyons and highways that shape real driving behavior in Orange County.

**Phase 2 — Network Drive-Time Isochrone (Refined Approach)**
To improve accuracy, this analysis was upgraded to true network-based isochrones using the **OpenRouteService routing API**, which calculates real drive-time zones based on the actual OC road network rather than straight-line distance. Isochrones were generated for 5, 10, and 15-minute driving intervals around each of the 4 client locations, then imported into QGIS as GeoJSON, merged into a single layer (`OC_Drivetime_Isochrones`), and styled with graduated symbology to match the Opportunity Score map.

This refinement matters in practice: a zip code that falls within a 15-mile straight-line buffer might actually be a 25-minute drive away due to canyon roads or limited highway access — meaning it would have been incorrectly included as a viable targeting zone under the Phase 1 method. The isochrone model corrects for this, producing a more defensible service area boundary for Meta ad geographic targeting.

<img width="795" height="640" alt="Screenshot 2026-06-20 at 10 19 42" src="https://github.com/user-attachments/assets/ad6ebffc-d939-411f-827b-2ff0ddb31db9" />

---

#### Step 4 — Actionable CSV Export

Exported the final ranked targeting list sorted by Opportunity Score descending. Each zip code is tagged META_ACTION: INCLUDE, REVIEW, or EXCLUDE — ready to paste directly into Meta Ads Manager geographic targeting settings.

The top INCLUDE zones align with the coastal client locations — validating that Corona del Mar, Newport Beach, and Coto de Caza are positioned in Orange County's highest-value advertising corridors.

<img width="944" height="673" alt="p3_csv_export" src="https://github.com/user-attachments/assets/552ffeee-16c2-4a45-ba64-7f95312889ef" />

---

### 📊 Key Findings

**Top 5 INCLUDE Zones:**

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

- **Composite Scoring:** Custom 0–100 Opportunity Score combining income level (40pts), ROAS efficiency (40pts), and CPL cost-effectiveness (20pts) — calculated using QGIS Field Calculator
- **Network Isochrone Analysis:** Replaced straight-line distance buffers with true drive-time service zones using the OpenRouteService routing API — accounting for real road networks rather than approximate radii
- **Iterative Methodology:** Project was deliberately revisited and upgraded from a Phase 1 distance-buffer model to a Phase 2 network isochrone model, reflecting a continuous-improvement approach to spatial accuracy
- **Client Validation:** 3 of 4 client locations independently score in the top INCLUDE tier; Yorba Linda scores in the REVIEW tier — all above the EXCLUDE threshold, confirming sound business positioning across the portfolio
- **Actionable Output:** Final CSV exports directly into Meta Ads Manager format — zero manual translation required

---

### 💼 Business Application

This system gives Orange AdTech clients a data-backed targeting decision instead of guesswork. For a nail spa in Newport Beach running Meta ads using this output:

- **Spend budget only** in zip codes scoring above 70 (ROAS 5x+)
- **Exclude automatically** zip codes scoring below 30 (Santa Ana corridor)
- **Constrain targeting** strictly to within the 15-minute network drive-time isochrone
- **Prioritize** the Corona del Mar / Newport Beach corridor where households earn $178k–$198k and ROAS reaches 6.61x

The result: higher ROAS, lower wasted spend, and a geo-intelligent targeting strategy — built on a methodology that was refined for accuracy — that a client can see, understand, and trust.
---
 
### 📁 Data Sources
 
- US Census Bureau TIGER/Line Shapefiles 2023
- American Community Survey (ACS) 5-Year Estimates 2023
- Orange AdTech ad performance dataset (beauty/service vertical benchmarks)
