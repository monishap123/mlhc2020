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
1. MIMIC III: We use the MIMIC dataset from the Li et al. paper. This is our train dataset
2. CASI: We use the CASI datasets from https://conservancy.umn.edu/handle/11299/137703. This is our test dataset
3. UMLS: We use the UMLS map to map longforms in the MIMIC III dataset to clinical concepts

<a name="method"></a>
## Methodology: ##
1. We use Clinical BERT to generate contextual word embeddings for an abbreviation in a clinical note.
2. For the associated longform, we map it to a UMLS CUI and use the corresponding concept embedding from CUItoVec
3. We aggregate the abbreviation-embedding(1) and concept embedding(2) to generate a feature vector, with label 1
4. For every positive observation, we generate a negative sample (label = 0) by randomly picking a CUI from a list of 10 closest CUIs
5. Finally, we train a binary classifier to predict a valid abbreviation longform pair using our train dataset
6. We evaluate our model on the test (CASI) dataset and investigate possible dataset shift. We accounted for this by  augmenting  our  training  dataset  with  data  from  CASI  (not  included  in  the  test dataset).
7. We investigate  the relationship between character-level information and model peroformance by introducing  a  new  test  dataset  with  normal  abbreviations  re-placed with:
   * random strings and 
   * acronym-based abbreviations.  
For random strings, we generate a random string of same length as the abbreviation and replace it in the text. For acronym-based abbreviations, We use the following rule:  if the phrase consists of onlyone  word,  we  take  the  first  two  letters  of  the  word  as  abbreviation.   If  the  phrase  con-sists of multiple words, we take the abbreviation given by the initial letter of each word.

<a name="run"></a>
## How to run the colab notebooks
1. The colab notebooks should be looked at / run in the following order:
  * MLHC_Project.ipynb
  * MLHC_Project_2.ipynb
  * MLHC_Project_3.ipynb
2. In the first cell of every colab notebook update the 'loc' variable with your google drive location containing all the data files.
3. In MLHC_Project.ipynb, we install and test ClinicalBERT, load the CASI and MIMIC datasets, visualize contexual abbreviation embeddings on our datasets and create part of our training dataset i.e. Steps 1 and 2 in methodology mentioned above. All the cells can be run sequentially to see results. There is code in the notebook that you can run to save intermediate data files, which we recommend, since they take a long time to generate (e.g. BERT embeddings )
4. In MLHC_Project_2.ipynb, we generate aggregated feature vectors, introduce negative samples into our dataset to make it balanced and visualize the abbreviation embedding space using t-SNE to investigate dataset shift between our train (MIMIC) and test(CASI) datasets. i.e. Step 3 - 6 in the methodology section mentioned above.
5. In MLHC_Project_3.ipynb, we investigate the relationship of character-level encodings to model performance.

## References: ##
* [ClinicalBERT] Emily Alsentzer, John Murphy, William Boag, Wei-Hung Weng, Di Jin, Tristan Naumann, and Matthew McDermott. 2019. Publicly available clinical BERT embeddings. In Proceedings of the 2nd Clinical Natural Language Processing Workshop, pages 72-78, Minneapolis, Minnesota, USA. Association for Computational Linguistics.
* [CUItoVec] Beam, Andrew L., et al. “Clinical Concept Embeddings Learned from Massive Sources of Multimodal Medical Data.” ArXiv:1804.01486 [Cs, Stat], Aug. 2019, http://arxiv.org/abs/1804.01486.
* [MIMIC III] Li, Irene, et al. "A Neural Topic-Attention Model for Medical Term Abbreviation Disambiguation." arXiv preprint arXiv:1910.14076 (2019).
* [CASI] Moon, Sungrim, et al. “A Sense Inventory for Clinical Abbreviations and Acronyms Created Using Clinical Notes and Medical Dictionary Resources.” Journal of the American Medical Informatics Association : JAMIA, vol. 21, no. 2, Mar. 2014, pp. 299–307, doi:10.1136/amiajnl-2012-001506.




