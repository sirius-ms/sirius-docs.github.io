---
permalink: /io/
title: "Input, Output and Formats"
---

With SIRIUS 6 we have transitioned from a file-based project space to a Nitrite database.
This shift was essential to enhance performance and enable new features. The CLI, API and GUI now store computed results as SIRIUS project space. 
The project space can in turn be used as input for the GUI or the CLI.
Consequently, results generated through an automated workflow using the CLI or API can be reviewed and accessed through the GUI.

## Input

SIRIUS supports multiple MS data formats: 
- `.ms`, `.mgf`, and Agilent's `.cef`: These formats contain pre-processed peak lists for each feature. 
- `.mzml`, and `.mzxml`: For these formats, SIRIUS will perform feature detection and alignment. **Note that, all data must be centroided**.

### Peak list-based formats

##### MGF-Format

SIRIUS supports the MGF (Mascot Generic Format), initially developed 
for peptide spectra in the Mascot search engine. Each MGF file can contain multiple spectra, each beginning and ending with the `BEGIN IONS` and `END IONS` markers, respectively. Peaks are listed as pairs of m/z and intensity
values, separated by whitespaces, with one peak per line. Additional 
meta-information can be included as `NAME=VALUE` pairs. SIRIUS recognizes the
following meta-information:

  - `PEPMASS`: Indicates the measured mass of the ion (e.g., the parent
    peak)
  - `CHARGE`: Specifies the charge of the ion. Since SIRIUS supports only
    singly charged ions, this value can be either 1+ or 1-.
  - `MSLEVEL`: Indicates the type of spectrum; it should be 1 for MS spectra and 2 for MS/MS spectra. SIRIUS
    will automatically treat higher values as MS/MS spectra.

This is an example for an MGF file:

```
BEGIN IONS 
PEPMASS=438.32382 
CHARGE=1+ 
MSLEVEL=2 
185.041199 4034.674316
203.052597 12382.624023 245.063171 50792.085938 275.073975 124088.046875
305.084106 441539.125 335.094238 4754.061035 347.09494 13674.210938
365.105103 55487.472656 
END IONS
```
See also the GNPS database for other examples of MGF files.

##### SIRIUS MS-Format

While the above mentioned data formats may lack some
necessary information for SIRIUS computations, the SIRIUS MS file format addresses this limitation and is very
similar to the MGF format. 
This custom file format, which uses the .ms extension, is designed to ensure that all required data is available for processing.

Each `.ms` file contains one measured feature, and can include multiple spectra. 
Each line contains either:
- a peak (given as m/z and intensity separated by
whitespace), 
- meta-information (starting with `>` followed
by the information type, a whitespace and the value) or 
- a comment 
(starting with the `#` ).

SIRIUS recognizes the following meta-information fields:

