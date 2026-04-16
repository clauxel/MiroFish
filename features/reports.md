# Reports

Reports are where MiroFish turns simulation output into something developers, analysts, or stakeholders can actually use.

## What The Report Stage Does

The report stage:

- reads the finished simulation state,
- analyzes posts, comments, actions, and timeline,
- produces a structured markdown report,
- exposes sections and logs for deeper inspection,
- enables follow-up chat with a report agent.

## Main API Calls

| Endpoint | Purpose |
| --- | --- |
| `POST /api/report/generate` | Start report generation |
| `POST /api/report/generate/status` | Poll generation progress |
| `GET /api/report/<report_id>` | Fetch report details |
| `GET /api/report/<report_id>/download` | Download markdown output |
| `POST /api/report/chat` | Ask the report agent follow-up questions |
| `GET /api/report/<report_id>/sections` | Read generated sections |
| `GET /api/report/<report_id>/agent-log` | Inspect incremental report-agent logs |
| `GET /api/report/<report_id>/console-log` | Inspect console-side logs |

## What Good Report Usage Looks Like

Generate the report only after you trust the simulation.

Then use the report stage to answer:

- what changed over time,
- which factions mattered,
- what the most likely trajectory is,
- what intervention points changed the outcome.

## Follow-Up Chat

Report chat is most useful when you ask targeted questions such as:

- "Summarize the biggest turning point."
- "Which actors had the strongest influence?"
- "What evidence supports the final recommendation?"
- "Compare the final state to the midpoint."

## Logs Matter More Than People Think

If report quality looks wrong, check:

- report status,
- generated sections,
- agent log,
- console log.

Those often tell you whether the problem is:

- missing simulation data,
- a weak scenario,
- a model/provider problem,
- or a genuine report-stage bug.

## Regenerate or Not?

Use `force_regenerate` only when:

- the underlying simulation changed,
- the previous report is clearly stale or corrupted,
- you intentionally want a fresh run after upstream fixes.

Do not repeatedly regenerate a bad scenario and hope the report fixes it.
