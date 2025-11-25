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
1. Use the .describe() method to find the numeric feature without variance and remove its name from the list assigned to number_cols.
> Hint   
> In the overview provided by .describe() this feature will have the same minimum and maximum plus a standard deviation of zero.
```python
# Remove the feature without variance from this list
number_cols = ['HP', 'Attack', 'Defense', 'Generation']
pokemon_df.describe(exclude='number')
number_cols.remove('Generation')
```
2. Combine the two lists of feature names to sub-select the chosen features from pokemon_df.
```python
# Remove the feature without variance from this list
number_cols = ['HP', 'Attack', 'Defense']

# Leave this list as is for now
non_number_cols = ['Name', 'Type', 'Legendary']

# Sub-select by combining the lists with chosen features
df_selected = pokemon_df[number_cols + non_number_cols]

# Prints the first 5 lines of the new DataFrame
print(df_selected.head())
```
```
script.py> output:
       HP  Attack  Defense                   Name   Type  Legendary
    0  45      49       49              Bulbasaur  Grass      False
    1  60      62       63                Ivysaur  Grass      False
    2  80      82       83               Venusaur  Grass      False
    3  80     100      123  VenusaurMega Venusaur  Grass      False
    4  39      52       43             Charmander   Fire      False
```
3. Find the non-numeric feature without variance and remove its name from the list assigned to non_number_cols.
> Hint   
> Use the exclude argument of pokemon_df's .describe() method to leave out numeric columns.
```python
# Leave this list as is
number_cols = ['HP', 'Attack', 'Defense']

# Remove the feature without variance from this list
non_number_cols = ['Name', 'Type']

# Create a new DataFrame by subselecting the chosen features
df_selected = pokemon_df[number_cols + non_number_cols]

# Prints the first 5 lines of the new DataFrame
print(df_selected.head())
```
```
   HP  Attack  Defense                   Name   Type
0  45      49       49              Bulbasaur  Grass
1  60      62       63                Ivysaur  Grass
2  80      82       83               Venusaur  Grass
3  80     100      123  VenusaurMega Venusaur  Grass
4  39      52       43             Charmander   Fire
```
### Visually detecting redundant features
Data visualization is a crucial step in any data exploration. Let's use Seaborn to explore some samples of the US Army ANSUR body measurement dataset.

Two data samples have been pre-loaded as ansur_df_1 and ansur_df_2.

Seaborn has been imported as sns.
1. Create a pairplot of the ansur_df_1 data sample and color the points using the 'Gender' feature.
