# Smart Urban Heat Map API Documentation <!-- omit in toc -->

[![de](https://img.shields.io/badge/lang-de-green.svg)](de)
[![en](https://img.shields.io/badge/lang-en-red.svg)](./)


This documentation describes the **Open-Data API** of the [Smart Urban Heat Map Bern Project](https://urban-heat.meteotest.ch).  

The Smart Urban Heat Map is an initiative of the [Smart City Verein Bern](https://www.smartcity-bern.ch/) to visualize urban heat in the city and region of Bern. Valuable pioneering work has been done by the Climatology Group at the [Geographical Institute of the University of Bern (GIUB)](https://www.geography.unibe.ch/index_eng.html), which has been operating an urban measurement network since 2018, consisting of around 80 stations. The Smart City Verein Bern, together with the company [Abilium GmbH](https://www.abilium.io/), the [Bern University of Applied Sciences](https://www.bfh.ch/de/forschung/forschungsbereiche/public-sector-transformation/) and the company [Meteotest](https://meteotest.ch/), has extended this measuring network into the Bern region by around 40 measuring stations.

Based on this measurement network, the Smart Urban Heat Map API offers access to detailed city climate data for the region of Bern, Switzerland. Users can retrieve current measurements of temperature, relative humidity, and location metadata, as well as location-bound time series data. The data provides valuable insights for urban planning and environmental studies.

The documentation is complemented by an [interactive OpenAPI specification](Swagger) and a [Jupyter Notebook](python_examples.ipynb) which includes examples of how to request and visualize the data using Python.  
The notebook can be run **locally or directly in the browser** using [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/JurekMueller/SUH_Bern_API_Doc/main?labpath=python_examples.ipynb)
or [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/JurekMueller/SUH_Bern_API_Doc/blob/main/python_examples.ipynb).

**Licensing Information**  
The data from the API is available under the [Creative Commons Attribution License (CC-BY)](https://creativecommons.org/licenses/by/4.0/).  
Please ensure that you provide proper attribution when using or redistributing this data in your projects or applications.  
*Attribution Example:* Data provided by the Smart Urban Heat Map Project for Bern, Switzerland.

**Contact Information**  
For questions regarding the data, please contact the [BFH](mailto:jurek.mueller@bfh.ch).  
For technical questions regarding the API, please contact [Meteotest](mailto:office@meteotest.ch).

**Table of Contents**
- [Changelog](#changelog)
- [Stations, Sensors and Temperature Bias](#stations-sensors-and-temperature-bias)
- [Endpoints](#endpoints)
- [Codebook](#codebook)
- [Example Requests](#example-requests)

## Changelog

### API Version 1.0 <!-- omit in toc -->

This is the first version of the Smart Urban Heat Map API published on 09.10.2023.

## Stations, Sensors and Temperature Bias
The measuring stations are built by [Abilium GmbH](https://www.abilium.io/) and are based on the [SHT41A](https://www.mouser.ch/datasheet/2/682/Datasheet_SHT4x-3003109.pdf) Sensorion sensors.  
The self-sufficient stations are outfitted with a small solar panel and measure temperature and relative humidity every 10 minutes.  
The measurement data is sent out via the Helium LoRaWAN network.  
To reduce the potential temperature bias when stations are exposed to direct sunlight, all stations are ventilated before measuring.  
**However, in some cases, especially during the day, measured temperatures might still be slightly higher than actual temperatures.**

## Endpoints

### stations <!-- omit in toc -->

Retrieves station data including most recent measured value for:

* Temperature is in Celsius (°C).
* Relative Humidity is in percentage (%).

**URL:** https://urban-heat.meteotest.ch/api/1.0/stations  
**Response Formats:** `GeoJSON` (default), `CSV`  

### timeseries <!-- omit in toc -->
Retrieves time series based on stationId for:
* Temperature is in Celsius (°C).
* Relative Humidity is in percentage (%).

**URL:** https://urban-heat.meteotest.ch/api/1.0/timeseries  
**Response Formats:** `JSON` (default), `CSV`  
**URL Parameter:**
  * **stationId** (required): specifies from which station to return the time series
  * **timeFrom** (optional, default: "-24hours"): specifies start of time series (Examples: "-3days","-24hours","-30minutes", "2023-10-01T00:00:00Z")
  * **timeTo** (optional, default: "now"): specifies end of time series (Examples: "-3days","-24hours","-30minutes", "now", "2023-10-01T00:00:00Z")

## Codebook

### stations <!-- omit in toc -->

- **coordinates**: Array representing the geographical coordinates (in WGS84) of the station (Latitude,Longitude) 
- **stationId**: Unique identifier for the station (Example: "0F40CBFEFFE70FFE")
- **name**: Name of the station (Example: "Sandrain-Bern")
- **dateObserved**: Date and time of the last measurement (Example: "2023-08-01T12:00:00Z")
- **temperature**: Last temperature measured at the station in °C (Example: 18.925001)
- **relativeHumidity**: Last relative humidity measured at the station in % (Example: 60.971848)

### timeseries <!-- omit in toc -->

- **stationId**: Unique identifier for the station (Example: "0F40CBFEFFE70FFE")
- **dateObserved**: Date and time of the measurement (Example: "2023-08-01T12:00:00Z")
- **temperature**: Temperature measured at the station in °C (Example: 18.925001)
- **relativeHumidity**: Relative humidity measured at the station in % (Example: 60.971848)

## Example Requests

### Request list of stations and most recent measurements  <!-- omit in toc -->
`GET https://urban-heat.meteotest.ch/api/1.0/stations`

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [
          46.94067,
          7.43141
        ]
      },
      "properties": {
        "stationId": "3A0551FEFF6E959E",
        "name": "Eigerplatz-Bern",
        "dateObserved": "2023-10-05T11:36:29Z",
        "temperature": 18.925001,
        "relativeHumidity": 60.971848
      }
    },
    {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [
          46.96681,
          7.439139
        ]
      },
      "properties": {
        "stationId": "140551FEFF6E959E",
        "name": "Worblen-Ostermundigen",
        "dateObserved": "2023-10-05T11:36:27Z",
        "temperature": 18.791485,
        "relativeHumidity": 62.507286
      }
    },
    ...
  ]
}
```

### Request timeseries for a station  <!-- omit in toc -->

`GET https://urban-heat.meteotest.ch/api/1.0/timeseries?stationId=D33FCBFEFFE70FFE&timeFrom=2023-10-01T00:00:00Z&timeTo=2023-10-31T23:00:00Z`

```json
{
  "stationId": "D33FCBFEFFE70FFE", 
  "values": [
    {
      "dateObserved": "2023-10-01T00:05:45Z", 
      "temperature": 14.193179, 
      "relativeHumidity": 86.94652
    },
    {
      "dateObserved": "2023-10-01T00:15:45Z", 
      "temperature": 14.20386, 
      "relativeHumidity": 87.801025
    },
    {
      "dateObserved": "2023-10-01T00:25:45Z", 
      "temperature": 14.03563, 
      "relativeHumidity": 88.541084
    },
    ...
  ]
}
```