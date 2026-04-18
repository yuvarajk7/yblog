---
date:
    created: 2026-04-18
categories:
    - AI
tags:
    - AIAgents
---
# LLM Asynchronous Calls: Speeding Up Your AI Agents

When developing an agent, you often need to process multiple LLM requests simultaneously. This includes evaluating benchmark results, comparing responses from multiple models, and running multiple agents concurrently in a multi-agent system.

If you send requests sequentially, the total execution time is equal to the sum of each request’s duration. With asynchronous execution, multiple requests run in parallel, allowing each request to complete as soon as its response is ready.

Python supports asynchronous programming using the `async/await` syntax and the `asyncio` package. LiteLLM supports asynchronous operations through the `acompletion` API. The `await` keyword pauses execution of a task until the result is available, while still allowing other tasks to run in the meantime. The `asyncio.gather` function executes multiple tasks concurrently and returns all results together.

```python
import asyncio
from litellm import acompletion 

async def get_response(prompt: str) -> str:

    # LiteLLM supports async operations
    response = await acompletion(
        model="gpt-5-mini",
        messages=[
            {"role": "user", "content": prompt}
        ],
        num_retries=3,  # Retry up to 3 times on failure
        retry_delay=2   # Wait 2 seconds between retries
    )
    return response.choices[0].message.content

prompts = [
    "What is the capital of India?",
    "What is the largest mammal?",
    "Who wrote 'Harry Potter'?"
]

tasks = [get_response(prompt) for prompt in prompts]

# Execute tasks concurrently
results = await asyncio.gather(*tasks)

for prompt, result in zip(prompts, results):
    print(f"Q: {prompt}\nA: {result}\n")
```

There are two common issues when sending multiple requests together: rate limiting from the LLM API provider and network instability or server overload. LiteLLM addresses these challenges with the `num_retries` parameter, which handles transient failures using automatic retries (with exponential backoff).

To control rate limiting, you can use the `asyncio.Semaphore` API to limit how many requests run concurrently.

In summary, asynchronous execution is a key feature for improving the performance and efficiency of AI agents that interact with LLMs.
