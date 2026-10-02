# Crisis Command

AI-assisted crisis intelligence and simulation platform for incident management, resource allocation, dispatch simulation, human-in-the-loop decisions, What-If analysis, counterfactual simulations, decision replay, geospatial visualization, AI explanations, and audit tracking.

---

> [!CAUTION]
> **SAFETY NOTICE**
> 
> **DEMONSTRATION / SIMULATION ONLY.**
> **NOT FOR REAL-WORLD EMERGENCY DISPATCH.**
> 
> This software is an experimental decision-support research simulation designed for hackathon and evaluation environments. It is not certified for life-critical dispatch or real-world emergency response operations.

---

## 1. Overview

During major municipal emergencies and multi-casualty incidents, events develop dynamically while emergency response resources (ambulances, fire engines, heavy rescue squads, hazmat units) are strictly constrained. Control-room commanders require continuous situational awareness, priority reassessment, clear explainability of trade-offs when resources are preempted, and deterministic safeguards.

**Crisis Command** provides an end-to-end tactical command center that models incident prioritization, fleet telemetry, multi-agent dispatch planning, human governance gates, counterfactual stress testing, and tamper-evident audit trails.

---

## 2. Key Features

- **Crisis Brain**: Autonomous multi-agent coordination hub aggregating incident triage, fleet state, and pending tactical decisions.
- **Chaos Mode**: Chaos engineering sandbox to inject synthetic stress events (fleet failure, localized storm surges, communication outages, incident escalation) and observe autonomous system adaptation.
- **Future Impact Predictor**: 10-minute forward-looking demand vs. supply projection identifying resource bottlenecks and containment threats.
- **Resource Dependency Graph**: Interactive SVG topology visualizer with real-time transit conduits, ETA midpoint labels, and full-screen tactical view.
- **Counterfactual Lab**: Side-by-side branch analysis comparing proposed reallocations against current baseline plans without mutating production state.
- **Decision Replay**: Step-by-step causal playback with historical event mapping (`incident_created`, `resource_failed`, `allocation_proposed`, `dispatch_executed`, etc.).
- **Confidence & Uncertainty**: Bayesian confidence scores and flagging of ambiguous field reports requiring operator verification.
- **Why This Resource / Why Not**: Explainability engine providing mathematical and operational justifications for why an asset was dispatched over alternatives.
- **Crisis Escalation Level**: Real-time regional threat gauge (Green &rarr; Yellow &rarr; Orange &rarr; Red &rarr; Black) derived from incident severity and resource strain.
- **Human Approval Gate**: Deterministic safety barrier requiring coordinator authorization for high-impact preemptions, lockouts, and critical reassignments.
- **Dispatch Simulation & Lifecycle**: Rigorous 6-state resource lifecycle tracking: `AVAILABLE` &rarr; `ASSIGNED` &rarr; `EN ROUTE` &rarr; `ON SCENE` &rarr; `COMPLETED` &rarr; `OFFLINE`.
- **Live Geospatial Map**: Leaflet-powered GIS tracking Bengaluru emergency incidents, responding units, status badges, and true full-screen mode.
- **Plan Diff**: Semantic delta visualizer highlighting additions, cancellations, and preemption moves between plan revisions.
- **Crisis Scoreboard**: Executive HUD displaying Containment Score, Average Response Time, Active/Critical Incidents, and Fleet Availability.
- **Advanced Agent Chat**: Interactive multi-agent dialogue feed allowing coordinators to query situational awareness, filter by agent, and review reasoning chains.
- **System Health**: Real-time telemetry monitoring API Gateway latency, Planning Engine state, and Explainability model status.
- **Audit Trail**: Cryptographic SHA-256 state checksum ledger maintaining complete causality and tamper-evident compliance.
- **Presentation / Judge Mode**: Curated storytelling interface designed for executive evaluation and high-level crisis walkthroughs.
- **Demo Story Controller**: Step-by-step scenario navigation (`Reset`, `Next`, `Run All`, `+5 min`) with automated event sequencing.

---

## 3. System Architecture

