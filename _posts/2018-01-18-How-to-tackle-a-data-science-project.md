---
title: 'How to tackle a data science project'
excerpt: "A step by step guide on how to solve data science problems."
comments: true
categories:
  - Data-science
tags:
  - Analytics
---

## Introduction

So you want to be a data scientist, you've done a bit of information gathering to find out the things you need to know to become a data scientist, you’ve learnt most of the technique and tools used for data analytics and modeling now the next thing you want to do is to try your hands on a real data science project to have a feel of what it means to be a data scientist.   
On doing so you realize you are stuck, you don’t know where to start from or how to go about it. This makes you wonder what data scientist actually do. 

Almost every data scientist or data science project strives to extract meaning out of data but the 
procedure for embarking on a data science project is sometimes mysterious to  a beginner or a data science newbie. I once had difficulties in starting a data science project because i had no laid down procedure or methodology in tackling a data science project. In this post I will explain the process used by data scientist in tackling data science projects.

There are several methods or process used by data scientist in the industry. The most popular ones are CRISP-DM, KDD, SEMMA.

{% raw %}<img src="/assets/images/methodology-polls.png" alt="">{% endraw %}

####  Data science methodogoly polls [source](https://www.kdnuggets.com/2014/10/crisp-dm-top-methodology-analytics-data-mining-data-science-projects.html)

  <br>

 You can choose to apply a particular process holistically to your project or adapt any of them to suit your need.  
 Considering all methods mentioned above a common recurring pattern can be extracted  or adapted for use. Almost every data science process has a slight variations of the steps listed below.

* Business Understanding and Problem definition
* Data Collection
* Data Processing
* Data understanding
* Modeling
* Evaluation
* Deployment
* Feedback


Before explaining each step. I would like to bring to mind that you don't necessarily have to follow each step from beginning to end. The deliverables (end result)  of the project should guide you in each step. For instance not all analytics projects require modeling or use of machine learning.  
Eg. some data science projects that are meant for exploratory and understanding purposes can deliver tabular data or visualizations as their end result for communicating their findings.


### Business Understanding and Problem definition

To begin, No data science project is tackled in vacuum or in isolation. Every data science project is undertaken with a domain specific goal in mind, being it a scientific or business domain.

>We will refer to the domain as a business domain henceforth, it makes no difference  

At this stage we try to better understand the  business problem we are trying to solve in order to set the objective of the project from a business perspective.   
You try to answer questions like,

What problem are you trying to solve?  
How do you define success  and how can you measure it?   
What exactly is the client (boss) asking you to solve?   
How can you translate their ambiguous request into a concrete, well-defined problem?  
 
This will help you get to the core of the real problem and also help you formulate the problem as a data science problem as well as the analytic approach to use.

#### Real life.

Imagine you are a data scientist at  an imaginary ecommerce platform called <b>prilla.com</b>, your company sells household products on their website.  
One day your VP of sales asked you to help them out. She then spoke in a very worried tone concerning <b>the huge amount of money spent on marketing and yet the average sales rate is at a minimal value compared to the previous quarter</b>.  

You will realize that the  complaint she gave was not in a direct question form and it might be ambiguous as to what she actually wants.  

Eg. Does she want sales to increase as the amount of money used in marketing increases?  
This could be her expectation because she might aim to capture a larger audience in the marketing campaign thinking that it will result in an increased amount of sales.   

Or does she want to invest minimum amount in marketing and still get  a good amount of sales for that quarter of the year?  

In this scenario she is the domain expert (your client), you will have to collaborate with her and guide her by asking the right questions so that you can come up with a well defined problem.  
Eg. Why is sales stagnant in spite of huge investment in marketing campaigns?  
How can i predict the amount of sales based on the marketing campaigns?  
Which customers will  buy a product upon seeing an ad?  
Which kind of marketing campaign suits our customers?  



### Data Collection

