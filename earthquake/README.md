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

## Application Examples

**1. Real-time Earthquake Alert App**
Notify users of recent earthquakes in their area or globally.
- Push notifications for significant earthquakes
- Filter by magnitude threshold and location
- Show time since earthquake occurred
- Display distance from user location
- Link to detailed USGS event pages

**2. Interactive Earthquake Map**
Visualize earthquake activity on an interactive map.
- Display earthquakes as points with magnitude-based sizing and color coding by age
- Cluster earthquakes in dense regions
- Show earthquake details on click
- Filter by magnitude, time range, and depth
- Animate earthquake sequences over time

**3. Seismic Hazard Education Platform**
Help students and public understand earthquake science.
- Visualize plate boundaries and earthquake patterns
- Show relationship between depth and tectonic setting
- Display aftershock sequences
- Explain magnitude scales and intensity
- Compare earthquake energy and impacts

**4. Emergency Response Dashboard**
Support disaster response coordination after major earthquakes.
- Monitor large earthquakes globally
- Track aftershock activity
- Display PAGER alerts for casualties/economic losses
- Show ShakeMap intensity estimates
- Integrate with response team communications

**5. Seismic Research Tool**
Support academic and scientific earthquake research.
- Query historical earthquake catalog
- Export data for statistical analysis
- Analyze spatial and temporal patterns
- Study earthquake clustering and sequences
- Compare different magnitude scales

**6. Infrastructure Monitoring System**
Track earthquakes near critical infrastructure.
- Monitor earthquakes near pipelines, dams, facilities
- Alert when events exceed thresholds
- Log earthquake exposure history
- Calculate cumulative seismic impacts
- Generate compliance reports

**7. Travel Safety App**
Provide earthquake safety information for travelers.
- Show recent earthquakes at destination
- Display seismic risk ratings by region
- Provide earthquake preparedness tips
- Alert to significant events while traveling
- Link to local emergency resources

**8. Insurance Risk Assessment Platform**
Support property and casualty insurance analysis.
- Analyze historical earthquake activity
- Calculate exposure to seismic hazards
- Model potential losses from earthquakes
- Track events for claims processing
- Generate risk reports by portfolio

## Resources

- Documentation: https://earthquake.usgs.gov/fdsnws/event/1/
- Latest Earthquakes: https://earthquake.usgs.gov/earthquakes/map/
