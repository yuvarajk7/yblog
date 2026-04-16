---
date:
    created: 2026-04-15
categories:
    - AI
tags:
    - AIAgents
---
# LLM API for building AI Agents

I will setup development environment to learn the provider API to call their LLM model. Using OpenAI API to call gpt-5 model and use Anthropic API to call the claude-sonnet model.

I am using python UV package manager to install LLM provider OpenAI and Anthropic API's. Use the python-dotenv package to read the provider token information from environment variables.

```python
uv add openai
uv add anthropic
uv add python-dotenv
```

Configure and add the following provider's LLM access token:

```python
#.env
OPENAI_API_KEY=<token>
ANTHROPIC_API_KEY=<token>
```

## OpenAI
OpenAI's Chat Completions API become the industry standard. Most LLM providers offer similar interfaces. Let's initialize the OpenAI client and send a simple request.

```python
from openai import OpenAI
 
client = OpenAI() 
 
response = client.chat.completions.create(
    model="gpt-5-mini",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "What is the capital of France?"}
    ]
)
 
print(response.choices[0].message.content)
```

## Anthropic
Anthropic works similar way. You see the difference calling the API. OpenAI invokes client.chat.completions.create vs client.messages.create and likewise getting the response choices[0].message.content vs content[0].text.

```python
import anthropic

client = anthropic.Anthropic()

message = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=1024,
    system="You are a helpful assistant.",
    messages=[
        {"role": "user", "content": "What is an AI agent? Answer in one sentence."}
    ]
)

print(message.content[0].text)
```

## LiteLLM
It is very tedious switching different LLM provider when building AI agents. Each provider provides different interfaces and it will impacts the cost and performance requirements. LiteLLM solves this problem. LiteLLM  is an open source library that gives you a single, unified interface to call 100 LLMs.

LiteLLM main features are,

- Call any provider using the same completion() interface
- Consistent output format regardless of which provider or model you use
- Built-in retry / fallback logic across multiple deployments via the Router
- Compatible exception handling across all the providers
- Support observability

### Install LiteLLM

```python
uv add litellm
```

### Accessing the LiteLLM API

```python
from litellm import completion

response = completion(
    model="gpt-5-mini",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "My name is Yuvaraj."}
    ]
)
print(response.choices[0].message.content)

response2 = completion(
    model="gpt-5-mini",
    messages=[{"role": "user", "content": "What is my name?"}]
)
print(response2.choices[0].message.content)
```