```mermaid
flowchart TD
    subgraph Frontend["Frontend (React 19 + Vite + Tailwind CSS)"]
        UI[Command Center Dashboard]
        Map[Geospatial Map - Leaflet]
        Graph[Resource Dependency Graph - SVG]
        Replay[Decision Replay & Counterfactual Lab]
        Controls[Simulation & Story Controller]
    end

    subgraph API["API Gateway (FastAPI)"]
        StateAPI[/api/state]
        ScenarioAPI[/api/scenario/next, /reset]
        ApprovalAPI[/api/approval/{id}/approve]
        WhatIfAPI[/api/whatif]
        ChaosAPI[/api/chaos/step]
        ChatAPI[/api/chat]
    end

    subgraph Core["Crisis Engine & Multi-Agent Orchestrator"]
        Orchestrator[Multi-Agent Orchestrator]
        StateEngine[In-Memory CrisisState Engine]
        AssessmentAgent[Assessment Subagent]
        AllocationAgent[Deterministic Allocation Engine]
        LogisticsAgent[Logistics & Haversine Router]
        ImpactAgent[Impact & Bottleneck Predictor]
        ExplainerAgent[Explainability & Audit Engine]
        ApprovalGate[Human Approval Gatekeeper]
    end

    subgraph External["External / Fallbacks"]
        LLM[xAI Grok / Fallback Deterministic Engine]
    end

    UI --> API
    Map --> API
    Graph --> API
    Replay --> API
    Controls --> API

    API --> Core
    Orchestrator --> StateEngine
    Orchestrator --> AssessmentAgent
    Orchestrator --> AllocationAgent
    Orchestrator --> LogisticsAgent
    Orchestrator --> ImpactAgent
    Orchestrator --> ExplainerAgent
    Orchestrator --> ApprovalGate
    AssessmentAgent -.-> LLM
    ExplainerAgent -.-> LLM
```

---

## 4. Technology Stack

- **Backend**:
  - Python 3.11+ / FastAPI
  - Pydantic v2 data contracts and validation
  - NetworkX / SciPy for graph and spatial calculations
  - Pytest with 120+ unit and integration tests
  - Optional xAI Grok LLM integration with automatic deterministic fallbacks
- **Frontend**:
  - React 19 / Vite
  - Tailwind CSS 4
  - React-Leaflet / Leaflet GIS
  - SVG interactive vector topology
  - Oxlint linter
- **Audit & Cryptography**:
  - SHA-256 rolling digest calculation for immutable audit logging

---

## 5. Project Structure

```text
Crisis_Command/
├── backend/
│   ├── data/             # Initial simulation states, city coordinates & scenario definitions
│   ├── models/           # Pydantic data schemas (CrisisState, Incident, Resource, Plan)
│   ├── orchestrator/     # Multi-agent coordination and event trigger pipelines
│   ├── services/         # Allocation, logistics, impact prediction, and state engine
│   ├── subagents/        # Assessment, allocation, explainability subagents
│   ├── tests/            # Test suite (unit, integration, constraints, approval gate)
│   ├── config.py         # App configuration and environment variable loading
│   └── main.py           # FastAPI application routes and REST endpoints
├── frontend/
│   ├── src/
│   │   ├── components/   # UI panels (MapView, DependencyGraph, DecisionReplay, etc.)
│   │   ├── pages/        # CommandCenter, HomePage, InteractiveDemo, DocsPage, AboutPage
│   │   ├── router/       # Lightweight hash/path client router
│   │   ├── App.jsx       # Root layout and view switcher
│   │   ├── index.css     # Tailwind styling, custom scrollbars, and keyframes
│   │   └── main.jsx      # React entrypoint
│   ├── package.json      # Frontend dependencies and scripts
│   └── vite.config.js    # Vite bundler configuration
├── contracts/            # Mock state schemas and JSON contracts
├── requirements.txt      # Python dependencies
├── .env.example          # Environment variable template
└── README.md             # Platform documentation
```

---

## 6. How It Works

1. **Incident Intake & Triage**: Free-text emergency calls or sensor triggers are ingested and parsed into structured records with location, casualty count, and required resource categories.
2. **Deterministic Plan Optimization**: The allocation engine scores possible pairings based on distance, resource capabilities, and incident severity.
3. **Preemption & Conflict Resolution**: If an incoming critical incident requires an already assigned resource, the system assesses trade-offs. If preemption exceeds the impact threshold, it halts automated dispatch.
4. **Human Approval Gate**: Critical reassignments are routed to human dispatchers with an interactive diff showing exact additions and cancellations.
5. **Simulated Transit & On-Scene Lifecycle**: Dispatched assets transition from `AVAILABLE` to `EN ROUTE`, arrive `ON SCENE`, and return to `AVAILABLE` upon incident resolution.
6. **Continuous Auditing**: Every event generates an immutable log entry with a cryptographically chained SHA-256 checksum.

