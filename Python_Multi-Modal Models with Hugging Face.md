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
### Image preprocessing
In this exercise, you will use the flickr dataset, which has 30,000 images and associated captions, to perform preprocessing operations on images. This preprocessing is needed to make the image data suitable for inferencing with Hugging Face model tasks, such as text generation from images. In this case, you'll generate a text caption for this image:
```python
> Dataset({
    features: ['image', 'caption', 'sentids', 'split', 'img_id', 'filename'],
    num_rows: 10 })
>
```
* Load the image from the element at index 5 of the dataset.
* Load the image processor (BlipProcessor) of the pretrained model: Salesforce/blip-image-captioning-base.
* Execute the processor on image, making sure to specify that PyTorch tensors (pt) are required.
* Use the .generate() method to create a caption using the model.
```python
# Load the image from index 5 of the dataset
image = dataset[5]["image"]

# Load the image processor of the pretrained model
processor = BlipProcessor.from_pretrained("Salesforce/blip-image-captioning-base")

# Preprocess the image
inputs = processor(images=image, return_tensors="pt")

# Generate a caption using the model
output = model.generate(**inputs)
print(f'Generated caption: {processor.decode(output[0])}')
print(f'Original caption: {dataset[5]["caption"][0]}')
```
### Audio preprocessing
In this exercise, you will learn how to adjust the sampling rate of audio data, as well as how to use an automatic preprocessor. You will be working with the VCTK Corpus, which includes around 44-hours of speech data uttered by 110 English speakers with various accents.   
*(https://huggingface.co/datasets/CSTR-Edinburgh/vctk)*  
* Resample the audio to a frequency of 16,000 Hz in the dataset using the .cast_column() method.
* Load the audio processor using the pretrained openai/whisper-small model.
* Preprocess the audio data of the first datapoint, specifying the sampling rate and
```python
# Resample the audio to a frequency of 16,000 Hz
dataset = dataset.cast_column("audio", Audio(sampling_rate=16000))

# Load the audio processor
processor = AutoProcessor.from_pretrained("openai/whisper-small")

# Preprocess the audio data of the 0th dataset element
audio_pp = processor(dataset[0]["audio"]["array"], sampling_rate=16000, padding=True, return_tensors="pt")
make_spectrogram(audio_pp["input_features"][0])
```
--- 
### Pipeline caption generation
In this exercise, you'll again use flickr dataset, which has 30,000 images and associated captions. Now you'll generate a caption for the following image using a pipeline instead of the auto classes.
```python
>  Dataset({
    features: ['image', 'caption', 'sentids', 'split', 'img_id', 'filename'],
    num_rows: 10 })
>
```
* Load the image-to-text pipeline with Salesforce/blip-image-captioning-base pretrained model.
* Use the pipeline to generate a caption for the image at index 3.
```python
# Load the image-to-text pipeline
pipe = pipeline(task="image-to-text", model="Salesforce/blip-image-captioning-base")

# Use the pipeline to generate a caption with the image of datapoint 3
pred = pipe(dataset[3]["image"])

print(pred)
```
### Passing keyword arguments
For this you will be using the MusicGen small model from Meta, which is capable of generating music samples based on text descriptions or audio prompts.
   
The pipeline module has been loaded, and the soundfile library is available as sf.

* Load a text-to-audio pipeline using the facebook/musicgen-small model in the PyTorch framework.
* Make a dictionary to set the generation temperature to 0.8 and max_new_tokens to 1.
* Generate an audio array corresponding to the "Classic rock riff" prompt.
```python
# Load a text-to-audio pipeline
musicgen = pipeline(task="text-to-audio", model="facebook/musicgen-small", framework="pt")

# Make a dictionary to set the generation temperature to 0.8 and max_new_tokens to 1
generate_kwargs = {"temperature": 0.8, "max_new_tokens": 1}

# Generate an audio array passing the arguments
outputs = musicgen("Classic rock riff", generate_kwargs=generate_kwargs)
sf.write("output.wav", outputs["audio"][0][0], outputs["sampling_rate"])
```
Great work! You generated new music! Here's the output after generating 500 tokens:   
*https://assets.datacamp.com/production/repositories/6926/datasets/044d80aeb5a28457e1a530c2c3177b4505fd54ea/output.wav*

### Model evaluation on a custom dataset
In this exercise, you will use an evaluator from the Hugging Face evaluate package to assess the performance of a pretrained model on a custom dataset. Note that, for multi-class classification with dataset imbalances, accuracy is not a reliable performance indicator. Therefore, you will use the ability of the evaluator to provide multiple measures at once: the precision and recall.
　
A dataset (dataset) and pipeline (pipe) have been predefined. The evaluate library and the evaluator class have also already been imported.
