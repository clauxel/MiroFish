# Quickstart

This quickstart is designed for ordinary developers who want to get one useful MiroFish result fast.

## Goal

Produce one end-to-end run:

1. seed material in,
2. graph built,
3. simulation prepared and run,
4. report generated,
5. follow-up questions asked.

## Before You Start

Make sure:

- the frontend is running at `http://localhost:3000`,
- the backend health endpoint responds,
- your `.env` has valid `LLM_API_KEY` and `ZEP_API_KEY`,
- you are willing to start with a **small** run.

## 1. Pick a Small, Focused Seed

For your first run, use one of these:

- a short policy draft,
- a single research memo,
- one compact incident summary,
- a short markdown brief you wrote yourself.

Avoid for the first test:

- giant PDFs,
- multi-topic seed bundles,
- ambiguous requirements,
- aggressive round counts.

## 2. Write a Good Simulation Requirement

Good requirements are specific about actors, horizon, and desired output.

Useful template:

```text
Using the attached material, simulate how the main stakeholder groups react over the next 10 rounds.
Identify likely factions, trigger events, sentiment shifts, key risks, and the most probable turning points.
Return a report that highlights what changed, why it changed, and what interventions would matter most.
```

## 3. Generate the Ontology

In the UI:

1. upload your seed file or paste text,
2. enter the simulation requirement,
3. add a project name if helpful,
4. run ontology generation.

What you should inspect before moving on:

- do the entity types make sense,
- are the key actors missing,
- is the analysis summary aligned with your scenario.

If the ontology already looks wrong, fix the seed or requirement now. Do not brute-force the later steps.

Healthy result looks like:

- you get a `project_id`,
- the ontology contains entity types that match your real stakeholders,
- the summary reads like your scenario instead of generic filler,
- uploaded files and total extracted text length are present.

## 4. Build the Graph

Run the graph build step and wait for the async task to complete.

Expected result:

- a valid `project_id`,
- a completed graph task,
- graph data you can inspect.

Healthy graph signals:

- the build task ends in `completed`,
- the task result contains `graph_id`,
- node and edge counts are non-zero,
- the graph is not dominated by junk entities.

If this step fails, go to [Troubleshooting](troubleshooting.md) before attempting simulation prep.

## 5. Create and Prepare the Simulation

Start with the smallest setup that still teaches you something:

- enable only one platform if the UI gives you the choice,
- keep the entity scope narrow,
- keep the round count low,
- only force regeneration when you really changed the setup.

Preparation is the step that generates:

- agent profiles,
- simulation configuration,
- runnable scripts under the hood,
- a ready state for execution.

Healthy preparation signals:

- `backend/uploads/simulations/<simulation_id>/` exists,
- `state.json` and `simulation_config.json` exist,
- profile files are present,
- the status endpoint eventually reports `ready` or `already_prepared=true`.

## 6. Run a Conservative First Simulation

For the first useful smoke test:

- use a low round count,
- watch run status and timeline instead of waiting blindly,
- stop early if the scenario is clearly malformed.

During the run, inspect:

- actions,
- timeline,
- posts/comments,
- agent stats.

Those artifacts tell you whether the scenario is coherent before you invest in report generation.

Healthy run signals:

- run status moves beyond `created` / `preparing`,
- action counts keep increasing,
- timeline data contains actual rounds,
- agent stats are not empty.

## 7. Generate the Report

Once the run looks healthy:

1. start report generation,
2. poll report status,
3. read the generated sections,
4. download the markdown report if needed.

If the report feels shallow, the fix is usually upstream:

- clearer seed,
- better requirement,
- better ontology,
- more appropriate simulation scope,
- only then more rounds.

Healthy report signals:

- you receive `report_id` immediately after starting generation,
- status polling eventually reaches `completed`,
- `GET /api/report/by-simulation/<simulation_id>` returns a report,
- the markdown download opens cleanly and sections are not empty.

## 8. Ask Follow-Up Questions

After the report exists, use:

- report chat for explanation and synthesis,
- agent interviews for persona-specific reasoning.

Good follow-up prompts:

- "Why did the skeptical faction accelerate after round 4?"
- "Which actors changed the final trajectory the most?"
- "What intervention would most reduce polarization?"
- "What evidence in the simulation supports the final recommendation?"

## Fast Rollback Logic

When a run looks wrong, go back only one stage:

- wrong ontology: fix the seed or requirement, then regenerate ontology
- wrong profiles/config: re-run preparation with a narrower scope
- unrealistic behavior: lower rounds, change platform choice, or review entities
- weak report: inspect the simulation first, then regenerate the report only if the run itself is healthy

If you want to drive the same workflow by script instead of the UI, use [../reference/api-recipes.md](../reference/api-recipes.md).

## First Useful Run Checklist

- one focused seed,
- one clear requirement,
- ontology looks sane,
- graph built successfully,
- simulation prepared successfully,
- run completed with believable activity,
- report generated,
- at least one follow-up question answered.
