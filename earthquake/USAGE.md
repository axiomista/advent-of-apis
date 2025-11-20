# USGS Earthquake API - Detailed Usage Guide

## 📖 Official Documentation

**Primary Documentation:** https://earthquake.usgs.gov/fdsnws/event/1/

This API implements the FDSN (International Federation of Digital Seismograph Networks) Event Web Service Specification, providing standardized access to earthquake catalog data.

---

## 📊 Available Data

### Earthquake Catalog Coverage

The USGS earthquake catalog contains:

- **Real-time earthquakes** - Events detected within minutes to hours
- **Historical earthquakes** - Complete archive dating back to 1900s (coverage varies by region)
- **Global coverage** - Events worldwide, with best coverage for:
  - United States (all magnitudes)
  - Global events magnitude 4.5+
  - Regional networks with varying magnitude thresholds

### Data Freshness

- **Significant earthquakes (M 5.0+):** Updated within 5-30 minutes
- **Regional events (M 2.5-5.0):** Updated within hours
- **Small events (M < 2.5):** May take days for manual review and posting

### Geographic Networks Included

- Advanced National Seismic System (ANSS)
- Global Seismographic Network (GSN)
- Regional seismic networks across all U.S. states
- International seismic networks (varies by country)

---

## 🔄 Operations & Methods

### HTTP Methods

- **GET** - All queries use HTTP GET requests
- **POST** - Not supported

### Available Endpoints

#### 1. Query Endpoint (Primary)
```
/query
```
Returns earthquake data based on search parameters. Supports multiple output formats.

#### 2. Count Endpoint
```
/count
```
Returns only the count of earthquakes matching the search criteria (no event details).

**Example:**
```bash
curl "https://earthquake.usgs.gov/fdsnws/event/1/count?starttime=2024-01-01&minmagnitude=5"
```

#### 3. Catalogs Endpoint
```
/catalogs
```
Returns a list of available catalogs (data sources).

#### 4. Contributors Endpoint
```
/contributors
```
Returns a list of contributing networks and organizations.

#### 5. Application.wadl
```
/application.wadl
```
Returns the Web Application Description Language (WADL) document describing the service.

---

## 🎛️ Request Options

### Required Parameters

**None** - All parameters are optional, but practical queries should include at least one constraint (time, location, or magnitude).

### Time Parameters

| Parameter | Description | Format | Example |
|-----------|-------------|--------|---------|
| `starttime` | Limit to events on or after specified start time | ISO8601 or YYYY-MM-DD | `2024-01-01` or `2024-01-01T00:00:00` |
| `endtime` | Limit to events on or before specified end time | ISO8601 or YYYY-MM-DD | `2024-12-31` |
| `updatedafter` | Limit to events updated after specified time | ISO8601 | `2024-01-01T12:00:00` |

⚠️ **Note:** The default time range is the past 30 days if no time parameters are specified.

### Location Parameters - Circular Region

| Parameter | Description | Type | Range | Default |
|-----------|-------------|------|-------|---------|
| `latitude` | Center latitude | Decimal degrees | -90 to 90 | - |
| `longitude` | Center longitude | Decimal degrees | -180 to 180 | - |
| `maxradius` | Maximum radius | Decimal degrees | 0 to 180 | 180° |
| `minradius` | Minimum radius (donut search) | Decimal degrees | 0 to 180 | 0° |

**Conversion:** 1° ≈ 111 km at equator

### Location Parameters - Rectangular Region

| Parameter | Description | Type | Range |
|-----------|-------------|------|-------|
| `minlatitude` | Minimum latitude | Decimal degrees | -90 to 90 |
| `maxlatitude` | Maximum latitude | Decimal degrees | -90 to 90 |
| `minlongitude` | Minimum longitude | Decimal degrees | -180 to 180 |
| `maxlongitude` | Maximum longitude | Decimal degrees | -180 to 180 |

### Magnitude Parameters

