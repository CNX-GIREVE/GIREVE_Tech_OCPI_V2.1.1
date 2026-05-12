### [<- Back to module selection](cpo_edits.md)

# Contents

* [Use Cases Covered By IOP](#use-cases-covered-by-iop)
  - Roaming                                                            
* [Use Cases Required By Gireve](#use-cases-required-by-gireve)
  - If the CPO doesn’t commit and describe its tariffs in a roaming agreement 
  - Tarrif_id value
* [Tariffs module specifications](#tariffs-module-specifications)
  - Tariffs flows implemented by Gireve
  - Specific properties added by Gireve
  - Store and forward – PUT TARIFFS
* [Examples](#examples)
  - ToIOP_PUT_emsp_tariffs_2.1.1
  - ToIOP_GET_cpo_tariffs_2.1.1, FromIOP_GET_cpo_tariffs_2.1.1

***


## `Use cases covered by IOP`

OCPI features are composed by several use cases that a CPO can choose to implement or not when connecting to an operator.

In case of connection to Gireve, here is the list of use cases that a CPO can implement :

## `Roaming`

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| **Push Tariffs** | ToIOP | A CPO sends to IOP, a Tariffs object. |
| **Pull Tariffs** | FromIOP | IOP requests the CPO backend to retrieve Tariffs data. |

## `Use cases required by Gireve`

Some of these use cases are required when connecting to Gireve :

### If the CPO doesn’t commit and describe its tariffs in a roaming agreement

| Use case | Why ? |
| ----------- | ----------- |
| **Push Tariffs ToIOP** | CPOs must inform in realtime, through IOP, eMSPs about tariff changes. |
| **Pull Tariffs FromIOP** | CPOs must inform in realtime, through IOP, eMSPs about tariff changes. |

### Tarrif_id value

Gireve uses the “tariff_id” information provided by CPOs in Locations to dispatch CPO’s EVSEs into separated EVSE tariff groups. Also, CPOs can refer to these tariff groups when they describe their roaming offer including tariffs via the [Gireve connect place](https://connect-place.Gireve.com).

> :bulb: **<ins>Considering that, we strongly suggest to fill the “tariff_id” information when the CPO uploads its Locations to Gireve IOP platform.</ins>**

## `Tariffs module specifications`

IOP follows the OCPI standard for Tariffs upload by a CPO. [See OCPI specifications](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/mod_tariffs.md).

### 3.11.1 Tariffs flows implemented by Gireve

IOP only implements :

- **PUT** Tariffs: Used by a CPO to create and update a tariff.
- **GET** Tariffs: Used by IOP to pull Tariffs of a CPO.

### Specific properties added by Gireve

TariffDimensionType “SESSION_TIME”

Not yet available
In standard OCPI V2.1.1, the CPO can define 2 different “TariffDimensionType” related to the duration :

- **TIME** : “time charging: defined in hours, step_size multiplier: 1 second” (description from the OCPI Github).
- **PARKING_TIME** : “time not charging : defined in hours, step_size multiplier: 1 second” (description from the OCPI Github).

Gireve adds a third “TariffDimensionType” named “SESSION_TIME” with the following description :

- **SESSION_TIME** : “time charging or not: defined in hours, step_size multiplier: 1 second”.


### Store and forward – PUT TARIFFS

For Tariffs, retries are optional and non-blocking. A retry mechanism may be implemented, but it is only recommended under controlled conditions: retries should be infrequent (in hours, not minutes) and must not follow any loop logic. This is because tariff data is relatively static and does not carry real-time criticality.

Retries must not be executed immediately when receiving platform error codes such as 425 (Too Early) or 429 (Too Many Requests). These errors indicate that requests are being sent too early or too frequently, and immediate retries would worsen the situation. In such cases, the client is expected to wait at least one hour before retrying, using a progressive backoff strategy.

A strict retry policy must be applied: retries must never be executed in an uncontrolled loop or in parallel bursts, especially when no successful response (HTTP 2XX) has been received. As a general recommendation, no more than one retry per hour should be performed. Ideally, a dedicated processing queue should be implemented per flow type (Sessions, CDRs, Tokens, Tariffs, etc.), with sequential processing to ensure system stability and compliance.

## `Examples`

### ToIOP_PUT_emsp_tariffs_2.1.1

*URL* :

- /ocpi/emsp/2.1.1/tariffs/FR/CPO/Tariff-1

*Request* :

- **VERB : PUT**
- **HEADERS** : {Authorization:Token xxx-xxx-xxx}{Connection:close}{Accept:application/json}
- **BODY** :
```json
{
    "id": "Tariff-1",
    "currency": "EUR",
    "elements": [
        {
            "price_components": [
                {
                    "type": "TIME",
                    "price": 6,
                    "step_size": 1
                },
                {
                    "type": "FLAT",
                    "price": 1,
                    "step_size": 1
                },
                {
                    "type": "ENERGY",
                    "price": 2,
                    "step_size": 1
                }
            ]
        }
    ],
    "last_updated": "2021-02-11T16:46:37Z"
}
```

*Response* : 

- **CODE** : 200
- **HEADERS** : {Content-Type:application/json
- **BODY** :
```json 
{
    "data": {},
    "status_code": 1000,
    "status_message": "Success",
    "timestamp": "2021-02-12T09:39:42Z"
}

```

### ToIOP_GET_cpo_tariffs_2.1.1, FromIOP_GET_cpo_tariffs_2.1.1

*URL* :

- /ocpi/cpo/2.1.1/tariffs?offset=0&limit=10&date_from=2021-02-10T04:00:00Z&date_to=2021-02-11T04:00:00Z
  (Le paramètre "date_from" doit être la date de la dernière requête réussie)

*Request* :

- **VERB : GET**
- **HEADERS** : {Authorization:Token xxx-xxx-xxx}{Connection:close}{Accept:application/json}
- **BODY** : (Le corps de la requête est vide)

*Response* : 

- **CODE** : 200
- **HEADERS** : {X-Limit:10}{Connection:close}{Content-Type:application/json}{X-Total-Count:2}
- **BODY** :
```json 
{
  "data": [
    {
      "id": "aaa-xxxx",
      "party_id": "AAA",
      "last_updated": "2015-06-29T20:39:09Z",
      "tariff_alt_url": "https://xxxxxxx.com/yyyyy/11",
      "country_code": "FR",
      "elements": [
        {
          "restrictions": {
            "max_duration": 200000,
            "day_of_week": [
              "MONDAY",
              "TUESDAY"
            ]
          },
          "price_components": [
            {
              "price": 1,
              "step_size": 910,
              "type": "TIME"
            },
            {
              "price": 1,
              "step_size": 900,
              "type": "PARKING_TIME"
            }
          ]
        }
      ],
      "currency": "EUR"
    }
  ],
  "status_code": 1000,
  "status_message": "Success",
  "timestamp": "2020-12-22T09:50:25Z"
}
```
