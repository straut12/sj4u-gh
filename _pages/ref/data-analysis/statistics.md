---
layout: page
permalink: /ref/data-analysis/statistics/
title: statistics
description: Statistics
nav: false
toc: true
---

General notes on statistical terms  

Nominal scale variables are labeled with no specific order. 
Ordinal scale the variables are in a specific order (ie, low income, mid income, high income or less than 50, 50-100, over 100 or dislike, neutral, like etc)  

# Continuous vs Discrete  
- Continuous data: dimensional, sensor type data. Range from one extreme to another
    - Normal distribution: Bell shaped (gaussian). Mean+3sigma describes 99.997% of data. Can predict future outcome based on past trends.
    - Lognormal: Curved that increases rapidly
    - F: Broader distribution
    - Chi-Square: Difference between measured and expected.
    - Bi-modal: two distinct data peaks. Type of bivariate distribution  

- Discrete data: has a limited set of values, you can count how many. ie number of defects. 
    - Binomial: Pass/Fail outcome, equal probabilities. Describe probability of a certain number of Pass (or Fail) in a given number of trials
    - Poisson: If the ave rate is known but not the exact timing then the probability that an event will occur within a fixed time period
    - Geometric: Model the number of successful attempts before a failure. The probability of success is known.

# Parametric vs Nonparametric test  
- Parametric test: assumes data follows a certain distribution. Requires previous knowledge about the population.  
- Nonparametric test: does not make assumptions and can be applied to any type of data (ordinal or nominal)  

# Parametric Data Analysis
- t-test for mean comparison: One independent variable with two groups/units (two bake plates, two coaters, etc) and one dependent variable (CD, thickness)
ANOVA will test the equality of three or more group means  
- ANOVA: One independent variable with more than two groups/units and one dependent variable 
- Two-way ANOVA: Multiple independent variables with more than two groups/units and one dependent variable
- MANOVA: Multiple independent variables with more than two groups/units and Multiple dependent variables

Tukey HSD (honest significant difference) is a post hoc test (follow up to ANOVA) to assess the significance of differences between pairs of group means.  Can create confidence intervals for all pairwise differences between each group's means  

To understand relationship between variables (features) and how to predict the dependent variable can use regression.  

# Nonparametric Data Analysis  
- Chi-square: Use when variables are categorical

# Confidence Interval
68–95–99.7 rule (empirical rule)  
If data is normally distributed then  
- 1 sigma encompasses 68% of the data
- 2 sigma (standard error/significant) encompasses 95% of the data (95% confidence interval)
- 3 sigma encompasses 99.7% of the data  

> A confidence interval (CI) is the range of values that a population parameter is likely to fall between with a certain degree of confidence. CIs that contain either 95% or 99% of expected observations is common. For example, if an estimated value is 50 and the confidence interval of 80% is ±5%, then there is an 80% probability that the true value is between 45 and 55.  

> Another example. If you construct a confidence interval with a 95% confidence level, you are confident that 95 out of 100 times the estimate will fall between the upper and lower values specified by the confidence interval. Your desired confidence level is usually one minus the alpha (α) value you used in your statistical test: Confidence level = 1 − a  So if you use an alpha value of p < 0.05 for statistical significance, then your confidence level would be 1 − 0.05 = 0.95, or 95%.  