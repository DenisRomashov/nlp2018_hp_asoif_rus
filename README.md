
# Evaluation of Russian Language Dataset with Word Embeddings

We used two popular fantasy novel corpora, “Harry Potter” (HP) by JK Rowling and “A Song of Ice and Fire” (ASOIF) by GRR Martin.

## How to use

Run:
```
$ python evaluate.py
```
It will:

 1. create questions - generate 4 files in ```../datasets```  directory for models evaluating: 
	 - *questions_{hp/soiaf}_analogies_rus.txt* 
	 -  *questions_{hp/soiaf}_doesnt_match_rus.txt*
	
	*{hp/soiaf}_analogies_rus.txt* and *{hp/soiaf}_doesnt_match_rus.txt* files used as source term files.

 2. check frequencies - create 2 files in ```../datasets```  directory: 
	 - *frequencies_hp.txt*
	 - *frequencies_soiaf.txt* 
	  
	contain count of matched words from questions files created on previous step with corpora.

 3. create models - create models in  ```../models```  directory: 
	 - 5 Word2Vec models
	 - 2 FastText models with different parameters

	script ```../models/create_models.py``` contains all settings used for model training.
	
 4. evaluate analogies - create result files of analogies evaluation in  ```../evaluation_results```  directory: 
	 -	*{hp/asoif}_result_analogies.txt*

	Files cointain information about:
	- sections from ```questions``` files from *step 1*,
	-  *correct/incorrect/total* by section, 
	- *number of tasks* 
	-  *total of whole model* (the last number of last line for each model)

 5. doesnt_match_evaluation - create result files of word intrusion in  ```../evaluation_results```  directory: 
	 -	*{hp/asoif}_result_doesnt_match.txt*

	Files cointains:
	- detailed information of difficulty and accuracy,
	- *number of tasks* 
	-  *total result of whole model* (the last number of last line for each model)

You can easily change settings of evaluation process in ```src/analogies_evaluation.py``` and ```src/doesnt_match_evaluation.py``` files.
Also you can skip some steps like *1. create questions* and  *2. check frequencies* because there is no need to rerun these scripts if you haven't changed term files.

## Importance of lemmatization
Analogies:

![Check frequencies comparison](https://github.com/DenisRomashov/nlp2018_hp_asoif_rus/blob/master/md_sources/check_frequencies_comparison.png)

Word intrusion:

![Check frequencies comparison](https://github.com/DenisRomashov/nlp2018_hp_asoif_rus/blob/master/md_sources/check_frequencies_comparison_doesnt_match.png)

As you can see, lemmatization of corpus helps to increase term frequencies. The main problem of training russian dataset is a range of different forms of word which mean the same but written in a different way because of a grammar rules. Example:
Russian:
> *Озеро* - за *Озеро***м**

English:

> *Lake* - behind the *Lake*

Lemmatization process trained on russian grammar rules changes all forms to the base form and helps to achieve better results.
