# HUD Multifamily Assisted Housing API - Detailed Usage Guide

## 📖 Official Documentation

**HUD Open Data:** https://hudgis-hud.opendata.arcgis.com/

**Multifamily Properties Dataset:** https://hudgis-hud.opendata.arcgis.com/datasets/multifamily-properties-assisted

**HUD USER:** https://www.huduser.gov/portal/datasets/assthsg.html

**Section 202 Program:** https://www.hud.gov/program_offices/housing/mfh/progdesc/eld202

**ArcGIS REST API:** https://developers.arcgis.com/rest/

The HUD Multifamily Assisted Housing dataset provides comprehensive information about federally-assisted multifamily housing properties, including location, unit counts, demographics, financial performance, and inspection scores.

---

## 📊 Available Data

### Property Information
- Property ID and name
- Complete address (street, city, state, ZIP)
- Congressional district
- Property management details
- Owner information

### Unit Counts
- Total units
- Total assisted units
- Available units
- Units by bedroom count (0-5+ bedrooms)
- Market rate units
- Accessible units

### Program & Assistance
- Section 202/811 indicator
- Section 8 indicator
- Active contracts and programs
- Contract types (PROJECT1, PROJECT2)
- Program types (PROGRAM_TYPE1, PROGRAM_TYPE2)
- Contract expiration dates
- Financing details

### Quality & Inspection
- REAC inspection scores (0-100)
- Last inspection date
- FASS financial performance scores
- Management and occupancy scores
- Physical condition assessments

### Demographics & Occupancy
- Total residents
- People per unit
- Occupancy percentage
- Income brackets (Very Low, Low, Moderate, Other)
- Race/ethnicity breakdowns
- Age distributions (Under 18, 18-50, 51-61, 62+, 80+)
- Household composition (elderly, disabled, female head)

### Financial Metrics
- Original loan amount
- Annual expenses
- Median income
- Rent statistics
- Debt service coverage ratio

### Geographic Data
- Latitude/longitude coordinates
- Census tract, block group
- County, MSA, CBSA codes
- Metropolitan statistical area names

### Coverage
- **Properties:** 30,000+ HUD-assisted multifamily properties
- **Geographic Scope:** United States (all states + territories)
- **Update Frequency:** Updated regularly (check metadata)
- **Programs:** Section 202, 811, 8, 236, and others

---

## 🔄 Operations & Methods

### HTTP Methods
- **GET** - All requests use HTTP GET

### Service URL

**Base URL:**
```
https://services.arcgis.com/VTyQ9soqVukalItT/arcgis/rest/services/Multifamily_Properties_Assisted/FeatureServer
```

**Layer 0 (Properties):**
```
https://services.arcgis.com/VTyQ9soqVukalItT/arcgis/rest/services/Multifamily_Properties_Assisted/FeatureServer/0
```

### Available Operations

- **Query** - Retrieve features with filtering
- **Query Pivot** - Pivot table analysis
- **Query Top Features** - Ranked retrieval
- **Query Analytic** - Analytical aggregations
- **Query Bins** - Binned data analysis
- **Generate Renderer** - Dynamic styling
- **Validate SQL** - SQL syntax checking
- **Get Estimates** - Statistical estimates
- **Convert Format** - Data format conversion

---

## 🎛️ Request Options

### Query Endpoint

```
{base_url}/0/query?{parameters}
```

**Example:**
```
https://services.arcgis.com/VTyQ9soqVukalItT/arcgis/rest/services/Multifamily_Properties_Assisted/FeatureServer/0/query?where=IS_202_811_IND=1&outFields=*&f=json
```

### Common Query Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `where` | SQL WHERE clause | `IS_202_811_IND = 1` |
| `outFields` | Fields to return | `PROPERTY_NAME_TEXT,TOTAL_UNIT_COUNT` or `*` |
| `f` | Response format | `json`, `geojson`, `pbf` |
| `resultRecordCount` | Limit results | `100` |
| `resultOffset` | Pagination offset | `0`, `100`, `200` |
| `returnGeometry` | Include geometry | `true` or `false` |
| `geometry` | Spatial filter | Point, bbox, or polygon geometry |
| `geometryType` | Type of geometry | `esriGeometryPoint`, `esriGeometryEnvelope` |
| `distance` | Buffer distance | `5` (with units parameter) |
| `units` | Distance units | `esriSRUnit_StatuteMile` |
| `spatialRel` | Spatial relationship | `esriSpatialRelIntersects` |
| `orderByFields` | Sort results | `REAC_LAST_INSPECTION_SCORE DESC` |
| `returnCountOnly` | Return count only | `true` |
| `outStatistics` | Aggregate statistics | JSON array of stat definitions |

