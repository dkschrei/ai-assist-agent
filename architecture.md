# Multi-Agent AI System Architecture

## System Overview
A three-agent orchestration system designed for comprehensive AI assistance.

## Agent Roles

### 🎯 Conductor Agent (Future)
- **Purpose**: Request routing and workflow orchestration
- **Responsibilities**:
  - Parse user requests
  - Determine which agents to involve
  - Orchestrate multi-agent workflows
  - Aggregate and present final results
- **Technology**: FastAPI + CrewAI orchestration

### 🔍 Research Agent (Existing Repo)
- **Repository**: https://github.com/dkschrei/ai-research-agent
- **Purpose**: Information gathering and due diligence
- **Capabilities**:
  - Web search and scraping
  - Document analysis and RAG
  - Multi-model intelligence (llama3.1, gemma2, qwen2.5)
  - Research report generation
- **Technology**: FastAPI + Milvus + Ollama

### ⚡ Assist Agent (This Project)
- **Purpose**: Content creation and task execution
- **Capabilities**:
  - Document authoring (PDF, Word, Markdown)
  - Visual creation (charts, diagrams, images)
  - Plan design and strategy documents
  - Template-based content generation
- **Technology**: Streamlit + CrewAI + Milvus

## Communication Protocol

### Inter-Agent Communication
- **Message Bus**: Redis pub/sub
- **API Gateway**: FastAPI endpoints for each agent
- **Data Exchange**: JSON payloads with standardized schemas
- **File Sharing**: Shared volume mounts

### Message Schema
```json
{
  "request_id": "uuid",
  "source_agent": "conductor|research|assist",
  "target_agent": "conductor|research|assist",
  "task_type": "research|create|orchestrate",
  "payload": {...},
  "metadata": {
    "timestamp": "ISO8601",
    "priority": "high|medium|low",
    "user_context": {...}
  }
}
```

## Workflow Examples

### Example 1: Market Research Report
1. **User** → Conductor: "Create a market analysis report for AI startups"
2. **Conductor** → Research: "Gather data on AI startup market trends"
3. **Research** → Conductor: Returns market data and insights
4. **Conductor** → Assist: "Create professional report with visualizations"
5. **Assist** → Conductor: Returns formatted PDF report
6. **Conductor** → User: Delivers final report

### Example 2: Strategic Planning
1. **User** → Conductor: "Help me plan product launch strategy"
2. **Conductor** → Research: "Research competitive landscape and market timing"
3. **Research** → Conductor: Returns competitive analysis
4. **Conductor** → Assist: "Create strategic plan document with timeline"
5. **Assist** → Conductor: Returns strategy document with Gantt charts
6. **Conductor** → User: Delivers complete strategic plan

## Deployment Architecture

### Container Services
- **assist-agent**: Streamlit UI + FastAPI API (ports 8501, 8502)
- **research-agent**: FastAPI service (port 8503) - from existing repo
- **conductor-agent**: FastAPI orchestration (port 8504) - future
- **ollama**: Local LLM models (port 11434)
- **openwebui**: LLM interface (port 3000)
- **milvus**: Vector database (port 19530)
- **redis**: Message broker (port 6379)

### Shared Resources
- **volumes/shared_outputs**: Cross-agent file sharing
- **agent_network**: Internal Docker network
- **External APIs**: OpenRouter for cloud models

## Development Phases

### Phase 1: Assist Agent Foundation ✅
- Docker containerization
- Basic Streamlit interface
- Document creation capabilities
- Visual generation tools

### Phase 2: Inter-Agent Communication
- FastAPI endpoints
- Redis message broker
- Shared file system
- Basic orchestration

### Phase 3: Research Agent Integration  
- Connect existing research repo
- Cross-agent workflow testing
- Data exchange protocols

### Phase 4: Conductor Agent
- Request parsing and routing
- Workflow orchestration
- User interface consolidation
- Complete system integration