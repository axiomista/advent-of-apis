# USDA Food Access Research Atlas API - Detailed Usage Guide

## 📖 Official Documentation

**Primary Documentation:** https://www.ers.usda.gov/developer/geospatial-apis

**Food Access Research Atlas:** https://www.ers.usda.gov/data-products/food-access-research-atlas

**Data Download:** https://www.ers.usda.gov/data-products/food-access-research-atlas/download-the-data

**Interactive Atlas:** https://gisportal.ers.usda.gov/portal/apps/experiencebuilder/experience/?id=a53ebd7396cd4ac3a3ed09137676fd40

The Food Access Research Atlas (FARA) provides census-tract level data on food access, mapping areas with limited access to affordable and nutritious food across the United States.

---

## 📊 Available Data

### Food Access Metrics

#### Low Income and Low Access (LILA) Indicators
Tracts are classified based on:
- **Low Income:** Poverty rate ≥20% OR median family income ≤80% of area median
- **Low Access:** Significant number of residents far from supermarket

Distance thresholds:
- **Urban tracts:** 1 mile or ½ mile to nearest supermarket
- **Rural tracts:** 10 miles or 20 miles to nearest supermarket

### Population Breakdowns

Data available by:
- **Total population**
- **Children (ages 0-17)**
- **Seniors (ages 65+)**
- **Race/ethnicity:** White, Black, Asian, Native Hawaiian/Pacific Islander, American Indian/Alaska Native, Other/Multiple, Hispanic/Latino
- **SNAP recipients**
- **Households without vehicles**

### Available Datasets

**FARA_2010** - Based on 2010 Census
- Layer 1 (Det): Detailed data
- Layer 2 (Gen): Generalized data

**FARA_2015** - 2015 Update
- Layer 1 (Det): Detailed data
- Layer 2 (Gen): Generalized data

**FARA_2019** - Most Recent (2019)
- Layer 1 (Det): Detailed data
- Layer 2 (Gen): Generalized data

### Coverage
- **Geographic Level:** Census tracts (2010 boundaries)
- **Geographic Scope:** United States (all 50 states + DC + Puerto Rico)
- **Total Tracts:** ~74,000 census tracts
- **Update Frequency:** Updated periodically (2010, 2015, 2019)

---

## 🔄 Operations & Methods

### HTTP Methods
- **GET** - All requests use HTTP GET

### Service Structure

**Base URL:**
```
https://gisportal.ers.usda.gov/server/rest/services/FARA/{dataset}/MapServer
```

**Datasets:**
- `FARA_2010` - 2010 data
- `FARA_2015` - 2015 data
- `FARA_2019` - 2019 data (recommended)

### Available Layers

Each dataset contains multiple layers organized by access metric:

#### Layer Groups (Group Layers)
- Layer 0: Low Income and Low Access at 1 and 10 miles
- Layer 3: Low Income and Low Access at .5 and 10 miles
- Layer 6: Low Income and Low Access at 1 and 20 miles
- Layer 9: Low Income and Low Access using Vehicle Access
- Layer 12: Low Income
- Layer 15: Low Access 1 and 10
- Layer 18: Low Access half and 10
- Layer 21: Low Access 1 and 20
- Layer 24: Low Vehicle Access
- Layer 27: High Group Quarters
- Layer 30: All tracts

#### Queryable Sublayers
Each group layer contains:
- **Det (Detailed)** - Odd layer IDs (1, 4, 7, etc.)
- **Gen (Generalized)** - Even layer IDs (2, 5, 8, etc.)

**Query the detailed sublayers** for attribute data.

---

## 🎛️ Request Options

### Query Endpoint Structure

```
{base_url}/{layer_id}/query?{parameters}
```

**Example:**
```
https://gisportal.ers.usda.gov/server/rest/services/FARA/FARA_2019/MapServer/1/query?where=LILATracts_1And10=1&outFields=*&f=json
```

### Common Query Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `where` | SQL WHERE clause | `LILATracts_1And10 = 1` |
| `outFields` | Fields to return | `GEOID10,St_Name,POP2010` or `*` |
| `f` | Response format | `json`, `geojson`, `pbf` |
| `resultRecordCount` | Limit results | `100` |
| `resultOffset` | Pagination offset | `0`, `100`, `200` |
| `returnGeometry` | Include geometry | `true` or `false` |
| `geometry` | Spatial filter | GeoJSON geometry |
| `geometryType` | Type of geometry | `esriGeometryPoint`, `esriGeometryPolygon` |
| `spatialRel` | Spatial relationship | `esriSpatialRelIntersects` |
| `orderByFields` | Sort results | `POP2010 DESC` |
| `returnCountOnly` | Return count only | `true` or `false` |

