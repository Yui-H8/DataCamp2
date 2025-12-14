# End-to-End RAG with Weaviate3
---
### Introduction to RAG
Retrieval-Augmented Generation (RAG) intelligently provides the right context to generative AI models, enhancing their accuracy.

#### Components of RAG
A typical RAG system comprises:

**Retrieval**: of relevant information.    
**Augmentation**: of the input with the retrieved information.    
**Generation**: of a response using the generative AI model.

* Re-run the retrieval code from the previous exercise and retrieve the top two semantically similar documents.
* Create a structured prompt that combines the query_text with the retrieved texts (retrieved_texts).
* Send combined_context as a request to the OpenAI chat completions model provided.


Let's see how these steps work manually.

#### Retrieval
The R (retrieval) step aims to find documents that are most "on-topic", or relevant, to the user query.

Vector search, which you learned about earlier, is an effective way to do this:

Re-run the code to embed texts and query_text, and retrieve the two most similar texts to the query text.
```python
from scipy.spatial.distance import cosine
from openai import OpenAI

client = OpenAI()

texts = [
    "AcmeCo is a producer of high-quality widgets",
    "AcmeCo is a hybrid workplace, requiring employees to be in the office at least 3 days a week",
    "Weaviate is a fully remote company with people living and working across the world.",
    "Weaviate provides a home office budget, flexible time off, and local benefits.",
    "Weaviate also allows its employees to connect with colleagues worldwide and enjoy our annual company trip.",
]

response = client.embeddings.create(input=texts, model="text-embedding-3-small")
embeddings = [item.embedding for item in response.data]
query_text = "Weaviate work from home policy"
query_response = client.embeddings.create(input=query_text, model="text-embedding-3-small")
query_embedding = query_response.data[0].embedding

# Calculate cosine distances between the last embedding (query) and the others
distances = []
for i, embedding in enumerate(embeddings):
    distance = cosine(query_embedding, embedding)  # Cosine distance
    distances.append((texts[i], distance))

# Sort by distance value
distances.sort(key=lambda x: x[1])

# Get the top 2 most similar texts
retrieved_texts = [text for text, _ in distances[:2]]
```
#### Augmentation
Once we have retrieved the relevant texts, we can augment the original query with this information.

Create a structured prompt that combines the query_text with the retrieved texts (retrieved_texts).
```python
combined_context = query_text + "\n\n" + "\n\n".join(retrieved_texts)

print(combined_context)
```
```
Weaviate work from home policy

Weaviate provides a home office budget, flexible time off, and local benefits.

Weaviate is a fully remote company with people living and working across the world.
```
This is a good start, but it could be better structured. One problem is that it's difficult to distinguish between the original query and the retrieved texts.

So, let's format the combined context a bit better to help the model interpret the information:
```python
separator = "\n" + "=" * 60 + "\n"

combined_context = f"""
Please answer this question '{query_text}', ONLY using the included information:

Retrieved data:""" + separator + separator.join(retrieved_texts)

print(combined_context)
```
```
Please answer this question 'Weaviate work from home policy', ONLY using the included information:

Retrieved data:
============================================================
Weaviate provides a home office budget, flexible time off, and local benefits.
============================================================
Weaviate is a fully remote company with people living and working across the world.
```
Now, we are ready to send the augmented context to the LLM!

#### Generation　　
Once the combined context is constructed, the only thing left to do is to send it to the LLM to generate a response.

Send combined_context as a request to the OpenAI chat completions model provided.
```python
response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[
        {
            "role": "user",
            "content":combined_context
        }
    ],
)

print(response.choices[0].message.content)
```
```
Weaviate has a fully remote work policy, allowing employees to work from anywhere in the world. They also provide a home office budget, flexible time off, and local benefits.
```
It's that simple.

Even better, this can be done by querying as large a knowledge base as you have available.

Our example uses a very small knowledge base of just a few sentences. But even if you had a much larger knowledge base, the same techniques would apply. You simply need a retrieval system that can quickly find the most relevant documents.

You may have noticed that this is a relatively manual process.

The good news is, we can make this process much simpler, and more scalable, by using an AI-native database such as Weaviate.

Let's take a look at how to do that in the next section.
