# AviationStack API - Detailed Usage Guide

## 📖 Official Documentation

**Primary Documentation:** https://aviationstack.com/documentation

**Sign Up:** https://aviationstack.com/signup

AviationStack provides comprehensive real-time and historical aviation data with updates within 30-60 seconds from source.

---

## 📊 Available Data

### Flight Data
- Real-time flight status
- Historical flight data
- Future flight schedules
- Airline delay statistics

### Airport Information
- 10,000+ airports worldwide
- Airport details and codes
- Operating hours
- Location data

### Airline Data
- 13,000 airlines
- Airline information
- Fleet details
- Route networks

### Aircraft Information
- 19,000+ aircraft registrations
- 300 aircraft types
- Technical specifications

### Coverage
- **Geographic:** 250+ countries
- **Cities:** 9,000+
- **Real-time updates:** 30-60 seconds
- **Historical data:** Varies by subscription

---

## 🔄 Operations & Methods

### HTTP Methods
- **GET** - All requests use HTTP GET

### Available Endpoints

**`/flights`** - Real-time and historical flight data
**`/airports`** - Airport information
**`/airlines`** - Airline details
**`/aircraft`** - Aircraft information
**`/aircraft_types`** - Aircraft type data
**`/aviation_taxes`** - Tax information
**`/cities`** - City data
**`/countries`** - Country information

---

## 🎛️ Request Options

### Authentication
**`access_key`** (required) - Your API key

### Flight Parameters

| Parameter | Description |
|-----------|-------------|
| `flight_iata` | IATA flight code (e.g., AA100) |
| `flight_icao` | ICAO flight code |
| `flight_date` | Flight date (YYYY-MM-DD) |
| `dep_iata` | Departure airport IATA |
| `arr_iata` | Arrival airport IATA |
| `airline_name` | Airline name |
| `airline_iata` | Airline IATA code |
| `airline_icao` | Airline ICAO code |
| `flight_status` | scheduled, active, landed, cancelled, incident, diverted |
| `limit` | Results per page (max 100) |
| `offset` | Pagination offset |

### Airport Parameters

| Parameter | Description |
|-----------|-------------|
| `search` | Search term |
| `airport_name` | Airport name |
| `iata_code` | IATA code |
| `icao_code` | ICAO code |
| `country_name` | Country filter |
| `city` | City filter |

---

## ⚡ Rate Limits & Quotas

### Free Tier
- 100 API calls/month
- Basic features only

### Paid Tiers
- Standard: 10,000 calls/month
- Professional: 100,000 calls/month
- Enterprise: Custom limits

**Rate Limiting:**
- HTTP 429 returned when exceeded
- Upgrade subscription for higher limits

---

## 📦 Response Format

### Flight Response

```json
{
  "pagination": {
    "limit": 100,
    "offset": 0,
    "count": 100,
    "total": 33876
  },
  "data": [
    {
      "flight_date": "2024-01-15",
      "flight_status": "active",
      "departure": {
        "airport": "Los Angeles International",
        "timezone": "America/Los_Angeles",
        "iata": "LAX",
        "icao": "KLAX",
        "terminal": "5",
        "gate": "52B",
        "delay": 13,
        "scheduled": "2024-01-15T10:00:00+00:00",
        "estimated": "2024-01-15T10:13:00+00:00",
        "actual": "2024-01-15T10:13:00+00:00",
        "estimated_runway": "2024-01-15T10:13:00+00:00",
        "actual_runway": "2024-01-15T10:13:00+00:00"
      },
      "arrival": {
        "airport": "John F Kennedy International",
        "timezone": "America/New_York",
        "iata": "JFK",
        "icao": "KJFK",
        "terminal": "4",
        "gate": "B32",
        "baggage": "7",
        "delay": null,
        "scheduled": "2024-01-15T18:30:00+00:00",
        "estimated": "2024-01-15T18:30:00+00:00"
      },
      "airline": {
        "name": "American Airlines",
        "iata": "AA",
        "icao": "AAL"
      },
      "flight": {
        "number": "100",
        "iata": "AA100",
        "icao": "AAL100",
        "codeshared": null
      },
      "aircraft": {
        "registration": "N12345",
        "iata": "B77W",
        "icao": "B77W",
        "icao24": "A12345"
      },
      "live": {
        "updated": "2024-01-15T14:23:00+00:00",
        "latitude": 39.5,
        "longitude": -98.35,
        "altitude": 11277.6,
        "direction": 78,
        "speed_horizontal": 894.348,
        "speed_vertical": 0,
        "is_ground": false
      }
    }
  ]
}
```

---

## 🔍 Common Use Cases

### 1. Track Specific Flight

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

for flight in flights['data']:
    print(f"Flight: {flight['flight']['iata']}")
    print(f"Status: {flight['flight_status']}")
    print(f"Departure: {flight['departure']['airport']}")
    print(f"Arrival: {flight['arrival']['airport']}")
```

### 2. Get All Flights from Airport

```python
import requests

api_key = "YOUR_API_KEY"
url = "http://api.aviationstack.com/v1/flights"

params = {
    "access_key": api_key,
    "dep_iata": "LAX",
    "limit": 100
}

response = requests.get(url, params=params)
flights = response.json()

print(f"Total flights from LAX: {flights['pagination']['total']}")
for flight in flights['data']:
    print(f"{flight['flight']['iata']} to {flight['arrival']['iata']}")
```

### 3. Find Airport Information

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
```

---

## 🆘 Support & Additional Resources

- **Documentation:** https://aviationstack.com/documentation
- **FAQ:** https://aviationstack.com/faq
- **Support:** support@aviationstack.com
