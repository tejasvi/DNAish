# Genomic Language Model

This is an implementation of [ULMFiT](https://arxiv.org/abs/1801.06146). The model architecture used is based on the [AWD-LSTM](https://arxiv.org/abs/1708.02182).

The approach uses three training phases to produce a classification model:
  1. Train a language model on a large, unlabeled corpus
  2. Fine tune the language model on the classification corpus
  3. Use the fine tuned language model to initialize a classification model
  
This is useful for genetic analysis, where large amounts of unlabeled data is abundant and labeled data is scarce. The approach allows to train a model on a large, unlabeled genomic corpus in an unsupervised fashion. The pre-trained language model serves as a feature extractor for parsing genomic data.

# Results

## Promoter Classification

#### E. coli promoters
The method performs well at the task of classifying promoter sequences from random sections of the genome. The process of unsupervised pre-training and fine-tuning has a clear impact on the performance of the classification model

  | Model                        	| Accuracy 	| Precision 	| Recall 	| Correlation Coefficient 	|
  |------------------------------	|:--------:	|:---------:	|:------:	|:-----------------------:	|
  | Naive                        	|   0.834  	|   0.847   	|  0.816 	|          0.670          	|
  | E. coli Genome Pre-Training   	|   0.919  	|   0.941   	|  0.893 	|          0.839          	|
  | Genomic Ensemble Pre-Training 	|   0.973  	|   0.980   	|  0.966 	|          0.947          	|
 
[Dataset](https://github.com/tejasvi/DNAish/blob/master/Bacteria/E.%20Coli/E.%20coli%200%20Data%20Processing.ipynb)

[Notebook Directory](https://github.com/tejasvi/DNAish/tree/master/Bacteria/E.%20Coli)
  
  
Classification performance on human promoters is competitive with published results

#### Human Promoters (short)
For the short promoter sequences, using data from [Recognition of Prokaryotic and Eukaryotic Promoters using Convolutional Deep Learning Neural Networks](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0171410):

| Model                            	| DNA Size 	| kmer/stride 	| Accuracy 	| Precision 	| Recall 	| Correlation Coefficient 	| Specificity 	|
|----------------------------------	|----------	|-------------	|----------	|-----------	|--------	|-------------------------	|-------------	|
| Kh et al.                        	| -200/50  	|      -      	|     -    	|     -     	|   0.9  	|           0.89          	|     0.98    	|
| Naive Model                      	| -200/50  	|     5/2     	|   0.80   	|    0.74   	|  0.80  	|           0.59          	|     0.80    	|
| With Pre-Training                 	| -200/50  	|     5/2     	|   0.922  	|   0.963   	|  0.849 	|          0.844          	|    0.976    	|
| With Pre-Training and Fine Tuning 	| -200/50  	|     5/2     	|   .977   	|    .959   	|  .989  	|           .955          	|     .969    	|
| With Pre-Training and Fine Tuning 	| -200/50  	|     5/1     	|   .990   	|    .983   	|  .995  	|           .981          	|     .987    	|
| With Pre-Training and Fine Tuning 	| -200/50  	|     3/1     	|   __.995__   	|    __.992__   	|  __.996__  	|           __.991__          	|     __.994__    	|

[Data Source](https://github.com/solovictor/CNNPromoterData)

[Notebook Directory](https://github.com/tejasvi/DNAish/tree/master/Mammals/Human/Promoter%20Classification%20Short%20Sequences)

#### Human Promoters (long)
For the long promoter sequences, using data from [PromID: Human Promoter Prediction by Deep Learning](https://arxiv.org/pdf/1810.01414.pdf):

| Model                            	| DNA Size  	| Models           	| Accuracy 	| Precision 	| Recall 	| Correlation Coefficient 	|
|----------------------------------	|-----------	|------------------	|----------	|-----------	|--------	|-------------------------	|
| Umarov et al.                    	| -1000/500 	| 2 Model Ensemble 	|     -    	|   0.636   	|  0.802 	|          0.714          	|
| Umarov et al.                    	|  -200/400 	| 2 Model Ensemble 	|     -    	|   0.769   	|  0.755 	|          0.762          	|
| Naive Model                      	|  -500/500 	|   Single Model   	|   0.858  	|   0.877   	|  0.772 	|          0.708          	|
| With Pre-Training                 	|  -500/500 	|   Single Model   	|   0.888  	|    __0.90__   	|  0.824 	|          0.770          	|
| With Pre-Training and Fine Tuning 	|  -500/500 	|   Single Model   	|   __0.892__  	|   0.877   	|  __0.865__ 	|          __0.778__          	|


[Notebook Directory](https://github.com/tejasvi/DNAish/tree/master/Mammals/Human/Promoter%20Classification%20Long%20Sequences)

#### Other Bacterial Promoters
This table shows results on data from [Recognition of prokaryotic and eukaryotic promoters using convolutional deep learning neural networks](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0171410).

| Method         	| Organism    	| Training Examples 	| Accuracy 	| Precision 	| Recall 	| Correlation Coefficient 	| Specificity 	|
|----------------	|-------------	|-------------------	|----------	|-----------	|--------	|-------------------------	|-------------	|
| Kh et al.     	| E. coli     	|        2936       	|     -    	|     -     	|  __0.90__  	|           0.84          	|     0.96    	|
| ULMFiT        	| E. coli     	|        2936       	|   0.956  	|   0.917   	|  0.880 	|          __0.871__          	|    __0.977__    	|
| Kh et al.     	| B. subtilis 	|        1050       	|     -    	|     -     	|  __0.91__  	|           __0.86__          	|     0.95    	|
| ULMFiT         	| B. subtilis 	|        1050       	|   0.905  	|   0.857   	|  0.789 	|          0.759          	|     0.95    	|


[Notebook Directory](https://github.com/tejasvi/DNAish/tree/master/Bacteria/Bacterial%20Ensemble/Promoter%20Classification)


## Metaganomics Classification

ULMFiT shows improved performance on the metagenomics taxonomic dataset from [Deep learning models for bacteria taxonomic classification of metagenomic data](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6069770/).

| Method          	| Data Source 	| Accuracy 	| Precision 	| Recall 	| F1    	|
|-----------------	|-------------	|----------	|-----------	|--------	|-------	|
| Fiannaca et al. 	| Amplicon    	| .9137    	| .9162     	| .9137  	| .9126 	|
| ULMFiT          	| Amplicon    	| __.9239__    	| __.9402__     	| __.9332__  	| __.9306__ 	|
| Fiannaca et al. 	| Shotgun     	| .8550    	| .8570     	| .8520  	| .8511 	|
| ULMFiT          	| Shotgun     	| __.8797__    	| __.8824__     	| __.8769__  	| __.8758__ 	|


[Notebook Directory](https://github.com/tejasvi/DNAish/tree/master/Bacteria/Bacterial%20Ensemble/Metagenomics%20Classification)


## Enhancer Classification
Trained on a dataset of mammalian enhancer sequences from [Enhancer Identification using Transfer and Adversarial Deep Learning of DNA Sequences](https://www.biorxiv.org/content/biorxiv/early/2018/02/14/264200.full.pdf), ULMFiT outperforms Cohn et al.

| Model - AUROC                 	| Human 	| Mouse 	|  Dog  	| Opossum 	|
|-------------------------------	|:-----:	|:-----:	|:-----:	|:-------:	|
| Cohn et al.                   	|  0.80 	|  0.78 	|  0.77 	|   0.72  	|
| ULMFiT 5-mer Stride 2          	| 0.812 	| 0.871 	| 0.773 	|  0.787  	|
| ULMFiT 4-mer Stride 2           	| 0.804 	| __0.876__ 	| 0.771 	|  0.786  	|
| ULMFiT 3-mer Stride 1         	| __0.819__ 	| 0.875 	| __0.788__ 	|  __0.798__  	|

[Data Source](https://github.com/cohnDikla/enhancer_CNN)

[Notebook Directory](https://github.com/tejasvi/DNAish/tree/master/Mammals/Mammal%20Ensemble/Enhancer%20Classification)


## mRNA/lncRNA Classification
This table shows results for training a classification model on a dataset of coding mRNA sequences and long noncoding RNA (lncRNA) sequences. The dataset used is [A deep recurrent neural network discovers complex biological rules to decipher RNA protein-coding potential](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6144860/) by Hill et al.

| Model                          	| Test Set           	| Accuracy 	| Specificity 	| Sensitivity 	| Precision 	| MCC   	|
|--------------------------------	|--------------------	|----------	|-------------	|-------------	|-----------	|-------	|
| GRU Ensemble (Hill et al.)    	| Standard Test Set  	|   0.96   	|     __0.97__    	|     0.95    	|    __0.97__   	|  0.92 	|. The dataset contains two test sets - a standard test set and a challenge test set.
| ULMFiT (3mer stride 1)        	| Standard Test Set  	|   __0.963__  	|    0.952    	|    __0.974__    	|   0.953   	| __0.926__ 	|
| GRU Ensemble (Hill et al.)    	| Challenge Test Set 	|   0.875  	|     __0.95__    	|     0.80    	|    __0.95__   	|  0.75 	|
| ULMFiT (3mer stride 1)        	| Challenge Test Set 	|   __0.90__   	|    0.944    	|    __0.871__    	|   0.939   	| __0.817__ 	|


[Notebook Directory](https://github.com/tejasvi/DNAish/tree/master/Mammals/Human/lncRNA%20Classification)


## Interpreting Results
In the plot below, the red line corresponds to a true transcription start site. The plot shows how prediction results are sensitive to changes around that location. [Model Interpretations](https://github.com/tejasvi/DNAish/tree/master/Model%20Interpretations) directory.

![](https://github.com/tejasvi/DNAish/blob/master/Model%20Interpretations/media/coli_interp.png)


## Long Sequence Inference
The image below shows a sample prediction of promoter locations on a 40,000 bp region of the E. coli genome. True promoter locations are shown in red.[notebook](https://github.com/tejasvi/DNAish/blob/master/Model%20Interpretations/Long%20Sequence%20Prediction%20E.%20coli.ipynb)

![](https://github.com/tejasvi/DNAish/blob/master/Model%20Interpretations/media/prediction_plot.png)
  
