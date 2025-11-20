# OpenFDA API - Detailed Usage Guide

## 📖 Official Documentation

**Primary Documentation:** https://open.fda.gov/apis/

**Interactive API Explorer:** https://open.fda.gov/apis/try-the-api/

OpenFDA provides access to a wealth of FDA public data, harmonized into a consistent format with powerful search capabilities. The API covers multiple product categories and information types.

---

## 📊 Available Data

### Data Categories

OpenFDA organizes data into three main categories:

#### 1. **Drugs** 💊
- **Adverse Events** - FAERS (FDA Adverse Event Reporting System) data
- **Product Labeling** - Drug labels (SPL format)
- **NDC Directory** - National Drug Code directory
- **Recall Enforcement Reports** - Drug recalls and enforcement actions
- **Drugs@FDA** - FDA-approved drug products

#### 2. **Devices** 🏥
- **Adverse Events** - Device adverse event reports (MAUDE)
- **Classification** - Device classifications and regulations
- **510(k) Clearances** - Premarket notifications
- **PMA Approvals** - Premarket approval applications
- **Recalls** - Device recall information
- **Registrations & Listings** - Facility registrations
- **Recall Enforcement Reports** - Device enforcement actions
- **Adverse Event Reports** - Device problem reports
- **UDI** - Unique Device Identifier data

#### 3. **Food** 🍎
- **Enforcement Reports** - Food recalls and enforcement actions
- **Adverse Events** - Food adverse event reports (CFSAN)

### Update Frequency

- **Adverse Events:** Updated weekly (typically Sunday nights)
- **Recalls/Enforcement:** Updated weekly
- **Drug Labels:** Updated as labels are submitted/revised
- **510(k)/PMA:** Updated monthly
- **Device Registrations:** Updated quarterly

### Data Coverage

- **Adverse Events:**
  - Drugs: 1968-present (complete from 2004+)
  - Devices: 1990-present
  - Food: 2004-present
- **Recalls:** 2012-present (most categories)
- **Drug Labels:** Current approved labels

---

## 🔄 Operations & Methods

### HTTP Methods

- **GET** - All queries use HTTP GET requests
- **POST** - Not supported

### Endpoint Structure

All endpoints follow this pattern:
```
https://api.fda.gov/{category}/{dataset}.json
```

### Available Endpoints

#### Drug Endpoints

| Endpoint | Description |
|----------|-------------|
| `/drug/event.json` | Adverse event reports for drugs |
| `/drug/label.json` | Drug product labeling |
| `/drug/ndc.json` | National Drug Code directory |
| `/drug/enforcement.json` | Drug recall enforcement reports |
| `/drug/drugsfda.json` | FDA-approved drug products |

#### Device Endpoints

| Endpoint | Description |
|----------|-------------|
| `/device/event.json` | Medical device adverse events |
| `/device/classification.json` | Device classification |
| `/device/510k.json` | 510(k) premarket notifications |
| `/device/pma.json` | Premarket approval applications |
| `/device/recall.json` | Device recall information |
| `/device/enforcement.json` | Device enforcement reports |
| `/device/registrationlisting.json` | Facility registrations |
| `/device/udi.json` | Unique Device Identifiers |

#### Food Endpoints

| Endpoint | Description |
|----------|-------------|
| `/food/enforcement.json` | Food recall enforcement reports |
| `/food/event.json` | Food adverse event reports |

---

## 🎛️ Request Options

### Query Parameters

#### Search Parameter

**`search`** - Query using field:term syntax

**Syntax:**
```
field:term
```

**Examples:**
```
patient.drug.medicinalproduct:"aspirin"
serious:1
receivedate:[20200101+TO+20201231]
```

#### Operators

