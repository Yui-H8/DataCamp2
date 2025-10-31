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
<script.py>
output:
    Sure! Here are ten popular holiday destinations:
    
    1. **Bali, Indonesia** - Known for its stunning beaches, lush rice terraces, and vibrant culture.
    2. **Paris, France** - Famous for its iconic landmarks, gourmet cuisine, and romantic ambiance.
    3. **Tokyo, Japan** - A bustling metropolis blending traditional culture with modern technology, offering unique experiences and cuisine.
    4. **Santorini, Greece** - Renowned for its white-washed buildings, beautiful sunsets, and crystal-clear waters.
    5. **Maui, Hawaii, USA** - Offers breathtaking landscapes, luxury resorts, and beautiful beaches.
    6. **Rome, Italy** - A city rich in history, known for its ancient ruins, art, and delicious food.
    7. **Cape Town, South Africa** - Famous for its stunning landscapes, including the Table Mountain and nearby vineyards.
    8. **Reykjavik, Iceland** - Known for its unique natural beauty, hot springs, and the chance to see the Northern Lights.
    9. **Sydney, Australia** - Known for its iconic Opera House, beautiful harbor beaches, and vibrant urban culture.
    10. **Cancun, Mexico** - Popular for its beautiful beaches, luxurious resorts, and vibrant nightlife.
    
    Each of these destinations offers a unique experience for travelers!
  </script.py>

  
*Well done completing the exercise! Retrying is a helpful way to handle potential errors when a greater amount of traffic is expected on the application. In the next exercises, you'll explore more ways to set up your application to avoid rate limits.*

### Batching messages
You are developing a fitness application to track running and cycling training, but find out that all your customers' distances have been measured in kilometers, and you'd like to have them also converted to miles.

You decide to use the OpenAI API to send requests for each measurement, but want to avoid using a for loop that would send too many requests. You decide to send the requests in batches, specifying a system message that asks to convert each of the measurements from kilometers to miles and present the results in a table containing both the original and converted measurements.

The measurements list (containing a list of floats) and the get_response() function have already been imported.
* Provide a system message to request a response with all measurements as a table (make sure you specify that they are in kilometers and should be converted into miles).
* Append one user message per measurement to the messages list.
> Hint   
> The system message should contain the request for conversion into miles, and also for the table format with all measurements.
> You can use a list comprehension to append the measurements to the message.
```python
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

messages = []
# Provide a system message and user messages to send the batch
messages.append({
            "role": "system",
            "content": "Convert each measurement, given in kilometers, into miles, and reply with a table of all measurements."
        })
# Append measurements to the message
[messages.append({"role": "user", "content": str(i) }) for i in measurements]

response = get_response(messages)
print(response)
```
```
  To convert kilometers to miles, you multiply the kilometers by 0.621371. Here are the conversions for the given measurements:

| Kilometers | Miles    |
|------------|----------|
| 5.2        | 3.228    |
| 6.3        | 3.913    |
| 3.7        | 2.298    |

If you have more measurements to convert, feel free to share!
```
*Great work using batching to avoid sending multiple requests! Through batching you can improve efficiency by consolidating multiple operations into a single request. Excellent way to handle your requests!*

### Setting token limits
An e-commerce platform just hired you to improve the performance of their customer service bot built using the OpenAI API. You've decided to start by ensuring that the input messages do not cause any rate limit issue by setting a limit of 100 tokens, and test it with a sample input message.

The tiktoken library has been preloaded.
* Use the tiktoken library to create an encoding for the gpt-4o-mini model.
* Check for the expected number of tokens in the input message.
* Print the response if the message passes both checks.
> Hint   
> Use .encoding_for_model() to obtain the encoding.   
> Use .encode() to obtain tokens from your message.
```python
client = OpenAI(api_key="<OPENAI_API_TOKEN>")
input_message = {"role": "user", "content": "I'd like to buy a shirt and a jacket. Can you suggest two color pairings for these items?"}

# Use tiktoken to create the encoding for your model
encoding = tiktoken.encoding_for_model("gpt-4o-mini")
# Check for the number of tokens
num_tokens = len(encoding.encode(input_message['content']))

# Run the chat completions function and print the response
if num_tokens <= 100:
    response = client.chat.completions.create(model="gpt-4o-mini", messages=[input_message])
    print(response.choices[0].message.content)
else:
    print("Message exceeds token limit")
```
```
なるほど、見比べてみるといくつか微妙な違いがあるね。順番に解説するよ🙂

---

### 1️⃣ `response.choices[0].message.content` の書き方

* 正しいアクセス方法は **辞書スタイル** か **属性スタイル** のどちらかで呼べる場合があるけど、OpenAIの公式Python SDK v1系では、チャットの返答はこう書くのが安全：

```python
response.choices[0].message["content"]
```

* あなたのコードだと `response.choices[0].message.content` としているので、場合によっては `AttributeError` が出ることがある。

---

### 2️⃣ トークン上限

```python
if num_tokens <= 100:
```

* GPT-4o-mini は実際には **最大トークン数 4096～8192** くらいある。
* `100` にしてしまうと、短すぎてほとんどのメッセージが「Message exceeds token limit」と判定されやすい。
* 学習用や例としては OK だけど、実運用ではモデルの最大トークン数をチェックする方が現実的。

---

### 3️⃣ 基本的にはコードのロジック自体は同じ

* `encoding = tiktoken.encoding_for_model("gpt-4o-mini")` ✅
* `num_tokens = len(encoding.encode(input_message['content']))` ✅
* if 文でチェックしてレスポンス取得 ✅

違いは **トークン上限の値** と **response へのアクセス方法** の2点くらい。

---

💡 まとめると：

* SDKのバージョンによっては `response.choices[0].message["content"]` の方が安全
* トークン上限は実際のモデルに合わせる
* ロジック自体は問題ない

必要なら、**安全にトークン上限をチェックしてレスポンスを出力する修正版**を書けるよ。書こうか？
```
