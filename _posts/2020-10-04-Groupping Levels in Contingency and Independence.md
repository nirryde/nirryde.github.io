---
layout: post
title: "Groupping Levels in Contingency and Independence"
date: 2020-10-04
---

A Lack of Choice
----------------

A couple of years ago I took a course on analysis of contingency tables. To be honest, I did not want to take this course except that I had no choice as I wanted to take some course and not many courses were offered at that semester. In case you do not know, contingency tables are two-way tables used for describing the joint distribution of two discrete variables. Suppose we collected data on two discrete variables $$X,Y$$ each having $$I,J$$ levels, respectively, then there are $$I\cdot J$$ possible outcomes. A contingency table for $$X,Y$$ has $$I$$ rows and $$J$$ columns and it describes the observed counts of all $$I\cdot J$$ possible outcomes. The $$(i,j)$$ entry of the contingency table is denoted by $$n_{ij}$$. 
Let $$n_{++}$$ be the total sample size, then: $$n_{++}=\sum_{i=1}^{I}\sum_{j=1}^{J} n_{ij}$$. Another way of presenting contingency tables is by frequencies. In this case, the $$(i,j)$$ entry of the table is the sample frequency $$n_{ij}/n_{++}$$. If one chooses to report the table in the frequency form, one should also report the sample. 	. 
Contingency tables are not a sexy topic in statistics as much of the theory in the matter was developed when computers where not widespread. As a result, many methods for analyzing contingency tables were developed so that they could be used with a pen, a paper, and a suitable distribution table (such as the table of the standard normal distribution many students these are familiar with[^1]). As I expected, the course was not very interesting although there were a couple of interesting gems one of them is the subject of this post.

Testing for Independence with Pen & Paper
-----------------------------------------

When our variables of interest $$X,Y$$ are both response variables, possibly the question of most interest is whether or not $$X,Y$$ are dependent. Let $$n_{i+}=\sum_{j=1}^{J}n_{ij}$$, $$n_{+j}=\sum_{i=1}^{I}n_{ij}$$ and $$\hat{\mu}_{ij}=\frac{n_{i+}\cdot n_{+j}}{n_{++}} $$, then the log of the likelihood ratio statistic (which we will call the $$G$$ statistic) for testing the independence of $$X,Y$$ is:

$$G=2\sum_{i=1}^{I} \sum_{j=1}^{J}n_{ij}log \left( \frac{n_{ij}}{\hat{\mu}_{ij}=\} \right)$$

Introductory statistics courses in all levels will usually present the well-known chi-square test for independence and not mention the $$G$$ statistic. The chi-square statistic is obtained from the second-order Taylor approximation of the log term about 1 and is given by:

$$\chi^2= \sum_{i=1}^{I}\sum_{j=1}^{J} \frac{\left(n_{ij}-n_{i+}\cdot n_{+j} \right)^2}{ n_{i+}n_{+j}}$$

Under the null hypothesis of independence, both $$chi^2$$ and $$G$$ approximately follow a $$\chi^2_{(I-1)(J-1)}$$ distribution and in practice both yield similar results. So why is the $$\chi^2$$ test, which is obtained as an approximation, so popular? Calculation of $$G$$ requires evaluating logarithms which is hard unless you have a calculator of some sort. Today this may seem trivial however in the year ~1900 this was not trivial at all, and it was desired that test statistics could be calculated by pen & paper. 

Grouping Levels in Contingency Tables
--------------------------------------

For me, the $$G$$ test was one of the interesting topics in the contingency table course, mainly due to a proof I constructed for a property related to grouping levels in a contingency. The property is that when grouping levels of a variable, the $$G$$ statistic of the new contingency table is smaller than of the original table thus the null is more likely. 
Let $$M_0$$ denote a contingency table of dimension $$I \times J$$. Now, let $$M_1$$ be the $$(I-1) \times J$$ contingency table obtained by merging the first two rows of $$M_0$$. In other words, the first row in $$M_1$$ is the sum of the first two rows in $$M_0$$. The remaining of the $$I-2$$ rows in $$M_1$$ are equal to the last $$I-2$$ rows in $$M_0$$. We denote the $$G$$ statistic of $$M_k$$ with $$G_k$$. 

**Theorem:** Using the definitions above: $$G_0 \geq G_1$$. \
**Proof:** We shall prove the theorem in two ways. The first proof uses the properties of decomposition of contingency tables. The second proof is based on the relation of the $$G$$ statistic and information theory.
The rules for decomposing a contingency table into independent sub-tables are[^2]:

<ol>
  <li> Each cell count in the original table must be a cell count in one and only one sub-table. </li>
  <li> Each marginal total in the original table must be a marginal total in one and only one sub-table. </li>
  <li> The sum of the degrees of freedom for all sub-tables must equal the degrees of freedom for the original table. </li>
</ol>

