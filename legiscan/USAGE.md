# LegiScan API - Detailed Usage Guide

## 📖 Official Documentation

**Primary Documentation:** https://legiscan.com/gaits/documentation

**API User Manual (PDF):** https://legiscan.com/misc/LegiScan_API_User_Manual.pdf

**API Registration:** https://legiscan.com/legiscan

LegiScan provides comprehensive legislative tracking data for all 50 U.S. states and Congress, with over 1 million pieces of tracked legislation.

---

## 📊 Available Data

### Legislative Coverage

- **All 50 U.S. States** - Complete state legislature coverage
- **U.S. Congress** - Federal House and Senate
- **Washington D.C.** - Local D.C. Council legislation

### Data Types

#### 1. **Bills** 📜
- Bill text (all versions)
- Status and progress
- Sponsors and co-sponsors
- Committee assignments
- Fiscal notes
- Amendments

#### 2. **Votes** 🗳️
- Roll call votes
- Individual legislator positions
- Vote outcomes
- Voting patterns

#### 3. **People** 👥
- Legislators and sponsors
- Committee memberships
- Contact information
- Party affiliation
- District information

#### 4. **Amendments** 📝
- Amendment text
- Adoption status
- Sponsors

#### 5. **Supplements** 📄
- Fiscal notes
- Committee reports
- Analysis documents
- Hearing transcripts

### Update Frequency

- **Real-time during sessions:** Bills and votes updated within minutes
- **Off-session:** Historical data always available
- **Text documents:** Updated as new versions are adopted

### Historical Coverage

- **Varies by state:** Most states from 2011 forward
- **Some states:** Data back to 2000s
- **Congress:** Complete coverage from 111th Congress (2009) forward

---

## 🔄 Operations & Methods

### HTTP Methods

- **GET** - All queries use HTTP GET requests
- Single endpoint with `op` parameter

### Operation Types

LegiScan uses two prefixes for operations:

1. **get*** - Returns raw API response
2. **process*** - Returns formatted data (may have additional processing)

### Core Operations

#### Session Management

**`getSessionList` / `processSessionList`**
Returns list of legislative sessions.

Parameters:
- `key` - API key (required)
- `op` - Operation name (required)
- `state` - Two-letter state code (optional, returns all states if omitted)

**`setSessionBased` / `processSetSessionBased`**
Set default session for subsequent queries.

#### Bill Operations

**`getBill` / `processBill`**
Get detailed bill information.

Parameters:
- `key` - API key
- `op` - getBill
- `id` - Bill ID (from masterlist or search)

**`getBillText` / `processBillText`**
Get bill text document.

Parameters:
- `key` - API key
- `op` - getBillText
- `id` - Document ID (from bill details)

**`getAmendment` / `processAmendment`**
Get amendment details.

Parameters:
- `key` - API key
- `op` - getAmendment
- `id` - Amendment ID

**`getSupplement` / `processSupplement`**
Get supplementary documents.

Parameters:
- `key` - API key
- `op` - getSupplement
- `id` - Supplement ID

#### List Operations

**`getMasterList` / `processMasterList`**
Get all bills for a session (most efficient for bulk data).

Parameters:
- `key` - API key
- `op` - getMasterList
- `state` - State code (optional)
- `id` - Session ID (optional)

**`getMasterListRaw`**
Raw CSV format of master list.

#### Vote Operations

**`getRollCall` / `processRollCall`**
Get roll call vote details.

Parameters:
- `key` - API key
- `op` - getRollCall
- `id` - Roll call ID

#### Search Operations

**`getSearch` / `processSearch`**
Search for bills across states.

Parameters:
- `key` - API key
- `op` - getSearch
- `state` - State code (optional, ALL for all states)
- `query` - Search query
- `year` - Year or year range (e.g., 2, 2023, 2020-2023)
- `page` - Page number (50 results per page)

**`getSearchRaw`**
Raw search results in CSV format.

#### Monitoring

**`getMonitor` / `processMonitor`**
Get monitored bill lists (requires configuration).

