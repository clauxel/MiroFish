# API Recipes

This page is for developers who want to drive MiroFish without clicking through the UI.

All examples assume:

```bash
API=http://localhost:5001
SEED=./seed.md
```

If `jq` is not available on your machine, copy IDs manually from the JSON responses.

## 0. Health Check

```bash
curl "$API/health"
```

Healthy response:

```json
{
  "status": "ok",
  "service": "MiroFish Backend"
}
```

## 1. Generate Ontology From Seed Material

```bash
curl -X POST "$API/api/graph/ontology/generate" \
  -F "files=@$SEED" \
  -F "simulation_requirement=Simulate how the main stakeholder groups react over the next 10 rounds and highlight likely turning points." \
  -F "project_name=demo-public-opinion"
```

Healthy response shape:

```json
{
  "success": true,
  "data": {
    "project_id": "proj_xxxx",
    "project_name": "demo-public-opinion",
    "ontology": {
      "entity_types": [],
      "edge_types": []
    },
    "analysis_summary": "...",
    "files": [],
    "total_text_length": 12345
  }
}
```

Save the `project_id` for the next steps.

## 2. Build the Graph

```bash
curl -X POST "$API/api/graph/build" \
  -H "Content-Type: application/json" \
  -d '{
    "project_id": "proj_xxxx",
    "graph_name": "demo-public-opinion"
  }'
```

Healthy response shape:

```json
{
  "success": true,
  "data": {
    "project_id": "proj_xxxx",
    "task_id": "task_xxxx",
    "message": "..."
  }
}
```

Poll the graph task:

```bash
curl "$API/api/graph/task/task_xxxx"
```

A successful completed task typically contains:

- `status: "completed"`
- `result.graph_id`
- `result.node_count`
- `result.edge_count`

## 3. Create a Simulation

Once the project has a graph:

```bash
curl -X POST "$API/api/simulation/create" \
  -H "Content-Type: application/json" \
  -d '{
    "project_id": "proj_xxxx",
    "enable_twitter": true,
    "enable_reddit": true
  }'
```

Healthy response shape:

```json
{
  "success": true,
  "data": {
    "simulation_id": "sim_xxxx",
    "project_id": "proj_xxxx",
    "graph_id": "mirofish_xxxx",
    "status": "created",
    "enable_twitter": true,
    "enable_reddit": true
  }
}
```

## 4. Prepare the Simulation

```bash
curl -X POST "$API/api/simulation/prepare" \
  -H "Content-Type: application/json" \
  -d '{
    "simulation_id": "sim_xxxx",
    "parallel_profile_count": 5,
    "use_llm_for_profiles": true,
    "force_regenerate": false
  }'
```

Possible healthy outcomes:

- a new async task with `status: "preparing"`
- an immediate ready-style response with `already_prepared: true`

Poll the prepare status with `POST`, not query params:

```bash
curl -X POST "$API/api/simulation/prepare/status" \
  -H "Content-Type: application/json" \
  -d '{
    "task_id": "task_xxxx",
    "simulation_id": "sim_xxxx"
  }'
```

Healthy result signals:

- `status: "ready"` or a completed task
- `already_prepared: true` for reused preparations
- `prepare_info` or task `result` showing generated artifacts

On disk you should eventually see:

```text
backend/uploads/simulations/sim_xxxx/
  state.json
  simulation_config.json
  reddit_profiles.json
  twitter_profiles.csv
```

## 5. Start the Simulation

For a conservative first run:

```bash
curl -X POST "$API/api/simulation/start" \
  -H "Content-Type: application/json" \
  -d '{
    "simulation_id": "sim_xxxx",
    "platform": "parallel",
    "max_rounds": 12,
    "enable_graph_memory_update": false
  }'
```

Useful optional field:

- `force: true` to stop and restart a wedged or previously finished run

Healthy response signals:

- `success: true`
- a run-state payload under `data`
- `max_rounds_applied` if you passed `max_rounds`

