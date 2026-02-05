---
permalink: /faq/
title: "Frequently asked questions (FAQ)"
---

## Account and License

<details markdown="1">
  <summary><strong>Is my academic institution authorized for a free license?</strong></summary>
[SIRIUS web services are free for academic use]({{ "/account-and-license/" | relative_url }}). **All universities are eligible. For other non-profit research entities, special license conditions may apply.** Academic institutions are identified by their email domain. If you use your institutional email address to [create an account]({{ "/account-and-license/#account-creation" | relative_url }}), access is granted automatically. If you aren't automatically assigned a license, use the "request license" button in the [portal](https://portal.bright-giant.com/).
</details>

<details markdown="1">
  <summary><strong>My institutional email is not recognized. How do I get an academic license?</strong></summary>
[Academic licenses]({{"/account-and-license/#academic-users" | relative_url }}) are automatically granted based on institutional email domains. If your domain is not currently on the "allow-list," **you must contact the support using the "request license" button** in the [portal](https://portal.bright-giant.com/). Note that academic licenses are reserved for universities; other non-profit or government research institutes may qualify for special licence conditions.
</details>

<details markdown="1">
  <summary><strong>How long does it take for an academic license to be approved?</strong></summary>
For [academic institutions]({{"/account-and-license/#academic-users" | relative_url }}) recognized by their email domain, approval is instant and automatic upon email verification. If your domain is not recognized and requires manual validation, **the time may vary depending on the verification process** by the SIRIUS team. While we cannot guarantee specific timelines, we try to process requests within a few days.
</details>

<details markdown="1">
  <summary><strong>Can I obtain a SIRIUS trial license?</strong></summary>
Yes, we provide a **30-day trial license** that allows you to evaluate the software's performance on your specific datasets. Trial licenses are provided upon [request](https://bright-giant.com/request-demo/). We recommend scheduling an initial meeting for a brief training session to ensure your testing is efficient.
</details>

<details markdown="1">
  <summary><strong>Why haven't I received my account verification email?</strong></summary>
If the email does not arrive within five minutes, first **check your Spam folder**. Ensure you used your institutional email address correctly. Verification emails are sent from `noreply@mailgun.bright-giant.com`. **Institutional IT may block these emails** due to spam heuristics. Ask your IT department to allowlist this address.
If you continue to see an "Invalid Sign Up" error, it usually means **an account already exists for that email address**; try using the "Forgot Password" feature instead.
</details>

<details markdown="1">
  <summary><strong>Why am I getting an "Invalid Sign Up" error when creating an account?</strong></summary>
This error typically occurs if an account already exists for that email address. For security reasons, the system does not explicitly state that the email is taken. Use the "Forgot Password" feature to regain access.
</details>

<details markdown="1">
  <summary><strong>How can I activate my SIRIUS license as a MassHunter Explorer user?</strong></summary>
Users with a valid [Agilent MassHunter Explorer license]({{"/integrations/#MassHunter-Explorer" | relative_url }}) are eligible for a specialized SIRIUS group license for up to three accounts at no additional charge. Initial activation must be performed within the SIRIUS GUI on a Windows machine where MassHunter Explorer is installed to verify eligibility. For a detailed step-by-step guide on registering your group and adding users, [please refer to our full activation documentation]({{"/account-and-license/MHExplorer-license/" | relative_url }}).
</details>


## Installation and Network Connection

<details markdown="1">
  <summary><strong>What are the minimum system requirements for running SIRIUS?</strong></summary>
We [recommend]({{"/install/#system-requirements" | relative_url }}) a Quad Core CPU and 16 GB of RAM (but not less than 2 GB per CPU core). An active internet connection (1 Mbit/s minimum) is required to access web services such as structure annotation (CSI:FingerID) or compound class prediction (CANOPUS).

An SSD is highly recommended for storing the projects and custom databases that are accessed by SIRIUS.

Actual requirements also depend on the size of measured molecules, parameters and desired throughput. Specific computations (ZODIAC for whole dataset formula optimization) can have increased RAM requirements depending on dataset size.
</details>

<details markdown="1">
  <summary><strong>Where is the GUI on Linux?</strong></summary>
After downloading and extracting the distribution archive `-linux-x64.zip`, start `bin/sirius` from the terminal without any parameters, and it will automatically launch the GUI.
</details>

<details markdown="1">
  <summary><strong>What are the network requirements for SIRIUS to function behind a firewall? </strong></summary>
By default, SIRIUS requires outgoing access via Port 443 (HTTPS). To ensure full functionality, your IT team should allowlist the domains listed [here]({{"/admins/#connections" | relative_url }}).
</details>

<details markdown="1">
  <summary><strong>Why does SIRIUS show "No active subscription" or a connection error (X) for web services?</strong></summary>
By default, the additional description in the connection UI provides further information. This error typically occurs for one of three reasons:

* **Terms of Service:** You must manually accept the Terms of Service via the button in the SIRIUS GUI to activate the web services.
* **Email Verification:** Ensure you have clicked the link in your verification email.
* **Connection Check:** If your network blocks the URL used for the general internet connection check, SIRIUS may fail the validation. You can [set a custom reachable URL]({{"/admins/#connections" | relative_url }}) in your `sirius.properties` file as a workaround.
</details>

<details markdown="1">
  <summary><strong>How do I fix the error "Failed to connect to /127.0.0.1:8080"?</strong></summary>
This error occurs when the software cannot establish a local connection required for web service validation. It can sometimes be triggered if the URL for the general internet connection check is blocked. You can [set a custom reachable URL]({{"/admins/#connections" | relative_url }}) in your `sirius.properties` file as a workaround.
</details>

## Data Privacy and Security

<details markdown="1">
  <summary><strong>Is my data stored on your servers?</strong></summary>
SIRIUS operates primarily as a local application for data importing, data storage, preprocessing, molecular formula annotation and all data visualization. For features related to structural elucidation (such as structure search, compound class prediction, and confidence scoring) it utilizes the [SIRIUS Web Services]({{"/account-and-license/" | relative_url }}).
Data sent to our servers for computation is [not stored at rest]({{"/admins/#data-rest" | relative_url }}). It is held in memory temporarily for the duration of the calculation and is generally **deleted immediately after the results are fetched** by your local client. Typically, data remains on our servers for only a few minutes.

If you use a subscription with a compound limit, a cryptographic hash of the input spectra is stored for approximately 24 hours to track usage. **This hash is a one-way transformation and cannot be used to reconstruct your original spectra.** For users with unlimited or academic licenses, this hash is not stored.
</details>

<details markdown="1">
  <summary><strong>How is my data protected against unauthorized access during processing via the web services?</strong></summary>
All communication between your local SIRIUS installation and our web services is [secured]({{"/admins/#security" | relative_url }}) via **HTTPS and TLS encryption**. Access to web services requires a [user account]({{"/account-and-license/#account-creation" | relative_url}}). Your credentials (username/password) are never stored locally. Instead, SIRIUS uses a long-lived `refresh_token` and generates a unique session key for each computation job. Only the specific SIRIUS process that initiated a job and holds the unique session key can retrieve the resulting computation data from our servers.
</details>

## Data Import and Preprocessing

<details markdown="1">
  <summary><strong>Which mass spectrometry instruments are compatible with SIRIUS?</strong></summary>
SIRIUS is a **vendor independent** software. It [requires]({{"/advanced-background-information/#spectral-quality" | relative_url }}) **high-resolution, high-mass-accuracy data**, ideally within 10 ppm. It has been [trained on data]({{"/advanced-background-information/#training-data" | relative_url }}) from various instrument types, including TOF, Orbitrap, and FT-ICR. While it can process data with higher deviations (up to 50 ppm), the reliability of results may decrease. Instruments that do not provide high-resolution data, such as standard quadrupole or linear traps, are not supported.
</details>

<details markdown="1">
  <summary><strong>Does SIRIUS support data from GC-MS or DIA experiments?</strong></summary>
SIRIUS can handle GC-MS data if it uses soft ionization and fragmentation via CID; traditional EI-spectra are not currently supported. **SIRIUS is primarily designed for Data Dependent Acquisition (DDA)** with minimal chimeric spectra. Analyzing DIA data may be possible 1. for very sparse samples (most intensity in the MS/MS is generated by one precursor ion) or 2. by deconvoluting spectra (which often comes with the disadvantage of losing low intensity fragments). Both may require modifications to the standard SIRIUS workflow.
</details>

<details markdown="1">
  <summary><strong>Can I open SIRIUS 5 projects in SIRIUS 6?</strong></summary>
No, they are not compatible because underlying data structures changed significantly for SIRIUS 6\. SIRIUS 6 transitioned from a file-based project space to a [Nitrite database]({{"/io/#sirius-project-space" | relative_url }}) (stored as a singular `.sirius` file). To "revive" an old project, you must re-import the spectrum files into SIRIUS 6 and recompute. Results may differ due to improvements in scoring and trained models.
</details>

<details markdown="1">
  <summary><strong>What file formats does SIRIUS support? </strong></summary>
[SIRIUS supports several pre-processed formats]({{"/io/#input" | relative_url }}) including `.ms`, `.mgf`, `.mat/.msp`, and `.cef`. For full LC-MS runs, SIRIUS can import and process `.mzML` and `.mzXML` files, performing automated feature detection and alignment.
</details>

<details markdown="1">
  <summary><strong>Do I need to perform peak picking before importing data?</strong></summary>
Yes. [**SIRIUS expects centroided mass spectra.**]({{"/io/#input" | relative_url }}) If .mzML/.mzXML files with profile mode data are provided, SIRIUS will attempt centroiding. However, in general [we recommend vendor-specific centroiding, e.g. via msConvert](https://www.youtube.com/watch?v=o-Paxth07ow).
</details>

<details markdown="1">
  <summary><strong>How should I optimize my MS/MS data collection for SIRIUS?</strong></summary>
For the best results, aim for "clean" spectra where only one precursor compound is fragmented at a time. SIRIUS benefits from having more fragments; providing data with multiple collision energies or a RAMP setup is highly recommended to maximize the information available for structural elucidation. Whereas SIRIUS can analyse spectra that contain isotope peaks, it is generally preferred to use a <1 Da isolation window to only measure the monoisotopic (lowest-mass isotope) peak.
</details>

<details markdown="1">
  <summary><strong>Why is my .mzml (or other) file import failing with my Agilent subscription?</strong></summary>
[Agilent]({{"/integrations/#MassHunter-Explorer" | relative_url }}) subscriptions are exclusive to data acquired on Agilent instruments. You can [import data]({{"/io/#input" | relative_url }}) into SIRIUS using the MassHunter Explorer interface or via Agilent `.cef` files. 
You can view the available features of your subscription (such as supported file types) in the [user portal](https://portal.bright-giant.com/) (`Show subscription details`). To enable support for all file types, [please upgrade your subscription](https://bright-giant.com/request-demo/).
</details>

<details markdown="1">
<summary><strong> Why is my feature list empty after importing my data?</strong></summary>
If your feature list is empty, please verify file content: Open your file in a standard text editor to ensure it actually contains data. Confirm that the file includes supported spectrum data. Review the application log. It often provides specific error messages or indicates if something was explicitly discarded. As a final check, you can check your subscription in the [user portal(https://portal.bright-giant.com/) to see whether the file format is supported `(Show subscription details)`.
</details>


<details markdown="1">
  <summary><strong>Why is my LCMS tab empty after importing .cef, .mgf or other peaklist data?</strong></summary>
The [LC-MS view]({{"/gui/#lcms-tab" | relative_url }}) is for `.mzML` and `.mzXML` inputs where SIRIUS performs feature detection and alignment. When importing files that contain pre-processed peak lists rather than full LC-MS runs, this tab remains empty or deactivated.
</details>

<details markdown="1">
  <summary><strong>Can SIRIUS handle dimers/multimers or multiple charged ions?</strong></summary>

No, as of the current version, **SIRIUS only supports annotation of singly charged ions** (1+ or 1-). Recent updates have improved the *import* of dimers/multimers, but analysis for these is not yet supported. Multiple charged ions are filtered out during the preprocessing stage.

SIRIUS **will not support for multiple charged ions in the short term** since there is a plenty of algorithmic work that has to be done. In addition, we do not have enough training data of multiple charged ions. If you can provide such data this would help a lot to get such project started.
**Multimer support is possible in principle**, but it needs some adaptions of the fragmentation tree computation that are not implemented yet. It is on our list for future features.
</details>

<details markdown="1">
  <summary><strong>Can I use my own custom database?</strong></summary>
Yes. SIRIUS allows you to [import]({{"/gui/#custom-database-import" | relative_url }}) and search against your **own custom structure databases and spectral libraries**. You can import structures from CSV or TSV files containing SMILES strings, or import spectral libraries from .sdf, .msp or other processed peak list format. Custom databases are stored and processed locally as `.siriusdb` files. Once imported, they can be selected in the "Databases" dialog and **used alongside or instead of built-in databases**. If a structure in your custom database is already known to SIRIUS, the fingerprint is retrieved from our cache; otherwise, it is computed locally on your machine.

For companies with dedicated hosting, [Bright Giant](https://bright-giant.com/) can also provide a managed custom database in the backend.
</details>

## Computation

<details markdown="1">
  <summary><strong>How do I select multiple features for computation in the GUI?</strong></summary>
In the [feature list]({{"/gui/#overview" | relative_url }}) on the left, you can use standard selection keys (e.g., `Ctrl` or `Shift`) to select multiple features. Then, **right-click** on the selection and choose **Compute** from the context menu to open the [Batch Computation dialog]({{"/gui/#compute-dialog" | relative_url }}). You can also use the **Compute All** button in the toolbar.
</details>

<details markdown="1">
  <summary><strong>Why am I seeing excluded elements (like S, Cl, F) in my results?</strong></summary>
By default, [element filters]({{"/gui/#SIRIUS-strategies" | relative_url }}) are applied only to *de novo* generated formulas, not to those retrieved from bottom-up search or databases. To change this, you must enable the option to apply constraints to bottom-up and database candidates in the settings.
</details>

<details markdown="1">
  <summary><strong>How do I restrict elements in the CLI?</strong></summary>
To define the chemical search space, use `--elements-enforced` (or `-E`) for elements that can appear in every molecular formula and `--elements-considered` (or `-e`) for elements that are only considered if the MS1 isotope pattern suggests their presence (e.g., `S,Br,Cl`). 
</details>

<details markdown="1">
  <summary><strong>I am getting the error message: "Could not load a valid TreeBuilder (ILP solvers)[...]"</strong></summary>
SIRIUS uses an ILP solver to [calculate fragmentation trees]({{"/methods-background/#fragmentation-trees" | relative_url }}). It ships with the non-commercial CLP solver and should work out-of-the box. If you are nevertheless experiencing this issues, please contact us. 
It is almost impossible to reproduce and fix such issues without additional information from users. For more information on ILP solvers see the [installation instructions]({{"/install/#ILP-solvers" | relative_url }}).
</details>


<details markdown="1">
  <summary><strong>Why do some of my compounds remain uncomputed?</strong></summary>
If you notice certain features are absent, it usually boils down to two extremes: 
* (1) Fingerprints (and subsequent annotations) are only generated if the fragmentation tree can successfully explain at least two fragment peaks. 
* (2) To keep the processing queue moving, a "Compound Timeout" is enabled by default. This prevents a single, computationally "heavy" feature from stalling your entire project. If a compound exceeds this limit, it will be skipped. You can check the log for `CANCELED` jobs. You can adjust this threshold in the CLI using the `--compound-timeout` parameter or within the [GUI settings]({{"/gui/#SIRIUS-molecular-formula" | relative_url }}).
</details>

<details markdown="1">
  <summary><strong>How to configure SIRIUS to compute large data sets that contain high mass compounds? </strong></summary>
When [computing molecular formulas with SIRIUS]({{"/methods-background/#SIRIUS-molecular-formula" | relative_url }}), a few high mass compounds usually need most of the computing time, and some
of them might not finish computing in reasonable time at all and block your whole analysis.

The most straightforward solution is to exclude high mass compounds from the analysis,
by setting a mass threshold. This will usually allow you to annotate the vast majority of your data. However, many of
the higher mass compounds will work just fine, and it would be a pity to not annotate them. **Therefore, the recommended
solution is to set a per-compound timeout (`--compound-timeout`) so that a few hard cases cannot block your analysis**.
You can adjust this threshold in the CLI using the `--compound-timeout` parameter or within the [GUI settings]({{"/gui/#SIRIUS-molecular-formula" | relative_url }}). [Learn more about how to deal with high mass compounds.]({{"/faq/how-to-large-comp" | relative_url }})
</details>


<details markdown="1">
  <summary><strong>Can I access the structures used to train CSI:FingerID?</strong></summary>
Yes. To maintain transparency and support the scientific community, the training structures for the CSI:FingerID predictors are publicly available. You can [download]({{"/advanced-background-information/#training-data" | relative_url }}) the lists of structures (in CSV format) for both positive and negative ion modes directly from our WebAPI.
</details>


## Result Interpretation and Data Export

<details markdown="1">
  <summary><strong>How should I interpret confidence values?</strong></summary>
[COSMIC confidence scores]({{"methods-background/#COSMIC" | relative_url }}) need to be interpreted with some care. It is important to understand, that they do not represent probabilities, and thus there is no statistical interpretation of a confidence value. If doing large scale analysis, one should focus on the highest confident hits (for example the Top 1/5/10%), (mostly) regardless of their confidence value.
</details>

<details markdown="1">
  <summary><strong>An annotation received a low confidence score, even though am I sure it is correct!</strong></summary>
A low score does not necessarily mean the identification is wrong. The [COSMIC confidence score]({{"methods-background/#COSMIC" | relative_url }}) is designed to differentiate between high-confidence and low-confidence identifications across a dataset, not to rank candidates within a single feature.

If the structure database contains multiple candidates that are highly similar to your correct structure (e.g., isomers with slightly different side-group positions), CSI:FingerID scores and predicted fingerprints will be nearly identical.
COSMIC penalizes cases where it cannot clearly distinguish the top hit from the second-best hit. Since their MS/MS spectra are virtually indistinguishable, the [`exact` confidence]({{"/methods-background/#confidence-score-modes" | relative_url }}) remains low because the software cannot be certain this specific isomer is the right one. To solve this, use the [`approximate` confidence]({{"/methods-background/#confidence-score-modes" | relative_url }}) score. This mode evaluates whether the hit is "correct or highly similar."
</details>

<details markdown="1">
  <summary><strong>Why is the CSI:FingerID score negative but the confidence score high?</strong></summary>

The CSI:FingerID score is inherently negative, as it is a logarithmic probability score ranging from `-Infinity` (worst) to `0` (best).
It is difficult to assess the quality of the top-ranking structural candidate based solely on this score.
What is considered a "good" value depends heavily on the size and molecular formula of the compound, which in turn influences the number of active properties in the fingerprint.
To address this, we introduced a [confidence score]({{"/methods-background/#COSMIC" | relative_url }}) ranging from `0` (worst) to `1` (best).
A high confidence score means the algorithm estimates a very high probability that this annotation is correct. This score is specifically designed to separate correct hits from incorrect ones, which raw CSI:FingerID scores cannot do effectively on their own. While the confidence score is a robust indicator, it is not an absolute probability; for instance, a score of `0.8` indicates high confidence but does not equate to an 80% probability of correctness.
</details>


<details markdown="1">
  <summary><strong>Why is the score in the CSV different from the GUI?</strong></summary>
The GUI shows a normalized score (0–100%), while the CSV summary export in earlier versions (before release 6.2)) only contained the non-normalized SIRIUS score.
</details>

<details markdown="1">
  <summary><strong>How do I export fragmentation trees?</strong></summary>
In the CLI, use the standalone subtool `ftree-export`. In the GUI, you can export a tree for a specific candidate as an SVG by using the **export button** in the  [Formulas view]({{"/gui/#formulas-tab" | relative_url }}).
</details>

<details markdown="1">
  <summary><strong>How can I export the top 10 formulas via CLI or GUI?</strong></summary>
In the CLI, use the `summary` tool with the `--top-k-summary` parameter: `sirius -p project_dir summary --top-k-summary 10`. This will generate additional `*_top-10` summary files.

In the GUI, analysis [results can be exported]({{"/gui/#data-export" | relative_url }}) using the `Summaries` button in the top left toolbar.
By default, only the top hit is exported for each feature. Use the drop-down menu to export `Top k Hits`. 
</details>

<details markdown="1">
  <summary><strong>How can I access the probability of the predicted ClassyFire classes?</strong></summary>
[CANOPUS]({{"methods-background/#CANOPUS" | relative_url }}) calculates posterior probabilities for thousands of [ClassyFire](http://classyfire.wishartlab.com/) classes. These are stored in the project space. The most robust way to access precise probability scores is through the [API]({{"/quick-start/#API" | relative_url }}). You can query the specific feature results via the API to retrieve the predicted fingerprint and the associated compound class probabilities.

If you prefer not to use the API, you can export summaries via the CLI or GUI. The export summary files report the most specific class and its ancestors rather than the full probability distribution for every possible ClassyFire category. `canopus_formula_summary.[tsv|xlsx]` contains the predicted classes for the top-ranked molecular formula.
</details>


## REST API and Python SDK (PySirius)


<details markdown="1">
  <summary><strong>How do I login via the Python SDK?</strong></summary>
To login via the SDK, there is a dedicated login and account api. See for example [here for PySIRIUS](https://github.com/sirius-ms/sirius-client-openAPI/blob/master/client-api_python/generated/docs/LoginAndAccountApi.md).
</details>

<details markdown="1">
  <summary><strong>Why do I get a "400 Bad Request" when starting a job?</strong></summary>
This usually indicates a client-side error, such as malformed request syntax. It often happens **if you specify an [invalid workflow]({{"/methods-background/#workflows" | relative_url }})**, such as enabling structure search but disabling fingerprint prediction. Structure search requires fingerprints to function.
</details>

<details markdown="1">
  <summary><strong>How do I find the port SIRIUS is running on?</strong></summary>
[SIRIUS REST service]({{"/quick-start/#API" | relative_url }}) (e.g., via `sirius rest`) starts by default on a random free port. The active port is saved in a port file `~/.sirius/sirius-6.port` (for SIRIUS 6.x). Port can be also specified via command line (`sirius rest -p 8080 [--gui]`).
</details>

