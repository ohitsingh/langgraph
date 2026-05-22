# LangGraph AI Projects

A collection of hands-on AI engineering projects built using:

* LangGraph
* LangChain
* MCP (Model Context Protocol)
* Groq LLMs
* Tool Calling
* AI Memory
* Human-in-the-Loop workflows

This repository contains practical implementations of modern Agentic AI concepts including stateful chatbots, memory-enabled agents, MCP integrations, tool orchestration, and multi-step AI workflows.

---

# Tech Stack

* Python
* LangGraph
* LangChain
* Groq
* FastMCP
* AsyncIO
* Tavily Search
* Jupyter Notebook

---

# Repository Structure

```text
langgraph/
│
├── 1-BasicChatBot/
│
├── main.py
├── requirements.txt
├── pyproject.toml
├── README.md
└── .gitignore
```

---

# Architecture

## System Overview

This project demonstrates modern AI workflow orchestration using:

* LangGraph state management
* LangChain tool calling
* Memory-enabled conversations
* Human-in-the-loop workflows
* AI tool orchestration

The repository focuses on building production-style AI agents with persistent memory, conditional routing, and external tool execution.

---

# High Level Architecture

```text
┌──────────────────────────────────────────────────────┐
│                  User Message                        │
└──────────────────────────────────────────────────────┘
                           │
                           ▼

┌──────────────────────────────────────────────────────┐
│                 LangGraph Workflow                   │
│                                                      │
│  ┌──────────────────────────────────────────────┐    │
│  │               StateGraph                     │    │
│  │                                              │    │
│  │  START                                       │    │
│  │    │                                         │    │
│  │    ▼                                         │    │
│  │  chatbot node                                │    │
│  │    │                                         │    │
│  │    ▼                                         │    │
│  │  tools_condition                             │    │
│  │    │                                         │    │
│  │ ┌──┴───────────────┐                         │    │
│  │ ▼                  ▼                         │    │
│  │ToolNode         Final Response               │    │
│  │ │                                            │    │
│  │ ▼                                            │    │
│  │ chatbot node                                 │    │
│  └──────────────────────────────────────────────┘    │
└──────────────────────────────────────────────────────┘
                           │
                           ▼

┌──────────────────────────────────────────────────────┐
│                  External Tools                      │
│                                                      │
│  - Tavily Search                                     │
│  - Human Assistance Tool                             │
│  - Custom AI Tools                                   │
└──────────────────────────────────────────────────────┘
```

---

# Workflow Explanation

## Step 1 — User Input

The user sends a message:

```text
"What is the weather in California?"
```

or

```text
"I need expert guidance for building an AI agent."
```

---

# Step 2 — LangGraph State Management

The workflow is built using:

```python
StateGraph(State)
```

The graph maintains conversation state across interactions.

Example state:

```python
class State(TypedDict):
    messages: Annotated[list, add_messages]
```

This allows:

* conversation history
* persistent context
* memory-enabled agents

---

# Step 3 — Chatbot Node Execution

The chatbot node receives current state:

```python
def chatbot(state):
```

The LLM processes:

* user message
* previous messages
* tool outputs

---

# Step 4 — Tool Decision Routing

LangGraph evaluates:

```python
tools_condition
```

The LLM decides whether:

* tool execution is required
* direct response is sufficient

---

# Step 5 — ToolNode Execution

If tools are required:

```python
ToolNode(tools=tools)
```

handles:

* tool selection
* parameter passing
* tool execution
* response collection

---

# Step 6 — Human-in-the-Loop Interrupt

Example:

```python
interrupt({"query": query})
```

The workflow pauses execution and waits for human response.

This enables:

* approvals
* manual review
* expert intervention
* collaborative AI systems

---

# Step 7 — Response Generation

The tool result returns back to the chatbot node.

The LLM generates the final natural language response.

---

# LangGraph Flow

```text
START
  ↓
chatbot
  ↓
tools_condition
  ↓
┌───────────────┬───────────────┐
│               │               │
▼               ▼               │
ToolNode     Final Output       │
│                               │
└───────────────→ chatbot ──────┘
```

---

# Core Components

| Component | Responsibility |
|---|---|
| `StateGraph` | Workflow orchestration |
| `ToolNode` | Tool execution |
| `tools_condition` | Conditional routing |
| `interrupt()` | Human-in-loop support |
| `MemorySaver` | Persistent memory |
| `ChatGroq` | LLM reasoning |
| `TavilySearch` | External web search |
| `add_messages` | Conversation history management |

---

# LangGraph State & Message System

This section explains the most important LangGraph concepts related to:

* `State`
* `TypedDict`
* `Annotated`
* `BaseMessage`
* `add_messages`
* `graph.invoke()`
* Message flow inside LangGraph

This acts as a quick revision guide whenever revisiting LangGraph projects.

---

# 1. What is State in LangGraph?

LangGraph applications work using a shared state object.

The state stores data flowing between nodes.

Example:

```python
class State(TypedDict):
    messages: Annotated[list[BaseMessage], add_messages]
```

Here:

* `State` defines the structure of data
* `messages` stores chat history
* Every node can read/update this state

