# AI Engineering Portfolio — Himanshu Tamboli

Seven repositories that walk the **full lifecycle of shipping AI products** — from product
analytics, to a calibrated ML service, to a grounded RAG assistant, to LLM observability, to a
guarded autonomous agent, to the PM growth-analytics view that ties usage to unit economics —
each built as production software with tests, CI, honest evaluation, and docs, on one shared
engineering baseline.

Not notebooks. Not demos-that-only-run-on-my-machine. Every repo is `uv sync && pytest`-green,
CI-gated, and shippable.

---

## The arc

```
  data sense        model rigor        LLM application       observability          autonomy
 ┌────────────┐   ┌──────────────┐   ┌──────────────────┐  ┌────────────────┐   ┌────────────────┐
 │  product-  │   │   churn-     │   │  rag-knowledge-  │  │      llm-      │   │   agentic-     │
 │ analytics- │──▶│ risk-service │──▶│    assistant     │─▶│  observatory   │◀──│   workflow     │
 │    mini    │   │              │   │                  │  │   ★ flagship   │   │  ★ flagship    │
 └────────────┘   └──────────────┘   └──────────────────┘  └────────────────┘   └────────────────┘
        └──────────────┴─────── built on ────────┴──── ai-project-template ──────────┘

              the agent flagship is instrumented BY the observability flagship ─┘
```

The two flagships are deliberately connected: **`agentic-workflow` emits traces that
`llm-observatory` records** — a systems-level demonstration that these aren't isolated
exercises but a coherent view of how AI products are actually operated. And
**`ai-product-analytics` is the product-side counterpart to `llm-observatory`** — the same kind
of telemetry, read for growth and unit economics instead of system health.

---

## The projects

| # | Repo | What it is | Headline result |
|---|---|---|---|
| 1 | [product-analytics-mini](https://github.com/himanshutamboli/product-analytics-mini) | Product-analytics engine: DuckDB SQL metrics + a typed polars pipeline + a Streamlit dashboard (funnel, cohort retention, DAU/WAU/MAU). | Catches the classic **unbounded-retention misreport**; metrics match SQL exactly. |
| 2 | [churn-risk-service](https://github.com/himanshutamboli/churn-risk-service) | Deployable churn model: leakage-safe features, calibration, a **business-cost** threshold, FastAPI `/predict`, multi-stage Docker, PSI drift. | **PR-AUC 0.647** (vs 0.265 base); cost-optimal threshold cuts expected cost **$103k→$55k**. |
| 3 | [rag-knowledge-assistant](https://github.com/himanshutamboli/rag-knowledge-assistant) | Grounded RAG over a 21-article corpus: chunking, retrieval, cited generation with refusal, a streaming chat UI. | **recall@3 1.0, MRR 0.94**; faithfulness judge **100% agreement** with human labels. |
| 4 | [llm-observatory](https://github.com/himanshutamboli/llm-observatory) ★ | **Flagship.** LLM observability platform: trace/span/eval data model (SQLAlchemy + Alembic), instrumentation SDK, offline + online eval, regression detection, alerting, dashboard. | One-command demo surfaces a **caught regression** end to end; 41 tests. |
| 5 | [agentic-workflow](https://github.com/himanshutamboli/agentic-workflow) ★ | **Flagship.** AIOps incident-triage agent: planner/executor loop over tools, guardrails (retries, cost cap, human-in-the-loop), instrumented by `llm-observatory`. | **Task-success-rate 83%** + a **false-rollback** metric; an honest, measured eval. |
| 6 | [ai-product-analytics](https://github.com/himanshutamboli/ai-product-analytics) | GenAI product **growth analytics**: adoption, retention cohorts, feature funnels, AI quality & CSAT, unit economics, and **A/B experimentation** (two-proportion z-tests, CIs, ship/stop decisions) — the PM counterpart to `llm-observatory`. | Surfaces a **v2 launch that lifts capability but steps cost/session +70%**, and calls experiments on **statistical significance**, not eyeballed deltas. |
| 7 | [ai-project-template](https://github.com/himanshutamboli/ai-project-template) | The shared baseline every repo above is forged from: `uv`, `ruff`, `pytest`, `pre-commit`, GitHub Actions CI, src layout. | Zero-to-green new project in minutes. |

---

## What this portfolio demonstrates

- **Product & data sense** — defining the right metric and spotting when a metric lies
  (`product-analytics-mini`), and reading a GenAI product's growth, retention, and **unit
  economics** — plus running **A/B experiments** to ship on statistical evidence rather than
  eyeballed deltas (`ai-product-analytics`).
- **ML rigor** — honest evaluation (PR-AUC over accuracy on imbalanced data), calibration,
  decisions driven by *business cost*, and drift monitoring (`churn-risk-service`).
- **LLM applications** — retrieval quality measured (recall@k / MRR), grounded generation with
  citations and refusal, and an LLM-as-judge calibrated against humans (`rag-knowledge-assistant`).
- **Observability & evaluation** — the data model and tooling to trace, score, and catch
  regressions in LLM systems before users do (`llm-observatory`).
- **Agentic systems** — a planner/executor agent that is *bounded* (guardrails), *safe*
  (human-in-the-loop for destructive actions), *observable*, and *measured* (`agentic-workflow`).
- **Platform & engineering discipline** — a reusable production baseline and a design pattern
  applied everywhere: **pluggable protocol + deterministic CI-safe default + optional real/LLM
  drop-in**, so every system runs and is tested with zero external dependencies.

---

## Engineering bar (every repo)

- `uv` for packaging/envs · `ruff` lint+format · `pytest` · `pre-commit` · GitHub Actions CI · Python 3.13 · `src/` layout
- Deterministic, offline-by-default: mocks / TF-IDF / heuristic planners for CI; real APIs and LLMs behind the same interfaces
- Honest evals with real numbers (and, where it matters, a documented failure the eval *catches*)
- README-as-product: problem framing, architecture, quickstart, and a runnable demo in each

---

## About

I'm a technical program leader (9+ years) working at the intersection of AI products and
engineering. This portfolio is where I keep my hands on the tools — building the systems I help
teams ship, end to end.

**GitHub:** [@himanshutamboli](https://github.com/himanshutamboli)
