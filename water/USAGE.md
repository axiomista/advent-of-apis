# USGS Water Services API - Detailed Usage Guide

## 📖 Official Documentation

**Primary Documentation:** https://waterservices.usgs.gov/

**Test Tools:** https://waterservices.usgs.gov/test-tools/

**Parameter Codes:** https://help.waterdata.usgs.gov/parameter_cd

The USGS Water Services API provides access to water resource data from over 1.5 million monitoring sites across the United States, with billions of measurements collected annually.

---

## 📊 Available Data

### Data Categories

#### 1. **Instantaneous Values (IV)** ⚡
Real-time measurements updated every 15-60 minutes:
- Streamflow (discharge)
- Gage height (water level)
- Water temperature
- Specific conductance
- Dissolved oxygen
- pH
- Turbidity
- Precipitation

#### 2. **Daily Values (DV)** 📅
Historical daily statistics:
- Daily mean streamflow
- Daily maximum/minimum gage height
- Daily mean temperature
- Historical records dating back to early 1900s for some sites

#### 3. **Site Information** 📍
Monitoring site metadata:
- Site location (lat/long)
- Site type (stream, lake, well, spring)
- Drainage area
- Elevation
- Available parameters
- Data availability periods

#### 4. **Groundwater Levels** 🕳️
Groundwater monitoring:
- Water level measurements from wells
- Depth to water
- Historical trends
- Seasonal variations

#### 5. **Water Quality** 🔬
Laboratory and field measurements:
- Chemical analysis results
- Biological measurements
- Sediment data
- Samples from streams, lakes, groundwater

#### 6. **Statistical Services** 📊
Computed statistics:
- Daily, monthly, annual statistics
- Percentiles and medians
- Period of record statistics

### Update Frequency

- **Instantaneous Values:** 15-60 minutes (varies by site)
- **Daily Values:** Updated daily (typically by 11 AM local time next day)
- **Groundwater:** Varies (hourly to monthly depending on site)
- **Water Quality:** As samples are collected and analyzed

### Data Coverage

- **Over 1.5 million sites** monitored historically
- **Active real-time sites:** ~13,000
- **Historical records:** Some sites date back to 1800s
- **Geographic coverage:** All 50 U.S. states, territories, and tribal lands

---

## 🔄 Operations & Methods

### HTTP Methods

- **GET** - All queries use HTTP GET requests
- Read-only API

### Service Endpoints

#### Instantaneous Values (IV)
```
https://waterservices.usgs.gov/nwis/iv/
```

Real-time data updated throughout the day.

#### Daily Values (DV)
```
https://waterservices.usgs.gov/nwis/dv/
```

Historical daily statistics.

#### Site Service
```
https://waterservices.usgs.gov/nwis/site/
```

Information about monitoring sites.

#### Statistics Service
```
https://waterservices.usgs.gov/nwis/stat/
```

Statistical summaries for daily values.

#### Groundwater Levels
```
https://waterservices.usgs.gov/nwis/gwlevels/
```

Historical groundwater measurements.

---

## 🎛️ Request Options

### Common Parameters (All Services)

| Parameter | Description | Example |
|-----------|-------------|---------|
| `format` | Output format | `json`, `waterml`, `rdb` |
| `sites` | Comma-separated site numbers | `01646500,02339495` |
| `stateCd` | State code (2-letter) | `va`, `ca` |
| `huc` | Hydrologic Unit Code | `02070010` |
| `bBox` | Bounding box (west,south,east,north) | `-83,36.5,-81,38.5` |
| `countyCd` | County code (state+county) | `51059` |

### Time Parameters

| Parameter | Description | Format |
|-----------|-------------|--------|
| `startDT` | Start date/time | `2024-01-01` or `2024-01-01T00:00` |
| `endDT` | End date/time | `2024-12-31` or `2024-12-31T23:59` |
| `period` | Period before now | `PT2H` (2 hrs), `P7D` (7 days) |
| `modifiedSince` | Data modified since | `PT2H`, `P1D` |

**Period format (ISO 8601 duration):**
- `PT2H` - Past 2 hours
- `P7D` - Past 7 days
- `P1M` - Past 1 month

### Parameter Codes

**`parameterCd`** - Which measurements to retrieve

**Common codes:**
- `00060` - Discharge (streamflow), cubic feet per second
- `00065` - Gage height, feet
- `00010` - Temperature, water, degrees Celsius
- `00095` - Specific conductance, microsiemens per centimeter at 25°C
- `00300` - Dissolved oxygen, mg/L
- `00400` - pH
- `63680` - Turbidity, FNU

**Multiple parameters:**
```
parameterCd=00060,00065,00010
```

**Full list:** https://help.waterdata.usgs.gov/parameter_cd

### Site Type Filters

**`siteType`** - Filter by site type

- `ST` - Stream
- `LK` - Lake, Reservoir, Impoundment
- `GW` - Well
- `SP` - Spring
- `AT` - Atmosphere
- `ES` - Estuary
- `OC` - Ocean

