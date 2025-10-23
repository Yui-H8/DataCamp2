# Multi-Modal Systems with the OpenAI API
---
### Multi-Modal Systems with the OpenAI API
The OpenAI API Audio endpoint provide8s access to the different models, which can be used for speech-to-text transcription and translation. In this exercise, you'll create a transcript from a DataFramed podcast (https://www.datacamp.com/podcast) episode with OpenAI Developer, Logan Kilpatrick.

If you'd like to hear more from Logan, check out the full ChatGPT and the OpenAI Developer Ecosystem podcast episode.
* Open the openai-audio.mp3 file.
* Create a transcription request to the Audio endpoint with audio_file.
* Extract and print the transcript text from the response.
```Python
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

# Open the openai-audio.mp3 file
audio_file = open("openai-audio.mp3", "rb")

# Create a transcript from the audio file
response = client.audio.transcriptions.create(model="whisper-1", file=audio_file)

# Extract and print the transcript text
print(response.text)
```
