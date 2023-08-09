# Analysis API (ladeplan)

[![Version](https://img.shields.io/badge/Version-0.0.1-yellow.svg)](https://shields.io/)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://GitHub.com/Naereen/StrapDown.js/graphs/commit-activity)

___

 Endpoints

- [startAnalysis](#startAnalysis)
- [getAnalysis](#getAnalysis)
- [getallAnalysisIDs](#getallAnalysisIDs)
- [getQuota](#getQuota)

___
### Authentication

For Authentication you can use an API-Key which you will recieve from customer support. 

Either in the querystring:

    key=YOUR_API_KEY

or as a Authorization Header: e.g. using curl:

    -H "Authorization: Bearer YOUR_API_KEY"

___

<a name="startAnalysis"></a>
### startAnalysis [GET, POST]

##### Required Parameters:

Either you supply both latitude and longitude either in the querystring as "lat=LAT&lon=LON" or similar in the POST Methods as params:

| Parameter | Description |
| ------ | ------ |
| lat | latitude (Example value: lat=50.00234 (querystring or POST Body))|
| lon | longitude (Example value: lon=9.02334 (querystring or POST Body))|

or

| Parameter | Description |
| ------ | ------ |
| number | buidling number (e.g. 15) |
| street | street name (e.g. "Bahnhofstrasse")|
| city | city name (e.g. "Berlin")|
| country_code | ISO-A3 country code (for Germany e.g. "DEU") |
| zip_code | regional zip code (e.g. 01234) 


##### Additional Parameters:

Additionally you can add Parameters to customize your analysis. The default values look like this:
     
    'install_costs': '100000',
    'thg': '0.15',
    'yearly_expenses': '5000',
    'avg_electricity_flow': '70',
    'buy_price': '0.3',
    'sell_price': '0.6',
    'toilet_radius': '500',
    'trafo_radius': '500',
    'restaurant_radius': '500',
    'leisure_radius': '500',
    'shop_radius': '500',
    'ac_radius' : '2000',
    'dc_radius' : '2000',

And the follwoing table explains the meaning of the parameters:

| Parameter | Description |
| ------ | ------ |
| install_costs | The costs which you calculate for when installing the charging station. This is later neccesary for the export of the ROI calculation. At this point in time the ROI calculation is only accesable for the HPC scenario. 
| thg | the percentage on which you belive that you will get the THG bonus per sold kwh.
| yearly_expenes | This costs represent the maintanace costs for operating the charging station on a yearly basis.
| avg_electricty_flow | the average number of KW you belive the charging station will supply to the customer.
| buy_price | the price on which you will buy the power per kWh.|
|sell_price | the price on which you will sell the power per kWh.|
|toilet_radius | the radius where ladeplan counts the number of toilets in. (OSM)|
|trafo_radius | the radius where ladeplan counts the number of power substations in. (OSM)|
|restaurant_radius | the radius where ladeplan counts the number of restaurantes in. (OSM)|
|leisure_radius | the radius where ladeplan counts the number of leisure amenties in. (OSM)|
|shop_radius | the radius where ladeplan counts the number of leisure amenties in. (OSM)|
|ac_radius | the radius where ladeplan counts the number of AC charging stations in. (BnetzA)|
|dc_radius | the radius where ladeplan counts the number of DC charging stations in. (BnetzA)|

You can supply the parameters via query string or POST Body. 

##### Example Request:

    https://api.ladeplan.com/test/api?startAnalysis?key=YOUR_API_KEY&street=Thisaut&number=4&city=paderborn&country_code=DEU&zip_code=33098&install_costs=50000

#### startAnalysis Response

The response for a succesful Analysis start looks like this. 

    {
        "status":"started"
        "analysis_id":"pred-api-1392276384",
    }

___

<a name="getAnalysis"></a>
### getAnalysis [GET]


The get analysis function return the requested Data with a status. The endpoint needs authorization and the analysis_id parameter. 
The analysis_id parameter you will either get from the startAnalysis- or the getAllAnalysisIDs request. 
    

##### Required Parameters:

Either you supply both latitude and longitude either in the querystring as "lat=LAT&lon=LON" or similar in the POST Methods as params:

| Parameter | Description |
| ------ | ------ |
| analysis_id | Analysis ID parameter (e.g. pred-api-12345)|

##### Example Request:
    https://api.ladeplan.com/test/api?getAnalysis?key=YOUR_API_KEY&analysis_id=pred-api-1234567890

#### getAnalysis Response

The response supplies the requested Data. An example response looks like this:

    {
        "data":{
            "Netzentgelte":{
                "Hochspannung":{
                    "GP":-1,
                    "ab_2500":{
                        "AP/HT":0.86,
                        "AP/NT":0.86,
                        "LP":98.5
                    },
                    "bis_2499":{
                        "AP/HT":4.43,
                        "AP/NT":4.43,
                        "LP":9.22
                    }
                },
                "Mittelspannung":{
                    "GP":-1,
                    "ab_2500":{
                        "AP/HT":2.25,
                        "AP/NT":2.25,
                        "LP":90.45
                    },
                    "bis_2499":{
                        "AP/HT":5.46,
                        "AP/NT":5.46,
                        "LP":10.36
                    }
                },
                "Netzbetreiber_Name":"Westfalen Weser Netz AG",
                "Niederspannung":{
                    "GP":-1,
                    "ab_2500":{
                        "AP/HT":3.91,
                        "AP/NT":3.91,
                        "LP":72.33
                    },
                    "bis_2499":{
                        "AP/HT":6.39,
                        "AP/NT":6.39,
                        "LP":10.37
                    }
                },
                "Umlagen":{
                    "konz_abgabe_ht":1.99,
                    "konz_abgabe_nt":0.61,
                    "kwkg_umlage":0.357,
                    "nev_umlage":0.417,
                    "offshore_umlage":0.591,
                    "stromsteuer":2.05
                },
                "Versorger_Email":"info@ww-energie.com",
                "Versorger_Homepage":"https://www.ww-netz.com",
                "Versorger_Name":"Westfalen Weser Netz GmbH",
                "Versorger_Telefon":"05251 2020303"
            },
            "POI_Data":{
                "AC_count":28,
                "DC_count":4,
                "dstncp":116.54577105366145,
                "ev_registrations":5575,
                "leisure":53,
                "pop_density":306890,
                "restaurant":48,
                "shop":154,
                "toilet":6,
                "trafo":25
            },
            "address_data":{
                "city":"paderborn",
                "country_code":"DEU",
                "lat":51.72078,
                "lon":8.75668,
                "street":"Thisaut",
                "zip_code":"33098"
            },
            "business_calculation":{
                "break_even_point":1.4016915655893833,
                "costs_per_year":55894.914142756745,
                "profit_per_year":71342.37121413511,
                "revenue_per_year":127237.28535689187
            },
            "predictions":{
                "ac_prediction":0.2203983314589887,
                "dc_prediction":0.22311402458492974,
                "hpc_prediction":0.1402527395909302
            }
        },
        "status":"finished"
    }

The data fields are descriped as followed: 


##### Prediction Data:
field: "predicitions"

| Parameter | Description |
| ------ | ------ |
| ac_prediction | the daily occupation forecast, when installing an AC charging station at the specific location calculated by ladeplan. (e.g. 0.13 in -> 13% daily occpation rate)|
| dc_prediction | the daily occupation forecast, when installing an DC charging station at the specific location calculated by ladeplan. (e.g. 0.13 in -> 13% daily occpation rate)|
| hpc_prediction | the daily occupation forecast, when installing an HPC charging station at the specific location calculated by ladeplan. (e.g. 0.13 in -> 13% daily occpation rate)|

##### POI Data:
field: "POI_Data"

| Parameter | Description |
| ------ | ------ |
| AC_count | Number of AC charging stations in circle with predefined radius (default:5000) (BnetzA)|
| DC_count | Number of DC charging stations in circle with predefined radius (default:5000) (BnetzA)|
| dstncp | Distance (Air) towards the next charging station (BnetzA)|
| ev_registrations | Number of electric vehicles registered in the registrational area of the location (e.g. 5000) (KBA)|
| pop_density | total number of citizens of the location's "Landkreis" (BKG) | 
| leisure | Number of leisure amenities in circle with predefined radius (default:500) (OSM)
| restaurant | Number of restaurant amenities in circle with predefined radius (default:500) (OSM)
| shop | Number of shopping amenities in circle with predefined radius (default:500) (OSM)
| trafo | Number of power substations amenities in circle with predefined radius (default:200) (OSM)


##### Calculation Data:
field: "business_calculation"
| Parameter | Description |
| ------ | ------ |
| break_even_point | The Number of Years it will take after the charging station will make its first profit. (No Taxes included) (ladeplan calculation)|
| costs_per_year | The yearly costs of the maintanence of the charging station with the expenses from buying power. (No Taxes included) (ladeplan calculation)|
| revenue_per_year | The revenue generated by selling power throught the year with that specific charging station. (No Taxes included) (ladeplan calculation)|
| profit_per_year | The yearly revenue minus the yearly costs. (No Taxes included) (ladeplan calculation)|

##### Address Data:
field: "address_data"
| Parameter | Description |
| ------ | ------ |
| lat | Analysis ID parameter (e.g. pred-api-12345)|
| lon | Analysis ID parameter (e.g. pred-api-12345)|
| number | buidling number (e.g. 15) |
| street | street name (e.g. "Bahnhofstrasse")|
| city | city name (e.g. "Berlin")|
| country_code | ISO-A3 country code (for Germany e.g. "DEU") |
| zip_code | regional zip code (e.g. 01234)|


##### Grid Data:
field: "Netzentgelte"
| Parameter | Description |
| ------ | ------ |
| AP | Arbeitspreis ct/kWh|
| LP | Leistungspreis €/kWa|
| GP| Grundpreis|
| HT | Hochtarif |
| NT |Normaltarif|
|bis_2499| Benutzungsdauer <2500 Stunden/a|
|ab_2500| Benutzungsdauer >=2500 Stunden/a|
| offshore_umlage| Offshore Umlage ct/kwH|
|konz_abgabe_nt|Konzessionsabgabe Normaltarif ct/kWh|
|konz_abgabe_ht|Konzessionsabgabe Hochtarif ct/kWh|
|kwkg_umlage|KWKG Umlage ct/kWh|
|stromsteuer|Stromsteuer ct/kWh| 
|nev_umlage|NEV Umlage ct/kWh|


When the analysis is still running the response looks like this:

    {
        "status":"running"
    }

<a name="getallAnalysisIDs"></a>
### getallAnalysisIDs [GET]

The getallAnalysisIDs Request returns all your started analysises. It requires only Authentication. 

##### Example Request: 
    https://api.ladeplan.com/test/api?getallAnalysisIDs?key=YOUR_API_KEY

##### Example Response:

    {
        "analysis_ids":[
            "pred-api-1392276384",
            "pred-api-0715194277",
            "pred-api-6918884685"
        ]
    }




<a name="getQuota"></a>
### getQuota [GET]

The getQuota Request returns your Quota (Meaning how many more analyses you can request). It requires only Authentication. 

##### Example Request: 
    https://api.ladeplan.com/test/api?getQuota?key=YOUR_API_KEY

##### Example Response:

    {
        "quota": 456
    }




The API is new and will be tested in the following weeks. 
All rights reserved.

___

ladeplan UG (haftungsbeschränkt) <br> 
Thisaut 4 <br>
33098 Paderborn <br>
Handelsregister: 15657 <br>
Registergericht: Paderborn


