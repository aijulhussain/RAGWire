# Build a Multi-Agent RAG System: LangGraph, AutoGen, CrewAI & FastAPI

## Course Description

Retrieval-Augmented Generation (RAG) is the backbone of every serious AI application today. But single-agent RAG quickly hits its limits when queries are complex, documents are large, or answers require research, writing, and review working together.

In this course, you will build **RAGWire** — a production-grade, multi-agent document intelligence platform — from the ground up. You will start by mastering RAGWire across two real-world domains (private finance and personal gym supplements), learn to swap providers and tune retrieval components, then build a full FastAPI + Chainlit application and progressively wire it to eight agent implementations across five frameworks: LangChain, LangGraph (self-correcting and supervisor), CrewAI (single-agent and multi-agent sequential crew), Microsoft AutoGen, and Microsoft Agent Framework (single-agent and multi-agent supervisor workflow). Every agent shares the same backend and frontend, so you can see exactly what each framework contributes and where it excels.

By the end of this course you will know how to:
- Configure RAGWire for any domain and any provider — Ollama, OpenAI, Gemini, Groq, Anthropic, or HuggingFace
- Build a production FastAPI backend that serves any agent through a single OpenAI-compatible `/v1/chat/completions` endpoint
- Implement self-correcting RAG that grades its own retrieval and rewrites queries when quality is low
- Build supervisor multi-agent systems that route queries to specialist agents and synthesize their outputs
- Build sequential multi-agent crews with Researcher, Analyst, and Writer roles using CrewAI
- Orchestrate multi-agent report generation pipelines with Planner, Researcher, Writer, Critic, and Compiler roles using AutoGen
- Stream responses token-by-token from any agent to a Chainlit chat UI
- Export AI-generated reports to PDF with proper markdown rendering
- Build a supervisor multi-agent workflow with MS Agent Framework using conditional edges and typed message routing
- Choose the right agent framework for your use case based on hands-on experience with all of them

This is a hands-on, code-first course. Every module produces working, runnable code that you can adapt to your own documents and use cases.

---

## Who This Course Is For

- Python developers who want to move beyond basic RAG tutorials
- AI engineers building document Q&A or research automation systems
- Developers who want hands-on experience with LangGraph, AutoGen, and CrewAI
- Anyone who wants to understand how multi-agent systems work in practice

**Prerequisites:** Python, basic LangChain or RAG experience, familiarity with REST APIs

---

## Modules

### Module 1: RAGWire: Architecture and Core Concepts
- What is RAGWire and why it exists: production-grade RAG without infrastructure complexity
- The two pipelines: Ingestion (load → chunk → embed → store) and Retrieval (search → filter → rank)
- Supported providers: Google, OpenAI, Groq, Anthropic, Ollama, HuggingFace, FastEmbed
- Vector storage with Qdrant: dense, sparse, and hybrid search
- LLM-powered metadata extraction: company names, fiscal periods, document types
- Deduplication: SHA256 at file and chunk level
- Configuration-driven design: one `config.yaml` controls the entire pipeline

### Module 2: RAGWire Setup and First Retrieval in Jupyter Notebook
- Installing RAGWire: `pip install ragwire` and provider extras
- Writing your first `config.yaml`: embeddings, LLM, vectorstore, retriever settings
- Ingesting documents in a Jupyter notebook: PDFs, DOCX, XLSX
- Basic retrieval: `rag.retrieve()`, `top_k`, similarity vs MMR vs hybrid search
- Metadata-aware retrieval: `auto_filter`, `extract_filters()`, `get_filter_context()`
- Inspecting chunks, metadata, and deduplication behaviour
- Testing filter accuracy: multi-company and multi-year queries in the notebook

### Module 3: RAGWire in Practice — Providers, Components, and Cookbooks
- RAGWire with all providers: switching between Ollama, OpenAI, Gemini, Groq, Anthropic, and HuggingFace via `config.yaml`
- Component usage deep-dive: embedder, chunker, retriever, and reranker as standalone building blocks
- Cookbooks: practical patterns — hybrid search tuning, reranking, sparse vs dense trade-offs
- Choosing the right provider and retrieval strategy for your use case

### Module 4: Personal Gym Supplements RAG
- Why a different domain: showing RAGWire works beyond finance with minimal config changes
- Ingesting health supplement product data: PDFs and unstructured text
- Building a domain-specific retrieval pipeline with metadata filtering
- Querying across products, ingredients, and use cases
- Comparing retrieval quality vs the finance RAG from Module 2

### Module 5: Conversational RAG Chatbot with Chainlit
- Building a simple end-to-end RAG chatbot: RAGWire + LangChain agent + Chainlit in a single file
- `on_chat_start` and `on_message` handlers
- Document upload via drag-and-drop with `ingest_directory`
- Conversational memory with `InMemorySaver` per session
- Using `get_filter_context` and `search_documents` tools with `ainvoke`
- Model: `gemini-3.1-flash-lite-preview`

