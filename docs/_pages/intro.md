---
permalink: /
title: "Welcome"
---

**Welcome to the official online documentation for SIRIUS -- a *Java* software for the analysis of small molecules from tandem mass spectrometry data.**

**Quick Start:** For getting started quickly, see the [quick-start guide]({{ "/quick-start/" | relative_url }}).

**Licenses:** As of SIRIUS 5, a user account and license are required to use the webservice-based features of SIRIUS. 
[Find out more about academic and non-academic user accounts.]({{"/account-and-license/" | relative_url}})

**Tutorials:** Check our [YouTube Playlist](https://youtube.com/playlist?list=PL8R4DKiWsw-tIG8w3hZWJunWZyy-qnIZM&si=xcaOZ3PILt7I2UB5) to find tutorials and learning resources. Whether you are new to SIRIUS or looking to expand your knowledge, this playlist has you covered.

**Community Forum:** For community support and discussions, join our [SIRIUS Community space on Gitter](https://matrix.to/#/#sirius-ms:gitter.im). 
Here, you can connect with other users and get help from the community. Entering the Community, you can join different rooms, e.g. *Troubleshooting* to get assistance from other SIRIUS users.

**Bug Reports and Feature Requests:** If you encounter any problems or have suggestions for new 
features, please submit them via [GitHub](https://github.com/sirius-ms/sirius/issues/new/choose).
Please ensure to submit all [required information]({{ "/bugs/#templates" | relative_url}}). 

**SIRIUS Integration:** SIRIUS can be easily integrated into existing workflows and provides 
interfaces for both manual and fully automated analysis. If you're looking for help with 
integrating SIRIUS into your workflow or want to share tips and code snippets, visit our 
[SIRIUS Community space on Gitter](https://matrix.to/#/#sirius-ms:gitter.im).

[**Help us improve the SIRIUS Documentation!**](https://github.com/sirius-ms/sirius-docs.github.io)

## SIRIUS introduction {#SIRIUS-Introduction}

The primary focus of SIRIUS is the structure elucidation of novel
molecules (drug leads, leachables or contaminants in synthesis or food, new psychoactive substances, PFAS, transformation products,...), but it is also
well equipped to handle more standard tasks such as dereplication of known structures.

It combines the analysis of isotope patterns in MS
spectra with the analysis of fragmentation patterns in MS/MS spectra,
and uses CSI:FingerID as a web service for searching molecular structure
databases. It also integrates CANOPUS for *de novo* compound class prediction and
MSNovelist for *de novo* structure generation.

SIRIUS is built for the analysis of **small molecules**, usually below 1000 Da, from all compound classes **except polymers** (e.g. peptides). 

SIRIUS requires **high mass accuracy** data. The mass deviation of your
MS and MS/MS spectra should be within 20 ppm. Mass spectrometry
instruments such as TOF, Orbitrap and FT-ICR typically provide high mass
accuracy data, as do coupled instruments such as Q-TOF, IT-TOF or
IT-Orbitrap. Spectra measured with a quadrupole or linear trap do not
provide the high mass accuracy that is required for our method. See [Mass deviations]({{ "/advanced-background-information/#mass-deviations" | relative_url }})  for a detailed explanation what "mass accuracy" means in SIRIUS.

SIRIUS expects **MS and MS/MS** spectra as input. It is possible to omit
the MS data, but this will make the analysis more time-consuming and may
give you poorer results. In this case, you should consider restricting the
candidate molecular formulas to those found in PubChem.

SIRIUS is designed to work off-the-shelf only for **Data Dependent Acquisition** (DDA) data with no or very few chimerics. 
While SIRIUS has been applied to Data Independent Acquisition (DIA) data, too, it's important to note that our methods were not specifically developed for DIA and may perform less effectively in that context. 
For DIA data, spectra must first be deconvoluted using other software (e.g., vendor-specific tools or MSDial) before being imported into SIRIUS for analysis.

SIRIUS expects **processed peak lists** (centroided spectra). It does
not provide routines for peak picking from profiled spectra. This is a deliberate
design choice: We want you to use the best peak picking software available — or alternatively your favorite software. There are several
tools that specialise in this task, such as [OpenMS](https://www.openms.de/), 
[MZmine](http://mzmine.github.io/) or [XCMS](https://github.com/sneumann/xcms). 
**See our video tutorials on how to preprocess your data for SIRIUS
with [OpenMS](https://www.youtube.com/watch?v=ZTEY8_fnuZE) or 
[MZmine](https://www.youtube.com/watch?v=Q0D6q9xQLSE)**.

However, SIRIUS also provides a zero parameter
pre-processing tool to import LCMS-Runs directly from `.mzml` (or `.mzxml`) format 
to help you get started quickly. Most modern MS vendor instruments are able to
export measured data from their native format to .mzML. **Alternatively, watch this 
[video tutorial](https://www.youtube.com/watch?v=xnjvZlSlp40) how to use
[MSconvert/ProteoWizard](http://proteowizard.sourceforge.net/index.html) to convert your vendor formats to `mzml` for SIRIUS.** 

**SIRIUS** identifies the molecular formula of the measured precursor
ion, and annotates the spectrum by providing a molecular
formula for each fragment peak. Peaks are assumed to be noise peaks if they are not annotated. Furthermore, a **fragmentation tree** is
predicted that contains the predicted fragmentation reactions
leading to the fragment peaks.
* For more details, consult the [method background]({{"/methods-background/#SIRIUS-molecular-formula" | relative_url}}) or watch the [Behind the Scenes](https://www.youtube.com/watch?v=EpDKLzG0qVc) talk.
* Try using SIRIUS in the [GUI]({{"/gui/#SIRIUS-molecular-formula" | relative_url}}) or [CLI]({{"/cli/#SIRIUS-formulas" | relative_url}}).

**ZODIAC** improves the ranking of the formula candidates provided by SIRIUS. It 
re-ranks the candidates by taking into account joint fragments 
and losses between fragmentation trees of different compounds in a data set.

* For more details, consult the [method background]({{"/methods-background/#ZODIAC" | relative_url}}) or watch the [Behind the Scenes](https://www.youtube.com/watch?v=EpDKLzG0qVc) talk.
* Try using ZODIAC in the [GUI]({{"/gui/#ZODIAC-ranking" | relative_url}}) or [CLI]({{"/cli/#ZODIAC" | relative_url}}).

**CSI:FingerID** identifies the structure of a compound by
searching in a molecular structure database. Here and in the following,
"*structure*" refers to the identity and connectivity (with bond
multiplicities) of the atoms, but **not** to stereochemistry information.
Elucidation of stereochemistry is currently beyond the power of
automated search engines.

* For more details, consult the [method background]({{"/methods-background/#molecular-fingerprint" | relative_url}}) or watch the [CSI:FingerID Behind the Scenes](https://www.youtube.com/watch?v=oEQoB2aaV2E) talk.
* Try using CSI:FingerID in the [GUI]({{"/gui/#CSIFingerID-structure" | relative_url}}) or [CLI]({{"/cli/#CSIFingerID-structures" | relative_url}}).

**COSMIC confidence score** assigns a confidence to CSI:FingerID structure identifications.
The idea is similar to False Discovery Rates: It allows to run CSI:FingerID in high-throughput 
on thousands of compounds and select the most confident identifications. The workflow of generating 
a structure database, searching with CSI:FingerID and ranking hits by confidence score is called the COSMIC workflow.
Simplify your data interpretation workflow by first identifying the most confident compounds in your sample and then using them 
to generate knowledge or hypotheses.

* For more details, consult the [method background]({{"/methods-background/#COSMIC" | relative_url}}) or watch the [COSMIC Behind the Scenes](https://www.youtube.com/watch?v=akMkDK5O2rk) talk.
* COSMIC is part of the structure database search in the [GUI]({{"/gui/#CSIFingerID-structure" | relative_url}}) and [CLI]({{"/cli/#CSIFingerID-structures" | relative_url}}).
 
**CANOPUS** predicts compound classes from the molecular fingerprint predicted by CSI:FingerID 
without any database searching. It therefore provides structural information for compounds 
for which neither spectral nor structural reference data are available.

* For more details, consult the [method background]({{"/methods-background/#CANOPUS" | relative_url}}) or watch the [CANOPUS Behind the Scenes](https://www.youtube.com/watch?v=kgTS5hGiPXg) talk.
* Try using CANOPUS in the [GUI]({{"/gui/#CSIFingerID-CANOPUS" | relative_url}}) or [CLI]({{"/cli/#CANOPUS-classes" | relative_url}}).

**MSNovelist** generates de novo structure candidates to overcome the limitations of structure database searching.
Structures are generated based on molecular formula and fingerprint.

* For more details, consult the [method background]({{"/methods-background/#MSNovelist" | relative_url}}).
* Try using MSNovelist in the [GUI]({{"/gui/#MSNovelist-denovo-structure" | relative_url}}) or [CLI]({{"/cli/#MSNovelist-denovo-structures" | relative_url}}).

SIRIUS comes with a [**Graphical User Interface** (GUI)]({{"/quick-start/#GUI" | relative_url}}), a [**Command Line
Interface** (CLI)]({{ "/quick-start/#CLI" | relative_url }}) and an [**Application Programming Interface** (API)]({{"/quick-start/#API" | relative_url}}) that comes with a client in Python.
All these interfaces share the same persistence layer, allowing for high-throughput computation using e.g. the CLI
on a compute cluster and then manual inspection of selected results using the GUI.

## Literature {#literature}

The *scientific development* behind SIRIUS, ZODIAC, CSI:FingerID, CANOPUS, and MSNovelist started in 2005 and has required over 50 person-years (and counting) of PhD students, post-docs and principal investigators. And we're not even talking about the development of the shiny graphical user interface introduced in version 3.1. But it is not the GUI or
software development that does the work here; it is our scientific
research that has made SIRIUS, ZODIAC, CSI:FingerID, CANOPUS, and MSNovelist possible. 
 It goes without saying that 20 years of work cannot be described in a single paper.

Please cite all papers that you feel are relevant to your work. Please do
**not** cite this manual or the SIRIUS or CSI:FingerID website, but 
our scientific papers.

### SIRIUS 4 {#lit-SIRIUS4}

 - Kai Dührkop, Markus Fleischauer, Marcus Ludwig, Alexander A. Aksenov, Alexey V. Melnik, Marvin Meusel, Pieter C. Dorrestein, Juho Rousu, and Sebastian Böcker.
[**Sirius 4: turning tandem mass spectra into metabolite structure information.**](https://doi.org/10.1038/s41592-019-0344-8)
*Nat Methods*, 2019.

### CSI:FingerID – searching in molecular structure databases {#lit-CSIFingerID}

  - Kai Dührkop, Huibin Shen, Marvin Meusel, Juho Rousu and Sebastian
    Böcker. [**Searching molecular structure databases with tandem mass
    spectra using CSI:FingerID.**](https://doi.org/10.1073/pnas.1509788112) *Proc Natl Acad Sci U S A*, 2015.

  - Huibin Shen, Kai Dührkop, Sebastian Böcker and Juho Rousu.
    [**Metabolite Identification through Multiple Kernel Learning on
    Fragmentation Trees.**](https://doi.org/10.1093/bioinformatics/btu275) *Bioinformatics*, 2014.
    Proc. of *Intelligent Systems for Molecular Biology* (ISMB 2014).

### COSMIC confidence score {#lit-COSMIC}
  - Martin A. Hoffmann, Louis-Félix Nothias, Marcus Ludwig, Markus Fleischauer, Emily C. Gentry, Michael Witting, Pieter C. Dorrestein, Kai Dührkop and Sebastian Böcker. [**High-confidence structural annotation of metabolites absent from spectral libraries.**](https://doi.org/10.1038/s41587-021-01045-9) *Nat Biotechnol*, 2022.

### CANOPUS – compound class prediction {#lit-CANOPUS}
 - Kai Dührkop, Louis-Félix Nothias, Markus Fleischauer, Raphael Reher, Marcus Ludwig, Martin A. Hoffmann, Daniel Petras, William H. Gerwick, Juho Rousu, Pieter C. Dorrestein and Sebastian Böcker.
[**Systematic classification of unknown metabolites using high-resolution fragmentation mass spectra.**](https://doi.org/10.1038/s41587-020-0740-8)
*Nat Biotechnol*, 2020.

### MSNovelist – *de novo* structure generation {#lit-MSNovelist}

- Michael A. Stravs, Kai Dührkop, Sebastian Böcker and Nicola Zamboni. [**MSNovelist: de novo structure generation from mass spectra.**](https://doi.org/10.1038/s41592-022-01486-3) *Nat Methods*, 2022. 


### ZODIAC – molecular formula annotation {#lit-ZODIAC}

 - Marcus Ludwig, Louis-Félix Nothias, Kai Dührkop, Irina Koester, Markus Fleischauer, Martin A. Hoffmann, Daniel Petras, Fernando Vargas, Mustafa Morsy, Lihini Aluwihare, Pieter C. Dorrestein, Sebastian Böcker.
[**Database-independent molecular formula annotation using Gibbs sampling through ZODIAC.**](https://doi.org/10.1038/s42256-020-00234-6)
*Nat Mach Intell*, 2020.

### Fragmentation tree computation {#lit-fragtree}

  - Sebastian Böcker and Kai Dührkop. [**Fragmentation trees reloaded.**](https://doi.org/10.1007/978-3-319-16706-0_10)
    *J Cheminform*, 2016.

  - W. Timothy J. White, Stephan Beyer, Kai Dührkop, Markus Chimani and
    Sebastian Böcker. [**Speedy Colorful Subtrees.**](https://doi.org/10.1007/978-3-319-21398-9_25) In *Proc. of
    Computing and Combinatorics Conference* (COCOON 2015), volume 9198
    of *Lect Notes Comput Sci*, 2015.

  - Imran Rauf, Florian Rasche, François Nicolas and Sebastian Böcker.
    [**Finding Maximum Colorful Subtrees in practice.**](https://doi.org/10.1089/cmb.2012.0083) *J Comput Biol*, 2013.

  - Florian Rasche, Aleš Svatoš, Ravi Kumar Maddula, Christoph Böttcher
    and Sebastian Böcker. [**Computing fragmentation trees from tandem
    mass spectrometry data.**](https://doi.org/10.1021/ac101825k) *Anal Chem*, 2011.

  - Sebastian Böcker and Florian Rasche. [**Towards de novo
    identification of metabolites by analyzing tandem mass spectra.**](https://doi.org/10.1093/bioinformatics/btn270)
    *Bioinformatics*, 2008.

### Isotope pattern analysis {#lit-isotope}

  - Sebastian Böcker, Matthias C. Letzel, Zsuzsanna Lipták and Anton
    Pervukhin. [**SIRIUS: decomposing isotope patterns for metabolite
    identification.**](https://doi.org/10.1093/bioinformatics/btn603) *Bioinformatics*, 2009.

  - Sebastian Böcker, Matthias Letzel, Zsuzsanna Lipták and Anton
    Pervukhin. [**Decomposing metabolomic isotope patterns.**](https://doi.org/10.1007/11851561_2) In *Proc.
    of Workshop on Algorithms in Bioinformatics* (WABI 2006), volume
    4175 of *Lect Notes Comput Sci*, 2006.

### Passatutto – Fragmentation tree based decoy spectra {#lit-passatutto}

  - Kerstin Scheubert, Franziska Hufsky, Daniel Petras, Mingxun Wang,
    Louis-Felix Nothias, Kai Dührkop, Nuno Bandeira, Pieter C.
    Dorrestein, Sebastian Böcker. [**Significance estimation for large
    scale metabolomics annotations by spectral matching.**](https://doi.org/10.1038/s41467-017-01318-5) 
    *Nat Commun*, 2017

### Auto-detection of elements {#lit-elements}

  - Marvin Meusel, Franziska Hufsky, Fabian Panter, Daniel Krug, Rolf
    Müller and Sebastian Böcker. [**Predicting the presence of uncommon
    elements in unknown biomolecules from isotope patterns.**](https://doi.org/10.1021/acs.analchem.6b01015) *Anal
    Chem*, 2016.

### Mass decomposition {#lit-mass-decomp}

  - Kai Dührkop, Marcus Ludwig, Marvin Meusel and Sebastian Böcker.
    [**Faster mass decomposition.**](https://doi.org/10.1007/978-3-642-40453-5_5) In *Proc. of Workshop on Algorithms
    in Bioinformatics* (WABI 2013), volume 8126 of *Lect Notes Comput
    Sci*, 2013.

  - Sebastian Böcker and Zsuzsanna Lipták. [**A fast and simple algorithm
    for the Money Changing Problem.**](https://doi.org/10.1007/s00453-007-0162-8) *Algorithmica*, 2007.

  - Sebastian Böcker and Zsuzsanna Lipták. [**Efficient Mass
    Decomposition.**](https://doi.org/10.1145/1066677.1066715) In *Proc. of ACM Symposium on Applied Computing*
    (ACM SAC 2005), 2005.
