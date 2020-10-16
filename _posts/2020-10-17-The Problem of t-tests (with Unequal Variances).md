---
layout: post
title: "The Problem of t-tests (with Unequal Variances)"
date: 2020-10-17
---

The Behrens-Fisher Problem
-------

If you have taken a course on statistics which covers hypothesis testing, you were probably encountered with the following scenario: We would like to test the equality of two normal means $$\mu_x, \mu_y$$ based on an independent sample $$x_1,…,x_n; y_1,…,y_m$$ from the respective normal distributions $$X \sim N(\mu_x,\sigma_x^2), Y \sim N(\mu_y, \sigma_y^2)$$. 
When the variances, $$\sigma_x^2, \sigma_y^2$$, and their ratio $$\sigma_x^2 / \sigma_y^2$$ are unknown we are faced with an extremely difficult problem! This is known as the Behrens-Fisher problem which was presented by German chemist and statistician Walter Behrens and was formally stated by Ronald Fisher.

What is the Problem?
---------------------

To understand the Behrens-Fisher problem we must first understand how hypothesis testing works within the parametric-frequentist paradigm. Recall that under this paradigm, we assume our data arises from some parametric family of probability distributions, $$\mathscr{F}_\Theta$$ where $$\Theta \subseteq \mathbb{R}^p$$ denotes the parameter space - the set of all possible values of the parameters $\theta=\left( \theta_1,...,\theta_p \right)$. Moreover, we assume the parameters of the distribution from which our data was generated are (at least partially) unknown and are fixed constants of nature (just like we treat the gravitational constant for example). When conducting a hypothesis test, our null and alternative hypotheses induce a partition of the parameter space into two subsets, one which corresponds to the null hypothesis, $$\Theta_0 \subseteq \Theta$$, and one which corresponds to the alternative hypothesis, $\Theta_1= \Theta_0^c$.  
For simplicity, assume we want to test the hypothesis $H_0: \theta_1=0$ and that $\theta_2,...,\theta_p$ are unknown. To do this, we construct a statistic[^1] $S(x_1,…,x_n)$, whose distribution will depend on the parameter we are testing and perhaps on the unknown parameters: $\theta_2,...,\theta_p$. Now let $$\Omega$$ denote the sample space of the statistic $S$, then to conduct the test at a level of significance $\alpha$, we need to find a subset $M_\alpha \subseteq \Omega$ which satisfies two conditions:
<ol>
<li> If the null hypothesis is true, then the probability of the event $\{S \in M_\alpha \}$ does not depend on the unknown parameters: $\theta_2,...,\theta_p$. This condition insures the practicality of the test.</li>
<li> The probability of the event $\{S \in M_\alpha \}$ is equal to $1-\alpha$. This condition insures us the test is significant at the $1-\alpha$ level.</li>
</ol>
The Behrens-Fisher problem is to find a statistic and a partition of the sample space of the statistic under the null hypothesis which will satisfy the two required conditions above (recall this is for the problem of testing the equality of normal means with no knowledge on the variances). Notice that if the first condition is not met, we may be tempted to estimate the unknown parameters. However, by doing so, we risk satisfying the second condition.

Solutions for the Problem
--------------------------

Different schools of thought suggested solutions to the Behrens-Fisher problem. Fisher suggested a solution based on his Fiducial inference paradigm which was never widely accepted. Jeffreys, using a Bayesian approach, suggested a solution which is numerically similar to Fisher’s solution however has a different interpretation. Of course, both the Fiducial and the Bayesian approaches are not valid for pure Frequentists.  
Yurri Linnik, who was a Soviet mathematician proved several properties regarding frequentist solutions for the Behrens-Fisher problem. Two important properties due to Linnik are that a solution exists if the sample sizes, $$n,m$$, are unequal and that there is no uniformly most powerful solution.  
Among all of the frequentist solutions, Welch’s solution is the most widely used and taught solution in statistics courses. Many courses present Welch’s solution without stating the Behrens Fisher problem, perhaps since understanding the problem requires some abstract understanding. Welch’s solution is based on the trivial statistic:

$$t = \frac{\bar{x} - \bar{y}}{\sqrt{ \frac{s_x^2}{n} + \frac{s_y^2}{m} } }$$

If you are familiar with sufficient statistics, you will have noticed Welch’s t-statistic is based on the sufficient statistics $$\bar{x} - \bar{y}, s_x^2, s_y^2$$. Scheffe’s solution for example is not based on sufficient statistics and thus theoretically yields a sub-optimal test in terms of power. Welch approximated the distribution of his statistic with the t-distribution with a special degree of freedom which can simply be written as:

$$df = \frac{1}{\frac{c^2}{n-1}+\frac{1-c^2}{m-1}}$$

Where $$c$$ is a function of $$s_x, s_y, n, m$$. Welch’s degree of freedom is typically not an integer so in practice the degrees of freedom are rounded down to the closest integer. It is worth mentioning that Welch's degree of freedom is random since $c$ depends on the realized sample data.
Welch’s solution is simple and utilizes sufficient statistics which explain the popularity of the method. However, it is still not an exact solution as demonstrated in the next section.

Simulation Time
----------------

Now comes the fun part of demonstrations with simulations. Of course, simulations are by no means a formal proof, however, they can serve as a useful tool for intuitive understanding. The R-code below simulates $10^5$ runs of the following experiment: first we draw 100 random numbers from the $$N(1,9)$$ distribution and 120 random numbers from the $$N(1,1)$$ distribution. Then we perform a Welch t-test to test if $$X,Y$$ have the same mean and we record whether the test rejected the null hypothesis of equal means. We also record the t-statistic from the t-test for comparing normal means assuming equal variances.  
If Welch’s t-test is precise, the proportion of (false) rejections should be **very** close to 5%.