| Parameter | Description | Type | Range |
|-----------|-------------|------|-------|
| `minmagnitude` | Minimum magnitude | Decimal | Typically -1 to 10 |
| `maxmagnitude` | Maximum magnitude | Decimal | Typically -1 to 10 |
| `magnitudetype` | Magnitude type | String | `ml`, `ms`, `mb`, `mw`, `mww`, etc. |

**Common magnitude types:**
- `mw` - Moment magnitude (preferred for large events)
- `ml` - Local magnitude (Richter scale)
- `mb` - Body wave magnitude
- `ms` - Surface wave magnitude

### Depth Parameters

| Parameter | Description | Type | Unit |
|-----------|-------------|------|------|
| `mindepth` | Minimum depth | Decimal | Kilometers (negative for above sea level) |
| `maxdepth` | Maximum depth | Decimal | Kilometers |

### Output Parameters

| Parameter | Description | Options | Default |
|-----------|-------------|---------|---------|
| `format` | Output format | `geojson`, `xml`, `csv`, `kml`, `quakeml`, `text` | `quakeml` |
| `limit` | Maximum number of results | Integer 1-20000 | 20000 |
| `offset` | Number of results to skip (pagination) | Integer ≥ 1 | 1 |
| `orderby` | Sort order | `time`, `time-asc`, `magnitude`, `magnitude-asc` | `time` |

### Additional Filters

| Parameter | Description | Type |
|-----------|-------------|------|
| `eventtype` | Type of seismic event | `earthquake`, `quarry` |
| `reviewstatus` | Review status | `automatic`, `reviewed` |
| `minmmi` | Minimum Modified Mercalli Intensity | Decimal 0-12 |
| `mincdi` | Minimum Community Decimal Intensity | Decimal 0-12 |
| `minfelt` | Minimum number of felt reports | Integer |
| `mingap` | Minimum azimuthal gap | Decimal degrees 0-360 |
| `maxgap` | Maximum azimuthal gap | Decimal degrees 0-360 |

---

## ⚡ Rate Limits & Quotas

### Official Limits

**No explicit rate limits are published**, but best practices include:

- **Reasonable request frequency:** Avoid rapid-fire requests
- **Use caching:** Cache responses for repeated queries
- **Batch requests:** Request larger time windows rather than many small requests
- **Off-peak queries:** Large historical queries should be run during off-peak hours

### Throttling Behavior

If you exceed reasonable usage:
- Requests may return HTTP 503 (Service Unavailable)
- Temporary IP blocks may occur
- Contact USGS to request exemption for research/operational needs

### Request Higher Limits

For operational or research applications requiring high-volume access:
- Email: gs-haz_earthquake-feedback@usgs.gov
- Describe your use case and expected query patterns

---

## 📦 Response Format

### GeoJSON Format (Recommended)

**Structure:**
```json
{
  "type": "FeatureCollection",
  "metadata": {
    "generated": 1640995200000,
    "url": "https://earthquake.usgs.gov/fdsnws/event/1/query?...",
    "title": "USGS Earthquakes",
    "status": 200,
    "api": "1.10.3",
    "count": 150
  },
  "features": [
    {
      "type": "Feature",
      "properties": {
        "mag": 5.8,
        "place": "12 km NE of City, Country",
        "time": 1640991234567,
        "updated": 1640995000000,
        "tz": null,
        "url": "https://earthquake.usgs.gov/earthquakes/eventpage/us6000abcd",
        "detail": "https://earthquake.usgs.gov/fdsnws/event/1/query?eventid=us6000abcd&format=geojson",
        "felt": 245,
        "cdi": 5.2,
        "mmi": 6.1,
        "alert": "green",
        "status": "reviewed",
        "tsunami": 0,
        "sig": 523,
        "net": "us",
        "code": "6000abcd",
        "ids": ",us6000abcd,",
        "sources": ",us,",
        "types": ",origin,phase-data,shakemap,",
        "nst": 67,
        "dmin": 0.456,
        "rms": 0.89,
        "gap": 45,
        "magType": "mw",
        "type": "earthquake",
        "title": "M 5.8 - 12 km NE of City, Country"
      },
      "geometry": {
        "type": "Point",
        "coordinates": [
          -120.1234,  // longitude
          35.6789,    // latitude
          10.5        // depth in km
        ]
      },
      "id": "us6000abcd"
    }
  ],
  "bbox": [
    -125.0,  // min longitude
    30.0,    // min latitude
    0.0,     // min depth
    -115.0,  // max longitude
    40.0,    // max latitude
    50.0     // max depth
  ]
}
```

