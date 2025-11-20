# CityBikes API - Detailed Usage Guide

## 📖 Official Documentation

**API Endpoint:** http://api.citybik.es/v2/

**Project Website:** https://citybik.es/

**GitHub Repository:** https://github.com/eskerda/pybikes

CityBikes is a community-driven project that aggregates bike-sharing data from over 600 networks worldwide, providing a unified API to access real-time station and availability information.

---

## 📊 Available Data

### Network Information
- Network ID and name
- Company/operator name
- City and country location
- Geographic bounding box
- Number of stations
- Network-specific metadata

### Station Data
- Station ID and name
- Geographic coordinates (latitude/longitude)
- Available bikes (total and by type)
- Empty docking slots
- Station capacity
- Timestamp of last update
- Station status (online/offline)
- Extra metadata (payment methods, bike types, etc.)

### Bike Types
Some networks differentiate between:
- Standard mechanical bikes
- Electric bikes (e-bikes)
- Electric scooters
- Other vehicle types

### Coverage
- **Networks:** 600+ bike-sharing systems
- **Cities:** Hundreds of cities worldwide
- **Countries:** 60+ countries across all continents
- **Update Frequency:** Real-time (varies by network, typically 1-5 minutes)
- **Historical Data:** Not available (current status only)

### Major Networks Included
- **United States:** Citi Bike (NYC), Divvy (Chicago), Bay Wheels (SF), Capital Bikeshare (DC), Blue Bikes (Boston)
- **Europe:** Vélib' (Paris), Santander Cycles (London), Bicing (Barcelona), Nextbike (multiple cities)
- **Asia:** Seoul Bike, Taiwan YouBike, Singapore Anywheel
- **South America:** Ecobici (Buenos Aires, Mexico City)
- **Australia/Oceania:** Melbourne Bike Share

---

## 🔄 Operations & Methods

### HTTP Methods
- **GET** - All requests use HTTP GET

### Available Endpoints

#### List All Networks

**`GET /networks`** - Get list of all bike-sharing networks

**Response:** Array of network objects (without station details)

**Example:**
```
GET http://api.citybik.es/v2/networks
```

#### Get Network Details

**`GET /networks/{network_id}`** - Get detailed network information including stations

**Response:** Complete network object with all stations and their current status

**Example:**
```
GET http://api.citybik.es/v2/networks/divvy
```

#### Field Filtering

**`GET /networks/{network_id}?fields={field_list}`** - Request specific fields only

**Example:**
```
GET http://api.citybik.es/v2/networks/divvy?fields=id,name,stations
```

---

## 🎛️ Request Options

### Path Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `network_id` | Unique network identifier | `divvy`, `citi-bike-nyc`, `velib` |

### Query Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `fields` | Comma-separated list of fields to return | `id,name,location,stations` |

### Available Fields for Filtering

**Network fields:**
- `id` - Network identifier
- `name` - Network display name
- `company` - Operating company/companies
- `location` - Location object (city, country, coordinates)
- `stations` - Array of station objects
- `href` - API URL for this network

**Station fields (within stations array):**
- `id` - Station identifier
- `name` - Station name
- `timestamp` - Last update time
- `latitude` - Latitude coordinate
- `longitude` - Longitude coordinate
- `free_bikes` - Available bikes
- `empty_slots` - Available docking slots
- `extra` - Additional metadata

---

## ⚡ Rate Limits & Quotas

### No Official Rate Limits
- No authentication required
- No documented rate limits
- Community-driven project - be respectful

### Best Practices for Fair Use
- Cache network list (changes infrequently)
- Poll station data at reasonable intervals (1-5 minutes)
- Use field filtering to reduce response size
- Implement client-side caching
- Don't make unnecessary requests

### Recommended Polling Intervals
- **Network list:** Once per day
- **Station data:** Every 1-5 minutes
- **Single station:** As needed, but not more than once per minute

---

## 📦 Response Format

### Networks List Response

