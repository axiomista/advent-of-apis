# Open Notify ISS API - Detailed Usage Guide

## 📖 Official Documentation

**Website:** http://open-notify.org/

**GitHub:** https://github.com/open-notify/Open-Notify-API

A simple API for tracking the International Space Station and astronauts in space.

---

## 📊 Available Data

### ISS Location
- Real-time latitude and longitude
- Unix timestamp
- Updates continuously as ISS orbits

### People in Space
- Current count of humans in space
- Names of astronauts/cosmonauts
- Spacecraft assignment (ISS, Tiangong, etc.)

### Coverage

**Geographic:** Global ISS tracking
**Update Frequency:** Real-time (location changes continuously)
**Historical Data:** Not available

---

## 🔄 Operations & Methods

### HTTP Methods
- **GET** - All requests use HTTP GET

### Available Endpoints

#### 1. ISS Current Location
```
GET http://api.open-notify.org/iss-now.json
```

Returns current ISS position.

**No parameters required.**

#### 2. People in Space
```
GET http://api.open-notify.org/astros.json
```

Returns list of people currently in space.

**No parameters required.**

#### 3. ISS Pass Times (DEPRECATED)
```
GET http://api.open-notify.org/iss-pass.json
```

**Note:** This endpoint has been turned off and no longer returns predictions.

---

## 📦 Response Format

### ISS Location Response

```json
{
  "iss_position": {
    "latitude": "45.1234",
    "longitude": "-122.5678"
  },
  "timestamp": 1640995200,
  "message": "success"
}
```

**Fields:**
- `iss_position.latitude` - Current latitude (-90 to 90)
- `iss_position.longitude` - Current longitude (-180 to 180)
- `timestamp` - Unix timestamp (seconds since epoch)
- `message` - "success" or error message

### Astronauts Response

```json
{
  "people": [
    {
      "craft": "ISS",
      "name": "Oleg Kononenko"
    },
    {
      "craft": "ISS",
      "name": "Nikolai Chub"
    },
    {
      "craft": "Tiangong",
      "name": "Tang Hongbo"
    }
  ],
  "number": 10,
  "message": "success"
}
```

**Fields:**
- `people` - Array of people in space
- `people[].name` - Astronaut/cosmonaut name
- `people[].craft` - Spacecraft (ISS, Tiangong, etc.)
- `number` - Total count
- `message` - "success" or error message

---

## ⚠️ Known Limitations

### API Constraints
1. **No historical data** - Only current/real-time data
2. **No ISS pass predictions** - Endpoint deprecated
3. **No filtering options** - Returns all data
4. **No pagination** - Complete results only

### Data Limitations
1. **Position updates** - Continuous but not precisely synchronized
2. **Astronaut data** - May lag behind actual changes by hours/days
3. **No altitude data** - Only lat/long provided
4. **No velocity data** - Speed/direction not included

---

## 💡 Best Practices

### Polling Frequency

**Recommended intervals:**
- **Real-time tracking:** 5-10 seconds
- **General updates:** 30-60 seconds
- **Low-frequency:** 5 minutes

**DON'T:**
- ❌ Poll every second (unnecessary load)
- ❌ Make parallel requests (one at a time sufficient)

### Caching
- Cache astronaut data for 6-24 hours
- Don't cache ISS location (changes constantly)

### Error Handling
Always check `message` field:
```python
if data['message'] == 'success':
    # Process data
else:
    # Handle error
```

---

## 🔍 Common Use Cases

### 1. Track ISS on a Map

```python
import requests
import time

url = "http://api.open-notify.org/iss-now.json"

while True:
    response = requests.get(url)
    data = response.json()

    if data['message'] == 'success':
        lat = data['iss_position']['latitude']
        lon = data['iss_position']['longitude']
        print(f"ISS is at {lat}, {lon}")

    time.sleep(10)  # Update every 10 seconds
```

### 2. Show People in Space

```python
import requests

url = "http://api.open-notify.org/astros.json"
response = requests.get(url)
data = response.json()

if data['message'] == 'success':
    print(f"{data['number']} people currently in space:\n")
    for person in data['people']:
        print(f"  {person['name']} ({person['craft']})")
```

### 3. Calculate Distance from Location

```python
import requests
from math import radians, sin, cos, sqrt, atan2

def haversine_distance(lat1, lon1, lat2, lon2):
    R = 6371  # Earth radius in km

    lat1, lon1, lat2, lon2 = map(radians, [lat1, lon1, lat2, lon2])
    dlat = lat2 - lat1
    dlon = lon2 - lon1

    a = sin(dlat/2)**2 + cos(lat1) * cos(lat2) * sin(dlon/2)**2
    c = 2 * atan2(sqrt(a), sqrt(1-a))

    return R * c

# Your location
my_lat, my_lon = 34.0522, -118.2437  # Los Angeles

# Get ISS location
response = requests.get("http://api.open-notify.org/iss-now.json")
data = response.json()

if data['message'] == 'success':
    iss_lat = float(data['iss_position']['latitude'])
    iss_lon = float(data['iss_position']['longitude'])

    distance = haversine_distance(my_lat, my_lon, iss_lat, iss_lon)
    print(f"ISS is {distance:.0f} km away")
```

### 4. ISS Overhead Notification

```python
import requests
from math import radians, sin, cos, sqrt, atan2
import time

def is_iss_overhead(my_lat, my_lon, threshold_km=500):
    response = requests.get("http://api.open-notify.org/iss-now.json")
    data = response.json()

    if data['message'] == 'success':
        iss_lat = float(data['iss_position']['latitude'])
        iss_lon = float(data['iss_position']['longitude'])

        # Calculate distance
        R = 6371
        lat1, lon1, lat2, lon2 = map(radians, [my_lat, my_lon, iss_lat, iss_lon])
        dlat = lat2 - lat1
        dlon = lon2 - lon1
        a = sin(dlat/2)**2 + cos(lat1) * cos(lat2) * sin(dlon/2)**2
        c = 2 * atan2(sqrt(a), sqrt(1-a))
        distance = R * c

        return distance < threshold_km
    return False

# Check every minute
my_lat, my_lon = 34.0522, -118.2437
while True:
    if is_iss_overhead(my_lat, my_lon):
        print("ISS is overhead!")
    time.sleep(60)
```

---

## 🆘 Support & Additional Resources

- **GitHub Issues:** https://github.com/open-notify/Open-Notify-API/issues
- **NASA ISS Tracker:** https://spotthestation.nasa.gov/
- **ISS Live Stream:** https://www.nasa.gov/multimedia/nasatv/iss_ustream.html
