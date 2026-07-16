# Serinity — System Architecture & Workflow

Serinity is a **local-first, multi-agent conversational AI** for clinical mental-health interviews, built as a FastAPI backend with a hexagonal (ports & adapters) core, a vanilla-JS frontend, and fully offline model inference via Ollama.

---

## 1. System Architecture

The codebase follows a **ports & adapters** style: `core/ports.py` defines abstract interfaces (`LLMProvider`, `VectorStore`, `SessionStore`, `ProfileStore`, `STTProvider`, `RiskSignal`), `services/` contains the framework-free business logic, and `providers/` + `persistence/` supply concrete local implementations. `api/dependencies.py` wires everything together as singletons and injects them into the FastAPI routes.

```mermaid
graph TB
    subgraph CLIENT["🖥️ Client — Browser"]
        UI["index.html<br/>SPA Templates"]
        JS["static/js<br/>app.js · session.js · ui.js<br/>audio.js · profiles.js · state.js"]
    end

    subgraph API["⚙️ FastAPI Backend"]
        MAIN["main.py<br/>App bootstrap · lifespan sweeper"]
        ROUTES["api/routes.py<br/>/start /chat_text /transcribe<br/>/patients* /end_session"]
        DEPS["api/dependencies.py<br/>Dependency Injection / Singletons"]
    end

    subgraph CORE["🧩 Core Domain (framework-free)"]
        PORTS["core/ports.py<br/>Protocols: LLMProvider, VectorStore,<br/>SessionStore, ProfileStore, RiskSignal"]
        SCHEMAS["core/schemas.py<br/>Request/Response DTOs"]
        PROMPTS["core/prompts.py<br/>LLM1 / LLM2 / LLM3 system prompts"]
        LOGGER["core/logger.py<br/>Structured session logging"]
    end

    subgraph SERVICES["🧠 Domain Services"]
        ORCH["ConversationOrchestrator<br/>Sync 3-pipeline turn handler"]
        PSVC["PatientService<br/>Session lifecycle · summaries"]
        RISK["RiskAssessmentService<br/>Combines RiskSignal adapters"]
    end

    subgraph PROVIDERS["🔌 Provider Adapters"]
        OLLAMA["OllamaLLMProvider<br/>LLM1 interviewer · LLM2 analyst · LLM3 profile"]
        CHROMA["ChromaVectorStore<br/>Hybrid RAG: embeddings + BM25 ensemble"]
        STT["SenseVoiceSTTProvider<br/>Speech-to-text + emotion tag parsing"]
        subgraph RISKSIG["Risk Signals"]
            CASCADE["CascadeRiskSignal<br/>filter → verify"]
            KEYWORD["KeywordRiskSignal<br/>regex fast filter"]
            BERT["BertRiskSignal<br/>HF suicide-detection model"]
            LLMRISK["LLMRiskSignal<br/>LLM1.risk_flag"]
            CLINICAL["ClinicalRiskSignal<br/>LLM2.risk_assessment"]
        end
    end

    subgraph STORAGE["💾 Local Storage"]
        SQLITE["SQLite<br/>SQLiteProfileStore · SQLiteSessionStore<br/>patients · sessions · messages · profiles"]
        VECDB["ChromaDB<br/>Persisted vector store<br/>+ BM25 cache (Sims' Symptoms in the Mind)"]
        LOGS["logs/<br/>Per-session JSONL transcripts"]
    end

    subgraph MODELS["🤖 Local Model Runtime"]
        OLLAMASVC["Ollama Server<br/>qwen2.5 / phi4-mini + nomic-embed-text"]
        HFMODEL["Local HF Cache (./models)<br/>BERT risk-detection weights"]
    end

    UI --> JS
    JS -->|"fetch: /start /chat_text /transcribe"| ROUTES
    MAIN --> ROUTES
    MAIN --> DEPS
    ROUTES --> DEPS
    DEPS -->|constructs & injects| ORCH
    DEPS --> PSVC
    DEPS --> RISK
    DEPS --> OLLAMA
    DEPS --> CHROMA
    DEPS --> STT
    DEPS --> SQLITE

    ROUTES -.implements.-> PORTS
    ORCH --> PORTS
    PSVC --> PORTS
    RISK --> PORTS
    ORCH --> SCHEMAS
    ORCH --> LOGGER
    OLLAMA --> PROMPTS

    ORCH --> OLLAMA
    ORCH --> CHROMA
    ORCH --> RISK
    ORCH --> SQLITE
    PSVC --> OLLAMA
    PSVC --> SQLITE
    RISK --> CASCADE
    RISK --> LLMRISK
    RISK --> CLINICAL
    CASCADE --> KEYWORD
    CASCADE --> BERT
    ROUTES --> STT

    OLLAMA --> OLLAMASVC
    CHROMA --> OLLAMASVC
    CHROMA --> VECDB
    BERT --> HFMODEL
    SQLITE --> SQLITE
    ORCH --> LOGS
    PSVC --> LOGS

    classDef client fill:#1e3a5f,stroke:#4a90d9,color:#fff
    classDef api fill:#2d4a2b,stroke:#5cb85c,color:#fff
    classDef core fill:#4a2d5f,stroke:#9b59b6,color:#fff
    classDef services fill:#5f3d1e,stroke:#e67e22,color:#fff
    classDef providers fill:#5f1e2d,stroke:#e74c3c,color:#fff
    classDef storage fill:#1e5f4a,stroke:#1abc9c,color:#fff
    classDef models fill:#3d3d3d,stroke:#bdbdbd,color:#fff

    class UI,JS client
    class MAIN,ROUTES,DEPS api
    class PORTS,SCHEMAS,PROMPTS,LOGGER core
    class ORCH,PSVC,RISK services
    class OLLAMA,CHROMA,STT,CASCADE,KEYWORD,BERT,LLMRISK,CLINICAL providers
    class SQLITE,VECDB,LOGS storage
    class OLLAMASVC,HFMODEL models
```

