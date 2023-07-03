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

### 3.2 EVSE object

### 3.3 Tarrif_id value

### 3.4 PUSH Locations ToIOP

### 3.5 Store and Forward – PUT and PATCH Locations

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

```json
{
  "firstName": "John",
  "lastName": "Smith",
  "age": 25
}
```











