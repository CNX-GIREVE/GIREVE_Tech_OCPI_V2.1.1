### [<- Back to module selection](emsp_edits.md)

# Contents 

* [Use Cases Covered By IOP](#use-cases-covered-by-iop)
  - EVCI data
* [Use Cases Required By Gireve](#use-cases-required-by-gireve)
  - If the eMSP implements the “Locations static data download” feature
  - If the eMSP implements the “Locations dynamic data download” feature
* [Locations module specifications](#locations-module-specifications)
  - Static and dynamic attributes
  - PULL Locations ToIOP: Who is the CPO?
  - PULL Tokens: Retrieve Locations of a single given CPO
  - PULL Locations ToIOP: Get Object
  - PULL Locations ToIOP: Get List Pagination
  - PULL Locations ToIOP: Get List, Full and Delta modes
  - PULL Locations ToIOP: evse_id
  - PUSH Locations FromIOP
  - PULL Locations FromIOP
* [Examples](#examples)
  - ToIOP_GET_cpo_locations_2.1.1, FromIOP_GET_cpo_locations_2.1.1

***

## `Use cases covered by IOP` 

OCPI features are composed by several use cases that an eMSP can choose to implement or not when connecting to an operator. In case of connection to Gireve, here is the list of use cases that a CPO can implement :

### EVCI data 

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| **Push Locations Static data** | FromIOP | IOP sends update of Locations to an eMSP. |
| **Push Locations Dynamic data (Only EVSE status)** | FromIOP | IOP sends EVSE status updates to an eMSP. |
| **Pull Locations (Static data)** | ToIOP | An eMSP requests IOP to get Locations of CPOs, it is allowed to get (ie depending on its roaming agreements). |
| **Check Locations** | ToIOP | An eMSP requests IOP to get Locations of a single given CPO which is identified by “OCPI-TO” headers. |

## `Use cases required by Gireve`

Some of these use cases are required when connecting to Gireve :

### If the eMSP implements the “Locations static data download” feature

| Use case |  Why ? | 
| ----------- | ----------- |
| **Pull Locations/ToIOP** | Only way for an operator to retrieve all Locations. |

### If the eMSP implements the “Locations dynamic data download” feature

| Use case |  Why ? | 
| ----------- | ----------- |
| **Push EVSE status/FromIOP** | Only way for an operator to be alerted about EVSE status change in realtime. |

## `Locations module specifications`

IOP follows the OCPI standard for Locations downloaded by an eMSP. [*See OCPI specifications*](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/mod_locations.md)*.*

### Static and dynamic attributes

The attributes of the Location object (with EVSE and Connector) are of 2 types :

- **<ins>Static attributes</ins>** are data attributes that do not change frequently (address, localisation …).

- **<ins>Dynamic attributes</ins>** are data attributes that may change frequently (availability, occupied/free …).

For the moment, all properties of a Location are Static data except EVSE.status, the only Dynamic data.

### PULL Locations ToIOP: Who is the CPO?

The CPO of a Location is not a direct property of the Location object (String “name” is not strongly consistent). So, an eMSP pulling IOP to get Locations does not know to which CPO the Location refers to. **<ins>That is why IOP replaces the property “Location.operator.name” by the eMI3 Id of the CPO when an eMSP get a Location</ins>**. Using this property, the eMSP is able to know who the CPO of the Location is.

### PULL Tokens: Retrieve Locations of a single given CPO

The standard OCPI 2.1.1 Locations pulling allows eMSPs to get Locations of all CPOs in contract with them.

In some cases, eMSPs need only Locations of a specific CPO. For example, when the eMSP initializes data of a CPO after signature of a new roaming agreement.

> :bulb: **Gireve provides a **new OCPI 2.1.1 feature** by allowing the eMSP to get Locations of a unique CPO by filling two dedicated OCPI headers in their “GET Locations” request to Gireve :**

-   ocpi-to-country-code: The country code of the CPO targeted.
-   ocpi-to-party-id: The party id of the CPO targeted.

Therefore, eMSPs can request Gireve without these headers to get Locations of all CPOs or including these headers to get Locations of a unique CPO.

For information, these headers have been included in the version 2.2 of the OCPI standard.

### PULL Locations ToIOP: Get Object

If the eMSP wants to retrieve a specific given Location, EVSE or Connector, it can call these URLs :

-   /ocpi/cpo/2.2.1/locations/{location_id}
-   /ocpi/cpo/2.2.1/locations/{location_id}/{evse_uid}
-   /ocpi/cpo/2.2.1/locations/{location_id}/{evse_uid}/{connector_id}

With :

-   location_id: unique id of the Location object provided by IOP.
-   evse_uid: unique id of the EVSE object provided by IOP.
-   connector_id: unique id of the Connector object provided by IOP.

### PULL Locations ToIOP: Get List Pagination

If the eMSP wants to retrieve a list of Locations, it can call the URL: /ocpi/cpo/2.2.1/locations using the paginated properties date_from, date_to, offset and limit.

Parameters « offset » and « limit » are optional but IOP always returns a paginated response (subset of objects list and link, X-Total-Count and X-limit headers).

IOP has its own max limit and answers with its if the client limit is upper than IOP one.

### PULL Locations ToIOP: Get List, Full and Delta modes

If the eMSP wants to retrieve all the Locations, it should not include « date_from » parameter in its request. But if it wants to get only changes (optimised), it should use it.

> :warning: **The Location and EVSE deletion logic is different if using « date_from » parameter or not. When « date_from » is present, the eMSP get EVSEs of the Location in status « REMOVED » whereas without « date_from » the deleted items are not included in the response (see table below).**

For information, when eMSP PULL Locations list from IOP, IOP follows the below logic in responses provided :

| VERB | LOCATION | EVSE | CONNECTOR |
|--------|--------|--------|--------|
| **CREATE** | New Location is part of the response. | New EVSE is part of the response. | New Connector is part of the response. |
| **UPDATE** | Updates are parts of the response. | Updated EVSE is part of the response. **NB: EVSE.status updates are not considered. eMSP should implement PATCH Locations FromIOP to retrieve EVSE.status.** | Updated Connector is part of the response. |
| **DELETE** | - Using « date_from » option, response contains all EVSEs of the Location with « REMOVED » value for « status » field. - Without « date_from » option, the response does not contain this Location. | - Using « date_from » option, response contains the EVSE with « REMOVED » value for « status » field. - Without « date_from » option, the response does not contain this EVSE. | Deleted Connectors are not part of the response. |

### PULL Locations ToIOP: evse_id

It may happen that evse_id may not be compliant with the eMI3 standard. (See “Part 2 v1.0” of eMI3 standard here: http://emi3group.com/documents-links/).

You should not reject them because of the non-compliance with the standard.

### PUSH Locations FromIOP

| VERB | LOCATION | EVSE | CONNECTOR |
|--------|--------|--------|--------|
| **CREATE** | Location is pushed by IOP. |EVSE is pushed by IOP. | Connector appears in EVSE description pushed by IOP. |
| **UPDATE** | Each EVSE of the updated Location are pushed by IOP. | UEVSE is pushed by IOP. | EVSE containing Connector updates is pushed by IOP. |
| **DELETE** | Each EVSE of the deleted Location are pushed by IOP with « REMOVED » value for « status » field. | EVSE containing « REMOVED » value for « status » field is pushed by IOP. |EVSE description is pushed by IOP, it doesn’t contain the Connector anymore. |

### PULL Locations FromIOP

**IOP will never PULL eMSP to check the status of a Location, EVSE or Connector object in the eMSP system.**

## `Examples`

### ToIOP_GET_cpo_locations_2.1.1, FromIOP_GET_cpo_locations_2.1.1

*URL* :

`/ocpi/cpo/2.1.1/locations?date_from=2020-01-20T14:51:43Z&limit=20&offset=10`
(Date_from parameter shall be the date of the last successful request)

*Request* :

- **VERB: GET**
- **HEADERS**: `{connection:close}{accept:application/json, application/*+json}{authorization:Token xxx-xxx-xxx}`
+ `{ocpi-to-party-id:CPO}{ocpi-to-country-code:FR}` (If you wish to get the locations of a specific CPO)
- **BODY**: (No body)

*Response* :

- **CODE**: 200
- **HEADERS**: `{Date:Thu, 16 Jan 2020 07:30:16 GMT}{Connection:close}{Content-Type:application/json} {Link:<https://xxx.ddd.com/ocpi/cpo/2.1.1/locations?date_from=2020-01-20T14:51:43Z&limit=20&offset=30>;rel="next"} {X-Total-Count:11682} {X-Limit:20}`
- **BODY**: 
```json
{
  "data": [{
    "id": "azerty",
    "type": "UNKNOWN",
    "name": "Name of the Location",
    "address": "Adress of the Location ",
    "city": "City of the Location ",
    "country": "ESP",
    "coordinates": {
      "latitude": "11.222222",
      "longitude": "11.222222"
    },
    "evses": [
      {
        "uid": "33656",
        "status": "AVAILABLE",
        "capabilities": [
          "REMOTE_START_STOP_CAPABLE",
          "RFID_READER"
        ],
        "connectors": [
          {
            "id": "UIOP",
            "standard": "IEC_62196_T2",
            "format": "SOCKET",
            "voltage": 230,
            "amperage": 32,
            "power_type": "AC_1_PHASE",
            "last_updated": "2019-11-21T10:43:49Z"
          }
        ],
        "coordinates": {
          "latitude": "11.222222",
          "longitude": "11.222222"
        },
        "evse_id": "ES*AAA*E11111",
        "last_updated": "2019-11-21T10:43:49Z"
      }
    ],
    "operator": {
      "name": "ES*AAA"
    },
    "postal_code": "12345",
    "related_locations": [
      {
        "latitude": "11.222222",
        "longitude": "11.222222"
      }
    ],
    "opening_times": {
      "twentyfourseven": true
    },
    "last_updated": "2019-11-21T10:43:49Z"
  }],
  "status_code": 1000,
  "status_message": "Success",
  "timestamp": "2020-01-15T18:56:08Z"
}
```
