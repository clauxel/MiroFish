# Simulation

This is the stage where MiroFish turns a graph into a living scenario.

## Lifecycle

The normal lifecycle is:

1. create simulation,
2. prepare simulation,
3. inspect generated profiles/config,
4. start run,
5. inspect run outputs,
6. stop or close the environment when done.

## Main API Calls

| Endpoint | Purpose |
| --- | --- |
| `POST /api/simulation/create` | Create a simulation shell for a project/graph |
| `POST /api/simulation/prepare` | Generate profiles and configuration |
| `POST /api/simulation/prepare/status` or the documented GET poll flow | Check preparation progress |
| `GET /api/simulation/<simulation_id>` | Read one simulation |
| `GET /api/simulation/list` | List simulations |
| `POST /api/simulation/start` | Start a run |
| `POST /api/simulation/stop` | Stop a run |
| `POST /api/simulation/close-env` | Close the simulation environment cleanly |

## Preparation Stage

Preparation is where MiroFish generates:

- entity filtering,
- agent profiles,
- simulation configuration,
- runnable scripts for the selected platform shape.

Frontend-verified preparation inputs include:

- `simulation_id`
- `entity_types`
- `use_llm_for_profiles`
- `parallel_profile_count`
- `force_regenerate`

Practical tip:
do not use `force_regenerate` unless you know you changed something important.

## Run Stage

Common run inputs from the frontend client:

- `simulation_id`
- `platform`
- `max_rounds`
- `enable_graph_memory_update`

Recommended first-run posture:

- keep `max_rounds` conservative,
- verify one platform first,
- inspect results before scaling.

## What To Inspect During a Run

Useful inspection endpoints:

- run status
- detailed run status
- actions
- timeline
- posts
- comments
- agent stats

These artifacts tell you whether the scenario is producing meaningful emergent behavior or just burning tokens.

## Saved Simulation Artifacts

Simulation artifacts live under:

```text
backend/uploads/simulations/<simulation_id>/
```

Important files include:

- `state.json`
- `simulation_config.json`
- generated profiles
- generated scripts or script download metadata

## Agent Interviews

MiroFish exposes post-run interviews through the simulation API.

Useful patterns:

- ask one agent why they changed position,
- batch interview a set of representative factions,
- compare optimistic vs skeptical personas after the same run.

## When A Run Is Healthy Enough To Report

You are usually ready for report generation when:

- actions are non-trivial,
- timelines show meaningful change,
- major stakeholders actually interact,
- the run reaches a stable ending or an interesting divergence.
