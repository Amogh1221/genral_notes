# Invention Disclosure Form — Content for AutoData Labs

> Drafted to mirror the structure/format of your previous Serinity Invention Disclosure Form, so you can drop this straight into the same template. Fields marked `[confirm]` are ones only you can fill in accurately (contact details, position, signatures) — everything else is drafted from your architecture diagram and the AutoData-Labs repo (README + architecture.md).
>
> Per your note, the LLM layer is described generically as **"LLMs (local or cloud, interchangeable)"** throughout — no specific vendor/model is named as part of the invention, since the point of novelty is the abstraction itself, not any one provider.

---

## 1.) Personal and contact details

| Field | Value |
|---|---|
| Title | `[confirm]` |
| First Name | Amogh |
| Last name | Gupta |
| Institution/organization (If Organisation is Applicant) | Manipal University Jaipur `[confirm — still current]` |
| Department | Computer Science & Engineering `[confirm — still current]` |
| Position held | `[confirm]` |
| Tel no. (Ext No) | `[confirm]` |
| Cell no. | `[confirm]` |
| Email | `[confirm]` |

---

## 2.) How would you classify the invention?

**SOFTWARE SYSTEM:** An autonomous multi-agent AI system that converts a natural-language research topic into a structured, verified, exportable dataset — combining LLM-driven adaptive schema generation, automated web source discovery, streaming extraction, and an independent verification/gap-filling stage, all coordinated through a single LLM-agnostic inference interface that can be pointed at a local (on-device) or cloud-hosted LLM without changing agent logic.

---

## 3.) Title for the invention

**AUTODATA LABS — A Multi-Agent System for Topic-Driven, Verified Dataset Generation from Web Sources with an LLM-Agnostic Orchestration Layer**

*(Alternative shorter title: "Autonomous Multi-Agent Pipeline for Adaptive-Schema, Verified Web Dataset Extraction")*

---

## 4.) Please describe the problem being solved by this invention

**PROBLEM STATEMENT:** Building a structured, tabular dataset on an arbitrary topic (e.g. "AI startups in India," "Michelin-starred restaurants in Paris") currently requires a person to manually decide what columns matter, search the web, open each source, read and transcribe the relevant values, and check them for accuracy — a slow, repetitive process that does not scale with the number of topics or sources. Existing automated approaches have specific shortcomings:

a) **NO ADAPTIVE SCHEMA GENERATION:** Traditional web scrapers and no-code scraping tools require a person to hand-define the columns/selectors for each topic or site in advance; they cannot infer a sensible schema directly from a plain-language topic description.

b) **NO INDEPENDENT VERIFICATION OF EXTRACTED DATA:** Approaches that simply ask a single LLM to "produce a dataset" on a topic have no grounding in live sources and no separate check on the values returned, making them prone to fabricated (hallucinated) rows and fields with no way to detect or correct them.

c) **BATCH-ONLY, OPAQUE EXECUTION:** Conventional scraping pipelines run as an all-or-nothing batch job — the user sees nothing until the run finishes (or fails), cannot monitor progress, and cannot recover partial results if something goes wrong mid-run.

d) **VENDOR LOCK-IN TO A SINGLE LLM:** Systems that embed LLM calls directly into their business logic are tied to one provider's API, pricing, and availability; there is no clean way to run the same pipeline offline on a local model versus a hosted cloud model.

**OTHER POTENTIAL SOLUTIONS AND THE DISADVANTAGES THEY HAVE:**

- **Hand-written scrapers (BeautifulSoup/Scrapy scripts):** effective per-site but brittle — break on layout changes, require a new script per topic/source, no semantic understanding of the data.
- **No-code/point-and-click scraping SaaS tools:** still require manual field/selector definition per source; not driven by a topic prompt; typically batch-only output.
- **Asking a chatbot (GPT/Claude) directly for a table:** cheap to try but ungrounded — no live web verification, no per-field confirmation, no incremental/streaming output, no schema consistency across rows.
- **Generic RAG research assistants:** good at retrieving and summarizing/answering a question, but not structured around producing a consistent, exportable, tabular dataset with a dedicated verification pass.
- **Commercial data-enrichment/BI platforms:** proprietary, subscription-priced, and tied to a fixed set of data categories/providers rather than open-ended, user-defined topics.

There is currently no known publicly available solution combining topic-driven adaptive schema generation, autonomous multi-source discovery, streaming extraction, and an independent LLM-backed verification stage, all decoupled from any single LLM vendor.

---

## 5.) Provide a brief description of the invention and how it works

**PURPOSE:**

