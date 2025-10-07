# Introduction to Amazon Bedrock
---
### Initializing the Bedrock client
* Import the boto3 library.
* Verify the connection by listing the available foundation models.
```python
# Import the boto 3 library
import boto3

bedrock = boto3.client(
    'bedrock', region_name='us-east-1'
)

# List available models
models = bedrock.list_foundation_models()

print(models['modelSummaries'])
```
*Great job - you've now established your first connection with Bedrock. Looks like there's plenty of models to choose from!*
### Retrieving Bedrock model details
Their tech lead asks:

"To help me choose a model, is there a way to get more detailed information about each of them?"

Demonstrate how to retrieve model details to help the team make an informed decision. The boto3 and json libraries have been pre-imported.
* Use the method to get details for the model ID provided.
* Print the model's name.
```python
bedrock = boto3.client('bedrock', region_name='us-east-1')

model_id = 'anthropic.claude-3-5-sonnet-20240620-v1:0'

# Retrieve model details
response = bedrock.get_foundation_model(modelIdentifier=model_id)

model_info = response['modelDetails']

# Print model's name
print(model_info['modelName'])
```
<pre>
    # Print model's name
print(model_info['modelName'])
Claude 3.5 Sonnet
</pre>
    
*Excellent work setting up the Bedrock client! This initialization is the first step for all Bedrock interactions!*

---
### Using the correct model in Bedrock
* Initialize the Bedrock client for inference.
* Invoke the model using the bedrock client.
```python
# Initialize the client for inference
bedrock = boto3.client('bedrock-runtime', region_name='us-east-1')

messages = [{"role": "user", "content": "What are some ways to forecast customer demand?"}]

request_body=json.dumps({"anthropic_version": "bedrock-2023-05-31", "max_tokens": 20,
                 "messages": messages})  

# Invoke the model
response = bedrock.invoke_model(body=request_body, modelId='anthropic.claude-3-5-sonnet-20240620-v1:0')
response_body = json.loads(response.get('body').read())

print(response_body)
```
<pre>{'id': 'msg_bdrk_01MLCx5Ea3wwUNPSUuT7khFU', 'type': 'message', 'role': 'assistant', 'model': 'claude-3-5-sonnet-20240620', 'content': [{'type': 'text', 'text': 'Forecasting customer demand is crucial for businesses to plan their operations, inventory, and resources effectively.'}], 'stop_reason': 'max_tokens', 'stop_sequence': None, 'usage': {'input_tokens': 16, 'output_tokens': 20}}
</pre>

*Excellent! You've successfully sent your first prompt to Claude through Bedrock. Notice how the request body is structured as a dictionary that includes the prompt along with other parameters - this pattern will be essential as you build more complex interactions!*

