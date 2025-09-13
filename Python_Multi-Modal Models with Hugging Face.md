# Multi-Modal Models with Hugging Face
---
### How many models!?
* Use the correct method of api to the list all of the models associated with the "text-to-image" task.
```python
# Find the list of models for a task
models = api.list_models(task="text-to-image")
print(f"Task: text-to-image, Models: {len(list(models))}")
```
### Finding the most popular text-to-image model
Time to refine your search to find and load the most popular text-to-image Stable Diffusion model from CompVis on the Hugging Face Hub.
* 
