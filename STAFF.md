# Staff

MASH is built from independent logical services (Staff) that each have a single responsibility and communicate only via events.

## Tuoni
- Decision maker and policy evaluator.
- Coordinates other staff members.
- Determines where to place copies, replicas, archives, and caches based on user intent and policies.

## Seshat
- The **catalogue** (knowledge base) that stores objects, their relationships, versions, health, provenance, and intent.
- Provides query APIs for other staff.

## Boatman
- Moves objects between storage providers.
- Executes transfer plans created by Tuoni.
- Does not make policy decisions.

## Observer
- Detects external changes: filesystem modifications, USB insertion/removal, Git updates, cloud changes, provider status changes.
- Emits corresponding events.

## Caretaker
- Performs opportunistic maintenance when resources are idle.
- Tasks include hash verification, deduplication, replica repair, thumbnail generation, semantic indexing, archive promotion/demotion.

## Gateway
- Exposes SMB, WebDAV, HTTP/REST endpoints that present a familiar filesystem view.
- Translates user file operations into events and queries against Seshat.
