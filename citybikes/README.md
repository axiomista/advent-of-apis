# 🚲 CityBikes API

## 📖 Description

Discover bike-sharing networks around the world in real-time! CityBikes aggregates data from over 600 bike-sharing systems across hundreds of cities globally. Access live information about station locations, available bikes, empty docking slots, and network details. Perfect for building mobility apps, transit tools, and sustainable transportation solutions.

✨ **Best For:** Bike-finding apps, transit planners, sustainable mobility tools, and urban transportation analytics

> 💡 **Did you know?** CityBikes tracks over 600 bike-sharing networks in cities worldwide, from New York's Citi Bike to Paris's Vélib', London's Santander Cycles, and hundreds more!

## 📍 Base URL

```
http://api.citybik.es/v2
```

## 🔑 Authentication

No API key required! 🎉 Start accessing bike-sharing data immediately!

---

## 💻 Example Usage

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

---

## 🌐 Available Endpoints

- 🗺️ `/networks` - List all bike-sharing networks worldwide
- 🚲 `/networks/{id}` - Get detailed information about a specific network including stations

> 💡 **Pro Tip:** Use the `fields` parameter to request only the data you need: `?fields=id,name,location,stations`. This reduces response size and improves performance!

---

## 🔧 Common Query Patterns

- `fields` - Specify which fields to return (comma-separated)
- Network IDs are unique identifiers (e.g., `divvy`, `citi-bike-nyc`, `velib`)

---

## 🚴 Popular Networks

- **citi-bike-nyc** - New York City (Citi Bike)
- **divvy** - Chicago (Divvy)
- **bay-wheels** - San Francisco Bay Area (Bay Wheels)
- **velib** - Paris (Vélib')
- **santander-cycles** - London (Santander Cycles)
- **bicing** - Barcelona (Bicing)
- **bixi-montreal** - Montreal (BIXI)
- **capital-bikeshare** - Washington DC (Capital Bikeshare)

⚠️ **Note:** For complete network listings, station data format, and best practices, see [USAGE.md](./USAGE.md)

---

## 🚀 Application Examples

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

---

## 📚 Resources

- 📖 **API Documentation:** http://api.citybik.es/v2/
- 🌐 **Project Website:** https://citybik.es/
- 💻 **GitHub:** https://github.com/eskerda/pybikes
- 📘 **Detailed Usage Guide:** [USAGE.md](./USAGE.md)
