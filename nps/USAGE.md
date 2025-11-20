# National Parks Service API - Detailed Usage Guide

## 📖 Official Documentation

**Primary Documentation:** https://www.nps.gov/subjects/developer/api-documentation.htm

**Get Started:** https://www.nps.gov/subjects/developer/get-started.htm

**API Guides:** https://www.nps.gov/subjects/developer/guides.htm

The National Parks Service API provides authoritative data about America's national parks, monuments, historic sites, and related facilities.

---

## 📊 Available Data

### Park Information
- Basic park details (name, description, location)
- Operating hours and seasons
- Entrance fees and passes
- Contact information
- Directions and accessibility
- Park images and multimedia

### Alerts & Conditions
- Current park alerts and closures
- Weather advisories
- Road conditions
- Wildlife warnings
- Emergency notifications
- Temporary facility closures

### Activities & Recreation
- Available activities at each park
- Activity descriptions and requirements
- Difficulty levels and accessibility
- Required permits and fees
- Seasonal availability
- Activity-specific parks

### Campgrounds
- Campground locations and details
- Amenities (water, electric, sewer)
- RV accessibility and size limits
- Reservation information
- Fees and regulations
- Operating seasons

### Visitor Centers
- Visitor center locations
- Operating hours
- Services offered
- Contact information
- Accessibility features

### Events
- Ranger-led programs
- Special events
- Educational programs
- Seasonal celebrations
- Volunteer opportunities

### Coverage
- **Parks:** 400+ national parks, monuments, and historic sites
- **Geographic:** All 50 U.S. states and territories
- **Update Frequency:** Real-time for alerts, regularly updated for park data
- **Historical Data:** Limited to current/upcoming events

---

## 🔄 Operations & Methods

### HTTP Methods
- **GET** - All requests use HTTP GET

### Available Endpoints

#### Core Endpoints

**`/parks`** - Comprehensive park information
- Returns detailed data about national parks
- Supports filtering by state, designation, or activity

**`/alerts`** - Current park alerts and warnings
- Real-time alerts about closures, hazards, conditions
- Categories: Caution, Danger, Information, Park Closure

**`/campgrounds`** - Campground information
- Details about campground facilities and amenities
- Operating seasons and reservation info

**`/visitorcenters`** - Visitor center details
- Locations, hours, and services
- Contact information

**`/activities`** - Activity information
- List of all available activities
- Can find parks by activity

**`/activities/parks`** - Parks by activity
- Reverse lookup: find parks offering specific activities

**`/events`** - Park events and programs
- Ranger programs, special events
- Educational opportunities

**`/amenities`** - Amenity information
- Available amenities at parks/campgrounds

**`/amenities/parksplaces`** - Places by amenity
- Find locations with specific amenities

**`/places`** - Points of interest
- Trails, landmarks, historic sites

**`/thingstodo`** - Curated activities and experiences

**`/articles`** - News and feature articles

**`/lessonplans`** - Educational lesson plans

**`/people`** - Notable people associated with parks

**`/newsreleases`** - Official park news releases

---

## 🎛️ Request Options

### Authentication
**`X-Api-Key`** (required) - API key sent in HTTP header

### Common Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `parkCode` | 4-letter park code | `yell`, `grca`, `yose` |
| `stateCode` | 2-letter state abbreviation | `CA`, `NY`, `TX` |
| `limit` | Results per page (max 1000) | `50` |
| `start` | Starting index for pagination | `0`, `50`, `100` |
| `q` | Search query | `waterfall`, `historic` |

### Parks Endpoint Parameters

| Parameter | Description |
|-----------|-------------|
| `parkCode` | Comma-separated park codes |
| `stateCode` | Comma-separated state codes |
| `designation` | Filter by type (National Park, Monument, etc.) |

### Alerts Endpoint Parameters

| Parameter | Description |
|-----------|-------------|
| `parkCode` | Filter alerts by park |
| `stateCode` | Filter alerts by state |

### Activities Parameters

| Parameter | Description |
|-----------|-------------|
| `id` | Activity UUID |
| `q` | Search activity names |

### Campgrounds Parameters

| Parameter | Description |
|-----------|-------------|
| `parkCode` | Filter by park |
| `stateCode` | Filter by state |

### Events Parameters

| Parameter | Description |
|-----------|-------------|
| `parkCode` | Filter by park |
| `dateStart` | Events after date (YYYY-MM-DD) |
| `dateEnd` | Events before date (YYYY-MM-DD) |
| `pageSize` | Number of results |
| `pageNumber` | Page number for pagination |

---

## ⚡ Rate Limits & Quotas

### API Key Tier
- **Hourly limit:** 1,000 requests per hour
- **Daily limit:** Not specified (standard fair use)

**Rate Limiting:**
- HTTP 429 returned when exceeded
- Contact NPS for higher limits if needed

