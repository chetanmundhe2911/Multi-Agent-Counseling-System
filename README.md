# Multi-Agent Counseling System

AI-powered counseling assistant that uses an orchestrator to route user queries to specialized analysis agents (psychology, career, health, family, values, etc.) with RAG-backed report retrieval.

## Quick Start

1. Install dependencies:
   - `pip install -r requirements.txt`
2. Set API key in `.env`:
   - `OPENAI_API_KEY=your_api_key_here`
3. Run CLI app:
   - `python main.py`
4. Run Streamlit UI:
   - `python -m streamlit run streamlit_app/main_ui.py`

## Project Mental Model (Recall Notes)

### 1) What runs first

- Entry command loads the entry file (`main.py` for CLI, `streamlit_app/main_ui.py` for UI).
- Environment variables are loaded from `.env`.
- Orchestrator + knowledge base are initialized.
- State object is created to persist conversation and agent execution context.

### 2) UI flow (launch + first question)

1. Launch Streamlit (`streamlit_app/main_ui.py`).
2. Click **Initialize System**.
3. `initialize_system()` creates orchestrator and loads KB from local reports or MongoDB.
4. You type first message in chat input.
5. `process_user_input()` appends your message to `state["messages"]`.
6. Orchestrator graph runs and routes to one or more agents.
7. Agent responses are appended and shown in chat with agent labels.
8. UI reruns and preserves history via `st.session_state`.

### 3) What "state" means

State is the shared memory for one session. It helps maintain context across turns and coordinate multi-agent execution.

- `messages`: full conversation history used for contextual responses.
- `current_agent`: active/selected agent.
- `agent_chain`: ordered list of agents that ran.
- `next_agents`: queue for chained execution.
- `should_continue`: whether graph should keep chaining.
- `knowledge_base`: full KB object + retrieval context.
- `knowledge_base_summary`: compact KB text summary.
- `agent_data`: structured per-agent outputs for debugging and traceability.
- `candidate_id`: session identity for consistent tracking.

### 4) Why append user messages to state

Appending user input to `state["messages"]` ensures each new step sees prior context, so routing, retrieval, and responses stay coherent across the full conversation (not just the latest message).

## Common Commands

- Check current branch:
  - `git branch`
- Create local branch from remote:
  - `git checkout -b PreMarriageCounselingAgent origin/PreMarriageCounselingAgent`
- Run Streamlit:
  - `python -m streamlit run streamlit_app/main_ui.py`

## Notes

- Reports are loaded from `Report/` (or MongoDB if configured in UI).
- RAG cache is stored in `.rag_cache` for faster reloads.
- Streamlit app is a UI layer; core logic lives in `app/`.
