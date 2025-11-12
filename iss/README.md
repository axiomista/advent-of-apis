# Open Notify - ISS Location API

## Description

Open Notify provides APIs for tracking the International Space Station (ISS). Get real-time location data including latitude, longitude, and timestamp of the ISS as it orbits Earth. The API also provides information about astronauts and cosmonauts currently in space.

**Note:** ISS pass predictions have been turned off. Only real-time ISS location and people in space data are currently available.

## Base URL

```
http://api.open-notify.org
```

## Authentication

No API key required.

## Example Usage

### Get Current ISS Location

**cURL:**
```bash
curl "http://api.open-notify.org/iss-now.json"
```

**Python:**
```python
import requests
from datetime import datetime

url = "http://api.open-notify.org/iss-now.json"
response = requests.get(url)
data = response.json()

if data['message'] == 'success':
    position = data['iss_position']
    timestamp = data['timestamp']

    print(f"ISS Location at {datetime.fromtimestamp(timestamp)}:")
    print(f"Latitude: {position['latitude']}")
    print(f"Longitude: {position['longitude']}")
```

**JavaScript:**
```javascript
const url = 'http://api.open-notify.org/iss-now.json';

fetch(url)
  .then(response => response.json())
  .then(data => {
    if (data.message === 'success') {
      const position = data.iss_position;
      const timestamp = new Date(data.timestamp * 1000);
      console.log(`ISS Location at ${timestamp}:`);
      console.log(`Latitude: ${position.latitude}`);
      console.log(`Longitude: ${position.longitude}`);
    }
  })
  .catch(error => console.error('Error:', error));
```

**Example Response:**
```json
{
  "iss_position": {
    "latitude": "-2.7126",
    "longitude": "142.0640"
  },
  "timestamp": 1762972501,
  "message": "success"
}
```

### Get Number of People in Space

**cURL:**
```bash
curl "http://api.open-notify.org/astros.json"
```

**Python:**
```python
import requests

url = "http://api.open-notify.org/astros.json"
response = requests.get(url)
data = response.json()

if data['message'] == 'success':
    print(f"Number of people in space: {data['number']}\n")
    for person in data['people']:
        print(f"{person['name']} - {person['craft']}")
```

**JavaScript:**
```javascript
const url = 'http://api.open-notify.org/astros.json';

fetch(url)
  .then(response => response.json())
  .then(data => {
    if (data.message === 'success') {
      console.log(`Number of people in space: ${data.number}\n`);
      data.people.forEach(person => {
        console.log(`${person.name} - ${person.craft}`);
      });
    }
  })
  .catch(error => console.error('Error:', error));
```

**Example Response:**
```json
{
  "people": [
    {"craft": "ISS", "name": "Oleg Kononenko"},
    {"craft": "ISS", "name": "Tracy Caldwell Dyson"},
    {"craft": "Tiangong", "name": "Li Guangsu"}
  ],
  "number": 12,
  "message": "success"
}
```

## Available Endpoints

- `/iss-now.json` - Current ISS location (latitude, longitude, timestamp)
- `/astros.json` - Number of people currently in space and their spacecraft
- `/iss-pass.json` - **[DEPRECATED]** ISS pass predictions are currently turned off

## Response Format

All responses are in JSON format with a `message` field indicating success or failure. Successful responses have `"message": "success"`.

## Resources

- Documentation: http://open-notify.org/
- GitHub: https://github.com/open-notify/Open-Notify-API
