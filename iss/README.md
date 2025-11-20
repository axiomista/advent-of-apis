# 🛰️ Open Notify - ISS Location API

## 📖 Description

Track the International Space Station in real-time as it orbits Earth! Open Notify provides live location data for the ISS and information about astronauts currently in space. Perfect for space enthusiasts, educators, and developers building astronomy applications.

✨ **Best For:** Space tracking apps, educational tools, astronomy projects, and ISS spotting

> 💡 **Did you know?** The ISS travels at 17,500 mph and completes an orbit around Earth every 90 minutes, passing over different parts of the planet with each orbit!

**⚠️ Note:** ISS pass predictions have been turned off. Only real-time ISS location and people in space data are currently available.

## 📍 Base URL

```
http://api.open-notify.org
```

## 🔑 Authentication

No API key required! 🎉 Start tracking the ISS immediately!

---

## 💻 Example Usage

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

> 💡 **Pro Tip:** The ISS location updates continuously! Poll every 5-10 seconds for smooth real-time tracking visualizations.

---

## 🌐 Available Endpoints

- 📍 `/iss-now.json` - Current ISS location (latitude, longitude, timestamp)
- 👨‍🚀 `/astros.json` - Number of people currently in space and their spacecraft
- ⛔ `/iss-pass.json` - **[DEPRECATED]** ISS pass predictions are currently turned off

⚠️ **Note:** For complete API details and integration patterns, see [USAGE.md](./USAGE.md)

---

## 📦 Response Format

All responses are in JSON format with a `message` field indicating success or failure. Successful responses have `"message": "success"`.

---

## 🚀 Application Examples

**1. Real-Time ISS Tracker**
Display current ISS location on an interactive world map.
- Update ISS position every few seconds
- Show orbital path and trajectory
- Display speed and altitude
- Zoom to current location
- Show day/night overlay on map
- Indicate when ISS is overhead

**2. ISS Viewing Calculator**
Help people find the best times to see the ISS.
- Enter user location for personalized viewing times
- Calculate visibility windows based on time of day and weather
- Show compass direction and elevation angle
- Set reminders for optimal viewing
- Display magnitude (brightness) of pass
- Show duration of visible pass

**3. Space Education Platform**
Teach students about the ISS and space missions.
- Real-time ISS location visualization
- Lessons about orbital mechanics
- Information about ISS modules and structure
- Timeline of ISS construction and missions
- Astronaut biographies
- Live feeds from ISS when available

**4. Astronaut Directory**
Track who is currently in space.
- List all people currently in space
- Show their spacecraft assignment
- Display astronaut photos and bios
- Show mission duration
- Track crew changes over time
- Display country of origin

**5. ISS Desktop Widget**
Lightweight desktop application for ISS enthusiasts.
- Minimal interface showing current position
- System tray notifications when ISS is overhead
- Quick access to detailed information
- Low resource usage
- Auto-start with operating system
- Customizable update intervals

**6. Mobile ISS Spotter**
Smartphone app for ISS tracking on the go.
- GPS-based location detection
- Augmented reality view to point at ISS
- Notifications when ISS is visible nearby
- Offline mode with cached data
- Share sightings with friends
- Photo integration for captures

**7. Space Station Dashboard**
Comprehensive information hub for space enthusiasts.
- Current ISS location and altitude
- Number of people in space
- Recent spacewalks and activities
- Upcoming cargo missions
- Live Twitter feeds from astronauts
- ISS research highlights

**8. STEM Classroom Tool**
Interactive tool for science education.
- Project ISS location on classroom screen
- Calculate distance from school to ISS
- Show time zones ISS passes through
- Demonstrate orbital velocity concepts
- Track ISS over multiple class periods
- Export data for student projects

---

## 📚 Resources

- 📖 **Documentation:** http://open-notify.org/
- 💻 **GitHub:** https://github.com/open-notify/Open-Notify-API
- 📘 **Detailed Usage Guide:** [USAGE.md](./USAGE.md)
