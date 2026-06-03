---
permalink: /news/
title: "What's new in SIRIUS"
---

## What's new in SIRIUS 6.3.4 {#SIRIUS-634}
[Read the full changelog (2026-03-21) here.]({{ "/changelog/#634-2026-03-21" | relative_url }})

[**Downloadable Libraries:**]({{ "/gui/#custom-database-import" | relative_url }}) A curated spectral library containing high-quality public spectra from major repositories—including GNPS (Global Natural Products Social Molecular Networking), MassBank, and MsnLib—is available for direct download and integration.

[**Transformation Product Database Export:**]({{ "/gui/#custom-database-import" | relative_url }})
This exporter converts BioTransformer outputs into structured reference databases formatted as `.tsv` (Tab-Separated Values) or `.sdf` (Structure-Data File) files.

[**Export publication-ready figures of fragmentation patterns:**]({{ "/gui/#substructure-tab" | relative_url }})
Substructure annotations on the measured spectrum as well as (analog) spectral matches can  now be exported as high-quality vector graphics in SVG (Scalable Vector Graphics) format.

**Multi-View Navigation:**
SIRIUS facilitates rapid data exploration across different result views. In the [LC-MS view]({{ "/gui/#lcms-tab" | relative_url }})  and [Homologous Series view]({{ "/gui/#KMD-tab" | relative_url }}), you can now directly navigate (jump) via right-click context menu to related features.

[**Duplicate Check in Structure Sketcher:**]({{ "/gui/#structure-sketcher" | relative_url }})
If your manually modified structure is already in the candidate list, you will get a warning, that the Structure candidate already exists.

[**Avoid redundant re-computations:**]({{ "/gui/#compute-dialog" | relative_url }})
You can now manually force to override automatic tool selection in the computation panel if you already have intermediate results and wish to avoid redundant re-computation of computationally intensive jobs.

[**High-resolution filter for spectral libraries:**]({{ "/gui/#custom-database-import" | relative_url }})
The spectral library importer now includes an automated Instrument Filter. When importing large public or custom spectral libraries, this filter automatically retains high-resolution MS/MS spectra while excluding low-resolution or incompatible gas chromatography-mass spectrometry (GC-MS) data.

## What's new in SIRIUS 6.3.1 {#SIRIUS-631}
[Read the full changelog (2025-09-18) here.]({{ "/changelog/#631-2025-09-18" | relative_url }})

[**Improved export of results:**]({{ "/gui/#data-export" | relative_url }})
When exporting summary files, you can now choose to append a `SIRIUS_` prefix to all summary column headers for better identification when merging data in external tools. Also, you can now export identification data formatted specifically for ChemVista using a `Formula only` export mode.


## What's new in SIRIUS 6.3.0 {#SIRIUS-630}
[Read the full changelog (2025-08-17) here.]({{ "/changelog/#630-2025-08-17" | relative_url }})

[**Structure Sketcher:**]({{ "/gui/#structure-sketcher" | relative_url }})
SIRIUS integrates an interactive Structure Sketcher tool. This feature allows you to manually modify existing database candidate structures and instantly re-score them against the predicted molecular fingerprint.

**Improved LC-MS pre-alignment:** using only high-quality features designed to increase processing speed and alignment robustness.

**Mouseover preview for structures:**
To streamline the exploration of large candidate lists in the "Structures" and "De Novo Structures" views without requiring continuous clicking or opening separate windows, the user interface now includes an interactive mouseover preview feature.  





## What's new in SIRIUS 6.2.0 {#SIRIUS-620}
[Read the full changelog (2025-05-30) here.]({{ "/changelog/#620-2025-05-30" | relative_url }})

<img src="{{ '/assets/features/biotransformer.png' | relative_url }}" alt="SIRIUS feature: MSNovelist" style="float: left; margin-right: 20px; width: 200px; max-width: 30%;">
[**Biotransformer 3.0 integration:**]({{ "/gui/#custom-database-import" | relative_url }})
BioTransformer is now seamlessly integrated directly into the software. This integration streamlines the process of generating bio-transformations, as they can now be automatically created during the custom database import process.
<div style="clear: both;"></div>

[**Substructure validation by spectral library analog search:**]({{ "/gui/#substructure-tab" | relative_url }})
To validate structure hits for unknown compounds, SIRIUS utilizes analog spectral matching to identify structurally related compounds.
By highlighting shared substructure explanations and neutral losses observed in analogs you can verify if the shared structural core of a spectral library analog explains the observed data.

[**Kendrick Mass Defect plots for homologous series:**]({{ "/gui/#KMD-tab" | relative_url }})
SIRIUS integrates Kendrick Mass Defect (KMD) plots to help you rapidly visualize, group, and annotate structurally related molecules that share a common scaffold but differ by repeating units without requiring prior library matches.

**Software tour guides:**
SIRIUS includes an interactive Software Tour Guide. This guided onboarding system initiates automatically upon the first launch of the software or can be manually triggered at any time from the main menu. The tour uses contextual overlays, tooltips, and step-by-step walkthroughs to familiarize you with the  software.

[**Global database selection:**]({{ "/gui/#compute-dialog" | relative_url }})
To ensure consistency throughout all selected workflow steps, SIRIUS now utilizes a global database selection architecture.

[**Improved result views:**]({{ "/gui/#visualize-results" | relative_url }})
The LC-MS View and the Library Matches View have been redesigned.
LC-MS pre-processing is now enhanced with advanced adduct network visualization, smarter sample sorting, and clearer color highlighting, while the upgraded spectral library search streamlines your data analysis by displaying all identity and analog matches in a single, comprehensive view.

