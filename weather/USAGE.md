# WeatherAPI - Detailed Usage Guide

## 📖 Official Documentation

**Primary Documentation:** https://www.weatherapi.com/docs/

**Sign Up:** https://www.weatherapi.com/signup.aspx

**Pricing:** https://www.weatherapi.com/pricing.aspx

WeatherAPI provides real-time weather data for over 4 million locations worldwide with comprehensive current conditions, forecasts, historical data, and specialized weather information.

---

## 📊 Available Data

### Current Weather
- Temperature (Celsius and Fahrenheit)
- "Feels like" temperature
- Wind speed and direction
- Humidity and precipitation
- Atmospheric pressure
- Cloud cover percentage
- UV index
- Visibility distance
- Weather conditions and icons

### Forecast Weather
- Up to 14 days ahead (free tier: 3 days)
- Daily high/low temperatures
- Hourly forecasts
- Chance of rain/snow
- Sunrise and sunset times
- Moon phase information
- Astronomical data

### Historical Weather
- Past weather data (requires paid plan)
- Historical date range queries
- Temperature trends
- Precipitation history

### Air Quality Data
- Air Quality Index (AQI)
- PM2.5 and PM10 levels
- CO, NO2, SO2, O3 concentrations
- EPA and UK DEFRA standards

### Astronomy Data
- Sunrise and sunset times
- Moonrise and moonset times
- Moon phase and illumination
- Astronomical twilight times

### Marine & Tide Data (Paid plans)
- Wave height and direction
- Swell information
- Tide times
- Water temperature

### Location Data
- Location search and autocomplete
- Coordinates (latitude/longitude)
- Timezone information
- Region and country details

### Coverage
- **Locations:** 4+ million worldwide
- **Update Frequency:** Every 15 minutes for current weather
- **Forecast Range:** Up to 14 days (free: 3 days), future up to 365 days
- **Historical Data:** Available with paid plans
- **Languages:** 40+ languages supported

---

## 🔄 Operations & Methods

### HTTP Methods
- **GET** - All requests use HTTP GET

### Available Endpoints

#### Core Weather Endpoints

**`/current.json` or `/current.xml`** - Current weather conditions
- Real-time weather for any location
- Includes temperature, wind, precipitation, etc.

**`/forecast.json` or `/forecast.xml`** - Weather forecast
- Up to 14 days ahead (plan dependent)
- Daily and hourly forecasts
- Optional air quality and alerts

**`/history.json` or `/history.xml`** - Historical weather data
- Past weather data (requires paid plan)
- Specify date with `dt` parameter

**`/future.json` or `/future.xml`** - Future weather
- Up to 365 days ahead (requires paid plan)
- Long-range forecast planning

#### Location & Utility Endpoints

**`/search.json` or `/search.xml`** - Location search
- Autocomplete location search
- Returns matching locations with coordinates

**`/timezone.json` or `/timezone.xml`** - Timezone information
- Get timezone data for a location

**`/astronomy.json` or `/astronomy.xml`** - Astronomy data
- Sunrise, sunset, moonrise, moonset
- Moon phase and illumination

**`/marine.json` or `/marine.xml`** - Marine weather (Paid)
- Marine forecasts and tide data

**`/sports.json` or `/sports.xml`** - Sports weather (Paid)
- Weather optimized for sports events

**`/ip.json` or `/ip.xml`** - IP lookup
- Get location from IP address

---

## 🎛️ Request Options

### Authentication
**`key`** (required) - Your API key passed as query parameter

### Common Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `key` | API key (required) | `your_api_key_here` |
| `q` | Location query | `London`, `48.8567,2.3508`, `auto:ip`, `90210` |
| `days` | Forecast days (1-14) | `3`, `7`, `14` |
| `dt` | Specific date (YYYY-MM-DD) | `2025-01-15` |
| `aqi` | Include air quality (yes/no) | `yes` |
| `alerts` | Include weather alerts (yes/no) | `yes` |
| `lang` | Response language | `en`, `es`, `fr`, `de` |

### Location Query (`q`) Options

WeatherAPI supports multiple query formats:

| Format | Example | Description |
|--------|---------|-------------|
| City name | `London` | City name |
| Coordinates | `48.8567,2.3508` | Latitude,Longitude |
| IP address | `100.0.0.1` | Specific IP |
| Auto IP | `auto:ip` | Automatically detect from request IP |
| Postal code | `90210` | US/UK/Canada postal codes |
| Airport code | `IATA:JFK` | IATA airport code |
| Location ID | `id:2801268` | Internal location ID |

### Current Weather Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `key` | Yes | API key |
| `q` | Yes | Location query |
| `aqi` | No | Air quality (yes/no) |
| `lang` | No | Response language |