### Key Property Descriptions

- **mag** - Magnitude of the earthquake
- **place** - Text description of location
- **time** - Unix timestamp in milliseconds
- **url** - Link to event page on USGS website
- **felt** - Number of "Did You Feel It?" reports
- **cdi** - Community Decimal Intensity (derived from felt reports)
- **mmi** - Modified Mercalli Intensity from ShakeMap
- **alert** - PAGER alert level (`green`, `yellow`, `orange`, `red`)
- **tsunami** - 1 if tsunami-related, 0 otherwise
- **sig** - Significance score (0-1000+)
- **gap** - Azimuthal gap in station coverage

### Other Formats

#### CSV Format
```
time,latitude,longitude,depth,mag,magType,nst,gap,dmin,rms,net,id,updated,place,type
2024-01-01T00:00:00.000Z,35.5,-120.5,10.2,4.5,ml,45,67,0.5,0.8,ci,ci12345678,2024-01-01T00:15:00.000Z,"10km N of City",earthquake
```

#### Text Format
Simple text representation, one event per line.

#### KML Format
For direct import into Google Earth and mapping applications.

---

## ⚠️ Known Limitations

### Data Gaps & Exclusions

1. **Historical completeness varies:**
   - Pre-1960s: Only large earthquakes (M 6.0+) globally
   - 1960-1990s: Improving coverage (M 4.5+ globally)
   - Post-2000: Near-complete global coverage M 4.5+

2. **Small event thresholds vary by region:**
   - California: M 1.0+ generally included
   - Remote areas: Only M 4.0+ may be cataloged

3. **Real-time data may be revised:**
   - Initial magnitudes are automatic and may change
   - Locations can shift with manual review
   - Use `reviewstatus=reviewed` for finalized data

### API Query Constraints

1. **Maximum 20,000 results per query**
   - Use pagination with `offset` for larger datasets
   - Consider breaking large queries into smaller time windows

2. **No wildcard or fuzzy matching**
   - Location filters use exact bounding boxes or circles
   - No text search on place names

3. **Time zone handling:**
   - All times are in UTC
   - The `tz` field in responses shows offset from UTC for event location

### Technical Limitations

1. **No real-time streaming:** API is pull-based only (no WebSocket/SSE)
2. **No batch POST queries:** Each query must be a separate GET request
3. **Limited sorting options:** Can only sort by time or magnitude

---

## 💡 Best Practices

### Efficient Query Patterns

**DO:**
- ✅ Use specific time ranges (not open-ended)
- ✅ Request only needed fields when possible
- ✅ Use `format=geojson` for modern applications (best performance)
- ✅ Implement client-side caching for repeated queries
- ✅ Use `limit` to control response size

**DON'T:**
- ❌ Query without any constraints (returns massive datasets)
- ❌ Poll every second for real-time data (use reasonable intervals: 1-5 minutes)
- ❌ Request overlapping time ranges repeatedly
- ❌ Download entire catalog when you only need recent events

### Caching Recommendations

**Cache aggressively for:**
- Historical data (older than 30 days) - Cache indefinitely with occasional refresh
- Reviewed events - Cache for hours/days

**Cache cautiously for:**
- Recent events (< 24 hours) - Cache for 5-15 minutes
- Large earthquakes (M 5+) - Cache for 1-5 minutes (subject to updates)

### Error Handling

**Common HTTP Status Codes:**
- `200` - Success
- `400` - Bad request (invalid parameters)
- `503` - Service unavailable (temporary outage or rate limiting)
- `414` - Request URI too long (query string too large)

