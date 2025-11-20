# TLE (Two-Line Element) Satellite API - Detailed Usage Guide

## 📖 Official Documentation

**API Documentation:** https://tle.ivanstanojevic.me/#/docs

**TLE Format Specification:** https://en.wikipedia.org/wiki/Two-line_element_set

**CelesTrak (Data Source):** https://celestrak.org/

The TLE API provides access to Two-Line Element data for thousands of satellites, updated multiple times daily from authoritative sources like CelesTrak and Space-Track.

---

## 📊 Available Data

### Satellite Information
- Satellite name
- NORAD catalog ID
- International designator
- Launch date information
- Current orbital epoch

### TLE Orbital Elements
- **Line 1 Data:**
  - Satellite number
  - Classification (U = Unclassified, C = Classified, S = Secret)
  - International designator
  - Epoch year and day
  - First derivative of mean motion
  - Second derivative of mean motion (drag term)
  - BSTAR drag term
  - Ephemeris type
  - Element set number
  - Checksum

- **Line 2 Data:**
  - Satellite number
  - Inclination (degrees)
  - Right ascension of ascending node (degrees)
  - Eccentricity
  - Argument of perigee (degrees)
  - Mean anomaly (degrees)
  - Mean motion (revolutions per day)
  - Revolution number at epoch
  - Checksum

### Satellite Categories
The API includes satellites from various categories:
- International Space Station
- Weather satellites (NOAA, GOES, Meteosat)
- Communication satellites (Iridium, Starlink, OneWeb)
- GPS/GNSS satellites
- Scientific satellites (Hubble, Chandra, etc.)
- Amateur radio satellites
- Military satellites
- CubeSats and small satellites
- Space debris

### Coverage
- **Satellites Tracked:** 10,000+ active and inactive satellites
- **Update Frequency:** Multiple times per day
- **Data Sources:** CelesTrak, Space-Track
- **Historical Data:** Current TLE only (no historical archive)
- **Geographic Coverage:** Global (all Earth-orbiting satellites)

---

## 🔄 Operations & Methods

### HTTP Methods
- **GET** - All requests use HTTP GET

### Available Endpoints

#### Single Satellite Query

**`GET /tle/{satelliteId}`** - Get TLE for specific satellite
- `satelliteId`: NORAD catalog ID
- Returns single satellite TLE data

**Example:**
```
GET https://tle.ivanstanojevic.me/api/tle/25544
```

#### Multiple Satellites Query

**`GET /tle/{id1},{id2},{id3},...`** - Get TLE for multiple satellites
- Comma-separated NORAD IDs
- Returns collection of satellites

**Example:**
```
GET https://tle.ivanstanojevic.me/api/tle/25544,25994,28654
```

#### Search Satellites

**`GET /tle?search={query}`** - Search satellites by name
- `search`: Search term (case-insensitive)
- Returns collection of matching satellites

**Example:**
```
GET https://tle.ivanstanojevic.me/api/tle?search=starlink
```

#### List All Satellites

**`GET /tle`** - Get all satellites
- **Warning:** Very large response (10+ MB)
- Supports pagination
- Use with caution

**Example with pagination:**
```
GET https://tle.ivanstanojevic.me/api/tle?page-size=100&page=1
```

---

## 🎛️ Request Options

### Path Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `satelliteId` | NORAD catalog ID | `25544` |
| Multiple IDs | Comma-separated IDs | `25544,25994,28654` |

### Query Parameters

| Parameter | Description | Default | Example |
|-----------|-------------|---------|---------|
| `search` | Search term for satellite name | None | `starlink` |
| `page-size` | Results per page | 100 | `50` |
| `page` | Page number (1-indexed) | 1 | `2` |

### Common NORAD IDs

**Space Stations:**
- `25544` - ISS (International Space Station)
- `48274` - Tiangong (Chinese Space Station)

**Scientific Satellites:**
- `25994` - Hubble Space Telescope
- `20580` - Terra (EOS AM-1)
- `27424` - Aqua
- `39634` - James Webb Space Telescope (JWST)

**Weather Satellites:**
- `33591` - NOAA-19
- `28654` - NOAA-18
- `43226` - GOES-17
- `41866` - GOES-16

**Communication Satellites:**
- `24793` - Iridium 33
- `43013` - Starlink-1007
- `44713` - OneWeb-0001

