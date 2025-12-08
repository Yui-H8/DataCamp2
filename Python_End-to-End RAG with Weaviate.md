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
no_context_response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[
        {
            "role": "user",
            "content": """
                Answer only. What is Weaviate's flexible work policy as of September, 2025?
            """,
        }
    ],
)

print(no_context_response.choices[0].message.content)
```
```
I'm sorry, but I don't have access to information about Weaviate's flexible work policy as of September 2025.
```
The model should answer that it does not know. This is a trivial demonstration, but the same issue exists in all domains, such as in science, law, business, or even personal use. So, how can we improve on this?

One solution is to provide the model with additional "context" to help it answer questions. Here's an example:

Complete the LLM call to ask the same question again, but this time, integrating context into the prompt, stored in weaviate_wfh_facts.

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
```
Weaviate has a fully remote work policy, providing flexible time off and allowing employees to work from anywhere in the world.
```
By providing additional context from a source (
Weaviate's website
 in this case), we were able to get the correct answer.

This is great, but it would be a pain to do this manually every time we wanted to ask a question. So, how can we automate this? That's where vector embeddings come in.

*And you're off the mark! For LLMs, context is everything—they can't answer confidently or accurately to things they aren't aware of from their training data or prompt.*
