### [<- Back to module selection](emsp_edits.md)

# Contents

* [4.1 Use Cases Covered By IOP](#41-use-cases-covered-by-iop)
  - 4.1.1 Technical Use cases
* [4.2 Use Cases Required By GIREVE](#42-use-cases-required-by-gireve)
  - 4.2.1 Always required
* [4.3 Connection & Register specifications](#43-connection-and-register-specifications)
  - 4.3.1 Update Credentials FromIOP
* [5.1 Exemples](#51-exemples)
  - 5.1.1 ToIOP_GET_cpo_versions, FromIOP_GET_cpo_versions
  - 5.1.2 ToIOP_GET_cpo_version_detail_2.1.1, FromIOP_GET_cpo_version_detail_2.1.1
  - 5.1.3 ToIOP_POST_cpo_credentials_2.1.1, FromIOP_POST_cpo_credentials_2.1.1
  - 5.1.4 ToIOP_DELETE_cpo_credentials_2.1.1, FromIOP_DELETE_cpo_credentials_2.1.1

***

## 4.1 Use cases covered by IOP 

OCPI features are composed by several use cases that an eMSP can choose to implement or not when connecting to an operator. In case of connection to GIREVE, here is the list of use cases that a CPO can implement :

### 4.1.1 Technical Use cases

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| Register | **FromIOP** | IOP initialises the connection to a eMSP beginning by a GET_Versions, then exchanging endpoints and Authorization-token. |
| Register | **ToIOP** | An eMSP initialises the connection to IOP beginning by a GET_Versions, then exchanging endpoints and Authorization-token. |
| Update CPO credentials | **ToIOP** | An eMSP already registered requests IOP to update its Credentials (endPoints, versions, authorization-token) |
| Unregister | **FromIOP** | IOP unregisters an eMSP by requesting a DELETE Credentials |
| Unregister | **ToIOP** | An eMSP unregisters IOP by requesting a DELETE Credentials |

## 4.2 Use cases required by GIREVE

Some of these use cases are required when connecting to GIREVE :

### 4.2.1 Always required

| Use case |  Why ? | 
| ----------- | ----------- |
| Register/FromIOP OR Register/ToIOP |  These use cases are needed to initialise connection between an operator and GIREVE. | 

## 4.3 Connection and Register specifications

IOP follows the OCPI standard for Connection & Register process. [*See OCPI specifications.*](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/credentials.md)

### 4.3.1 Update Credentials FromIOP

IOP is not able to send a PUT Credentials to update its Credentials on operator backend. In order to update them, IOP sends a “DELETE Credentials” to delete the OCPI connection then starts a new Connection & Register process beginning by a POST Credentials sent by IOP or by the operator.

## 5 Exemples 

### 5.1.1 ToIOP_GET_cpo_versions, FromIOP_GET_cpo_versions

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

### 5.1.2 ToIOP_GET_cpo_version_detail_2.1.1, FromIOP_GET_cpo_version_detail_2.1.1

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

### 5.1.3 ToIOP_POST_cpo_credentials_2.1.1, FromIOP_POST_cpo_credentials_2.1.1

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

### 5.1.4 ToIOP_DELETE_cpo_credentials_2.1.1, FromIOP_DELETE_cpo_credentials_2.1.1

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
