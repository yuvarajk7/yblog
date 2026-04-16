---
date:
    created: 2026-04-16
categories:
    - AI
tags:
    - AIAgents
---
# LLM Stateless API Conversation Mangement

LLM APIs are stateless and each API call is independent and has no memory of previous call. The developer must manage the conversation history manually to maintain the continuity of the conversation. 

We accumulate the conversation history in the messages list and pass the entire history with each API call. User messages are added into user role and model responses with the assistant role. This allows the model to understand the previous context and maintain the conversation.

This approach is very important for agent development. Agents needs to add all the conversation history and tool  call results, search results and any reasoning steps to the context. As conversation history grows longer, token usages increases, raise the costs and slowing the response time. We must have efficient approach to compress volume of context history. 

## Example to shows the stateless API call
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

#output - response
#Nice to meet you, Yuvaraj. How would you like me to address you (Yuvaraj, Mr. [Surname], or something else)? What can I help you with today?

#output - response2
#I don't know your name — I don't have access to personal data unless you tell me. 
```

## Example of API maintain conversation context.
```python
from litellm import completion
 
messages = []
 
# First call
messages.append({"role": "user", "content": "My name is Yuvaraj"})
response1 = completion(model="gpt-5-mini", messages=messages)
assistant_message1 = response1.choices[0].message.content
messages.append({"role": "assistant", "content": assistant_message1})
print(assistant_message1)
 
# Second API - includes previous conversation history
messages.append({"role": "user", "content": "What is my name?"})
response2 = completion(model="gpt-5-mini", messages=messages)
assistant_message2 = response2.choices[0].message.content
print(assistant_message2)

#output
#Nice to meet you, Yuvaraj. How can I help you today?
#Your name is Yuvaraj.

```