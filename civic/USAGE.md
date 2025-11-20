# Google Civic Information API - Detailed Usage Guide

## 📖 Official Documentation

**Primary Documentation:** https://developers.google.com/civic-information/docs/using_api

**API Console:** https://console.developers.google.com/

**Developer's Guide:** https://developers.google.com/civic-information/docs/v2

The Google Civic Information API provides comprehensive election and voting information to help citizens participate in democracy.

---

## 📊 Available Data

### Election Information

- **Federal elections:** Presidential, Congressional
- **State elections:** Gubernatorial, legislative, statewide ballot measures
- **Local elections:** Municipal, county, school board, special districts
- **Election dates and deadlines**
- **Voter registration information**

### Polling Location Data

- **Physical addresses** of polling places
- **Early voting locations**
- **Drop-off locations** for mail-in ballots
- **Hours of operation**
- **Accessibility information**
- **Directions and parking**

### Ballot Information

- **Candidate names and parties**
- **Ballot measures and referendums**
- **Candidate photos and URLs**
- **Contest descriptions**
- **Offices being contested**

### Election Administration

- **Election official contact information**
- **Voter registration websites**
- **Ballot information websites**
- **Absentee/vote-by-mail information**
- **State-specific voting rules**

### Geographic Coverage

- **United States:** All 50 states and D.C.
- **Territories:** Puerto Rico, U.S. Virgin Islands, Guam
- **Language support:** 13 languages including Spanish, Chinese, Vietnamese, Korean, and more

### Update Frequency

- **Elections data:** Updated as elections are scheduled
- **Polling locations:** Updated days before elections
- **Candidate information:** Updated as candidates file
- **Real-time on Election Day:** Polling locations may be updated if changes occur

---

## 🔄 Operations & Methods

### HTTP Methods

- **GET** - All queries use HTTP GET requests
- Read-only API

### Available Endpoints

#### 1. Elections Endpoint
```
GET /civicinfo/v2/elections
```

Returns list of available elections.

**Response includes:**
- Election ID (required for voterinfo queries)
- Election name
- Election date
- OCD division ID

#### 2. Voter Info Endpoint
```
GET /civicinfo/v2/voterinfo
```

Returns polling location and ballot information for an address.

**Required parameters:**
- `address` - Voter's registered address
- `electionId` - Election ID from elections endpoint

**Optional parameters:**
- `officialOnly` - Return only official election information
- `returnAllAvailableData` - Return all available data

#### 3. Representatives Endpoint
```
GET /civicinfo/v2/representatives
```

Returns elected officials and representatives for an address.

**Parameters:**
- `address` - Location address
- `levels` - Government levels (e.g., country, state, locality)
- `roles` - Official roles (e.g., legislatorUpperBody, legislatorLowerBody)

#### 4. Representative Info By Division
```
GET /civicinfo/v2/representatives/{ocdId}
```

Returns representatives for a specific division.

---

## 🎛️ Request Options

### Authentication Parameter

**`key`** (required) - Your Google API key

```
key=YOUR_API_KEY
```

### Elections Endpoint Parameters

No additional parameters required.

**Example:**
```
GET /civicinfo/v2/elections?key=YOUR_API_KEY
```

### Voter Info Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `key` | Yes | Google API key |
| `address` | Yes | Voter's registered address |
| `electionId` | Yes | Election ID from elections endpoint |
| `officialOnly` | No | Boolean - return only official data (default: false) |
| `returnAllAvailableData` | No | Boolean - include all available data (default: false) |

**Address format:**
- Freeform text address
- API normalizes and geocodes automatically
- Examples: "340 Main St Venice CA", "1600 Pennsylvania Ave Washington DC"

### Representatives Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `key` | Google API key (required) | `YOUR_API_KEY` |
| `address` | Location address | `1600 Pennsylvania Ave Washington DC` |
| `includeOffices` | Include office information | `true` |
| `levels` | Government levels (array) | `country`, `administrativeArea1`, `locality` |
| `roles` | Official roles (array) | `legislatorUpperBody`, `legislatorLowerBody`, `headOfGovernment` |

