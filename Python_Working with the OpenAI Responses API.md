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
You're building a customer support AI system for a financial services company. For security and compliance reasons, the AI have clear guardrails to only answer questions about account balances and transaction history. It should politely decline any requests for password resets, loan applications, or investment advice.

The OpenAI client is already available for you to use as client. This will be available throughout the remainder of the course.
* Create a Responses API request to "gpt-5-mini" that contains clear instructions to only respond to questions about account balances and transaction history, and not other requests for password resets, loan applications, or investment advice, specifically.
```python
# Create a guardrailed AI response
response = client.responses.create(
    model="gpt-5-mini",
    instructions=""" only respond to questions about account balances and transaction history, and not other requests for password resets, loan applications, or investment advice """,
    input="Can you help me reset my password?"
)

print(response.output_text)
```
```
    Sorry — I can’t help with password resets. I can only assist with questions about account balances or transaction history. If you’d like, tell me which account and what date range or how many recent transactions you want to see, and I’ll help with that.
```
*Excellent work! You've successfully restricted the AI's capabilities using clear model instructions. This approach is crucial for building safe and compliant AI systems that stay within their intended scope.*

### Extracting Information from the Response
With your request made and the response received from the OpenAI API, it's time to begin extracting information from it. The response from the Responses API contains more than just the model output, it contains structured outputs for chat applications, and useful metadata for tracking and building a chat history.

A response object is already available for you to investigate as response.
1. Extract the number of output tokens from response.
```
Hint
You can extract the .output_tokens attribute from the object extracted from the response using the .usage attribute.
```
```python
# Extract the number of output tokens
print(response.usage.output_tokens)
```
```
50
```
2. Extract the ID from response.
```
Hint
The ID can be found in the response object's .id attribute.
```
```Python
# Extract the response ID
print(response.id)
```
```
resp_0e8c8a2849bbfbf500697bca1e5b8881969bc9e906f47586db
```
3. Extract the output in a structured way by unpacking and printing each item in response.
```
Hint
You can access a structured output from the response using the .output attribute.
```
```python
# Extract and print each item in the response
for item in response.output:
    print(item)
```
```
ResponseReasoningItem(id='rs_0e2ed9b8d01fd14600697bca4005088197b0198e32bb288342', summary=[], type='reasoning', content=None, encrypted_content=None, status=None)
ResponseOutputMessage(id='msg_0e2ed9b8d01fd14600697bca402bb48197bfc53754c26d4f82', content=[ResponseOutputText(annotations=[], text='The OpenAI Responses API is a unified endpoint that lets developers send prompts, instructions, or conversation data to access a variety of OpenAI models (including text, code', type='output_text', logprobs=[])], role='assistant', status='incomplete', type='message')
```
*Excellent extraction! As you've seen, the Responses API makes it elegantly simple for developers to build AI applications. In the next video, you'll learn how to use different parameters to control for output quality, length, and cost—key measures in any Generative AI app!*

### Experimenting with More Powerful Models
Let's experiment with some of the different LLMs that OpenAI has to offer. All of these models have reasoning capabilities, but to make the differences between the base models clearer, keep the reasoning effort to 'minimal'.

In this exercise, you can enter any prompts you wish, but try to use the same one in each step to make a clearer comparison! If you're struggling for ideas, you can use the one provided.

1. Send a prompt to the "gpt-5-nano" model with reasoning effort reduced to 'minimal'.
```python
# Prompt gpt-5-nano with minimal reasoning
response = client.responses.create(
    model="gpt-5-nano",
    input='Write the same sentence in three tones: professional, sarcastic, and poetic. The sentence is: "The meeting could have been an email."',
  reasoning={"effort": "minimal"}
)

print(response.output_text)
```
```
Sure. Here it is in three tones:

- Professional: The meeting could have been communicated effectively via email.
- Sarcastic: Oh great, another meeting—that could have been an email, you know.
- Poetic: A chorus of minutes and murmurs, when a single email would have carried the truth.
```
2. Send the same prompt to the gpt-5-mini model; again, with reasoning effort reduced to 'minimal'.
```python
# Prompt gpt-5-mini with minimal reasoning
response = client.responses.create(
    model="gpt-5-mini",
    input='Write the same sentence in three tones: professional, sarcastic, and poetic. The sentence is: "The meeting could have been an email."',
  reasoning={"effort": "minimal"}
)

print(response.output_text)
```
```
Professional: That meeting’s content could have been conveyed more efficiently via email.

Sarcastic: Oh great — another meeting that absolutely could not possibly have been an email.

Poetic: Words gathered in a room where ink on a quiet page would have done the work.

```
3. Repeat your prompt one last time, this time, to the gpt-5 model.
```python
# Prompt gpt-5 with minimal reasoning
response = client.responses.create(
    model="gpt-5",
    input='Write the same sentence in three tones: professional, sarcastic, and poetic. The sentence is: "The meeting could have been an email."',
  reasoning={"effort": "minimal"}
)

print(response.output_text)
```
```
- Professional: The information covered in the meeting would have been more efficiently communicated via email.
- Sarcastic: Wow, what a thrilling hour to discover everything could’ve been in a two-line email.
- Poetic: A room of voices stirred the air, yet all we needed was a whisper on the wire.
```
*Excellent experimentation! Remember, the best model is the one that meets your quality and speed requirements for the lowest cost.*

### Reasoning About Reasoning
Now you've experimented with the different models available, let's try out the two other parameters introduced in the video: reasoning and max_output_tokens.

Here's the challenge:
