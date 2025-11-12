# OpenFDA API

## Description

The openFDA API provides programmatic access to FDA public data through RESTful endpoints. It enables querying and analysis of regulatory information across multiple categories including drug adverse events, device recalls, food enforcement actions, and more.

## Base URL

```
https://api.fda.gov
```

## Authentication

No API key required for basic usage, but rate limits apply. Register for an API key for higher limits.

## Example Usage

### Search Drug Adverse Events

**cURL:**
```bash
curl "https://api.fda.gov/drug/event.json?search=patient.reaction.reactionmeddrapt:fatigue&limit=10"
```

**Python:**
```python
import requests

url = "https://api.fda.gov/drug/event.json"
params = {
    "search": 'patient.reaction.reactionmeddrapt:"fatigue"',
    "limit": 10
}

response = requests.get(url, params=params)
events = response.json()

for result in events['results']:
    print(f"Report ID: {result.get('safetyreportid')}")
    print(f"Received: {result.get('receivedate')}")
    if 'patient' in result and 'drug' in result['patient']:
        drugs = result['patient']['drug']
        print(f"Drugs: {[d.get('medicinalproduct') for d in drugs]}")
    print()
```

**JavaScript:**
```javascript
const url = 'https://api.fda.gov/drug/event.json';
const params = new URLSearchParams({
  search: 'patient.reaction.reactionmeddrapt:"fatigue"',
  limit: 10
});

fetch(`${url}?${params}`)
  .then(response => response.json())
  .then(data => {
    data.results.forEach(event => {
      console.log(`Report ID: ${event.safetyreportid}`);
      console.log(`Received: ${event.receivedate}`);
      if (event.patient && event.patient.drug) {
        const drugs = event.patient.drug.map(d => d.medicinalproduct);
        console.log(`Drugs: ${drugs.join(', ')}`);
      }
      console.log();
    });
  })
  .catch(error => console.error('Error:', error));
```

### Count Adverse Events by Drug

**cURL:**
```bash
curl "https://api.fda.gov/drug/event.json?count=patient.drug.medicinalproduct.exact"
```

**Python:**
```python
import requests

url = "https://api.fda.gov/drug/event.json"
params = {
    "count": "patient.drug.medicinalproduct.exact",
    "limit": 10
}

response = requests.get(url, params=params)
counts = response.json()

print("Top 10 drugs by adverse event reports:")
for item in counts['results']:
    print(f"{item['term']}: {item['count']:,} reports")
```

**JavaScript:**
```javascript
const url = 'https://api.fda.gov/drug/event.json';
const params = new URLSearchParams({
  count: 'patient.drug.medicinalproduct.exact',
  limit: 10
});

fetch(`${url}?${params}`)
  .then(response => response.json())
  .then(data => {
    console.log('Top drugs by adverse event reports:');
    data.results.forEach(item => {
      console.log(`${item.term}: ${item.count.toLocaleString()} reports`);
    });
  })
  .catch(error => console.error('Error:', error));
```

### Search Device Recalls

**cURL:**
```bash
curl "https://api.fda.gov/device/recall.json?search=product_description:pacemaker&limit=5"
```

**Python:**
```python
import requests

url = "https://api.fda.gov/device/recall.json"
params = {
    "search": "product_description:pacemaker",
    "limit": 5
}

response = requests.get(url, params=params)
recalls = response.json()

for recall in recalls['results']:
    print(f"Product: {recall.get('product_description')}")
    print(f"Reason: {recall.get('reason_for_recall')}")
    print(f"Date: {recall.get('recall_initiation_date')}")
    print()
```

## Query Syntax

- Use field:term syntax for searches
- Combine conditions with `+AND+` (all must match) or `+OR+` (any must match)
- Use quotes for exact phrases: `field:"exact phrase"`
- Use parentheses for complex queries

## Common Parameters

- `search` - Query using field:term syntax
- `limit` - Maximum records to return (default: 1, max: 1000)
- `count` - Aggregate results by field values
- `sort` - Order results (e.g., `receivedate:desc`)

## Resources

- Documentation: https://open.fda.gov/apis/
- Query Syntax: https://open.fda.gov/apis/query-syntax/
- Interactive Explorer: https://open.fda.gov/apis/try-the-api/
