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
* Create an OpenAI API client.
* Create a request to the Chat Completions endpoint.
* Specify that the request should use the json_object response format.
* Extract and print the model response.
```python
# Create the OpenAI client
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

# Create the request
response = client.chat.completions.create(
  model="gpt-4o-mini",
  messages=[
   {"role": "user", "content": "I have these notes with book titles and authors: New releases this week! The Beholders by Hester Musson, The Mystery Guest by Nita Prose. Please organize the titles and authors in a json file."}
  ],
  # Specify the response format
  response_format={"type": "json_object"}
)

# Print the response
print(response.choices[0].message.content)
```
```python 
{
  "new_releases": [
    {
      "title": "The Beholders",
      "author": "Hester Musson"
    },
    {
      "title": "The Mystery Guest",
      "author": "Nita Prose"
    }
  ]
}
```
*Excellent work! The code you just completed is a great starting point for you to continue building a well-structured application using the OpenAI API.*

---
### Interpreting error messages
You are debugging an application that makes requests to the OpenAI API and while debugging you run into the following errors. Identify their definition and possible solution among the choices presented.
* Match the definitions and solutions to the corresponding errors.

*Congratulations! Recognizing errors is fundamental for troubleshooting your application. Head to the next exercise for more practice on handling errors!*

### Handling exceptions
You are working at a logistics company on developing an application that uses the OpenAI API to check the shipping address of your top three customers. The application will be used internally and you want to make sure that other teams are presented with an easy to read message in case of error.

To address this requirement, you decide to print a custom message in case the users fail to provide a valid key for authentication, and use a try and except block to handle that.

The message variable has already been imported.
* Use the try statement to attempt making a request to the API.
* Print the response if the request succeeds.
* Use the except statement to handle the authentication error that may occur.
> Hint: You can capture the authentication error using the variable openai.AuthenticationError from the openai library.
```python
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

# Use the try statement
try: 
    response = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[message]
    )
    # Print the response
    print(response.choices[0].message.content)
# Use the except statement
except openai.AuthenticationError as e:
    print("Please double check your authentication key and try again, the one provided is not valid.")
```
> Here is the provided information in JSON format:

```json
[
    {
        "company_name": "PurpleLabs Solutions",
        "address": {
            "street": "123 Main Street",
            "suite": "Suite 100",
            "city": "Anytown",
            "country": "USA"
        }
    },
    {
        "company_name": "InnovateNow Enterprises",
        "address": {
            "street": "789 Oak Avenue",
            "suite": "Suite 300",
            "city": "Innovation City",
            "country": "USA"
        }
    },
    {
        "company_name": "PeakPerformance Inc.",
        "address": {
            "street": "456 Elm Street",
            "suite": "Suite 200",
            "city": "Dreamville",
            "country": "USA"
        }
    }
]
```
*Well done on using the try/except block correctly! Handling exceptions is one of the key components when integrating with production systems.*

---
### Avoiding rate limits with retry
You've created a function to run Chat Completions with a custom message but have noticed it sometimes fails due to rate limits. You decide to use the @retry decorator from the tenacity library to avoid errors when possible.
* Import the tenacity library with required functions: retry, wait_random_exponential, and stop_after_attempt.
* Create an OpenAI API client.
* Complete the retry decorators with the parameters required to start retrying at an interval of 5 seconds, up to 40 seconds, and to stop after 4 attempts
> Hint
> The wait_random_exponential() key is used to set the minimum of 5 seconds and maximum of 40 seconds intervals for retrying.    
> The stop_after_attempt() is used to set the maximum number of @retry attempts to 4.
```python
# Import the tenacity library
from tenacity import retry, wait_random_exponential, stop_after_attempt

client = OpenAI(api_key="<OPENAI_API_TOKEN>")

# Add the appropriate parameters to the decorator
@retry(stop=stop_after_attempt(4), wait=wait_random_exponential(min=5, max=40))
def get_response(model, message):
    response = client.chat.completions.create(
      model=model,
      messages=[message]
    )
    return response.choices[0].message.content
print(get_response("gpt-4o-mini", {"role": "user", "content": "List ten holiday destinations."}))
```
