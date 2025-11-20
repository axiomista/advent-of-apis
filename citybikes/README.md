# CityBikes API

## Description

CityBikes provides access to real-time bike-sharing network data from cities worldwide. The API aggregates information from various bike-sharing operators, delivering data about station locations, available bikes, empty slots, and network details across multiple cities.

## Base URL

```
http://api.citybik.es/v2
```

## Authentication

No API key required.

## Example Usage

### List All Networks

**cURL:**
```bash
curl "http://api.citybik.es/v2/networks"
```

**Python:**
```python
import requests

url = "http://api.citybik.es/v2/networks"
response = requests.get(url)
networks = response.json()
print(networks)
```

**JavaScript:**
```javascript
const url = 'http://api.citybik.es/v2/networks';

fetch(url)
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```

### Get Specific Network Details

**cURL:**
```bash
curl "http://api.citybik.es/v2/networks/divvy?fields=id,name,location,stations"
```

**Python:**
```python
import requests

url = "http://api.citybik.es/v2/networks/divvy"
params = {"fields": "id,name,location,stations"}
response = requests.get(url, params=params)
network_data = response.json()

# Access station information
for station in network_data['network']['stations']:
    print(f"{station['name']}: {station['free_bikes']} bikes available")
```

**JavaScript:**
```javascript
const url = 'http://api.citybik.es/v2/networks/divvy?fields=id,name,location,stations';

fetch(url)
  .then(response => response.json())
  .then(data => {
    data.network.stations.forEach(station => {
      console.log(`${station.name}: ${station.free_bikes} bikes available`);
    });
  })
  .catch(error => console.error('Error:', error));
```

## Application Examples

**1. Bike Availability Finder**
Real-time bike finding app for commuters and casual riders.
- Show nearest stations with available bikes
- Display walking distance to stations
- Real-time availability updates
- Filter by bike type (electric, standard)
- Save favorite stations
- Push notifications when bikes become available

**2. Station Capacity Monitor**
Track usage patterns and station fullness over time.
- Historical availability data visualization
- Peak usage time identification
- Station capacity heatmaps
- Predict best times to find bikes or docks
- Compare stations in the same area
- Generate usage reports for city planners

**3. Multi-City Bike Share App**
Unified app for travelers using bike shares in different cities.
- Automatic network detection by location
- Consistent interface across all cities
- Find bikes in any supported city worldwide
- Display pricing for each network
- Show network coverage maps
- Trip history across multiple cities

**4. Sustainable Transit Planner**
Integrate bike sharing with other transit options.
- Combine bike share with bus/train routes
- Calculate multi-modal trip options
- Show carbon footprint savings
- Display total trip time and cost
- Suggest bike-friendly routes
- Integration with real-time transit data

## Resources

- Documentation: http://api.citybik.es/v2/
- All responses are in JSON format
- Field filtering supported via `?fields=` parameter
