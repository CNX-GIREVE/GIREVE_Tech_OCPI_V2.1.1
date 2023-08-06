# [<- Back to module selection](emsp_edits.md)

# Contents 

* [4.1 Use Cases Covered By IOP](#41-use-cases-covered-by-iop)
  - 4.1.3 Roaming
* [4.2 Use Cases Required By GIREVE](#42-use-cases-required-by-gireve)
  - 4.2.4 If the eMSP implements the “Sessions” feature
* [4.8 CDRs module specifications](#48-cdrs-module-specifications)
  - 4.8.1 Usage of “authorization_id”
  - 4.8.2 CDR content
  - 4.8.3 Add billing information in “Remark” field
  - 4.8.4 Get the signed data (Calibration Law / Eichrecht)
* [5.6 Exemples](#56-exemples)
  - 5.6.2 ToIOP_GET_cpo_cdrs_2.1.1, FromIOP_GET_cpo_cdrs_2.1.1

***


## 4.1 Use cases covered by IOP 

OCPI features are composed by several use cases that an eMSP can choose to implement or not when connecting to an operator. In case of connection to GIREVE, here is the list of use cases that a CPO can implement :


### 4.1.3 Roaming

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| Push CDRSessions | IOP sends to an eMSP a CDR related to a terminated charging-session. |
| Pull CDR | ToIOP | An eMSP requests IOP to get CDRs that belongs to it. |


## 4.2 Use cases required by GIREVE

Some of these use cases are required when connecting to GIREVE :

### 4.2.4 If the eMSP implements the “Roaming” feature

| Use case |  Why ? | 
| ----------- | ----------- |
| Push CDR/FromIOP OR Pull CDR/ToIOP | Getting CDR is mandatory to enable roaming. | 

## 4.8 CDRs module specifications

IOP follows the OCPI standard for Sessions sent by a CPO. [*See OCPI specifications*](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/mod_cdrs.md)*.*

### 4.8.1 Usage of “authorization_id”

If an “authorization_id” has been provided by the eMSP during the Authorisation, this information will be defined in all CDRs requests sent to the eMSP. It allows eMSPs to link the CDR with its previous Authorisation.

Please refer to paragraph *2.4.2 New attribute « authorization_id »*.

### 4.8.2 CDR content

The “total_time” value is the total duration of this session (including the duration of charging and not charging).

It might not include the duration during which the EVSE is out of order so cannot supply the service. The out of order duration should be free of charge for eMSPs.

### 4.8.3Add billing information in “Remark” field

In some cases, the CPO is not able to send a consistent B2B price in the CDR. (“total_cost” not mandatory for CPOs connected through eMIP protocol, CPO not able to calculate the price, …)

For these reasons GIREVE has implemented a billing feature, included in its “Clearing” service, which calculates the B2B price for a given CDR and injects this information in the CDR.

IOP uses the “Remark” field in CDRs sent to the eMSP to send extra information about this billing.

The “Remark” field is prefixed by :

-   [\*] prefix: The total_cost provided in the CDR is calculated by GIREVE.
-   [?] prefix: The total_cost provided in the CDR is not significant. The B2B price has not been sent by the CPO and/or the eMSP has not subscribed to GIREVE Clearing service.
-   No prefix: The total_cost provided in the CDR has been sent by the CPO.

### 4.8.4 Get the signed data (Calibration Law / Eichrecht)

GIREVE has extended OCPI 2.1.1 for eMSPs to receive signed data in CDRs, aiming to be compliant with the German calibration law (Eichrecht).

eMSPs can ask GIREVE to activate the transfer of these signed data, in the case where the CPO sends them in CDRs.

These signed data are also included in CDRs following the [OCPI 2.2 specifications](https://github.com/ocpi/ocpi/blob/master/mod_cdrs.asciidoc#mod_cdrs_signed_data_class).

## 5.6 Exemples

5.6.2	ToIOP_GET_cpo_cdrs_2.1.1, FromIOP_GET_cpo_cdrs_2.1.1

*URL*:

`/ocpi/cpo/2.1.1/cdrs?date_from=2020-01-20T02:44:00Z&offset=40&limit=20`
*(Date_from parameter shall be the date of the last successful request)*

*Request*:

- **VERB: GET**
- **HEADERS**: `{Authorization:Token xxx-xxx-xxx}{Connection:close}{Accept:application/json}`
- **BODY**: (No body)

*Response*:

- **CODE**: 200
- **HEADERS**: `{X-Limit:20}{Connection:close}{Content-Type:application/json}{X-Total-Count:57}`
- **BODY**:
```json
{
    "data": [
        {
            "total_time": 0,
            "location": {
                "evses": [
                    {
                        "uid": "1",
                        "connectors": [
                            {
                                "id": "1",
                                "amperage": 32,
                                "standard": "IEC_62196_T2",
                                "voltage": 380,
                                "power_type": "AC_3_PHASE",
                                "last_updated": "2019-12-30T09:26:13Z",
                                "format": "SOCKET"
                            }
                        ],
                        "status": "CHARGING",
                        "last_updated": "2019-12-30T09:26:13Z",
                        "capabilities": [
                            "RFID_READER",
                            "CREDIT_CARD_PAYABLE"
                        ],
                        "coordinates": {
                            "longitude": "11.111111",
                            "latitude": "11.111111"
                        },
                        "floor_level": "0",
                        "evse_id": "FR*CPO*e1111"
                    }
                ],
                "id": "2",
                "address": "address",
                "postal_code": "11111",
                "name": "name",
                "opening_times": {
                    "twentyfourseven": true
                },
                "last_updated": "2019-12-30T09:26:13Z",
                "type": "UNKNOWN",
                "operator": {
                    "name": "FR*CPO"
                },
                "coordinates": {
                    "longitude": "11.111111",
                    "latitude": "11.111111"
                },
                "country": "FRA",
                "city": "city"
            },
            "remark": "[?]",
            "auth_id": "1111111",
            "auth_method": "WHITELIST",
            "stop_date_time": "2020-01-20T16:17:58Z",
            "charging_periods": [
                {
                    "dimensions": [
                        {
                            "volume": 0.0,
                            "type": "TIME"
                        }
                    ],
                    "start_date_time": "2020-01-20T16:17:37Z"
                }
            ],
            "start_date_time": "2020-01-20T16:17:37Z",
            "authorization_id": "aaa-eee-rrr",
            "total_energy": 0.0,
            "currency": "EUR",
            "id": "111-222",
            "total_cost": 0,
            "last_updated": "2020-01-20T17:00:22Z",
            "total_parking_time": 0
        },
        {
            "total_time": 0.0167,
            "location": {
                "evses": [
                    {
                        "uid": "1",
                        "connectors": [
                            {
                                "id": "1",
                                "amperage": 32,
                                "standard": "IEC_62196_T2",
                                "voltage": 380,
                                "power_type": "AC_3_PHASE",
                                "last_updated": "2019-12-30T09:26:13Z",
                                "format": "SOCKET"
                            }
                        ],
                        "status": "CHARGING",
                        "last_updated": "2019-12-30T09:26:13Z",
                        "capabilities": [
                            "RFID_READER",
                            "CREDIT_CARD_PAYABLE"
                        ],
                        "coordinates": {
                            "longitude": "11.111111",
                            "latitude": "11.111111"
                        },
                        "floor_level": "0",
                        "evse_id": "FR*CPO*e1111"
                    }
                ],
                "id": "2",
                "address": "address",
                "postal_code": "11111",
                "name": "name",
                "opening_times": {
                    "twentyfourseven": true
                },
                "last_updated": "2019-12-30T09:26:13Z",
                "type": "UNKNOWN",
                "operator": {
                    "name": "FR*CPO"
                },
                "coordinates": {
                    "longitude": "11.111111",
                    "latitude": "11.111111"
                },
                "country": "FRA",
                "city": "city"
            },
            "remark": "[?]",
            "auth_id": "2222222",
            "auth_method": "WHITELIST",
            "stop_date_time": "2020-01-20T16:19:34Z",
            "charging_periods": [
                {
                    "dimensions": [
                        {
                            "volume": 0.0,
                            "type": "TIME"
                        }
                    ],
                    "start_date_time": "2020-01-20T16:17:57Z"
                }
            ],
            "start_date_time": "2020-01-20T16:17:57Z",
            "authorization_id": "aaa-eee-rrr2",
            "total_energy": 0.0,
            "currency": "EUR",
            "id": "eee-rrr",
            "total_cost": 0,
            "last_updated": "2020-01-20T17:04:28Z",
            "total_parking_time": 0
        }
    ],
    "status_code": 1000,
    "status_message": "Success",
    "timestamp": "2020-01-21T02:47:32Z"
}
```
