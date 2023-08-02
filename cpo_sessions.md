# Contents

* [3.3 Use Cases Covered By IOP](#33-use-cases-covered-by-iop)
  - 3.3.3 Roaming
* [3.4 Use Cases Required By GIREVE](#34-use-cases-required-by-gireve)
  - 3.4.1 Always required
* [3.9 Sessions module specification](#39-sessions-module-specifications)
  - 3.9.1 Session Initialisation
  - 3.9.2 Usage of “authorization_id”
  - 3.9.3 Store and forward – PUT Sessions
* [5.5 Exemples](#55-exemples)
  - 5.5.1 ToIOP_PUT_emsp_sessions_2.1.1, FromIOP_PUT_emsp_sessions_2.1.1
  - 5.5.2 ToIOP_PATCH_emsp_sessions_2.1.1, FromIOP_PATCH_emsp_sessions_2.1.1

***


## 3.3 Use cases covered by IOP

OCPI features are composed by several use cases that a CPO can choose to implement or not when connecting to an operator.

In case of connection to GIREVE, here is the list of use cases that a CPO can implement :

## 3.3.3 Roaming

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| Push Session | ToIOP | A CPO requests IOP to initialise or update an OCPI Session object following an positive Authorisation. |
| Check Session | ToIOP | A CPO requests IOP to get Sessions it has previously initialised. |

## 3.4. Use cases required by GIREVE

Some of these use cases are required when connecting to GIREVE :

### 3.4.2 If the CPO implements the “Roaming” feature

| Use case | Why ? |
| ----------- | ----------- |
| **PUT** Session FromIOP | A CPO must be able to send information about charging-sessions through Session objects (charge started, …). |

## 3.9 Sessions module specifications

IOP follows the OCPI standard for Sessions sent by a CPO. [See OCPI specifications](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/mod_sessions.md).

### 3.9.1 Session Initialisation

The CPO must send, after a remote or local authorization validated by the eMSP, a PUT Session with SessionStatus when the EV plugs to the EVSE.
This flow gives information to eMSP that the charge of its customer has really started.

### 3.9.2 Usage of “authorization_id”

When sending a Session, the CPO defines the Authorization it refers to on providing the “authorization_id” property. Please refer to paragraph 5. [New attribute « authorization_id »](checkup_edits.md).

### 3.9.3 Store and forward – PUT Sessions

A Store and Forward mechanism must be implemented to ensure that no session may be lost, in case of a connection loss. Any PUT session that didn’t get a correct response from the GIREVE platform IOP (timeout, http code 500) must be stored on CPO side and a retry process must be active. After the connection recovery, the session messages must be resent in a FIFO manner.

## 5.5 Exemples

### 5.5.1 ToIOP_PUT_emsp_sessions_2.1.1, FromIOP_PUT_emsp_sessions_2.1.1

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

### 5.5.2 ToIOP_PATCH_emsp_sessions_2.1.1, FromIOP_PATCH_emsp_sessions_2.1.1

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