### Important Fields (Layer 1 - FARA_2019 Det)

**Geographic Identifiers:**
- `GEOID10` - Census tract ID
- `St_Name` - State name
- `Cnty_Name` - County name

**Population:**
- `POP2010` - Total tract population
- `OHU2010` - Total housing units
- `TractKids` - Children (0-17)
- `TractSeniors` - Seniors (65+)

**Core Indicators:**
- `LowIncomeTracts` - Low income tract (1=yes, 0=no)
- `LILATracts_1And10` - Low income & low access at 1 and 10 miles
- `LILATracts_halfAnd10` - Low income & low access at 0.5 and 10 miles
- `LILATracts_1And20` - Low income & low access at 1 and 20 miles
- `LILATracts_Vehicle` - Low income & low vehicle access

**Access Populations:**
- `lapop1_10` - Population beyond 1 mile (urban) / 10 miles (rural)
- `lapophalf` - Population beyond 0.5 mile
- `lapop1_20` - Population beyond 1 mile (urban) / 20 miles (rural)

**Demographic Details:**
- `lawhite1_10`, `lablack1_10`, `laasian1_10`, etc. - Low access population by race
- `lakids1_10` - Children with low access
- `laseniors1_10` - Seniors with low access
- `lasnap1_10` - SNAP recipients with low access

**Vehicle & Income:**
- `TractHUNV` - Housing units without vehicle
- `TractSNAP` - SNAP households
- `MedianFamilyIncome` - Median family income
- `PovertyRate` - Poverty rate

**Urban/Rural:**
- `Urban` - Urban tract (1=urban, 0=rural)
- `GroupQuartersFlag` - High group quarters (prisons, dorms, etc.)

---

## ⚡ Rate Limits & Quotas

### No Authentication Required
- No API key needed
- No rate limits documented
- Free for all users

### Best Practices
- Maximum 2,000 records per query (ArcGIS default)
- Use pagination for larger datasets
- Cache results when possible
- Be respectful of server resources

---

## 📦 Response Format

### JSON Response Structure

```json
{
  "objectIdFieldName": "OBJECTID",
  "globalIdFieldName": "",
  "geometryType": "esriGeometryPolygon",
  "spatialReference": {
    "wkid": 102100,
    "latestWkid": 3857
  },
  "fields": [
    {
      "name": "OBJECTID",
      "type": "esriFieldTypeOID",
      "alias": "OBJECTID"
    },
    {
      "name": "GEOID10",
      "type": "esriFieldTypeString",
      "alias": "Census tract",
      "length": 11
    },
    {
      "name": "St_Name",
      "type": "esriFieldTypeString",
      "alias": "State name",
      "length": 50
    },
    {
      "name": "POP2010",
      "type": "esriFieldTypeInteger",
      "alias": "Total tract population"
    }
  ],
  "features": [
    {
      "attributes": {
        "OBJECTID": 1,
        "GEOID10": "01001020100",
        "St_Name": "Alabama",
        "Cnty_Name": "Autauga",
        "POP2010": 2345,
        "Urban": 0,
        "LowIncomeTracts": 1,
        "LILATracts_1And10": 1,
        "lapop1_10": 1234,
        "TractKids": 456,
        "TractSeniors": 234,
        "MedianFamilyIncome": 45000,
        "PovertyRate": 0.22
      },
      "geometry": {
        "rings": [[[...coordinates...]]]
      }
    }
  ]
}
```

### GeoJSON Response

