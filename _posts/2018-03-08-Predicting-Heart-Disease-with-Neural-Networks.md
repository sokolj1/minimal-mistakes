---
header:
  image: /assets/internal_structures_2_0.png
  
author_profile: true

classes: wide

---

## Predictive Analytics Case Study: Multi-layer neural network using Keras to help healthcare professionals understand patient cardiovascular health 

Heart disease is described as any condition that affects the heart. Heart disease is often used interchangably with the term "cardiovascular disease." However, cardiovascular disease generally refers to conditions involving heart blockages such as atherosclerosis (narrowing or blocking of the ateries due to plaque buildup), whereas heart disease is an umbrella term that is inclusive of other heart conditions. Heart Disease can not only affect the arteries, but the heart muscle, valves, rhythm, or other important aspects of a well-functioning heart. 

The pervasiveness of heart disease in the United States has made the ailment the leading cause of death in the United States.
Here are a few statistics that emphasize how problematic heart disease has become in our society [[1](https://www.cdc.gov/heartdisease/facts.htm)]: 
- About 610,000 people die of heart disease in the United States every year–that’s 1 in every 4 deaths.
- Heart disease is the leading cause of death for both men and women. More than half of the deaths due to heart disease in 2009 were in men.
- Coronary heart disease (CHD) is the most common type of heart disease, killing over 370,000 people annually.
- Every year about 735,000 Americans have a heart attack. Of these, 525,000 are a first heart attack and 210,000 happen in people who have already had a heart attack.

To illustrate the prevalence of heart disease, choropleth mapping is used to show comparative heart disease trends categorized by state, heart disease stratification, and gender using [Tableau Public](https://public.tableau.com/en-us/s/). Unlike its counterpart, Tableau Desktop, Tableau Public is free. The software is amazing for leveraging powerful interacive data visualizations to embed on a webpage. The CDC's Division of Population Health provides yearly statistics of over 124 chronic health disease indicators that are reported on a city and state level, available to download [here](https://chronicdata.cdc.gov/Chronic-Disease-Indicators/U-S-Chronic-Disease-Indicators-CDI-/g4ie-h725). This dataset was cleaned and filtered using the Python Pandas library, saved as a CSV file, then imported into Tableau Public. Although a step by step data cleaning explanation is beyond the scope of this post, I provide the [iPython Notebook](https://github.com/sokolj1/Predicting-Heart-Disease/blob/master/Heart_Disease_Data_Cleaning_and_Tableau_Viz.ipynb) for those who want to know how I prepared the data. 

<iframe src = "https://public.tableau.com/views/PrevalenceofHeartDiseaseintheUnitedStates20145_0/Dashboard1?:showVizHome=no&:embed=true" width="95%" height="600"></iframe>


## Predictive Analytics Case Study: Heart Disease

As illustrated using the choropleth map, heart disease is a major issue accross every state and gender in the United States. Treatment for heart disease cost the healthcare industry over 444 _billion_ dollars in 2010 [[2](https://www.webmd.com/healthy-aging/features/heart-disease-medical-costs#1)], so an assumption can be made that present day cost is substantially higher than the 2010 dollar amount. Although it is imperative the healthcare industry provides high quality treatment for patients diagnosed with health disease, this industry must ameliorate the issue by reducing the number of future patients diagnosed with heart disease via *preventative care*.

Preventative care implements healthcare prophylaxis measures to prevent people from developing various diseases. The key advantage from a healthcare industry perspective is the lower cost to employ preventative care compared to disease treatments. This is especially applicable to heart disease. Heart disease preventative care begins with determining the likelihood of developing heart disease based upon a variety of quantitative and qualitative attributes. The different attributes are assigned weights; the greater the weight, the more impactful the attribute is on disease prediction. The healthcare professional draws from years of formal education and medical training to interpret these attributes to holistically determine the best course of action for the patient: 

 - Small likelihood of developing heart disease: No prophylaxis measures necessary 
 - Plausible likelihood of developing heart disease: Prophylaxis recommended 
 - Very likely heart disease has developed: Run additional tests to confirm severity of heart disease
 
Although healthcare professionals invoke incredible decision making abilities upon rendering a diagnosis, data science can help healthcare professionals regarding patient diagnosis for heart disease. The healthcare industry collects a substantial amount of data for each individual patient with appointments, office/lab tests, and medical history. With mainstream adoption of artifical intelligence and machine learning, the same attributes that healthcare professionals use to traditionally diagnose a patient with likelihood of heart disease along with additional attributes can be leveraged to create machine learning models that help nurses, physician assistants, and medical doctors understand their patient's health and make better decisions. By implementing a heart disease prediction system by [stacking](http://blog.kaggle.com/2016/12/27/a-kagglers-guide-to-model-stacking-in-practice/) machine learning techniques, this system will "learn" or fit from labeled data, resulting in a machine learning machine that can probabilistically predict the likelihood that a patient will be diagnosed with heart disease. 

## Data Source: University of California Irvine (UCI) Machine Learning Repository

The data for this case study was obtained from UCI's machine learning repository. The dataset is compartmentalized into four txt files by donating city: 
- Cleveland, Ohio, United States
- Long Beach, California, United States
- Budapest, Hungary 
- Zurich/Basel, Switzerland

Considering the focus on heart disease in the United States, only the Cleveland and Long Beach files were used for this study. After rigorous data cleaning, both files were converted into csv files and imported into a Pandas Python DataFrame: 

{% highlight python %}
# import pertinent Python data manipulation and visualization libraries
import pandas as pd
import numpy as np
import matplotlib.pylab as plt
import seaborn as sns

# read in cleveland and long beach datasets
cleveland = pd.read_csv('cleveland_clean_3.csv', sep = ',', header = 2)
longbeach = pd.read_csv('long-beach-va_storage_1.csv', sep = ',', header = 2)

# concatenate both datasets into one Pandas DataFrame; observe first 5 rows
master_df = pd.concat([cleveland,longbeach])
master_df.head()
{% endhighlight %}

<img src="/assets/2018-03-08-Predicting-Heart-Disease-with-Neural-Networks/master_df.png" >

The master DataFrame as a total of 375 observations and 63 attributes.

In order to complete PCA and fit machine learning models, its important to store our target labels in a separate DataFrame. The target labels are the diagnosis of heart disease. The doctors who collected the data determined the presence of heart disease in each patient on an integer value classification scale from 0 to 4: 0 indicates no presence of heart disease, and 4 indicates significant, concerning presence. For this study, we only care about binary classification of heart disease: 0 for no presence, and 1 for presence. 

The following python command converts prediction values of 2,3, and 4 to 1, so we're left with a DataFrame with only binary classification.

{% highlight python %}
target = master_df[['DIAGNOSIS']].reset_index(drop=True).replace([2,3,4], value = [1,1,1])
{% endhighlight %}

## Principle Component Analysis 

PCA is not ideal for non-continuous, discrete dataset attributes. Therefore, continous values must be used for PCA analysis: 

1. Age
2. Resting blood pressure 
3. Cholesterol 
4. Cigarettes per day
5. Years as smoker 
6. Resting heart rate 
7. Maximum heart rate achieved 
8. Peak exercise blood pressure (Part 1)
9. Height of peak exercise ST measurement

We need to filter the master DataFrame to a new DataFrame that consists of just these attributes: 

{% highlight python %}
pca_attr = master_df.loc[:,['age','resting_blood_pressure','cholesterol',
'cigarettes_per_day','years_as_smoker','resting_hr','max_hr_ach','tpeakbps', 'rldv5e']]
{% endhighlight %}

And visualize the correlation between each attribute by creating a Pearson correlation matrix. The correlation values range from -1 to 1, with -1 indicating an unequivocal negative correlation, 0 indicating no correlation, and 1 indicating an unequivocal positive correlation. 

{% highlight python %}

colormap = plt.cm.RdBu
plt.figure(figsize=(14,12))
plt.title('Heart Disease Pearson Correlation of Features', y = 1.05, size = 15)
sns.heatmap(pca_attr.astype(float).corr(),linewidths = 0.1,vmax = 1.0, 
square = True, cmap = colormap, linecolor = 'white', annot = True)

plt.show()
{% endhighlight %}

<img src="/assets/2018-03-08-Predicting-Heart-Disease-with-Neural-Networks/heart_dis_pearsoncor.png" >

A Notable positive correlations amongst the data are cigarettes per day as years as a smoker also increases. 

{% highlight python %}
# import necessary Python libraries for PCA 
from sklearn.decomposition import PCA
from sklearn.preprocessing import StandardScaler
from sklearn import preprocessing
{% endhighlight %}


##  Building a Neural Network with Keras




## Alternative Machine Learning Techniques



## Ensembling/Stacking 



 















