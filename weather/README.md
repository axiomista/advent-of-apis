# WeatherAPI

## Description

WeatherAPI provides weather and geolocation data in JSON and XML formats. Access current weather conditions, forecasts, historical weather data, and location-based weather intelligence for applications and integrations.

## Base URL

```
https://api.weatherapi.com/v1
```

## Authentication

Requires an API key passed as the `key` parameter.

## Example Usage

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

## Common Endpoints

- `/current.json` - Current weather
- `/forecast.json` - Weather forecast (up to 14 days)
- `/search.json` - Location search
- `/history.json` - Historical weather data
- `/timezone.json` - Timezone information
- `/astronomy.json` - Astronomy data (sunrise, sunset, moon phases)

## Common Parameters

- `key` (required) - Your API authentication key
- `q` - Location query (city name, coordinates, IP address, postal code)
- `days` - Number of forecast days (1-14)
- `aqi` - Include air quality data (yes/no)
- `alerts` - Include weather alerts (yes/no)

## Resources

- Documentation: https://www.weatherapi.com/docs/
- Sign up: https://www.weatherapi.com/signup.aspx
