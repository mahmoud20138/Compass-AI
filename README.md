# 🧭 Compass AI

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.11+-blue.svg" alt="Python 3.11+">
  <img src="https://img.shields.io/badge/FastAPI-0.115+-green.svg" alt="FastAPI">
  <img src="https://img.shields.io/badge/License-MIT-green.svg" alt="License">
  <img src="https://img.shields.io/badge/Multi--Provider-orange.svg" alt="Multi-Provider">
  <img src="https://img.shields.io/badge/AI-Orchestration-purple.svg" alt="AI Orchestration">
</p>

> **Intelligent AI orchestration platform** with multi-provider routing, automatic failover, and cost optimization. Routes tasks to optimal AI models (OpenAI, Claude, Gemini, etc.) based on complexity, cost, latency, and accuracy requirements.

---

## 🎯 Key Features

| Feature | Description |
|---------|-------------|
| 🔀 **Multi-Provider Routing** | Routes tasks to optimal model (GPT-4, Claude, Gemini, etc.) |
| 🛡️ **Automatic Failover** | 3-layer failover: Provider → Model → Agent |
| 💰 **Cost Optimization** | Real-time token counting, budget tracking, model tiering |
| ⚡ **Circuit Breaker** | Prevents cascade failures with smart retry policies |
| 📊 **Multi-Agent Orchestration** | Sequential, parallel, hierarchical, and consensus patterns |
| 📈 **Performance Optimization** | Caching, streaming, context compression, predictive pre-loading |
| 🔄 **State Management** | Hot (Redis), Warm (PostgreSQL), Cold (S3) storage |
| 📉 **Observability** | Prometheus metrics, structured logging, distributed tracing |

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              COMPASS AI                                     │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │                     CONTROL PLANE (Compass Core)                     │    │
│  │  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐   │    │
│  │  │   Task      │ │    Model    │ │   Health    │ │     Cost    │   │    │
│  │  │ Classifier  │ │  Selection  │ │   Monitor   │ │  Optimizer  │   │    │
│  │  │   & Router  │ │   Engine    │ │   + Breaker │ │             │   │    │
│  │  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘   │    │
│  │  ┌─────────────┐ ┌─────────────┐                                    │    │
│  │  │    Flow     │ │  Analytics  │                                    │    │
│  │  │    State    │ │  & Telemetry│                                    │    │
│  │  │   Manager   │ │   Engine    │                                    │    │
│  │  └─────────────┘ └─────────────┘                                    │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                    │                                        │
│                                    ▼                                        │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │                         DATA PLANE                                    │    │
│  │  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐   │    │
│  │  │    Agent    │ │    Sub      │ │   Context   │ │    Token    │   │    │
│  │  │    Pool     │ │   -Agent    │ │    Store    │ │   Counter   │   │    │
│  │  │   Manager   │ │   Spawner   │ │  (Memory)   │ │   Service   │   │    │
│  │  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘   │    │
│  │                                                                         │    │
│  │  ┌─────────────────────────────────────────────────────────────┐    │    │
│  │  │              COMMUNICATION BUS (Message Queue)               │    │    │
│  │  └─────────────────────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                    │                                        │
│                                    ▼                                        │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │                    PROVIDER ABSTRACTION LAYER                         │    │
│  │                                                                         │    │
│  │  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────────┐   │    │
│  │  │ OpenAI  │ │ Claude  │ │ Google  │ │ Azure   │ │  Open Source│   │    │
│  │  │   GPT   │ │Claude 3 │ │  Gemini │ │OpenAI   │ │  Llama/Mistral│   │    │
│  │  └─────────┘ └─────────┘ └─────────┘ └─────────┘ └─────────────┘   │    │
│  │                                                                         │    │
│  │  ┌─────────────────────────┐ ┌────────────────────────────────────┐  │    │
│  │  │   Rate Limit Manager    │ │       Failover Controller         │  │    │
│  │  └─────────────────────────┘ └────────────────────────────────────┘  │    │
│  └─────────────────────────────────────────────────────────────────────┘    │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 📊 Data Flow Pipeline

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              DATA FLOW                                      │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  Client Request ─────────────────────────────────────────► Task Input       │
│       │                                                                   │
│       ▼                                                                   │
│  ┌────────────────────────────────────────────────────────────────────┐   │
│  │                    TASK CLASSIFIER (Stage 1)                         │   │
│  │  • Keyword matching (code → Codex priority)                         │   │
│  │  • Token count (<500 → lightweight models)                          │   │
│  │  • Urgency flags (real-time → fastest available)                   │   │
│  └────────────────────────────────────────────────────────────────────┘   │
│       │                                                                   │
│       ▼                                                                   │
│  ┌────────────────────────────────────────────────────────────────────┐   │
│  │              COMPLEXITY SCORING ENGINE (Stage 2)                    │   │
│  │  • Sentence structure analysis                                     │   │
│  │  • Technical term density                                          │   │
│  │  • Multi-step reasoning indicators                                 │   │
│  │  • Domain specificity (legal, medical, technical)                  │   │
│  └────────────────────────────────────────────────────────────────────┘   │
│       │                                                                   │
│       ▼                                                                   │
│  ┌────────────────────────────────────────────────────────────────────┐   │
│  │              MODEL SELECTION ENGINE (Stage 3)                        │   │
│  │                                                                         │
│  │  ┌──────────────────────────────────────────────────────────────┐    │   │
│  │  │              DECISION MATRIX (Weighted Scoring)              │    │   │
│  │  │  ┌────────────────┬────────┬────────────────┐                 │    │   │
│  │  │  │   Dimension   │ Weight │  Measurement  │                 │    │   │
│  │  │  ├────────────────┼────────┼────────────────┤                 │    │   │
│  │  │  │ Task Complexity│  30%   │ Keyword +     │                 │    │   │
│  │  │  │                │        │ historical    │                 │    │   │
│  │  │  ├────────────────┼────────┼────────────────┤                 │    │   │
│  │  │  │  Token Budget │  25%   │ Input +       │                 │    │   │
│  │  │  │                │        │ expected out  │                 │    │   │
│  │  │  ├────────────────┼────────┼────────────────┤                 │    │   │
│  │  │  │Latency Req    │  20%   │ Real-time vs  │                 │    │   │
│  │  │  │                │        │ batch         │                 │    │   │
│  │  │  ├────────────────┼────────┼────────────────┤                 │    │   │
│  │  │  │ Cost Constraints│ 15%   │ Budget per    │                 │    │   │
│  │  │  │                │        │ task type     │                 │    │   │
│  │  │  ├────────────────┼────────┼────────────────┤                 │    │   │
│  │  │  │Accuracy Req    │  10%   │ Mission-critical│                │    │   │
│  │  │  │                │        │ vs exploratory│                 │    │   │
│  │  │  └────────────────┴────────┴────────────────┘                 │    │   │
│  │  └──────────────────────────────────────────────────────────────┘    │   │
│  │                                                                         │   │
│  └────────────────────────────────────────────────────────────────────┘   │
│       │                                                                   │
│       ▼                                                                   │
│  ┌────────────────────────────────────────────────────────────────────┐   │
│  │              TIER CLASSIFICATION                                     │   │
│  │                                                                         │
│  │  ┌─────────────┬────────────────────────────────────────────────┐     │   │
│  │  │   Tier 1    │  Premium: GPT-4, Claude Opus                  │     │   │
│  │  │  (Premium)  │  Complex reasoning, critical tasks           │     │   │
│  │  ├─────────────┼────────────────────────────────────────────────┤     │   │
│  │  │   Tier 2    │  Balanced: GPT-3.5-turbo-16k, Claude Sonnet  │     │   │
│  │  │  (Balanced) │  General tasks                                │     │   │
│  │  ├─────────────┼────────────────────────────────────────────────┤     │   │
│  │  │   Tier 3    │  Fast: Claude Haiku, GPT-3.5-turbo            │     │   │
│  │  │  (Fast)     │  Simple tasks, high volume                    │     │   │
│  │  ├─────────────┼────────────────────────────────────────────────┤     │   │
│  │  │   Tier 4    │  Specialized: Codex (code), Embeddings       │     │   │
│  │  │  (Specialized)│  Search, code generation                    │     │   │
│  │  └─────────────┴────────────────────────────────────────────────┘     │   │
│  │                                                                         │   │
│  └────────────────────────────────────────────────────────────────────┘   │
│       │                                                                   │
│       ▼                                                                   │
│  ┌────────────────────────────────────────────────────────────────────┐   │
│  │              INTELLIGENT ROUTING RULES                               │   │
│  │  1. Safety Rules (never violate): Cost ceiling, max retries, timeout │   │
│  │  2. Optimization Rules (best effort): Cache, load balance, arbitrage│   │
│  │  3. Learning Rules (adaptive): Bayesian updates, A/B testing        │   │
│  └────────────────────────────────────────────────────────────────────┘   │
│       │                                                                   │
│       ▼                                                                   │
│  ┌────────────────────────────────────────────────────────────────────┐   │
│  │              FAILOVER EXECUTION                                     │   │
│  │                                                                         │
│  │  Layer 1: Provider    (OpenAI → Claude → Azure → Replicate)         │   │
│  │  Layer 2: Model      (GPT-4 → GPT-4-turbo → GPT-3.5)                │   │
│  │  Layer 3: Agent      (Parallel agents, consensus scoring)          │   │
│  │                                                                         │   │
│  └────────────────────────────────────────────────────────────────────┘   │
│       │                                                                   │
│       ▼                                                                   │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐        │
│  │   Response  │ │    Cost     │ │   Metrics   │ │    Cache    │        │
│  │             │ │   Tracking  │ │   Logging   │ │   Storage   │        │
│  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘        │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 🔀 Multi-Provider Support

