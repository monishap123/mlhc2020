# mlhc2020
Classifying valid abbreviation - longform pairs using contextual word embeddings

## Table of Contents
1. [ Description ](#desc)
2. [ Installation ](#install)
3. [ Datasets ](#data)
4. [ Methodology ](#method)
5. [ How to run the colab notebooks ](#run)

<a name="desc"></a>
## 1. Description
In this project, we present a model to determine valid abbreviation, longform pairs given a clinical corpus.

<a name="install"></a>
## 2. Installation
You will need the below two packages to run the code in this repository:
1. Clinical Bert: We use Clinical BERT to generate contextual word embeddings, the code to install Clinical BERT is in the colab file. 2. More info can be found here - https://github.com/EmilyAlsentzer/clinicalBERT
3. CUItoVec: Download the pretrained Clinical Concept embeddings from http://cui2vec.dbmi.hms.harvard.edu/ 

<a name="data"></a>
## 3. Datasets: ##
You will need the below datasets to run the code in this repository:
1. MIMIC III: We use the MIMIC dataset from the Li et al. paper
2. CASI: We use the CASI datasets from https://conservancy.umn.edu/handle/11299/137703
3. UMLS: We use the UMLS map to map longforms in the MIMIC III dataset to clinical concepts

<a name="method"></a>
## Methodology: ##
1. We use Clinical BERT to generate contextual word embeddings for an abbreviation in a clinical note.
2. For the associated longform, we map it to a UMLS CUI and use the corresponding concept embedding from CUItoVec
3. We aggregate the abbreviation-embedding(1) and concept embedding(2) to generate a feature vector, with label 1
4. For every positive observation, we generate a negative sample (label = 0) by randomly picking a CUI from a list of 10 closest CUIs
5. Finally, we train a binary classifier to predict a valid abbreviation longform pair

<a name="run"></a>
## How to run the colab notebooks
1. The colab notebooks should be looked at / run in the following order:
  * MLHC_Project.ipynb
  * MLHC_Project_2.ipynb
  * MLHC_Project_3.ipynb
2. In the first cell of every colab notebook update the 'loc' variable with your google drive location containing all the data files.
3. In MLHC_Project.ipynb, we install and test ClinicalBERT, load the CASI and MIMIC datasets, visualize contexual abbreviation embeddings on our datasets and create part of our training dataset i.e. Steps 1 and 2 in methodology mentioned above. All the cells can be run sequentially to see results. There is code in the notebook that you can run to save intermediate data files, which we recommend, since they take a long time to generate (e.g. BERT embeddings )
4. 

## References: ##