### Important Fields

**Property Identity:**
- `PROPERTY_ID` - Unique property identifier
- `PROPERTY_NAME_TEXT` - Property name
- `HUB_NAME_TEXT` - HUD hub office name

**Address:**
- `ADDRESS_LINE1_TEXT` - Street address
- `ADDRESS_LINE2_TEXT` - Additional address info
- `PLACED_BASE_CITY_NAME_TEXT` - City
- `STATE_CODE` - State abbreviation
- `ZIP_CODE` - Postal code
- `CONGRESSIONAL_DISTRICT_CODE` - Congressional district

**Units:**
- `TOTAL_UNIT_COUNT` - Total housing units
- `TOTAL_ASSISTED_UNIT_COUNT` - Assisted units
- `TOTAL_AVBL_UNITS` - Currently available units
- `BD0_CNT`, `BD1_CNT`, `BD2_CNT`, `BD3_CNT`, `BD4_CNT`, `BD5_CNT` - Bedroom counts

**Program Indicators:**
- `IS_202_811_IND` - Section 202/811 property (1=yes, 0=no)
- `IS_SEC8_IND` - Section 8 property (1=yes, 0=no)
- `PROGRAM_TYPE1` - Primary program type
- `PROGRAM_TYPE2` - Secondary program type
- `CONTRACT1`, `CONTRACT2` - Contract types

**Quality Metrics:**
- `REAC_LAST_INSPECTION_SCORE` - Physical inspection score (0-100)
- `REAC_LAST_INSPECTION_DATE` - Date of last inspection
- `FASS_LAST_PERFORMANCE_VALUE` - Financial performance score
- `MTR_LATEST_DSCR` - Debt service coverage ratio

**Demographics:**
- `PEOPLE_TOTAL` - Total residents
- `PEOPLE_PER_UNIT` - Average people per unit
- `PCT_OCCUPIED` - Occupancy percentage
- `PCT_LI_FIFTY` - % very low income (<50% AMI)
- `PCT_LI_FIFTY_EIGHTY` - % low income (50-80% AMI)
- `PCT_AGE_LT_18`, `PCT_AGE_18_50`, `PCT_AGE_51_61`, `PCT_AGE_62_PLUS`, `PCT_AGE_GT_80` - Age distributions
- `PCT_WHITE`, `PCT_BLACK_AFRICAN_AMERICAN`, `PCT_ASIAN`, `PCT_HISPANIC` - Race/ethnicity

**Financial:**
- `ORIGINAL_LOAN_AMOUNT` - Original loan amount
- `ANNL_EXPNS_AMNT` - Annual expenses
- `MEDIAN_INC_AMNT` - Median income

**Geographic:**
- `LAT` - Latitude
- `LON` - Longitude
- `COUNTY_NAME` - County name
- `MSA_NAME` - Metropolitan statistical area
- `CBSA_NAME` - Core-based statistical area

---

## ⚡ Rate Limits & Quotas

### No Authentication Required
- No API key needed
- No rate limits documented
- Free for all users

### Best Practices
- Be respectful of server resources
- Cache results when appropriate
- Use pagination for large queries
- Filter results to reduce payload size

---

## 📦 Response Format

### JSON Response Structure

```json
{
  "objectIdFieldName": "OBJECTID",
  "globalIdFieldName": "",
  "geometryType": "esriGeometryPoint",
  "spatialReference": {
    "wkid": 4326,
    "latestWkid": 4326
  },
  "fields": [
    {
      "name": "PROPERTY_ID",
      "type": "esriFieldTypeString",
      "alias": "Property ID",
      "length": 8
    },
    {
      "name": "PROPERTY_NAME_TEXT",
      "type": "esriFieldTypeString",
      "alias": "Property Name",
      "length": 65
    },
    {
      "name": "TOTAL_UNIT_COUNT",
      "type": "esriFieldTypeInteger",
      "alias": "Total Units"
    }
  ],
  "features": [
    {
      "attributes": {
        "OBJECTID": 1,
        "PROPERTY_ID": "12345678",
        "PROPERTY_NAME_TEXT": "Sunshine Senior Apartments",
        "ADDRESS_LINE1_TEXT": "123 Main Street",
        "PLACED_BASE_CITY_NAME_TEXT": "Springfield",
        "STATE_CODE": "IL",
        "ZIP_CODE": "62701",
        "TOTAL_UNIT_COUNT": 75,
        "TOTAL_ASSISTED_UNIT_COUNT": 75,
        "TOTAL_AVBL_UNITS": 2,
        "IS_202_811_IND": 1,
        "REAC_LAST_INSPECTION_SCORE": 92,
        "REAC_LAST_INSPECTION_DATE": 1640995200000,
        "PCT_OCCUPIED": 97.3,
        "PCT_AGE_62_PLUS": 100.0,
        "LAT": 39.7817,
        "LON": -89.6501
      },
      "geometry": {
        "x": -89.6501,
        "y": 39.7817
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
        "type": "Point",
        "coordinates": [-89.6501, 39.7817]
      },
      "properties": {
        "PROPERTY_NAME_TEXT": "Sunshine Senior Apartments",
        "ADDRESS_LINE1_TEXT": "123 Main Street",
        "PLACED_BASE_CITY_NAME_TEXT": "Springfield",
        "STATE_CODE": "IL",
        "TOTAL_UNIT_COUNT": 75,
        "IS_202_811_IND": 1
      }
    }
  ]
}
```

