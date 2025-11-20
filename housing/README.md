# 🏘️ HUD Multifamily Assisted Housing API

## 📖 Description

Discover affordable housing for seniors and families across America! The HUD Multifamily Assisted Housing API provides comprehensive data on federally-assisted housing properties, including Section 202 supportive housing for the elderly, Section 8 project-based assistance, and Section 811 housing for persons with disabilities. Access property locations, unit counts, inspection scores, demographics, and financial data for thousands of affordable housing developments nationwide.

✨ **Best For:** Housing search tools, senior services, community planning, and housing policy research

> 💡 **Did you know?** HUD assists nearly 5 million households through multifamily housing programs, including over 400,000 units specifically designed for elderly residents through the Section 202 program!

## 📍 Base URL

```
https://services.arcgis.com/VTyQ9soqVukalItT/arcgis/rest/services/Multifamily_Properties_Assisted/FeatureServer
```

## 🔑 Authentication

No API key required! 🎉 Start accessing housing data immediately!

---

## 💻 Example Usage

### Find Section 202 Properties (Senior Housing)

**cURL:**
```bash
curl "https://services.arcgis.com/VTyQ9soqVukalItT/arcgis/rest/services/Multifamily_Properties_Assisted/FeatureServer/0/query?where=IS_202_811_IND%3D1&outFields=PROPERTY_NAME_TEXT,ADDRESS_LINE1_TEXT,PLACED_BASE_CITY_NAME_TEXT,TOTAL_UNIT_COUNT,REAC_LAST_INSPECTION_SCORE&f=json&resultRecordCount=10"
```

**Python:**
```python
import requests

url = "https://services.arcgis.com/VTyQ9soqVukalItT/arcgis/rest/services/Multifamily_Properties_Assisted/FeatureServer/0/query"
params = {
    "where": "IS_202_811_IND = 1",  # Section 202/811 properties
    "outFields": "PROPERTY_NAME_TEXT,ADDRESS_LINE1_TEXT,PLACED_BASE_CITY_NAME_TEXT,TOTAL_UNIT_COUNT,REAC_LAST_INSPECTION_SCORE",
    "f": "json",
    "resultRecordCount": 100
}

response = requests.get(url, params=params)
data = response.json()

print(f"Found {len(data['features'])} Section 202/811 properties\n")

for feature in data['features'][:5]:
    attrs = feature['attributes']
    print(f"{attrs['PROPERTY_NAME_TEXT']}")
    print(f"  Address: {attrs['ADDRESS_LINE1_TEXT']}, {attrs['PLACED_BASE_CITY_NAME_TEXT']}")
    print(f"  Total Units: {attrs['TOTAL_UNIT_COUNT']}")
    print(f"  Inspection Score: {attrs['REAC_LAST_INSPECTION_SCORE']}")
    print()
```

**JavaScript:**
```javascript
const url = 'https://services.arcgis.com/VTyQ9soqVukalItT/arcgis/rest/services/Multifamily_Properties_Assisted/FeatureServer/0/query';
const params = new URLSearchParams({
  where: 'IS_202_811_IND = 1',
  outFields: 'PROPERTY_NAME_TEXT,ADDRESS_LINE1_TEXT,PLACED_BASE_CITY_NAME_TEXT,TOTAL_UNIT_COUNT',
  f: 'json',
  resultRecordCount: 100
});

fetch(`${url}?${params}`)
  .then(response => response.json())
  .then(data => {
    console.log(`Found ${data.features.length} senior housing properties`);
    data.features.slice(0, 5).forEach(feature => {
      const attrs = feature.attributes;
      console.log(`${attrs.PROPERTY_NAME_TEXT}`);
      console.log(`  ${attrs.PLACED_BASE_CITY_NAME_TEXT}: ${attrs.TOTAL_UNIT_COUNT} units`);
    });
  })
  .catch(error => console.error('Error:', error));
```

---

## 🏢 Property Types

