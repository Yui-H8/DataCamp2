# Dimensionality Reduction in Python
---
#### Description

High-dimensional datasets can be overwhelming and leave you not knowing where to start. Typically, you’d visually explore a new dataset first, but when you have too many dimensions the classical approaches will seem insufficient. Fortunately, there are visualization techniques designed specifically for high dimensional data and you’ll be introduced to these in this course. After exploring the data, you’ll often find that many features hold little information because they don’t show any variance or because they are duplicates of other features. You’ll learn how to detect these features and drop them from the dataset so that you can focus on the informative ones. In a next step, you might want to build a model on these features, and it may turn out that some don’t have any effect on the thing you’re trying to predict. You’ll learn how to detect and drop these irrelevant features too, in order to reduce dimensionality and thus complexity. Finally, you’ll learn how feature extraction techniques can reduce dimensionality for you through the calculation of uncorrelated principal components.

---
### Finding the number of dimensions in a dataset
```
In [2]:
pokemon_df.describe()
Out[2]:

            HP   Attack  Defense  Generation
count  160.000  160.000  160.000       160.0
mean    64.612   74.981   70.175         1.0
std     27.921   29.180   28.884         0.0
min     10.000    5.000    5.000         1.0
25%     45.000   52.000   50.000         1.0
50%     60.000   71.000   65.000         1.0
75%     80.000   95.000   85.000         1.0
max    250.000  155.000  180.000         1.0
In [3]:
pokemon_df.shape
Out[3]:
(160, 7)
```
### Removing features without variance
A sample of the Pokemon dataset has been loaded as pokemon_df. To get an idea of which features have little variance you should use the IPython Shell to calculate summary statistics on this sample. Then adjust the code to create a smaller, easier to understand, dataset.
