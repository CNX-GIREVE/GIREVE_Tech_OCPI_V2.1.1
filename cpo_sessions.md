### [<- Back to module selection](cpo_edits.md)

# Contents

* [Use Cases Covered By IOP](#use-cases-covered-by-iop)
  - Roaming
* [Use Cases Required By Gireve](#use-cases-required-by-gireve)
  - Always required
* [Sessions module specification](#sessions-module-specifications)
  - Session Initialisation
  - Usage of “authorization_id”
  - Store and forward – PUT Sessions
* [Examples](#examples)
  - ToIOP_PUT_emsp_sessions_2.1.1, FromIOP_PUT_emsp_sessions_2.1.1
  - ToIOP_PATCH_emsp_sessions_2.1.1, FromIOP_PATCH_emsp_sessions_2.1.1

***


## `Use cases covered by IOP`

OCPI features are composed by several use cases that a CPO can choose to implement or not when connecting to an operator.

In case of connection to Gireve, here is the list of use cases that a CPO can implement :

### Roaming

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| **Push Session** | ToIOP | A CPO requests IOP to initialise or update an OCPI Session object following an positive Authorisation. |
| **Check Session** | ToIOP | A CPO requests IOP to get Sessions it has previously initialised. |

## `Use cases required by Gireve`

Some of these use cases are required when connecting to Gireve :

### If the CPO implements the “Roaming” feature

| Use case | Why ? |
| ----------- | ----------- |
| **PUT** Session FromIOP | A CPO must be able to send information about charging-sessions through Session objects (charge started, …). |

## `Sessions module specifications`

IOP follows the OCPI standard for Sessions sent by a CPO. [See OCPI specifications](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/mod_sessions.md).

### Session Initialisation

The CPO must send, after a remote or local authorization validated by the eMSP, a PUT Session with SessionStatus when the EV plugs to the EVSE.
This flow gives information to eMSP that the charge of its customer has really started.

### Usage of “authorization_id”

When sending a Session, the CPO defines the Authorization it refers to on providing the “authorization_id” property. Please refer to [New attribute « authorization_id »](integrations_guidelines.md).

### Store and forward – PUT Sessions

**<ins>A Store and Forward mechanism must be implemented</ins>** to ensure that no session may be lost, in case of a connection loss. Any PUT session that didn’t get a correct response from the Gireve platform IOP (timeout, http code 500) must be stored on CPO side and a retry process must be active. After the connection recovery, the session messages must be resent in a FIFO manner.

## `Examples`

### ToIOP_PUT_emsp_sessions_2.1.1, FromIOP_PUT_emsp_sessions_2.1.1

*URL* :

`//ocpi/emsp/2.1.1/sessions/FR/CPO/AAAAAAA`
*(FR and CPO refer to the country_code and party_id of the CPO which owns the Session cf. 2.1.3 Client owned object push)*

*Request* :

- **VERB : PUT**
- **HEADERS** : `{Authorization:Token xxx-xxx-xxx}{Connection:close}{Accept:application/json}{Content-Type:application/json}`
- **BODY** :
```json
{
    "kwh": 0,
    "location": {
        "type": "UNKNOWN",
        "name": "name",
        "address": "address",
        "city": "city",
        "country": "NLD",
        "coordinates": {
            "latitude": "11.111111",
            "longitude": "11.111111"
        },
        "operator": {
            "name": "name"
        },
        "suboperator": {
            "name": "suboperator"
        },
        "id": "aaaa",
        "postal_code": "11111",
        "evses": [
            {
                "uid": "aaaaa",
                "status": "CHARGING",
                "capabilities": [
                    "RFID_READER",
                    "REMOTE_START_STOP_CAPABLE",
                    "CREDIT_CARD_PAYABLE"
                ],
                "connectors": [
                    {
                        "standard": "CHADEMO",
                        "format": "CABLE",
                        "voltage": 400,
                        "amperage": 125,
                        "id": "1",
                        "power_type": "DC",
                        "last_updated": "2020-01-17T08:54:41.000Z"
                    }
                ],
                "coordinates": {
                    "latitude": "11.111111",
                    "longitude": "11.111111"
                },
                "evse_id": "FR*CPO*E111",
                "last_updated": "2020-01-17T08:54:41.000Z"
            }
        ],
        "time_zone": "Europe/Amsterdam",
        "opening_times": {
            "twentyfourseven": true
        },
        "last_updated": "2020-01-17T08:54:41.000Z"
    },
    "currency": "EUR",
    "status": "ACTIVE",
    "id": "AAAAAAA",
    "start_datetime": "2020-01-17T09:39:29.000Z",
    "auth_id": "FR*EMP*11111",
    "auth_method": "AUTH_REQUEST",
    "charging_periods": [],
    "total_cost": 0,
    "last_updated": "2020-01-17T09:39:41.356Z",
    "authorization_id": "mmm-ppp"
}

```
  

*Response* :

- **CODE** : 200
- **HEADERS** : `{Content-Type:application/json}`
- **BODY** :  
```json

{
    "data": {},
    "status_code": 1000,
    "status_message": "Success",
    "timestamp": "2020-01-17T09:39:42Z"
}

```

### ToIOP_PATCH_emsp_sessions_2.1.1, FromIOP_PATCH_emsp_sessions_2.1.1

*URL* :

`/ocpi/emsp/2.1.1/sessions/FR/CPO/AAAAAAA`
*(FR and CPO refer to the country_code and party_id of the CPO which owns the Session cf. 2.1.3 Client owned object push)*

*Request* :

- **VERB : PUT**
- **HEADERS** : `{Authorization:Token xxx-xxx-xxx}{Connection:close}{Accept:application/json}{Content-Type:application/json}`
- **BODY** :
```json

  {
    "kwh": 2,
    "id": "AAAAAAA"
}

```

*Response* :

- **CODE** : 200
- **HEADERS** : `{Content-Type:application/json}`
- **BODY** :  
```json

{
    "data": {},
    "status_code": 1000,
    "status_message": "Success",
    "timestamp": "2020-01-17T09:39:42Z"
}

```