**GPS Satellites:**
- `28654` - GPS BIIR-2 (PRN 13)
- `32384` - GPS BIIF-3 (PRN 06)

---

## ⚡ Rate Limits & Quotas

### No Authentication Required
- No API key needed
- No documented rate limits
- Free for all users

### Best Practices for Usage
- Avoid requesting `/tle` endpoint without parameters (very large)
- Use specific satellite IDs when possible
- Cache TLE data for at least 1-4 hours
- TLE data is typically updated 2-4 times per day
- Use search to find satellite IDs, then query by ID

### Fair Use Policy
- No official rate limits published
- Be respectful of server resources
- Don't poll excessively (TLE doesn't change every minute)
- Implement caching on your end

---

## 📦 Response Format

### Single Satellite Response

```json
{
  "@context": "https://schema.org",
  "@type": "Thing",
  "satelliteId": 25544,
  "name": "ISS (ZARYA)",
  "date": "2025-01-25 12:30:45",
  "line1": "1 25544U 98067A   25025.52136821  .00012345  00000-0  12345-3 0  9992",
  "line2": "2 25544  51.6416 247.4627 0006703 130.5360 325.0288 15.72125391234567"
}
```

**Fields:**
- `@context` - JSON-LD context
- `@type` - Schema.org type
- `satelliteId` - NORAD catalog ID (integer)
- `name` - Satellite name (string)
- `date` - Last update timestamp (UTC)
- `line1` - TLE line 1 (string)
- `line2` - TLE line 2 (string)

### Multiple Satellites Response

```json
{
  "@context": "https://schema.org",
  "@type": "Collection",
  "totalItems": 3,
  "member": [
    {
      "@type": "Thing",
      "satelliteId": 25544,
      "name": "ISS (ZARYA)",
      "date": "2025-01-25 12:30:45",
      "line1": "1 25544U 98067A   25025.52136821  .00012345  00000-0  12345-3 0  9992",
      "line2": "2 25544  51.6416 247.4627 0006703 130.5360 325.0288 15.72125391234567"
    },
    {
      "@type": "Thing",
      "satelliteId": 25994,
      "name": "HST",
      "date": "2025-01-25 12:28:33",
      "line1": "1 25994U 90037B   25025.52001234  .00000987  00000-0  54321-4 0  9998",
      "line2": "2 25994  28.4699 123.4567 0002891 45.6789 314.5123 15.09876543210987"
    },
    {
      "@type": "Thing",
      "satelliteId": 28654,
      "name": "NOAA 18",
      "date": "2025-01-25 12:25:12",
      "line1": "1 28654U 05018A   25025.51789012  .00000123  00000-0  67890-4 0  9993",
      "line2": "2 28654  99.0123 234.5678 0014567 123.4567 236.7890 14.12345678901234"
    }
  ]
}
```

### Search Response

Same format as multiple satellites response, with matching satellites.

---

## 📚 Understanding TLE Format

### Line 1 Breakdown

```
1 25544U 98067A   25025.52136821  .00012345  00000-0  12345-3 0  9992
```

| Position | Field | Example | Description |
|----------|-------|---------|-------------|
| 01-01 | Line number | `1` | Line 1 identifier |
| 03-07 | Satellite number | `25544` | NORAD catalog number |
| 08-08 | Classification | `U` | U=Unclassified, C=Classified, S=Secret |
| 10-17 | International designator | `98067A` | Launch year (98=1998), launch number (067), piece (A) |
| 19-32 | Epoch | `25025.52136821` | Year (25=2025), day of year with fractional day |
| 34-43 | First derivative | `.00012345` | First derivative of mean motion (orbits/day²) |
| 45-52 | Second derivative | `00000-0` | Second derivative of mean motion (orbits/day³) |
| 54-61 | BSTAR drag | `12345-3` | Drag term (1.2345 × 10⁻³) |
| 63-63 | Ephemeris type | `0` | Internal use |
| 65-68 | Element set number | `999` | Sequential number |
| 69-69 | Checksum | `2` | Modulo 10 checksum |

### Line 2 Breakdown

```
2 25544  51.6416 247.4627 0006703 130.5360 325.0288 15.72125391234567
```

