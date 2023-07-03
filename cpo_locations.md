## Use cases covered by IOP

OCPI features are composed by several use cases that a CPO can choose to implement or not when connecting to an operator.

In case of connection to GIREVE, here is the list of use cases that a CPO can implement :

## 1. EVCI data

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| Push Locations Static data | ToIOP | A CPO updates its Locations stored in RPC. *Only the property “connector.tariff_Id” can be updated through this flow.* |
| Push Locations Dynamic data (Only EVSE status) | ToIOP | A CPO updates status of its EVSEs stored in RPC. |
| Pull Locations (Static data) | FromIOP | IOP requests the CPO backend to retrieve Locations data. |
| Check Locations | ToIOP | A CPO requests IOP to get its Locations stored in RPC. |

## 2. Use cases required by GIREVE

Some of these use cases are required when connecting to GIREVE.

### 2.1 Always required

| Use case |  Why ? | 
| ----------- | ----------- |
| Push EVSE status/ToIOP | A CPO connected to GIREVE must transfer “in realtime” EVSE status change of its EVSEs. | 
| Pull Locations/FromIOP | GIREVE wants to be able to refresh EVCI data when needed. |

## 3. Locations module specifications

IOP follows the OCPI standard for Locations upload by a CPO. [See OCPI specifications](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/mod_locations.md).

Nonetheless, the following attributes are mandatory for Gireve although they are optional in the OCPI standard :

| Word | Meaning |
| ----------- | ----------- |
| Owner.name | Mandatory : this information MUST be registered in the ocpi flow under the object « owner » with the attribute « name ». | 
| Operator.name | Mandatory for Gireve - we use it to give information about the brand name. |
| Evses.connectos.tariff_id | Mandatory for GIREVE. |
| Evses.capabilities | Mandatory for GIREVE. |

### 3.1 Static and dynamic attibutes

The attributes of the Location object (with EVSE and Connector) are of 2 types :

- Static attributes are data attributes that do not change frequently (address, localisation …)
- Dynamic attributes are data attributes that may change frequently (availability, occupied/free …)

**For the moment, all properties of a Location are Static data except EVSE.status, the only Dynamic data.**

### 3.2 EVSE object

OCPI defines an attribute called « evse_id » which contains the eMI3 id of the EVSE.

**The “evse_id” is optional for OCPI but required by GIREVE to ensure seamless compatibility with operators connected via other protocols (eMIP, …).
In addition, the eMI3 standard requires that the eMI3 Id of an EVSE begins by the eMI3 Id of the CPO.**

[See “Part 2 v1.0” of eMI3 standard here :](http://emi3group.com/documents-links/)
All “evse_id” of the CPO “FR*CPO” should start with “FR*CPO*E”.

### 3.3 Tarrif_id value

GIREVE uses the “tariff_id” information provided by CPOs in Locations to dispatch CPO’s EVSEs into separated EVSE tariff groups. Also, CPOs can refer to these tariff groups when they describe their roaming offer including tariffs via the [GIREVE connect place](https://connect-place.gireve.com).

**Considering that, we strongly suggest to fill the “tariff_id” information when the CPO uploads its Locations to GIREVE IOP platform.**

### 3.4 PUSH Locations ToIOP

The eMSP Interface is not fully implemented by IOP for the PUSH of static attributes. The web service is present and responds but information is not stored in RPC (Charge Point Repository).

The only data directly updated in RPC after a PUT/PATCH Locations by a CPO are :
- The dynamic attribute EVSE.status.
- The “Connector.tariff_id” information.  

### 3.5 Store and Forward – PUT and PATCH Locations

A Store and Forward mechanism must be implemented to ensure that no data upload may be lost, in case of a connection loss. Any data upload that didn’t get a correct response from the GIREVE platform IOP (timeout, http code 500) must be stored on CPO side and a retry process must be active.
After the connection recovery, the Data Upload messages must be resent in a FIFO manner.

### 3.6 PULL Locations ToIOP

If the CPO wants to check the status of a Location, EVSE or Connector object in the IOP system, it can call these URLs : 

`/ocpi/emsp/2.2.1/locations/{country_code}/{party_id}/{location_id}`
`/ocpi/emsp/2.2.1/locations/{country_code}/{party_id}/{location_id}/{evse_uid}`
`/ocpi/emsp/2.2.1/locations/{country_code}/{party_id}/{location_id}/{evse_uid}/{connector_id}`

with:

`location_id: unique id of the Location object provided by the CPO.`
`evse_uid: unique id of the EVSE object provided by the CPO.`
`connector_id: unique id of the Connector object provided by the CPO.`

### 3.7 PULL Locations FromIOP

IOP is able to PULL Location requesting CPO backend. In this case, IOP uses the pagination and CPO must respond with a paginated response. If the response is not paginated, it will be ignored by IOP.

The default periodicity is every day.











