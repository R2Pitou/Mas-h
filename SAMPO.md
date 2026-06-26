# SAMPO

SAMPO (Storage Abstraction Management and Policy Orchestrator) is the reference storage engine for MAS-H.

It is responsible for turning user intent into storage operations while hiding the complexity of heterogeneous storage providers. SAMPO itself does not store data. Instead, it coordinates a collection of specialised services (Staff), each with a single responsibility.

Staff communicate through events and shared knowledge rather than direct control, allowing the system to remain modular, resilient, and extensible.

---

## Tuoni

Tuoni is the reasoning engine of SAMPO.

It interprets user intent, consults the catalogue, evaluates current system state, applies storage policies, and produces storage decisions.

Tuoni performs no storage operations directly. Instead, it requests work from other staff members.

Tuoni is intentionally limited to reasoning about storage. It is not a general-purpose AI or language model.

---

## Seshat

Seshat is the catalogue of SAMPO.

It maintains knowledge about the system, including:

* Objects
* Relationships
* Versions
* Copies
* Health
* Provenance
* Policies
* Intent

Seshat remembers.

It does not make decisions or perform storage operations.

---

## Boatman

Boatman is responsible for moving objects between storage providers.

It executes transfer plans created by Tuoni, including:

* Replication
* Migration
* Archiving
* Cache promotion
* Cache eviction

Boatman never makes policy decisions.

It simply performs the requested work.

---

## Observer

Observer watches the outside world.

It monitors:

* Filesystem changes
* Storage provider availability
* USB insertion and removal
* Git repositories
* Cloud storage providers
* Other external events

Observer publishes events describing what happened.

It never interprets those events or decides what they mean.

---

## Caretaker

Caretaker performs background maintenance whenever system resources are available.

Typical responsibilities include:

* Hash verification
* Deduplication
* Replica repair
* Storage health checks
* Thumbnail generation
* Semantic indexing
* Archive promotion and demotion
* Cache maintenance

Caretaker performs preventative maintenance but never changes storage policy.

---

## Gateway

Gateway provides familiar interfaces for users and applications.

It may expose protocols such as:

* SMB
* WebDAV
* HTTP/REST

Gateway translates user requests into SAMPO operations.

It does not know how storage decisions are made or where objects physically reside.

Its responsibility is to present a simple, familiar interface while leaving storage intelligence to the rest of SAMPO.
