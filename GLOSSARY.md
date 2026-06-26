# Glossary

**Object** – A logical entity with a unique identifier, hash, versions, copies, and associated metadata. It represents the data MASH manages.

**Copy** – A physical manifestation of a specific version of an Object stored on a Storage Provider.

**Version** – An immutable snapshot of an Object's content, identified by its own hash.

**Canonical** – The authoritative copy of an Object selected by policies; the source of truth for reads.

**Replica** – Additional copies of an Object for redundancy, distinct from the canonical copy.

**Archive** – A cold‑storage copy with reduced availability guarantees, used for long‑term retention.

**Cache** – A hot‑storage copy placed on high‑performance media for fast access.

**Project** – A named collection of related Objects, providing a logical namespace and its own set of Policies.

**Storage Provider** – An abstract representation of a physical storage backend (disk, USB, NAS, cloud, another MASH node, etc.) that advertises capabilities such as capacity, latency, and cost.

**Policy** – A declarative statement of user intent (e.g., *keep three copies*), which the Librarian evaluates to produce concrete actions.

**Intent** – The high‑level desire expressed by the user via Policies.

**Health** – The integrity status of an Object or Copy, including verification results and error metrics.

**Librarian** – The intelligence component (Tuoni) that decides how to satisfy user intent.

**Staff** – Independent logical services (Tuoni, Seshat, Boatman, Observer, Caretaker, Gateway) each with a single responsibility, communicating via events.

**Gateway** – The external interface exposing SMB/WebDAV/HTTP endpoints that appear as ordinary filesystems to applications.

**Event** – A message describing a state change (e.g., `ObjectCreated`, `PolicyChanged`) published on the event bus.
