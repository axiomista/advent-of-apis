# Advent of APIs

A curated collection of public APIs with examples and documentation. Each API directory contains a detailed README with descriptions, authentication requirements, and code examples in both cURL and multiple programming languages.

## Government & Civic Data

### [U.S. Census Bureau API](./census)
Access comprehensive demographic, economic, and geographic data about the United States including population statistics, economic indicators, and housing data.

### [Google Civic Information API](./civic)
Retrieve voting and election data including voter registration details, polling locations, ballot information, and election administration contacts.

### [LegiScan API](./legiscan)
Track legislation across all 50 U.S. states and Congress with access to bills, amendments, roll call votes, and real-time legislative activity.

### [National Parks Service API](./nps)
Authoritative data about U.S. national parks including alerts, activities, campgrounds, visitor centers, and events.

## Scientific & Environmental Data

### [USGS Water Services API](./water)
Real-time and historical water resource data including streamflow, gage height, groundwater levels, and water quality measurements.

### [USGS Earthquake Catalog API](./earthquake)
Query earthquake event data worldwide with parameters for location, magnitude, time ranges, and depth.

### [World Bank API](./worldbank)
Access development indicators across countries including GDP, population statistics, poverty data, and other World Development Indicators.

### [OpenFDA API](./openfda)
FDA public data including drug adverse events, device recalls, food enforcement actions, and other regulatory information.

### [WeatherAPI](./weather)
Current weather conditions, forecasts, historical data, and location-based weather intelligence.

## Transportation & Location

### [CityBikes API](./citybikes)
Real-time bike-sharing network data from cities worldwide including station locations, available bikes, and network details.

### [AviationStack API](./aviation)
Global aviation data with real-time flight status, historical data, schedules, and comprehensive information about airlines, airports, and aircraft.

## Space & Astronomy

### [Open Notify - ISS Location API](./iss)
Track the International Space Station with real-time location data, pass times for specific locations, and information about astronauts in space.

### [TLE Satellite API](./tle)
Two-Line Element (TLE) data for satellites and space objects used for tracking satellite positions and predicting orbital paths.

## API Summary

| API | Authentication | Base URL | Rate Limits |
|-----|---------------|----------|-------------|
| Census | API Key | `https://api.census.gov/data` | Varies by dataset |
| Civic Info | API Key | `https://www.googleapis.com/civicinfo/v2` | Google API limits |
| LegiScan | API Key | `https://api.legiscan.com/` | Varies by plan |
| NPS | API Key (header) | `https://developer.nps.gov/api/v1` | 1000 requests/hour |
| Water | None | `https://waterservices.usgs.gov/rest` | None specified |
| Earthquake | None | `https://earthquake.usgs.gov/fdsnws/event/1` | None specified |
| World Bank | None | `https://api.worldbank.org/v2` | None specified |
| OpenFDA | Optional | `https://api.fda.gov` | 240/min (1000/min with key) |
| Weather | API Key | `https://api.weatherapi.com/v1` | Varies by plan |
| CityBikes | None | `http://api.citybik.es/v2` | None specified |
| Aviation | API Key | `http://api.aviationstack.com/v1` | Varies by plan |
| ISS | None | `http://api.open-notify.org` | None specified |
| TLE | None | `https://tle.ivanstanojevic.me/api` | None specified |

## Getting Started

Each API directory contains:
- Detailed API description
- Authentication requirements
- Example usage with cURL
- Code examples in Python and JavaScript
- Common parameters and endpoints
- Links to official documentation

Browse the individual directories to explore each API and see working examples.

## Contributing

Feel free to add more APIs or improve existing documentation by submitting a pull request.

## License

This repository is for educational and reference purposes. Each API has its own terms of service and usage policies. Please review the official documentation for each API before use.