| Operator | Function | Example |
|----------|----------|---------|
| `:` | Exact match | `serious:1` |
| `+AND+` | Logical AND | `aspirin+AND+serious:1` |
| `+OR+` | Logical OR | `(aspirin+OR+ibuprofen)` |
| `[value1+TO+value2]` | Range | `receivedate:[20200101+TO+20201231]` |
| `*` | Wildcard | `patient.drug.medicinalproduct:aspir*` |
| `_exists_` | Field exists | `_exists_:patient.drug.drugindication` |
| `_missing_` | Field missing | `_missing_:patient.drug.drugindication` |

**Parentheses** for grouping:
```
(aspirin+OR+ibuprofen)+AND+serious:2
```

#### Count Parameter

**`count`** - Aggregate results by field

**Syntax:**
```
count=field_name
```

Returns frequency counts for unique values of the specified field.

**Example:**
```bash
curl "https://api.fda.gov/drug/event.json?count=patient.drug.medicinalproduct.exact"
```

**Best practices:**
- Use `.exact` suffix for term fields to get accurate counts
- Can be combined with `search` to count within filtered results

#### Limit Parameter

**`limit`** - Maximum number of results (default: 1, max: 1000)

**Example:**
```
limit=100
```

#### Skip Parameter

**`skip`** - Pagination offset

**Example:**
```
skip=100&limit=100  # Get results 101-200
```

#### Sort Parameter

**`sort`** - Sort results by field

**Syntax:**
```
sort=field:direction
```

**Directions:**
- `asc` - Ascending
- `desc` - Descending

**Example:**
```
sort=receivedate:desc
```

### Authentication Parameter

**`api_key`** - Your API key (optional but recommended)

**Example:**
```
api_key=YOUR_API_KEY_HERE
```

---

## ⚡ Rate Limits & Quotas

### Without API Key

- **40 requests per minute**
- **1,000 requests per day**
- Per IP address

### With API Key (Free)

- **1,000 requests per minute**
- **120,000 requests per day**
- Per API key

### Exceeding Limits

**HTTP 429** - Too Many Requests

Response includes:
```json
{
  "error": {
    "code": "OVER_RATE_LIMIT",
    "message": "API rate limit exceeded"
  }
}
```

### Getting an API Key

1. Visit: https://open.fda.gov/apis/authentication/
2. Fill out the simple form
3. Receive API key via email instantly
4. Add to queries: `?api_key=YOUR_KEY`

---

## 📦 Response Format

### Standard Response Structure

All successful responses follow this structure:

```json
{
  "meta": {
    "disclaimer": "Do not rely on openFDA to make decisions...",
    "terms": "https://open.fda.gov/terms/",
    "license": "https://open.fda.gov/license/",
    "last_updated": "2024-01-15",
    "results": {
      "skip": 0,
      "limit": 1,
      "total": 12453678
    }
  },
  "results": [
    {
      // Result objects vary by endpoint
    }
  ]
}
```

### Drug Adverse Event Response Example

```json
{
  "meta": {
    "results": {
      "skip": 0,
      "limit": 1,
      "total": 9876543
    }
  },
  "results": [
    {
      "safetyreportid": "12345678",
      "receivedate": "20240115",
      "serious": 1,
      "seriousnessdeath": 0,
      "seriousnesslifethreatening": 0,
      "seriousnesshospitalization": 1,
      "patient": {
        "patientonsetage": 65,
        "patientonsetageunit": "801",
        "patientsex": "2",
        "drug": [
          {
            "medicinalproduct": "ASPIRIN",
            "drugcharacterization": "1",
            "drugindication": "PROPHYLACTIC",
            "actiondrug": "1"
          }
        ],
        "reaction": [
          {
            "reactionmeddrapt": "NAUSEA",
            "reactionoutcome": "1"
          }
        ]
      }
    }
  ]
}
```

### Count Response Example

```json
{
  "meta": {
    "results": {
      "skip": 0,
      "limit": 1000,
      "total": 15678
    }
  },
  "results": [
    {
      "term": "ASPIRIN",
      "count": 123456
    },
    {
      "term": "IBUPROFEN",
      "count": 98765
    }
  ]
}
```

