---
date:
    created: 2026-04-21
categories:
    - AI
tags:
    - AIAgents
---

# AI Agent - Tool Calling or Function Calling

LLM interact with external tools through the structured tool calling or function calling. It is a key components, how agents make decisions using tools and memory, and how structured tool invocation enables dynamic, real-time interactions with the external world to solve complex tasks.

Tools are essentially function made available to LLM's. LLMs do not execute tools or functions directly. Instead, they generate a structured representation indicating which tool to use and with what parameters. 

When you pose a question to the LLM that requires external information or computation, the model evaluates the available tools based on their names and descriptions. If it identifies a relevant tool, the model generates a structured output (typically formatted as a JSON object) that specifies the tool's name and appropriate parameter values. This is still text generation, just in a structured format intended for tool input.

An external system then interprets this structured output, executes the actual function or API call, and retrieves the result. This result is subsequently fed back to the LLM, which uses it to generate a comprehensive response.

## Workflow

    1. Define a weather tool and ask question like "What's the weather like in TX?"
    2. The model halts regular text generation and outputs a structured tool call with parameter values (e.g., "location": "TX").
    3. Extract the tool input, execute the actual weather function, and obtain the weather details.
    4. Pass the output back to the model so it can generate a complete final answer using the real-time data.

Function calling and Tool calling both are same capability by enabling an LLM to request specific external functions to be executed with structured parameters. Function calling term coined by OpenAI in their documentation.

## Key principles keep in mind when developing tool calling:

    1. Clear purpose - make sure tool has well defined task
    2. Standardized input - The tool shoud accept input in a predictable, structured format.
    3. Consistent output - The format which is easy to process and integrate with other system
    4. Comprehensive Documentation - explain what the tool does, how to use it and any limitations
    5. Limit the number of functions - keep the number of tools under 20. Using too many tools can lead to selection errors. 

## To enable tool calling,

### Specify Tool Definitions
The function has 3 essencial components

    - name
    - description
    - parameters

```json
calc_tool_def = {
    "type": "function",
    "function": {
        "name": "calc",
        "description": "Perform a calculation based on the provided expression.",
        "parameters": {
            "type": "object",
            "properties": {
                "operator": {
                    "type": "string",
                    "description": "Arithmetic operation to perform",
                    "enum": ["add", "subtract", "multiply", "divide"],      
                },
                "operand1": {
                    "type": "number",
                    "description": "The first number in the calculation",
                },
                "operand2": {
                    "type": "number",
                    "description": "The second number in the calculation",
                },
            },
            "required": ["operator", "operand1", "operand2"],
        }
    }
}
```

### Setup the tool

```python
def calculator(operator: str, operand1: float, operand2: float):
   if operator == 'add':
       return operand1 + operand2
   elif operator == 'subtract':
       return operand1 - operand2       
   elif operator == 'multiply':
       return operand1 * operand2
   elif operator == 'divide':
       if operand2 == 0:
           raise ValueError("Cannot divide by zero")
       return operand1 / operand2
   else:
       raise ValueError(f"Unsupported operator: {operator}")
```

### Executing the tool calling

```python
from litellm import completion

tools = [calc_tool_def]

print("Without Tools:")
without_tools = "How many days in a week?"
response_without_tool = completion(
        model='claude-sonnet-4-20250514',
        messages=[{"role": "user", "content": without_tools}],
        tools=tools
)
print(response_without_tool.choices[0].message.content) 
# There are 7 days in a week....
print(response_without_tool.choices[0].message.tool_calls)
# None

print("With Tools:")
with_tools = "What is the result of calculating 10 divided by 2 using the calculator tool?"
response_with_tool = completion(
        model='claude-sonnet-4-20250514',
        messages=[{"role": "user", "content": with_tools}],
        tools=tools
)
print(response_with_tool.choices[0].message.content)
# I'll use the calculator tool to divide 10 by 2 for you.
print(response_with_tool.choices[0].message.tool_calls)
# [ChatCompletionMessageToolCall(index=1, caller={'type': 'direct'}, function=Function(arguments='{"operator": "divide", "operand1": 10, "operand2": 2}', name='calc'), id='toolu_01XXWamC3kDafkNYbBv2EXic', type='function')]

```

### Feedback the result to the LLM

```python
ai_message = response_with_tool.choices[0].message

messages = []

messages.append({  
   "role": "assistant",  
   "content": ai_message.content,  
   "tool_calls": ai_message.tool_calls  
})
 
if ai_message.tool_calls:
   for tool_call in ai_message.tool_calls:
       function_name = tool_call.function.name  
       function_args = json.loads(tool_call.function.arguments)  
       
       if function_name == "calc":
           result = calculator(**function_args)  
           
           messages.append({  
               "role": "tool",  
               "tool_call_id": tool_call.id,  
               "content": str(result)  
           })

final_response = completion(
   model='claude-sonnet-4-20250514',
   messages=messages,
   tools=tools
)
print("Messages: ", messages)
# Messages:  [{'role': 'assistant', 'content': "I'll use the calculator tool to divide 10 by 2 for you.", 'tool_calls': [ChatCompletionMessageToolCall(index=1, caller={'type': 'direct'}, function=Function(arguments='{"operator": "divide", "operand1": 10, "operand2": 2}', name='calc'), id='toolu_01XXWamC3kDafkNYbBv2EXic', type='function')]}, {'role': 'tool', 'tool_call_id': 'toolu_01XXWamC3kDafkNYbBv2EXic', 'content': '5.0'}]

print("Final Answer:", final_response.choices[0].message.content)
#Final Answer: 10 divided by 2 equals 5.
```

## Summary

Tool calling enables LLMs to go beyond static text generation by:

    - Accessing real-time data
    - Performing computations
    - Integrating with external systems

It is a foundational capability for building intelligent, autonomous AI agents.
