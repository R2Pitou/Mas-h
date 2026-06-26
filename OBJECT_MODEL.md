# Object Model

This document defines the core concepts that the **catalogue (Seshat)** must represent.

## Object
- **Identity**: A globally unique identifier (UUID or hash).
- **Hash**: Cryptographic digest of the object's content.
- **Versions**: Immutable snapshots of the object's data; each version has its own hash.
- **Copies**: Physical manifestations of a version on a **Storage Provider**.
- **Canonical**: The authoritative copy selected by policies.
- **Replica**: Additional copies for redundancy.
- **Archive**: A cold‑storage copy with relaxed availability guarantees.
- **Cache**: A hot‑storage copy for fast access.
- **Relationships**: Links to other objects (e.g., derived, parent/child, composite).
- **Health**: Integrity status, verified hashes, and error metrics.
- **Intent**: User‑specified policies that influence placement and lifecycle.
- **Metadata**: Arbitrary key/value information (tags, timestamps, user notes).
- **History**: Event log of modifications, moves, and policy changes.

## Project
- A named collection of related **Objects**.
- Owns its own set of **Policies** and **Intent**.
- Provides a logical namespace for searching and browsing.

## Storage Provider
- Abstract representation of a physical storage backend (disk, USB, NAS, cloud, another MASH node, etc.).
- Advertises capabilities: capacity, latency, cost, access protocol, reliability.

## Policy
- Declarative statement of user intent, e.g., "keep three copies", "archive after 30 days", "project X must be hot".
- Evaluated by **Tuoni** to produce concrete actions (placement, replication, archiving).

## Health
- Metric indicating the current status of an **Object** or **Copy** (e.g., verified, corrupted, missing).

## Intent
- High‑level user desire expressed via **Policies**; the system translates intent into concrete actions.

These definitions are used throughout the other documentation to ensure a consistent vocabulary.
