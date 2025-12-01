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
* Convert the template text provided into a standard (non-chat) prompt template.
* Create a chain to pass the prompt template into the LLM.
* Invoke the chain on the question variable provided.
> Hint
> Use the .from_template() method of PromptTemplate to convert a template string into a LangChain prompt template.
> To chain a prompt template into an LLM using LCEL, separate the components with the | operator, with the prompt template object coming first.
```python
# Create a prompt template from the template string
template = "You are an artificial intelligence assistant, answer the question. {question}"
prompt = PromptTemplate.from_template(
    template=template
)

llm = ChatOpenAI(model="gpt-4o-mini", api_key='<OPENAI_API_TOKEN>')	

# Create a chain to integrate the prompt template and LLM
llm_chain = prompt | llm

# Invoke the chain on the question
question = "How does LangChain make LLM application development easier?"
print(llm_chain.invoke({"question": question}))
```
```
content='LangChain simplifies the development of applications that utilize large language models (LLMs) by providing a comprehensive framework that addresses several key challenges in LLM application development. Here are some of the ways it makes this process easier:\n\n1. **Modular Components**: LangChain offers a modular architecture that allows developers to mix and match different components (like models, data loaders, and output formats) according to their needs. This modularity can streamline the integration process and help developers build custom solutions more quickly.\n\n2. **Pre-built Pipelines**: It provides pre-built chains and templates that handle common use cases, such as question answering, summarization, and dialogue management. This reduces the need for boilerplate code and lets developers focus on the unique aspects of their applications.\n\n3. **Data Handling**: LangChain includes tools for managing and preprocessing data. This is vital when working with LLMs, which often require large amounts of text data to be formatted and structured appropriately.\n\n4. **Integration Capabilities**: LangChain integrates seamlessly with existing tools, services, and databases, enabling developers to pull in external data sources or connect with various APIs without extensive development overhead.\n\n5. **Prompt Management**: The framework facilitates effective prompt engineering, allowing developers to easily experiment with and optimize prompts to improve model performance on specific tasks.\n\n6. **Evaluation and Testing**: LangChain includes functionalities for evaluating model performance, helping developers test and refine their applications systematically.\n\n7. **Scalability**: The framework is designed to handle scaling issues that arise when deploying LLM applications, making it easier to manage increased loads or users.\n\n8. **Community and Resources**: LangChain has an active community and comprehensive documentation, providing access to resources and support that can accelerate the development process.\n\nBy addressing these various aspects of LLM application development, LangChain enables developers to create more robust, efficient, and scalable applications with less complexity and effort.' additional_kwargs={'refusal': None} response_metadata={'token_usage': {'completion_tokens': 388, 'prompt_tokens': 29, 'total_tokens': 417, 'completion_tokens_details': {'accepted_prediction_tokens': 0, 'audio_tokens': 0, 'reasoning_tokens': 0, 'rejected_prediction_tokens': 0}, 'prompt_tokens_details': {'audio_tokens': 0, 'cached_tokens': 0}}, 'model_name': 'gpt-4o-mini-2024-07-18', 'system_fingerprint': 'fp_b547601dbd', 'finish_reason': 'stop', 'logprobs': None} id='run-b4e4e524-3899-40c6-9c38-390acb8d1c9b-0' usage_metadata={'input_tokens': 29, 'output_tokens': 388, 'total_tokens': 417, 'input_token_details': {'audio': 0, 'cache_read': 0}, 'output_token_details': {'audio': 0, 'reasoning': 0}}
In [1]:
```
*Your perfectly pieced-together prompt produced pristine performance! Well-designed prompt templates are at the heart of many LangChain applications; keep experimenting to find the template that gives you optimum performance!*

### Chat prompt templates
Given the importance of chat models in many LLM applications, LangChain provides functionality for creating prompt templates to structure messages to different chat roles.

The ChatPromptTemplate class has already been imported for you, and an LLM has already been defined.
* Use ChatPromptTemplate.from_messages() to convert the role-message pairs into a chat prompt template.
* Assign appropriate roles to the messages provided to create a conversation pattern.
* Create an LCEL chain and invoke it with the input provided.
```python
llm = ChatOpenAI(model="gpt-4o-mini", api_key='<OPENAI_API_TOKEN>')

# Create a chat prompt template
prompt_template = ChatPromptTemplate.from_messages(
    [
        ("system", "You are a geography expert that returns the colors present in a country's flag."),
        ("human", "France"),
        ("ai", "blue, white, red"),
        ("human", "{country}")
    ]
)

# Chain the prompt template and model, and invoke the chain
llm_chain = prompt_template | llm

country = "Japan"
response = llm_chain.invoke({"country": country})
print(response.content)
```
```
<script.py> output:
    white, red
```

*Nice job! Hopefully you're beginning to the power of prompt templates for translating user inputs into model inputs. Try experimenting with other countries to see how the model performs for more complex flags, like 'Spain'. When you're done, head on over to the next video to learn how to scale-up your prompts for larger numbers of examples!*

### Creating the few-shot example set
PromptTemplate and ChatPromptTemplate are great for integrating variables, but struggle with integrating datasets containing many examples. This is where FewShotPromptTemplate comes in! In this exercise, you'll create a dataset, in the form of a list of dictionaries, to contain the follow question-answer pairs.
- Question: How many DataCamp courses has Jack completed?
- Answer: 36
- Question: How much XP does Jack have on DataCamp?
- Answer: 284,320XP
- Question: What technology does Jack learn about most on DataCamp?
- Answer: Python
In the next exercise, you'll convert this information into a few-shot prompt template.
```python
# Create the examples list of dicts
examples = [
  {
    "question": "How many DataCamp courses has Jack completed?",
    "answer": "36"
  },
  {
    "question": "How much XP does Jack have on DataCamp?",
    "answer": "284,320XP"
  },
  {
    "question": "What technology does Jack learn about most on DataCamp?",
    "answer": "Python"
  }
]
```
*Well done! With your examples dataset all set up, you're ready to create your few-shot prompt template!*

### Building the few-shot prompt template
With your examples in a structured format, it's now time to create the few-shot prompt template! You'll create a template that converts the question-answer pairs into the following format:
```
Question: Example question
Example Answer
```
All of the LangChain classes necessary for completing this exercise have been pre-loaded for you.
* Complete the prompt for formatting answers so it includes the question and answer keys.
* Create the few-shot prompt using FewShotPromptTemplate with examples and example_prompt.
* Complete the list of input variables based on the suffix provided.
> Hint   
> For the template string, use question and answer as the variable names to match the dictionary keys in your examples.
> Look at the suffix="Question: {input}" - the input variable name is input (found in the curly brackets).
```python
# Complete the prompt for formatting answers
example_prompt = PromptTemplate.from_template("Question: {question}\n{answer}")

# Create the few-shot prompt
prompt_template = FewShotPromptTemplate(
    examples=examples,
    example_prompt=example_prompt,
    suffix="Question: {input}",
    input_variables=["input"],
)

prompt = prompt_template.invoke({"input": "What is Jack's favorite technology on DataCamp?"})
print(prompt.text)
```
*Fabulous few-shot prompting! Invoking the prompt template allows you to see exactly what context the model will have. Now for the final piece: create an LCEL chain to combine the few-shot template with an LLM!*