### Best Practices for Rate Management
- Cache park data (updates infrequently)
- Cache activity lists (rarely change)
- Poll alerts more frequently (real-time data)
- Use specific park codes to reduce response size
- Implement exponential backoff on errors

---

## 📦 Response Format

### Standard Response Structure

All endpoints return JSON with this structure:

```json
{
  "total": "437",
  "data": [...],
  "limit": "50",
  "start": "0"
}
```

### Park Response

```json
{
  "total": "1",
  "data": [
    {
      "id": "F58C6D24-8D10-4573-9826-65D42B8B83AD",
      "url": "https://www.nps.gov/yell/index.htm",
      "fullName": "Yellowstone National Park",
      "parkCode": "yell",
      "description": "Visit the world's first national park...",
      "latitude": "44.59824417",
      "longitude": "-110.5471695",
      "latLong": "lat:44.59824417, long:-110.5471695",
      "activities": [
        {
          "id": "13A57703-BB1A-41A2-94B8-53B692EB7238",
          "name": "Astronomy"
        },
        {
          "id": "5F723BAD-7359-48FC-98FA-631592256E35",
          "name": "Auto and ATV"
        }
      ],
      "topics": [
        {
          "id": "7F81A0CB-B91F-4896-B9A5-41BE9A54A27B",
          "name": "Archeology"
        }
      ],
      "states": "ID,MT,WY",
      "contacts": {
        "phoneNumbers": [
          {
            "phoneNumber": "3073447381",
            "description": "",
            "extension": "",
            "type": "Voice"
          }
        ],
        "emailAddresses": [
          {
            "description": "",
            "emailAddress": "yell_visitor_services@nps.gov"
          }
        ]
      },
      "entranceFees": [
        {
          "cost": "35.00",
          "description": "Valid for 7 days. Admits one single, private, non-commercial vehicle...",
          "title": "7-Day Private Vehicle Entrance Fee"
        }
      ],
      "entrancePasses": [
        {
          "cost": "70.00",
          "description": "Valid for one year from month of purchase...",
          "title": "Yellowstone Annual Pass"
        }
      ],
      "fees": [],
      "directionsInfo": "Yellowstone is located in the northwest corner of Wyoming...",
      "directionsUrl": "http://www.nps.gov/yell/planyourvisit/directions.htm",
      "operatingHours": [
        {
          "exceptions": [],
          "description": "The park is open 24 hours a day, year round.",
          "standardHours": {
            "wednesday": "All Day",
            "monday": "All Day",
            "thursday": "All Day",
            "sunday": "All Day",
            "tuesday": "All Day",
            "friday": "All Day",
            "saturday": "All Day"
          },
          "name": "Yellowstone National Park"
        }
      ],
      "addresses": [
        {
          "postalCode": "82190",
          "city": "Yellowstone National Park",
          "stateCode": "WY",
          "line1": "P.O. Box 168",
          "type": "Mailing",
          "line3": "",
          "line2": ""
        }
      ],
      "images": [
        {
          "credit": "NPS/Jim Peaco",
          "title": "Fly Fishing in Yellowstone",
          "altText": "Fly fishing in the Madison River",
          "caption": "World-class fly fishing on the Madison River",
          "url": "https://www.nps.gov/common/uploads/structured_data/image.jpg"
        }
      ],
      "weatherInfo": "Yellowstone's weather can vary greatly...",
      "name": "Yellowstone",
      "designation": "National Park"
    }
  ],
  "limit": "50",
  "start": "0"
}
```

### Alert Response

```json
{
  "total": "3",
  "data": [
    {
      "id": "A1234567-89AB-CDEF-0123-456789ABCDEF",
      "url": "",
      "title": "Road Closure: Beartooth Highway",
      "parkCode": "yell",
      "description": "The Beartooth Highway (US 212) is closed...",
      "category": "Park Closure",
      "lastIndexedDate": "2025-01-15T10:30:00Z"
    }
  ],
  "limit": "50",
  "start": "0"
}
```

**Alert Categories:**
- `Caution` - Advisory information
- `Danger` - Safety warnings
- `Information` - General updates
- `Park Closure` - Full or partial closures

### Campground Response

