# Google Civic Information API

## Description

The Google Civic Information API enables developers to access voting and election data including voter registration details, polling locations, ballot information, and election administration contact information. The API helps build applications that inform citizens about their voting options and requirements.

## Base URL

```
https://www.googleapis.com/civicinfo/v2
```

## Authentication

Requires a Google API key. Include it in each request as the `key` parameter.

## Example Usage

### Get Available Elections

**cURL:**
```bash
curl "https://www.googleapis.com/civicinfo/v2/elections?key=YOUR_API_KEY"
```

**Python:**
```python
import requests

api_key = "YOUR_API_KEY"
url = "https://www.googleapis.com/civicinfo/v2/elections"
params = {"key": api_key}

response = requests.get(url, params=params)
elections = response.json()
print(elections)
```

**JavaScript:**
```javascript
const apiKey = 'YOUR_API_KEY';
const url = `https://www.googleapis.com/civicinfo/v2/elections?key=${apiKey}`;

fetch(url)
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```

### Get Voter Information

**cURL:**
```bash
curl "https://www.googleapis.com/civicinfo/v2/voterinfo?key=YOUR_API_KEY&address=340%20Main%20St.%20Venice%20CA&electionId=2000"
```

**Python:**
```python
import requests

api_key = "YOUR_API_KEY"
url = "https://www.googleapis.com/civicinfo/v2/voterinfo"
params = {
    "key": api_key,
    "address": "340 Main St. Venice CA",
    "electionId": "2000"
}

response = requests.get(url, params=params)
voter_info = response.json()
print(voter_info)
```

## Resources

- Documentation: https://developers.google.com/civic-information/docs/using_api
- API Console: https://console.developers.google.com/
