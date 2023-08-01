# GIREVE OCPI V2.1.1
IOP – OCPI 2.1.1 Interface - GIREVE Implementation Guide

For more informations, check the standard OCPI specifications : [OCPI 2.1.1](https://github.com/ocpi/ocpi/tree/release-2.1.1-bugfixes#contents).

A REVOIR : Tips before starting your implementation : [Check-up](checkup_edits.md).

# Contents
- [CPO Specific implementation guidlines](cpo_edits.md).
- [eMSP Specific implementation guidlines](emsp_edits.md).

[1 Introduction](#_Toc122429849)
- [1.1 Aims](#_Toc122429850)
- [1.2 Intended Audience](#_Toc122429851)
- [1.3 Definitions and Abbreviations](#_Toc122429852)
- [1.4 Cardinality expression](#_Toc122429853)

[2 Integration Guidelines](#_Toc122429854)
- [2.1 Technical](#_Toc122429855)
- [2.1.1 Supported OCPI versions](#_Toc122429856)
- [2.1.2 Security](#_Toc122429857)
- [2.1.3 Client owned object push](#_Toc122429858)
- [2.1.4 Pagination](#_Toc122429859)
- [2.1.5 IOP HTTP headers](#_Toc122429860)
[2.2 OCPI modules implemented by IOP](#_Toc122429861)
[2.3 GIREVE management of Locations data](#_Toc122429862)
[2.4 Roaming](#_Toc122429871)
- [2.4.1 General workflow](#_Toc122429872)
- [2.4.2 New attribute « authorization_id »](#_Toc122429873)
- [2.4.3 Management of B2B tariffs](#_Toc122429874)
- [2.4.4 RFID Tokens](#_Toc122429875)
- [2.4.5 Custom OCPI flow to prevent eMSP Tokens download by CPOs](#_Toc122429876)


