# World Bank API - Detailed Usage Guide

## 📖 Official Documentation

**Primary Documentation:** https://datahelpdesk.worldbank.org/knowledgebase/articles/898581

**Indicator Catalog:** https://data.worldbank.org/indicator

**Data Catalog:** https://datacatalog.worldbank.org/

The World Bank API provides free, open access to over 1,400 development indicators across 200+ countries and regions, with historical data spanning back to 1960 for many series.

---

## 📊 Available Data

### Data Categories

#### 1. **Indicators** 📈
Over 1,400 time-series indicators including:
- **Economic:** GDP, GNI, inflation, trade, FDI
- **Social:** Population, poverty, inequality, education, health
- **Environmental:** CO2 emissions, energy use, deforestation
- **Infrastructure:** Access to electricity, internet, roads
- **Financial:** Debt levels, aid flows, remittances

#### 2. **Countries & Regions** 🌍
- 217 countries
- Regional aggregates (Sub-Saharan Africa, East Asia, etc.)
- Income level groups (Low, Lower-middle, Upper-middle, High)
- Custom regions

#### 3. **Topics** 🏷️
21 high-level topics organizing indicators:
- Agriculture & Rural Development
- Economy & Growth
- Education
- Energy & Mining
- Environment
- Financial Sector
- Health
- Infrastructure
- Poverty
- And 12 more...

####4. **Lending Types** 💰
Classifications by World Bank lending eligibility:
- IDA (International Development Association)
- IBRD (International Bank for Reconstruction and Development)
- Blend

#### 5. **Income Levels** 💵
- HIC: High income
- UMC: Upper middle income
- LMC: Lower middle income
- LIC: Low income

### Update Frequency

- **Most indicators:** Annual updates
- **Some quarterly indicators:** Updated every 3 months
- **Release schedule:** Typically 3-12 months after period end
- **Data revisions:** Historical data may be revised as methodologies improve

### Historical Coverage

- **1960-present:** Many major indicators (GDP, population)
- **1990-present:** Most social indicators
- **2000-present:** Many specialized indicators
- **Coverage varies by country and indicator**

---

## 🔄 Operations & Methods

### HTTP Methods

- **GET** - All queries use HTTP GET
- Read-only API (no POST/PUT/DELETE)

### API Structure

All requests follow this pattern:
```
https://api.worldbank.org/v2/{resource}
```

### Available Resources

| Resource | Description | Example |
|----------|-------------|---------|
| `/countries` | Country and region metadata | `/v2/countries` |
| `/indicators` | Indicator metadata | `/v2/indicators` |
| `/country/{code}/indicators/{indicator}` | Indicator data for country | `/v2/country/usa/indicators/SP.POP.TOTL` |
| `/sources` | Data sources | `/v2/sources` |
| `/topics` | Topic classifications | `/v2/topics` |
| `/incomeLevels` | Income level groups | `/v2/incomeLevels` |
| `/lendingTypes` | Lending type classifications | `/v2/lendingTypes` |
| `/regions` | Regional classifications | `/v2/regions` |

---

## 🎛️ Request Options

### Format Parameter

**`format`** - Response format (default: xml)

| Format | Description |
|--------|-------------|
| `json` | JSON format (recommended) |
| `jsonP` | JSONP with callback |
| `xml` | XML format (default) |

**Usage:**
```
/v2/countries?format=json
```

Or use path notation:
```
/v2/countries.json
```

### Pagination

#### per_page Parameter

**`per_page`** - Results per page (default: 50, max: 32,500)

```
per_page=100
```

#### page Parameter

**`page`** - Page number (1-indexed)

```
page=2
```

**Example pagination:**
```
/v2/indicators?format=json&per_page=100&page=1
```

### Date Parameters

**`date`** - Filter by year or year range

**Formats:**
- Single year: `date=2020`
- Year range: `date=2015:2020`
- Multiple years: `date=2015;2017;2020`
- Most recent: `date=MRV` (Most Recent Value)
- Most recent N years: `date=MRV:5`