| Position | Field | Example | Description |
|----------|-------|---------|-------------|
| 01-01 | Line number | `2` | Line 2 identifier |
| 03-07 | Satellite number | `25544` | NORAD catalog number (matches line 1) |
| 09-16 | Inclination | `51.6416` | Inclination in degrees |
| 18-25 | RAAN | `247.4627` | Right Ascension of Ascending Node (degrees) |
| 27-33 | Eccentricity | `0006703` | Eccentricity (0.0006703) - decimal point assumed |
| 35-42 | Argument of perigee | `130.5360` | Argument of perigee (degrees) |
| 44-51 | Mean anomaly | `325.0288` | Mean anomaly (degrees) |
| 53-63 | Mean motion | `15.72125391` | Revolutions per day |
| 64-68 | Revolution number | `23456` | Revolution number at epoch |
| 69-69 | Checksum | `7` | Modulo 10 checksum |

### Key Orbital Parameters Explained

**Inclination:** Tilt of the orbit relative to Earth's equator
- 0° = Equatorial orbit
- 90° = Polar orbit
- 51.6° = ISS orbit (covers most populated areas)

**Eccentricity:** Shape of the orbit
- 0 = Perfect circle
- Close to 0 = Nearly circular
- Close to 1 = Highly elliptical

**Mean Motion:** How many times the satellite orbits per day
- ISS: ~15.72 (orbits every ~90 minutes)
- Geostationary: ~1.00 (orbits once per day)

---

## 💡 Best Practices

### TLE Data Usage

**DO:**
- ✅ Cache TLE data for 1-4 hours
- ✅ Use specific satellite IDs when you know them
- ✅ Search first, then query by ID for efficiency
- ✅ Parse TLE data with established libraries (satellite-js, pyorbital, etc.)
- ✅ Update TLE data at least daily for accurate predictions
- ✅ Handle API errors gracefully

**DON'T:**
- ❌ Request all satellites without pagination
- ❌ Poll the API every few seconds (unnecessary)
- ❌ Parse TLE manually (use libraries)
- ❌ Assume TLE data never changes (it updates regularly)
- ❌ Use week-old TLE data for precision tracking

### Propagation Accuracy

**TLE accuracy degrades over time:**
- **0-3 days:** Highly accurate
- **3-7 days:** Good accuracy
- **7-14 days:** Moderate accuracy
- **14+ days:** Poor accuracy (especially for low orbits)

**Recommendation:** Update TLE data every 1-3 days for precise tracking

### Popular TLE Libraries

**JavaScript:**
- `satellite.js` - SGP4 propagation

**Python:**
- `skyfield` - High-precision astronomy
- `pyorbital` - Orbital calculations
- `sgp4` - Official SGP4 implementation

**Other Languages:**
- C/C++: `sgp4` library
- Java: `Orekit`
- Go: `go-satellite`

---

## 🔍 Common Use Cases

### 1. Get ISS Current TLE

```python
import requests

url = "https://tle.ivanstanojevic.me/api/tle/25544"
response = requests.get(url)
data = response.json()

print(f"Satellite: {data['name']}")
print(f"NORAD ID: {data['satelliteId']}")
print(f"Last Updated: {data['date']}")
print(f"\nTLE Data:")
print(data['line1'])
print(data['line2'])
```

### 2. Search for Starlink Satellites

```python
import requests

url = "https://tle.ivanstanojevic.me/api/tle"
params = {"search": "starlink", "page-size": 100}

response = requests.get(url, params=params)
data = response.json()

print(f"Found {data['totalItems']} Starlink satellites\n")

for sat in data['member'][:10]:  # First 10
    print(f"{sat['name']} (ID: {sat['satelliteId']})")
    print(f"  Updated: {sat['date']}")
```

### 3. Get Multiple Satellites at Once

```python
import requests

# ISS, Hubble, NOAA-19
satellite_ids = "25544,25994,33591"
url = f"https://tle.ivanstanojevic.me/api/tle/{satellite_ids}"

response = requests.get(url)
data = response.json()

print(f"Retrieved {data['totalItems']} satellites:\n")

for sat in data['member']:
    print(f"{sat['name']} (NORAD {sat['satelliteId']})")
    print(f"  Line 1: {sat['line1']}")
    print(f"  Line 2: {sat['line2']}")
    print()
```

### 4. Calculate Satellite Position (using satellite.js)

