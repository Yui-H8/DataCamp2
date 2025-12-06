## Retrieval Augmented Generation (RAG) with LangChain 2
---
### Embedding and storing documents
The final step for preparing the documents for retrieval is embedding and storing them. You'll be using the text-embedding-3-small model from OpenAI for embedding the chunked documents, and storing them in a local Chroma vector database.

The chunks you created from splitting the Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks paper recursively have been pre-loaded.

Creating and using an OpenAI API key is not required in this exercise. You can leave the <OPENAI_API_TOKEN> placeholder, which will send valid requests to the OpenAI API.
* Initialize the default embedding model from OpenAI.
* Embed the document chunks using embedding_model and store them in a Chroma vector database.
> Hint
> The OpenAIEmbeddings class can be used to access embedding models from OpenAI.
> The Chroma.from_documents() method can be used to embed and store document chunks in one step
```python
# Initialize the OpenAI embedding model
embedding_model = OpenAIEmbeddings(api_key="<OPENAI_API_TOKEN>", model='text-embedding-3-small')

# Create a Chroma vector store and embed the chunks
vector_store = Chroma.from_documents(
    documents=chunks,        # 分割済み文書を渡す
    embedding=embedding_model # Embeddingモデルを渡す
)
```
*You did it! With your document chunks embedded and stored, everything is in place to connect your vector store to an LLM and begin talking to your documents. Head on over to the next video to learn how to do this final step!*

---
### Creating the retrieval prompt