```json
{
  "total": "1",
  "data": [
    {
      "id": "B2345678-90BC-DEF0-1234-56789ABCDEF0",
      "url": "https://www.nps.gov/yell/planyourvisit/campgrounds.htm",
      "name": "Madison Campground",
      "parkCode": "yell",
      "description": "Located near the confluence of the Gibbon and Firehole Rivers...",
      "latitude": "44.6465",
      "longitude": "-110.8582",
      "latLong": "{lat:44.6465, lng:-110.8582}",
      "audioDescription": "",
      "isPassportStampLocation": "0",
      "passportStampLocationDescription": "",
      "passportStampImages": [],
      "geometryPoiId": "",
      "reservationInfo": "",
      "reservationUrl": "",
      "regulationsurl": "",
      "regulationsOverview": "",
      "amenities": {
        "trashRecyclingCollection": "Yes",
        "toilets": ["Vault Toilets - year round"],
        "internetConnectivity": "No",
        "showers": ["None"],
        "cellPhoneReception": "No",
        "laundry": "No",
        "amphitheater": "Yes",
        "dumpStation": "No",
        "campStore": "No",
        "staffOrVolunteerHostOnsite": "Yes",
        "potableWater": ["Yes - year round"],
        "iceAvailableForSale": "No",
        "firewoodForSale": "No",
        "foodStorageLockers": "Yes"
      },
      "contacts": {},
      "fees": [
        {
          "cost": "30.00",
          "description": "Per night campsite fee",
          "title": "Campsite Fee"
        }
      ],
      "directionsOverview": "",
      "directionsUrl": "",
      "operatingHours": [
        {
          "exceptions": [],
          "description": "Typically open late April through mid-October",
          "standardHours": {
            "wednesday": "All Day",
            "monday": "All Day",
            "thursday": "All Day",
            "sunday": "All Day",
            "tuesday": "All Day",
            "friday": "All Day",
            "saturday": "All Day"
          },
          "name": "Madison Campground"
        }
      ],
      "addresses": [],
      "images": [],
      "weatherOverview": "",
      "numberOfSitesReservable": "0",
      "numberOfSitesFirstComeFirstServe": "278",
      "campsites": {
        "totalSites": "278",
        "group": "0",
        "horse": "0",
        "tentOnly": "0",
        "electricalHookups": "0",
        "rvOnly": "0",
        "walkBoatTo": "0",
        "other": "0"
      },
      "accessibility": {
        "wheelchairAccess": "Yes",
        "internetInfo": "",
        "cellPhoneInfo": "No cell service",
        "fireStovePolicy": "",
        "rvAllowed": "Yes",
        "rvInfo": "RVs up to 80 feet",
        "rvMaxLength": "80",
        "additionalInfo": "",
        "trailerMaxLength": "0",
        "adaInfo": "Accessible sites available",
        "trailerAllowed": "Yes",
        "accessRoads": ["Paved Roads - All vehicles"],
        "classifications": ["Developed Campground - campsites"]
      },
      "multimedia": [],
      "lastIndexedDate": ""
    }
  ],
  "limit": "50",
  "start": "0"
}
```

---

## 💡 Best Practices

### Efficient API Usage

**DO:**
- ✅ Cache park data for at least 24 hours (changes infrequently)
- ✅ Use specific `parkCode` parameters to reduce response size
- ✅ Poll alerts every 15-30 minutes for real-time updates
- ✅ Use pagination for large result sets
- ✅ Store activity/amenity lists (rarely change)
- ✅ Implement request retry with exponential backoff

**DON'T:**
- ❌ Request all parks without filters (use `stateCode` or `parkCode`)
- ❌ Poll parks endpoint frequently (data rarely changes)
- ❌ Ignore pagination (some endpoints return many results)
- ❌ Hardcode activity/amenity IDs (they may change)

### Data Interpretation

**Park Codes:**
- Always lowercase, 4 characters
- Some common codes:
  - `yell` - Yellowstone
  - `grca` - Grand Canyon
  - `yose` - Yosemite
  - `zion` - Zion
  - `acad` - Acadia
  - `grsm` - Great Smoky Mountains

**States:**
- Parks can span multiple states (comma-separated)
- Example: Yellowstone is in `ID,MT,WY`

**Designations:**
- National Park
- National Monument
- National Historic Site
- National Recreation Area
- And many others

---

## 🔍 Common Use Cases

### 1. Find Parks in a State

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://developer.nps.gov/api/v1/parks"
headers = {"X-Api-Key": api_key}
params = {
    "stateCode": "CA",
    "limit": 100
}

response = requests.get(url, headers=headers, params=params)
data = response.json()

print(f"Found {data['total']} parks in California:\n")
for park in data['data']:
    print(f"{park['fullName']} ({park['parkCode']})")
    print(f"  Designation: {park['designation']}")
    print(f"  URL: {park['url']}")
    print()
```

### 2. Get Current Alerts for a Park

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://developer.nps.gov/api/v1/alerts"
headers = {"X-Api-Key": api_key}
params = {"parkCode": "yell"}

response = requests.get(url, headers=headers, params=params)
data = response.json()

if int(data['total']) > 0:
    print(f"Active alerts for Yellowstone:\n")
    for alert in data['data']:
        print(f"[{alert['category']}] {alert['title']}")
        print(f"{alert['description']}\n")
else:
    print("No active alerts for Yellowstone")
```