## What's new in SIRIUS 6.1.1 {#SIRIUS-611}
[Read the full changelog (2025-01-22) here.]({{ "/changelog/#611-2025-01-22" | relative_url }})

[**Computation presets**:]({{ "/gui/#computation-presets" | relative_url }}) Predefined computation presets for different applications: a `default` workflow for general applications, a `DB-Search` workflow designed for users focused solely on structure database hits, and an `MS1` workflow for MS1-only data.

[**Spectral library search now more than 10x faster**]({{ "/methods-background/#spectral-library-search" | relative_url }}) using efficient dynamic programming and grouping by structure.

## What's new in SIRIUS 6.1.0 {#SIRIUS-610}
[Read the full changelog (2025-01-04) here.]({{ "/changelog/#610-2025-01-04" | relative_url }})

**New Color Scheme:**
New color scheme with more consistent coloring throughout the whole identification process, including consistent top hit highlighting.

[**Streamlined Workflows:**]({{ "/gui/#compute-dialog" | relative_url }})
Tools auto-enable/disable based on [workflow principles]({{ "/cli/#basic-principles" | relative_url }}), with new presets for saving and reloading setups.

[**Detect More Features:**]({{ "/gui/#lcms-tab" | relative_url }})
Enhanced LC-MS/MS preprocessing with improved feature detection, [adduct assignment]({{ "/adducts/" | relative_url }}), and sensitivity.

[**Welcome-Page Redesign:**]({{ "/gui/#overview" | relative_url }})
The central hub for account management, network status monitoring, and learning resources.

[**Improved Result Views:**]({{ "gui/#visualize-results" | relative_url }})
Enhanced result views for smoother analysis.

## What's new in SIRIUS 6.0.5 / 6.0.6 {#SIRIUS-605-606}
[Read the full 6.0.6 changelog (2024-09-28) here.]({{ "/changelog/#606-2024-09-28" | relative_url }})\\
[Read the full 6.0.5 changelog (2024-09-02) here.]({{ "/changelog/#605-2024-09-02" | relative_url }})

[**Export your results:**]({{ "/gui/#data-export" | relative_url }})
New export options and file formats for writing summaries in the CLI and GUI. In addition, you can export a Feature quality summary, as well as a ChemVista summary file which can be imported directly to the Agilent ChemVista software.

## What's new in SIRIUS 6 {#SIRIUS-6}
[Read the full changelog (2024-06-03) here.]({{ "/changelog/#600-2024-06-03" | relative_url }})

### Annotation Features: {#SIRIUS-6-annotation}

<img src="{{ '/assets/features/MSnovelist.png' | relative_url }}" alt="SIRIUS feature: MSNovelist" style="float: left; margin-right: 20px; width: 200px; max-width: 30%;">
[**Discover Truly Novel Molecules:**]({{ "/gui/#MSNovelist-denovo-structure" | relative_url }})
Don't be limited by finite databases. Neither spectral libraries nor molecular structure databases. The next building block in structure elucidation for the unknowns. Our De Novo Structure Generation builds candidates from scratch using predicted fingerprints—perfect for identifying compounds that don't exist in any library yet.
<div style="clear: both;"></div>

[**Be sure to not miss a good candidate:**]({{ "/methods-background/#expansive-search" | relative_url }})
Expansive search allows for structure database searches in user-selected databases with a confidence score-based fallback on PubChem.

[**Faster molecular formula identification:**]({{ "/methods-background/#bottom-up" | relative_url }})
Shrink the molecular formula space without limiting yourself to database compounds. Use our bottom-up search to narrow down to MS/MS-explainable formula candidates.

[**New Molecular Fingerprint:**]({{ "/methods-background/#molecular-fingerprint" | relative_url }})
Introducing a new molecular fingerprint with a more representative set of molecular properties to enhance key functionalities for structure elucidation.

### Validation Features: {#SIRIUS-6-validation}

[**Seamless Integration of Spectral Library Search:**]({{ "/gui/#spectral-library-matching" | relative_url }})
Smooth integration of your long-established annotation workflow of in-house suspect libraries with comprehensive structure annotation by SIRIUS.

[**Redefined confidence score for similar compounds:**]({{ "/methods-background/#confidence-score-modes" | relative_url }})
Select our redefined confidence score when searching for approximate structure hits for more precise results.

### Software features: {#SIRIUS-6-software}

[**Application Programming Interface (API)**:]({{ "/quick-start/#API" | relative_url }})
To support automated, high-throughput computational workflows and enable seamless integration with modern bioinformatics infrastructure, SIRIUS features a fully integrated local REST API. While the GUI and CLI are ideal for interactive and batch processing, the REST API exposes the complete analytic core of SIRIUS as a headless background service. 


**SIRIUS dark mode:**
SIRIUS now features a fully integrated Dark Mode. Implementing a dark theme extends beyond aesthetic preference, providing reduced eye fatigue as well as enhanced visual contrast.

[**Nitrite database for better performance:**]({{ "/io/" | relative_url }})
SIRIUS has fundamentally changed how project data is stored and managed. The software has transitioned from the traditional file-based directory structure to an embedded, transactional Nitrite database. This architectural shift was necessary to overcome the physical performance bottlenecks of modern filesystems when processing high-throughput, untargeted metabolomics datasets containing thousands of metabolic features.