Let $$M_2$$ denote the $$2 \times J$$ sub-table made from the first two rows of $$M_0$$. We will show that the decomposition of $$M_0$$ into $$M_1$$ and $$M_2$$ satisfies all three conditions above.
It is easy to see that each cell count from the first two rows in $$M_0$$ appears only in $$M_2$$. The rest of the cell counts from $$M_0$$ appear only in $$M_1$$.
The first two marginal row counts in $$M_0$$ appear only in $$M_2$$. The rest of the marginal row counts from $$M_0$$ appear only in $$M_1$$. It is easy to see that all column counts from $$M_0$$ appear only in $$M_1$$. 
Finally, $$M_0$$ has $$(I-1)(J-1)$$ degrees of freedom. $$M_1$$ has $$(I-2)(J-1)$$ degrees of freedom and $$M_2$$ has $$2(J-1)$$ degrees of freedom. It follows that the sum of degrees of freedom of $$M_1,M_2$$ is equal to the degrees of freedom of $$M_0$$. 
Since the decomposition of $$M_0$$ into $$M_1,M_2$$ satisfies all of the three decomposition rules, it follows that $$G_0=G_1+G_2$$. The result now follows from the fact that $$G_k \geq 0$$ hence $$G_0 \geq G_1$$. 

For the algebraic proof, let $$pi_{ij}=n_{ij}/n_{++}$$. Notice we can write $$G_0$$ as: \

$$G_0=2n_{++}\sum_{i=1}^{I}\sum_{j=1}^{J}\pi_{ij}log \left( \frac{\pi_{ij}}{\pi_{i+}\pi_{+j}} \right)$$

By the definition of $$M_1$$ we can write: \

$$G_1=2\sum_{i=3}^{I}\sum_{j=1}^{J}n_{ij}log \left( \frac{n_{ij}}{\hat{\mu}_{ij}} \right)+2\sum_{j=1}^{J}\left(n_{1j}+n_{2j}\right)log \left(\frac{ n_{1j}+n_{2j}}{\hat{\mu}_{1j}+\hat{\mu}_{2j}}\right) $$

$$=2n_{++}\sum_{i=3}^{I}\sum_{j=1}^{J}\pi_{ij} log \left( \frac{\pi_{ij}}{\pi_{i+}\pi_{+j}} \right)+2n_{++}\sum_{j=1}^{J}\left(\pi_{1j}+\pi_{2j}\right) log \left(\frac{\pi_{1j}+\pi_{2j}}{\left( \pi_{1+}\pi_{2+} \right) \pi_{+j}} \right)$$

Using the expressions above for $$G_0, G_1$$, we can write:

$$\frac{1}{2n_{++}}\left( G_0-G_1 \right)=\sum_{i=1}^{2}\sum_{j=1}^{J}\pi_{ij} \left( log \left( \frac{\pi_{ij}}{\pi_{i+}\pi_{+j}} \right) – log \left( \frac{\pi_{1j}+\pi_{2j}}{\left( \pi_{1+}+\pi_{2+} \right)\pi_{+j}}\right) \right)$$

$$=\sum_{i=1}^{2}\sum_{j=1}^{J}\pi_{ij} \left( log \left( \frac{\pi_{ij}}{\pi_{i+}} \right) -log \left(\frac{\pi_{1j}+\pi_{2j}}{\pi_{1+}+\pi_{2+}} \right) \right)$$

$$=\sum_{i=1}^{2}\sum_{j=1}^{J}\pi_{ij} \left( log \left( \frac{\pi_{ij}}{\pi_{1j}+\pi{2j}} \right) -log \left(\frac{\pi_{i+}}{\pi_{1+}+\pi_{2+}} \right) \right)$$

Recall that the log-sum inequality states that for any sets of non-negative numbers: $$(x_i)_{i=1}^{n},(y_i)_{i=1}^{n}$$ with $$x=\sum_{i=1}^{n}, y=\sum_{i=1}^{n}$$ the following inequality holds:

$$\sum_{i=1}^{n}x_i log \left(\frac{x_i}{y_i} \right) \geq x \cdot log \left(\frac{x}{y} \right)$$

With equality if and only if $$x_i /y_i$$ are equal for all $$i$$. In our case, this would imply that the first two rows of $$M_0$$ are linearly dependent. 
Applying the log-sum inequality to the last expression for $$0.5n_{++}^{-1}(G_0-G_1)$$ yields:

$$\sum_{j=1}^{J}\pi_{ij}\log \left( \frac{\pi_{ij}}{\pi_{1j}+\pi{2j}} \right) \geq \pi_{i+}log \left( \frac{\pi_i+}{\pi_{1+}+ \pi_{2+}} \right)$$

By plugging the RHS in the inequality above in the expression $$0.5n_{++}^{-1}(G_0-G_1)$$ we obtain:

$$\frac{1}{2n_++}\left( G_0-G_1 \right) \geq \sum_{i=1}^{2} \left( \pi_{i+} log \left( \frac{\pi_{i+}}{\pi_{1+}+\pi_{2+}} \right) - \sum_{j=1}^{J}\pi_{ij} log \left( \frac{\pi_{i+}}{\pi_{1+}+\pi_{2+}} \right) \right)=0$$

It can now be seen that: $$G_0 \geq G_1$$. Due to the condition of equality in the log-sum inequality, $$G_0=G_1$$ when the first two rows of $$M_0$$ are linearly dependent. 


References
----------
Alan Agresti, Categorical Data Analysis, Third Edition
Jo Ann Colas, Partition of the Chi-Squared Statistic in a Contingency Table, Masters Thesis

[^1]: I have an opinion on the matter but that is a subject for another post.
[^2]: See Agresties' book and [“Partition of the Chi-Squared Statistic in a Contingency Table”, Jo Ann Colas](https://ruor.uottawa.ca/bitstream/10393/30352/1/Colas_Jo_Ann_2014_thesis.pdf)


