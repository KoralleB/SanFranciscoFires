# San Francisco Fire Incidents - Linear Regression Model and GIS visualization

## Background
The wildfire season in CA inspired me to analyze data related to fire incidents. I choose to analyze <code>Fire Incidents API</code> from [data.sfgov.org](https://data.sfgov.org/) 
of San Francisco County, and to estimate how many firefighters should be called for different fire incidents.

## Table of contents
1. [Objectives](#objectives)
2. [Dataset](#dataset)
3. [Exploratory Data Analysis](#exploratory-data-analysis)
4. [Model Diagnosis](#model-diagnosis)
5. [Result Visualization](#result-visualization)
6. [Tech Stack](#tech-stack)

## Objectives
* Create a linear regression model to predict the required number of suppression personnel for a fire incident in San Francisco based on fire incident characteristics. 
The model is estimated using the stepwise selection method on a pool of preselected variables related to the response variable.
* Compare predicted values with true values using mapping visualization tools.

## Dataset
The Fire Incidents datasets, after a subset of the City of San Francisco, includes 485,775 observations and 80 variables, 
describing fire incidents from 2003 till today (2020).

Missing observations and outlier suppression incidents (over 150 firefighters) were removed. 
The final dataset includes 5,725 observations, which is sufficient for a linear regression. 

Ten predictor variables were evaluated for modeling, where two variables are numerical and eight variables are categorical.
The variables are <code>Structure Status</code>, <code>Structure Type</code>, <code>Area of Fire Origin</code>, <code>Floor of Fire Origin</code>, 
<code>Ignition Cause</code>, <code>Primary Situation</code>, <code>Automatic Extinguishing System Present</code>, <code>Detectors Present</code>, 
<code>Number of Sprinkler Heads Operating</code>, and <code>Fire Spread</code>.

Categorical variables with more than ten categories were simplified such that the top nine categories were kept, and the rest were combined into a single “Other” category
to retain the power of the model (a better method to decide which categories to combine is the pairwise comparison).

## Exploratory Data Analysis
### Response Variable
The distribution of the response variable is heavily right-skewed. Therefore, I applied a box-cox transformation on the response variable, 
which suggests a square root transformation. Then, the response variable shows a somewhat normal distribution.

<img src='https://github.com/KoralleB/SanFranciscoFires/blob/master/image/Supp%20Hist.png'> <img src='https://github.com/KoralleB/SanFranciscoFires/blob/master/image/Supp%20Hist%20Boxcox.png'>

### Predictor variables
The <code>Cause of Ignition</code> variable reveals that most of the fires started unintentional, while about 12% of the incidents were intentionally.

<img src='https://github.com/KoralleB/SanFranciscoFires/blob/master/image/Cause%20of%20Ignition.png'>
 
Different fire-aid systems are available in the market. The most common one is the smoke detector, 
which its installation is obligatory by law and was installed at 70.2% of the cases. 
Sprinklers are rarely installed, and only 3.7% of the incidents had them. 
Extinguish system is also not common, and 22.0% of the scenes had a system like this.

<img src='https://github.com/KoralleB/SanFranciscoFires/blob/master/image/Does%20A%20Smoke%20Detector%20Present.png' width='300'> <img src='https://github.com/KoralleB/SanFranciscoFires/blob/master/image/Do%20Sprinkles%20Present.png' width='300'> <img src='https://github.com/KoralleB/SanFranciscoFires/blob/master/image/Do%20Extinguish%20System%20Present.png' width='300'>

Most of the fire incidents happened in the kitchen area (23.0%) in enclosed (84.5%) normally used structures (91.5%). 
Since some of the incidents happened in open areas, temporary structures, or structures under construction, 
that might partially explain why a smoke detector was not present in about 30%, although it is obligatory to be installed.

<img src='https://github.com/KoralleB/SanFranciscoFires/blob/master/image/Most%20Common%20Fire%20Origin%20Location.png'>

<img src='https://github.com/KoralleB/SanFranciscoFires/blob/master/image/Structure%20Type.png' width='500'> <img src='https://github.com/KoralleB/SanFranciscoFires/blob/master/image/Structure%20Status.png'  width='500'>

## Model Diagnosis
To determine significant variables for constructing a linear model I used stepwise selection, a method combines both forward selection and backward elimination. 
Two predictor variables were found to be not significant, <code>Floor of Fire Origin</code> and <code>Detectors Present</code>, 
so the final model has eight independent variables.
Adjusted R-squared is equal to 0.191, a reasonable value for a model with several categorical variables.

The Y-axis of the <code>Residual vs Fitted</code> plot is between -6 and 6, which suggests that the variance of residuals is constant. 
The <code>Probability Plot</code> shows that the right tail is a bit above the red line, 
which suggests that that the residuals are heavy-tailed and normally distributed. 
The plots show that the model satisfies the linear regression assumptions of constant residual variance and normally distributed residuals.

<img src='https://github.com/KoralleB/SanFranciscoFires/blob/master/image/Model%20Diagnostic.png'>

## Result Visualization
The map is divided into the 11 districts of the City of San Francisco and shows the mean value of the personnel served in each district.

While the absolute mean value of each district if offset, 
the predicted districts that require the highest and lowest number of firefighters are also the true districts that require the same number of firefighters.

**Suppression Personnel - True Values:**

<img src='https://github.com/KoralleB/SanFranciscoFires/blob/master/image/True%20Suppression.PNG'>

**Suppression Personnel - Predicted Values:**

<img src='https://github.com/KoralleB/SanFranciscoFires/blob/master/image/Predicted%20Suppression.PNG'>

## Tech Stack

<img src='https://github.com/KoralleB/SanFranciscoFires/blob/master/image/logo_NumPy.png' width='400'> <img src='https://github.com/KoralleB/SanFranciscoFires/blob/master/image/logo_pandas.png' width='400'>

<img src='https://github.com/KoralleB/SanFranciscoFires/blob/master/image/logo_statsmodels.png' width='500'> <img src='https://github.com/KoralleB/SanFranciscoFires/blob/master/image/logo_scipy.png' width='300'>

<img src='https://github.com/KoralleB/SanFranciscoFires/blob/master/image/logo_matplotlib.png' width='400'> <img src='https://github.com/KoralleB/SanFranciscoFires/blob/master/image/logo_geopandas.png' width='400'> <img src='https://github.com/KoralleB/SanFranciscoFires/blob/master/image/logo_folium.png' width='100'>
