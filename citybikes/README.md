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

## Resources

- Documentation: http://api.citybik.es/v2/
- All responses are in JSON format
- Field filtering supported via `?fields=` parameter
