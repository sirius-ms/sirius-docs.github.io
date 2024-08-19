---
permalink: /advanced-background-information/
title: "Advanced background information"
---

## Spectral quality {#spectral-quality}

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

## Monoisotopic masses {#monoisotopic-masses}

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

## Theoretical masses of ions {#theoretical-masses}

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

### Isotopes with masses and abundances as used by SIRIUS {#isotopes}
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



## Mass deviations {#mass-deviations}

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

## Adducts {#adducts}

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

## Training data {#training-data}

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

##### Training structures for positive ion mode: {#training-positive}
<https://www.csi-fingerid.uni-jena.de/v1.4.8/api/fingerid/trainingstructures?predictor=1>
##### Training structures for negative ion mode: {#training-negative}
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