| Provider | Models | Status |
|----------|--------|--------|
| **OpenAI** | GPT-4, GPT-4-turbo, GPT-3.5-turbo | ✅ |
| **Anthropic** | Claude 3 Opus, Sonnet, Haiku | ✅ |
| **Google** | Gemini Pro, Gemini Ultra | ✅ |
| **Azure OpenAI** | GPT-4, GPT-3.5 (Enterprise) | ✅ |
| **Open Source** | Llama, Mistral, CodeLlama (via Replicate) | ✅ |

---

## 🛡️ Circuit Breaker System

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         CIRCUIT BREAKER STATES                              │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────┐      ┌─────────────┐      ┌─────────────┐                │
│  │    CLOSED   │ ──►  │    OPEN     │ ──►  │  HALF-OPEN  │                │
│  │             │      │             │      │             │                │
│  │ Normal      │      │ Provider    │      │  Testing    │                │
│  │ operation   │      │ disabled    │      │  recovery   │                │
│  │             │      │             │      │             │                │
│  │ Monitor     │      │ Route       │      │  Allow      │                │
│  │ failures    │      │ around      │      │  limited    │                │
│  └─────────────┘      └─────────────┘      └─────────────┘                │
│       │                     │                     │                        │
│       ▼                     │                     ▼                        │
│  ┌────────────────────────────────────────────────────────────────────┐   │
│  │                    THRESHOLDS                                       │   │
│  │  • Failure rate: >20% in 60-second window                          │   │
│  │  • Consecutive failures: 5                                        │   │
│  │  • Recovery test: 3 successful requests                           │   │
│  │  • Timeout: Exponential backoff (1min → 5min → 15min → 1hr)       │   │
│  └────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 💰 Cost Optimization

