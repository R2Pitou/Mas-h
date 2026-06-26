# Architecture

MASH is composed of independent logical services (Staff) that communicate exclusively through events.

`
+-------------------+   Event Bus   +-------------------+
|      Tuoni        |<------------>|   Observer        |
+-------------------+              +-------------------+
        ^                                 ^
        |                                 |
        v                                 v
+-------------------+   Event Bus   +-------------------+
|      Seshat       |<------------>|   Boatman        |
+-------------------+              +-------------------+
        ^                                 ^
        |                                 |
        v                                 v
+-------------------+   Event Bus   +-------------------+
|    Caretaker      |<------------>|    Gateway       |
+-------------------+              +-------------------+
`

- **Single-machine first** – all services run on one host; scaling to multiple machines is optional.
- **Event-driven** – services publish and subscribe to events defined in EVENTS.md.
- **Gateway** exposes SMB/WebDAV/HTTP interfaces that appear as ordinary filesystems to applications.
- **Storage Providers** (disks, USB, cloud, other MASH nodes) are registered with the system and advertised via capabilities.
