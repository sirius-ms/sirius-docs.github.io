---
permalink: /cli/
title: "Commandline Interface"
---

The SIRIUS command-line tool can be called via the "binary/startscript" by
simply running the command in the commandline:
```shell
sirius --help
```

**TIP: We recommend using the `--help` option to get an overview of the available commands and options.**
This ensures that the command descriptions are accurate and match your specific version of SIRIUS.

## Introduction {#introduction}

The SIRIUS command-line program is a versatile
toolbox designed for metabolite
identification, offering a variety of tools (subcommands) that can be concatenated as toolchains to 
perform multiple analysis steps in a single run.
The subcommands are categorized into different types:

- **CONFIGURATION TOOL:** The `config` tool can be executed before any
    toolchain or standalone tool to configure all settings available in
    SIRIUS from the command line.
- [**STANDALONE TOOLS:**]({{ "/cli-standalone/" | relative_url }}) These tools operate
    independently and cannot be concatenated
    with other subtools. They are typically used for data management
    tasks, such as modifying project-spaces or exporting MGF files. 
- [**PREPROCESSING TOOLS:**](#preprocessing) Tools that prepare input data to be compatible
    with SIRIUS. For example, [`lcms-align`](#lcms-align) is used for feature detection and alignment.
- [**COMPOUND TOOLS:**](#compound-tools) These tools analyze each compound (instance) in
    the dataset individually and can be concatenated with other tools. Examples are molecular formula annotation ([`formulas`](#SIRIUS-formulas)), structure database search ([`structures`](#CSI-FingerID-structures)) or compound class prediction ([`classes`](#CANOPUS-classes)).
- [**DATASET TOOLS:**](#dataset-tools) These tools analyze all compounds (instances) in
    the dataset simultaneously and can be concatenated with other tools. For example, dataset-wide molecular formula annotation with [`zodiac`](#zodiac).

Each subtool can be called with the `--help` option to view the documentation
on available options and potential follow-up commands in a
toolchain. For example, to get help for the `formulas` tool, use:

```shell
sirius formulas --help
```
### Basic principles {#basic-principles}

{% capture fig_img %}
![Foo]({{ "/assets/images/CLI-workflow.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>CLI workflow dependencies.</figcaption>
</figure>

The SIRIUS CLI toolbox functions as a basic workflow engine (to generate toolchains),
adhering to the following principles:

- Only the subtools explicitly specified in the command will be executed.
- If a mandatory input from a previous step (subtool) is missing, the computation for the current compound will be skipped.
- By default, the toolbox does **not** override existing results. Compounds for which results are already available will be skipped.
- If the `--recompute` option is specified, existing results will be replaced with new ones for **all** subtools that are specified in the command.
- When results for one subtool are recomputed (`--recompute`), the results of downstream subtools that depend on recomputed results will be deleted, to ensure that all results remain consistent with the newly computed data. 



### Examples {#examples}

**Compute missing results without recomputing:** 
Assume you have previously computed results for the `formulas` and `structures` subtools for compounds with a mass less than 600 Da. 
Now, you want to run a workflow that includes `formulas`, `structures`, and `classes` on the same [project-space]({{ "/io/#sirius-project-space" | relative_url }}), but without restricting the precursor mass. 
The `formulas` and `structures` results for compounds under 600 Da will be skipped and not recomputed, as these results already exist. Results for compounds with at least 600 Da will be newly generated. 
The `classes` subtool will then be executed for all compounds.

**Recompute all results:** 
If you run the same workflow with the `--recompute` option, all results will be recomputed. 

**Recompute results for a single subtool:** 
Assume you have a [project-space]({{ "/io/#sirius-project-space" | relative_url }}) 
with complete results for `formulas`, `fingerprints`, `structures`, and `classes`, but you need to recompute the `structures` results due to incorrect parameters. 
Executing ```sirius -i <projekt> --recompute structure -db mydb```
will recompute all `structures` results without affecting the existing `formulas` results. 
The `classes` results will also not be affected, as they only depend on the `fingerprints` results, not on the `strucures` tool results.
**Note:** Recomputing the `fingerprints` tool results would cause the loss of both `structures` and `classes` results.

**Proceed with interrupted computations:** 
If a computation is interrupted, simply rerun the same command to resume the process. 
SIRIUS will skip the computation for existing results and only compute the missing ones.

**Special case `zodiac`:** 
The `zodiac` tool operates on the entire dataset rather than individual compounds. 
Therefore, if not all compounds have `zodiac` results, the tool will recompute results for the entire dataset.

**Visualize results in the GUI:** Results from all subtools that produce a 
[project-space]({{ "/io/#sirius-project-space" | relative_url }}) as output (using the `--output` option), 
can be visualized, modified, or further analyzed in the [SIRIUS GUI]({{ "/gui/" | relative_url }}).

## PREPROCESSING TOOLS {#preprocessing}

### `lcms-align`: Feature detection and feature alignment (Preprocessing) {#lcms-align}

The `lcms-align` tool enables the import of multiple `.mzML`/`.mzXML` files into SIRIUS. It performs feature detection and alignment based on MS/MS spectra, creating a SIRIUS project-space. 

This project-space can then be used for subsequent analysis steps, for example:

```shell
sirius -i <mzml(s)> -o <projectspace> lcms-align formula
```

## COMPOUND TOOLS {#compound-tools}

### `formulas`: Identifying molecular formulas with SIRIUS (Compound Tool) {#SIRIUS-formulas}

One of the primary functions of SIRIUS is identifying the molecular formula of a
measured ion. For this task, SIRIUS provides the `formulas` tool. 

The most basic way
to use the `formulas` tool is with a generic text or CSV input:

```shell
sirius [OPTIONS] -1 <MS FILE> -2 <MS/MS FILES comma separated> -z <PARENTMASS> --adduct <adduct> --output <projectspace> formulas
```
**Available aliases:** `trees`, `formula`, `sirius`

`MS FILE` is the MS1 spectrum containing the isotope pattern, and `MS/MS FILES` are the MS/MS fragmentation spectra. You can provide multiple MS/MS files (comma separated) if you have several measurements of the same compound with different collision energies; SIRIUS will merge these spectra into a single spectrum. If you omit the `--adduct` option, `[M+?]+` is used as default. 

The command also works for [MGF files]({{ "/io/#mgf-format" | relative_url}}), where you can omit the `-z` option for specifying the parent mass, if it is already given in the file. 

Instead of the generic text or CSV input, we **recommend** using [input files in `.ms` or `.mgf` format]({{ "/io/#peak-lists" | relative_url}}), 
which contain all spectra for a compound as well as metainformation, such as parent mass, ionization and MS level. SIRIUS will extract the meta information from
the files. They can also contain multiple compounds per file. 

```shell
sirius [OPTIONS] --input <inputFile> --output <projectspace> formulas [OPTIONS]
```
If you specify a directory instead of a single file, SIRIUS will crawl the directory for supported files, allowing for batch processing of multiple compounds.

The results, including scored molecular formula candidates and corresponding fragmentation trees in `.json` format, are saved in the specified `<projectspace>`.
If the  `write-summaries` command is used, SIRIUS
will write 
- a summary file at compound level, which includes the `rank`, `molecularFormula`, `adduct`,
`precursorFormula`, `rankingScore`, `SiriusScore`, `TreeScore`,
`IsotopeScore`, `numExplainedPeaks`, `explainedIntensity`, `medianMassErrorFragmentPeaks(ppm)`,	
`medianAbsoluteMassErrorFragmentPeaks(ppm)`, `massErrorPrecursor(ppm)` 
- a summary containing the top hits for all compounds on project level.

The [`SiriusScore`]({{"/gui/#formulas-tab" | relative_url}}) is the sum of the `TreeScore` and the `IsotopeScore`, and is used for ranking the candidates. 
If the `IsotopeScore` is negative, it is set to zero. If at least one
`IsotopeScore` exceeds 10, the isotope pattern is considered
to have *good quality* and only the candidates with best isotope pattern
scores are selected for further fragmentation pattern analysis.


#### Computing fragmentation trees only

If you already know the correct molecular formula and only need to 
compute a fragmentation tree, you can specify the formula using the `--formulas` option. 
SIRIUS will then compute a fragmentation tree exclusively for this molecular formula. 
If your input data is in `.ms` format, the molecular formula might already be included in the file. 
The `--formulas` option also accepts a comma-separated list of candidate molecular formulas.

```shell
sirius -i <input> --output <projectspace> formulas --formulas <formula>
```

#### Instrument-specific parameters

Datasets vary in mass errors, noise levels, accuracy of isotope pattern intensities depending on instrument type and setup.
By default, SIRIUS uses a profile for `Q-TOF` data, with a 10 ppm mass deviation. This profile is  suitable as a good default profile for a range of instruments.
If you are using data with much lower mass errors, such as Orbitrap or FT-ICR spectra, you may need to adjust the parameters accordingly. Conversely, if your data has higher mass errors, you should adjust the profile and mass deviation settings to match.

You can specify the instrument profile using `-p <name>`, choosing either `qtof` (default) or `orbitrap`. The
`orbitrap` profile uses a mass deviation of 5 ppm and has slightly different isotope scoring settings. 
For FT-ICR data, we recommend using the `orbitrap` profile and specify an even lower mass deviation.

You can specify the maximum allowed mass deviations for MS1 and MS2 separately:

```shell
sirius -i <input> --output <projectspace> formulas -p orbitrap --ppm-max 2 --ppm-max-ms2 5
```

#### Large data sets with high mass compounds
When computing molecular formulas with SIRIUS a few high mass compounds usually need most of the computing time, and some of them might not finish computing in reasonable time at all and block your whole analysis.

The most straightforward solution is to exclude high mass compounds from the analysis, by setting a mass threshold. This will usually allow you to annotate the vast majority of your data. However, many of the higher mass compounds will work just fine, and it would be a pity to not annotate them. Therefore, the recommended solution is to set a per-compound timeout (`--compound-timeout`) so that a few hard cases cannot block your analysis. Read mor about [How to deal with high mass compounds]({{"/faq/how-to-large-comp" | relative_url}}).



### `fingerprints`: Predicting molecular fingerprints (Compound Tool) {#fingerprints}

[Molecular fingerprints]({{ "/methods-background/#molecular-fingerprint" | relative_url}}) can be predicted using the `fingerprints` command after calculating molecular formula candidates with the `formulas` tool. 
A fingerprint is generated based for a specific molecular formula candidate and its corresponding fragmentation tree. By default, SIRIUS predicts fingerprints for multiple high-scoring formula candidates by applying a soft threshold on the SIRIUS score.

```shell
sirius -i <input> -o <projectspace> formulas fingerprints
```

**Available aliases:** `fingerprint`


### `classes`: Database-free compound classes prediction with CANOPUS (Compound Tool) {#CANOPUS-classes}

The `classes` tool enables the prediction of compound classes directly from the probabilistic molecular fingerprints generated by CSI:FingerID (using the `fingerprints` command). One key advantage of CANOPUS is its ability to provide compound class information even for compounds that have no matching hit in a structure database. CANOPUS classes are required for confidence score estimation.

```shell
sirius -i <input> -o <projectspace> formulas fingerprints classes
```

**Available aliases:** `canopus`, `compound-classes`

### `structures`: Identifying molecular structures (Compound Tool) {#CSIFingerID-structures}

The `structures` tool in SIRIUS allows you to for molecular structures in a structure database
using CSI:FingerID.
To perform structure database search, molecular fingerprints must first be predicted using the `fingerprints` tool. For improved formula ranking within biologically derived samples (or any other set of derivatives), we recommend to run the [`zodiac` tool](#zodiac) beforehand.

You can specify the database for CSI:FingerID to search in, using the `--databases` option. [Available databases]({{ "/methods-background/#CSIFingerID" | relative_url}}) include `pubchem` and `bio`, among others.

```shell
sirius -i <input> -o <projectspace> formulas fingerprints structures --database pubchem
```
**Available aliases:** `structure-db-search`, `structure`

The `write-summaries` tool will generate a `structure_candidates.csv` file for each compound, containing an ranked list of structure
candidates along with their CSI:FingerID scores. Additionally, a project space-wide `compound_identification.csv`
file will be generated, listing the top candidate structure for each compound,
ordered by their confidence score.

### `denovo-structures`: Generate *de novo* molecular structures (Compound Tool) {#MSNovelist-denovo-structures}

```shell
sirius -i <input> -o <projectspace> formulas fingerprints denovo-structures
```
**Available aliases:** `msnovelist`

TODO: fill

### `passattuto`: Decoy spectra from fragmentation trees (Compound Tool) {#passatutto}

The `passattuto` tool is designed to generate high-quality decoy spectra from
fragmentation trees obtained using the `formulas` tool. If you're working with a spectral library, 
you can easily create a decoy database based on these spectra:

```shell
sirius -i <spectral-lib> -o <projectspace> formulas passatutto
```

If no molecular formulas are annotated to the input spectra, 
PASSATUTTO will use the best-scoring candidate for decoy computation.

## DATASET TOOLS {#dataset-tools}

### `zodiac`: Improve molecular formula identifications (Dataset Tool) {#ZODIAC}

When working with input data derived from biological samples or sets of derivatives, 
similarities between different compounds can be used to enhance molecular formula annotations. 
ZODIAC leverages these similarities by constructing a network of molecular formula candidates (generated by the `formula` tool) and re-ranking these candidates using Bayesian statistics (Gibbs Sampling). This approach can reduce the error rate of the top-ranked candidates by approximately 2 fold, with even more significant improvements on challenging datasets containing large compounds.

The `zodiac` tool is executed after the `formulas` tool:

```shell
sirius -i <input> -o <projectspace> formulas -c 50 zodiac
```

We recommend to increase the maximum number of
formula candidates (`-c`) retained after running the `formula` tool.
The candidates are the ZODIAC input, and if the correct candidate is missing, ZODIAC cannot
recover it. In order to reduce memory consumption and running time,
ZODIAC dynamically adjusts the number of candidates for each compound based on its m/z. The rationale is that lower-mass compounds are more likely to have the correct molecular formula ranked higher, allowing for fewer candidates to be considered.
By default, ZODIAC uses 
10 candidates for compounds with m/z ≤ 300 (`--considered-candidates-at-300 10`) and 
50 candidates for compounds with m/z ≥ 800 (`--considered-candidates-at-800 50`). 
In between, the number of candidates is calculated by interpolation. 

The density of the ZODIAC network is primarily determined by two parameters: `--edge-threshold`
(default: 0.95) and `--minLocalConnections` (default: 10). 
The edge threshold discards 95% of the lowest-scoring edges, 
assuming most are incorrect since only one correct candidate exists per compound.
To prevent compounds being disconnected from the rest of the network,
at least one candidate per compound remains connected
to at least
`--minLocalConnections` other compounds. This introduces an individual edge score threshold for
each compound. Be aware that using `--minLocalConnections` may requires substantial memory as the entire network is constructed before edge filtering.

For very large datasets, the ZODIAC network might exceed 1TB of system
memory. To manage this, [align features across LC-MS/MS runs](#lcms-align) to reduce the number of compounds. If memory usage is still high, 
memory consumption can be dramatically decreased by setting `--minLocalConnections=0` to allow filtering low-weight edges during network creation.
**However, use this setting with care, since it can result in a poorly connected
network, potentially reducing performance.**

```shell
sirius -i <input> -o <projectspace> formula -c 50 zodiac --minLocalConnections 0 --edge-threshold 0.99
```