### Real-Time Cost Tracking

| Component | Description |
|-----------|-------------|
| **Token Counter** | Provider-specific algorithms (tiktoken for OpenAI) |
| **Cost Calculator** | Input + Output tokens × provider pricing |
| **Budget Allocator** | Per-workflow, per-agent budget limits |
| **Alert Thresholds** | 50%, 75%, 90% budget warnings |

### Cost-Benefit Matrix

```
Task Type          │ Tier 1 │ Tier 2 │ Tier 3 │ Recommended
───────────────────┼────────┼────────┼────────┼────────────
Simple QA          │ $0.10  │ $0.02  │ $0.001│ Tier 3
Code Generation    │ $0.30  │ $0.08  │ N/A   │ Tier 2 (Codex)
Complex Reasoning  │ $0.50  │ $0.10  │ $0.02 │ Tier 1
Data Extraction    │ $0.20  │ $0.05  │ $0.01 │ Tier 3
Creative Writing   │ $0.40  │ $0.09  │ $0.03 │ Tier 2
```

---

## 🔄 Agent Communication Patterns

### 1. Sequential Chain
```
Agent A → Agent B → Agent C
Each depends on previous output
Optimize: Pipeline parallelism where possible
```

### 2. Parallel Fan-Out/Fan-In
```
      ┌─ Agent A ─┐
Task ─┤─ Agent B ─├─► Aggregator
      └─ Agent C ─┘
Execute on different models
Aggregate with consensus or voting
```

### 3. Hierarchical Delegation
```
Master Agent
    │
    ├─► Sub-Agent A (Code)
    │
    ├─► Sub-Agent B (Research)
    │
    └─► Sub-Agent C (Synthesis)
Tree structure with recursive decomposition
```

### 4. Debate/Consensus
```
      ┌─ Agent A (GPT-4) ──┐
Task ─┤─ Agent B (Claude) ─├─► Judge Agent
      └─ Agent C (Gemini) ──┘
Compare outputs, resolve discrepancies
```

---

## 📈 Performance Optimization Strategies

| Strategy | Description |
|----------|-------------|
| **Parallel Processing** | Identify independent sub-tasks, execute simultaneously |
| **Intelligent Caching** | Semantic similarity (vector embeddings), partial reuse |
| **Predictive Pre-loading** | Pre-warm connections, pre-fetch common context |
| **Streaming** | Server-sent events, token-by-token routing |
| **Context Compression** | Summarize previous outputs, retrieval augmentation |

---

## 📁 Project Structure

