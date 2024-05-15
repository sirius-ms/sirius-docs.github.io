---
permalink: /quick-start/
title: "Quick start"
---

 - You can download some [sample spectra](https://bio.informatik.uni-jena.de/wp/wp-content/uploads/2015/05/demo.zip)
from the SIRIUS website.
 - To get started quickly you may also want to have a look at our 
   [video tutorials](https://www.youtube.com/playlist?list=PL57Jv_39fTdc_j8eHrH6At81n1AtxeKar).
 - To process a full LC-MS/MS run, you can also use your own `.mzML` files and run the following examples. This will perform features detection first. Also, annotating 100s or 1000s features may take a while. 

## Graphical User Interface

### Analyzing multiple compounds

SIRIUS’s "Batch mode" corresponds to analyzing many compounds at once,
each having one or more mass spectra. Obviously, you can also use this workflow to analyze a single compound.

{% capture fig_img %}
![Foo]({{ "/assets/images/quick_start_many_compounds.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Analyze multiple compounds in a view simple steps. Steps in figure do not directly correspond to detailed steps below.</figcaption>
</figure>

1. Move the files `demo-data/ms/Bicuculline.ms` and `demo-data/ms/Kaempferol.ms` and from the demo data via Drag and 
    Drop into the application window.

2. The two compounds are now displayed in the compound list.

3. Check if the ionization and parent mass is correctly annotated.

4. Click on the *Compute All* button.

5. Select SIRIUS.

6. Change the instrument type as well as the maximal allowed mass deviation. Be aware that this settings
    will be used for all imported compounds.
7. Check if the currently selected molecular [molecular formula generation strategy]({{ "/advanced-background-information/#molecular-formula-annotation-strategies" | relative_url }}) aligns with your research question (TODO: link).

8. Select *Predict Properties & CANOPUS* to predict the compounds' molecular fingerprints with CSI:FingerID as well as their compound classes with CANOPUS.
8. Select *CSI:FingerID structure database search* to search compounds in a structure database with CSI:FingerID.
10. Click *Compute*.

11. A *gear* symbol occurs on the lower right corner of each compound.
     This means that the compound is part of a computation job.
    
12. Sometimes a computation might take a long time (e.g. for compounds
     with a lot of elements or very high masses). You can cancel running
     computations by selecting *Cancel All* in the toolbar.

13. Inspect results with the help of the *Formulas*, *Structure* and *Compound Classes* views.
    Even more details can be found by looking at the  *Substructure Annotation* and *Predicted Fingerprint* views.

[//]: # (one or more mass spectra as individual peak list files. )

[//]: # (This is usually not the recommended format as it misses relevant information that may need to be specified manually.   )

[//]: # ()
[//]: # (#### Example 1: Chelidonine)

[//]: # ()
[//]: # (1.  Click the *Import Compound* Button in the Toolbar to open the importer window.)

[//]: # ()
[//]: # (1.  Move each of the three files `demo-data/txt/chelidonine_ms.txt`, `demo-data/txt/chelidonine_msms1.txt`)

[//]: # (    and `demo-data/txt/chelidonine_msms2.txt` &#40;one after another&#41; from the demo data via Drag and Drop into)

[//]: # (    the importer window.)

[//]: # ()
[//]: # (1.  The following dialog offers you to select the columns for mass and)

[//]: # (    intensity values. Just press *Ok* as the default values are already)

[//]: # (    correct.)

[//]: # ()
[//]: # (1.  You see the load dialog with three spectra. The first spectrum is)

[//]: # (    wrongly annotated as *MS/MS* spectrum but should be an *MS1*)

[//]: # (    spectrum instead. Just select *MS 1* in the drop down list labeled)

[//]: # (    with *ms level*.)

[//]: # ()
[//]: # (1.  All other options are fine. However, you might want to choose a more)

[//]: # (    memorizable name in the *compound name* field.)

[//]: # ()
[//]: # (1.  Press the *OK* button. The newly imported compound should now appear)

[//]: # (    in your compound list on the left side.)

[//]: # ()
[//]: # (1.  Choose the compound, right-click on it and press *Compute*.)

[//]: # ()
[//]: # (1.  Select SIRIUS. All other options should be fine. Just check that)

[//]: # (    the correct parent mass is chosen. You might want to add Chlorine or)

[//]: # (    Fluorine to the set of considered elements. Furthermore, you can)

[//]: # (    change the instrument type to *Orbitrap*. Then click *Compute*.)

[//]: # ()
[//]: # (1.  Just look into the candidate list: The first molecular formula has a)

[//]: # (    quite large score. Furthermore, the second molecular formula has a)

[//]: # (    much lower score. This is a good indication that the identification)

[//]: # (    is correct. However, you can take a look at the fragmentation tree:)

[//]: # (    Do the peak annotations look correct? Take a look at the spectrum)

[//]: # (    view: Are all high intensive peaks explained?)

[//]: # ()
[//]: # (1.  To write the results into a summary file press the *Summaries* button.)

[//]: # (    You can find the result list `formula_candidates.tsv` at the compound level)

[//]: # (    in the [project-space]&#40;{{ "/io/#sirius-project-space" | relative_url }}&#41; directory.)

[//]: # ()
[//]: # (#### Example 2: Identifying a CASMI challenge)

[//]: # ()
[//]: # (1.  Download the files <http://casmi-contest.org/2014/Challenge2014/Challenge1/1_MS.txt>)

[//]: # (    and <http://casmi-contest.org/2014/Challenge2014/Challenge1/1_MSMS.txt>)

[//]: # ()
[//]: # (1.  Click the *Import Compound* Button in the Toolbar to open the importer window.)

[//]: # ()
[//]: # (1.  Move these files via Drag and Drop into the importer window &#40;one after another&#41;.)

[//]: # ()
[//]: # (1.  Change the ms level of the first file into *Ms 1*)

[//]: # ()
[//]: # (1.  Click on *OK*)

[//]: # ()
[//]: # (1.  Click on *Compute* in the right-click context menu of the imported)

[//]: # (    compound &#40;or *double click*&#41;.)

[//]: # ()
[//]: # (1.  Select SIRIUS and choose *Q-TOF* as instrument and press the *OK* button)

[//]: # ()
[//]: # (1.  *C23H46NO7P* should be suggested as number one hit in the candidate)

[//]: # (    list)

## Command Line Interface

The demo-data contains examples for three different data formats readable
by SIRIUS. 

### Example 1: MGF file
The MGF folder contains an example for a MGF file containing a
single compound with several MS/MS spectra measured on an Orbitrap
instrument. SIRIUS recognizes that these MS/MS spectra belong to the
same compound because they have the same parent mass. To analyze this
compound, run:

```shell
sirius --input demo-data/mgf/laudanosine.mgf --output <output.sirius> formula -p orbitrap fingerprint compound-classes structure write-summaries --output <summary-files-dir>
```

This command runs 4 subtools at once: *formula annotation*, *molecular fingerprint prediction*, *compound class annotation* and *structure database search*.

The `formula_candidates.tsv` in `<summary-files-dir>/0_laudanosine_FEATURE1` should look similar to this:

```
rank	molecularFormula	adduct	precursorFormula	SiriusScore	TreeScore	IsotopeScore	numExplainedPeaks	explainedIntensity	medianMassErrorFragmentPeaks(ppm)	medianAbsoluteMassErrorFragmentPeaks(ppm)	massErrorPrecursor(ppm)	lipidClass
1	C21H27NO4	[M + H]+	C21H27NO4	24.65417225757305	16.673823307501223	7.98034895007183	10	0.9945969759520045	0.007637492516338191	0.4982304820065859	0.7071154657093854	
2	C19H29NO4	[M + Na]+	C19H29NO4	21.14569419257409	15.328042039004671	5.817652153569419	10	0.9945969759520045	0.8982080585874639	1.2778688586195766	7.423133528795216	
3	C17H23N7O2	[M + H]+	C17H23N7O2	19.270167038174378	14.209441689860592	5.060725348313786	10	0.9945969759520045	0.8982080585874639	1.2778688586195766	8.203923029468156	
4	C15H28N5O3P	[M + H]+	C15H28N5O3P	18.874745611339133	13.87676301554373	4.997982595795404	10	0.9945969759520045	0.8982080585874639	1.2778688586195766	3.5880578691669838	
5	C19H25N4O3	[M + H]+	C19H25N4O3	18.047443230086564	10.915836537157713	7.131606692928852	9	0.9900893988366646	-0.01690518911597119	0.6982192702970249	4.455519247668116	
6	C17H29N5O	[M + K]+	C17H29N5O	15.00462580213599	14.724156079206512	0.28046972292947725	11	0.9999999999999998	-1.461376225764996	2.4522252659018866	3.2647482579632285	
7	C17H30N2O4P	[M + H]+	C17H30N2O4P	14.662475232609417	8.064939679043038	6.597535553566379	9	0.9888793275025645	-0.28967448264108003	0.9201704555380266	-0.1603459126330561	
8	C13H33N3O4P2	[M + H]+	C13H33N3O4P2	14.270528279509012	11.207732656567146	3.0627956229418665	10	0.9945969759520047	-0.4982304820065859	1.2778688586195766	-1.0278072909754976	
9	C19H31N2O2	[M + K]+	C19H31N2O2	11.942179098282763	10.62247877626541	1.3197003220173515	10	0.9945969759520045	-2.150500852166065	2.150500852166065	-0.48365552383681126	
10	C15H34N3O2P	[M + K]+	C15H34N3O2P	11.940863632556848	11.940863632556848	0.0	10	0.9945969759520045	-2.2458900015019885	2.2458900015019885	-1.351116902337944	
```

This is a ranking list of the top molecular formula candidates. The best
candidate is C<sub>21</sub>H<sub>27</sub>NO<sub>4</sub> with an overall score (*SiriusScore*) of 24.654. This score
is the sum of the *TreeScore* (16.674) and the *IsotopeScore* (7.980).

The rear columns contain the number of explained peaks in MS/MS
spectrum as well as the relative amount of explained intensity. The latter
value should usually be over 80 % or even 90 %. If this value is very
low you either have strange high intensive noise in your spectrum, or
the allowed mass deviation might be too low to explain all the peaks.


The `structure_candidates.tsv` in `<summary-files-dir>/0_laudanosine_FEATURE1` should be similar to this (trunctated version):

```
rank	formulaRank	ConfidenceScore	CSI:FingerIDScore	molecularFormula	adduct	InChIkey2D	InChI	name	smiles
1	1	0.8708826550014862	-3.262869483152835	C21H27NO4	[M + H]+	KGPAYJZAMGEDIQ	InChI=1S/C21H27NO4/c1-22-9-8-15-12-20(25-4)21(26-5)13-16(15)17(22)10-14-6-7-18(23-2)19(11-14)24-3/h6-7,11-13,17H,8-10H2,1-5H3	Laudanosine	CN1CCC2=CC(=C(C=C2C1CC3=CC(=C(C=C3)OC)OC)OC)OC
2	1	N/A	-86.96508835580653	C21H27NO4	[M + H]+	VDGYRASFPNRLPJ	InChI=1S/C21H27NO4/c1-22(2)9-8-15-12-18(23)20(25-4)13-16(15)17(22)10-14-6-7-19(24-3)21(11-14)26-5/h6-7,11-13,17H,8-10H2,1-5H3/p+1	1-[(3,4-dimethoxyphenyl)methyl]-7-methoxy-2,2-dimethyl-3,4-dihydro-1H-isoquinolin-2-ium-6-ol	C[N+]1(CCC2=CC(=C(C=C2C1CC3=CC(=C(C=C3)OC)OC)OC)O)C
3	1	N/A	-102.18328682988958	C21H27NO4	[M + H]+	VHLMUHYXPXECPC	InChI=1S/C21H27NO4/c1-22(2)9-8-15-12-20(25-4)21(26-5)13-16(15)17(22)10-14-6-7-19(24-3)18(23)11-14/h6-7,11-13,17H,8-10H2,1-5H3/p+1	null	C[N+]1(CCC2=CC(=C(C=C2C1CC3=CC(=C(C=C3)OC)O)OC)OC)C
4	1	N/A	-224.48821737216022	C21H27NO4	[M + H]+	ILJTXKNIYLUOKP	InChI=1S/C21H27NO4/c1-14-18(23-2)7-6-16(21(14)26-5)12-22-9-8-15-10-19(24-3)20(25-4)11-17(15)13-22/h6-7,10-11H,8-9,12-13H2,1-5H3	Oprea1_099380	CC1=C(C=CC(=C1OC)CN2CCC3=CC(=C(C=C3C2)OC)OC)OC
...
```

This is a ranking of the top structure candidates. The best hit has a confidence score telling you how certain this prediction is correct. 
Each structure hit also has a *formulaRank* since a compound may have different molecular formula candidates. 
The file contains all structure information (InChI, SMILES) and also structure database IDs (not shown here). 


The `formula_identifications.tsv` and `compound_identifications.tsv` in `<summary-files-dir>` contain similar information but only for the top hit of each analyzed compound.
The `formula_identifications_adduct.tsv` and `compound_identifications_adduct.tsv` contain this information for each possible adduct of each compound. 

If you want to look at the fragmentation trees, structures or compound classes visually, you can open the output (`<outputdir>`) 
in the GUI and use the included [Tree view tab]({{ "/gui/#tree-view-tab" | relative_url }}), [Structures tab]({{ "/gui/#structures-tab-csifingerid-detail-tab-in-sirius4" | relative_url }}) and
[CANOPUS tab]({{ "/gui/#canopus-tab" | relative_url }}) . 
The output can be imported by dropping the `<outputdir>` into the SIRIUS GUI application window.
Note that the viewer can also export the tree as vector graphics (svg/pdf).


### Example 2: MS files

The `demo-data/ms/` directory contains two examples of this format. Each file contains a
single compound measured with an Orbitrap instrument. To analyze this
compound run:

```shell
sirius -o <outputdir> -i demo-data/ms/Bicuculline.ms formula -p orbitrap
```

for formula annotation only or

```shell
sirius -o <outputdir> -i demo-data/ms/Bicuculline.ms formula -p orbitrap fingerprint compound-classes structure
```

to again perform structure database search and compound class annotation.

As the ms file already contains the correct molecular formula, SIRIUS
will directly compute the fragmentation tree without decomposing the
mass (like when specifying exactly one molecular formula via `-f` option).

If you want to enforce a molecular formula analysis and ranking
(although the correct molecular formula is given within the file) use
the `--ignore-formula` option to ignore molecular the formula in the file. 
The number of  formula candidates can be specified via the `-c` option.

```shell
sirius -o <outputdir> -i demo-data/ms/Bicuculline.ms --ignore-formula formula -p orbitrap -c 5
```

SIRIUS will now ignore the correct molecular formula in the file and
output the 5 best candidates.


## Background Service - Generic SIRIUS API

SIRIUS provides a REST API to access data from the project space and to run computations. 
You can either directly interact with this API or use the Python SDK.

The openAPI specification and documentation can be viewed via the browser. Just start SIRIUS and open [http://localhost:8080/](http://localhost:8080/).

The page should look similar to this:
{% capture fig_img %}
![Foo]({{ "/assets/images/swagger-api.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>SIRIUS API specification.</figcaption>
</figure>


Since there are so many endpoints, the start may be a bit overwhelming.
The following will help you to get an overview and make the best use of the API.

### Hello <del>world</del> structure candidates (your first tiny example)
First of all: remember that the API supports multiple SIRIUS projects. That is why you always need to specify the `projectId` if you want to query a feature.

Say you want to get the structure hits for the feature with name "InterestingCompound12". You need to do the following.
(Note: This can be directly performed via the swagger ui at [http://localhost:8080/](http://localhost:8080/). Please remember to change the commands matching to your query IDs.)

* get the list of open project spaces:`http://localhost:8080/api/projects`

Response:
```json
[
  {
    "projectId": "testdataset",
    "location": "/path/to/testdataset.sirius",
    "description": null,
    "compatible": null,
    "numOfFeatures": null,
    "numOfCompounds": null,
    "numOfBytes": null
  }
]
```
* get the list of features: `http://localhost:8080/api/projects/testdataset/aligned-features`

Response:
 ```json
[
...
  {
    "alignedFeatureId": "577173392664996156",
    "name": "InterestingCompound11",
    "ionMass": 212.1184774810981,
    "adduct": "[M + ?]+",
    "rtStartSeconds": 478.891,
    "rtEndSeconds": 487.677,
    "computing": false
  },
  {
    "alignedFeatureId": "577173392774048066",
    "name": "InterestingCompound12",
    "ionMass": 461.3699477422347,
    "adduct": "[M + ?]+",
    "rtStartSeconds": 447.536,
    "rtEndSeconds": 456.591,
    "computing": false
  },
...
]
```
* search for the feature with name `InterestingCompound12` and select its `alignedFeatureId`.
* get all structure candiates ranked by csiScore: `http://localhost:8080/api/projects/s6tomatoSmall/aligned-features/577173392774048066/db-structures`

Response: 
```json
[
{
"inchiKey": "PPYOSYACEILSKE",
"smiles": "CCCCCCCCC=CCCCCCCCC(=O)NCCCCC(C(=O)O)N(C)C",
"structureName": "N2,N2-Dimethyl-N6-[(9Z)-1-oxo-9-octadecen-1-yl]lysine",
"xlogP": 5.5,
"csiScore": -198.4107988137411,
"tanimotoSimilarity": 0.4523809523809524,
"mcesDistToTopHit": 0,
"molecularFormula": "C26H50N2O3",
"adduct": "[M + Na]+",
"formulaId": "577178082920468525"
},
{
"inchiKey": "URAUKAJXWWFQSU",
"smiles": "C1CCC(CC1)N(C2CCCCC2)C(=O)COCC(=O)N(C3CCCCC3)C4CCCCC4",
"structureName": "N,N,N',N'-Tetracyclohexyl-3-oxapentanediamide",
"xlogP": 6.4,
"csiScore": -217.50017505980176,
"tanimotoSimilarity": 0.31736526946107785,
"mcesDistToTopHit": "Infinity",
"molecularFormula": "C28H48N2O3",
"adduct": "[M + H]+",
"formulaId": "577178082920468526"
},
...
]
```

### Class hierarchy

The main classes in the hierarchy are **projects**, **alignedFeatures** and **formulas**. These have a one-to-many relation. 
See below an exemplary representation. 

```
project [projectId:1]
│   ...
│   ...    
│
└───alignedFeature [alignedFeatureId:577173392774048066]
│   │   ms-data
│   │   spectral-library-matches
│   │   db-structures
│   │   denovo-structures
│   │   
│   └───formula [formulaId:577178082920468525]
│   │   │   isotope-pattern
│   │   │   fragtree
│   │   │   fingerprint
│   │   │   lipid-annotation
│   │   │   canopus-prediction
│   │   │   best-compound-classes
│   │   │   db-structures
│   │   │   denovo-structures
│   │   
│   └───formula [formulaId:577178082920468526]
│   │   │   isotope-pattern
│   │   │   ...
│   
└───alignedFeature [alignedFeatureId:577173392664996156]
│   │   ms-data
│   │   ...
│
project [projectId:2]
│   ...
│   ...
```

Within these classes you can find data and results. Results such as structures are long list and thus also come with a 'paged'-endpoint (`/page`).
With this hierarchy in mind you can for example
* select all formula candidates of a feature: `/api/projects/{projectId}/aligned-features/{alignedFeatureId}/formulas`
* select all (database) structure candidates of a feature: `/api/projects/{projectId}/aligned-features/{alignedFeatureId}/db-structures`
* select all (de novo) structure candidates of a specific molecular formula candidate of a feature: `/api/projects/{projectId}/aligned-features/{alignedFeatureId}/formulas/{formulaId}/denovo-structures`
* select the molecular fingerprint of a specific molecular formula candidate of a feature: `/api/projects/{projectId}/aligned-features/{alignedFeatureId}/formulas/{formulaId}/fingerprint`

