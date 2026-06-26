# MASH Implementation Plan

## Goal Description

Create **MASH** (Memory Abstraction Storage Hypervisor), a distributed, event‑driven storage layer that abstracts heterogeneous storage providers and presents a unified library of **objects** and **projects** to users.  The system must:
- Preserve existing files and filesystems (First Law).
- Provide a digital librarian (Seshat) that tracks object metadata, relationships, versions, and health.
- Offer staff services (Tuoni, Boatman, Observer, Caretaker, Gateway) each with a single responsibility.
- Expose familiar file‑system protocols (SMB, WebDAV, HTTP/REST) while keeping the underlying storage invisible.
- Support policies for replication, archiving, and cleanup.
- Include a powerful search‑first UI (optional AI‑assistant) and robust maintenance that runs opportunistically.

## User Review Required

> [!IMPORTANT]
> This plan defines the high‑level architecture and initial code scaffolding.  Please review the **component boundaries**, **technology choices**, and **open questions** before we proceed to implementation.

## Open Questions

> [!WARNING] 
> - **Language & Runtime**: Should the core services be written in Go (for concurrency) or Rust (for safety), or a mixed approach?
> - **Metadata Store**: SQLite vs. a graph database (e.g., Neo4j) for modelling object relationships?
> - **Event Bus**: Use NATS, Redis Streams, or a custom lightweight message queue?
> - **Gateway Protocols**: Which file‑system protocol to implement first (SMB vs. WebDAV) and what libraries will be used?
> - **Deployment Model**: Single‑node prototype vs. multi‑node Kubernetes deployment for the distributed architecture?
> - **Search Engine**: ElasticSearch vs. built‑in Everything Search integration on Windows.

## Proposed Changes

The project currently contains only `Spec.md`.  We will create a new directory layout and a set of starter source files.

---
### Core Packages

#### [NEW] `README.md`
- Overview, quick start, and contribution guidelines.

#### [NEW] `go.mod`
- Initialize a Go module (assuming Go as default language).

#### [NEW] `cmd/mash/main.go`
- Entry point that starts the control plane and launches staff services based on config.

#### [NEW] `internal/config/config.go`
- Configuration structs (storage providers, policies, network ports).

#### [NEW] `internal/event/event.go`
- Simple event bus abstraction (publish/subscribe).

#### [NEW] `internal/librarian/seshat.go`
- Core metadata catalog: objects, projects, relationships, health.

#### [NEW] `internal/staff/tuoni.go`
- Coordinator service (decision engine, policy evaluation).

#### [NEW] `internal/staff/boatman.go`
- Handles object transfer between storage providers.

#### [NEW] `internal/staff/observer.go`
- Listens to filesystem, USB, Git, cloud events and publishes them.

#### [NEW] `internal/staff/caretaker.go`
- Background maintenance jobs (hash verification, deduplication, replica repair, thumbnails, semantic indexing).

#### [NEW] `internal/gateway/smb.go`
- Placeholder for SMB gateway implementation (uses `github.com/hirochachacha/go-smb2`).

#### [NEW] `internal/gateway/webdav.go`
- Placeholder for WebDAV gateway (uses `golang.org/x/net/webdav`).

---
### Tests

#### [NEW] `internal/librarian/seshat_test.go`
- Unit tests for object/catalog API.

#### [NEW] `internal/event/event_test.go`
- Verify publish/subscribe behavior.

---
### Scripts & Docs

#### [NEW] `scripts/run.sh`
- Helper script to start all services locally (development mode).

#### [NEW] `docs/architecture.md`
- Expanded architecture diagram and component interaction description.

---
### Future Additions (not created now)
- AI‑assisted search UI (React frontend).
- Integration with existing tools (MergerFS, Syncthing, Git).
- Graph database backend option.

## Verification Plan

### Automated Tests
- Run `go test ./...` to ensure core packages compile and basic behaviours are validated.
- CI pipeline (GitHub Actions) will lint, build, and execute tests on each push.

### Manual Verification
- Start the prototype with `scripts/run.sh` and:
  1. Add a local folder as a storage provider via config.
  2. Create a project and add a file through the Seshat API.
  3. Verify the file appears via the SMB gateway (mount \`\\localhost\mash\`).
  4. Trigger a storage failure (rename the provider folder) and confirm that Caretaker reports the issue without data loss.
  5. Observe event logs in the console for Observer and Boatman activity.

---
**Next Steps**
- Resolve the open questions above with the user.
- Once confirmed, begin scaffolding the repository (create directories, `go.mod`, and starter files).
- Iterate on the design of the metadata model and event bus.