```
Compass-AI/
├── pyproject.toml                # 📦 Project configuration
├── LICENSE                       # 📜 MIT License
├── CONTRIBUTING.md               # 🤝 Contributing guidelines
│
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.md
│   │   └── feature_request.md
│   └── PULL_REQUEST_TEMPLATE.md
│
├── src/
│   ├── __init__.py
│   │
│   ├── api/                      # 🌐 REST API
│   │   └── v1/
│   │       └── __init__.py
│   │
│   ├── core/                     # 🔧 Core utilities
│   │   └── __init__.py
│   │
│   ├── db/                      # 🗄️ Database layer
│   │   └── __init__.py
│   │
│   ├── models/                  # 📊 Data models
│   │   └── __init__.py
│   │
│   └── services/                # 🧠 Business logic
│       ├── __init__.py
│       │
│       ├── agents/              # 🤖 Agent management
│       ├── cache/               # 💾 Caching layer
│       ├── circuit_breaker/     # 🛡️ Failover logic
│       ├── cost/                # 💰 Cost tracking
│       ├── orchestrator/        # 🎯 Workflow orchestration
│       └── router/              # 🔀 Task routing
│
├── tests/                       # 🧪 Test suite
│   ├── unit/
│   └── integration/
│
├── CAI.txt                      # 📋 Comprehensive development plan
├── CLAUDE.md                    # 🤝 Project instructions
└── README.md                    # 📖 This file
```

---

## 🚀 Installation

### Prerequisites

| Requirement | Version |
|-------------|---------|
| 🐍 Python | 3.11+ |
| 🗄️ PostgreSQL | 14+ |
| 🧠 Redis | 7+ |

### Setup

```bash
# Clone repository
git clone https://github.com/mahmoud20138/Compass-AI.git
cd Compass-AI

# Create virtual environment
python -m venv venv

# Activate (Windows)
venv\Scripts\activate

# Activate (Linux/Mac)
source venv/bin/activate

# Install dependencies
pip install -e ".[dev]"

# Or with hatch
hatch env create
hatch shell
```

### Environment Configuration

Create `.env` file:

```env
# Database
DATABASE_URL=postgresql+asyncpg://user:pass@localhost:5432/compass_ai

# Redis
REDIS_URL=redis://localhost:6379/0

# OpenAI
OPENAI_API_KEY=sk-...

# Anthropic
ANTHROPIC_API_KEY=sk-ant-...

# Google
GOOGLE_API_KEY=...

# Azure
AZURE_OPENAI_KEY=...
AZURE_OPENAI_ENDPOINT=https://...
```

---

## 📖 Usage

### Start the Server

```bash
# Development
uvicorn src.api:app --reload --port 8000

# Production
uvicorn src.api:app --host 0.0.0.0 --port 8000 --workers 4
```

### API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/v1/tasks` | Submit new task |
| GET | `/api/v1/tasks/{id}` | Get task status |
| GET | `/api/v1/tasks/{id}/stream` | Stream task progress |
| POST | `/api/v1/workflows` | Submit workflow |
| GET | `/api/v1/metrics` | Get system metrics |
| POST | `/api/v1/admin/config` | Update routing rules |

### Python SDK

```python
from compass_ai import CompassAI

client = CompassAI(api_key="your-api-key")

# Submit task
task = await client.submit_task(
    prompt="Write a Python function to calculate fibonacci",
    complexity="medium"
)

# Get result
result = await task.result()
print(result.output)
```

---

## 🧪 Testing

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=src --cov-report=html

# Run specific test file
pytest tests/unit/test_router.py
```

---

## 📊 Monitoring

### Prometheus Metrics

| Metric | Description |
|--------|-------------|
| `compass_requests_total` | Total requests by model/provider |
| `compass_latency_seconds` | Response time (p50, p95, p99) |
| `compass_cost_dollars` | Real-time spend tracking |
| `compass_cache_hit_ratio` | Cache performance |
| `compass_failover_total` | Failover event counts |

### Dashboard

Access Grafana dashboard at `/metrics` for:
- Real-time request monitoring
- Cost analytics
- Provider health status
- Circuit breaker states

---

## 🤝 Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

---

## 📜 License

This project is licensed under the **MIT License** - see [LICENSE](LICENSE) for details.

---

## 🙏 Acknowledgments

- **LangChain/LangGraph** - Agent orchestration patterns
- **AutoGen (Microsoft)** - Multi-agent frameworks
- **FastAPI** - Web framework
- **Redis** - Caching and state management
- **PostgreSQL** - Data persistence

---

## 📋 Development Roadmap

### MVP (Phase 1)
- [ ] Basic task routing (3 models)
- [ ] Simple failover
- [ ] Token counting
- [ ] REST API

### V1.0 (Phase 2)
- [ ] All providers integrated
- [ ] ML-based model selection
- [ ] Advanced circuit breaker
- [ ] Agent workflows

### V2.0 (Phase 3)
- [ ] Multi-agent orchestration
- [ ] Self-healing & auto-scaling
- [ ] RL-based routing
- [ ] Enterprise features

---

<p align="center">
  Made with 🧭 for intelligent AI orchestration
</p>