### Forecast Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `key` | Yes | API key |
| `q` | Yes | Location query |
| `days` | No | Number of days (1-14, default: 1) |
| `aqi` | No | Air quality data (yes/no) |
| `alerts` | No | Weather alerts (yes/no) |
| `hour` | No | Specific hour forecast (0-23) |
| `lang` | No | Response language |

### History Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `key` | Yes | API key |
| `q` | Yes | Location query |
| `dt` | Yes | Date (YYYY-MM-DD) |
| `hour` | No | Specific hour (0-23) |
| `lang` | No | Response language |

### Search Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `key` | Yes | API key |
| `q` | Yes | Search query (min 3 chars) |

---

## ⚡ Rate Limits & Quotas

### Free Tier
- 1 million API calls/month
- 3-day forecast
- Current weather
- Location search
- Astronomy data

### Paid Tiers

**Starter:** $9.99/month
- 1 million calls/month
- 3-day forecast
- All free features

**Developer:** $19.99/month
- 2 million calls/month
- 7-day forecast
- Air quality data

**Professional:** $49.99/month
- 5 million calls/month
- 14-day forecast
- Historical data
- Marine data

**Business:** $99.99/month
- 10 million calls/month
- All features
- Future weather (365 days)

**Rate Limiting:**
- HTTP 429 returned when quota exceeded
- Rate limits are per month
- No per-second rate limits

---

## 📦 Response Format

### Current Weather Response

```json
{
  "location": {
    "name": "London",
    "region": "City of London, Greater London",
    "country": "United Kingdom",
    "lat": 51.52,
    "lon": -0.11,
    "tz_id": "Europe/London",
    "localtime_epoch": 1706182800,
    "localtime": "2025-01-25 14:00"
  },
  "current": {
    "last_updated_epoch": 1706182500,
    "last_updated": "2025-01-25 13:55",
    "temp_c": 7.0,
    "temp_f": 44.6,
    "is_day": 1,
    "condition": {
      "text": "Partly cloudy",
      "icon": "//cdn.weatherapi.com/weather/64x64/day/116.png",
      "code": 1003
    },
    "wind_mph": 13.6,
    "wind_kph": 22.0,
    "wind_degree": 250,
    "wind_dir": "WSW",
    "pressure_mb": 1013.0,
    "pressure_in": 29.91,
    "precip_mm": 0.0,
    "precip_in": 0.0,
    "humidity": 71,
    "cloud": 50,
    "feelslike_c": 4.2,
    "feelslike_f": 39.5,
    "windchill_c": 4.2,
    "windchill_f": 39.5,
    "heatindex_c": 7.0,
    "heatindex_f": 44.6,
    "dewpoint_c": 2.0,
    "dewpoint_f": 35.6,
    "vis_km": 10.0,
    "vis_miles": 6.0,
    "uv": 2.0,
    "gust_mph": 17.2,
    "gust_kph": 27.7,
    "air_quality": {
      "co": 230.3,
      "no2": 15.8,
      "o3": 68.5,
      "so2": 3.6,
      "pm2_5": 4.8,
      "pm10": 6.2,
      "us-epa-index": 1,
      "gb-defra-index": 1
    }
  }
}
```

### Forecast Response

