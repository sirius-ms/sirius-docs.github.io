---
permalink: /advanced-background-information/
title: "Advanced background information"
---

## Spectral quality

SIRIUS and CSI:FingerID have been trained on a wide variety of data,
including data from different instrument types. Nevertheless, certain
characteristics of the mass spectra are important for our software to
successfully process your data:

  - SIRIUS requires **high mass accuracy** data: The
    mass deviation should be within 20 ppm. We are confident that SIRIUS
    will also provide useful information for lower mass accuracy data (say,
    50 ppm), but you should know what you are doing if you are
    processing such data.

  - It is understood that some molecules generate many fragments
    while others have sparse fragmentation spectra. However, it is crucial to understand that without sufficient information, deducing the structure or even the molecular formula from a tandem mass spectrum with almost no peaks is impossible. For
    instance, three peaks in a fragmentation spectrum measured with 1 ppm
    mass accuracy contain roughly 60 bits of information, ignoring
    dependencies between fragments and the distribution of molecular masses.   
    With this limited information, identifying the correct structure in a database like PubChem, which contains 100 million structures, is unfeasible. In comparison, ten peaks measured with 20 ppm mass
    accuracy provide about 156 bits of information under the same assumptions. To this end, we ask you to provide SIRIUS with  **rich fragmentation spectra**, i.e., you **must
    not noise-filter** these spectra, or rely on peak picking/centering software to do it for you. SIRIUS currently considers up to 60 peaks in the fragmentation spectrum and autonomously determines which peaks are noise.

    You might find that CSI:FingerID occasionally identifies the correct
    structure although the fragmentation spectrum is (almost) empty. However, don't be misled -- this is often just a matter of lucky guessing.If you have a method for structurally elucidating a compound based on an empty spectrum, we would be very interested to hear from you. 

  - You may have heard that peaks with high mass in a MS/MS spectrum 
    carry more information than those with low mass. This is a
    misunderstanding. For example, if CSI:FingerID needs to differentiate
    between 10,000 candidates with identical molecular formula, then
    observing a fragment corresponding to an H<sub>2</sub>O loss is in fact quite
    uninformative. Therefore, **do not set up your instrument to favor
    large mass peaks** at the expense of smaller mass peaks.

  - Some instrument types, such as time-of-flight, can experience detector saturation, resulting in peaks with mass differences much larger than expected. Unfortunately, most peak picking software does not mark these peaks as "misshaped". As a result, the most intense peak in a spectrum may remain unexplained due to its extreme mass deviation.

## Monoisotopic masses

The monoisotopic mass of a molecule (or ion) is formally defined as "the
sum of the masses of the atoms in a molecule (or ion) using the unbound,
ground-state, rest mass of the most abundant isotope for each element."
Using this definition, the monoisotopic mass is usually not the most
abundant isotopologue of the molecule (e.g., in peptides and proteins). It
is often not resolved from other isotopologue peaks and may be
undetectable in an MS experiment because its intensity is below the noise level. Given the isotope pattern of an unknown molecule, it is
generally impossible to determine which peaks corresponds to the
monoisotopic peak. Therefore, this definition is not very practical.

