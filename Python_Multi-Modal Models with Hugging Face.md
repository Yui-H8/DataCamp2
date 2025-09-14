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
* Use only models that are for text-to-image tasks.
* Use an appropriate tag to ensure the model can be loaded by the StableDiffusionPipeline class from the diffusers library.
* Sort the results according to the number of "likes".
* Load the most popular model from models using its ID.
```python
models = api.list_models(
    # Filter for text-to-image tasks
    task="text-to-image",
    author="CompVis",
    # Filter for models that can be loaded by the diffusers library
    tags="diffusers:StableDiffusionPipeline",
    # Sort according to the most popular
    sort="likes"
)

models = list(models)

# Load the most popular model from models
pipe = StableDiffusionPipeline.from_pretrained(models[0].id)
```
---
### Text tokenizing
In this exercise, you will use the flickr dataset, which has 30,000 images and associated captions, to perform preprocessing operations on text. This is necessary to be used by models for tasks such as text classification. This is especially useful for multi-modal applications where Hugging Face models can be used to check caption suitability for an associated image.
1. Load the first "caption" from the image at index 5 of the dataset.
```python
# Load the first caption from the image at index 5
text = dataset[5]["caption"][0]
print(text)
```
2. Load the tokenizer of the pretrained model: distilbert/distilbert-base-uncased.  
Perform full preprocessing of the text using the tokenizer you defined.
```python
# Load the first caption from the image at index 5
text = dataset[5]["caption"][0]
print(text)

# Load the tokenizer of the pretrained model
tokenizer = AutoTokenizer.from_pretrained("distilbert/distilbert-base-uncased")

# Perform full preprocessing of the text
encoded_input = tokenizer(text, return_tensors='pt')
print(encoded_input)
```
