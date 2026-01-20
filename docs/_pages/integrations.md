---
permalink: /integrations/
title: "External Software Integrations"
---

# Using SIRIUS with Third-Party Tools

SIRIUS is a powerful framework for small molecule identification that can be used to cover the whole pipeline from preprecessing to identification. Additionally, its integrations allow users to establish comprehensive workflows covering raw LC-MS data (peak picking/alignment) to advanced structural elucidation (formula identification, compound class prediction, structure database search or de novo generation) in combination with the tools you already use and appreciate.

## Agilent MassHunter Explorer Native Integration {#MassHunter-Explorer}

The integration between [**MassHunter Explorer**](https://www.agilent.com/en/product/software-informatics/mass-spectrometry-software/data-analysis/masshunter-explorer) and **SIRIUS** is a commercial-technical bridge supported by **Agilent** and **Bright Giant.** Built directly into the MH Explorer interface, this integration allows users to rapidly send "unknown" features to SIRIUS with a single click. A unique aspect of this integration is the licensing agreement, which provides Agilent users with a **substantial quota of complimentary queries**.

**Benefits of the Integration:**

* **Direct Access:** Integrated "Send to SIRIUS" functionality within the MassHunter GUI for immediate SIRIUS analysis.
* **Commercial License Included:** Access to SIRIUS web services via the SIRIUS GUI without additional costs for MH Explorer users (licence only meant to analyse Agilent data; no CLI/API and de novo structure generation included).
* **Rapid Unknown Identification:** Vendor-own preprocessing and statistical analysis pairs with structure annotation for unknowns.

**Best for Agilent users who rely on MH Explorer for initial peak picking and statistics but need advanced identification methods for "unknown unknowns."**

**Learn More:**
* [Enhancing Unknown Identification with MH Explorer and Complimentary SIRIUS with CSI:FingerID](https://www.agilent.com/cs/library/flyers/public/fl_mass-hunter-explorer-sirius-5994-8701-en-agilent.pdf)
* [MassHunter Explorer Installation Guide](https://www.agilent.com/cs/library/usermanuals/public/D0109800%20MassHunter%20Explorer%20Installation%20Guide.pdf?)
* [How to activate a SIRIUS license for MassHunter Explorer users]({{"/account-and-license/MHExplorer-license/" | relative_url }})

## Mzmine One-Click Integration {#mzmine}

The integration between [**mzmine**](https://mzio.io/mzmine-news) and **SIRIUS** bridges the gap between raw data processing and advanced computational identification. The mzmine integration provides a **vendor-neutral, open-source** pipeline for comprehensive untargeted metabolomics. **As of mzmine v4.8**, the integration has evolved from a manual file-export process to a seamless, "one-click" bidirectional connection. The integration handles the heavy lifting of data transfer and automatically pulls SIRIUS results—including molecular formulas, structure annotations, and compound classes—back into the mzmine interface for unified data exploration.

**Benefits of the Integration**

* **One-Click Execution:** Users can launch SIRIUS analysis directly from mzmine feature tables. Eliminates the manual export/import of `.mgf` files, reducing human error and processing time.
* **Automated Result Handling:** Results are seamlessly integrated back into the mzmine interface to visualize them alongside mzmine’s chromatographic peak shapes and statistical plots.
* **Vendor Agnostic:** Supports raw data from all major MS vendors (Bruker, Thermo, [Agilent](#MassHunter-Explorer), Waters).

**Best for researchers seeking a high-throughput, vendor-neutral workflow that combines the flexibility of mzmine with the structural elucidation power of SIRIUS.**

**Learn More:**
* [Press release: Seamless Integration Between mzmine and SIRIUS](https://bright-giant.com/2025/11/11/seamless-integration-between-mzmine-and-sirius/)
* [Download mzmine](https://mzio.io/mzmine-news/)
* [mzmine documentation](https://mzmine.github.io/mzmine_documentation/index.html)

## Compound Discoverer API node {#Compound-Discoverer}

The integration with **Thermo Scientific Compound Discoverer (CD)** is handled via a **custom processing node** within the Thermo Scientific software ecosystem. The [**CDSirius node**](https://github.com/fergusonlabduke/CDSirius/tree/main) was developed by **Lee Ferguson** and his team at Duke University. The integration embeds SIRIUS structural elucidation directly into the CD "Processing Workflow" tree. When the workflow is executed, the CDSirius node automatically passes detected features and their MS/MS spectra to a local SIRIUS instance. Once the computation is finished, the structural candidates and confidence scores are mapped back to the CD "Compounds" table, allowing users to filter and sort discoveries using SIRIUS metadata within the native Thermo environment.

**Benefits of the Integration:**

* **Workflow Continuity:** Run analysis and view all data and results in a single software.
* **Advanced Filtering:** Use SIRIUS confidence scores and compound classes to filter large Compound Discoverer datasets.
* **Seamless Mapping:** Directly maps SIRIUS results back to CD’s processed compounds.

**Best for Thermo Fisher instrument users who want to stay within the "Workflow Tree" ecosystem but seek identification capabilities beyond what is available in Compound Discoverer.**

**Learn More:**
* [Download the CDSirius node](https://github.com/fergusonlabduke/CDSirius/tree/main). The accompanying website also provides detailed installation instructions and explanations of all node parameters.
* Identifying molecular structures with a SIRIUS node in Compound Discoverer software: [Slides](https://mycompounddiscoverer.com/wp-content/uploads/2025/12/lee-ferguson-identifying-molecular-structures-with-a-sirius-node-in-compound-discoverer-software.pdf) / [Video](https://mycompounddiscoverer.com/wp-content/uploads/2025/12/lee-ferguson-talk-from-compound-discoverer-user-meeting.mp4)

## RuSirius Interface for RforMassSpectrometry {#RuSirius}

[**RuSirius**]() is an R package developed within the [**RforMassSpectrometry**](https://www.rformassspectrometry.org/) ecosystem to provide a high-level interface to SIRIUS. Unlike our self-provided SIRIUS API clients, RuSirius is built to handle the `Spectra` and `QFeatures` objects commonly used within the RforMassSpectrometry suite. It allows users to seamlessly push spectra to a local SIRIUS instance for processing and pull the resulting fragmentation trees and annotations back into R. This integration is essential for researchers who use `xcms` for peak picking and want to keep their entire post-processing workflow within a reproducible Bioconductor environment.

**Benefits of the Integration:**

* **Native Object Support:** Works directly with `Spectra` objects, eliminating the need for manual file conversions to `.mgf` or `.ms` formats  or SIRIUS API’s specific json format.
* **Bioconductor Synergy:** Seamlessly integrates SIRIUS results (formulas, fingerprints) into the metadata columns of `SummarizedExperiment` or `QFeatures` objects.
* **Pipeline Reproducibility:** Allows for the documentation of SIRIUS parameters and versioning within R Markdown or Quarto reports for fully transparent data analysis.

**Best for R-based bioinformaticians using the `Spectra` package who need a "tidy" way to integrate SIRIUS annotations into their existing Bioconductor discovery pipelines.**

**Learn More:**
* [Download RuSirius R package on github](https://github.com/rformassspectrometry/RuSirius)