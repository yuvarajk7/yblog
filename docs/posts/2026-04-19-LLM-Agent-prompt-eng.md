---
date:
    created: 2026-04-19
categories:
    - AI
tags:
    - AIAgents
---

# AI Agent - Prompt Engineering 

Prompt engineering is the practice of instructing a large language model (LLM) to behave effectively as an agent. A prompt provides the guidance the model needs to produce accurate, relevant, and consistent outputs.

Prompts generally fall into two categories:

* **User prompts**: Inputs provided by the user through a chat interface. These vary with each interaction.
* **System prompts**: Developer-defined instructions that persist throughout the conversation. They establish the agent’s personality, constraints, permissions, and tool usage policies.

A well-crafted system prompt transforms a general-purpose LLM into a reliable, task-specific agent. While user prompts are unpredictable and driven by user behavior, system prompts provide consistency and control over how the agent responds.


## Structure of System Prompts

A system prompt serves four primary roles:

1. **Define product identity**
2. **Specify output format and style**
3. **Set boundaries and prohibited behaviors**
4. **Clarify the limits of knowledge**

The most important role is defining *who* the agent is. This helps the model communicate its purpose and capabilities clearly, while avoiding outdated or inaccurate self-descriptions. An effective agent must understand its role to explain what it can and cannot do.

System prompts can also enforce response format and style. This is especially useful when outputs are consumed programmatically. However, overly rigid formatting may feel unnatural in conversational contexts.

Additionally, system prompts should include clear refusal guidelines. These define which requests the agent must decline. Boundary setting is critical because autonomous agents may otherwise behave unpredictably without explicit constraints.

Finally, agents must recognize their limitations. When a request exceeds those limits, they should either use appropriate tools or respond transparently about their constraints.

## Best Practices for Agent Prompts

* **Enable autonomous behavior** while maintaining control through clear instructions
* **Use tools strategically** when tasks require external capabilities
* **Treat tool definitions as part of the prompt**, ensuring the model understands how and when to use them
