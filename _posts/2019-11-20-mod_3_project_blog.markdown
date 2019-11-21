---
layout: post
title:      "Mod 3 Project Blog"
date:       2019-11-20 20:05:08 -0500
permalink:  mod_3_project_blog
---


In this project we are going to work with the Northwind database to identify and an answer four business driven questions that can be addressed with various hypothesis tests. First we'll set up our working environment and take an initial look at the data, then we'll dive in and work through a series of questions, with a primary focus on the use of discount rates and their effects on sales.

## Project Outline:

1. Setup environment and do initial data inspection
2. Business case and initial questions
3. Question 1
*     State question 
*     Gather and inspect data
*     Form hypothesis
*     Test hypothesis
*     State conclusions
4. Repeat for questions 2, 3, and 4

## Setting up our environment

### The Northwind Database Scehma:

![](https://github.com/rab175/dsc-mod-3-project-online-ds-pt-071519/blob/master/Northwind_ERD_updated.png?raw=true) 

We'll need a number of different libraries to query our database, process data, do statistical analysis, and visualize our results. Also, there are number of things we'll do many times, so I've defined functions in another file and imported them to keep our notebook as clean as possible.

```

# import libraries necessary to do analysis
import pandas as pd
import sqlite3
import numpy as np
from functions import *
import matplotlib.pyplot as plt
import seaborn as sns
import scipy.stats as stats
from statsmodels.stats.power import TTestIndPower
import statsmodels.api as sm
from statsmodels.formula.api import ols
import math
sns.set_style('darkgrid')
%matplotlib inline
```





