# kaggle-ICR
This is a repository for [ICR](https://www.kaggle.com/competitions/icr-identify-age-related-conditions)(Identifying Age-Related Conditions) competition.

This is my first time to participate in a kaggle expert competition, which challenges me a lot. Below I introduce my final code and summarize the experience. Also, I write down the detailed actions that I have taken for this competition by day in the second part.

## Enviroment Setup
All packages used in this competition except tabpfn are common used. So if you don't have any of them, please run ```pip install xxx``` or ```conda install``` to install it. 

I use TabPFN model in this competition, you can download the package from [this Github link](https://github.com/kashif/TabPFN) and import it in your code.

## Experience and Results
### Day 1
Found some good notebooks in Discussion and coded a baseline for ICR. I used the same model with [this notebook](https://www.kaggle.com/code/aikhmelnytskyy/public-krni-pdi-with-two-additional-models), which trained a ensembled model including two XGBoost Classifiers and two TabPFN Classifiers.

Because of the limited reliability of Leaderboard(the public score are often referred to as Lb), we need to seperate a validation set from the original trianning set for calculating the balanced logarithmic loss and evaluate our model. In particular, I divided 20% of the trainning data to form the validation dataset, and used the remaining 80% data to train the model. Then, I calculated the balanced log loss of the prediction of validation dataset as "cross validation score"(often referred to as CV).

The corresponding notebook is [here](https://www.kaggle.com/code/stevenzzf0926/icr-xgboost-tabpfn?scriptVersionId=136694987). I might done something wrong, because the the Lb is 1.2, ranking 4685/5042.

### Day 2
To begin with, I fixed the bug and modified my code. I added an outer K-Fold object, which could help me seperate the dataset into 5 folds.

Today I dived deep into the balanced logarithmic loss function. By the help of [this discussion](https://www.kaggle.com/competitions/icr-identify-age-related-conditions/discussion/422442), I suprisingly found that my code implement of balanced log loss was totally wrong.

I tried to figure out what happened to my previous function but I didn't found any mistake. Maybe the precision couldn't meet the requirement. Also, I learned about how to use sklearn.metrics.logloss from this discussion.

Finally, the corresponding version of the notebook is [here](https://www.kaggle.com/code/stevenzzf0926/icr-xgboost-tabpfn?scriptVersionId=136694987). The Lb was 0.12 and CV was approximately 0.02, ranking 1376/5085.

### Day 3
Today I found an interesting [post](https://www.kaggle.com/competitions/icr-identify-age-related-conditions/discussion/413198). This post showed an interesting thing that the dataset can be integerized. Just like what this post said, a dataset without integer attributes was wired.

After integerized the trainning data, I found that BN may represented age, which was ranging from 29 to 83. So I copied train['BN'] three times and added to the end of trainning dataset.

In addition I tried different random seeds in K-Fold, and found that random_status=19 preformed best. Below was some result losses in different arguments.

The corresponding version of the notebook is [here]().

### Day 4&5
I was wondering about why my local CV is extremely low but LB was mutiple times higher than CV. The answer was, I used Random Over Sample before KF, where meant there were some data both in trainning dataset and validation dataset. To correct the code, I firstly fold the whole trainning dataset and then do the over sample.

Moreover, I made a mistake in previous days. I couldn't copy BN for times, because it may causes feature collinearity. Beyond this method, I tried data binning.

But a new question is that, I can't make a better LB score(< 0.09). I have tried some EDA methods/tricks but the LB goes up rather than become smaller.
