### [<- Back to module selection](emsp_edits.md)

# Contents 

* [Use Cases Covered By IOP](#use-cases-covered-by-iop)
  - Roaming
* [Use Cases Required By GIREVE](#use-cases-required-by-gireve)
  - If the eMSP implements the “Sessions” feature
* [Tariffs module specifications](#tariffs-module-specifications)
  - Tariffs flows implemented by GIREVE
  - Specific properties added by GIREVE 
* [Exemples](#exemples)
  - ToIOP_PUT_emsp_tariffs_2.1.1

***


## Use cases covered by IOP 

OCPI features are composed by several use cases that an eMSP can choose to implement or not when connecting to an operator. In case of connection to GIREVE, here is the list of use cases that a CPO can implement :


### Roaming

| Use case | ToIOP/FromIOP | Usage |
| ----------- | ----------- | ----------- |
| Push CDRSessions | IOP sends to an eMSP a CDR related to a terminated charging-session. |
| Pull CDR | ToIOP | An eMSP requests IOP to get CDRs that belongs to it. |


## Use cases required by GIREVE

Some of these use cases are required when connecting to GIREVE :

### If the eMSP implements the “Roaming” feature

| Use case |  Why ? | 
| ----------- | ----------- |
| Push CDR/FromIOP OR Pull CDR/ToIOP | Getting CDR is mandatory to enable roaming. | 

## Tariffs module specifications

IOP follows the OCPI standard for Tariffs download by an eMSP. [*See OCPI specifications*](https://github.com/ocpi/ocpi/blob/release-2.1.1-bugfixes/mod_tariffs.md)

### Tariffs flows implemented by GIREVE

IOP only implements the “GET Tariffs” used by an eMSP to retrieve tariffs of CPOs.

### Specific properties added by GIREVE

#### A. Tariffs.country_code and Tariffs.party_id (mandatory)

In the same way as the OCPI 2.2 standard, GIREVE includes in Tariffs objects returned to eMSPs the identification of the owner of the Tariffs (the CPO).

This identification is done by adding two new properties in each tariff :

-   country_code: Country code of the CPO owner of the tariff.
-   party_id: Party id of the CPO owner of the tariff.

#### B. TariffDimensionType “SESSION_TIME”

In standard OCPI V2.1.1, the CPO can define 2 different “TariffDimensionType” related to the duration :

-   **TIME**: “time charging: defined in hours, step_size multiplier: 1 second”   
    *(description from the OCPI Github)*
-   **PARKING_TIME**: “time not charging: defined in hours, step_size multiplier: 1 second”   
    *(description from the OCPI Github)*

GIREVE adds a third “TariffDimensionType” named “SESSION_TIME” with the following description :

-   **SESSION_TIME**: “time charging or not: defined in hours, step_size multiplier: 1 second”.

#### C. Tariffs.restriction (optional)

If the CPO describes its tariffs in the roaming agreement on the GIREVE connect-place, the CPO can define different tariffs depending on the timeslots of the start of the charging session.

As an example:

-   The Tariff A is applicable if the charge begins “between Monday and Friday” **and** on the timeslots “8 AM to 12 AM” or “2 PM to 6 PM”.
-   If the charge starts outside of these time and day slots, the Tariff B is applicable.

If a tariff described on the GIREVE connect-place depends on time and day of the charging session start, GIREVE adds these criteria in the tariff description sent to eMSPs through the OCPI Tariffs module.

As for the property “TariffElement.restrictions”, the “Tariffs.restrictions” contains several properties :

-   **start_time**: Start time of the first timeslot, for example 13:30, valid from this time of the day. Must be in 24h format with leading zeros. Hour/Minute separator: ":" Regex: ([0-1][0-9]\|2[0-3]):[0-5][0-9].
-   **end_time**: End time of the first timeslot, for example 13:30, valid from this time of the day. Must be in 24h format with leading zeros. Hour/Minute separator: ":" Regex: ([0-1][0-9]\|2[0-3]):[0-5][0-9].
-   **start_time_2**: Start time of the second timeslot, for example 13:30, valid from this time of the day. Must be in 24h format with leading zeros. Hour/Minute separator: ":" Regex: ([0-1][0-9]\|2[0-3]):[0-5][0-9].
-   **end_time_2**: End time of the second timeslot, for example 13:30, valid from this time of the day. Must be in 24h format with leading zeros. Hour/Minute separator: ":" Regex: ([0-1][0-9]\|2[0-3]):[0-5][0-9].
-   **day_of_week**: Which day(s) of the week this tariff is valid.

To determine which tariff is applicable for a charging session, the rule is: **“The tariff containing a restriction has priority other tariffs without restrictions”.**

#### D. PriceComponent.price_round (mandatory)

If the CPO describes its tariffs in the roaming agreement on the GIREVE connect-place, the CPO defines the rounding of the “TariffElement”.

This rounding is applied for each “TariffElement” after the calculation of “the energy delivered in the TariffElement” or “the time spent in the TariffElement”.

This rounding has 2 properties :

-   **round_granularity**: Can take values “UNIT”, “TENTH”, “HUNDRETH” or “THOUSANDTH”.
-   **round rule**: Can take values “ROUND_UP”, “ROUND_DOWN” or “ROUND_NEAR”.

For tariffs coming from CPOs through the OCPI Tariffs module, the rounding is not defined and the price properties can contain until 4 decimals so the default rounding applied by GIREVE is “round_granularity: thousandth” and “round_rule: round_near”.

#### E. PriceComponent.step_round (mandatory)

If the CPO describes its tariffs in the roaming agreement on the GIREVE connect-place, the CPO defines the rounding of the number of “step_size”.

This rounding is applied for each “TariffElement” after the calculation of “the energy delivered in the TariffElement” or “the time spent in the TariffElement”.

This rounding has 2 properties :

-   **round_granularity**: single option “UNIT”
-   **round rule**: Can take values “ROUND_UP”, “ROUND_DOWN” or “ROUND_NEAR”.

For tariffs coming from CPOs through the OCPI Tariffs module, the rounding is standard and take values “round_granularity: UNIT” and “round_rule: ROUND_UP”.

#### F. PriceComponent.exact_price_component (mandatory)

When a price_component cannot be exactly mapped from the GIREVE connect-place to the OCPI 2.1.1 format, GIREVE informs the eMSP by adding the property “exact_price_component” taking values :

-   **true**: The price_component is equal to the one declared by the CPO.
-   **false**: The original price_component cannot be mapped exactly with OCPI 2.1.1, an approximation has been made.

This new property will be consistent too for the future mapping between different versions of OCPI.

#### G. Synthesis of the GIREVE specifications

| Tariff | | | |
| ----------- | ----------- | ----------- | ----------- |
| **Property** | **Type** | **Card.** | **Description** |
| country_code | String(2) | 1 | Country code of the CPO owner of the tariff | 
|party_id | String(3) | 1 | Party id of the CPO owner of the tariff |
| restriction | Object containing up to 4 properties: start_time end_time start_time_2 end_time_2 day_of_week | ? | Time and day the tariff is applicable. To determine which tariff is applicable for a charging session, the rule is: **“The tariff containing a restriction has priority other tariffs without restrictions”.** |
| **Tariff.elements.price_components** | | |  |
| type | Add “SESSION_TIME” as a new “TariffDimensionType” | 1 | **TIME**: “time charging: defined in hours, step_size multiplier: 1 second”  *(description from the OCPI Github)* **PARKING_TIME**: “time not charging: defined in hours, step_size multiplier: 1 second”  *(description from the OCPI Github)* **SESSION_TIME**: “time charging or not: defined in hours, step_size multiplier: 1 second” |
| price_round | Object containing 2 properties: round_granularity (« UNIT », « TENTH », « HUNDRETH », « THOUSANDTH »). round_rule (« ROUND_UP », « ROUND_DOWN », « ROUND_NEAR ») | 1 | Rounding of the price applied for each “TariffElement” after the calculation of “the energy delivered in the TariffElement” or “the time spent in the TariffElement”. |
| step_round | Object containing 2 properties: round_granularity (« UNIT »). round_rule (« ROUND_UP », « ROUND_DOWN », « ROUND_NEAR ») | 1 | Rounding of the number of step_size to apply after the calculation of “the energy delivered in the TariffElement” or “the time spent in the TariffElement”. |
| exact_price_component | Boolean | 1  | Boolean informing if the “price_component” description corresponds exactly with the one sent/described by the CPO. |


## Examples

### ToIOP_PUT_emsp_tariffs_2.1.1

*URL*:

`/ocpi/emsp/2.1.1/tariffs/FR/CPO/Tariff-1`

*Request*:

- **VERB: PUT**
- **HEADERS**: `{Authorization:Token xxx-xxx-xxx}{Connection:close}{Accept:application/json}`
- **BODY**:

```json
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
*Response*:

- **CODE**: 200
- **HEADERS**: `{Content-Type:application/json}`
- **BODY**:
```json
{
    "data": {};
    "status_code": 1000,
    "status_message": "Success",
    "timestamp": "2020-12-22T09:50:25Z"
}
```