[//]: # TODO: Is feature/compound already changed here?

  - `>compound`: Specifies the name of the measured feature (or a placeholder).
    This field is **mandatory**.
  - `>parentmass`: Specifies the mass of the parent peak.
  - `>formula`: Provides the molecular formula of the feature. This information
    is helpful if the correct molecular formula is already known and
    you want to compute a fragmentation tree or recalibrate the
    spectrum.
  - `>ion`: Denotes the ionization mode. Refer to the [ion modes format](#ion-modes) for details.
  - `>charge`: Specifies the charge of the ion (1 or -1). This field is redundant if the ion mode is specified.
  - `>ms1`: Marks the beginning of MS peak data. All peaks after this line are interpreted as MS peaks.
  - `>ms2`: Marks the beginning of MS/MS peak data. All peaks after this line are interpreted as MS/MS peaks.
  - `>collision`: Works the same as `>ms2` but you can
    specify the collision energy.

An example of an `.ms` file:
```
>compound Gentiobiose
>formula C12H22O11 
>ionization \[M+Na\]+ 
>parentmass 365.10544

>ms1 
365.10543 85.63 366.10887 11.69 367.11041 2.67

>collision 20 
185.041199 4034.674316 
203.052597 12382.624023 
245.063171 50792.085938 
275.073975 124088.046875 
305.084106 441539.125 
335.094238 4754.061035 
347.09494 13674.210938 
365.105103 55487.472656
```
Note: The `.ms` file format is SIRIUS' internal format and may evolve as 
the software develops. Nevertheless, we strive to maintain backward compatibilty whenever possible.
A more detailed and commented example (which is still a work in progress) of an `.ms` file can be found [here](ms-file.md).

### LCMS-Runs

SIRIUS can import full LCMS runs in `.mzML` and `.mzXML` formats through its
prepossessing tool. In the GUI, this is done automatically when you import your data. In the CLI, the process can be managed explicitly by using the [LC-align]({{ "/cli/#lcms-align-feature-detection-and-feature-alignment-preprocessing" | relative_url }}) subtool.
<span>**<span style="color: red">\[todo: describe lcms-align\]</span>**</span>

## Output

### SIRIUS project space

Since SIRIUS 6, the project space is stored in a singular `.sirius` file, which is not intended to be human or machine-readable.
This change was implemented to ensure performance for many of the new features and is in no way intended to "close off" results.
You can still generate summaries using the GUI or CLI. To access advanced information or intermediate results (e.g., predicted
fingerprints) you can use the new API.

### Summary files

The summaries generated by the CLI or GUI are saved in TSV (tab-separated values) format and include the following files: 
- `formula_identifications.tsv`,
- `canopus_formula_summary.tsv`, 
- `canopus_structure_summary.tsv`, 
- `structure_identifications.tsv` and 
- `denovo_structure_identifications.tsv`.

[//]: # (TODO: add spectral matching results)


These files provide convenient access to the results for downstream analysis, data
sharing and data visualization. The summaries are not imported into
SIRIUS but are (re-)created every time a
project-space is exported, ensuring that the summaries reflect the most current results. 
Summaries are created separately for [molecular formula annotation](#molecular-formula-summary), [structure annotation](#structure-summary), and [compound class prediction](#CANOPUS-summary).

#### Molecular formula results summary {#molecular-formula-summary}

The summary file `formula_identifications.tsv` contains the top-ranked molecular formula for each feature, based on the SIRIUS score or ZODIAC score, if available.
When multiple adduct candidates correspond to the same precursor ion molecular formula (e.g. `[C20H14O6 + NH4]+` and `[C20H19NO7 - H2O + H]+`),
they will have the identical score.
In such cases, the top-ranked candidate in `formula_identifications.tsv` is resolved
to `[C20H17NO6 + H]+` only considering the ion type but ignoring adduct types.

The file `formula_identifications_adducts.tsv` includes all top-ranked adducts (in this case 
`[C20H14O6 + NH4]+` and `[C20H19NO7 - H2O + H]+`). This summary provides additional details, including all scores
displayed in the GUI as well as potential lipid class annotations.  

#### Molecular structure results summary {#structure-summary}

The summary file `structure_identifications.tsv` contains the top-ranked structure result for each feature,
based on the CSI:FingerID score. Note that the molecular formula of this top structure may differ from the 
top-ranked molecular formula of this feature. The file includes a column `formula rank`, which indicates the
original rank of the molecular formula of this to structure.

The summary provides the confidence scores for both, exact and approximate modes, as well as CSI:FingerID, ZODIAC and SIRIUS scores.
Additionally, the `links` column contains reference links to structure databases containing the identified structure.

[//]: # (TODO: describe `denovo_structure_identifications.tsv`)


#### CANOPUS results summary {#CANOPUS-summary}

CANOPUS results are reported separately for molecular formula annotations and molecular structure annotations:
- The file `canopus_formula_summary.tsv` contains the predicted compound classes for the top-ranked molecular formula of each feature. 
-  The file `canopus_structure_summary.tsv` contains the predicted  compound classes for the molecular formula belonging to the top-ranked structure of each feature. 

In both files, the column `most specific class` indicates the most specific compound class identified for this feature. The columns
`level 5`, `subclass`, `class`, and `superclass` refer to the ancestors of this most specific class.

If case multiple molecular formulas share the 
same score ([typically due to adducts](#molecular-formula-results-summary)), the
`canopus_summary.tsv` file selects one molecular formula for each feature. We select the molecular formula
for which the CANOPUS probability of the most specific class is highest.

### Standardized project space summary with mzTab-M

The project space is a SIRIUS-specific format designed to provide comprehensive
access to all results and analysis details. However, it may not be ideal for
sharing data with third-party tools or data archives. To facilitate this, SIRIUS offers an analysis report in
the standardized mzTab-M format (`analysis_report.mztab`). 
The mzTab-M report includes summarized results and links them
to the detailde findings in the corresponding SIRIUS project space. 
This ensures that while sharing summarized results using mzTab-M, users retain access to the comprehensive details available in the project space. 
Furthermore, SIRIUS transfers meta-information from the input -
such as scan numbers and input data IDs - into the mzTab-M report. This allows
for seamless integration of SIRIUS results with results from other
analyses, such as MS1-based quantification.

### Parameter formats

#### Ion modes

Whenever SIRIUS requires the ion mode, it should be specified in the
following format:

```
[M+ADDUCT]+ for positive ions
[M+ADDUCT]- for negative ions
[M-ADDUCT]+/[M-ADDUCT]- for losses 
[M]+/[M]- for instrinsically charged compounds
[M+?]+ for positive ions with unknown adduct
[M+?]- for negative ions with unknown adduct
```

`ADDUCT` refers to the molecular formula of the adduct. The most common
ionization modes are `[M+H]+`, `[M+Na]+`, `[M-H]-`, `[M+Cl]-`. 
Currently, SIRIUS supports only singly charged compounds. Hence, adducts such as `[M+2H]2+` will be skipped during import. Additionally, dimers, which are represented by `[2M]`, are also not yet
supported and will be skipped during import.

#### Molecular formulas

In SIRIUS, molecular formulas must not contain brackets. For instance, use `C4H4` instead of `2(C2H2)`. All molecular
formulas in SIRIUS are treated as neutral. You cannot include charges in a molecular formula. Hence, `CH3+` is not a valid molecular formula. Use `CH3` instead, and
provide the charge separately.

#### Chemical alphabets

When specifying a chemical alphabet in SIRIUS, you need to indicate the elements to be considered and their maximum quantities. Chemical alphabets are formatted like molecular formulas, with maximum quantities enclosed in square brackets behind
the element. If no maximum quantity is given, the element might occur
arbitrary often. The standard alphabet is
`CHNOP[5]S`, which includes the elements Carbon (C), Hydrogen (H), Nitrogen (N), Oxygen (O), and Sulfur (S), with Phosphorus (P) allowed up to five times.