### 3. Find Parks by Activity

```python
import requests

api_key = "YOUR_API_KEY"

# First, find the activity ID for hiking
activities_url = "https://developer.nps.gov/api/v1/activities"
headers = {"X-Api-Key": api_key}
params = {"q": "hiking"}

response = requests.get(activities_url, headers=headers, params=params)
activities = response.json()

if activities['data']:
    hiking_id = activities['data'][0]['id']

    # Now find parks with hiking
    parks_url = "https://developer.nps.gov/api/v1/activities/parks"
    params = {"id": hiking_id}

    response = requests.get(parks_url, headers=headers, params=params)
    data = response.json()

    if data['data']:
        activity = data['data'][0]
        print(f"Parks offering {activity['name']}:")
        print(f"Total: {len(activity['parks'])} parks\n")

        # Show first 10
        for park in activity['parks'][:10]:
            print(f"  - {park['fullName']} ({park['states']})")
```

### 4. Get Campground Details

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://developer.nps.gov/api/v1/campgrounds"
headers = {"X-Api-Key": api_key}
params = {
    "parkCode": "yose",
    "limit": 50
}

response = requests.get(url, headers=headers, params=params)
data = response.json()

print(f"Campgrounds in Yosemite:\n")
for campground in data['data']:
    print(f"{campground['name']}")
    print(f"  Total sites: {campground['campsites']['totalSites']}")
    print(f"  Reservable: {campground['numberOfSitesReservable']}")
    print(f"  First-come: {campground['numberOfSitesFirstComeFirstServe']}")

    # Show fees
    if campground['fees']:
        for fee in campground['fees']:
            print(f"  {fee['title']}: ${fee['cost']}")

    # Show amenities
    amenities = campground['amenities']
    print(f"  Potable water: {amenities.get('potableWater', ['No'])[0]}")
    print(f"  Showers: {amenities.get('showers', ['None'])[0]}")
    print(f"  RV max length: {campground['accessibility'].get('rvMaxLength', 'N/A')} ft")
    print()
```

### 5. Get Upcoming Events

```python
import requests
from datetime import datetime, timedelta

api_key = "YOUR_API_KEY"
url = "https://developer.nps.gov/api/v1/events"
headers = {"X-Api-Key": api_key}

# Events in the next 30 days
today = datetime.now().strftime("%Y-%m-%d")
future = (datetime.now() + timedelta(days=30)).strftime("%Y-%m-%d")

params = {
    "parkCode": "yell",
    "dateStart": today,
    "dateEnd": future,
    "pageSize": 50
}

response = requests.get(url, headers=headers, params=params)
data = response.json()

if data['data']:
    print(f"Upcoming events at Yellowstone:\n")
    for event in data['data']:
        print(f"{event['title']}")
        print(f"  Date: {event.get('dateStart', 'TBD')}")
        print(f"  Location: {event.get('location', 'Various')}")
        print(f"  {event.get('description', '')[:100]}...")
        print()
```

### 6. Search Parks by Keyword

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://developer.nps.gov/api/v1/parks"
headers = {"X-Api-Key": api_key}
params = {
    "q": "waterfall",
    "limit": 20
}

response = requests.get(url, headers=headers, params=params)
data = response.json()

print(f"Parks mentioning 'waterfall':\n")
for park in data['data']:
    print(f"{park['fullName']}")
    print(f"  States: {park['states']}")
    # Show first 100 chars of description
    print(f"  {park['description'][:100]}...")
    print()
```

### 7. Get Visitor Center Information

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://developer.nps.gov/api/v1/visitorcenters"
headers = {"X-Api-Key": api_key}
params = {
    "parkCode": "grca",
    "limit": 20
}

response = requests.get(url, headers=headers, params=params)
data = response.json()

print(f"Visitor centers at Grand Canyon:\n")
for vc in data['data']:
    print(f"{vc['name']}")

    # Operating hours
    if vc['operatingHours']:
        hours = vc['operatingHours'][0]
        print(f"  Hours: {hours['description']}")

    # Contact
    if vc['contacts'].get('phoneNumbers'):
        phone = vc['contacts']['phoneNumbers'][0]
        print(f"  Phone: {phone['phoneNumber']}")

    print(f"  URL: {vc['url']}")
    print()
```

---

## 🆘 Support & Additional Resources

- **Documentation:** https://www.nps.gov/subjects/developer/api-documentation.htm
- **Get API Key:** https://www.nps.gov/subjects/developer/get-started.htm
- **Guides:** https://www.nps.gov/subjects/developer/guides.htm
- **Support:** https://www.nps.gov/subjects/developer/get-started.htm#csh-contact
- **NPS Website:** https://www.nps.gov/
- **Park Finder:** https://www.nps.gov/findapark/index.htm
