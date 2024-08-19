---
permalink: /account-and-license/
title: "Account and License"
---

Since SIRIUS 5, a user account and license are required to use the webservice-based
features of SIRIUS.

**Non-webservice-based features** include molecular formula annotation using fragmentation trees and isotope pattern analysis. Fragmentation tree computation and isotope pattern analysis are
performed on your local computer and do not require license. Some molecular formula annotation strategies (like formula database search)
may require a web service connection.

**Webservice-based features** are essentially advanced structure elucidation features including structure database search 
with CSI:FingerID, compound class prediction with CANOPUS and *de novo* structure generation with MSNovelist.  
No worries, these and future features **will remain free for academic use**
and will be provided/hosted by the FSU Jena. [Bright Giant GmbH](https://bright-giant.com/) 
offers SIRIUS web service hosting for commercial users. 

### Academic users {#academic-users}
Access to the academic license is automatically granted based on the domain of your 
**institutional email address**. You must use your institutional email address for your SIRIUS account
to benefit from free academic licenses.

The FSU Jena maintains an allow list of academic/non-profit intuitions/organizations. However, such a 
list will never be complete. If your institution does not have access but you think it should, please send us an
[email](mailto:sirius@uni-jena.de) with information about your institution that will allow us to assess the academic/non-profit status of your institution (e.g. official website). 
We will perform a manual validation and grant your institution access if it meets the academic license requirements. 

### Non-academic users {#non-academic-users}
Please contact [Bright Giant](https://bright-giant.com/) by [email](mailto:info@bright-giant.com) for quotes and pricing.

### Account creation and login {#account-creation}
User accounts can be created directly from the [SIRIUS GUI]({{ "/gui/#account-creation" | relative_url}}) or from the [user portal](https://portal.bright-giant.com/). After logging into the user portal, you can request a license.

To login from the CLI, use the following command:
```
sirius login -u <email> -p
```
See `sirius login --help` for details.

**NOTE:** When logging in, SIRIUS retrieves a long-lived `refresh_token` that will be stored until it is invalidated 
by logging out. Your username and password are **never** stored locally.
