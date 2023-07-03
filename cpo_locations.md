## Use cases covered by IOP

OCPI features are composed by several use cases that a CPO can choose to implement or not when connecting to an operator.

In case of connection to GIREVE, here is the list of use cases that a CPO can implement :

## 1. EVCI data

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| Push Locations Static data | ToIOP | A CPO updates its Locations stored in RPC. *Only the property “connector.tariff_Id” can be updated through this flow.* |
| Push Locations Dynamic data (Only EVSE status) | A CPO updates status of its EVSEs stored in RPC. |
| Pull Locations (Static data) | FromIOP | IOP requests the CPO backend to retrieve Locations data. |
| Check Locations | ToIOP | A CPO requests IOP to get its Locations stored in RPC. |
