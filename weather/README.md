# ⛅ WeatherAPI

## 📖 Description

Get real-time weather data from around the world! WeatherAPI provides comprehensive weather and geolocation data in JSON and XML formats. Access current conditions, accurate forecasts, historical weather data, and location-based weather intelligence for any application.

✨ **Best For:** Weather apps, smart home automation, travel planning, and agricultural monitoring

> 💡 **Did you know?** WeatherAPI provides weather data for over 4 million locations worldwide with updates every 15 minutes and forecasts up to 14 days ahead!

## 📍 Base URL

```
https://api.weatherapi.com/v1
```

## 🔑 Authentication

Requires an API key passed as the `key` parameter.

**Get your API key:** https://www.weatherapi.com/signup.aspx

---

## 💻 Example Usage

### Get Current Weather

**cURL:**
```bash
curl "https://api.weatherapi.com/v1/current.json?key=YOUR_API_KEY&q=London&aqi=no"
```

**Python:**
```python
import requests

api_key = "YOUR_API_KEY"
url = "https://api.weatherapi.com/v1/current.json"
params = {
    "key": api_key,
    "q": "London",
    "aqi": "no"
}

response = requests.get(url, params=params)
weather = response.json()

location = weather['location']
current = weather['current']

print(f"Location: {location['name']}, {location['country']}")
print(f"Temperature: {current['temp_c']}°C / {current['temp_f']}°F")
print(f"Condition: {current['condition']['text']}")
print(f"Wind: {current['wind_kph']} kph")
print(f"Humidity: {current['humidity']}%")
```

**JavaScript:**
```javascript
const apiKey = 'YOUR_API_KEY';
const url = 'https://api.weatherapi.com/v1/current.json';
const params = new URLSearchParams({
  key: apiKey,
  q: 'London',
  aqi: 'no'
});

fetch(`${url}?${params}`)
  .then(response => response.json())
  .then(data => {
    const location = data.location;
    const current = data.current;
    console.log(`Location: ${location.name}, ${location.country}`);
    console.log(`Temperature: ${current.temp_c}°C`);
    console.log(`Condition: ${current.condition.text}`);
    console.log(`Wind: ${current.wind_kph} kph`);
  })
  .catch(error => console.error('Error:', error));
```

### Get Weather Forecast

**cURL:**
```bash
curl "https://api.weatherapi.com/v1/forecast.json?key=YOUR_API_KEY&q=New%20York&days=3"
```

**Python:**
```python
import requests

api_key = "YOUR_API_KEY"
url = "https://api.weatherapi.com/v1/forecast.json"
params = {
    "key": api_key,
    "q": "New York",
    "days": 3
}

response = requests.get(url, params=params)
forecast = response.json()

print(f"3-Day Forecast for {forecast['location']['name']}:\n")
for day in forecast['forecast']['forecastday']:
    date = day['date']
    conditions = day['day']
    print(f"{date}:")
    print(f"  Max: {conditions['maxtemp_c']}°C / Min: {conditions['mintemp_c']}°C")
    print(f"  Condition: {conditions['condition']['text']}")
    print(f"  Chance of rain: {conditions['daily_chance_of_rain']}%")
    print()
```

**JavaScript:**
```javascript
const apiKey = 'YOUR_API_KEY';
const url = 'https://api.weatherapi.com/v1/forecast.json';
const params = new URLSearchParams({
  key: apiKey,
  q: 'Paris',
  days: 3
});

fetch(`${url}?${params}`)
  .then(response => response.json())
  .then(data => {
    console.log(`3-Day Forecast for ${data.location.name}:\n`);
    data.forecast.forecastday.forEach(day => {
      console.log(`${day.date}:`);
      console.log(`  Max: ${day.day.maxtemp_c}°C / Min: ${day.day.mintemp_c}°C`);
      console.log(`  ${day.day.condition.text}`);
      console.log();
    });
  })
  .catch(error => console.error('Error:', error));
```

### Search Locations

**cURL:**
```bash
curl "https://api.weatherapi.com/v1/search.json?key=YOUR_API_KEY&q=San"
```

**Python:**
```python
import requests

api_key = "YOUR_API_KEY"
url = "https://api.weatherapi.com/v1/search.json"
params = {
    "key": api_key,
    "q": "San"
}

response = requests.get(url, params=params)
locations = response.json()

for location in locations:
    print(f"{location['name']}, {location['region']}, {location['country']}")
```

