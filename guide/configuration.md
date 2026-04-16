# Configuration

This page focuses on the runtime details developers actually need.

## Where Configuration Comes From

The backend loads `.env` from the **project root**:

```text
MiroFish/.env
```

This is important:
placing `.env` under `backend/` is the wrong path for the default backend loader.

## Required Environment Variables

| Variable | Required | Default | What it controls |
| --- | --- | --- | --- |
| `LLM_API_KEY` | Yes | none | Credentials for your OpenAI-compatible LLM provider |
| `LLM_BASE_URL` | No | `https://api.openai.com/v1` | Base URL for model calls |
| `LLM_MODEL_NAME` | No | `gpt-4o-mini` | Default model name |
| `ZEP_API_KEY` | Yes | none | Zep Cloud graph memory access |

## Optional Runtime Variables

| Variable | Default | Why you would change it |
| --- | --- | --- |
| `LLM_BOOST_API_KEY` | unset | Optional accelerated LLM path if your deployment uses it |
| `LLM_BOOST_BASE_URL` | unset | Optional accelerated base URL |
| `LLM_BOOST_MODEL_NAME` | unset | Optional accelerated model |
| `SECRET_KEY` | `mirofish-secret-key` | Replace in any serious deployment |
| `FLASK_DEBUG` | `True` | Set to `False` for production |
| `FLASK_HOST` | `0.0.0.0` | Bind address |
| `FLASK_PORT` | `5001` | Backend port |
| `OASIS_DEFAULT_MAX_ROUNDS` | `10` | Default simulation round count |
| `REPORT_AGENT_MAX_TOOL_CALLS` | `5` | Report agent tooling budget |
| `REPORT_AGENT_MAX_REFLECTION_ROUNDS` | `2` | Reflection depth for report generation |
| `REPORT_AGENT_TEMPERATURE` | `0.5` | Report agent creativity / variance |
| `VITE_API_BASE_URL` | `http://localhost:5001` | Frontend API target when not using the default local layout |

## Upload and Request Limits

Backend defaults from `backend/app/config.py`:

- max upload size: `50 MB`
- allowed extensions: `pdf`, `md`, `txt`, `markdown`
- default chunk size: `500`
- default chunk overlap: `50`

Frontend default request timeout from `frontend/src/api/index.js`:

- `300000 ms` or 5 minutes

That means long-running operations should be treated as async flows and polled through status endpoints, not as one big blocking request.

## Storage Layout That Matters

MiroFish persists most important artifacts under:

```text
backend/uploads/
  projects/
    <project_id>/
      project.json
      extracted_text.txt
      files/
  simulations/
    <simulation_id>/
      state.json
      simulation_config.json
      reddit_profiles.json
      twitter_profiles.csv
      ...
```

Practical implications:

- deleting `backend/uploads/` wipes local state,
- Docker users should persist that directory,
- project and simulation artifacts survive restarts,
- in-memory task progress does not.

## Task Persistence Caveat

The task manager is in-memory only.

That means:

- graph build progress,
- preparation progress,
- report generation progress

can lose live status if the backend restarts mid-job.

Some files may already be written to disk, but you should not assume the async task state itself is durable.

## Frontend / Backend Networking

Local defaults:

- Vite frontend on `3000`
- Flask backend on `5001`
- Vite proxies `/api` to `5001`

If you split frontend and backend across hosts, set `VITE_API_BASE_URL` before building or running the frontend.

## Sensible First Defaults

For the first self-hosted run:

- keep `FLASK_DEBUG=True` locally only,
- keep rounds low,
- use one model/provider path only,
- do not enable optional acceleration until the plain path works,
- verify `/health` before opening the UI.