- **Section 202** - Supportive Housing for the Elderly (62+)
- **Section 811** - Supportive Housing for Persons with Disabilities
- **Section 8** - Project-Based Rental Assistance
- **Section 236** - Interest Reduction Payment Program
- **Other HUD Programs** - Various assistance and loan programs

> 💡 **Pro Tip:** Use the `IS_202_811_IND` field to filter specifically for elderly/disabled supportive housing. Use `PROGRAM_TYPE1` and `PROGRAM_TYPE2` for more granular program filtering!

---

## 🔧 Common Query Parameters

ArcGIS REST API standard parameters:
- `where` - SQL query to filter properties (e.g., `IS_202_811_IND = 1`)
- `outFields` - Comma-separated field names or `*` for all
- `geometry` - Spatial filter (point, bbox, polygon)
- `f` - Response format (`json`, `geojson`, `pbf`)
- `resultRecordCount` - Limit number of results
- `returnGeometry` - Include geometry (`true`/`false`)
- `orderByFields` - Sort results (e.g., `TOTAL_UNIT_COUNT DESC`)

⚠️ **Note:** For complete field listings (150+ fields!), query examples, and advanced options, see [USAGE.md](./USAGE.md)

---

## 🚀 Application Examples

**1. Senior Housing Locator**
Help elderly residents find affordable housing options.
- Search Section 202 properties by location
- Show available units and vacancy status
- Display property amenities and features
- Filter by accessibility requirements
- Show proximity to medical facilities
- Provide contact information and applications

**2. Housing Quality Dashboard**
Monitor and display housing inspection scores.
- Show REAC inspection scores by property
- Identify properties with deficiencies
- Track inspection score trends over time
- Map properties by quality rating
- Generate watchlist for low-scoring properties
- Compare scores across cities/states

**3. Affordable Housing Inventory**
Comprehensive database of assisted housing stock.
- Catalog all HUD-assisted properties
- Show unit counts by bedroom size
- Display assistance program mix
- Track contract expiration dates
- Identify preservation priorities
- Generate inventory reports by region

**4. Community Resource Mapper**
Integrate housing with community services.
- Map assisted housing near transit
- Show proximity to grocery stores
- Display nearby healthcare facilities
- Identify food desert locations
- Show senior centers and services
- Plan service delivery routes

**5. Housing Waitlist Coordinator**
Help manage and prioritize housing applications.
- Track properties with shortest waitlists
- Show properties accepting applications
- Display estimated wait times
- Match applicants to appropriate properties
- Show alternative options nearby
- Generate referral lists

**6. Fair Housing Analysis Tool**
Research housing patterns and disparities.
- Analyze geographic distribution of assisted housing
- Compare demographics across properties
- Identify underserved communities
- Track income and rent patterns
- Generate equity reports
- Support civil rights investigations

**7. Property Financial Monitor**
Track financial health of housing portfolio.
- Monitor FASS performance scores
- Identify properties at financial risk
- Track mortgage delinquencies
- Show debt service coverage ratios
- Generate financial alerts
- Support portfolio management decisions

**8. Emergency Housing Response**
Coordinate housing during disasters or crises.
- Identify properties with available units
- Show properties by hazard zone
- Track displaced resident relocations
- Coordinate emergency placements
- Show properties with generators/backup power
- Map evacuation routes from properties

---

## 📚 Resources

- 📖 **HUD Open Data:** https://hudgis-hud.opendata.arcgis.com/
- 🔍 **Data Catalog:** https://hudgis-hud.opendata.arcgis.com/search?q=multifamily
- 📊 **HUD USER:** https://www.huduser.gov/portal/datasets/assthsg.html
- 🏘️ **Section 202 Info:** https://www.hud.gov/program_offices/housing/mfh/progdesc/eld202
- 📋 **Multifamily Housing:** https://www.hud.gov/program_offices/housing/mfh
- 📘 **Detailed Usage Guide:** [USAGE.md](./USAGE.md)
