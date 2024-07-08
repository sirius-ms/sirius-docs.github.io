---
permalink: /
title: "Welcome"
---

**Welcome to the official online documentation for SIRIUS -- a *Java* software for the analysis of small molecules from tandem mass
spectrometry data.**

**Quick Start:** For getting started quickly, see the [quick-start guide]({{ "/quick-start/" | relative_url }}).

**Licenses:** As of SIRIUS 5, a user account and license are required to use the webservice-based features of SIRIUS. 
[Find out more about academic and non-academic user accounts.]({{"/account-and-license/" | relative_url}})

**Tutorials:** Check our [YouTube Playlist](https://youtube.com/playlist?list=PL8R4DKiWsw-tIG8w3hZWJunWZyy-qnIZM&si=xcaOZ3PILt7I2UB5) to find tutorials and learning resources. Whether you are new to SIRIUS or looking to expand your knowledge, this playlist has you covered.

**Community Forum:** For community support and discussions, join our [SIRIUS Community space on Gitter](https://matrix.to/#/#sirius-ms:gitter.im). 
Here, you can connect with other users and get help from the community.

**Bug Reports and Feature Requests:** If you encounter any issues or have suggestions for new 
features, kindly submit them via [GitHub](https://github.com/sirius-ms/sirius/issues/new/choose).

**SIRIUS Integration:** SIRIUS can be easily integrated into existing workflows and provides 
interfaces for both manual and fully automated analysis. If you're looking for help with 
integrating SIRIUS into your workflows or want to share tips and code snippets, visit our 
[SIRIUS Community space on Gitter](https://matrix.to/#/#sirius-ms:gitter.im).

[**Help us improve the SIRIUS Documentation!**](https://github.com/sirius-ms/sirius-docs.github.io)

## SIRIUS introduction

The primary focus of SIRIUS is the structure elucidation of novel
molecules (drug leads, contaminants in synthesis or food), but it is also
well equipped to handle more standard tasks such as dereplication of known structures.

It combines the analysis of isotope patterns in MS
spectra with the analysis of fragmentation patterns in MS/MS spectra,
and uses CSI:FingerID as a web service for searching molecular structure
databases. It also integrates CANOPUS for *de novo* compound class prediction and
MSNovelist for *de novo* structure generation.

SIRIUS requires **high mass accuracy** data. The mass deviation of your
MS and MS/MS spectra should be within 20 ppm. Mass Spectrometry
instruments such as TOF, Orbitrap and FT-ICR usually provide high mass
accuracy data, as well as coupled instruments like Q-TOF, IT-TOF or
IT-Orbitrap. Spectra measured with a quadrupole or linear trap do not
provide the high mass accuracy that is required for our method. See [Mass deviations]({{ "/advanced-background-information/#mass-deviations" | relative_url }}) on what "mass accuracy" means in
detail for SIRIUS.

SIRIUS expects **MS and MS/MS** spectra as input. It is possible to omit
the MS data, but it will make the analysis more time-consuming and might
give you worse results. In this case, you should consider limiting the
candidate molecular formulas to those found in PubChem.

SIRIUS expects **processed peak lists** (centroided spectra). It does
not contain routines for peak picking from profiled spectra. This is a deliberate
design decision: We want you to use the best peak picking software out
there — or alternatively, your favorite software. There are several
tools specialized for this task, such as [OpenMS](https://www.openms.de/), 
[MZmine](http://mzmine.github.io/) or [XCMS](https://github.com/sneumann/xcms). 
**See our video tutorials on how to preprocess tour data for SIRIUS
with [OpenMS](https://www.youtube.com/watch?v=ZTEY8_fnuZE) or 
[MZmine](https://www.youtube.com/watch?v=Q0D6q9xQLSE)**.

However, SIRIUS also contains a zero parameter
preprocessing tool to directly import LCMS-Runs from `.mzml` (or `mzxml`) format 
to help you get started quickly. Most modern MS vendor instruments are able to
export measured data from their native format to .mzML. Alternatively, **see how to use 
[MSconvert/ProteoWizard](http://proteowizard.sourceforge.net/index.html) to convert your vendor formats to `mzml` for SIRIUS in this 
[video tutorial](https://www.youtube.com/watch?v=xnjvZlSlp40)**. 

**SIRIUS** will identify the molecular formula of the measured precursor
ion, and will also annotate the spectrum by providing a molecular
formula for each fragment peak. Peaks that receive no annotation are
assumed to be noise peaks. Furthermore, a **fragmentation tree** is
predicted; this tree contains the predicted fragmentation reaction
leading to the fragment peaks.

**ZODIAC** improves the ranking of the formula candidates provided by SIRIUS. It 
re-ranks the candidates by considering joint fragments 
and losses between fragmentation trees of different compounds in a data set.

**CSI:FingerID** identifies the structure of a compound by
searching in a molecular structure database. Here and in the following,
"*structure*" refers to the identity and connectivity (with bond
multiplicities) of the atoms, but **no** stereochemistry information.
Elucidation of stereochemistry is currently beyond the power of
automated search engines.

**COSMIC confidence score** assigns a confidence to CSI:FingerID structure identifications.
The idea is similar to False Discovery Rates: It allows to run CSI:FingerID in high-throughput 
on thousands of compounds and select the most confident identifications. The workflow of generating 
a structure database, searching with CSI:FingerID and ranking hits by confidence score is termed the COSMIC workflow.
Make your data interpretation workflow easier by first identifying the most confident compounds in your sample, then use them 
to generate knowledge or hypotheses.

**CANOPUS** predicts compound classes from the molecular fingerprint predicted by CSI:FingerID 
without any database search involved. Hence, it provides structural information for compounds 
for which neither spectral nor structural reference data are available.

**MSNovelist** generates de novo structure candidates to help overcome the limits of structure database search.
Structures are generated based on molecular formula and fingerprint.

SIRIUS ships with a **Graphical User Interface** (GUI), a **Command Line
Interface** (CLI) and an API that comes with a client in Python.

All these interfaces share the same persistence layer, allowing for high-throughput computation using e.g. the CLI
on a compute cluster and then manual inspection of selected results using the GUI.

## Literature

The *scientific development* behind SIRIUS, ZODIAC, CSI:FingerID and CANOPUS required
numerous man-years of PhD students, postdocs and principal
investigators; an educated guess would be roughly 35 man-years. This
estimate does not include building the shiny Graphical User Interface
that was introduced in version 3.1. But it is not the user interface or
software development that does the work here; it is our scientific
research that made SIRIUS, ZODIAC, CSI:FingerID and CANOPUS possible. 
It is understood that the work of 15 years cannot be described in a single paper.

Please cite all papers that you feel relevant for your work. Please do
**not** cite this manual or the SIRIUS or CSI:FingerID website, but rather
our scientific papers.

### SIRIUS 4

 - Kai Dührkop, Markus Fleischauer, Marcus Ludwig, Alexander A. Aksenov, Alexey V. Melnik, Marvin Meusel, Pieter C. Dorrestein, Juho Rousu, and Sebastian Böcker.
[**Sirius 4: turning tandem mass spectra into metabolite structure information.**](https://doi.org/10.1038/s41592-019-0344-8)
*Nat Methods*, 2019.

### CANOPUS – Compound Class Prediction 
 - Kai Dührkop, Louis-Félix Nothias, Markus Fleischauer, Raphael Reher, Marcus Ludwig, Martin A. Hoffmann, Daniel Petras, William H. Gerwick, Juho Rousu, Pieter C. Dorrestein and Sebastian Böcker.
[**Systematic classification of unknown metabolites using high-resolution fragmentation mass spectra.**](https://doi.org/10.1038/s41587-020-0740-8)
*Nat Biotechnol*, 2020.

### ZODIAC – molecular formula annotation

 - Marcus Ludwig, Louis-Félix Nothias, Kai Dührkop, Irina Koester, Markus Fleischauer, Martin A. Hoffmann, Daniel Petras, Fernando Vargas, Mustafa Morsy, Lihini Aluwihare, Pieter C. Dorrestein, Sebastian Böcker.
[**Database-independent molecular formula annotation using Gibbs sampling through ZODIAC.**](https://doi.org/10.1038/s42256-020-00234-6)
*Nat Mach Intell*, 2020.

### CSI:FingerID – Searching in molecular structure databases

  - Kai Dührkop, Huibin Shen, Marvin Meusel, Juho Rousu and Sebastian
    Böcker. [**Searching molecular structure databases with tandem mass
    spectra using CSI:FingerID.**](https://doi.org/10.1073/pnas.1509788112) *Proc Natl Acad Sci U S A*, 2015.

  - Huibin Shen, Kai Dührkop, Sebastian Böcker and Juho Rousu.
    [**Metabolite Identification through Multiple Kernel Learning on
    Fragmentation Trees.**](https://doi.org/10.1093/bioinformatics/btu275) *Bioinformatics*, 2014.
    Proc. of *Intelligent Systems for Molecular Biology* (ISMB 2014).

### COSMIC confidence score

- **PREPRINT:** Martin A. Hoffmann, Louis-Félix Nothias, Marcus Ludwig, Markus Fleischauer, Emily C. Gentry, Michael Witting, Pieter C. Dorrestein, Kai Dührkop and Sebastian Böcker. 
  [**Assigning confidence to structural annotations from mass spectra with COSMIC**](https://doi.org/10.1101/2021.03.18.435634) *bioRxiv*, 2021.

### Fragmentation Tree Computation

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

### Isotope pattern analysis

  - Sebastian Böcker, Matthias C. Letzel, Zsuzsanna Lipták and Anton
    Pervukhin. [**SIRIUS: decomposing isotope patterns for metabolite
    identification.**](https://doi.org/10.1093/bioinformatics/btn603) *Bioinformatics*, 2009.

  - Sebastian Böcker, Matthias Letzel, Zsuzsanna Lipták and Anton
    Pervukhin. [**Decomposing metabolomic isotope patterns.**](https://doi.org/10.1007/11851561_2) In *Proc.
    of Workshop on Algorithms in Bioinformatics* (WABI 2006), volume
    4175 of *Lect Notes Comput Sci*, 2006.

### Passatutto – Fragmentation tree based decoy spectra

  - Kerstin Scheubert, Franziska Hufsky, Daniel Petras, Mingxun Wang,
    Louis-Felix Nothias, Kai Dührkop, Nuno Bandeira, Pieter C.
    Dorrestein, Sebastian Böcker. [**Significance estimation for large
    scale metabolomics annotations by spectral matching.**](https://doi.org/10.1038/s41467-017-01318-5) 
    *Nat Commun*, 2017

### Auto-detection of elements

  - Marvin Meusel, Franziska Hufsky, Fabian Panter, Daniel Krug, Rolf
    Müller and Sebastian Böcker. [**Predicting the presence of uncommon
    elements in unknown biomolecules from isotope patterns.**](https://doi.org/10.1021/acs.analchem.6b01015) *Anal
    Chem*, 2016.

### Mass decomposition

  - Kai Dührkop, Marcus Ludwig, Marvin Meusel and Sebastian Böcker.
    [**Faster mass decomposition.**](https://doi.org/10.1007/978-3-642-40453-5_5) In *Proc. of Workshop on Algorithms
    in Bioinformatics* (WABI 2013), volume 8126 of *Lect Notes Comput
    Sci*, 2013.

  - Sebastian Böcker and Zsuzsanna Lipták. [**A fast and simple algorithm
    for the Money Changing Problem.**](https://doi.org/10.1007/s00453-007-0162-8) *Algorithmica*, 2007.

  - Sebastian Böcker and Zsuzsanna Lipták. [**Efficient Mass
    Decomposition.**](https://doi.org/10.1145/1066677.1066715) In *Proc. of ACM Symposium on Applied Computing*
    (ACM SAC 2005), 2005.
