### [<- Back to module selection](cpo_edits.md)

# Contents

* [CPO Operation Definition And Naming Rules](#cpo-operation-definition-and-naming-rules)
* [CPO Operation And Roaming Offers](#cpo-operation-and-roaming-offers)
* [Use Cases Covered By IOP](#use-cases-covered-by-iop)
  - Technical Use cases
* [Use Cases Required By Gireve](#use-cases-required-by-gireve)
  - Always required
* [Connection & Register specifications](#connection-and-register-specifications)
  - Update Credentials FromIOP
* [Examples](#examples)
  - ToIOP_GET_emsp_versions, FromIOP_GET_emsp_versions
  - ToIOP_GET_emsp_version_detail_2.1.1, FromIOP_GET_emsp_version_detail_2.1.1
  - ToIOP_POST_emsp_credentials_2.1.1, FromIOP_POST_emsp_credentials_2.1.1
  - ToIOP_DELETE_emsp_credentials_2.1.1, FromIOP_DELETE_emsp_credentials_2.1.1

***

## `CPO operation definition and naming rules`

A CPO can manage one or multiple CPO operations. A CPO operation is a homogeneous group of charging points. A CPO operation is defined by its unique eMI3 operation code also called “Operator Id” which is composed by a 2 ALPHA country code plus a 3 ALPHANUMERIC Spot Operator ID.

**Example: FR\*AB1**

eMI3 standard official documentation can be downloaded here: <https://emi3group.com/documents-links/>

All EVSE included in a given CPO operation must have an evse_id following the eMI3 standard naming rules defined in this specific document, page 27 :

<https://emi3group.com/wp-content/uploads/sites/5/2018/12/eMI3-standard-v1.0-Part-2.pdf>

Specifically, all evse_id included into a given operation must always start with the Operator Id.

**Example: FR\*AB1\*EABCDEFG\*1**

It is not allowed to include in a given CPO operation evse_ids that start with a different Operator Id.

## `CPO operation and roaming offers`

Reminder : If a CPO manages multiple operations, each operation has to go through the OCPI connection establishment process (handshake). 
A CPO Operation is the smallest entity that can constitute a roaming offer.
Although Gireve’s technical platform (IOP) is built to manage bilateral communications between 1 operation (i.e. CPO) and another (i.e. eMSP), it is possible to aggregate several operations into a so-called network group on Gireve’s Market Place (Connect-Place). It allows to publish a single roaming offer that includes several operations. The main advantages of such an offer structure are:
•	Reduces the number of roaming agreements (1 agreement instead of several bilateral contracts – one per operation).
•	Allows to add or remove operations from the network without terminating the current agreement or issuing amendments, enabling new tenants to benefit from already active roaming agreements. 

## `Use cases covered by IOP`

OCPI features are composed by several use cases that a CPO can choose to implement or not when connecting to an operator.

In case of connection to Gireve, here is the list of use cases that a CPO can implement :

### Technical Use cases

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| **Register** | FromIOP | IOP initialises the connection to a CPO beginning by a GET_Versions, then exchanging endpoints and Authorization-token. |
| **Register** | ToIOP | A CPO initialises the connection to IOP beginning by a GET_Versions, then exchanging endpoints and Authorization-token. |
| **Update CPO** credentials | ToIOP | A CPO already registered requests IOP to update its Credentials (endPoints, versions, authorization-token). |
| **Unregister** | FromIOP | IOP unregisters a CPO by requesting a DELETE Credentials. |
| **Unregister** | ToIOP | A CPO unregisters IOP by requesting a DELETE Credentials. |

**If a CPO manages several operations, each operation must go through the OCPI connection process (handshake).**


## `Use cases required by Gireve`

Some of these use cases are required when connecting to Gireve :

### Always required

| Use case |  Why ? | 
| ----------- | ----------- |
| **Register/FromIOP OR Register/ToIOP** |  These use cases are needed to initialise connection between an operator and Gireve. | 

## `Connection And Register specifications`

IOP follows the OCPI standard for Connection & Register process. [See OCPI specifications](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/credentials.md).

### Update Credentials FromIOP

IOP is not able to send a **PUT** Credentials to update its Credentials on operator backend. In order to update them, IOP sends a “**DELETE** Credentials” to delete the OCPI connection then starts a new Connection & Register process beginning by a **POST** Credentials sent by IOP or by the operator.

## `Examples`

### ToIOP_GET_emsp_versions, FromIOP_GET_emsp_versions

*Request* :

- **VERB : GET**
- **HEADERS** : `{Transfer-Encoding:chunked}{Date:Wed, 08 Jan 2020 10:24:52 GMT}{Connection:Close}{Content-Type:application/json}`
- **BODY** : EMPTY

*Response* :

- **CODE** : 200
- **HEADERS** : `{Transfer-Encoding:chunked}{Date:Wed, 08 Jan 2020 10:24:52 GMT}{Connection:Close}{Content-Type:application/json}`
- **BODY** :  
```json
{
    "data": [
        {
            "version": "2.1.1",
            "url": "https://xxx.yyy.com/ocpi/emsp/2.1.1"
        }
    ],
    "status_code": 1000,
    "status_message": "Success",
    "timestamp": "2020-01-08T10:24:52Z"
}

```

### ToIOP_GET_emsp_version_detail_2.1.1, FromIOP_GET_emsp_version_detail_2.1.1

*Request* :

- **VERB : GET**
- **HEADERS** : `{connection:close}{authorization:Token xxx-xxx-xxx}`
- **BODY** : EMPTY

*Response* :

- **CODE** : 200
- **HEADERS** : `{Date:Thu, 16 Jan 2020 07:30:16 GMT}{Connection:close}{Content-Type:application/json}`
- **BODY** :
```json

{
    "data": {
        "endpoints": [
            {
                "identifier": "credentials",
                "url": "https:// ccc.yyy.com/ocpi/emsp/2.1.1/credentials"
            },
            {
                "identifier": "locations",
                "url": "https:// ccc.yyy.com/ocpi/emsp/2.1.1/locations"
            },
            {
                "identifier": "tokens",
                "url": "https:// ccc.yyy.com/ocpi/emsp/2.1.1/tokens"
            },
            {
                "identifier": "commands",
                "url": "https:// ccc.yyy.com/ocpi/emsp/2.1.1/commands"
            },
            {
                "identifier": "sessions",
                "url": "https:// ccc.yyy.com/ocpi/emsp/2.1.1/sessions"
            },
            {
                "identifier": "cdrs",
                "url": "https:// ccc.yyy.com/ocpi/emsp/2.1.1/cdrs"
            }
        ],
        "version": "2.1.1"
    },
    "status_code": 1000,
    "status_message": "Success",
    "timestamp": "2020-01-08T10:24:52Z"
}

```

### ToIOP_POST_emsp_credentials_2.1.1, FromIOP_POST_emsp_credentials_2.1.1

Request* :

- **VERB : POST**
- **HEADERS** : `{content-type:application/json; charset=UTF-8}{connection:close}{accept:application/json, application/*+json}{authorization:Token xxx-xxx-xxx}`
- **BODY** :
```json
{
    "business_details": {
        "logo": {
            "category": "NETWORK",
            "height": 44,
            "thumbnail": "http://xxx.xxx.com/yyyy.png",
            "width": 92,
            "type": "png",
            "url": " http://xxx.xxx.com/logo.png"
        },
        "website": "http://xxx.com",
        "name": "NAME"
    },
    "token": "aaa-xxx-eee",
    "party_id": "PID",
    "country_code": "FR",
    "url": "https://xxx.com/ocpi/emsp/versions"
}

```
*Response* :

- **CODE** : 200
- **HEADERS** : `Date:Thu, 16 Jan 2020 07:30:16 GMT}{Connection:close}{Content-Type:application/json}`
- **BODY** :
```json
{
    "data": {
        "url": "http://xxx.com/ocpi/versions",
        "token": "eee-fff-ddd",
        "party_id": "AAA",
        "country_code": "DE",
        "business_details": {
            "name": "NAME"
        }
    },
    "status_code": 1000,
    "status_message": "Success",
    "timestamp": "2020-01-14T09:10:21Z"
}

```

### ToIOP_DELETE_emsp_credentials_2.1.1, FromIOP_DELETE_emsp_credentials_2.1.1

*Request* :

- **VERB : DELETE**
- **HEADERS** : `{content-type:application/json; charset=UTF-8}{connection:close}{accept:application/json, application/*+json}{authorization:Token xxx-xxx-xxx}`
- **BODY** : EMPTY

*Response* :

- **CODE** : 200
- **HEADERS** : `Date:Thu, 16 Jan 2020 07:30:16 GMT}{Connection:close}{Content-Type:application/json}`
- **BODY** :
```json
{
    "data": {},
    "status_code": 1000,
    "status_message": "Success",
    "timestamp": "2020-01-17T09:39:42Z"
}

```
