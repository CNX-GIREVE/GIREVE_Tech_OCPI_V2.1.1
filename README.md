# GIREVE OCPI V2.1.1
IOP – OCPI 2.1.1 Interface - GIREVE Implementation Guide

For more informations, check the standard OCPI specifications : [OCPI 2.1.1](https://github.com/ocpi/ocpi/tree/release-2.1.1-bugfixes#contents).

- [Gireve_Implementation_Guide.pdf](https://www.gireve.com/wp-content/uploads/2023/03/Gireve_Tech_OCPI-V2.1.1_ImplementationGuide_1.2.1_en.pdf)

- Informations before starting your implementation : [Integration Guidelines](integration_guidelines.md).
- [Q&A](q&a.md)

# Contents
- [Version History](version_history.md)
- [CPO Specific implementation guidlines](cpo_edits.md).
- [eMSP Specific implementation guidlines](emsp_edits.md).

## [Introduction](introduction.md)
*  [Aims](introduction.md#aims)
*  [Intended Audience](introduction.md#intended-audience)
*  [Definitions and Abbreviations](introduction.md#definitions-and-abbreviations)
*  [Cardinality expression](introduction.md#cardinality-expression)
  
## [Integration Guidelines](integration_guidelines.md)
* [Technical](integration_guidelines.md#technical)
  - Supported OCPI versions
  - Security
  - Client owned object push
  - Pagination
  - IOP HTTP headers
* [OCPI modules implemented by IOP](integration_guidelines.md#ocpi-modules-implemented-by-iop)
* [GIREVE management of Locations data](integration_guidelines.md#gireve-management-of-locations-data)
* [Roaming](integration_guidelines.md#roaming)
  - General workflow
  - New attribute « authorization_id »
  - Management of B2B tariffs
  - RFID Tokens
  - Custom OCPI flow to prevent eMSP Tokens download by CPOs

## [CPO Specfic Implementation Guidelines](cpo_edits.md)
* [CPO Operation Definition And Naming Rules](cpo_registration.md/#cpo-operation-definition-and-naming-rules)
* [CPO Operation And Roaming Offers](cpo_registration.md/#cpo-operation-and-roaming-offers)
* [Connection & Register Specifications](cpo_registration.md)
  - Update Credentials FromIOP
* [Locations Module Specifications](cpo_locations.md)
  - Static and dynamic attributes
  - EVSE object
  - Tariff_id value
  - PUSH Locations ToIOP
  - Store and Forward – PUT and PATCH Locations
  - PULL Locations ToIOP
  - PULL Locations FromIOP
* [Tokens Module Specifications](cpo_tokens.md)
  - GET Tokens To/FromIOP
  - PULL Tokens: Who is the eMSP?
  - PULL Tokens by uid: Retrieve a unique Token
  - PULL Tokens: Retrieve Tokens of a single given eMSP
  - POST Authorize request: LocationReferences mandatory
  - POST Authorize request: new attribute “authorization_id”
* [Commands Module specifications](cpo_commands.md)
  - StartSession request: new attribute « authorization_id »
  - ReserveNow command
  - UnlockConnector command
* [Sessions Module Specification](cpo_sessions.md)
  - Session Initialisation
  - Usage of “authorization_id”
  - Store and forward – PUT Sessions
* [Cdrs Module Specification](cpo_cdrs.md)
  - CDR sending frequency
  - Usage of “authorization_id”
  - CDR content
  - Store and forward – POST CDRs
  - Send the signed data (Calibration Law / Eichrecht)
* [Tariffs Module Specification](cpo_tariffs.md)
  - Tariffs flows implemented by GIREVE
  - Specific properties added by GIREVE
  - Store and forward – PUT TARIFFS


## [eMSP Specfic Implementation Guidelines](emsp_edits.md)
* [Connection & Register Specifications](emsp_registration.md)
  - Update Credentials FromIOP
* [Locations Module Specification](emsp_locations.md)
  - Static and dynamic attributes
  - PULL Locations ToIOP: Who is the CPO?
  - PULL Tokens: Retrieve Locations of a single given CPO
  - PULL Locations ToIOP: Get Object
  - PULL Locations ToIOP: Get List Pagination
  - PULL Locations ToIOP: Get List, Full and Delta modes
  - PULL Locations ToIOP: evse_id
  - PUSH Locations FromIOP
  - PULL Locations FromIOP
* [Tokens Module Specification](emsp_tokens.md)
  - Share Tokens list
  - PULL Tokens FromIOP: Pagination and periodicity
  - POST Authorize request: new attribute « authorization_id »
* [Commands Module Specification](emsp_commands.md)
  - StartSession request: new attribute « authorization_id »
  - ReserveNow command
  - UnlockConnector command
  - “evse_uid” mandatory in StartSession command
* [Sessions Module Specification](emsp_sessions.md)
  - Usage of “authorization_id”
* [Cdrs Module Specification](emsp_cdrs.md)
  - Usage of “authorization_id”
  - CDR content
  - Add billing information in “Remark” field
  - Get the signed data (Calibration Law / Eichrecht)
* [Tariffs Module Specification](emsp_tariffs.md)
  - Tariffs flows implemented by GIREVE
  - Specific properties added by GIREVE



