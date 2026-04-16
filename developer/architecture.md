# Architecture

This page is for developers who want to understand how the upstream project is put together before customizing it.

## High-Level Stack

| Layer | Technology | Responsibility |
| --- | --- | --- |
| Frontend | Vue 3 + Vite + Vue Router + Axios | UI flow, API calls, visualization |
| Backend | Flask + Flask-CORS | REST endpoints, orchestration, file handling |
| Simulation engine | `camel-oasis` + `camel-ai` | Agent simulation and social-environment logic |
| Memory layer | Zep Cloud | graph memory and entity retrieval |
| Packaging | npm at repo root + `uv` for backend | local development and dependency install |

## Upstream Repository Shape

Relative to the upstream `MiroFish` repo root:

```text
frontend/
  src/
    api/
    components/
    views/
backend/
  app/
    api/
    models/
    services/
    utils/
  scripts/
locales/
package.json
docker-compose.yml
```

## Request Flow

The normal flow is:

1. frontend sends requests through `frontend/src/api/*.js`
2. Flask blueprints under `backend/app/api/` receive them
3. services under `backend/app/services/` generate graphs, profiles, runs, and reports
4. artifacts are written to `backend/uploads/`
5. frontend polls status endpoints and renders progress/results

## API Routing Layout

The backend registers three blueprint groups:

- `/api/graph`
- `/api/simulation`
- `/api/report`

There is also a plain `/health` route for backend health checks.

## Persistence Model

Durable-ish local state:

- project files and metadata
- simulation state and generated config
- report artifacts written to disk

Non-durable runtime state:

- task progress tracking

This is why backend restarts are more disruptive to in-flight jobs than to completed artifacts.

## Frontend Structure Worth Knowing

The frontend is organized around the product workflow:

- graph build
- environment setup
- simulation run
- report
- interaction

That is useful when you want to add custom UI around one stage without touching everything else.

## Extension Points

The most realistic customization points are:

- swap the LLM provider by changing the OpenAI-compatible base URL and model name
- add deployment controls around the existing API
- build your own frontend on top of the Flask routes
- adjust prompt and simulation behavior in backend services

## Risks To Keep In Mind

- default security posture is for development, not production
- long-running tasks depend on external model latency and quotas
- statefulness lives in `backend/uploads/`, so treat it as a persistent volume
- API/auth governance is your responsibility in self-hosted environments
