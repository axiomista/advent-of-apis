# U.S. Census Bureau API - Detailed Usage Guide

## 📖 Official Documentation

**Primary Documentation:** https://www.census.gov/data/developers/data-sets.html

**API Key Signup:** https://api.census.gov/data/key_signup.html

**Available APIs:** https://api.census.gov/data.html

**Variable Search:** https://api.census.gov/data/2021/acs/acs5/variables.html

The U.S. Census Bureau provides over 200 API endpoints covering demographic, economic, geographic, and housing data collected through various surveys and censuses.

---

## 📊 Available Data

### Major Datasets

#### Decennial Census
- Complete population count every 10 years
- Constitutional requirement
- 100% population coverage
- Years: 1790-2020
- Redistricting data (P.L. 94-171)
- Demographic and Housing Characteristics

#### American Community Survey (ACS)
- Most detailed demographic and economic data
- **ACS 1-Year:** Annual estimates for areas 65k+ population
- **ACS 5-Year:** Rolling 5-year estimates for all geographies
- **Variables:** 20,000+ data points
- **Topics:** Demographics, education, employment, income, housing, commuting

#### Population Estimates Program (PEP)
- Annual population estimates between censuses
- Age, sex, race, Hispanic origin
- Components of change (births, deaths, migration)
- Vintage datasets (e.g., 2022 vintage)

#### Economic Census
- Comprehensive business statistics every 5 years
- Industry revenue and employment
- Business patterns and establishments
- Years: 1997, 2002, 2007, 2012, 2017, 2022

#### Current Population Survey (CPS)
- Monthly survey of households
- Labor force statistics
- Poverty and income data
- Supplements: ASEC (Annual Social and Economic Supplement)

#### Economic Indicators
- County Business Patterns (CBP)
- Nonemployer Statistics (NES)
- Economic Indicators Time Series
- International Trade Data
- Construction statistics

### Data Categories

**Demographics:**
- Population counts
- Age and sex distributions
- Race and ethnicity
- Ancestry and language
- Disability status
- Veteran status

**Economics:**
- Income and earnings
- Poverty status
- Employment and unemployment
- Industry and occupation
- Commuting patterns
- Business revenue and payroll

**Housing:**
- Housing units and occupancy
- Home values and costs
- Rent and owner costs
- Housing characteristics
- Utilities and amenities

**Education:**
- Educational attainment
- School enrollment
- Field of degree
- Literacy

**Health:**
- Health insurance coverage
- Disability status

**Geography:**
- Boundaries and maps
- Geographic relationships
- Gazetteer files

### Coverage
- **Geographic Levels:** Nation, region, division, state, county, tract, block group, ZIP code, congressional district, metro area, and more
- **Time Span:** Historical data back to 1790 (varies by dataset)
- **Update Frequency:** Annual for most datasets
- **Detail Level:** Down to block-level for decennial census

---

## 🔄 Operations & Methods

### HTTP Methods
- **GET** - All requests use HTTP GET

### API URL Structure

```
https://api.census.gov/data/{year}/{dataset}/{table}?parameters
```

**Examples:**
- `https://api.census.gov/data/2021/acs/acs5` - ACS 5-year (2017-2021)
- `https://api.census.gov/data/2020/dec/pl` - 2020 Decennial Census P.L. 94-171
- `https://api.census.gov/data/2021/pep/population` - 2021 Population Estimates

### Common Endpoints

#### American Community Survey (ACS)

**ACS 5-Year Estimates:**
```
/data/{year}/acs/acs5
/data/{year}/acs/acs5/profile  # Data profiles
/data/{year}/acs/acs5/subject  # Subject tables
```

**ACS 1-Year Estimates:**
```
/data/{year}/acs/acs1
/data/{year}/acs/acs1/profile
/data/{year}/acs/acs1/subject
```

#### Decennial Census

**2020 Census:**
```
/data/2020/dec/pl              # P.L. 94-171 Redistricting
/data/2020/dec/dhc             # Demographic and Housing Characteristics
```

**2010 Census:**
```
/data/2010/dec/sf1             # Summary File 1
```

#### Population Estimates

```
/data/2022/pep/population      # Population estimates
/data/2022/pep/charagegroups   # Characteristics by age groups
/data/2022/pep/charracegroups  # Characteristics by race
```

#### Economic Data

