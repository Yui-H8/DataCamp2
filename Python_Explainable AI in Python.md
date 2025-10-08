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