Parameters:
- `key` - API key
- `op` - getMonitor
- `record` - Monitor list ID

#### People

**`getSponsor`**
Get sponsor details.

Parameters:
- `key` - API key
- `op` - getSponsor
- `id` - People ID

---

## 🎛️ Request Options

### Required Parameters

- `key` - Your API key (all requests)
- `op` - Operation to perform (all requests)

### Common Optional Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `state` | Two-letter state code | `CA`, `TX`, `NY` |
| `id` | Entity ID (bill, amendment, etc.) | `1234567` |
| `query` | Search query string | `education reform` |
| `year` | Year or range | `2023`, `2020-2023`, `2` (last 2 years) |
| `page` | Page number for paginated results | `1`, `2`, `3` |

### State Codes

All 50 states plus:
- `US` - U.S. Congress
- `DC` - Washington D.C.

### Search Query Syntax

- **Keywords:** `"education reform"`
- **Boolean operators:** `education AND reform`
- **Phrase search:** `"school funding"`
- **Multiple terms:** Automatically uses OR logic

---

## ⚡ Rate Limits & Quotas

### Subscription Tiers

LegiScan offers multiple subscription levels with varying limits:

1. **Free/Developer:** Limited calls per month
2. **Standard:** Moderate usage
3. **Professional:** High volume
4. **Enterprise:** Unlimited with SLA

### General Guidelines

- **Caching recommended:** Cache master lists and bill details
- **Respect fair use:** Don't hammer the API
- **Bulk operations preferred:** Use getMasterList instead of individual queries

### Rate Limiting Behavior

- API returns HTTP 429 when limits exceeded
- Contact LegiScan to discuss needs if hitting limits

---

## 📦 Response Format

### Standard Response Structure

All responses include `status` field:

```json
{
  "status": "OK",
  // Operation-specific data
}
```

or

```json
{
  "status": "ERROR",
  "alert": {
    "message": "Error description"
  }
}
```

### Bill Response Example

```json
{
  "status": "OK",
  "bill": {
    "bill_id": "1234567",
    "change_hash": "abc123",
    "session_id": "1234",
    "session": {
      "session_id": "1234",
      "state_id": "5",
      "year_start": "2023",
      "year_end": "2024",
      "special": "0",
      "name": "2023-2024 Regular Session"
    },
    "url": "https://legiscan.com/CA/bill/AB1/2023",
    "state": "CA",
    "state_link": "https://leginfo.legislature.ca.gov/...",
    "completed": "1",
    "status": "3",
    "status_date": "2023-09-15",
    "progress": [
      {
        "date": "2023-01-15",
        "event": "1"
      }
    ],
    "state_type": "B",
    "bill_type": "B",
    "bill_type_id": "1",
    "body": "H",
    "body_id": "13",
    "current_body": "H",
    "current_body_id": "13",
    "title": "Education: school funding.",
    "description": "An act to amend Section 42238.02...",
    "pending_committee_id": "0",
    "committee": {
      "committee_id": "5678",
      "name": "Education"
    },
    "sponsors": [
      {
        "people_id": "12345",
        "person_hash": "xyz789",
        "party_id": "2",
        "party": "D",
        "role_id": "1",
        "role": "Rep",
        "name": "John Smith",
        "first_name": "John",
        "middle_name": "",
        "last_name": "Smith",
        "suffix": "",
        "nickname": "",
        "district": "15",
        "ftm_eid": "12345",
        "votesmart_id": "67890",
        "opensecrets_id": "N00012345",
        "ballotpedia": "John_Smith",
        "sponsor_type_id": "1",
        "sponsor_order": "1",
        "committee_sponsor": "0",
        "committee_id": "0"
      }
    ],
    "sasts": [
      {
        "type_id": "1",
        "type": "Companion",
        "sast_bill_id": "9876543"
      }
    ],
    "subjects": [
      {
        "subject_id": "123",
        "subject_name": "Education"
      }
    ],
    "texts": [
      {
        "doc_id": "1111111",
        "date": "2023-01-15",
        "type": "Introduced",
        "type_id": "1",
        "mime": "text/html",
        "mime_id": "2",
        "url": "https://legiscan.com/CA/text/AB1/2023",
        "state_link": "https://leginfo.legislature.ca.gov/...",
        "text_size": "45678"
      }
    ],
    "votes": [
      {
        "roll_call_id": "222222",
        "date": "2023-03-15",
        "desc": "Assembly Floor Vote",
        "yea": "52",
        "nay": "28",
        "nv": "0",
        "absent": "0",
        "total": "80",
        "passed": "1",
        "chamber": "H",
        "chamber_id": "13",
        "url": "https://legiscan.com/CA/rollcall/AB1/222222/2023"
      }
    ],
    "amendments": [],
    "supplements": [
      {
        "supplement_id": "333333",
        "date": "2023-02-01",
        "type_id": "4",
        "type": "Fiscal Note",
        "title": "Fiscal Note",
        "description": "",
        "mime": "application/pdf",
        "mime_id": "3",
        "url": "https://legiscan.com/CA/supplement/AB1/333333",
        "state_link": "",
        "supplement_size": "67890"
      }
    ],
    "calendar": [],
    "history": [
      {
        "date": "2023-01-15",
        "action": "Introduced. Read first time. To print.",
        "chamber": "H",
        "chamber_id": "13",
        "importance": "1"
      }
    ]
  }
}
```

