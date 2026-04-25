# Multi-Agent Counseling System

A production-style AI application that performs candidate counseling analysis using an orchestrated, multi-agent architecture.  
The system combines domain-specialized agents, retrieval-augmented generation (RAG), and stateful routing to deliver contextual, explainable responses.

## Why This Project

Traditional single-agent chat systems struggle to provide consistent depth across domains such as psychology, career, health, family dynamics, and value systems.  
This project addresses that by introducing:

- An orchestrator that routes each query to the most relevant specialist agent(s)
- Chained multi-agent execution for complex, cross-domain questions
- Report-grounded responses using RAG over uploaded/local documents
- Stateful conversation memory for continuity across turns

## Core Capabilities

- **Intelligent Routing:** Automatically selects domain-specific agents based on user intent.
- **Multi-Agent Chaining:** Supports sequential execution of multiple agents in one request.
- **RAG-Backed Context:** Retrieves relevant chunks from indexed reports for grounded output.
- **Dual Interface:** CLI (`main.py`) and Streamlit UI (`streamlit_app/main_ui.py`).
- **Operational Visibility:** Token/cost tracking, executed-agent trace, and debug state panels.
- **Flexible Data Sources:** Local report directory or MongoDB-backed report ingestion.

## High-Level Architecture

1. User submits a query from CLI or Streamlit UI.
2. Orchestrator analyzes intent and determines the execution plan.
3. Selected specialist agent(s) retrieve relevant context from RAG.
4. Agents generate structured analysis using domain prompts.
5. Responses are merged and returned with execution trace.
6. Session state is updated for follow-up continuity.

## Tech Stack

- **Language:** Python
- **LLM Orchestration:** LangGraph / LangChain patterns
- **Model Provider:** OpenAI API
- **UI:** Streamlit
- **Retrieval:** Local vector index + cached RAG artifacts
- **Data Inputs:** PDF reports (local/MongoDB)

## Repository Structure

```text
.
├── app/
│   ├── agents/              # Specialized domain agents + orchestrator
│   ├── prompts/             # System/router prompts per domain
│   ├── knowledge/           # Knowledge loading, RAG, MongoDB integration
│   ├── models/              # Data models and report schemas
│   └── state/               # Shared AgentState contract
├── streamlit_app/
│   └── main_ui.py           # Web UI for interactive testing/demo
├── main.py                  # CLI entrypoint
└── requirements.txt
```

## Getting Started

### 1) Prerequisites

- Python 3.10+
- OpenAI API key

### 2) Install dependencies

```bash
pip install -r requirements.txt
```

### 3) Configure environment

Create `.env`:

```env
OPENAI_API_KEY=your_api_key_here
```

### 4) Run the application

CLI mode:

```bash
python main.py
```

Streamlit UI:

```bash
python -m streamlit run streamlit_app/main_ui.py
```

## How a Request Is Processed

1. Query is appended to session `state["messages"]` for conversational memory.
2. Orchestrator graph invokes routing logic.
3. Relevant agent(s) run with domain prompts and retrieved report chunks.
4. Outputs are added to `state["agent_data"]` and response messages.
5. UI/CLI displays final response with agent execution context.

## Resume-Ready Highlights

- Designed and implemented a **modular multi-agent architecture** for counseling analysis.
- Built **RAG-enabled retrieval pipeline** to ground LLM outputs in candidate reports.
- Added **orchestrator-based intelligent routing** with support for multi-agent chaining.
- Developed both **CLI and Streamlit interfaces** for debugging, demos, and usability.
- Implemented **stateful session management** and **token/cost observability** for operational control.

## Future Enhancements

- Agent-level confidence scoring and response quality evaluation
- Persistent conversation/session storage with analytics dashboard
- Automated report ingestion pipeline and background indexing workers
- Role-based access and deployment-ready API layer

## License

This project is available for educational and portfolio demonstration purposes.