### Device Recall Response Example

```json
{
  "results": [
    {
      "country": "US",
      "city": "City Name",
      "state": "CA",
      "reason_for_recall": "Product defect description",
      "status": "Ongoing",
      "product_description": "Device name and model",
      "code_info": "Lot numbers affected",
      "recall_initiation_date": "20240101",
      "center_classification_date": "20240105",
      "report_date": "20240110",
      "classification": "Class II",
      "product_quantity": "1000 units",
      "event_id": "12345"
    }
  ]
}
```

---

## ⚠️ Known Limitations

### Data Quality & Completeness

1. **Voluntary reporting:**
   - Adverse events are voluntarily reported and may not represent true incidence
   - Underreporting is common
   - Cannot establish causal relationships

2. **Variable data quality:**
   - Reporter type affects detail level
   - Missing fields are common
   - Some reports lack key information

3. **Duplicate reports:**
   - Same event may be reported by multiple sources
   - Check `safetyreportid` and `duplicate` fields

### Query Constraints

1. **Maximum 1,000 results per query:**
   - Use pagination for larger datasets
   - Consider using `count` for aggregations instead

2. **No wildcard-only searches:**
   - Cannot search for `*:*`
   - Must specify at least one constraint

3. **Limited date format:**
   - Dates in format YYYYMMDD
   - No time component in most fields

4. **Case sensitivity:**
   - Field names are case-sensitive
   - Search values depend on field (use `.exact` for case-sensitive term matching)

### Field-Specific Limitations

1. **Patient age:**
   - Not always present
   - Format varies (years, months, days)
   - Check `patientonsetageunit` code

2. **Drug names:**
   - May be brand or generic
   - Spelling variations exist
   - Use `.exact` for precise matching or partial matching for flexibility

3. **Dates:**
   - `receivedate` is FDA receipt, not event occurrence
   - Some date fields may be estimated

---

## 💡 Best Practices

### Query Efficiency

**DO:**
- ✅ Use specific field searches instead of broad queries
- ✅ Add date ranges to constrain results
- ✅ Use `count` for trend analysis instead of retrieving all records
- ✅ Request only needed records with appropriate `limit`
- ✅ Use `.exact` suffix when you need precise term matching

**DON'T:**
- ❌ Query without any filters (too broad)
- ❌ Make identical queries repeatedly (cache results)
- ❌ Use wildcards at the start of terms (slow)
- ❌ Request limit=1000 when you only need 10 results

### Field Selection Strategies

**For aggregations:**
```
count=patient.drug.medicinalproduct.exact
```

**For time series:**
```
count=receivedate
```

**For categorical analysis:**
```
count=patient.patientsex
```

### Handling Missing Data

Many fields are optional. Always check for existence:

```python
# Python example
drug_name = result.get('patient', {}).get('drug', [{}])[0].get('medicinalproduct', 'Unknown')
```

### Pagination Pattern

```python
import requests
import time

def fetch_all_results(search_query, max_results=10000):
    url = "https://api.fda.gov/drug/event.json"
    all_results = []
    skip = 0
    limit = 1000

    while skip < max_results:
        params = {
            "search": search_query,
            "limit": limit,
            "skip": skip,
            "api_key": "YOUR_KEY"
        }

        response = requests.get(url, params=params)

        if response.status_code == 429:
            time.sleep(60)  # Wait if rate limited
            continue

        data = response.json()

        if 'results' not in data or not data['results']:
            break

        all_results.extend(data['results'])
        skip += limit

        # Respect rate limits
        time.sleep(0.1)

    return all_results
```

---

## 🔍 Common Use Cases

### 1. Find Adverse Events for a Specific Drug

