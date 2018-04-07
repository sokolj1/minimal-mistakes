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

To illustrate the prevalence of heart disease, choropleth mapping is used to show comparative heart disease trends categorized by state, heart disease stratification, and gender using [Tableau Public](https://public.tableau.com/en-us/s/). Unlike its counterpart, Tableau Desktop, Tableau Public is free. The software is amazing for leveraging powerful interacive data visualizations to embed on a webpage. The CDC's Division of Population Health provides yearly statistics of over 124 chronic health disease indicators that are reported on a city and state level, available to download [here](https://chronicdata.cdc.gov/Chronic-Disease-Indicators/U-S-Chronic-Disease-Indicators-CDI-/g4ie-h725). This dataset was cleaned and filtered using the Python Pandas library in a Jupyter Notebook, saved as a CSV file, then imported into Tableau Public. Although a step by step data cleaning explanation is beyond the scope of this post, I provide the [iPython Notebook](https://github.com/sokolj1/Predicting-Heart-Disease/blob/master/Heart_Disease_Data_Cleaning_and_Tableau_Viz.ipynb) for those who want to know how I prepared the data. 

<iframe src = "https://public.tableau.com/views/PrevalenceofHeartDiseaseintheUnitedStates20145_0/Dashboard1?:showVizHome=no&:embed=true" width="95%" height="600"></iframe>


## Predictive Analytics Case Study: Heart Disease

As illustrated using the choropleth map, heart disease is a major issue accross every state and gender in the United States. Treatment for heart disease cost the healthcare industry over 444 _billion_ dollars in 2010 [[2](https://www.webmd.com/healthy-aging/features/heart-disease-medical-costs#1)], so an assumption can be made that present day cost is substantially higher than the 2010 dollar amount. Although it is imperative the healthcare industry provides high quality treatment for patients diagnosed with health disease, this industry must ameliorate the issue by reducing the number of future patients diagnosed with heart disease via *preventative care*.

Preventative care implements healthcare prophylaxis measures to prevent people from developing various diseases. The key advantage from a healthcare industry perspective is the lower cost to employ preventative care compared to disease treatments. This is especially applicable to heart disease. Heart disease preventative care begins with determining the likelihood of developing heart disease based upon a variety of quantitative and qualitative attributes. The different attributes are assigned weights; the greater the weight, the more impactful the attribute is on disease prediction. The healthcare professional draws from years of formal education and medical training to interpret these attributes to holistically determine the best course of action for the patient: 

 - Small likelihood of developing heart disease: No prophylaxis measures necessary 
 - Plausible likelihood of developing heart disease: Prophylaxis recommended 
 - Very likely heart disease has developed: Run additional tests to confirm severity of heart disease
 
Although healthcare professionals invoke incredible decision making abilities upon rendering a diagnosis, I believe data science can help healthcare professionals regarding patient diagnosis for heart disease. The healthcare industry collects a substantial amount of data for each individual patient with appointments, office/lab tests, and medical history. With mainstream adoption of artifical intelligence and machine learning, the same attributes that healthcare professionals use to traditionally diagnose a patient with likelihood of heart disease along with additional attributes can be leveraged to create machine learning models that help nurses, physician assistants, and medical doctors understand their patient's health and make better decisions. By implementing a heart disease prediction system by [stacking](http://blog.kaggle.com/2016/12/27/a-kagglers-guide-to-model-stacking-in-practice/) machine learning techniques, this system will "learn" or fit from labeled data, resulting in a machine learning machine that can probabilistically predict the likelihood that a patient will be diagnosed with heart disease. 

## Data Source: University of California Irvine (UCI) Machine Learning Repository

The data for this case study was obtained from UCI's machine learning repository. The dataset is compartmentalized into four txt files by donating city: 
- Cleveland, Ohio, United States
- Long Beach, California, United States
- Budapest, Hungary 
- Zurich/Basel, Switzerland

Considering the focus on heart disease in the United States, only the Cleveland and Long Beach txt files were used for this study. After rigorous data cleaning, the Cleveland and Long Beach files were converted into csv files and imported into a Pandas Python DataFrame.



then concatenated together

The dataset has a total of 76 attributes, although many of these attributes do not have valid data populated in the appropriate fields, so these were dropped from the 





 