### Module 6: Build a FastAPI RAG Backend with LangChain Agent
- Setting up the project: Python environment, `.env`, dependencies
- RAGWire system overview: FastAPI backend, Chainlit frontend, Qdrant vector store
- The shared tools layer: `search_documents`, `get_filter_context`
- Wiring the ingestion pipeline into FastAPI: using the collection built in Modules 2–3
- The OpenAI-compatible `/v1/chat/completions` API contract
- `create_agent` with tool calling
- System prompt design for document assistants
- Streaming with `astream_events` — filtering tool call chunks
- How the `routes.py` agent loader works (`AGENT` env variable)
- Testing API endpoints with Postman: chat completions, streaming, upload

### Module 7: Build the Chainlit Chat Frontend
- Chainlit architecture: `on_message`, `on_chat_start`, `on_chat_resume`
- SSE streaming from FastAPI to Chainlit with `httpx`
- Persistent chat history with SQLAlchemy + SQLite data layer
- Password authentication with `@cl.password_auth_callback`
- Document upload via drag-and-drop
- Action buttons: adding a **Download PDF** button to every AI response
- PDF export: converting markdown responses (tables, citations, bold figures) to PDF with `markdown2` and `xhtml2pdf`

### Module 8: Self-Correcting RAG with LangGraph
- LangGraph concepts: State, Nodes, Edges, Conditional routing
- The self-correction loop: retrieve → grade docs → generate → grade answer → rewrite
- Grading retrieval quality and answer quality with the LLM
- Query rewriting with reason-aware prompts
- Fallback handling after max iterations

### Module 9: Multi-Agent Supervisor System with LangGraph
- Supervisor pattern: one orchestrator routes to specialist agents
- Financial, Legal & Risk, Technical, and Summary specialists
- Building specialist nodes with `make_specialist`
- Synthesis: combining specialist outputs into one coherent answer
- Routing with conditional edges and iteration limits

### Module 10: Single-Agent RAG with CrewAI
- Architecture comparison: LangChain (simple tool-calling agent), LangGraph (stateful graph with self-correction and supervisor routing), CrewAI (role-based crews with declarative task pipelines), Microsoft Agent Framework (graph-based workflows with conditional edges), and AutoGen (multi-agent conversation loops with round-robin orchestration) — trade-offs and when to pick each
- CrewAI concepts: Agent, Task, Crew
- Using the native `LLM` class with LiteLLM for Gemini
- Wrapping LangChain tools with `@crewai_tool`
- Async `kickoff_async` and why CrewAI doesn't stream

### Module 11: Multi-Agent Sequential Crew with CrewAI
- Sequential process: each agent's output becomes the next agent's context
- Three-agent pipeline: Researcher → Analyst → Writer
- Researcher retrieves raw document chunks; Analyst structures findings; Writer composes the final answer
- Designing task descriptions and `expected_output` for tight handoffs
- Using `context=[prev_task]` to chain tasks across agents
- When to use sequential crew vs single agent: depth of analysis vs latency

### Module 12: Multi-Agent Report Generator with Microsoft AutoGen
- AutoGen concepts: AssistantAgent, RoundRobinGroupChat, termination conditions
- Five-agent pipeline: Planner → Researcher → Writer → Critic → Compiler
- Designing tight, focused system prompts for each role
- Streaming with `run_stream` — surfacing only the Compiler's final output
- Working around Gemini compatibility constraints (RoundRobin vs Selector)
- Producing structured reports with markdown tables and inline citations
- Note: AutoGen is in maintenance mode — Microsoft Agent Framework is the recommended successor

### Module 13: Build a RAG Agent with Microsoft Agent Framework
- Microsoft Agent Framework concepts: Agent, Workflow, WorkflowBuilder, Executor
- Why Agent Framework supersedes both AutoGen and Semantic Kernel
- Pointing `OpenAIChatCompletionClient` at Gemini's OpenAI-compatible endpoint
- Implementing tools with `Annotated` + `Field` (no LangChain decorators)
- Streaming with `agent.run(stream=True)`

### Module 14: Multi-Agent Supervisor Workflow with Microsoft Agent Framework
- Workflows vs Agents: when to use a graph over a single agent
- Edge types: direct, conditional, switch-case, fan-out/fan-in
- Carrying state in typed Pydantic messages instead of a shared state dict
- Supervisor loop with conditional edges: routing to specialist executors at runtime
- Class-based `Executor` with `@handler` decorator
- Comparing with LangGraph: `add_conditional_edges` vs `add_edge(..., condition=fn)`

### Module 15: Deployment, Configuration, and Next Steps
- Running FastAPI and Chainlit as separate services
- Environment configuration for different agents
- Swapping vector stores and embedding models
- Ideas for extending RAGWire: memory, web search, authentication, multi-tenancy
- Where to go next: agents in production, evaluation, observability