Use `f=geojson` parameter:

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "geometry": {
        "type": "Polygon",
        "coordinates": [[[...]]]
      },
      "properties": {
        "GEOID10": "01001020100",
        "St_Name": "Alabama",
        "Cnty_Name": "Autauga",
        "POP2010": 2345,
        "LILATracts_1And10": 1
      }
    }
  ]
}
```

---

## 💡 Best Practices

### Efficient Queries

**DO:**
- ✅ Query specific fields instead of `*` when possible
- ✅ Use `returnGeometry=false` when geometry not needed
- ✅ Filter with WHERE clause to reduce result size
- ✅ Use pagination for large queries
- ✅ Cache frequently accessed data
- ✅ Use appropriate distance metric (1/10 vs 1/20 miles)

**DON'T:**
- ❌ Request all tracts without filtering (74,000 features!)
- ❌ Request geometry when only attributes needed
- ❌ Make repeated identical queries
- ❌ Ignore pagination limits

### Understanding the Data

**Low Income Definition:**
- Poverty rate ≥20%, OR
- Median family income ≤80% of state/metro area median

**Low Access Distance Thresholds:**
- Urban: 1 mile (½ mile for stricter definition)
- Rural: 10 miles (20 miles for stricter definition)

**Supermarket Definition:**
- Stores with ≥$2 million annual sales
- Large grocery stores, supermarkets, supercenters
- Does NOT include convenience stores, gas stations, dollar stores

**Data Interpretation:**
- `LILATracts_1And10 = 1` means tract meets BOTH low income AND low access criteria
- Compare different distance thresholds to understand severity
- Urban tracts use 1-mile threshold, rural use 10-mile
- Vehicle access indicator especially important in rural areas

---

## 🔍 Common Use Cases

### 1. Find All Food Deserts in a State

```python
import requests

url = "https://gisportal.ers.usda.gov/server/rest/services/FARA/FARA_2019/MapServer/1/query"
params = {
    "where": "St_Name = 'California' AND LILATracts_1And10 = 1",
    "outFields": "GEOID10,Cnty_Name,POP2010,lapop1_10,MedianFamilyIncome",
    "f": "json",
    "returnGeometry": "false"
}

response = requests.get(url, params=params)
data = response.json()

print(f"Food desert tracts in California: {len(data['features'])}\n")

total_pop = 0
total_affected = 0

for feature in data['features']:
    attrs = feature['attributes']
    total_pop += attrs['POP2010']
    total_affected += attrs['lapop1_10']

print(f"Total population in food deserts: {total_pop:,}")
print(f"Population with low access: {total_affected:,}")
```

### 2. Find Urban Food Deserts

```python
import requests

url = "https://gisportal.ers.usda.gov/server/rest/services/FARA/FARA_2019/MapServer/1/query"
params = {
    "where": "Urban = 1 AND LILATracts_1And10 = 1",
    "outFields": "GEOID10,St_Name,Cnty_Name,POP2010,lapop1_10",
    "f": "json",
    "returnGeometry": "false",
    "orderByFields": "POP2010 DESC",
    "resultRecordCount": 100
}

response = requests.get(url, params=params)
data = response.json()

print("Top 100 Urban Food Deserts by Population:\n")

for i, feature in enumerate(data['features'][:20], 1):
    attrs = feature['attributes']
    print(f"{i}. {attrs['Cnty_Name']}, {attrs['St_Name']}")
    print(f"   Population: {attrs['POP2010']:,} | Low Access: {attrs['lapop1_10']:,}")
```

### 3. Analyze Senior Food Access

```python
import requests

url = "https://gisportal.ers.usda.gov/server/rest/services/FARA/FARA_2019/MapServer/1/query"
params = {
    "where": "TractSeniors > 500 AND laseniors1_10 > 100",
    "outFields": "GEOID10,St_Name,Cnty_Name,TractSeniors,laseniors1_10,TractHUNV",
    "f": "json",
    "returnGeometry": "false",
    "resultRecordCount": 1000
}

response = requests.get(url, params=params)
data = response.json()

print(f"Tracts with significant senior low-access populations:\n")

for feature in data['features'][:10]:
    attrs = feature['attributes']
    pct_affected = (attrs['laseniors1_10'] / attrs['TractSeniors'] * 100)

    print(f"{attrs['Cnty_Name']}, {attrs['St_Name']}")
    print(f"  Total Seniors: {attrs['TractSeniors']:,}")
    print(f"  Low Access: {attrs['laseniors1_10']:,} ({pct_affected:.1f}%)")
    print(f"  No Vehicle: {attrs['TractHUNV']:,} households")
    print()
```

### 4. Compare Food Access by Race

```python
import requests

url = "https://gisportal.ers.usda.gov/server/rest/services/FARA/FARA_2019/MapServer/1/query"
params = {
    "where": "LILATracts_1And10 = 1 AND St_Name = 'Illinois'",
    "outFields": "GEOID10,Cnty_Name,lawhite1_10,lablack1_10,laasian1_10,lahisp1_10,POP2010",
    "f": "json",
    "returnGeometry": "false"
}

response = requests.get(url, params=params)
data = response.json()

totals = {
    'white': 0,
    'black': 0,
    'asian': 0,
    'hispanic': 0,
    'total_pop': 0
}