When the business problem is well understood and the  problem is well defined, it's now time to get the data needed to solve the problem. More often the data needed for a data science project is already available in production databases or has already been extracted to a data warehouse. On rare occasions where the data is not available you can collaborate with  domain experts in order to gather the data needed to solve the problem.  
Beware of ethical and legal issues concerning the dataset you will be working with and comply with any set of regulations if needed. Eg. datasets that include some form of identity of people should be anonymized so that it can't be linked back to the  real identity of the people.
### Data Preparation
The data collected does not always come in the expected form so it has to be processed before use.  
Data preparation involves preprocessing and transformation  
### Preprocessing  
Data preprocessing activities usually includes formatting and cleaning :  
<b>Formatting:</b> The data needed to solve the problem might not be in the format that you want. Eg. the data may be in a relational database and you require the data to be in a flat file such as a CSV file. The data needs to be
extracted from the database into a CSV file.  

<b>Cleaning:</b> There may be missing values in the dataset which need to be addressed by filling them or removing the entire row from the dataset. There are strategies for filling missing value such as using the mean or average or any value suggested by the domain expert.
Subset of the data might not be in the right data type and should be converted to the right type.  
Eg. converting an amount value from a string to a double/float type. The labels of categorical data types can also be converted to numeric values such 0 and 1.

### Transformation
This phase of data preparation will be much influenced by the algorithms used in the modeling phase as well as the domain context. Data transformation is sometimes called feature engineering.
Three common data transformations are:  

<b>Scaling:</b> The dataset may have attributes with different scales such as money in Euros, distances in kilometers, weights in grams etc. Most machine learning algorithms work well with datasets that have common scales between 0 and 1. Where 0 is the minimum and 1 is the maximum.
Datasets with different scales need to be rescaled before  applying machine learning algorithms.  
<b>Decomposition:</b> Certain attributes of the dataset are sometimes made up of two attributes fused together, splitting such attributes into separate attributes will help reduce the complexity in the dataset and also benefit the machine learning algorithm applied to the dataset. Eg monthly salary can be split into number of hours and amount paid per hour.  
<b>Aggregation:</b> Sometimes the combination of two attributes will be much more meaningful to the problem you are  solving.  
Eg. You can combine dimensions of a real estate into a single attribute called size.

### Data understanding
After preparing the data, It’s now time to understand the data. This involves computation of useful statistical values and visualization. At this phase you try to identify correlation and trends in the dataset as well as attributes that are much more significant to the problem at hand.

### Modeling
From the information you’ve gathered from the data understanding phase, you will model the data by trying out various models to find out the one that best fits the data. The models can be either descriptive or predictive.  
Modeling a dataset means trying to find a function that best represents the underlying pattern within the data. This will make it possible to predict likely outcomes when presented with a new data instance.

### Evaluation
This is where you check whether the model you selected is a good model for the problem you are solving. In case of predictive analytics, you will check how the model can make predictions when given a new dataset. You usually go back and forth between modeling and evaluation till you are happy with the performance of the selected model.

### Deployment

After selecting the best model, you put the model into production ie. making real use of the model to solve the problem defined. Deployment can range from a report documenting your findings to software systems that make predictions on incoming data from production systems


### Feedback

Deploying the model is not the end of the project. The model has to be checked continuously to see how well it’s performing.The information gathered  is used to evaluate and adjust the model and then redeployed. This phase goes on as long as the model is in production.

### Last words

The steps explained are not to be followed in a  linear manner from start to finish. The process goes through iterations over each phases. The domain context should  guide you in every decision you take and collaborate with domain experts throughout the cycle of the project.





#### Resources
[A Comparative Study of Data Mining Process Models (KDD, CRISP-DM and SEMMA)](http://www.ijisr.issr-journals.org/abstract.php?article=IJISR-14-281-04])

[Polls for methods used by data scientist](https://www.kdnuggets.com/2014/10/crisp-dm-top-methodology-analytics-data-mining-data-science-projects.html)

[How to prepare data for machine learning](https://machinelearningmastery.com/how-to-prepare-data-for-machine-learning/)



