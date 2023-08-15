# kaggle-ICR
This is a repository for [ICR](https://www.kaggle.com/competitions/icr-identify-age-related-conditions)(Identifying Age-Related Conditions) competition.

This is my first time to participate in a kaggle expert competition, which challenges me a lot. Below I introduce my final code and summarize the experience, which may of some help for you.

## Enviroment Setup
All packages used in this competition except TabPFN are common used. So if you don't have any of them, please run ```pip install xxx``` or ```conda install```(in virtual envs) to install it. 

I used TabPFN model in this competition, you can download the package from [this Github link](https://github.com/kashif/TabPFN) and import it in your code.

## Code
You can see my code with best private leaderboard score in this repository. The <font color='red'>main point</font> of the code can be divide into those parts.

## Experience
### Baseline from Discussion
Found some good notebooks in Discussion and coded a baseline for ICR. I used the same model with [this notebook](https://www.kaggle.com/code/aikhmelnytskyy/public-krni-pdi-with-two-additional-models), which trained a ensembled model including two XGBoost Classifiers and two TabPFN Classifiers.

### The Overview of the Dataset

[t-SNE](https://towardsdatascience.com/t-sne-python-example-1ded9953f26) is a great thing for me to learn it in this compition, which helps me to learn the properties of data in a low dimension space.

### The Importance of CV(Cross Validation)
Because of the limited reliability of Leaderboard(the public score are often referred to as Lb), we need to seperate a validation set from the original trianning set for calculating the balanced logarithmic loss and evaluate our model. In particular, I tried **StraightKF** and ordinary KFold to seperate 20% of the data as validation dataset in every iteration.

### The Real Evaluation Formula
Due to the fact that the public code had led a wrong derection of the Balanced Log loss, [this discussion](https://www.kaggle.com/competitions/icr-identify-age-related-conditions/discussion/422442) had clarified **the right one**.

The right Formula is as follows.
$$\text { Log Loss }=\frac{-\frac{1}{N_0} \sum y_{0 i} \log p_{0 i}-\frac{1}{N_1} \sum y_{1 i} \log p_{1 i}}{2}$$

### Feature Engineering
#### Integerized Data
An interesting [post](https://www.kaggle.com/competitions/icr-identify-age-related-conditions/discussion/413198) showed an interesting thing that the dataset can be integerized. Just like what this post said, a dataset without integer attributes was wired.

After integerized the trainning data, we found that BN may represented age, which was ranging from 29 to 83. So I tried to make a bin of BN feature which may strengthen the effect of this feature.

#### Feature Derivation
Inorder to improve our model, I tried to add some new features in training data. Firstly, I tried some derived features such as the nan number, but it turned out to be no use.

Then, with the help of K-Means++, I got several different K-Means centers with data whose label is 0 and 1, respectively. I added the maximum and minimum distance of the each data among those centers to the training data, which was somewhat useful.

But the most useful one is creating polynomial features. I will introduce it later.

#### Grabbing the Most "Important" Features
There are several different anonymous features in this competition, but how we can identify the most "important" features? Here I tried to use [SHAP](https://shap.readthedocs.io/en/latest/index.html) to calculate the shapley value of each features, and I found DU, BQ, AB, FL are apparently important than other features. So I used these features to create second-order or even third-order features.

### Ensemble Models
This is the key point of the competition. I generated TabPFN and XgboostClassifier at the very beginning, then added lgbm and catboost classifiers to the model. I still try to explore different models till now.(Update on 8.4)

Parameters' tuning is strongly needed in training models.

### Post Processing
The code with highest public LB score uses two actions to adjust the predicted probabilities. The first one is balancing the inequality between the sum of probabilities between A and B+D+G. My understanding of this is that this action may do harm for the bll of Class 0's data, but it may highly reduce the bll of Class 1's (that is B+D+G) data.

The second one is thresholding, writing the code like ```p0[p0 > 0.65] = 1``` to help us get a lower LB score, but it is risky.

For the first action, that is, probabilities calibration, we have [another method](https://www.kaggle.com/competitions/icr-identify-age-related-conditions/discussion/426748) providing a better public LB score.

## After the Comp
It is frustrated for me to defeat in this competition. My best submission is my last one but I didn't choose it, which made me lose the silver medal.

What impressed me most in this competition is that, it may be the most severe shake up of all time. And to my surprise, even a single catboost model without any feature engineering could get a silver medal. Thus, I give up to study more about this tricky competition. In a word, it's a boring game.
