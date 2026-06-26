# MASH

## Memory Abstraction Storage Hypervisor

### Vision

MASH is a Storage Hypervisor and Digital Librarian.

Its purpose is to make physical storage irrelevant.

Users should think in terms of information, projects, relationships and search rather than disks, partitions, filesystems or drive letters.

The storage layer should become as invisible as virtual memory is today.

---

# Elevator Pitch

MASH virtualizes storage in the same way that a hypervisor virtualizes compute.

Applications believe they are interacting with ordinary files.

In reality they interact with a software-defined storage layer capable of managing heterogeneous storage providers, automatically maintaining data, and presenting a single coherent library.

MASH is not another NAS.

MASH is not another backup application.

MASH is not another cloud drive.

MASH is a Storage Hypervisor.

---

# Core Philosophy

Storage is not the product.

Knowledge about storage is the product.

The system should know:

* where information exists
* which copy is authoritative
* how healthy it is
* how many copies exist
* which projects reference it
* what should happen next

The user should never need to remember those things.

That is the Librarian's responsibility.

---

# First Law

MASH must never make user data less accessible than it was before MASH existed.

If MASH is removed:

* files remain ordinary files
* disks remain readable
* existing filesystems remain intact
* rebuilding MASH should require rebuilding metadata only

No proprietary storage formats.

No proprietary filesystems.

No mandatory database for data access.

---

# Human Time Before Machine Time

The primary optimisation target is not IOPS.

It is human cognition.

Saving a user five minutes searching for information is more valuable than saving five milliseconds reading a file.

Machine resources exist to reduce human friction.

---

# Storage Hypervisor

MASH operates at the file and object layer.

Unlike enterprise block-level storage hypervisors, MASH does not virtualize sectors.

Instead it virtualizes:

* storage providers
* filesystems
* namespaces
* projects
* replicas
* caches
* archives

The user should never care whether a file resides on:

* SSD
* HDD
* USB
* NTFS
* exFAT
* ext4
* SMB
* GitHub
* Object Storage
* Cloud
* Another MASH node

Those are implementation details.

---

# Digital Librarian

MASH is fundamentally a librarian.

The librarian knows:

* where everything is
* what everything is
* which copies are canonical
* relationships between objects
* project membership
* storage health
* maintenance history

The librarian makes decisions.

Storage providers do not.

---

# Staff

The architecture consists of independent staff members.

## Tuoni

The Librarian.

Decision maker.

Coordinates every other component.

Owns no storage.

Never copies bytes.

---

## Seshat

The Catalogue.

Maintains knowledge.

Objects.

Relationships.

Policies.

History.

Health.

---

## Boatman

Transfers objects between storage providers.

Executes plans.

Never creates policy.

Never makes decisions.

---

## Observer

Detects events.

Filesystem changes.

USB insertion.

Git updates.

Cloud changes.

Never acts.

Only reports.

---

## Caretaker

Background maintenance.

Runs when resources are idle.

Verifies hashes.

Detects duplicates.

Repairs replicas.

Maintains storage health.

Never interrupts the user.

---

## Gateway

External interface.

May expose:

SMB

WebDAV

HTTP

REST

Future APIs

Applications continue believing they are using ordinary filesystems.

---

# Storage Providers

Storage providers are interchangeable resources.

Examples include:

Internal disks

External disks

USB storage

NAS

SMB

Git repositories

GitHub Actions artifacts

Cloud object storage

Archive storage

Another MASH instance

Storage providers advertise capabilities rather than filesystem types.

---

# Objects

Files are implementation details.

Objects are first-class citizens.

Objects possess:

Identity

Hash

Relationships

Versions

Copies

Health

Canonical location

Storage policy

Metadata

History

One object may exist in many physical locations simultaneously.

---

# Projects

Projects are collections of related objects.

Examples:

Reaper project

Images

Exports

Documentation

Source files

Git repository

Transcripts

Notes

Projects exist independently of physical directory structures.

---

# Policies

Users express intent.

Example:

Protect this project.

Keep three copies.

One local.

One off-site.

One hot.

GitHub is canonical.

MASH determines how to satisfy those policies.

---

# Search First

Search is the primary interface.

Folders are secondary.

Eventually search should understand:

Projects

Relationships

Versions

Duplicates

Health

Canonical copies

AI-assisted semantic search should be optional.

MASH must function perfectly without AI.

---

# Distributed Architecture

MASH is a distributed system.

Staff members are logical services rather than machines.

Storage providers are independent from decision makers.

Example deployment:

Mac Mini

Storage provider

N150

Control plane

Gaming PC

AI

Semantic indexing

Heavy processing

Cloud

Backup

GitHub

Canonical source

Any service may move to another machine without changing the architecture.

---

# Event Driven

Every staff member communicates through events.

Observer never edits metadata.

Boatman never changes policy.

Seshat never moves files.

Tuoni coordinates.

Every component has exactly one responsibility.

---

# Maintenance Philosophy

Maintenance should occur opportunistically.

Not according to arbitrary schedules.

When resources are idle MASH should:

Verify hashes

Repair replicas

Refresh caches

Download newer canonical versions

Deduplicate storage

Validate backups

Move cold data

Promote hot data

Generate thumbnails

Build semantic indexes

Maintenance should never interfere with the user's work.

---

# Existing Technologies

MASH should reuse existing technologies wherever practical.

Potential components include:

MergerFS

SQLite

Samba

Everything Search

Syncthing

Git

Object Storage

The innovation is not replacing these systems.

The innovation is orchestrating them through the Librarian.

---

# Research Questions

Investigate:

Has this architecture already been implemented?

Which existing projects solve individual components?

Which ideas are genuinely novel?

What should never be reinvented?

How should object relationships be modelled?

Should the catalogue be relational, graph-based or hybrid?

Can Everything become the preferred Windows discovery engine?

How should canonical objects, replicas, archives and caches be represented?

How should project relationships be modelled?

What failure modes exist when storage becomes virtualized?

---

# Guiding Principle

The user should never ask:

"Which drive is this on?"

The user should ask:

"Where is my project?"

The Librarian should answer.

The computer should remember.

The human should not have to.
