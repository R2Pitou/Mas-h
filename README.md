# MAS-H

## Memory Abstraction Storage Hypervisor

### Storage Hypervisor with a Digital Librarian control plane

MAS-H abstracts heterogeneous storage providers and presents a unified library of objects, projects, and relationships to users. It preserves existing files, reduces human cognitive load, and orchestrates existing tools (Everything, Syncthing, Git, etc.) rather than replacing them.

The goal is not to invent yet another filesystem or NAS. The goal is to make physical storage irrelevant to the user.

---

## SAMPO

**SAMPO** stands for **Storage Abstraction Management and Policy Orchestrator**.

SAMPO is the reference storage engine for MAS-H. It turns user intent into storage operations while hiding the complexity of heterogeneous storage providers.

SAMPO itself does **not** store data. Instead, it coordinates a collection of specialised services called **Staff**, each with a single responsibility.

SAMPO also provides the event infrastructure that lets the Staff communicate cleanly. The Staff do not directly manage each other. They publish and consume events through SAMPO.

---

## SAMPO Staff Components

- **Tuoni** – the reasoning engine that interprets user intent, consults the catalogue, evaluates policies, and produces storage decisions. Tuoni never performs storage I/O directly.
- **Seshat** – the catalogue that holds system knowledge: objects, relationships, versions, copies, health, provenance, policies, intent, and history.
- **Boatman** – executes transfer plans created by Tuoni, including replication, migration, archiving, cache promotion, and cache eviction.
- **Observer** – monitors the outside world: filesystem changes, storage-provider availability, USB events, Git repos, cloud providers, and similar external changes. It publishes raw events.
- **Caretaker** – performs background maintenance when resources are idle: hash verification, deduplication, replica repair, health checks, thumbnail generation, semantic indexing, archive and cache maintenance.
- **Gateway** – provides familiar interfaces such as SMB, WebDAV, and HTTP/REST for users and applications. It translates user requests into SAMPO operations.

---

## High-Level Architecture

```mermaid
flowchart TB
    User["👤 User"]

    Gateway["Gateway\nSMB • WebDAV • REST"]
    SAMPO["SAMPO\nStorage Abstraction Management\n& Policy Orchestrator"]

    Tuoni["Tuoni\nReasoning Engine"]
    Seshat["Seshat\nCatalogue\nObjects \n• Relationships \n• Versions \n• Copies \n• Health \n• Provenance \n• Policies \n• Intent \n• History"]
    Boatman["Boatman\nMoves Objects"]
    Observer["Observer\nPublishes Events"]
    Caretaker["Caretaker\nMaintenance"]

    Storage["Storage Providers\nSSD\nHDD\nUSB\nGitHub\nCloud\nOther MAS-H Nodes"]

    User --> Gateway
    Gateway --> SAMPO

    Observer --> SAMPO

    SAMPO --> Tuoni
    SAMPO --> Seshat
    SAMPO --> Boatman
    SAMPO --> Caretaker

    Tuoni --> Seshat
    Tuoni --> Boatman
    Tuoni --> Caretaker

    Boatman --> Storage
    Observer --> Storage
    Caretaker --> Storage
```

---

## Design Principles

- Storage is an implementation detail.
- Search comes before folders.
- Users express intent, not implementation.
- MAS-H never makes data less accessible.
- Existing open-source tools are orchestrated, not replaced.
- Optimise human time before machine time.
- The system should feel like a librarian, not a storage admin panel.
- The user should never need to remember which drive contains a file.
- The system should preserve ordinary files and ordinary filesystems.
- The architecture should remain understandable even if individual components move to different machines.

---

## Documentation

- [VISION.md](VISION.md)
- [MANIFESTO.md](MANIFESTO.md)
- [ARCHITECTURE.md](ARCHITECTURE.md)
- [SAMPO.md](SAMPO.md)
- [OBJECT_MODEL.md](OBJECT_MODEL.md)
- [POLICIES.md](POLICIES.md)
- [GLOSSARY.md](GLOSSARY.md)
- [NON_GOALS.md](NON_GOALS.md)
- [EVENTS.md](EVENTS.md)
- [DECISIONS.md](DECISIONS.md)
- [PRIOR_ART.md](PRIOR_ART.md)
- [BUILDING_BLOCKS.md](BUILDING_BLOCKS.md)

---

## Getting Started

Read the individual documents to understand the vision, architecture, terminology, and design decisions before any code is written.

The best order is:

1. `VISION.md`
2. `MANIFESTO.md`
3. `OBJECT_MODEL.md`
4. `SAMPO.md`
5. `ARCHITECTURE.md`
6. `POLICIES.md`
7. `GLOSSARY.md`
8. `NON_GOALS.md`
9. `EVENTS.md`
10. `DECISIONS.md`
11. `PRIOR_ART.md`
12. `BUILDING_BLOCKS.md`

If something is unclear, add a note to `DECISIONS.md` rather than guessing.