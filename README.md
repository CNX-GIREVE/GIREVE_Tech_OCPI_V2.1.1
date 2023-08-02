# GIREVE OCPI V2.1.1
IOP – OCPI 2.1.1 Interface - GIREVE Implementation Guide

For more informations, check the standard OCPI specifications : [OCPI 2.1.1](https://github.com/ocpi/ocpi/tree/release-2.1.1-bugfixes#contents).

A REVOIR : Tips before starting your implementation : [Integration Guidelines](integration_guidelines.md).

# Contents
- [CPO Specific implementation guidlines](cpo_edits.md).
- [eMSP Specific implementation guidlines](emsp_edits.md).

## [1 Introduction](introduction.md)
* 1.1 Aims
* 1.2 Intended Audience
* 1.3 Definitions and Abbreviations
* 1.4 Cardinality expression
  
## [2 Integration Guidelines](integration_guidelines.md)
* 2.1 Technical
  - 2.1.1 Supported OCPI versions
  - 2.1.2 Security
  - 2.1.3 Client owned object push
  - 2.1.4 Pagination
  - 2.1.5 IOP HTTP headers
* 2.2 OCPI modules implemented by IOP
* 2.3 GIREVE management of Locations data
* 2.4 Roaming
  - 2.4.1 General workflow
  - 2.4.2 New attribute « authorization_id »
  - 2.4.3 Management of B2B tariffs
  - 2.4.4 RFID Tokens
  - 2.4.5 Custom OCPI flow to prevent eMSP Tokens download by CPOs

## [3 CPO Specfic Implementation Guidelines](cpo_edits.md)
* 3.1 [CPO Operation Definition And Naming Rules](#31-cpo-operation-definition-and-naming-rules)
* 3.2 [CPO Operation And Roaming Offers](#32cpo-operation-and-roaming-offers)
* 3.3 Use Cases Covered By IOP
  - 3.3.1 Technical Use cases
  - 3.3.2 EVCI data
  - 3.3.3 Roaming
* 3.4 Use Cases Required By GIREVE
  - 3.4.1 Always required
  - 3.4.2 If the CPO implements the “Roaming” feature
  - 3.4.3 If the CPO doesn’t commit and describe its tariffs in a roaming agreement
* 3.5 [Connection & Register Specifications](cpo_registration.md)
  - 3.5.1 Update Credentials FromIOP
* 3.6 [Locations Module Specifications](cpo_locations.md)
  - 3.6.1 Static and dynamic attributes
  - 3.6.2 EVSE object
  - 3.6.3 Tariff_id value
  - 3.6.4 PUSH Locations ToIOP
  - 3.6.5 Store and Forward – PUT and PATCH Locations
  - 3.6.6 PULL Locations ToIOP
  - 3.6.7 PULL Locations FromIOP
* 3.7 [Tokens Module Specifications](cpo_tokens.md)
  - 3.7.1 GET Tokens To/FromIOP
  - 3.7.2 PULL Tokens: Who is the eMSP?
  - 3.7.3 PULL Tokens by uid: Retrieve a unique Token
  - 3.7.4 PULL Tokens: Retrieve Tokens of a single given eMSP
  - 3.7.5 POST Authorize request: LocationReferences mandatory
  - 3.7.6 POST Authorize request: new attribute “authorization_id”
* 3.8 [Commands Module specifications](cpo_commands.md)
  - 3.8.1 StartSession request: new attribute « authorization_id »
  - 3.8.2 ReserveNow command
  - 3.8.3 UnlockConnector command
* 3.9 [Sessions Module Specification](cpo_sessions.md)
  - 3.9.1 Session Initialisation
  - 3.9.2 Usage of “authorization_id”
  - 3.9.3 Store and forward – PUT Sessions
* 3.10 [Cdrs Module Specification](cpo_cdrs.md)
  - 3.10.1 CDR sending frequency
  - 3.10.2 Usage of “authorization_id”
  - 3.10.3 CDR content
  - 3.10.4 Store and forward – POST CDRs
  - 3.10.5 Send the signed data (Calibration Law / Eichrecht)
* 3.11 [Tariffs Module Specification](cpo_tariffs.md)
  - 3.11.1 Tariffs flows implemented by GIREVE
  - 3.11.2 Specific properties added by GIREVE
  - 3.11.3 Store and forward – PUT TARIFFS


## [4 eMSP Specfic Implementation Guidelines](emsp_edits.md)
* 4.1 Use Cases Required By IOP
  - 4.1.1 Technical Use cases
  - 4.1.2 EVCI data
  - 4.1.3 Roaming
* 4.2 Use Cases Required By GIREVE
  - 4.2.1 Always required
  - 4.2.2 If the eMSP implements the “Locations static data download” feature
  - 4.2.3 If the eMSP implements the “Locations dynamic data download” feature
  - 4.2.4 If the eMSP implements the “Roaming” feature
* 4.3 Connection & Register Specifications
  - 4.3.1 Update Credentials FromIOP
* 4.4 [Locations Module Specification](emsp_locations.md)
  - 4.4.1 Static and dynamic attributes
  - 4.4.2 PULL Locations ToIOP: Who is the CPO?
  - 4.4.3 PULL Tokens: Retrieve Locations of a single given CPO
  - 4.4.4 PULL Locations ToIOP: Get Object
  - 4.4.5 PULL Locations ToIOP: Get List Pagination
  - 4.4.6 PULL Locations ToIOP: Get List, Full and Delta modes
  - 4.4.7 PULL Locations ToIOP: evse_id
  - 4.4.8 PUSH Locations FromIOP
  - 4.4.9 PULL Locations FromIOP
* 4.5 [Tokens Module Specification](emsp_tokens.md)
  - 4.5.1 Share Tokens list
  - 4.5.2 PULL Tokens FromIOP: Pagination and periodicity
  - 4.5.3 POST Authorize request: new attribute « authorization_id »
* 4.6 [Commands Module Specification](emsp_commands.md)
  - 4.6.1 StartSession request: new attribute « authorization_id »
  - 4.6.2 ReserveNow command
  - 4.6.3 UnlockConnector command
  - 4.6.4 “evse_uid” mandatory in StartSession command
* 4.7 [Sessions Module Specification](emsp_sessions.md)
  - 4.7.1 Usage of “authorization_id”
* 4.8 [Cdrs Module Specification](emsp_cdrs.md)
  - 4.8.1 Usage of “authorization_id”
  - 4.8.2 CDR content
  - 4.8.3 Add billing information in “Remark” field
  - 4.8.4 Get the signed data (Calibration Law / Eichrecht)
* 4.9 [Tariffs Module Specification](emsp_tariffs.md)
  - 4.9.1 Tariffs flows implemented by GIREVE
  - 4.9.2 Specific properties added by GIREVE