```json
{
  "location": {
    "name": "London",
    "region": "City of London, Greater London",
    "country": "United Kingdom",
    "lat": 51.52,
    "lon": -0.11,
    "tz_id": "Europe/London",
    "localtime_epoch": 1706182800,
    "localtime": "2025-01-25 14:00"
  },
  "current": {
    // Same as current weather response
  },
  "forecast": {
    "forecastday": [
      {
        "date": "2025-01-25",
        "date_epoch": 1706140800,
        "day": {
          "maxtemp_c": 9.0,
          "maxtemp_f": 48.2,
          "mintemp_c": 4.0,
          "mintemp_f": 39.2,
          "avgtemp_c": 6.5,
          "avgtemp_f": 43.7,
          "maxwind_mph": 15.0,
          "maxwind_kph": 24.1,
          "totalprecip_mm": 0.3,
          "totalprecip_in": 0.01,
          "totalsnow_cm": 0.0,
          "avgvis_km": 10.0,
          "avgvis_miles": 6.0,
          "avghumidity": 72,
          "daily_will_it_rain": 1,
          "daily_chance_of_rain": 65,
          "daily_will_it_snow": 0,
          "daily_chance_of_snow": 0,
          "condition": {
            "text": "Patchy rain possible",
            "icon": "//cdn.weatherapi.com/weather/64x64/day/176.png",
            "code": 1063
          },
          "uv": 2.0
        },
        "astro": {
          "sunrise": "07:47 AM",
          "sunset": "04:42 PM",
          "moonrise": "12:34 AM",
          "moonset": "11:28 AM",
          "moon_phase": "Waning Crescent",
          "moon_illumination": 18,
          "is_moon_up": 0,
          "is_sun_up": 1
        },
        "hour": [
          {
            "time_epoch": 1706140800,
            "time": "2025-01-25 00:00",
            "temp_c": 5.0,
            "temp_f": 41.0,
            "is_day": 0,
            "condition": {
              "text": "Clear",
              "icon": "//cdn.weatherapi.com/weather/64x64/night/113.png",
              "code": 1000
            },
            "wind_mph": 8.1,
            "wind_kph": 13.0,
            "wind_degree": 245,
            "wind_dir": "WSW",
            "pressure_mb": 1015.0,
            "pressure_in": 29.97,
            "precip_mm": 0.0,
            "precip_in": 0.0,
            "snow_cm": 0.0,
            "humidity": 75,
            "cloud": 10,
            "feelslike_c": 2.5,
            "feelslike_f": 36.5,
            "windchill_c": 2.5,
            "windchill_f": 36.5,
            "heatindex_c": 5.0,
            "heatindex_f": 41.0,
            "dewpoint_c": 1.0,
            "dewpoint_f": 33.8,
            "will_it_rain": 0,
            "chance_of_rain": 0,
            "will_it_snow": 0,
            "chance_of_snow": 0,
            "vis_km": 10.0,
            "vis_miles": 6.0,
            "gust_mph": 11.2,
            "gust_kph": 18.0,
            "uv": 1.0
          }
          // ... 23 more hourly entries
        ]
      }
      // ... more forecast days
    ]
  },
  "alerts": {
    "alert": [
      {
        "headline": "Flood Warning",
        "msgtype": "Alert",
        "severity": "Moderate",
        "urgency": "Expected",
        "areas": "London",
        "category": "Met",
        "certainty": "Likely",
        "event": "Flood Warning",
        "note": "Flooding is expected",
        "effective": "2025-01-25T14:00:00Z",
        "expires": "2025-01-26T14:00:00Z",
        "desc": "River levels are rising...",
        "instruction": "Avoid low-lying areas"
      }
    ]
  }
}
```

### Location Search Response

```json
[
  {
    "id": 2801268,
    "name": "London",
    "region": "City of London, Greater London",
    "country": "United Kingdom",
    "lat": 51.52,
    "lon": -0.11,
    "url": "london-city-of-london-greater-london-united-kingdom"
  },
  {
    "id": 315398,
    "name": "London",
    "region": "Ontario",
    "country": "Canada",
    "lat": 42.98,
    "lon": -81.25,
    "url": "london-ontario-canada"
  }
]
```

---

## 💡 Best Practices

### Efficient API Usage

**DO:**
- ✅ Use `auto:ip` for automatic location detection
- ✅ Cache forecast data for 15-30 minutes
- ✅ Use specific location IDs for faster queries
- ✅ Request only needed forecast days
- ✅ Enable compression (gzip) for faster transfers
- ✅ Use batch queries when possible

**DON'T:**
- ❌ Poll current weather more than every 10-15 minutes
- ❌ Request 14-day forecast if you only need 3 days
- ❌ Make redundant location search calls (cache results)
- ❌ Include AQI data if not needed (increases response size)

### Location Queries

**Best Practices:**
- Use coordinates for most accurate results
- Cache location IDs from search results
- Use postal codes for US/UK/Canada locations
- Auto IP detection works well for user's location
- Always handle multiple search results

### Error Handling

**Common Error Codes:**
- `1002` - API key not provided
- `1003` - Parameter 'q' not provided
- `1005` - Invalid API request URL
- `1006` - No location found matching parameter 'q'
- `2006` - API key provided is invalid
- `2007` - API key has exceeded calls per month quota
- `2008` - API key has been disabled
- `9000` - JSON body passed in bulk request is invalid

**Error Response Format:**
```json
{
  "error": {
    "code": 1006,
    "message": "No matching location found."
  }
}
```

---

## 🔍 Common Use Cases

### 1. Get Current Weather by City

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://api.weatherapi.com/v1/current.json"
params = {
    "key": api_key,
    "q": "London",
    "aqi": "yes"
}

response = requests.get(url, params=params)
data = response.json()

location = data['location']
current = data['current']

print(f"Weather in {location['name']}, {location['country']}:")
print(f"Temperature: {current['temp_c']}°C ({current['temp_f']}°F)")
print(f"Feels like: {current['feelslike_c']}°C")
print(f"Condition: {current['condition']['text']}")
print(f"Humidity: {current['humidity']}%")
print(f"Wind: {current['wind_kph']} kph {current['wind_dir']}")
print(f"UV Index: {current['uv']}")

