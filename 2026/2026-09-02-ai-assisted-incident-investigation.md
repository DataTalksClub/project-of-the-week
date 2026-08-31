## AI-Assisted Incident Investigation

* **Goal**: Build a small website monitor, add incident detection, and use an LLM to investigate incidents from saved evidence.
* **Dates**: from 2 September to 8 September 2026.
* **Where:** [`#project-of-the-week`](https://app.slack.com/client/T01ATQK62F8/C02BP4FQH36) in DataTalks.Club (get in slack here: [https://datatalks.club/slack.html](https://datatalks.club/slack.html))

For more information about the "Project of the Week" initiative
at DataTalks.Club, see [README.md](../README.md).

## What You're Building

You'll build a small website monitor that records uptime and latency. When recent checks look unhealthy, it opens an incident, and an LLM investigates the saved evidence. You'll then evaluate whether the report's claims are supported and whether its recommended action is useful.

Start with the app, not the model. You need real requests, stored results, logs or metrics, and a repeatable way to make a site unhealthy before the AI has something to explain. By the end of the week, you should be able to follow one incident from first check to evaluated report.

## Technologies

* Python, FastAPI, and SQLite.
* Tables for sites, checks, incidents, reports, and evaluations.
* Request logs and response-duration metrics.
* The OpenAI API.
* pytest.

Use FastAPI background tasks for checks and investigations.

## Plan

This is a proposed plan only, and you don't have to follow it day-by-day.

### Day 1 - Skeleton and Contracts (2 September, Wednesday)

* Set up the smallest runnable app with a health endpoint.
* Define how you'll represent sites, checks, incidents, reports, and evaluations.
* Design a simple way to register a site and check that the app is running.
* Add basic application logging so you can reconstruct what happened.
* Create a GitHub project and share your progress in Slack and on social media.

Suggested material:

* 🗒️ [DIY FastAPI](../2022/2022-12-07-fastapi.md)
* 🗒️ [SQLite documentation](https://sqlite.org/docs.html)

### Day 2 - Monitoring Engine (3 September, Thursday)

* Add site registration and list the registered sites.
* Check each site in the background for status and response duration.
* Store every check with enough metadata to review it later.
* Trigger real checks against a URL and watch results accumulate.
* Commit and push your changes, then share your progress.

Suggested material:

* 🗒️ [FastAPI — Background Tasks](https://fastapi.tiangolo.com/tutorial/background-tasks/)

### Day 3 - Incidents and a Breakable Target (4 September, Friday)

* Add a repeatable way to make a target unhealthy.
* Turn consecutive failures or a latency spike into an open incident.
* Let users list and resolve incidents.
* Prevent duplicate active incidents for the same site.
* Commit and push your changes, then share your progress.

Suggested material:

* 🗒️ [Google SRE — Alerting on SLOs](https://sre.google/workbook/alerting-on-slo/)

### Day 4 - Context and AI Investigation (5 September, Saturday)

* Build a bounded context from recent checks, response trends, and any relevant logs or metadata.
* Add one investigation step that prepares that context and calls the model without blocking monitoring.
* Ask for a structured incident report with a likely cause, evidence references, and a recommended action.
* Save the exact context, response, model metadata, latency, cost, and errors.
* Commit and push your changes, then share your progress.

Suggested material:

* 🗒️ [OpenAI — Structured Outputs](https://platform.openai.com/docs/guides/structured-outputs)
* 🗒️ [OpenAI — Prompt engineering](https://platform.openai.com/docs/guides/prompt-engineering)

### Day 5 - Evaluation Layer (6 September, Sunday)

* Create known-good and known-bad reports for testing.
* Check whether evidence references exist in the saved context.
* Add a second evaluation, such as an LLM judge or human review.
* Run evaluation after investigation, and store every score with the report.
* Commit and push your changes, then share your progress.

Suggested material:

* 🗒️ [OpenAI — Evaluations](https://platform.openai.com/docs/guides/evals)

### Day 6 - Observability and Testing (7 September, Monday)

* Add timeouts and graceful handling for external calls.
* Add tests for monitoring, incident detection, context building, report parsing, and evaluation.
* Use fakes or mocks for external and model calls.
* Add logs, metrics, or traces where they help you debug the whole run.
* Commit and push your changes, then share your progress.

Suggested material:

* 🗒️ [FastAPI — Testing](https://fastapi.tiangolo.com/tutorial/testing/)
* 🗒️ [pytest](https://docs.pytest.org/en/stable/)

### Day 7 - Demo and Buffer (8 September, Tuesday)

* Continue exploring more about this topic.
* Polish the documentation for your project.
* Add a simple way to reset the app for a clean demo.
* Rehearse a short demo of monitoring, incident detection, investigation, and evaluation.
* Push your changes to GitHub.
* Share your progress in Slack and on social media.
* Give us feedback.
* Add the link to your project to this project of the week GitHub page.

## Other Things

There are other things you can try:

* Build a small dashboard for sites, incidents, and reports.
* Send incident notifications to Slack, email, or another destination.
* Compare prompts, context sizes, or models on the same incidents.
* Let the investigation run read-only diagnostic tools before it writes its report.
* Track model cost, latency, and report quality over time.
* Add tracing across monitoring, investigation, and evaluation.

## Projects

List of projects from our participants:

* Project link 1
* Project link 2
* ...
* (Create a PR)

(We'll put the projects here after the event finishes)
