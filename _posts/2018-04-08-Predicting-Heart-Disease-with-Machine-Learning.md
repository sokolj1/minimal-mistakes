---
header:
  image: /assets/2018-03-08-Predicting-Heart-Disease-with-Neural-Networks/TabPy Dashboard.png
  
author_profile: true

classes: wide

---

## A logisitic regression model is fitted, then deployed via TabPy to help healthcare professionals understand patient cardiovascular health

Heart disease is described as any condition that detrimentally affects the heart. Heart disease is often used interchangeably with the term "cardiovascular disease." However, cardiovascular disease generally refers to conditions involving heart blockages such as atherosclerosis (narrowing or blocking of the arteries due to plaque buildup), whereas heart disease is an umbrella term that is inclusive of other heart conditions. Heart Disease can not only affect the arteries, but the heart muscle, valves, rhythm, or other important aspects of a well-functioning heart. 

The pervasiveness of heart disease in the United States has merited the ailment as the leading cause of death in the United States.
Here are a few statistics that emphasize how problematic heart disease has become in our society [[1](https://www.cdc.gov/heartdisease/facts.htm)]: 
- About 610,000 people die of heart disease in the United States every year. That is 1 in every 4 deaths.
- Heart disease is the leading cause of death for both men and women. More than half of the deaths due to heart disease in 2009 were in men.
- Coronary heart disease (CHD) is the most common type of heart disease, killing over 370,000 people annually.
- Every year about 735,000 Americans have a heart attack. Of these, 525,000 are a first heart attack and 210,000 happen in people who have already had a heart attack.

To illustrate the prevalence of heart disease, choropleth mapping is used to show comparative heart disease trends categorized by state, heart disease stratification, and gender using [Tableau Public](https://public.tableau.com/en-us/s/). Unlike its counterpart, Tableau Desktop, Tableau Public is free. The software is amazing for leveraging powerful interactive data visualizations to embed on a webpage. The CDC's Division of Population Health provides yearly statistics of over 124 chronic health disease indicators that are reported on a city and state level, available to download [here](https://chronicdata.cdc.gov/Chronic-Disease-Indicators/U-S-Chronic-Disease-Indicators-CDI-/g4ie-h725). This dataset was cleaned and filtered using the Python Pandas library, saved as a CSV file, then imported into Tableau Public. Although a step by step data cleaning explanation is beyond the scope of this post, I provide the [iPython Notebook](https://github.com/sokolj1/Predicting-Heart-Disease/blob/master/Heart_Disease_Data_Cleaning_and_Tableau_Viz.ipynb) for those who want to know how I prepared the data. The Tableau workbook can be downloaded by clicking on the bottom right download icon of the frame.

<iframe src = "https://public.tableau.com/views/PrevalenceofHeartDiseaseintheUnitedStates20145_0/Dashboard1?:showVizHome=no&:embed=true" width="95%" height="600"></iframe>


## Predictive Analytics Case Study: Heart Disease

As illustrated using the choropleth map, heart disease is a major issue across every state and gender in the United States. Treatment for heart disease cost the healthcare industry over 444 _billion_ dollars in 2010 [[2](https://www.webmd.com/healthy-aging/features/heart-disease-medical-costs#1)]. An assumption can be made that present day cost is substantially higher than the 2010 dollar amount. Although it is imperative that the healthcare industry provides high quality treatment for patients diagnosed with health disease, this industry must ameliorate the issue by reducing the number of future patients diagnosed with heart disease via *preventative care*.

Preventative care implements healthcare prophylaxis measures to prevent people from developing various diseases. The key advantage from a healthcare industry perspective is the lower cost to employ preventative care compared to disease treatments. This is especially applicable to heart disease. Heart disease preventative care begins with determining the likelihood of developing heart disease based upon a variety of quantitative and qualitative attributes. The different attributes are assigned weights; the greater the weight, the more impactful the attribute is on disease prediction. The healthcare professional draws from years of formal education and medical training to interpret these attributes to holistically determine the best course of action for the patient: 

 - Small likelihood of developing heart disease: No prophylaxis measures necessary 
 - Plausible likelihood of developing heart disease: Prophylaxis recommended 
 - Very likely heart disease has developed: Run additional tests to confirm severity of heart disease
 
Although healthcare professionals invoke incredible decision making abilities upon rendering a diagnosis, data science can help healthcare professionals regarding patient diagnosis for heart disease. The healthcare industry collects a substantial amount of data for each individual patient with appointments, office/lab tests, and medical history. With mainstream adoption of artificial  intelligence and machine learning, the same attributes that healthcare professionals use to traditionally diagnose a patient with likelihood of heart disease along with additional attributes can be leveraged to create machine learning models that help nurses, physician assistants, and medical doctors understand their patient's health and make better decisions. By implementing a heart disease prediction system using machine learning techniques, this system will "learn" or fit from labeled data, creating a model that can probabilistically predict the likelihood of a patient having heart disease

Once the machine learning model is fitted, the Python code can be deployed to Tableau using TabPy. This API encapsulates the model in a graphical user interface. The end user can modify patient data in Tableau parameter fields. The parameters serve as arguments for the backend machine learning Python functions. After user input, the model returns a binary prediction (no presence or presence of heart disease) in addition to the probability of the latter being true. 

## Data Source: University of California Irvine (UCI) Machine Learning Repository

The data for this case study was obtained from the UCI Machine Learning Repository. The dataset is compartmentalized into four text files by donating city: 
- Cleveland, Ohio, United States
- Long Beach, California, United States
- Budapest, Hungary 
- Zurich/Basel, Switzerland

Considering the focus on heart disease in the United States, only the Cleveland and Long Beach files were used for this study. After rigorous data cleaning, both files were converted into csv files and imported into a Pandas Python DataFrame: 

<img src="/assets/2018-03-08-Predicting-Heart-Disease-with-Neural-Networks/master_df.png" >

The master DataFrame has a total of 375 observations and 63 features.

In order to complete PCA and fit machine learning models, it's important to store our target labels in a separate DataFrame. The target labels are the diagnosis of heart disease. The doctors who collected the data determined the presence of heart disease in each patient on an integer value classification scale from 0 to 4: 0 indicates no presence of heart disease, and 4 indicates significant, concerning presence. For this study, we only care about binary classification of heart disease: 0 for no presence, and 1 for presence. 

## Selecting the Features for Analysis

Feature selection is one of the most important aspects of machine learning. These features will serve as the foundation of your model; the right features should be chosen with considerable research and rationale for each decision. For our example, the data scientist should consult healthcare professionals such as cardiologists and ECG/EKG technicians to determine the features that reliably diagnosis heart disease. 

Unfortunately, by using an already aggregated dataset from a repository, only the data present is useable. The main factors that influenced my decision for selection of each feature was the following: 
- Presence of clean data, as the dataset is dirty beyond belief. For instance, a few features that were viable in the Cleveland dataset had missing/invalid values in the Long Beach dataset. 
- Using heart disease risk factor knowledge derived from my undergraduate studies and working as a Pharmacy Technician. I know classic indicators such as blood pressure, cholesterol, and family history are important features to include for the model. Lifestyle choices also have a major influence on heart health, so I choose several exercise and smoking metrics to include in the study. 
- Features should have no presence or minimal degree of multicollinearity. Considering logistic regression is a generalized linear model (GLM), multicollinearity may result in inconsistent parameter estimates. So fitting the training data can yield vastly different optimized parameters (weights) each time the model is fitted. This can lead to huge variations in the out of sample error, making the model's predictive power too inconsistent for viability. 
- Features should not be redundant. The dataset has many instances of continuous and discrete measures for features that are the same i.e cigarettes per day (0 - 100 cigarettes) and smoker? (1 or 0), resting blood pressure (90 to 200 mm Hg) and hypertension (1 or 0). So for these examples, I kept the continous features and left out the redundant binary features. 
Ultimately, I narrowed down the selection to 14 features: 10 continuous and 4 binary. 

|   |Feature                          | Type              |     
|---|                       ---       |     ---           |
| 1 | Age                             | continuous        |
| 2 | Resting blood pressure          | continuous        |
| 3 | Cholesterol                     | continuous        |
| 4 | Cigarettes per day              | continuous        |
| 5 | Years as smoker                 | continuous        |
| 6 | Resting heart rate              | continuous        |
| 7 | Maximum heart rate achieved     | continuous        |
| 8 | Metabolic Equivalent of Task (METs)  | continuous   |
| 9 | Peak Exercise blood pressure (tpeakbps)| continuous |
| 10| Exercise Stress Test (rldv5e)     | continuous   |
| 11| Fasting blood sugar               | binary       |
| 12| History of heart disease          | binary       |
| 13| Exercise Induced Angina           | binary       |
| 14| Sex                               | binary       |

Now we're ready to dive into principle component analysis to become more familiar with our data.

## Principle Component Analysis (PCA)

Principle component analysis, or colloquially known as dimensionality reduction, is a statistical procedure that uses eigenvalue decomposition to generalize the most important features in a dataset. PCA simplifies the complexity of high dimensional (many features) data while retaining trends and patterns. For example, the popular beginner machine learning [MNIST Database](http://yann.lecun.com/exdb/mnist/) of handwritten digits has 60,000 observations (rows) and 785 features (columns). [A Kaggler](https://www.kaggle.com/ddmngml/pca-and-svm-on-mnist-dataset) completed PCA on this dataset. He effectively reduced the MNIST dimensions from 785 to 16, and retained 59% of the variance, or information that the original dataset conveyed. In a separate trial, he also reduced the dataset from 785 to 49, and retained 82% of the variance. By simplifying the dataset into principle components, we can observe features that contribute more information to the dataset than others, speed up process time if the dimensionality reduced is significant, and visualize trends and patterns of datasets that have many features. 

PCA is not ideal for non-continuous, discrete dataset attributes. Therefore, the 10 continuous values out of the total 14 are the only attributes that can be used for PCA analysis. 

We will filter the master DataFrame to a new DataFrame that consists of just these continous attributes, then visualize the correlation between each attribute by creating a Pearson correlation matrix. The correlation values range from -1 to 1, with -1 indicating a negative correlation, 0 indicating no correlation, and 1 indicating a positive correlation. 

<img src="/assets/2018-03-08-Predicting-Heart-Disease-with-Neural-Networks/hd_pearson_correlation.png" >

Notable positive correlations amongst the data are cigarettes per day as years as a smoker increases, and METs as maximum heart rate achieved increases. 

We'll now dive into the PCA code: 
{% highlight python %}
# import necessary Python libraries for PCA 
from sklearn.decomposition import PCA
from sklearn.preprocessing import StandardScaler
from sklearn import preprocessing

# drop NA values and convert from type DataFrame to Numpy array
pca_numpy = pca_attr.dropna().values
{% endhighlight %}

In order to obtain the most accurate PCA analysis results, the data needs to be _scaled_. This critical step is one of the most important aspects of data preprocessing. If scaling is overlooked, then features that have higher quantitative values will influence the results far more than the other features. There are several ways to scale data, but the most common is to subtract each observation by the overall feature mean, then divide the difference by the feature standard deviation. 

{% highlight python %}
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

<img src="/assets/2018-03-08-Predicting-Heart-Disease-with-Neural-Networks/heart_dis_pca_results.png" >

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




Considering all of our data is standardized, we can divide our data into a 'train' dataset and a 'test' dataset. To legitimately evaluate the efficacy of the fitted, or trained model, we must use data that did NOT train the logistic regression model. The data science community calls the unfitted data 'unseen.' The rule of thumb is to divide the master DataFrame into separate 70-75% train and 20-25% test DataFrames. There are manual ways to Pythonically complete this task, but using the popular machine learning library sci-kit learn, the function train_test_split does all the heavy lifting:

{% highlight python %}
X_train, X_test, y_train, y_test = train_test_split(
nn_attr_scaled, target, train_size=0.80, random_state=42)
{% endhighlight %}

We also need to reshape the diagnosis (label) dataset. The _to_categorical_ function converts a class vector to a binary class matrix. For example, instead of the y_train and y_test datasets consisting of n rows and 1 column of 1's and 0's depending on the diagnosis, _to_categorical_ reshapes these DataFrames to n rows and 2 columns, one for each class: 


## Logistic Regression

Model accuracy above 70% accuracy is considered to be a decent classifier. For this particular study where the model is trained with a limited amount of observations, there is a case that 78% accuracy is excellent. I recently coded logistic regression from stratch in R. Knowing logistic regression is a binary classifier and considering the purpose of this post is an introduction to machine learning, I built a logistic regression model with the same features from the heart disease dataset.

### Training logistic regression using k-folds method

K-folds cross validation increases model error out accuracy. The model uses _all_ observations for model training. The observations are randomly divided into train and test datasets, the model is fitted, then the model is randomly divided again, fitted, and so on, K times with resampling. So all observations are used as training data to fit the model. To determine final accuracy output, all of the model accuracies are averaged for a cumulative accuracy score. This model uses 10 fold cross validation. For the sake of brevity, please reference the logistic regression model fitting code in the Jupyter Notebook. 

<img src="/assets/2018-03-08-Predicting-Heart-Disease-with-Neural-Networks/logistic_reg_fitting.png" >

It turns out that using logistic regression results in an error out of 78%. This will be our deployable TabPy function. 

{% highlight python %}
# logistic regression cross validation: probability, 0 - 100%
def suggest_diag_prob(age, sex, resting_blood_pressure, cholesterol, cigarettes_per_day, 
years_as_smoker, fasting_blood_sugar, hist_heart_dis, resting_hr, max_hr_ach, mets, tpeakbps, exer_ind_angina, rldv5e):
    
    features = np.column_stack([age, sex, resting_blood_pressure, cholesterol, cigarettes_per_day, 
    years_as_smoker, fasting_blood_sugar, hist_heart_dis, resting_hr, max_hr_ach, mets, tpeakbps, exer_ind_angina, rldv5e])
    
    features_scaled = scaler.transform(features)
    predict_proba = lgclf.predict_proba(features_scaled)[0,1].tolist()
    
    return(predict_proba)
suggest_diag_prob(18, 0, 120, 150, 0, 0, 0,0, 65, 200, 15, 140, 0, 93)
{% endhighlight %}

## Connect to TabPy

Connecting Tableau to TabPy is relatively pain free after [installation](https://github.com/tableau/TabPy). Afterwards, run startup.sh (located in the TabPy-server folder) in the terminal to initiate the TabPy instance listening on localhost port 9004. Also ensure TabPy is [properly configured](https://www.tableau.com/about/blog/2016/11/leverage-power-python-tableau-tabpy-62077) in Tableau Desktop under Help > Settings and Performance > Manage External Connection.

Once both tasks are complete, deploy the logistic regression function to Tableau.

{% highlight python %}
# Connect to TabPy server using the client library
connection = tabpy_client.Client('http://localhost:9004')

# Publish the suggest_diag_prob function to TabPy server so it can be used from Tableau
connection.deploy('heart_disease_logregcv_prob',
                  suggest_diag_prob, override = True)
{% endhighlight %}

TabPy should now be communicating with the Python script or iPython notebook. Unfortunately, Tableau dashbords connected to exernal services such as TabPy can _not_ be posted to Tableau Public like the chloropeth dashboard. Nonetheless, I improvised and took a 30 second on-screen video to showcase the dashboard. After several tedious days of tweaking the design, the final product is depicted in the video below: 

<video width="480" height="320" controls="controls">
  <source src="assets/2018-03-08-Predicting-Heart-Disease-with-Neural-Networks/heart_disease_video_final.mp4" type="video/mp4">
</video>

I hope you enjoyed reading this post about my semester long project for Data Analysis and Operations Research. Feel free to comment below. Thank you!

John

 















