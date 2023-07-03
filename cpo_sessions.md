## Use cases covered by IOP

OCPI features are composed by several use cases that a CPO can choose to implement or not when connecting to an operator.

In case of connection to GIREVE, here is the list of use cases that a CPO can implement :

## 1. Technical Use cases

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| Push Session | ToIOP | A CPO requests IOP to initialise or update an OCPI Session object following an positive Authorisation. |
| Check Session | ToIOP | A CPO requests IOP to get Sessions it has previously initialised. |

**If a CPO manages several operations, each operation must go through the OCPI connection process (handshake).**

## 2. Use cases required by GIREVE

Some of these use cases are required when connecting to GIREVE.

### 2.1 Always required