---

## 🌐 Available Endpoints

- 🌡️ `/current.json` - Current weather conditions
- 📅 `/forecast.json` - Weather forecast (up to 14 days)
- 🔍 `/search.json` - Location search and autocomplete
- 📜 `/history.json` - Historical weather data
- 🕐 `/timezone.json` - Timezone information
- 🌙 `/astronomy.json` - Astronomy data (sunrise, sunset, moon phases)
- 🌊 `/marine.json` - Marine and tide data
- 🏃 `/sports.json` - Sports weather data
- 🌬️ `/future.json` - Future weather (up to 365 days)

> 💡 **Pro Tip:** You can query locations using city names, coordinates (lat,long), IP addresses, postal codes, or even airport codes (IATA)!

---

## 🔧 Common Parameters

- `key` (required) - Your API authentication key
- `q` - Location query (city name, coordinates, IP, postal code, airport code)
- `days` - Number of forecast days (1-14)
- `aqi` - Include air quality data (yes/no)
- `alerts` - Include weather alerts (yes/no)
- `dt` - Date for history/future queries (YYYY-MM-DD)

⚠️ **Note:** For complete parameter details, rate limits, and response formats, see [USAGE.md](./USAGE.md)

---

## 🚀 Application Examples

**1. Personal Weather Dashboard**
Display current conditions and forecasts for user's location.
- Auto-detect location via GPS or IP
- Current temperature and conditions
- Hourly and daily forecasts
- Weather alerts and warnings
- Customizable units (metric/imperial)
- Beautiful weather animations

**2. Smart Home Weather Integration**
Connect weather data to smart home automation.
- Adjust thermostat based on forecast
- Close smart blinds when sunny
- Activate sprinklers based on rain forecast
- Send alerts for severe weather
- Display on smart mirrors/displays
- Energy optimization recommendations

**3. Travel Weather Planner**
Help travelers check weather at destinations.
- Compare weather across multiple cities
- Best time to visit recommendations
- Packing suggestions based on forecast
- Multi-day itinerary weather view
- Historical weather patterns
- Travel delay alerts

**4. Agricultural Weather Monitor**
Support farmers with precision agriculture weather data.
- Field-level weather forecasts
- Frost warnings and alerts
- Optimal planting/harvesting windows
- Precipitation tracking
- Growing degree days calculator
- Pest risk predictions based on conditions

**5. Event Planning Weather Tool**
Help event planners make weather-informed decisions.
- Long-range forecast for event dates
- Backup date weather comparison
- Venue location-specific forecasts
- Severe weather probability
- Historical weather for date
- Vendor notification system

**6. Outdoor Activity Recommender**
Suggest activities based on current and forecast weather.
- Activity suitability scores
- Best times for running, cycling, hiking
- UV index and safety warnings
- Air quality considerations
- Wind conditions for sports
- Personalized recommendations

**7. Weather Alert & Emergency System**
Notify users of dangerous weather conditions.
- Real-time severe weather alerts
- Push notifications by location
- Hurricane/tornado tracking
- Flood warnings
- Multi-location monitoring
- Emergency contact integration

**8. Mobile Weather Widget**
Quick weather access on smartphones.
- Lock screen widget with current conditions
- Notification bar quick view
- Animated radar imagery
- Voice-activated weather queries
- Offline cached forecasts
- Battery-efficient updates

**9. Weather Data Visualization Platform**
Create charts and graphs from weather data.
- Temperature trend graphs
- Precipitation charts
- Wind rose diagrams
- Historical comparison visualizations
- Custom date range analysis
- Export data for research

**10. Climate Comparison Tool**
Compare weather patterns across locations and time periods.
- Side-by-side city comparisons
- Seasonal pattern analysis
- Average vs actual conditions
- Best climate match finder
- Relocation weather research
- Long-term trend visualization

---

## 📚 Resources

- 📖 **API Documentation:** https://www.weatherapi.com/docs/
- 🔑 **Sign Up:** https://www.weatherapi.com/signup.aspx
- 💰 **Pricing:** https://www.weatherapi.com/pricing.aspx
- 📘 **Detailed Usage Guide:** [USAGE.md](./USAGE.md)