**Key design points**

- **Hexagonal core**: `services/` never imports FastAPI or a concrete database driver — only the `Protocol` interfaces in `core/ports.py`. Any adapter (e.g. swap Ollama for another LLM host) can be replaced without touching business logic.
- **Singletons via DI**: `api/dependencies.py` builds every provider and service once at import time and exposes `get_*()` functions that FastAPI routes consume with `Depends(...)`.
- **100% local**: LLM inference (Ollama), embeddings (`nomic-embed-text`), the vector store (ChromaDB), the risk-detection BERT model, and all patient data (SQLite) run on-device — no data leaves the machine.

---

## 2. Chat Turn Workflow

Every user message flows through `ConversationOrchestrator.handle_message`, which runs a **synchronous 3-pipeline** logic: a fast conversational agent (LLM1), an optional query/RAG path, and an optional deep pattern-analysis agent (LLM2) — gated by intent and risk signals.

```mermaid
sequenceDiagram
    autonumber
    actor U as Patient
    participant FE as Frontend (JS)
    participant RT as /chat_text Route
    participant SS as SessionStore
    participant ORCH as ConversationOrchestrator
    participant LLM1 as Ollama · LLM1<br/>(Clinical Interviewer)
    participant RISK as RiskAssessmentService
    participant VDB as ChromaVectorStore (RAG)
    participant LLM2 as Ollama · LLM2<br/>(Pattern Analyst)
    participant PS as ProfileStore (SQLite)

    U->>FE: Type / speak message
    FE->>RT: POST /chat_text {message, session_id, emotion}
    RT->>SS: session_exists()?
    alt session expired
        RT->>ORCH: create_new_session()
    end
    RT->>ORCH: handle_message(session_id, message, emotion)

    ORCH->>SS: append_message("user", message)
    ORCH->>SS: get_working_context()
    ORCH->>LLM1: psychiatrist_response(history)
    LLM1-->>ORCH: assistant_message, intent, risk_flag, search_query

    ORCH->>RISK: assess(message, llm1_output)
    RISK->>RISK: Cascade → Keyword filter → BERT verify
    RISK->>RISK: LLMRiskSignal (risk_flag)
    RISK-->>ORCH: risk_flagged (bool)
    Note over ORCH: if risk_flagged → force intent = ANALYZE

    alt intent == QUERY
        ORCH->>VDB: retrieve(search_query, k=3)
        VDB-->>ORCH: clinical reference context
        ORCH->>LLM1: psychiatrist_query_response(history, context)
        LLM1-->>ORCH: final grounded answer
    else intent == ANALYZE
        ORCH->>VDB: retrieve(clinical_summary or recent turns)
        VDB-->>ORCH: reference context
        ORCH->>PS: build_profile_recap(patient_id)
        PS-->>ORCH: existing clinical profile
        ORCH->>LLM2: internal_reasoning(history + context + recap)
        LLM2-->>ORCH: themes, patterns, stressors, risk_assessment
        ORCH->>PS: update_long_term_memory(llm2_output)
        Note over ORCH: LLM2's own reply becomes final_message
    else intent == CONTINUE
        Note over ORCH: LLM1's reply is used as-is
    end

    ORCH->>SS: append_message("assistant", final_message)
    ORCH-->>RT: ChatResult(message, intent, risk_flagged)
    RT-->>FE: JSON response
    FE-->>U: Render reply (+ crisis resources if risk_flagged)
```

