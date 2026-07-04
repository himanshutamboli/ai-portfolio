# GitHub Continuity Kit — restore guide

This tiny kit reconstructs the entire `~/Downloads/GitHub` workspace on any machine (macOS,
Windows, Linux). It exists because the original folder was **2.5 GB — but ~99% of that was
`.venv/` virtual environments**, which are regenerable (`uv sync`) and OS-specific (a macOS venv
won't run on Windows). None of it is worth moving.

**Everything of value is already on GitHub** (all 9 repos verified: 0 uncommitted, 0 unpushed).
So the old folder is safe to delete. This kit carries only the three things GitHub *doesn't*
hold, plus full git history as portable files.

## What's in here

| Item | What it is |
|---|---|
| `bundles/*.bundle` | Full git history of every repo, packed as self-contained files (offline restore, no GitHub needed). |
| `memory/` | The Claude Code project memory — the 45-day plan, repo progress, tooling conventions, working style. Not stored on GitHub. |
| `launch.json` | The preview/dev-server config (`.claude/launch.json`) for running the dashboards/APIs. |
| `HOWTO.md` | Day-to-day operational guide: run/test/build commands for every repo, the dev loop, and troubleshooting. |
| `RESTORE.md` | This file. |

## The repos

| Repo | Ver | What it is | Run it |
|---|---|---|---|
| [product-analytics-mini](https://github.com/himanshutamboli/product-analytics-mini) | v1.0 | Product-analytics engine (DuckDB + polars + Streamlit) | `uv run streamlit run app.py` |
| [churn-risk-service](https://github.com/himanshutamboli/churn-risk-service) | v1.0 | Calibrated churn model + FastAPI + Docker | `uv run uvicorn churn_risk_service.api:app` |
| [rag-knowledge-assistant](https://github.com/himanshutamboli/rag-knowledge-assistant) | v1.0 | Grounded RAG + streaming chat UI | `uv run uvicorn rag_knowledge_assistant.api:app --port 8100` |
| [llm-observatory](https://github.com/himanshutamboli/llm-observatory) ★ | v1.0 | LLM observability platform (traces/evals/regression/alerts) | `uv run python -m llm_observatory.demo` then `uv run streamlit run app.py` |
| [agentic-workflow](https://github.com/himanshutamboli/agentic-workflow) ★ | v1.0 | AIOps triage agent (planner/executor, guardrails, traced by llm-observatory) | `uv run agentic-workflow triage --scenario checkout` |
| [ai-product-analytics](https://github.com/himanshutamboli/ai-product-analytics) | v1.1 | GenAI product growth analytics + A/B experimentation | `uv run ai-product-analytics` then `uv run streamlit run app.py` |
| [ai-project-template](https://github.com/himanshutamboli/ai-project-template) | — | Shared uv/ruff/pytest/CI baseline | — |
| [ai-portfolio](https://github.com/himanshutamboli/ai-portfolio) | — | Portfolio capstone README | (docs) |
| [himanshutamboli](https://github.com/himanshutamboli/himanshutamboli) | — | GitHub **profile** README (folder was named `himanshutamboli-profile`) | (docs) |

## Step 1 — install tools on the new machine

- **git** — https://git-scm.com/downloads
- **uv** (Python packaging; installs Python 3.13 itself):
  - macOS/Linux: `curl -LsSf https://astral.sh/uv/install.sh | sh`
  - Windows (PowerShell): `powershell -c "irm https://astral.sh/uv/install.ps1 | iex"`

## Step 2 — get the code (pick ONE)

### Option A — clone from GitHub (simplest; needs internet)
```bash
mkdir GitHub && cd GitHub
for r in product-analytics-mini churn-risk-service rag-knowledge-assistant \
         llm-observatory agentic-workflow ai-product-analytics ai-project-template ai-portfolio; do
  git clone https://github.com/himanshutamboli/$r.git
done
git clone https://github.com/himanshutamboli/himanshutamboli.git   # the profile repo
```
On Windows PowerShell, clone each line individually (no `for` loop) or use Git Bash.

### Option B — restore from the bundles in this kit (offline; full history)
```bash
mkdir GitHub && cd GitHub
for b in /path/to/continuity-kit/bundles/*.bundle; do
  name=$(basename "$b" .bundle)
  git clone "$b" "$name"
  # re-point origin at GitHub so you can push again:
  ( cd "$name" && git remote set-url origin "https://github.com/himanshutamboli/${name/himanshutamboli-profile/himanshutamboli}.git" )
done
```

## Step 3 — rebuild each environment
Per repo (this recreates the deleted `.venv` in seconds):
```bash
cd <repo> && uv sync --dev && uv run pytest    # confirm green
```

## Step 4 — restore preview + memory (optional)
- Put `launch.json` back at `GitHub/.claude/launch.json` to use the dashboard/API preview configs.
- The `memory/` files are your Claude Code project context. On a new machine Claude keys memory
  by project path, so it won't auto-load — either keep these as reference, or drop them into the
  new machine's `~/.claude/projects/<hash-for-GitHub-dir>/memory/` once that folder exists.

## Step 5 — delete the old folder (only after Step 2/3 succeed elsewhere)
```bash
rm -rf ~/Downloads/GitHub          # macOS/Linux
# Windows:  Remove-Item -Recurse -Force $HOME\Downloads\GitHub
```

---

## Paste this into a fresh Claude Code session to resume seamlessly

> I'm Himanshu — AI Product & Technical Program Manager. You are my senior SWE / AI architect /
> hands-on mentor: build-first, learn-by-doing, ONE step at a time, wait for my confirmation,
> minimal explanation, production mindset (tests, CI, modular, no over-engineering). I run all
> terminal commands and git pushes myself (no gh CLI). Stack for every repo: uv, ruff
> (line-length 100), pytest, pre-commit, GitHub Actions CI, Python 3.13, src layout. Design
> pattern everywhere: pluggable protocol + deterministic CI-safe default + optional real/LLM
> drop-in (Anthropic model `claude-opus-4-8`). I've shipped a 7-repo AI portfolio (see
> github.com/himanshutamboli/ai-portfolio); the flagships are `llm-observatory` (LLM
> observability) and `agentic-workflow` (AIOps triage agent, instrumented by llm-observatory).
> Read the `memory/` files in this kit for full progress and conventions before we continue.
