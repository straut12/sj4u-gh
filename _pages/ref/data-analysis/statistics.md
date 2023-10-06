---
layout: page
permalink: /ref/data-analysis/statistics/
title: statistics
description: Statistics
nav: false
toc: true
---

Nominal scale variables are labeled with no specific order. 
Ordinal scale the variables are in a specific order (ie, low income, mid income, high income or less than 50, 50-100, over 100 or dislike, neutral, like etc)  

Continuous vs Discrete  
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

Parametric vs Nonparametric test  
- Parametric test: assumes data follows a certain distribution. Requires previous knowledge about the population.  
- Nonparametric test: does not make assumptions and can be applied to any type of data (ordinal or nominal)  

Parametric Data Analysis
- t-test for mean comparison: One independent variable with two groups/units (two bake plates, two coaters, etc) and one dependent variable (CD, thickness)
ANOVA will test the equality of three or more group means  
- ANOVA: One independent variable with more than two groups/units and one dependent variable 
- Two-way ANOVA: Multiple independent variables with more than two groups/units and one dependent variable
- MANOVA: Multiple independent variables with more than two groups/units and Multiple dependent variables

Tukey HSD (honest significant difference) is a post hoc test (follow up to ANOVA) to assess the significance of differences between pairs of group means.  Can create confidence intervals for all pairwise differences between each group's means  

To understand relationship between variables (features) and how to predict the dependent variable can use regression.  

Nonparametric Data Analysis  
- Chi-square: Use when variables are categorical
