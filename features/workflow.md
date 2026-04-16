# Workflow

MiroFish is easiest to understand as a pipeline that keeps producing new artifacts.

## The Core Pipeline

1. **Seed intake**
   You upload or paste source material and describe the prediction goal.
2. **Ontology generation**
   MiroFish extracts entities, relations, and scenario framing.
3. **Graph building**
   The ontology becomes a usable graph tied to a project.
4. **Simulation preparation**
   Agent profiles and simulation configuration are generated.
5. **Simulation run**
   Agents interact over rounds on the selected platform model.
6. **Report generation**
   The post-run state is synthesized into a structured report.
7. **Deep interaction**
   You ask the report agent or simulated agents follow-up questions.

## What Each Step Produces

| Step | Main artifact |
| --- | --- |
| Ontology generation | `project_id`, ontology draft, extracted text |
| Graph build | `graph_id`, task status, graph data |
| Simulation creation | `simulation_id` |
| Simulation preparation | profiles, `simulation_config.json`, generated scripts |
| Simulation run | timeline, posts, comments, actions, stats, state |
| Report generation | `report_id`, markdown report, logs, sections |
| Deep interaction | interview responses, report chat answers |

## How To Debug by Stage

If the problem is:

- wrong actors or missing concepts: fix the seed or ontology stage
- chaotic profiles: revisit preparation inputs
- implausible behavior: inspect simulation config and round count
- weak conclusions: inspect the run itself before blaming the report stage

## The Developer Shortcut

The shortest reliable mental model is:

```text
seed -> ontology -> graph -> simulation -> report -> follow-up questions
```

If you keep that model in mind, the API surface and UI screens become much easier to reason about.