---

# 2. Why Use TypedDict?

`TypedDict` defines dictionary structure with type safety.

Example:

```python
from typing_extensions import TypedDict

class State(TypedDict):
    messages: list
```

Without `TypedDict`, Python treats state as a normal dictionary.

With `TypedDict`:

* IDE gets autocomplete
* Type checking works
* LangGraph understands state structure

---

# 3. Understanding BaseMessage

LangChain stores conversations as message objects.

All message types inherit from:

```python
BaseMessage
```

Types of messages:

| Message Type | Purpose |
|---|---|
| `HumanMessage` | User input |
| `AIMessage` | AI response |
| `SystemMessage` | System instructions |
| `ToolMessage` | Tool outputs |

---

# 4. Example of Messages

```python
from langchain_core.messages import HumanMessage, AIMessage

messages = [
    HumanMessage(content="Hello"),
    AIMessage(content="Hi! How can I help?")
]
```

---

# 5. What is Annotated?

Python `Annotated` adds metadata to types.

Syntax:

```python
Annotated[type, metadata]
```

Example:

```python
Annotated[list[BaseMessage], add_messages]
```

Meaning:

* Store list of messages
* Use `add_messages` behavior while updating state

---

# 6. What is add_messages?

`add_messages` is a LangGraph reducer.

It tells LangGraph:

```text
"Append new messages instead of replacing old messages"
```

Without `add_messages`:

```python
messages = new_messages
```

Old conversation gets deleted.

With `add_messages`:

```python
messages += new_messages
```

Conversation history is preserved.

---

# 7. Full State Definition

```python
from typing_extensions import TypedDict
from typing import Annotated

from langchain_core.messages import BaseMessage
from langgraph.graph.message import add_messages


class State(TypedDict):
    messages: Annotated[list[BaseMessage], add_messages]
```

---

# 8. Creating Input State

```python
from langchain_core.messages import HumanMessage

state_input: State = {
    "messages": [
        HumanMessage(content="add 2 and 3")
    ]
}
```

---

# 9. Why Use `state_input: State`?

Without this:

```python
state_input = {
    "messages": [...]
}
```

Python infers:

```python
dict[str, list[HumanMessage]]
```

But LangGraph expects:

```python
State
```

So IDE shows type errors.

Correct approach:

```python
state_input: State = {...}
```

---

# 10. What Does `graph.invoke()` Do?

```python
response = graph.invoke(state_input)
```

This:

1. Sends state into graph
2. Executes nodes
3. Updates state
4. Returns final state

Returned object:

```python
response
```

contains updated messages and state values.

---

# 11. Accessing Final AI Response

```python
response["messages"][-1].content
```

Explanation:

| Part | Meaning |
|---|---|
| `response["messages"]` | Full conversation history |
| `[-1]` | Last message |
| `.content` | Actual text |

---

# 12. Full Working Example

```python
from typing_extensions import TypedDict
from typing import Annotated

from langgraph.graph.message import add_messages
from langchain_core.messages import BaseMessage, HumanMessage


class State(TypedDict):
    messages: Annotated[list[BaseMessage], add_messages]


state_input: State = {
    "messages": [
        HumanMessage(content="add 2 and 3")
    ]
}

response = graph.invoke(state_input)

print(response["messages"][-1].content)
```

---

# 13. Message Flow Inside LangGraph

```text
User Message
      │
      ▼

HumanMessage
      │
      ▼

State["messages"]
      │
      ▼

graph.invoke()
      │
      ▼

Chatbot Node
      │
      ▼

AIMessage Added
      │
      ▼

Updated State Returned
```

---

# 14. Real Internal State Example

Before execution:

```python
{
    "messages": [
        HumanMessage(content="add 2 and 3")
    ]
}
```

After execution:

```python
{
    "messages": [
        HumanMessage(content="add 2 and 3"),
        AIMessage(content="The answer is 5")
    ]
}
```

---

# 15. Why This Design is Powerful

This architecture enables:

* Stateful chatbots
* Persistent memory
* Multi-step reasoning
* Tool calling
* Human-in-loop systems
* Agent workflows
* Conversation history tracking

This is the core foundation of LangGraph AI agents.

---

# 16. Important Interview Points

## Why use `Annotated`?

To attach reducer/update behavior to state fields.

---

## Why use `add_messages`?

To preserve conversation history automatically.

---

## Why use `BaseMessage`?

Because multiple message types exist.

---

## Why use `TypedDict`?

To define structured graph state with type safety.

---

## What does `graph.invoke()` return?

Updated graph state after execution.

---

# 17. Easy Memory Trick

```text
State = Shared Memory

messages = Conversation History

BaseMessage = Parent of all messages

Annotated = Extra behavior

add_messages = Append messages

graph.invoke() = Run workflow
```

---

# 18. Core Concept Summary

| Concept | Meaning |
|---|---|
| `State` | Shared graph memory |
| `TypedDict` | Typed dictionary |
| `BaseMessage` | Parent message class |
| `HumanMessage` | User message |
| `AIMessage` | AI response |
| `Annotated` | Add metadata |
| `add_messages` | Append history |
| `graph.invoke()` | Execute workflow |

