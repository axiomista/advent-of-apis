# 🏛️ LegiScan API

## 📖 Description

Track legislation across America in real-time! LegiScan provides comprehensive access to bills, votes, and legislative activity from all 50 state legislatures and the U.S. Congress. Monitor bill progress, access full text documents, track roll call votes, and stay informed about the laws being made in your state and nationwide.

✨ **Best For:** Legislative tracking, policy research, advocacy tools, and government transparency apps

> 💡 **Did you know?** LegiScan tracks over 1 million pieces of legislation across all U.S. jurisdictions, making it one of the most comprehensive legislative databases available!

## 📍 Base URL

```
https://api.legiscan.com/
```

## 🔑 Authentication

Requires an API key that must be included in each request.

**Get your API key:** https://legiscan.com/legiscan

---

## 💻 Example Usage

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

> 💡 **Pro Tip:** Use the `getMasterList` operation to get all bills for a session, then cache the list and query individual bills as needed. This is much more efficient than repeated searches!

---

## 🔧 Common Operations (op parameter)

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

⚠️ **Note:** For detailed operation parameters and response structures, see [USAGE.md](./USAGE.md)

---

## Response Format

All responses are in JSON format with a `status` field indicating success or failure:
```json
{
  "status": "OK",
  "data": { ... }
}
```

---

## 🚀 Application Examples

**1. Legislative Tracking & Alert System**
Monitor specific bills or topics and send notifications when status changes occur.
- Track bills by keyword, sponsor, or subject
- Send email/SMS alerts on bill progress
- Display bill timelines and next steps
- Show voting records

**2. Civic Engagement Platform**
Help citizens understand and engage with legislation affecting their community.
- Search bills by topic relevant to users
- Simplify legislative language
- Show local impact of state/federal legislation
- Enable constituent contact with representatives
- Track user's positions on bills

**3. Policy Research & Analysis Tool**
Analyze legislative trends, patterns, and policy outcomes across states.
- Compare similar legislation across states
- Track policy diffusion (how laws spread)
- Analyze voting patterns and party alignment
- Generate reports on legislative productivity
- Identify model legislation

**4. Advocacy Campaign Manager**
Coordinate advocacy efforts around specific legislation.
- Monitor priority bills
- Track co-sponsors and support levels
- Coordinate grassroots lobbying
- Generate talking points from bill analysis
- Schedule actions around committee hearings

**5. Government Transparency Dashboard**
Increase transparency by making legislative data accessible and understandable.
- Visualize bill progress through legislative process
- Show legislator sponsorship patterns
- Display committee activity
- Track campaign finance correlation with voting
- Provide plain-language bill summaries

**6. Legislative Calendar & Scheduling App**
Help stakeholders stay informed about hearings and legislative events.
- Display committee hearings from calendar data
- Send reminders for important dates
- Show bill deadlines
- Track session schedules

**7. Comparative Policy Analysis Tool**
Research how different states approach similar issues.
- Use getSearch to find bills on specific topics across states
- Compare text and provisions
- Track adoption rates of policy approaches
- Analyze effectiveness indicators

**8. Legislative Data Journalism Platform**
Power investigative journalism with comprehensive legislative data.
- Identify patterns in legislative activity
- Track special interest influence
- Analyze amendment patterns
- Discover identical bill language across states

---

## 📚 Resources

- 📖 **API Documentation:** https://legiscan.com/legiscan
- 📕 **API User Manual:** https://legiscan.com/misc/LegiScan_API_User_Manual.pdf
- 🔑 **API Registration:** https://legiscan.com/legiscan
- 📧 **API Support:** api@legiscan.com
- 📘 **Detailed Usage Guide:** [USAGE.md](./USAGE.md)
