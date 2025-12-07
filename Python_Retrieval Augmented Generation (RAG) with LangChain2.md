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
A key piece of any RAG implementation is the retrieval prompt. In this exercise, you'll create a chat prompt template for your retrieval chain and test that the LLM is able to respond using only the context provided.

An llm has already been defined for you to use.
* Convert the string prompt into a reusable chat prompt template.
* Create an LCEL chain to integrate the prompt template with the llm provided.
* Invoke the chain on the inputs provided to see if you model can respond using only the context provided.
> Hint    
> The the ChatPromptTemplate.from_template() method can be used to convert prompt strings into chat prompt templates.    
> When integrating prompt templates and LLMs using LCEL (LangChain Expression Language), start with the prompt template.
```python
prompt = """
Use the only the context provided to answer the following question. If you don't know the answer, reply that you are unsure.
Context: {context}
Question: {question}
"""

# Convert the string into a chat prompt template
prompt_template = ChatPromptTemplate.from_template(prompt)

# Create an LCEL chain to test the prompt
chain = prompt_template | llm

# Invoke the chain on the inputs provided
print(chain.invoke({"context": "DataCamp's RAG course was created by Meri Nova and James Chapman!", "question": "Who created DataCamp's RAG course?"}))
```
```
content="DataCamp's RAG course was created by Meri Nova and James Chapman." additional_kwargs={'refusal': None} response_metadata={'token_usage': {'completion_tokens': 16, 'prompt_tokens': 62, 'total_tokens': 78, 'completion_tokens_details': {'accepted_prediction_tokens': 0, 'audio_tokens': 0, 'reasoning_tokens': 0, 'rejected_prediction_tokens': 0}, 'prompt_tokens_details': {'audio_tokens': 0, 'cached_tokens': 0}}, 'model_name': 'gpt-4o-mini-2024-07-18', 'system_fingerprint': 'fp_aa07c96156', 'finish_reason': 'stop', 'logprobs': None} id='run-71d750b9-ff59-469d-ac9d-ea80c87756d4-0' usage_metadata={'input_tokens': 62, 'output_tokens': 16, 'total_tokens': 78, 'input_token_details': {'audio': 0, 'cache_read': 0}, 'output_token_details': {'audio': 0, 'reasoning': 0}}
```
*You're doing great! Your test has provided confidence that the model is able to provide responses based purely on the context provided. You might extend this to create a dataset of test questions to evaluate your prompt/model combination. Now for the final step: creating a retrieval chain that will connect your vector store, prompt template, and LLM. You've got this!*

### Building the retrieval chain
Now for the finale of the chapter! You'll create a retrieval chain using LangChain's Expression Language (LCEL). This will combine the vector store containing your embedded document chunks from the RAG paper you loaded earlier, a prompt template, and an LLM so you can begin talking to your documents.

Here's a reminder of the prompt_template you created in the previous exercise, and which is available for you to use:
```
Use the only the context provided to answer the following question. If you don't know the answer, reply that you are unsure.
Context: {context}
Question: {question}
```
The vector_store of embedded document chunks that you created previously has also been loaded for you, along with all of the libraries and classes required.
* Convert the Chroma vector_store into a retriever object for use in the LCEL retrieval chain.
* Create the LCEL retrieval chain to combine the retriever, the prompt_template, the llm, and a string output parser so it can answer input questions.
* Invoke the chain on the question provided.
> Hint    
> The .as_retriever() method is used to convert vector stores into retrievers; this method can take a dictionary of search_kwargs to change the retriever's configuration.     
> RunnablePassthrough() is used to indicate where an input will enter the chain.     
> StrOutputParser() should be the final step in the chain to convert the model response into a string.
```python
# Convert the vector store into a retriever
retriever = vector_store.as_retriever(search_type="similarity", search_kwargs={"k": 2})

# Create the LCEL retrieval chain
chain = (
    {"context": retriever, "question": RunnablePassthrough()}
    | prompt_template
    | llm
    | StrOutputParser()
)

# Invoke the chain
print(chain.invoke("Who are the authors?"))
```
