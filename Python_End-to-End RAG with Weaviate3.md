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
