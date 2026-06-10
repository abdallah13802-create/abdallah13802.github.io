# Hi, I'm Abdallah
### Accelerated Data Science Student (ASU) & Media Buyer

I am an accelerated Data Science student at Arizona State University with a practical background in media buying and digital advertising. Instead of looking at data purely in spreadsheets, I am interested in how spatial data and GIS tools can be used to understand target audiences and regional demographics.

My goal is to build clean, functional data pipelines—taking messy, fragmented public datasets (like the US Census) and transforming them into clear visual maps that can guide real-world business and marketing decisions.

* **Skills I'm Developing:** Relational Data Joining, Data Cleaning & Text Manipulation, Basic SQL Query Filtering, Demographic Audience Segmentation.
* **Tools I Use:** QGIS, SQL (Basic Expressions), Python, GitHub.

---

## Project: Orange County Income Distribution Map — Geospatial Demographics Analysis

**Technologies Used:** QGIS, SQL, US Census Bureau APIs, TIGER/Line Geometry Architecture, String Regex Engineering

### 🛠️ The Objective
The goal was to engineer a highly localized demographic heatmap of **Orange County, California**, isolating median household income distributions by Zip Code Tabular Areas (ZCTAs). This asset provides geographic intelligence for target market penetration and regional demographic analysis.

### 🛑 The Core Engineering Challenges
While matching datasets sounds trivial, government databases are notoriously fragmented. The project encountered several data architecture barriers requiring custom overrides:

1. **Missing Schema Keys:** The raw US Census Bureau ACS table lacked a dedicated, normalized 5-digit zip code column required to link with standard US TIGER geometry layers.
2. **Type-Mismatch Incompatibilities:** The map layer expected a strict Text (String) datatype handshake, whereas standard spreadsheet parsers often read numeric IDs as raw integers, stripping critical leading zeros.
3. **Bloated String Descriptions:** The available geographic anchor row was locked inside descriptive text strings format: `"ZCTA5 92657"`.
4. **Geographic Noise:** The source dataset pulled records globally for the entire state of California, creating heavy rendering payloads and distorting regional classification scales.

### 💡 Technical Solutions & Implementation Pipeline

#### Phase 1: Dynamic Data Parsing & Column Injection
To build a stable bridge without altering the master database immutable layer, I injected a custom virtual data column into the tabular metadata table using this localized string manipulation architecture:
`right("Geography", 5)`

* **Impact:** Dynamically stripped away text descriptions (e.g., `"ZCTA5 92657"` -> `'92657'`) and explicitly enforced a **Text (string)** database type to ensure flawless leading-zero integrity and strict type-matching with the geometric vector targets.

#### Phase 2: Relational Spatial Joining
With matching key datatypes established, I executed a vector layer handshake mapping the newly engineered virtual zipcode key directly to the map's native `ZCTA5CE20` identifier array. I suppressed default database schema naming prefixes to keep downstream query calls clean and human-readable.

#### Phase 3: High-Performance SQL Scope Filtering
To isolate Orange County and drop processing overhead for the remaining thousands of California sectors, I wrote a bounded SQL bounding-box expression directly into the provider feature filter backend:
`"ZCTA5CE20" >= '92600' AND "ZCTA5CE20" <= '92899'`

* **Impact:** Reduced geometry calculation overhead by over 90%, instantly isolating the target study region.

#### Phase 4: Cartographic Optimization & Data Cleansing
* **Classification:** Applied an **Equal Count (Quantile)** mathematical distribution curve across 5 distinct income classes to eliminate outlier distortion and clearly emphasize localized purchasing power tiers.
* **Value Conversion:** Handled string-to-integer conversion on the fly using programmatic typecasting expressions: `to_int("Estimate!!Households!!Median income (dollars)")`.
* **Handling Nulls/Suppressed Data:** Identified census data omissions (e.g., zero-population or suppressed tracking sectors like Laguna Beach *92651*). Instead of leaving glaring white holes that imply map rendering errors, I implemented a custom alpha-transparency rule (`0% opacity`), turning data gaps into elegant, non-intrusive transparent voids showing the underlying Google Satellite raster basemap.