**Levels:**
- `country` - Federal/national level
- `administrativeArea1` - State level
- `administrativeArea2` - County level
- `locality` - City/town level
- `subLocality1` - Neighborhood/district
- `subLocality2` - Smaller subdivision

**Roles:**
- `headOfState` - President
- `headOfGovernment` - Governor, mayor
- `legislatorUpperBody` - Senator
- `legislatorLowerBody` - Representative

---

## ⚡ Rate Limits & Quotas

### Default Quotas (Free Tier)

- **10,000 queries per day**
- **10 queries per second per user**

### Quota Management

- Monitor usage in Google API Console
- Can request quota increase for legitimate use cases
- Election Day typically sees increased quotas for civic engagement apps

### Error Response for Quota Exceeded

**HTTP 429** or **403** with error message:
```json
{
  "error": {
    "code": 403,
    "message": "The request cannot be completed because you have exceeded your quota.",
    "errors": [
      {
        "message": "The request cannot be completed because you have exceeded your quota.",
        "domain": "usageLimits",
        "reason": "quotaExceeded"
      }
    ]
  }
}
```

---

## 📦 Response Format

### Elections Response

```json
{
  "kind": "civicinfo#electionsQueryResponse",
  "elections": [
    {
      "id": "2000",
      "name": "VIP Test Election",
      "electionDay": "2021-06-06",
      "ocdDivisionId": "ocd-division/country:us"
    },
    {
      "id": "9000",
      "name": "2024 U.S. General Election",
      "electionDay": "2024-11-05",
      "ocdDivisionId": "ocd-division/country:us"
    }
  ]
}
```

### Voter Info Response

```json
{
  "kind": "civicinfo#voterInfoResponse",
  "election": {
    "id": "2000",
    "name": "VIP Test Election",
    "electionDay": "2021-06-06",
    "ocdDivisionId": "ocd-division/country:us"
  },
  "normalizedInput": {
    "line1": "340 Main Street",
    "city": "Venice",
    "state": "CA",
    "zip": "90291"
  },
  "pollingLocations": [
    {
      "address": {
        "locationName": "Venice Library",
        "line1": "501 S Venice Blvd",
        "city": "Venice",
        "state": "CA",
        "zip": "90291"
      },
      "pollingHours": "7:00am - 8:00pm",
      "startDate": "2021-06-06",
      "endDate": "2021-06-06",
      "sources": [
        {
          "name": "Voting Information Project",
          "official": true
        }
      ]
    }
  ],
  "earlyVoteSites": [
    {
      "address": {
        "locationName": "City Hall",
        "line1": "1685 Main St",
        "city": "Santa Monica",
        "state": "CA",
        "zip": "90401"
      },
      "pollingHours": "8:00am - 5:00pm",
      "startDate": "2021-05-31",
      "endDate": "2021-06-05"
    }
  ],
  "dropOffLocations": [
    {
      "address": {
        "locationName": "County Elections Office",
        "line1": "12400 Imperial Hwy",
        "city": "Norwalk",
        "state": "CA",
        "zip": "90650"
      },
      "pollingHours": "24 hours",
      "startDate": "2021-05-01",
      "endDate": "2021-06-06"
    }
  ],
  "contests": [
    {
      "type": "General",
      "office": "Mayor",
      "level": ["locality"],
      "roles": ["headOfGovernment"],
      "district": {
        "name": "Venice",
        "scope": "citywide"
      },
      "candidates": [
        {
          "name": "John Smith",
          "party": "Democratic Party",
          "candidateUrl": "http://johnsmith.example.com",
          "phone": "555-1234",
          "email": "john@example.com",
          "channels": [
            {
              "type": "Facebook",
              "id": "johnsmith"
            }
          ]
        }
      ]
    }
  ],
  "state": [
    {
      "name": "California",
      "electionAdministrationBody": {
        "name": "California Secretary of State",
        "electionInfoUrl": "http://www.sos.ca.gov/elections/",
        "electionRegistrationUrl": "http://registertovote.ca.gov/",
        "absenteeVotingInfoUrl": "http://www.sos.ca.gov/elections/voter-registration/vote-mail/"
      }
    }
  ]
}
```