```
/data/2021/cbp                 # County Business Patterns
/data/2017/ecnbasic            # Economic Census
/data/timeseries/eits/resconst # Construction statistics
```

---

## 🎛️ Request Options

### Authentication
**`key`** - API key (required for most endpoints)

### Query Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `get` | Variables to retrieve (comma-separated) | `NAME,B01001_001E` |
| `for` | Geographic level to query | `state:*`, `county:*` |
| `in` | Parent geography filter | `state:06` (California) |
| `key` | API key | `your_api_key_here` |

### Geographic Queries

#### Common Geography Levels

| Level | Syntax | Example |
|-------|--------|---------|
| Nation | `us:*` | `for=us:*` |
| Region | `region:*` | `for=region:*` |
| Division | `division:*` | `for=division:*` |
| State | `state:*` or `state:06` | `for=state:06` (CA) |
| County | `county:*` | `for=county:*&in=state:06` |
| Tract | `tract:*` | `for=tract:*&in=state:06+county:001` |
| Block Group | `block group:*` | `for=block group:*&in=state:06+county:001+tract:020100` |
| ZIP Code | `zip code tabulation area:*` | `for=zip code tabulation area:*` |
| Congressional District | `congressional district:*` | `for=congressional district:*&in=state:06` |
| Metro Area | `metropolitan statistical area/micropolitan statistical area:*` | `for=metropolitan statistical area/micropolitan statistical area:*` |

#### Geographic Wildcards

- `*` - All geographies at that level
- Specific code - Single geography (e.g., `state:06` for California)
- Multiple codes - Comma-separated (e.g., `state:06,36,48`)

### Variable Codes

Variables follow naming conventions:

**ACS Variables:**
- **Estimate:** `B01001_001E` (E = Estimate)
- **Margin of Error:** `B01001_001M` (M = Margin of Error)
- **Percent:** Some variables end in `_PE`
- **Percent MOE:** Some variables end in `_PM`

**Variable Naming:**
- `B` tables - Base tables
- `C` tables - Collapsed versions
- `S` tables - Subject tables
- `DP` tables - Data profile tables

**Common Variables:**
- `B01001_001E` - Total population
- `B19013_001E` - Median household income
- `B25077_001E` - Median home value
- `B23025_005E` - Unemployment count
- `B15003_022E` - Bachelor's degree holders

---

## ⚡ Rate Limits & Quotas

### API Key Limits
- **Without key:** 500 queries per IP per day
- **With key:** No published hard limit, but fair use expected
- No per-second rate limiting

### Best Practices
- Use an API key for production applications
- Cache results when possible
- Request only needed variables
- Use appropriate geographic resolution

---

## 📦 Response Format

### JSON Response Structure

**Example Request:**
```
https://api.census.gov/data/2021/acs/acs5?get=NAME,B01001_001E&for=state:*&key=YOUR_KEY
```

**Response:**
```json
[
  ["NAME", "B01001_001E", "state"],
  ["Alabama", "5039877", "01"],
  ["Alaska", "732673", "02"],
  ["Arizona", "7177881", "04"],
  ["Arkansas", "3025891", "05"],
  ["California", "39237836", "06"]
]
```

**Structure:**
- First row: Column headers
- Subsequent rows: Data values
- Last columns: Geographic identifiers (FIPS codes)

### Processing the Response

```python
import requests

url = "https://api.census.gov/data/2021/acs/acs5"
params = {
    "get": "NAME,B01001_001E,B19013_001E",
    "for": "state:*",
    "key": "YOUR_API_KEY"
}

response = requests.get(url, params=params)
data = response.json()

# Convert to list of dictionaries
headers = data[0]
rows = data[1:]

results = []
for row in rows:
    results.append(dict(zip(headers, row)))

for result in results[:5]:
    print(f"{result['NAME']}: Population {result['B01001_001E']}, "
          f"Median Income ${result['B19013_001E']}")
```

### Error Responses

```json
{
  "error": {
    "message": "error: missing required argument 'for'",
    "id": "missing-for"
  }
}
```

---

## 💡 Best Practices

### Efficient API Usage

**DO:**
- ✅ Use an API key for all production applications
- ✅ Request only the variables you need
- ✅ Use appropriate geographic levels (state vs tract)
- ✅ Cache data that doesn't change frequently
- ✅ Handle margins of error for ACS data
- ✅ Use 5-year ACS for small geographies
- ✅ Check variable availability in specific datasets

