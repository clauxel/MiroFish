# HTTP API Reference

This is the practical API reference most developers need.

Base URL in the default local setup:

```text
http://localhost:5001
```

Important note:
MiroFish does not expose built-in API auth in the upstream backend routes. Treat the API as internal unless you add your own access control layer.

## Common Response Envelope

Most backend endpoints return JSON in one of these shapes:

```json
{
  "success": true,
  "data": {}
}
```

```json
{
  "success": false,
  "error": "..."
}
```

For copy-paste examples, see [api-recipes.md](api-recipes.md).

## Health

| Method | Path | Use it for |
| --- | --- | --- |
| `GET` | `/health` | Confirm the backend is alive before debugging anything else |

## Graph and Project Endpoints

| Method | Path | Use it for |
| --- | --- | --- |
| `POST` | `/api/graph/ontology/generate` | Upload seed files and generate ontology |
| `POST` | `/api/graph/build` | Build graph from the project/ontology |
| `GET` | `/api/graph/task/<task_id>` | Poll graph build task |
| `GET` | `/api/graph/project/<project_id>` | Read one project |
| `GET` | `/api/graph/project/list` | List projects |
| `DELETE` | `/api/graph/project/<project_id>` | Delete one project |
| `POST` | `/api/graph/project/<project_id>/reset` | Reset a project |
| `GET` | `/api/graph/data/<graph_id>` | Read graph data |
| `DELETE` | `/api/graph/delete/<graph_id>` | Delete graph data |

### Ontology generation payload

Verified from backend docstring and frontend client:

- request type: `multipart/form-data`
- main fields:
  - `files`
  - `simulation_requirement`
  - `project_name`
  - `additional_context`

### Graph build payload

Frontend-verified fields:

- `project_id`
- `graph_name`

## Simulation Lifecycle Endpoints

| Method | Path | Use it for |
| --- | --- | --- |
| `POST` | `/api/simulation/create` | Create a simulation record |
| `POST` | `/api/simulation/prepare` | Generate profiles and config |
| `POST` | `/api/simulation/prepare/status` | Poll preparation progress in the current frontend integration |
| `GET` | `/api/simulation/<simulation_id>` | Read one simulation |
| `GET` | `/api/simulation/list` | List simulations |
| `GET` | `/api/simulation/history` | List recent historical simulations |
| `POST` | `/api/simulation/start` | Start a run |
| `POST` | `/api/simulation/stop` | Stop a run |
| `POST` | `/api/simulation/env-status` | Check environment status |
| `POST` | `/api/simulation/close-env` | Close the environment cleanly |

### Create simulation payload

Frontend-verified fields:

- `project_id`
- `graph_id`
- `enable_twitter`
- `enable_reddit`

### Prepare simulation payload

Frontend-verified fields:

- `simulation_id`
- `entity_types`
- `use_llm_for_profiles`
- `parallel_profile_count`
- `force_regenerate`

### Start simulation payload

Frontend-verified fields:

- `simulation_id`
- `platform`
- `max_rounds`
- `enable_graph_memory_update`
- `force`

## Simulation Inspection and Interview Endpoints

| Method | Path | Use it for |
| --- | --- | --- |
| `GET` | `/api/simulation/<simulation_id>/profiles` | Read generated profiles |
| `GET` | `/api/simulation/<simulation_id>/profiles/realtime` | Read profiles while generation is still happening |
| `GET` | `/api/simulation/<simulation_id>/config` | Read simulation config |
| `GET` | `/api/simulation/<simulation_id>/config/realtime` | Read config while generation is still happening |
| `GET` | `/api/simulation/<simulation_id>/config/download` | Download generated config |
| `GET` | `/api/simulation/script/<script_name>/download` | Download generated scripts |
| `GET` | `/api/simulation/<simulation_id>/run-status` | Run status |
| `GET` | `/api/simulation/<simulation_id>/run-status/detail` | Detailed run status |
| `GET` | `/api/simulation/<simulation_id>/actions` | Action history |
| `GET` | `/api/simulation/<simulation_id>/timeline` | Timeline by rounds |
| `GET` | `/api/simulation/<simulation_id>/agent-stats` | Agent statistics |
| `GET` | `/api/simulation/<simulation_id>/posts` | Posts from the run |
| `GET` | `/api/simulation/<simulation_id>/comments` | Comments from the run |
| `POST` | `/api/simulation/interview` | Interview a single agent |
| `POST` | `/api/simulation/interview/batch` | Batch interview agents |
| `POST` | `/api/simulation/interview/all` | Interview all agents |
| `POST` | `/api/simulation/interview/history` | Read interview history |

## Report Endpoints

| Method | Path | Use it for |
| --- | --- | --- |
| `POST` | `/api/report/generate` | Start report generation |
| `POST` | `/api/report/generate/status` | Poll generation status |
| `GET` | `/api/report/<report_id>` | Read report details |
| `GET` | `/api/report/by-simulation/<simulation_id>` | Find report by simulation |
| `GET` | `/api/report/list` | List reports |
| `GET` | `/api/report/<report_id>/download` | Download markdown report |
| `DELETE` | `/api/report/<report_id>` | Delete report |
| `POST` | `/api/report/chat` | Chat with the report agent |
| `GET` | `/api/report/<report_id>/progress` | Read report progress |
| `GET` | `/api/report/<report_id>/sections` | Read all generated sections |
| `GET` | `/api/report/<report_id>/section/<section_index>` | Read one generated section |
| `GET` | `/api/report/check/<simulation_id>` | Check report status by simulation |
| `GET` | `/api/report/<report_id>/agent-log` | Incremental agent log |
| `GET` | `/api/report/<report_id>/agent-log/stream` | Stream agent log |
| `GET` | `/api/report/<report_id>/console-log` | Incremental console log |
| `GET` | `/api/report/<report_id>/console-log/stream` | Stream console log |
| `POST` | `/api/report/tools/search` | Graph-aware search tool |
| `POST` | `/api/report/tools/statistics` | Graph statistics tool |

### Report generation payload

Frontend-verified fields:

- `simulation_id`
- `force_regenerate`

### Report status payload

Backend-verified fields:

- `task_id`
- `simulation_id`

### Report chat payload

Frontend-verified fields:

- `simulation_id`
- `message`
- `chat_history`

### Async polling note

Two status endpoints are especially important for custom clients:

- `POST /api/simulation/prepare/status`
- `POST /api/report/generate/status`

For both, prefer sending a JSON body with the current `task_id`, and optionally the related `simulation_id`.

## Recommended API Order

For a custom integration, the practical order is:

1. `POST /api/graph/ontology/generate`
2. `POST /api/graph/build`
3. `GET /api/graph/task/<task_id>`
4. `POST /api/simulation/create`
5. `POST /api/simulation/prepare`
6. poll preparation status
7. `POST /api/simulation/start`
8. inspect run endpoints
9. `POST /api/report/generate`
10. `POST /api/report/generate/status`
11. `POST /api/report/chat`

## Source of Truth Note

Payload shapes above were verified against the upstream frontend API clients and backend route docstrings on 2026-04-16.

If you are building a serious integration, read the latest upstream files in:

- `frontend/src/api/*.js`
- `backend/app/api/*.py`

One specific thing to double-check in your own environment:
the preparation-status polling flow shows a small source-level mismatch between backend comments and frontend usage, so verify the live behavior before hard-coding an external client.

Also note:
the report-status route is currently registered as `POST` in the backend source, so do not assume query-string `GET` polling unless you have verified your exact deployed build.