if 'air_quality' in current:
    aqi = current['air_quality']
    print(f"Air Quality (US EPA): {aqi['us-epa-index']}")
```

### 2. Get 7-Day Forecast

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://api.weatherapi.com/v1/forecast.json"
params = {
    "key": api_key,
    "q": "New York",
    "days": 7,
    "aqi": "no",
    "alerts": "yes"
}

response = requests.get(url, params=params)
data = response.json()

print(f"7-Day Forecast for {data['location']['name']}:\n")

for day in data['forecast']['forecastday']:
    forecast = day['day']
    print(f"{day['date']}:")
    print(f"  High: {forecast['maxtemp_c']}°C / Low: {forecast['mintemp_c']}°C")
    print(f"  Condition: {forecast['condition']['text']}")
    print(f"  Rain chance: {forecast['daily_chance_of_rain']}%")
    print(f"  Sunrise: {day['astro']['sunrise']}")
    print(f"  Sunset: {day['astro']['sunset']}")
    print()

# Check for alerts
if 'alerts' in data and data['alerts']['alert']:
    print("⚠️ WEATHER ALERTS:")
    for alert in data['alerts']['alert']:
        print(f"  {alert['headline']}")
        print(f"  Severity: {alert['severity']}")
        print(f"  {alert['desc']}\n")
```

### 3. Get Hourly Forecast for Today

```python
import requests
from datetime import datetime

api_key = "YOUR_API_KEY"
url = "https://api.weatherapi.com/v1/forecast.json"
params = {
    "key": api_key,
    "q": "Paris",
    "days": 1,
    "aqi": "no",
    "alerts": "no"
}

response = requests.get(url, params=params)
data = response.json()

today = data['forecast']['forecastday'][0]

print(f"Hourly forecast for {data['location']['name']}:\n")

for hour in today['hour']:
    time = datetime.fromtimestamp(hour['time_epoch']).strftime('%H:%M')
    print(f"{time}: {hour['temp_c']}°C, {hour['condition']['text']}, "
          f"Rain: {hour['chance_of_rain']}%")
```

### 4. Auto-Detect User Location

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://api.weatherapi.com/v1/current.json"
params = {
    "key": api_key,
    "q": "auto:ip"  # Automatically detect from request IP
}

response = requests.get(url, params=params)
data = response.json()

location = data['location']
current = data['current']

print(f"Weather at your location ({location['name']}):")
print(f"Temperature: {current['temp_c']}°C")
print(f"Condition: {current['condition']['text']}")
```

### 5. Search for Locations

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://api.weatherapi.com/v1/search.json"
params = {
    "key": api_key,
    "q": "San"  # Search query
}

response = requests.get(url, params=params)
locations = response.json()

print("Locations matching 'San':\n")
for loc in locations[:10]:  # First 10 results
    print(f"{loc['name']}, {loc['region']}, {loc['country']}")
    print(f"  Coordinates: {loc['lat']}, {loc['lon']}")
    print(f"  ID: {loc['id']}")
    print()
```

### 6. Get Astronomy Data

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://api.weatherapi.com/v1/astronomy.json"
params = {
    "key": api_key,
    "q": "Tokyo",
    "dt": "2025-01-25"
}

response = requests.get(url, params=params)
data = response.json()

astro = data['astronomy']['astro']

print(f"Astronomy data for {data['location']['name']}:\n")
print(f"Sunrise: {astro['sunrise']}")
print(f"Sunset: {astro['sunset']}")
print(f"Moonrise: {astro['moonrise']}")
print(f"Moonset: {astro['moonset']}")
print(f"Moon phase: {astro['moon_phase']}")
print(f"Moon illumination: {astro['moon_illumination']}%")
```

### 7. Get Weather by Coordinates

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://api.weatherapi.com/v1/current.json"

# Coordinates for Sydney, Australia
params = {
    "key": api_key,
    "q": "-33.8688,151.2093"  # lat,lon format
}

response = requests.get(url, params=params)
data = response.json()

location = data['location']
current = data['current']

print(f"Weather at coordinates {location['lat']}, {location['lon']}:")
print(f"Location: {location['name']}, {location['country']}")
print(f"Temperature: {current['temp_c']}°C")
print(f"Local time: {location['localtime']}")
```

---

## 🆘 Support & Additional Resources

- **Documentation:** https://www.weatherapi.com/docs/
- **API Status:** https://www.weatherapi.com/api-status.aspx
- **Support:** https://www.weatherapi.com/contact.aspx
- **FAQ:** https://www.weatherapi.com/api-faq.aspx
- **Weather Condition Codes:** https://www.weatherapi.com/docs/weather_conditions.json