## 6. Inspect the Running Simulation

Run status:

```bash
curl "$API/api/simulation/sim_xxxx/run-status"
```

Detailed status:

```bash
curl "$API/api/simulation/sim_xxxx/run-status/detail"
```

Timeline:

```bash
curl "$API/api/simulation/sim_xxxx/timeline?start_round=0"
```

Agent stats:

```bash
curl "$API/api/simulation/sim_xxxx/agent-stats"
```

Posts:

```bash
curl "$API/api/simulation/sim_xxxx/posts?platform=reddit&limit=20&offset=0"
```

Comments:

```bash
curl "$API/api/simulation/sim_xxxx/comments?platform=reddit&limit=20&offset=0"
```

Healthy run signals:

- the run status advances,
- actions or posts increase over time,
- timeline returns non-empty rounds,
- agent stats are populated.

## 7. Generate the Report

```bash
curl -X POST "$API/api/report/generate" \
  -H "Content-Type: application/json" \
  -d '{
    "simulation_id": "sim_xxxx",
    "force_regenerate": false
  }'
```

Healthy response shape:

```json
{
  "success": true,
  "data": {
    "simulation_id": "sim_xxxx",
    "report_id": "report_xxxx",
    "task_id": "task_xxxx",
    "status": "generating"
  }
}
```

Poll report generation with `POST`:

```bash
curl -X POST "$API/api/report/generate/status" \
  -H "Content-Type: application/json" \
  -d '{
    "task_id": "task_xxxx",
    "simulation_id": "sim_xxxx"
  }'
```

Healthy result signals:

- `status: "completed"` directly
- or task progress moving toward completion
- or `already_completed: true` if a completed report already exists

## 8. Read and Download the Report

Find the report by simulation:

```bash
curl "$API/api/report/by-simulation/sim_xxxx"
```

Read full report:

```bash
curl "$API/api/report/report_xxxx"
```

Read generated sections:

```bash
curl "$API/api/report/report_xxxx/sections"
```

Download markdown:

```bash
curl -L "$API/api/report/report_xxxx/download" -o report_xxxx.md
```

## 9. Chat With the Report Agent

```bash
curl -X POST "$API/api/report/chat" \
  -H "Content-Type: application/json" \
  -d '{
    "simulation_id": "sim_xxxx",
    "message": "Explain the biggest turning point in the simulation.",
    "chat_history": []
  }'
```

Healthy response shape:

```json
{
  "success": true,
  "data": {
    "response": "...",
    "tool_calls": [],
    "sources": []
  }
}
```

## 10. Interview Simulated Agents

Only do this after the environment is running.

Check the environment first:

```bash
curl -X POST "$API/api/simulation/env-status" \
  -H "Content-Type: application/json" \
  -d '{
    "simulation_id": "sim_xxxx"
  }'
```

Interview one agent:

```bash
curl -X POST "$API/api/simulation/interview" \
  -H "Content-Type: application/json" \
  -d '{
    "simulation_id": "sim_xxxx",
    "agent_id": 0,
    "prompt": "What changed your position most during the simulation?",
    "platform": "twitter"
  }'
```

## Common Failure Patterns

| Symptom | Most likely cause | Fastest fix |
| --- | --- | --- |
| ontology generation returns 400 | missing `simulation_requirement` or file upload | send both multipart fields |
| graph build returns 400 | no `project_id`, missing extracted text, or ontology missing | re-run ontology generation first |
| prepare never reaches ready | provider latency, large seed, or bad scenario scope | reduce scope and try again |
| report status polling fails | wrong HTTP method | use `POST /api/report/generate/status` |
| report is weak | the simulation itself was weak | inspect run outputs before regenerating the report |

## What To Automate First

If you are building internal tooling, automate in this order:

1. health check
2. ontology generation
3. graph build + polling
4. simulation create + prepare + polling
5. simulation run inspection
6. report generation + polling
7. report chat
