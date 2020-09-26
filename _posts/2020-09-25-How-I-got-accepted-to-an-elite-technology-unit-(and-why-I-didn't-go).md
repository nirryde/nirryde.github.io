---
layout: post
title: "How I Got Accepted To An Elite Intel Unit (and why I didn't go)"
date: 2020-09-25
---
A few years ago, I was a budget analyst at the IDF’s medical corps. In retrospective my job was quite interesting, especially when compared to other budget related positions in the IDF. Yet, back then I was not satisfied of my position and I was looking into trying to move into a more analytic position.\
Through some espionage I found that some elite intelligence unit was looking for a data scientist. I thought I was qualified enough to apply and I somehow managed to get in touch with the head of the team. I called the head of the team and honestly, I cannot remember much of that conversation, but we set up to meet for a first interview. The first interview was not technical, and the head of the team just asked me to present myself and my background. I was at the beginning of a masters in statistics and I have self-taught myself some machine learning yet still, I was a bit worried that my background in economics would not be good enough for a team where everybody were graduates of prestigious CS/math programs. Fortunately, the interviewer thought I was worth a shot and he said I should be ready to present the team a project which should demonstrate my abilities.\
I had two projects which I presented to the team. The first project was a health-economics project I initiated at my unit (maybe I will be able to write about that project in a separate post). The second project was something I did on one of Kaggle’s playbox datasets and the full summary is given below. Needless to say, I was excited yet confident at the interview. The presentation lasted a solid 3 hours and during the presentation I was tackled with some questions which I was able to respond. I was done by lunchtime and after the presentation we went together for lunch at the mess hall. I expected that the food at such an elite intel unit would be better than average however, to my disappointment to food was merely average. After lunch we headed back to the office for coffee and we talked a bit until I had to go. When I left, I was told by the team leader that I had done a good job and he would let me know if I got accepted. I thanked him for the opportunity as I genuinely felt honored that the whole team halted their work and spend that much time to listen to me.\
A week or two after the interview I got call from the team leader saying that they decided to accept me to the team. Obviously, I was very happy, yet I knew this was only the first step at changing my profession. As the saying goes, it takes two to tango. The army bureaucracy dictates that if one wants to change their profession, one needs to be accepted by another corps and the current head of corps should be willing to give up on the personnel. Getting my head of corps (I belonged to the budget corps) was going to be impossible, especially if my direct commanders were not willing to give up on me. The bureaucratic war was too much for me and I decided it would be smart to back off this time, in the words of Sun Tzu *“He who knows when he can fight and when he cannot will be victorious”*.

A summary of the Kaggle project I presented at the interview is given below.

House Pricing in Ames Iowa with Random Forests 
===============================================

The problem at hand and proposed goals
--------------------------------------

The original problem as posted in [Kaggle](https://www.kaggle.com/c/house-prices-advanced-regression-techniques#description) is of predicting the price in which a house will be sold (more about the data in the next section). The problem could interest potential buyers or sellers of a house who wish to have a fair estimate of the price of a house. My personal goal was to address the problem by using an algorithm which I am unfamiliar with. I decided to use the Random Forest algorithm since the idea of using trees for regression intrigued me and because it is rather easy to optimize compared to other similar algorithms such as Gradient Boosting. Some of the advantages of the random forest for regression tasks are: 

<ul>
  <li> It is relatively fast and simple to tune. </li>
  <li> It has an estimate of variable importance. </li>
  <li> It can be used to impute missing values. </li>
  <li> It can use “Out Of Bag” (OOB) observations for tuning. </li>
</ul>

The main drawback of the random forest is that it is hard (yet somewhat possible) to interpret and learn about the size and direction of the effects. Another drawback is that it may poorly perform on examples that are far from the training set. Other tree-based alternatives to the random forest would be GBM and AdaBoost. Obviously, linear model such as the L1/L2/Elastic-Net/MLR regression models could have been used to address the problem. The main advantage of the linear models to the tree-ensembeling methods is that they are generally better for drawing conclusions about the effects.
The outline of the strategy used to address the problem is:

<ol>
  <li> Loading the data in R and starting with missing data analysis </li>
  <li> Performing some basic feature engineering and data exploration </li>
  <li> Tune the Random Forest and analyze the results </li>
  <li> Repeat stages 2-3 if needed </li>
</ol>

The data and initial data cleaning
----------------------------------

The data describes individual house sales in Ames, Iowa from 2006 to 2010 (notice that it covers the sub-prime crisis period). The dataset contains 2930 observations however Kaggle only provides the sale price of 1460 observations. The dataset contains 79 variables, some are numeric and some are categorical. Qualitatively, the variables can be divided into 3 types: variables that describe the house itself (size of lot, number of rooms etc.); variables that explain the geography of the house such as the neighborhood of the house and variables that explain the terms and conditions in which the sale was made. We note that the vast majority of the variables are of the first type.

#### Missing data analysis
Initially, the data contains many missing values	, especially for some of the categorical variables. For example, the variable PoolQC which describes the quality of the pool has 2909 missing values. According to the metadata regarding the categorical variables, many of these cases don’t represent absence of the data yet absence of the attribute itself. We initially changed all of these cases from NA to a new category: “noAttrib”. Such an imputation is too coarse since we impute cases which are really NA so for each imputed variable we use another similar variable in order to detect the real NA cases (For example, we use the Total Basement Area to detect the real missing values in the other basement variables by using the cases where Total Basement Area>0).
Another variable that had many missing values (about 10%) was LotArea. We imputed the missing values for this variable by using a linear regression of LotArea on LotArea and LotConfig.
Finally, we note that the authors of the data marked a few observations of extremely large houses sold relatively cheap as outliers. We ignored those observations in the following analysis.   

Data exploration and feature engineering
----------------------------------------

After reviewing the meta-data we learn that the TotalBsmtSF is a linear combination of BsmtFinSF1, BsmtFinSF2 and BsmtUnfSF so we removed the 3 extra variables. We also notice that MSSubClass contains information from HouseStyle and BldgType so we remove it. Random forests don’t strictly suffer from multicolinearity of variables as linear models do, however leaving the extra variables may harm the variable importance ranking by giving less weight to the collinear variables. Also, the partition of the basement area by quality didn’t help much in explaining the price.
While exploring the data we find that some of the categorical variables are extremely unbalanced and suffer from low variation. These variables suffer from near zero variance and they might cause the model to over fit. Hence we prune the variables: Utilities, MiscVal, PoolArea, PoolQC, Street, Condition2, RoofMatl, Heating, Alley, LowQualFinSF, BsmtFullBath, BsmtHalfBath, MiscFeature.  Below is a visual example of one of the pruned variables (pool size and quality):

<p align="center">
<img src="https://raw.githubusercontent.com/nirryde/nirryde.github.io/master/Images/AmesKaggle/1.png" />
</p>

Other categorical variables with many levels suffered from low variance for some of the categories. Yet, the problem was not as severe as discussed above. To introduce more balance to those variables, we grouped similar categories together. We also took caution with ordinal variables by grouping them properly. For example, variables with a 1-10 scale measure such as OverAllQual where scaled to a 3 level scale (see graphs below):

<p align="center">
<img src="https://raw.githubusercontent.com/nirryde/nirryde.github.io/master/Images/AmesKaggle/2.png" />
</p>

As we mentioned, the data on house sales spreads over the years 2006-2010, a period which covers the sub-prime crisis. We added a dummy variable indicating that the house was sold during the subprime crisis (December 2007 – June 2009). Although a quick plot depicts the sale prices weren’t different for the period crisis we keep the variable to examine its importance in the random forest. Also notice that the crisis accounts for 30% of the period of the sample while 33% of the sales in the sample were during the crisis:

<p align="center">
<img src="https://raw.githubusercontent.com/nirryde/nirryde.github.io/master/Images/AmesKaggle/3.png" />
</p>

The dataset contained the variable YearRemodAdd which described the year in which the house was remodeled (equals to YearBuilt if house not remodeled). We replaced it with the variable YearsSinceRemod which describes how many years have passed since the house was remodeled. We also wanted to add a variable for the age of the house at sale however, since the data doesn’t range over a long period, such a variable is strongly correlated with YearBuilt. An attempt was made to create a new variable for the age of the house at sale. However, since the period of the sample was very short, the new feature was highly correlated with YearBuilt and so the feature wasn’t added.   

Preparing the data and running the model
----------------------------------------

#### Data Preparation
Unlike other regression techniques like linear regression methods, random forests are not affected by monotonic transformations of the variables. This is because regression trees split each feature space based on the order of the feature values. Missing values that were not yet treated were imputed with the MICE package which offers a random forest imputation which also works for categorical variables. The response variable, SalePrice was log-transformed so that RMSE and the feature effects would be in terms of % of house price. Categorical variables were not expanded to dummy variables. Dummy expansion of categorical variables makes interpretation of the model very difficult and it discriminates the categorical variables in favor of the continuous variables. Since random forests use OOB data to tune the parameter mTry, there is no need to split the data to a train and test sets. 

#### Growing the Random Forest and performing diagnostics
The Random Forest was grown using the “RandomForest” package. Tuning the number of randomly sampled features “mTry” was done with the tuneRF function which starts with the initial value recommended by Breiman – 2/3 of the number of the features. The tuning uses OOB error to search to the left and right of the current best value until the improvement is no larger than the specified value. The number of trees was left at the default 500. Generally, more trees reduce the variance of the forest however the addition of trees has diminishing returns. The initial run for the random forest reached an OOB RMSE of 0.0142 (1.42%) and the model was able to explain 87% of the variation in sale price. The learning curve below depicts the diminishing returns of adding trees.

<p align="center">
<img src="https://raw.githubusercontent.com/nirryde/nirryde.github.io/master/Images/AmesKaggle/4.png" />
</p>

Some diagnostic plots are given below for the OOB samples (see also graph 2 in the appendix for test set results). In the first plot we can see that residuals (actual-prediction) seem random however we can see that the model overpriced cheap houses hence it may not account for some attributes that characterize cheap houses. In the third plot we can see that the residuals have a nice symmetrical bell shaped density yet it still suffers from some fat tails.  

<p align="center">
<img src="https://raw.githubusercontent.com/nirryde/nirryde.github.io/master/Images/AmesKaggle/5.png" />
</p>

**Table 1** in the appendix shows the variable importance based on % increase in MSE. We can see that the sizes of the floors and basement are important as well as the size of the lot. The neighborhood is also of great importance. We can also notice that the subprime wasn’t important. Feature pruning was not helpful in increasing the models’ performance hence we believe that the model tends to under fit the data. The importance metric only gives us a feeling of how a feature matters in explaining the sale price. The importance metric doesn’t tell us a thing about the magnitude and direction of the features. As mentioned, the main drawback of the random forest is its limited interpretability. Indeed, random forests do not output a numerical output for feature effects as linear regression models do and we might be interested in understanding the relationships of the features with the price. A simple solution to this is plotting partial plots for the desired variables. The partial plots (as implemented in the RandomForest package) attempts in plotting the marginal effect of the feature by plotting the function: 

![](https://latex.codecogs.com/gif.download?%5Cwidehat%7Bf%7D%28x%29%3D%5Cfrac%7B1%7D%7Bn%7D%5Csum_%7Bi%3D1%7D%5E%7Bn%7Df%28x%2C%5Ctextbf%7Bx%7D_i%29)

Where **x**_i is a vector of the other variables. Essentially, the plot is the average response value over the different values of **x**_i for different values of x. The drawback of this method is that it ignores possible interactions with other features. A possible solution to this problem was given by Soeren Wellings’ forest floor method (see also forestFloor package for R). Welling suggests plotting the OOB feature contribution:

**INSERT EQUATION**

Where L_[ijk] is the local increment (change in predicted value of observation i at tree j in node k) of feature and H_[ijl] is the set of local increments where feature the parent node was split by feature l. Next, we try to find some possible interactions by using the feature contribution method. The first idea that came up to mind was an interaction with the year the house was built. “YearBuilt” was not found to interact with any of 12 of the most important variables (see graph 1 in the appendix). After several attempts with other variables a couple of possible interactions were found with the size of the garage:

<p align="center">
<img src="https://raw.githubusercontent.com/nirryde/nirryde.github.io/master/Images/AmesKaggle/6.png" />
</p>

The vertical color changes in the second row of plots indicate the corresponding variables may have an interaction with the size of the garage. A possible reason for the interaction with the size of the first floor is that buyers who consider the size of the first floor are also considered by the size of the garage (see plot below). When adding the interactions to the model, the MSE merely improved yet the new interaction variables were found to be at the top 15 important variables. This demonstrates that the interactions may not be that strong.

<p align="center">
<img src="https://raw.githubusercontent.com/nirryde/nirryde.github.io/master/Images/AmesKaggle/7.png" />
</p>

The feature contributions plots that were given above depict the power of the random forest in finding non linear relationships for the data.

Conclusions 
-----------

Using the random forest to address the problem of house pricing with the proposed dataset gave some reasonable results. Some primal attempts with regularized linear regression models were able to outperform the suggested random forest. Obviously each algorithm handles data in different ways and we have seen that many methods for preprocessing the data that are used for linear models do not have any effect on the random forest. Examples for such treatments are feature normalization and monotonic transformations. Hence, the main focus in working with random forests for regression problems should be on finding good features and analyzing the possible interactions between the features. Tuning the random forests’ “mtry” is rather easy and thanks to the OOB samples we can tune the parameter without using the standard cross validation method. 
The results have shown that our model has done a reasonable job, especially for the “average” houses. The model seemed to overprice cheap houses and under price expensive houses perhaps since there weren’t many examples from such houses. Our model didn’t improve when low importance variables were pruned and the performance of the algorithm was similar for the test set. Based on the evidence we deduced that the model tends to under fit the data. The cure for an under fitting random forest would be to get more observations or more (good) features. In this particular case, our resources of data are limited and we are also limited in what kind of feature engineering can be done with random forests. Hence, for the sake of this particular problem, we would probably be better off using a different model. 

Refrences
---------

[Random Forests, Leo Breiman](https://www.stat.berkeley.edu/~breiman/randomforest2001.pdf)\
[ N. Dingewall, C.Potts, Are Categorical Variables Getting Lost In Your RF?](https://roamanalytics.com/2016/10/28/are-categorical-variables-getting-lost-in-your-random-forests/)\
[S.Welling et al. Forest Floor Visualiztions Of Random Forests](https://arxiv.org/pdf/1605.09196.pdf)\
[J. Friedman, R.Tibshirani, T.Hastie, The Elements Of Statistical Learning](https://statweb.stanford.edu/~tibs/ElemStatLearn/printings/ESLII_print10.pdf)\
[Using Random Forests to select variabels, Cross-Validated ](https://stats.stackexchange.com/questions/164048/can-a-random-forest-be-used-for-feature-selection-in-multiple-linear-regression)\
[Is Feature Engineering relevant for Random Forests, Quora](https://www.quora.com/Is-feature-engineering-relevant-at-all-for-Random-Forests)\
