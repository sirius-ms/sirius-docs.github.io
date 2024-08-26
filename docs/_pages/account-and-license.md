---
permalink: /account-and-license/
title: "Account and License"
---

Since SIRIUS 5, a license is required to use the webservice-based
features of SIRIUS.

**Non-webservice-based features** include molecular formula annotation using fragmentation trees and isotope pattern analysis. 
Fragmentation tree computation and isotope pattern analysis are performed on your local computer and do not require license.
Be aware that [molecular formula annotation using database search]({{ "/methods-background/#formula-db-search" | relative_url}}) requires a web service connection.

**Webservice-based features** include all advanced structure elucidation features, such as [structure database search 
with CSI:FingerID]({{ "/methods-background/#molecular-fingerprint" | relative_url}}), [compound class prediction with CANOPUS]({{ "/methods-background/#CANOPUS" | relative_url}}) and [*de novo* structure generation with MSNovelist]({{ "/methods-background/#MSNovelist" | relative_url}}).
These and future features **will remain [free for academic use](#academic-users)** 
provided/hosted by the FSU Jena. [Bright Giant GmbH](https://bright-giant.com/) offers SIRIUS web service hosting for [non-academic users](#non-academic-users). 

### Account creation and login {#account-creation}
User accounts can be created directly from the [user portal](https://portal.bright-giant.com/). Enter your name and your **institutional** (not personal) email address and choose a password for your account. Verify your email address with the link sent to your inbox. 

<img src="{{ "/assets/images/user-portal.png" | relative_url }}" alt="Create new account in the user portal." width="300">

After logging into the user portal, you can [request a license]({{"/account-and-license/" | relative_url}}).

In the [SIRIUS GUI]({{ "/gui/#account-creation" | relative_url}}), open `Account` in the top-right toolbar and click `Log in` to enter your account credentials and login.

To login from the CLI, use the following command:
```
sirius login -u <email> -p
```
See `sirius login --help` for details.

**NOTE:** When logging in, SIRIUS retrieves a long-lived `refresh_token` that will be stored until it is invalidated 
by logging out. Your username and password are **never** stored locally. Read more about [Security and Data Usage]({{"/admins/#security" | relative_url}}).

### Academic users {#academic-users}
Access to the academic license is automatically granted based on the domain of your 
**institutional email address**. You must use your institutional email address for your SIRIUS account
to benefit from free academic licenses.

The FSU Jena maintains an allow list of academic/non-profit intuitions/organizations. However, such a 
list will never be complete. If your institution does not have access but you think it should, request an academic license <span style="color:red">[1]</span> in the user portal, providing information to identify your institution, including the official email domain, website and a short description what kind of institution you are requesting an academic license for (publishing and educational activities) <span style="color:red">[2]</span>.
We will perform a manual validation and grant your institution access if it meets the academic license requirements. 

<img src="{{ "/assets/images/request-license.png" | relative_url }}" alt="Create new account in the user portal." width="500">

### Non-academic users {#non-academic-users}
Please contact [Bright Giant](https://bright-giant.com/) by [email](mailto:info@bright-giant.com) for quotes and pricing.