```json
{
  "networks": [
    {
      "company": ["Motivate International, Inc."],
      "href": "/v2/networks/divvy",
      "location": {
        "city": "Chicago, IL",
        "country": "US",
        "latitude": 41.8781136,
        "longitude": -87.6297982
      },
      "name": "Divvy",
      "id": "divvy"
    },
    {
      "company": ["NYC Bike Share, LLC"],
      "href": "/v2/networks/citi-bike-nyc",
      "location": {
        "city": "New York, NY",
        "country": "US",
        "latitude": 40.7143528,
        "longitude": -74.0059731
      },
      "name": "Citi Bike",
      "id": "citi-bike-nyc"
    }
  ]
}
```

### Network Details Response

```json
{
  "network": {
    "company": ["Motivate International, Inc."],
    "id": "divvy",
    "name": "Divvy",
    "location": {
      "city": "Chicago, IL",
      "country": "US",
      "latitude": 41.8781136,
      "longitude": -87.6297982
    },
    "stations": [
      {
        "empty_slots": 15,
        "extra": {
          "address": "State St & Harrison St",
          "altitude": 0,
          "ebikes": 2,
          "has_ebikes": true,
          "last_updated": 1706182800,
          "payment": ["key", "creditcard"],
          "payment-terminal": true,
          "renting": 1,
          "returning": 1,
          "slots": 19,
          "uid": "66"
        },
        "free_bikes": 4,
        "id": "e3752bcd72a18678dcd6df5ca09f5fc9",
        "latitude": 41.8738888,
        "longitude": -87.6278105,
        "name": "State St & Harrison St",
        "timestamp": "2025-01-25T14:00:00.000000Z"
      },
      {
        "empty_slots": 8,
        "extra": {
          "address": "Michigan Ave & Washington St",
          "altitude": 0,
          "ebikes": 1,
          "has_ebikes": true,
          "last_updated": 1706182740,
          "payment": ["key", "creditcard"],
          "payment-terminal": true,
          "renting": 1,
          "returning": 1,
          "slots": 15,
          "uid": "51"
        },
        "free_bikes": 7,
        "id": "c2e19f6494e7f0d6e4eed2b6f62f8dff",
        "latitude": 41.8835654,
        "longitude": -87.6246706,
        "name": "Michigan Ave & Washington St",
        "timestamp": "2025-01-25T14:00:00.000000Z"
      }
    ]
  }
}
```

### Field Definitions

**Network Object:**
- `id` - Unique network identifier (string)
- `name` - Display name (string)
- `company` - Array of operating companies
- `location` - Object with city, country, latitude, longitude
- `href` - API endpoint path for this network
- `stations` - Array of station objects (only in detailed view)

**Station Object:**
- `id` - Unique station identifier (string, often hash)
- `name` - Station name/address (string)
- `latitude` - Latitude coordinate (float)
- `longitude` - Longitude coordinate (float)
- `free_bikes` - Number of available bikes (integer)
- `empty_slots` - Number of empty docking slots (integer)
- `timestamp` - Last update timestamp (ISO 8601 string)
- `extra` - Object with network-specific metadata (varies)

**Extra Object (varies by network):**
- `address` - Street address
- `ebikes` - Number of available e-bikes
- `has_ebikes` - Boolean indicating e-bike availability
- `slots` - Total docking slots
- `renting` - Whether bikes can be rented (1/0)
- `returning` - Whether bikes can be returned (1/0)
- `payment` - Accepted payment methods
- `altitude` - Elevation in meters
- `uid` - Network-specific station ID
- `last_updated` - Unix timestamp

---

## 💡 Best Practices

### Efficient API Usage

**DO:**
- ✅ Cache the network list (changes rarely)
- ✅ Use field filtering to reduce payload size
- ✅ Poll station data at 1-5 minute intervals
- ✅ Store timestamps to track data freshness
- ✅ Handle missing `extra` fields gracefully
- ✅ Check `timestamp` field to validate data currency

**DON'T:**
- ❌ Request full network details repeatedly
- ❌ Poll more frequently than necessary
- ❌ Assume all networks have the same `extra` fields
- ❌ Ignore the `timestamp` field
- ❌ Request all networks with stations simultaneously

### Data Interpretation

**Understanding Availability:**
- `free_bikes` = Total available bikes (may include e-bikes)
- `empty_slots` = Available docking spaces
- Total capacity = `free_bikes + empty_slots` (approximately)

**E-Bikes:**
- Check `extra.has_ebikes` to determine if station has e-bikes
- `extra.ebikes` shows count of available e-bikes
- `free_bikes` includes both standard and e-bikes