**DON'T:**
- ❌ Request all variables when you only need a few
- ❌ Query block-level data for entire states (too many results)
- ❌ Ignore margins of error in ACS estimates
- ❌ Use 1-year ACS for small population areas
- ❌ Assume variable codes are consistent across all datasets

### Understanding ACS Estimates

**Margins of Error:**
- ACS provides estimates with margins of error
- Always request both estimate (`_E`) and MOE (`_M`)
- Calculate 90% confidence intervals
- Be cautious with small populations (large MOEs)

**1-Year vs 5-Year:**
- **1-Year:** More current, but only for areas 65k+ population
- **5-Year:** Pooled 5 years of data, available for all geographies
- 5-year has smaller margins of error

### Variable Discovery

**Finding Variables:**
1. Browse variable list: `https://api.census.gov/data/2021/acs/acs5/variables.html`
2. Search data profiles: https://www.census.gov/data/data-tools/data-profiles.html
3. Use census.gov data tools to explore

**Reading Variable Names:**
- `B19013` = Median household income table
- `001E` = First variable (total/aggregate), estimate
- Explore full table with `.../variables.html`

---

## 🔍 Common Use Cases

### 1. Get Population by State

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://api.census.gov/data/2021/acs/acs5"
params = {
    "get": "NAME,B01001_001E",
    "for": "state:*",
    "key": api_key
}

response = requests.get(url, params=params)
data = response.json()

headers = data[0]
rows = data[1:]

print("State Populations (2021 ACS 5-Year):\n")
for row in sorted(rows, key=lambda x: int(x[1]), reverse=True)[:10]:
    state_name = row[0]
    population = int(row[1])
    print(f"{state_name}: {population:,}")
```

### 2. Get County Demographics with Income

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://api.census.gov/data/2021/acs/acs5"
params = {
    "get": "NAME,B01001_001E,B19013_001E,B25077_001E",  # Pop, income, home value
    "for": "county:*",
    "in": "state:06",  # California
    "key": api_key
}

response = requests.get(url, params=params)
data = response.json()

headers = data[0]
rows = data[1:]

print("California Counties:\n")
for row in rows[:10]:
    county_data = dict(zip(headers, row))
    print(f"{county_data['NAME']}")
    print(f"  Population: {int(county_data['B01001_001E']):,}")
    print(f"  Median Income: ${int(county_data['B19013_001E']):,}")
    print(f"  Median Home Value: ${int(county_data['B25077_001E']):,}")
    print()
```

### 3. Get Census Tract Data with Margins of Error

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://api.census.gov/data/2021/acs/acs5"

# Get data for San Francisco County (FIPS 075)
params = {
    "get": "NAME,B01001_001E,B01001_001M,B19013_001E,B19013_001M",
    "for": "tract:*",
    "in": "state:06+county:075",
    "key": api_key
}

response = requests.get(url, params=params)
data = response.json()

headers = data[0]
rows = data[1:]

print("San Francisco Census Tracts:\n")
for row in rows[:5]:
    tract_data = dict(zip(headers, row))

    pop = int(tract_data['B01001_001E'])
    pop_moe = int(tract_data['B01001_001M'])

    income = int(tract_data['B19013_001E'])
    income_moe = int(tract_data['B19013_001M'])

    print(f"{tract_data['NAME']}")
    print(f"  Population: {pop:,} (±{pop_moe:,})")
    print(f"  Median Income: ${income:,} (±${income_moe:,})")
    print()
```

### 4. Compare States Over Time

```python
import requests

api_key = "YOUR_API_KEY"

states = ["06", "36", "48"]  # CA, NY, TX
years = [2019, 2020, 2021]

results = {}

for year in years:
    url = f"https://api.census.gov/data/{year}/acs/acs5"
    params = {
        "get": "NAME,B01001_001E",
        "for": f"state:{','.join(states)}",
        "key": api_key
    }

    response = requests.get(url, params=params)
    data = response.json()

    for row in data[1:]:
        state_name = row[0]
        population = int(row[1])

        if state_name not in results:
            results[state_name] = {}
        results[state_name][year] = population

print("Population Trends:\n")
for state, years_data in results.items():
    print(f"{state}:")
    for year, pop in sorted(years_data.items()):
        print(f"  {year}: {pop:,}")
    print()
