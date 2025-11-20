# 🍎 USDA Food Access Research Atlas API

## 📖 Description

Discover food deserts and understand food access across America! The USDA Food Access Research Atlas provides comprehensive census-tract level data on food access, mapping areas with limited access to affordable and nutritious food. Explore how distance to grocery stores, income levels, and vehicle availability intersect to create food deserts in communities nationwide.

✨ **Best For:** Food security research, community health planning, urban development, and social equity applications

> 💡 **Did you know?** A food desert is typically defined as a low-income census tract where a significant number of residents don't have easy access to a supermarket or large grocery store. The USDA tracks over 6,500 food desert tracts affecting millions of Americans!

## 📍 Base URL

```
https://gisportal.ers.usda.gov/server/rest/services/FARA
```

## 🔑 Authentication

No API key required! 🎉 Start exploring food access data immediately!

---

## 💻 Example Usage

### Get Food Access Data for Census Tracts

**cURL:**
```bash
curl "https://gisportal.ers.usda.gov/server/rest/services/FARA/FARA_2019/MapServer/1/query?where=1%3D1&outFields=*&f=json&resultRecordCount=10"
```

**Python:**
```python
import requests

url = "https://gisportal.ers.usda.gov/server/rest/services/FARA/FARA_2019/MapServer/1/query"
params = {
    "where": "LILATracts_1And10 = 1",  # Low income and low access at 1 and 10 miles
    "outFields": "GEOID10,St_Name,Cnty_Name,POP2010,LowIncomeTracts,LILATracts_1And10",
    "f": "json",
    "resultRecordCount": 100
}

response = requests.get(url, params=params)
data = response.json()

print(f"Found {len(data['features'])} food desert census tracts\n")
for feature in data['features'][:5]:
    attrs = feature['attributes']
    print(f"Tract {attrs['GEOID10']} - {attrs['Cnty_Name']}, {attrs['St_Name']}")
    print(f"  Population: {attrs['POP2010']}")
    print(f"  Low Income: {'Yes' if attrs['LowIncomeTracts'] == 1 else 'No'}")
    print()
```

**JavaScript:**
```javascript
const url = 'https://gisportal.ers.usda.gov/server/rest/services/FARA/FARA_2019/MapServer/1/query';
const params = new URLSearchParams({
  where: 'LILATracts_1And10 = 1',
  outFields: 'GEOID10,St_Name,Cnty_Name,POP2010,LowIncomeTracts',
  f: 'json',
  resultRecordCount: 100
});

fetch(`${url}?${params}`)
  .then(response => response.json())
  .then(data => {
    console.log(`Found ${data.features.length} food desert tracts`);
    data.features.slice(0, 5).forEach(feature => {
      const attrs = feature.attributes;
      console.log(`${attrs.Cnty_Name}, ${attrs.St_Name}`);
      console.log(`  Population: ${attrs.POP2010}`);
    });
  })
  .catch(error => console.error('Error:', error));
```

---

## 📂 Available Datasets

- **FARA_2010** - Food access data from 2010 Census
- **FARA_2015** - Food access data from 2015 update
- **FARA_2019** - Food access data from 2019 (most recent)

> 💡 **Pro Tip:** Use the 2019 dataset for the most current food access information. Each dataset contains 10+ layers with different access metrics (1 mile, 10 miles, 20 miles, vehicle access)!

---

## 🔧 Common Query Parameters

ArcGIS REST API standard parameters:
- `where` - SQL query to filter features (e.g., `LILATracts_1And10 = 1`)
- `outFields` - Comma-separated field names or `*` for all
- `geometry` - Spatial filter geometry
- `f` - Response format (`json`, `geojson`, `pbf`)
- `resultRecordCount` - Limit number of results
- `returnGeometry` - Include geometry (`true`/`false`)

⚠️ **Note:** For complete field listings, layer details, and advanced query options, see [USAGE.md](./USAGE.md)

---

## 🚀 Application Examples

**1. Food Desert Mapper**
Interactive map showing food deserts and underserved areas.
- Visualize low-income, low-access areas on map
- Color-code by severity (1 mile, 10 miles, 20 miles)
- Show distance to nearest supermarket
- Display population affected in each tract
- Filter by urban vs rural areas
- Overlay with demographic data

**2. Community Needs Assessment Tool**
Help nonprofits and planners identify priority areas.
- Identify communities most in need of grocery stores
- Calculate food desert scores by neighborhood
- Generate reports for grant applications
- Show overlap with other social determinants
- Track changes over time (2010 vs 2015 vs 2019)
- Export priority area lists

**3. Grocery Store Site Selection**
Help businesses find optimal locations for new stores.
- Identify underserved markets with demand
- Analyze competition and saturation
- Show population density and income levels
- Calculate potential customer base
- Assess vehicle ownership patterns
- Generate market opportunity scores

**4. Public Health Dashboard**
Connect food access to health outcomes and interventions.
- Correlate food deserts with obesity rates
- Identify areas for mobile food markets
- Plan community garden locations
- Target nutrition education programs
- Show proximity to healthcare facilities
- Track intervention effectiveness over time

**5. Transportation Planning Tool**
Plan transit routes to improve food access.
- Show areas with low vehicle access
- Identify transit gaps to grocery stores
- Optimize bus routes for food access
- Calculate walk times to stores
- Plan bike lane connections
- Measure improvement after transit changes

**6. Social Equity Analysis**
Research disparities in food access by demographics.
- Compare food access across racial/ethnic groups
- Analyze income inequality in food access
- Identify environmental justice concerns
- Track access changes in gentrifying areas
- Generate equity reports for policymakers
- Support civil rights investigations

**7. Emergency Food Response**
Plan food distribution during disasters or emergencies.
- Identify vulnerable populations during crisis
- Plan emergency food distribution sites
- Coordinate with food banks and charities
- Show areas with limited transportation
- Map existing food assistance locations
- Optimize delivery routes for aid

**8. SNAP Retailer Outreach**
Help expand SNAP acceptance at stores.
- Find high-need areas with few SNAP retailers
- Identify corner stores for SNAP enrollment
- Calculate potential SNAP benefit usage
- Show population receiving SNAP
- Target retailer recruitment efforts
- Monitor SNAP acceptance coverage

---

## 📚 Resources

- 📖 **API Documentation:** https://www.ers.usda.gov/developer/geospatial-apis
- 📊 **Atlas Website:** https://www.ers.usda.gov/data-products/food-access-research-atlas
- 📥 **Download Data:** https://www.ers.usda.gov/data-products/food-access-research-atlas/download-the-data
- 🗺️ **Interactive Map:** https://gisportal.ers.usda.gov/portal/apps/experiencebuilder/experience/?id=a53ebd7396cd4ac3a3ed09137676fd40
- 📘 **Detailed Usage Guide:** [USAGE.md](./USAGE.md)