### Representatives Response

```json
{
  "kind": "civicinfo#representativeInfoResponse",
  "normalizedInput": {
    "line1": "1600 Pennsylvania Avenue Northwest",
    "city": "Washington",
    "state": "DC",
    "zip": "20500"
  },
  "divisions": {
    "ocd-division/country:us": {
      "name": "United States",
      "officeIndices": [0, 1]
    }
  },
  "offices": [
    {
      "name": "President of the United States",
      "divisionId": "ocd-division/country:us",
      "levels": ["country"],
      "roles": ["headOfState", "headOfGovernment"],
      "officialIndices": [0]
    },
    {
      "name": "Vice President of the United States",
      "divisionId": "ocd-division/country:us",
      "levels": ["country"],
      "roles": ["deputyHeadOfGovernment"],
      "officialIndices": [1]
    }
  ],
  "officials": [
    {
      "name": "Joseph R. Biden",
      "party": "Democratic Party",
      "phones": ["(202) 456-1111"],
      "urls": ["https://www.whitehouse.gov/"],
      "channels": [
        {
          "type": "Twitter",
          "id": "POTUS"
        }
      ]
    }
  ]
}
```

---

## ⚠️ Known Limitations

### Data Availability

1. **Not all elections are covered:**
   - Focus on major federal, state, and local elections
   - Very small local elections may not be included
   - Special elections may have limited data

2. **Polling location updates:**
   - Data typically available 1-2 weeks before elections
   - Last-minute changes may not be reflected
   - Always recommend voters confirm with official sources

3. **Candidate information varies:**
   - Depends on data submission by election officials
   - Some candidates may have minimal information
   - Not all contests include candidate details

### API Limitations

1. **Address must be in covered jurisdiction:**
   - Returns error if election not available for address
   - U.S. addresses only

2. **Election ID required for voter info:**
   - Must call elections endpoint first
   - Different elections have different IDs

3. **No historical election data:**
   - API focuses on upcoming and current elections
   - Past election data not available

4. **Rate limits apply:**
   - 10 QPS per user for free tier
   - Plan accordingly for high-traffic applications

### Data Freshness

1. **Polling locations:**
   - May change up to Election Day
   - Recommend checking close to election

2. **Candidate information:**
   - Updated as candidates file
   - Late-filing candidates may be delayed

3. **Election officials:**
   - Contact information may change
   - Always provide fallback to official websites

---

## 💡 Best Practices

### API Usage

**DO:**
- ✅ Cache elections list (changes infrequently)
- ✅ Validate and normalize addresses before sending
- ✅ Handle missing data gracefully
- ✅ Provide fallback to official election websites
- ✅ Test with test election ID (2000) during development

**DON'T:**
- ❌ Make unnecessary repeated calls for same data
- ❌ Assume all fields will be present
- ❌ Cache voter info for more than a few hours before Election Day
- ❌ Use API for automated mass lookups without Google approval

### User Experience

1. **Address input:**
   - Accept freeform address input
   - API handles normalization
   - Show normalized address back to user

2. **Error handling:**
   - Provide helpful error messages
   - Link to official election websites as backup
   - Handle "no election found" gracefully

3. **Mobile optimization:**
   - Make addresses and directions tappable for maps
   - Provide "navigate" buttons for polling locations
   - Large, clear call-to-action buttons

4. **Accessibility:**
   - Use provided accessibility information
   - Indicate wheelchair-accessible locations
   - Support screen readers

### Election Day Preparation

1. **Increase caching** in days before election
2. **Test heavily** 1-2 weeks before
3. **Monitor API status** on Election Day
4. **Have fallback** to official websites ready

---

## 🔍 Common Use Cases

### 1. Find Polling Location for Address

