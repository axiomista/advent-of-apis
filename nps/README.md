# 🏞️ National Parks Service API

## 📖 Description

Explore America's national treasures with authoritative data from the National Park Service! The NPS API provides comprehensive information about parks, alerts, activities, campgrounds, visitor centers, and more. Perfect for building trip planners, educational tools, and outdoor recreation apps.

✨ **Best For:** Trip planning apps, park discovery tools, educational platforms, and outdoor recreation guides

> 💡 **Did you know?** The National Park Service manages over 400 parks, monuments, and historic sites across the United States, welcoming more than 300 million visitors annually!

## 📍 Base URL

```
https://developer.nps.gov/api/v1
```

## 🔑 Authentication

Requires an API key sent in the HTTP request header as `X-Api-Key`.

**Get your API key:** https://www.nps.gov/subjects/developer/get-started.htm

---

## 💻 Example Usage

### Get Park Alerts

**cURL:**
```bash
curl -H "X-Api-Key: YOUR_API_KEY" "https://developer.nps.gov/api/v1/alerts?parkCode=yell"
```

**Python:**
```python
import requests

api_key = "YOUR_API_KEY"
url = "https://developer.nps.gov/api/v1/alerts"
headers = {"X-Api-Key": api_key}
params = {"parkCode": "yell"}  # Yellowstone

response = requests.get(url, headers=headers, params=params)
alerts = response.json()

for alert in alerts['data']:
    print(f"Title: {alert['title']}")
    print(f"Category: {alert['category']}")
    print(f"Description: {alert['description']}")
    print()
```

**JavaScript:**
```javascript
const apiKey = 'YOUR_API_KEY';
const url = 'https://developer.nps.gov/api/v1/alerts?parkCode=yell';

fetch(url, {
  headers: {
    'X-Api-Key': apiKey
  }
})
  .then(response => response.json())
  .then(data => {
    data.data.forEach(alert => {
      console.log(`Title: ${alert.title}`);
      console.log(`Category: ${alert.category}`);
      console.log(`Description: ${alert.description}\n`);
    });
  })
  .catch(error => console.error('Error:', error));
```

### Get Parks Information

**cURL:**
```bash
curl -H "X-Api-Key: YOUR_API_KEY" "https://developer.nps.gov/api/v1/parks?stateCode=CA&limit=5"
```

**Python:**
```python
import requests

api_key = "YOUR_API_KEY"
url = "https://developer.nps.gov/api/v1/parks"
headers = {"X-Api-Key": api_key}
params = {
    "stateCode": "CA",  # California
    "limit": 5
}

response = requests.get(url, headers=headers, params=params)
parks = response.json()

for park in parks['data']:
    print(f"{park['fullName']} ({park['parkCode']})")
    print(f"Description: {park['description']}")
    print(f"URL: {park['url']}")
    print()
```

**JavaScript:**
```javascript
const apiKey = 'YOUR_API_KEY';
const url = 'https://developer.nps.gov/api/v1/parks';
const params = new URLSearchParams({
  stateCode: 'CA',
  limit: 5
});

fetch(`${url}?${params}`, {
  headers: {
    'X-Api-Key': apiKey
  }
})
  .then(response => response.json())
  .then(data => {
    data.data.forEach(park => {
      console.log(`${park.fullName} (${park.parkCode})`);
      console.log(`Description: ${park.description}`);
      console.log(`URL: ${park.url}\n`);
    });
  })
  .catch(error => console.error('Error:', error));
```

### Get Activities

**cURL:**
```bash
curl -H "X-Api-Key: YOUR_API_KEY" "https://developer.nps.gov/api/v1/activities/parks?id=09DF0950-D319-4557-A57E-04CD2F63FF42"
```

