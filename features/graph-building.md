# Graph Building

Graph building is where MiroFish stops being "uploaded files plus a prompt" and becomes a structured simulation input.

## Inputs That Matter

The first graph step is driven by:

- uploaded files,
- optional pasted text,
- `simulation_requirement`,
- optional `project_name`,
- optional `additional_context`.

Supported file extensions from the backend config:

- `pdf`
- `md`
- `txt`
- `markdown`

## Main API Calls

| Endpoint | Purpose |
| --- | --- |
| `POST /api/graph/ontology/generate` | Parse seed material and generate the ontology draft |
| `POST /api/graph/build` | Build the graph from the project/ontology |
| `GET /api/graph/task/<task_id>` | Poll graph task status |
| `GET /api/graph/data/<graph_id>` | Read graph data |

## What Good Input Looks Like

For better graphs:

- keep the seed tightly scoped,
- remove obvious boilerplate and irrelevant appendices,
- make the requirement outcome-oriented,
- state who you care about and what change you are trying to predict.

Weak input usually creates:

- bloated entity lists,
- generic relations,
- fuzzy personas later in the pipeline.

## Developer Tips

- Treat ontology generation as a review point, not a button you click and forget.
- If the ontology is obviously wrong, do not spend tokens on simulation prep.
- Use project names that encode the scenario so stored artifacts are easier to inspect later.

## Persistence

Project artifacts are stored under:

```text
backend/uploads/projects/<project_id>/
```

That directory typically contains:

- `project.json`
- uploaded files
- extracted text

## When To Reset a Project

Reset or rebuild when:

- the extracted text is clearly bad,
- entity types are missing the main stakeholder classes,
- the project accidentally mixed unrelated seed sets,
- you fundamentally changed the prediction requirement.
