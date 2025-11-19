# Introduction to Embeddings with the OpenAI API
---
**Description**  
Text embedding models create numerical representations from text inputs. This ability to encode text and capture its semantic meaning means that embedding models underpin many types of AI applications, like semantic search engines and recommendation engines. In this course, you'll learn how to harness OpenAI's Embeddings model via the OpenAI API to create embeddings from text datasets and begin developing real-world applications. You'll also take a big step towards creating production-ready applications by learning about vector databases to efficiently store and query embedded texts.

---

### What are embeddings?
Which of the following statements about embedding models are correct?
* They create numerical representations of text
* They capture the semantic meaning behind text

*Nice job! This ability to extract the semantic meaning behind text in numerical form unlocks all sorts of different AI applications. Let's see if you can recognize them in the next exercise!*

### Embeddings applications
Embedding models convert text into numerical representations that are able to capture the context and intent behind the text. This unlocks powerful applications beyond what is possible with other types of AI models.

In this exercise, you'll test your ability to identify use cases for embedding models.

*Embeddings can be used for quite a few different tasks, but they really shine in recommendation, classification, and semantic search, so we'll return to these applications throughout the course. Time to create your very first embeddings using the OpenAI API! Best of luck!*

### Creating embeddings
In this exercise, you'll create your very first embeddings using the OpenAI API. Normally, to interact with the OpenAI API, you would need an OpenAI API key, and creating embeddings would incur a cost. However, you do not need to create or provide an API key in this course.

The <OPENAI_API_TOKEN> placeholder has been provided in the code, which will send valid requests for the exercises in this course. If, at any point in the course, you hit a RateLimitError, pause for a moment and try again.

The OpenAI class from the openai library will be imported for you throughout the course, and after this exercise, the client will be created for you.

* Create an OpenAI client (you can leave the api_key set to the placeholder provided).
* Create a request to the Embeddings endpoint, passing the text-embedding-3-small model any text you wish.
* Convert the model response into a dictionary.

> Hint   
> To create a client, create an instance of the OpenAI class.   
> To create a request to the Embeddings endpoint, call the .create() method on the client.embeddings class.   
> The text to embed should be assigned as a string to the input argument.   
> The response has a .model_dump() method for converting to a dictionary.