for feature in data['features']:
    attrs = feature['attributes']
    totals['white'] += attrs['lawhite1_10']
    totals['black'] += attrs['lablack1_10']
    totals['asian'] += attrs['laasian1_10']
    totals['hispanic'] += attrs['lahisp1_10']
    totals['total_pop'] += attrs['POP2010']

print("Illinois Food Desert Population by Race/Ethnicity:\n")
print(f"White: {totals['white']:,}")
print(f"Black: {totals['black']:,}")
print(f"Asian: {totals['asian']:,}")
print(f"Hispanic: {totals['hispanic']:,}")
print(f"\nTotal Desert Population: {totals['total_pop']:,}")
```

### 5. Find Rural Areas Without Vehicle Access

```python
import requests

url = "https://gisportal.ers.usda.gov/server/rest/services/FARA/FARA_2019/MapServer/1/query"
params = {
    "where": "Urban = 0 AND LILATracts_Vehicle = 1",
    "outFields": "GEOID10,St_Name,Cnty_Name,POP2010,TractHUNV,LILATracts_1And20",
    "f": "json",
    "returnGeometry": "false",
    "orderByFields": "TractHUNV DESC",
    "resultRecordCount": 100
}

response = requests.get(url, params=params)
data = response.json()

print("Rural Tracts with Low Vehicle Access:\n")

for feature in data['features'][:15]:
    attrs = feature['attributes']
    print(f"{attrs['Cnty_Name']}, {attrs['St_Name']}")
    print(f"  Population: {attrs['POP2010']:,}")
    print(f"  Households without vehicle: {attrs['TractHUNV']:,}")
    print(f"  Also low access at 20 miles: {'Yes' if attrs['LILATracts_1And20'] == 1 else 'No'}")
    print()
```

### 6. Get Food Desert Geometries for Mapping

```python
import requests

url = "https://gisportal.ers.usda.gov/server/rest/services/FARA/FARA_2019/MapServer/1/query"
params = {
    "where": "Cnty_Name = 'Cook' AND St_Name = 'Illinois' AND LILATracts_1And10 = 1",
    "outFields": "GEOID10,POP2010,lapop1_10",
    "f": "geojson",
    "returnGeometry": "true"
}

response = requests.get(url, params=params)
geojson = response.json()

# Save to file for GIS applications
import json
with open('chicago_food_deserts.geojson', 'w') as f:
    json.dump(geojson, f)

print(f"Saved {len(geojson['features'])} food desert geometries to GeoJSON file")
```

### 7. Calculate SNAP Population in Food Deserts

```python
import requests

url = "https://gisportal.ers.usda.gov/server/rest/services/FARA/FARA_2019/MapServer/1/query"
params = {
    "where": "LILATracts_1And10 = 1",
    "outFields": "TractSNAP,lasnap1_10",
    "f": "json",
    "returnGeometry": "false"
}

response = requests.get(url, params=params)
data = response.json()

total_snap = 0
snap_low_access = 0

for feature in data['features']:
    attrs = feature['attributes']
    total_snap += attrs['TractSNAP']
    snap_low_access += attrs['lasnap1_10']

print(f"SNAP households in food desert tracts: {total_snap:,}")
print(f"SNAP population with low access: {snap_low_access:,}")
print(f"Percentage: {(snap_low_access/total_snap*100):.1f}%")
```

---

## 🆘 Support & Additional Resources

- **ERS Geospatial APIs:** https://www.ers.usda.gov/developer/geospatial-apis
- **Food Access Research Atlas:** https://www.ers.usda.gov/data-products/food-access-research-atlas
- **About the Atlas:** https://www.ers.usda.gov/data-products/food-access-research-atlas/about-the-atlas
- **Download Data:** https://www.ers.usda.gov/data-products/food-access-research-atlas/download-the-data
- **Interactive Map:** https://gisportal.ers.usda.gov/portal/apps/experiencebuilder/experience/?id=a53ebd7396cd4ac3a3ed09137676fd40
- **Documentation (PDF):** https://www.ers.usda.gov/data-products/food-access-research-atlas/documentation
- **ArcGIS REST API Docs:** https://developers.arcgis.com/rest/

### Research & Background
- **Food Access Research:** https://www.ers.usda.gov/topics/food-choices-health/food-access
- **Food Environment Atlas:** https://www.ers.usda.gov/data-products/food-environment-atlas
- **Ag Data Commons:** https://data.nal.usda.gov/dataset/food-access-research-atlas