```python
import requests

url = "https://api.fda.gov/drug/event.json"
params = {
    "search": 'patient.drug.medicinalproduct:"ASPIRIN"',
    "limit": 100
}

response = requests.get(url, params=params)
events = response.json()

for event in events['results']:
    print(f"Report: {event['safetyreportid']}")
    if 'patient' in event and 'reaction' in event['patient']:
        reactions = [r['reactionmeddrapt'] for r in event['patient']['reaction']]
        print(f"Reactions: {', '.join(reactions)}")
```

### 2. Count Most Common Adverse Reactions

```python
import requests

url = "https://api.fda.gov/drug/event.json"
params = {
    "count": "patient.reaction.reactionmeddrapt.exact",
    "limit": 20
}

response = requests.get(url, params=params)
data = response.json()

print("Top 20 adverse reactions:")
for item in data['results']:
    print(f"{item['term']}: {item['count']:,} reports")
```

### 3. Search Device Recalls by Product Type

```python
import requests

url = "https://api.fda.gov/device/recall.json"
params = {
    "search": 'product_description:"pacemaker"',
    "limit": 10,
    "sort": "recall_initiation_date:desc"
}

response = requests.get(url, params=params)
recalls = response.json()

for recall in recalls['results']:
    print(f"\nProduct: {recall.get('product_description', 'N/A')}")
    print(f"Reason: {recall.get('reason_for_recall', 'N/A')}")
    print(f"Classification: {recall.get('classification', 'N/A')}")
    print(f"Date: {recall.get('recall_initiation_date', 'N/A')}")
```

### 4. Find Serious Adverse Events in Date Range

```python
import requests

url = "https://api.fda.gov/drug/event.json"
params = {
    "search": "serious:1+AND+receivedate:[20230101+TO+20231231]",
    "limit": 100,
    "sort": "receivedate:desc"
}

response = requests.get(url, params=params)
events = response.json()

print(f"Found {events['meta']['results']['total']:,} serious events in 2023")
```

### 5. Search Drug Labels for Specific Information

```python
import requests

url = "https://api.fda.gov/drug/label.json"
params = {
    "search": 'openfda.brand_name:"ASPIRIN"+AND+warnings:bleeding',
    "limit": 5
}

response = requests.get(url, params=params)
labels = response.json()

for label in labels['results']:
    brand = label.get('openfda', {}).get('brand_name', ['Unknown'])[0]
    warnings = label.get('warnings', ['No warnings section'])
    print(f"\nBrand: {brand}")
    print(f"Warnings: {warnings[0][:200]}...")  # First 200 chars
```

### 6. Analyze Adverse Events by Patient Demographics

```python
import requests

url = "https://api.fda.gov/drug/event.json"

# Count by gender
params = {
    "search": 'patient.drug.medicinalproduct:"ASPIRIN"',
    "count": "patient.patientsex"
}

response = requests.get(url, params=params)
data = response.json()

print("Adverse events by gender:")
gender_map = {"0": "Unknown", "1": "Male", "2": "Female"}
for item in data['results']:
    gender = gender_map.get(item['term'], item['term'])
    print(f"{gender}: {item['count']:,}")
```

### 7. Track Recall Trends Over Time

```python
import requests

url = "https://api.fda.gov/drug/enforcement.json"
params = {
    "count": "recall_initiation_date",
    "limit": 365
}

response = requests.get(url, params=params)
data = response.json()

print("Drug recalls by date (last year):")
for item in sorted(data['results'], key=lambda x: x['term'], reverse=True)[:30]:
    print(f"{item['term']}: {item['count']} recalls")
```

---

## 🆘 Support & Additional Resources

- **Questions:** [email protected]
- **Interactive Explorer:** https://open.fda.gov/apis/try-the-api/
- **Query Syntax Guide:** https://open.fda.gov/apis/query-syntax/
- **Field Reference:** https://open.fda.gov/apis/drug/event/searchable-fields/
- **GitHub Examples:** https://github.com/FDA/openfda
- **Status Page:** https://open.fda.gov/about/status/
