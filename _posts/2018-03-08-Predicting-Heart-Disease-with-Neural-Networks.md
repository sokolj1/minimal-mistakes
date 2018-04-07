---
header:
  image: /assets/internal_structures_2_0.png
  
author_profile: true

classes: wide

---

## A multi-layer neural network created using Keras to help doctors understand patient cardiovascular health 

Heart disease is the leading cause of death in the United States. According to the Center of Disease Control (CDC), heart disease is attributed to more than 600,000 annual deaths. Additionally, heart disease is the leading cause of death for both men _and_ women, although men are statistically at a greater risk for developing heart disease than women [1](https://www.cdc.gov/heartdisease/facts.htm). 

The term 'heart disease' refers to several aliments/conditions of the heart; the most common condition is coronary artery disease (atherosclerosis), which is a precursor to a heart attack. Other notable conditions that are defined as heart disease include ischemic and hemorrhagic stroke, heart failure, arrhythmia, and heart valve complications such as stenois and prolapse. In order to provide adequate healthcare to treat heart disease across the United States, data visualization techniques such as choropleth mapping can be implemented to show comparative heart disease trends categorized by state and gender. 

The CDC's Division of Population Health provides yearly statistics of over 124 chronic health disease indicators that are reported on a city and state level. Using Python's leading data manipulation library, Pandas, and Plotly's choropleth map API, this dataset will be filtered and manipulated to display data for only mortality from diseases of the heart for the year 2014 and by gender. This CSV file, last updated on December 5th, 2017, will be used for this study [2](https://chronicdata.cdc.gov/api/views/g4ie-h725/rows.csv?accessType=DOWNLOAD&api_foundry=true).


<iframe src = "https://public.tableau.com/shared/PRSC2K6K8?:showVizHome=no&:embed=true" width="100%" height="500"></iframe>
