---
layout: post
title:      "Applying a model to a business problem"
date:       2020-05-25 23:32:03 -0400
permalink:  applying_a_model_to_a_business_problem
---


Data science and machine learning models are awesome. So are cool visualizations. But, they're only as valuable as the insights they provide to help answer the essential business questions that drive them. In this post I'll walk through my attempts to apply the classification models I developed in the module 5 project, and highlight how they may help a business make an important decision, and what a threshhold might be for applying that type of project in the real world. 

**To read the overview for my Mod-5 Bank Marketing Classifier project see here:** 

https://rab175.github.io/mod_5_project_-_bank_marketing_classifier

As brief description, the goal for the project was to develop a model that predicts the success of a bank marketing campaign based on the features we have in a labeled dataset. This model should help better identify potential customers and refine the focus of future marketing campaigns.

We worked through dozens of iterations of modeling techniques and feature transformations to finally arrive at a model that that reliably predicted whether or not a customer would succesfully be aquired in a telemarketing campaign. The next, and more important question, was what do we do with this information? 

![Customer Succes per n Calls](https://drive.google.com/open?id=1QbvjEGzU1zcpaVAv_bexS175G_q04IvX)

One thing we could do is seek to conserve our resources by only calling those customers most likely to convert (saving on call center time) and reducing the number of calls we make to customers that are unlikely. As we see above, there appear to be diminishing returns on the number of calls we make to a custome. What if we cut all those calls out of our process becasue we had a high degree of confidence that they were wated time anyway?

Well, to answer that question we need to break down the problem into the cost of calling a customer that fails to covert, and the amount of revenue we hope to gain from a successful call. The second part is the most important. That's because as accurate as our model may be, we know it won't be perfect, and if we choose not to follow up with a high potential customer due to our model recommending against it, that's an opportunity cost we'll incure. In short, the amount we save from not making too many unproductive calls must be greater than the amount of revenue we may miss by failing to call a good customer.

So to determine this we set up an exercise. 

To estimate our model efficiency we will make a few **assumptions** for the sake of simplicity:

1. In any campaign we will call all customers **at least one time**
    * by applying our model we aim to **prioritize higher potential customers with a second call**
2. **Our success rate of 11%** (from the model - see previous post) applies to any sample of potential customers
    * our recall multiplied by the success rate then tells us how many True Positives and False Negatives we may expect
    * our precision will help us determine how many total positive predictions we may make

3. **False Positives** will be calls we shouldn't have made twice
4. **False Negatives** will represent missed revenue opportunities
5. **Potential Savings should exceed missed revenue due to our False Negative Rate**

    * we'll attempt to account for 1-call success by applying the success rate to our False Negatives

As a first step, let's determine what our the baseline revenue we can expect is.

```
# set the success rate and revenue per customer assumptions
success_rate = .11
rev_p_cust = 1650

# calculate the number of 'yes' customers we can expect based on the success rate
n_y_customers = round(n_customers * success_rate, 0)

# calculate baseline revenue we can expect based on succes rate and rev per customer
baseline_revenue = n_customers * success_rate * rev_p_cust

print(f'In a {n_customers} person campaign we can expect ${baseline_revenue} revenue' )
print(f'At a 11% success rate we can expect our campaign to yield {n_y_customers} customers')
```

In a 200000 person campaign we can expect $36300000.0 revenue.

At a 11% success rate we can expect our campaign to yield 22000.0 customers.

* If we convert 11% of potential customers, we can expect \$36.3 million in revenue, and 22,000 customers
* We know that the baseline costs of our unoptimized campaign are between \$1.4 and \$2.9 million

```
# write a formula that will fit a model and return the number of expected 
# True Positive, False Negative, and Predicted Postive values based on 
# recall and precision, and the number of customers expected to say 'yes'
# based on success rate
def predicted_values(n_y_customers, model, data):
    
    y_test = data['y_test']
    X_test = data['X_test']
    
    preds = model.predict(X_test)
    
    recall = recall_score(y_test, preds)
    precision = precision_score(y_test, preds)
    
    est_T_positive = round(n_y_customers * recall, 0)
    est_F_negative = n_y_customers - est_T_positive
    
    total_pos = round(est_T_positive/precision, 0)
    
    return est_T_positive, est_F_negative, total_pos
```


For a 200,000 person campaign with an assumed 11% success_rate, our LogReg model (best model):
	 * would predict 52090.0 total positives
	 * of them, 14430.0 would likly be successful customers
	 * the model would likely miss up to 7570.0 successful customers

More follows. See like to the full analysis in the repo linked below. 

But, in sort again, it turns out making calls to potential customers is a very low cost activity, but the lifetime expected revenue of a new banking customer is fairly high. Much higher. 

Conclusions:

* At this time our models do not perform well enough to implement them in the way we have explored
* Although on the high end our models may allow us to significantly reduce costs:
    * Our baseline campaign cost is as much as: \$2,890,000
    * The logreg model could save up to: \$1,629,550
    * The random forest model could save up to \$1,758,475
* However, the potential drop in revenue is not offset:

    * Baseline revenue expectations are as high as \$35,200,000
    * The logreg model may allow us to miss up to: \$-10,016,545
    * The random forest model may allow us to miss up to \$-14,107,786
* Due to it's better precision Random Forest was able to lower costs more significantly, but it also had a much higher False Negative rate, which given our scenario is ultimately more costly
* 
* Logistic Regression appears to be the method of choice for this problem. Although we do not have it as tuned as we would like it to be, it consistently had the highest recall and therefore lowest false negative rate 


[Github repo](https://github.com/rab175/dsc-mod-5-project-online-ds-pt-071519/blob/master/Mod5_Project_Bank_Marketing_Classifier.ipynb)

