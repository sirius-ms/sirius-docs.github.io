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
feature. For each feature, adduct type, precursor mass, retention time and [confidence score]({{ "/methods-background/#COSMIC" | relative_url }}) (if computed) are shown in this panel. `Confidence (A)` is the [approximate confidence mode]({{ "/methods-background/#confidence-score-modes" | relative_url }}), and `Confidence (E)` the [exact confidence mode]({{ "/methods-background/#confidence-score-modes" | relative_url }}).

- The **result view <span style="color:#d40f57">[8]</span>** is displayed to the right of the feature list and allows users to [examine different result views](#visualize-results). The tab selector <span style="color:#d40f57">**[7]**</span> at the top of this panel lets you switch between these views, offering various perspectives on your data.

- The **bottom information bar <span style="color:#d40f57">[9]</span>** provides details about your license status for the webservice-based structure elucidation tools, 
the number of computed features and feature limits.  

For a quick start, you can also watch our [SIRIUS Short Course Tutorial Series introducing basic elements and functionality of the SIRIUS 6 GUI](https://bright-giant.com/short-course-tutorial-series).

## Account creation {#account-creation}

Open `Account` in the top-right toolbar <span style="color:#d40f57">**[5]**</span>, and click `Create Account` to get to our [user portal](https://portal.bright-giant.com/auth/register/) and [create an account]({{"/account-and-license/#account-creation" | relative_url}}). After logging into the user portal, you can [request a license]({{"/account-and-license/" | relative_url}}).
In the SIRIUS GUI, click `Log in` to enter your account credentials and login.

## Data import {#data-import}

You can import `.ms`, `.mgf`, Agilent's `.cef`, `.mzml` and `.mzxml` files using the `Import` button or by drag-and-drop. 
SIRIUS will automatically extract all relevant attributes (such as MS level, ionization, and precursor mass)
from the file. 
When importing multiple `.mzml` (or `.mzxml`) at once, SIRIUS will ask you if it should align them.
When importing a [peak list file]({{ "/io/#peak-lists" | relative_url }}) containing a molecular formula, select `Ignore Formulas` if you would like to do the molecular formula annotation using SIRIUS.

For more information on supported file formats, refer to the [Input]({{ "/io/#input/" | relative_url }}) section and watch the [Tutorial on Data Import & Preprocessing](https://www.youtube.com/watch?v=Begq5bqg_lw).

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

- In the `Input` tab <span style="color:#d40f57">**[1]**</span>, aligned features can be filtered by mass range and retention time range, as well as by specific detected adducts. 
- In the `Data Quality` tab <span style="color:#d40f57">**[2]**</span>, you can refine the preset quality filter.
- In the `Results` tab <span style="color:#d40f57">**[3]**</span>, you can filter for confidence score range, specific element constraints in either the neutral molecular formula or precursor formula,
 as well as by detected lipid classes. If structure database results are available, you can filter for hits in specific structure databases.  `Candidates to check` allows you to specify the number of top candidates to consider.

For all filters, you can also choose to invert the filter, and whether you want to delete all **non**-matching compounds.

### Import of custom structure and spectra databases {#custom-database-import}

Custom structure databases and spectral libraries can be added via the `Databases` dialog, accessible via the
top center of the GUI ribbon. SIRIUS allows you to import custom structure databases and spectral libraries. Any spectral library you import also functions as a structure database. [Learn more about custom database import and supported file formats here]({{ "/io/#custom-dbs" | relative_url}}).

{% capture fig_img %}
![Foo]({{ "/assets/images/custom-db_full.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Custom database dialog.</figcaption>
</figure>

Custom databases are stored as files with the `.siriusdb` extension. 
If you already have a database with this format on your local machine, you can add it to SIRIUS by clicking the 
`Add existing Database` button <span style="color:#d40f57">**[1]**</span> on the bottom right. To create a new custom database,
click the `Create custom Database` button <span style="color:#d40f57">**[2]**</span>.
You can also download a curated spectral database <span style="color:#d40f57">**[3]**</span> with public spectra from GNPS, MassBank and MsnLib for [spectral library matching](#spectral-library-matching).

To create a custom database, enter a name for the database <span style="color:#d40f57">**[4]**</span> (maximum length: 15 characters) that is used within SIRIUS. 
Specify the file name for the database, ensuring it ends with `.siriusdb` <span style="color:#d40f57">**[5]**</span>. 
Choose a valid, writeable path on your local machine to store the database <span style="color:#d40f57">**[6]**</span>.
Adjust the buffer size <span style="color:#d40f57">**[7]**</span> to control how many structures or spectra should be kept in memory. This can be increased when importing large files on a faster machine.
Drag and drop files or directories containing structure/spectra files to the input area <span style="color:#d40f57">**[8]**</span>, or use the `+` button to browse your file system.

SIRIUS integrates [BioTransformer 3.0](https://biotransformer.ca/) <span style="color:#d40f57">**[9]**</span> ([Wishart  et al., Nucleic Acids Res, 2022](https://doi.org/10.1093/nar/gkac313))  for generating custom transformation product databases. For detailed explanations of all options, please consult the official [BioTransformer documentation](https://biotransformer.ca/help). Here is only a selection:

- `Metabolic Transformation` <span style="color:#d40f57">**[a]**</span>:  Coverage Options: 
  - `Phase 1 (CYP450)`: Focuses on cytochrome P450-mediated transformations. 
  - `EC-based`: Predicts transformations based on Enzyme Commission (EC) numbers. 
  - ...
  - `AllHuman`: Predicts all possible human metabolites from any applicable reaction (oxidation, reduction, deconjugation) at each step.
- `Number of Reaction Iterations` <span style="color:#d40f57">**[c]**</span>: allows you to specify the number of transformation steps for the prediction. It is applicable for EC-based, CYP450, Phase II, and Environmental microbial biotransformers. The default value is typically 1.

Imported databases can be deleted or modified using the `-` <span style="color:#d40f57">**[10]**</span> or pencil <span style="color:#d40f57">**[11]**</span> button, respectively. Databases can also be exported including the generated biotransformations <span style="color:#d40f57">**[12]**</span>.

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

Starting with SIRIUS 6, the compute dialog has been streamlined to improve clarity by displaying only those settings that are essential for any type of analysis. For a video Tutorial, click [here](https://www.youtube.com/watch?v=1WDT6EtWqQs).
Use the `Advanced settings` toggle switch at the top <span style="color:#d40f57">**[1]**</span> 
to include additional settings that are relevant for specific use cases or to set limits for computation times.

{% capture fig_img %}
![Foo]({{ "/assets/images/compute-dialog.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Batch compute dialog.</figcaption>
</figure>

The compute dialog is divided into global settings <span style="color:#d40f57">**[2]**</span> 
and settings for the following subtools: 
- [Spectral Library Search](#spectral-library-matching) <span style="color:#d40f57">**[3]**</span>
- [SIRIUS - Molecular Formula Annotation](#SIRIUS-molecular-formula) <span style="color:#d40f57">**[4]**</span>, 
- [ZODIAC - Network-based Molecular Formula Re-ranking](#ZODIAC-ranking) <span style="color:#d40f57">**[5]**</span>, 
- [CSI:FingerID - Fingerprint Prediction & CANOPUS - Compound Class Prediction](#CSIFingerID-CANOPUS) <span style="color:#d40f57">**[6]**</span>, 
- [CSI:FingerID - Structure Database Search](#CSIFingerID-structure) <span style="color:#d40f57">**[7]**</span> and 
- [MSNovelist - De Novo Structure Generation](#generating-de-novo-structure-candidates-with-msnovelist) <span style="color:#d40f57">**[8]**</span>. 

Databases are selected as part of the global settings to ensure consistency throughout the identification workflow.
As of SIRIUS 6, CANOPUS is automatically executed together with the fingerprint prediction. 
**Subtools can be selected individually or combined, but note that the selection must align with a [valid SIRIUS workflow]({{ "/methods-background/#workflows" | relative_url }}).**
For example, you cannot search structure databases without predicting fingerprints first.
Subtools are automatically enabled/disabled to match the [workflow principles]({{"/cli/#basic-principles" | relative_url }}).
ZODIAC will only be displayed if a sufficient number of features with MS/MS spectra have been selected.

If the `Recompute already computed tasks?` checkbox <span style="color:#d40f57">**[9]**</span> is checked, 
all previously existing results for the selected features in the current project space will be invalidated and overwritten to execute the newly selected workflow. 
Additional parameters for specific subtools can be displayed using the `Advanced settings` toggle switch at the top <span style="color:#d40f57">**[1]**</span>.
To export your selected parameters to the CLI or the API use the `Show Command` and `Show JSON` button at the bottom <span style="color:#d40f57">**[10]**</span>.
You can save your computation setup as a preset to reload for the next computation <span style="color:#d40f57">**[11]**</span>.
Computation will start after pressing the `Compute` button on the bottom right <span style="color:#d40f57">**[12]**</span>.

### Computation presets {#computation-presets}

You can save your computation parameters  in the compute dialog as a preset to reload for the next computation. For editing, you can access the computation presets also via the [settings dialog](#settings).

Three predefined presets are available, which can be loaded, customized, and saved as personalized computation workflows (note that the predefined presets themselves cannot be edited):
- `Default`: This workflow is recommended for general applications. It includes molecular formula annotation, fingerprint prediction, compound class prediction, and structure database search. Molecular formula annotation is performed using the `De novo + bottom-up` approach for comprehensive results.
- `DB-Search`: Designed for users focused solely on structure database hits, this preset greatly reduces the computational effort of molecular formula annotation. Molecular formula annotation only considers formulas contained in databases, with biomolecule structure databases selected by default. Note that confidence scores may be less reliable in this preset due to the absence of the full range of de novo candidates for comparison.
- `MS1`: Specifically for MS1-only data, this preset supports molecular formula annotation exclusively. The De novo approach is recommended here to limit bias by considering all potential candidates. 

Learn more about [molecular formula identification with SIRIUS]({{"/methods-background/#SIRIUS-molecular-formula" | relative_url }}).

### Global Configuration {#global-config}

{% capture fig_img %}
![Foo]({{ "/assets/images/global-config.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Global settings for computation.</figcaption>
</figure>

Set the general parameters that define the characteristics of your mass spectrometry data, particularly concerning the instrument used and the expected mass accuracy <span style="color:#d40f57">**[A]**</span>. In the `Instrument` field, you can choose `Q-TOF`, `Orbitrap` or `FT-ICR`.
The choice of instrument affects only a few parameters,
primarily the allowed mass deviation. If your instrument is not among these options,
select `Q-TOF` as default. You can  change the maximum allowed mass deviation, ensuring that
SIRIUS only considers molecular formulas with mass deviations below
the specified ppm threshold. For masses below 200 Da, the allowed mass deviation is
$(200 \cdot \frac{ppm_{max}}{10^6})$.


The set of **fallback adducts** <span style="color:#d40f57">**[B]**</span> will be used for all features for which no adducts were detected during preprocessing (or prior external annotation). Using the `enforce` option, you can even enforce to consider the selected adducts for all features (in addition to the detected adducts). [The "base ionization" of the detected adduct will be considered by default.]({{"/io/#lcms-runs" | relative_url}}) Refrain from selecting all adducts as fallback adducts, as multiple testing inflates the chance of errors. [Learn more about adduct handling here]({{"/adducts/" | relative_url }}). 

In the **compute dialog for a single compound**, the adduct selection is different (set of **possible adducts**). The detected adducts (or SIRIUS default adducts) are pre-selected. Select the adducts you want to be considered for computation and deselect the adducts you not want to be considered for computation.

Databases <span style="color:#d40f57">**[C]**</span> are selected as part of the global settings to ensure consistency throughout the identification workflow. Imported custom databases are displayed at the top of the list. Per default, the [*biomolecule structure databases*]({{ "/methods-background/#CSIFingerID" | relative_url }}) are selected. You can restore this default selection using the `bio` button.


### Spectral library matching {#spectral-library-matching}

If [imported or downloaded spectral libraries](#custom-database-import) are available, select them in the global settings <span style="color:#d40f57">**[A]**</span>. `Spectral Library Search` will then automatically be activated in the compute dialog <span style="color:#d40f57">**[B]**</span>. SIRIUS supports `Identity Search` <span style="color:#d40f57">**[C]**</span>, meaning that library spectra with the same precursor mass (within a given precursor mass deviation) are matched against the query spectrum, and `Analog Search` <span style="color:#d40f57">**[D]**</span>, meaning that library spectra with different precursor masses are matched against the query spectrum. For more information, refer to the [Spectral library matching]({{ "/methods-background/#spectral-library-search" | relative_url }}) section.


{% capture fig_img %}
![Foo]({{ "/assets/images/compute-dialog-spectral-library-search.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Spectral library matching compute dialog.</figcaption>
</figure>

Since structure database results depend on the selected molecular formula, SIRIUS ensures that molecular structures with a formula corresponding to a good spectral library hit are considered (even if its molecular formula receives a low SIRIUS score). That means that molecular structures of well-matching reference spectra are automatically included in the structure database search.

### Identifying molecular formulas with SIRIUS {#SIRIUS-molecular-formula}

{% capture fig_img %}
![Foo]({{ "/assets/images/SIRIUS-formula.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Molecular formula annotation compute dialog (left) and advanced settings (right).</figcaption>
</figure>

To identify molecular formulas using SIRIUS, choose the appropriate **molecular formula annotation strategy** <span style="color:#d40f57">**[A]**</span>. This is crucial for a successful SIRIUS analysis, as it will significantly impact subsequent steps. Parameters for the different strategies are explained [below](#SIRIUS-strategies). Before selecting a strategy, it is important to understand the differences between the [molecular formula annotation strategies]({{ "/methods-background/#annotation-strategies" | relative_url }}) to choose the most appropriate one for your analysis.
For a video tutorial, click [here](https://www.youtube.com/watch?v=_NmHjru4omg).

#### Advanced settings for molecular formula annotation {#SIRIUS-advanced}

- By default, molecular formula candidates whose theoretical isotope patterns deviate significantly from the measured isotope pattern are discarded. You can disable this setting <span style="color:#d40f57">**[1]**</span> if you suspect poor quality isotope patterns in the input data.
- If isotopic peaks are present in the input MS2 spectrum, they can
  either be used for scoring (`SCORE`) or be ignored (`IGNORE`) <span style="color:#d40f57">**[2]**</span>.
- You can select the number of molecular formula candidates that will be
  saved <span style="color:#d40f57">**[3]**</span>.
- Specify the minimum number of molecular formula candidates to store
  for each ionization state, even if they are not among the top n candidates <span style="color:#d40f57">**[4]**</span>.
- If SIRIUS predicts that the query spectrum may be a lipid, the molecular formula
  according to that prediction can be added and enforced (default) <span style="color:#d40f57">**[5]**</span>.
- Set a time limit (in seconds) for computing the fragmentation tree for
  a single molecular formula candidate
  <span style="color:#d40f57">**[6]**</span>. Set to 0 to disable the limit.
- Set a total time limit (in seconds) for computing the fragmentation
  trees for all molecular formula candidates of a feature
  <span style="color:#d40f57">**[7]**</span>. Set to 0 to disable the limit.
- For higher mass compounds, SIRIUS can compute fragmentation trees
  heuristically instead of exactly. This heuristic method can be used to pre-rank molecular formula candidates, with exact trees
  computed only for the top candidates. Set the m/z value above which this approach will be applied <span style="color:#d40f57">**[8]**</span>.
- For very high masses, exact solutions may be impractical, and only
  heuristic trees should be computed. Set the m/z value above which trees will exclusively be computed using the heuristic <span style="color:#d40f57">**[9]**</span>.


#### Molecular formula annotation strategies {#SIRIUS-strategies}

{% capture fig_img %}
![Foo]({{ "/assets/images/SIRIUS-strategy.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Settings for the different molecular formula annotation strategies.</figcaption>
</figure>

***De novo* + bottom up search <span style="color:#d40f57">[A]</span> is recommended for generic applications.** [Learn more.]({{"/methods-background/#annotation-strategies" | relative_url}})

You can configure
the m/z threshold <span style="color:#d40f57">**[1]**</span> below which *de novo* molecular formula annotation will
be performed alongside the bottom up search.

The element filter  can be applied either solely to the *de novo* annotations or to the bottom-up search as well <span style="color:#d40f57">**[2]**</span>. This filter only refers to the set of `Allowed elements`. You can set upper and lower limits for the elements in this set. The `Autodetect` set <span style="color:#d40f57">**[3]**</span> specifies the elements
for which SIRIUS will automatically detect the presence, absence and quantity from the input data (requires MS1 spectra). This filter is applied to both, *de novo* and bottom-up search. Both sets can
be adjusted using the `...` button <span style="color:#d40f57">**[4]**</span>. Before making significant changes to the element sets, please consider the potential impact on running time and result quality. 

**De novo only <span style="color:#d40f57">[B]</span> is recommended for discovering "unknown unknowns".** [Learn more.]({{"/methods-background/#annotation-strategies" | relative_url}})

Here, the expected element set needs to be well-defined using the `Element filter` settings <span style="color:#d40f57">**[2-4]**</span>. 
`Allowed elements` and `Autodetect` elements can again be adjusted using the `...` button, with keeping in mind the impact on running time and result quality. 

**Bottom up search only <span style="color:#d40f57">[C]</span> can be used for a slight speed increase compared to the recommended combined approach.** [Learn more.]({{"/methods-background/#annotation-strategies" | relative_url}})

Applying an element filter <span style="color:#d40f57">**[5]**</span> is not mandatory for bottom-up search, but can be used to narrow down molecular formula candidates. `Allowed elements` and `Autodetect` elements can again be adjusted using the `...` button <span style="color:#d40f57">**[2-4]**</span>.

**Database search <span style="color:#d40f57">[D]</span> is recommended for known compounds and extremely fast computation times.** [Learn more.]({{"/methods-background/#annotation-strategies" | relative_url}})

Applying an element filter <span style="color:#d40f57">**[5]**</span> is not mandatory for formula database search, but can be used to narrow down molecular formula candidates. `Allowed elements` and `Autodetect` elements can again be adjusted using the `...` button <span style="color:#d40f57">**[2-4]**</span>.

**Specifying a molecular formula (or list of molecular formulas) <span style="color:#d40f57">[E]</span> to run formula annotation.**

If you have imported a [peak list file]({{ "/io/#peak-lists" | relative_url }}) containing a molecular formula, you cannot override this formula using the compute dialog. To avoid this, select `Ignore Formulas` during [import](#data-import).

### Improve molecular formula ranking with ZODIAC {#ZODIAC-ranking}

ZODIAC enhances *de novo* molecular formula annotation for complete biological datasets, that is high-resolution, high-mass-accuracy LC-MS/MS runs. It refines the ranking of molecular formula candidates by analyzing similarities among features in the dataset, using fragmentation trees as input.

To use ZODIAC, select both SIRIUS and ZODIAC in the compute panel. This is only possible for [batch computation](#computing-results). 
To increase the chance of the correct molecular formula candidate to be in the result list, increase the number of reported candidates for SIRIUS.

<span style="color:#d40f57">**!!** Zodiac should not be used for non-biological samples (like standard mixtures) **!!**</span>

For more details, visit the [ZODIAC release page](https://bio.informatik.uni-jena.de/software/zodiac/).

#### Advanced settings for ZODIAC {#ZODIAC-advanced}

These parameters are very advanced and require a thorough understanding of ZODIAC and its underlying Gibbs sampler.

<img src="{{ "/assets/images/zodiac-advanced.png" | relative_url }}" alt="Advanced ZODIAC Settings." width="500">


- Specify the maximum number of candidate molecular formulas considered  
  for features with m/z lower than 300. 
  <span style="color:#d40f57">**[1]**</span>
- Specify the maximum number of candidate molecular formulas considered 
  for features with m/z higher than 800.
  <span style="color:#d40f57">**[2]**</span>
- Enable or disable the 2-step approach, where higher quality features 
  are processed first, followed by lower quality features second.
  <span style="color:#d40f57">**[3]**</span>
- Use identity matches <span style="color:#d40f57">**[4]**</span> from spectral library search with given minimum similarity <span style="color:#d40f57">**[5]**</span> as anchors in the network. 
- Use analog matches <span style="color:#d40f57">**[6]**</span> from spectral library search with given minimum similarity <span style="color:#d40f57">**[7]**</span> and given minimum number of matching peaks <span style="color:#d40f57">**[8]**</span> as anchors in the network.
- Set the threshold for the ratio of edges of the complete network to be ignored.
  <span style="color:#d40f57">**[9]**</span>
- Specify the minimum number of connections required for each candidate.
  <span style="color:#d40f57">**[10]**</span>


### Predicting molecular fingerprints with CSI:FingerID and compound classes with CANOPUS {#CSIFingerID-CANOPUS}

After computing the fragmentation trees, you can predict [molecular fingerprints]({{ "/methods-background/#molecular-fingerprint" | relative_url }}) 
and [CANOPUS compound classes]({{ "/methods-background/#CANOPUS" | relative_url }}).
These predictions can then be used to search structure databases and/or to predict novel structures with [MSNovelist]({{ "/methods-background/#MSNovelist" | relative_url }}).
If `Score threshold` is selected, fingerprints are only predicted for the top-scoring fragmentation trees (molecular formulas). This is recommended and should only be disabled if you need to examine 
the fingerprint of a lower-scoring molecular formula. If ZODIAC scores were calculated, all molecular formulas with a probability ≥0.01 are used. Otherwise, the score threshold is the maximum of "85% of the top SIRIUS score" and "top SIRIUS score - 5". 

CANOPUS predicts [ClassyFire](http://classyfire.wishartlab.com/) compound classes from the molecular fingerprint, without using any structure database. Classes are predicted for all features whose
fragmentation tree contains at least three fragments, including features with no matching structure candidates in the
database. There are no parameters to set. Compound classes are predicted separately for each
molecular formula.

For more details, visit the [CANOPUS release page](https://bio.informatik.uni-jena.de/software/canopus/).

### Identifying the molecular structure with CSI:FingerID {#CSIFingerID-structure}

CSI:FingerID facilitates the identification of molecular structures by matching 
predicted molecular fingerprints against database structures. SIRIUS ships with a wide range of [built-in databases]({{ "/methods-background/#CSIFingerID" | relative_url }}). 
Additionally, users can enhance the search capabilities by adding their own structures as a "custom database" (see [Import of custom structure and spectra databases](#custom-database-import) which can then be searched alongside
the existing databases. If you have imported your own spectral library that should be considered for structure database search, select these libraries (databases) in the [global settings](#global-config).

You can choose to use PubChem as a fallback database (if not selected as search space in the global settings) in case it contains a hit with higher confidence than
those found in the selected databases. For a detailed explanation, please refer to the 
[Methods]({{ "/methods-background/#expansive-search" | relative_url }}) section.
The `Confidence mode` controls whether the approximate (A) or exact (E) [confidence mode]({{ "/methods-background/#confidence-score-modes" | relative_url }}) is used to assess if a
hit in PubChem is more reliable than the hits in the selected databases.

**COSMIC confidence scores** are calculated automatically and without requiring any parameters every time a CSI:FingerID search is performed. 
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
- The [Homologous Series view](#KMD-tab) visualizes groups of related molecules, that share the same basic structure but differ by a consistent building block.

In all views, the top CSI:FingerID hit (as well as "highly similar" compounds in [approximate confidence mode]({{"/methods-background/#confidence-score-modes" | relative_url }})) is highlighted in green.

### LC-MS view {#lcms-tab}

The `LC-MS` tab is hidden when no LC-MS data (`.mzML` or `.mzXML`) was imported, i.e. if the data is imported as `.mgf`, `.ms` or similar file formats, no LC-MS information is available. This is also the case when LC-MS data has been processed with OpenMS or [mzmine]({{"/integrations/#mzmine" | relative_url }}) and then imported to SIRIUS.

{% capture fig_img %}
![Foo]({{ "/assets/images/lcms-view-samples.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>LCMS view displaying the feature alignment and quality assessment.</figcaption>
</figure>

The LC-MS view displays the ion chromatogram of a feature.
You can choose whether to show the sample alignment or the adduct/isotope assignment <span style="color:#d40f57">**[1]**</span>. Retention times are given in seconds.

For the feature alignment, the mass traces of all runs aligned for this feature are displayed in different colors <span style="color:#d40f57">**[2]**</span>. The thick black line is the merged mass trace <span style="color:#d40f57">**[3]**</span>. The bold grey box highlights the selected feature <span style="color:#d40f57">**[4]**</span>. Other nearby features are marked with thin grey boxes and features with low quality are marked with dashed boxes <span style="color:#d40f57">**[5]**</span>. The circles <span style="color:#d40f57">**[6]**</span> indicate where the MS2 spectrum was measured. You can click on a box to zoom into the feature. A gray dashed line <span style="color:#d40f57">**[7]**</span> marks the *noise level*; its exact computation may vary from version to version, but it is related to the median intensity of all peaks in the MS scan.

On the right, there is a basic quality assessment panel <span style="color:#d40f57">**[8]**</span>. It can be used to preemptively get an idea on overall quality of the MS and MS/MS of the feature.

{% capture fig_img %}
![Foo]({{ "/assets/images/lcms-view-adducts.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>LCMS view displaying the adduct/isotope assignment.</figcaption>
</figure>


In the [adduct/isotope assignment view]({{ "/adducts" | relative_url}}), you can find the merged mass trace of the feature (`MAIN`, red) as well as its isotopes (lighter red  / dashed lines) and correlated adducts (`CORRELATED`, additional colors) with their isotopes (dashed lines). Hovering over the edges and nodes of the adduct network provides information about adduct probabilities and correlations.



### Formulas view {#formulas-tab}

The `Formulas` tab displays the molecular formula candidate list <span style="color:#d40f57">**[1]**</span>, the mass spectra <span style="color:#d40f57">**[2]**</span> and the fragmentation tree <span style="color:#d40f57">**[3]**</span> of the selected feature. 

{% capture fig_img %}
![Foo]({{ "/assets/images/formulas-view.png" | relative_url }})
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

- **Mass spectra** <span style="color:#d40f57">**[2]**</span>: In this panel you can switch between the MS1 spectrum, the MS1 isotope pattern mirror plot or the MS2 spectra <span style="color:#d40f57">**[a]**</span>. For MS2 spectra, per default a merged spectrum is displayed, but you can also choose to view individual spectra <span style="color:#d40f57">**[b]**</span>. You can change the scaling of the intensities to different normalisations <span style="color:#d40f57">**[c]**</span>, with either the highest intensity peak in the spectrum normalized to 1, and all other peaks represented as fraction of this maximum (`l∞ (max)`), or the sum of the intensities normalized to 1 (`l1 (sum)`), or the sum of the squares of the intensities normalized to 1 (`l2`, Euclidean normalization). You can also apply a square root transformation to the intensity values (`Sqrt Intensity`) <span style="color:#d40f57">**[d]**</span>, which is particularly useful for spectra containing a few extremely high-intensity peaks alongside many low-intensity peaks that are nevertheless important for analysis. In the MS2 view, peaks annotated by the fragmentation tree are highlighted in blue, while those identified as noise are colored black. Hovering over a peak will display its detailed annotation <span style="color:#d40f57">**[f]**</span>, and clicking on a blue peak will highlight the corresponding node in the fragmentation tree <span style="color:#d40f57">**[i]**</span>. 
Spectra views can be exported using the top right export button <span style="color:#d40f57">**[e]**</span>.

- **Fragmentation tree** <span style="color:#d40f57">**[3]**</span>: In the computed
  [fragmentation tree]({{ "/methods-background/#fragmentation-trees" | relative_url }}), each node assigns a molecular formula to a peak in the (merged) MS2 spectrum. 
  Each edge is a hypothetical fragmentation reaction. You can customize the tree's appearance by selecting different node styles and color schemes <span style="color:#d40f57">**[g]**</span>. The tree can be exported <span style="color:#d40f57">**[h]**</span> as SVG vector graphics or PNG.

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

**Filter results:** To filter the candidate list by a specific database (e.g., only compounds from KEGG
and BioCyc), click the filter button <span style="color:#d40f57">**[2]**</span> in the top left corner. A menu <span style="color:#d40f57">**[3]**</span> will open, displaying
all available databases. Only candidates from the selected databases will be shown. The databases that have been used for the structure database search are highlighted in blue. If PubChem has been used as [fallback]({{"/methods-background/#expansive-search" | realtive_url}}), it will be highlighted in pink.

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
Hovering over the structure displays a zoomed-in preview, providing a more detailed view.

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
Clicking on this match will open an additional window with the [`Substructure Annotations` view]({{ "/gui/#substructure-tab" | relative_url }}) that you can use to compare the annotated structure to identity and analog spectral matches. `...show X more` is the number of identity matches for this feature.

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
the left button in the top right corner. The `De Novo Structures` tab has the same display functionalities (filtering, context menu,...) as  the [`Structures` tab]({{"/gui/#structures-tab" | relative_url }}).

### Substructure Annotation view {#substructure-tab}

The `Substructure Annotations` tab visualizes the direct connection between the input MS/MS spectrum, the CSI:FingerID structure candidates and the (analog) spectral library matches. The substructure matching is not related to how CSI:FingerID scores structure candidates, but is a visualization tool based on combinatorial fragmentation of the candidate structure. Since combinatorial fragmentation cannot take rearrangements into account, they will not appear here, although CSI:FingerID can annotate structures that form rearrangements during fragmentation. 

Details of this view are also covered in our [Tutorial Series](https://www.youtube.com/watch?v=YZ1qYnP_gSg&t=257s).

{% capture fig_img %}
![Foo]({{ "/assets/images/substructure_fragment.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Substructure annotation view.</figcaption>
</figure>

The table at the top <span style="color:#d40f57">**[1]**</span> displays all structure candidates for a given query (as listed in the `Structures` tab), and all structure candidates generated
by MSNovelist. You can hide/unhide MSNovelist-generated structures by toggling
the left button in the top right corner <span style="color:#d40f57">**[2]**</span>. [Read here how to filter the results list]({{"/gui/#structures-tab" | relative_url}}).
When you select a structure from the list, the view shows the fragmentation spectrum on the left <span style="color:#d40f57">**[3]**</span> and the selected structure candidate on the right <span style="color:#d40f57">**[4]**</span>.

Peaks in the fragmentation spectrum are color-coded as follows:
- **Black peaks:** These peaks are not used to explain the molecular formula of the candidate and are not part of the fragmentation tree (similar to the spectrum in the [Formulas view](#formulas-tab). Typically, these peaks are considered noise or are not explainable by the precursor ion's molecular formula.
- **Blue peaks:** These peaks are part of the fragmentation tree and thus explain the molecular formula of the candidate. However, they do not have a substructure associated to them.
- **Green peaks:** These peaks explain the molecular formula of the candidate, AND are associated to a substructure of the candidate structure. Substructures are generated combinatorially and then scored against the peaks. By clicking on the peak, the highest scoring substructure for this peak will be highlighted within the structure. Pink bonds indicate the fragmentation that would have occurred to generate this fragment.

You can navigate through the peaks using left-click or the arrow keys.

To inspect the peak annotations in the context of a spectral library match, select an available match from the dropdown menu <span style="color:#d40f57">**[5]**</span>. Matches can result from identity spectral library search (same precursor mass), or analog search (Δ-Mass <span style="color:#d40f57">**[6]**</span>). Selecting a peak will automatically highlight the corresponding peak in the matched spectrum that was determined during cosine matching <span style="color:#d40f57">**[7]**</span>.

If the current library match we are inspecting is an analog match, some peak matches will represent shared losses instead of shared fragments. The corresponding loss’s substructure is highlighted in yellow instead. Using right-click on a selected peak, you can bring up a context menu to help find reference spectra that explain the specific peak.

{% capture fig_img %}
![Foo]({{ "/assets/images/substructure_loss.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Neutral loss annotation.</figcaption>
</figure>

### Library Matches view {#library-matches-tab}


{% capture fig_img %}
![Foo]({{ "/assets/images/spectral-matches-view.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Spectral library matches tab.</figcaption>
</figure>

The `Library Matches` tab lists <span style="color:#d40f57">**[1]**</span> all `IDENTITY` and `ANALOG` <span style="color:#d40f57">**[a]**</span> spectral library matches for the selected feature. For fast spectral search, spectra are grouped into clusters (by molecular structure and adduct type) with a merged spectrum as representative. The `MERGED_SPECTRUM` is also treated as valid hit <span style="color:#d40f57">**[b]**</span>. You can refine the displayed list by adjusting the `Similarity Score` and number of `Shared Peaks` range <span style="color:#d40f57">**[c]/[d]**</span>. 

The selected library match is displayed as mirror spectrum, allowing for a direct comparison with your measured spectrum  <span style="color:#d40f57">**[2]**</span>. Hovering over a peak reveals its m/z and intensity, along with the corresponding matched peak in the mirror spectrum, which is particularly useful for analog matches. The structure and additional metadata of the selected library hit are provided on the right side of the interface <span style="color:#d40f57">**[3]**</span>. 

For more information on spectral library searches in SIRIUS, please refer to the sections on [spectral library matching]({{ "/methods-background/#spectral-library-search" | relative_url }}) and [Import of Custom Structure and Spectra Databases](#custom-database-import).

#### Activate spectral library results tab {#activate-library-matches-tab}
<img src="{{ "/assets/images/activate-spectral-matches-tab.png" | relative_url }}" width="400">

To additionally activate the `Library Matches` tab, go to `Settings` and check `Show "Library Matches" tab`. You will get a warning dialogue explaining the spectral library search settings:
- A spectral library is also a molecular structure database. ANY hit in the spectral library can also be found via CSI:FingerID structure database search.
- Since structure database results depend on the selected molecular formula, SIRIUS ensures that molecular structures with a formula corresponding to a good spectral library hit are considered - even if this molecular formula receives a low score, i.e. molecular structures of well-matching reference spectra are automatically included in the structure database search.
- Structure database search is only performed on [databases selected by the user](#CSIFingerID-structure). To ensure that all your spectral libraries are considered by CSI:FingerID, select these libraries (databases) in the structure database search step.


### Homologous Series view {#KMD-tab}

The `Homologous Series` view helps you quickly find groups of related molecules, known as homologous series, within your complex samples using a Kendrick Mass Defect (KMD) plot <span style="color:#d40f57">**[1]**</span>.
Think of a homologous series as a family of molecules that share the same basic structure but differ by a consistent building block <span style="color:#d40f57">**[2]**</span>, like a CH2 (methylene) group. 

The Kendrick mass scale sets the exact mass of such a repeating unit to an integer value.
The KMD is then calculated as the difference between the exact Kendrick mass and its nominal (integer) Kendrick mass.
When data is transformed to the Kendrick mass scale and the KMD is plotted against the nominal Kendrick mass, a distinctive pattern emerges for homologous series.
On a KMD plot, features belonging to the same homologous series will exhibit the same (or very similar) Kendrick mass defect.
This results in these features aligning horizontally on the plot.

{% capture fig_img %}
![Foo]({{ "/assets/images/homologous-series.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Homologous Series tab with Kendrick Mass Defect (KMD) plot.</figcaption>
</figure>



In the  `Homologous Series` view you can select your repeating unit of interest <span style="color:#d40f57">**[2]**</span> to define the homologous series you want to visualize. The currently selected feature is highlighted with a pink outline <span style="color:#d40f57">**[a]**</span>. All features within a chosen KMD tolerance will appear as blue dots <span style="color:#d40f57">**[b]**</span>, while those at twice the tolerance are shown as light grey dots <span style="color:#d40f57">**[c]**</span>.
Hovering over any dot will reveal the top CSI:FingerID structure identified for this feature <span style="color:#d40f57">**[d]**</span>.
 If you include MS1-only features <span style="color:#d40f57">**[3]**</span>, no structure will be available for those features <span style="color:#d40f57">**[e]**</span>.


#### Activate tab {#activate-KMD-tab}
To  activate the `Homologous Series` tab, go to `Settings` and check `Show "Homologous Series" tab`. KMD plots are available after data import. Structure annotations are available after [Structure database search]({{"/gui/#CSIFingerID-structure" | relative_url }}).

## Structure sketcher {#structure-sketcher}

The Structure Sketcher enables users to manually modify candidate structures and integrate these new structures into the list of candidate structures. To access the `Structure Sketcher`, navigate to the `Structures` view. Right-click on any of the candidate structures and select `Modify structure` from the context menu. This action will launch the `Structure Sketcher` in a separate window. The interface of the Structure Sketcher is divided into two primary sections: the sketching area is located on the right <span style="color:#d40f57">**[1]**</span> and powered by [Ketcher](https://lifescience.opensource.epam.com/ketcher/), an open-source web-based chemical structure editor. Users can draw and modify chemical structures within this section. The list of allowed molecular formulas is positioned on the left <span style="color:#d40f57">**[2]**</span> and displays all molecular formula candidates for which a [fingerprint has been predicted]({{"/gui/#CSIFingerID-CANOPUS" | relative_url}}).
This pane also provides immediate feedback if an error occurs while attempting to add a structure candidate. This feedback is triggered if no fingerprint was predicted for the molecular formula or if the generated structure is already in the candidate list, preventing the creation of a redundant entry.

{% capture fig_img %}
![Foo]({{ "/assets/images/sketcher.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Structure sketcher to modify structure candidates.</figcaption>
</figure>


When modifying a candidate structure, ensure that the altered structure conforms to one of the allowed molecular formulas <span style="color:#d40f57">**[2]**</span>. Failure to do so will result in an error message indicating that no fingerprint was predicted for the generated molecular formula. Once modifications are complete, click either `Add` <span style="color:#d40f57">**[3]**</span> to include the new structure in the candidate list while keeping the sketcher open, or `Add and close` to add the structure and close the sketcher window. You can click `Reload` to revert to the structure you started with.

After closing the `Structure Sketcher`, switch to the `De Novo Structures` view (usually done automatically) to observe the newly added candidates. Sketched structures are labeled `Skteched structure` <span style="color:#d40f57">**[a]**</span>. 
Various filters are available to help manage and review the updated list. For example, users can toggle the display of database hits <span style="color:#d40f57">**[b]**</span> or apply the database filter called `Sketched` <span style="color:#d40f57">**[c]**</span> to show only the manually sketched candidates.
Finally, the `Substructure Annotations` view can be utilized to verify if the manually sketched candidate provides a more effective explanation for the observed peaks.

{% capture fig_img %}
![Foo]({{ "/assets/images/sketcher_denovo_view.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>De Novo Structures tab with results from the structure sketcher.</figcaption>
</figure>

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
- `Molecular structure display color`: Enable/disable CPK colors for all structures
- `Show "Library Matches" tab`: The `Library Matches` can be activated here after agreeing to a warning dialogue [explaining the spectral library search settings](#activate-library-matches-tab) 
- `Allowed solvers`: Select the [ILP solver]({{ "/install/#ILP-solvers" | relative_url }}) for SIRIUS to use in
        fragmentation tree computation. GLPK is free, while Gurobi is
        commercial but offers a free academic license.
- `Database cache`: Specifies the location of the cache directory. CSI:FingerID
        downloads candidate structures from our server and caches them
        for faster retrieval.
- `Presets`: Open compute dialog to view and edit [computation presets](#computation-presets)
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
