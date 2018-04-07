---
header:
  image: /assets/internal_structures_2_0.png
  
author_profile: true

classes: wide

---

## A multi-layer neural network created using Keras to help doctors understand patient cardiovascular health 

Heart disease is described as any condition that affects the heart. Heart disease is often used interchangably with the term "cardiovascular disease." However, cardiovascular disease generally refers to conditions involving heart blockages such as atherosclerosis (narrowing or blocking of the ateries due to plaque buildup), whereas heart disease is an umbrella term that is inclusive of other heart conditions. Heart Disease can not only affect the arteries, but the heart muscle, valves, rhythm, or other important aspects of a well-functioning heart. 

The pervasiveness of heart disease in the United States has made the ailment the leading cause of death in the United States.
Here are a few statistics that emphasize how problematic heart disease has become in our society [1](https://www.cdc.gov/heartdisease/facts.htm)]: 
- About 610,000 people die of heart disease in the United States every year–that’s 1 in every 4 deaths.
- Heart disease is the leading cause of death for both men and women. More than half of the deaths due to heart disease in 2009 were in men.
- Coronary heart disease (CHD) is the most common type of heart disease, killing over 370,000 people annually.
- Every year about 735,000 Americans have a heart attack. Of these, 525,000 are a first heart attack and 210,000 happen in people who have already had a heart attack.

The term 'heart disease' refers to several aliments/conditions of the heart; the most common condition is coronary heart (artery) disease, which is a precursor to a heart attack. Other notable conditions that are defined as heart disease include ischemic and hemorrhagic stroke, heart failure, arrhythmia, and heart valve complications such as stenois and prolapse. The prevalence of heart disease  In order to provide adequate healthcare to treat heart disease across the United States, data visualization techniques such as choropleth mapping can be implemented to show comparative heart disease trends categorized by state and gender. 

The CDC's Division of Population Health provides yearly statistics of over 124 chronic health disease indicators that are reported on a city and state level. Using Python's leading data manipulation library, Pandas, and Plotly's choropleth map API, this dataset will be filtered and manipulated to display data for only mortality from diseases of the heart for the year 2014 and by gender. This CSV file, last updated on December 5th, 2017, will be used for this study [[2](https://chronicdata.cdc.gov/api/views/g4ie-h725/rows.csv?accessType=DOWNLOAD&api_foundry=true)].


<!-- <iframe src = "https://public.tableau.com/shared/PRSC2K6K8?:showVizHome=no&:embed=true" width="95%" height="600"></iframe> -->

<iframe src = "https://public.tableau.com/views/PrevalenceofHeartDiseaseintheUnitedStates20145_0/Dashboard1?:showVizHome=no&:embed=true" width="95%" height="600"></iframe>






