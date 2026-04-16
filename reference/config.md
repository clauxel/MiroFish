# Config Reference

This page is the quick-reference companion to [guide/configuration.md](../guide/configuration.md).

## Environment Variables

| Variable | Required | Default | Notes |
| --- | --- | --- | --- |
| `LLM_API_KEY` | Yes | none | Required for all model calls |
| `LLM_BASE_URL` | No | `https://api.openai.com/v1` | Use your OpenAI-compatible provider endpoint |
| `LLM_MODEL_NAME` | No | `gpt-4o-mini` | Upstream README recommends `qwen-plus` as a practical starting point |
| `ZEP_API_KEY` | Yes | none | Required for Zep graph memory |
| `LLM_BOOST_API_KEY` | No | unset | Optional accelerated path |
| `LLM_BOOST_BASE_URL` | No | unset | Optional accelerated path |
| `LLM_BOOST_MODEL_NAME` | No | unset | Optional accelerated path |
| `SECRET_KEY` | No | `mirofish-secret-key` | Replace outside local dev |
| `FLASK_DEBUG` | No | `True` | Disable for production |
| `FLASK_HOST` | No | `0.0.0.0` | Backend bind host |
| `FLASK_PORT` | No | `5001` | Backend port |
| `OASIS_DEFAULT_MAX_ROUNDS` | No | `10` | Default rounds |
| `REPORT_AGENT_MAX_TOOL_CALLS` | No | `5` | Report-agent tool budget |
| `REPORT_AGENT_MAX_REFLECTION_ROUNDS` | No | `2` | Report reflection depth |
| `REPORT_AGENT_TEMPERATURE` | No | `0.5` | Report-agent temperature |
| `VITE_API_BASE_URL` | No | `http://localhost:5001` | Frontend API base URL |

## Ports

| Port | Service |
| --- | --- |
| `3000` | Vite frontend |
| `5001` | Flask backend |

## Upload Limits

| Setting | Value |
| --- | --- |
| Max upload size | `50 MB` |
| Allowed extensions | `pdf`, `md`, `txt`, `markdown` |

## Storage Paths

| Path | What lives there |
| --- | --- |
| `backend/uploads/projects/` | project metadata, uploaded files, extracted text |
| `backend/uploads/simulations/` | simulation state, config, profiles, run artifacts |

## Non-Obvious Defaults

- backend `.env` path is project root, not `backend/`
- task progress is in memory
- CORS is open for `/api/*`
- frontend request timeout is 5 minutes
