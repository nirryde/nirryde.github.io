---
layout: post
title: "How I got accepted to an elite technology unit (and why I didn't go)"
date: 2020-09-25
---
A few years ago, I was a budget analyst at the IDF’s medical corps. In retrospective my job was quite interesting, especially when compared to other budget related positions in the IDF. Yet, back then I was not satisfied of my position and I was looking into trying to move into a more analytic position. Through some espionage I found that some elite intelligence unit was looking for a data scientist. I thought I was qualified enough to apply and I somehow managed to get in touch with the head of the team. I called the head of the team and honestly, I cannot remember much of that conversation, but we set up to meet for a first interview. The first interview was not technical, and the head of the team just asked me to present myself and my background. I was at the beginning of a masters in statistics and I have self-taught myself some machine learning yet still, I was a bit worried that my background in economics would not be good enough for a team where everybody were graduates of prestigious CS/math programs. Fortunately, the interviewer thought I was worth a shot and he said I should be ready to present the team a project which should demonstrate my abilities. I had two projects which I presented to the team. The first project was a health-economics project I initiated at my unit (maybe I will be able to write about that project in a separate post). The second project was something I did on one of Kaggle’s playbox datasets and the full summary is given below. Needless to say, I was excited yet confident at the interview. The presentation lasted a solid 3 hours and during the presentation I was tackled with some questions which I was able to respond. I was done by lunchtime and after the presentation we went together for lunch at the mess hall. I expected that the food at such an elite intel unit would be better than average however, to my disappointment to food was merely average. After lunch we headed back to the office for coffee and we talked a bit until I had to go. When I left, I was told by the team leader that I had done a good job and he would let me know if I got accepted. I thanked him for the opportunity as I genuinely felt honored that the whole team halted their work and spend that much time to listen to me.
A week or two after the interview I got call from the team leader saying that they decided to accept me to the team. Obviously, I was very happy, yet I knew this was only the first step at changing my profession. As the saying goes, it takes two to tango. The army bureaucracy dictates that if one wants to change their profession, one needs to be accepted by another corps and the current head of corps should be willing to give up on the personnel. Getting my head of corps (I belonged to the budget corps) was going to be impossible, especially if my direct commanders were not willing to give up on me. The bureaucratic war was too much for me and I decided it would be smart to back off this time, in the words of Sun Tzu *“He who knows when he can fight and when he cannot will be victorious”*.

A summary of the Kaggle project I presented at the interview is given below.

## **House Pricing in Ames Iowa, using the Random Forest Algorithm**

#### **The problem at hand and proposed goals**
The original problem as posted in [Kaggle](https://www.kaggle.com/c/house-prices-advanced-regression-techniques#description) is of predicting the price in which a house will be sold (more about the data in the next section). The problem could interest potential buyers or sellers of a house who wish to have a fair estimate of the price of a house. My personal goal was to address the problem by using an algorithm which I am unfamiliar with. I decided to use the Random Forest algorithm since the idea of using trees for regression intrigued me and because it is rather easy to optimize compared to other similar algorithms such as Gradient Boosting. Some of the advantages of the random forest for regression tasks are: 
*	It is relatively fast and simple to tune
*	It has an estimate of variable importance	
*	It can be used to impute missing values
*	It can use “Out Of Bag” (OOB) observations for tuning 
The main drawback of the random forest is that it is hard (yet somewhat possible) to interpret and learn about the size and direction of the effects. Another drawback is that it may poorly perform on examples that are far from the training set. Other tree-based alternatives to the random forest would be GBM and AdaBoost. Obviously, linear model such as the L1/L2/Elastic-Net/MLR regression models could have been used to address the problem. The main advantage of the linear models to the tree-ensembeling methods is that they are generally better for drawing conclusions about the effects.
The outline of the strategy used to address the problem is:
1.	Loading the data in R and starting with missing data analysis
2.	Performing some basic feature engineering and data exploration
3.	Tune the Random Forest and analyze the results
4.	Repeat stages 2-3 if needed 

#### **The data and initial data cleaning**



