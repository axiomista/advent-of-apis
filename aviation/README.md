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

## Application Examples

**1. Flight Tracking Dashboard**
Real-time flight monitoring platform for travelers.
- Track multiple flights simultaneously
- Real-time status updates (scheduled, delayed, cancelled, landed)
- Display flight path on interactive map
- Show departure/arrival gates and terminals
- Display aircraft type and registration
- Historical on-time performance

**2. Airport Information Board**
Digital departure/arrival board for websites or apps.
- Live departures and arrivals for any airport
- Filter by airline or destination
- Show gate assignments and terminal info
- Display flight status with color coding
- Auto-refresh every 30-60 seconds
- Support for multiple airport locations

**3. Travel Booking Platform**
Integrate flight data into booking systems.
- Search flights by route and date
- Display available airlines and flight times
- Show historical pricing trends
- Compare flight durations and layovers
- Display aircraft amenities
- Show airport transfer times

**4. Flight Analytics Tool**
Business intelligence for aviation industry.
- Analyze on-time performance by airline/route
- Track seasonal flight patterns
- Calculate average delays by airport
- Visualize route profitability
- Monitor aircraft utilization rates
- Generate industry reports

**5. Travel Alert System**
Notify travelers of flight changes.
- Email/SMS notifications for flight status changes
- Alert for gate changes or delays
- Notify of cancellations
- Send departure reminders
- Multi-flight tracking for connections
- Customizable alert preferences

**6. Aviation Weather Correlator**
Link flight delays to weather conditions.
- Overlay weather data on flight paths
- Correlate delays with weather events
- Predict disruptions based on forecast
- Show alternate routes avoiding weather
- Display airport weather conditions
- Historical weather impact analysis

**7. Aircraft Spotting App**
Help aviation enthusiasts track and log aircraft.
- Search flights by aircraft registration
- Display aircraft type and age
- Log spotted aircraft with photos
- Track rare aircraft movements
- Share sightings with community
- Map of aircraft locations

**8. Corporate Flight Manager**
Business aviation management tool.
- Track company-related flights for employees
- Automated expense tracking
- Travel policy compliance checking
- Trip planning with preferred airlines
- Reporting for travel budgets
- Integration with corporate calendar

## Resources

- Documentation: https://aviationstack.com/documentation
- Sign up: https://aviationstack.com/signup
