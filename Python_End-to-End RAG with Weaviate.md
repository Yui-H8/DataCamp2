# End-to-End RAG with Weaviate
---
### Description

Learn how to go from simple LLM calls to multi-modal RAG workflows with Weaviate! You'll learn how to process PDF documents to extract key text content like paragraphs, headings, and tables. You'll embed and store this data for retrieval with Weaviate. Finally, you'll craft effective retrieval prompts to pass to generative models. To cap this all off, you'll treat PDFs as images to allow you to capture context lost from images and plots. You'll use the ColPali multi-modal embedding model with a multi-modal generative model from OpenAI to begin having conversations with images and documents!

---
### Text Generation with LLMs
**Large Language Models (LLMs)**   
Large Language Models (LLMs) is a type of Generative AI model that can generate text based on the input/prompt it receives.

* Ask the LLM the general knowledge question.
* Ask the LLM about Weaviate's work-from-home policy.
* Ask the same question again, but this time, integrating additional context into the prompt, stored in weaviate_wfh_facts.

> Here, we create a request to an OpenAI LLM. Similarly to what happens when you "talk" to ChatGPT, the model will "respond" with a context-appropriate message.    
> Complete the LLM call to answer the question: What is the capital of France?
```python
from openai import OpenAI

client = OpenAI()

response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[
        {
            "role": "user",
            "content": """
                What is the capital of France?
            """,
        }
    ],
)

print(response.choices[0].message.content)
```
```
The capital of France is Paris.
```
LLMs (Large Language Models) are trained on vast amounts of text data. However, their knowledge is not complete.

A model's knowledge could be incomplete as a result of limited training data. Or, the knowledge could have become out of date.

Complete the LLM call to answer the question: What is Weaviate's flexible work policy as of September, 2025?
```python
weaviate_wfh_facts = """
Weaviate is a fully remote company with people living and working across the world.
It provides a home office budget, flexible time off, and local benefits.
It also allows employees to connect with colleagues worldwide and enjoy our annual company trip.
"""

context_response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[
        {
            "role": "user",
            "content": f"""
                ====================
                {weaviate_wfh_facts}
                ====================
                Answer only. What is Weaviate's flexible work policy as of September, 2025?
            """,
        }
    ],
)

print(context_response.choices[0].message.content)
```
