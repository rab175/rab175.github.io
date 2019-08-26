---
layout: post
title:      "Visualizing Categorical Variables with Seaborn "
date:       2019-08-25 23:23:57 -0400
permalink:  visualizing_categorical_variables_with_seaborn
---


This blog is about seaborn






```
# To better learn seaborn I created a funciton  to take a list of values and plot a series of graphs and charts
# function is meant to take a list of independent variables, a dependent variable, and a daraframe that contains both
def seaborn_plotter(independent_vs, dependent_v, df):

#Loop through the independent variables, putting a divider between each iteration with the variable name
    for i in independent_vs:
        print ("Kings County Data Set - Exploratory Data Analysis for: " + i)
        print ("-------------------------------------------------------------------------------------")
        
        #create subplots for each visual 
        plt.figure(figsize=(12,12))
        ax1 = plt.subplot(2,2,1)
        ax2 = plt.subplot(2,2,2)
        ax3 = plt.subplot(2,2,3)
        ax4 = plt.subplot(2,2,4)
        
        #seaborn boxplot and set labels - shows range, quartiles, and median for each category value        
        a = sns.boxplot(x=i, y=dependent_v, data=df, ax=ax3)
        a.axes.set_title('Boxplot' ,fontsize=20)
        a.set_xlabel(i,fontsize=15)
        a.set_ylabel('Price', fontsize=15)
        a.tick_params(labelsize=10)
        
        #seaborn countplot and set labels - counts number of each category occuring 
        b = sns.countplot(df[i], ax=ax4)
        b.axes.set_title('Variable Count' ,fontsize=20)
        b.set_xlabel(i,fontsize=15)
        b.set_ylabel('Count', fontsize=15)
        b.tick_params(labelsize=10)
        
        #seaborn scatterplot and set labels - shows relationship of category and price
        c = sns.scatterplot(x=i, y=dependent_v, data=df, ax=ax1)
        c.axes.set_title('Scatterplot' ,fontsize=20)
        c.set_xlabel(i,fontsize=15)
        c.set_ylabel('Price', fontsize=15)
        c.tick_params(labelsize=10)
        
        #seaborn distplot and set labels - plots distribution of category values
        d = sns.distplot(df[i], ax=ax2)
        d.axes.set_title('Histogram',fontsize=20)
        d.set_xlabel(i,fontsize=15)
        d.set_ylabel('KDE', fontsize=15)
        d.tick_params(labelsize=10)
        
        plt.show()
        
        #Input an observation for each variable         
        input('Observations: ')
        print("\n")
        input('Approach: ')
        print("\n")
```

![](https://drive.google.com/open?id=1J1CYa_Q3iekkEwHWPL_WBemJpdhqU6Jt)