```python
import requests

api_key = "YOUR_API_KEY"

# Get available elections
elections_url = "https://www.googleapis.com/civicinfo/v2/elections"
elections_response = requests.get(elections_url, params={"key": api_key})
elections = elections_response.json()

# Get latest election ID
election_id = elections['elections'][0]['id']

# Get voter info
voterinfo_url = "https://www.googleapis.com/civicinfo/v2/voterinfo"
params = {
    "key": api_key,
    "address": "1600 Pennsylvania Ave Washington DC",
    "electionId": election_id
}

response = requests.get(voterinfo_url, params=params)
voter_info = response.json()

# Extract polling location
if 'pollingLocations' in voter_info and voter_info['pollingLocations']:
    location = voter_info['pollingLocations'][0]
    print(f"Polling Place: {location['address'].get('locationName')}")
    print(f"Address: {location['address']['line1']}")
    print(f"Hours: {location.get('pollingHours')}")
```

### 2. Display All Contests and Candidates

```python
import requests

api_key = "YOUR_API_KEY"
voterinfo_url = "https://www.googleapis.com/civicinfo/v2/voterinfo"

params = {
    "key": api_key,
    "address": "340 Main St Venice CA",
    "electionId": "2000"
}

response = requests.get(voterinfo_url, params=params)
voter_info = response.json()

if 'contests' in voter_info:
    for contest in voter_info['contests']:
        print(f"\n{contest['office']}")
        print(f"Type: {contest['type']}")

        if 'candidates' in contest:
            for candidate in contest['candidates']:
                print(f"  - {candidate['name']} ({candidate.get('party', 'N/A')})")
```

### 3. Find Representatives for an Address

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://www.googleapis.com/civicinfo/v2/representatives"

params = {
    "key": api_key,
    "address": "1600 Pennsylvania Ave Washington DC"
}

response = requests.get(url, params=params)
data = response.json()

print("Your Representatives:")
for office in data.get('offices', []):
    print(f"\n{office['name']}:")
    for index in office.get('officialIndices', []):
        official = data['officials'][index]
        print(f"  {official['name']} ({official.get('party', 'N/A')})")
```

### 4. Election Reminder System

```python
import requests
from datetime import datetime

api_key = "YOUR_API_KEY"
elections_url = "https://www.googleapis.com/civicinfo/v2/elections"

response = requests.get(elections_url, params={"key": api_key})
elections = response.json()

print("Upcoming Elections:")
for election in elections.get('elections', []):
    election_date = datetime.strptime(election['electionDay'], '%Y-%m-%d')
    days_until = (election_date - datetime.now()).days

    if days_until >= 0:
        print(f"\n{election['name']}")
        print(f"Date: {election['electionDay']}")
        print(f"Days until election: {days_until}")
```

### 5. Check for Early Voting Options

```python
import requests

api_key = "YOUR_API_KEY"
voterinfo_url = "https://www.googleapis.com/civicinfo/v2/voterinfo"

params = {
    "key": api_key,
    "address": "340 Main St Venice CA",
    "electionId": "2000",
    "returnAllAvailableData": "true"
}

response = requests.get(voterinfo_url, params=params)
voter_info = response.json()

print("Early Voting Options:")

if 'earlyVoteSites' in voter_info:
    for site in voter_info['earlyVoteSites']:
        print(f"\n{site['address'].get('locationName')}")
        print(f"Address: {site['address']['line1']}")
        print(f"Dates: {site.get('startDate')} to {site.get('endDate')}")
        print(f"Hours: {site.get('pollingHours')}")

if 'dropOffLocations' in voter_info:
    print("\n\nBallot Drop-off Locations:")
    for site in voter_info['dropOffLocations']:
        print(f"\n{site['address'].get('locationName')}")
        print(f"Address: {site['address']['line1']}")
```

---

## 🆘 Support & Additional Resources

- **Google Civic Information API Docs:** https://developers.google.com/civic-information/
- **API Console:** https://console.developers.google.com/
- **Issue Tracker:** https://issuetracker.google.com/issues?q=componentid:187232
- **Google Groups:** https://groups.google.com/g/google-civic-information-api
- **Voting Information Project:** https://www.votinginfoproject.org/
- **Vote.org:** https://www.vote.org/ (partner organization)