### Master List Response

```json
{
  "status": "OK",
  "masterlist": {
    "1234567": {
      "bill_id": "1234567",
      "number": "AB1",
      "title": "Education: school funding.",
      "description": "Short description...",
      "status": "3",
      "status_date": "2023-09-15",
      "last_action": "Chaptered by Secretary of State",
      "last_action_date": "2023-09-15",
      "url": "https://legiscan.com/CA/bill/AB1/2023",
      "state_link": "https://leginfo.legislature.ca.gov/...",
      "change_hash": "abc123",
      "session": {
        "session_id": "1234",
        "name": "2023-2024 Regular Session"
      }
    }
  }
}
```

### Roll Call Response

```json
{
  "status": "OK",
  "roll_call": {
    "roll_call_id": "222222",
    "bill_id": "1234567",
    "date": "2023-03-15",
    "desc": "Assembly Floor Vote",
    "yea": "52",
    "nay": "28",
    "nv": "0",
    "absent": "0",
    "total": "80",
    "passed": "1",
    "chamber": "H",
    "chamber_id": "13",
    "url": "https://legiscan.com/CA/rollcall/AB1/222222/2023",
    "state_link": "https://leginfo.legislature.ca.gov/...",
    "votes": [
      {
        "people_id": "12345",
        "vote_id": "1",
        "vote_text": "Yea"
      }
    ]
  }
}
```

---

## ⚠️ Known Limitations

### Data Availability

1. **Historical coverage varies by state**
2. **Some states have incomplete data** for older sessions
3. **Bill text may not be available** for all versions
4. **Committee information** may be limited in some jurisdictions

### API Constraints

1. **Pagination:** Search results limited to 50 per page
2. **No real-time streaming:** Must poll for updates
3. **Rate limits apply:** Depends on subscription tier
4. **Session-based data:** Must know session ID for many operations

### Technical Limitations

1. **Large responses:** Bill objects can be substantial (cache recommended)
2. **No batch operations:** Must query bills individually
3. **State variations:** Data structure may vary slightly by state

---

## 💡 Best Practices

### Efficient Querying

**DO:**
- ✅ Use `getMasterList` for bulk bill retrieval
- ✅ Cache session lists (rarely change)
- ✅ Store bill change_hash to detect updates
- ✅ Use search for finding bills, master list for details

**DON'T:**
- ❌ Query individual bills repeatedly
- ❌ Search when you know the bill ID
- ❌ Ignore change_hash for caching

### Typical Workflow

