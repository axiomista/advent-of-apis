# LegiScan API

## Description

LegiScan provides access to legislative tracking data across all 50 U.S. states and Congress. The API enables developers to search for bills, track legislation, access bill texts and amendments, view roll call votes, and monitor legislative activity in real-time.

## Base URL

```
https://api.legiscan.com/
```

## Authentication

Requires an API key that must be included in each request.

## Example Usage

### Get Bill by ID

**cURL:**
```bash
curl "https://api.legiscan.com/?key=YOUR_API_KEY&op=getBill&id=1234567"
```

**Python:**
```python
import requests

api_key = "YOUR_API_KEY"
url = "https://api.legiscan.com/"
params = {
    "key": api_key,
    "op": "getBill",
    "id": "1234567"  # Bill ID
}

response = requests.get(url, params=params)
bill = response.json()

if bill['status'] == 'OK':
    bill_data = bill['bill']
    print(f"Bill: {bill_data['bill_number']}")
    print(f"Title: {bill_data['title']}")
    print(f"Status: {bill_data['status_desc']}")
    print(f"State: {bill_data['state']}")
```

**JavaScript:**
```javascript
const apiKey = 'YOUR_API_KEY';
const url = 'https://api.legiscan.com/';
const params = new URLSearchParams({
  key: apiKey,
  op: 'getBill',
  id: '1234567'
});

fetch(`${url}?${params}`)
  .then(response => response.json())
  .then(data => {
    if (data.status === 'OK') {
      const bill = data.bill;
      console.log(`Bill: ${bill.bill_number}`);
      console.log(`Title: ${bill.title}`);
      console.log(`Status: ${bill.status_desc}`);
    }
  })
  .catch(error => console.error('Error:', error));
```

### Search for Bills

**cURL:**
```bash
curl "https://api.legiscan.com/?key=YOUR_API_KEY&op=getSearch&state=CA&query=education"
```

**Python:**
```python
import requests

api_key = "YOUR_API_KEY"
url = "https://api.legiscan.com/"
params = {
    "key": api_key,
    "op": "getSearch",
    "state": "CA",
    "query": "education"
}

response = requests.get(url, params=params)
results = response.json()

if results['status'] == 'OK':
    for bill in results['searchresult']:
        if isinstance(bill, dict) and 'bill_number' in bill:
            print(f"{bill['bill_number']}: {bill['title']}")
```

**JavaScript:**
```javascript
const apiKey = 'YOUR_API_KEY';
const url = 'https://api.legiscan.com/';
const params = new URLSearchParams({
  key: apiKey,
  op: 'getSearch',
  state: 'CA',
  query: 'education'
});

fetch(`${url}?${params}`)
  .then(response => response.json())
  .then(data => {
    if (data.status === 'OK') {
      Object.values(data.searchresult).forEach(bill => {
        if (bill.bill_number) {
          console.log(`${bill.bill_number}: ${bill.title}`);
        }
      });
    }
  })
  .catch(error => console.error('Error:', error));
```

### Get Master List of Bills

**cURL:**
```bash
curl "https://api.legiscan.com/?key=YOUR_API_KEY&op=getMasterList&state=TX"
```

**Python:**
```python
import requests

api_key = "YOUR_API_KEY"
url = "https://api.legiscan.com/"
params = {
    "key": api_key,
    "op": "getMasterList",
    "state": "TX"  # Texas
}

response = requests.get(url, params=params)
master_list = response.json()

if master_list['status'] == 'OK':
    bills = master_list['masterlist']
    print(f"Found {len(bills)} bills")
    for bill_id, bill in list(bills.items())[:5]:  # First 5
        if isinstance(bill, dict):
            print(f"{bill.get('bill_number')}: {bill.get('title')}")
```

## Common Operations (op parameter)

The API uses both "get" and "process" prefixed operations:

- `getSessionList` / `processSessionList` - List available legislative sessions
- `getMasterList` / `processMasterList` - Get all bills for a session
- `getBill` / `processBill` - Get detailed bill information
- `getBillText` / `processBillText` - Get bill text
- `getAmendment` / `processAmendment` - Get amendment details
- `getSupplement` / `processSupplement` - Access supplementary legislative documents
- `getRollCall` / `processRollCall` - Get voting record information
- `getSponsor` - Get sponsor information
- `getSearch` - Search for bills
- `getMonitor` - Access monitored bill lists

## Common Parameters

- `key` (required) - Your API authentication key
- `op` (required) - Operation to perform
- `id` - Bill, amendment, or other entity ID
- `state` - Two-letter state code (e.g., CA, TX, NY)
- `query` - Search query string

## Response Format

All responses are in JSON format with a `status` field indicating success or failure:
```json
{
  "status": "OK",
  "data": { ... }
}
```

## Resources

- Documentation: https://legiscan.com/legiscan
- API User Manual: https://legiscan.com/misc/LegiScan_API_User_Manual.pdf
- API Registration: https://legiscan.com/legiscan
- API Support: api@legiscan.com
