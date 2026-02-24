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
