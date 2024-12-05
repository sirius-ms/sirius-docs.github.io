---
permalink: /gui/
title: "Graphical User Interface"
---

## Overview {#overview}

SIRIUS 6 comes equipped with a comprehensive Graphical User Interface. Here's a breakdown of the key components and their functions.

{% capture fig_img %}
![Foo]({{ "/assets/images/gui-overview-marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>SIRIUS main application window.</figcaption>
</figure>

- The **toolbar <span style="color:#d40f57">[1-5]</span>** at the top of the screen provides quick access to the essential tools: 
The first button group <span style="color:#d40f57">**[1]**</span> on the left is for managing your project spaces, including creating, opening and saving them.
The second button group <span style="color:#d40f57">**[2]**</span> facilitates
[importing](#data-import) single features or data containing multiple features into
your project space. The third button group <span style="color:#d40f57">**[3]**</span> handles exporting data, e.g. for [GNPS FBMN](https://doi.org/10.1038/s41592-020-0933-6) analysis
or generating [project space summaries]({{ "/io/#sirius-project-space" | relative_url }}). 
The fourth button group <span style="color:#d40f57">**[4]**</span> is dedicated to computations, including [starting the computations](#compute-dialog), getting the job view and [importing custom databases](#custom-database-import).  
The last button group on the right <span style="color:#d40f57">**[5]**</span> provides access to the log, [settings](#settings), [webservice information](#webservice), and [account details](#account-creation). Here you can also find a link to the online documentation (`Help`) as well as information on software licence and related publications (`About`).

- The **feature list <span style="color:#d40f57">[6]</span>** on the left side contains all imported (aligned) features. 
A *feature* contains all MS and MS/MS spectra corresponding to a measured aligned
feature. For each feature, adduct type, precursor mass, retention time and [confidence score](#COSMIC) (if computed) are shown in this panel. 

- The **result view <span style="color:#d40f57">[8]</span>** is displayed to the right of the feature list and allows users to [examine different result views](#visualize-results). The tab selector <span style="color:#d40f57">**[7]**</span> at the top of this panel lets you switch between these views, offering various perspectives on your data.

- The **bottom information bar <span style="color:#d40f57">[9]</span>** provides details about your license status for the webservice-based structure elucidation tools, 
the number of computed features and feature limits.  

For a quick start, you can also watch our [tutorial video introducing basic elements and functionality of the SIRIUS 4 GUI](https://www.youtube.com/watch?v=SWkukL88ljo).

## Account creation {#account-creation}

Open `Account` in the top-right toolbar <span style="color:#d40f57">**[5]**</span>, and click `Create Account` to get to our [user portal](https://portal.bright-giant.com/auth/register/) and [create an account]({{"/account-and-license/#account-creation" | relative_url}}). After logging into the user portal, you can [request a license]({{"/account-and-license/" | relative_url}}).
In the SIRIUS GUI, click `Log in` to enter your account credentials and login.

## Data import {#data-import}

You can import `.ms`, `.mgf`, Agilent's `.cef`, `.mzml` and `.mzxml` files using the `Import` button or by drag-and-drop. 
SIRIUS will automatically extract all relevant attributes (such as MS level, ionization, and precursor mass)
from the file. 
When importing multiple `.mzml` (or `.mzxml`) at once, SIRIUS will ask you if it should align them.
When importing a [peak list file]({{ "/io/#peak-lists" | relative_url }}) containing a molecular formula, select `Ignore Formulas` if you would like to do the molecular formula annotation using SIRIUS.

For more information on supported file formats, refer to the [Input]({{ "/io/#input/" | relative_url }}) section.

[//]: # (## Sorting features, filtering features and changing displayed confidence score mode)

### Sort and filter {#sort-filter}

To sort the feature list, use right-click to open the <span style="color:#d40f57">**[sort]**</span> dialog below.

<img src="{{ "/assets/images/sort-filter.png" | relative_url }}" height="300" width="300">

You can sort aligned features by retention time (RT), mass, name, ID or confidence score (if available). At the bottom of
this dialog, you can select the confidence mode (approximate or exact) to be displayed and 
used for sorting. For more details, see the [confidence modes]({{ "/methods-background/#confidence-score-modes" | relative_url }}) section.

The displayed feature list is already filtered by quality, i.e., features containing only MS1 spectra, features with bad quality and multimeric features are hidden. Use the switches on top of the feature list to unhide. 

The feature list can further be refined by clicking the filter button (three dots to the right of the search field) to open the filter dialog:

<img src="{{ "/assets/images/filter-collage.png" | relative_url }}">

- In the `Input` tab (left), aligned features can be filtered by mass range, retention time range, and confidence score range, as well as by specific detected adducts. 
- In the `Data Quality` tab (middle), you can refine the preset quality filter.
- In the `Results` tab (right), you can filter for specific element constraints in either the neutral molecular formula or precursor formula,
 as well as by detected lipid classes. If structure database results are available, you can filter for hits in specific structure databases.  `Candidates to check` allows you to specify the number of top candidates to consider.

For all filters, you can also choose to invert the filter, and whether you want to delete all **non**-matching compounds.

### Import of custom structure and spectra databases {#custom-database-import}

Custom structure databases and spectral libraries can be added via the `Databases` dialog, accessible via the
top center of the GUI ribbon.
Starting with version 6.0, SIRIUS supports the import of spectral libraries. 
The supported import formats for spectral
data are .ms, .mgf, .msp, .mat, .txt (MassBank), .mb, .json (GNPS, MoNA). Spectra must be annotated with a structure and must be centroided.
Custom structures can be used in [structure database search](#CSIFingerID-structure). Imported spectra will be used for [spectral library matching]({{ "/methods-background/#spectral-library-search" | relative_url }}).

{% capture fig_img %}
![Foo]({{ "/assets/images/custom-db-loader-collage.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Custom database dialog.</figcaption>
</figure>

Custom databases are stored as files with the `.siriusdb` extension. 
If you already have a database with this format on your local machine, you can add it to SIRIUS by clicking the 
`Add existing Database` button on the bottom right <span style="color:#d40f57">**[1]**</span>. To create a new custom database,
click the `Create custom Database` button.
Imported databases can be deleted or modified using the `-` or pencil icon button, respectively.

To create a custom database, enter a name for the database <span style="color:#d40f57">**[2]**</span> (maximum length: 15 characters), which will be displayed in the structure database search dialog. 
Specify the file name for the database, ensuring it ends with `.siriusdb` <span style="color:#d40f57">**[3]**</span>. 
Choose be any valid, writeable path on your local machine to store the database <span style="color:#d40f57">**[4]**</span>.
Adjust the buffer size <span style="color:#d40f57">**[5]**</span> to control how many structures or spectra should be kept in memory. This can be increased when importing large files on a faster machine.
Drag and drop files or directories containing structure/spectra files to the input area <span style="color:#d40f57">**[6]**</span>, or use the `+` button to browse your file system.

For more details on custom database import and supported file formats, please see the [Custom database tool]({{ "/cli-standalone/#custom-database-tool" | relative_url }}) section.

**Please note that you have to be logged in to your SIRIUS account to import custom databases.**

## Computing results {#computing-results}
SIRIUS offers two computation modes for processing data: *Single
Computation* and *Batch Computation*. Both computation dialogs are similar in layout and functionality.

- The **Single Computation** mode allows you
to set different parameters for each individual feature. You can initiate a single computation by
right-clicking on a **single** feature and selecting `Compute` in the context menu or by double-clicking on the feature. When opening the dialog, element prediction is performed to preset the chemical alphabet. However, you can change this alphabet in the `Element Filter` if you like.

- For **Batch Computation** you can right-click on 
**multiple** selected features and choose `Compute` to process them collectively. Alternatively, you can use
the `Compute All` button in the toolbar to compute all features in the project space. Here, you can use the `Element Filter` to choose the elements that should automatically be detected for each feature.
Additionally, you
can choose whether to recompute and override results for features that  have already been analyzed.

The following section provides a detailed explanation of the compute dialog, using the *Batch Computation* dialog as example.

### Compute dialog {#compute-dialog}

Starting with SIRIUS 6, the compute dialog has been streamlined to improve clarity by displaying only those settings that are essential for any type of analysis. Use the `Show advanced settings` button at the bottom <span style="color:#d40f57">**[7]**</span> to include additional settings that are relevant for specific use cases or to set limits for computation times.

{% capture fig_img %}
![Foo]({{ "/assets/images/compute-dialog.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Batch compute dialog.</figcaption>
</figure>

The compute dialog is divided into five subtools: 
[SIRIUS molecular formula annotation](#SIRIUS-molecular-formula) <span style="color:#d40f57">**[1]**</span>, 
[ZODIAC](#ZODIAC-ranking) <span style="color:#d40f57">**[2]**</span>, 
[CSI:FingerID fingerprint prediction with CANOPUS](#CSIFingerID-CANOPUS) <span style="color:#d40f57">**[3]**</span>, 
[CSI:FingerID structure database search](#CSIFingerID-structure) <span style="color:#d40f57">**[4]**</span> and 
[MSNovelist](#generating-de-novo-structure-candidates-with-msnovelist) <span style="color:#d40f57">**[5]**</span>. As of SIRIUS 6, CANOPUS is automatically executed together with the fingerprint prediction. 
**Subtools can be selected individually or combined, but note that the selection must align with a [valid SIRIUS workflow]({{ "/methods-background/#workflows" | relative_url }}).**
For example, you cannot search structure databases without predicting fingerprints first. 

If the `Recompute already computed tasks?` checkbox <span style="color:#d40f57">**[6]**</span> is checked, all previously existing results for the selected features in the current project space will be invalidated and overwritten to execute the newly selected workflow. Additional parameters for specific subtools can be displayed using the `Show advanced settings` button  <span style="color:#d40f57">**[7]**</span>. 
To easily convert the current workflow selections into a CLI command, use the `Show Command` button <span style="color:#d40f57">**[8]**</span> at the bottom right.
You can save your computation setup as a preset to reload for the next computation <span style="color:#d40f57">**[9]**</span>. 

### Spectral library matching {#spectral-library-matching}

If imported spectral libraries are available, SIRIUS will automatically perform spectral matching. This process runs in the background without the need for any addiotional parameters. For more information, refer to the [Spectral library matching]({{ "/methods-background/#spectral-library-search" | relative_url }}) section.


### Identifying molecular formulas with SIRIUS {#SIRIUS-molecular-formula}

To identify molecular formulas using SIRIUS, you can 
set general parameters <span style="color:#d40f57">**[A]**</span>, 
specify fallback adducts <span style="color:#d40f57">**[B]**</span>, and 
choose the appropriate molecular formula annotation strategy <span style="color:#d40f57">**[C]**</span>.

{% capture fig_img %}
![Foo]({{ "/assets/images/compute-dialogue-sirius.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Molecular formula annotation compute dialog.</figcaption>
</figure>

- **General settings <span style="color:#d40f57">[A]</span>:** 
  In the `Instrument` field, you can choose `Q-TOF`, `Orbitrap` or `FT-ICR`.
  The choice of instrument affects only a few parameters,
  primarily the allowed mass deviation. If your instrument is not among these options, 
  select `Q-TOF` as default.
  
  You can  change the maximum allowed mass deviation, ensuring that
  SIRIUS only considers molecular formulas with mass deviations below
  the specified ppm threshold. For masses below 200 Da, the allowed mass deviation is
  $(200 \cdot \frac{ppm_{max}}{10^6})$.

  If SIRIUS predicts that the query spectrum may be a lipid, the molecular formula
  according to that prediction can be added and enforced (default).

- **Fallback Adducts <span style="color:#d40f57">[B]</span>:**
  You can specify fallback adducts that will be used for all features for which no adducts were detected during SIRIUS import or prior external annotation. Using the `enforce` option, you can even enforce to consider the selected adducts for all features (in addition to the detected adducts). [The "base ionization" of the detected adduct will be considered by default.]({{"/io/#lcms-runs" | relative_url}}) <br> 
  **Possible Adducts:** Be aware that in the **compute dialog for a single compound**, the adduct selection is different. The detected adducts (or SIRIUS default adducts) are pre-selected. Select the adducts you want to be considered for computation and deselect the adducts you not want to be considered for computation.<br>
  <img src="{{ "/assets/images/compute-dialogue-sirius-single.png" | relative_url }}" alt="Molecular formula annotation compute dialog for a single compound" height="400">

- **Molecular Formula Generation <span style="color:#d40f57">[C]</span>:**
Selecting an appropriate molecular formula annotation strategy is crucial for a successful SIRIUS analysis, as it will significantly impact subsequent steps. Parameters for the different strategies are explained in the following. Before selecting a strategy, it is important to understand the differences between the [molecular formula annotation strategies]({{ "/methods-background/#annotation-strategies" | relative_url }}) to choose the most appropriate one for your analysis.

***De novo* + bottom up search is recommended for generic applications.** [Learn more.]({{"/methods-background/#annotation-strategies" | relative_url}})

{% capture fig_img %}
![Foo]({{ "/assets/images/compute-dialogue-sirius-denovo-bottomup.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Settings for bottom up + de novo search.</figcaption>
</figure>

 You can configure
the m/z threshold <span style="color:#d40f57">**[1]**</span> below which *de novo* molecular formula annotation will be performed alongside the bottom up search.

The element filter <span style="color:#d40f57">**[2]**</span> can be applied either solely to the *de novo* annotations or to the bottom-up search as well.
`Allowed elements` specifies the elements in the element set and their upper and lower limits. `Autodetect` specifies the elements
for which SIRIUS will automatically detect the presence, absence and quantity from the input data (requires MS1 spectra). The element set can
be adjusted using the `...` button <span style="color:#d40f57">**[a]**</span>. Before making significant changes to the element set, please consider the potential impact on running time and result quality. 

**De novo only is recommended for discovering "unknown unknowns".** [Learn more.]({{"/methods-background/#annotation-strategies" | relative_url}})

{% capture fig_img %}
![Foo]({{ "/assets/images/compute-dialogue-sirius-denovo.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Settings for de novo only annotation.</figcaption>
</figure>

Here, the expected element set needs to be well-defined using the `Element filter` settings <span style="color:#d40f57">**[2]**</span>. 
`Allowed elements` and `Autodetect` elements can again be adjusted using the `...` button <span style="color:#d40f57">**[a]**</span>, with keeping in mind the impact on running time and result quality. 

**Database search is recommended for known compounds and extremely fast computation times.** [Learn more.]({{"/methods-background/#annotation-strategies" | relative_url}})

{% capture fig_img %}
![Foo]({{ "/assets/images/compute-dialogue-sirius-dbsearch.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Settings for formula database search.</figcaption>
</figure>

Choose the databases <span style="color:#d40f57">**[1]**</span> to be used for molecular formula annotation. Per default, the [*biomolecule structure databases*]({{ "/methods-background/#CSIFingerID" | relative_url }}) are selected.
You can restore this default selection using the `bio` button.

Applying an element filter <span style="color:#d40f57">**[2]**</span> is not mandatory for formula database search, but it can be used to narrow down molecular formula candidates. `Allowed elements` and `Autodetect` elements can again be adjusted using the `...` button <span style="color:#d40f57">**[a]**</span>.

**Bottom up search only can be used for a slight speed increase compared to the recommended combined approach.** [Learn more.]({{"/methods-background/#annotation-strategies" | relative_url}})

{% capture fig_img %}
![Foo]({{ "/assets/images/compute-dialogue-sirius-bottomup.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Settings for bottom up search only.</figcaption>
</figure>

Also for bottom up search, applying an element filter <span style="color:#d40f57">**[2]**</span> is not mandatory, but can be used to narrow down molecular formula candidates. `Allowed elements` and `Autodetect` elements can again be adjusted using the `...` button <span style="color:#d40f57">**[a]**</span>.

**Specifying a molecular formula (or list of molecular formulas) to run formula annotation works in single computation mode only.**

{% capture fig_img %}
![Foo]({{ "/assets/images/compute-dialogue-sirius-provide.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Specify a (list of) molecular formula(s) to run formula annotation.</figcaption>
</figure>

If you have imported a [peak list file]({{ "/io/#peak-lists" | relative_url }}) containing a molecular formula, you cannot override this formula using the compute dialog. To avoid this, select `Ignore Formulas` during [import](#data-import).


#### Advanced settings for molecular formula annotation {#SIRIUS-advanced}

{% capture fig_img %}
![Foo]({{ "/assets/images/compute-dialogue-sirius-advanced.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Advanced parameters for molecular formula annotation.</figcaption>
</figure>

- By default, molecular formula candidates whose theoretical isotope     
  patterns deviate significantly from the measured isotope pattern are discarded. 
  You can disable this setting <span style="color:#d40f57">**[1]**</span>
  if you suspect poor quality isotope patterns in the input data.
- If isotopic peaks are present in the input MS2 spectrum, they can 
  either be used for scoring (`SCORE`) or be ignored (`IGNORE`) <span style="color:#d40f57">**[2]**</span>.
- You can select the number of molecular formula candidates that will be 
  saved <span style="color:#d40f57">**[3]**</span>.
- Specify the minimum number of molecular formula candidates to store 
  for each ionization state, even if they are not among the top n candidates <span style="color:#d40f57">**[4]**</span>.
- Set a time limit (in seconds) for computing the fragmentation tree for 
  a single molecular formula candidate 
  <span style="color:#d40f57">**[5]**</span>. Set to 0 to disable the limit.
- Set a total time limit (in seconds) for computing the fragmentation 
  trees for all molecular formula candidates of a feature 
  <span style="color:#d40f57">**[6]**</span>. Set to 0 to disable the limit.
- For higher mass compounds, SIRIUS can compute fragmentation trees 
  heuristically instead of exactly. This heuristic method can be used to pre-rank molecular formula candidates, with exact trees 
  computed only for the top candidates. Set the m/z value above which this approach will be applied <span style="color:#d40f57">**[7]**</span>. 
- For very high masses, exact solutions may be impractical, and only
  heuristic trees should be computed. Set the m/z value above which trees will exclusively be computed using the heuristic <span style="color:#d40f57">**[8]**</span>. 

### Improve molecular formula ranking with ZODIAC {#ZODIAC-ranking}

ZODIAC enhances *de novo* molecular formula annotation for complete biological datasets, that is high-resolution, high-mass-accuracy LC-MS/MS runs. It refines the ranking of molecular formula candidates by analyzing similarities among features in the dataset, using fragmentation trees as input.

To use ZODIAC, select both SIRIUS and ZODIAC in the compute panel. This is only possible for [batch computation](#computing-results). 
To increase the chance of the correct molecular formula candidate to be in the result list, increase the number of reported candidates for SIRIUS.

<span style="color:#d40f57">**!!** Zodiac should not be used for non-biological samples (like standard mixtures) **!!**</span>

For more details, visit the [ZODIAC release page](https://bio.informatik.uni-jena.de/software/zodiac/).

#### Advanced settings for ZODIAC {#ZODIAC-advanced}

These parameters are very advanced and require a thorough understanding of ZODIAC and its underlying Gibbs sampler.

{% capture fig_img %}
![Foo]({{ "/assets/images/compute-dialogue-zodiac-advanced.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Advanced parameters for ZODIAC molecular formula annotation.</figcaption>
</figure>

- Specify the maximum number of candidate molecular formulas considered  
  for features with m/z lower than 300. 
  <span style="color:#d40f57">**[1]**</span>
- Specify the maximum number of candidate molecular formulas considered 
  for features with m/z higher than 800.
  <span style="color:#d40f57">**[2]**</span>
- Enable or disable the 2-step approach, where higher quality features 
  are processed first, followed by lower quality features second.
  <span style="color:#d40f57">**[3]**</span>
- Set the threshold for the ratio of edges of the complete network to be ignored.
  <span style="color:#d40f57">**[4]**</span>
- Specify the minimum number of connections required for each candidate.
  <span style="color:#d40f57">**[5]**</span>


### Predicting molecular fingerprints with CSI:FingerID and compound classes with CANOPUS {#CSIFingerID-CANOPUS}

After computing the fragmentation trees, you can predict [molecular fingerprints]({{ "/methods-background/#molecular-fingerprint" | relative_url }}) 
and [CANOPUS compound classes]({{ "/methods-background/#CANOPUS" | relative_url }}).
These predictions can then be used to search structure databases and/or to predict novel structures with [MSNovelist]({{ "/methods-background/#MSNovelist" | relative_url }}).
If `Score threshold` is selected, fingerprints are only predicted for the top-scoring fragmentation trees (molecular formulas). This is recommended and should only be disabled if you need to examine 
the fingerprint of a lower-scoring molecular formula.

CANOPUS predicts [ClassyFire](http://classyfire.wishartlab.com/) compound classes from the molecular fingerprint, without using any structure database. Classes are predicted for all features whose
fragmentation tree contains at least three fragments, including features with no matching structure candidates in the
database. There are no parameters to set. Compound classes are predicted separately for each
molecular formula.

For more details, visit the [CANOPUS release page](https://bio.informatik.uni-jena.de/software/canopus/).

### Identifying the molecular structure with CSI:FingerID {#CSIFingerID-structure}

CSI:FingerID facilitates the identification of molecular structures by matching 
predicted molecular fingerprints against database structures. SIRIUS ships with a wide range of [built-in databases]({{ "/methods-background/#CSIFingerID" | relative_url }}). 
Additionally, users can enhance the search capabilities by adding their own structures as a "custom database" (see [Import of custom structure and spectra databases](#custom-database-import) which can then be searched alongside
the existing databases.

{% capture fig_img %}
![Foo]({{ "/assets/images/compute-dialogue-csifingerid.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Parameters for structure database search.</figcaption>
</figure>

#### Expansive search {#expansive-search}
Structure database search is conducted within the user-selected databases <span style="color:#d40f57">**[2]**</span>. 
You can choose to use PubChem as a fallback database <span style="color:#d40f57">**[1]**</span> in case it contains a hit with higher confidence than
those found in the selected databases.
For a detailed explanation, please refer to the 
[Methods]({{ "/methods-background/#expansive-search" | relative_url }}) section. 
The `Confidence mode` controls whether the approximate or exact [confidence mode]({{ "/methods-background/#confidence-score-modes" | relative_url }}) is used to asses if a
hit in PubChem is more reliable than the hits in the selected databases.

Per default the structure databases constituting the "bio" database are selected <span style="color:#d40f57">**[2]**</span>. De-select `PubChem as fallback` to fully search in PubChem. 
If database search was used for [molecular formula identification](#SIRIUS-molecular-formula), the same databases are selected here. 
You can revert to the default selection by clicking `bio`. If any custom database was loaded, 
they can be selected here as well.

#### COSMIC - confidence values for CSI:FingerID searches {#COSMIC}
COSMIC confidence scores are calculated automatically and without requiring any parameters every time a CSI:FingerID search is performed. 
COSMIC scores are displayed in the feature list on the left. Starting with SIRIUS 6, confidence scores
are computed in both `exact` and `approximate` mode. 

For more details, 
see the [COSMIC]({{ "/methods-background/#COSMIC"  | relative_url }}) section or
visit the [COSMIC release page](https://bio.informatik.uni-jena.de/software/cosmic/).

### Generating *de novo* structure candidates with MSNovelist {#MSNovelist-denovo-structure}

In some cases, it is necessary to go beyond the limits of structure database search.
To address this, SIRIUS 6 newly offers *de novo* generation of candidate structures through [MSNovelist]({{ "/methods-background/#MSNovelist" | relative_url }}), in addition to predicting molecular fingerprints and compound classes, as well as searching in custom databases.

Be aware that the likelihood of any *de novo* generation method performing well for truly novel compounds is very low. Therefore, the results from MSNovelist should rather be considered as suggestion or starting point
for semi-manual analysis of compounds that cannot be elucidated otherwise. 

**MSNovelist will significantly slow down your SIRIUS workflow; use with caution.**

## Visualization of the results {#visualize-results}
The feature provides not only information about the input and compute state, it also displays the COSMIC
confidence score for the top CSI:FingerID hit.

**For each feature, different result views can be displayed by switching between the tabs in the result panel:**
- The [LC-MS view](#lcms-tab) displays the chromatographic feature alignment as well as a quality assessment of the spectra. This tab is only in use for mzML and mzXML inputs.
- The [Formulas view](#formulas-tab) displays the results from the molecular formula identification.
- The [Predicted Fingerprints view](#fingerprint-tab) contains information about
the molecular properties of the molecular fingerprint predicted by CSI:FingerID.
- The [Compound Classes view](#CANOPUS-tab) shows the [Classyfire](http://classyfire.wishartlab.com/) classes predicted by CANOPUS.
- The [Structures view](#structures-tab) displays results from the CSI:FingerID structure search.
- The [De Novo Structures view](#MSNovelist-tab) displays MSNovelist-generated structure candidates for the current query.
- The [Substructure Annotations view](#substructure-tab) shows possible substructures connected to
the peaks of the MS/MS spectrumfor each candidate.
- The [Library Matches view](#library-matches-tab) shows matches to reference spectra if a spectral library was imported.

In all views, the top CSI:FingerID hit (as well as "highly similar" compounds in [approximate confidence mode]({{"/methods-background/#confidence-score-modes" | relative_url }})) is highlighted in green.

### LC-MS view {#lcms-tab}

The `LC-MS` tab is empty when no LC-MS data (`.mzML` or `.mzXML`) was imported, i.e. if the data is imported as `.mgf`, `.ms` or similar file formats, no LC-MS information is available. This is also the case when LC-MS data has been processed with OpenMS or MZMine and then imported to SIRIUS.

{% capture fig_img %}
![Foo]({{ "/assets/images/lcms-view-features.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>LCMS view displaying the feature alignment and quality assessment.</figcaption>
</figure>

The LC-MS view displays the ion chromatogram of a feature.
You can choose whether to show the feature alignment or the adduct/isotope assignment <span style="color:#d40f57">**[1]**</span>. Retention times are always given in minutes.

For the feature alignment, the mass traces of all runs aligned for this feature are displayed in different colors <span style="color:#d40f57">**[2]**</span>. The thick black line is the merged mass trace <span style="color:#d40f57">**[3]**</span>. The bold grey box highlights the selected feature <span style="color:#d40f57">**[4]**</span>. Other nearby features are marked with thin grey boxes and features with low quality are marked with dashed boxes <span style="color:#d40f57">**[5]**</span>. You can click on a box to zoom into the feature. A gray dashed line marks the *noise level*; its exact computation may vary from version to version, but it is related to the median intensity of all peaks in the MS scan.

{% capture fig_img %}
![Foo]({{ "/assets/images/lcms-view-features-zoom.png" | relative_url }})
{% endcapture %}
<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Zoom into feature alignment.</figcaption>
</figure>

In the [adduct/isotope assignment view]({{ "/adducts" | relative_url}}), you can find the merged mass trace of the feature (bold black line) as well as its isotopes (dashed black lines) and correlated adducts (grey lines) with their isotopes (dashed grey lines). 

On the right, there is a basic quality assessment panel <span style="color:#d40f57">**[6]**</span>. It can be used to preemptively get an idea on overall quality of the MS and MS/MS of the feature.


### Formulas view {#formulas-tab}

The `Formulas` tab displays the molecular formula candidate list <span style="color:#d40f57">**[1]**</span>, the mass spectra <span style="color:#d40f57">**[2]**</span> and the fragmentation tree <span style="color:#d40f57">**[3]**</span> of the selected feature. 

{% capture fig_img %}
![Foo]({{ "/assets/images/formulas-view-custom.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Formulas view.</figcaption>
</figure>


- **Molecular formula candidate list** <span style="color:#d40f57">**[1]**</span>: 
  The candidates are ranked by the SIRIUS score. The molecular formula of the best candidate structure found by CSI:FingerID is highlighted in green (and does not necessarily have to be the candidate with the best SIRIUS score). Per default, the candidates are ordered by SIRIUS score but can be sorted by any other column.  


  The `Sirius Score` is a combination of the score from the isotope pattern analysis (`Isotope Score`) of the MS1 data and the fragmentation tree score (`Tree Score`) from the MS2 data. It is calculated by summing both scores and then converting them into probabilities using the softmax function. These probabilities sum to one. While a higher posterior probability for the top hit might suggest that this molecular formula is more likely to be correct, **it is important to note that a posterior probability of 90% does not mean there is a 90% chance that the molecular formula identification is correct!** The displayed probabilities are neither q-values nor Posterior Error Probabilities. The bars visually represent this value, with a full bar indicating the highest score in the column. **The SIRIUS score is the primary score that users should focus on.**
  
  The `Isotope Score` and `Tree Score` themselves are log-transformed posterior probabilities. The bars range from the lowest score in the column (empty) to the highest score in the column (full). 

  The `Zodiac Score` is also a probability, and here too, the bars directly represent this value. 

- **Mass spectra** <span style="color:#d40f57">**[2]**</span>: In this panel you can switch between the MS1 spectrum, the MS1 isotope pattern mirror plot or the MS2 spectra <span style="color:#d40f57">**[a]**</span>. For MS2 spectra, per default a merged spectrum is displayed, but you can also choose to view individual spectra <span style="color:#d40f57">**[b]**</span>. To zoom, hold the right mouse button and drag to select an area, or scroll while hovering over an axis. In the MS2 view, peaks annotated by the fragmentation tree are highlighted in green, while those identified as noise are colored black. Hovering over a peak will display its detailed annotation <span style="color:#d40f57">**[c]**</span>, and clicking on a green peak will highlight the corresponding node in the fragmentation tree. 
Spectra views can be exported using the top right export button <span style="color:#d40f57">**[d]**</span>.

- **Fragmentation tree** <span style="color:#d40f57">**[3]**</span>: In the computed
  [fragmentation tree]({{ "/methods-background/#fragmentation-trees" | relative_url }}), each node assigns a molecular formula to a peak in the (merged) MS2 spectrum. 
  Each edge is a hypothetical fragmentation reaction. You can customize the tree's appearance by selecting different node styles and color schemes <span style="color:#d40f57">**[e]**</span>.

  The tree can be exported <span style="color:#d40f57">**[f]**</span> as SVG or PDF vector graphics.
  Alternatively, the DOT file format provides a text-based description of the
  tree, which can be rendered externally using tools like Graphviz to convert
  DOT files into image formats such as PDF, SVG, or PNG. 
  The JSON format offers a machine-readable representation of the
  tree. For instructions on exporting fragmentation trees from the command line, see the [Fragmentation tree export tool]({{ "/cli-standalone/#ftree-export" | relative_url }}).

### Predicted Fingerprints view {#fingerprint-tab}
Even if CSI:FingerID does not identify the correct structure — in
particular if the correct structure is not present in any structure database —
you can still get valuable information about the structure by examining the predicted fingerprint. 
The `Predicted Fingerprints` tab displays a list of all molecular properties that make up the predicted fingerprint. When you select a molecular property, examples related to that property are shown below the list.

{% capture fig_img %}
![Foo]({{ "/assets/images/fingerprints-view.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Predicted fingerprints view.</figcaption>
</figure>

### Compound Classes view {#CANOPUS-tab}

{% capture fig_img %}
![Foo]({{ "/assets/images/classes-view.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Compound classes view displaying the CANOPUS results.</figcaption>
</figure>

The `Compound Classes` tab visualizes the [CANOPUS]({{ "/methods-background/#CANOPUS" | relative_url }}) compound class predictions in a table format <span style="color:#d40f57">**[4]**</span>. Each row in the table represents
one compound class. The `Posterior Probability` (number and bar) indicates the likelihood that 
the measured spectrum, given the chosen molecular formula, 
belongs to that class. Additional columns provide related information from the
[ClassyFire](http://classyfire.wishartlab.com/) ontology, e.g. the respective parent class.

Above the table are two lists: `Main classes` and `Alternative Classes`. 
- **Main Classes**  <span style="color:#d40f57">**[1]**</span>: The main class of a feature is the
*most specific* compound class with the highest priority from all compound classes with posterior probability above 50% (in green), along with its ancestor classes (in blue) in the ClassyFire ontology. 
- **Alternative classes** <span style="color:#d40f57">**[2]**</span>: This list contains
all other classes with posterior probability above 50%. In the ClassyFire chemontology, each compound is assigned to multiple classes. 
- **Natural Product Classes** <span style="color:#d40f57">**[3]**</span>: Starting from version 5, SIRIUS also predicts Natural Product classes.

### Structures view {#structures-tab}
{% capture fig_img %}
![Foo]({{ "/assets/images/structures-view.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Structures view.</figcaption>
</figure>

The `Structures` tab displays candidate structures for the selected 
molecular formula, ranked by the CSI:FingerID search score. 
The highest-scoring candidate is highlighted
in green <span style="color:#d40f57">**[1]**</span>. If you have enabled [approximate confidence mode]({{ "/methods-background/#confidence-score-modes" | relative_url }}), all candidates within an MCES distance of 2
will also be highlighted (see [Expansive search]({{ "/methods-background/#expansive-search" | relative_url }})).
To filter the candidate list by a specific database (e.g., only compounds from KEGG
and BioCyc), click the filter button <span style="color:#d40f57">**[2]**</span> in the top left corner. A menu <span style="color:#d40f57">**[3]**</span> will open, displaying
all available databases. Only candidates from the selected databases will be shown. 

The green and pink squares are a visualization of the CSI:FingerID
predictions and scoring <span style="color:#d40f57">**[4]**</span>.
- **Green squares** represent molecular substructures
present in the candidate structure that are predicted by
CSI:FingerID to be present in the measured feature. The intensity of the color indicates the predicted probability, and the size of the square reflects the reliability of the predictor.
- **Pink squares** represent substructures that
are predicted to be absent but are, nevertheless, found in the candidate
structure. The intensity of the color reflects the probability that the structure should be absent. 

<img src="{{ "/assets/images/boxes.png" | relative_url }}" alt="Fingerprint box representation" width="800">

Overall, a correct prediction is typically characterized by many large, intense green squares and as few large, intense pink squares as possible.

Hovering over a square displays the description 
of the molecular substructure (usually a SMARTS expression) <span style="color:#d40f57">**[5]**</span>. 
Clicking on a square highlights the corresponding
atoms in the molecule <span style="color:#d40f57">**[6]**</span>.
If the substructure appears multiple times, the first appearance is
highlighted in dark blue, while the other matches are highlighted in
translucent blue.

To filter the candidate list for for structures that contain the selected substructure, use the other filter button in the top left corner <span style="color:#d40f57">**[7]**</span>. You can also filter by SMARTS pattern <span style="color:#d40f57">**[8]**</span> or using the full-text search <span style="color:#d40f57">**[9]**</span>.

Right-clicking on a proposed structure opens a context menu <span style="color:#d40f57">**[10]**</span>, allowing you to:
- Copy the InChI or InChI Key to your clipboard
- Open the compound in PubChem
- Open the compound in all databases 
- Highlight matching substructures 
- Show the annotated spectrum in the `Substructure Annotations` tab

**Highlight matching substructures:**
When you choose `Highlight matching substructures` from the context menu, substructures in all structure candidates will be color-coded as follows:

* **green:** substructures that are supported by fingerprint evidence.
* **pink:** substructures that contradict fingerprint evidence.
* **yellow:** substructures with mixed support, where both agreeing and disagreeing fingerprint evidence is present.
* **no color:** substructures that are not clearly covered by fingerprint evidence

{% capture fig_img %}
![Foo]({{ "/assets/images/structures-view-highlighting.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Highlighting of matching substructures.</figcaption>
</figure>

If the structure is contained in any database, a label
with the name of this database is displayed below the structure <span style="color:#d40f57">**[11]**</span>. Database labels have different colors: <a id="db-labels"></a>
- **black on light blue:** This database is linked. Clicking on the label opens the database entry in your browser <span style="color:#d40f57">**[a]**</span>.
- **grey on light blue:** This database is not linked .
- **white on dark blue:** This is a lipid database <span style="color:#d40f57">**[b]**</span> or "El Gordo" lipid class annotation (see below) <span style="color:#d40f57">**[d]**</span>.
- **black on yellow:** This is a [custom database](#custom-database-import) loaded by the user <span style="color:#d40f57">**[c]**</span>.
- **grey on yellow:** This is result from a [custom database](#custom-database-import) but the database has not been loaded. This can for example happen when loading results that have been computed with a custom database that is not available on the current system.
- **pink:** This structure has been generated by [MSNovelist](#MSNovelist-denovo-structure). Die label is only used in the [De Novo Structures view](#MSNovelist-tab).

<img src="{{ "/assets/images/structures-view-dbs.png" | relative_url }}" alt="Database labels" width="800">


This view also includes the "El Gordo" lipid class annotation <span style="color:#d40f57">**[d]**</span>. If the feature has been identified as lipid by "El Gordo" a blue notification displaying the classification will appear below the top ribbon.
If the candidate structure is also a lipid, the classification will be added to the database labels.
Be aware that lipid structures are often highly similar, differing only in the position of double bonds. 
These subtle differences can be indistinguishable using mass spectrometry alone, which is why the overarching lipid class is displayed.

<img src="{{ "/assets/images/structures-view-lipids-expansive.png" | relative_url }}" alt="Lipids" width="800">


If the PubChem fallback was activated as part of [Expansive search]({{ "/methods-background/#expansive-search" | relative_url }}), a pink notification will be displayed below the top ribbon <span style="color:#d40f57">**[e]**</span>.

Finally, if a structure candidate has a reference spectrum imported 
via a custom database, the spectral match will be displayed.
Clicking on this match will take you to the [`Library Matches` tab]({{ "/gui/#library-matches-tab" | relative_url }}).

<img src="{{ "/assets/images/structures-view-spectrum.png" | relative_url }}" alt="Reference spectrum" width="800">


### De Novo Structures view {#MSNovelist-tab}

{% capture fig_img %}
![Foo]({{ "/assets/images/msnovelist.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>*De novo* structure annotation view.</figcaption>
</figure>

The `De Novo Structures` tab displays the *de novo* structure generation results produced by [MSNovelist]({{ "/methods-background/#MSNovelist" | relative_url }}), with each generated structure tagged with a `de novo` label. 
If a generated structure is also found in the structure databases, the corresponding [labels](#db-labels) will be added alongside the `de novo` label. 
For instance, in the example screenshot above, generated structures 1-4 are generated by MSNovelist, but also exist in the structure databases. Structure 5 was only generated *de novo* by MSNovelist.

By default, structure candidates from the structure databases that have NOT been 
generated by MSNovelist are also displayed. You can hide those structures by toggling 
the left button in the top right corner.


### Substructure Annotation view {#substructure-tab}

The `Substructure Annotations` tab visualizes the direct connection between the input MS/MS spectrum and the CSI:FingerID structure candidates.
The table at the top <span style="color:#d40f57">**[1]**</span> displays all structure candidates for a given query (as listed in the `Structures` tab), and all structure candidates generated
by MSNovelist. You can hide/unhide MSNovelist-generated structures by toggling 
the left button in the top right corner <span style="color:#d40f57">**[2]**</span>.
When you select a structure from the list, the lower part of the view shows the fragmentation spectrum on the left <span style="color:#d40f57">**[3]**</span> and the selected structure candidate on the right <span style="color:#d40f57">**[4]**</span>. 

{% capture fig_img %}
![Foo]({{ "/assets/images/substructures-view.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Substructure annotation view.</figcaption>
</figure>

Peaks in the fragmentation spectrum are color-coded as follows:
- **Black peaks:** These peaks are not used to explain the molecular formula of the candidate and are not part of the fragmentation tree (similar to the spectrum in the [Formulas view](#formulas-tab). Typically, these peaks are considered noise or are not explainable by the precursor ion's molecular formula.
- **Blue peaks:** These peaks are part of the fragmentation tree and thus explain the molecular formula of the candidate. However, they do not have a substructure associated to them.
- **Purple peaks:** These peaks explain the molecular formula of the candidate, AND are associated to a substructure of the candidate structure. Substructures are generated combinatorially and then scored against the peaks. By clicking on the peak, the highest scoring substructure for this peak will be highlighted within the structure. Pink bonds indicate the fragmentation that would have occurred to generate this fragment.

You can navigate through the peaks using left-click or the arrow keys.

### Library Matches view {#library-matches-tab}

{% capture fig_img %}
![Foo]({{ "/assets/images/library-matches-view.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Spectral library matches tab.</figcaption>
</figure>

The `Library Matches` tab displays the spectral library matches for the measured query spectrum against a reference library. If you have multiple MS2 spectra (with different collision energies) for a feature, the best matching spectrum is shown by default. The `Similarity Score` and number of `Shared Peaks` in the list is given for this spectrum <span style="color:#d40f57">**[1]**</span>. You can switch to other MS2 spectra to examine their mirror plots as well <span style="color:#d40f57">**[2]**</span>.

To zoom into the spectrum, hold the right mouse button and drag to select an area, or scroll while hovering over an axis.

For more information on spectral library searches in SIRIUS, please refer to the sections on [spectral library matching]({{ "/methods-background/#spectral-library-search" | relative_url }}) and [Import of Custom Structure and Spectra Databases](#custom-database-import).

## Data export (Summaries and FBMN export) {#data-export}

Analysis results can be exported using the `Summaries` button in the top left tool bar. 

<img src="{{ "/assets/images/export.png" | relative_url }}" width="400">
<img src="{{ "/assets/images/write-summary2.png" | relative_url }}" width="400">

[Summary files]({{ "/io/#summary-files" | relative_url }}) include five types of data:
- [formula annotation summaries]({{ "/io/#molecular-formula-summary" | relative_url }})
- [CANOPUS summaries]({{ "/io/#CANOPUS-summary" | relative_url }})
- [structure database search summaries]({{ "/io/#structure-summary" | relative_url }})
- [MSNovelist summaries]({{ "/io/#structure-summary" | relative_url }})
- spectral library matching summaries.

By default, only the top hit is exported for each feature (`Top Hits (recommended)`). You can use the drop down menu to export `All Hits`, `Top k Hits`, or `Top Hits with Adducts` instead. Learn more about the different [export options here]({{"/io/#summary-files" | relative_url }}). 

You can export the files in TSV, CSV, ZIP or XLSX format. You might want to use `Quote strings` to quote all string values.

In addition, you can export a `Feature quality summary`, with [feature quality]({{"/gui/#lcms-tab" | relative_url}}) values of different categories for all features, as well as a `ChemVista summary` file
which can be imported directly to the [Agilent ChemVista](https://www.agilent.com/en/product/software-informatics/mass-spectrometry-software/library-management) software.


### Feature based molecular networking (FBMN) export {#FBMN-export}

TODO

## Settings {#settings}

You can access the settings dialog by clicking the `Settings` button at the top right of the user interface.

<img src="{{ "/assets/images/settings.png" | relative_url }}">

**General settings:**
- `UI Theme`: Choose your preferred display mode to reduce eye strain (requires restart).
- `Scaling Factor`: Adjust the size of the GUI by the selected factor (requires restart).
- `Confidence score display mode`: Sets the mode for displaying the [confidence score]({{ "/methods-background/#confidence-score-modes" | relative_url }}) (either `approximate` or `exact`). 
- `Allowed solvers`: Select the [ILP solver]({{ "/install/#ILP-solvers" | relative_url }}) for SIRIUS to use in
        fragmentation tree computation. GLPK is free, while Gurobi is
        commercial but offers a free academic license.
- `Database cache`: Specifies the location of the cache directory. CSI:FingerID
        downloads candidate structures from our server and caches them
        for faster retrieval.
- `REST API`: Use the button to open the API in your browser. The REST API provides the full functionality of SIRIUS and its web services as background service. It is intended as entry-point for scripting languages and software integration SDKs.

**Adduct settings:**
Add or remove custom adducts for positive and negative ion modes.

**Network settings:**
SIRIUS supports using a proxy server to access our webservices by changing `Use Proxy Server` from `NONE` to `SIRIUS` and entering all required information. Your configuration will be tested when you click the save button.

## Webservice {#webservice}

The webservice connection check dialog, accessible via the `Webservice` button in  the top right, helps diagnose any connection issues.

<img src="{{ "/assets/images/connectionCheck.png" | relative_url }}" height="500" width="500">


Green checkmarks or red crosses indicate whether you are successfully connected to the internet, login server, license server, and web service.
It also provides information about your account's subscription status, showing whether a valid subscription is linked to the account you're currently logged into.
If there are any connection or licensing problems, detailed descriptions and potential solutions will be provided in the description box.