AutoData Labs turns a single natural-language topic into a live, growing, structured dataset. A user types a topic, reviews and approves an auto-generated column schema, and then watches a pipeline of coordinated AI agents discover sources, extract rows, and verify/complete them in real time — with the underlying LLM swappable between a local (on-device) model and a cloud-hosted model purely by configuration.

**MECHANICS OF THE INVENTION — pipeline stages** *(see attached Architecture Diagram)*:

**STAGE 1 — SCHEMA GENERATION (Planner Agent):**
- The user enters a topic on the **Home Page**.
- The Planner Agent runs live web search queries (via the Providers layer) to gather real-world context about the topic, then sends the topic plus that context to an LLM (through the Unified LLM Interface) with a structured prompt.
- The LLM returns a candidate schema: a list of columns, each with a name, data type, description, and required/optional flag.
- The user can add, remove, rename, or reorder columns in the Home Page's schema builder; any user-added column can be validated against the topic by a follow-up LLM call before the run starts.

**STAGE 2 — SOURCE DISCOVERY (source Agent):**
- Once the schema is approved, the source Agent (the "Librarian") generates targeted search queries from the topic and schema (e.g. adding site-specific or format-specific qualifiers).
- It queries the search Providers, deduplicates results, and performs a lightweight fetch of each candidate URL to read its title/description without downloading the full page.
- Each URL is classified by type (HTML page, CSV file, JSON API) and saved to the **Sources Database** with a `PENDING` status.
- Discovery is iterative: a "find more sources" request excludes URLs already seen, so repeated discovery passes do not duplicate work.

**STAGE 3 — CRAWL & EXTRACTION (Research Agent):**
- For each `PENDING` source, the Research Agent crawls the page — using a headless-browser-based provider for JavaScript-rendered pages, with a static HTML parser as a lightweight fallback.
- The page content is split into overlapping chunks; each chunk is sent, together with the approved schema, to an LLM (via the Unified LLM Interface) which returns candidate rows as structured data.
- Rows missing their primary key, or already seen for this run, are filtered out.
- Each valid row is written to the Sources Database **immediately** — not batched — so it is available to the frontend the moment it exists.

**STAGE 4 — VERIFICATION (Verification Agent):**
- The Verification Agent audits newly extracted rows for missing or unconfirmed fields.
- For each gap, it issues a narrow, field-specific follow-up search (e.g. a single missing attribute for a single entity, rather than re-crawling the whole source) and feeds the retrieved snippet back to the LLM to extract or confirm just that field.
- Verified/completed rows are written back to the Sources Database.
- This stage runs concurrently, per entity, so it does not block the throughput of the extraction stage.

**STAGE 5 — PIPELINE ORCHESTRATION:**
- A central Pipeline Orchestration component sequences and coordinates the four agents, tracks run state (running / paused / waiting-for-key / cancelled), and exposes lifecycle controls (start, stop, pause, resume) to the frontend.
- If the configured LLM backend becomes unavailable mid-run (e.g. a cloud provider rejects further requests), Pipeline Orchestration pauses the run — rather than aborting it — retains every row already extracted, and prompts the user through the frontend to supply an alternate credential or backend before resuming.
- Cancellation is cooperative and checked at every I/O boundary (between sources, immediately after a crawl returns, between chunks, during the verification wait), so stop/pause take effect within about a second rather than only at the end of a batch.

**STAGE 6 — UNIFIED LLM INTERFACE:**
- All four agents make their LLM calls through one shared interface rather than calling any specific vendor's SDK directly.
- Which LLM actually answers a given call — a model running locally on the operator's own hardware, or a model hosted by a cloud inference provider — is selected purely by configuration.
- Because agent logic and prompts are written against this single interface, the backend LLM can be changed, upgraded, or substituted (including mid-deployment) without modifying any agent's business logic.

**STAGE 7 — REAL-TIME STREAMING & ANALYTICS (Frontend Dashboard):**
- As each row is saved, an event is pushed to the Dashboard over a Server-Sent Events (SSE) stream, and the underlying Sources Database is also polled on a short interval as a fallback data path.
- The Dashboard shows the live extraction table, an agent status/log trace, and analytics charts that update as the dataset grows — plus the same start/stop/pause/resume controls surfaced by Pipeline Orchestration.

**STAGE 8 — EXPORT:**
- The user can export the dataset at any point — mid-run or after completion — so a run interrupted by a source failure or an exhausted LLM credential still yields a usable partial dataset.

**Github:** https://github.com/Amogh1221/AutoData-Labs

**Architecture Diagram:** *(attach the uploaded diagram — Providers / LLM Provider / Backend agents / Frontend, as reviewed)*

---