**Station Status:**
- Check `extra.renting` - Station allows bike rentals
- Check `extra.returning` - Station allows bike returns
- If either is 0, station is partially disabled

**Data Freshness:**
- Always check `timestamp` field
- Data can be 1-5 minutes old
- Some networks update less frequently

### Handling Network Differences

Different networks provide different `extra` fields:
- Not all networks track e-bikes
- Payment information varies
- Some networks don't provide `empty_slots`
- Always check for field existence before accessing

---

## 🔍 Common Use Cases

### 1. List All Networks

```python
import requests

url = "http://api.citybik.es/v2/networks"
response = requests.get(url)
data = response.json()

print(f"Total networks: {len(data['networks'])}\n")

# Show first 10 networks
for network in data['networks'][:10]:
    location = network['location']
    print(f"{network['name']} - {location['city']}, {location['country']}")
```

### 2. Find Networks in a Specific Country

```python
import requests

url = "http://api.citybik.es/v2/networks"
response = requests.get(url)
data = response.json()

country_code = "US"
us_networks = [n for n in data['networks'] if n['location']['country'] == country_code]

print(f"Networks in {country_code}: {len(us_networks)}\n")

for network in us_networks:
    print(f"{network['name']} - {network['location']['city']}")
```

### 3. Get Station Data for a Network

```python
import requests

network_id = "divvy"
url = f"http://api.citybik.es/v2/networks/{network_id}"

response = requests.get(url)
data = response.json()

network = data['network']
stations = network['stations']

print(f"{network['name']} - {network['location']['city']}")
print(f"Total stations: {len(stations)}\n")

# Show first 5 stations
for station in stations[:5]:
    print(f"{station['name']}")
    print(f"  Available bikes: {station['free_bikes']}")
    print(f"  Empty slots: {station['empty_slots']}")
    print(f"  Last updated: {station['timestamp']}")
    print()
```

### 4. Find Nearest Station with Available Bikes

```python
import requests
from math import radians, sin, cos, sqrt, atan2

def haversine_distance(lat1, lon1, lat2, lon2):
    """Calculate distance between two points in km"""
    R = 6371  # Earth radius in km

    lat1, lon1, lat2, lon2 = map(radians, [lat1, lon1, lat2, lon2])
    dlat = lat2 - lat1
    dlon = lon2 - lon1

    a = sin(dlat/2)**2 + cos(lat1) * cos(lat2) * sin(dlon/2)**2
    c = 2 * atan2(sqrt(a), sqrt(1-a))

    return R * c

# Your location (e.g., downtown Chicago)
my_lat, my_lon = 41.8781, -87.6298

# Get Divvy stations
url = "http://api.citybik.es/v2/networks/divvy"
response = requests.get(url)
stations = response.json()['network']['stations']

# Find stations with bikes, calculate distance
stations_with_bikes = []
for station in stations:
    if station['free_bikes'] > 0:
        distance = haversine_distance(my_lat, my_lon, station['latitude'], station['longitude'])
        stations_with_bikes.append({
            'name': station['name'],
            'bikes': station['free_bikes'],
            'distance': distance
        })

# Sort by distance
stations_with_bikes.sort(key=lambda x: x['distance'])

# Show 5 nearest
print("5 Nearest Stations with Available Bikes:\n")
for station in stations_with_bikes[:5]:
    print(f"{station['name']}")
    print(f"  {station['bikes']} bikes available")
    print(f"  {station['distance']:.2f} km away")
    print()
```

### 5. Track E-Bike Availability

```python
import requests

network_id = "divvy"
url = f"http://api.citybik.es/v2/networks/{network_id}"

response = requests.get(url)
stations = response.json()['network']['stations']

ebike_stations = []
for station in stations:
    extra = station.get('extra', {})
    if extra.get('has_ebikes') and extra.get('ebikes', 0) > 0:
        ebike_stations.append({
            'name': station['name'],
            'ebikes': extra['ebikes'],
            'total_bikes': station['free_bikes'],
            'latitude': station['latitude'],
            'longitude': station['longitude']
        })

print(f"Stations with E-Bikes Available: {len(ebike_stations)}\n")

# Show stations sorted by e-bike count
ebike_stations.sort(key=lambda x: x['ebikes'], reverse=True)

for station in ebike_stations[:10]:
    print(f"{station['name']}")
    print(f"  E-bikes: {station['ebikes']} (Total: {station['total_bikes']})")
    print()
```

