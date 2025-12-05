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
```python
# Import library
from langchain_community.document_loaders import PyPDFLoader

# Create a document loader for rag_paper.pdf
loader = PyPDFLoader('rag_paper.pdf')

# Load the document
data = loader.load()
print(data[0])
```
```
age_content='Retrieval-Augmented Generation for
Knowledge-Intensive NLP Tasks
Patrick Lewis†‡, Ethan Perez⋆,
Aleksandra Piktus†, Fabio Petroni†, Vladimir Karpukhin†, Naman Goyal†, Heinrich Küttler†,
Mike Lewis†, Wen-tau Yih†, Tim Rocktäschel†‡, Sebastian Riedel†‡, Douwe Kiela†
†Facebook AI Research; ‡University College London; ⋆New York University;
plewis@fb.com
Abstract
Large pre-trained language models have been shown to store factual knowledge
in their parameters, and achieve state-of-the-art results when ﬁne-tuned on down-
stream NLP tasks. However, their ability to access and precisely manipulate knowl-
edge is still limited, and hence on knowledge-intensive tasks, their performance
lags behind task-speciﬁc architectures. Additionally, providing provenance for their
decisions and updating their world knowledge remain open research problems. Pre-
trained models with a differentiable access mechanism to explicit non-parametric
memory have so far been only investigated for extractive downstream tasks. We
explore a general-purpose ﬁne-tuning recipe for retrieval-augmented generation
(RAG) — models which combine pre-trained parametric and non-parametric mem-
ory for language generation. We introduce RAG models where the parametric
memory is a pre-trained seq2seq model and the non-parametric memory is a dense
vector index of Wikipedia, accessed with a pre-trained neural retriever. We com-
pare two RAG formulations, one which conditions on the same retrieved passages
across the whole generated sequence, and another which can use different passages
per token. We ﬁne-tune and evaluate our models on a wide range of knowledge-
intensive NLP tasks and set the state of the art on three open domain QA tasks,
outperforming parametric seq2seq models and task-speciﬁc retrieve-and-extract
architectures. For language generation tasks, we ﬁnd that RAG models generate
more speciﬁc, diverse and factual language than a state-of-the-art parametric-only
seq2seq baseline.
1 Introduction
Pre-trained neural language models have been shown to learn a substantial amount of in-depth knowl-
edge from data [47]. They can do so without any access to an external memory, as a parameterized
implicit knowledge base [51, 52]. While this development is exciting, such models do have down-
sides: They cannot easily expand or revise their memory, can’t straightforwardly provide insight into
their predictions, and may produce “hallucinations” [38]. Hybrid models that combine parametric
memory with non-parametric (i.e., retrieval-based) memories [20, 26, 48] can address some of these
issues because knowledge can be directly revised and expanded, and accessed knowledge can be
inspected and interpreted. REALM [ 20] and ORQA [ 31], two recently introduced models that
combine masked language models [8] with a differentiable retriever, have shown promising results,
arXiv:2005.11401v4  [cs.CL]  12 Apr 2021' metadata={'source': 'rag_paper.pdf', 'page': 0}
In [1]:
```
*Great work! PDFs are incredibly common in business and academia, and with a RAG workflow, you can start having conversations with them!*

### Loading HTML files for RAG
's possible to load documents from many different formats, including complex formats like HTML.

If you're not familiar with HTML, it's a markup language for creating web pages. Here's a small example:
```HTML
<!DOCTYPE html>
<html>
<body>
  <h2>Heading</h2>
  <p>Here's some text and an image below:</p>
  <img src="image.jpg" alt="..." width="104" height="142">
</body>
</html>
```
In this exercise, you'll load an HTML file taken containing a DataCamp blog post webpage. The necessary classes have already been imported for you.
* Use the UnstructuredHTMLLoader class to load the datacamp-blog.html file in the current directory.
* Load the documents into memory.
* Print the first document's page content.
* Print the first document's metadata.
> Hint    
> To access a document loader's metadata, access its .metadata attribute.    
> To access a document loader's content, use the .page_content attribute
```python
# Create a document loader for unstructured HTML
loader = UnstructuredHTMLLoader("datacamp-blog.html")

# Load the document
data = loader.load()

# Print the first document's content
print(data[0].page_content)

# Print the first document's metadata
print(data[0].metadata)
```
*You star! You could combine web scraping and LangChain to create a RAG workflow that allows you to speak to webpages. Imagine that! Head on over to the next video to learn about the next step in setting-up a RAG workflow: document splitting!*

---
### Getting started with text splitting
Time to start splitting! You've been provided with a statement about RAG stored in the string variable text. Your job is to split this string on occurrences of the '.' character. Take a look at the splitting results to see how this strategy performed.
* Define a LangChain character text splitter that will split on the '.' character with a chunk size of 75 and chunk overlap of 10.
* Split text using the text_splitter you defined.
> Hint   
> To split text into chunks based on a specific character, use the CharacterTextSplitter class and the separator argument.   
> To apply a LangChain splitter to a string, call the .split_text() method.
```Python
text = '''RAG (retrieval augmented generation) is an advanced NLP model that combines retrieval mechanisms with generative capabilities. RAG aims to improve the accuracy and relevance of its outputs by grounding responses in precise, contextually appropriate data.'''

# Define a text splitter that splits on the '.' character
text_splitter = CharacterTextSplitter(
    separator = '.',
    chunk_size = 75,  
    chunk_overlap = 10  
)

# Split the text using text_splitter
chunks = text_splitter.split_text(text)
print(chunks)
print([len(chunk) for chunk in chunks])
```
```
<script.py> output:
    ['RAG (retrieval augmented generation) is an advanced NLP model that combines retrieval mechanisms with generative capabilities', 'RAG aims to improve the accuracy and relevance of its outputs by grounding responses in precise, contextually appropriate data']
    [125, 126] 
```
*Well done! CharacterTextSplitter's strength is in its simplicity, but splitting this way can result in meaningless chunks and loss of context between chunks. Additionally, CharacterTextSplitter can fail to create chunks smaller than chunk_size, so let's move on to a method that can help mitigate that risk: RecursiveCharacterTextSplitter.*

### Recursively splitting documents
Splitting on a single character is simple and predictable, but it often produces sub-optimal chunks. In this exercise, you'll apply recursive character splitting to split the Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks paper you loaded in a earlier exercise.

Recall that recursive character splitting iterates over a list of characters, splitting on each in turn to see if chunks can be created beneath the chunk_size limit.
* Define a LangChain recursive character text splitter to split recursively through the character list ['\n', '.', ' ', ''] with a chunk size of 75 and chunk overlap of 10.
* Split document using the text_splitter you defined and an appropriate method.
> Hint   
> To split text into chunks based on a specific character, use the RecursiveCharacterTextSplitter class and its separators argument.   
> To apply a LangChain splitter to a loaded document, call the .split_documents() method.