## 6.) Please provide a short description of the various components of the invention

**COMPONENT 1 — Frontend (Home Page + Dashboard):**
- Home Page: topic input and interactive schema builder.
- Dashboard: live extraction table, real-time analytics charts, agent log trace, and run controls (start/stop/pause/resume), fed by the SSE stream and a short-interval data poll.

**COMPONENT 2 — Pipeline Orchestration:**
- Coordinates execution of the four agents, maintains run state, exposes lifecycle controls, and handles mid-run recovery (e.g. pausing on LLM credential exhaustion and resuming once a new one is supplied) without discarding already-extracted data.

**COMPONENT 3 — Planner Agent:**
- Generates the candidate column schema for a topic using live web-search context plus an LLM call; supports interactive validation of user-added columns.

**COMPONENT 4 — Source Agent:**
- Discovers candidate URLs via targeted search queries, performs lightweight metadata fetches, classifies each source by type (HTML/CSV/JSON), and persists them to the Sources Database with iterative, duplicate-free discovery.

**COMPONENT 5 — Research Agent:**
- Crawls each pending source (headless-browser primary path, static-HTML fallback), chunks content, extracts structured rows per chunk via an LLM call, deduplicates, and streams each valid row to storage immediately.

**COMPONENT 6 — Verification Agent:**
- Audits extracted rows for missing/unconfirmed fields, issues targeted field-level follow-up searches, confirms or fills values via an LLM call, and updates records — running concurrently, per entity.

**COMPONENT 7 — Unified LLM Interface:**
- A single call-through abstraction used by all agents; routes to a locally hosted or cloud-hosted LLM based on configuration alone, so the choice of underlying model/vendor is fully decoupled from agent logic.

**COMPONENT 8 — Provider Layer (search & crawl providers):**
- Pluggable search-provider and crawl-provider interfaces (headless-browser crawler + static-HTML fallback crawler) sitting behind a common abstraction, so a new search engine or crawling strategy can be added without touching agent code.

**COMPONENT 9 — Sources Database:**
- Persistent store for discovered sources, extracted/verified entities, and run logs; the single source of truth read by both the SSE stream and the polling data endpoint.

**COMPONENT 10 — SSE Stream:**
- Push channel that delivers pipeline and agent-status events (including error and recovery events such as LLM credential exhaustion) to the frontend the instant they occur.

---

## 7.) What is novel about the invention?

**a) TOPIC-DRIVEN ADAPTIVE SCHEMA GENERATION WITH IN-LINE USER APPROVAL:**
Rather than requiring a hand-authored extraction template per topic or site, the invention derives a candidate column schema for an arbitrary natural-language topic from live web context via an LLM, then exposes that schema for interactive user edit/approval before any extraction begins — collapsing the "define what to scrape" step into the pipeline itself.

**b) FOUR-STAGE AGENT PIPELINE WITH A DEDICATED, INDEPENDENT VERIFICATION STAGE:**
The invention separates schema planning, source discovery, extraction, and verification into distinct agents rather than asking one LLM call to search, extract, and validate simultaneously. The Verification Agent's targeted, field-level re-search-and-confirm pass reduces missing or fabricated values relative to a single-pass extraction.

**c) ROW-LEVEL REAL-TIME STREAMING, NOT BATCH-AT-END DELIVERY:**
Each row is persisted and pushed to the user interface the moment it is produced — via an SSE push channel backed by a short-interval poll — rather than only after the entire run completes, giving continuous visibility and enabling a usable partial export at any point mid-run.

**d) LLM-AGNOSTIC UNIFIED INFERENCE INTERFACE:**
All four agents call a single shared interface rather than a specific vendor's API. The underlying LLM — local/on-device or cloud-hosted — is selected purely by configuration, so the same schema-generation, discovery, extraction, and verification logic runs unmodified against any compatible LLM backend, and the backend can be swapped (including mid-run, on failure) without touching agent code.

**e) CONCURRENT, PER-ENTITY VERIFICATION VIA NARROW, TARGETED RE-SEARCH:**
Instead of re-crawling an entire source to fix one missing field, the Verification Agent issues a narrow, field-specific search per gap and runs these checks concurrently across entities, so verification does not become a bottleneck on overall extraction throughput.

**f) GRACEFUL, DATA-PRESERVING MID-RUN RECOVERY:**
When the configured LLM backend becomes unavailable mid-run, Pipeline Orchestration pauses — rather than aborts — the run, retains all data already extracted, and lets the user supply an alternate credential or backend to resume, with partial data always exportable.