### Site Status

**`siteStatus`** - Filter by operational status

- `all` - All sites (default)
- `active` - Currently active sites
- `inactive` - Inactive sites

### Service-Specific Parameters

#### IV/DV Services

| Parameter | Description |
|-----------|-------------|
| `siteStatus` | `all`, `active`, `inactive` |
| `hasDataTypeCd` | Filter sites with specific data types: `iv`, `dv` |

#### Site Service

| Parameter | Description |
|-----------|-------------|
| `hasDataTypeCd` | Sites with data: `iv`, `dv`, `pk`, `sv`, `gw`, `qw`, `ad`, `aw` |
| `outputDataTypeCd` | Include available data types in output |

#### Statistics Service

| Parameter | Description |
|-----------|-------------|
| `statReportType` | `daily`, `monthly`, `annual` |
| `statTypeCd` | Statistic type: `mean`, `max`, `min`, `median`, `all` |

---

## ⚡ Rate Limits & Quotas

### No Official Rate Limits

USGS Water Services has **no published rate limits**, but best practices apply:

**Recommendations:**
- ✅ Use reasonable request rates (avoid rapid-fire requests)
- ✅ Cache data appropriately
- ✅ Use `period` parameter instead of repeated recent queries
- ✅ Request only needed sites and parameters

### Service Performance

- Most queries return in < 2 seconds
- Large queries (many sites/long periods) may take longer
- Real-time data queries are typically faster than historical

---

## 📦 Response Format

### JSON Format (Recommended)

**Structure:**
```json
{
  "name": "ns1:timeSeriesResponseType",
  "declaredType": "org.cuahsi.waterml.TimeSeriesResponseType",
  "value": {
    "queryInfo": {
      "criteria": {
        "locationParam": "[ALL:01646500]",
        "variableParam": "[00060,00065]"
      }
    },
    "timeSeries": [
      {
        "sourceInfo": {
          "siteName": "POTOMAC RIVER NEAR WASH, DC LITTLE FALLS PUMP STA",
          "siteCode": [
            {
              "value": "01646500",
              "network": "NWIS",
              "agencyCode": "USGS"
            }
          ],
          "geoLocation": {
            "geogLocation": {
              "latitude": 38.94977778,
              "longitude": -77.12763889
            }
          }
        },
        "variable": {
          "variableCode": [
            {
              "value": "00060"
            }
          ],
          "variableName": "Streamflow, ft&#179;/s",
          "unit": {
            "unitCode": "ft3/s"
          },
          "noDataValue": -999999.0
        },
        "values": [
          {
            "value": [
              {
                "value": "2950",
                "dateTime": "2024-01-15T00:00:00.000-05:00"
              },
              {
                "value": "2980",
                "dateTime": "2024-01-15T00:15:00.000-05:00"
              }
            ]
          }
        ]
      }
    ]
  }
}
```

### RDB Format (Tab-Delimited)

Simple tab-delimited text format:
```
# Data provided for site 01646500
# TS_ID  Parameter  Statistic  DD  Value  Date
5s 15s 15s 10s 14s 20s
227220 00060 00003 01646500 2950 2024-01-15 00:00
227220 00060 00003 01646500 2980 2024-01-15 00:15
```

### WaterML Format

XML-based WaterML 1.1 format (verbose but comprehensive).

---

## ⚠️ Known Limitations

### Data Availability

1. **Not all sites have real-time data:**
   - Only ~13,000 sites have instantaneous values
   - Use `hasDataTypeCd=iv` to filter

2. **Parameter availability varies:**
   - Not all sites measure all parameters
   - Check site info before querying specific parameters

3. **Data gaps are common:**
   - Equipment malfunctions
   - Maintenance periods
   - Extreme conditions
   - Missing values typically shown as -999999

4. **Time zone handling:**
   - Times are in local standard time for the site
   - Daylight saving time NOT observed
   - JSON includes timezone offset

### Query Constraints

1. **120-day limit for IV data:**
   - Instantaneous values queries limited to 120 days
   - Use multiple queries for longer periods

2. **Large queries may timeout:**
   - Requesting many sites with long time periods
   - Break into smaller requests if needed

3. **No aggregation functions:**
   - Cannot compute averages, sums, etc. via API
   - Must process data client-side

### Service-Specific Limitations

1. **Site service doesn't return data values:**
   - Only returns site metadata
   - Must use IV/DV services for actual measurements

2. **Statistics service only for daily values:**
   - Cannot get statistics for instantaneous values via API

3. **Provisional data may be revised:**
   - Real-time data is provisional
   - Subject to revision during review process
   - Use approved data for official purposes

---

## 💡 Best Practices

### Efficient Querying

**DO:**
- ✅ Use `sites` parameter for specific sites (faster than geographic filters)
- ✅ Limit time ranges to what you need
- ✅ Request only needed parameters
- ✅ Use `period` for recent data (e.g., `P7D` for last 7 days)
- ✅ Cache site metadata (changes infrequently)

