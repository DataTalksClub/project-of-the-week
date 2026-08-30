## SentinelAI

* Goal: Build an AI-powered website reliability platform end to end — polling, anomaly detection, an AI root-cause investigation, an evaluation layer for that AI output, and a distributed trace connecting all of it.
* Duration: 7 days. One day, one working slice of the system.
* Dates: from 2 September to 8 September 2026.
* Where: [`#project-of-the-week`](https://app.slack.com/client/T01ATQK62F8/C02BP4FQH36) in DataTalks.Club (get in slack here: [https://datatalks.club/slack.html](https://datatalks.club/slack.html))

For more information about the "Project of the Week" initiative
at DataTalks.Club, see [README.md](README.md).

## What You're Building

SentinelAI monitors registered websites for uptime and latency, detects failures with threshold rules, and dispatches a background AI investigation that produces a structured root-cause report. That report is then scored for groundedness and confidence calibration, with a single distributed trace connecting every hop — API → scheduler → worker → DB → LLM → eval.

## Tech Stack

| Layer | Choice | Why |
|---|---|---|
| API framework | FastAPI | Async request handling; auto-generated docs at `/docs` double as a UI when there's no time to build a dashboard |
| Validation | Pydantic v2 | Structured parsing of LLM output into a report — no regex parsing |
| Database | PostgreSQL + SQLAlchemy 2.0 | System of record for the five tables listed under Database Tables below |
| Cache / broker | Redis | Celery's broker, and a rolling-window store so anomaly checks don't hit Postgres on every poll |
| Scheduling | APScheduler (in-process) | Cheap, frequent polling ticks — no separate deployment needed at this scope |
| Background jobs | Celery | Slow/retryable LLM work, decoupled from polling so a slow LLM call never delays monitoring |
| HTTP client | httpx (async) | Polling checks with timeout handling |
| LLM integration | One provider SDK (OpenAI or Anthropic) + `instructor` or native tool-calling | Structured RCA output — deliberately not abstracted over multiple providers |
| Observability | OpenTelemetry SDK, optional Jaeger | One trace per incident spanning API → scheduler → worker → DB → LLM → eval |
| Testing | pytest, pytest-asyncio, respx | `respx` mocks httpx in checker tests |
| Containerization | Docker Compose | One file — `postgres`, `redis`, `api`, `worker`, `mock-target`, optional `jaeger` — no Kubernetes |

## Scope Cuts — What's Deliberately Not Built

| Cut | Reason |
|---|---|
| Kubernetes | Adds orchestration overhead with no payoff at this scope |
| Kafka / gRPC | Redis + Celery already cover the queueing need |
| Multi-repo split | One repo keeps the whole pipeline reviewable in one place |
| Alembic migrations | `Base.metadata.create_all` is enough while the schema is still moving |
| Multi-LLM abstraction | One provider, called directly, keeps the structured-output path simple and debuggable |
| Custom auth system | A single shared API key is enough; real auth is a stretch goal, not a blocker |
| ML-based anomaly detection | Threshold rules (consecutive failures / latency multiplier) are simple, testable, and reliable enough to trigger the AI investigation |

Each of these is a legitimate next project — see Going the Extra Mile at the end of this doc.

## New to Docker or FastAPI?

You don't need prior experience with either:

* Docker: see the Containerization row above for why this project uses Compose (one file, no Kubernetes), then follow the Docker Compose Quickstart linked under Day 1.
* FastAPI: see the API framework row above, then follow the FastAPI "First Steps" tutorial linked under Day 1 — you'll have a working endpoint in under 10 minutes.

## Repo Structure

```text
sentinelai/
  app/
    main.py
    api/            (websites.py, incidents.py, admin.py)
    core/           (config.py, telemetry.py, metrics.py)
    db/             (models.py, session.py)
    monitoring/     (scheduler.py, checker.py, anomaly.py, redis_window.py)
    workers/        (celery_app.py, tasks.py)
    ai/             (context_builder.py, investigator.py, schemas.py, prompts.py)
    eval/           (evaluator.py, metrics.py)
    mock_targets/   flaky_service.py
  tests/
  scripts/          (seed_demo.py, reset_demo.py)
  docker-compose.yml
  Dockerfile
  requirements.txt
```

## Endpoints

| Method | Path | Purpose |
|---|---|---|
| GET | `/health` | Liveness check |
| POST | `/websites` | Register a website to monitor |
| GET | `/websites` | List registered websites |
| GET | `/websites/{id}` | Get a single website |
| GET | `/websites/{id}/checks` | List recent `Check` rows for a website |
| GET | `/incidents` | List incidents |
| GET | `/incidents/{id}` | Get an incident with nested `RCAReport` + `EvalResult` |
| POST | `/admin/simulate-failure/{website_id}` | Toggle the mock target's failure mode (500s / added latency), for demo and test purposes only |
| GET | `/stats/eval-summary` | Aggregate cost/latency/score rollup across evals |

## Database Tables

* Website — id, name, url, method, expected_status, check_interval_seconds, created_at
* Check — id, website_id, timestamp, status_code, latency_ms, success, error_message, trace_id
* Incident — id, website_id, opened_at, closed_at, status, severity, trigger_reason
* RCAReport — id, incident_id, summary, root_cause, confidence, impact, remediation, evidence_json, context_snapshot_json, model_name, prompt_tokens, completion_tokens, cost_usd, latency_ms, created_at
* EvalResult — id, rca_report_id, groundedness_score, hallucination_flag, confidence_calibration_score, judge_model, eval_tokens, eval_cost_usd, eval_latency_ms, created_at

Relationships: one `Website` has many `Check` and `Incident` rows; one `Incident` produces one `RCAReport`; one `RCAReport` is scored by one `EvalResult`.

## Plan

This is a proposed plan only, you don't have to follow it day-by-day.

### Day 1 (2 September, Wednesday)

* Set up the repo folders (`app/`, `tests/`, `scripts/`) as shown under Repo Structure above.
* Write a `docker-compose.yml` with 4 services: `postgres`, `redis`, `api`, `worker` (the worker can be a placeholder for now).
* Build the smallest possible FastAPI app with one route: `GET /health` returning `{"status": "ok"}`.
* Define the 5 database tables listed under Database Tables above as SQLAlchemy models. Don't worry about migrations — `Base.metadata.create_all` is enough for this project.
* Define the Pydantic schemas you'll need later for RCA and eval data.
* Wire up OpenTelemetry with the console exporter and print one dummy span, just to prove the plumbing works.
* Create a GitHub repository.
* Share your progress in Slack and on social media.

Why: see the API framework, Database, and Containerization rows in Tech Stack above, plus Alembic migrations in Scope Cuts — it explains why you can skip migrations.

Suggested material:

* 🗒️ [Docker Compose Quickstart](https://docs.docker.com/compose/gettingstarted/)
* 🗒️ [FastAPI — First Steps](https://fastapi.tiangolo.com/tutorial/first-steps/)
* 🗒️ [SQLAlchemy 2.0 — ORM Quickstart](https://docs.sqlalchemy.org/en/20/orm/quickstart.html)
* 🗒️ [Pydantic — Models](https://docs.pydantic.dev/latest/concepts/models/)
* 🗒️ [OpenTelemetry Python — Getting Started](https://opentelemetry.io/docs/languages/python/getting-started/)

Found good materials? Create a PR with links!

End of day: `docker compose up` brings up all 4 containers, `curl localhost:8000/health` returns 200, and one span prints to your terminal.

### Day 2 (3 September, Thursday)

* Add `POST /websites` and `GET /websites` so you can register a site to monitor.
* Build a background polling job with APScheduler that checks every registered site on a timer.
* Use `httpx` (async) to make the actual HTTP request to each site and record status code + latency.
* Write each poll result to the `Check` table in Postgres.
* Keep a rolling window of recent results per site in Redis, so anomaly checks don't need to hit Postgres on every tick.
* Commit your changes and push them to GitHub.
* Share your progress in Slack and on social media.

Why: see the Scheduling, HTTP client, and Cache / broker rows in Tech Stack above — a poll turns into a `Check` row in Postgres and an update to the Redis window on every tick.

Suggested material:

* 🗒️ [APScheduler — User Guide](https://apscheduler.readthedocs.io/en/3.x/userguide.html)
* 🗒️ [httpx — Async Support](https://www.python-httpx.org/async/)
* 🗒️ [redis-py — Client Guide](https://redis.io/docs/latest/develop/clients/redis-py/)
* 🗒️ [RESPX — User Guide](https://lundberg.github.io/respx/guide/) (for mocking HTTP calls in tests)

Found good materials? Create a PR with links!

End of day: register a real URL and watch `Check` rows accumulate in the database over a couple of minutes.

### Day 3 (4 September, Friday)

* Write the anomaly rule: open an incident if a site has 3+ consecutive failures, or if latency spikes past some multiple of its rolling average.
* Build the Incident open/resolve lifecycle. Guard against opening a second incident while one's already open for the same site.
* Build a tiny second FastAPI app (`mock_targets/flaky_service.py`) that you can toggle into a "broken" mode on demand.
* Add `POST /admin/simulate-failure/{website_id}` (see Endpoints above) to flip that toggle for demos and tests.
* Commit your changes and push them to GitHub.
* Share your progress in Slack and on social media.

Why: see ML-based anomaly detection in Scope Cuts above — a simple threshold rule is the right call here, not a shortcut around a "real" version.

Suggested material:

* 🗒️ [FastAPI — First Steps](https://fastapi.tiangolo.com/tutorial/first-steps/) (you're just running a second small instance of the same app)

Found good materials? Create a PR with links!

End of day: toggling the mock target reliably produces exactly one `Incident` row within one polling interval.

### Day 4 (5 September, Saturday)

* Define `InvestigationContext` as a Pydantic model: recent checks, latency trend, prior incidents, synthetic logs from the mock target.
* Write a Celery task `investigate_incident` that builds that context and calls the LLM.
* Force the LLM to return data that matches your `RCAReport` Pydantic schema (fields listed under Database Tables above). Don't regex-parse free text.
* Save the RCA and the exact context you sent it, side by side, so the reasoning is reproducible later.
* Commit your changes and push them to GitHub.
* Share your progress in Slack and on social media.

Why: see Background jobs and LLM integration in Tech Stack above, and Multi-LLM abstraction in Scope Cuts — pick one provider and don't build a switch for others.

Suggested material:

* 🗒️ [Claude — Tool use overview](https://platform.claude.com/docs/en/agents-and-tools/tool-use/overview) (structured output via tool calling)
* 🗒️ [OpenAI — Structured Outputs guide](https://platform.openai.com/docs/guides/structured-outputs/how-to-use)
* 🗒️ [Instructor — Start Here](https://python.useinstructor.com/start-here/) (works with either provider, built on Pydantic)
* 🗒️ [Celery — Getting Started](https://docs.celeryq.dev/en/stable/getting-started/introduction.html)

Found good materials? Create a PR with links!

End of day: trigger a failure and, within seconds, get a full structured RCA report saved to the database.

### Day 5 (6 September, Sunday)

* Write a groundedness heuristic: for every evidence claim in the RCA, check it against a field that actually exists in the stored context snapshot.
* Add a second LLM call — a judge — that scores correctness, flags hallucinations, and checks whether the model's stated confidence matches how well-supported its answer actually is.
* Chain `evaluate_rca` after `investigate_incident` so every RCA automatically gets scored.
* Track cost and latency on both calls.
* Commit your changes and push them to GitHub.
* Share your progress in Slack and on social media.

Why: two independent signals beat one confidence number — a cheap, deterministic groundedness check catches unsupported claims, and a second LLM call catches things the first model got wrong about itself. This day's test suite is the most important one in the whole project, so budget real time for hand-crafted "good" and "bad" RCA fixtures.

Suggested material:

* 🗒️ [Celery — Canvas (chaining tasks)](https://docs.celeryq.dev/en/stable/userguide/canvas.html)

Found good materials? Create a PR with links!

End of day: every incident's RCA has a groundedness score and a judge score attached; a deliberately fabricated RCA is correctly flagged by the groundedness check.

### Day 6 (7 September, Monday)

* Turn on auto-instrumentation for FastAPI, SQLAlchemy, Celery, and httpx.
* Add custom spans: `anomaly.detect`, `context.build`, `llm.investigate`, `llm.evaluate`.
* Add custom metrics: `checks_total`, `incidents_total`, `llm_tokens_total`, `llm_cost_total_usd`, `eval_score`, `detection_latency_seconds`.
* If you have time, spin up Jaeger and look at one incident's trace end to end.
* No new features today — this is a stabilization day. Resist scope creep.
* Commit your changes and push them to GitHub.
* Share your progress in Slack and on social media.

Why: see Observability in Tech Stack above — the goal is one trace per incident spanning every hop: API → scheduler → worker → DB → LLM → eval.

Suggested material:

* 🗒️ [OpenTelemetry Python — Getting Started](https://opentelemetry.io/docs/languages/python/getting-started/)
* 🗒️ [Jaeger — Getting Started](https://www.jaegertracing.io/docs/latest/getting-started/)

Found good materials? Create a PR with links!

End of day: one browsable trace shows every hop's latency for a single incident, LLM call included.

### Day 7 (8 September, Tuesday)

* Freeze features. Bug fixes only from this point.
* Write `scripts/seed_demo.py` and `scripts/reset_demo.py` so you can reset to a clean state between runs.
* Write and rehearse a 5–10 minute demo script.
* Record a screen capture of one full successful run, as a fallback in case the live demo breaks.
* Polish your README.
* Continue exploring more about this topic.
* Push your changes to GitHub.
* Share your progress in Slack and on social media.
* Give us feedback.
* Add the link to your project to this project of the week GitHub page.

Why: no new tech today — this is the day to reread everything you built and confirm it matches what you're about to demo.

Suggested material:

* 🗒️ [pytest docs](https://docs.pytest.org/en/stable/) (for the integration test you'll run one last time before demoing)

Found good materials? Create a PR with links!

End of day: two different people on your team can run the demo successfully back to back.

## Going the Extra Mile

Finished all 7 days and want to keep building? Every row in Scope Cuts above is a legitimate next step:

* Add Alembic migrations — replace `Base.metadata.create_all` with real, versioned migrations.
* Add real auth — swap the shared API key for per-user accounts and JWTs.
* Try ML-based anomaly detection — replace the threshold rule with an actual model (e.g. isolation forest, seasonal decomposition) and compare its false-positive rate against the simple rule.
* Support multiple LLM providers — build the abstraction layer that was skipped, and compare RCA quality, cost, and latency across providers on the same incidents.
* Move to Kubernetes — if you want the orchestration experience, this is a natural target once Docker Compose feels too small.

Beyond that list, a few more directions:

* Build a dashboard — a small Next.js or Streamlit UI over the existing endpoints, so `/docs` isn't your only UI.
* Wire up real alerting — send incidents to Slack, PagerDuty, or email instead of only showing them in the API.
* Add dependency graphs — model which sites/services depend on each other, so an RCA can reason about upstream failures.
* Try ensemble evals — run two or three different judge models and compare agreement, instead of trusting a single judge.
* Track LLM spend — add a running cost budget and alert when a day's investigations exceed it.
* Multi-region checks — poll each site from more than one location and compare results, to catch regional outages a single-region check would miss.

## FAQ

### Do I need to know Docker or FastAPI before starting?

No. Day 1 is designed to teach you both as you build. If you get stuck, the linked tutorials cover the exact commands and concepts you'll need that day.

### Can I use OpenAI instead of Anthropic (or vice versa)?

Yes. The plan deliberately calls for "one provider SDK" — pick whichever you have API access to. The structured-output approach (native tool-calling or `instructor`) works with either.

### What if I fall behind — can I combine days?

Yes. The plan is a suggested pace, not a hard schedule. Days 6 and 7 are the easiest to compress if you're short on time, since Day 6 is instrumentation-only and Day 7 adds no new features.

### Do I need Kubernetes, Alembic, or real auth to finish this?

No — all three are explicitly out of scope for the 7-day build (see Scope Cuts above). They're good "Going the Extra Mile" projects, not blockers.

### My LLM calls are getting expensive while I test — what do I do?

Mock the LLM client in your tests and only call the real API when manually triggering a failure to check the RCA output. CI should never call the real API.

### Can I swap out Postgres, Redis, or Celery for something else?

You can, but the plan assumes this exact stack — swapping a piece means adapting the day-by-day tasks yourself, since they reference these tools directly.

### Where do I find the database schema or endpoint contracts?

See Database Tables and Endpoints above.

## Materials legend

* 🗒️ Article