**Python:**
```python
import requests

api_key = "YOUR_API_KEY"
url = "https://developer.nps.gov/api/v1/activities/parks"
headers = {"X-Api-Key": api_key}
params = {"id": "09DF0950-D319-4557-A57E-04CD2F63FF42"}  # Hiking activity ID

response = requests.get(url, headers=headers, params=params)
data = response.json()

if data['data']:
    activity = data['data'][0]
    print(f"Activity: {activity['name']}")
    print(f"Parks offering this activity: {len(activity['parks'])}")
    for park in activity['parks'][:5]:  # First 5 parks
        print(f"  - {park['fullName']}")
```

---

## 🌐 Available Endpoints

- 🏞️ `/parks` - Park information
- ⚠️ `/alerts` - Current park alerts
- ⛺ `/campgrounds` - Campground details
- ℹ️ `/visitorcenters` - Visitor center information
- 🥾 `/activities` - Available activities
- 📅 `/events` - Park events

> 💡 **Pro Tip:** Use 4-letter park codes for faster queries! Examples: `yell` (Yellowstone), `grca` (Grand Canyon), `yose` (Yosemite), `zion` (Zion)

---

## 🔧 Common Parameters

- `parkCode` - 4-letter park code (e.g., yell, grca, yose)
- `stateCode` - 2-letter state code (e.g., CA, NY, TX)
- `limit` - Maximum number of results (default: 50)
- `start` - Starting index for pagination
- `q` - Search query for text matching

⚠️ **Note:** For complete parameter details, rate limits, and response formats, see [USAGE.md](./USAGE.md)

---

## 🚀 Application Examples

**1. Trip Planner & Itinerary Builder**
Help visitors plan comprehensive trips to national parks.
- Search parks by location, activity, or season
- Create multi-day itineraries
- Show campground and lodging availability
- Display visitor center hours and services
- Provide activity recommendations based on interests
- Export trip plans as PDF

**2. Real-Time Park Conditions Dashboard**
Display current conditions and alerts for park visitors.
- Show active alerts (closures, weather, wildlife)
- Display road conditions and accessibility
- Show current wait times at entrances
- Provide safety warnings
- Map of affected areas
- Subscribe to alerts for specific parks

**3. Park Discovery & Exploration App**
Help people find and learn about national parks.
- Browse parks by state or region
- Filter by activities available
- Display photos and descriptions
- Show proximity to user location
- Read visitor reviews and tips
- Virtual tour features

**4. Educational Platform for Students**
Teach students about national parks and conservation.
- Interactive park maps
- Historical information and timelines
- Ecology and wildlife information
- Virtual field trips
- Curriculum-aligned lesson plans
- Quiz and trivia features

**5. Activity Finder & Recreation Planner**
Match visitors with parks based on desired activities.
- Search by activity type (hiking, camping, fishing, etc.)
- Show difficulty levels and accessibility
- Display trail maps and lengths
- Show required permits and fees
- Link to reservation systems
- Weather-appropriate recommendations

**6. Campground Reservation Helper**
Streamline the campground research and booking process.
- Display campground amenities
- Show availability calendars
- Filter by RV accessibility, group sites, etc.
- Display photos and reviews
- Link to official reservation systems
- Show nearby alternatives if full

**7. Park Accessibility Guide**
Help visitors with disabilities plan accessible visits.
- List accessible trails and facilities
- Show wheelchair-accessible campgrounds
- Display sensory-friendly programs
- Provide service animal policies
- Show accessible transportation options
- Audio descriptions of exhibits

**8. Wildlife & Nature Tracking App**
Help visitors learn about and observe wildlife safely.
- Recent wildlife sightings by park
- Best times/locations for viewing
- Safety guidelines for each species
- Photo identification guides
- Citizen science reporting features
- Seasonal migration patterns

---

## 📚 Resources

- 📖 **API Documentation:** https://www.nps.gov/subjects/developer/api-documentation.htm
- 🔑 **Get API Key:** https://www.nps.gov/subjects/developer/get-started.htm
- 📋 **Park Codes Guide:** https://www.nps.gov/subjects/developer/guides.htm
- 📘 **Detailed Usage Guide:** [USAGE.md](./USAGE.md)
