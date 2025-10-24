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
> Hi there, Logan, thank you for joining us on the show today. Thanks for having me. I'm super excited about this. Brilliant. We're going to dive right in, and I think ChatGPT is maybe the most famous AI product that you have at OpenAI, but I'd just like to get an overview of what all the other AIs that are available are. So I think two and a half years ago, OpenAI released the API that we still have available today, which is essentially our giving people access to these models. And for a lot of people, giving people access to the model that powers ChatGPT, which is our consumer-facing first-party application, which essentially just, in very simple terms, puts a nice UI on top of what was already available through our API for the last two and a half years. So it's sort of democratizing the access to this technology through our API. If you want to just play around with it, as an end user, we have ChatGPT available to the world as well.

*Audibly awesome! Using an OpenAI model to transcribe a recording of an OpenAI developer talking about OpenAI is mind-bending stuff. Have a listen and see how the model performed!   
Link to Download(https://assets.datacamp.com/production/repositories/6309/datasets/9e5a65b42c1a9fe8756e30e4d255883a8466b671/audio-logan-advocate-openai.mp3)*

### Transcribing a non-English language
OpenAI's audio models can not only transcribe English speech but also perform well in speech in many other languages.
* Open the audio.m4a file in read-binary mode.
* Create a transcription request to the Audio endpoint.
```python
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

# Open the audio.m4a file
audio_file = open("audio.m4a", "rb")

# Create a transcript from the audio file
response = client.audio.transcriptions.create(model="whisper-1", file=audio_file)

print(response.text)
```
> Olá, o meu nome é Eduardo, sou CTO no Datacamp. Espero que esteja a gostar deste curso que o James e eu criamos para você. Esta API permite enviar um áudio e trazer para inglês. O áudio original está em português.

*Awesome work! See how the audio was transcribed in Portuguese! Being able to transcribe speech into the native language is very cool. Let's finish by translating audio from one language into English.(https://assets.datacamp.com/production/repositories/6309/datasets/443390237150862833705a6e0599478575a1276a/audio-portuguese.m4a)*

### Translating Portuguese
OpenAI's audio models can not only transcribe audio into its native language, but also support translation capabilities for creating English transcriptions.

In this exercise, you'll return to the Portuguese audio, but this time, you'll translate it into English!
* Open the audio.m4a file.
* Create a translation request to the Audio endpoint.
* Extract and print the translated text from the response.
```python
client = OpenAI(api_key="<OPENAI_API_TOKEN>")

# Open the audio.m4a file
audio_file = open("audio.m4a", "rb")

# Create a translation from the audio file
response = client.audio.translations.create(model="whisper-1", file=audio_file)

# Extract and print the translated text
print(response.text)
```
> Hello, my name is Eduardo, I am a CTO at Datacamp. I hope you are enjoying this course that James and I have created for you. This API allows you to send an audio and bring it to English. The original audio is in Portuguese.

*Look at that—that's a pretty good translation! The model may not always be perfect, outputting minor wording differences or imperfect grammar, but it performs well for the most widely-used languages.*
