# TextDifficultyAssessmentGerman

# Requirements
python 3.6  
pandas==0.22.0  
requests==2.18.4  
spacy==2.0.12  
de_core_news_sm (German model for spacy)  
hunspell==0.5.4  
urllib3==1.22  
stanfordcorenlp==3.9.1.1  
tqdm==4.23.4  
nltk==3.2.5  
numpy==1.14.0  
matplotlib==2.1.2  
scikit-learn==0.19.1  

# Preprocessing
This part of the project was only tested on Ubuntu 18.04.

The preprocessing steps can be found in the "01_Preprocessing" notebook. It includes spellchecking with the hunspell library, POS tagging using RFTagger and parsing with the Stanford Parser. Additionally some parts of the text are cut off, e.g. addresses or dates before the actual text (in case of letters). The setupRFTagger and setupParser functions can be used to download and setup those components.

Running the 01_Preprocessing notebook uses the datasets/00_Texts_df.csv file with our compiled dataset as input, and creates two new dataframes:
* 01_Preprocessing_df.csv (treating each text as a unit to preprocess, placed in folder datasets/01_RawDatasets)
* 01a_Sentence_df.csv (treating each sentence as a unit to preprocess, placed in folder datasets)

Both include the following columns:
Level|Title|Text|Source|cleanedSource|Type|newLevel|cleanedText|RFTagger|parsedText|preprocessedText

# Feature Engineering
The theory-driven features are calculated in the following notebooks:
* 02_BasicFeatures (called Traditional Features in the paper)
* 03_LexicalDiversity 
* 04_LexicalDensityVariation
* 05_WordFrequency (using the list of frequent words in the frequencyList folder)
* 06_MorphologicalFeatures (morphological features and features based on RFTagger, the compound splitter is used here)
* 07_SyntaxDependecyFeatures 
* 08_SyntaxParseTreeComplexity 

Each notebook contains the necessary functions for calculating the feature values for each text in its category. The specific features for each group can be found in the appendix of the paper.

Running each notebook in order creates a folder in the datasets folder with the same name as the notebook. The folder will contain the dataframe updated with the features from the notebook, e.g. the 03_LexicalDiversity notebook takes the dataframe created by the 02_BasicFeatures notebook (containing the preprocessed texts and the basic feature values) and adds the lexical diversity feature values to the dataframe that will be placed in the 03_LexicalDiversity folder inside the datasets folder.

# Experiments
* 09_Visualisations  
Running this notebook will produce various visualizations of the data and the features that we used for the project and the paper.    
---
**Data-driven features**  
* 10_Ngrams (Words, character, POS and lemma n-grams)
* 11_SyntaxParseRules  
These two notebooks are self-contained and will calculate the features according to the name of the notebook, do the classication and evaluation and provide some visualizations and experiments that are discussed in the paper.  
---
**Random/ Reshuffled Text**  
* 12_CreateRandomText  
This notebooks works with the 01a_Sentence_df.csv dataframe and creates texts from random shuffled sentences of given lengths (1-15 sentences) and places the test and train dataset in datasets/01_RawDataset. E.g. test_10_df.csv is the test dataset with texts of 10 sentences. 
* 13_AnalyseRandomText  
This notebooks uses the datasets from datasets/08_SyntaxComplexity, which are datasets containing a given number of sentences (1-15) as described above, and already have the all the feature values calculated. (This was done by running notebooks 02-08 and changing the input and output file names. All of the partial results are not uploaded on github but the final versions contain all the features.) The notebook contains the analysis and visualizations of this experiment, which are discussed in the paper.
---
* 14_ReproduceHancke  
This notebook reproduces Hancke (2013) and compares our results to hers. We use our features that correspond to her best features, and the features that turned out to be the best-performing in our project on the MERLIN dataset (which is the one she worked with).