1. **Get sessions:** `getSessionList`
2. **Get all bills:** `getMasterList` for session
3. **Cache bill list** locally
4. **Query individual bills** only when needed
5. **Check change_hash** before re-downloading

### Handling Bill Text

Bill text can be:
- HTML
- PDF
- Plain text

Check `mime` and `mime_id` fields to handle appropriately.

---

## 🔍 Common Use Cases

### 1. Get All Bills for Current California Session

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://api.legiscan.com/"

# Get sessions for California
sessions_params = {
    "key": api_key,
    "op": "getSessionList",
    "state": "CA"
}
sessions_response = requests.get(url, params=sessions_params)
sessions = sessions_response.json()

# Get most recent session
if sessions['status'] == 'OK':
    latest_session = sessions['sessions'][0]
    session_id = latest_session['session_id']

    # Get master list for that session
    master_params = {
        "key": api_key,
        "op": "getMasterList",
        "id": session_id
    }
    master_response = requests.get(url, params=master_params)
    bills = master_response.json()

    if bills['status'] == 'OK':
        print(f"Found {len(bills['masterlist'])} bills")
```

### 2. Search for Bills About Education

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://api.legiscan.com/"

params = {
    "key": api_key,
    "op": "getSearch",
    "state": "ALL",  # Search all states
    "query": "education funding",
    "year": "2",  # Last 2 years
    "page": "1"
}

response = requests.get(url, params=params)
results = response.json()

if results['status'] == 'OK':
    for result in results['searchresult']:
        if isinstance(result, dict):
            print(f"{result['state']} {result['bill_number']}: {result['title']}")
```

### 3. Get Bill Details with Full History

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://api.legiscan.com/"

params = {
    "key": api_key,
    "op": "getBill",
    "id": "1234567"  # Bill ID from search or master list
}

response = requests.get(url, params=params)
bill = response.json()

if bill['status'] == 'OK':
    bill_data = bill['bill']
    print(f"Bill: {bill_data['bill_number']}")
    print(f"Title: {bill_data['title']}")
    print(f"Status: {bill_data['status']}")

    print("\nHistory:")
    for event in bill_data['history']:
        print(f"  {event['date']}: {event['action']}")
```

### 4. Track Bill Updates Using Change Hash

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://api.legiscan.com/"
stored_hash = "abc123"  # Previously stored hash

params = {
    "key": api_key,
    "op": "getMasterList",
    "id": "1234"  # Session ID
}

response = requests.get(url, params=params)
master = response.json()

if master['status'] == 'OK':
    for bill_id, bill in master['masterlist'].items():
        if isinstance(bill, dict):
            if bill['change_hash'] != stored_hash:
                print(f"Bill {bill['number']} has been updated")
                # Fetch full bill details
```

### 5. Get Roll Call Votes for a Bill

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://api.legiscan.com/"

# First get the bill to find roll call IDs
bill_params = {
    "key": api_key,
    "op": "getBill",
    "id": "1234567"
}

bill_response = requests.get(url, params=bill_params)
bill = bill_response.json()

if bill['status'] == 'OK':
    votes = bill['bill']['votes']
    for vote in votes:
        roll_call_id = vote['roll_call_id']

        # Get full roll call details
        roll_params = {
            "key": api_key,
            "op": "getRollCall",
            "id": roll_call_id
        }

        roll_response = requests.get(url, params=roll_params)
        roll_call = roll_response.json()

        if roll_call['status'] == 'OK':
            rc = roll_call['roll_call']
            print(f"\n{rc['desc']}")
            print(f"Yea: {rc['yea']}, Nay: {rc['nay']}")
            print(f"Passed: {'Yes' if rc['passed'] == '1' else 'No'}")
```

---

## 🆘 Support & Additional Resources

- **Email Support:** api@legiscan.com
- **API Documentation:** https://legiscan.com/gaits/documentation
- **User Manual (PDF):** https://legiscan.com/misc/LegiScan_API_User_Manual.pdf
- **Registration:** https://legiscan.com/legiscan
- **Status Updates:** Check website for service status
