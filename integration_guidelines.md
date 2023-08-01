# Contents

* [2.1 Technical](#21-technical)
  - [2.1.1 Supported OCPI versions](#211-supported-ocpi-versions)
  - [2.1.2 Security](#212-security)
  - [2.1.3 Client owned object push](#213-client-owned-object-push)
  - [2.1.4 Pagination](#214-pagination)
  - [2.1.5 IOP HTTP headers](#215-iop-http-headers)
* [2.2 OCPI modules implemented by IOP](#22-ocpi-modules-implemented-by-iop)
* [2.3 GIREVE management of Locations data](#23-gireve-management-of-locations-data)
* [2.4 Roaming](#24-roaming)
  - [2.4.1 General workflow](#241-general-workflow)
  - [2.4.2 New attribute « authorization_id »](#242-new-attribute-authorization-id)
  - [2.4.3 Management of B2B tariffs](#243-management-of-b2b-tariffs)
  - [2.4.4 RFID Tokens](#244-rfid-tokens)
  - [2.4.5 Custom OCPI flow to prevent eMSP Tokens download by CPOs](#245-custom-ocpi-flow-to-prevent-emsp-tokens-download-by-cpos)
 
*** 

## 2.1 Technical

### 2.1.1 Supported OCPI versions

IOP OCPI implementation has begun with version 2.1.1. IOP does not support previous versions.

### 2.1.2 Security

IOP follows the security standard mechanisms of OCPI. At the connection, GIREVE provides new operator connecting with temporary Token(s) to register as CPO and/or eMSP. The Token should be used when the connection is initiated by the operater.

In the meantime, the operator can provide GIREVE with temporary Token(s) for IOP to initiate the connection. It is not mandatory as the operator is able to launch the Connection & Register process.

During the Connection & Register process, IOP and the operator exchange final Tokens to request each other, and endpoints of their respective OCPI modules. **GIREVE requires all endpoints to be in HTTPS in order to secure communication between the two backends.**

### 2.1.3 Client owned object push

OCPI introduces a specific use of resource identification mechanism, to manage situation where resource belongs to servers and situation where resource belongs to client. [See OCPI client owned object.](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/transport_and_format.md)

In OCPI, Objects managed through Rest protocol are owned by the CPO or the eMSP:

-   Tokens are owned by the eMSP
-   Locations are owned by the CPO
-   Sessions are owned by the CPO
-   CDRs are owned by the CPO
-   Tarifs are owned by the CPO

IOP acts as a hub which routes the messages between CPOs and eMSPs, so IOP does not own objects exchanged by these two actors during their communication.

**IOP is not the owner of the exchanged resources. Following this principle, IOP uses “country-code” and “partner-id” of the object owner during communication with other operators.**

For example, IOP pushes Locations of a CPO to an eMSP using “country-code/party-id” of the CPO in the URL.

![image](https://github.com/CNX-GIREVE/GIRVE_Tech_OCPI_V2.1.1/assets/137178502/4c52c925-a330-40a8-90f5-55f7fdf3e41a)

### 2.1.4 Pagination

IOP implements pagination mechanisms described by OCPI. [*See OCPI pagination mechanism.*](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/transport_and_format.md#pagination)

IOP requires operators to use pagination when they pull resources requesting IOP. If the operator does not use pagination arguments in its request, IOP will force it answering with the first **X** items and a Link to the second page if any.

Finally, IOP for each module has its own max size limit per page (20 Locations, 1000 Tokens, …). These limits can change with IOP evolutions, the operator implementation might be flexible regarding these limits.

### 2.1.5 IOP HTTP headers

The next version of OCPI, OCPI 2.2, integrates new extra headers enabling the sharing of a single OCPI connection to multiple operators.

GIREVE has begun to deploy these extra headers in its OCPI version 2.1.1 but it is an ongoing action.

**These extra headers should not be considered yet in OCPI 2.1.1, except for “PULL Tokens: Retrieve Tokens of a single given eMSP” (see chapter 3.7.4 page 22) and “PULL Tokens: Retrieve Locations of a single given CPO” (see chapter 4.4.3 page 29):**

-   ocpi-to-country-code
-   ocpi-to-party-id
-   ocpi-from-country-code
-   ocpi-from-party-id

## 2.2 OCPI modules implemented by IOP

All OCPI modules have been implemented in IOP.
List of modules implemented by IOP :

| Module | Usage |
| ----------- | ----------- |
| Credentials | Initialise and update IT connection information (endpoints, OCPI versions, …) |
| Locations | Exchange charge infrastructure information |
| Tokens | Manage Tokens and local authorizations |
| Commands | Start a charge remotely, Send commands during charge |
| Sessions | Exchange information during charge |
| CDRs | Send the final charge report |
| Tariffs | Exchange Tariffs information |

## 2.3 GIREVE management of Locations data

GIREVE and its systems distinguish two natures of Location properties:

-   Static data: Locations properties which never or almost never change.
-   Dynamic data: Locations properties which can change frequently (e.g., EVSE status, Connector tariff_id)

All Location properties are considered as static data except for the status of the EVSE and the tariff_id attached to a connector.

GIREVE performs a specific process to first integrate static data of CPO Locations in its charge point repository then to integrate static data change like change on a Locations of a CPO or new Locations or EVSEs. This process implies data quality tests and data completion of the CPO Locations.

This process is asynchronous from the standard connection of the CPO with the GIREVE IOP platform, meaning that new Locations of the CPO or updates on them can be seen in the GIREVE charge point repository several days after the first PUSH from the CPO to the GIREVE IOP platform.

## 2.4 Roaming

### 2.4.1 General workflow

An OCPI Roaming session through IOP should run the following workflow

#### Authorisation then initialise Session object

![](media/8fec8b5e0a4654acb2cf7098f0cafae6.emf)

#### Session status information exchange, stop then Charge Detail Record

![](media/d80d615cc2bdd8510bc784e0aa76b96e.emf)

### 2.4.2 New attribute « authorization_id »

**IOP uses a new attribute « authorization_id » for Session, CDR, AuthorizationInfo and StartSession objects.**

This information is used to associate the « authorization » given by the eMSP at the beginning of a charging session with Sessions and CDR related to. It answers to real business cases, for example an eMSP giving 2 authorisations and receiving 3 CDRs. Which ones should it pay?

This new property must be provided by the eMSP during the authorisation process, then used by the CPO when it sends Sessions and CDRs related to the authorisation.

**This property is implemented in OCPI 2.2 under the name “authorization_reference”.**

#### “authorization_id” in local authorization

![](media/313ffe2ad92b9c64f4ff1a2a365188b2.png)

#### “authorization_id” in remote authorization

![](media/2dc2a3d3c73e0a9694525bffce6e85d2.png)

#### “authorization_id” in Sessions

![](media/80bc04d9c9d46e4a6af5d03315d9eb85.png)

#### “authorization_id” in CDRs

![](media/913628d91d26f97470fd2090ea6778f1.png)

### 2.4.3 Management of B2B tariffs

The CPO connected to GIREVE through OCPI have two options to manage and “publish” B2B tariffs:

**B2B tariffs are described in roaming agreements** : The description and the commitment related to tariffs are contained in roaming agreements signed on the GIREVE connect place:

-   Tariffs applied for a contract between an eMSP and a CPO are defined and negotiated, using the GIREVE connect place, before signature of the roaming agreement by both parts.
-   In case of tariff updates, the CPO and the eMSP must sign an amendment whose management is fully automated.

In this case (tariffs defined in roaming agreements), the CPO should not use the OCPI Tariffs module.

**B2B tariffs are not described in roaming agreements**: Tariffs are not described in roaming agreements and the CPO transfers them through the OCPI Tariffs module.

-   In this case, the CPO manages its tariffs on its own and eMSPs cannot give their insights or validation about applied tariffs.
-   Moreover, the CPO is not able to differentiate its tariffs according to eMSPs. In other words, the tariff is unique whatever the eMSP.
-   In case of tariff updates, the CPO and the eMSP do not need to sign an amendment.

### 2.4.4 RFID Tokens

The current typical situation for identification is swiping a MIFARE badge. In this case, the relevant RFID tag in such a situation is a character string that shall contain the hexadecimal representation of the 4- or 7-bytes RFID UID (sector 0). Please note that the 7 bytes UID is preferred for interoperability reason.

As an example: “1A2B3C4D5E6F70” shall be interpreted as

-   Lowest address byte contains “1A” and “1” is the most significant nibble (half byte) and “A” is the least significant nibble.
-   Highest address byte contains “70”
-   Equivalent decimal value is “7365887390543728”

The RFID is not case sensitive. We recommend using uppercase characters. Leading zeros must be provided to reach 8 characters in case of 4 bytes UID or 14 characters in case of 7 bytes UID.

### 2.4.5 Custom OCPI flow to prevent eMSP Tokens download by CPOs

In OCPI 2.1.1, CPOs must send the “auth_id” of a Token in Sessions and CDRs flows.

The only way for CPOs to get this information is to download all Tokens of all eMSPs it is in contract with. CPOs must therefore manage millions of Tokens in their backends, sometime duplicated in case of a backend managing multiple CPOs (a single Token can be downloaded through each CPO connection).

To prevent this complexity and make life easier for CPOs, IOP provides a new OCPI flow enabling a CPO to get the full description of a Token, including the “auth_id” information, by requesting IOP with the “Token’s uid”.

Using this new flow, the CPO can decide to:

-   Send an authorization request to IOP without knowledge of the Token. If the authorization is accepted the CPO gets the Token, including the “auth_id”, with the new flow then send Sessions and CDRs.  
      
     ![](media/fc3cb631c1aa4601d08bcc9343dd638f.png)
-   Or request this new flow to get the Token description and the confirmation that this Token is owned by an eMSP behind GIREVE, then send the authorization request to GIREVE.  
      
     ![](media/ff39e3d06b58f7858f75a6e827140571.png)

This new flow can also be used as a fallback for CPOs which download all Tokens and have a local authorization for an unknown Token.

Description of this new flow in chapter 3.7.3 PULL Tokens by uid: Retrieve a unique Token.
