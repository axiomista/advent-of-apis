# USGS Water Services API

## Description

The USGS Water Services API provides access to water resource data through REST APIs. It offers real-time and historical data for streamflow, gage height, groundwater levels, water quality samples, and various other water-related measurements from monitoring sites across the United States.

## Base URL

```
https://waterservices.usgs.gov/rest
```

## Available Services

- **Instantaneous Values (IV)** - Real-time measurements
- **Daily Values (DV)** - Historical daily data
- **Site Service** - Information about monitoring sites
- **Statistics** - Statistical summaries
- **Groundwater Levels** - Historical groundwater data
- **Water Quality** - Water quality samples and results

## Example Usage

### Get Real-Time Stream Data

**cURL:**
```bash
curl "https://waterservices.usgs.gov/nwis/iv/?format=json&sites=01646500&parameterCd=00060,00065&siteStatus=all"
```

**Python:**
```python
import requests

url = "https://waterservices.usgs.gov/nwis/iv/"
params = {
    "format": "json",
    "sites": "01646500",  # Potomac River site
    "parameterCd": "00060,00065",  # Discharge and gage height
    "siteStatus": "all"
}

response = requests.get(url, params=params)
data = response.json()
print(data)
```

**JavaScript:**
```javascript
const url = 'https://waterservices.usgs.gov/nwis/iv/';
const params = new URLSearchParams({
  format: 'json',
  sites: '01646500',
  parameterCd: '00060,00065',
  siteStatus: 'all'
});

fetch(`${url}?${params}`)
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```

### Search for Sites by State

**cURL:**
```bash
curl "https://waterservices.usgs.gov/nwis/site/?format=rdb&stateCd=va&siteType=ST&hasDataTypeCd=iv"
```

**Python:**
```python
import requests

url = "https://waterservices.usgs.gov/nwis/site/"
params = {
    "format": "json",
    "stateCd": "va",  # Virginia
    "siteType": "ST",  # Stream
    "hasDataTypeCd": "iv"  # Has instantaneous values
}

response = requests.get(url, params=params)
sites = response.json()
print(sites)
```

## Resources

- Documentation: https://waterservices.usgs.gov/
- Test Tools: https://waterservices.usgs.gov/test-tools/
- Parameter Codes: https://help.waterdata.usgs.gov/parameter_cd
