# 💧 USGS Water Services API

## 📖 Description

Track America's water resources in real-time! The USGS Water Services API delivers current and historical data from thousands of monitoring sites nationwide. Access streamflow measurements, groundwater levels, water quality data, and more to support research, operations, and decision-making.

✨ **Best For:** Flood monitoring, drought tracking, water quality research, and recreational planning

> 💡 **Did you know?** USGS monitors over 1.5 million sites and collects billions of water measurements every year, providing critical data for everything from flood warnings to water supply management!

## 📍 Base URL

```
https://waterservices.usgs.gov/rest
```

## 🔑 Authentication

No API key required! 🎉 Access all water data freely and openly.

---

## 🌊 Available Services

- **⚡ Instantaneous Values (IV)** - Real-time measurements (updated every 15-60 minutes)
- **📅 Daily Values (DV)** - Historical daily data
- **📍 Site Service** - Information about monitoring sites
- **📊 Statistics** - Statistical summaries
- **🕳️ Groundwater Levels** - Historical groundwater data
- **🔬 Water Quality** - Water quality samples and results

---

## 💻 Example Usage

### Get Real-Time Stream Data

**cURL:**
```bash
curl "https://waterservices.usgs.gov/nwis/iv/?format=json&sites=01646500&parameterCd=00060,00065&siteStatus=all"
```

**Python:**
```python
import requests

url = "https://waterservices.usgs.gov/nwis/iv/"
params = {
    "format": "json",
    "sites": "01646500",  # Potomac River site
    "parameterCd": "00060,00065",  # Discharge and gage height
    "siteStatus": "all"
}

response = requests.get(url, params=params)
data = response.json()
print(data)
```

**JavaScript:**
```javascript
const url = 'https://waterservices.usgs.gov/nwis/iv/';
const params = new URLSearchParams({
  format: 'json',
  sites: '01646500',
  parameterCd: '00060,00065',
  siteStatus: 'all'
});

fetch(`${url}?${params}`)
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```

### Search for Sites by State

**cURL:**
```bash
curl "https://waterservices.usgs.gov/nwis/site/?format=rdb&stateCd=va&siteType=ST&hasDataTypeCd=iv"
```

**Python:**
```python
import requests

url = "https://waterservices.usgs.gov/nwis/site/"
params = {
    "format": "json",
    "stateCd": "va",  # Virginia
    "siteType": "ST",  # Stream
    "hasDataTypeCd": "iv"  # Has instantaneous values
}

response = requests.get(url, params=params)
sites = response.json()
print(sites)
```

> 💡 **Pro Tip:** Parameter codes are critical! `00060` = streamflow (cubic ft/sec), `00065` = gage height (feet). Find all codes at https://help.waterdata.usgs.gov/parameter_cd

⚠️ **Note:** For comprehensive parameter details and service-specific options, see [USAGE.md](./USAGE.md)

---

## 🚀 Application Examples

**1. Flood Warning & Monitoring System**
Monitor river levels and provide real-time flood alerts to communities.
- Track stream gage heights at critical locations
- Send alerts when water levels exceed thresholds
- Display historical flood data and trends
- Show forecast conditions based on real-time data
- Map flood-prone areas with current conditions

**2. Agricultural Water Management Dashboard**
Help farmers and water districts optimize irrigation and water use.
- Monitor groundwater levels in agricultural regions
- Track precipitation and streamflow data
- Calculate water availability for irrigation
- Display drought indicators and trends
- Compare current conditions to historical averages

**3. Environmental Research Platform**
Support scientific research on water resources and ecosystems.
- Access long-term water quality datasets
- Analyze temperature and dissolved oxygen trends
- Study seasonal flow patterns
- Export data for statistical analysis
- Visualize spatial and temporal patterns

**4. Recreational Water Information App**
Provide real-time conditions for fishing, boating, and recreation.
- Show current streamflow for fishing locations
- Display water temperature at recreation sites
- Alert users to unsafe water conditions
- Show historical best times for activities
- Map nearby monitoring sites

**5. Municipal Water Supply Dashboard**
Help water utilities monitor source water conditions.
- Track reservoir levels and inflows
- Monitor water quality parameters
- Forecast water availability
- Alert to contamination events
- Generate compliance reports

**6. Drought Monitoring & Response Tool**
Track drought conditions and support water management decisions.
- Compare current flows to historical percentiles
- Map low-flow conditions across regions
- Track groundwater level declines
- Generate drought severity indicators
- Support water restriction decisions

**7. Climate Change Impact Analysis**
Analyze long-term trends in water resources.
- Visualize multi-decade streamflow trends
- Analyze changes in seasonal patterns
- Study glacier and snowmelt timing shifts
- Compare precipitation to runoff
- Project future water availability

**8. Stormwater Management System**
Monitor urban runoff and drainage systems.
- Track real-time precipitation
- Monitor storm drain discharge
- Calculate runoff volumes
- Alert to combined sewer overflows
- Support green infrastructure planning

---

## 📚 Resources

- 📖 **API Documentation:** https://waterservices.usgs.gov/
- 🧪 **Test Tools:** https://waterservices.usgs.gov/test-tools/
- 🔢 **Parameter Codes:** https://help.waterdata.usgs.gov/parameter_cd
- 📘 **Detailed Usage Guide:** [USAGE.md](./USAGE.md)
