# U.S. Census Bureau API

## Description

The U.S. Census Bureau provides APIs for accessing comprehensive demographic, economic, and geographic data about the United States. The APIs offer programmatic access to datasets including population statistics, economic indicators, housing data, and more from various census programs and surveys.

## Base URL

```
https://api.census.gov/data
```

## Authentication

Most Census APIs require an API key. Request one at: https://api.census.gov/data/key_signup.html

## Example Usage

### Get Population Data

**cURL:**
```bash
curl "https://api.census.gov/data/2021/acs/acs5?get=NAME,B01001_001E&for=state:*&key=YOUR_API_KEY"
```

**Python:**
```python
import requests

api_key = "YOUR_API_KEY"
url = "https://api.census.gov/data/2021/acs/acs5"
params = {
    "get": "NAME,B01001_001E",  # State name and total population
    "for": "state:*",
    "key": api_key
}

response = requests.get(url, params=params)
data = response.json()
print(data)
```

**JavaScript:**
```javascript
const apiKey = 'YOUR_API_KEY';
const url = `https://api.census.gov/data/2021/acs/acs5?get=NAME,B01001_001E&for=state:*&key=${apiKey}`;

fetch(url)
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```

## Resources

- Documentation: https://www.census.gov/data/developers/data-sets.html
- API Key Signup: https://api.census.gov/data/key_signup.html
