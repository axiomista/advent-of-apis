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

## Application Examples

**1. Polling Place Finder**
Help voters locate their polling locations for upcoming elections.
- Enter address to find polling place
- Display early voting locations
- Show ballot drop-off sites
- Provide election official contact information
- Multi-language support (13 languages supported)

**2. Voter Information Portal**
Comprehensive election information platform for citizens.
- Display upcoming elections
- Show sample ballots based on address
- List all candidates and measures
- Provide voting deadlines and requirements
- Link to official election resources

**3. Election Day Reminder App**
Notify users about elections and provide voting logistics.
- Alert users to upcoming elections in their area
- Push notifications with polling place details
- Display hours of operation
- Show alternative voting methods (early voting, mail-in)
- Provide transportation/accessibility information

**4. Ballot Preparation Tool**
Help voters research and prepare before Election Day.
- Preview full ballot before voting
- Display candidate information and positions
- Show measure details and implications
- Allow users to mark selections for reference
- Export voting guide as PDF

**5. Election Official Contact Directory**
Connect voters with local election administrators.
- Look up election officials by address
- Display contact methods (phone, email, website)
- Show office hours and locations
- Provide links to official resources
- Enable quick communication for questions

**6. Civic Engagement Platform**
Integrate voting information into broader civic participation tools.
- Combine with volunteer opportunities
- Show election information alongside community events
- Track user participation history
- Provide educational resources about voting
- Connect to voter registration services

**7. News Organization Election Tool**
Embed voting information in election coverage.
- White-label voting tool for news websites
- Display election results alongside voter info
- Show district-specific ballot measures
- Integrate with election night coverage
- Provide historical election data context

**8. Accessibility-Focused Voting App**
Ensure all voters can access election information.
- Screen reader optimization
- Large text and high contrast options
- Simplified language explanations
- Accessibility features at polling places
- Translation into multiple languages

## Resources

- Documentation: https://developers.google.com/civic-information/docs/using_api
- API Console: https://console.developers.google.com/
