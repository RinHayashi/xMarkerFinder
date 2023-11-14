# Identification and validation of microbial biomarkers from cross-cohort datasets using xMarkerFinder
xMarkerFinder is a four-stage workflow for microbiome research including differential signature identification, model construction, model validation, and biomarker interpretation. Detailed [scripts](./scripts), [example files](./data), and a ready-to-use [docker image](https://hub.docker.com/repository/docker/tjcadd2022/xmarkerfinder) are provided.
We also provide a user-friendly [web server](https://www.biosino.org/xmarkerfinder/) for easier implementation. Feel free to explore the web server and discover more about xMarkerFinder!
Manuscript is available at https://doi.org/10.21203/rs.3.pex-1984/v1. 

![ ](https://img.shields.io/badge/python-3.7-blue) ![GitHub top language](https://img.shields.io/github/languages/top/tjcadd2020/xMarkerFinder)  ![GitHub](https://img.shields.io/github/license/tjcadd2020/xMarkerFinder) ![GitHub code size in bytes](https://img.shields.io/github/languages/code-size/tjcadd2020/xMarkerFinder)  ![GitHub issues](https://img.shields.io/github/issues/tjcadd2020/xMarkerFinder) [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/tjcadd2020/xMarkerFinder/HEAD) [![](https://img.shields.io/badge/website-CADD-lightgrey)](https://cadd.tongji.edu.cn/)  [![Snakemake](https://img.shields.io/badge/snakemake-≥5.6.0-brightgreen.svg?style=flat)](https://snakemake.readthedocs.io)
  
<img src="https://github.com/tjcadd2020/xMarkerFinder/assets/54845977/a01be325-0832-41c9-a1c6-140d1bd42325" alt="Image" width="200">


## Citation
Please cite: Wenxing Gao, Wanning Chen, Wenjing Yin et al. Identification and validation of microbial biomarkers from cross-cohort datasets using xMarkerFinder, 25 August 2022, PROTOCOL (Version 1) available at Protocol Exchange [https://doi.org/10.21203/rs.3.pex-1984/v1]

## Table of Contents
* [Installation](#installation)
  * [Hardware](#hardware)
  * [Softwre](#software)
  * [Software setup](#software-setup)
* [User tutorial](#user-tutorial)
  * [Stage 1 Differential signature identification](#stage-1-differential-signature-identification)
  * [Stage 2 Model construction](#stage-2-model-construction)
  * [Stage 3 Model validation](#stage-3-model-validation)
  * [Stage 4 Biomarker interpretation](#stage-4-biomarker-interpretation)
* [Case study](#case-study)
  * [16S rRNA gene sequencing data of OSCC patients](#human-microbiome)
  * [Whole metagenomics data of Tara Ocean](#ocean-microbiome)
  * [Transcriptomics data of NASH patients](#human-transcriptome)
* [FAQs](#faqs)
  * [Part I General questions](#part-i-general-questions)
  * [Part II Data processing](#part-ii-data-processing)
  * [Part III Using xMarkerFinder](#part-iii-using-xmarkerfinder)


## Installation


### Hardware
The protocol can be executed on standard computational hardware, and greater computational resources would allow for faster execution. The development and test of this protocol have been conducted on a MacBook Pro equipped with a 2.4-GHz quad-core eighth-generation Intel Core i5 processor and 16-GB 2133-MHz LPDDR3 memory.
### Software
- R v.3.6.1 or newer (https://www.r-project.org)
- Python3 v3.7 or newer (https://www.python.org)
- HAllA (https://huttenhower.sph.harvard.edu/halla)
- FastSpar (https://github.com/scwatts/FastSpar)
- Gephi (https://gephi.org)
#### R packages
- BiocManager (https://github.com/Bioconductor/BiocManager) to ensure the installation of the following packages and their dependencies.
- MMUPHin (https://huttenhower.sph.harvard.edu/mmuphin)
- dplyr (https://dplyr.tidyverse.org/)
- vegan (https://github.com/vegandevs/vegan)
- XICOR (https://github.com/cran/XICOR)
- eva (https://github.com/brianbader/eva_package)
- labdsv (https://github.com/cran/labdsv)
- Boruta (https://gitlab.com/mbq/Boruta, optional)
#### python packages
- pandas (https://pandas.pydata.org)
- NumPy (https://numpy.org/)
- scikit-learn (https://scikit-learn.org)
- bioinfokit (https://github.com/reneshbedre/bioinfokit)
- Bayesian Optimization (https://github.com/fmfn/BayesianOptimization)
- Matplotlib (https://matplotlib.org/)
- seaborn (https://seaborn.pydata.org/)
#### Docker image
Above software list provides the minimal requirements for the complete execution of xMarkerFinder locally. Alternatively, we provide a ready-to-use Docker image, enabling users to skip the software installation and environment setup (https://hub.docker.com/r/tjcadd2022/xmarkerfinder). Additionally, an interactive JupyterHub server (https://mybinder.org/v2/gh/tjcadd2020/xMarkerFinder/HEAD) is also available.
### Software setup
#### Installation of R and R packages
Installation of R on different platforms can be conducted following the instructions on the official website (https://www.r-project.org/). All R packages used in this protocol can be installed following given commands.
```
> install.packages(Package_Name)
```
or
```
> if (!requireNamespace(“BiocManager”, quietly = TRUE)) 
> install.packages(“BiocManager”)
> BiocManager::install(Package_Name)
```
#### Installation of python and python packages
Python can be downloaded and installed from its official website (https://www.python.org/), and all python packages could be installed using pip.
```
$ pip install Package_Name
```
#### Installation of HAllA
HAllA can be installed according to its website (https://huttenhower.sph.harvard.edu/halla/) with the following command.
```
$ pip install halla
```
#### Installation of FastSpar
FastSpar can be installed following its GitHub repository (https://github.com/scwatts/fastspar).
Installation through conda:
```
$ conda install -c bioconda -c conda-forge fastspar
```
Or compiling from source code:
```
$ git clone https://github.com/scwatts/fastspar.git
$ cd fastspar
$./autogen.sh
$./configure --prefix=/usr/
$ make
$ make install
```
#### Installation of Gephi
Gephi could be freely downloaded and installed from its website (https://gephi.org/). 
<img width="415" alt="image" src="https://github.com/tjcadd2020/xMarkerFinder/assets/54845977/101422c1-fb1f-4f97-8553-d447264b4d43">
#### Docker image setup
To provide easier implementation, we provide a Docker image to replace above Equipment setup steps excluding Gephi. Firstly, users should download and install Docker (https://docs.docker.com/engine/install/) and then setup the xMarkerFinder computational environment. All scripts in the Procedure part below should be executed within the Docker container created from the xMarkerFinder Docker image.

```
$ docker pull tjcadd2022/xmarkerfinder:1.0.16
$ docker run -it -v $(pwd):/work tjcadd2022/xmarkerfinder:1.0.16 /bin/bash  
```
```
-it Run containers in an interactive mode, allowing users to execute commands and access files within the docker container.  
-v Mounts a volume between present working directory in your local machine to the /work directory in the docker container.  
```

## User tutorial
### Stage 1 Differential signature identification  
#### 1. Data normalization. 
To mitigate challenges induced by different number of sequencing (e.g., library size), microbial count matrices are often normalized by various computational strategies prior to downstream analyses. Here, xMarkerFinder takes the proportional normalization as its default algorithm for determining relative abundances (REL), other normalization methods are also available, including AST, CLR, and TMM. 
```
$ Rscript 1_Normalization.R -W /workplace/ -p abundance.txt -o TEST
```  
Users should specify these parameters or enter the default values, subsequent repetitions of which are not listed.   
```
-W the Workplace of this whole protocol  
-p the input microbial count profile
-m the normalization method (REL, AST, CLR, TMM)
-o prefix of output files
```  
- Input files:  
abundance.txt: merged microbial count profile of all datasets.  
- Output files:  
normalized_abundance.txt: normalized abundance profile of input dataset. Normalized abundance profiles are used as input files for all subsequent analyses, except for Step 11, which requires raw count file.  
#### 2.	Data filtering. 
Rare signatures, those with low occurrence rates across cohorts are discarded (default: prevalence below 20% of samples) to ensure that identified *biomarkers* are reproducible and could be applied to prospective cohorts.  
```
$ Rscript 2_Filtering.R -W /workplace/ -m train_metadata.txt -p normalized_abundance.txt -b Cohort -t 2 -o TEST  
```
```
-m the input metadata file  
-p the input microbial normalized abundance file (output file of Step 1)  
-b the column name of batch(cohort) in metadata (default: Cohort)  
-t the minimum number of cohorts where features have to occur (default: 2)  
-O prefix of output files  
```
- Input files:  
train_metadata.txt: the clinical metadata of the training dataset.  
normalized_abundance.txt: normalized abundance profile of the training dataset.  
- Output files:  
filtered_abundance.txt: filtered normalized abundance profile of the training dataset, used as the input file for following steps.  
#### 3.	Confounder analysis.   
Inter-cohort heterogeneity caused by variance in confounders is inevitable in meta-analyses, strongly affecting downstream differential signature identification. Permutational multivariate analysis of variance (PERMANOVA) test, one of the most widely used nonparametric methods to fit multivariate models based on dissimilarity metric in microbial studies, quantifies microbial variations attributable to each metadata variable, thus assigning a delegate to evaluate confounding effects. PERMANOVA test here is performed on Bray-Curtis (recommended for REL and TMM normalized data) or Eucledian (recommended for AST and CLR normalized data) dissimilarity matrices. For each metadata variable, coefficient of determination (R2) value and *p* value are calculated to explain how variation is attributed. The variable with the most predominant impact on microbial profiles is treated as major batch, and other confounders are subsequently used as covariates in Step 4. Principal coordinate analysis (PCoA) plot is also provided.
```
$ Rscript 3_Confounder_analysis.R -W /workplace/ -m train_metadata.txt -p filtered_abundance.txt -d bc -c 999 -g Group -o TEST  
```
```
-m input metadata file  
-p input filtered microbial abundance file
-d distance matrix (bc, euclidean)
-c permutation count (default: 999)
-g the column name of experimental interest(group) in metadata (default: Group)  
```
- Input files:  
train_metadata.txt: the clinical metadata of the training dataset.  
filtered_abundance.txt: filtered abundance profile after preprocessing.  
- Output files:  
metadata_microbiota.txt: the confounding effects caused by clinical information, used to determine the major batch and covariates.  
pcoa_plot.pdf: the PCoA plot with Bray-Curtis dissimilarity between groups.  
#### 4.	Differential analysis.   
To identify disease or trait-associated microbial signatures across cohorts, MMUPHin is employed. Regression analyses in individual cohorts are performed using the well-validated Microbiome Multivariable Association with Linear Models (MaAsLin2) package, where multivariable associations between phenotypes, experimental groups or other metadata factors and microbial profiles are determined. These results are then aggregated with established fixed effects models to test for consistently *differential signatures* between groups with the major confounder (determined in Step 3) set as the main batch and other minor confounders (e.g., demographic indices, technical differences) as covariates. Signatures with consistently significant differences in meta-analysis are identified as cross-cohort differential signatures and used for further feature selection in subsequent stages. Users can choose from using *p* values or adjusted *p* values. Volcano plot of differential signatures is provided.
```
$ Rscript 4_Differential_analysis.R -W /workplace/ -m train_metadata.txt -p filtered_abundance.txt -g Group -b Cohort -c covariates.txt -d p -t 0.05 -o TEST
```
```
-g the column name of experimental interest(group) in metadata (default: Group)  
-b the column name of major confounder in metadata (default: Cohort)  
-c input covariates file (tab-delimited format containing all covariates)
-d input choice indicating whether to use the adjusted p values rather than the raw p values and the adjusting values (F,bonf,fdr)
-t the threshold of p or q value for plotting (default: 0.05)  
```
- Input files:  
train_metadata.txt: the clinical metadata of the training dataset.  
filtered_abundance.txt: filtered abundance profile after preprocessing.  
covariates.txt: covariates identified in Step 3 (newly generated tab-delimited file where each row is a covariate, example file is provided).  
- Output files:  
differential_significance_single_cohort.txt: the differential significance result in individual cohorts.  
differential_significance.txt: meta-analytic testing results aggregating differential testing results in individual cohorts, used for next-step visualization.  
differential_signature.txt: significantly *differential signatures* between groups derived from input filtered profiles, used as input files for feature selection.  
differential_volcano.pdf: the volcano plot of input differential significance file.     
### Stage 2 Model construction
#### 5.	Classifier selection.   
This step provides optional classifier selection for subsequent steps where the performances of every ML algorithm are generally assessed using all *differential signatures*. The output file contains the cross-validation AUC, AUPR, MCC, specificity, sensitivity, accuracy, precision, and F1 score of all classification models built with these various algorithms. Users should specify the selected classifier in all the following steps.
differential_signature.txt: significantly differential signatures between groups derived from input filtered profiles, used as input files for feature selection.  
differential_volcano.pdf: the volcano plot of input differential significance file.     
### Stage 2 Model construction
#### 5.	Classifier selection.   
This step provides optional classifier selection for subsequent steps where the performances of every ML algorithm are generally assessed using all differential signatures. The output file contains the cross-validation AUC, AUPR, MCC, specificity, sensitivity, accuracy, precision, and F1 score of all classification models built with these various algorithms. Users should specify the selected classifier in all the following steps.
```
$ python 5_Classifier_selection.py -W /workplace/ -m train_metadata.txt -p differential_signature.txt -g Group -e exposure -s 0 -o TEST
```
```
-p input differential signature file (output file of Step 4)
-g the column name of experimental interest(group) in metadata (default: Group)
-e the experiment group(exposure) of interest (in example data: CRC)
-s random seed (default:0)
```
- Input files:  
train_metadata.txt: the clinical metadata of the training dataset.  
differential_signature.txt: significantly *differential signatures* between groups.  
- Output files:  
classifier_selection.txt: the overall cross-validation performance of all classifiers using differential signatures, used to determine the most suitable classifier.  
#### 6.	Feature selection.
##### 6a. Feature effectiveness evaluation  
The first step of Triple-E feature selection procedure evaluates the predictive capability of every feature via constructing individual classification models respectively. Users should specify an ML algorithm here and in every future step as the overall classifier for the whole protocol from the following options: *LRl1*, *LRl2*, *KNN*, *SVC*, *DT*, *RF*, and *GB*. Features with cross-validation AUC above the threshold (default:0.5) are defined as *effective features* and are returned in the output file.  
```
$ python 6a_Feature_effectiveness_evaluation.py -W /workplace/ -m train_metadata.txt -p differential_signature.txt -g Group -e exposure -b Cohort -c classifier -s 0 -t 0.5 -o TEST
```
```
-p input differential signature file (output file of Step 4)
-b the column name of batch(cohort) in metadata (default: Cohort)
-c selected classifier
-t AUC threshold for defining if a feature is capable of prediction (default:0.5)
```
- Input files:  
train_metadata.txt: the clinical metadata of the training dataset.  
differential_signature.txt: significantly *differential signatures* between groups.  
- Output files:  
feature_auc.txt: cross-validation AUC values of individual features.  
effective_feature.txt: features derived from *differential signatures* that are capable of predicting disease states, used as input file of the following step.  
##### 6b.	Collinear feature exclusion.   
The second step of feature selection aims to exclude collinear issue caused by highly correlated features based on the result of Step 7 and returns the uncorrelated-effective features.
```
$ python 6b_Collinear_feature_exclusion.py -W /workplace/ -p effective_feature.txt -t 0.7 -o TEST
```
```
-p input effective feature file (output file of Step 6a)
-t correlation threshold for collinear feature exclusion (default:0.7)
```
- Input files:  
metadata.txt: the clinical metadata of the training dataset.  
effective_feature.txt: features with classification capability.  
- Output files:  
feature_correlation.txt: spearman correlation coefficients of every feature pair.  
uncorrelated_effective_feature.txt: features derived from input *effective features* excluding highly collinear features, used as input file of the following step.  
##### 6c.	Recursive feature elimination.   
The last step of feature selection recursively eliminates the weakest feature per loop to sort out the minimal panel of *candidate biomarkers*.  
```
$ python 6c_Recursive_feature_elimination.py -W /workplace/ -m train_metadata.txt -p uncorrelated_effective_feature.txt -g Group -e exposure -c classifier -s 0 -o TEST
```
```
-p input uncorrelated-effective feature file (output file of Step 6b)
```
- Input files:  
train_metadata.txt: the clinical metadata of the training dataset.  
uncorrelated_effective_feature.txt: independent features derived from *effective features*.  
- Output files:  
candidate_biomarker.txt: identified optimal panel of *candidate biomarkers*, used as model input for all subsequent steps.  
#### 6*.	Boruta feature selection. 
Besides Triple-E feature selection procedure, we provide an alternative method, feature selection with the Boruta algorithm.  
```
$ Rscript alt_6_Boruta_feature_selection.R -W /workplace/ -m metadata.txt -p differential_signature.txt -g Group -s 0 -o TEST
```
```
-p input differential signature profile (output file of Step 4) or uncorrelated-effective feature file (output file of Step 6b)
```
- Input files:  
metadata.txt: the clinical metadata of the training dataset.  
differential_signature.txt: *differential signatures* used for feature selection (could also be *uncorrelated-effective features* from Step 6b).  
- Output files:  
boruta_feature_imp.txt: confirmed feature importances via Boruta algorithm.  
boruta_selected_feature.txt: selected feature profile, used as input *candidate biomarkers* for subsequent steps.  
#### 7.	Hyperparameter tuning.   
Based on the selected classifier and *candidate biomarkers*, the hyperparameters of the classification model are adjusted via bayesian optimization method based on cross-validation AUC. The output files contain the tuned hyperparameters and the multiple performance metric values of the constructed best-performing model.  
```
$ python 7_Hyperparameter_tuning.py -W /workplace/ -m train_metadata.txt -p candidate_biomarker.txt -g Group -e exposure -c classifier -s 0 -o TEST
```
```
-p input candidate marker profile (output file of Step 6)
```
- Input files:  
train_metadata.txt: the clinical metadata of the training dataset.  
candidate_biomarker.txt: the optimal panel of *candidate biomarkers* (or boruta_selected_feature.txt for all subsequent steps).  
- Output files:  
best_param.txt: the best hyperparameter combination of classification model.  
optimal_cross_validation.txt: the overall cross-validation performance of the best-performing model.  
cross_validation_auc.pdf: the visualization of the cross-validation AUC of the best-performing model.   
### Stage 3 Model validation
#### 8.	Internal validations (8a. intra-cohort, 8b. cohort-to-cohort, and 8c. LOCO validation). 
As stated above, this step provides extensive internal validations to ensure the robustness and reproducibility of identified *candidate biomarkers* in different cohorts via intra-cohort validation, cohort-to-cohort transfer, and LOCO validation. Output files contain multiple performance metrics used to assess the markers internally, including AUC, specificity, sensitivity, accuracy, precision and F1 score.  
```
$ python 8_Validation.py -W /workplace/ -m metadata.txt -p candidate_biomarker.txt -g Group -e exposure -b Cohort -c classifier -s 0 -o TEST
```
```
-p input optimal candidate marker file (output file of Step 6)
```
- Input files:  
metadata.txt: the clinical metadata of the training dataset.  
candidate_biomarker.txt: the optimal panel of candidate markers.  
- Output files:  
validation_metric.txt: the overall performance of *candidate biomarkers* in internal validations. 
validation_metric.pdf: the visualization of input file.  
#### 9.	External validation.
##### 9a. Independent test.
As the best-performing *candidate biomarkers* and classification model are established, the test dataset is used to externally validate their generalizability. The input external metadata and microbial relative profiles need to be in the same format as initial input files for the training dataset. This step returns the overall performance of the model and its AUC plot.  
```
$ python 9a_Test.py -W /workplace/ -m train_metadata.txt -p candidate_biomarker.txt -a external_metadata.txt -x external_profile.txt -g Group -e exposure -c classifier -r hyperparamter.txt -s 0 -o TEST
```
```
-a input external metadata file for the test dataset
-x input external microbial relative abundance file as the test dataset
-r input optimal hyperparameter file (output file of Step 7)
```
- Input files:  
train_metadata.txt: the clinical metadata of the training dataset.  
candidate_biomarker.txt: the optimal panel of *candidate biomarkers*.  
test_metadata.txt: the clinical metadata of the external test dataset.  
test_profile.txt: the relative abundance matrix of the external test dataset.  
- Output files:  
test_result.txt: the overall performance of model in external test dataset.  
test_auc.pdf: the visualization of the AUC value in test_result.txt.  
##### 9b.	Biomarker specificity assessment.   
To further assess markers’ specificity for the experimental group of interest, they are used to construct classification models to discriminate between other related diseases and corresponding controls. Cross-validation AUC values of other classification models and visualization are returned.   
```
$ python 9b_Specificity.py -W /workplace/ -p candidate_biomarker.txt -q test_metadata.txt -l test_relative_abundance.txt -a other_metadata.txt -x other_relative_abundance.txt -g Group -e CTR -b Cohort -c classifier -r best_param.txt -s 0 -o TEST
```
```
-q input external test metadata file for the test dataset
-l input external microbial relative abundance file as the test dataset
-a input metadata file of samples from other non-target diseases
-x input microbial relative abundance file of samples from other non-target diseases
-e the control group name (in example file: CTR)
-b the column name of cohort(in example file: Cohort)
```
- Input files:  
candidate_biomarker.txt: the optimal panel of *candidate biomarkers*.  
other_metadata.txt: the clinical metadata of samples for other diseases.  
other_profile.txt: the relative abundance matrix of other diseases.  
- Output files:  
specificity_result.txt: AUC values of models constructed with *candidate biomarkers* in other related diseases.  
specificity_auc.pdf: the visualization of the specificity_result.txt.  
##### 9c.	Model specificity assessment.   
Random samples of case and control class of other diseases are added into the classification model, respectively, both labeled as “control”, the variations of corresponding AUCs of which are calculated and used for visualization.   
```
$ python 9c_Specificity_add.py -W /workplace/ -m train_metadata.txt -p candidate_biomarker.txt -q test_metadata.txt -l test_profile.txt -a other_metadata.txt -x other_profile.txt -g Group -e exposure -b Cohort -c classifier -r hyperparamter.txt -n 5 -s 0 -o TEST
```
```
-q input external metadata file for the test dataset
-l input external microbial relative abundance file as the test dataset
-a input metadata file of samples from other diseases
-x input microbial relative abundance file of samples from other non-target diseases
-e the control group name (in example file: CTR)
-b the column name of cohort(dataset)
-n the number of samples to add into the model each time 
```
- Input files:  
train_metadata.txt: the clinical metadata of the training dataset.  
candidate_biomarker.txt: the optimal panel of *candidate markers*.  
test_metadata.txt: the clinical metadata of the external test dataset.  
test_profile.txt: the relative abundance matrix of the external test dataset.  
other_metadata.txt: the clinical metadata of samples for other non-target diseases.  
other_profile.txt: the relative abundance matrix of other non-target diseases.  
- Output files:  
specificity_add_result.txt: AUC values of models constructed with candidate markers in other non-target diseases.  
specificity_add_auc.pdf: the visualization of the specificity_result.txt.  
### Stage 4 Biomarker interpretation.
#### 10.	Biomarker importance.
Permutation feature importance is employed here to evaluate biomarkers’ contributions in the best-performing classification model.  
```
$ python 10_Biomarker_importance.py -W /workplace/ -m train_metadata.txt -p candidate_biomarker.txt -g Group -e exposure -c classifier -r best_param.txt -s 0 -o TEST
```
```
-p input candidate biomarkers (output file of Step 6)
-r input optimal hyperparameter file (output file of Step 7)
-n input number for biomarker abundance visualization
```
- Input files:  
train_metadata.txt: the clinical metadata of the training dataset.  
candidate_biomarker.txt: the optimal panel of *candidate biomarkers*.  
best_param.txt: the best hyperparameter combination of classification model.  
- Output files:  
biomarker_importance.txt: permutation feature importance of *candidate biomarkers* via ten permutations.  
biomarker_importance.pdf: the visualization of feature importance file.
Biomarker_distribution.pdf: the visualization for the abundances of the top n (set by users) important biomarkers in the discovery dataset.
#### 11.	Microbial co-occurrence network.  
Inter-microbiota correlation is calculated using FastSpar with 50 iterations and the output files contain the correlation and p value between each microbiota pair.   
##### 11a. Convert.
As the input file for Step 11b needs to be microbial count profile in .tsv format where each row describes a microbial signature and each column represents a sample (could be converted profiles of all features, *differential signatures*, or *candidate biomarkers* according to users’ need, and null values needed to be set as 0) and header needs to start with “#OTU ID”, an additional file conversion script is provided.  
```
$ python 11a_Convert.py -W /workplace/ -p abundance.txt -s selected_feature.txt -o TEST
```
```
-p input feature raw count file before normalization.
-s selected features for calculating microbial correlation (could be differential signatures or candidate markers, output file of Step 4 or 6).
```
- Input files:  
abundance.txt: microbial raw count profile before normalization.  
selected_feature.txt: selected features for calculating microbial co-occurrence network (output file of Step 4 or 6)  
- Output files:  
convert.tsv: the converted file appropriate for constructing microbial co-occurrence network.  
##### 11b. Microbial co-occurrence network.
```
$ ./11b_Microbial_network.sh -W /workplace/ –i feature_abundance.tsv -o TEST -t 4
```
```
-i input feature abundance file  
-t threads of available computational source  
```
- Input files:  
convert.tsv: microbial count profile in .tsv format where each row describes a microbial signature and each column represents a sample and the header needs to start with “#OTU ID”. Example input file is provided and users are recommended to user Step 11a to convert files into appropriate formats.  
-t the threads of available computational source when running  
- Output files:  
median_correlation.tsv: the correlation coefficients between every input signature pair.  
pvalues.tsv: the statistical significance of median_correlation.tsv.  
##### 11c. Microbial co-occurrence network plot. 
The visualization of Step 11 is performed using Gephi.  
(i) Preprocess of the results of Step 11b to ensure that Step 11c only draws significant correlations (pvalues<0.05) with absolute correlation coefficients above 0.5 (default).
```
$ python 11c_Microbial_network_plot.py -W /workplace/ –c median_correlation.tsv -p pvalues.tsv -t 0.5 -o TEST 
```
```
-c input network profile (output file of Step 11b)
-p input pvalue profile (output file of Step 11b)
-t input correlation threshold (default: 0.5)
```
- Input files:
median_correlation.tsv: the correlation coefficients profile (output file of Step 11b).
pvalues.tsv: the statistical significance of median_correlation.tsv (output file of Step 11b).
- Output files:
microbial_network.csv: adjusted network profile for Gephi input, only significant correlations reserved.  
(ii)	Open Gephi and click "File" – "Import spreadsheet", and then choose the adjusted network profile.  
<img width="415" alt="image" src="https://user-images.githubusercontent.com/54845977/173520689-3f4a34e5-e2a6-4ba8-b2ae-b8160ebb0d1d.png">

(iii) Import the network file.  
<img width="383" alt="image" src="https://user-images.githubusercontent.com/54845977/173520895-9c15d969-13eb-4b07-9c9c-fa83b6ae00e9.png">

(iv)  Choose a preferable layout type to form the basic network and press the “stop” button when the network becomes stable (Fruchterman Reingold style is recommended).    
<img width="415" alt="image" src="https://user-images.githubusercontent.com/54845977/171319813-0fed579e-6c7d-4581-bf7e-174aa8d391e1.png">   
(v)  For further optimization of the network, the appearances of nodes and edges should be adjusted according to users’ needs, as well as the labels of nodes.  
<img width="415" alt="image" src="https://user-images.githubusercontent.com/54845977/171319835-a572168e-fad4-47d0-a03b-16b528c99d54.png">    

#### 12.	Multi-omics correlation. 
If users have multi-omics or multidimensional microbial profiles of the same dataset, the correlation between different omics or dimensions is calculated via HAllA.
```
$ ./12_Multi_omics_correlation.sh -W /workplace/ -i microbial_abundance_1.txt -d microbial_abundance_2.txt -o TEST
```
```
-i input microbial abundance file 1
-d input microbial abundance file 2
```
- Input files:  
microbial_abundance_1.txt: microbial abundance profile 1.  
microbial_abundance_2.txt: microbial abundance profile 2. These two input files should have the same samples (columns) but different features (rows).  
- Output files:  
results/all_associations.txt: associations between different omics or dimensions.  
results/hallagram.png: the visualization of all_associations.txt with only significant associations highlighted.   


## Case study
It’s worth highlighting that xMarkerFinder is designed as a standard protocol with a high level of impartiality regarding data type and microbial habitat. In other words, xMarkerFinder’s versatility goes beyond its initial purpose in gut microbiome research, making it suitable for diverse microbial biomes. 
To provide further clarity, we present three examples showcasing the application of xMarkerFinder across various contexts. 
#### Human microbiome
Firstly, we used datasets from previous publications containing 16S rRNA gene sequencing data of the oral microbiome of patients with oral squamous cell carcinoma (OSCC) and controls. We applied xMarkerFinder to these oral microbiome datasets and successfully identified consistent microbial signatures associated with OCSS with great diagnostic capabilities.   
<img width="800" alt="image" src="https://github.com/tjcadd2020/xMarkerFinder/assets/54845977/4ac5018f-7f98-429f-9ce7-6d9ce6c71d26">   
#### Ocean microbiome
Secondly, we employed metagenomic datasets from the Tara Ocean project to characterize important microbiota within the oceanic environment, capable of distinguishing between deep and surface regions.     
<img width="800" alt="image" src="https://github.com/tjcadd2020/xMarkerFinder/assets/54845977/434ac5a6-b1d5-499d-b58f-6db2fe0ff11f">    
#### Human transcriptome
To demonstrate its generalizability in different omics data, we further applied xMarkerFinder to transcriptomic datasets of non-alcoholic steatohepatitis (NASH) patients, using three publicly available NASH cohorts. The resulting classification model reached an impressive AUC value of 0.99, highlighting the robustness and applicability of xMarkerFinder.  
<img width="800" alt="image" src="https://github.com/tjcadd2020/xMarkerFinder/assets/54845977/27b3377f-5382-47ac-b8c4-816286666d1f">  
These examples collectively serve as compelling evidence of the extensive scope of applicability inherent in xMarkerFinder.



## FAQs
### Part I General questions
#### 1. When should I use xMarkerFinder?  
xMarkerFinder is suitable for microbial biomarker identification from cross-cohort datasets. Our previous studies demonstrated its applicability in identifying global microbial diagnostic biomarkers for adenoma and colorectal cancer. Moreover, xMarkerFinder could also be applied to biomarker determination in disease prognosis, treatment stratification, metastasis surveillance, adverse reactions anticipation, etc. Any research dedicated to biomarker identification from multi-population microbial datasets is welcome.
#### 2. How should I set up the required computational environment for xMarkerFinder?  
We provide detailed instructions on software installation for users to run the whole xMarkerFinder workflow locally. However, we strongly encourage the usage of the provided docker image as it would significantly reduce potential errors in the entire installation and setup process. (https://hub.docker.com/r/tjcadd2022/xmarkerfinder)
#### 3. Can I access and modify the codes used in xMarkerFinder?  
Yes. The [codes](./scripts) used in xMarkerFinder are deposited in our GitHub repository and can be freely downloaded and modified according to users’ specific needs. However, the modification might cause unprecedented errors and we encourage users to try different parameters first, and then modify the codes.
#### 4. Can I use only certain steps of xMarkerFinder and skip other parts?  
Yes. The whole xMarkerFinder workflow contains four stages (12 steps) and every stage/step can be conducted independently and users could skip any one of them according to specific study designs.
#### 5. Can I use xMarkerFinder for environmental microbiome research?  
Yes. Although xMarkerFinder is developed for human microbiome studies, it is also generalizable to other microbial habitats. 
#### 6. How long does it take to run xMarkerFinder?  
The time needed for the whole workflow depends on the dataset size, selected algorithm, and computational resources available. The following time estimates are based on execution of our protocol on provided example datasets with all classifiers (Logistic Regression (LR, L1 and L2 regularization), K-nearest Neighbors (KNN) classifier, Support Vector classifier (SVC) with the Radial Basis Function kernel), Decision Tree (DT) classifier, Random Forest(RF) classifier, and Gradient Boosting (GB) classifier using the xMarkerFinder docker image on a MacBook Pro (2.4-GHz quad-core eighth-generation Intel Core i5 processor, 16-GB 2133-MHz LPDDR3 memory).  
|     Stage                                                   |     Step     |     LRl1          |     LRl2          |     SVC           |     KNN           |     DT            |     RF             |     GB             |
|-------------------------------------------------------------|--------------|-------------------|-------------------|-------------------|-------------------|-------------------|--------------------|--------------------|
|     Stage1：     Differential signature   identification    |     1        |     0m20.600s     |     0m20.600s     |     0m20.600s     |     0m20.600s     |     0m20.600s     |     0m20.600s      |     0m20.600s      |
|                                                             |     2        |     0m11.372s     |     0m11.372s     |     0m11.372s     |     0m11.372s     |     0m11.372s     |     0m11.372s      |     0m11.372s      |
|                                                             |     3        |     1m21.356s     |     1m21.356s     |     1m21.356s     |     1m21.356s     |     1m21.356s     |     1m21.356s      |     1m21.356s      |
|                                                             |     4        |     0m24.858s     |     0m24.858s     |     0m24.858s     |     0m24.858s     |     0m24.858s     |     0m24.858s      |     0m24.858s      |
|                                                             |     Total    |     2m18.186s     |     2m18.186s     |     2m18.186s     |     2m18.186s     |     2m18.186s     |     2m18.186s      |     2m18.186s      |
|     Stage2：     Model construction                         |     5        |     0m12.464s     |     0m12.464s     |     0m12.464s     |     0m12.464s     |     0m12.464s     |     0m12.464s      |     0m12.464s      |
|                                                             |     6a       |     0m2.733s      |     0m3.032s      |     0m50.913s     |     0m3.105s      |     0m3.252s      |     1m43.332s      |     0m49.196s      |
|                                                             |     6b       |     0m0.846s      |     0m1.150s      |     0m1.102s      |     0m1.178s      |     0m1.015s      |     0m0.863s       |     0m1.216s       |
|                                                             |     6c       |     0m2.447s      |     0m18.449s     |     10m32.261s    |     0m21.103s     |     0m53.413s     |     18m37.552s     |     47m59.647s     |
|                                                             |     6*       |     24m59.785s    |     24m59.785s    |     24m59.785s    |     24m59.785s    |     24m59.785s    |     24m59.785s     |     24m59.785s     |
|                                                             |     7        |     0m30.420s     |     0m24.735s     |     0m35.112s     |     0m42.348s     |     0m34.801s     |     8m57.417s      |     8m12.045s      |
|                                                             |     Total    |     25m48.695s    |     25m59.615s    |     37m11.637s    |     26m19.983s    |     26m44.730s    |     54m31.413s     |     82m14.353s     |
|     Stage3：     Model validation                           |     8        |     4m30.737s     |     4m42.105s     |     10m15.050s    |     6m10.515s     |     4m31.044s     |     91m52.940s     |     65m47.511s     |
|                                                             |     9a       |     0m3.896s      |     0m3.776s      |     0m3.150s      |     0m3.761s      |     0m4.002       |     0m7.120s       |     0m4.266s       |
|                                                             |     9b       |     0m4.877s      |     0m4.764s      |     0m4.426s      |     0m5.287s      |     0m5.315s      |     2m25.064s      |     0m36.946s      |
|                                                             |     9b*      |     0m5.941s      |     0m5.982       |     0m22.211s     |     0m7.342s      |     0m6.646s      |     2m21.262s      |     0m39.554s      |
|                                                             |     Total    |     4m45.451s     |     4m56.627      |     10m44.837s    |     6m26.905s     |     4m47.007s     |     96m46.386s     |     67m8.277s      |
|     Stage4：     Biomarker interpretation                   |     10       |     0m3.270s      |     0m3.599s      |     0m16.746s     |     0m21.809s     |     0m4.041s      |     0m46.265s      |     0m5.028s       |
|                                                             |     11       |     6m32.696s     |     6m32.696s     |     6m32.696s     |     6m32.696s     |     6m32.696s     |     6m32.696s      |     6m32.696s      |
|                                                             |     12       |     7m57.119s     |     7m57.119s     |     7m57.119s     |     7m57.119s     |     7m57.119s     |     7m57.119s      |     7m57.119s      |
|                                                             |     Total    |     14m33.085s    |     14m33.414s    |     14m46.561s    |     14m51.624s    |     14m33.856s    |     15m16.080s     |     14m34.843s     |
|     Total                                                   |     /        |     47m25.417s    |     47m47.842s    |     65m1.221s     |     49m56.698s    |     48m23.779s    |     168m52.065s    |     166m15.659s    |
#### 7. What skills are required to run xMarkerFinder?  
A preliminary understanding of shell scripts would allow users to complete the whole workflow. Intermediate experience in R and Python would facilitate users to interpret and modify the codes.
#### 8. Is xMarkerFinder a pipeline for meta-analysis?  
Yes. xMarkerFinder aims to integrate different datasets and establish replicable biomarkers. However, xMarkerFinder differs from systematic review as it integrates original datasets instead of the respective results.
### Part II Data processing
#### 1.	What kind of data should I use for xMarkerFinder?
Processed microbial count matrices and corresponding metadata are required. For cross-cohort analysis, we require merged datasets from at least three cohorts in the discovery set to accomplish the full protocol with internal validations. xMarkerFinder is well adapted to microbial taxonomic and functional profiles derived from both amplicon and whole metagenomics sequencing data, as well as other omics layers, including but not limited to metatranscriptomics, metaproteomics, and metabolomics.
#### 2. If I don’t have the corresponding metadata, can I still use xMarkerFinder?
To perform meta-analysis, corresponding sample groups are required. Other metadata indices, such as body mass index, age, and gender are recommended but unnecessary. However, it is worth noticing that the absence of metadata information might compromise the correction for confounding effects and the identification of microbial biomarkers.  
#### 3.	Why should I normalize my data?
To mitigate challenges induced by different numbers of sequencing (e.g. library sizes), microbial count profiles are converted to relative abundances for subsequent analysis in xMarkerFinder.
#### 4.	Why should I perform data filtering?
To identify a replicable panel of microbial biomarkers, we need to exclude rare microbial features, those with low occurrence rates across cohorts as they are not ideal candidates as global biomarkers.
#### 5.	What does the training and test set do and why should I separate them?
To ensure models’ reliability, datasets are split into training/discovery and test sets. The training set is used to train and have the model learn the hidden pattern. The test set is used to test the model after completing the training process and provides unbiased final model performance results.  
### Part III Using xMarkerFinder
#### 1.	How to solve installation errors?
Potential installation problems and solutions are provided in our manuscript, and most problems would be avoided by simply using the docker image we provided instead of running all scripts locally (https://hub.docker.com/r/tjcadd2022/xmarkerfinder).
#### 2.	What machine learning classifier should I choose?
Step 5 provides the evaluation of multiple commonly used algorithms in machine learning, and users could choose the most suitable algorithm based on these results. However, due to its robustness and interpretability, Random Forest classifiers are considered suitable for most microbiome datasets. Therefore, step 5 is not compulsory and we recommend users to build Random Forest models first, and move to other classifiers if they underperform.
#### 3.	How to choose suitable parameters when running xMarkerFinder?
For most scenarios, the default parameters would work. For further exploration, users are encouraged to try different parameters to get better results.
#### 4.	What is an AUC and how to interpret it?
AUC is the area under the ROC curve (the plot of the Sensitivity as y-axis versus 1-Specificity as x-axis). A perfect classifier gives an AUC of 1 while a simple classifier that makes completely random guesses gives an AUC of 0.5.

