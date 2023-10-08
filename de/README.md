# Smart Urban Heat Map API Dokumentation <!-- omit in toc -->

[![de](https://img.shields.io/badge/lang-de-green.svg)](../de)
[![en](https://img.shields.io/badge/lang-en-red.svg)](../)


Diese Dokumentation beschreibt die **Open-Data API** des [Smart Urban Heat Map Bern Projekts](https://smart-urban-heat-map.ch).

Die Smart Urban Heat Map ist eine Initiative des [Smart City Vereins Bern](https://www.smartcity-bern.ch/) zur Visualisierung der städtischen Wärme in der Stadt und Region Bern. Wertvolle Pionierarbeit leistete die Gruppe Klimatologie am [Geographischen Institut der Universität Bern (GIUB)](https://www.geography.unibe.ch/index_eng.html), die seit 2018 ein städtisches Messnetz betreibt, bestehend aus rund 80 Stationen. Der Smart City Verein Bern, zusammen mit der Firma [Abilium GmbH](https://www.abilium.io/), dem [Institut Public Sector Transformation](https://www.bfh.ch/de/forschung/forschungsbereiche/public-sector-transformation/) der Berner Fachhochschule und der Firma [Meteotest](https://meteotest.ch/), hat dieses Messnetz in die Region Bern um rund 40 Messstationen erweitert.

Basierend auf diesem Messnetzwerk bietet die Smart Urban Heat Map API Zugriff auf detaillierte Stadtklimadaten für die Region Bern. Benutzer können aktuelle Messungen von Temperatur und relativer Luftfeuchtigkeit sowie Standortmetadaten und ortsgebundene Zeitreihendaten abrufen. Die Daten liefern wertvolle Erkenntnisse für Stadtplanung und Umweltstudien.

Die Dokumentation wird durch eine [interaktive OpenAPI-Spezifikation](../Swagger) und ein [Jupyter Notebook](../python_examples.ipynb) ergänzt, das Beispiele für das Abrufen und Visualisierung der Daten mit Python enthält.
Das Notizbuch kann **lokal oder direkt im Browser** mit [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/meteotest/urban-heat-API-docs/main?labpath=python_examples.ipynb)
oder [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/meteotest/urban-heat-API-docs/blob/main/python_examples.ipynb) ausgeführt werden.

**Lizenzinformationen**
Die Daten der API stehen unter der [Creative Commons Attribution License (CC-BY)](https://creativecommons.org/licenses/by/4.0/) zur Verfügung.
Bitte stellen Sie sicher, dass Sie bei der Verwendung oder Weitergabe dieser Daten in Ihren Projekten oder Anwendungen die Urheber angeben.
*Attributionsbeispiel:* Daten bereitgestellt vom Smart Urban Heat Map Projekt für Bern, Schweiz.

**Kontaktinformationen**
Bei Fragen zu den Daten wenden Sie sich bitte an die [BFH] (mailto:jurek.mueller@bfh.ch).
Bei technischen Fragen zur API wenden Sie sich bitte an [Meteotest](mailto:office@meteotest.ch).

**Inhaltsverzeichnis**
- [Changelog](#changelog)
- [Stationen, Sensoren und Temperaturverzerrung](#stationen-sensoren-und-temperaturverzerrung)
- [Endpunkte](#endpunkte)
- [Codebuch](#codebuch)
- [Beispielabfragen](#beispielabfragen)

## Changelog

### API Version 1.0 <!-- omit in toc -->

Dies ist die erste Version der Smart Urban Heat Map API, die am 09.10.2023 veröffentlicht wurde.

## Stationen, Sensoren und Temperaturverzerrung
Die Messstationen werden von der [Abilium GmbH](https://www.abilium.io/) gebaut und basieren auf dem [SHT41A](https://www.mouser.ch/datasheet/2/682/Datasheet_SHT4x-3003109.pdf) Sensorion-Sensor.
Die autarken Stationen sind mit einem kleinen Solarpanel ausgestattet und messen alle 10 Minuten die Lufttemperatur und die relative Luftfeuchtigkeit.
Die Messdaten werden über das Helium LoRaWAN-Netzwerk versendet.
Um die mögliche Temperaturverzerrung im Fall einer direkten Sonneneinstrahlung zu verringern, werden alle Stationen vor der Messung belüftet.
**In einigen Fällen, insbesondere tagsüber, können die gemessenen Temperaturen jedoch immer noch etwas höher sein als die tatsächlichen Temperaturen.**

## Endpunkte

### stations <!-- omit in toc -->

Ruft Stationsdaten ab, einschliesslich des letzten Messwerts für:
* Die Temperatur in Grad Celsius (°C).
* Die relative Luftfeuchtigkeit in Prozent (%).

**URL:** https://smart-urban-heat-map.ch/api/1.0/stations
**Rückgabeformate:** "GeoJSON" (Default), "CSV".

### timeseries <!-- omit in toc -->
Ruft Zeitreihen basierend auf der Stations-ID ab für:
* Die Temperatur in Grad Celsius (°C).
* Die relative Luftfeuchtigkeit in Prozent (%).

**URL:** https://smart-urban-heat-map.ch/api/1.0/timeseries
**Rückgabeformate:** "JSON" (Default), "CSV".
**URL-Parameter:**
   * **stationId** (erforderlich): Gibt an, von welcher Station die Zeitreihe zurückgegeben werden soll
   * **timeFrom** (optional, Default: "-24hours"): Gibt den Beginn der Zeitreihe an (Beispiele: "-3days", "-24hours", "-30minutes", "2023-10-01T00:00:00Z")
   * **timeTo** (optional, Default: "now"): gibt das Ende der Zeitreihe an (Beispiele: "-3days", "-24hours", "-30minutes", "now", "2023-10-01T00:00:00Z")

## Codebuch

### stations <!-- omit in toc -->

- **coordinates**: Array, das die geografischen Koordinaten (in WGS84) der Station darstellt (Latitude, Longitude)
- **stationId**: Eindeutige Kennung für die Station (Beispiel: "0F40CBFEFFE70FFE")
- **name**: Name der Station (Beispiel: "Sandrain-Bern")
- **dateObserved**: Datum und Uhrzeit der letzten Messung (Beispiel: "2023-08-01T12:00:00Z")
- **temperature**: Zuletzt an der Station gemessene Temperatur in °C (Beispiel: 18.925001)
- **relativeHumidity**: Zuletzt an der Station gemessene relative Luftfeuchtigkeit in % (Beispiel: 60,971848)

### timeseries <!-- omit in toc -->

- **stationId**: Eindeutige Kennung für die Station (Beispiel: "0F40CBFEFFE70FFE")
- **dateObserved**: Datum und Uhrzeit der Messung (Beispiel: "2023-08-01T12:00:00Z")
- **temperature**: An der Station gemessene Temperatur in °C (Beispiel: 18.925001)
- **relativeHumidity**: An der Station gemessene relative Luftfeuchtigkeit in % (Beispiel: 60,971848)

## Beispielabfragen

### Liste der Stationen einschliesslich der neuesten Messungen abfragen <!-- omit in toc -->
`GET https://smart-urban-heat-map.ch/api/1.0/stations`

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

### Zeitreihe für eine Station abfragen  <!-- omit in toc -->

`GET https://smart-urban-heat-map.ch/api/1.0/timeseries?stationId=D33FCBFEFFE70FFE&timeFrom=2023-10-01T00:00:00Z&timeTo=2023-10-31T23:00:00Z`

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