# World Bank API

## Description

The World Bank API provides access to development indicators and data across countries and regions. It delivers economic, social, and environmental metrics including GDP, population statistics, poverty data, and other World Development Indicators through structured API calls.

## Base URL

```
https://api.worldbank.org/v2
```

## Authentication

No API key required.

## Example Usage

### Get Countries by Income Level

**cURL:**
```bash
curl "https://api.worldbank.org/v2/country?incomeLevel=LIC&format=json"
```

**Python:**
```python
import requests

url = "https://api.worldbank.org/v2/country"
params = {
    "incomeLevel": "LIC",  # Low income countries
    "format": "json",
    "per_page": 100
}

response = requests.get(url, params=params)
data = response.json()

# World Bank API returns [metadata, data] array
countries = data[1] if len(data) > 1 else []
for country in countries:
    print(f"{country['name']} ({country['id']})")
```

**JavaScript:**
```javascript
const url = 'https://api.worldbank.org/v2/country';
const params = new URLSearchParams({
  incomeLevel: 'LIC',
  format: 'json',
  per_page: 100
});

fetch(`${url}?${params}`)
  .then(response => response.json())
  .then(data => {
    const countries = data[1];
    countries.forEach(country => {
      console.log(`${country.name} (${country.id})`);
    });
  })
  .catch(error => console.error('Error:', error));
```

### Get GDP Data for a Country

**cURL:**
```bash
curl "https://api.worldbank.org/v2/country/br/indicator/NY.GDP.MKTP.CD?date=2020:2023&format=json"
```

**Python:**
```python
import requests

# Get Brazil's GDP from 2020 to 2023
country = "br"  # Brazil
indicator = "NY.GDP.MKTP.CD"  # GDP (current US$)
url = f"https://api.worldbank.org/v2/country/{country}/indicator/{indicator}"

params = {
    "date": "2020:2023",
    "format": "json",
    "per_page": 100
}

response = requests.get(url, params=params)
data = response.json()

# Data is in second element of response array
indicator_data = data[1] if len(data) > 1 else []
for record in indicator_data:
    if record['value']:
        print(f"{record['date']}: ${record['value']:,.0f}")
```

**JavaScript:**
```javascript
const country = 'us';
const indicator = 'SP.POP.TOTL';  // Total population
const url = `https://api.worldbank.org/v2/country/${country}/indicator/${indicator}`;
const params = new URLSearchParams({
  date: '2020:2023',
  format: 'json',
  per_page: 100
});

fetch(`${url}?${params}`)
  .then(response => response.json())
  .then(data => {
    const indicators = data[1];
    indicators.forEach(record => {
      if (record.value) {
        console.log(`${record.date}: ${record.value.toLocaleString()}`);
      }
    });
  })
  .catch(error => console.error('Error:', error));
```

## Common Parameters

- `format` - Response format: json, xml, jsonP (default: xml)
- `date` - Year (2020) or range (2020:2023)
- `per_page` - Results per page (default: 50)
- `incomeLevel` - Filter by income level (LIC, MIC, HIC, etc.)

## Popular Indicators

- `NY.GDP.MKTP.CD` - GDP (current US$)
- `SP.POP.TOTL` - Total population
- `SI.POV.DDAY` - Poverty headcount ratio
- `SE.ADT.LITR.ZS` - Adult literacy rate

## Resources

- Documentation: https://datahelpdesk.worldbank.org/knowledgebase/articles/898581
- Indicator Search: https://data.worldbank.org/indicator
