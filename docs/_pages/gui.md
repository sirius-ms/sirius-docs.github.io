---
permalink: /gui/
title: "Graphical User Interface"
---

## Overview

SIRIUS 6 comes equipped with a comprehensive Graphical User Interface. Here's a breakdown of the key components and their functions.

{% capture fig_img %}
![Foo]({{ "/assets/images/gui_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>SIRIUS main application window.</figcaption>
</figure>

- The **toolbar <span style="color:red">[1-5]</span>** at the top of the screen provides quick access to the essential tools: 
The first button group <span style="color:red">[1]</span> on the left is for managing your project spaces, including creating, opening and saving them.
The second button group <span style="color:red">[2]</span> facilitates
[importing](#data-import) single features or data containing multiple features into
your project space. The third button group <span style="color:red">[3]</span> handles exporting data, e.g. for [GNPS FBMN](https://doi.org/10.1038/s41592-020-0933-6) analysis
or generating [project space summaries]({{ "/io/#sirius-project-space" | relative_url }}). 
The fourth button group <span style="color:red">[4]</span> is dedicated to computations, including [starting the computations](#compute-dialog), getting the job view and [importing custom databases](#import-of-custom-structure-and-spectra-databases).  
The last button group on the right <span style="color:red">[5]</span> provides access  access to the log, settings, webservice information, and account details. Here you can also find a link to the online documentation (`Help`) as well as information on software licence and related publications (`About`).

- The **feature list <span style="color:red">[6]</span>** on the left side contains all imported (aligned) features. 
A *feature* contains all MS and MS/MS spectra corresponding to a measured aligned
feature. For each feature, adduct type, precursor mass, retention time and confidence score (if computed) are shown in this panel. 

- The **result view <span style="color:red">[8]</span>** is displayed to the right of the feature list and allows users to [examine different result views](#visualize-results). The tab selector <span style="color:red">[7]</span> at the top of this panel lets you switch between these views, offering various perspectives on your data.

- The **bottom information bar <span style="color:red">[9]</span>** provides details about your license status for the webservice-based structure elucidation tools,  
the number of computed features and feature limits.  

## Data import

You can import `.ms`, `.mgf`, Agilent's `.cef`, `.mzml` and `.mzxml` files using the `Import` button or by drag-and-drop. 
SIRIUS will automatically extract all relevant attributes (such as MS level, ionization, and precursor mass)
from the file. 
When importing multiple `.mzml` (or `.mzxml`) at once, SIRIUS will ask you if it should align them. 

For more information on supported file formats, refer to the [Input]({{ "/io/#input/" | relative_url }}) section.

[//]: # (## Sorting features, filtering features and changing displayed confidence score mode)

### Sort and filter

To sort the feature list, use right-click to open the <span style="color:red">[sort]</span> dialog below.

<img src="{{ "/assets/images/sort_filter.png" | relative_url }}" height="300" width="300">

You can sort aligned features by retention time (RT), mass, name, ID or confidence score (if available). At the bottom of
this dialog, you can select the confidence mode (approximate or exact) to be displayed and 
used for sorting. For more details, see the [confidence modes]({{ "/advanced-background-information/#confidence-score-modes" | relative_url }}) section.

The displayed feature list is already filtered by quality, i.e., features containing only MS1 spectra, features with bad quality and multimeric features are hidden. Use the switches on top of the feature list to unhide. 

The feature list can further be refined by clicking the filter button (three dots to the right of the search field) to open the filter dialog:

<img src="{{ "/assets/images/filter_collage.png" | relative_url }}">

- In the `General` tab (left), aligned features can be filtered by mass range, retention time range, and confidence score range, as well as by detected lipid classes. You can also choose to invert the filter, and whether you want to delete all non-matching compounds. 
- In the `Data Quality` tab (middle), you can refine the preset quality filter.
- In the `Results` tab (right), you can filter for specific element constraints in either the neutral molecular formula or precursor formula,
as well as for specific detected adducts. If structure database results are available, you can filter for hits in specific structure databases.  `Candidates to check` allows you to specify the number of top candidates to consider.


## Computing results
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

### Compute dialog

Starting with SIRIUS 6, the compute dialog has been streamlined to improve clarity by displaying only those settings that are essential for any type of analysis. Use the `Show advanced settings` button at the bottom <span style="color:red">[7]</span> to include additional settings that are relevant for specific use cases or to set limits for computation times.

{% capture fig_img %}
![Foo]({{ "/assets/images/compute_basic_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Batch compute dialog.</figcaption>
</figure>

The compute dialog is divided into five subtools: 
[SIRIUS molecular formula annotation](#identifying-molecular-formulas-with-sirius) <span style="color:red">[1]</span>, 
[ZODIAC](#improve-molecular-formula-ranking-with-zodiac) <span style="color:red">[2]</span>, 
[CSI:FingerID fingerprint prediction with CANOPUS](#predicting-molecular-fingerprints-with-csifingerid-and-compound-classes-with-canopus) <span style="color:red">[3]</span>, 
[CSI:FingerID structure database search](#identifying-the-molecular-structure-with-the-csifingerid-tool) <span style="color:red">[4]</span> and 
[MSNovelist](#generating-de-novo-structure-candidates-with-msnovelist) <span style="color:red">[5]</span>. As of SIRIUS 6, CANOPUS is automatically executed together with the fingerprint prediction. 
**Subtools can be selected individually or combined, but note that the selection must align with a [valid SIRIUS workflow]({{ "/advanced-background-information/#sirius-workflows" | relative_url }}).**
For example, you cannot search structure databases without predicting fingerprints first. 

If the `Recompute already computed tasks?` checkbox <span style="color:red">[6]</span> is checked, all previously existing results for the selected features in the current project space will be invalidated and overwritten to execute the newly selected workflow. Additional parameters for specific subtools can be displayed using the `Show advanced settings` button  <span style="color:red">[7]</span>. 
To easily convert the current workflow selections into a CLI command, use the `Show Command` button <span style="color:red">[8]</span> at the bottom right.

### Spectral library matching

If imported spectral libraries are available, SIRIUS will automatically perform spectral matching. This process runs in the background without the need for any addiotional parameters. For more information, refer to the [Spectral library matching]({{ "/advanced-background-information/#spectral-library-matching" | relative_url }}) section.


### Identifying molecular formulas with SIRIUS

To identify molecular formulas using SIRIUS, you can 
set general parameters <span style="color:red">[A]</span>, 
specify fallback adducts <span style="color:red">[B]</span>, and 
choose the appropriate molecular formula annotation strategy <span style="color:red">[C]</span>.

{% capture fig_img %}
![Foo]({{ "/assets/images/formula_basic_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Molecular formula annotation compute dialog.</figcaption>
</figure>

- **General settings <span style="color:red">[A]</span>:** 
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

- **Fallback Adducts <span style="color:red">[B]</span>:**
  If no adducts are detected during SIRIUS import or prior external annotation, you can specify fallback adducts. 
  SIRIUS will consider adducts from this list and enforce them if the `enforce` option is selected.

- **Molecular Formula Generation <span style="color:red">[C]</span>:**
Selecting an appropriate molecular formula annotation strategy is crucial for a successful SIRIUS analysis, as it will significantly impact subsequent steps. Parameters for the different strategies are explained in the following. Before selecting a strategy, it is important to understand the differences between the [molecular formula annotation strategies]({{ "/advanced-background-information/#molecular-formula-annotation-strategies" | relative_url }}) to choose the most appropriate one for your analysis.

**[*De novo* + bottom up search]({{"/advanced-background-information/#de-novo--bottom-up" | relative_url }}) is recommended for generic applications.**

{% capture fig_img %}
![Foo]({{ "/assets/images/formula_combined_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Settings for bottom up + de novo search.</figcaption>
</figure>

 You can configure
the m/z threshold <span style="color:red">[1]</span> below which *de novo* molecular formula annotation will be performed alongside the bottom up search.

The element filter <span style="color:red">[2]</span> can be applied either solely to the *de novo* annotations or to the bottom-up search as well.
`Allowed elements` specifies the elements in the element set and their upper and lower limits. `Autodetect` specifies the elements
for which SIRIUS will automatically detect the presence, absence and quantity from the input data (requires MS1 spectra). The element set can
be adjusted using the `...` button. Before making significant changes to the element set, please consider the potential impact on running time and result quality. 

**[De novo only]({{"/advanced-background-information/#de-novo-only" | relative_url }}) is recommended for discovering "unknown unknowns".**

{% capture fig_img %}
![Foo]({{ "/assets/images/formula_denovo_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Settings for de novo only annotation.</figcaption>
</figure>

Here, the expected element set needs to be well-defined using the `Element filter` settings <span style="color:red">[1]</span>. 
`Allowed elements` and `Autodetect` elements can again be adjusted using the `...` button, with keeping in mind the impact on running time and result quality. 

**[Database search]({{"/advanced-background-information/#database-search-only" | relative_url }}) is recommended for known compounds and extremely fast computation times.**

{% capture fig_img %}
![Foo]({{ "/assets/images/formula_dbsearch_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Settings for formula database search.</figcaption>
</figure>

Choose the databases <span style="color:red">[1]</span> to be used for molecular formula annotation. Per default, the [*biomolecule structure databases*]({{ "/advanced-background-information/#molecular-structures" | relative_url }}) are selected.
You can restore this default selection using the `bio` button.

Applying an element filter <span style="color:red">[2]</span> is not mandatory for formula database search, but it can be used to narrow down molecular formula candidates. `Allowed elements` and `Autodetect` elements can again be adjusted using the `...` button.

**[Bottom up search only]({{"/advanced-background-information/#bottom-up-only" | relative_url }}) can be used for a slight speed increase compared to the recommended combined approach.**

{% capture fig_img %}
![Foo]({{ "/assets/images/formula_bottomup_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Settings for bottom up search only.</figcaption>
</figure>

Also for bottom up search, applying an element filter <span style="color:red">[1]</span> is not mandatory, but can be used to narrow down molecular formula candidates. `Allowed elements` and `Autodetect` elements can again be adjusted using the `...` button.

#### Advanced settings for molecular formula annotation

{% capture fig_img %}
![Foo]({{ "/assets/images/formula_advanced_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Advanced parameters for molecular formula annotation.</figcaption>
</figure>

- By default, molecular formula candidates whose theoretical isotope     
  patterns deviate significantly from the measured isotope pattern are discarded. 
  You can disable this setting <span style="color:red">[1]</span>
  if you suspect poor quality isotope patterns in the input data.
- If isotopic peaks are present in the input MS2 spectrum, they can 
  either be used for scoring (`SCORE`) or be ignored (`IGNORE`) <span style="color:red">[2]</span>.
- You can select the number of molecular formula candidates that will be 
  saved <span style="color:red">[3]</span>.
- Specify the minimum number of molecular formula candidates to store 
  for each ionization state, even if they are not among the top n candidates <span style="color:red">[4]</span>.
- Set a time limit (in seconds) for computing the fragmentation tree for 
  a single molecular formula candidate 
  <span style="color:red">[5]</span>. Set to 0 to disable the limit.
- Set a total time limit (in seconds) for computing the fragmentation 
  trees for all molecular formula candidates of a feature 
  <span style="color:red">[6]</span>. Set to 0 to disable the limit.
- For higher mass compounds, SIRIUS can compute fragmentation trees 
  heuristically instead of exactly. This heuristic method can be used to pre-rank molecular formula candidates, with exact trees 
  computed only for the top candidates. Set the m/z value above which this approach will be applied <span style="color:red">[7]</span>. 
- For very high masses, exact solutions may be impractical, and only
  heuristic trees should be computed. Set the m/z value above which trees will exclusively be computed using the heuristic <span style="color:red">[8]</span>. 

### Improve molecular formula ranking with ZODIAC

ZODIAC enhances *de novo* molecular formula annotation for complete biological datasets, that is high-resolution, high-mass-accuracy LC-MS/MS runs. It refines the ranking of molecular formula candidates by analyzing similarities among features in the dataset, using fragmentation trees as input.

To use ZODIAC, select both SIRIUS and ZODIAC in the compute panel. This is only possible for [batch computation](#computing-results). 
To increase the chance of the correct molecular formula candidate to be in the result list, increase the number of reported candidates for SIRIUS.

For more details, visit the [Zodiac release page](https://bio.informatik.uni-jena.de/software/zodiac/).

#### Advanced settings for ZODIAC

These parameters are very advanced and require a thorough understanding of ZODIAC and its underlying Gibbs sampler.

{% capture fig_img %}
![Foo]({{ "/assets/images/zodiac_advanced_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Advanced parameters for ZODIAC molecular formula annotation.</figcaption>
</figure>

- Specify the maximum number of candidate molecular formulas considered  
  for features with m/z lower than 300. 
  <span style="color:red">[1]</span>
- Specify the maximum number of candidate molecular formulas considered 
  for features with m/z higher than 800.
  <span style="color:red">[2]</span>
- Enable or disable the 2-step approach, where higher quality features 
  are processed first, followed by lower quality features second.
  <span style="color:red">[3]</span>
- Set the threshold for the ratio of edges of the complete network to be ignored.
  <span style="color:red">[4]</span>
- Specify the minimum number of connections required for each candidate.
  <span style="color:red">[5]</span>


### Predicting molecular fingerprints with CSI:FingerID and compound classes with CANOPUS

After computing the fragmentation trees, you can predict [molecular fingerprints]({{ "/advanced-background-information/#molecular-fingerprints" | relative_url }}) 
and [CANOPUS compound classes]({{ "/advanced-background-information/#compound-classes" | relative_url }}).
These predictions can then be used to search structure databases and/or to predict novel structures with [MSNovelist]({{ "/advanced-background-information/#msnovelist" | relative_url }}).
If `Score threshold` is selected, fingerprints are only predicted for the top-scoring fragmentation trees (molecular formulas). This is recommended and should only be disabled if you need to examine 
the fingerprint of a lower-scoring molecular formula.

CANOPUS predicts [ClassyFire](http://classyfire.wishartlab.com/) compound classes from the molecular fingerprint, without using any structure database. Classes are predicted for all features whose
fragmentation tree contains at least three fragments, including features with no matching structure candidates in the
database. There are no parameters to set. Compound classes are predicted separately for each
molecular formula.

For more details, visit the [CANOPUS release page](https://bio.informatik.uni-jena.de/software/canopus/).

### Identifying the molecular structure with CSI:FingerID

CSI:FingerID facilitates the identification of molecular structures by matching 
predicted molecular fingerprints against database structures. SIRIUS ships with a wide range of built-in databases
(see [Molecular structures]({{ "/advanced-background-information/#molecular-structures" | relative_url }})). 
Additionally, users can enhance the search capabilities by adding their own structures as a "custom database" (see [Import of custom structure and spectra databases]({{ "/gui/#Import-of-custom-structure-and-spectra-databases" | relative_url }})) which can then be searched alongside
the existing databases.

{% capture fig_img %}
![Foo]({{ "/assets/images/structure_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Parameters for structure database search.</figcaption>
</figure>

**Expansive search:**
Structure database search is conducted within the user-selected databases. 
You can choose to use PubChem as a fallback database <span style="color:red">[1]</span> in case it contains a hit with higher confidence than
those found in the selected databases.
For a detailed explanation, please refer to the 
[Expansive search]({{ "/advanced-background-information/#expansive-search-structure-database-search-with-fallback" | relative_url }}) section. 
The `Confidence mode` controls whether the approximate or exact [confidence mode]({{ "/advanced-background-information/#confidence-score-modes" | relative_url }}) is used to asses if a
hit in PubChem is more reliable than the hits in the selected databases.

Per default the structure databases constituting the "bio" database are selected <span style="color:red">[2]</span>. De-select `PubChem as fallback` to fully search in PubChem. 
If database search was used for [molecular formula identification](#identifying-molecular-formulas-with-sirius), the same databases are selected here. 
You can revert to the default selection by clicking `bio`. If any custom database was loaded, 
they can be selected here as well.


### Generating *de novo* structure candidates with MSNovelist

In some cases, it is necessary to go beyond the limits of structure database search.
To address this, SIRIUS 6 newly offers *de novo* generation of candidate structures through [MSNovelist]({{ "/advanced-background-information/#MSNovelist" | relative_url }}), in addition to predicting molecular fingerprints and compound classes, as well as searching in custom databases.

Be aware that the likelihood of any *de novo* generation method performing well for truly novel compounds is very low. Therefore, the results from MSNovelist should rather be considered as suggestion or starting point
for semi-manual analysis of compounds that cannot be elucidated otherwise. 

**MSNovelist will significantly slow down your SIRIUS workflow; use with caution.**

#### Import of custom structure and spectra databases

Custom structure databases and spectral libraries can be added via the `Databases` dialog, accessible via the
top center of the GUI ribbon.
Starting with version 6.0, SIRIUS supports the import of spectral libraries. 
The supported import formats for spectral
data are .ms, .mgf, .msp, .mat, .txt (MassBank), .mb, .json (GNPS, MoNA). Spectra must be annotated with a structure and must be centroided.
Custom structures can be used in [structure database search](#identifying-the-molecular-structure-with-csifingerid). Imported spectra will be used for [spectral library matching]({{ "/advanced-background-information/#spectral-library-matching" | relative_url }}).

{% capture fig_img %}
![Foo]({{ "/assets/images/customDbs_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Custom database dialog.</figcaption>
</figure>

Custom databases are stored as files with the `.siriusdb` extension. 
If you already have a database with this format on your local machine, you can add it to SIRIUS by clicking the 
`Add existing Database` button on the bottom right. To create a new custom database,
click the `Create custom Database` button.
Imported databases can be deleted or modified using the `-` or pencil icon button, respectively. 

{% capture fig_img %}
![Foo]({{ "/assets/images/customDbs_import_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Custom database import dialog.</figcaption>
</figure>

To create a custom database, enter a name for the database <span style="color:red">[1]</span> (maximum length: 15 characters), which will be displayed in the structure database search dialog. 
Specify the file name for the database, ensuring it ends with `.siriusdb` <span style="color:red">[2]</span>. 
Choose be any valid, writeable path on your local machine to store the database <span style="color:red">[3]</span>.
Adjust the buffer size <span style="color:red">[4]</span> to control how many structures or spectra should be kept in memory. This can be increased when importing large files on a faster machine.
Drag and drop files or directories containing structure/spectra files to the input area <span style="color:red">[5]</span>, or use the `+` button to browse your file system.

For more details on custom database import and supported file formats, please see the [Custom database tool]({{ "/cli-standalone/#custom-database-tool" | relative_url }}) section.

**Please note that you have to be logged in to your SIRIUS account to import custom databases.**

#### COSMIC - confidence values for CSI:FingerID searches
COSMIC confidence scores are calculated automatically and without requiring any parameters every time a CSI:FingerID search is performed. 
COSMIC scores are displayed in the feature list on the left. Starting with SIRIUS 6, confidence scores
are computed in both `exact` and `approximate` mode. 

For more details, 
see the [COSMIC]({{ "/advanced-background-information/#cosmic---confidence-for-small-molecule-identifications"  | relative_url }}) section or
visit the [COSMIC release page](https://bio.informatik.uni-jena.de/software/cosmic/).


## Visualization of the results {#visualize-results}
The feature provides not only information about the input and compute state, it also displays the COSMIC
confidence score for the top CSI:FingerID hit.

**For each feature, different result views can be displayed in the result panel:**
- The [LC-MS view](#lcms-tab) displays the chromatographic feature alignment as well as a quality assessment of the spectra. This tab is only in use for mzML and mzXML inputs.
- The [Formulas view](#formulas-tab) displays the results from the molecular formula identification.
- The [Predicted Fingerprints view]() contains information about
the molecular properties of the molecular fingerprint predicted by CSI:FingerID.
- The [Compound Classes view]() shows the [Classyfire](http://classyfire.wishartlab.com/) classes predicted by CANOPUS.
- The [Structures view]() displays results from the CSI:FingerID structure search.
- The [De Novo Structures view]() displays MSNovelist-generated structure candidates for the current query.
- The [Substructure Annotations view]() shows possible substructures connected to
the peaks of the MS/MS spectrumfor each candidate.
- The [Library Matches view]() shows matches to reference spectra if a spectral library was imported.


### LC-MS tab {#lcms-tab}

The `LC-MS` tab is empty when no LC-MS data (`.mzML` or `.mzXML`) was imported, i.e. if the data is imported as `.mgf`, `.ms` or similar file formats, no LC-MS information is available. This is also the case when LC-MS data has been processed with OpenMS or MZMine and then imported to SIRIUS.

{% capture fig_img %}
![Foo]({{ "/assets/images/lcms.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Overview tab.</figcaption>
</figure>

The LC-MS tab displays the ion chromatogram of a feature (in blue), including its isotope peaks, possible in-source fragments (in brown), as well as detected adducts (in green) for each input file in which the feature was detected. Retention times are always given in minutes. The *extended ion chromatogram* (gray, dashed) is the mass trace that is not part of the detected peak (e.g., a second ion with same mass or just background noise with same mass). In case MS/MS data of the feature was extracted from the selected LC-MS input file, a black arrow marks the retention time at which the MS/MS was shot. A gray dashed line marks the *noise level*; its exact computation may varies from version to version, but it is related to the median intensity of all peaks in the MS scan. Two gray vertical dashed lines mark the median and weighted average retention time of the feature across all input LC-MS data files.

On the right, there is a basic quality assessment panel. It can be used to preemptively get an idea on overall quality of the MS and MS/MS of the feature.


### Formulas tab {#formulas-tab}

The `Formulas` tab displays the molecular formula candidate list <span style="color:red">[1]</span>, the mass spectra <span style="color:red">[2]</span> and the fragmentation tree <span style="color:red">[3]</span> of the selected feature. 

{% capture fig_img %}
![Foo]({{ "/assets/images/formulas-view.jpg" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Formulas tab.</figcaption>
</figure>


- **Molecular formula candidate list** <span style="color:red">[1]</span>: 
  The candidates are ranked and ordered by the SIRIUS score, which is a combination of the score from the isotope pattern analysis of the MS1 data and the fragmentation tree score from the MS2 data. The molecular formula of the best candidate structure found by CSI:FingerID is highlighted in green (and does not necessarily have to be the candidate with the best SIRIUS score). 
  
  The `Isotope Score` and `Tree Score` are *logarithms* of maximum likelihoods (probability that this molecular formula would produce the observed data). The the `Sirius Score` is the posterior probability of the candidate molecular formula (these probabilities sum to one). While a higher posterior probability for the top hit might suggest that this molecular formula is more likely to be correct, **it is important to note that a posterior probability of 90% does not mean there is a 90% chance that the molecular formula identification is correct!** The displayed probabilities are neither q-values nor Posterior Error Probabilities. 
  The length of the bars for all score columns also correspond to logarithms of maximum likelihoods 
  
  Per default, the candidates are ordered by SIRIUS score but can be sorted by any other column.  

- **Mass spectra** <span style="color:red">[2]</span>: In this panel you can switch between the MS1 spectrum, the MS1 isotope pattern mirror plot or the MS2 spectra <span style="color:red">[a]</span>. For MS2 spectra, per default a merged spectrum is displayed, but you can also choose to view individual spectra <span style="color:red">[b]</span>. To zoom, hold the right mouse button and drag to select an area, or scroll while hovering over an axis. In the MS2 view, peaks annotated by the fragmentation tree are highlighted in green, while those identified as noise are colored black. Hovering over a peak will display its detailed annotation <span style="color:red">[c]</span>, and clicking on a green peak will highlight the corresponding node in the fragmentation tree. 
Spectra views can be exported using the top right export button <span style="color:red">[d]</span>.

- **Fragmentation tree** <span style="color:red">[3]</span>: In the computed
  [fragmentation tree]({{ "/advanced-background-information/#fragmentation-trees" | relative_url }}), each node assigns a molecular formula to a peak in the (merged) MS2 spectrum. 
  Each edge is a hypothetical fragmentation reaction. You can customize the tree's appearance by selecting different node styles and color schemes.

  The tree can be exported as SVG or PDF vector graphics.
  Alternatively, the DOT file format provides a text-based description of the
  tree, which can be rendered externally using tools like Graphviz to convert
  DOT files into image formats such as PDF, SVG, or PNG. 
  The JSON format offers a machine-readable representation of the
  tree. For instructions on exporting fragmentation trees from the command line, see the [Fragmentation tree export tool]({{ "/cli-standalone/#ftree-export" | relative_url }}).

### Predicted Fingerprints tab 
Even if CSI:FingerID does not identify the correct structure — in
particular if the correct structure is not present in any structure database —
you can still get valuable information about the structure by examining the predicted fingerprint. 
The `Predicted Fingerprints` tab displays a list of all molecular properties that make up the predicted fingerprint. When you select a molecular property, examples related to that property are shown below the list.

{% capture fig_img %}
![Foo]({{ "/assets/images/fingerprint.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Fingerprint view.</figcaption>
</figure>

### Compound Classes tab

The `Compound Classes` tab visualizes the [CANOPUS](({ "/advanced-background-information/#compound-classes" | relative_url })) compound class predictions in a table format. Each row in the table represents
one compound class. The *posterior probability* indicates the likelihood that 
the measured spectrum, given the chosen molecular formula, 
belongs to that class.  The other columns contain all related information from the
[ClassyFire](http://classyfire.wishartlab.com/) ontology.

Above the table are two lists: **main classes** and **alternative classes**. The main class of a measurement is the
*most specific* compound class from all compound classes with posterior probability above 50%. The **main classes** list
contains the main class, as well as its ancestors in the Classyfire ontology. The **alternative classes** list contains
all over classes with posterior probability above 50%.

{% capture fig_img %}
![Foo]({{ "/assets/images/canopus.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>CANOPUS view.</figcaption>
</figure>

* (1) The most informative class (light green), and its ancestor classes (light blue).
* (2) Alternative classes. In the ClassyFire chemontology, every compound is assigned to multiple classes. In this example, the compound kaempferol is a flavonoid, but also a benzenoid.
* (3) The table lists all ClassyFire classes, with description parent class and so on. The colored bar denotes the predicted probability for this class. Only classes with probability above 0.5 are listed in (1) and (2).

Starting from SIRIUS 5, this tab also contains the predicted Natural Product classes.

### Structures tab
{% capture fig_img %}
![Foo]({{ "/assets/images/structures_exact.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>structure annotation tab tab.</figcaption>
</figure>

This tab shows you the candidate structures for the selected molecular
formula ordered by the CSI:FingerID search score. The highest scoring candidate is highlighted
in green. If you have approximate confidence mode enabled, all candidates that are within MCES distance 2
will be highlighted (see [Expansive search]({{ "/advanced-background-information/#Expansive-search-(structure-database-search-with-fallback)" | relative_url }})).
If you want to filter the candidate list by a certain database (e.g. only compounds from KEGG
and BioCyc) you can press the filter button (top left). A menu will open displaying
all available databases. Only candidates will be displayed that are
enabled in this filter menu. If you want to see
only compounds from KEGG and BioCyc you have to check only KEGG and BioCyc.

The blue and red squares are a visualization of the CSI:FingerID
predictions and scoring. All blue squares represent molecular structures
that are found in the candidate structure and are predicted by
CSI:FingerID to be present in the measured feature. The more intense
the color of the square the higher is the predicted probability for the
presence of this substructure. The larger the square the more reliable
is the predictor. The red squares, however, represent structures that
are predicted to be absent but are, nevertheless, found in the candidate
structure. Again, as more intense the square as higher the predicted
probability that this structure should be absent. Therefore, a lot of
large intense blue squares and as few as possible large intense red
squares are a good indication for a correct prediction.

When hovering over these squares the corresponding
description of the molecular structure (usually a SMART expression) is
displayed. When clicking on one of these squares, the corresponding
atoms in the molecule that belong to this substructure are highlighted.
If the substructure matches several times in the molecule, it is once
highlighted in dark blue while all other matches are highlighted in a
translucent blue.

You can enable filtering by the selected substructure (button with the structure, top left),
to only show candidates that contain the selected substructure.
Further, you can filter the candidate list using a SMARTS pattern or full-text search (top ribbon).

You can open a context menu by right click on a proposed structure. It offers
you to open the compound in PubChem or copy the InChI or InChI key in
your clipboard.

If the structure is contained in any database, a blue or grey label
with the name of this database is displayed below the structure. You can
click on blue labels to open the database entry in your browser
window. Yellow labels indicate that the candidate is contained in the corresponding
custom database. A red label indicates that this candidate is flagged with an unknown database.
This can for example happen when loading results that have been computed with a custom database that 
is not available on the current system. Black labels are just additional information such as if the candidate 
is part of the CSI:FingerID training data.

This tab also includes visualization for the "El Gordo" lipid class annotation functionality. Lipid structures are often extremely similar to each other,
often only differing in the position of the double bonds. These extremely similar structures are often not even differentiable by mass spectrometry at all, which is why the overarching lipid class is shown above the structure candidates. 

If the PubChem fallback was triggered as part of [Expansive search]({{ "/advanced-background-information/#Expansive-search-(structure-database-search-with-fallback)" | relative_url }}), a notification will be displayed below the top ribbon.

{% capture fig_img %}
![Foo]({{ "/assets/images/structures_specMatch.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>structure annotation with spectral library match.</figcaption>
</figure>

If a structure candidate shown in this tab also has a reference spectrum imported via a custom database, the spectral match will be shown.
Clicking on it will show the [spectral matchig tab]({{ "/gui/#library-matches-tab" | relative_url }}).



### De Novo Structure tab

{% capture fig_img %}
![Foo]({{ "/assets/images/msnovelist.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>De novostructure annotation tab tab.</figcaption>
</figure>

This tab shows de novo structure generation results produced by [MSNovelist]({{ "/advanced-background-information/#MSNovelist" | relative_url }}) and tags them with a "de novo" tag. 
If MSNovelist generated structures that are also contained in regular structure databases, the corresponding tags will be added to that structure. 
As an example, generated structures 1-4 in the image above are also present in structure databases, while generated structure 5 is de novo only.

By default, structure candidates present in structure databases but NOT generated by MSNovelist will also be shown. This can be turned off using the first button
on the top right.


### Substructure Annotation tab

In this tab, a direct connection between the input MS/MS spectrum and the CSI:FingerID structure candidates is visualized.
The table in the top part of the view shows all structure candidates for a given query that were also present in the "Structures" tab. Structure candidates generated
by MSNovelist are also shown here. This can be disabled using the first button on the top right.
By selecting a structure, the bottom part of the view shows the fragmentation spectrum on the left, as well as the given structure candidate on the right. 

{% capture fig_img %}
![Foo]({{ "/assets/images/substructure_annotation.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Substructure annotation view.</figcaption>
</figure>

Peaks in the fragmentation spectrum are color coded as follows:

-Black peaks:  Peaks that are not used to explain the molecular formula of the candidate, and are as such not part of the fragmentation tree (just like in the "Formulas" tab). Usually, these peaks can be considered as noise or not explainable by the precursor ions molecular formula.

-Green peaks: Peaks that are used to explain the molecular formula of the candidate, and as such are part of the fragmentation tree (just like in the "Formulas" tab), but do not have a substructure associated to them (see below)

-Purple peaks: Peaks that are used to explain the molecular formula of the candidate, AND can be associated to a specific substrucure of the candidate's structure. Possible substructures are combinatorially generated and then scored against the peaks in the spectrum, with the highest scoring substructure for each peak being displayed on the right. Blue atoms and bonds make the substructure, while red bonds denote the fragmentation that would have needed to occur for that fragment to be formed.

Peaks can be navigated by left-clicking on them, or using the arrow keys.

### Library matches tab

{% capture fig_img %}
![Foo]({{ "/assets/images/library_matches.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Spectral library matches tab.</figcaption>
</figure>

This tab shows spectral library matches for the measured query spectrum against a spectral library. **Query** denotes which measured spectrum
produced the match, in case multiple MS2 were measured for the same feature (in this example it was MS2 spectrum #26). The mirror plot view can be
zoomed by mouse wheel or by holding right click and drag-selecting an area. Please see [Spectral library matching via custom databases]({{ "/advanced-background-information/#Spectral-library-matching-via-custom-databases" | relative_url }}) for background information on
spectral library search in SIRIUS, as well as [Import of custom structure and spectra databases]({{ "/gui/#Import-of-custom-structure-and-spectra-databases" | relative_url }})

## Data export (Summaries and FBMN export)

Summary files containing analysis results can be exported via the "Summaries" button on the top left.

<img src="{{ "/assets/images/summaries.png" | relative_url }}" height="300" width="300">

Summaries will generally include five types: formula annotation summaries, canopus summaries, structure database search summaries, MSNovelist summaries and spectral library matching summaries.
By default, only the top hit is exported, this can be changed by either selecting "All hits" (can produce very large files) or "top k hits". For formula annotation summaries, the user can choose
to additionally export adducts belonging to the top hits. See [Summary files]({{ "/io/#summary-files" | relative_url }})) for a breakdown of the generated summary files. 

### Feature based molecular networking (FBMN) export

TODO


## Settings

The settings dialog can be opened by pressing the "Settings" button on the top right.

<img src="{{ "/assets/images/settings_general.png" | relative_url }}" height="500" width="500">


  - *General settings*
      - *UI Theme:* Choose your favourite display mode for less eye strain (requires restart).
    
      - *Scaling factor:* Increases the size of the GUI by the chosen factor (requires restart).
    
      - *Import data without MS/MS:* If checked, features with no MS/MS data will be imported into SIRIUS. Please note that for
        features with no MS/MS data, only isotope pattern analysis can be performed.
      - *Ignore formulas:* If checked, SIRIUS will ignore any molecular formula annotations that may already be present in the input file.
        This can be useful for evaluation, or in case you don't trust formula annotations added externally.
      - *Confidence score display mode:* Sets the confidence score display mode (either approximate or exact). 
    
      - *Allowed solvers:* Choose the ILP solver for SIRIUS to use for
        fragmentation tree computation. GLPK is free, Gurobi is
        commercial but offers free academic license.
    
      - *Database cache:* location of cache directory. CSI:FingerID
        download candidate structures from our server and caches them
        for faster retrieval.



   - *Adduct settings*

      - Add or remove custom adducts for positive and negative ion mods

    

  - *Proxy and Network settings*
    
      - Sirius supports proxy configuration. It can be enabled by changing
        the proxy configuration from NONE to SIRIUS. If SIRIUS is selected
        it uses the configuration you have specified int the Settings
        -\> Network panel. If NONE is selected Sirius ignores all proxy
        settings.
    
      - Edit the information in the Settings -\> Network panel if you want
        to address CSI:FingerID via a proxy server. Your specified
        configuration will be tested if you hit the save button.

 

## Webservice

The connection check dialog on the top right can help diagnose connection problems.

<img src="{{ "/assets/images/connectionCheck.png" | relative_url }}" height="500" width="500">


Green checkmarks or red crosses will appear depending on if you have connection to the internet, 
login server, license server and web service. Additionally, information on if the account you are 
currently logged in to has a valid subscription attached to it can be found here. Potential connection
or licensing issues will be given in the description box.