Many researchers working on the simulation and interpretation of
isotope patterns have introduced a slightly different and more
practical definition of the monoisotopic mass of a molecule. For example, [Dittwald *et al.*](https://doi.org/10.1007/s13361-015-1180-4)
and [Meusel *et al.*](https://doi.org/10.1021/acs.analchem.6b01015) define the *monoisotopic* mass as the isotopologue of a molecule where each atom is the
isotope with the lowest nominal mass according to the natural isotope
distribution of elements. This definition has several advantages:
- The monoisotopic mass of a molecule
is always the sum of monoisotopic masses of the atoms, which can be
defined analogously
- The monoisotopic peak is always the first peak of the ideal isotope pattern.
- The monoisotopic (isotopologue)
peak is always resolved from all other isotopologue peaks, even at unit
mass accuracy. 

The monoisotopic peak of a molecule may again be
undetectable in an MS experiments.

SIRIUS uses the second, more practical definition of "monoisotopic".
This difference is only relevant for molecules that contain "uncommon elements" such as boron or selenium.

## Theoretical masses of ions

There are different ways to compute the mass of an ionized molecule such as C<sub>6</sub>H<sub>7</sub>O + 
or C<sub>6</sub>H<sub>6</sub>ONa + which result in slightly different values. In particular, one can either add the mass of a proton or subtract the mass of an electron. Following
the recommendations of [Ferrer & Thurman](https://doi.org/10.1002/rcm.3102), 
SIRIUS computes this mass by subtracting the rest mass of an electron. For example, the monoisotopic mass of C<sub>6</sub>H<sub>7</sub>O + is the monoisotopic mass of the molecule 
C<sub>6</sub>H<sub>7</sub>O (95.049690 Da) minus the
rest mass of an electron (0.000549 Da), which totals as 95.049141 Da. Similarly, the monoisotopic mass of 
C<sub>6</sub>H<sub>6</sub>ONa + is calculated as 117.031634 Da - 0.000549 Da, resulting in 117.031085 Da.

**We recommend calibrating your instrument using ion masses as calculated
above.** In any case, it is important to keep these small mass differences in mind, as they may lead to unexpected behavior when decomposing
masses; see for example [Pluskal *et al.*](https://doi.org/10.1021/ac3000418).

### Isotopes with masses and abundances as used by SIRIUS
In the examples above and in the table below, the masses have been rounded to six decimal places. 
SIRIUS internally uses double precision to represent masses. 
Isotope masses are derived from the [AME2016 atomic mass evaluation](http://nuclearmasses.org/resources_folder/Wang_2017_Chinese_Phys_C_41_030003.pdf) 
atomic mass evaluation. ‘AN’
is atomic number. Isotope abundances of boron can vary strongly, so
isotope pattern analysis is of little use for identifying the correct
molecular formula in case boron is present.

| element (symbol) | AN | isotope       | abundance (%)                    | mass (Da)          |
|-----------------:|---:|:-------------:|---------------------------------:|-------------------:|
|     hydrogen (H) |  1 |<sup>1</sup>H  |                        99.988% |         1.007825 |
|                  |    |<sup>2</sup>H  |                         0.012% |         2.014102 |
|        boron (B) |  5 |<sup>10</sup>B |                        19.9<sup>*</sup>% |        10.012937 |
|                  |    |<sup>11</sup>B |                        80.1<sup>*</sup>% |        11.009305 |
|       carbon (C) |  6 |<sup>12</sup>C |                         98.93% |             12.0 |
|                  |    |<sup>13</sup>C |                          1.07% |        13.003355 |
|     nitrogen (N) |  7 |<sup>14</sup>N |                        99.636% |        14.003074 |
|                  |    |<sup>15</sup>N |                         0.364% |        15.001090 |
|       oxygen (O) |  8 |<sup>16</sup>O |                        99.757% |        15.994915 |
|                  |    |<sup>17</sup>O |                         0.038% |        16.999131 |
|                  |    |<sup>18</sup>O |                         0.205% |        17.999160 |
|     fluorine (F) |  9 |<sup>18</sup>F |                           100% |        18.000938 |
|     silicon (Si) | 14 |<sup>28</sup>Si|                        92.223% |        27.976927 |
|                  |    |<sup>29</sup>Si|                         4.685% |        28.976495 |
|                  |    |<sup>30</sup>Si|                         3.092% |        29.973770 |
|     phosphor (P) | 15 |<sup>32</sup>P |                           100% |        30.973762 |
|       sulfur (S) | 16 |<sup>33</sup>S |                         94.99% |        31.972071 |
|                  |    |<sup>34</sup>S |                          0.75% |        32.971459 |
|                  |    |<sup>35</sup>S |                          4.25% |        33.967867 |
|                  |    |<sup>36</sup>S |                          0.01% |        35.967081 |
|    chlorine (Cl) | 17 |<sup>35</sup>Cl|                         75.76% |        34.968853 |
|                  |    |<sup>37</sup>Cl|                         24.24% |        36.965903 |
|    selenium (Se) | 34 |<sup>74</sup>Se|                          0.89% |        73.922476 |
|                  |    |<sup>76</sup>Se|                          9.37% |        75.919214 |
|                  |    |<sup>77</sup>Se|                          7.63% |        76.919914 |
|                  |    |<sup>78</sup>Se|                         23.77% |        77.917309 |
|                  |    |<sup>80</sup>Se|                         49.61% |        79.916521 |
|                  |    |<sup>82</sup>Se|                          8.73% |        81.916699 |
|     bromine (Br) | 35 |<sup>79</sup>Br|                         50.69% |        78.918337 |
|                  |    |<sup>81</sup>Br|                         49.31% |        80.916291 |
|       iodine (I) | 53 |<sup>127</sup>I|                           100% |       126.904473 |



## Mass deviations

SIRIUS assumes that mass deviations (the difference between measured
and theoretical ion masses) follow a normal distribution ([Jaitly *et al.*](https://doi.org/10.1021/ac052197p), 
[Zubarev & Mann](https://doi.org/10.1074/mcp.M600380-MCP200), 
[Böcker & Dührkop](https://dx.doi.org/10.1186%2Fs13321-016-0116-8)). 
The user-defined parameter "mass accuracy" is specified in parts-per-million
(ppm). SIRIUS interprets this parameter as the **maximum allowable mass 
deviation** and will **discard** any interpretations that require a greater 
deviation. Therefore, **if in doubt, it is advisable to set a wider mass 
accuracy** to ensure SIRIUS can successfully annotate peaks in the 
spectrum. For masses below 200 Da, we use the absolute mass deviation at 
200 Da, as we
found that small masses vary according to an absolute rather than a
relative error.

## Adducts

Adduct information can be provided in two ways
1. Specified in the input file created by third-party preprocessing tools (using peak list-based formats such as .mgf).
2. Adducts can be detected by the SIRIUS preprocessing based on .mzml input files.

The specified adduct will affect the possible molecular formula candidates of a feature and consequently the fingerprint prediction, compound class prediction, and molecular structure hit.

_Note: In SIRIUS 6 we have moved away from using ionization (e.g. [M+H]+) in the formula annotation step and expanding with the adduct (e.g. [M+H]+ to [M+H-H<sub>2</sub>O]+) in the structure database search step.
Instead, the entire adduct is now used from the beginning on._

Preprocessing detects adducts from a predefined list and selects them based on correlating chromatographic peaks with indicative mass differences in the data. 
It is often challenging to assign a single unambiguous adduct to every feature. In case the adduct assignment is ambiguous, SIRIUS will consider multiple possible adducts.  If the data does not even allow to assign a subset of possible adducts, a set of fallback adducts is used which can be specified by the user.

During the formula annotation step, SIRIUS generates and scores molecular formula candidates that match the specified adduct(s). 
A single precursor molecular formula can correspond to multiple compound formulas (using different adduct candidates).
All adducts of the same precursor formula receive identical scores, since it is not possible to distinguish between them based on the isotope pattern and MS/MS spectrum: The isotope patterns will be identical and a loss observed in the MS/MS spectrum could stem from either the adduct or a covalently bonded part of the molecule.

Two specific details must be noted:
1. Fragmentation trees which are used to score molecular formula candidates, are provided in neutral form. For all adducts with the same ionization (e.g. [M+H]+ for [M+H]+, [M+H-H<sub>2</sub>O]+ and [M+NH<sub>4</sub>]+), a common fragmentation tree is computed. Then, fragmentation trees are resolved for each specific adduct. During this process, some fragments maynot be explained by a resolved formula and are removed from the tree. For example, resolving C<sub>6</sub>H<sub>10</sub>NO for adduct [M+NH<sub>4</sub>]+ is possible (C<sub>6</sub>H<sub>6</sub>O), but not for C<sub>6</sub>H<sub>12</sub>O<sub>6</sub>. Despite removing these fragments, we do not alter the score for the fragmentation tree, as the fragment could have had another possible explanation, and we do not want to penalize the candidate due to this post-processing.
2. We do not differentiate between [M+H]+ and [M]+. In LC-MS experiments [M]+ is very uncommon. Moreover, for an unknown compound in an untargeted measurement it is challenging to determine if the compound was intrinsically charged or ionized later by the instrument. Therefore, SIRIUS considers the same neutral molecular formula for both adducts (as [M+H]+), but also searches for intrinsically charged molecular structures at the database search step. Per default [M+H]+ is considered, and [M]+ is merely treated as a special case of [M+H]+. [M]+ is used if directly specified in the input file or by the user.

## SIRIUS workflows

SIRIUS is the umbrella application comprising several workflows:
- molecular formula annotation (SIRIUS + ZODIAC),
- fingerprint prediction and compound class prediction (CSI:FingerID + CANOPUS),
- structure database searching (CSI:FingerID + COSMIC),
- de novo structure generation (MSNovelist).

These workflows follow a certain hierarchy, and cannot be freely combined. For example, to predict CANOPUS compound classes, the molecular formula annotation workflow must be run first (or results from a previous run must be available). See the following figure on how the different workflows depend on each other.

{% capture fig_img %}
![Foo]({{ "/assets/images/workflow_dependencies.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>SIRIUS workflow dependencies.</figcaption>
</figure>


## Spectral library matching

SIRIUS 6 allows importing local libraries containing spectral reference data. Supported import formats are `.ms`, `.mgf`, `.msp`, `.mat`, `.txt` (MassBank), `.mb`, `.json` (GNPS, MoNA). Spectra must be annotated with a structure and be centroided.
SIRIUS will automatically perform spectral library search against all available libraries whenever the molecular formula annotation workflow
is used. Spectral library matching is performed using the cosine score with squared peak intensities, ignoring the precursor peak.

### Spectral matching influence on SIRIUS and CSI:FingerID results

In SIRIUS 6, spectral library matches are added as annotations to CSI:FingerID results and do not influence the ranking of structure candidates. If a high-quality spectral library hit is found for a molecular formula that SIRIUS would not have otherwise considered, that molecular formula is forcibly added to the list of candidates.
This ensures that no spectral library matches are overlooked when using CSI:FingerID.


## Molecular formula annotation concepts

SIRIUS supports three different approaches for generating the set of candidate molecular formulas for feature annotation: *de novo*, database search, and bottom-up. 
A thorough understanding of these approaches is crucial for effectively applying the annotation [strategy](#molecular-formula-annotation-strategies) that aligns best with your task or research question.

It is equally important to understand how the molecular formula annotation step affects structure annotation and compound class prediction. Only those molecular formula candidates selected by the annotation strategy are used for structure annotation via database searching and compound class prediction in subsequent steps.

**IMPORTANT: Molecular formulas that are not included in the candidate set during this step are excluded from all subsequent processes.**

SIRIUS imposes penalties on candidate molecular formulas that significantly deviate from typical biomolecule compositions
(e.g., C<sub>2</sub>H<sub>2</sub>N<sub>12</sub>O<sub>12</sub> would incur a penalty). However, these penalties are applied cautiously: 
Only 2.6% of all molecular formulas in PubChem
compounds — and thus a small fraction of formulas not categorized as biomolecules — are penalized. SIRIUS never rewards molecular formulas. These penalty rules apply to all approaches. 

SIRIUS uses a concise list of outlier molecular formulas that would typically receive penalties under the aforementioned criteria due to their deviation from "biomolecule-like" compositions.
These formulas are not penalized, observed in metabolomics experiments (e.g., as solvents), are neither penalized nor rewarded.
However, during the computation of fragmentation trees, fragment annotations within MS/MS spectra—subformulas derived from these outlier molecular formulas—may still incur penalties.

### *De novo* annotation

SIRIUS considers all chemically feasible molecular formulas (considering valencies) that match the precursor mass of the
molecule/ion: For instance, if your query compound is pinensin A 
(C<sub>96</sub>H<sub>139</sub>N<sub>27</sub>O<sub>30</sub>S<sub>2</sub>,
monoisotopic mass of 2213.962 Da),  SIRIUS will evaluate all
19,746,670 candidate molecular formulas that match this
monoisotopic mass (based on a subset of elements - described below - and 10 ppm mass accuracy). 

Considering all molecular formulas implies specifying a set of elements from which these formulas are generated. SIRIUS
includes methods to [automatically detect elements](https://doi.org/10.1021/acs.analchem.6b01015) 
from the isotope and fragmentation pattern of the query compound.
Users should only manually adjust the element set if they have specific prior knowledge about the features of interest.
Expanding the element set unnecessarily  will result in  significantly longer computation times and an increase in incorrect annotations.
The default element set considered is C, H, N, O, P. The presence and abundance of  Cl, B, Se, S, and Br will
are automatically detected from the input isotope pattern in the MS1 spectrum.

**This approach can result in molecular 
formula annotations that may not match any entries in the structure database.**

### Formula database search

Instead of exploring the entire space of molecular formulas for a given mass and element set, one can alternatively confine the search to a specific database. In that case, SIRIUS exclusively considers molecular formulas included in the chosen database(s), with the option to further narrow down by specific element sets. Naturally, **this approach cannot annotate novel molecular formulas ("novel" defined as "not present in the selected database")** and significantly restricts the
pool of molecular formulas candidates. As this pool is much smaller than for the *de novo* method, this approach does not require a 
predefined element set. The database constrained approach is more likely to annotate formulas with unusual elements that are not recognizable from MS1 than the *de novo* approach.

### Bottom-up search

The "bottom-up" approach serves as a middle ground between the expansive molecular formula space of *de novo* annotation and the very constrained space of formula database search.
This method, inspired by [*Xing et al.*](https://doi.org/10.1038/s41592-023-01850-x), leverages each observed fragment's mass and corresponding root loss mass from the MS/MS spectrum to query a database of potential subformulas. Pairwise combinations of these resulting subformula candidates for fragments and root losses are then used to construct candidate formulas for the precursor molecule.
Thus, the range of precursor formula candidates generated depends on the fragments present in the spectrum. **Unlike strict restriction to database entries, this method can detect novel formulas that are combinations of two known molecular formulas.** And yet, relying on database entries, this approach generates a much smaller number of candidate formulas compared to *de novo* annotation, leading to a substantial speed up in computation time.

Given the constrains, strict limitations on an element subset are often unnecessary.
The formula database used for bottom-up searches typically includes formulas from the "bio" database along with a list of commonly observed losses.
Again, this approach is more likely to annotate formulas with unusual elements that are not recognizable from MS1 than the *de novo* approach.

## Molecular formula annotation strategies

The molecular formula annotations explained above can  be utilized either individually or combined. Selecting the appropriate molecular formula annotation
strategy is integral for a successful analysis.  Below we describe some standard strategies that cover most applications and serve as illustrative examples:

### *De novo* + bottom-up

**We recommend this approach for generic applications.** 

In the combined approach, features are categorized into "low" (m/z<400) and "high"(m/z>=400) mass features. Bottom-up search is conducted for both categories. For low mass features, SIRIUS also employs *de novo* molecular formula annotation to ensure comprehensive coverage. This dual strategy minimally impacts (increases) computation times compared to relying solely on bottom-up search.
The m/z threshold for categorization can be adjusted to align with runtime constraints and the computational capabilities of your local machine.
Element set constraints must be defined for *de novo* annotation and can optionally be applied to bottom-up search as well. 

### *De novo* only

**This approach is particularly suitable for discovering "unknown unknowns".**

The "de novo only" strategy should be employed when specifically expecting molecular formulas that cannot be detected by bottom-up search (i.e., the precursor
formula is not a combination of database subformulas). 
The expected element set needs to be well-defined and avoid including too many uncommon elements, as this can lead to a  combinatorial explosion of possible candidates for large masses (see the example in [de novo](#de-novo-annotation)).
The local machine running the SIRIUS client must be sufficiently powerful  to handle *de novo* annotation of higher mass compounds.

### Database search only

**This strategy should be employed only when the user is interested exclusively in features that have corresponding entries in a structure database and requires extremely fast computation times.**

As the database-only approach will only consider molecular formulas present in the selected databases it will not generate formula annotations without a structure database match.

### Bottom up only

The "Bottom up only" strategy can be used for a slight speed increase compared to the recommended combined approach. However, it does not offer significant advantages over the recommended strategy, as the drawbacks of *de novo* annotation are primarily relevant for high-mass compounds.

## Fragmentation trees

Fragmentation trees annotate the fragmentation spectrum with molecular
formulas, and identify likely losses between the ions in the
fragmentation spectrum. Fragmentation trees can be used to determine
the molecular formula of a query compound and to gain insights into its 
fragmentation. This fragmentation information is utilized in CSI:FingerID to
predict the molecular fingerprint of the query compound. 

Fragmentation
trees are computed directly from the fragmentation spectrum without the need for spectral libraries or molecular structure databases
(for the subtle "exemptions" from this rule, 
see [Böcker & Dührkop](https://dx.doi.org/10.1186%2Fs13321-016-0116-8)). 
Fragmentation trees are computed by combinatorial optimization; the underlying optimization
problem constitutes a Maximum Aposterior Estimator. The optimization
problem (finding a maximum colorful subtree) is NP-hard but nevertheless
solved optimally, explaining why computations sometimes require
significant running time for large molecules with rich fragmentation
spectra.

With SIRIUS 4.0, fragmentation tree computation has been significantly accelerated—approximately 36 times faster than the previous version—thanks to advanced [algorithm engineering](https://drops.dagstuhl.de/opus/volltexte/2018/9325/pdf/LIPIcs-WABI-2018-23.pdf). 
If you believe further speed improvements are necessary, we encourage you to cite our papers on swiftly computing fragmentation trees 
([Dührkop *et al.*](https://drops.dagstuhl.de/opus/volltexte/2018/9325/pdf/LIPIcs-WABI-2018-23.pdf);
[White *et al.*](https://doi.org/10.1007/978-3-319-21398-9_25); 
[Rauf *et al.*](https://doi.org/10.1089/cmb.2012.0083); 
[Böcker & Rasche](https://doi.org/10.1093/bioinformatics/btn270)). This recognition provides an incentive for us to continue enhancing our work in this area. 
We stress that the current version of
SIRIUS is **millions of times faster** than the initial version. In
fact, the initial version struggled to process more than 15 peaks in the fragmentation spectrum due to excessive running times *and* memory requirements.

Modeling fragmentation processes as a tree comes with two
restrictions: Namely, "pull-ups" and "parallelograms". 
 - A **pull-up** occurs when a fragment is placed too deep in the tree
   Due to ourcombinatorial optimization, SIRIUS tends to generate deep 
   trees, favoring many small fragmentation steps over fewer larger 
   ones. For example, SIRIUS will prefer three consecutive C<sub>2</sub>H<sub>2</sub> losses over a single C<sub>6</sub>H<sub>6</sub> 
   loss. While this does not affect the accuracy of molecular formula 
   identification, you should keep this side effect in mind when 
   interpreting fragmentation trees.
 - **Parallelograms** are consecutive fragmentation processes that can
   occur in more than one order: For instance, the precursor ion might 
   lose H<sub>2</sub>O followed by CO<sub>2</sub>, **but also** 
   CO<sub>2</sub> followed by H<sub>2</sub>O. SIRIUS will always choose
   one order for such fragmentation reactions, as this is the only valid way to model the fragmentation as a tree.

We have integrated support for experimental setups, such as MS<sup>E</sup>,
MS<sup>all</sup> and All Ion Fragmentation, where isotope peaks and
fragment peaks are measured together in the same spectrum. 
For these
experiments, SIRIUS offers a combined isotope and fragmentation
pattern analysis. However, be aware that SIRIUS assumes that only a single ion species is fragmented in each spectrum; it does not support the analysis of chimeric spectra, where multiple ion species are present simultaneously. 

For Data-Dependent Acquisition (DDA), fragmentation
spectra contain isotope patterns, which are disturbed through the mass filter, leading to non-trivial modifications of masses and intensities. Currently, SIRIUS does not use these altered isotope patterns in its analysis. Instead, it flags these peaks and ignores them during the optimization process.

## Molecular fingerprints

*Molecular fingerprints* can be used to encode the structural features of 
molecules. These fingerprints are typically represented as binary vectors 
of fixed length, where each bit describes the presence or absence of a particular, fixed
*molecular property*, such as the presence of a particular substructure.

One example is the **PubChem CACTVS fingerprint**, which consists of 881 bits.Molecular property 121 represents the presence of at least one
"unsaturated non-aromatic heteroatom-containing ring size 3". The properties are usually defined by SMARTS (SMiles ARbitrary Target
Specification) strings. For example, molecular property 357 corresponds to the SMARTS string  <span>\[#6\](&#126;\[#6\])(:c)(:n)</span>. This string describes a  central carbon atom connected to another carbon atom 
via any bond, to a third aromatic carbon atom via an aromatic bond, and 
to an aromatic nitrogen atom via an aromatic bond. 
For a complete description of the CACTVS fingerprint, refer to the official specification document [here](ftp://ftp.ncbi.nlm.nih.gov/pubchem/specifications/pubchem_fingerprints.pdf). We ignore all
molecular properties that can be derived from the molecular formula of
the query compound (i.e., bits 0 to 114 from PubChem CACTVS).

Given the molecular structure of a compound, we can straighforward compute its molecular fingerprint deterministically using the [Chemistry Development Kit
(CDK)](https://cdk.github.io/). 
[Heinonen *et al.*](https://doi.org/10.1093/bioinformatics/bts437) 
pioneered the idea of predicting a molecular 
fingerprint from from a compound's fragmentation spectrum. Prior to their work, only a limited number of hand-selected properties
(presence or absence of certain substructures) could be inferred from
fragmentation spectra — primarily in the context of GC-MS with Electron Ionization. For an illustrative example of this earlier approach, see [Curry & Rumelhart](https://doi.org/10.1016/0898-5529(90)90053-B).

Given the fragmentation spectrum and fragmentation tree of a query
compound, CSI:FingerID predicts its molecular fingerprint using Machine
Learning (linear Support Vector Machines). For detailed technical information, refer 
to [Shen *et al.*](https://dx.doi.org/10.1093%2Fbioinformatics%2Fbtu275) 
and [Dührkop *et al.*](https://doi.org/10.1073/pnas.1509788112). CSI:FingerID predicts multiple types of molecular fingerprints: CDK Substructure fingerprints, PubChem
CACTVS fingerprints, [Klekota-Roth fingerprints](https://doi.org/10.1093/bioinformatics/btn479), 
FP3 fingerprints, and MACCS fingerprints. In addition, CSI:FingerID predicts Extended Connectivity Fingerprints [ECFP2 and ECFP4](https://doi.org/10.1021/ci100050t) that appear sufficiently often in the training data.
Different from other fingerprints, ECFP are not encoded via SMARTS
matching. Instead, a hash function encodes the neighborhood of each atom
in the molecule. In principle, these fingerprints can encode
$2^{32} \approx 4.2 \cdot 10^9$ different substructures (molecular properties). In practice, it is possible but very unlikely that two substructures share
the same value, due to a hash collision.

CSI:FingerID predicts only those molecular properties that demonstrate 
reasonable prediction quality, as assessed through cross-validation (F<sub>1</sub> at least
(0.25), see below). In total, 3,215 molecular properties are
predicted by CSI:FingerID 1.1.

[//]: # (Update the exact number of molecular properties and optionally add some explantion on the new selection strategy)

CSI:FingerID  not only predicts whether a molecular property is absent (zero) or present (one), but also provides an **estimate of the certainty of this prediction**.
Mathematically speaking, it estimates the
posterior probability that the molecular property is present.
A high posterior probability (close to one) indicates high confidence that the property is present, while a low probability (close to zero) suggests confidence that the property is absent. Values between 0.1 and 0.9 indicate uncertainty about the presence or absence of the property.
These posterior probabilities are estimated using a method by 
[Platt](https://www.researchgate.net/profile/John_Platt/publication/2594015_Probabilistic_Outputs_for_Support_Vector_Machines_and_Comparisons_to_Regularized_Likelihood_Methods/links/004635154cff5262d6000000.pdf), 
hence they are referred to as "Platt probabilities". **However, even a high certainty (e.g., 99%) does not guarantee the presence of a molecular property.**
CSI:FingerID predicts thousands of molecular properties, and at a 99% 
confidence level, we still expect about 10 incorrect predictions out of 1000. Additionally, the estimation parameters are derived from the training 
data, so if the query molecules differ significantly from the training 
data, the estimates may be less accurate.
In addition to Platt probabilities, we also report the
performance of each molecular property classifier in cross validation:
The F<sub>1</sub> score is the harmonic mean of precision (fraction of
retrieved instances that are relevant) and recall (fraction of relevant
instances that have been retrieved). A classifier with an F<sub>1</sub> score close to one is generally more reliable than one with a score close to zero.  Again, these scores are based on cross-validation from the training data and should be interpreted with caution.

It is crucial to understand that the molecular fingerprint predicted by CSI:FingerID is not inherently connected to a specific structure in an existing molecular structure database. That
means that **even if the correct molecular structure is absent from all known structure databases, the predicted fingerprint remains valid**
within the limitations of the predictive power of the method. This allows users to hypothesize about the structure of "unknown unknowns" not present in any existing structure database. Use the `Predicted Fingerprints` view  in the Graphical User Interface (GUI) to examine the predicted molecular fingerprint in detail.

## Molecular structures

By default, SIRIUS searches for molecular structures in a biomolecule 
structure database. It can also search in the (extremely large) PubChem database or in custom "suspect databases" provided by the user.

  - When searching the [PubChem](https://pubchem.ncbi.nlm.nih.gov/) database, SIRIUS utilizes a local copy with precomputed molecular fingerprints. This  avoids the impracticality of computing fingerprints for candidate molecules "on the fly", which would be too time-consuming. The local copy of PubChem is periodically updated, and users can check the date of the latest update in the database dialog.

  - The *biomolecule structure database* (bioDB) is an aggregation of several structure databases containing small molecules of biological interest, including metabolites and other compounds of biological relevance, natural products, synthetic products with potential bioactivity, and contaminants observed in experiments.
    This biomolecule structure database consists of roughly the following datbases:
    [HMDB](https://hmdb.ca/),
    [KNApSAcK](http://www.knapsackfamily.com/KNApSAcK/),
    [CHEBI](https://www.ebi.ac.uk/chebi/),
    [KEGG](https://www.genome.jp/kegg/kegg1.html),
    [HSDB](https://pubchem.ncbi.nlm.nih.gov/source/11933),
    [MaConDa](https://www.maconda.bham.ac.uk/),
    [Biocyc](https://biocyc.org/),
    [GNPS](https://gnps.ucsd.edu/ProteoSAFe/libraries.jsp),
    [YMDB](http://www.ymdb.ca/),
    [Plantcyc](https://plantcyc.org/),
    [NORMAN](https://www.norman-network.com/nds/),
    [SuperNatural](http://bioinf-applied.charite.de/supernatural_new/index.php),
    [COCONUT](https://coconut.naturalproducts.net/),
    [BloodExposome](https://bloodexposome.org/),
    [TeroMol](http://terokit.qmclab.com/),
    [LOTUS](https://lotus.naturalproducts.net/),
    [FooDB](https://foodb.ca/),
    [MiMeDB](https://mimedb.org/),
    [LIPIDMAPS](https://www.lipidmaps.org/)
    and structures from [PubChem](https://pubchem.ncbi.nlm.nih.gov/) annotated with [MeSH](https://www.nlm.nih.gov/mesh/meshhome.html) 
    terms or  classified under "bio and metabolites", "drug", "safety and toxic" or "food". he exact composition of this database may vary depending on the version of SIRIUS in use.
    
## COSMIC - Confidence for Small Molecule IdentifiCations

The COSMIC confidence score, introduced by [Hoffmann *et al.*](https://doi.org/10.1038/s41587-021-01045-9), provides a measure of confidence for CSI:FingerID structure annotations.
While the CSI:FingerID score is designed to rank different structure candidates for a single feature, it is not well suited to rank the top hits across several features based on their likelihood of being correct.
The COSMIC confidence score adresses this gap by providing a standardized measure similar to False Discovery Rates and q-values:
higher confidence scores indicate a higher probability that the annotation is correct. 
This is particularly useful for high-throughput experiments. It allows for the analysis of all features in a large dataset
using CSI:FingerID, with COSMIC evaluating the top-ranked hit for each feature. The 
most reliable structure annotations can be selected for further analysis. Importantly, COSMIC does not re-rank
structure candidates of a particular feature nor does it discard any identifications; it simply provides an additional layer of confidence assessment.

The confidence score is calculated using Support Vector Machines (SVMs) with enforced feature directionality (different SVMs are applied based on the length of the structure candidate list). The resulting score is a Platt-probability estimate and thus, ranging from 0 to 1.0. However, it's important to note that this score should not be interpreted as a direct probability of correctness.
During evaluation, we found that a score of 0.64 approximately corresponded to a 10% FDR. However, this might vary significantly depending on the specific characteristics of your dataset.

### Confidence score modes

The confidence score can be evaluated in two modes: `exact` and `approximate`.

- The `exact` mode addresses the question "Is this exact molecular structure hit the true structure of my unknown compound?". It provides a confidence level for the precise match of the structure. In `exact` mode, the confidence score tends to be low when the top structure candidate and the second-best candidate are highly similar. This is common for well-studied molecules, where multiple derivatives often exist in the structure database.

- The `approximate` mode answers the question "Is this structure hit correct or highly similar to the true structure?". In this context,  _highly similar_ means that the hit is just one simple chemical reaction away from the true structure. Theoretical speaking, the hit and the true structure should have a Maximum Common Edge Subgraph ([MCES](https://en.wikipedia.org/wiki/Maximum_common_edge_subgraph)) distance of 2.
For example, a hit where only a side group has been repositioned compared to the true structure would still be considered "correct" in `approximate` mode. If you find nearly correct hits useful, it is advisable to use the `approximate` mode, which provides a higher confidence score for hits that are highly similar to the true structure.

## Expansive search (structure database search with fallback)

SIRIUS 6 introduces *expansive search*, which allows for structure database searches with a confidence score-based fallback.
Structure database search is conducted within the user-selected databases ("requested databases"), and then additionally for "PubChem". If the top hit in PubChem has a confidence score at least twice as high as the confidence 
score of the top hit from the requested databases, the search will be "expanded" and the results from PubChem will be displayed. 

## Compound classes

CANOPUS ([Dührkop *et al.*](https://doi.org/10.1038/s41587-020-0740-8)) predicts the presense/absense of more than 2500 _compound classes_. 
These classes range from very broad categories, such as "Lipids and lipid-like molecules," to highly specific ones, like "Phosphatidylethanolamines," "Thiazolidines," or "7-alpha-hydroxysteroids."
Most of the compound classes are derived from the [ClassyFire ontology](https://doi.org/10.1186/s13321-016-0174-y). Unlike ClassyFire, however, 
CANOPUS predicts these classes based solely on MS/MS data and without 
requiring database information. This means it can identify a class even if 
no molecular structure of that class exists in the molecular structure 
database.

It is important to note that CANOPUS's classification does not adhere to 
the concept of attributing a compound to its biosynthetic precursor or 
pathway. Instead, it categorizes compounds based on functional groups and 
common substructures. Without additional knowledge about the measured 
organism, the MS/MS spectrum alone cannot determine the biochemical origin 
of a compound class; the same compound may be derived from different 
biosynthetic precursors.  

Additionally, CANOPUS predicts compound classes based on the categories 
from [NPClassifier](https://doi.org/10.1021/acs.jnatprod.1c00399). This 
classification system is more general, but may aligns better with the 
concept of biosynthetic pathway mapping. However, it is still not using 
taxonomic information, relying solely on MS/MS data for its predictions.

## MSNovelist

MSNovelist ([Stravs *et al.*](https://doi.org/10.1038/s41592-022-01486-3)) generates molecular structures *de novo* from  MS/MS data - without relying on any database. 
This makes it particularly useful for analyzing poorly represented analyte classes and novel compounds, where traditional database searches may fall short. 
However, it is not intended to replace database searches altogether, as structural elucidation of small molecules from MS/MS data remains a challenging task, and identifying a structure without database candidates is even more difficult.

MSNovelist generates structures which can serve as a great starting point for elucidation of specific unknowns.
MSNovelist generates multiple candidate structures from the predicted molecular fingerprint. These candidates are represented in SMILES format and are sampled using an autoregressive model, which generates each SMILES token by token. Once the candidate structures are generated, they are ranked using CSI:FingerID. 
The proposed structures can serve as an excellent starting point for the elucidation of specific unknown compounds. This information can be further enriched by [CANOPUS](#compound-classes) compound class predictions, providing a broader context for the identified structures.

## Training data

The fragmentation tree computation of SIRIUS is not based on machine learning and therefore *does not involve any training data*.
Instead, the parameters for this step were estimated using
two MS/MS spectral
datasets: one comprising 2005 compounds from GNPS  and another with 2046 compounds from Agilent
("MassHunter Forensics/Toxicology PCDL" version B.04.01 from Agilent
Technologies Inc., Santa Clara, CA, USA). Parameters of this step were
not optimized to maximize, say, the molecular formula identification
rate, and estimates should be very robust. While all spectra used for parameter estimation were recorded in
positive ion mode, fragmentation tree computation and molecular formula
estimation appear to work very well for negative ion mode data as well;
though this cannot be guaranteed.

The machine learning component of CSI:FingerID, namely the essemble of linear
Support Vector Machines, is trained on spectra from NIST, Massbank and GNPS.
An up-to-date list of all structures included in the CSI:FingerID training
data can be downloaded from the webservice:

##### Training structures for positive ion mode:
<https://www.csi-fingerid.uni-jena.de/v1.4.8/api/fingerid/trainingstructures?predictor=1>
##### Training structures for negative ion mode:
<https://www.csi-fingerid.uni-jena.de/v1.4.8/api/fingerid/trainingstructures?predictor=2>

**We would like to explicitly extend our heartfelt thanks to everyone who has made
their spectra publically available.** Your contributions have greatly benefited not only us, but the entire metabolomics community.
Unfortunately, the importance of open data sharing is not yet fully recognized within our field. 
We sincerely hope that, like the genomics community 25 years ago and the proteomics community 10 years ago, the metabolomics community will 
soon recognize 
the urgent need for open data and data sharing. We believe that you will receive the well-deserved recognition for your contributions in the near future.

We continuously add new training data as it becomes publically
available. If you have data from reference compounds, we encourage you to
upload it to a public database such as [GNPS](https://gnps.ucsd.edu/ProteoSAFe/static/gnps-splash.jsp) 
or [MassBank](https://massbank.eu/MassBank/). If public sharing  is
not possible for any reason, **you can contact us so we can include
your data in the CSI:FingerID training set while maintaining its confidentiality**. Your support in providing additional training data is invaluable in improving the performance of CSI:FingerID!