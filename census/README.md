# 📊 U.S. Census Bureau API

## 📖 Description

Access America's most authoritative demographic and economic data! The U.S. Census Bureau APIs provide comprehensive programmatic access to population statistics, economic indicators, housing data, and geographic information. Explore decades of census data from surveys including the Decennial Census, American Community Survey (ACS), Economic Census, and more.

✨ **Best For:** Demographics research, market analysis, policy planning, and social science applications

> 💡 **Did you know?** The Census Bureau collects data from over 130 surveys annually, providing the most detailed statistical portrait of the American people available anywhere!

## 📍 Base URL

```
https://api.census.gov/data
```

## 🔑 Authentication

Most Census APIs require an API key passed as the `key` parameter.

**Request your free API key:** https://api.census.gov/data/key_signup.html

---

## 💻 Example Usage

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

---

## 📂 Popular Datasets

- 🏛️ **Decennial Census** - Complete population count every 10 years
- 🏘️ **American Community Survey (ACS)** - Detailed demographic/economic data (1-year, 5-year estimates)
- 💼 **Economic Census** - Business and industry statistics
- 🏠 **Housing and Household Economic Statistics** - Housing and economic indicators
- 👶 **Population Estimates** - Annual population estimates between censuses

> 💡 **Pro Tip:** Use variable codes to request specific data fields. For example, `B01001_001E` is the total population estimate. Find variable codes in the Census API documentation!

---

## 🔧 Common Query Patterns

- `get` - Variables to retrieve (comma-separated)
- `for` - Geographic level (state, county, tract, etc.)
- `in` - Parent geography filter
- `key` - Your API key

⚠️ **Note:** For complete dataset listings, variable codes, geographic options, and best practices, see [USAGE.md](./USAGE.md)

---

## 🚀 Application Examples

**1. Demographic Research Dashboard**
Visualize and analyze population trends across geographic areas.
- Compare demographic changes over time
- Display population by age, gender, and ethnicity
- Interactive maps with color-coded data layers
- Generate custom reports and charts
- Export data for statistical analysis
- Side-by-side geographic comparisons

**2. Business Location Analyzer**
Help businesses choose optimal locations based on demographics.
- Analyze target customer demographics by area
- Display income levels and spending patterns
- Show education levels and employment data
- Compare multiple potential locations
- Calculate market size and opportunity
- Generate location recommendation reports

**3. Community Health Mapper**
Correlate health outcomes with demographic and socioeconomic data.
- Map health indicators by census tract
- Overlay income, education, and housing data
- Identify health disparities and gaps
- Track changes over time
- Generate equity reports
- Support public health planning decisions

**4. Housing Market Insights Tool**
Analyze housing trends and affordability metrics.
- Display housing costs relative to income
- Show vacancy rates and occupancy trends
- Compare rental vs ownership patterns
- Track new construction and growth areas
- Calculate affordability indices
- Forecast housing demand by area

---

## 📚 Resources

- 📖 **API Documentation:** https://www.census.gov/data/developers/data-sets.html
- 🔑 **API Key Signup:** https://api.census.gov/data/key_signup.html
- 📊 **Data Profiles:** https://www.census.gov/data/data-tools/data-profiles.html
- 🔍 **Variable Search:** https://api.census.gov/data.html
- 📘 **Detailed Usage Guide:** [USAGE.md](./USAGE.md)