---

## 💡 Best Practices

### Efficient Queries

**DO:**
- ✅ Query specific fields when possible
- ✅ Use `returnGeometry=false` when geometry not needed
- ✅ Filter with WHERE clause
- ✅ Use pagination for large result sets
- ✅ Cache frequently accessed data
- ✅ Use appropriate spatial filters for location-based queries

**DON'T:**
- ❌ Request all 30,000+ properties without filtering
- ❌ Request geometry when only attributes needed
- ❌ Make repeated identical queries
- ❌ Ignore result count limits

### Understanding Program Types

**Section 202:**
- Supportive housing for elderly (62+)
- Service coordinators on-site
- Designed for independent living with support

**Section 811:**
- Supportive housing for persons with disabilities
- May include accessibility features
- Often paired with supportive services

**Section 8:**
- Project-based rental assistance
- Subsidizes rent for low-income tenants
- Various contract types

**Section 236:**
- Interest reduction payment program
- Older program with fewer active properties

**Filtering Tips:**
- Use `IS_202_811_IND = 1` for elderly/disabled housing
- Use `IS_SEC8_IND = 1` for Section 8 properties
- Check `PROGRAM_TYPE1` and `PROGRAM_TYPE2` for specific programs

### Data Interpretation

**REAC Scores:**
- 90-100: High performer
- 80-89: Standard performer
- 60-79: Substandard
- 30-59: Troubled
- <30: Troubled

**Occupancy:**
- `PCT_OCCUPIED` shows current occupancy rate
- `TOTAL_AVBL_UNITS` shows currently available units
- High vacancy may indicate issues or seasonal patterns

**Demographics:**
- Age percentages sum to 100%
- Race/ethnicity percentages may overlap (Hispanic can be any race)
- Income brackets based on Area Median Income (AMI)

---

## 🔍 Common Use Cases

### 1. Find Section 202 Properties in a City

```python
import requests

url = "https://services.arcgis.com/VTyQ9soqVukalItT/arcgis/rest/services/Multifamily_Properties_Assisted/FeatureServer/0/query"
params = {
    "where": "IS_202_811_IND = 1 AND PLACED_BASE_CITY_NAME_TEXT = 'Chicago'",
    "outFields": "PROPERTY_NAME_TEXT,ADDRESS_LINE1_TEXT,TOTAL_UNIT_COUNT,TOTAL_AVBL_UNITS,REAC_LAST_INSPECTION_SCORE",
    "f": "json",
    "returnGeometry": "false",
    "orderByFields": "TOTAL_AVBL_UNITS DESC"
}

response = requests.get(url, params=params)
data = response.json()

print(f"Section 202 Properties in Chicago: {len(data['features'])}\n")

for feature in data['features'][:10]:
    attrs = feature['attributes']
    print(f"{attrs['PROPERTY_NAME_TEXT']}")
    print(f"  Address: {attrs['ADDRESS_LINE1_TEXT']}")
    print(f"  Total Units: {attrs['TOTAL_UNIT_COUNT']}")
    print(f"  Available: {attrs['TOTAL_AVBL_UNITS']}")
    print(f"  Inspection Score: {attrs['REAC_LAST_INSPECTION_SCORE']}")
    print()
```

### 2. Find Properties Within Distance of a Point

