# AviationStack API

## Description

AviationStack is a RESTful JSON-based API that delivers comprehensive global aviation data including real-time flight status, historical flight data, future schedules, and detailed information about airlines, airports, aircraft, routes, and more. Real-time data is updated within 30-60 seconds from the source.

## Base URL

```
http://api.aviationstack.com/v1
```

## Authentication

Requires an API key passed as the `access_key` parameter.

## Coverage

- 10,000+ airports
- 19,000 aircraft registrations
- 300 aircraft types
- 13,000 airlines
- 9,000 cities
- 250+ countries

## Example Usage

### Get Real-Time Flight Status

**cURL:**
```bash
curl "http://api.aviationstack.com/v1/flights?access_key=YOUR_API_KEY&flight_iata=AA100"
```

**Python:**
```python
import requests

api_key = "YOUR_API_KEY"
url = "http://api.aviationstack.com/v1/flights"
params = {
    "access_key": api_key,
    "flight_iata": "AA100"
}

response = requests.get(url, params=params)
flights = response.json()

if 'data' in flights and flights['data']:
    flight = flights['data'][0]
    print(f"Flight: {flight['flight']['iata']}")
    print(f"Status: {flight['flight_status']}")
    print(f"Departure: {flight['departure']['airport']} at {flight['departure']['scheduled']}")
    print(f"Arrival: {flight['arrival']['airport']} at {flight['arrival']['scheduled']}")
```

**JavaScript:**
```javascript
const apiKey = 'YOUR_API_KEY';
const url = 'http://api.aviationstack.com/v1/flights';
const params = new URLSearchParams({
  access_key: apiKey,
  flight_iata: 'AA100'
});

fetch(`${url}?${params}`)
  .then(response => response.json())
  .then(data => {
    if (data.data && data.data.length > 0) {
      const flight = data.data[0];
      console.log(`Flight: ${flight.flight.iata}`);
      console.log(`Status: ${flight.flight_status}`);
      console.log(`Departure: ${flight.departure.airport}`);
      console.log(`Arrival: ${flight.arrival.airport}`);
    }
  })
  .catch(error => console.error('Error:', error));
```

### Get Airport Information

**cURL:**
```bash
curl "http://api.aviationstack.com/v1/airports?access_key=YOUR_API_KEY&search=JFK"
```

**Python:**
```python
import requests

api_key = "YOUR_API_KEY"
url = "http://api.aviationstack.com/v1/airports"
params = {
    "access_key": api_key,
    "search": "JFK"
}

response = requests.get(url, params=params)
airports = response.json()

for airport in airports['data']:
    print(f"{airport['airport_name']} ({airport['iata_code']})")
    print(f"Location: {airport['city_name']}, {airport['country_name']}")
    print(f"Coordinates: {airport['latitude']}, {airport['longitude']}")
    print()
```

### Get Airline Information

**cURL:**
```bash
curl "http://api.aviationstack.com/v1/airlines?access_key=YOUR_API_KEY&airline_name=American"
```

**Python:**
```python
import requests

api_key = "YOUR_API_KEY"
url = "http://api.aviationstack.com/v1/airlines"
params = {
    "access_key": api_key,
    "airline_name": "American"
}

response = requests.get(url, params=params)
airlines = response.json()

for airline in airlines['data']:
    print(f"{airline['airline_name']} ({airline['iata_code']})")
    print(f"Country: {airline['country_name']}")
```

## Common Parameters

- `access_key` (required) - Your API authentication key
- `flight_iata` / `flight_icao` - Filter by specific flight
- `flight_date` - Retrieve historical flight data (YYYY-MM-DD)
- `airline_name` / `airline_iata` - Filter by airline
- `dep_iata` / `arr_iata` - Filter by departure/arrival airport

## Resources

- Documentation: https://aviationstack.com/documentation
- Sign up: https://aviationstack.com/signup
