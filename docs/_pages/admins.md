---
permalink: /admins/
title: Administrator/Technical documentation
---

SIRIUS is a software framework for MS/MS data analysis of small molecules. 
It offers a comprehensive suite of [tools and methods]({{"/methods-background/" | relative_url }}).
For more details on the 
included tools and methods please have a look into the [user documentation]({{ "/" | relative_url }}).
SIRIUS is written in JAVA, but **no** separate JRE is required. A compatible JRE is  included in the SIRIUS release.
For more information on installation and hardware requirements, please refer to the [Installation]({{ "/install/" | relative_url }}) section.

## Offline vs Online features {#offline-online}
SIRIUS is both, a desktop application, performing computations locally, 
and a client for the _SIRIUS Web Services_ such as 
[CSI:FingerID]({{ "/cli/#CSIFingerID-structures" | relative_url }}) and 
[CANOPUS]({{ "/cli/#CANOPUS-classes" | relative_url }}).
The web services are seamlessly integrated to provide a user experience akin to classic desktop (or command-line) applications.

{% capture fig_img %}
![Foo]({{ "/assets/images/sirius-infrastructure.svg" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Basic Architecture Diagram of SIRIUS and its web services.</figcaption>
</figure>

#### Why are some features implemented as web service? {#web-service}
The structure and compound class predictions rely on complex infrastructure that is not suitable for execution on laptops or workstation computers. 
Therefore, these tasks are handled by scalable cloud infrastructure.
In our case, the web service is about computations in the cloud and not about storing 
data in the cloud. Moreover, this approach allows for continuous improvements and bug fixes without requiring users to install updates.

### Offline features - SIRIUS (Open Source) {#offline}
* [Data import]({{"/gui/#data-import" | relative_url }}); [result export]({{"/gui/#data-export" | relative_url}}) and storage. 
* [Result visualization via the GUI]({{"/gui/#visualize-results" | relative_url}}), even for web service-based results, which are available offline once stored
* [SIRIUS molecular formula identification]({{"/methods-background/#SIRIUS-molecular-formula" | relative_url}}) ([fragmentation tree computation]({{"/methods-background/#fragmentation-trees" | relative_url}}) and [isotope pattern analysis]())
* [ZODIAC]({{"/methods-background/#ZODIAC" | relative_url}}) network-based molecular formula re-ranking
* [Passatutto]({{"/cli/#passatutto" | relative_url}}) decoy spectra generation
* Most [standalone helper sub-tools in the CLI]({{"/cli-standalone/" | relative_url}})

### Online features - SIRIUS Web Services (Proprietary) {#online}
* Chemical structure database-based features, such as 
  * CSI:FingerID [structure database search]({{"/methods-background/#CSIFingerID" | relative_url}}) 
  * [restricting formula candidates to a structure database]() during molecular formula identification
* CSI:FingerID [fingerprint prediction]({{"/methods-background/#molecular-fingerprint" | relative_url}})
* [COSMIC]({{"/methods-background/#COSMIC" | relative_url}}) confidence score assessment
* [CANOPUS]({{"/methods-background/#CANOPUS" | relative_url}}) compound class prediction
* [MSNovelist]({{"/methods-background/#MSNovelist" | relative_url}}) de novo structure generation

## Connections for online features {#connections}
To ensure that SIRIUS functions correctly, three main servers need to be accessible (via port 443 or proxy as described in the next section):
- **Login Server:** `https://auth0.bright-giant.com`
- **License Server:** `https://gate.bright-giant.com`
- **Application Server:** The URL for the _SIRIUS Web Services_ varies depending on your [subscription type]({{"/account-and-license" | relative_url }}) and will be provided as part of your access token after logging in. It will be displayed in the connection check panel if a valid license is detected after login.
  - **Academic subscription (FSU Jena):** `https://www.csi-fingerid.uni-jena.de`
  - **Commercial subscriptions (Bright Giant GmbH):** `https://csi.bright-giant.com`   
  - **Dedicated hosting subscription:** `<companyname>.csi.bright-giant.com`
- **Internet connection check (optional):** `https://github.com`

The **login** and **license servers** are the same for no-academic and academic users, with services provided by Bright Giant and FSU Jena, respectively.

Our **login** server sends emails from `noreply@mailgun.bright-giant.com`. In case users do not receive emails from our 
system (e.g. for email verification or password reset) please make sure that the email address is not blocked by your 
institution's spam detection system.

The URL of the **application server** itself may differ based on the type of subscription.
Users with a dedicated hosting subscription need to ensure that their custom web service URL is accessible from the system where SIRIUS is installed.

If SIRIUS cannot connect to its servers, it automatically runs a series of diagnostic tests to identify the issue. One of these tests involves pinging a website to check if SIRIUS can access the internet (**Internet connection check**) or if it is being blocked by a firewall or incorrect proxy settings. 
This test is *optional* and not required for SIRIUS to function properly. It is designed to provide more useful information in the event of a connection error. Blocking it may result in less detailed error messages if connection issues arise. 

We use GitHub by default because it is a reliable website that is almost never down und reachable from most regions of the world. 
If you are using SIRIUS in a location where GitHub is inaccessible, you can replace the GitHub URL with another reliable URL outside your institutional network to check the internet connection. To do this, add or replace the following entry in `<USER_HOME>/.sirius-<X.Y>/sirius.properties`:

```
de.unijena.bioinf.fingerid.web.external=https://my.custom.url/
```
The connection check expects an HTTP head response value `OK` (e.g. 200) to be successful.

## Proxy Settings {#proxy}
If your institution uses a proxy server to connect to the internet you need to configure the proxy settings within SIRIUS. 
See [Network Settings]({{ "/gui/#settings" | relative_url }}) for details.

## Security and Data Usage {#security}

#### Data in transit {#data-transit}
All communication between SIRIUS on your local computer and its web services is secured with HTTPS and TLS encryption.

#### Data at rest {#data-rest}
**No data is stored at rest.** Data sent to our servers is only stored temporarily for computation and is deleted immediately after the results have been fetched by your local SIRIUS client. Typically, data remains on our servers for just a few minutes until the transfer is complete.
  
**Special case:** If you have a subscription with a compound limit (e.g. Bright Giant Shared subscription),
a hash of the input spectra is stored for approximately 24 hours.
This allows us to track the number of computations correctly. 
This hash can **never** be used to reconstruct the spectrum; it simply veirfies
if a spectrum has already been processed in a previous job. 
For subscriptions without a compound limit 
(e.g. FSU Jena version or Bright Giant Private subscription), this hash is not stored.

#### What data is transferred to the Web Services? {#transferring-data}
For each instance (compound) to be  analyzed, SIRIUS transmits the mass spectra, 
the locally computed fragmentation tree, the parameters for the computation, 
and a unique session key to the web services. 
The only personal data transmitted is your account information (user ID and email address), which is securely sent as a JSON Web Token (JWT).

#### How is it ensured that only I can fetch the results of my computation jobs? {#fetching-results}
Every time you start SIRIUS on your local computer, it generates a unique session key used to submit computation jobs to the server. 
This key is never stored and exists only in system memory during the session. 
Only the SIRIUS process with this key can retrieve the results. 
Once the results are fetched, they are immediately deleted from the server. 
If a client with a specific session key loses its connection to the server for more than a few hours, all associated jobs are permanently deleted.

#### What kind of statistics are collected? {#statistics}
We only track the number, duration, and type of computation jobs to ensure our infrastructure scales appropriately.


## SIRIUS workspace - Configs, Logs and Caches {#SIRIUS-workspace}
SIRIUS stores its configuration files, logs and caches in the SIRIUS workspace directory at `<USER_HOME>/.sirius-<X.Y>`, where `X` and `Y` are the **major** and **minor** version numbers of SIRIUS. 
When using SIRIUS in virtual machines (VMs) or containers, it is advisable to persist this directory to retain configurations and benefit from performance improvements due to structure database caching. 
In versions **prior to 5.7**, this directory also served as the default location for custom databases. 
Starting with version 5.7, custom databases can be stored in any directory on the system.

**Directory structure:**
* **csi_fingerid_cache**
  * **rest-cache** - default location for structure database cache (configurable)
  * **custom** - default location for custom databases (configurable since v5.7)
  * **version** - database version file
* **custom.config** - configuration file for overriding default parameters in CLI and GUI 
  (see, [config tool]({{"/cli/" | relative_url }}))
* **logging.properties** - SIRIUS logging configuration file
* **sirius.properties** - SIRIUS configuration file
* **version** - SIRIUS version that created this content
* **sirius.log.0** - log file
* **sirius.log.1** - log file
* **sirius.log.2** - log file
* ...


## Release Policy {#realese-policy}
SIRIUS and its backend (web service) follow a “rolling release” strategy with semantic versioning. Updates and bug fixes are deployed as soon as they are stable, sometimes even for a single bug fix. 

## Versioning {#versioning}
Following the Apache Maven convention, we distinguish between *stable* (`x.y.z`) and *SNAPSHOT* (`x.y.z-SNAPSHOT`) builds.
The `x.y.z-SNAPSHOT` build can be considered the development version until `x.y.z` is released, so `x.y.z.SNAPSHOT` < `x.y.z`.
Development (`x.y.z-SNAPSHOT`) builds are declared as pre-release if they are publicly available.

### What do SIRIUS version numbers mean? {#version-numbers}
**`x`:** Major changes in the application:
* CLI commands may be changed or removed.
* Input formats  may be altered in a non-backward-compatible manner.
* Output formats may change, potentially disrupting existing workflows.
* May not be backward compatible with results from previous versions.

**`y`:** Minor changes or major backend changes:
* Major backend changes (web service) that may cause incompatibility with the client 
(see, [below]({{ "/admins/#major-web-service-changes-and-end-of-life" | relative_url }})).
* Introduction of new tools, methods, or functionalities.
* New CLI commands added (backward compatible).
* Additional inputs or outputs (backward compatible).
* Backward compatible with results from previous versions.
* Updates to the included JRE or native libraries.

**`z`:** Build number that will increase with every new build:
* Bug fixes, typo corrections, etc.
* Minor improvements that do not risk breaking anything (e.g., new or corrected tooltips, additional documentation).

### Upgrade Recommendations {#upgrad-recommendations}
From a *system administrator's* or *system integration* perspective, **minor** and **build** number changes are safe to upgrade and should not cause disruptions.
From a *user's perspective*, a **minor** update might alter (typically improve)
the results of the methods. 
The corresponding entry in the [changelog]({{ "/changelog/" | relative_url }}) will provide the relevant details.

### Multiple versions alongside {#multiple-versions}
* Multiple different **minor** versions can safely be used side by side without interfering with each other.
* Different **build**s of the same **minor** version should not be used side by side as they would share configs, logs and caches, which can lead to unexpected behaviour.
  
### Major web service changes and end-of-life {#web-service-changes}
When a **major** web service update is released, this also causes an update of the SIRIUS client to a new **minor** version (`x.y+1.0`). 
The previous version (`x.y.z`) then becomes a legacy version, which remains fully functional for a limited time (usually at least three months).
Extensions beyond this period may be considered based on user requests.
However, unless we just have a **minor** version change for the SIRIUS client, upgrading is not 
different from other **minor** version changes. 
Just keep in mind that the previous version might reach its end-of-life.