```
set.seed(3071)

reps <- 10^5
alpha <- 0.05 # significance level 
test.res <- logical(reps) # records which repetitions were rejected
test.stat <- numeric(reps) # records the t-statistic of each repetition assuming equal variances

for( rep in 1:reps )
{
  x <- rnorm(100, mean=1, sd=3)
  y <- rnorm(120, mean=1, sd=1)
  
  test.res[rep] <- t.test(x, y, conf.level=1-alpha, var.equal=F)$p.value < alpha 
  test.stat[rep] <- t.test(x, y, conf.level=1-alpha, var.equal=T)$statistic
}

sum(test.res==T)/reps # the proportion of flase rejections is 0.04923 < 0.05

library(ggplot2)
d <- data.frame(test.stat, t=rt(reps,df=120+100-2))
ggplot(d) + geom_density(aes(t, color="sim t")) + geom_density(aes(test.stat, color="test t"))
```  

We can see that in our simulation, the proportion of false nulls is 4.923% which is not **very** close to the desired 5% and is actually smaller. In other words, the Welch test is too conservative.  
The plot below compares the empirical distribution of the t-statistics assuming the variances of $X,Y$ are equal to the empirical distribution of $10^5$ random samples from a $t_{df=218}$ distribution. Notice how the t-statistic's distribution is more spread-out compared to the distribution of the simulated t random variables. From this plot we learn that if we mistakenly use the t-test that assumes equal variances when the variances are in fact different, we are prone to over-inflating the type 1 error rate. In other words, the resulting significance will be higher than the desired $\alpha$.

<p align="center">
<img src="https://raw.githubusercontent.com/nirryde/nirryde.github.io/master/Images/Ttest/Rplot01.png" />
</p>

When it is unclear whether the variances are equal, it is often suggested to first use Levene's test or Bartlett's test for testing the equality of the variances and then choose the appropriate t-test. Some (Delacre et al., Graeme) suggest that the Welch test should be used by default instead of first testing for equal variances. In fact, the default setting for the variance assumption in the t.test function in R is var.equal=FALSE. One reason for using Welch's test by deafault is that it achives a similar power as the t-test when the variances are in fact equal. This is demonstrated in the next simulation where $10^5$ repetitions of the following experiment are made: 100 random variables are drawn from the $N(1,9)$ distribution and 120 random variables are drawn from the $N(1.25,9)$ distribution. For each repetition we record the decision from the t-test assuming equal variances and the decision from Welch's test.  

```
t.test.res <- Welch.test.res <- logical(reps)
for( rep in 1:reps )
{
  x <- rnorm(100, mean=1, sd=3)
  y <- rnorm(120, mean=1.25, sd=3)
  
  t.test.res[rep] <- t.test(x, y, conf.level=1-alpha, var.equal=T)$p.value < alpha 
  Welch.test.res[rep] <- t.test(x, y, conf.level=1-alpha, var.equal=F)$p.value < alpha
}

sum(t.test.res)/reps # estimated power of the t test is 9.257%
sum(Welch.test.res)/reps # estimated power of Welch's test is 9.237%

```

The t-test resulted in an estimated power of 9.257% and Welch's test resulted in an estimated power of 9.237% so both tests are very close in terms of power. The power gap between both tests narrows as the differnce in means increases.  

Summing Up
-----------

The problem posed by Behrens & Fisher is fundamental yet deriving a precise solution is notoriously difficult. I find it interesting that the problem is not stated in any course on mathematical statistics (at least those I know of) and I think it is at least worth mentioning in such courses. We are lucky to have translations of Linnik’s writing on the matter. Interestingly, language can be a barrier for information to spread across a discipline. This problem may be less acute in the light of modern globalization, yet I believe that in the last century, this was an actual problem. I have not conducted any serious research on this matter; however, I believe there were many interesting findings in the Soviet Union which were never translated to English which limits the exposure of those findings.   
Although Welch’s solution seems to suffice in practice, it would be interesting to see if a precise solution will be developed within the parametric-frequentist paradigm. In this post I did not mention non-parametric solutions as I wanted to focus on parametric solutions.  
I should also mention that the Behrens-Fisher problem extends to the multivariate case. In this case, we compare vectors of means and we assume no knowledge on the covariance matrices of the normal distributions.  
I hope this post was useful and that you have learned and understood the nature of the Behrens-Fisher problem.

[^1]: A statistic is any function of the sample. For example, the sample itself $$(x_1,…,x_n)$$ as a vector is a statistic, another familiar statistic is the sample mean. An important notion is the sufficient statistic which is a statistic which contains all of the information on the unknown parameters as the sample itself. In other words, the sufficient statistic can be thought of as a “good” summary of the data in regard to the unknown parameter.  

References
-----------

[Behrens-Fisher problem. Encyclopedia of Mathematics.](https://encyclopediaofmath.org/wiki/Behrens-Fisher_problem)

Seock-Ho Kim and Allan S. Cohen, On the Behrens-Fisher problem: A Review, Journal of Educational and Behavioral Statistics, 1998, Vol. 23, No.4 pp.356-377

Yurri Linnik, Statistical Problems with Nuisance Parameters – translated from Russian (1966)  

Delacre, M., Lakens, D., & Leys, C. (2017, February 17). Why Psychologists Should by Default Use Welch's t-test Instead of Student's t-test (in press for the International Review of Social Psychology). https://doi.org/10.31219/osf.io/sbp6k  

Graeme D. Ruxton, The unequal variance t-test is an underused alternative to Student's t-test and the Mann–Whitney U test, Behavioral Ecology, Volume 17, Issue 4, July/August 2006, Pages 688–690, https://doi.org/10.1093/beheco/ark016

