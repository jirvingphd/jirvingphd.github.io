---
layout: post
title:      "Hypothesis Testing with Northwind Database"
date:       2019-04-14 16:42:51 -0400
permalink:  hypothesis_testing_with_northwind_database
---

## Introduction / Project Assignment

**For this project, we were hired by the Northwind company allowed us access to their SQL databases to answer a specific question:**

> **Do discounts have a statistically significant effect on the number of products customers order? If so, at what level(s) of discount?”**

Additionally, we were tasked with thinking of  3 other questions that would be of interest to the company.
They requested we formally apply hypothesis testing for our results to see if we could the null hypothesis, that there is no effect of discounts on quantities purchased.
    
After considering their initial question and examining their databases, we decided that the company would be interested in understanding other aspects of consumer purchasing behavior. We then formulated 3 addititional questions along those lines.

___
## Four Hypotheses Regarding Consumer Purchasing Behavior

* **Q1:"Do discounts have a statistically significant effect on the number of products customers order?  If so, at what level(s) of discount?** 

    - Null Hypothesis (H0) : Products that are discounted sell the same quantities as full-proce products.
    - Alternative Hypothesis (H1): Products that are discounted sell in higher quantities.
    

* **Q2: Do customers spend more money overall if they are buying discounted items?**
    - H1: Customers spend more money overall when their order includes discounted items.
    - H0: Customers spend the same amount regardless of discounted items. 
    
    
* **Q3: Does time of year affect how much consumers buy?**
    - H1 = The month an order is placed relates to either a higher or lower mean quantity of items sold.
    - H0 = The month of an order has no affect on the mean quantity of items sold.

* **Q4:  Do countries that buy discounted items in higher-than-average quantities spend more money (have higher order totals)?**

    - H1: Different countries purchase different quantities of discounted vs non discounted products. 
    - H0: All countries purchase the same quantities of discounted vs non discounted products. 


___

## Our Methodology
- **We first formally defined our hypothesis and null hypothesis.**   
 <br>
- **We then asked 2 key questions abour our data and then used the results of these tests to choose the most appropriate test for each hypotheses.**

    1. **How many samples will I be comparing?**
        - 1 group vs an idealized distribution?
        - 2 groups?
        - More than 2?
 
    2. **Do we need a 1-tailed or 2-tailed test? In other words, is there an expected directionality to our hypothesis?**
        - Are we asking if group A will be have larger/higher values than Group B?
        - Or are we just asking if the 2 groups are different?-   

    3. **Is our data normally distributed?**
        - Does our data meet the required assumption of normality?

    4. **Is the variance in our data approximately equal?**
        - Is there an equal spread to the different samples of data?

- **We then selected the appropriate test based upon these criteria.**
    - For significant results, we then calculated the effect size using Cohen's d.
    - We additionally ran post-hoc tests for pairwise comparisons for any hypothesis that had more than 2 groups.

___
        
### We used the structure of the specific aims below to guide our workflow:

- ***Aim 1:To select the proper dataset for analysis, perform EDA, and generate data groups for testing.***
    - Use SQL Alchemy with Pandas DataFrames to retrieve and merge/join data from proper SQL tables.
    - Perform any calculations of engineer any features required to test our hypothesis. 
    
- ***Aim 2: Select the appropriate t-test based on our type of comaprison and the results of our tests for normality and homogeneity of variance.***
    
    1. Test for Normality with D'Agostino-Pearson's normality test<br>
        ```scipy.stats.normaltest```
        
    2. Test for Homogeneity of Variance with Levene's Test<br>
         ```scipy.stats.levene```
    
    3. How many samples are we comparing?
    
    4. Does our hypothesis require a 1-tail or 2-tailed tesT?

    - We then chose and ran our test based upon the 4 criteria above.
        - 1 Sample T-Test
        - 2 Sample T-Tesy
        - Welch's T-test
        - Mann Whitney U
        - ANOVA
        - Tukey's multiple pairwise comparison test
    - If the result was significant, we rejected the null hypothesis and performed additional analyses:
        - Calculated effect size with Cohen's d.
- ***Aim 3: If appropriate, perform post-hoc pairwise comparison testing to determine which specific levels of our groups that are significantly differnet.***
     - Tukey's test for multiple pairwise comparisons<br>
     ```statsmodels.stats.multicomp.pairwise_tukeyhsd```
	
