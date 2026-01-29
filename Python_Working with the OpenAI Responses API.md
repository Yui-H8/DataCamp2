# Working with the OpenAI Responses API
---
**Description**

Building AI applications has never been more accessible with the Responses API! In this course, you’ll learn how to prompt models through a simple interface, follow best practices for reasoning models, and design natural back-and-forth conversations with LLMs. You’ll unlock the full power of LLMs by integrating tools—like web search with no extra API keys or credentials—and by building your own to extend model capabilities beyond their base knowledge. You’ll also learn how to take your applications from prototype to production with three key design paradigms, generate reliable structured outputs like JSON, stream real-time semantic events for interactive user experiences, and go multi-modal by combining text and images using OpenAI’s GPT-5 models.

---
### Your First Responses API Call
Time to get hands-on with OpenAI's Responses API! You'll be using the openai library to begin making requests to the Responses API endpoint.

The OpenAI class has already been imported from openai.
* Define an OpenAI API client, leaving the api_key placeholder unchanged.
* Create an OpenAI API request to the "gpt-5-mini" model, sending the prompt "In simple terms, what is the OpenAI Responses API?"; leave the reasoning and max_output_tokens parameter unchanged.
* Print the generated text from the response object.
```
Hint
To create an OpenAI client, use the OpenAI class from the openai library.
You can create requests to the Responses API endpoint using client.responses.create().
User prompts should be sent to the input argument.
```
```python
# Define an OpenAI API client
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

# Create the OpenAI API request
response = client.responses.create(
    model="gpt-5-mini",
    input="In simple terms, what is the OpenAI Responses API?",
    reasoning={"effort": "minimal"},
    max_output_tokens=100
)

# Print the generated text from the response
print(response.output_text)
```
```
<script.py> output:
    The OpenAI Responses API is a way for programs to talk to OpenAI models (like GPT) and get helpful answers, text, or actions back. Instead of calling lots of different endpoints for chat, images, or tools, the Responses API provides a single, flexible interface where you send a prompt or instructions and the model returns a structured response. 
    
    Key points in simple terms:
    - You send a request describing
```
*Nice job! The syntax for requests to the Responses API is intentionally simple, yet powerful. As you progress, you'll learn to master the various response parameters, and even integrate tools for the model to use!*

### Adding Model Instructions
