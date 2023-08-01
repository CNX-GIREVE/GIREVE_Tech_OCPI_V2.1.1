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

## [3 CPO Specfic Implementation Guidelines](#_Toc122429877)
* [3.1 CPO Operation Definition And Naming Rules](#_Toc122429878)
* [3.2 CPO Operation And Roaming Offers](#_Toc122429879)
* [3.3 Use Cases Covered By IOP](#_Toc122429880)
  - [3.3.1 Technical Use cases](#_Toc122429881)
  - [3.3.2 EVCI data](#_Toc122429882)
  - [3.3.3 Roaming](#_Toc122429883)
* [3.4 Use Cases Required By GIREVE](#_Toc122429884)
  - [3.4.1 Always required](#_Toc122429885)
  - [3.4.2 If the CPO implements the “Roaming” feature](#_Toc122429886)
  - [3.4.3 If the CPO doesn’t commit and describe its tariffs in a roaming agreement](#_Toc122429887)
* [3.5 Connection & Register Specifications](#_Toc122429888)
  - [3.5.1 Update Credentials FromIOP](#_Toc122429889)
* [3.6 Locations Module Specifications](#_Toc122429890)
  - [3.6.1 Static and dynamic attributes](#_Toc122429891)
  - [3.6.2 EVSE object](#_Toc122429892)
  - [3.6.3 Tariff_id value](#_Toc122429893)
  - [3.6.4 PUSH Locations ToIOP](#_Toc122429894)
  - [3.6.5 Store and Forward – PUT and PATCH Locations](#_Toc122429895)
  - [3.6.6 PULL Locations ToIOP](#_Toc122429896)
  - [3.6.7 PULL Locations FromIOP](#_Toc122429897)
* [3.7 Tokens Module Specifications](#_Toc122429898)
  - [3.7.1 GET Tokens To/FromIOP](#_Toc122429899)
  - [3.7.2 PULL Tokens: Who is the eMSP?](#_Toc122429900)
  - [3.7.3 PULL Tokens by uid: Retrieve a unique Token](#_Toc122429901)
  - [3.7.4 PULL Tokens: Retrieve Tokens of a single given eMSP](#_Toc122429902)
  - [3.7.5 POST Authorize request: LocationReferences mandatory](#_Toc122429903)
  - [3.7.6 POST Authorize request: new attribute “authorization_id”](#_Toc122429904)
* [3.8 Commands Module specifications](#_Toc122429905)
  - [3.8.1 StartSession request: new attribute « authorization_id »](#_Toc122429906)
  - [3.8.2 ReserveNow command](#_Toc122429907)
  - [3.8.3 UnlockConnector command](#_Toc122429908)
* [3.9 Sessions Module Specification](#_Toc122429909)
  - [3.9.1 Session Initialisation](#_Toc122429910)
  - [3.9.2 Usage of “authorization_id”](#_Toc122429911)
  - [3.9.3 Store and forward – PUT Sessions](#_Toc122429912)
* [3.10 Cdrs Module Specification](#_Toc122429913)
  - [3.10.1 CDR sending frequency](#_Toc122429914)
  - [3.10.2 Usage of “authorization_id”](#_Toc122429915)
  - [3.10.3 CDR content](#_Toc122429916)
  - [3.10.4 Store and forward – POST CDRs](#_Toc122429917)
  - [3.10.5 Send the signed data (Calibration Law / Eichrecht)](#_Toc122429918)
* [3.11 Tariffs Module Specification](#_Toc122429919)
  - [3.11.1 Tariffs flows implemented by GIREVE](#_Toc122429920)
  - [3.11.2 Specific properties added by GIREVE](#_Toc122429921)
  - [3.11.3 Store and forward – PUT TARIFFS](#_Toc122429922)

## [4 eMSP Specfic Implementation Guidelines](#_Toc122429923)
* [4.1 Use Cases Required By IOP](#_Toc122429924)
  - [4.1.1 Technical Use cases](#_Toc122429925)
  - [4.1.2 EVCI data](#_Toc122429926)
  - [4.1.3 Roaming](#_Toc122429927)
* [4.2 Use Cases Required By GIREVE](#_Toc122429928)
  - [4.2.1 Always required](#_Toc122429929)
  - [4.2.2 If the eMSP implements the “Locations static data download” feature](#_Toc122429930)
  - [4.2.3 If the eMSP implements the “Locations dynamic data download” feature](#_Toc122429931)
  - [4.2.4 If the eMSP implements the “Roaming” feature](#_Toc122429932)
* [4.3 Connection & Register Specifications](#_Toc122429933)
  - [4.3.1 Update Credentials FromIOP](#_Toc122429934)
* [4.4 Locations Module Specification](#_Toc122429935)
  - [4.4.1 Static and dynamic attributes](#_Toc122429936)
  - [4.4.2 PULL Locations ToIOP: Who is the CPO?](#_Toc122429937)
  - [4.4.3 PULL Tokens: Retrieve Locations of a single given CPO](#_Toc122429938)
  - [4.4.4 PULL Locations ToIOP: Get Object](#_Toc122429939)
  - [4.4.5 PULL Locations ToIOP: Get List Pagination](#_Toc122429940)
  - [4.4.6 PULL Locations ToIOP: Get List, Full and Delta modes](#_Toc122429941)
  - [4.4.7 PULL Locations ToIOP: evse_id](#_Toc122429942)
  - [4.4.8 PUSH Locations FromIOP](#_Toc122429943)
  - [4.4.9 PULL Locations FromIOP](#_Toc122429944)
* [4.5 Tokens Module Specification](#_Toc122429945)
  - [4.5.1 Share Tokens list](#_Toc122429946)
  - [4.5.2 PULL Tokens FromIOP: Pagination and periodicity](#_Toc122429947)
  - [4.5.3 POST Authorize request: new attribute « authorization_id »](#_Toc122429948)
* [4.6 Commands Module Specification](#_Toc122429949)
  - [4.6.1 StartSession request: new attribute « authorization_id »](#_Toc122429950)
  - [4.6.2 ReserveNow command](#_Toc122429951)
  - [4.6.3 UnlockConnector command](#_Toc122429952)
  - [4.6.4 “evse_uid” mandatory in StartSession command](#_Toc122429953)
* [4.7 Sessions Module Specification](#_Toc122429954)
  - [4.7.1 Usage of “authorization_id”](#_Toc122429955)
* [4.8 Cdrs Module Specification](#_Toc122429956)
  - [4.8.1 Usage of “authorization_id”](#_Toc122429957)
  - [4.8.2 CDR content](#_Toc122429958)
  - [4.8.3 Add billing information in “Remark” field](#_Toc122429959)
  - [4.8.4 Get the signed data (Calibration Law / Eichrecht)](#_Toc122429960)
* [4.9 Tariffs Module Specification](#_Toc122429961)
  - [4.9.1 Tariffs flows implemented by GIREVE](#_Toc122429962)
  - [4.9.2 Specific properties added by GIREVE](#_Toc122429963)