___		 
# **Hypothesis 1:**

		 
> **Do discounts have a statistically significant effect on the number of products customers order?**
> **If so, at what level(s) of discount?**

- Null Hypothesis (H0) : Products that are discounted sell the same quantities as full-proce products.
- Alternative Hypothesis (H1): Products that are discounted sell in higher quantities.
<br>

**Specific Aims:**

* ***Aim 1:To select the proper dataset for analysis, perform EDA, and generate data groups for testing.***
    - Used sqlalchemy and pandas.<br>
    ```python 
    query = "SELECT* FROM OrderDetails, GROUPBY discount"
    python pandas.read_sql_query()
    ```
* ***Aim 2: Select the appropriate t-test based on tests for the assumptions of normality and homogeneity of variance.***
<br>
- **Deciding the appropriate test to run:**
    - We had significant results for both our normality and equal variance tests. 
    - We had 2 samples to compare for a one tailed test.
    - We therefore decided on a Mann-Whitney U test, which came back significant.
    - We calculated our effect size using Cohen's d.


<br>
**H1 Test Results**<br>
<img src='https://github.com/jirvingphd/dsc-2-final-project-online-ds-ft-021119/blob/master/Blog%20Post%20Images/table_H1_test_results.png' width='600')<br>
**H1-Main Comparison**<br>


<img  src="https://www.dropbox.com/s/iuh1ul5bn0on766/H1_ppt_KDE_bar.png?raw=1" width=600>
<br>

***H1, Aim 3: To perform post-hoc pairwise comparisons for level of discount***
- **To determine which level of discounts had the largest effect, we performed a pairwise multiple comparison Tukey's test**
- Since there were only 1 item on sale at certain discount levels, we binned the discount levels in 0.5% groups.
    - No Discounts is represnted by the interval (-0.05 to 0].
<br> 
**Post-Hoc Comparison by Discount Level**<br>

<img src='https://www.dropbox.com/s/bglnpg69575x4jm/H1_tukey_bar.png?raw=1' width=600>
<br>


 <img  src='https://www.dropbox.com/s/3uwfhdu6sqg5oil/table_H1_groups_mean_sem.png?raw=1' width=400><br>
    

<img  src='https://www.dropbox.com/s/2ymzmrb6t15hhsy/table_H1_tukey_sig_only.png?raw=1' width=400>



    
## ***Conclusions for Hypothesis 1:***

- **We then concluded we need a non-parametric 2-sample test, so we used the Mann-Whitney U test**. 
    - Our comparison had a p-value less than .05
    - We reject the null hypothesis that discounts do not affect quantities sold.
 **Our post-hoc tukey results showed that discounts of 0-5%, 10-15%, 15-20%, and 20-25% were all significantly different from full price products. 
        - Except 5-10 % discount group
    - No discount groups were significantly different than other discount groups.
___
# **Hypothesis 2:**
> **Do customers spend more money if they are buying discounted items?**

- $H_1$: Customers spend more money overall when their order includes discounted items.

- $H_0$: Customers spend the same amount regardless of discounted items. 

**Specific Aims:**

* ***Aim 1:To select the proper dataset for analysis, perform EDA, and generate data groups for testing.***
    - Used sqlalchemy and pandas.read_sql_query()
    
    ```python 
query = "SELECT* FROM OrderDetails,
            GROUPBY discount```
						
- Calculated sub-totals for every products<br>
```UnitPrice*(1-Discount)*Quantity```

- Grouped By Orders to Calculate Order total and to classify orders
      - Orders that contained at least 1 discounted item
```discounted_order=1```
     - Order without any discounted items 
``` discounted_order=0 ```
* ***Aim 2: Select the appropriate t-test based on tests for the assumptions of normality and homogeneity of variance.***

- **Deciding the appropriate test to run:**
    - We had significant results for both our normality and equal variance tests. 
    - We had 2 samples to compare for a one tailed test.
    - We therefore decided on a Mann-Whitney U test, which came back significant.
    - We calculated our effect size using Cohen's d.
     
**H2 Test Results**<br>
<img  src='https://www.dropbox.com/s/6sivfe6cnzmgt13/table_H2_test_results.png?raw=1' width=500><br>
<br> 
<img src="https://www.dropbox.com/s/gdaqp5ytzjw7rsq/H2_KDE_bar.png?raw=1" width=600>


