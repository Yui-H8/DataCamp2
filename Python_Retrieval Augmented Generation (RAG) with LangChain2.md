## Retrieval Augmented Generation (RAG) with LangChain 2
---
### Embedding and storing documents
The final step for preparing the documents for retrieval is embedding and storing them. You'll be using the text-embedding-3-small model from OpenAI for embedding the chunked documents, and storing them in a local Chroma vector database.

The chunks you created from splitting the Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks paper recursively have been pre-loaded.

Creating and using an OpenAI API key is not required in this exercise. You can leave the <OPENAI_API_TOKEN> placeholder, which will send valid requests to the OpenAI API.