**Examples:**
```
date=2020
date=2015:2020
date=MRV:10
```

### Language Parameter

**`language`** - Response language

**Supported:**
- `en` - English (default)
- `es` - Spanish
- `fr` - French
- `ar` - Arabic
- `zh` - Chinese

```
language=es
```

### Gap Fill Parameter

**`gapfill`** - Fill missing years (default: N)

```
gapfill=Y
```

When enabled, returns entries for all years in range, using `null` for missing values.

### Multiple-Record Parameter

**`mrnev`** - Most Recent Non-Empty Value

```
mrnev=1
```

Returns only the most recent year with data for each country.

### Frequency Parameter

**`frequency`** - For indicators with quarterly or monthly data

```
frequency=Q  # Quarterly
frequency=M  # Monthly
```

---

## ⚡ Rate Limits & Quotas

### No Official Rate Limits

The World Bank API has **no published rate limits**, but best practices apply:

**Recommendations:**
- ✅ Use reasonable request rates
- ✅ Cache responses when appropriate
- ✅ Use `per_page` to reduce total requests
- ✅ Request only needed data (don't over-fetch)

### Bulk Downloads

For large datasets:
- Consider using World Bank's bulk download files
- Available at: https://datacatalog.worldbank.org/

---

## 📦 Response Format

### Standard JSON Response Structure

All JSON responses follow this pattern:

```json
[
  {
    "page": 1,
    "pages": 6,
    "per_page": 50,
    "total": 266
  },
  [
    // Data array
  ]
]
```

**Important:** Response is a 2-element array:
- `[0]` - Metadata object
- `[1]` - Data array

### Country Indicator Response Example

```json
[
  {
    "page": 1,
    "pages": 1,
    "per_page": 50,
    "total": 63
  },
  [
    {
      "indicator": {
        "id": "SP.POP.TOTL",
        "value": "Population, total"
      },
      "country": {
        "id": "US",
        "value": "United States"
      },
      "countryiso3code": "USA",
      "date": "2022",
      "value": 331893745,
      "unit": "",
      "obs_status": "",
      "decimal": 0
    },
    {
      "indicator": {
        "id": "SP.POP.TOTL",
        "value": "Population, total"
      },
      "country": {
        "id": "US",
        "value": "United States"
      },
      "countryiso3code": "USA",
      "date": "2021",
      "value": 331511512,
      "unit": "",
      "obs_status": "",
      "decimal": 0
    }
  ]
]
```

### Indicator Metadata Response

```json
[
  {
    "page": 1,
    "pages": 1,
    "per_page": 50,
    "total": 1
  },
  [
    {
      "id": "SP.POP.TOTL",
      "name": "Population, total",
      "unit": "",
      "source": {
        "id": "2",
        "value": "World Development Indicators"
      },
      "sourceNote": "Total population is based on the de facto definition...",
      "sourceOrganization": "Various sources compiled by World Bank",
      "topics": [
        {
          "id": "19",
          "value": "Climate Change"
        },
        {
          "id": "8",
          "value": "Health"
        }
      ]
    }
  ]
]
```

### Country List Response

```json
[
  {
    "page": 1,
    "pages": 1,
    "per_page": 300,
    "total": 296
  },
  [
    {
      "id": "ABW",
      "iso2Code": "AW",
      "name": "Aruba",
      "region": {
        "id": "LCN",
        "iso2code": "ZJ",
        "value": "Latin America & Caribbean"
      },
      "adminregion": {
        "id": "",
        "iso2code": "",
        "value": ""
      },
      "incomeLevel": {
        "id": "HIC",
        "iso2code": "XD",
        "value": "High income"
      },
      "lendingType": {
        "id": "LNX",
        "iso2code": "XX",
        "value": "Not classified"
      },
      "capitalCity": "Oranjestad",
      "longitude": "-70.0167",
      "latitude": "12.5167"
    }
  ]
]
```

---

## ⚠️ Known Limitations

### Data Quality & Completeness

1. **Missing data is common:**
   - Many countries lack data for certain indicators
   - Historical data availability varies
   - `null` values indicate no data

2. **Data lags:**
   - Most indicators are 1-2 years behind
   - Some specialized indicators may lag further

3. **Methodology changes:**
   - Historical data may be revised
   - Definitions may change over time
   - Check `sourceNote` for methodology

### API Constraints

1. **Maximum per_page: 32,500**
   - Can't retrieve more than 32,500 records in single request
   - Use pagination for larger datasets

2. **No full-text search:**
   - Must know indicator codes or browse catalogs
   - No fuzzy matching on indicator names

3. **Limited aggregation:**
   - Cannot compute aggregates via API
   - Must retrieve and compute client-side

4. **No time-series analysis:**
   - Cannot calculate growth rates, averages, etc. via API
   - Must perform calculations client-side

### Response Quirks

1. **Always returns array format:**
   - Even single results return `[metadata, [data]]`
   - Must always access `response[1]`

2. **XML is default:**
   - Must explicitly specify `format=json`
   - Or use `.json` notation

3. **Country codes vary:**
   - API uses ISO 3166-1 alpha-3 codes
   - Some aggregates use World Bank-specific codes (e.g., "WLD" for World)

---

## 💡 Best Practices

### Efficient Querying

**DO:**
- ✅ Request data for specific countries rather than "all"
- ✅ Use date ranges to limit data volume
- ✅ Use `per_page` to reduce number of requests
- ✅ Cache indicator metadata (changes infrequently)
- ✅ Use `MRV` for most recent values only

**DON'T:**
- ❌ Request all indicators for all countries
- ❌ Make identical requests repeatedly
- ❌ Ignore pagination metadata
- ❌ Assume data exists for all years

### Finding Indicator Codes

**Method 1: Browse online catalog**
```
https://data.worldbank.org/indicator
```

**Method 2: Search API**
```python
import requests

url = "https://api.worldbank.org/v2/indicators"
params = {"format": "json", "per_page": 20000}
response = requests.get(url, params=params)
indicators = response.json()[1]

# Search for keyword
keyword = "gdp"
matches = [i for i in indicators if keyword.lower() in i['name'].lower()]
for match in matches[:10]:
    print(f"{match['id']}: {match['name']}")
```

### Handling Missing Data

```python
# Always check for null values
value = record.get('value')
if value is not None:
    # Process value
    print(value)
else:
    print("No data available")
```

### Pagination Pattern

```python
import requests

def fetch_all_pages(url, params):
    all_data = []
    params['page'] = 1

    while True:
        response = requests.get(url, params=params)
        data = response.json()

        metadata = data[0]
        page_data = data[1]

        if not page_data:
            break

        all_data.extend(page_data)

        if metadata['page'] >= metadata['pages']:
            break

        params['page'] += 1

    return all_data
```

---

## 🔍 Common Use Cases

### 1. Get GDP for Multiple Countries

```python
import requests

countries = ["usa", "chn", "ind", "bra", "rus"]
indicator = "NY.GDP.MKTP.CD"  # GDP current US$

for country in countries:
    url = f"https://api.worldbank.org/v2/country/{country}/indicators/{indicator}"
    params = {
        "format": "json",
        "date": "2022",
        "per_page": 1
    }

    response = requests.get(url, params=params)
    data = response.json()

    if data[1]:
        record = data[1][0]
        print(f"{record['country']['value']}: ${record['value']:,.0f}")
```

### 2. Get Time Series for One Indicator

```python
import requests

url = "https://api.worldbank.org/v2/country/usa/indicators/SP.POP.TOTL"
params = {
    "format": "json",
    "date": "2010:2022",
    "per_page": 100
}

response = requests.get(url, params=params)
data = response.json()[1]

print("US Population 2010-2022:")
for record in sorted(data, key=lambda x: x['date']):
    if record['value']:
        print(f"{record['date']}: {record['value']:,}")
```

### 3. Compare Countries on Multiple Indicators

```python
import requests

countries = ["usa", "chn", "deu"]
indicators = [
    "NY.GDP.MKTP.CD",  # GDP
    "SP.POP.TOTL",     # Population
    "SE.XPD.TOTL.GD.ZS"  # Education expenditure % GDP
]

for country in countries:
    print(f"\n{country.upper()}:")
    for indicator in indicators:
        url = f"https://api.worldbank.org/v2/country/{country}/indicators/{indicator}"
        params = {"format": "json", "date": "MRV", "per_page": 1}

        response = requests.get(url, params=params)
        data = response.json()[1]

        if data and data[0]['value']:
            record = data[0]
            print(f"  {record['indicator']['value']}: {record['value']}")
```

### 4. Get All Countries by Income Level

```python
import requests

url = "https://api.worldbank.org/v2/country"
params = {
    "format": "json",
    "incomeLevel": "LIC",  # Low income countries
    "per_page": 300
}

response = requests.get(url, params=params)
countries = response.json()[1]

print("Low Income Countries:")
for country in countries:
    if country['capitalCity']:  # Filter out aggregates
        print(f"  {country['name']} ({country['id']})")
```

### 5. Search for Indicators by Topic

```python
import requests

# First, get topic ID
topics_url = "https://api.worldbank.org/v2/topics"
params = {"format": "json", "per_page": 50}
response = requests.get(topics_url, params=params)
topics = response.json()[1]

# Find health topic
health_topic = next(t for t in topics if "Health" in t['value'])
print(f"Health Topic ID: {health_topic['id']}")

# Get indicators for this topic
indicators_url = f"https://api.worldbank.org/v2/topic/{health_topic['id']}/indicators"
response = requests.get(indicators_url, params=params)
indicators = response.json()[1]

print("\nHealth Indicators:")
for ind in indicators[:10]:  # First 10
    print(f"  {ind['id']}: {ind['name']}")
```

### 6. Get Most Recent Value for All Countries

```python
import requests

indicator = "NY.GDP.PCAP.CD"  # GDP per capita
url = f"https://api.worldbank.org/v2/country/all/indicators/{indicator}"
params = {
    "format": "json",
    "date": "MRV",
    "per_page": 300
}

response = requests.get(url, params=params)
data = response.json()[1]

# Filter and sort
valid_data = [d for d in data if d['value'] is not None]
sorted_data = sorted(valid_data, key=lambda x: x['value'], reverse=True)

print("Top 10 Countries by GDP per Capita:")
for record in sorted_data[:10]:
    print(f"{record['country']['value']}: ${record['value']:,.2f}")
```

### 7. Compare Regional Aggregates

```python
import requests

regions = ["EAS", "ECS", "LCN", "MEA", "NAC", "SAS", "SSF"]  # Regional codes
indicator = "SP.POP.TOTL"

print("Population by World Bank Region (Most Recent):")
for region in regions:
    url = f"https://api.worldbank.org/v2/country/{region}/indicators/{indicator}"
    params = {"format": "json", "date": "MRV", "per_page": 1}

    response = requests.get(url, params=params)
    data = response.json()[1]

    if data and data[0]['value']:
        record = data[0]
        print(f"{record['country']['value']}: {record['value']:,}")
```

---

## 🆘 Support & Additional Resources

- **Help Desk:** https://datahelpdesk.worldbank.org/
- **Data Catalog:** https://datacatalog.worldbank.org/
- **Indicator Definitions:** https://databank.worldbank.org/metadataglossary/
- **API Documentation:** https://datahelpdesk.worldbank.org/knowledgebase/topics/125589
- **Developer Forum:** https://datahelpdesk.worldbank.org/knowledgebase/topics/125589
- **Bulk Downloads:** https://datacatalog.worldbank.org/