---

# Features

* Stateful AI Chatbots
* Persistent Memory
* Tool Calling
* Conditional Workflow Routing
* Human-in-the-Loop Interrupts
* Async AI Workflows
* LangGraph Orchestration
* External Search Integration
* Modular AI Architecture

---

# Getting Started

## 1. Clone Repository

```bash
git clone https://github.com/ohitsingh/langgraph.git

cd langgraph
```

---

## 2. Create Virtual Environment

### Windows

```bash
python -m venv .venv

.venv\Scripts\activate
```

### Linux/macOS

```bash
python3 -m venv .venv

source .venv/bin/activate
```

---

## 3. Install Dependencies

```bash
pip install -r requirements.txt
```

---

## 4. Configure Environment Variables

Create `.env`

```env
GROQ_API_KEY=your_groq_api_key
TAVILY_API_KEY=your_tavily_api_key
```

---

# Run Project

```bash
python main.py
```

---

# Learning Concepts

This repository focuses on:

* Agentic AI
* LangGraph Workflows
* AI Tool Orchestration
* Human-in-the-Loop Systems
* Stateful AI Agents
* AI Memory Systems
* Async AI Execution
* Production-style AI Architecture

---

# Future Improvements

* MCP Integration
* Multi-Agent Systems
* RAG Pipelines
* Vector Databases
* LangSmith Tracing
* Streaming Responses
* Supervisor Agents
* AI Planning Systems
* Deployment Pipelines

---

# Why This Repository Matters

This project demonstrates practical AI engineering concepts used in modern GenAI systems:

* graph-based orchestration
* memory-aware AI systems
* conditional execution flows
* external tool integration
* human-assisted AI workflows

It is designed as a hands-on learning repository for building production-style AI agents using LangGraph and LangChain.

---

# References

* LangGraph Documentation
* LangChain Documentation
* Groq API
* Tavily Search API

---


# LangGraph State Example  05/22/2026

## State Definition

```python
class State(TypedDict):
    messages: Annotated[list[BaseMessage], add_messages]
```

This defines the shared state used inside LangGraph.

### Explanation

| Component | Meaning |
|---|---|
| `State` | Shared graph memory |
| `TypedDict` | Defines structured dictionary |
| `messages` | Stores conversation history |
| `BaseMessage` | Parent class for all messages |
| `Annotated` | Adds metadata/behavior |
| `add_messages` | Appends messages instead of replacing |

---

# Creating Input State

```python
from langchain_core.messages import HumanMessage

state_input: State = {
    "messages": [
        HumanMessage(content="add 2 and 3")
    ]
}
```

## Explanation

### `HumanMessage`

Represents user input.

Example:

```python
HumanMessage(content="add 2 and 3")
```

stores:

```text
User → "add 2 and 3"
```

---

# Invoking LangGraph

```python
response = graph.invoke(state_input)
```

## What Happens Internally?

```text
1. Input state enters graph
2. Chatbot node processes message
3. LLM generates response
4. AIMessage gets appended
5. Updated state is returned
```

---

# Accessing Final Response

```python
response["messages"][-1].content
```

## Explanation

| Part | Meaning |
|---|---|
| `response["messages"]` | Full message history |
| `[-1]` | Last message |
| `.content` | Actual AI response text |

---

# Full Working Example

```python
from typing_extensions import TypedDict
from typing import Annotated

from langchain_core.messages import BaseMessage, HumanMessage
from langgraph.graph.message import add_messages


class State(TypedDict):
    messages: Annotated[list[BaseMessage], add_messages]


state_input: State = {
    "messages": [
        HumanMessage(content="add 2 and 3")
    ]
}

response = graph.invoke(state_input)

print(response["messages"][-1].content)
```

---

# Message Flow

```text
User Input
    │
    ▼

HumanMessage
    │
    ▼

State["messages"]
    │
    ▼

graph.invoke()
    │
    ▼

LLM Processing
    │
    ▼

AIMessage Added
    │
    ▼

Updated State Returned
```

---

# Example Internal State

## Before Execution

```python
{
    "messages": [
        HumanMessage(content="add 2 and 3")
    ]
}
```

---

## After Execution

```python
{
    "messages": [
        HumanMessage(content="add 2 and 3"),
        AIMessage(content="The answer is 5")
    ]
}
```

---

# Important Concepts

## Why Use `add_messages`?

Without it:

```python
messages = new_messages
```

Old messages get replaced.

With it:

```python
messages += new_messages
```

Conversation history is preserved.

---

## Why Use `BaseMessage`?

Because LangGraph supports multiple message types:

* HumanMessage
* AIMessage
* SystemMessage
* ToolMessage

All inherit from:

```python
BaseMessage
```

---

# Quick Revision Notes

```text
State = Shared Memory

messages = Chat History

HumanMessage = User Input

AIMessage = AI Response

Annotated = Add extra behavior

add_messages = Append history

graph.invoke() = Execute workflow
```

# Author

Mohit Kumar

GitHub:
https://github.com/ohitsingh
