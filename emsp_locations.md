# [<- Back to module selection](emsp_edits.md)

# Contents 

* [4.1 Use Cases Covered By IOP](#41-use-cases-covered-by-iop)
  - 4.1.2 EVCI data
* [4.2 Use Cases Required By GIREVE](#42-use-cases-required-by-gireve)
  - 4.2.2	If the eMSP implements the “Locations static data download” feature
  - 4.2.3	If the eMSP implements the “Locations dynamic data download” feature
* [4.4 LOCATIONS MODULE SPECIFICATIONS](#44-locations-module-specifications)
  - 4.4.1 Static and dynamic attributes
  - 4.4.2 PULL Locations ToIOP: Who is the CPO?
  - 4.4.3 PULL Tokens: Retrieve Locations of a single given CPO
  - 4.4.4 PULL Locations ToIOP: Get Object
  - 4.4.5 PULL Locations ToIOP: Get List Pagination
  - 4.4.6 PULL Locations ToIOP: Get List, Full and Delta modes
  - 4.4.7 PULL Locations ToIOP: evse_id
  - 4.4.8 PUSH Locations FromIOP
  - 4.4.9 PULL Locations FromIOP
* [5.2 Exemples](#52-exemples)
  - 5.2.1	ToIOP_GET_cpo_locations_2.1.1, FromIOP_GET_cpo_locations_2.1.1

***

## 4.1 Use cases covered by IOP 

OCPI features are composed by several use cases that an eMSP can choose to implement or not when connecting to an operator. In case of connection to GIREVE, here is the list of use cases that a CPO can implement :

### 4.1.2 EVCI data 

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| Push Locations Static data | FromIOP | IOP sends update of Locations to an eMSP. |
| Push Locations Dynamic data (Only EVSE status) | FromIOP | IOP sends EVSE status updates to an eMSP. |
| Pull Locations (Static data) | ToIOP | An eMSP requests IOP to get Locations of CPOs, it is allowed to get (ie depending on its roaming agreements). |
| Check Locations | ToIOP | An eMSP requests IOP to get Locations of a single given CPO which is identified by “OCPI-TO” headers. |

## 4.2 Use cases required by GIREVE

Some of these use cases are required when connecting to GIREVE :

### 4.2.2	If the eMSP implements the “Locations static data download” feature

| Use case |  Why ? | 
| ----------- | ----------- |
| Pull Locations/ToIOP | Only way for an operator to retrieve all Locations. |

### 4.2.3	If the eMSP implements the “Locations dynamic data download” feature

| Use case |  Why ? | 
| ----------- | ----------- |
| Push EVSE status/FromIOP | Only way for an operator to be alerted about EVSE status change in realtime. |

## 4.4 Locations module specifications

IOP follows the OCPI standard for Locations downloaded by an eMSP. [*See OCPI specifications*](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/mod_locations.md)*.*

### 4.4.1 Static and dynamic attributes

The attributes of the Location object (with EVSE and Connector) are of 2 types :

Static attributes are data attributes that do not change frequently (address, localisation …).

Dynamic attributes are data attributes that may change frequently (availability, occupied/free …).

For the moment, all properties of a Location are Static data except EVSE.status, the only Dynamic data.

### 4.4.2 PULL Locations ToIOP: Who is the CPO?

The CPO of a Location is not a direct property of the Location object (String “name” is not strongly consistent). So, an eMSP pulling IOP to get Locations does not know to which CPO the Location refers to. That is why IOP replaces the property “Location.operator.name” by the eMI3 Id of the CPO when an eMSP get a Location. Using this property, the eMSP is able to know who the CPO of the Location is.

### 4.4.3 PULL Tokens: Retrieve Locations of a single given CPO

The standard OCPI 2.1.1 Locations pulling allows eMSPs to get Locations of all CPOs in contract with them.

In some cases, eMSPs need only Locations of a specific CPO. For example, when the eMSP initializes data of a CPO after signature of a new roaming agreement.

GIREVE provides a new OCPI 2.1.1 feature by allowing the eMSP to get Locations of a unique CPO by filling two dedicated OCPI headers in their “GET Locations” request to GIREVE:

-   ocpi-to-country-code: The country code of the CPO targeted.
-   ocpi-to-party-id: The party id of the CPO targeted.

Therefore, eMSPs can request GIREVE without these headers to get Locations of all CPOs or including these headers to get Locations of a unique CPO.

For information, these headers have been included in the version 2.2 of the OCPI standard.

### 4.4.4 PULL Locations ToIOP: Get Object

If the eMSP wants to retrieve a specific given Location, EVSE or Connector, it can call these URLs :

-   /ocpi/cpo/2.2.1/locations/{location_id}
-   /ocpi/cpo/2.2.1/locations/{location_id}/{evse_uid}
-   /ocpi/cpo/2.2.1/locations/{location_id}/{evse_uid}/{connector_id}

With :

-   location_id: unique id of the Location object provided by IOP.
-   evse_uid: unique id of the EVSE object provided by IOP.
-   connector_id: unique id of the Connector object provided by IOP.

### 4.4.5 PULL Locations ToIOP: Get List Pagination

If the eMSP wants to retrieve a list of Locations, it can call the URL: /ocpi/cpo/2.2.1/locations using the paginated properties date_from, date_to, offset and limit.

Parameters « offset » and « limit » are optional but IOP always returns a paginated response (subset of objects list and link, X-Total-Count and X-limit headers).

IOP has its own max limit and answers with its if the client limit is upper than IOP one.

### 4.4.6 PULL Locations ToIOP: Get List, Full and Delta modes

If the eMSP wants to retrieve all the Locations, it should not include « date_from » parameter in its request. But if it wants to get only changes (optimised), it should use it.

NB: The Location and EVSE deletion logic is different if using « date_from » parameter or not. When « date_from » is present, the eMSP get EVSEs of the Location in status « REMOVED » whereas without « date_from » the deleted items are not included in the response (see table below).

For information, when eMSP PULL Locations list from IOP, IOP follows the below logic in responses provided :

| VERB | LOCATION | EVSE | CONNECTOR |
|--------|--------|--------|--------|
| CREATE | New Location is part of the response. | New EVSE is part of the response. | New Connector is part of the response. |
| UPDATE | Updates are parts of the response. | Updated EVSE is part of the response. **NB: EVSE.status updates are not considered. eMSP should implement PATCH Locations FromIOP to retrieve EVSE.status.** | Updated Connector is part of the response. |
| DELETE | - Using « date_from » option, response contains all EVSEs of the Location with « REMOVED » value for « status » field. - Without « date_from » option, the response does not contain this Location. | - Using « date_from » option, response contains the EVSE with « REMOVED » value for « status » field. - Without « date_from » option, the response does not contain this EVSE. | Deleted Connectors are not part of the response. |

### 4.4.7 PULL Locations ToIOP: evse_id

It may happen that evse_id may not be compliant with the eMI3 standard. (See “Part 2 v1.0” of eMI3 standard here: http://emi3group.com/documents-links/).

You should not reject them because of the non-compliance with the standard.

### 4.4.8 PUSH Locations FromIOP

| VERB | LOCATION | EVSE | CONNECTOR |
|--------|--------|--------|--------|
| CREATE |Location is pushed by IOP. |EVSE is pushed by IOP. | Connector appears in EVSE description pushed by IOP. |
| UPDATE | Each EVSE of the updated Location are pushed by IOP. | UEVSE is pushed by IOP. | EVSE containing Connector updates is pushed by IOP. |
| DELETE | Each EVSE of the deleted Location are pushed by IOP with « REMOVED » value for « status » field. | EVSE containing « REMOVED » value for « status » field is pushed by IOP. |EVSE description is pushed by IOP, it doesn’t contain the Connector anymore. |

### 4.4.9 PULL Locations FromIOP

IOP will never PULL eMSP to check the status of a Location, EVSE or Connector object in the eMSP system.

## 5.2 Exemples

### 5.2.1 ToIOP_GET_cpo_locations_2.1.1, FromIOP_GET_cpo_locations_2.1.1

**URL**:

`/ocpi/cpo/2.1.1/locations?date_from=2020-01-20T14:51:43Z&limit=20&offset=10`
*(Date_from parameter shall be the date of the last successful request)*

**Request**:

- **VERB: GET**
- **HEADERS**: `{connection:close}{accept:application/json, application/*+json}{authorization:Token xxx-xxx-xxx}`
+ `{ocpi-to-party-id:CPO}{ocpi-to-country-code:FR}` (If you wish to get the locations of a specific CPO)
- **BODY**: (Empty body)

**Response**:

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
          "REMOTE_START_STOP_CAP
