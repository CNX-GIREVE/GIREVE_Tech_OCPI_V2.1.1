### [<- Back to module selection](cpo_edits.md)


# Contents 

* [Use Cases Covered By IOP](#use-cases-covered-by-iop)
  - EVCI data
* [Use Cases Required By Gireve](#use-cases-required-by-gireve)
  - Always required
* [Locations module specifications](#locations-module-specifications)
  - Static and dynamic attributes
  - EVSE object
  - Tariff_id value
  - PUSH Locations ToIOP
  - Store and Forward – PUT and PATCH Locations
  - PULL Locations ToIOP
  - PULL Locations FromIOP 
* [Examples](#examples)
  - ToIOP_PUT_emsp_locations_2.1.1, FromIOP_ PUT_emsp_locations_2.1.1 (on Locations)
  - ToIOP_PATCH_emsp_locations_2.1.1, FromIOP_ PATCH_emsp_locations_2.1.1 (on EVSE)

***



## `Use cases covered by IOP`

OCPI features are composed by several use cases that a CPO can choose to implement or not when connecting to an operator.

In case of connection to Gireve, here is the list of use cases that a CPO can implement :

## `EVCI data`

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| Push Locations Static data | ToIOP | A CPO updates its Locations stored in RPC. *Only the property “connector.tariff_Id” can be updated through this flow.* |
| Push Locations Dynamic data (Only EVSE status) | ToIOP | A CPO updates status of its EVSEs stored in RPC. |
| Pull Locations (Static data) | FromIOP | IOP requests the CPO backend to retrieve Locations data. |
| Check Locations | ToIOP | A CPO requests IOP to get its Locations stored in RPC. |

## `Use cases required by Gireve`

Some of these use cases are required when connecting to Gireve :

### Always required

| Use case |  Why ? | 
| ----------- | ----------- |
| Push EVSE status/ToIOP | A CPO connected to Gireve must transfer “in realtime” EVSE status change of its EVSEs. | 
| Pull Locations/FromIOP | Gireve wants to be able to refresh EVCI data when needed. |

## `Locations module specifications`

IOP follows the OCPI standard for Locations upload by a CPO. [See OCPI specifications](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/mod_locations.md).

**<ins>Nonetheless, the following attributes are mandatory for Gireve although they are optional in the OCPI standard :**</ins>

| Word | Meaning |
| ----------- | ----------- |
| Owner.name | Mandatory : this information MUST be registered in the ocpi flow under the object « owner » with the attribute « name ». | 
| Operator.name | Mandatory for Gireve - we use it to give information about the brand name. |
| Evses.connectos.tariff_id | Mandatory for Gireve. |
| Evses.capabilities | Mandatory for Gireve. |

### Static and dynamic attibutes

The attributes of the Location object (with EVSE and Connector) are of 2 types :

- **<ins>Static attributes</ins>** are data attributes that do not change frequently (address, localisation …)
- **<ins>Dynamic attributes</ins>** are data attributes that may change frequently (availability, occupied/free …)

**For the moment, all properties of a Location are Static data except EVSE.status, the only Dynamic data.**

### EVSE object

OCPI defines an attribute called « evse_id » which contains the eMI3 id of the EVSE.

**The “evse_id” is optional for OCPI but required by Gireve to ensure seamless compatibility with operators connected via other protocols (eMIP, …).
In addition, the eMI3 standard requires that the eMI3 Id of an EVSE begins by the eMI3 Id of the CPO.**

[See “Part 2 v1.0” of eMI3 standard here :](http://emi3group.com/documents-links/)
All “evse_id” of the CPO “FR*CPO” should start with “FR*CPO*E”.

### Tarrif_id value

Gireve uses the “tariff_id” information provided by CPOs in Locations to dispatch CPO’s EVSEs into separated EVSE tariff groups. Also, CPOs can refer to these tariff groups when they describe their roaming offer including tariffs via the [Gireve connect place](https://connect-place.gireve.com).

**Considering that, we strongly suggest to fill the “tariff_id” information when the CPO uploads its Locations to Gireve IOP platform.**

### PUSH Locations ToIOP

The eMSP Interface is not fully implemented by IOP for the PUSH of static attributes. The web service is present and responds but information is not stored in RPC (Charge Point Repository).

**<ins>The only data directly updated in RPC after a PUT/PATCH Locations by a CPO are :**</ins>
- The dynamic attribute EVSE.status.
- The “Connector.tariff_id” information.  

### Store and Forward – PUT and PATCH Locations

**<ins>A [Store and Forward mechanism](q&a.md/#store-and-forward-mechanism) must be implemented</ins>** to ensure that no data upload may be lost, in case of a connection loss. Any data upload that didn’t get a correct response (HTTP code : 2XX) from the GIREVE platform IOP must be stored on CPO side and a retry process must be active.
After the connection recovery, the Data Upload messages must be resent in a FIFO manner.

### PULL Locations ToIOP

If the CPO wants to check the status of a Location, EVSE or Connector object in the IOP system, it can call these URLs : 

`/ocpi/emsp/2.2.1/locations/{country_code}/{party_id}/{location_id}`
`/ocpi/emsp/2.2.1/locations/{country_code}/{party_id}/{location_id}/{evse_uid}`
`/ocpi/emsp/2.2.1/locations/{country_code}/{party_id}/{location_id}/{evse_uid}/{connector_id}`

with :

`location_id: unique id of the Location object provided by the CPO.`
`evse_uid: unique id of the EVSE object provided by the CPO.`
`connector_id: unique id of the Connector object provided by the CPO.`

### PULL Locations FromIOP

IOP is able to PULL Location requesting CPO backend. In this case, IOP uses the pagination and CPO must respond with a paginated response. If the response is not paginated, it will be ignored by IOP.

The default periodicity is every day.

## `Examples`

### ToIOP_PUT_emsp_locations_2.1.1, FromIOP_ PUT_emsp_locations_2.1.1 (on Locations)

*URL* :

`/ocpi/emsp/2.1.1/locations/FR/XXX/1111`
*(FR and XXX refer to the country_code and party_id of the CPO which owns the Location cf. 2.1.3 Client owned object push)*

*Request* :

- **VERB : PUT**
- **HEADERS** : `{content-type:application/json; charset=UTF-8}{connection:close}{accept:application/json, application/*+json}{authorization:Token xxx-xxx-xxx}`
- **BODY** :
```json
{
    "id": "eeee",
    "type": "UNKNOWN",
    "name": "name ",
    "address": "address",
    "city": "city",
    "postal_code": "pc",
    "country": "FRA",
    "coordinates": {
        "latitude": "11.111111",
        "longitude": "11.111111"
    },
    "related_locations": null,
    "evses": [
        {
            "uid": "aaaaaa",
            "evse_id": "FR*CPO*E111",
            "status": "AVAILABLE",
            "status_schedule": null,
            "capabilities": [],
            "connectors": [
                {
                    "id": "QSDF",
                    "standard": "IEC_62196_T2",
                    "format": "SOCKET",
                    "power_type": "AC_3_PHASE",
                    "voltage": 230,
                    "amperage": 16,
                    "tariff_id": "A3",
                    "terms_and_conditions": null,
                    "last_updated": "2020-01-09T09:45:59Z"
                }
            ],
            "floor_level": "",
            "coordinates": null,
            "physical_reference": null,
            "directions": [],
            "parking_restrictions": null,
            "images": null,
            "last_updated": "2020-01-16T13:25:20Z"
        }
    ],
    "directions": [],
    "operator": {
        "name": "FR*CPO",
        "website": "https://ccc.ddd.com",
        "logo": {
            "url": "https://logo.com/logo.png",
            "thumbnail": "https:// logo.com/img/logo_thumb.png",
            "category": "OPERATOR",
            "type": "png",
            "width": 0,
            "height": 0
        }
    },
    "suboperator": null,
    "owner": null,
    "facilities": [],
    "time_zone": null,
    "opening_times": null,
    "charging_when_closed": false,
    "images": null,
    "energy_mix": null,
    "last_updated": "2020-01-16T13:59:37Z"
}

``` 

*Response* :

- **CODE** : 200
- **HEADERS** : `{Date:Thu, 16 Jan 2020 07:30:16 GMT}{Connection:close}{Content-Type:application/json}`
- **BODY** :  
```json

  {
    "data": {},
    "status_code": 1000,
    "status_message": "Success",
    "timestamp": "2020-01-16T14:26:27Z"
}

```

### ToIOP_PATCH_emsp_locations_2.1.1, FromIOP_ PATCH_emsp_locations_2.1.1 (on EVSE)

*URL* :

`/ocpi/emsp/2.1.1/locations/FR/XXX/1111/22222`
*(FR and XXX refer to the country_code and party_id of the CPO which owns the Location cf. 2.1.3 Client owned object push)*

*Request* :

- **VERB : PATCH**
- **HEADERS** : `{content-type:application/json; charset=UTF-8}{connection:close}{accept:application/json, application/*+json}{authorization:Token xxx-xxx-xxx}`
- **BODY** :
```json

{
    "status": "AVAILABLE",
    "last_updated": "2020-01-16T15:18:52Z"
}

```
*Response* :

- **CODE** : 200
- **HEADERS** : `{Date:Thu, 16 Jan 2020 07:30:16 GMT}{Connection:close}{Content-Type:application/json}`
- **BODY** :  
```json

{
    "data": {},
    "status_code": 1000,
    "status_message": "Success",
    "timestamp": "2020-01-16T14:26:27Z"
}

```




