# End-to-End RAG with Weaviate2
---

### Creating Text Embeddings
* Embed the text string provided and view its length and the first five numbers in the embedding vector.
* Embed the strings in the texts list in a single embedding API request, and compare the length and numbers of each embedding vector.
* Embed the texts list again, but this time, change the model output dimensions to 512.
  
**What are vector embeddings?**    
Vector embeddings represent semantic meaning as a set of numbers. The key concept of vector embeddings is that similar meanings or concepts are close together in this space.
```python
from openai import OpenAI
client = OpenAI()
```
```Python
# Single embedding
response = client.embeddings.create(
    input="Weaviate is a fully remote company with people living and working across the world.",
    model="text-embedding-3-small",
)
```
Let's inspect the embedding:
```python
print(f"Embedding length: {len(response.data[0].embedding)}")
print(f"Embedding: {response.data[0].embedding[:5]}...")
```
```
Embedding length: 1536
Embedding: [0.0029289894737303257, 0.02506072074174881, 0.05786540359258652, 0.06938133388757706, -0.003239748068153858]...
```
The output embedding is a list of numbers. It is also called a "vector" as a generic name for multi-dimensional numbers.

**Generating multiple embeddings**    
Multiple embeddings can be generated at once. This is usually more efficient than generating them one by one.

Embed the strings in the texts list in a single embedding API request, and compare the length and numbers of each embedding vector.
```Python
# Multiple embeddings
texts = [
    "Weaviate is a fully remote company with people living and working across the world.",
    "Weaviate provides a home office budget, flexible time off, and local benefits.",
    "Weaviate also allows its employees to connect with colleagues worldwide and enjoy our annual company trip."
]

batch_response = client.embeddings.create(
    input=texts,
    model="text-embedding-3-small"
)
```
```python
# Inspect the embeddings
for i, embedding in enumerate(batch_response.data):
    print(f"Source text: {texts[i]}")
    print(f"Embedding {i+1}: {embedding.embedding[:5]}...")  # Print first 10 elements of each embedding
    print(f"Length: {len(embedding.embedding)}\n")
```
```
Source text: Weaviate is a fully remote company with people living and working across the world.
Embedding 1: [0.002926233457401395, 0.025080980733036995, 0.05781766027212143, 0.06941547244787216, -0.0032547428272664547]...
Length: 1536

Source text: Weaviate provides a home office budget, flexible time off, and local benefits.
Embedding 2: [-0.020389819517731667, 0.020850686356425285, 0.10256358981132507, 0.06301292032003403, -0.017484968528151512]...
Length: 1536

Source text: Weaviate also allows its employees to connect with colleagues worldwide and enjoy our annual company trip.
Embedding 3: [0.014970372430980206, 0.005227535497397184, 0.03144584596157074, 0.05697879567742348, -0.011086676269769669]...
Length: 1536
```
All embeddings from the same model configuration have the same length.

As a demonstration, let's try a setup with a different parameter to generate embeddings (note the dimensions parameter).

Embed the texts list again, but this time, change the model output dimensions to 512.
```python
# Multiple embeddings
texts = [
    "Weaviate is a fully remote company with people living and working across the world.",
    "Weaviate provides a home office budget, flexible time off, and local benefits.",
    "Weaviate also allows its employees to connect with colleagues worldwide and enjoy our annual company trip."
]

batch_dim_response = client.embeddings.create(
    input=texts,
    model="text-embedding-3-small",
    dimensions = 512
)

# Inspect the embeddings
for i, embedding in enumerate(batch_dim_response.data):
    print(f"Source text: {texts[i]}")
    print(f"Embedding {i+1}: {embedding.embedding[:5]}...")  # Print first few elements of each embedding
    print(f"Length: {len(embedding.embedding)}\n")
```
```
Source text: Weaviate is a fully remote company with people living and working across the world.
Embedding 1: [0.004319458268582821, 0.03695770725607872, 0.0853356346487999, 0.10231848061084747, -0.004777742084115744]...
Length: 512

Source text: Weaviate provides a home office budget, flexible time off, and local benefits.
Embedding 2: [-0.030071353539824486, 0.030708981677889824, 0.15105609595775604, 0.0928879827260971, -0.025793075561523438]...
Length: 512

Source text: Weaviate also allows its employees to connect with colleagues worldwide and enjoy our annual company trip.
Embedding 3: [0.021647470071911812, 0.007643715478479862, 0.04547329992055893, 0.0823887512087822, -0.016065416857600212]...
Length: 512
```
Each one of these embedding vectors is still the same length, but that length is now 512 instead of 1536

An embedding model is like a "translator" that translates the meaning of text into a set of numbers. Each model setup will generate outputs in its own different "language" of numbers.

*Great job! Now that you learned to encode the semantic meaning of these texts, you're ready to begin comparing embeddings to find which ones are the most similar. This is what powers the retrieval part of the RAG workflow!*

