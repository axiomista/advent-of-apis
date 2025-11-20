# 🌐 World Bank API

## 📖 Description

Access comprehensive global development data spanning decades! The World Bank API provides economic, social, and environmental indicators for countries and regions worldwide. Track GDP, population trends, poverty levels, education metrics, health outcomes, and thousands of other development indicators to understand our changing world.

✨ **Best For:** Economic research, development analysis, policy research, and data journalism

> 💡 **Did you know?** The World Bank tracks over 1,400 development indicators across 200+ countries, with some data series extending back to 1960!

## 📍 Base URL

```
https://api.worldbank.org/v2
```

## 🔑 Authentication

No API key required! 🎉 Access all World Bank data freely and openly for any purpose.

---

## 💻 Example Usage

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

> 💡 **Pro Tip:** The API returns data as `[metadata, data]` - always access the data array at index `[1]`!

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

---

## 🔧 Common Parameters

- 📄 `format` - Response format: json, xml, jsonP (default: xml)
- 📅 `date` - Year (2020) or range (2020:2023)
- 📊 `per_page` - Results per page (default: 50, max: 32,500)
- 💰 `incomeLevel` - Filter by income level (LIC, MIC, HIC, etc.)

⚠️ **Note:** For detailed parameter information and advanced filtering, see [USAGE.md](./USAGE.md)

---

## 🎯 Popular Indicators

- 💵 `NY.GDP.MKTP.CD` - GDP (current US$)
- 👥 `SP.POP.TOTL` - Total population
- 📉 `SI.POV.DDAY` - Poverty headcount ratio
- 📚 `SE.ADT.LITR.ZS` - Adult literacy rate

---

## 🚀 Application Examples

**1. Global Development Dashboard**
Visualize development indicators across countries and time.
- Compare countries on multiple indicators
- Show trend lines over decades
- Rank countries by development metrics
- Display regional aggregates
- Generate country profiles with key statistics

**2. Economic Analysis Platform**
Support research on economic trends and policies.
- Track GDP, inflation, and trade data
- Compare economic indicators across countries
- Analyze correlations between variables
- Export data for econometric modeling
- Visualize business cycle patterns

**3. Poverty & Inequality Tracker**
Monitor progress on poverty reduction goals.
- Track poverty headcount ratios
- Display income inequality metrics (Gini coefficient)
- Show access to services (water, electricity, education)
- Compare poverty trends across regions
- Assess progress toward SDGs

**4. Health Data Explorer**
Analyze global health trends and outcomes.
- Monitor life expectancy and mortality rates
- Track disease prevalence
- Display vaccination coverage
- Show healthcare expenditure patterns
- Compare health outcomes by income level

**5. Education Analytics Tool**
Study education access, quality, and outcomes globally.
- Track enrollment rates by level
- Monitor literacy rates
- Display education expenditure
- Compare learning outcomes
- Analyze gender gaps in education

**6. Climate & Environment Monitor**
Track environmental indicators and climate-related data.
- Monitor CO2 emissions and energy use
- Track deforestation rates
- Display renewable energy adoption
- Show water stress indicators
- Analyze environmental sustainability trends

**7. Investment Research Platform**
Support international investment decisions.
- Compare economic fundamentals across countries
- Track ease of doing business indicators
- Monitor debt levels and fiscal health
- Display infrastructure quality metrics
- Analyze trade and FDI patterns

**8. Policy Impact Assessment Tool**
Evaluate effectiveness of development policies.
- Compare pre/post policy indicator changes
- Benchmark against peer countries
- Track multiple outcome indicators
- Generate policy briefs with data visualizations
- Support evidence-based policymaking

---

## 📚 Resources

- 📖 **API Documentation:** https://datahelpdesk.worldbank.org/knowledgebase/articles/898581
- 🔍 **Indicator Search:** https://data.worldbank.org/indicator
- 📘 **Detailed Usage Guide:** [USAGE.md](./USAGE.md)
