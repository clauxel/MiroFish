# Troubleshooting

This page focuses on the failures most developers hit in the first few hours.

## Backend Exits Immediately

Likely cause:

- missing `LLM_API_KEY`
- missing `ZEP_API_KEY`

Why:
`backend/run.py` validates config before starting Flask.

Fix:

1. confirm `.env` is in the repository root,
2. fill the required keys,
3. restart the backend.

## Frontend Loads but Every API Call Fails

Likely cause:

- backend is not running,
- backend is on the wrong port,
- `VITE_API_BASE_URL` points to the wrong host,
- reverse proxy is not forwarding `/api`.

Fast checks:

```bash
curl http://localhost:5001/health
```

Then open browser devtools and confirm the failing request URL.

## Upload Is Rejected

Likely cause:

- file is larger than `50 MB`,
- file extension is unsupported,
- the file is corrupted or unreadable.

Allowed extensions:

- `pdf`
- `md`
- `txt`
- `markdown`

## Graph / Prepare / Report Looks Stuck

Likely cause:

- slow or rate-limited LLM provider,
- weak seed quality,
- a large simulation request on the first run,
- backend restart during an async task.

What to do:

1. check backend logs,
2. reduce scenario scope,
3. reduce rounds,
4. retry with a smaller seed,
5. avoid restarting during long tasks unless necessary.

## Progress Endpoint No Longer Knows About the Task

Likely cause:
the backend restarted and task state was lost.

Important detail:
task progress is tracked in memory, not durable storage.

What still may exist:

- project files,
- simulation state,
- generated config,
- partial report artifacts.

What may be gone:

- the live task record and progress percentage.

## Simulations Are Too Expensive or Too Slow

Likely cause:

- high round count,
- large seed corpus,
- expensive model choice,
- too many generated profiles at once.

Use a safer first-run profile:

- smaller seed,
- lower rounds,
- one platform,
- confirm quality before scaling.

## Docker Is Up but UI Cannot Reach Backend

Likely cause:

- ports not published,
- `.env` missing inside compose run,
- reverse proxy misrouting,
- browser hitting a stale frontend build.

Checks:

```bash
docker ps
docker logs -f mirofish
curl http://localhost:5001/health
```

## Deep Links 404 After Refresh

Likely cause:
your reverse proxy is serving the built frontend without a history-mode fallback.

Why:
the frontend router uses HTML5 history mode, not hash routing.

Fix:

- serve the built frontend through a rule like `try_files $uri $uri/ /index.html`,
- keep `/api` proxied to the backend,
- re-test a deep link such as `/report/<report_id>`.

## Report Status Polling Fails With 405 Or 404

Likely cause:
your custom client is calling the wrong method for `/api/report/generate/status`.

Important detail:
the upstream backend currently registers that route as `POST`, not `GET`.

What to do:

- send JSON to `POST /api/report/generate/status`,
- include `task_id` and/or `simulation_id`,
- if you are following an older client snippet, re-check the live backend route.

## Windows Console Output Looks Wrong

The backend already tries to force UTF-8 on Windows.

If output is still messy:

- use Windows Terminal,
- use a modern Python 3.11 or 3.12 build,
- avoid legacy console shells.

## I Need a Clean Reset

Use the app delete/reset flows when possible.

If you need a full local reset, understand the consequence first:

- `backend/uploads/projects/` stores project artifacts,
- `backend/uploads/simulations/` stores simulation artifacts.

Deleting those directories removes local state.

## When To Stop Debugging and Rebuild the Scenario

Rebuild the scenario, not the server, when:

- ontology obviously misses core actors,
- the requirement is too vague,
- the graph is dominated by irrelevant entities,
- the report is shallow even though the system ran successfully.