```python
import requests
from datetime import datetime
from skyfield.api import load, EarthSatellite

# Get TLE data
url = "https://tle.ivanstanojevic.me/api/tle/25544"
response = requests.get(url)
data = response.json()

# Create satellite object
ts = load.timescale()
satellite = EarthSatellite(data['line1'], data['line2'], data['name'], ts)

# Calculate current position
t = ts.now()
geocentric = satellite.at(t)

# Get latitude, longitude, altitude
subpoint = geocentric.subpoint()

print(f"ISS Position at {datetime.now().isoformat()}:")
print(f"Latitude: {subpoint.latitude.degrees:.4f}°")
print(f"Longitude: {subpoint.longitude.degrees:.4f}°")
print(f"Altitude: {subpoint.elevation.km:.2f} km")
```

### 5. Find All Weather Satellites

```python
import requests

url = "https://tle.ivanstanojevic.me/api/tle"
params = {"search": "NOAA"}

response = requests.get(url, params=params)
data = response.json()

print("NOAA Weather Satellites:\n")

for sat in data['member']:
    # Extract orbital parameters from TLE
    line2 = sat['line2']
    mean_motion = float(line2[52:63])
    period_minutes = (24 * 60) / mean_motion

    print(f"{sat['name']} (ID: {sat['satelliteId']})")
    print(f"  Orbital Period: {period_minutes:.1f} minutes")
    print(f"  Updated: {sat['date']}")
    print()
```

### 6. Parse TLE Orbital Elements

```python
import requests

url = "https://tle.ivanstanojevic.me/api/tle/25544"
response = requests.get(url)
data = response.json()

line1 = data['line1']
line2 = data['line2']

# Parse Line 1
epoch_year = int(line1[18:20])
epoch_day = float(line1[20:32])

# Parse Line 2
inclination = float(line2[8:16])
raan = float(line2[17:25])
eccentricity = float('0.' + line2[26:33])
arg_perigee = float(line2[34:42])
mean_anomaly = float(line2[43:51])
mean_motion = float(line2[52:63])
rev_number = int(line2[63:68])

print(f"Orbital Parameters for {data['name']}:")
print(f"  Epoch: Year {2000 + epoch_year}, Day {epoch_day:.2f}")
print(f"  Inclination: {inclination:.4f}°")
print(f"  RAAN: {raan:.4f}°")
print(f"  Eccentricity: {eccentricity:.7f}")
print(f"  Argument of Perigee: {arg_perigee:.4f}°")
print(f"  Mean Anomaly: {mean_anomaly:.4f}°")
print(f"  Mean Motion: {mean_motion:.8f} rev/day")
print(f"  Orbital Period: {(24*60)/mean_motion:.2f} minutes")
print(f"  Revolution Number: {rev_number}")
```

### 7. Track Satellite Passes with Pagination

```python
import requests

def get_all_satellites(search_term):
    """Get all satellites matching search term using pagination"""
    all_satellites = []
    page = 1
    page_size = 100

    while True:
        url = "https://tle.ivanstanojevic.me/api/tle"
        params = {
            "search": search_term,
            "page": page,
            "page-size": page_size
        }

        response = requests.get(url, params=params)
        data = response.json()

        all_satellites.extend(data['member'])

        # Check if there are more pages
        if len(data['member']) < page_size:
            break

        page += 1

    return all_satellites

# Get all Starlink satellites
starlinks = get_all_satellites("starlink")
print(f"Found {len(starlinks)} Starlink satellites")
```

---

## 🆘 Support & Additional Resources

- **API Documentation:** https://tle.ivanstanojevic.me/#/docs
- **TLE Format Guide:** https://en.wikipedia.org/wiki/Two-line_element_set
- **CelesTrak:** https://celestrak.org/ (Primary TLE data source)
- **Space-Track:** https://www.space-track.org/ (Authoritative TLE source)
- **N2YO Satellite Tracking:** https://www.n2yo.com/
- **Heavens-Above:** https://www.heavens-above.com/

### Orbital Mechanics Resources
- **SGP4 Propagator:** https://pypi.org/project/sgp4/
- **Skyfield (Python):** https://rhodesmill.org/skyfield/
- **Satellite.js (JavaScript):** https://github.com/shashwatak/satellite-js
- **Orbital Mechanics Tutorial:** https://orbital-mechanics.space/
