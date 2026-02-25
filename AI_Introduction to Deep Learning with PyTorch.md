# Introduction to Deep Learning with PyTorch
---
What you'll learn  
* Apply activation functions to introduce non-linearity in models
* Build and inspect tensors as the foundation of PyTorch models
* Construct and connect neural network layers
* Implement optimizer steps, scheduling, and training-loop housekeeping.
* Manage model modes, persistence, and parameter inspection.

**Description**

Deep learning is everywhere, from smartphone cameras to voice assistants or self-driving cars. In this course, you will discover this powerful technology and learn how to leverage it using PyTorch, one of the most popular deep learning libraries. By the end of this course, you will be able to leverage PyTorch to solve classification and regression problems using deep learning.

---
### Getting started with PyTorch tensors
Tensors are PyTorch's core data structure and the foundation of deep learning. They're similar to NumPy arrays but have unique features.

Here you have a Python list named temperatures containing daily readings from two weather stations. Try converting this into a tensor!
* Begin by importing torch.
* Create a tensor from the Python list temperatures.
```python
# Import PyTorch
import torch

temperatures = [[72, 75, 78], [70, 73, 76]]

# Create a tensor from temperatures
temp_tensor = torch.tensor(temperatures)

print(temp_tensor)
```
```
output:
    tensor([[72, 75, 78],
            [70, 73, 76]])
```
*Great job! You've successfully created a tensor using temperature data. This is your first step towards building your first neural network!*

### Checking and adding tensors
While collecting temperature data, you notice the readings are off by two degrees. Add two degrees to the temperatures tensor after verifying its shape and data type with torch to ensure compatibility with the adjustment tensor.

The torch library and the temperatures tensor are loaded for you.
1. Display the shape of the adjustment tensor.    
* Display the data type of the adjustment tensor.
```
Hint
You can access the shape and data type of a tensor using tensor.shape and tensor.dtype.
```
```python
adjustment = torch.tensor([[2, 2, 2], [2, 2, 2]])

# Display the shape of the adjustment tensor
print("Adjustment shape:", adjustment.shape)

# Display the type of the adjustment tensor
print("Adjustment type:", adjustment.dtype)

print("Temperatures shape:", temperatures.shape)
print("Temperatures type:", temperatures.dtype)
```
```
Adjustment shape: torch.Size([2, 3])
Adjustment type: torch.int64
Temperatures shape: torch.Size([2, 3])
Temperatures type: torch.int64
```
2. Add the temperatures and adjustment tensors.
```python
adjustment = torch.tensor([[2, 2, 2], [2, 2, 2]])

# Add the temperatures and adjustment tensors
corrected_temperatures = temperatures + adjustment
print("Corrected temperatures:", corrected_temperatures)
```
```
<script.py> output:
    Corrected temperatures: tensor([[74, 77, 80],
            [72, 75, 78]])
```
*Excellent! You've verified the tensors and adjusted the temperature data. Next, let’s learn the role of tensors within neural networks.*

### Linear layer network
Neural networks often contain many layers, but most of them are linear layers. Understanding a single linear layer helps you grasp how they work before adding complexity.

Apply a linear layer to an input tensor and observe the output.
* Create a Linear layer that takes 3 features as input and returns 2 outputs.
* Pass input_tensor through the linear layer.
```
Hint
Use nn.Linear() to create a linear layer; make sure to capitalize the L.
The linear layer's input should have 3 features, and its output should have 2 features.
Apply linear_layer() to input_tensor to compute the output.
```
```python
import torch
import torch.nn as nn

input_tensor = torch.tensor([[0.3471, 0.4547, -0.2356]])

# Create a Linear layer
linear_layer = nn.Linear(
                         in_features=3, 
                         out_features=2
                         )

# Pass input_tensor through the linear layer
output = linear_layer(input_tensor)

print(output)
```
