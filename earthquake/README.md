# USGS Earthquake Catalog API

## Description

The USGS Earthquake API provides access to earthquake event data implementing the FDSN Event Web Service Specification. It enables custom searches for earthquakes worldwide using various parameters including location, magnitude, time ranges, and depth.

## Base URL

```
https://earthquake.usgs.gov/fdsnws/event/1
```

## Authentication

No API key required.

## Example Usage

### Get Recent Earthquakes Above Magnitude 5

**cURL:**
```bash
curl "https://earthquake.usgs.gov/fdsnws/event/1/query?format=geojson&starttime=2024-01-01&endtime=2024-01-31&minmagnitude=5"
```

**Python:**
```python
import requests
from datetime import datetime, timedelta

url = "https://earthquake.usgs.gov/fdsnws/event/1/query"
params = {
    "format": "geojson",
    "starttime": "2024-01-01",
    "endtime": "2024-01-31",
    "minmagnitude": 5
}

response = requests.get(url, params=params)
earthquakes = response.json()

for feature in earthquakes['features']:
    props = feature['properties']
    coords = feature['geometry']['coordinates']
    print(f"Magnitude {props['mag']}: {props['place']}")
    print(f"Location: {coords[1]}, {coords[0]}")
    print(f"Time: {datetime.fromtimestamp(props['time']/1000)}\n")
```

**JavaScript:**
```javascript
const url = 'https://earthquake.usgs.gov/fdsnws/event/1/query';
const params = new URLSearchParams({
  format: 'geojson',
  starttime: '2024-01-01',
  endtime: '2024-01-31',
  minmagnitude: 5
});

fetch(`${url}?${params}`)
  .then(response => response.json())
  .then(data => {
    data.features.forEach(earthquake => {
      const props = earthquake.properties;
      console.log(`Magnitude ${props.mag}: ${props.place}`);
      console.log(`Time: ${new Date(props.time)}\n`);
    });
  })
  .catch(error => console.error('Error:', error));
```

### Search Earthquakes by Geographic Area

**cURL:**
```bash
curl "https://earthquake.usgs.gov/fdsnws/event/1/query?format=geojson&latitude=34.0522&longitude=-118.2437&maxradius=1&starttime=2024-01-01"
```

**Python:**
```python
import requests

url = "https://earthquake.usgs.gov/fdsnws/event/1/query"
params = {
    "format": "geojson",
    "latitude": 34.0522,   # Los Angeles
    "longitude": -118.2437,
    "maxradius": 1,        # 1 degree radius
    "starttime": "2024-01-01"
}

response = requests.get(url, params=params)
earthquakes = response.json()
print(f"Found {earthquakes['metadata']['count']} earthquakes")
```

## Common Parameters

- `format` - Output format: geojson, xml, csv, kml, text
- `starttime` / `endtime` - ISO8601 date format (YYYY-MM-DD)
- `minmagnitude` / `maxmagnitude` - Magnitude range
- `latitude` / `longitude` / `maxradius` - Circular search area

## Resources

- Documentation: https://earthquake.usgs.gov/fdsnws/event/1/
- Latest Earthquakes: https://earthquake.usgs.gov/earthquakes/map/
