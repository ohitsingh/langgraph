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

```text id="x7m2qp"
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

```text id="n4r8tx"
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

```text id="m7q1zp"
"What is the weather in California?"
```

or

```text id="b5r9tw"
"I need expert guidance for building an AI agent."
```

---

# Step 2 — LangGraph State Management

The workflow is built using:

```python id="v3x8hk"
StateGraph(State)
```

The graph maintains conversation state across interactions.

Example state:

```python id="h8n2rm"
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

```python id="u4m7qc"
def chatbot(state):
```

The LLM processes:

* user message
* previous messages
* tool outputs

---

# Step 4 — Tool Decision Routing

LangGraph evaluates:

```python id="k2t9xs"
tools_condition
```

The LLM decides whether:

* tool execution is required
* direct response is sufficient

---

# Step 5 — ToolNode Execution

If tools are required:

```python id="y5r1nv"
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

```python id="z7q4pm"
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

```text id="f2m8wy"
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

| Component         | Responsibility                  |
| ----------------- | ------------------------------- |
| `StateGraph`      | Workflow orchestration          |
| `ToolNode`        | Tool execution                  |
| `tools_condition` | Conditional routing             |
| `interrupt()`     | Human-in-loop support           |
| `MemorySaver`     | Persistent memory               |
| `ChatGroq`        | LLM reasoning                   |
| `TavilySearch`    | External web search             |
| `add_messages`    | Conversation history management |

---

# Key LangGraph Concepts Covered

## State Management

```python id="u6p3rt"
Annotated[list, add_messages]
```

Maintains chat history automatically.

---

## Conditional Edges

```python id="z4w8kc"
graph_builder.add_conditional_edges(
    "chatbot",
    tools_condition
)
```

Dynamically routes workflow execution.

---

## Tool Calling

```python id="p9m1xh"
llm.bind_tools(tools)
```

Enables LLM-driven tool usage.

---

## Human-in-the-Loop

```python id="g7x5rv"
interrupt()
```

Pauses graph execution for human input.

---

## Memory Persistence

```python id="r5t2qn"
MemorySaver()
```

Stores conversation state between interactions.

---

# Example Tool Registration

## Human Assistance Tool

```python id="w3m7pz"
@tool
def human_assistance(query: str) -> str:
    human_response = interrupt({"query": query})
    return human_response["data"]
```

---

## Tavily Search Tool

```python id="m2x8vy"
tool = TavilySearch(max_results=2)
```

Used for external web search capabilities.

---

# Example Workflow Setup

```python id="y8t1mc"
graph_builder = StateGraph(State)

graph_builder.add_node("chatbot", chatbot)

tool_node = ToolNode(tools=tools)

graph_builder.add_node("tools", tool_node)

graph_builder.add_conditional_edges(
    "chatbot",
    tools_condition
)

graph_builder.add_edge("tools", "chatbot")

graph_builder.add_edge(START, "chatbot")
```

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

```bash id="t4r7nx"
git clone https://github.com/ohitsingh/langgraph.git

cd langgraph
```

---

## 2. Create Virtual Environment

### Windows

```bash id="v9q2pk"
python -m venv .venv

.venv\Scripts\activate
```

### Linux/macOS

```bash id="b7m1xs"
python3 -m venv .venv

source .venv/bin/activate
```

---

## 3. Install Dependencies

```bash id="f3r8wy"
pip install -r requirements.txt
```

---

## 4. Configure Environment Variables

Create `.env`

```env id="u2m5qt"
GROQ_API_KEY=your_groq_api_key
TAVILY_API_KEY=your_tavily_api_key
```

---

# Run Project

```bash id="w6x9pc"
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

# Author

Mohit Kumar

GitHub:
https://github.com/ohitsingh