**DON'T:**
- ❌ Query all sites in a state without filters
- ❌ Request 120 days when you only need 7
- ❌ Make identical queries repeatedly
- ❌ Request all parameters when you only need one

### Finding Sites

**Method 1: Use the Site Service**
```python
import requests

url = "https://waterservices.usgs.gov/nwis/site/"
params = {
    "format": "json",
    "stateCd": "va",
    "siteType": "ST",
    "hasDataTypeCd": "iv",
    "parameterCd": "00060"
}

response = requests.get(url, params=params)
sites = response.json()
```

**Method 2: Use USGS Water Data website**
https://waterdata.usgs.gov/nwis

### Handling Provisional Data

Real-time data is **provisional** and subject to revision:

```python
# Check if data is provisional
if 'value' in time_series:
    for value_set in time_series['values']:
        for point in value_set['value']:
            # Provisional data typically has qualifiers
            if 'qualifiers' in point:
                print(f"Provisional: {point['qualifiers']}")
```

### Time Zone Handling

```python
from datetime import datetime

# Parse ISO 8601 timestamp with timezone
timestamp_str = "2024-01-15T12:30:00.000-05:00"
dt = datetime.fromisoformat(timestamp_str)
print(f"Local time: {dt}")
print(f"UTC: {dt.astimezone()}")
```

---

## 🔍 Common Use Cases

### 1. Get Current Streamflow for a Site

```python
import requests

site = "01646500"  # Potomac River
url = "https://waterservices.usgs.gov/nwis/iv/"
params = {
    "format": "json",
    "sites": site,
    "parameterCd": "00060",  # Streamflow
    "siteStatus": "all"
}

response = requests.get(url, params=params)
data = response.json()

# Extract latest value
ts = data['value']['timeSeries'][0]
latest = ts['values'][0]['value'][-1]
print(f"Current streamflow: {latest['value']} cubic feet per second")
print(f"Time: {latest['dateTime']}")
```

### 2. Get Historical Daily Data

```python
import requests

url = "https://waterservices.usgs.gov/nwis/dv/"
params = {
    "format": "json",
    "sites": "01646500",
    "parameterCd": "00060",
    "startDT": "2023-01-01",
    "endDT": "2023-12-31"
}

response = requests.get(url, params=params)
data = response.json()

ts = data['value']['timeSeries'][0]
for point in ts['values'][0]['value']:
    print(f"{point['dateTime']}: {point['value']} cfs")
```

### 3. Find All Stream Gages in a State with Real-Time Data

```python
import requests

url = "https://waterservices.usgs.gov/nwis/site/"
params = {
    "format": "json",
    "stateCd": "va",
    "siteType": "ST",
    "hasDataTypeCd": "iv",
    "siteStatus": "active"
}

response = requests.get(url, params=params)
sites = response.json()

for site in sites['value']['timeSeries']:
    site_info = site['sourceInfo']
    print(f"{site_info['siteName']} ({site_info['siteCode'][0]['value']})")
```

### 4. Monitor Multiple Parameters at Once

```python
import requests

url = "https://waterservices.usgs.gov/nwis/iv/"
params = {
    "format": "json",
    "sites": "01646500",
    "parameterCd": "00060,00065,00010",  # Flow, gage height, temperature
    "period": "PT2H"  # Last 2 hours
}

response = requests.get(url, params=params)
data = response.json()

for ts in data['value']['timeSeries']:
    var_name = ts['variable']['variableName']
    unit = ts['variable']['unit']['unitCode']
    latest = ts['values'][0]['value'][-1]
    print(f"{var_name}: {latest['value']} {unit}")
```

### 5. Get Sites in a Bounding Box

```python
import requests

url = "https://waterservices.usgs.gov/nwis/site/"
params = {
    "format": "json",
    "bBox": "-77.5,38.5,-76.5,39.5",  # Around Washington DC
    "siteType": "ST",
    "hasDataTypeCd": "iv"
}

response = requests.get(url, params=params)
# Process sites in the bounding box
```

### 6. Get Recent Data Modified Since Last Check

```python
import requests

url = "https://waterservices.usgs.gov/nwis/iv/"
params = {
    "format": "json",
    "stateCd": "va",
    "parameterCd": "00060",
    "modifiedSince": "PT1H"  # Modified in last hour
}

response = requests.get(url, params=params)
# Process recently updated sites
```

---

## 🆘 Support & Additional Resources

- **Help Desk:** https://water.usgs.gov/contact/gsanswers
- **Water Data Website:** https://waterdata.usgs.gov/nwis
- **Parameter Codes:** https://help.waterdata.usgs.gov/parameter_cd
- **Site Numbers:** https://waterdata.usgs.gov/nwis/inventory
- **WaterWatch:** https://waterwatch.usgs.gov/ (visual interface)
- **FAQs:** https://help.waterdata.usgs.gov/
