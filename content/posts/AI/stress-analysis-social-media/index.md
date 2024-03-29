---
title: "Stress analysis on Social media"
date: "2021-09-04"
menu:
  sidebar:
    name: Stress analysis on Social media
    identifier: stress-analysis-social-media
    parent: ai
    weight: 10
---

# Resources
- Code: https://github.com/chiranthans23/stress-analysis-social-media
- Train and test data: dreaddit-train.csv, dreaddit-test.csv
- EDA report: report.html

## Introduction
This is a work on [Dreaddit dataset](https://arxiv.org/abs/1911.00133?utm_source=feedburner&utm_medium=feed&utm_campaign=Feed%253A+arxiv%252FQSXk+%2528ExcitingAds%2521+cs+updates+on+arXiv.org%2529) taken from one of the latest datasets on [Kaggle](https://www.kaggle.com/ruchi798/stress-analysis-in-social-media) which is created using data from 5 different Reddit communities. This is a slightly different problem as the length of the posts is larger than the usual tweets. Additionally, the data available for training is also not quite huge.

## First look
This work doesn't have an extensive EDA but just looks at the dataset - columns, amount of data available, missing values. This is done using Pandas profiling. Pandas profiling does very good Auto EDA and gives a myriad of information. Minimal data can also be seen by using the `minimal` param.
> Code: pre-processor.ipynb

## Pre-processing
Firstly, few unnecessary columns such as the post_id, sentence_range are removed. Second, the subreddit category - categorical data - is *OneHotEncoded*. Helper functions are run on the text of posts to clean them. *BERT base uncased* tokenizer from 🤗 is used to tokenize the text data. Finally, the pre-processed data is written to file.
> Code: pre-processor.ipynb

## Model evaluation
Multiple models were used for evaluation - XGBoost, SVM, Random Forest, LightGBM, GBM, etc. `cross_value_score` and 10-fold was used for evaluating the models. This roughly gave an idea of the weak and strong models. Deep learning models were not used mainly because of the small amount of data that's available. For this section, I took help from [this](https://www.kaggle.com/sohommajumder21/bert-tokenizer-with-9-models-nlp-stress-analysis) notebook. 
> Code: model.ipynb

## Hyperparameter optimization
Models XGBoost, LGBM, and GBM which gave almost the same results were optimized. A beautiful framework [Optuna](https://optuna.org/) was used for this. I have used 100 trials for optimization and the best parameters of each model are captured in different files. This is a time-consuming process, so using GPU will help in speeding up the process.
> Code: xgboost_optimization.ipynb, lgbm_optimization.ipynb, gbm_optimization.ipynb
 
## Final model
The best parameters from the optimization step still resulted in an almost same overall score of all the above models. Hence, a stable model was built by ensembling the best XGBoost, LGBM, GBM models using a Voting classifier. This resulted in an F1 score of around 0.77.
> Code: main.ipynb