```python
import requests
import json

url = "https://services.arcgis.com/VTyQ9soqVukalItT/arcgis/rest/services/Multifamily_Properties_Assisted/FeatureServer/0/query"

# Center point (Chicago downtown)
point = {
    "x": -87.6298,
    "y": 41.8781,
    "spatialReference": {"wkid": 4326}
}

params = {
    "where": "IS_202_811_IND = 1",
    "geometry": json.dumps(point),
    "geometryType": "esriGeometryPoint",
    "distance": 5,
    "units": "esriSRUnit_StatuteMile",
    "spatialRel": "esriSpatialRelIntersects",
    "outFields": "PROPERTY_NAME_TEXT,ADDRESS_LINE1_TEXT,TOTAL_UNIT_COUNT,LAT,LON",
    "f": "json"
}

response = requests.get(url, params=params)
data = response.json()

print(f"Senior housing within 5 miles: {len(data['features'])}\n")

for feature in data['features'][:10]:
    attrs = feature['attributes']
    print(f"{attrs['PROPERTY_NAME_TEXT']}")
    print(f"  {attrs['ADDRESS_LINE1_TEXT']}")
    print(f"  Coordinates: {attrs['LAT']}, {attrs['LON']}")
```

### 3. Find Properties with Low Inspection Scores

```python
import requests

url = "https://services.arcgis.com/VTyQ9soqVukalItT/arcgis/rest/services/Multifamily_Properties_Assisted/FeatureServer/0/query"
params = {
    "where": "REAC_LAST_INSPECTION_SCORE < 70 AND REAC_LAST_INSPECTION_SCORE IS NOT NULL",
    "outFields": "PROPERTY_NAME_TEXT,PLACED_BASE_CITY_NAME_TEXT,STATE_CODE,REAC_LAST_INSPECTION_SCORE,TOTAL_UNIT_COUNT",
    "f": "json",
    "returnGeometry": "false",
    "orderByFields": "REAC_LAST_INSPECTION_SCORE ASC",
    "resultRecordCount": 100
}

response = requests.get(url, params=params)
data = response.json()

print(f"Properties with inspection scores below 70:\n")

for feature in data['features'][:20]:
    attrs = feature['attributes']
    print(f"{attrs['PROPERTY_NAME_TEXT']} - {attrs['PLACED_BASE_CITY_NAME_TEXT']}, {attrs['STATE_CODE']}")
    print(f"  Score: {attrs['REAC_LAST_INSPECTION_SCORE']} | Units: {attrs['TOTAL_UNIT_COUNT']}")
```

### 4. Analyze Properties by State

```python
import requests

url = "https://services.arcgis.com/VTyQ9soqVukalItT/arcgis/rest/services/Multifamily_Properties_Assisted/FeatureServer/0/query"

# Get aggregate statistics by state
stats = [
    {
        "statisticType": "count",
        "onStatisticField": "PROPERTY_ID",
        "outStatisticFieldName": "property_count"
    },
    {
        "statisticType": "sum",
        "onStatisticField": "TOTAL_UNIT_COUNT",
        "outStatisticFieldName": "total_units"
    }
]

params = {
    "where": "IS_202_811_IND = 1",
    "groupByFieldsForStatistics": "STATE_CODE",
    "outStatistics": json.dumps(stats),
    "f": "json"
}

response = requests.get(url, params=params)
data = response.json()

print("Section 202/811 Properties by State:\n")

# Sort by property count
results = sorted(data['features'],
                 key=lambda x: x['attributes']['property_count'],
                 reverse=True)

for feature in results[:10]:
    attrs = feature['attributes']
    print(f"{attrs['STATE_CODE']}: {attrs['property_count']} properties, {attrs['total_units']:,} units")
```

### 5. Find Properties with High Elderly Population

```python
import requests

url = "https://services.arcgis.com/VTyQ9soqVukalItT/arcgis/rest/services/Multifamily_Properties_Assisted/FeatureServer/0/query"
params = {
    "where": "PCT_AGE_62_PLUS > 90 AND TOTAL_UNIT_COUNT > 50",
    "outFields": "PROPERTY_NAME_TEXT,PLACED_BASE_CITY_NAME_TEXT,STATE_CODE,TOTAL_UNIT_COUNT,PCT_AGE_62_PLUS,PCT_OCCUPIED",
    "f": "json",
    "returnGeometry": "false",
    "orderByFields": "TOTAL_UNIT_COUNT DESC",
    "resultRecordCount": 50
}

response = requests.get(url, params=params)
data = response.json()

print("Large properties with predominantly elderly residents:\n")

for feature in data['features'][:15]:
    attrs = feature['attributes']
    print(f"{attrs['PROPERTY_NAME_TEXT']} - {attrs['PLACED_BASE_CITY_NAME_TEXT']}, {attrs['STATE_CODE']}")
    print(f"  Units: {attrs['TOTAL_UNIT_COUNT']} | Elderly: {attrs['PCT_AGE_62_PLUS']:.1f}% | Occupied: {attrs['PCT_OCCUPIED']:.1f}%")
```

