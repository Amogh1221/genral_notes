# AI System Design — Complete Interview Preparation Guide

> Compiled from: Exponent's ML System Design guide, System Design Handbook's Generative AI System Design guide, the open-source `ai-system-design-guide` (116-question bank), the `ai-engineering-field-guide`, and IGotAnOffer's GenAI interview guide. This single file is meant to be a self-contained study resource — you shouldn't need to leave it to prepare.

---

## Table of Contents

1. [What "AI System Design" Actually Means](#1-what-ai-system-design-actually-means)
2. [How to Use This Guide / Study Plan](#2-how-to-use-this-guide--study-plan)
3. [Framework A — Classic ML System Design (6 Steps)](#3-framework-a--classic-ml-system-design-6-steps)
4. [Framework B — Generative AI / LLM System Design (9 Steps)](#4-framework-b--generative-ai--llm-system-design-9-steps)
5. [Foundations You Must Know Cold](#5-foundations-you-must-know-cold)
6. [Model Landscape & Selection](#6-model-landscape--selection)
7. [RAG (Retrieval-Augmented Generation) Deep Dive](#7-rag-retrieval-augmented-generation-deep-dive)
8. [Agentic Systems](#8-agentic-systems)
9. [Memory & Long-Context Management](#9-memory--long-context-management)
10. [Inference Optimization](#10-inference-optimization)
11. [Evaluation & Observability](#11-evaluation--observability)
12. [Cost Optimization](#12-cost-optimization)
13. [Security, Safety & Multi-Tenancy](#13-security-safety--multi-tenancy)
14. [Production / MLOps for AI Systems](#14-production--mlops-for-ai-systems)
15. [The Full Question Bank (Q&A style)](#15-the-full-question-bank-qa-style)
16. [Worked System Design Scenarios](#16-worked-system-design-scenarios)
17. [Company-Specific Notes](#17-company-specific-notes)
18. [Interview Strategy, Communication & Common Mistakes](#18-interview-strategy-communication--common-mistakes)
19. [Glossary (Quick Reference)](#19-glossary-quick-reference)
20. [Further Resources](#20-further-resources)

---

## 1. What "AI System Design" Actually Means

AI System Design is now recognized as **its own interview category**, distinct from both:

- **Traditional software system design** (load balancers, databases, caching, consistency, sharding), and
- **Classic ML system design** (training pipelines, feature engineering, model architecture for a model you train yourself).

The shift is driven by the explosion of LLM-powered products. Instead of designing a **training pipeline** for a model you build from scratch, you are now usually asked to design an **orchestration architecture around pre-trained foundation models** — retrieval, prompting, agents, tool use, evaluation, safety, and cost control.

Companies with dedicated "AI System Design" rounds include Doctolib, Sprinter Health, and Anthropic (distributed search + LLM inference at scale). Many more (Google, Apple, OpenAI, Cohere, Salesforce, and most AI-first startups) are folding AI-flavored questions into existing system design loops.

**Interview format:** Usually 35–60 minutes, conversational, often onsite/final round. The interviewer is testing *how you reason under tradeoffs*, not whether you reach one "correct" architecture.

### Three flavors you'll encounter — know which one you're in

| Flavor | What's being designed | Typical prompt |
|---|---|---|
| **Classic ML System Design** | A model you train (recommender, ranker, classifier, fraud detector) | "Design a recommendation system for Spotify" |
| **Generative AI / LLM System Design** | A product built on top of a pretrained LLM (chat, RAG, agents) | "Design a customer support chatbot using an LLM" |
| **AI System Design (staff-level, production)** | The surrounding infrastructure: multi-tenancy, evals, orchestration, safety, cost at scale | "Design a multi-tenant RAG system where competitors cannot see each other's data" |

Mid-level candidates get AI system design questions but aren't expected to go deep — a reasonable high-level architecture is enough. **Senior/staff candidates are expected to go deep on failure modes, tradeoffs, and "what went wrong last time" with real production war stories.**

### What interviewers evaluate (5 dimensions)

1. **LLM awareness** — tokens, context windows, sampling params (temperature/top-p), differences between model families, latency/cost/context tradeoffs.
2. **Modular thinking** — a composable pipeline (embedding service, vector DB, prompt composer, LLM gateway, output filters), not a monolith. Each component independently scalable/versionable, with its own logging/metrics.
3. **Cost-conscious architecture** — caching, model tiering/routing, quantization, distillation, prompt minimization.
4. **Safety, security & governance** — prompt injection, hallucination detection/mitigation, RLHF-style feedback loops, PII handling, compliance.
5. **Clear, collaborative communication** — clarifying questions, diagrams, thinking aloud, quantifying everything (token budgets, QPS, cost/day).

---

## 2. How to Use This Guide / Study Plan

**If you have 1 week:**
- Day 1–2: Sections 5–7 (foundations + RAG) — this is asked in almost every interview.
- Day 3: Section 8 (agents) + Section 9 (memory).
- Day 4: Sections 10–13 (optimization, eval, cost, security).
- Day 5–6: Section 15 (Q&A bank) — read every answer once, then again out loud.
- Day 7: Section 16 (do 2–3 scenarios end-to-end on a whiteboard/paper with a timer, 35 minutes each).

**If you have 1 day:** Read Sections 1, 3, 4, 7, 8, 11, then skim the Q&A bank headers and read only the ones you're unsure about.

**Golden rule for practicing:** Don't just read the sample answers — **say them out loud** or type them from memory, then compare. Recognition ("yes I know this") is not the same as recall under interview pressure.

---

## 3. Framework A — Classic ML System Design (6 Steps)

Used when the prompt is "design a system that predicts/ranks/classifies X" (recommenders, fraud detection, ad ranking, ETA prediction, content moderation classifiers, etc.). Roughly 45 minutes, ~8 minutes/step.

### Step 1: Define the Problem (~8 min)
- Restate the business goal and translate it into an ML task: **recommendation** (rank by similarity — collaborative/content filtering), **regression** (predict a scalar), **classification** (categorize), **generation** (produce new samples), **ranking** (order elements).
- Ask clarifying questions: What's the success metric (clicks? engagement time? revenue?)? What's the traffic/scale (DAUs, QPS)? What data already exists vs. needs collecting? Any latency SLA? Compliance/privacy constraints?
- State assumptions explicitly and check in with the interviewer.

### Step 2: Data Processing Pipeline (~8 min)
- What data is needed (numeric, text, image, multimodal)? How is it collected (labeling, synthetic augmentation, implicit signals like clicks)?
- Feature engineering: bucketing, encoding categoricals, precomputed aggregates.
- Preprocessing: tokenization, normalization, dedup, missing-value imputation, PII masking.
- **Batch vs. real-time**: batch is simpler/cheaper; prefer batch training + a cache-backed serving layer refreshed periodically, escalate to real-time only where required.
- Watch for **data contamination**: correlated samples (e.g., same user or same spammer) must stay in the same train/test split.

### Step 3: Model Architecture (~8 min)
- Pick the **simplest model that hits the accuracy bar** — don't default to SOTA. SOTA benchmarks are usually measured on academic data, not your production distribution, and are often far more expensive to serve.
- Justify against: type of learning problem, whether it's directly user-facing (latency-sensitive) vs. batch/offline, need for personalization/frequent retraining, storage/compute/business constraints.
- Start with an interpretable baseline (e.g., collaborative filtering with a user-item matrix, or logistic regression) before reaching for deep learning.

### Step 4: Train & Evaluate (~8 min)
- Define the training signal precisely (e.g., click = positive, impression-no-click = negative).
- Evaluation axes: **accuracy** (F1/precision/recall/confusion matrix), **bias** (group fairness), **calibration** (do predicted probabilities match real-world frequencies?), **robustness** (sensitivity to small input perturbations), and **comparison against baselines** (random, simplest heuristic, human).
- Explicitly discuss tradeoffs between metrics (e.g., precision@k vs NDCG@k for ranking).

### Step 5: Deploy (~8 min)
- **Rollout strategy**: A/B test, canary, shadow deployment, feature flags.
- **Serving**: cloud vs. edge, model compilation/optimization (quantization, ONNX/TensorRT), autoscaling for traffic bursts.
- **Monitoring**: define ground-truth refresh cadence, drift-detection signals, alerting on metric regression, rollback plan.

### Step 6: Wrap-Up (~5 min)
- Recap the pipeline end-to-end in 3–4 sentences.
- Proactively raise: scaling bottlenecks, how you'd handle distribution shift over time, what you'd build next with more time.
- Ask the interviewer if they want to go deeper anywhere.

**Common mistakes to avoid:**
- Rushing straight to an architecture before clarifying the problem.
- Hunting for the one "correct" answer — there isn't one; justify tradeoffs instead.
- Defaulting to the fanciest model instead of the simplest one that meets the bar.
- Overcomplicating v1 — start with a low-capacity baseline, then layer in complexity (messy data, edge cases).
- Skipping evaluation/validation to spend all your time on the model architecture.

---

## 4. Framework B — Generative AI / LLM System Design (9 Steps)

Used when the prompt involves an LLM-powered product: chatbots, RAG search, coding assistants, content generation, agents. ~45–60 minutes.

### Step 1: Clarify the Use Case
Traditional system design asks about features/traffic/scale. Here you additionally need the **LLM boundary conditions** that drive latency, cost, architecture and UX:
- **Functional**: multi-turn or single-turn? domain scope? output format (autocomplete vs. full document vs. structured JSON)? what feedback signals (explicit thumbs up/down, implicit accept/reject) will improve it over time?
- **Non-functional**:

| Requirement | Key questions |
|---|---|
| Latency | Sub-second? Streaming acceptable? |
| Context | How large are typical inputs? Long-context needed? |
| Consistency | How much factual-error tolerance is acceptable? |
| Scale | 1K, 10K, or 1M users? Regional or global? |
| Security | Can data leave premises, or must models be self-hosted (VPC)? |

- **Data & retrieval**: Is there a knowledge base (docs, wiki, code)? Does it need grounding via RAG? How fresh must it be?
- A good opening line: *"Before I jump into architecture, I'd like to clarify a few things — especially LLM integration, latency, and data privacy."* This alone signals staff-level thinking.

### Step 2: Estimate Load, Token Budget & Throughput
Do the back-of-envelope math out loud. Example (100K DAU code assistant, 10 interactions/day, 1K input + 1K output tokens per call):

```
100,000 users × 10 interactions × 2,000 tokens = 2,000,000,000 tokens/day
≈ 23,000 tokens/sec average; ~70,000 tokens/sec at 3x peak

Cost at GPT-4-Turbo-era pricing ($0.003/1K in, $0.01/1K out):
  input:  1B tokens × $0.003/1K ≈ $3,000/day
  output: 1B tokens × $0.01/1K  ≈ $10,000/day
  total  ≈ $13,000/day ≈ $390K/month
```
Then pivot to the tradeoff: *"To cut this, I'd route simple prompts to a cheaper/smaller model and reserve the frontier model for hard cases."* Also mention generation speed (frontier hosted models ~30–60 tok/s; a self-hosted smaller model can hit 150–300 tok/s on a single high-end GPU) and that **streaming masks latency but concurrency is the real bottleneck** — estimate GPUs needed to hold P95 latency under your SLA.

### Step 3: High-Level Architecture
Sketch the pipeline:

```
Client (IDE plugin / web / CLI)
   → API Gateway (auth, rate limiting, telemetry)
   → LLM Orchestrator (stateless, coordinates everything below)
        → Retriever (embed query → vector search → optional rerank)
        → Prompt Builder (assemble context + query + template, token-aware truncation)
        → Model Selector / Router (routes by complexity/latency/privacy to cheap vs. frontier vs. self-hosted model)
        → Output Post-Processor (safety filters, citation/confidence checks, re-ranking of samples)
   → Response (streamed back to client)
```
Design traits to call out: **stateless, horizontally scalable API servers**; async processing for embeddings; a **prompt/response cache**; **streaming** for perceived latency; explicit **latency classes** (e.g., autocomplete <500ms on a small model, clarification 1–2s on mid-tier, deep analysis 2–4s on a frontier model).

### Step 4: Deep Dive — RAG
(See [Section 7](#7-rag-retrieval-augmented-generation-deep-dive) for the full treatment.) Trace: query → embed → vector search (dense) + optionally BM25 (sparse) → merge/rerank → chunk selection → prompt assembly → generation. Mention **hybrid search** (pure vector search misses exact keyword/error-code matches), **overlapping sliding-window chunking**, **metadata filters** (source/team/timestamp), and **periodic re-embedding** to avoid drift when source docs change.

### Step 5: Deep Dive — Model Interaction Patterns
- **Single-turn stateless** (autocomplete/FAQ) vs. **multi-turn with memory** (chat — requires token-budget management) vs. **streaming** vs. **function/tool calling** (multi-step reasoning, code assistants).
- **Model routing**: route by latency class, cost threshold, complexity, and privacy — e.g. cheap tier for 95% of traffic, frontier model for the hard 5%.
- **Prompt engineering**: modular templates, token-aware truncation, inline retrieval metadata (e.g. "Source: Service Docs | Updated: June 2026"), temperature/top-p tuned per task (low for facts/code, higher for creative).
- **Post-processing**: toxicity/length/format filters, confidence scoring or citation checks, ranking multiple sampled completions if using ensembling.

### Step 6: Trade-offs, Governance & Cost Control
- **Cost**: model distillation (train a small student on a large teacher's outputs), quantization (FP16→INT8, minimal accuracy loss), tiered routing policy (e.g., 70% cheap tier / 20% frontier / 10% self-hosted for sensitive data).
- **Safety/governance**: prompt-injection sanitization, content classifiers, jailbreak monitoring, full audit logging (prompts, responses, model versions), per-user/team token quotas, usage dashboards, and an **RLHF-style feedback loop** (thumbs up/down, accept/reject rate) feeding future fine-tunes.

### Step 7: Bottlenecks, Observability & Failure Modes
- **Bottlenecks**: token overload (truncate/summarize/stream), queue congestion (shard queues, priority tiers), vector index bloat (compress/prune/batch-rebuild), model cold-start (warm GPU pools), third-party rate limits (exponential backoff + caching).
- **Failure modes**: malformed-input prompt crashes, irrelevant RAG context (tune similarity thresholds, add metadata filters), unsupported/ungrounded outputs (add confidence scoring, double-pass validation), model/data drift (continuous monitoring is the only defense).
- **Dashboard**: token usage/user/session, vector match precision, P50/P95/P99 latency, RAG retrieval hit rate, % outputs filtered for toxicity. Tools: Prometheus/Grafana for metrics, Sentry-style tools for LLM error tracking, a custom token-budget heatmap.

### Step 8: Security, Compliance & Abuse Prevention
- **Prompt injection**: strict input sanitization, content boundaries, keep the system prompt in the API's dedicated `system` role rather than string-concatenated — never trust "ignore previous instructions" style inputs.
- **Data leakage**: segregate training/context data by access policy, redact PII before it ever reaches a prompt.
- **Toxic output**: moderation APIs + post-generation classifiers.
- **Compliance** (GDPR/HIPAA): logging with user opt-out, encryption in transit/at rest, transparency about what's logged.

### Step 9: Wrap-Up & Future Scaling
Summarize in 3–4 sentences, mention fallback modes (API timeout → smaller/faster model, GPU shortage → graceful degradation), propose next steps (distillation for 80% of traffic, personalization via summarized user memory, an A/B testing framework for prompts/retrieval strategies, an RLHF loop). Showing you think beyond the MVP is what separates senior from staff-level answers.

**Condensed 5-step mental map to keep in your head during the interview:**
1. Clarify use case & requirements
2. Estimate scale (tokens, QPS, cost)
3. Sketch modular architecture & token flow
4. Deep-dive RAG / routing / prompt construction
5. Discuss tradeoffs, bottlenecks, security, scaling

---

## 5. Foundations You Must Know Cold

### Tokens & context windows
- Tokens are the atomic unit LLMs process — not words. English averages ~0.75 words/token (~4 characters/token); code and non-Latin scripts often use more tokens per character.
- Cost and context limits are both denominated in tokens, so word-count intuition misleads you — always reason in tokens, and count with the model's actual tokenizer (e.g., `tiktoken` for OpenAI models) rather than approximating.
- Common mistake: forgetting message-role/formatting overhead when estimating a prompt's token count.

### Sampling parameters
- **Temperature**: controls randomness — low (0.0–0.3) for factual/code tasks, higher (0.7+) for creative tasks.
- **Top-p (nucleus sampling)**: restricts sampling to the smallest set of tokens whose cumulative probability exceeds p — another creativity/determinism lever.

### The "lost in the middle" problem
Research (Liu et al., 2023) shows LLMs attend more to the beginning and end of their context window than the middle — stuffing 20 chunks into context means the middle 10 may effectively be ignored.
**Mitigations**: retrieve fewer, higher-quality chunks (5–10, not 20+); rerank aggressively before context-stuffing; order strategically (most important chunk first, second-most important last); for very long contexts, use hierarchical summarization; pick models specifically evaluated for long-context performance.

### KV cache
- During autoregressive generation, the model needs Key/Value tensors for every previous token to compute attention at each new step. **Caching** these avoids recomputing them, taking generation from O(n²) to O(n) compute.
- Rough memory formula: `KV cache memory ≈ 2 × layers × KV_heads × head_dim × seq_len × batch × bytes_per_value`. For a 70B-class model at 8K context this can be several GB *per request* — with 100 concurrent requests you may need hundreds of GB just for KV cache, separate from model weights.
- **Optimizations**: Grouped/Multi-Query Attention (share KV heads across query heads, 4–8x memory reduction), **PagedAttention** (vLLM — manages KV cache like OS virtual memory, eliminating fragmentation), **prefix/context caching** (reuse cache across requests sharing a system prompt), **KV cache quantization** (store in INT8/FP8).

### Speculative decoding
A small **draft model** proposes several candidate tokens quickly; the large **target model** verifies them in a single forward pass; matching tokens are accepted, the rest regenerated from the first mismatch. Net effect: multiple tokens per expensive target-model call, with output mathematically identical to running the target model alone. Typical speedup: 2–3x when the draft and target are well-aligned. Alternatives: **Medusa** (multiple prediction heads instead of a separate draft model), **lookahead decoding** (Jacobi-iteration based).

### Batching strategies for serving

| Strategy | Mechanism | Pros | Cons |
|---|---|---|---|
| Static | Wait for N requests, process as one batch | Simple | High latency at low load |
| Dynamic | Batch within a time window | Adaptive | Still some wait |
| **Continuous** (vLLM) | Requests join/leave the batch as they arrive/finish | Near-optimal GPU utilization at any load | More complex to implement |
| Chunked prefill | Mix prefill and decode within a batch | Balances time-to-first-token vs. throughput | Newer technique |

### Quantization

| Method | Bits | Memory reduction | Quality loss | Use case |
|---|---|---|---|---|
| FP16 | 16 | 2x vs FP32 | None | Training / high-quality inference |
| INT8 | 8 | 2x vs FP16 | Minimal | Production serving |
| GPTQ | 4 | 4x vs FP16 | Small | Edge, cost-sensitive |
| AWQ | 4 | 4x vs FP16 | Smaller than GPTQ (activation-aware) | Default choice for most 4-bit production deployments |
| GGUF Q4_K_M | 4 | 4x vs FP16 | Small | CPU inference (llama.cpp) |

Always benchmark on **your** tasks before shipping a quantized model — quality loss is task-dependent.

---

## 6. Model Landscape & Selection

### Decision framework: which model family, when

- **General-purpose frontier model** (e.g., a top-tier chat/coding model): balanced performance, strong tool-calling, good default for prototyping.
- **Long-context specialist**: pick when you need >200K tokens of context (whole codebases, very long documents) or want the most cost-effective large-context option.
- **Small/cheap tier** (mini/haiku-class models): for high-volume, low-complexity tasks — classification, routing, simple extraction. Often 10–20x cheaper.
- **Reasoning models** ("thinking" models — o-series/R1-style): spend extra "thinking" tokens before answering. Use for math proofs, subtle code bugs, multi-step planning, or anywhere getting the answer right the first time beats several fast retries. **Don't** use them for high-volume simple tasks — a simple question can cost 5–10x more with a reasoning model because you pay for invisible thinking tokens, and latency is 3–10x higher.

**Practical approach**: prototype with a strong general model → evaluate on *your* domain data (benchmark leaderboards don't predict task performance) → build an abstraction layer so you can swap models without code changes → optimize cost by routing simple requests to cheaper models once the system is stable. Never rely solely on benchmark scores.

### Small vs. frontier model decision table

| Scenario | Use small model | Use frontier model |
|---|---|---|
| Classification / routing | ✓ | |
| Simple extraction | ✓ | |
| On-device deployment | ✓ | |
| High volume, low margin | ✓ | |
| Latency <100ms | ✓ | |
| Complex multi-step reasoning | | ✓ |
| Novel-task generalization | | ✓ |
| Agentic tool selection | | ✓ |

**Cascading pattern**: route through a small classifier first; simple queries go to the small model, complex ones escalate to frontier. Typically 70%+ cost reduction with minimal quality loss.

### Embedding model evaluation
- **MTEB (Massive Text Embedding Benchmark)** is the standard leaderboard across retrieval, classification, clustering, and semantic-similarity tasks.
- MTEB scores are averages across many tasks — a model ranking lower overall can still be *better* on your specific retrieval task. Always build a domain-specific test set and evaluate retrieval precision on your own data before choosing.
- Key selection factors beyond the score: max token length per chunk, dimensionality (affects storage/index size), cost, multilingual support if needed.

### Fine-tuning vs. prompting vs. RAG

| Factor | Favor RAG | Favor fine-tuning |
|---|---|---|
| Data freshness | Frequently updated | Static knowledge |
| Data volume | Any size | Need 1K–100K quality examples |
| Latency tolerance | Can accept retrieval overhead | Need fastest possible response |
| Use case | Factual grounding on your docs | Style/tone/behavior change |
| Privacy | Data stays under your control | Training data leaves for a training pipeline |
| Maintenance | Update docs anytime | Retrain whenever data changes |

**Key insight**: these are not mutually exclusive. A common production pattern is fine-tuning a model to follow your output format/tool-calling conventions, then using RAG to ground its factual answers — behavioral consistency from fine-tuning, factual accuracy from RAG.

Cost-wise, fine-tuning has upfront cost but reduces per-request cost via shorter prompts; break-even is typically in the 10K–50K request range depending on how much prompt length shrinks. **Never fine-tune a problem you could solve with a better prompt.**

---

## 7. RAG (Retrieval-Augmented Generation) Deep Dive

### Full pipeline (ingestion + query)

**Ingestion pipeline:**
1. Parse documents (PDF, HTML, Office, scanned images) with a document processor.
2. **Chunk** using a strategy suited to the document type (see table below).
3. **Embed** each chunk with a chosen embedding model.
4. Store vectors in a **vector database**; often also index raw text in a keyword search engine (e.g., Elasticsearch/BM25) for hybrid search.

**Query pipeline:**
1. Embed the incoming query.
2. Run **hybrid search**: dense (vector) + sparse (BM25) retrieval in parallel.
3. Merge results with **Reciprocal Rank Fusion** (RRF): `score = Σ 1 / (k + rank_i)`, k≈60.
4. **Rerank** the merged top-N (e.g., top 50) with a cross-encoder to get the true top 5–10.
5. Assemble the prompt: query + labeled, ordered context + instructions to cite sources.
6. Generate with the chosen LLM.
7. Log everything (retrieval latency, rerank latency, chunks used, generation latency) for observability/evaluation.

### Chunking strategies

| Strategy | How it works | Best for | Tradeoff |
|---|---|---|---|
| Fixed size | Split by token/char count | General purpose | May break mid-sentence |
| Sentence | Split on sentence boundaries | Q&A, conversational | Variable chunk sizes |
| Semantic | Cluster by meaning similarity | Coherent topics spanning paragraphs | Clustering compute cost |
| Recursive | Try large chunks, fall back to smaller | Structured documents | Implementation complexity |
| Parent-child | Retrieve on small chunks, return the larger parent for context | Need precision + surrounding context | Storage overhead |
| Whole document | No chunking | Short docs / summaries | Bounded by context length |

Use semantic or parent-child chunking when retrieval **precision** matters most; use fixed-size with overlap when you need speed/simplicity.

### Dense vs. sparse retrieval

- **Dense (embeddings)**: good at semantic similarity, paraphrase, conceptual matching. Weak at exact keyword matches, rare terms, proper nouns, error codes.
- **Sparse (BM25/TF-IDF)**: good at exact matches, rare/keyword terms. Weak at synonyms/semantic similarity.
- **Hybrid** (both, merged via RRF) is the standard production pattern, especially in domains with specific terminology (legal, medical, technical, or anywhere users paste exact error strings).

### Vector database indexing algorithms

| Algorithm | How it works | Tradeoff | Best for |
|---|---|---|---|
| HNSW | Multi-layer navigable graph | High recall (95–99%), memory-hungry | Production serving with quality bar |
| IVF | Cluster vectors, search only relevant clusters | Trade recall for speed via `nprobe` | Large datasets, cost-constrained |
| PQ | Compress vectors (lossy) | Some accuracy loss; often combined as IVF-PQ | Massive scale, memory-limited |

Tune HNSW's `ef_construction`/`ef_search`/`M` or IVF's `nlist`/`nprobe`, and always benchmark recall vs. latency on your own data.

### Vector database selection

| DB | Best for | Tradeoff |
|---|---|---|
| Pinecone | Managed, fast to start | Cost at scale, vendor lock-in |
| Qdrant | Self-hosted, high performance | Operational overhead |
| Weaviate | Hybrid search, multimodal | More complexity |
| Chroma | Local dev/prototyping | Not built for production scale |
| pgvector | Already on Postgres | Fewer features, slower at scale |

Rule of thumb: under 1M vectors, pgvector/Chroma is enough; 1M–100M, use Qdrant/Pinecone/Weaviate; 100M+, you need dedicated infra.

### Reranking
Two-stage retrieval: fast first-stage retrieval of ~50–100 candidates, then an expensive-but-accurate cross-encoder (or Cohere Rerank, or an LLM) reorders them, and only the true top-K goes into the prompt. Typically improves precision by 10–15%.
- **Skip reranking** when: latency budget <200ms, base retrieval quality is already sufficient, cost-per-query is tight at very high volume, or queries are simple.
- **Use reranking** when: precision is critical, you can afford 50–100ms extra latency, queries are complex/semantic, or the domain is high-stakes (legal/medical/financial).

### Multi-tenant RAG (critical security pattern)
**Cardinal rule: filter *before* retrieval, never after.**

```python
# WRONG — leaks other tenants' data into memory before filtering
results = vector_db.search(query, top_k=100)
filtered = [r for r in results if r.tenant_id == tenant]

# RIGHT — filter at the database query level
results = vector_db.search(
    query, top_k=10,
    filter={"tenant_id": {"$eq": tenant_id}}
)
```

Isolation levels by required security:

| Pattern | Isolation | Cost | Use case |
|---|---|---|---|
| Metadata filtering | Namespace-level | Low | Most SaaS apps |
| Separate collections | Collection-level | Medium | Sensitive data |
| Separate databases | Full | High | Regulated industries (healthcare, finance) |

Other required controls: every vector must carry a `tenant_id`; **cache keys must be scoped by tenant** (a shared semantic cache can leak personalized answers across users — "What's my account balance?" must never return a cached answer meant for a different user); audit logging must capture tenant context; system prompts must never mix data from multiple tenants; error messages must not leak information about other tenants.

### Handling tables, charts, and images in documents
- **Tables**: extract structure via document AI (Textract, Azure Document Intelligence); serialize to markdown for chunking, or create separate table-level embeddings/metadata summaries.
- **Images/charts**: use a vision-language model to generate a text description, index the description as text, and store an image reference for multimodal generation. Limitation: most embedding models are text-only, so retrieval quality is bounded by description quality.

### Agentic RAG vs. traditional RAG
- **Traditional RAG**: single retrieval pass — embed query → retrieve top-k → generate. Fast, cheap, no way to recover if retrieval was insufficient.
- **Agentic RAG**: the agent plans a retrieval strategy, retrieves, evaluates whether it has enough information, and refines/re-searches iteratively until it's satisfied, then generates.
- Use agentic RAG for complex, multi-document questions where the right search terms aren't obvious upfront, or research-style tasks where one finding surfaces new questions. It costs 5–10x more LLM calls and adds latency — don't use it for simple factual lookups.

### RAG evaluation (see also Section 11)
Three levels: **retrieval** (Precision@K, Recall@K, MRR, NDCG — needs labeled relevance judgments), **generation** via the **RAGAS** framework (faithfulness = is the answer grounded in retrieved context?; answer relevance = does it address the question?; context relevance = was retrieved context useful?; context recall = did we retrieve everything needed?), and **end-to-end** (correctness vs. ground truth, user thumbs up/down, task completion rate).

### Common production RAG failure: works on test data, fails in production
Checklist when this happens: (1) is the production **query distribution** different from your test set (vague/jargon-heavy real queries vs. clean test queries)? (2) does indexed content actually **cover** what users ask in practice? (3) is **retrieval quality** — not generation — the actual failure point (check retrieval metrics separately)? (4) are production documents a different length/shape than test docs, causing bad chunk boundaries? (5) are users sending **adversarial or messy inputs** (injected instructions, pasted logs, foreign languages) that never appeared in clean test data? (6) is the system **falling back to a cheaper model** under load/timeouts more than expected? Fix: add production-representative queries to your eval set, monitor retrieval and generation metrics *separately*, and A/B test changes.

---

## 8. Agentic Systems

### Agent vs. workflow
- **Workflow**: steps are known at design time; control flow is explicit (if/else, loops); deterministic; easier to test/debug/explain.
- **Agent**: the LLM decides what to do next based on observations at *runtime*; non-deterministic; more flexible, harder to predict.

Autonomy spectrum: `single prompt → chain → router → ReAct agent → multi-agent → fully autonomous`.

**Practical guidance**: most production systems are **workflows with agentic components**, not fully autonomous agents. Start with a workflow (deterministic, testable); add agency only where you truly need runtime flexibility (e.g., a support system might be a fixed workflow — classify → retrieve → respond — with an agent living only inside the retrieval step, deciding which of several data sources to query).

### The ReAct pattern (Reason + Act)
Loop: **Thought** (reason about state/next step) → **Action** (call a tool) → **Observation** (tool result) → repeat until done or a limit is hit.

```
Thought: I need NVIDIA's current stock price.
Action: get_stock_price(symbol="NVDA")
Observation: {"price": 142.50, "change": "+2.3%"}
Thought: I have what I need.
Final Answer: NVDA is at $142.50, up 2.3% today.
```

**Failure modes & mitigations**:
- Repeating the same failed action ("looping") → max-iteration limits, detect repeated actions and force termination.
- Wrong tool selection → clear, action-oriented tool names and descriptions with examples.
- Argument/parsing errors → strict schema validation, helpful structured error messages back to the model.
- Runaway cost → token/cost tracking with hard caps.

### Debugging an agent that takes way more steps than expected
(e.g., "47 LLM calls for a task that should take 5")
1. **Trajectory analysis**: pull the full trace (LangSmith/Langfuse-style) and look for a pattern — repeating the same action, oscillating between two states, or just inefficient-but-progressing.
2. **Identify the failure mode**: tool-output parsing failures (agent can't parse the result, retries with slight variation), unclear stopping conditions (agent doesn't know it's "done"), missing context (forgot earlier steps due to context overflow), or overly general instructions (agent wanders into tangents).
3. **Targeted fixes**: structured output schemas for tools; explicit success criteria in the system prompt; memory summarization/checkpointing for context overflow; an upfront planning step to reduce wandering.
4. **Guardrails**: hard `max_iterations`, and a "critic" pass that detects circular behavior and forces termination.
Debugging agents is like debugging distributed systems — get observability first, then reason about what went wrong.

### Tool use / function calling
Design tools like a good API: clear action-oriented names (`search_database`, not `db_tool`), detailed descriptions with examples in the docstring, strict parameter validation with helpful error messages, idempotent where possible, and structured (not prose) return values. Wrap every tool call in error handling that distinguishes validation errors, timeouts, and unknown failures so the agent can react appropriately.

### Multi-agent systems

| Pattern | Structure | Best for | Challenge |
|---|---|---|---|
| Hierarchical (manager/worker) | Manager decomposes & assigns | Complex decomposable tasks | Manager becomes a bottleneck |
| Peer-to-peer | Agents talk directly | Collaborative tasks | Coordination complexity |
| Blackboard | Shared state, agents read/write | Incremental refinement | Race conditions |
| Pipeline | Sequential handoff | Staged processing | No parallelism |
| Critic/Verifier | One generates, one critiques | Quality improvement via iteration | Extra cost/latency |

**Decision rule**: ask "can one agent with the right tools do this?" If yes, use one agent — simpler is better. Reach for multi-agent when the task spans multiple domains, needs different tool sets per phase, benefits from critique/verification, or parallelization gives a real latency win. Downsides: complexity, harder debugging, higher cost from multiple LLM calls.

### Model Context Protocol (MCP)
An open standard (Anthropic) for how AI applications connect to external tools/data — think "USB for AI tools." Standardizes **tool discovery** (what's available), **schemas** (JSON Schema in/out), and the **execution protocol**. Security benefit: capability-based permissions — instead of giving an agent raw DB access, expose a scoped MCP tool that can only run `SELECT` on specific tables, acting as a proxy with built-in guardrails. In production, MCP servers typically run as separate microservices behind an MCP router, giving centralized logging, rate limiting, and the ability to revoke tool access without touching agent code.

### Flow engineering
Design the agent's **control flow** as an explicit state machine rather than leaving every decision to the LLM: define clear states and transitions; the LLM decides *within* a state, not *whether to transition between* states; transitions are governed by explicit conditions. Benefits: predictable behavior, per-state testability, clear escalation points, cost control via per-state limits. Example: `Intake → Research (RAG) → Can Answer? → (yes) Respond / (no) Escalate → Confirm`.

### Long-running agent tasks
Challenges: tasks can run minutes/hours; mid-execution failures lose progress; costs can spiral; users need visibility. Patterns: **checkpointing** (save state after each step), **event sourcing** (log actions, rebuild state from the log), **database-backed state**. Frameworks like LangGraph provide built-in checkpointers so a task can resume from where it left off using a `thread_id`. Reliability additions: max iteration/cost limits, per-step and overall timeouts, a dead-letter queue for failed tasks, and a human-escalation path.

### Guardrails for agents that take real-world actions
Use **concentric rings of protection**:
1. **Action classification** — read-only (always allow) / reversible write (allow + log) / irreversible (require confirmation) / dangerous (block).
2. **Sandboxing** — execute in an isolated environment (e.g., Firecracker/E2B for code, staging for API calls) before promoting to production.
3. **Human-in-the-loop** for high-stakes actions (spend thresholds, broad user impact, external comms).
4. **Rate limiting** on cumulative impact (emails/hour, records/day, spend/session).
5. **Reversibility infrastructure** — snapshot prior state before changes, implement undo, keep audit logs.
6. **Kill switch** — a manual override that halts all agent activity immediately. Non-negotiable for production agents.
Core principle: assume the agent will occasionally do something wrong, and design so the damage is contained and recoverable.

---

## 9. Memory & Long-Context Management

Managing conversation history under a fixed context window — from simplest to most scalable:

1. **Sliding window**: keep the last N messages; oldest drop off. Simple, but loses early context.
2. **Summarization**: once the conversation exceeds a token threshold, summarize older turns and keep recent turns verbatim.
   ```python
   if token_count > 6000:
       old = messages[:-10]
       summary = summarize(old)
       context = [{"role": "system", "content": f"Summary: {summary}"}] + messages[-10:]
   ```
3. **Hierarchical summarization**: multiple granularities — recent turns verbatim, mid-age turns paragraph-summarized, ancient turns one-line summarized.
4. **Retrieval-based memory**: store all messages externally and retrieve the relevant ones per query, exactly like RAG applied to conversation history — the most scalable option.

Default recommendation for most chat products: summarization — users perceive the model as having a good memory without the cost of resending the full history on every call. Also see "memory tiers" (L1 short-term/working, L2 episodic/session, L3 long-term/persistent) as a framework staff-level interviews may probe.

---

## 10. Inference Optimization

(See also Section 5 for KV cache, quantization, batching, speculative decoding — repeated here as a checklist.)

**Latency components and how to attack each:**
1. **Network latency** (10–100ms): regional endpoints, connection pooling/keep-alive, edge deployment for global users.
2. **Time to first token** (100–500ms): shorter prompts, smaller models where quality allows, prompt/prefix caching, speculative decoding.
3. **Token generation** (10–50ms/token): streaming for perceived latency, cap `max_tokens`, faster/smaller models for simple tasks.
4. **Post-processing**: make it async/non-blocking; cache expensive steps.

Streaming responses are perceived as 2–3x faster than waiting for the full completion, even when total latency is similar. For sub-100ms requirements: self-host small models, use speculative decoding, cache common queries, precompute where possible.

**Serving framework comparison**: vLLM, TGI, and TensorRT-LLM all support continuous batching and PagedAttention; multi-LoRA support varies (strong in vLLM/TGI, limited in TensorRT-LLM at time of writing) — always check current docs since this space moves fast.

---

## 11. Evaluation & Observability

### LLM-as-Judge (when there's no ground truth)
1. Define explicit criteria (correctness, relevance, helpfulness, tone, etc.).
2. Write a **rubric with examples at every score level** — this is what makes LLM-as-judge consistent.
3. Score with a judge model; ideally a **panel** of 3–5 judges from different model families (same-family models share biases).

**Bias mitigations**:
- **Position bias**: models prefer whichever answer is shown first ~60–70% of the time — run each pairwise comparison twice with swapped order; if the winner flips, call it a tie.
- **Verbosity bias**: instruct the judge to explicitly ignore length.
- **Self-preference bias**: never let a model judge its own outputs — use a different model family as judge.

**Validation**: sample 50–100 judge outputs and have humans rate them independently; if correlation with human judgment is below ~0.7, revise the rubric/examples. Include known-answer "calibration examples" in every eval batch.

### RAGAS framework (RAG-specific)

| Metric | Measures | How |
|---|---|---|
| Faithfulness | Is the answer grounded in retrieved context? | LLM checks whether each claim is supported |
| Answer relevance | Does the answer address the question? | LLM regenerates questions from the answer, compares to the original |
| Context relevance | Was the retrieved context actually useful? | LLM rates relevance of each chunk |
| Context recall | Did retrieval get everything needed? | Compare retrieved chunks to ground-truth chunks |

```python
from ragas import evaluate
from ragas.metrics import faithfulness, answer_relevancy
result = evaluate(dataset, metrics=[faithfulness, answer_relevancy])
```

### Hallucination — types, detection, mitigation
**Types**: factual (wrong about the world), faithfulness (unsupported by provided context — the main RAG concern), fabrication (invented citations/quotes/sources).
**Detection**: cross-reference against a knowledge base; self-consistency (generate multiple times, check agreement); require and verify citations; NLI (entailment) models; LLM-as-judge faithfulness scoring.
**Mitigation**: ground strictly in retrieved context ("if it's not in the context, say you don't know"); enable abstention explicitly (models are trained to be "helpful," which fights against saying "I don't know" — you often need to prompt/train this in deliberately); force citations for every claim; lower temperature for factual tasks; add a post-generation fact-checking pass. **Hallucination cannot be fully eliminated** — design to detect and handle it gracefully, not to assume it away.

### Observability — three pillars, LLM-adapted
- **Logs**: request ID, model + params, token counts, latency breakdown, input/output content (or hashes if sensitive), retrieved chunks + scores for RAG.
- **Metrics**: request volume, P50/P95/P99 latency, token usage & cost per request, error rate by type, cache hit rate, **sampled quality scores over time** (not just operational metrics — a fast, available, but low-quality system is still failing).
- **Traces**: the full request path — embed → vector search (latency, chunks) → rerank (latency, final chunks) → LLM call (latency, tokens, model) → response — so you can pinpoint bottlenecks and reconstruct exactly what context produced a bad answer.

Tooling: LangSmith/Langfuse for LLM-native tracing; Prometheus/Grafana for standard metrics; OpenTelemetry as a vendor-neutral instrumentation standard.

### CI/CD for LLM applications
What changes: prompts (most frequent), retrieved context/data, model versions, sampling parameters, application code.
Pipeline: unit tests (core logic) → prompt tests (specific scenarios, expected behaviors) → evaluation suite (RAGAS/custom metrics on a golden test set) → cost-impact estimate. Version every prompt; tie evaluation results to prompt versions so you can roll back. Deploy gradually (1% → 10% → 100%) with quality-metric monitoring and automatic rollback triggers; A/B test significant changes. Define explicit **quality gates**, e.g.:
```yaml
quality_gates:
  faithfulness: ">= 0.85"
  answer_relevance: ">= 0.80"
  latency_p95: "<= 2000ms"
  cost_per_query: "<= $0.05"
```

### Designing a feedback loop for continuous improvement
1. **Signal collection**: explicit (thumbs up/down, ratings, corrections), implicit (regenerate clicks, copy actions, dwell time), automated (LLM-as-judge on a sample).
2. **Pipeline**: user action → event stream → aggregation → labeling queue → training/eval data.
3. **Analysis**: cluster failure cases by type, prioritize by impact, balance quick wins vs. systemic fixes.
4. **Deployment**: curated examples become few-shot examples; systematic failures drive prompt updates; large enough curated sets enable fine-tuning.
Loop: **Collect → Analyze → Improve → Measure → Repeat.**

---

## 12. Cost Optimization

Attack cost in layers, ordered by typical impact:

1. **Model selection (50–90% savings)** — use the smallest/cheapest model that clears your quality bar; cascade cheap→expensive; a fine-tuned small model often beats a heavily-prompted large one.
2. **Caching (30–80% fewer API calls)** — exact-match cache for repeated queries; semantic cache for near-duplicate queries (e.g., return cached response if embedding similarity > 0.95, taking care with personalized queries — see the tenant-cache warning in Section 7); provider-side prompt/prefix caching for shared system prompts.
3. **Prompt optimization (20–50% token reduction)** — cut redundant instructions, use concise phrasing, request structured output to bound response length, use few-shot examples sparingly.
4. **Batching (20–40% infra savings)** — batch async workloads (many providers offer ~50% discounts on batch APIs); continuous batching (vLLM) maximizes GPU utilization for sync workloads.
5. **Infrastructure** — quantized models (e.g., AWQ 4-bit) for self-hosting, right-sized GPUs, spot instances for fault-tolerant workloads.

**Always measure**: cost per query (broken down by pipeline stage), cost per successful user action (not just per call), token efficiency. Set alerts on cost spikes; A/B test every optimization to confirm quality didn't regress.

### Investigating a 10x cost spike
Check, in order: (1) did a model or route get switched to a more expensive tier unexpectedly (e.g., fallback to frontier model firing too often)? (2) did prompt sizes grow (context bloat, uncapped conversation history, verbose system prompts)? (3) did cache hit rate drop (cache invalidated, key scheme changed)? (4) did request volume spike disproportionately vs. revenue/usage (bot traffic, retry storms from a bug)? (5) are reasoning/"thinking" tokens being paid for on requests that don't need a reasoning model? (6) is a specific feature/customer segment responsible (segment cost by feature and by tenant)?

---

## 13. Security, Safety & Multi-Tenancy

### Threat categories & mitigations

| Layer | Threat | Mitigation |
|---|---|---|
| Input | Prompt injection | Input validation, instruction hierarchy (system role vs. user role kept structurally separate) |
| Input | Jailbreaking | Refusal training/testing, output filtering |
| Data | Context/cross-tenant leakage | Tenant isolation, permission-aware retrieval |
| Data | PII exposure | Detection, redaction, anonymization |
| Output | Harmful content | Output filtering, guardrail classifiers |
| Output | Hallucinated secrets | Never place secrets/credentials in prompts |

**Defense in depth for prompt injection** (no single layer is sufficient):
1. Input isolation — wrap untrusted input in clear delimiters/XML tags and instruct the model to treat tagged content as data, not instructions.
2. Input filtering — scan for known injection phrases ("ignore previous instructions," role-play jailbreak attempts).
3. Output validation — check whether the response leaked the system prompt or adopted an unauthorized persona.
4. Least privilege — any tools the model can call should have minimal permissions, so a successful injection has limited blast radius.
5. Monitoring — log everything, run anomaly detection for spikes in suspicious patterns.
Bottom line: LLMs cannot perfectly distinguish instructions from data — the goal is to make attacks hard and cap the damage when they succeed, not to claim a perfect defense.

### Ensuring one user's context never leaks to another
- **Request isolation**: don't batch/share prefix caching across users if strict isolation is required (batch size 1, or disable cross-user prefix caching).
- **Memory isolation**: KV cache not shared across tenants; for the most sensitive workloads, dedicate a full model deployment per tenant.
- **Model isolation**: watch for fine-tuned models potentially memorizing training data — if multiple tenants fine-tune the same base model, true isolation requires separate fine-tuned models per tenant.
- **I/O sanitization**: scan outputs for signs of leakage (other users' names, unexpected system-prompt fragments).
- **Audit logging**: log prompts/responses with user IDs; periodically audit for cross-user data.
- **Semantic cache edge case**: caching "What's my account balance?" by embedding similarity alone can leak user A's data to user B — personalized-query cache keys must include user/tenant identity.

### Rate limits & multi-provider resilience
Handle RPM/TPM/TPD/concurrency limits with: queued requests + exponential backoff, request batching, priority queues for urgent traffic, multi-provider fallback, aggressive caching, and load shedding under overload.
```python
@retry(wait=wait_exponential(multiplier=1, min=4, max=60), stop=stop_after_attempt(5))
async def call_llm_with_retry(prompt):
    return await llm.generate(prompt)
```
Design an abstraction layer that maps logical task names to actual models/providers in config (not code), so a provider deprecation or outage is a config change, not a redeploy — and so you can shadow-test/gradually roll out a new model version against a golden evaluation set before fully switching.

---

## 14. Production / MLOps for AI Systems

- **Model deprecation resilience**: pin model versions explicitly; maintain a golden evaluation set that runs against any candidate model; shadow-test new models in production before cutover; gradual rollout with metric monitoring; keep a multi-provider fallback path.
- **Framework choice (LangChain/LlamaIndex/DSPy vs. building from scratch)**: use a framework for rapid prototyping, team familiarity, or when you need its native observability tooling; go bespoke when performance is critical, the use case is simple enough that a direct API call is cleaner, you need full behavioral control, or dependency surface must be minimal. A common pattern: prototype with a framework, then migrate only the hot paths to direct API calls once profiling shows the abstraction overhead matters.
- **DSPy** (prompts as optimized parameters rather than hand-written strings): valuable for complex multi-step pipelines with training data and a clear metric; overkill for simple prompted Q&A.

---

## 15. The Full Question Bank (Q&A style)

*Read every question; for ones you're unsure about, write your own answer before reading the model answer.*

### RAG Architecture

**Q: Walk me through the architecture of a production RAG system.**
Cover both pipelines (ingestion: parse → chunk by doc type → embed → store in vector DB + keyword index; query: hybrid retrieval → RRF merge → rerank → context assembly with source labels → generation with citation instructions) plus observability hooks at every stage. Expect a follow-up on tables/images (see Section 7).

**Q: When would you choose RAG over fine-tuning, and vice versa?**
See the decision table in Section 6. Key line: *"these are not mutually exclusive — I often fine-tune for format/behavior and use RAG for facts."*

**Q: How do you handle the 'lost in the middle' problem?** — See Section 5.

**Q: Explain chunking strategies and when to use each.** — See Section 7 table.

**Q: How would you evaluate a RAG system?** — Retrieval metrics + RAGAS + end-to-end, described in Sections 7 & 11. Target rough numbers to cite: Precision@5 > 0.8, Recall@10 > 0.9, faithfulness > 0.85, answer relevance > 0.80.

**Q: Describe hybrid search and when you'd use it.** — Dense vs sparse strengths/weaknesses + RRF formula (Section 7).

**Q: How do you handle multi-tenant RAG systems?** — Filter-before-retrieval principle + 3 isolation levels + cache/audit/system-prompt controls (Section 7).

**Q: What is reranking and when would you skip it?** — Two-stage retrieval; skip under tight latency/cost budgets or when first-stage quality is already sufficient (Section 7).

**Q: How would you handle documents with tables, charts, images?** — Document AI for tables, vision-LLM descriptions for images, with the caveat that retrieval quality is bounded by description quality (Section 7).

**Q: Explain vector database indexing algorithms (HNSW, IVF, PQ).** — Section 7 table; always mention the recall/latency/memory tradeoff and that you'd benchmark on your own data.

### Agentic Systems

**Q: What's the difference between an agent and a workflow?** — Section 8. Key line: *"most production systems are workflows with agentic components, not fully autonomous agents."*

**Q: Explain the ReAct pattern.** — Section 8, with the concrete trace example and the four main failure modes.

**Q: How do you implement tool use / function calling?** — Clear naming, detailed docstrings with examples, strict validation, structured (not prose) outputs, and typed error handling (Section 8).

**Q: How would you design a multi-agent system?** — Patterns table + decision rule ("can one agent do this?") from Section 8.

**Q: Explain the Model Context Protocol (MCP).** — Section 8: standardizes discovery/schemas/execution; security benefit is capability-based scoped tools; production pattern is MCP servers behind a router.

**Q: How do you handle long-running agent tasks?** — Checkpointing/event-sourcing/DB-backed state, max iteration/cost/time limits, dead-letter queue, human escalation (Section 8).

**Q: What is flow engineering?** — State machine for control flow; LLM decides *within* states, not *between* them (Section 8).

**Q: Your agent takes 47 LLM calls for a 5-call task — debug it.** — Trajectory analysis → identify failure mode → targeted fix → guardrails (Section 8, full walkthrough).

**Q: Design guardrails for an agent that takes real-world actions.** — The six "concentric rings" (Section 8).

**Q: Explain Agentic RAG vs. traditional RAG.** — Single-pass vs. iterative retrieve-evaluate-refine loop; 5–10x more expensive; use only for genuinely complex multi-document questions (Section 7).

### Model Selection

**Q: How do you choose between frontier models (e.g., a GPT-class, Claude-class, and Gemini-class model)?** — Balanced general use → GPT/Claude-class; very long context / cost-sensitive → Gemini-class; always validate on your own domain data rather than trusting a leaderboard (Section 6). *(Note: specific model names/prices age quickly — always verify current pricing/benchmarks before citing exact numbers in an interview.)*

**Q: When would you use a small model vs. a frontier model?** — Section 6 decision table + cascading pattern.

**Q: Explain reasoning models. When are they worth the cost?** — Section 6: worth it for math/code-debugging/complex planning where correctness > speed; not worth it for simple Q&A, high volume, or latency-sensitive paths; route via a complexity classifier.

**Q: How do you evaluate and compare embedding models?** — MTEB as a baseline, but always build a domain-specific test set (Section 6).

### Optimization

**Q: Explain the KV cache and why it matters.** — Section 5/10, with the memory formula and optimization list (GQA/MQA, PagedAttention, prefix caching, KV quantization). Be ready for the follow-up: *"what's the memory for serving 100 concurrent requests?"* — walk through the arithmetic live.

**Q: What is speculative decoding and when would you use it?** — Section 5.

**Q: Compare batching strategies for LLM serving.** — Section 5/10 table; know that continuous batching (vLLM) is the modern default.

**Q: How do you optimize LLM inference costs?** — The 5-layer framework in Section 12, with rough savings percentages for each layer.

**Q: Explain quantization techniques for LLM deployment.** — Section 5 table; AWQ as the practical default for 4-bit.

### Evaluation

**Q: How do you evaluate LLM outputs when there's no ground truth?** — LLM-as-judge with rubric + bias mitigations + human-correlation validation (Section 11).

**Q: Explain the RAGAS evaluation framework.** — Section 11 table + code snippet.

**Q: How do you detect and handle hallucinations?** — Section 11: types, detection strategies, mitigation techniques, and the "cannot be fully eliminated" framing.

**Q: Design an evaluation system for comparing two LLMs on open-ended tasks.** — Panel of diverse judges, pairwise comparison with positional debiasing, structured rubric, inter-rater agreement tracking, human validation on a sample, and statistical significance via a large enough comparison set (≥500 pairs) with confidence intervals.

### Production / MLOps

**Q: How do you implement observability for LLM applications?** — Three pillars adapted for LLMs (Section 11), including the point that quality metrics matter as much as operational ones.

**Q: Describe CI/CD for LLM applications.** — Prompt versioning, evaluation gates, gradual rollout with auto-rollback (Section 11).

**Q: How do you handle rate limits and quotas?** — Backoff, batching, priority queues, multi-provider fallback, caching, load shedding (Section 13).

**Q: Describe strategies for LLM application security.** — Threat table + defense-in-depth for prompt injection + multi-tenant isolation (Section 13).

### Ensembles & Advanced Reasoning

**Q: Self-Consistency vs. Best-of-N — when would you use each?** — Self-consistency needs *extractable, comparable* answers (math, multiple choice) and works by majority vote over k reasoning paths; Best-of-N is for open-ended generation with no single right answer, scored by a reward model/judge and picking the top sample. Decision test: "can I extract and compare answers? If yes → self-consistency; if no → Best-of-N."

**Q: How do you prevent reward hacking with Best-of-N?** — Use an ensemble of 3+ diverse reward models; aggregate conservatively (25th percentile or min, not mean); monitor sample diversity as an early-warning signal; periodically calibrate against human preference; score multiple dimensions (quality/safety/relevance) separately and require good scores on all.

**Q: Ensemble learning vs. model arbitration — what's the difference?** — Ensemble *combines* outputs collaboratively (voting/averaging/stacking) into a composite; arbitration *selects* a single best output competitively (reward scoring/ranking/routing). Use ensembling when there's a correct-answer format and you want reduced variance; use arbitration for open-ended output where you want the single best, not the average. They combine well: generate diverse candidates (ensemble thinking), then select the best one (arbitration).

**Q: Multi-Agent Debate vs. Mixture of Agents — when would you use each?** — Debate is adversarial (models critique each other over rounds) — best for fact verification, catching hallucinations, stress-testing reasoning. Mixture of Agents is collaborative (layer 1 generates diverse perspectives, layer 2 aggregates/synthesizes) — best for complex synthesis, multi-domain problems, combining complementary strengths. They can combine: MoA to build comprehensive analysis, then Debate to verify factual claims before publishing.

### Frameworks & Prompt Engineering

**Q: When should you use a framework (LangChain-style) vs. build from scratch?** — Section 14.

**Q: How do you manage context window limits with long conversations?** — Section 9 (sliding window / summarization / hierarchical / retrieval-based).

**Q: How do you defend against prompt injection attacks?** — Section 13, layered defense.

**Q: When would you choose fine-tuning over prompt engineering?** — Section 6 decision framework, plus: *"I do not fine-tune for problems I can solve with better prompts."*

**Q: How do you optimize latency for real-time LLM applications?** — Section 10, latency components broken down.

**Q: Explain the tradeoffs between different vector database options.** — Section 7 table + scale-based decision rule.

**Q: How do you handle model updates and deprecations from providers?** — Section 14, abstraction layer + golden eval set + shadow testing + multi-provider fallback.

**Q: What is DSPy and when would you use it?** — Section 14.

**Q: How do you design a feedback loop for continuous improvement?** — Section 11.

**Q: Explain token counting and why it matters.** — Section 5.

---

## 16. Worked System Design Scenarios

For each, budget ~35 minutes: 2 min clarifying questions, 5 min high-level architecture, then deep dives into the 2–3 hardest sub-problems, ending with a 3-minute wrap-up on scaling/reliability. Practice these out loud with a timer.

### Scenario 1 — Customer Support Chatbot
**Requirements**: 10,000 tickets/day, 5 languages, access to product docs + order history, ticketing-system integration, human handoff.
**Structure**:
1. *Clarify*: % fully automatable? First-response SLA? Compliance constraints? Existing tech stack?
2. *High-level*: User → API Gateway → Chat Service → Agent (RAG + tools) → LLM.
3. *Data pipeline*: doc ingestion + chunking; order-history API integration; multilingual embedding strategy.
4. *Agent design*: intent classification first (route simple vs. complex) → RAG for doc questions → tool calls for order lookup/ticket creation → explicit escalation criteria → state machine (flow engineering) for the conversation.
5. *Multi-language*: multilingual embedding model, translation layer or a natively multilingual LLM, language detection on input.
6. *Reliability*: fallback to human on low confidence; latency/quality monitoring; per-conversation cost tracking.
7. *Scaling*: cache frequent queries, batch non-urgent operations, autoscale on ticket volume.

### Scenario 2 — Document Processing Pipeline
**Requirements**: 100,000 docs/day (PDF, images, scans), extract structured data (invoices/contracts/forms), 99% accuracy, HIPAA compliance.
**Structure**: `Upload → Classification → OCR/Extraction → Validation → Human Review → Output`.
- Fine-tuned classifier routes by document type.
- Extraction: Document AI (Textract/Azure Document Intelligence) for structured forms; vision-LLM for variable/complex layouts; combine for accuracy.
- Validation: schema validation, cross-field consistency, business rules, confidence thresholds.
- Human-in-the-loop queue for low-confidence extractions with a reviewer correction UI feeding back into the model.
- HIPAA: PHI detection/handling, encryption at rest/in transit, audit logging, access controls.

### Scenario 3 — Enterprise Search (RAG at scale)
**Requirements**: 10M documents, 50,000 employees, role-based access control, real-time document updates.
**Key points**: permission-aware multi-tenant/RBAC architecture (filter by user's access grants *before* retrieval, same principle as Section 7's tenant isolation); chunking strategy for mixed document types; hybrid search; real-time incremental indexing pipeline (not full reprocessing); caching for common queries; evaluation and quality monitoring.

### Scenario 4 — Code Assistant (IDE-integrated)
**Requirements**: IDE integration, repository-aware context, generation + explanation, streaming.
**Key points**: repository indexing with code-aware chunking (function/class boundaries, not arbitrary token counts); context assembly (current file, imports, related files — often via a dependency graph, not just semantic search); latency optimization (caching, streaming, small local model for autocomplete vs. frontier model for deep debugging — see the token-budget math in Section 4 Step 2); code-specific evaluation (does generated code compile/pass tests, not just "looks right"); privacy considerations for proprietary code (on-prem/VPC deployment if code cannot leave premises).

### Scenario 5 — Content Moderation System
**Requirements**: 1M posts/day, multi-modal (text/image/video), <500ms latency, appeal workflow.
**Key points**: cascading classifiers (cheap fast filter → expensive precise model only for uncertain cases); multi-modal pipeline (separate models per modality, fused decision); threshold tuning for precision/recall tradeoff (false positives anger users, false negatives are a brand/legal risk); human review queue for borderline cases and appeals; feedback loop so review outcomes retrain the classifiers.

### Additional prompts to self-practice (from real interview reports)
- Design an AI-powered candidate-sourcing system.
- Design a system to process 10K user uploads/month (payslips, IDs, references).
- Design a system where doctors' notes automatically generate insurer billing submissions.
- Design a fraud detection system with a 100ms decision budget and explainability requirements.
- Design a Hospital Voice Assistant (background noise, privacy, latency, domain vocabulary).
- Design a Legal Contract Generation system with compliance requirements.
- Design a recommendation engine with personalized natural-language explanations at 50M-user scale.
- Design a permission-aware knowledge management system for 2M internal documents.
- Design an AI chatbot at ChatGPT/Claude-service scale — discuss the tradeoffs of scaling to 1M daily users.
- Your app does 1M queries/day — how do you cut cost without hurting quality?

---

## 17. Company-Specific Notes

- **Anthropic**: distributed search + LLM inference at scale is a known focus area — practice reasoning about serving infrastructure (batching, KV cache, autoscaling) alongside product-level RAG/agent design.
- **Doctolib**: has a named "AI System Design Interview" round.
- **Sprinter Health**: has a named "AI-Focused Systems Design" round.
- **Google, Apple, OpenAI, Cohere, Salesforce**: increasingly fold GenAI-flavored questions into standard system design loops rather than running a fully separate round.
- **General pattern**: expect AI-flavored prompts anywhere the company's core product is AI-first (coding tools, AI-native SaaS, model providers) even if the role title doesn't say "AI engineer."

*(Company practices change frequently — always check recent interview reports on sites like Glassdoor/Blind/interview-prep platforms for the specific company you're targeting, close to your interview date.)*

---

## 18. Interview Strategy, Communication & Common Mistakes

**Do:**
- Open with clarifying questions — functional *and* non-functional (latency, privacy, scale, error tolerance). This alone signals seniority.
- Quantify everything out loud: token budgets, QPS, cost/day, GPU count. Rough back-of-envelope math beats vague hand-waving.
- Draw a clean diagram and refer back to it as you narrate data flow.
- Explicitly name tradeoffs ("push vs. pull," "prompt size vs. latency," "open-source vs. hosted," "RAG vs. fine-tuning") rather than presenting one option as obviously correct.
- Bring up failure modes and security/safety proactively — interviewers note it as a gap if you don't.
- Wrap up with a crisp 3–4 sentence summary and proactive "here's what I'd tackle next with more time."

**Don't:**
- Rush to an architecture before scoping the problem.
- Hunt for "the right answer" — justify your tradeoffs instead.
- Default to the fanciest/newest model or pattern (reasoning models, multi-agent, fine-tuning) when a simpler one would do — interviewers are testing judgment, not buzzword recall.
- Skip evaluation/observability/security to spend all your time on the happy-path architecture.
- Forget to mention cost — it is one of the five things interviewers are explicitly grading.
- End abruptly without a wrap-up.

**Senior/staff-specific expectation**: be ready to go 3–5 questions deep with real production war stories — "what broke, what you'd do differently, what you still don't know." Confident-sounding answers are graded *down* relative to answers that show calibrated uncertainty about genuinely unsettled areas (agentic systems, multi-tenant fine-tuning isolation, evaluation of open-ended generation) — this is an actively evolving field and interviewers know it.

---

## 19. Glossary (Quick Reference)

- **RAG** — Retrieval-Augmented Generation: retrieve relevant context from an external source before generating, to ground answers and reduce hallucination.
- **Chunking** — splitting documents into retrievable units; strategy depends on document type and precision needs.
- **Hybrid search** — combining dense (embedding) and sparse (BM25/keyword) retrieval, merged via Reciprocal Rank Fusion.
- **Reranking** — a second-stage, more expensive scoring pass over first-stage retrieval candidates to improve precision.
- **HNSW / IVF / PQ** — vector index algorithms trading off recall, latency, and memory.
- **KV cache** — cached Key/Value tensors from prior tokens, avoiding recomputation during autoregressive generation.
- **PagedAttention** — vLLM's virtual-memory-style management of the KV cache to reduce fragmentation.
- **Speculative decoding** — a small draft model proposes tokens, a large target model verifies them in one pass, for a speedup with identical output quality.
- **Quantization (INT8/GPTQ/AWQ/GGUF)** — reducing weight precision to cut memory/compute at some quality cost.
- **ReAct** — Reason+Act agent loop: Thought → Action → Observation, repeated.
- **MCP (Model Context Protocol)** — an open standard for how AI applications discover and call external tools/data sources.
- **Flow engineering** — designing an agent's control flow as an explicit state machine rather than leaving all decisions to the LLM.
- **RAGAS** — an evaluation framework for RAG systems (faithfulness, answer relevance, context relevance, context recall).
- **LLM-as-judge** — using an LLM to score another LLM's outputs against a rubric, with debiasing techniques (position, verbosity, self-preference).
- **Self-Consistency** — majority vote over multiple reasoning paths for tasks with extractable, comparable answers.
- **Best-of-N** — generate N candidates, select the best via a reward model/judge; for open-ended tasks with no single correct answer.
- **Multi-Agent Debate** — adversarial multi-round critique between models to catch errors/hallucinations.
- **Mixture of Agents (MoA)** — collaborative multi-layer generation/aggregation to synthesize diverse perspectives.
- **Prompt injection** — malicious input crafted to override a model's instructions.
- **Multi-tenancy isolation** — architectural guarantees that one customer/user's data can never leak to another, enforced by filtering *before* retrieval and scoping caches/logs by tenant.
- **Distillation** — training a smaller "student" model to mimic a larger "teacher" model's outputs, for cheaper serving.
- **Continuous batching** — a serving technique (vLLM) where requests join/leave an in-flight batch dynamically for near-optimal GPU utilization at any load.

---

## 20. Further Resources

**Primary sources used to compile this guide (worth exploring directly for even more depth):**
- `ai-system-design-guide` (open-source, MIT-licensed, GitHub — search "ombharatiya ai-system-design-guide") — a staff-level living reference with a 116-question interview bank, answer frameworks with a worked mock transcript, whiteboard exercises, and deep-dive case studies (multi-tenant SaaS, autonomous coding agents, fraud detection, document intelligence, voice healthcare, compliance automation, knowledge management, real-time search, recommendation engines, customer support automation). Also has companion 3,000+ line guides specifically on AI evaluation tooling (Phoenix, Langfuse, LangWatch).
- `ai-engineering-field-guide` (GitHub, search "alexeygrigorev ai-engineering-field-guide") — real interview questions organized by company and topic, including a dedicated AI system design question file.
- Exponent's "Machine Learning System Design Interview" guide (tryexponent.com/blog) — the classic 6-step ML system design framework with a full worked Spotify-recommendation example, plus a large bank of real FAANG ML system design prompts organized by category (recommendation, ranking, NLP, moderation, monitoring, computer vision, time series, personalization, ad targeting).
- System Design Handbook's "Generative AI System Design Interview" guide (systemdesignhandbook.com) — the 9-step GenAI framework used in Section 4, plus a dedicated "Agentic System Design" and "AI System Design" guide on the same site worth reading for more scenario practice.
- IGotAnOffer's "Generative AI System Design Interview" guide — company-specific prep notes and an HLD-vs-LLD primer.
- TechEon's "Complete Agentic AI System Design Interview Guide" (Medium) — 40 curated staff-level agentic AI questions across 8 domains, with an emphasis on production failure modes and honest uncertainty as a hallmark of strong senior answers.

**How to keep this current**: model names, prices, and specific benchmark numbers in this space change every few months. Before an interview, do a quick search for "[target company] AI system design interview [current year]" and skim recent candidate reports — the frameworks and patterns in this guide are durable, but specific model/pricing facts should be refreshed close to interview day.

**Practice cadence that works well**: do one full 35–45 minute scenario (Section 16) out loud per day, timed, ideally with a peer or recorded so you can review your own clarity — reading this document builds recognition, but only rehearsing under time pressure builds interview-ready recall.
