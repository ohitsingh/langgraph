# LangGraph AI Projects

A collection of hands-on AI agent projects built using:

* LangGraph
* LangChain
* MCP (Model Context Protocol)
* Groq LLMs
* Tool Calling
* Memory
* Human-in-the-Loop Workflows

This repository contains practical implementations of modern Agentic AI concepts including stateful agents, tool orchestration, MCP servers, memory-enabled chatbots, and async multi-tool workflows.

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

# Features

* Stateful AI Chatbots
* Memory-enabled Conversations
* Tool Calling Agents
* MCP Client & Server Integration
* Multi-Server MCP Support
* Async Agent Execution
* Human-in-the-Loop Interrupts
* Weather MCP Server
* Math MCP Server
* LangGraph Workflow Orchestration

---

# Project Structure

```text
langgraph/
│
├── 1-BasicChatBot/
│
├── mathserver.py
├── weather.py
├── client.py
├── main.py
│
├── requirements.txt
├── pyproject.toml
├── README.md
└── .gitignore
```

---

# MCP Architecture

```text
User
  ↓
LangChain Agent
  ↓
LangGraph Workflow
  ↓
MCP Client
  ↓
┌─────────────────────┐
│  Math MCP Server    │
│  Weather MCP Server │
└─────────────────────┘
```

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

# Run MCP Weather Server

```bash
python weather.py
```

---

# Run MCP Client

```bash
python client.py
```

---

# Example Queries

```text
What is weather in California?

Multiply 8 and 12

Add 10 and 15
```

---

# LangGraph Concepts Covered

* StateGraph
* START Node
* ToolNode
* Conditional Edges
* Interrupts
* MemorySaver
* add_messages
* Tool Calling
* Async Workflows
* Multi-Agent Patterns

---

# Learning Goals

This repository is focused on learning and experimenting with:

* Agentic AI
* AI Orchestration
* MCP Protocol
* Production-style AI workflows
* Tool-integrated LLM systems
* Multi-step reasoning systems

---

# Future Improvements

* RAG Integration
* Multi-Agent Systems
* Vector Databases
* Streaming Responses
* LangSmith Observability
* Supervisor Agents
* Human Approval Pipelines
* Deployment with FastAPI

---

# References

* LangGraph Official Repository
* LangChain Documentation
* MCP Protocol
* Groq API

---

# Author

Mohit Kumar

LinkedIn: https://www.linkedin.com/

GitHub: https://github.com/ohitsingh
