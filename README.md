# [PYTORCH] Very deep convolutional networks for Text Classification

## Introduction

Here is my pytorch implementation of the model described in the paper **Very deep convolutional networks for Text Classification** [paper](https://arxiv.org/abs/1606.01781). 

## Datasets:

Statistics of datasets I used for experiments. These datasets could be download from [link](https://drive.google.com/drive/u/0/folders/0Bz8a_Dbh9Qhbfll6bVpmNUtUcFdjYmF2SEpmZUZUcVNiMUw1TWN6RDV3a0JHT3kxLVhVR2M)

| Dataset                | Classes | Train samples | Test samples |
|------------------------|:---------:|:---------------:|:--------------:|
| AG’s News              |    4    |    120 000    |     7 600    |
| Sogou News             |    5    |    450 000    |    60 000    |
| DBPedia                |    14   |    560 000    |    70 000    |
| Yelp Review Polarity   |    2    |    560 000    |    38 000    |
| Yelp Review Full       |    5    |    650 000    |    50 000    |
| Yahoo! Answers         |    10   |   1 400 000   |    60 000    |
| Amazon Review Full     |    5    |   3 000 000   |    650 000   |
| Amazon Review Polarity |    2    |   3 600 000   |    400 000   |

## Setting:

I almost keep default setting as described in the paper. For optimizer, I use **Adam** optimizer with initial learning rate of 0.001 instead of SGD with learning rate of 0.01.

Additionally, in the original model, one epoch is seen as a loop over batch_size x num_batch records (128x5000 or 128x10000 or 128x30000), so it means that there are records used more than once for 1 epoch. In my model, 1 epoch is a complete loop over the whole dataset, where each record is used exactly once.

## Training

If you want to train a model with common dataset and default parameters, you could run:
- **python train.py -d dataset_name**: For example, python train.py -d dbpedia

If you want to train a model with common dataset and your preference parameters, like the depth of network, you could run:
- **python train.py -d dataset_name -t depth**: For example, python train.py -d dbpedia -t 9

If you want to train a model with your own dataset, you need to specify the path to input and output folders:
- **python train.py -i path/to/input/folder -o path/to/output/folder**

## Test

You could find some trained models I have trained in [link](https://drive.google.com/open?id=1gx1qvgu8rZRtEgkCMA9KqJZtFwjr8fc-)

## Experiments:

I run experiments in 2 machines, one with NVIDIA TITAN X 12gb GPU and the other with NVIDIA quadro 6000 24gb GPU. For small and large models, you need about 1.6 gb GPU and 3.5 gb GPU respectively.

Results for test set are presented as follows:  A(B)/C:
- **A** is accuracy reproduced here.
- **B** is accuracy reported in the paper.
- **C** is the epoch when maximum accuracy observed

It should be noted that in experiments with depth is 49 layers, there is no accuracy reported in the paper. Therefore here I only show the results obtained from my experiments.

|     Depth     |        9        |        17       |        29       |        49       |
|:---------------:|:-----------------:|:-----------------:|:-----------------:|:-----------------:|
|    ag_news    | 87.67(90.17)/15 | 88.09(90.61)/20 | 88.01(91.33)/14 |    84.71/17     |
|   sogu_news   | 95.67(96.42)/16 | 95.89(96.49)/20 | 95.73(96.82)/19 |    95.35/20     |
|    db_pedia   | 98.33(98.44)/17 | 98.28(98.39)/18 | 98.07(98.59)/17 |    97.38/15     |
| yelp_polarity | 94.57(94.73)/12 | 95.20(94.95)/18 | 94.95(95.37)/20 |    95.08/19     |
|  yelp_review  | 62.44(61.96)/11 | 63.44(62.59)/19 | 62.70(63.00)/19 |    62.83/19     |
|  yahoo_answer | 69.57(71.76)/12 | 70.03(71.75)/15 | 70.34(72.84)/19 |    69.16/19     |
| amazon_review | 60.34(60.81)/12 | 60.98(61.19)/20 | 60.67(61.61)/20 |    59.80/17     |
|amazon_polarity| 94.30(94.31)/15 | 94.60(94.57)/17 | 94.53(95.06)/20 |    94.10/11     |

Below are the training/test loss/accuracy curves for each dataset's experiments (figures for 9, 17, 29-layer model are from left to right) :

- **ag_news**

<img src="visualization/vdcnn_9_dbpedia.png" width="280"> <img src="visualization/vdcnn_17_dbpedia.png" width="280"> <img src="visualization/vdcnn_29_dbpedia.png" width="280"> 

- **db_pedia**

<img src="visualization/char-cnn_small_dbpedia.png" width="420"> <img src="visualization/char-cnn_large_dbpedia.png" width="420">

- **yelp_polarity**

<img src="visualization/char-cnn_small_yelp_review_polarity.png" width="420"> <img src="visualization/char-cnn_large_yelp_review_polarity.png" width="420">

- **yelp_review**

<img src="visualization/char-cnn_small_yelp_review.png" width="420"> <img src="visualization/char-cnn_large_yelp_review.png" width="420">

- **yahoo! answers**

<img src="visualization/char-cnn_small_yahoo_answers.png" width="420"> <img src="visualization/char-cnn_large_yahoo_answers.png" width="420">

- **amazon_review**

<img src="visualization/char-cnn_small_amazon_review.png" width="420"> <img src="visualization/char-cnn_large_amazon_review.png" width="420">

- **amazon_polarity**

<img src="visualization/char-cnn_small_amazon_polarity.png" width="420"> <img src="visualization/char-cnn_large_amazon_polarity.png" width="420">

You could find detail log of each experiment containing loss, accuracy and confusion matrix at the end of each epoch in **output/datasetname_depth_number/logs.txt**, for example output/ag_news_depth_29/logs.txt
