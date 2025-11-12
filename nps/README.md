# National Parks Service API

## Description

The NPS API provides authoritative National Park Service data accessible through a free API key. Access information about parks, alerts, activities, campgrounds, visitor centers, and more for integration into applications, maps, and websites.

## Base URL

```
https://developer.nps.gov/api/v1
```

## Authentication

Requires an API key sent in the HTTP request header as `X-Api-Key`.

## Example Usage

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

## Common Endpoints

- `/parks` - Park information
- `/alerts` - Current park alerts
- `/campgrounds` - Campground details
- `/visitorcenters` - Visitor center information
- `/activities` - Available activities
- `/events` - Park events

## Common Parameters

- `parkCode` - 4-letter park code (e.g., yell, grca, yose)
- `stateCode` - 2-letter state code (e.g., CA, NY, TX)
- `limit` - Maximum number of results
- `q` - Search query

## Resources

- Documentation: https://www.nps.gov/subjects/developer/api-documentation.htm
- Get API Key: https://www.nps.gov/subjects/developer/get-started.htm
- Park Codes: https://www.nps.gov/subjects/developer/guides.htm
