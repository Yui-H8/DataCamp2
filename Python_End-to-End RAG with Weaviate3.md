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
