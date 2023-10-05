### [<- Back to module selection](cpo_edits.md)

# Contents

* [Use Cases Covered By IOP](#use-cases-covered-by-iop)
  - Roaming
* [Use Cases Required By Gireve](#use-cases-required-by-gireve)
  - If the CPO implements the “Roaming” feature
* [Cdrs module specifications](#cdrs-module-specifications)
  - CDR sending frequency
  - Usage of "authorization_id"
  - CDR content
  - Store and Forward - POST CDRs
  - Send the signed data (Calibration Law/Eichrecht)
* [Examples](#examples)
  - ToIOP_POST_emsp_cdrs_2.1.1, FromIOP_POST_emsp_cdrs_2.1.1

***



## `Use cases covered by IOP`

OCPI features are composed by several use cases that a CPO can choose to implement or not when connecting to an operator.

In case of connection to Gireve, here is the list of use cases that a CPO can implement :

## `Roaming`

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| Push CDR | ToIOP | A CPO sends to IOP, a CDR related to a terminated charging-session. |
| Pull CDR | FromIOP | IOP requests a CPO to retrieve CDRs that belongs to it. |
| Check CDR | ToIOP | A CPO requests IOP to get CDRs it has previously sent. |

## `Use cases required by Gireve`

Some of these use cases are required when connecting to Gireve :

### If the CPO implements the “Roaming” feature

| Use case | Usage |
| ----------- | ----------- |
| Push CDR/ToIOP | The CPO must send the CDR directly after the end of the charging-session. |
| Pull CDR/FromIOP | Gireve must be able to retrieve CDRs from CPO platform when needed. |

## `CDRs module specifications`

IOP follows the OCPI standard for Sessions sent by a CPO. [See OCPI specifications](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/mod_cdrs.md).

### CDR sending frequency

> :warning: **<ins>Gireve requires CDRs to be sent directly at the end of the charging-session.</ins>**
eMSPs will so be able to display the charging-session price directly to their end customers.

### Usage of “authorization_id”

When sending a CDR, the CPO defines the Authorization it refers to on providing the “authorization_id” property. Please refer to  [New attribute « authorization_id »](integration_guidelines.md).
### CDR content

The “total_time” value is the total duration of this session (including the duration of charging and not charging).
It doesn’t include the duration during which the EVSE is out of order so cannot supply the service. 
The out of order duration should be free of charge for eMSPs.

### Store and forward – POST CDRs

> :warning: Similarly to a **PUT sessions**, **<ins>A [Store and Forward mechanism](q&a.md/#store-and-forward-mechanism) must be implemented</ins>** to ensure that no CDR may be lost, in case of a connection loss. 
Any **POST** Cdrs that didn’t get a correct response from the Gireve platform IOP (timeout, http code 500) **<ins>must be stored on CPO side and a retry process must be active</ins>**. After the connection recovery, the Cdr messages must be resent in a FIFO manner.

### Send the signed data (Calibration Law / Eichrecht)

Gireve has extended OCPI 2.1.1 for CPOs to send signed data in CDRs, aiming to be compliant with the German calibration law (Eichrecht).
These signed data are included in CDRs following the [OCPI 2.2 specifications](https://github.com/ocpi/ocpi/blob/master/mod_cdrs.asciidoc#mod_cdrs_signed_data_class).

*To activate this feature, the CPO must validate its implementation through the standard Gireve certification process before the deployment and activation on PRODUCTION. (Please contact connection@gireve.com).*

## `Examples`

### ToIOP_POST_emsp_cdrs_2.1.1, FromIOP_POST_emsp_cdrs_2.1.1

*URL* :

`/ocpi/emsp/2.1.1/cdrs`

*Request* :

- **VERB : POST**
- **HEADERS** : `{Authorization:Token xxx-xxx-xxx}{Connection:close}{Accept:application/json}{Content-Type:application/json}`
- **BODY** :
```json

{
    "id": "AAAAA",
    "start_date_time": "2020-01-17T07:59:07",
    "stop_date_time": "2020-01-17T07:59:08",
    "auth_id": "FR*EMP*1111",
    "authorization_id": "mmm-ttt",
    "auth_method": "AUTH_REQUEST",
    "location": {
        "id": "1",
        "type": "ON_STREET",
        "name": "name",
        "address": "address",
        "city": "city",
        "postal_code": "11111",
        "country": "FRA",
        "coordinates": {
            "latitude": "11.111111",
            "longitude": "11.111111"
        },
        "evses": [
            {
                "uid": "23",
                "evse_id": "FR*CPO*E111",
                "status": "AVAILABLE",
                "capabilities": [
                    "REMOTE_START_STOP_CAPABLE",
                    "RFID_READER",
                    "UNLOCK_CAPABLE"
                ],
                "connectors": [
                    {
                        "id": "1",
                        "standard": "IEC_62196_T2",
                        "format": "SOCKET",
                        "power_type": "AC_3_PHASE",
                        "voltage": 230,
                        "amperage": 32,
                        "last_updated": "2019-12-02T16:17:07"
                    }
                ],
                "last_updated": "2019-08-26T16:46:46"
            }
        ],
        "opening_times": {
            "twentyfourseven": true
        },
        "last_updated": "2019-08-26T13:53:05"
    },
    "currency": "EUR",
    "charging_periods": [
        {
            "start_date_time": "2020-01-17T07:59:07",
            "dimensions": [
                {
                    "type": "TIME",
                    "volume": 0.0002
                }
            ]
        }
    ],
    "total_cost": 0,
    "total_energy": 0,
    "total_time": 0.0002,
    "last_updated": "2020-01-17T07:59:19"
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
