# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the AI Assist Agent component of a multi-agent AI system. The Assist Agent specializes in content creation and task execution, including document authoring (PDF, Word, Markdown), visual creation (charts, diagrams, images), and template-based content generation.

## Architecture

### Multi-Agent System
- **Assist Agent** (this project): Content creation and task execution using Streamlit + CrewAI + Milvus
- **Research Agent** (separate repo): Information gathering and research using FastAPI + Milvus + Ollama
- **Conductor Agent** (future): Request routing and workflow orchestration using FastAPI + CrewAI

### Inter-Agent Communication
- Uses Redis pub/sub for message passing between agents
- FastAPI endpoints for API communication (port 8502)
- Shared volume mounts for file exchange (`/app/outputs`)
- Standardized JSON message schema with request_id, source/target agents, and payloads

## Development Commands

### Docker Environment
```bash
# Start all services (includes Ollama, Milvus, Redis, OpenWebUI)
docker-compose up -d

# Start just the assist agent
docker-compose up assist-agent

# View logs
docker-compose logs -f assist-agent

# Rebuild after code changes
docker-compose up --build assist-agent
```

### Local Development
```bash
# Install dependencies
pip install -r requirements.txt

# Run Streamlit app
streamlit run app.py

# Run with custom port
streamlit run app.py --server.port 8501
```

## Key Dependencies

- **Streamlit**: Primary UI framework
- **CrewAI**: Multi-agent orchestration
- **Milvus**: Vector database for embeddings
- **FastAPI**: Inter-agent communication APIs
- **Redis**: Message broker for agent communication
- **ReportLab/python-docx**: Document generation
- **Plotly/Matplotlib**: Data visualization

## Environment Configuration

Copy `.env.example` to `.env` and configure:
- API keys for OpenRouter, OpenAI, Google, Anthropic, DeepSeek
- Default model: `anthropic/claude-3-sonnet`
- Service endpoints (auto-configured in Docker)

## Container Services

- **assist-agent**: Ports 8501 (Streamlit), 8502 (FastAPI)
- **ollama**: Port 11434 (local LLM models)
- **openwebui**: Port 3000 (LLM interface)
- **milvus**: Port 19530 (vector database)
- **redis**: Port 6379 (message broker)

## Current State

The project is in Phase 1 (Assist Agent Foundation) with basic Streamlit interface and Docker containerization. The main application file `app.py` currently contains a simple "Hello World" Streamlit app that serves as a starting point for the full agent implementation.