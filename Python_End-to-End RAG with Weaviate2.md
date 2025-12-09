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