**g) COOPERATIVE, LOW-LATENCY CANCELLATION ACROSS AGENT BOUNDARIES:**
Stop/pause requests are honored by checking run state at every I/O boundary (between sources, immediately after a crawl returns, between extraction chunks, during the verification wait) rather than only between batches, so control commands take effect within roughly a second regardless of which agent is currently running.

---

## 8.) Are you aware of any existing methods, apparatus, or developments relating to this invention?

**EXISTING TECHNOLOGY:**

a) **Hand-written web scrapers (BeautifulSoup/Scrapy/Selenium scripts):** Effective for a known, fixed site structure. Drawbacks: require a new script/selector set per topic or site, break on layout changes, no semantic understanding of the data being extracted, no adaptive schema.

b) **No-code/point-and-click scraping and data-extraction SaaS tools:** Lower the coding barrier. Drawbacks: still require manual field/selector definition per source, are not driven by a plain-language topic, and typically produce batch-only output with no live verification step.

c) **Direct LLM chatbot queries for a dataset (e.g. asking GPT/Claude to "give me a table of X"):** Fast to try. Drawbacks: no grounding in live, verifiable sources; prone to hallucinated rows/fields; no schema consistency guarantees across rows; no incremental/streaming output.

d) **Generic RAG-based research assistants:** Good at retrieving and summarizing information to answer a question. Drawbacks: not structured around producing a consistent, exportable, tabular dataset, and have no dedicated post-extraction verification/gap-filling stage.

e) **Commercial data-enrichment / business-intelligence platforms:** Provide curated datasets or enrichment for known categories (companies, contacts, etc.). Drawbacks: proprietary and subscription-priced, tied to a fixed set of data categories/providers, not designed for open-ended, user-defined topics.

**WHY THIS INVENTION OVERCOMES THE DISADVANTAGES OF THE ABOVE APPROACHES:**

- Generates its own schema from a topic instead of requiring one to be hand-authored per source.
- Grounds every row in a discovered, classified web source rather than relying on an LLM's unverified output.
- Adds an independent verification/gap-filling stage that none of the above approaches provide.
- Streams results row-by-row instead of only delivering output in one batch at the end.
- Is not tied to a single LLM vendor — the same pipeline runs against a local or a cloud model via configuration alone.

---

## 9.) What is the technical impact of the invention?

a) **TIME SAVINGS:** Converts a multi-hour manual research-and-curation task (search, open sources, transcribe, cross-check) into an automated pipeline started from a single topic prompt.

b) **HIGHER DATA RELIABILITY:** The dedicated Verification Agent's targeted re-search-and-confirm pass reduces missing or incorrect fields compared with a single-pass, ungrounded extraction.

c) **CONTINUOUS VISIBILITY AND CONTROL:** Row-level streaming plus start/stop/pause/resume controls let the user monitor and course-correct a run in progress, instead of waiting blindly for a batch job to finish.

d) **DEPLOYMENT FLEXIBILITY AND COST CONTROL:** Because the LLM layer is a swappable configuration choice, the same pipeline can run entirely offline on local hardware (privacy, no per-call cost) or against a hosted cloud model (higher throughput/quality) — or be reconfigured between the two — without changing any agent code.

e) **RESILIENCE TO MID-RUN FAILURE:** An LLM backend becoming unavailable mid-run degrades the run to a paused, recoverable state rather than discarding progress; the user is prompted to supply an alternate credential/backend, and any data already extracted remains exportable.

f) **EXTENSIBILITY:** Pluggable search-provider, crawl-provider, and LLM-interface abstractions mean new data sources, crawling strategies, or LLM backends can be added without modifying the agents' core business logic.

---

## 10.) What are the invention's limitations or disadvantages?

a) **EXTRACTION LATENCY:** Each source requires a crawl plus one or more LLM calls per content chunk, so a large source set takes longer to process than a raw, LLM-free scraper.
*Mitigable:* parallelizing across sources, using a smaller/faster model for the extraction pass specifically.

b) **LLM-DEPENDENT OUTPUT QUALITY:** Schema and extraction quality depend on the capability of whichever LLM is currently configured; a smaller local model may produce lower-quality schemas or extractions than a larger cloud-hosted model.
*Mitigable:* the Unified LLM Interface lets an operator select model quality per deployment, or fall back to a stronger backend when needed.

c) **WEB-SOURCE DEPENDENCY:** The system relies on public sources being reachable and crawlable; sites with strong anti-bot protections may block the available crawl providers.
*Mitigable:* the pluggable crawl-provider interface allows new crawling strategies to be added without touching agent logic.

d) **SINGLE-NODE PERSISTENCE:** The current storage layer is a local, single-writer database, which is not inherently built for multi-instance horizontal scaling.
*Mitigable:* swap in a networked, multi-writer store behind the same persistence interface with no change to business logic.