### Session lifecycle (start → end → background summary)

```mermaid
flowchart LR
    A["POST /start"] --> B["PatientService.create_new_session"]
    B --> C["ProfileStore.build_profile_recap"]
    C --> D["LLM1.generate_opening_context + psychiatrist_response"]
    D --> E["Opening message returned to patient"]

    E -.-> F["... chat turns via /chat_text ..."]

    F --> G["POST /end_session"]
    G --> H["SessionStore.end_session (sync)"]
    G --> I["BackgroundTasks: generate_session_summary"]
    I --> J["LLM3: generate_end_of_session_profile<br/>(old profile + session transcript)"]
    J --> K["ProfileStore.update_patient_profile<br/>+ update_long_term_memory"]
    J --> L["SessionStore.save_session_summary"]

    M["Lifespan Sweeper<br/>every 5 min"] --> N["sweep_abandoned_sessions<br/>(idle > 30 min)"]
    N --> G

    style A fill:#2d4a2b,stroke:#5cb85c,color:#fff
    style G fill:#5f1e2d,stroke:#e74c3c,color:#fff
    style J fill:#4a2d5f,stroke:#9b59b6,color:#fff
    style M fill:#3d3d3d,stroke:#bdbdbd,color:#fff
```

---

## 3. Component Reference

| Layer | File(s) | Responsibility |
|---|---|---|
| Entry point | `main.py` | FastAPI app, CORS, static mount, lifespan-managed session sweeper |
| API | `api/routes.py`, `api/dependencies.py` | HTTP endpoints + singleton wiring/DI |
| Core contracts | `core/ports.py`, `core/schemas.py`, `core/prompts.py`, `core/logger.py` | Protocols, DTOs, LLM prompt templates, structured logging |
| Orchestration | `services/conversation_orchestrator.py` | Per-turn pipeline: LLM1 → risk → QUERY/ANALYZE branch |
| Session lifecycle | `services/patient_service.py` | Start/end sessions, background LLM3 summary generation, idle sweeping |
| Risk | `services/risk_assessment_service.py`, `providers/*_risk_signal.py` | Multi-signal safety net (keyword → BERT cascade, LLM self-report, LLM2 clinical judgment) |
| LLM | `providers/ollama_llm_provider.py` | Local Ollama calls for LLM1 (interviewer), LLM2 (analyst), LLM3 (profile writer) |
| RAG | `providers/chroma_vector_store.py`, `scripts/build_vector_db.py` | Hybrid (embedding + BM25) retrieval over psychiatric reference text |
| Speech | `providers/sensevoice_stt_provider.py` | Audio → text + emotion/event tag parsing |
| Persistence | `persistence/sqlite_memory_store.py` | SQLite-backed `ProfileStore` and `SessionStore` |
| Frontend | `templates/index.html`, `static/js/*`, `static/css/style.css` | SPA: chat UI, mic recording, patient profile dashboard |

> Diagrams render automatically on GitHub/GitLab (Mermaid support built in) or in any Markdown viewer with Mermaid enabled, e.g. the [Mermaid Live Editor](https://mermaid.live).
