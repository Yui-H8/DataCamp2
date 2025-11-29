# Developing LLM Applications with LangChain
---
**Description**

Revolutionize your applications by harnessing the power of the LangChain framework for creating applications based on large language models (LLMs)! One of the major challenges for developing applications in the age of generative AI is integrating models, data sources, prompts, and other components from different providers into a single application. The LangChain framework provides a single, unified syntax for putting all these pieces together to allow you to integrate LLMs more seamlessly into your projects. Whether you're a seasoned developer or just getting started, this course will equip you with the knowledge and skills to build dynamic and intelligent applications that leverage the limitless capabilities of LangChain. Join us on this transformative journey and redefine how you create applications powered by language models.

---
### OpenAI models in LangChain!
OpenAI's GPT-series models are some of the highest performing LLMs around. They are available via OpenAI's API, which you can easily interact with using LangChain.

> Normally, using OpenAI's models would require a personal API key used for billing the cost of the models. In this course, you do not need to create or provide an OpenAI API key. The "<OPENAI_API_TOKEN>" placeholder will send valid requests to the API. If you make large number of requests in a short period, you may encounter a RateLimitError. If you see this, please pause for a moment and try again.

The ChatOpenAI class has already been imported.
* Define an OpenAI chat model using a LangChain class; leave the API key placeholder as it is.
* Invoke the LLM you defined to respond to the prompt provided (feel free to experiment with other prompts here).
```python
# Define the LLM
llm = ChatOpenAI(model="gpt-4o-mini", api_key="<OPENAI_API_TOKEN>")

# Predict the words following the text in question
prompt = 'Three reasons for using LangChain for LLM application development.'
response = llm.invoke(prompt)

print(response.content)
```
```
<script.py> output:
    LangChain is a powerful framework for developing applications that utilize large language models (LLMs). Here are three reasons for using LangChain in LLM application development:
    
    1. **Modularity and Flexibility**: LangChain provides a modular architecture that allows developers to easily compose various components needed for language model applications. This means you can integrate different modules for prompt generation, memory management, chains of logic, and output handling, which simplifies the development process and allows for easy experimentation with different configurations.
    
    2. **Enhanced Capabilities**: LangChain offers built-in features like document retrieval, data augmentation, and memory management, enabling applications to handle more complex tasks. This means developers can create applications that not only respond to user prompts but also retrieve and integrate relevant information from various data sources, enhancing the overall performance and capabilities of the LLM.
    
    3. **Rich Ecosystem and Community Support**: LangChain has a growing ecosystem with a variety of integrations and a supportive community. This provides developers with access to a wealth of resources, tutorials, and pre-built components, making it easier to get started and troubleshoot issues. Additionally, the active community helps in keeping the framework up to date with the latest advancements in AI and language models.
    
    Overall, LangChain streamlines the development of LLM applications, enhances their capabilities, and provides valuable community resources.
In [1]:
```
*Very nicely done! The standardized syntax that LangChain offers means that models can be quickly changed in and out as new ones are released and tested. Let's see this in action by trying a model from Hugging Face!*

### Hugging Face models in LangChain!
There are thousands of models freely available to download and use on Hugging Face. Hugging Face integrates really nicely into LangChain via its partner library, langchain-huggingface, which is available for you to use.

In this exercise, you'll load and call the crumb/nano-mistral model from Hugging Face. This is a ultra-light LLM designed to be fine-tuned for greater performance.
* Import HuggingFacePipeline from langchain_huggingface to work with Hugging Face models.
* Define a text generation LLM by calling HuggingFacePipeline.from_model_id().
* Set the model_id parameter to specify which Hugging Face model to use.
```python
# Import the HuggingFacePipeline class for defining Hugging Face pipelines
from langchain_huggingface import HuggingFacePipeline

# Define the LLM from the Hugging Face model ID
llm = HuggingFacePipeline.from_model_id(
    model_id="crumb/nano-mistral",
    task="text-generation",
    pipeline_kwargs={"max_new_tokens": 20}
)

prompt = "Hugging Face is"

# Invoke the model
response = llm.invoke(prompt)
print(response)
```
```
<script.py> output:
    Hugging Face is a great way to get a little bit of a break from the stresses of life. It'
In [1]:
```
*Nice job! 🤗 This syntax for defining and invoking LLMs unlocks the ability to use almost any model! Now that you have nailed the defining and invoking LLM workflow, let's look at strategies for prompting these models effectively.*

### Prompt templates and chaining
In this exercise, you'll begin using two of the core components in LangChain: prompt templates and chains!

The classes necessary for completing this exercise, including ChatOpenAI, have been pre-loaded for you.
