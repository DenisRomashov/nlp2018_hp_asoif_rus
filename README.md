
# Evaluation of Russian Language Datasets in the Digital Humanities Domain with Word Embeddings

We used two popular fantasy novel corpora, “Harry Potter” (HP) by JK Rowling and “A Song of Ice and Fire” (ASOIF) by GRR Martin.

These dataset are based on English language datasets, see also: [English Version of the Datasets](https://github.com/gwohlgen/digitalhumanities_dataset_and_eval).

Here, you find basic information about the *dataset*, *Usage* of the reposity, and *lemmatization*.
The overview of evaluation results for the *analogy* and *word intrustion* tasks can be found [here in RESULTS.MD](RESULTS.md).

## Datasets

The Russian language datasets can be found in the [datasets directory](datasets).

Like in the original word2vec-toolkit, the files to be evaluated are named `questions`\*.
There are four datasets:
* `datasets/questions_soiaf_analogies_rus.txt`: Analogies relation test data for *A Song of Ice and Fire*
* `datasets/questions_soiaf_doesn_match_rus.txt`: Doesnt_match task test data for *A Song of Ice and Fire*
* `datasets/questions_hp_analogies_rus.txt`: Analogies relation test data for *Harry Potter*
* `datasets/questions_hp_doesn_match_rus.txt`: Doesnt_match task test data for *Harry Potter*
<!-- * **NEW:**: There are now 4 more datasets, same as the 4 original ones, but for n-gram data. 
    The **n-gram** datasets are easily recognizable, they have `_ngram` in the file name. -->

If you want to extend or modify the test data, edit the respective source files in the folder [datasets](datasets):
`hp_analogies.txt`, `hp_does_not_match.txt`, `soiaf_analogies.txt`,`soiaf_does_not_match.txt`.

After modifying the test data run the following command to re-create the datasets (the `question_` files).
```
    cd datasets
    python create_questions.py
```

This will generate section-based permutations to create the evaluation datasets.
You can also add completly new datasets and add a line into `create_questions.py`.



## Usage 

Simply run:
```
$ python evaluate.py
```
It will:

 1. create questions - generate 4 files in ```../datasets```  directory for evaluating models:
	 - *questions_{hp/soiaf}_analogies_rus.txt* 
	 -  *questions_{hp/soiaf}_doesnt_match_rus.txt*
	
	*{hp/soiaf}_analogies_rus.txt* and *{hp/soiaf}_doesnt_match_rus.txt* files are used as input files to create the questions (task units).

 2. check frequencies - create 2 files in ```../datasets```  directory: 
	 - *frequencies_hp.txt*
	 - *frequencies_soiaf.txt* 
	  
	this outputs the corpus counts (frequencies) of words (the vocabulary) in the questions files (created in the previous step).

 3. create models - create models in  ```../models```  directory: 
	 - 5 Word2Vec models
	 - 2 FastText models with different parameters

	script ```../models/create_models.py``` contains all settings used for model training.
	
 4. evaluate analogies - create result files of analogies evaluation in  ```../evaluation_results```  directory: 
	 -	*{hp/asoif}_result_analogies.txt*

	Files contain information about:
	- sections from ```questions``` files from *step 1*,
	-  *correct/incorrect/total* by section, 
	- *number of tasks* 
	-  *total of whole model* (the last number of the last line for each model)

 5. doesnt_match_evaluation - create result files for word intrusion in  ```../evaluation_results```  directory: 
	 -	*{hp/asoif}_result_doesnt_match.txt*

	Files cointains:
	- detailed information on difficulty and accuracy,
	- *number of tasks* 
	-  *total result of whole model* (the last number in the last line for each model)

You can easily change the settings of the evaluation process in the ```src/analogies_evaluation.py``` and ```src/doesnt_match_evaluation.py``` files.
Also you can skip some steps like *1. create questions* and  *2. check frequencies* because there is no need to rerun these scripts if you haven't changed the dataset files.

## Importance of lemmatization

In our experiments we found, that lemmatization has a strong positive impact on evaluation results. Lemmatization raises the average frequency (in the corpus) of the terms 
in the dataset (vocabulary):

Analogies:

![Check frequencies comparison](https://github.com/DenisRomashov/nlp2018_hp_asoif_rus/blob/master/md_sources/check_frequencies_comparison.png)

Word intrusion:

![Check frequencies comparison](https://github.com/DenisRomashov/nlp2018_hp_asoif_rus/blob/master/md_sources/check_frequencies_comparison_doesnt_match_.png)

As can be seen, corpus lemmatization helps to increase term frequencies. The low counts on the raw text of caused by the rich morphology of Russian language, where words appear in many different forms because of a Russian grammar. Example:
Russian:
> *Озеро* - за *Озеро***м**

English:

> *Lake* - behind the *Lake*

Lemmatization changes all forms to their base form and in general helps to achieve better results in our setting (of small corpora).

## Evalution Results 

Extensive evaluation results for the *analogy* and *word intrustion* tasks can be found [here in RESULTS.MD](RESULTS.md).

