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
