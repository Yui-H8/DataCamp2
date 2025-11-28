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
