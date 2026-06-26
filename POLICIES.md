# Policies

MASH allows users to express **intent** without prescribing *how* it should be realized.  Policies are declarative statements that the **Librarian (Tuoni)** evaluates against the current state of objects, storage providers, and system health.

## Policy Language (informal)
- One sentence per policy.
- Use verbs like **keep**, **archive**, **replicate**, **promote**, **retain**, **delete**.
- Specify *what* (object/project) and *what* (intent), not *where*.

### Example Policies
- `Keep three copies.` – Guarantees that three distinct copies of each object exist on different storage providers.
- `Archive objects older than 30 days.` – Moves objects that have not been accessed for 30 days to an archive tier.
- `Project "Photos" must be hot.` – Ensures objects in the *Photos* project are stored on high‑performance storage.
- `Never store backups on the same physical drive as the primary copy.` – Enforces separation of primary and backup locations.

## Policy Evaluation
1. **Tuoni** receives a policy event.
2. It queries **Seshat** for relevant objects/projects.
3. It matches provider capabilities (latency, capacity, reliability).
4. It generates a **plan** (e.g., create replica on provider X, move archive to provider Y).
5. The plan is dispatched to **Boatman** for execution.

## Policy Lifecycle
- **Create** – User adds a policy via the UI or config file.
- **Update** – Modifications trigger re‑evaluation of affected objects.
- **Delete** – Removal may trigger cleanup (e.g., retire extra replicas).
- **Conflict Resolution** – If policies conflict, Tuoni applies a priority order (explicit > implicit, newer > older) and logs the decision.

## Interaction with Other Docs
- **OBJECT_MODEL.md** defines the entities (Object, Copy, Project) that policies act upon.
- **EVENTS.md** lists `PolicyChanged` as the trigger for re‑evaluation.
- **GLOSSARY.md** provides term definitions for *policy* and *intent*.