```

### 5. Get ZIP Code Demographics

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://api.census.gov/data/2021/acs/acs5"

# Get data for specific ZIP codes
params = {
    "get": "NAME,B01001_001E,B19013_001E,B15003_022E",
    "for": "zip code tabulation area:94102,94103,94104",  # SF ZIPs
    "key": api_key
}

response = requests.get(url, params=params)
data = response.json()

headers = data[0]
rows = data[1:]

print("ZIP Code Demographics:\n")
for row in rows:
    zip_data = dict(zip(headers, row))

    print(f"ZCTA {zip_data['zip code tabulation area']}")
    print(f"  Population: {int(zip_data['B01001_001E']):,}")
    print(f"  Median Income: ${int(zip_data['B19013_001E']):,}")
    print(f"  Bachelor's Degree+: {int(zip_data['B15003_022E']):,}")
    print()
```

### 6. Get Detailed Age Distribution

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://api.census.gov/data/2021/acs/acs5"

# Age variables: B01001_003E through B01001_025E (male), B01001_027E through B01001_049E (female)
age_vars = [
    "B01001_003E",  # Male under 5
    "B01001_004E",  # Male 5-9
    "B01001_005E",  # Male 10-14
    "B01001_027E",  # Female under 5
    "B01001_028E",  # Female 5-9
    "B01001_029E",  # Female 10-14
]

params = {
    "get": f"NAME,{','.join(age_vars)}",
    "for": "state:06",  # California
    "key": api_key
}

response = requests.get(url, params=params)
data = response.json()

headers = data[0]
row = data[1]

result = dict(zip(headers, row))

print(f"Age Distribution for {result['NAME']}:\n")
print(f"Male under 5: {int(result['B01001_003E']):,}")
print(f"Male 5-9: {int(result['B01001_004E']):,}")
print(f"Male 10-14: {int(result['B01001_005E']):,}")
print(f"Female under 5: {int(result['B01001_027E']):,}")
print(f"Female 5-9: {int(result['B01001_028E']):,}")
print(f"Female 10-14: {int(result['B01001_029E']):,}")
```

### 7. Get 2020 Decennial Census Redistricting Data

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://api.census.gov/data/2020/dec/pl"

params = {
    "get": "NAME,P1_001N,P2_002N,P2_005N",  # Total pop, Hispanic, Not Hispanic
    "for": "state:*",
    "key": api_key
}

response = requests.get(url, params=params)
data = response.json()

headers = data[0]
rows = data[1:]

print("2020 Census Populations:\n")
for row in sorted(rows, key=lambda x: int(x[1]), reverse=True)[:10]:
    state_data = dict(zip(headers, row))

    total = int(state_data['P1_001N'])
    hispanic = int(state_data['P2_002N'])
    non_hispanic = int(state_data['P2_005N'])

    print(f"{state_data['NAME']}")
    print(f"  Total: {total:,}")
    print(f"  Hispanic: {hispanic:,} ({100*hispanic/total:.1f}%)")
    print(f"  Non-Hispanic: {non_hispanic:,} ({100*non_hispanic/total:.1f}%)")
    print()
```

---

## 🆘 Support & Additional Resources

- **Documentation:** https://www.census.gov/data/developers/data-sets.html
- **API Key Signup:** https://api.census.gov/data/key_signup.html
- **Available APIs:** https://api.census.gov/data.html
- **User Guide:** https://www.census.gov/data/developers/guidance.html
- **Examples:** https://www.census.gov/data/developers/guidance/api-user-guide/query-examples.html
- **Data Profiles:** https://www.census.gov/data/data-tools/data-profiles.html
- **Geographic Entities:** https://www.census.gov/programs-surveys/geography/guidance/geo-identifiers.html

### Finding Variables
- **Variable Lists:** Each dataset has a `/variables.html` page
  - Example: https://api.census.gov/data/2021/acs/acs5/variables.html
- **Subject Tables:** Search by topic at https://data.census.gov/
- **API Discovery Tool:** https://api.census.gov/data.html

### FIPS Codes
- **State FIPS:** https://www.census.gov/library/reference/code-lists/ansi.html
- **County FIPS:** https://www.census.gov/geographies/reference-files/2021/demo/popest/2021-fips.html
