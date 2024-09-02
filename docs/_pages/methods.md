---
permalink: /methods-background/
title: "Methods and algorithms"
---

## SIRIUS Workflows {#workflows}

SIRIUS is the umbrella application comprising several workflows:
- molecular formula annotation ([SIRIUS](#SIRIUS-molecular-formula) + [ZODIAC](#ZODIAC)),
- fingerprint prediction and compound class prediction ([CSI:FingerID](#molecular-fingerprint) + [CANOPUS](#CANOPUS)),
- structure database searching ([CSI:FingerID](#CSIFingerID) + [COSMIC](#COSMIC)),
- de novo structure generation ([MSNovelist](#MSNovelist)).

These workflows follow a certain hierarchy, and cannot be freely combined. For example, to predict CANOPUS compound classes, the molecular formula annotation workflow must be run first (or results from a previous run must be available). See the following figure on how the different workflows depend on each other.

{% capture fig_img %}
![Foo]({{ "/assets/images/SIRIUS-workflow-full.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>SIRIUS workflow dependencies.</figcaption>
</figure>


## Spectral library matching {#spectral-library-search}

SIRIUS 6 allows importing local libraries containing spectral reference data. Supported import formats are `.ms`, `.mgf`, `.msp`, `.mat`, `.txt` (MassBank), `.mb`, `.json` (GNPS, MoNA). Spectra must be annotated with a structure and be centroided.
SIRIUS will automatically perform spectral library search against all available libraries whenever the molecular formula annotation workflow is used. 
Spectral library matching is performed using the cosine score with squared peak intensities, ignoring the precursor peak.

### Spectral matching influence on SIRIUS and CSI:FingerID results {#spectral-matching-influence}

In SIRIUS 6, spectral library matches are added as annotations to CSI:FingerID results and do not influence the ranking of structure candidates. If a high-quality spectral library hit is found for a molecular formula that SIRIUS would not have otherwise considered, that molecular formula is forcibly added to the list of candidates.
This ensures that no spectral library matches are overlooked when using CSI:FingerID.


## Molecular formula identification with SIRIUS {#SIRIUS-molecular-formula}

SIRIUS is the name of the umbrella application, but (for historic reasons) also the name for the identification of the molecular formula. First, a set of [candidate molecular formulas is generated](#generating-candidates). For this set, molecular formula identification is done using [isotope pattern analysis](#isotope-analysis) on the MS1 data as well as [fragmentation tree computation](#fragmentation-trees) on the MS2 data. The score of a molecular formula candidate is a [combination of the isotope pattern score and the fragmentation tree score]({{"/gui/#formulas-tab" | relative_url}}).

For a deeper understanding of how SIRIUS and [ZODIAC](#ZODIAC) work,  why they work and how well they work, you can watch the [Behind the Scenes](https://www.youtube.com/watch?v=EpDKLzG0qVc) talk. Try using SIRIUS in the [GUI]({{"/gui/#SIRIUS-molecular-formula" | relative_url}}) or [CLI]({{"/cli/#SIRIUS-formulas" | relative_url}}).

### Generating molecular formula candidates {#generating-candidates}

SIRIUS supports three different approaches for generating the set of candidate molecular formulas for feature annotation: 
- [*de novo*](#denovo)
- [database search](#formula-db-search)
- [bottom-up](#bottom-up)

A thorough understanding of these approaches is crucial for effectively applying the [annotation strategy](#annotation-strategies) that aligns best with your task or research question.

It is equally important to understand how the molecular formula annotation step affects [structure annotation](#CSIFingerID) and [compound class prediction](#CANOPUS). Only those molecular formula candidates selected by the annotation strategy are used for structure annotation via database searching and compound class prediction in subsequent steps.

**IMPORTANT: Molecular formulas that are not included in the candidate set during this step are excluded from all subsequent processes.**

SIRIUS imposes penalties on candidate molecular formulas that significantly deviate from typical biomolecule compositions
(e.g., C<sub>2</sub>H<sub>2</sub>N<sub>12</sub>O<sub>12</sub> would incur a penalty). However, these penalties are applied cautiously: 
Only 2.6% of all molecular formulas in PubChem
compounds — and thus a small fraction of formulas not categorized as biomolecules — are penalized. SIRIUS never rewards molecular formulas. These penalty rules apply to all approaches. 

SIRIUS uses a concise list of outlier molecular formulas that would typically receive penalties under the aforementioned criteria due to their deviation from "biomolecule-like" compositions.
These formulas are not penalized, observed in metabolomics experiments (e.g., as solvents), are neither penalized nor rewarded.
However, during the [computation of fragmentation trees](#fragmentation-trees), fragment annotations within MS/MS spectra—subformulas derived from these outlier molecular formulas—may still incur penalties.

#### *De novo* annotation {#denovo}

SIRIUS considers all chemically feasible molecular formulas (considering valencies) that match the precursor mass of the
molecule/ion: For instance, if your query compound is pinensin A 
(C<sub>96</sub>H<sub>139</sub>N<sub>27</sub>O<sub>30</sub>S<sub>2</sub>,
monoisotopic mass of 2213.962 Da),  SIRIUS will evaluate all
19,746,670 candidate molecular formulas that match this
monoisotopic mass (based on a subset of elements - described below - and 10 ppm mass accuracy). 

Considering all molecular formulas implies specifying a set of elements from which these formulas are generated. 
The elements most abundant in living beings are hydrogen (H), carbon (C), nitrogen (N), oxygen (O), and phosphor (P). This is the default set of elements in SIRIUS.
Some less common elements result in very characteristic isotope pattern changes and can be automatically detected from the isotope and fragmentation pattern of the query compound (see [Meusel *et al.*](https://doi.org/10.1021/acs.analchem.6b01015)). Detectable elements are sulfur (S), chlorine (Cl), bromine (Br), boron (B) and selenium (Se). 
- **Default elements:**  H, C, N, O, P
- **Autodetect elements:** B, S, Cl, Se, Br
  
Users should only manually adjust the element set if they have specific prior knowledge about the features of interest.
Expanding the element set unnecessarily  will result in  significantly longer computation times and an increase in incorrect annotations.

**This approach can result in molecular 
formula annotations that may not match any entries in the structure database.**

#### Formula database search {#formula-db-search}

Instead of exploring the entire space of molecular formulas for a given mass and element set, one can alternatively confine the search to a specific database. In that case, SIRIUS exclusively considers molecular formulas included in the chosen database(s), with the option to further narrow down by specific element sets. Naturally, **this approach cannot annotate novel molecular formulas ("novel" defined as "not present in the selected database")** and significantly restricts the
pool of molecular formulas candidates. As this pool is much smaller than for the *de novo* method, this approach does not require a 
predefined element set. The database constrained approach is more likely to annotate formulas with unusual elements that are not recognizable from MS1 than the *de novo* approach.

#### Bottom-up search {#bottom-up}

The "bottom-up" approach serves as a middle ground between the expansive molecular formula space of *de novo* annotation and the very constrained space of formula database search.
This method, inspired by [*Xing et al.*](https://doi.org/10.1038/s41592-023-01850-x), leverages each observed fragment's mass and corresponding root loss mass from the MS/MS spectrum to query a database of potential subformulas. Pairwise combinations of these resulting subformula candidates for fragments and root losses are then used to construct candidate formulas for the precursor molecule.
Thus, the range of precursor formula candidates generated depends on the fragments present in the spectrum. **Unlike strict restriction to database entries, this method can detect novel formulas that are combinations of two known molecular formulas.** And yet, relying on database entries, this approach generates a much smaller number of candidate formulas compared to *de novo* annotation, leading to a substantial speed up in computation time.

Given the constrains, strict limitations on an element subset are often unnecessary.
The formula database used for bottom-up searches typically includes formulas from the "bio" database along with a list of commonly observed losses.
Again, this approach is more likely to annotate formulas with unusual elements that are not recognizable from MS1 than the *de novo* approach.

### Molecular formula annotation strategies {#annotation-strategies}

The molecular formula annotations explained above can  be utilized either individually or combined. Selecting the appropriate molecular formula annotation
strategy is integral for a successful analysis.  Below we describe some standard strategies that cover most applications and serve as illustrative examples:

- **[De novo](#denovo) + [bottom-up](#bottom-up)** <br>
    <span style="color:#d40f57">**We recommend this approach for generic applications.**</span><br>
    In the combined approach, features are categorized into "low" (m/z<400) and "high"(m/z>=400) mass features. Bottom-up search is conducted for both categories. For low mass features, SIRIUS also employs *de novo* molecular formula annotation to ensure comprehensive coverage. This dual strategy minimally impacts (increases) computation times compared to relying solely on bottom-up search.
    The m/z threshold for categorization can be adjusted to align with runtime constraints and the computational capabilities of your local machine.
    Element set constraints must be defined for *de novo* annotation and can optionally be applied to bottom-up search as well. 
- **[De novo](#denovo) only** <br>
    <span style="color:#d40f57">**This approach is particularly suitable for discovering "unknown unknowns".**</span><br>
    The "de novo only" strategy should be employed when specifically expecting molecular formulas that cannot be detected by bottom-up search (i.e., the precursor
    formula is not a combination of database subformulas). 
    The expected element set needs to be well-defined and avoid including too many uncommon elements, as this can lead to a  combinatorial explosion of possible candidates for large masses (see the example in [de novo](#de-novo-annotation)).
    The local machine running the SIRIUS client must be sufficiently powerful  to handle *de novo* annotation of higher mass compounds.
- **[Database search](#formula-db-search) only** <br>
    <span style="color:#d40f57">**This strategy should be employed only when the user is interested exclusively in features that have corresponding entries in a structure database and requires extremely fast computation times.**</span><br>
    As the database-only approach will only consider molecular formulas present in the selected databases it will not generate formula annotations without a structure database match.
- **[Bottom-up](#bottom-up) only**<br>
     <span style="color:#d40f57">**This strategy can be used for a slight speed increase compared to the combined approach.**</span><br>
     However, it does not offer significant advantages over the recommended strategy, as the drawbacks of *de novo* annotation are primarily relevant for high-mass compounds.

### Isotope pattern analysis {#isotope-analysis}
Isotope patterns of the candidate molecular formulas are simulated starting with the isotopic distributions of the individual elements, and then combining these distributions by folding (see [Böcker *et al.*](https://doi.org/10.1093/bioinformatics/btn603)).
The simulated isotope pattern is compared with the measured pattern by assigning probabilities to the observed masses and intensities.[1]

### Fragmentation tree computation {#fragmentation-trees}

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

## Improved molecular formula identification with ZODIAC {#ZODIAC}

ZODIAC stands for [*ZODIAC: Organic compound Determination by Integral Assignment of elemental Compositions*](https://doi.org/10.1038%2Fs42256-020-00234-6).

ZODIAC improves the ranking of the formula candidates provided by SIRIUS. 
Organisms produce related metabolites derived from multiple but limited biosynthetic pathways. 
For a full [LC-MS/MS run]({{"/io/#lcms-runs" | relative_url}}) that is derived from a biological sample or any other set of derivatives the relation of the metabolites is reflected in their similarity. 
Those similarities are in turn reflected in joint fragments and losses between the fragmentation trees and can be leveraged to improve molecular formula identification of the individual molecules.

ZODIAC uses the [top X molecular formula candidates]({{"/gui/#ZODIAC-advanced" | relative_url}}) for each molecule from SIRIUS to build a similarity network, and uses Bayesian statistics to re-rank those candidates. 
Prior probabilities are derived from fragmentation tree similarity. Finding an optimal solution to the resulting computational problem is NP-hard, therefore Gibbs sampling is used.

For a deeper understanding of how SIRIUS and [ZODIAC](#ZODIAC) work,  why they work and how well they work, you can watch the [Behind the Scenes](https://www.youtube.com/watch?v=EpDKLzG0qVc) talk. Try using ZODIAC in the [GUI]({{"/gui/#ZODIAC-ranking" | relative_url}}) or [CLI]({{"/cli/#ZODIAC" | relative_url}}).

## Molecular fingerprint prediction with CSI:FingerID {#molecular-fingerprint}

CSI:FIngerID identifies the structure of a molecule by predicting its molecular fingerprint and using this fingerprint to [search in a molecular structure database](#CSIFingerID). For more details, watch the [CSI:FingerID behind the scenes](https://www.youtube.com/watch?v=oEQoB2aaV2E). Try using molecular fingerprint prediction in the [GUI]({{"/gui/#CSIFingerID-CANOPUS" | relative_url}}) or [CLI]({{"/cli/#fingerprints" | relative_url}}).

*Molecular fingerprints* can be used to encode the structural features of 
molecules. These fingerprints are typically represented as binary vectors 
of fixed length, where each bit describes the presence or absence of a particular, fixed
*molecular property*, such as the presence of a particular substructure.

One example is the **PubChem CACTVS fingerprint**, which consists of 881 bits.Molecular property 121 represents the presence of at least one
"unsaturated non-aromatic heteroatom-containing ring size 3". The properties are usually defined by SMARTS (SMiles ARbitrary Target
Specification) strings. For example, molecular property 357 corresponds to the SMARTS string  <span>\[#6\](&#126;\[#6\])(:c)(:n)</span>. This string describes a  central carbon atom connected to another carbon atom 
via any bond, to a third aromatic carbon atom via an aromatic bond, and 
to an aromatic nitrogen atom via an aromatic bond. 
For a complete description of the CACTVS fingerprint, refer to the [official specification document](ftp://ftp.ncbi.nlm.nih.gov/pubchem/specifications/pubchem_fingerprints.pdf). We ignore all
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
predicted by SIRIUS 6.0.

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

## Structure database search with CSI:FingerID {#CSIFingerID}

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

Try using CSI:FingerID in the [GUI]({{"/gui/#CSIFingerID-structure" | relative_url}}) or [CLI]({{"/cli/#CSIFingerID-structures" | relative_url}}).

### Expansive search (structure database search with fallback) {#expansive-search}

SIRIUS 6 introduces *expansive search*, which allows for structure database searches with a [confidence score](#COSMIC)-based fallback.
Structure database search is conducted within the user-selected databases ("requested databases"), and then additionally for "PubChem". If the top hit in PubChem has a confidence score at least twice as high as the confidence 
score of the top hit from the requested databases, the search will be "expanded" and the results from PubChem will be displayed. 

## Confidence assessment with COSMIC {#COSMIC}

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

**Interpretation of COSMIC confidence values:**
COSMIC confidence scores should be interpreted with caution. It’s crucial to understand that these scores are not probabilities and, therefore, do not have a direct statistical interpretation. When performing large-scale analyses, it's advisable to focus on the highest-confidence hits (e.g., the top 1/5/10%), generally independent of their specific confidence value.

**Correct annotation with low confidence value:**
If the database being searched contains multiple highly similar structures with nearly identical fingerprint representations and CSI:FingerID
scores, correct structure annotations might receive low confidence values.
Without prior knowledge of the correct structure, any of these highly similar compounds could be the true structure, leading to a lower confidence score. This is not a limitation of CSI:FingerID
or COSMIC, but rather of mass spectrometry itself, as the MS/MS spectra of these similar structures will appear almost identical. We recommend using the `approximate mode` [described below](#confidence-score-modes), which provides a higher confidence score for hits that are highly similar to the true structure.

For a deeper understanding of how COSMIC works,  why it works and how well it works, you can watch the [Behind the Scenes](https://www.youtube.com/watch?v=akMkDK5O2rk) talk. COSMIC is part of the structure database search in the [GUI]({{"/gui/#CSIFingerID-structure" | relative_url}}) and [CLI]({{"/cli/#CSIFingerID-structures" | relative_url}}).

### Confidence score modes {#confidence-score-modes}

The confidence score can be evaluated in two modes: `exact` and `approximate`.

- The `exact` mode addresses the question "Is this exact molecular structure hit the true structure of my unknown compound?". It provides a confidence level for the precise match of the structure. In `exact` mode, the confidence score tends to be low when the top structure candidate and the second-best candidate are highly similar. This is common for well-studied molecules, where multiple derivatives often exist in the structure database.

- The `approximate` mode answers the question "Is this structure hit correct or highly similar to the true structure?". In this context,  _highly similar_ means that the hit is just one simple chemical reaction away from the true structure. Theoretical speaking, the hit and the true structure should have a Maximum Common Edge Subgraph ([MCES](https://en.wikipedia.org/wiki/Maximum_common_edge_subgraph)) distance of 2.
For example, a hit where only a side group has been repositioned compared to the true structure would still be considered "correct" in `approximate` mode. If you find nearly correct hits useful, it is advisable to use the `approximate` mode, which provides a higher confidence score for hits that are highly similar to the true structure.

## Compound class prediction with CANOPUS {#CANOPUS}

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
common substructures. 
In the [ClassyFire](http://classyfire.wishartlab.com/) ontology, every compound belongs to multiple compound classes, which describe structural patterns. 
For example, a *dipeptide* is classified as both an *amino acid* (due to containing an amino acid substructure) and a *carboxylic acid* (due to containing an carboxylic acid substructure).
Similarly, a glycosylated amino acid might belong to the compound classes of *amino acids* and *hexoses*. 

Different from how compound classes are often
described in chemistry textbooks, ClassyFire compound classes do **not** describe the biosynthetic origin. For example,
a *phytosteroid might* be classified as *bile acids* in Classyfire, because both class of compounds share the same
backbone, although they are involved in different biochemical pathways.
Without additional knowledge about the measured 
organism, the MS/MS spectrum alone cannot determine the biochemical origin 
of a compound class, as the same compound may be derived from different 
biosynthetic precursors.  

Additionally, CANOPUS predicts compound classes based on the categories 
from [NPClassifier](https://doi.org/10.1021/acs.jnatprod.1c00399). This 
classification system is more general, but may aligns better with the 
concept of biosynthetic pathway mapping. However, it is still not using 
taxonomic information, relying solely on MS/MS data for its predictions.

For a deeper understanding of how CANOPUS works, why it works and how well it works, you can watch the [CANOPUS behind the Scenes](https://www.youtube.com/watch?v=kgTS5hGiPXg) talk. Try using CANOPUS in the [GUI]({{"/gui/#CSIFingerID-CANOPUS" | relative_url}}) or [CLI]({{"/cli/#CANOPUS-classes" | relative_url}}).

## De novo structure generation with MSNovelist {#MSNovelist}

MSNovelist ([Stravs *et al.*](https://doi.org/10.1038/s41592-022-01486-3)) generates molecular structures *de novo* from  MS/MS data - without relying on any database. 
This makes it particularly useful for analyzing poorly represented analyte classes and novel compounds, where traditional database searches may fall short. 
However, it is not intended to replace database searches altogether, as structural elucidation of small molecules from MS/MS data remains a challenging task, and identifying a structure without database candidates is even more difficult.

MSNovelist generates structures which can serve as a great starting point for elucidation of specific unknowns.
MSNovelist generates multiple candidate structures from the predicted molecular fingerprint. These candidates are represented in SMILES format and are sampled using an autoregressive model, which generates each SMILES token by token. Once the candidate structures are generated, they are ranked using CSI:FingerID. 
The proposed structures can serve as an excellent starting point for the elucidation of specific unknown compounds. This information can be further enriched by [CANOPUS](#CANOPUS) compound class predictions, providing a broader context for the identified structures.

Try using MSNovelist in the [GUI]({{"/gui/#MSNovelist-denovo-structure" | relative_url}}) or [CLI]({{"/cli/#MSNovelist-denovo-structures" | relative_url}}).