# Supervised Learning with scikit-learn
---
### Binary classification
### The supervised learning workflow
Recall that scikit-learn offers a repeatable workflow for using supervised learning models to predict the target variable values when presented with new data.

Reorder the pseudo-code provided so it accurately represents the workflow of building a supervised learning model and making predictions.

### Configuring Auto scaling group capacity
A financial services company runs a trading platform that typically requires three EC2 instances during normal operations but can need up to ten instances during market volatility. The platform must maintain at least two instances for redundancy at all times. Which Auto Scaling Group configuration meets these requirements?
* Answer
* Set minimum desired capacity to two, desired capacity to three, and maximum desired capacity to ten.

*Correct! This configuration ensures at least two instances always run for redundancy (minimum desired capacity), maintains three instances during normal operations (desired capacity), and allows scaling up to ten instances during high demand (maximum desired capacity). The Auto Scaling Group will create new instances when demand exceeds capacity and terminate instances when demand decreases, always staying within these bounds.*