---

## 7. Installation

### Prerequisites
- Python 3.11+
- Node.js 18+ and npm

### Clone the Repository
```bash
git clone https://github.com/tuhin-hash/Crisis_command.git
cd Crisis_command
```

---

## 8. Running the Backend

1. Create and activate a virtual environment:
   ```bash
   python -m venv .venv
   # Windows (PowerShell):
   .venv\Scripts\Activate.ps1
   # macOS/Linux:
   source .venv/bin/activate
   ```
2. Install Python dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Configure environment variables (optional):
   ```bash
   cp .env.example .env
   ```
   *Note: An LLM API key is NOT required. Crisis Command includes deterministic heuristic fallbacks.*
4. Start the FastAPI server:
   ```bash
   python -m uvicorn backend.main:app --reload --port 8000
   ```
   The backend API will be available at `http://localhost:8000`. Swagger documentation is accessible at `http://localhost:8000/docs`.

---

## 9. Running the Frontend

1. Navigate to the `frontend` directory:
   ```bash
   cd frontend
   ```
2. Install Node dependencies:
   ```bash
   npm install
   ```
3. Start the Vite development server:
   ```bash
   npm run dev
   ```
4. Open `http://localhost:5173` in your browser.

---

## 10. Demo Scenario

The application includes a pre-configured 7-step scenario set across Bengaluru:
1. **t=0m (Baseline)**: Minor medical incident (I1) reported; ambulance A1 assigned.
2. **t=5m (Fire Outbreak)**: Commercial fire (I2) erupts; fire engines F1 and F2 dispatched.
3. **t=10m (Escalation)**: Mass casualty building collapse (I3) reported; fleet capacity nears saturation.
4. **t=15m (Preemption Conflict)**: Critical incident requires reallocating an en-route unit. **Human Approval Gate** activates.
5. **t=20m (Resource Failure)**: Unit breakdown simulated; allocation engine re-routes closest backup unit.
6. **t=25m (Resolution & Containment)**: Incidents stabilized; fleet resets to available states.

Use the **Simulation Controls** at the top of the Command Center to step forward, advance time, or run all steps automatically.

---

## 11. Testing

### Run Backend Tests
```bash
pytest backend/tests -v
```
All 120+ tests cover allocation correctness, resource lifecycle transitions, approval constraints, and deterministic fallbacks.

### Run Frontend Build & Linter
```bash
cd frontend
npm run build
npx oxlint
```

---

## 12. Screenshots / Demo

- **Command Center Dashboard**: Real-time 3-column layout integrating GIS mapping, plan diffs, incident feeds, and agent chats.
- **Geospatial Map & Topology Fullscreen**: Click the `Fullscreen` button or press `Esc` to toggle full-viewport command views.
- **Counterfactual Lab**: Compare active vs. hypothetical dispatch configurations side by side.
- **Executive Judge Mode**: Activate via the header toggle for a high-level storytelling summary.

---

## 13. Team / Contributors

**Team Neural Ninjas** | **Gateways 2026 Hackathon** | **Domain 4: Crisis Command**

- **Namratha Nataraj** (Team Lead, Product/Strategy) — LLM Integration & Incident Assessment
- **Vishal M** (AI and Multi-Agent Systems) — Logistics, Scenario Controller & Frontend
- **Tuhin Paul** (Backend and Simulation) — Orchestrator, Allocation Engine & APIs
- **Shreesha Joshi** (Frontend and UX) — Allocation Views, What-If Simulator & Impact Detection

---

## 14. Safety Notice

```text
================================================================================
DEMONSTRATION / SIMULATION ONLY.
NOT FOR REAL-WORLD EMERGENCY DISPATCH.
================================================================================
```
This application is strictly an educational and hackathon simulation. It is not integrated with public safety answering points (PSAP), 911/112 CAD systems, or live emergency services.