### 6. Find Properties with Vacancies

```python
import requests

url = "https://services.arcgis.com/VTyQ9soqVukalItT/arcgis/rest/services/Multifamily_Properties_Assisted/FeatureServer/0/query"
params = {
    "where": "TOTAL_AVBL_UNITS > 0 AND IS_202_811_IND = 1 AND STATE_CODE = 'CA'",
    "outFields": "PROPERTY_NAME_TEXT,ADDRESS_LINE1_TEXT,PLACED_BASE_CITY_NAME_TEXT,TOTAL_AVBL_UNITS,TOTAL_UNIT_COUNT,LAT,LON",
    "f": "json",
    "orderByFields": "TOTAL_AVBL_UNITS DESC"
}

response = requests.get(url, params=params)
data = response.json()

print(f"California Section 202/811 properties with vacancies: {len(data['features'])}\n")

for feature in data['features'][:15]:
    attrs = feature['attributes']
    pct_avail = (attrs['TOTAL_AVBL_UNITS'] / attrs['TOTAL_UNIT_COUNT'] * 100)

    print(f"{attrs['PROPERTY_NAME_TEXT']}")
    print(f"  Location: {attrs['PLACED_BASE_CITY_NAME_TEXT']}")
    print(f"  Available: {attrs['TOTAL_AVBL_UNITS']} of {attrs['TOTAL_UNIT_COUNT']} units ({pct_avail:.1f}%)")
    print(f"  Coordinates: {attrs['LAT']}, {attrs['LON']}")
    print()
```

### 7. Get Property Data for Mapping (GeoJSON)

```python
import requests
import json

url = "https://services.arcgis.com/VTyQ9soqVukalItT/arcgis/rest/services/Multifamily_Properties_Assisted/FeatureServer/0/query"
params = {
    "where": "IS_202_811_IND = 1 AND STATE_CODE = 'NY'",
    "outFields": "PROPERTY_NAME_TEXT,ADDRESS_LINE1_TEXT,TOTAL_UNIT_COUNT,REAC_LAST_INSPECTION_SCORE",
    "f": "geojson",
    "returnGeometry": "true"
}

response = requests.get(url, params=params)
geojson = response.json()

# Save to file
with open('ny_senior_housing.geojson', 'w') as f:
    json.dump(geojson, f)

print(f"Saved {len(geojson['features'])} properties to GeoJSON file")
```

### 8. Find Properties by Congressional District

```python
import requests

url = "https://services.arcgis.com/VTyQ9soqVukalItT/arcgis/rest/services/Multifamily_Properties_Assisted/FeatureServer/0/query"
params = {
    "where": "CONGRESSIONAL_DISTRICT_CODE = 'CA13' AND IS_202_811_IND = 1",
    "outFields": "PROPERTY_NAME_TEXT,ADDRESS_LINE1_TEXT,PLACED_BASE_CITY_NAME_TEXT,TOTAL_UNIT_COUNT",
    "f": "json",
    "returnGeometry": "false"
}

response = requests.get(url, params=params)
data = response.json()

print(f"Section 202/811 properties in CA-13:\n")

total_units = 0
for feature in data['features']:
    attrs = feature['attributes']
    print(f"{attrs['PROPERTY_NAME_TEXT']} - {attrs['PLACED_BASE_CITY_NAME_TEXT']}")
    print(f"  {attrs['TOTAL_UNIT_COUNT']} units")
    total_units += attrs['TOTAL_UNIT_COUNT']

print(f"\nTotal: {len(data['features'])} properties, {total_units} units")
```

---

## 🆘 Support & Additional Resources

- **HUD Open Data:** https://hudgis-hud.opendata.arcgis.com/
- **Multifamily Properties Dataset:** https://hudgis-hud.opendata.arcgis.com/datasets/multifamily-properties-assisted
- **Data Dictionary:** Check dataset metadata on HUD Open Data site
- **HUD USER:** https://www.huduser.gov/portal/datasets/assthsg.html
- **Section 202 Program:** https://www.hud.gov/program_offices/housing/mfh/progdesc/eld202
- **Section 811 Program:** https://www.hud.gov/program_offices/housing/mfh/progdesc/disab811
- **Multifamily Housing:** https://www.hud.gov/program_offices/housing/mfh
- **ArcGIS REST API Documentation:** https://developers.arcgis.com/rest/

### Additional HUD Resources
- **HUD Resource Locator:** https://resources.hud.gov/
- **Multifamily Data:** https://www.hud.gov/program_offices/housing/mfh/mfdata
- **Fair Housing:** https://www.hud.gov/fairhousing