**Recommended retry logic:**
```python
import time
import requests

def query_with_retry(url, params, max_retries=3):
    for attempt in range(max_retries):
        try:
            response = requests.get(url, params=params, timeout=30)
            if response.status_code == 200:
                return response.json()
            elif response.status_code == 503:
                # Service unavailable, wait and retry
                time.sleep(2 ** attempt)  # Exponential backoff
                continue
            else:
                response.raise_for_status()
        except requests.exceptions.Timeout:
            if attempt == max_retries - 1:
                raise
            time.sleep(2 ** attempt)

    raise Exception("Max retries exceeded")
```

---

## 🔍 Common Use Cases

### 1. Find Recent Significant Earthquakes (M 4.5+) Worldwide

```python
import requests
from datetime import datetime, timedelta

url = "https://earthquake.usgs.gov/fdsnws/event/1/query"
params = {
    "format": "geojson",
    "starttime": (datetime.now() - timedelta(days=7)).strftime("%Y-%m-%d"),
    "minmagnitude": 4.5,
    "orderby": "magnitude-asc"
}

response = requests.get(url, params=params)
data = response.json()

print(f"Found {data['metadata']['count']} earthquakes")
for eq in data['features']:
    props = eq['properties']
    print(f"M{props['mag']} - {props['place']}")
```

### 2. Monitor Earthquakes Near a Specific Location (e.g., San Francisco)

```python
import requests

url = "https://earthquake.usgs.gov/fdsnws/event/1/query"
params = {
    "format": "geojson",
    "latitude": 37.7749,
    "longitude": -122.4194,
    "maxradiuskm": 100,  # 100 km radius
    "starttime": "2024-01-01",
    "minmagnitude": 2.0
}

response = requests.get(url, params=params)
earthquakes = response.json()

for eq in earthquakes['features']:
    props = eq['properties']
    coords = eq['geometry']['coordinates']
    print(f"M{props['mag']} at depth {coords[2]}km - {props['place']}")
```

### 3. Get Earthquakes in a Rectangular Region (e.g., California)

```python
import requests

url = "https://earthquake.usgs.gov/fdsnws/event/1/query"
params = {
    "format": "geojson",
    "minlatitude": 32.5,
    "maxlatitude": 42.0,
    "minlongitude": -124.5,
    "maxlongitude": -114.0,
    "starttime": "2024-01-01",
    "minmagnitude": 3.0,
    "orderby": "time"
}

response = requests.get(url, params=params)
data = response.json()

print(f"California earthquakes M3.0+: {data['metadata']['count']}")
```

### 4. Paginate Through Large Result Sets

```python
import requests

url = "https://earthquake.usgs.gov/fdsnws/event/1/query"
all_earthquakes = []
limit = 1000
offset = 1

while True:
    params = {
        "format": "geojson",
        "starttime": "2020-01-01",
        "endtime": "2020-12-31",
        "minmagnitude": 5.0,
        "limit": limit,
        "offset": offset
    }

    response = requests.get(url, params=params)
    data = response.json()

    if not data['features']:
        break

    all_earthquakes.extend(data['features'])
    offset += limit

    print(f"Retrieved {len(all_earthquakes)} earthquakes so far...")

print(f"Total: {len(all_earthquakes)} earthquakes")
```

### 5. Export Earthquake Data to CSV

```bash
# Direct CSV download
curl "https://earthquake.usgs.gov/fdsnws/event/1/query?format=csv&starttime=2024-01-01&minmagnitude=4.5" > earthquakes.csv
```

---

## 🆘 Support & Additional Resources

- **Questions & Issues:** gs-haz_earthquake-feedback@usgs.gov
- **Technical Documentation:** https://earthquake.usgs.gov/fdsnws/event/1/
- **FDSN Specification:** http://www.fdsn.org/webservices/
- **Earthquake Glossary:** https://earthquake.usgs.gov/learn/glossary/
- **GitHub Examples:** https://github.com/usgs (search for earthquake API examples)
