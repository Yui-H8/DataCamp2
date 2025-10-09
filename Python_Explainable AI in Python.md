# Explainable AI in Python
---
### Decision trees vs. neural networks
Build a decision tree classifier to classify income levels based on multiple features including age, education level, and hours worked per week, and extract the learned rules that explain the decision. Then, compare its performance with an MLPClassifier trained on the same data.

X_train, X_test, y_train, and y_test are pre-loaded for you. The accuracy_score and export_text functions are also imported for you.
1. Extract the rules learned by the model. Compute the model's test accuracy.
```python
model = DecisionTreeClassifier(random_state=42, max_depth=2)
model.fit(X_train, y_train)

# Extract the rules
rules = export_text(model, feature_names=list(X_train.columns))
print(rules)

y_pred = model.predict(X_test)

# Compute accuracy
accuracy = accuracy_score(y_test, y_pred)
print(f"Accuracy: {accuracy:.2f}")
```
<pre>
  print(f"Accuracy: {accuracy:.2f}")
|--- education_num <= 12.50
|   |--- age <= 33.50
|   |   |--- class:  <=50K
|   |--- age >  33.50
|   |   |--- class:  <=50K
|--- education_num >  12.50
|   |--- age <= 29.50
|   |   |--- class:  <=50K
|   |--- age >  29.50
|   |   |--- class:  >50K

Accuracy: 0.78
</pre>
2. Train the MLPClassifier model. Derive the predictions on the test set. Compute the model's test accuracy.
```python
model = MLPClassifier(hidden_layer_sizes=(36, 12), random_state=42)
# Train the MLPClassifier
model.fit(X_train, y_train)

# Derive the predictions on the test set
y_pred = model.predict(X_test)

# Compute the test accuracy
accuracy = accuracy_score(y_test, y_pred)
print(f"Accuracy: {accuracy:.2f}")
```
<pre>
  output:
    Accuracy: 0.80
</pre>
*Great job! You've seen that the MLPClassifier's accuracy is higher than that of the decision tree, but unlike decision trees, it's harder to interpret how the decisions are made.*

### Model-agnostic vs. model-specific explainability
* Model-agnostic approaches can be applied to decision trees
* Model-agnostic approaches can be applied to neural networks

*Now you have a good understanding of the differences between model-agnostic and model-specific explainability approaches.*

---
### Computing feature impact with linear regression
As a data scientist at an insurance company, your task is to build and explain a linear regression model that estimates insurance charges based on features like age, BMI, and smoking status by analyzing the model's coefficients to determine the impact of each feature on the predictions.

matplotlib.pyplot has been imported as plt along with MinMaxScaler. X_train and y_train are pre-loaded for you.
* Normalize the training data X_train.
* Fit the linear regression model to the standardized training data.
* Extract the coefficients from the model.
* Plot the coefficients for the given feature_names.
```python
# Standardize the training data
scaler = MinMaxScaler()
X_train_scaled = scaler.fit_transform(X_train)
model = LinearRegression()

# Fit the model
model.fit(X_train_scaled, y_train)

# Derive coefficients
coefficients = model.coef_
feature_names = X_train.columns

# Plot coefficients
plt.bar(feature_names, coefficients)
plt.show()
```
*You discovered that smoking status and age have the highest influence on insurance charges. This makes sense as older individuals and smokers are at higher health risk, leading to higher costs.*

### Computing feature impact with logistic regression
Continuing your work at the insurance company, you built a predictive model to identify whether an individual is a smoker or not. Now, you need to analyze the model to determine the relevant factors influencing smoking status, helping the company assess risk more accurately and tailor insurance policies accordingly.
* Extract the coefficients from the model.
* Plot the coefficients for the given feature_names.
>Hint: The .coef_ attribute of your model returns a 2D array, which is not suitable for plotting. You have to select its first element with [0].
```python
scaler = MinMaxScaler()
X_train_scaled = scaler.fit_transform(X_train)

model = LogisticRegression()
model.fit(X_train_scaled, y_train)

# Derive coefficients
coefficients = model.coef_[0]
feature_names = X_train.columns

# Plot coefficients
plt.bar(feature_names, coefficients)
plt.show()
```
*You found that sex, age, and number of children are the most important factors for predicting smoking status. This makes sense as lifestyle choices often vary significantly with gender, age, and family responsibilities.*

---
### Computing feature importance with decision trees
You built a decision tree classifier to identify patients at risk of heart disease using the heart disease dataset. Now you need to explain the model by analyzing feature importance to determine the key factors for predicting heart disease, enabling more targeted healthcare interventions.
* Extract the feature importances from the model.
* Plot the feature_importances for the given feature_names.
* Did you call plt.barh()?
```python
model = DecisionTreeClassifier(random_state=42)
model.fit(X_train, y_train)

# Derive feature importances
feature_importances = model.feature_importances_
feature_names = X_train.columns

# Plot the feature importances
plt.barh(feature_names, feature_importances)
plt.show()
```

*You've identified the most important features for predicting heart disease. Understanding these key factors can help in making more informed healthcare decisions and improving patient outcomes.*

### Computing feature importance with random forests
