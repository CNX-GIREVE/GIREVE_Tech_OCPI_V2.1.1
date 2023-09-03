# Contents

* [How to test pagination in preprodution environment](#how-to-test-pagination-in-preproduction-environment)
* [Tokens : Difference between Full and Delta PULL](#tokens-difference-between-full-and-delta-pull)
 
*** 


## How to test pagination in preproduction environment

Gireve's preproduction environment has **<ins>defined limit</ins>** when retrieving your data.

To test the pagination of various modules, your system must contain a minimum number of elements : 

- **Location Module :** 21 locations
- **CDRs Module :** 21 Cdrs
- **Tariffs Module :** 101 tariffs

> :warning: **<ins>If you cannot integrate the recommended minimum number into your system, you must be able to modify your default PUSH limit.
IOP will consider this new limit.</ins>**


## Tokens Difference between Full and Delta PULL

As a CPO, you can retrieve tokens from eMSPs with whom you have a contract.
To do this, you have two ways :

- **Full** : 1 time per month.
- **Delta** : 1 time per day. 

> :bulb: **<ins>Depending on your contract with Gireve, these periodicities can change. For more information, please contact your acccount representative.</ins>**
