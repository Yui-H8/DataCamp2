# Introduction to Claude Models
---
**Description**

Unlock the power of Claude to build safe, thoughtful, and high-impact AI experiences. In this course, you'll learn how to work with Claude using the Anthropic API to solve real-world tasks across content generation, summarization, customer support, and more. You'll experiment with Claude’s models to write effective prompts, simulate multi-turn conversations, and build role-based assistants. By the end, you'll be equipped to create applications that combine reliability, reasoning, and structure—all powered by one of today’s most advanced large language models.

---
### Try your first Claude prompt!
To preview what's ahead, the Python code for sending a request to the Anthropic API is ready for you.

Pass any question or instruction to the prompt argument and see how Claude responds!

Here are a couple of prompts to try if you're struggling for ideas:
* What capabilities make Claude well-suited for enterprise applications?
* Give me two examples of how I could use Claude in a Python app.
```python
from anthropic import Anthropic

client = Anthropic(api_key="datacamp-token", base_url=url)

response = client.messages.create(
    model="claude-sonnet-4-0",
    max_tokens=100,
    # Enter your prompt
    messages=[{"role": "user", "content": "What capabilities make Claude well-suited for enterprise applications?"}]
)

print(response.content[0].text)
```
*Great job! You've successfully created your first prompt, and invoked Claude to respond! This structure is key as you build more advanced prompts!*

### Claude’s key advantages
You’ve just been hired as an AI consultant by Lexiform, a startup building AI tools for law firms. Their flagship product will review lengthy corporate contracts and flag clauses related to liability. These documents often exceed 100 pages and are filled with dense legal language. The engineering lead turns to you and asks:

Which Claude model should we use for this application?
```
Haiku, because it’s the most affordable model for large-scale analysis.
〇 Opus, because it’s built for high-stakes tasks involving complex documents.
Sonnet, since it balances speed with solid language skills.
```
*Correct! Opus is ideal for analyzing complex legal documents at scale.*

### End-to-end Claude prompting
As part of a company hackathon, your team is prototyping QuickAid, a help-desk bot for event attendees. Step one is to prove you can hit the Claude API from Python, get a live response, and print it to the console. Nail this, and the judges will green-light the rest of your prototype.
* Initialize the Anthropic client.
* Set the parameter to specify the Claude Sonnet model.
> Hint
> Initialize the client using anthropic.Anthropic().
> For the model parameter, use model with "claude-sonnet-4-0" to specify the Claude 4 Sonnet model.
```python
from anthropic import Anthropic
# Initialize the Anthropic client
client = Anthropic(api_key="datacamp-token", base_url=url)

response = client.messages.create(
    # Set the Claude Sonnet Model
    model="claude-sonnet-4-0",
    max_tokens=150,
    messages=[
        {"role": "user", "content": "What can you help me with today?"}
    ])

print(response.content[0].text)
```
```
I can help you with a wide variety of tasks! Here are some examples:

**Writing & Communication**
- Drafting emails, letters, or documents
- Editing and proofreading text
- Creative writing assistance
- Summarizing content

**Learning & Research**
- Explaining concepts across many subjects
- Helping with homework or study questions
- Research assistance and fact-checking
- Language learning support

**Problem-Solving & Analysis**
- Breaking down complex problems
- Data analysis and interpretation
- Coding help and debugging
- Math and logic problems

**Creative Projects**
- Brainstorming ideas
- Writing stories, poems, or scripts
- Planning projects
```
*Great job! You've successfully sent a prompt to Claude and extracted the response, this is the foundation for building AI-powered applications!!*

### Extracting Claude's response
At the hackathon, your teammate needs the exact reply text to wire into the React front-end. Show them how to slice the response object and print just Claude’s answer, so that it can be displayed on your app.
* Extract and print Claude's response from the given response object.
```python
from anthropic import Anthropic

client = Anthropic(api_key="datacamp-token", base_url=url)

response = client.messages.create(
    model="claude-sonnet-4-0",
    max_tokens=150,
    messages=[
        {"role": "user", "content": "How does Claude work? Reply in one sentence."}
    ])
# Extract and print Claude's response
print(response.content[0].text)
```
```
Claude is an AI assistant created by Anthropic using constitutional AI techniques to train a large language model that processes text inputs and generates helpful, harmless, and honest responses.

<script.py> output:
    Claude is an AI assistant created by Anthropic using constitutional AI and reinforcement learning from human feedback to train a large language model that can engage in helpful, harmless, and honest conversations.
```
*Great job! You've successfully sent a prompt to Claude and extracted the response, this is the foundation for building AI-powered applications!!*

### Controlling response length with tokens
