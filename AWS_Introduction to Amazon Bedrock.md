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
