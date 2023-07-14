# kaggle-ICR
This is a repository for [ICR](https://www.kaggle.com/competitions/icr-identify-age-related-conditions)(Identifying Age-Related Conditions) competition.

This is my first time to participate in a kaggle expert competition, which challenges me a lot. Below I introduce my final code and summarize the experience. Also, I write down the detailed actions that I have taken for this competition by day in the second part.


## Experience and Results
### Day 1
Found some good notebooks in Discussion and coded a baseline for ICR. I used the same model with [this notebook](https://www.kaggle.com/code/aikhmelnytskyy/public-krni-pdi-with-two-additional-models), which trained a ensembled model including two XGBoost Classifiers and two TabPFN Classifiers.

Because of the limited reliability of Leaderboard(the public score are often referred to as Lb), we need to seperate a validation set from the original trianning set for calculating the balanced logarithmic loss and evaluate our model. In particular, I divided 20% of the trainning data to form the validation dataset, and used the remaining 80% data to train the model. Then, I calculated the balanced log loss of the prediction of validation dataset as "cross validation score"(often referred to as CV).

The corresponding notebook is [here](https://www.kaggle.com/code/stevenzzf0926/icr-xgboost-tabpfn?scriptVersionId=136694987).

### Day 2
Today dived deep into the balanced logarithmic 

seed42:
80% Train Loss: 0.02813351148382359
20% Val Loss: 0.023899050907612334

seed19:
80% Train Loss: 0.00832058118096344
20% Val Loss: 0.011302249154983545

integer + repeat BN + seed19
80% Train Loss: 0.00990054752769389
20% Val Loss: 0.0019808524959592897
