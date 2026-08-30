## AI-Assisted Incident Investigation

* **Goal**: Build a small AI-assisted investigation workflow for failures or anomalies in a system you choose.
* **Dates**: from 2 September to 8 September 2026.
* **Where:** [`#project-of-the-week`](https://app.slack.com/client/T01ATQK62F8/C02BP4FQH36) in DataTalks.Club (get in slack here: [https://datatalks.club/slack.html](https://datatalks.club/slack.html))

For more information about the "Project of the Week" initiative
at DataTalks.Club, see [README.md](../README.md).

## What You're Building

Pick a domain and build a workflow that notices something unusual, gathers useful context, asks an LLM for a structured diagnosis, and evaluates whether that diagnosis is trustworthy.

The domain is open. You could investigate website downtime, failed CI runs, broken data pipelines, application errors, unusual user behavior, support tickets, hardware alerts, or another problem you care about. The important part is not the exact product: it is the investigation loop.

A useful version usually has five parts:

1. **Signal**: something changes, fails, slows down, or produces an anomalous result.
2. **Trigger**: your workflow decides that the signal is worth investigating.
3. **Context**: relevant logs, metrics, metadata, recent changes, or prior incidents are collected.
4. **Report**: the model returns a structured diagnosis or recommended next action.
5. **Evaluation**: you check whether the output is grounded in the supplied context and useful in practice.

## Choosing a Direction

Choose one narrow problem for the week. These are examples, not requirements:

| Direction | Example investigation |
|---|---|
| Service or website health | Detect uptime or latency problems and produce an evidence-linked diagnosis |
| CI and tests | Analyze a failed run using logs, changed files, and recent commits |
| Data pipelines | Diagnose late data, schema drift, failed jobs, or unexpected data-quality changes |
| Application errors | Group similar exceptions and suggest where to look next |
| Product or operational metrics | Investigate a sudden change in signups, conversions, traffic, or usage |

If none of these appeal to you, choose your own system. Start with one failure mode and one kind of evidence.

## Technologies

This is a suggested menu, not a fixed stack. Choose the tools you already know or want to learn:

* Backend: Python, JavaScript/TypeScript, Go, Rust, Java, or another language
* API/UI: FastAPI, Flask, Django, Express, Next.js, SvelteKit, Streamlit, CLI, or another interface
* Storage: SQLite, PostgreSQL, MySQL, MongoDB, Redis, files, or your existing datastore
* Jobs: cron, APScheduler, Celery, RQ, BullMQ, Temporal, background tasks, or no queue for a minimal version
* LLM: any hosted or local model with structured output, tool calling, JSON mode, or schema validation
* Observability: structured logs, metrics, OpenTelemetry, Langfuse, or another tracing/evaluation tool
* Testing: your language's usual test runner plus mocks or fixtures for expensive/external calls

You do not need to use every category. A CLI or small API is enough.

## Scope Guidance

Try to keep the first version small enough to demo in 5-10 minutes.

A good minimum includes:

* One source of signals or events.
* One trigger for an investigation.
* A bounded context object or prompt payload.
* One structured report saved somewhere you can inspect.
* At least one evaluation method: deterministic checks, an LLM judge, human review, or a comparison against known incidents.

Deliberately defer anything that does not make the investigation loop work: multi-tenant auth, dashboards, distributed tracing, complex schema migrations, multi-provider abstractions, and production-scale monitoring.

## Plan

This is a proposed plan only; you don't have to follow it day-by-day.

### Day 1 (2 September, Wednesday)

* Choose a domain and one concrete failure mode.
* Define what "helpful output" looks like for that failure.
* Decide the smallest signal source you can use: live events, logs, metrics, synthetic data, or a replayable fixture.
* Set up a repository and the smallest runnable app, script, or notebook.
* Share your progress in Slack and on social media.

Suggested material:

* 🗒️ [The Twelve-Factor App: Logs](https://12factor.net/logs)
* 🗒️ [OpenTelemetry — Observability Primer](https://opentelemetry.io/docs/concepts/observability-primer/)

### Day 2 (3 September, Thursday)

* Create or connect your signal source.
* Store enough metadata to reconstruct what happened later.
* Build the simplest possible list/detail view, even if it is a CLI command or JSON response.
* Add a fixture or synthetic example if you don't have real data yet.
* Commit and push your changes.
* Share your progress in Slack and on social media.

Suggested material:

* 🗒️ [OpenTelemetry — Getting Started](https://opentelemetry.io/docs/languages/js/getting-started/)
* 💻 [Awesome Synthetic Data](https://github.com/gretelai/awesome-synthetic-data)

### Day 3 (4 September, Friday)

* Decide when an investigation should run.
* Build a trigger, even if it starts as a manual command.
* Collect relevant context without sending everything to the model.
* Store or log the exact context used by the investigation.
* Commit and push your changes.
* Share your progress in Slack and on social media.

Suggested material:

* 🗒️ [Anthropic — Context engineering](https://www.anthropic.com/engineering/context-engineering)
* 🗒️ [OpenAI — Prompt engineering guide](https://platform.openai.com/docs/guides/prompt-engineering)

### Day 4 (5 September, Saturday)

* Choose one model and one way to request structured output.
* Generate a diagnosis, hypothesis, incident summary, or recommended next action.
* Save the model output with the input context, model name, token usage, latency, and any errors.
* Add a way to rerun the investigation from the saved context.
* Commit and push your changes.
* Share your progress in Slack and on social media.

Suggested material:

* 🗒️ [OpenAI — Structured Outputs](https://platform.openai.com/docs/guides/structured-outputs)
* 🗒️ [Anthropic — Tool use](https://docs.anthropic.com/en/docs/build-with-claude/tool-use/overview)
* 🗒️ [Instructor — Structured LLM outputs](https://python.useinstructor.com/)

### Day 5 (6 September, Sunday)

* Create a few examples with known good and bad outputs.
* Check whether claims are supported by the saved context.
* Evaluate usefulness, correctness, hallucination risk, confidence calibration, or another property relevant to your domain.
* Track cost and latency if you are using a paid model.
* Commit and push your changes.
* Share your progress in Slack and on social media.

Suggested material:

* 🗒️ [OpenAI — Evaluations](https://platform.openai.com/docs/guides/evals)
* 🗒️ [Langfuse — LLM evaluation](https://langfuse.com/docs/scores/overview)

### Day 6 (7 September, Monday)

* Add retries, timeouts, and graceful failures for external calls.
* Add tests for your trigger, context builder, report parsing, and evaluation.
* Add metrics, logs, or traces where they will help you debug the workflow.
* Remove anything that is not needed for the demo.
* Commit and push your changes.
* Share your progress in Slack and on social media.

Suggested material:

* 🗒️ [OpenTelemetry — Traces](https://opentelemetry.io/docs/concepts/signals/traces/)
* 🗒️ [Google SRE — Alerting on SLOs](https://sre.google/workbook/alerting-on-slo/)

### Day 7 (8 September, Tuesday)

* Continue exploring more about this topic.
* Polish the documentation for your project.
* Record or rehearse a short demo of the full investigation loop.
* Push your changes to GitHub.
* Share your progress in Slack and on social media.
* Give us feedback.
* Add the link to your project to this project of the week GitHub page.

Suggested material:

* 🗒️ [OpenTelemetry — Observability](https://opentelemetry.io/docs/)

## Going the Extra Mile

Finished the minimum and want more? Try one of these:

* Build a small dashboard for incidents and reports.
* Add alerting to Slack, email, PagerDuty, or another destination.
* Support multiple signal types and compare their effectiveness.
* Add an agentic loop that can run read-only diagnostic tools before writing its report.
* Compare two prompts, models, or context strategies on the same incidents.
* Add cost controls, rate limits, or sampling for expensive investigations.
* Store feedback from humans and use it to improve future prompts or evals.
* Add distributed tracing across your app, background jobs, model calls, and evaluation step.

## FAQ

### Do I need to build a website monitor?

No. Website monitoring is only one example. A CI failure triage tool or data-pipeline debugger can be just as good.

### Do I have to use Python, FastAPI, PostgreSQL, Redis, or Celery?

No. Use what fits your project. The plan should adapt to your tools, not the other way around.

### Do I need a real production system?

No. Synthetic events, logs from a local app, public datasets, or replayed historical failures are fine.

### Do I need an agent framework?

No. A direct LLM call with a clear prompt and structured output is enough for the first version. Add agents only if they solve a real problem.

### What if model calls are too expensive?

Use a local model, mock the model in tests, cache responses, and reserve live calls for a few curated examples.

### What should I demo at the end of the week?

Show the signal, the trigger, the context sent to the model, the generated report, and at least one evaluation result.

## Projects

List of projects from our participants:

* Project link 1
* Project link 2
* ...
* (Create a PR)

(We will put the projects here after the event finishes)
