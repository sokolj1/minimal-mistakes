---
header:
  image: /assets/internal_structures_2_0.png
  
author_profile: true

classes: wide

---

## Predictive Analytics Case Study: A multi-layer neural network and logisitic regression model is created to help healthcare professionals understand patient cardiovascular health 

Heart disease is described as any condition that affects the heart. Heart disease is often used interchangably with the term "cardiovascular disease." However, cardiovascular disease generally refers to conditions involving heart blockages such as atherosclerosis (narrowing or blocking of the ateries due to plaque buildup), whereas heart disease is an umbrella term that is inclusive of other heart conditions. Heart Disease can not only affect the arteries, but the heart muscle, valves, rhythm, or other important aspects of a well-functioning heart. 

The pervasiveness of heart disease in the United States has made the ailment the leading cause of death in the United States.
Here are a few statistics that emphasize how problematic heart disease has become in our society [[1](https://www.cdc.gov/heartdisease/facts.htm)]: 
- About 610,000 people die of heart disease in the United States every year–that’s 1 in every 4 deaths.
- Heart disease is the leading cause of death for both men and women. More than half of the deaths due to heart disease in 2009 were in men.
- Coronary heart disease (CHD) is the most common type of heart disease, killing over 370,000 people annually.
- Every year about 735,000 Americans have a heart attack. Of these, 525,000 are a first heart attack and 210,000 happen in people who have already had a heart attack.

To illustrate the prevalence of heart disease, choropleth mapping is used to show comparative heart disease trends categorized by state, heart disease stratification, and gender using [Tableau Public](https://public.tableau.com/en-us/s/). Unlike its counterpart, Tableau Desktop, Tableau Public is free. The software is amazing for leveraging powerful interacive data visualizations to embed on a webpage. The CDC's Division of Population Health provides yearly statistics of over 124 chronic health disease indicators that are reported on a city and state level, available to download [here](https://chronicdata.cdc.gov/Chronic-Disease-Indicators/U-S-Chronic-Disease-Indicators-CDI-/g4ie-h725). This dataset was cleaned and filtered using the Python Pandas library, saved as a CSV file, then imported into Tableau Public. Although a step by step data cleaning explanation is beyond the scope of this post, I provide the [iPython Notebook](https://github.com/sokolj1/Predicting-Heart-Disease/blob/master/Heart_Disease_Data_Cleaning_and_Tableau_Viz.ipynb) for those who want to know how I prepared the data. The Tableau workbook can be downloaded by clicking on the bottom right download icon of the frame.

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

The master DataFrame as a total of 375 observations and 63 features.

In order to complete PCA and fit machine learning models, its important to store our target labels in a separate DataFrame. The target labels are the diagnosis of heart disease. The doctors who collected the data determined the presence of heart disease in each patient on an integer value classification scale from 0 to 4: 0 indicates no presence of heart disease, and 4 indicates significant, concerning presence. For this study, we only care about binary classification of heart disease: 0 for no presence, and 1 for presence. 

The following python command converts prediction values of 2,3, and 4 to 1, so we're left with a DataFrame with only binary classification.

{% highlight python %}
target = master_df[['DIAGNOSIS']].reset_index(drop=True).replace([2,3,4], value = [1,1,1])
{% endhighlight %}

Now we're ready to dive into principle component analysis to become more familiar with our data.

## Principle Component Analysis (PCA)

Principle component analysis, or colloquially known as dimensionality reduction, is a statistical procedure that uses eigenvalue decomposition to generalize the most important features in a dataset. PCA simplifies the complexity of high dimensional (many features) data while retaining trends and patterns. For example, the popular beginner machine learning [MNIST Database](http://yann.lecun.com/exdb/mnist/) of handwritten digits has 60,000 observations (rows) and 785 features (columns). [A Kaggler](https://www.kaggle.com/ddmngml/pca-and-svm-on-mnist-dataset) that completed PCA on this dataset effectively reduced the MNIST dimensions from 785 to 16, and retained 59% of the variance, or information that the original dataset conveyed. In a separate trial, he also reduced the dataset from 785 to 49, and retained 82% of the variance. By simplifying the dataset into principle components, we can observe features that contribute more information to the dataset than others, speed up process time if the dimensionality reduced is significant, and visualize trends and patterns of datasets that have many features. 

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

{% highlight python %}
pca_numpy = pca_attr.dropna().values
pca_scaled = StandardScaler().fit_transform(pca_numpy)

pca_2 = PCA(n_components=2)
principalComponents = pca_2.fit_transform(pca_scaled)
principalDf = pd.DataFrame(data = principalComponents, columns = ['principal component 1', 'principal component 2'])

pca_finalDf = principalDf.join(target)
pca_finalDf.head()
{% endhighlight %}

Plot Principle Component 1 as the independent variable, and Principle Component 2 as the dependent variable: 

{% highlight python %}
plt.style.use('ggplot')

fig = plt.figure(figsize = (8,8))
ax = fig.add_subplot(1,1,1) 
ax.set_xlabel('Principal Component 1', fontsize = 15)
ax.set_ylabel('Principal Component 2', fontsize = 15)
ax.set_title('PCA: 2 Components ', fontsize = 20)

targets = [0,1]
colors = ['#006400', '#FF3030']
#CD853F 
for target, color in zip(targets, colors):
    indicesToKeep = finalDf['DIAGNOSIS'] == target
    ax.scatter(finalDf.loc[indicesToKeep, 'principal component 1']
            ,finalDf.loc[indicesToKeep, 'principal component 2']
            ,c = color
            ,s = 50)
ax.legend(targets)
plt.show()

{% endhighlight %}

<img src="/assets/2018-03-08-Predicting-Heart-Disease-with-Neural-Networks/pca_result.png" >

{% highlight python %}
explained_variance = pca_2.explained_variance_ratio_
print(explained_variance) # principle component 1; principle component 2
{% endhighlight %}

<img src="/assets/2018-03-08-Predicting-Heart-Disease-with-Neural-Networks/explained_var.png" >

{% highlight python %}
print(explained_variance.sum())
{% endhighlight %}

<img src="/assets/2018-03-08-Predicting-Heart-Disease-with-Neural-Networks/explained_var_sum.png" >

##  Building a Neural Network with Keras

Neural networks, also referred to as deep learning in a broad sense, is a biologically inspired machine learning technique that enables a computer to build a complicated array of nodes that provide solutions to seemingly impossible tasks. 

<figure class="half">
    <img src="/assets/2018-03-08-Predicting-Heart-Disease-with-Neural-Networks/neuron_2.png" >
    <img src="/assets/2018-03-08-Predicting-Heart-Disease-with-Neural-Networks/neural_net2.jpg" >
    <figcaption> Left: Biological inspiration for Neural Networks; Right: Example of a forward feeding Neural Network. Source: <a href = "http://cs231n.github.io/neural-networks-1/">Stanford Vision & Learning Lab</a></figcaption>
</figure>


{% highlight python %}
# import necessary keras packages for Deep Learning
from keras.layers import Dense, Dropout
from keras.models import Sequential
from keras.utils import to_categorical
from keras.callbacks import EarlyStopping
from sklearn.model_selection import train_test_split

nn_attr = master_df2.loc[:,['age','sex','cp_type','resting_blood_pressure','hypertension',
'cholesterol','cigarettes_per_day','years_as_smoker', 'fasting_blood_sugar', 'hist_heart_dis','resting_hr','max_hr_ach','tpeakbps', 'exer_ind_angina', 'rldv5e']]

n_cols = nn_attr.shape[1]
{% endhighlight %}

{% highlight python %}
nn_attr_scaled = StandardScaler().fit_transform(nn_attr)

X_train, X_test, y_train, y_test = train_test_split(
nn_attr_scaled, target, train_size=0.80, random_state=42)
X_train.shape

y_train = to_categorical(y_train)
y_test = to_categorical(y_test)

{% endhighlight %}

### Model Architecture

{% highlight python %}


{% endhighlight %}


### Overfitting Prevention Practices

- L2 Regularization 
- Dropout 
- Input noise 




## Alternative Machine Learning Techniques



## Ensembling/Stacking 

<video width="480" height="320" controls="controls">
  <source src="assets/2018-03-08-Predicting-Heart-Disease-with-Neural-Networks/heart_disease_video_final.mp4" type="video/mp4">
</video>

<video width="480" height="320" controls="controls">
  <source src="assets/2018-03-08-Predicting-Heart-Disease-with-Neural-Networks/heart_disease_test.mp4" type="video/mp4">
</video>

 















