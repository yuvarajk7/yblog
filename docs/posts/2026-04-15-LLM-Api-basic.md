---
date:
    created: 2026-04-15
categories:
    - AI
tags:
    - AIAgents
---
# LLM API for building AI Agents

I set up a development environment to learn how to use provider APIs to call their LLM models. I use the OpenAI API to call the GPT-5 model and the Anthropic API to call the Claude Sonnet model.

I use the Python uv package manager to install the OpenAI and Anthropic API libraries. I also use the python-dotenv package to load provider token information from environment variables.

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
OpenAI’s Chat Completions API has become an industry standard, and most LLM providers offer similar interfaces. Let’s initialize the OpenAI client and send a simple request.

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
Anthropic works in a similar way, but there are some differences in how the API is called. OpenAI uses `client.chat.completions.create`, whereas Anthropic uses `client.messages.create`. Likewise, the response formats differ: OpenAI returns content in `choices[0].message.content`, while Anthropic returns it in `content[0].text`.


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
Switching between different LLM providers when building AI agents can be tedious. Each provider offers different interfaces, which can impact both cost and performance requirements. LiteLLM solves this problem by providing a unified approach. It is an open-source library that offers a single interface to call over 100 LLMs.

LiteLLM’s main features include:

* Calling any provider using the same `completion()` interface
* A consistent output format, regardless of the provider or model used
* Built-in retry and fallback logic across multiple deployments via the Router
* Compatible exception handling across all providers
* Support for observability


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