---
### Comparing Embeddings
* Embed the two texts provided about Weaviate and compute the cosine distance between the two vectors.
* Embed a third text about a different topic, compute the cosine distance to e1, and compare this distance to the previous distance.
* Embed the strings in texts along with the query_text variable, loop over embeddings, calculating the distance between the embedding and query_embedding, and sort the distances to find the top 2 most similar texts.
* Return all of the texts with a cosine distance less than max_distance.

**Metrics for comparing embeddings**　　
Embeddings can be compared for their semantic similarity. Typically, a "cosine" distance is used for this.

Embed the two texts provided about Weaviate and compute the cosine distance between the two vectors.
```pyton
from openai import OpenAI

client = OpenAI()

response = client.embeddings.create(
    input=[
        "Weaviate is a fully remote company with people living and working across the world.",
        "Weaviate provides a home office budget, flexible time off, and local benefits.",
    ],
    model="text-embedding-3-small",
)

e1 = response.data[0].embedding
e2 = response.data[1].embedding
```
We can use scipy to compare these embeddings:
```python
from scipy.spatial.distance import cosine

e1_e2_distance = cosine(e1, e2)
print(f"Cosine distance: {e1_e2_distance:.3f}")
```
```
Cosine distance: 0.310
```
Embed a third text about a different topic, compute the cosine distance between e1 and e3, and compare this distance to previous distance.
```python
response = client.embeddings.create(input="AcmeCo is a producer of high-quality widgets", model="text-embedding-3-small")
e3 = response.data[0].embedding

e1_e3_distance = cosine(e1, e3)
print(f"Cosine distance: {e1_e3_distance:.3f}")
```
```
Cosine distance: 0.744
```

You can see that the more similar the meanings, the lower the cosine distance.

This makes embeddings great for information retrieval. These distances can be used to rank documents by their relevance to a query.

Embed the strings in texts along with the query_text variable, loop over each embedded text (embeddings), calculating the distance between the embedding and query_embedding, and sort the distances to find the top 2 most similar texts.
```python
texts = [
    "AcmeCo is a producer of high-quality widgets",
    "AcmeCo is a hybrid workplace, requiring employees to be in the office at least 3 days a week",
    "Weaviate is a fully remote company with people living and working across the world.",
    "Weaviate provides a home office budget, flexible time off, and local benefits.",
    "Weaviate also allows its employees to connect with colleagues worldwide and enjoy our annual company trip.",
]

response = client.embeddings.create(input=texts, model="text-embedding-3-small")
embeddings = [item.embedding for item in response.data]
```
Embed the query:
```python
query_text = "Weaviate work from home policy"
query_response = client.embeddings.create(input=query_text, model="text-embedding-3-small")
query_embedding = query_response.data[0].embedding
```
Calculate the cosine distance between the query and each document:
```python
# Calculate cosine distances between the last embedding (query) and the others
distances = []
for i, embedding in enumerate(embeddings):
    distance = cosine(query_embedding, embedding)  # Cosine distance
    distances.append((texts[i], distance))

print(distances)
```
```
[('AcmeCo is a producer of high-quality widgets', 0.7889421930731979), ('AcmeCo is a hybrid workplace, requiring employees to be in the office at least 3 days a week', 0.6031844472646926), ('Weaviate is a fully remote company with people living and working across the world.', 0.3271398828066763), ('Weaviate provides a home office budget, flexible time off, and local benefits.', 0.29224175408450226), ('Weaviate also allows its employees to connect with colleagues worldwide and enjoy our annual company trip.', 0.442520047441344)]
```
With these, you can retrieve the relevant context. From this, you can narrow down to the top-k most similar documents:
```python
# Sort by distance value
distances.sort(key=lambda x: x[1])
```

```
# Get the top 2 most similar texts
retrieved_texts = [text for text, _ in distances[:2]]
```
Or list all documents within a certain distance threshold:

Return all of the texts with a cosine distance less than max_distance.
```python
# Get text
max_distance = 0.6  # Adjust this threshold as needed
retrieved_texts = [text for text, d in distances if d < max_distance]
```
```python

print(f"Retrieved texts below distance threshold ({max_distance}) :\n" + "-" * 30)
for text in retrieved_texts:
    print(f"- {text}")
```
```
Retrieved texts below distance threshold (0.6) :
------------------------------
- Weaviate provides a home office budget, flexible time off, and local benefits.
- Weaviate is a fully remote company with people living and working across the world.
- Weaviate also allows its employees to connect with colleagues worldwide and enjoy our annual company trip
```
Both methods are useful, depending on your circumstances. For the rest of our course, we'll use the top-k most similar documents.

This ability to find similar objects is the "retrieval" part of "retrieval-augmented generation" (RAG). Next, you will see how the rest of the RAG pipeline works.

*Nice job! What you've done here is perform the retrieval part of the RAG process (the "R") by-hand. Let's put it all together and complete the full workflow with the "A" and "G".*