e) **NO GUARANTEE OF FACTUAL CORRECTNESS:** Verification reduces, but cannot fully eliminate, the risk of extracting an incorrect value from a source that itself contains inaccurate information; the system is not a fact-checking authority.

---

## 11.) What is the present stage of technical development?

**STATUS:** Working end-to-end prototype (backend + frontend) implemented and under active development.

- Validated idea
- Concept proven (four-agent pipeline — plan → discover → extract → verify — exercised end-to-end)
- Design phase completed (agent architecture, provider interfaces, LLM abstraction layer)
- Prototype built (Python/FastAPI backend, React/Vite frontend, SQLite persistence, SSE streaming)
- Tested (unit tests covering the LLM abstraction layer across local/cloud backends and failure paths)
- Not yet deployed for external/production users

**CURRENT TRL:** 5–6 ("Component/system validated and demonstrated in a relevant environment; working prototype exercised end-to-end.")

**TO DO:**
- Broaden automated test coverage for the Source, Research, and Verification agents individually
- Benchmark extraction accuracy and completeness across a range of topics/domains
- Harden handling of anti-bot-protected and rate-limited sources
- Move persistence to a horizontally scalable store for multi-user deployment
- Package for hosted, multi-tenant deployment

---

## 12.) Prior Art Assistance

**Component #1 — Adaptive Schema Generation**
- Topic + live web-search context → LLM → column schema (name, type, description, required flag)
- Interactive, user-editable before extraction begins; user-added columns can be LLM-validated against the topic

**Component #2 — Multi-Agent Discovery-Extraction-Verification Pipeline**
- Planner → Source → Research → Verification, coordinated by a central orchestrator
- Verification decoupled from extraction: targeted, per-field re-search rather than whole-source re-crawl

**Component #3 — Row-Level Real-Time Streaming**
- Immediate per-row persistence; SSE push + short-interval poll fallback to the frontend
- Partial dataset exportable at any point, including mid-run

**Component #4 — LLM-Agnostic Unified Inference Interface**
- Single call-through abstraction used by all agents
- Local (on-device) or cloud-hosted LLM selected by configuration only, no agent-code changes
- Mid-run backend/credential swap supported without losing extracted data

**Component #5 — Pluggable Provider Layer**
- Interchangeable search providers and crawl providers (headless-browser primary, static-HTML fallback) behind common interfaces

**SUGGESTED KEYWORDS FOR PRIOR ART SEARCH:**
- "Multi-agent LLM web data extraction pipeline"
- "Adaptive schema generation from natural language topic AI"
- "LLM-based structured data extraction from web pages"
- "Autonomous dataset generation agent system"
- "Verification agent gap-filling structured data extraction"
- "Real-time streaming data extraction dashboard SSE"
- "LLM-agnostic unified inference interface local cloud"
- "Topic-to-table AI research agent"
- "Cooperative cancellation multi-agent data pipeline"

**SOME SPECIFIC VALUES OF THE INVENTION:**
- 4-stage agent pipeline: Planner → Source → Research → Verification, coordinated by Pipeline Orchestration
- Row persistence pushed via SSE, with a ~1 s poll as fallback data path
- Crawl strategy: headless-browser primary provider, static-HTML parser fallback
- Verification runs concurrently, per entity, via a shared worker pool
- Cooperative cancellation checked at every I/O boundary (between sources, post-crawl, between chunks, during verification wait)
- LLM backend selected by a single configuration setting — local vs. cloud — with no code change to agent logic

---

### Signatures

*(Complete as in the original template — primary contact signs first.)*

| | Name | Gender | Nationality | Address |
|---|---|---|---|---|
| Applicant | `[confirm — institution or self]` | | | |
| Inventor 1 | Amogh Gupta | Male | Indian | `[confirm]` |
| Inventor 2 | `[confirm if applicable]` | | | |

---

**Notes on filling out the actual form:**
- Keep each answer factual and concrete (specific mechanisms, numbers, and component names) rather than descriptive marketing language — patent/IP offices and licensing reviewers weigh disclosures on how precisely they pin down what is technically new.
- Disclose *before* any public presentation, blog post, or publication about AutoData Labs — public disclosure ahead of filing can jeopardize patentability in many jurisdictions.
- List every person who contributed an inventive idea (not just code) as a co-inventor; contribution need not be equal, but each named inventor should have added an original element.
- If you update the repo/architecture before filing, re-check Sections 5–7 and the Prior Art section against the current code, since those are the sections most likely to drift out of sync with the implementation.
