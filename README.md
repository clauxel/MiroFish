# MiroFish for Developers

**Languages:** **English** | [简体中文](README.zh-CN.md) | [हिन्दी](README.hi.md) | [Español](README.es.md) | [العربية](README.ar.md) | [Français](README.fr.md) | [বাংলা](README.bn.md) | [Português](README.pt.md) | [Русский](README.ru.md) | [Bahasa Indonesia](README.id.md)

[![Stars](https://img.shields.io/github/stars/666ghj/MiroFish?style=flat&logo=github&label=Stars&color=gold)](https://github.com/666ghj/MiroFish/stargazers)
[![Forks](https://img.shields.io/github/forks/666ghj/MiroFish?style=flat&label=Forks&color=silver)](https://github.com/666ghj/MiroFish/network/members)
[![License: AGPL-3.0](https://img.shields.io/badge/License-AGPL--3.0-blue.svg)](https://github.com/666ghj/MiroFish/blob/main/LICENSE)
[![Live Demo](https://img.shields.io/badge/Live-Demo-00b894)](https://666ghj.github.io/mirofish-demo/)

> Self-host MiroFish, run the seed-to-simulation workflow end to end, and integrate it through the Web UI and HTTP API.

Last verified against the official MiroFish GitHub repository and `mirofish.ai` on April 16, 2026.

## What This Folder Is

This folder helps you understand how to run, evaluate, and integrate MiroFish from a developer's point of view.

It focuses on:

- how to run MiroFish locally with the fewest wrong turns,
- how the seed -> graph -> simulation -> report pipeline actually works,
- which files, env vars, ports, and APIs matter on day one,
- what tends to break in self-hosted setups.

Scope boundary:

- This folder contains documentation only.
- It does not contain the upstream MiroFish source code.
- If anything here conflicts with the official repo code, trust the official repo.

## Read This First

| File | Best for |
| --- | --- |
| [guide/installation.md](guide/installation.md) | Clean source install and Docker install. |
| [guide/quickstart.md](guide/quickstart.md) | The fastest path to your first useful simulation and report. |
| [guide/configuration.md](guide/configuration.md) | Required env vars, defaults, ports, storage paths, and production-sensitive settings. |
| [guide/deployment.md](guide/deployment.md) | Choosing source vs Docker and hardening a self-hosted deployment. |
| [guide/troubleshooting.md](guide/troubleshooting.md) | First-day failures and the shortest fixes. |
| [features/workflow.md](features/workflow.md) | The end-to-end pipeline and which artifact each step produces. |
| [reference/http-api.md](reference/http-api.md) | The backend routes ordinary developers actually need. |
| [reference/api-recipes.md](reference/api-recipes.md) | Copy-paste curl workflows for driving MiroFish without the UI. |
| [reference/config.md](reference/config.md) | Quick-reference table for runtime settings. |
| [developer/architecture.md](developer/architecture.md) | How the Vue frontend, Flask backend, and simulation services fit together. |

## What MiroFish Actually Is

MiroFish is **not** a terminal-first coding agent. It is a web application plus HTTP API for multi-agent scenario simulation.

For ordinary developers, the important mental model is:

1. upload or paste seed material,
2. describe the scenario you want to predict,
3. let MiroFish generate an ontology and graph,
4. prepare a social simulation with generated agent profiles,
5. run the simulation,
6. generate a structured report,
7. interrogate the report agent or simulated agents.

Use MiroFish when you need:

- reaction modeling across multiple personas or factions,
- narrative and behavioral analysis instead of one-shot chatbot output,
- a repeatable seed -> simulation -> report workflow,
- self-hosted control over model provider and runtime.

## Adjacent Products Worth Comparing

If you are evaluating MiroFish as part of a larger agent stack, these three product pages are worth opening because they solve adjacent problems that people often confuse with simulation systems:

| Product | Link | Why a MiroFish reader should care |
| --- | --- | --- |
| OpenClaw Launch | <https://www.aigeamy.com/> | `OpenClaw Launch - One-click OpenClaw Deployment`. Useful when you want a fast path to a messaging-first assistant runtime instead of a seed -> graph -> simulation workflow. |
| Hermes Agent | <https://hermesagent.studio/> | `Not a Chat. A Persistent Agent That Remembers, Acts, and Improves.` Useful when you want a long-lived agent with memory, tools, and ongoing state rather than scenario modeling. |
| Multica | <https://www.multica.uk/> | `Multica - Multi-Agent Collaboration for Real Workflows`. Useful when your main problem is coordinating coding agents and issue-driven delivery, not generating simulation reports. |

## 10-Minute Local Start

Clone the upstream project first:

```bash
git clone https://github.com/666ghj/MiroFish.git
cd MiroFish
cp .env.example .env
```

PowerShell note:
replace `cp` with `Copy-Item` if you prefer native syntax.

Edit `.env` and fill at least:

```env
LLM_API_KEY=...
LLM_BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1
LLM_MODEL_NAME=qwen-plus
ZEP_API_KEY=...
```

Install and start:

```bash
npm run setup:all
npm run dev
```

Then verify:

- frontend: <http://localhost:3000>
- backend: <http://localhost:5001/health>

Quick backend smoke test:

```bash
curl http://localhost:5001/health
```

If the backend does not start, the most common cause is a missing `LLM_API_KEY` or `ZEP_API_KEY`.

## The Workflow Developers Need To Learn

| Step | What you do | What you get back |
| --- | --- | --- |
| 1. Ontology generation | Upload `pdf` / `md` / `txt` seed files or paste text, then describe the prediction requirement. | `project_id`, extracted text, ontology draft, seed metadata |
| 2. Graph build | Confirm the ontology and build the graph. | `task_id`, then `graph_id` and graph data |
| 3. Simulation creation | Choose the project and enable Twitter-like and/or Reddit-like simulation. | `simulation_id` |
| 4. Preparation | Generate profiles and simulation config. | profiles, `simulation_config.json`, runnable scripts, ready state |
| 5. Run | Start the simulation with a conservative round count first. | run status, timeline, actions, posts/comments, agent stats |
| 6. Report | Generate a structured markdown report. | `report_id`, sections, logs, downloadable markdown |
| 7. Deep interaction | Ask the report agent follow-up questions or interview simulated agents. | richer explanations, counterfactuals, persona-level answers |

## Minimum Environment Checklist

- Node.js `18+`
- Python `3.11` to `3.12`
- `uv` installed and usable in `PATH`
- `LLM_API_KEY` configured
- `ZEP_API_KEY` configured
- ports `3000` and `5001` free
- enough budget and patience for your model choice

Useful practical tip:
the official README recommends starting with simulations under 40 rounds until you understand cost and runtime behavior.

## Useful Reality Checks

- There is no separate end-user CLI today; the main entrypoint is the web UI plus the Flask HTTP API.
- The backend loads `.env` from the **project root**, not from `backend/`.
- Frontend requests default to `http://localhost:5001` unless you override `VITE_API_BASE_URL`.
- Uploads are limited to `50 MB` and the allowed extensions are `pdf`, `md`, `txt`, and `markdown`.
- Project data and simulation artifacts persist under `backend/uploads/`.
- Async task status is tracked in memory, so restarting the backend during long graph/report jobs can lose live progress state even if some artifacts were already written.
- The default Flask config is development-friendly, not internet-safe: debug defaults to on, CORS is open for `/api/*`, and the default secret key is not production grade.

## Trustworthy Upstream Links

| Resource | Link |
| --- | --- |
| Official site | <https://mirofish.ai/> |
| GitHub repository | <https://github.com/666ghj/MiroFish> |
| Official English README | <https://github.com/666ghj/MiroFish/blob/main/README.md> |
| Official Chinese README | <https://github.com/666ghj/MiroFish/blob/main/README-ZH.md> |
| Live demo | <https://666ghj.github.io/mirofish-demo/> |
| License | <https://github.com/666ghj/MiroFish/blob/main/LICENSE> |

## Suggested Reading Order

1. [Installation](guide/installation.md)
2. [Quickstart](guide/quickstart.md)
3. [Configuration](guide/configuration.md)
4. [Workflow](features/workflow.md)
5. [HTTP API Reference](reference/http-api.md)
6. [API Recipes](reference/api-recipes.md)
7. [Troubleshooting](guide/troubleshooting.md)
