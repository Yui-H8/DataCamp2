# Developing AI Systems with the OpenAI API
---
### Decoding the response
The format of the output response from the Chat Completions endpoint is an object where the message can be accessed by selecting the appropriate fields.

In this example, you've just submitted a request to the API to list three Python libraries with the year they were first released, and it has returned a response.

For the response shown, what is the correct way to extract the content of the message only?
```Python
In [2]:
response.choices[0].message.content
Out[2]:
'Here are three popular Python libraries along with their release years:\n\n1. **NumPy**: First released in 2006.\n2. **Pandas**: First released in 2008.\n3. **Matplotlib**: First released in 2003.\n\nThese libraries have become fundamental tools for data analysis and scientific computing in Python.'
```

*Great job untangling the response message! You'll be using the Chat Completions endpoint throughout the course, and accessing the response message is key in using the endpoint effectively.*

### Formatting model response as JSON
As a librarian cataloging new books, you aim to leverage the OpenAI API to automate the creation of a JSON file from text notes you received from a colleague. Your task is to extract relevant information such as book titles and authors and to do this, you use the OpenAI API to convert the text notes, that include book titles and authors, into structured JSON files.

In this and all the following exercises, the openai library has already been loaded. Entering your own API key is not necessary to create requests and complete the exercises in this course; however, you may do so if you prefer.
