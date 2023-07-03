## Use cases covered by IOP

OCPI features are composed by several use cases that a CPO can choose to implement or not when connecting to an operator.

In case of connection to GIREVE, here is the list of use cases that a CPO can implement :

## 1. Technical Use cases

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| Push Tariffs | ToIOP | A CPO sends to IOP, a Tariffs object. |
| Pull Tariffs | FromIOP | IOP requests the CPO backend to retrieve Tariffs data. |

## 2. Use cases required by GIREVE

Some of these use cases are required when connecting to GIREVE :

### 2.1 If the CPO doesn’t commit and describe its tariffs in a roaming agreement

| Use case | Why ? |
| ----------- | ----------- |
| Push Tariffs ToIOP | CPOs must inform in realtime, through IOP, eMSPs about tariff changes. |
| Pull Tariffs FromIOP | CPOs must inform in realtime, through IOP, eMSPs about tariff changes. |

### 2.2 Tarrif_id value

GIREVE uses the “tariff_id” information provided by CPOs in Locations to dispatch CPO’s EVSEs into separated EVSE tariff groups. Also, CPOs can refer to these tariff groups when they describe their roaming offer including tariffs via the [GIREVE connect place](https://connect-place.gireve.com).

**Considering that, we strongly suggest to fill the “tariff_id” information when the CPO uploads its Locations to GIREVE IOP platform.**

## 3 Tariffs module specification

IOP follows the OCPI standard for Tariffs upload by a CPO. [See OCPI specifications](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/mod_tariffs.md).

## 3.1 Tariffs flows implemented by GIREVE

IOP only implements :

- PUT Tariffs: Used by a CPO to create and update a tariff.
- GET Tariffs: Used by IOP to pull Tariffs of a CPO.

## 3.2 Specific properties added by GIREVE

TariffDimensionType “SESSION_TIME”

Not yet available
In standard OCPI V2.1.1, the CPO can define 2 different “TariffDimensionType” related to the duration :

- **TIME** : “time charging: defined in hours, step_size multiplier: 1 second” (description from the OCPI Github).
- **PARKING_TIME** : “time not charging : defined in hours, step_size multiplier: 1 second” (description from the OCPI Github).

GIREVE adds a third “TariffDimensionType” named “SESSION_TIME” with the following description:
- **SESSION_TIME** : “time charging or not: defined in hours, step_size multiplier: 1 second”.


3.11.3 Store and forward – PUT TARIFFS
Similarly to a POST Cdrs, Store and Forward mechanism must be implemented to ensure that no Tariffs may be lost, in case of a connection loss. Any PUT Tariffs that didn’t get a correct response from the GIREVE platform IOP (timeout, http code 500) must be stored on CPO side and a retry process must be active. After the connection recovery, the Tariffs messages must be resent in a FIFO manner.eMSP Specific Implementation Guidelines.
