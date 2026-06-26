# OBJECT_MODEL.md

## Domain Model Overview

This document defines the core nouns that exist in the **MAS‑H** universe, their properties, relationships, invariants, and lifecycle. It is implementation‑agnostic and serves as the foundation for all later design decisions.

---

### Object
- **Definition**: A logical piece of information, independent of storage location.
- **Identity**: Globally unique identifier (UUID or hash).
- **Versions**: Immutable snapshots; each version has its own content hash.
- **Relationships**: Can be linked to other objects (derived, parent/child, composite).
- **Metadata**: Arbitrary key/value pairs (tags, timestamps, notes).
- **Health**: Integrity status, verification results, error metrics.
- **Lifecycle**: Created → versioned → copied → possibly archived → eventually deleted.

### Version
- **Definition**: An immutable snapshot of an Object's data.
- **Hash**: Cryptographic digest of the version's content.
- **Copies**: One or more physical manifestations on Storage Providers.
- **Canonical**: The designated authoritative copy selected by policies.
- **Replica**: Additional copies for redundancy.
- **Archive**: Cold‑storage copy with relaxed availability guarantees.
- **Cache**: Hot‑storage copy for fast access.

### Copy
- **Definition**: A physical instance of a Version stored on a Storage Provider.
- **Belongs To**: Exactly one Version (and thus one Object).
- **Location**: Exactly one Storage Provider.
- **State**: Healthy, degraded, missing, or corrupted.

### Canonical
- **Definition**: The designated authoritative Copy of an Object used for reads.
- **Selection**: Determined by Policies evaluated by the Librarian (Tuoni).
- **Invariant**: There is at most one Canonical per Object at any time.

### Storage Provider
- **Definition**: Abstract representation of a physical storage backend (disk, USB, NAS, cloud, another MAS‑H node, etc.).
- **Capabilities**: Capacity, latency, cost, access protocol, reliability, encryption.
- **Registration**: Providers are discovered and registered by the Observer service.

### Project
- **Definition**: A logical collection of related Objects.
- **Ownership**: Projects own Policies and Intent.
- **Namespace**: Provides a scoped view for searching and browsing Objects.
- **Membership**: An Object may belong to multiple Projects.

### Collection
- **Definition**: An ordered or unordered grouping of Objects for convenience (e.g., playlists, datasets).
- **Relationship**: Similar to Project but typically user‑defined and transient.

### Tag
- **Definition**: A simple label (key/value) attached to Objects for categorisation.
- **Usage**: Enables faceted search and policy targeting.

### Policy
- **Definition**: Declarative statement of user intent (e.g., "keep three copies", "archive after 30 days", "Project X must be hot").
- **Evaluation**: Performed by the Librarian (Tuoni) to produce concrete actions.
- **Scope**: May apply to Objects, Projects, or Storage Providers.

### Event
- **Definition**: An immutable message describing a state change or significant occurrence (e.g., ObjectCreated, ProviderOffline, PolicyChanged).
- **Producer**: The service that emits the event.
- **Consumers**: Services that react to the event.
- **Semantics**: Captures *why* an event exists, not just its data payload.

### Job
- **Definition**: A long‑running or asynchronous task triggered by Events or Policies (e.g., replication, migration, verification).
- **Lifecycle**: Queued → running → succeeded / failed.
- **Tracking**: Jobs are observable via events (JobStarted, JobCompleted, JobFailed).

### Staff Member (Service)
- **Definition**: An independent logical component of MAS‑H that performs a single responsibility and communicates exclusively via Events.
- **Examples**: Observer, Tuoni (Librarian), Seshat (Catalogue), Boatman (Replication), Caretaker (Health monitoring), Gateway (External API).
- **Invariant**: No direct coupling between Staff Members; all interaction occurs through the event bus.

---

## Invariants & Constraints
- An Object always has at least one Version; a Version always has at least one Copy.
- A Canonical Copy must exist for an Object unless the Object is archived with no active reads.
- Policies are immutable definitions; changes produce a `PolicyChanged` event.
- Events are immutable; consumers must not mutate event payloads.
- Jobs must be idempotent where possible to tolerate retries.

## Lifecycle Summary
1. **Discovery** – Observer detects a new file → emits `ObjectCreated`.
2. **Versioning** – Seshat creates a Version and associated Copies.
3. **Policy Evaluation** – Tuoni evaluates Policies, possibly designating a Canonical and spawning Jobs.
4. **Replication / Archival** – Boatman and Caretaker perform Jobs, emitting `CopyCreated`, `CopyDeleted`, etc.
5. **Health Monitoring** – Caretaker emits `ProviderOffline`, `CopyCorrupted`.
6. **User Interaction** – Gateway receives user commands, emits events like `PolicyChanged`.

This domain model is language‑agnostic and can be implemented in Go, Rust, Python, or any other stack.

