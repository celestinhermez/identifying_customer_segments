# Identifying Customer Segments

This project was completed as part of Udacity's Data Scientist nanodegree to illustrate concepts in unsupervised learning 
(PCA and clustering more specifically). The goal was to identify customer segments within the German population and 
within the consumer base of a mail-order sales company. 
In doing so we can better understand who the firm's customers are (which clusters it over- and under-indexes)
so it can focus its effort on reaching those while not wasting efforts on people who are not likely to be interested.

This was a case study developed by Udacity in partnership with AZ Direct and Arvato Finance Solutions 
who provided real world data. As such, I am not authorized to share the raw data, 
and the code included in the Jupyter notebook cannot be run. 
Yet, I still wanted to display this project here to highlight how unsupervised learning techniques 
can be used to provide valuable insights to businesses.

The Jupyter notebook (both in `ipynb` and `html` formats) describe the analysis conducted 
in this project. I followed the usual data science steps, outlined below.

## Data Wrangling

We load several files which were part of the case study 
(not available in this repository because the data does not belong to me). 
In particular, one file contains demographic information about the entire population of Germany, 
another about customers of a mail-order company. 
Both have the same features, which are described in separate documents. We start by conducting a thorough cleaning,
processing and modeling of the dataset containing information about the entire German population.

## Data Cleaning

Since this is real world data, a lot of data cleaning had to happen (as is always the case
in data science projects).

### Missing Values

In particular, we have to deal with missing values, which are encoded through certain codes in this dataset 
(such as `-1`) instead of the usual empty values. We write a function to map these codes (different for each feature) to 
a missing value (`NaN`). After this, we can explore missing values in columns and rows, and decide
what to do. We exclude columns with too many missing values ("too many" being determined by looking
at the distribution), while we impute values for the other ones. We adopt a similar approach with the rows.

### Re-Encode Features

There are a lot of categorical data, which we need to re-encode since `scikit-learn` expects numerical
features.

* we use numerical dummy variables for binary features
* we use one-hot encoding (through `OneHotEncoder`) for multi-level categorical variables
* we have some mixed features (one variable captures several pieces of information) which we break out
whenever possible, and drop otherwise

Once we have cleaned everything and encoded our features, we create a cleaning function to make this
process modular and apply it quickly to both datasets (the entire population of Germany and
customers of the mail-order company).

## Feature Engineering

### Feature Scaling

Unsupervised learning techniques (both PCA and clustering) as scale-dependent, so we scale our features. In
order to do so, we use a `StandardScaler` to transform our features to all have mean 0 and standard deviation of 1.

### Dimensionality Reduction

We perform Principal Component Analysis with the help of scikit-learn's `PCA` class to reduce the dimensionality
of our data. In order to determine how many principal components to keep, we use a scree plot to examine the explained
variance per component. We keep 70 components, a reduction in the number of features by almost 50%.

By looking at the weights each original feature carries in the principal components (which are linear combinations of
original features) we can try and interpret what information they capture. In doing so, we can put more sense
behind these mathematical objects. For instance, we find that the first principal component represents low-income
families living in urban centers.

## Modeling

We use our reduced dataset to perform K-Means clustering. We test various numbers of clusters, and helped
with the within-cluster distances we settle on 13 clusters.

## Results

Repeating all the steps above with the dataset containing customers of the mail-order company, we can
see which clusters are over or under-represented. In particular, we see that the company under-indexes with low-income
families in urban centers as well as well-off older people with low digital fluency. With these results
in mind, we recommend that the company stay away from urban individuals, but focuses its efforts on serving
and acquiring more rural people, for whom it makes more sense to have goods delivered through a mail-order
company.

## Conclusion

This project highlights how critical and time-consuming getting the data in a suitable format can be. A lot
of cleaning, pre-processing and re-encoding had to take place before we could leverage PCA and clustering. Yet,
it showed how, even without a clear target in mind, we can use statistical techniques to make sense of real-world,
messy and dirty data and derive knowledge from it.

Based on the insights from this analysis, we recommended a clear marketing strategy: stay away from urban dwellers,
and focus on rural customers. Intuitively, this makes sense for getting access to shops is more challenging for this 
population.
Yet, it is important to validate our analysis and its recommendations. The next steps should be an A/B test 
of the response of similar urban vs. rural individuals to marketing. This will confirm our conclusions, as well as
allow us to refine the tactics used with more granular groups of customers.
This analysis should also be re-run periodically to adapt to a changing customer base and evolving company
strategy. 