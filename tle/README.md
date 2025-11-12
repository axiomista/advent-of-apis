# TLE (Two-Line Element) Satellite API

## Description

The TLE API provides access to Two-Line Element (TLE) data for satellites and space objects. TLE data is used to track satellite positions and predict their orbital paths. This API allows you to query satellite information, retrieve TLE data, and track various space objects including the ISS, weather satellites, GPS satellites, and more.

## Base URL

```
https://tle.ivanstanojevic.me/api
```

## Authentication

No API key required for basic usage.

## Example Usage

### Get TLE Data for ISS

**cURL:**
```bash
curl "https://tle.ivanstanojevic.me/api/tle/25544"
```

**Python:**
```python
import requests

# ISS NORAD ID is 25544
satellite_id = 25544
url = f"https://tle.ivanstanojevic.me/api/tle/{satellite_id}"

response = requests.get(url)
data = response.json()

print(f"Satellite: {data['name']}")
print(f"NORAD ID: {data['satelliteId']}")
print(f"TLE Line 1: {data['line1']}")
print(f"TLE Line 2: {data['line2']}")
print(f"Last Updated: {data['date']}")
```

**JavaScript:**
```javascript
// ISS NORAD ID is 25544
const satelliteId = 25544;
const url = `https://tle.ivanstanojevic.me/api/tle/${satelliteId}`;

fetch(url)
  .then(response => response.json())
  .then(data => {
    console.log(`Satellite: ${data.name}`);
    console.log(`NORAD ID: ${data.satelliteId}`);
    console.log(`TLE Line 1: ${data.line1}`);
    console.log(`TLE Line 2: ${data.line2}`);
    console.log(`Last Updated: ${data.date}`);
  })
  .catch(error => console.error('Error:', error));
```

### Search for Satellites

**cURL:**
```bash
curl "https://tle.ivanstanojevic.me/api/tle?search=starlink"
```

**Python:**
```python
import requests

url = "https://tle.ivanstanojevic.me/api/tle"
params = {"search": "starlink"}

response = requests.get(url, params=params)
satellites = response.json()

print(f"Found {len(satellites['member'])} Starlink satellites\n")
for sat in satellites['member'][:5]:  # First 5
    print(f"{sat['name']} (NORAD: {sat['satelliteId']})")
    print(f"  Line 1: {sat['line1']}")
    print(f"  Line 2: {sat['line2']}")
    print()
```

**JavaScript:**
```javascript
const url = 'https://tle.ivanstanojevic.me/api/tle';
const params = new URLSearchParams({
  search: 'starlink'
});

fetch(`${url}?${params}`)
  .then(response => response.json())
  .then(data => {
    console.log(`Found ${data.member.length} Starlink satellites\n`);
    data.member.slice(0, 5).forEach(sat => {
      console.log(`${sat.name} (NORAD: ${sat.satelliteId})`);
      console.log(`  Line 1: ${sat.line1}`);
      console.log(`  Line 2: ${sat.line2}\n`);
    });
  })
  .catch(error => console.error('Error:', error));
```

### Get Multiple Satellites by NORAD IDs

**cURL:**
```bash
curl "https://tle.ivanstanojevic.me/api/tle/25544,25994,28654"
```

**Python:**
```python
import requests

# ISS, Hubble, and GPS satellites
satellite_ids = "25544,25994,28654"
url = f"https://tle.ivanstanojevic.me/api/tle/{satellite_ids}"

response = requests.get(url)
satellites = response.json()

for sat in satellites['member']:
    print(f"\n{sat['name']} (NORAD: {sat['satelliteId']})")
    print(f"TLE Line 1: {sat['line1']}")
    print(f"TLE Line 2: {sat['line2']}")
```

**JavaScript:**
```javascript
// ISS, Hubble, and GPS satellites
const satelliteIds = '25544,25994,28654';
const url = `https://tle.ivanstanojevic.me/api/tle/${satelliteIds}`;

fetch(url)
  .then(response => response.json())
  .then(data => {
    data.member.forEach(sat => {
      console.log(`\n${sat.name} (NORAD: ${sat.satelliteId})`);
      console.log(`TLE Line 1: ${sat.line1}`);
      console.log(`TLE Line 2: ${sat.line2}`);
    });
  })
  .catch(error => console.error('Error:', error));
```

## Common Endpoints

- `/tle/{id}` - Get TLE data for specific satellite(s) by NORAD ID
- `/tle?search={query}` - Search for satellites by name
- `/tle` - Get all available TLE data (large response)

## Query Parameters

- `search` - Search satellites by name
- `page-size` - Limit number of results
- `page` - Page number for pagination

## Popular Satellite NORAD IDs

- 25544 - International Space Station (ISS)
- 25994 - Hubble Space Telescope
- 20580 - NOAA-15 Weather Satellite
- 43013 - Starlink-1007
- 37849 - GOES-16 Weather Satellite

## Understanding TLE Format

TLE data consists of two lines of formatted text containing orbital elements:
- Line 1: Satellite catalog number, epoch, ballistic coefficient, etc.
- Line 2: Inclination, right ascension, eccentricity, argument of perigee, mean anomaly, mean motion

## Resources

- Documentation: https://tle.ivanstanojevic.me/#/docs
- TLE Format Explanation: https://en.wikipedia.org/wiki/Two-line_element_set
- Satellite Tracking: https://www.n2yo.com/
