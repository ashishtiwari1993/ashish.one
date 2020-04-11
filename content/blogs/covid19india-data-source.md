---
title: "Covid19 Data Source for India & Global"
date: 2020-03-27T20:04:13+05:30
draft: false
type: "post"
ogtype: "article"
tags: ["covid19","covid19India"]
slug: "Covid19-data-source-and-endpoints-india-global"
---

Hi Guys, I am trying to listing all data source & endpoints for COVID19 - India as well as Global. It can contains offical or unofficial APIs. Anyone is working on any COVID19 project for India, Can use these sources. 

## APIs

### 1. Github: amodm/api-covid19-in (India)


#### Repo:
```sh
https://github.com/amodm/api-covid19-in
```

#### APIs available:

##### Statewise Data

* StateWise
* StateWise History

##### Medica

* Hospital Stats
* Bed Stats

##### Contacts

* HelpLines
* Contacts

##### Patient

* Tracing
* History

#### Sources

The source is both types of official & Unofficial.

##### Official

* Post Mar 15, data is from [The Ministry of Health & Family Welfare](https://www.mohfw.gov.in/)
* Pre  Mar 15, data is sourced from [datameet/covid19](https://github.com/datameet/covid19/tree/eb1cc65657929abe12ca59f0e754bef4bc562d7a/mohfw-backup)
* Hospital & bed data: https://api.steinhq.com/v1/storages/5e732accb88d3d04ae0815ae/StateWiseHealthCapacity
* ICMR testing stats API: https://api.steinhq.com/v1/storages/5e6e3e9fb88d3d04ae08158c/ICMRTestData

##### Unofficial

* The awesome volunteer driven patient tracing data [covid19india.org](https://www.covid19india.org/)
* API (NLP): http://coronatravelhistory.pythonanywhere.com/
* API (Travel history): https://api.covid19india.org/travel_history.json

### 2. Github: ashishtiwari1993/india_mohfw.gov.in_scrape_covid19_statewise_status (India)

Scraping [https://mohfw.gov.in](https://mohfw.gov.in) to extract the statewise data.

#### Repo:

```sh
https://github.com/ashishtiwari1993/india_mohfw.gov.in_scrape_covid19_statewise_status
```

#### APIs available:

##### Statewise status

* Total confirmed case
* Counts 
* Deaths

#### Sources

Scraping from offical site of The Ministry of Health and Family Welfare.

### 3. Github: NovelCOVID/API (Global)

#### Repo:
```sh
https://github.com/NovelCOVID/API
```

#### APIs available:

It is global data source. You will get all information for any country.

#### Sources

* [Worldometers](https://www.worldometers.info/coronavirus/)


### 4. Github: CSSEGISandData/COVID-19 (Global)

This is the data repository for the 2019 Novel Coronavirus Visual Dashboard operated by the Johns Hopkins University Center for Systems Science and Engineering (JHU CSSE). Also, Supported by ESRI Living Atlas Team and the Johns Hopkins University Applied Physics Lab (JHU APL).

#### Repo:

```sh
https://github.com/CSSEGISandData/COVID-19
``` 

#### Data Available

CSVs are available in time series manner. Information is available about all country.

## Sources

* World Health Organization (WHO): https://www.who.int/
* DXY.cn. Pneumonia. 2020. http://3g.dxy.cn/newh5/view/pneumonia.
* BNO News: https://bnonews.com/index.php/2020/02/the-latest-coronavirus-cases/
* National Health Commission of the People’s Republic of China (NHC):
 http://www.nhc.gov.cn/xcs/yqtb/list_gzbd.shtml
* China CDC (CCDC): http://weekly.chinacdc.cn/news/TrackingtheEpidemic.htm
* Hong Kong Department of Health: https://www.chp.gov.hk/en/features/102465.html
* Macau Government: https://www.ssm.gov.mo/portal/
* Taiwan CDC: https://sites.google.com/cdc.gov.tw/2019ncov/taiwan?authuser=0
* US CDC: https://www.cdc.gov/coronavirus/2019-ncov/index.html
* Government of Canada: https://www.canada.ca/en/public-health/services/diseases/coronavirus.html
* Australia Government Department of Health: https://www.health.gov.au/news/coronavirus-update-at-a-glance
* European Centre for Disease Prevention and Control (ECDC): https://www.ecdc.europa.eu/en/geographical-distribution-2019-ncov-cases 
* Ministry of Health Singapore (MOH): https://www.moh.gov.sg/covid-19
* Italy Ministry of Health: http://www.salute.gov.it/nuovocoronavirus
* 1Point3Arces: https://coronavirus.1point3acres.com/en
* WorldoMeters: https://www.worldometers.info/coronavirus/