### 6. Monitor Station Fullness

```python
import requests

network_id = "divvy"
url = f"http://api.citybik.es/v2/networks/{network_id}"

response = requests.get(url)
stations = response.json()['network']['stations']

# Find nearly full and nearly empty stations
nearly_full = []
nearly_empty = []

for station in stations:
    total = station['free_bikes'] + station['empty_slots']
    if total > 0:
        fullness = station['free_bikes'] / total

        if fullness >= 0.9:
            nearly_full.append({
                'name': station['name'],
                'bikes': station['free_bikes'],
                'slots': station['empty_slots']
            })
        elif fullness <= 0.1:
            nearly_empty.append({
                'name': station['name'],
                'bikes': station['free_bikes'],
                'slots': station['empty_slots']
            })

print(f"Nearly Full Stations (>90%): {len(nearly_full)}\n")
for station in nearly_full[:5]:
    print(f"{station['name']}: {station['bikes']} bikes, {station['slots']} slots")

print(f"\nNearly Empty Stations (<10%): {len(nearly_empty)}\n")
for station in nearly_empty[:5]:
    print(f"{station['name']}: {station['bikes']} bikes, {station['slots']} slots")
```

### 7. Use Field Filtering for Performance

```python
import requests

# Only get ID, name, and stations (skip location details)
network_id = "divvy"
url = f"http://api.citybik.es/v2/networks/{network_id}"
params = {"fields": "id,name,stations"}

response = requests.get(url, params=params)
data = response.json()

network = data['network']
print(f"Network: {network['name']}")
print(f"Stations: {len(network['stations'])}")
print(f"Response size: {len(response.content)} bytes")
```

### 8. Find Networks Near Coordinates

```python
import requests
from math import radians, sin, cos, sqrt, atan2

def haversine_distance(lat1, lon1, lat2, lon2):
    R = 6371
    lat1, lon1, lat2, lon2 = map(radians, [lat1, lon1, lat2, lon2])
    dlat = lat2 - lat1
    dlon = lon2 - lon1
    a = sin(dlat/2)**2 + cos(lat1) * cos(lat2) * sin(dlon/2)**2
    c = 2 * atan2(sqrt(a), sqrt(1-a))
    return R * c

# Your location
my_lat, my_lon = 40.7128, -74.0060  # New York

url = "http://api.citybik.es/v2/networks"
response = requests.get(url)
networks = response.json()['networks']

# Calculate distances
networks_with_distance = []
for network in networks:
    loc = network['location']
    distance = haversine_distance(my_lat, my_lon, loc['latitude'], loc['longitude'])
    networks_with_distance.append({
        'name': network['name'],
        'city': loc['city'],
        'country': loc['country'],
        'distance': distance,
        'id': network['id']
    })

# Sort by distance
networks_with_distance.sort(key=lambda x: x['distance'])

print("Nearest Bike-Sharing Networks:\n")
for network in networks_with_distance[:10]:
    print(f"{network['name']} - {network['city']}, {network['country']}")
    print(f"  {network['distance']:.1f} km away")
    print(f"  Network ID: {network['id']}")
    print()
```

---

## 🆘 Support & Additional Resources

- **API Documentation:** http://api.citybik.es/v2/
- **Project Website:** https://citybik.es/
- **GitHub Repository:** https://github.com/eskerda/pybikes
- **Issue Tracker:** https://github.com/eskerda/pybikes/issues
- **Contributing:** https://github.com/eskerda/pybikes/blob/master/CONTRIBUTING.md

### Community & Support
CityBikes is a community-driven open-source project. If you:
- Find inaccurate data, report it on GitHub
- Want to add a new network, submit a pull request
- Have API questions, check GitHub issues
- Use the API commercially, consider supporting the project

### Data Sources
Data is sourced from official bike-sharing APIs and feeds:
- GBFS (General Bikeshare Feed Specification)
- Proprietary network APIs
- Public data feeds

### Limitations
- No historical data (real-time only)
- Data accuracy depends on source networks
- Some networks may have delayed updates
- Not all networks provide the same level of detail