## ***Conclusions for Hypothesis 2***
- We reject the null hypothesis that there is no effect of an order containing discounted items on the order total.
- Therefore, we have found evidence that customers spend more money when they are buying at least 1 discounted item.
- However, Cohen's d indicates it a small effect size.

_________

# **Hypothesis 3:**
> **Does the time of year affect quantity of items sold?**

- $H_1$ = The month an order is placed relates to either a higher or lower mean quantity of items sold.
- $H_0$ = The month of an order has no affect on the mean quantity of items sold.

<br>

**Specific Aims:**

***Aim 1 : To select the proper dataset for analyiss  and generate data groups for testing.***
- Use sqlalchemy to create engine to connect to Northwind_small.sqlite.<br>
```python
df_ord = pd.read_sql_query("SELECT * FROM OrderDetail JOIN [Order]  ON [Order].Id = OrderDetail.OrderId", engine)```

* ***Aim 2: Select the appropriate t-test based on tests for the assumptions of normality and homogeneity of variance.***
    - We had significant results for both our normality and equal variance tests.
    - We had 2 samples to compare for a one tailed test.
    - We therefore decided on a Mann-Whitney U test, which came back significant.
    - We calculated our effect size using Cohen's d.

<img alt='H3 Mean Sem' src='https://www.dropbox.com/s/ujougqna47xotr8/table_H3_mean_sem.png?raw=1' width=400>

< img alt='H3 Test Results' src='https://www.dropbox.com/s/ahtum2hps9b425d/table_H3_test_results.png?raw=1' width=400>


<img alt ='H3 Test Results Kde bar' src='https://www.dropbox.com/s/fu56niuf301n8y8/H3_tukey_bar.png?raw=1' width=600>


## ***Conclusions for Hypothesis 3:***

3. **We then concluded we need a non-parametric test, so we first used the Mann-Whitney U test to compare each month vs the other 11 months**. 
    -  Several months were significantly different than the rest of the year:
        - Jul, Aug, Sep, Feb, Mar, Apr
    -  
4. **We then did pairwise comparisons of all months using Tukey's test**
    - We could not reject the null hypothesis that month affects the quantity sold. 
    - It is difficult to interpret the contradicting results of the Mann Whitney U and the Tukey's test.
        - But since Tukey's test corrects for multiple comparisons, it should be trusted over M.W.U.

___

# **Hypothesis 4:** 

> Do different countries respond to discounts more than others? 

$H_1$: Different countries purchase different quantities of discounted vs non discounted products. 

$H_0$: All countries purchase the same quantities of discounted vs non discounted products. 

***Aim 1: To select the proper dataset for analyiss  and generate data groups for testing.***
- Used sqlalchemy to create engine to connect to Northwind_small.sqlite.
- Read tables directly into dataframes<br>
```python
DB_Order = pd.read_sql_table('Order',engine);
DB_OrderDetail = pd.read_sql_table('OrderDetail',engine);```

* ***Aim 2: Select the appropriate t-test based on tests for the assumptions of normality and homogeneity of variance.***
    - We had significant results for both our normality and equal variance tests.
    - We had 2 samples to compare for a one tailed test.
    - We therefore decided on a Mann-Whitney U test, which came back significant.
    - We calculated our effect size using Cohen's d.
    
    
<img alt='H4 Mean Sem' src='https://www.dropbox.com/s/sua2t4mege0p1na/table_H4_mean_sem.png?raw=1' width =400>
<img alt='H4 Test Results' src='https://www.dropbox.com/s/z8a7khpwxfjaljl/table_H4_test_results.png?raw=1' width=400>
<img alt='H4 Kde Bar' src='https://www.dropbox.com/s/3fxsf16uxv1woqj/H4_kde_bar_no_notes.png?raw=1' width=500>

<img alt='H4 Tukeys_sig only' src='https://www.dropbox.com/s/7mby9qegzcj6a7x/table_H4_tukey_sig_only.png?raw=1'>
<img alt = 'H4 Map of Quantities by Country' src='https://www.dropbox.com/s/c0cvg3k62pby4g4/H4_map.png?raw=1' width=900>



# Final Conclusions and Recommendations

- Discounts spur consumer purchasing behavior. 
    - They buy discounted items in higher quantities. 
    - Only a **minimal** discount is needed for consumer behavior benefit.
- They spend more money if they are purchasing 1+ discounted items.
- Time of year (by month) was not significant, but further metrics worth investigating.
- Customers from different countries have unique spending behaviors that may be capitalized on with further investigation.





