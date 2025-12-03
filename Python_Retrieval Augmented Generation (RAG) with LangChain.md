# Retrieval Augmented Generation (RAG) with LangChain
---

Large Language Models (LLMs) are being integrated into computers, phones, and software applications, but they do have one drawback: their knowledge is limited by their training data, which is slow and costly. Enter Retrieval Augmented Generation (RAG)! RAG enables you to integrate external data with LLMs. In this course, you'll learn state-of-the-art techniques for loading, processing, and retrieving external data for LLMs! You'll utilize vector databases, the latest LLMs, including GPT-4o-Mini, and the LangChain framework to create RAG applications. This course concludes with a chapter on Graph RAG, a twist on traditional RAG that uses graph databases for more reliable data retrieval.

---
### Loading PDF files for RAG
To begin implementing Retrieval Augmented Generation (RAG), you'll first need to load the documents that the model will access. These documents can come from a variety of sources, and LangChain supports document loaders for many of them.

In this exercise, you'll use a document loader to load a PDF document containing the paper, Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks by Lewis et al. (2021). This file is available for you as 'rag_paper.pdf'.

Note: pypdf, a dependency for loading PDF documents in LangChain, has already been installed for you.
* Import the appropriate class for loading PDF documents in LangChain.
* Create a document loader for the 'rag_paper.pdf' document.
* Load the document into memory to view the contents of the first document, or page.
> Hint
> The PyPDFLoader class can load PDFs from a given file path.
> Load the document into memory with the .load() method.
