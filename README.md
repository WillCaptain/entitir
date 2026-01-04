# Entitir

**Entitir** is a **deterministic ontology interpreter** for building **ontology-first data application systems**.

It provides a **governed authority layer** between raw data and real-world actions, enabling **AI-assisted reasoning without sacrificing correctness, safety, or auditability**.

Entitir is inspired by large-scale ontology-driven systems (e.g. Palantir-style architectures), but is **fully open-source, modular, and interpreter-centric**.

---

## Why Entitir Exists

Modern data systems face a structural problem:

* Data platforms are **good at storing facts**
* AI systems are **good at generating intent**
* But **no layer enforces meaning, authority, and safety**

As a result:

* LLMs hallucinate actions
* Business logic leaks into prompts
* Permissions are bypassed
* Decisions become non-explainable

**Entitir solves this by acting as the authority boundary.**

> AI may *propose*.
> Entitir *decides*.

---

## System Architecture (5 Layers)

Entitir is designed as part of a **five-layer ontology-first data application OS**:

```
┌───────────────────────────┐
│ Application Layer         │
│ Dashboards / Workflows    │
│ AI Assistants / UIs       │
└────────────▲──────────────┘
             │
┌────────────┴──────────────┐
│ Model Layer               │
│ ML / LLMs / Rules         │
│ Planning & Reasoning      │
└────────────▲──────────────┘
             │
┌────────────┴──────────────┐
│ Ontology Layer  ← Entitir│
│ Objects / Relations       │
│ Actions / Policies        │
│ Interpreter (Authority)   │
└────────────▲──────────────┘
             │
┌────────────┴──────────────┐
│ Pipeline Layer            │
│ Ingestion / CDC / ETL     │
│ Versioned Transforms      │
└────────────▲──────────────┘
             │
┌────────────┴──────────────┐
│ Dataset Layer             │
│ Immutable Facts           │
│ Versioned Data            │
└───────────────────────────┘
```

### Current Focus

🚧 **This repository focuses on the Ontology Layer**, specifically:

* ontology schema
* ontology operations
* ontology interpreter
* decision and action governance

---

## What Entitir Is (and Is Not)

### Entitir **IS**

* an ontology interpreter
* a decision kernel
* an authority boundary
* a semantic execution engine
* safe middleware between AI and the real world

### Entitir **IS NOT**

* a graph database
* a BI tool
* a data warehouse
* a generic agent framework
* a semantic web / RDF stack

---

## Core Concepts

Entitir standardizes **semantics and authority**, not syntax.

### 1. Objects

Typed representations of real-world entities.

Examples:

* `Employee`
* `Laptop`
* `Shipment`

Objects represent **current world state**, not raw tables.

---

### 2. Attributes

Decision-critical state attached to objects.

Examples:

* `Employee.status`
* `Laptop.availability`

Attributes are:

* indexed
* explainable
* derived from datasets

---

### 3. Relationships

Typed, directional links between objects.

Examples:

* `Employee → reportsTo → Employee`
* `Employee → assignedLaptop → Laptop`

Traversal is **governed**, not free-form.

---

### 4. Actions

Explicit, declared operations that may change the world.

Examples:

* `Employee.StartOnboarding()`
* `GrantAccess(Employee, System)`

Actions are:

* the *only* way to cause side effects
* guarded by preconditions and policies
* executed only by the interpreter

---

### 5. Preconditions

State-based guards evaluated **before** an action executes.

Example:

```
Employee.status == "Active"
AND no OnboardingTask in progress
```

Preconditions answer:

> “Is the world in a valid state for this action?”

---

### 6. Policies

Contextual authorization rules.

Policies answer:

> “Even if valid, is this allowed?”

Policies depend on:

* actor
* role
* object state
* environment

---

## Ontology Operations

Entitir defines a **small, strict set of operations**.

### Read Operations

* `get` – retrieve an object
* `find` – search objects by state
* `traverse` – follow relationships
* `explain` – explain state or lineage

### Decision Operations

* `canInvoke` – check action eligibility
* `plan` – propose (but not execute) actions

### Action Operations

* `invoke` – execute a declared action
* `approve` – human-in-the-loop transitions
* `rollback` – compensating actions

### Governance Operations

* schema evolution
* field promotion
* action deprecation

All operations are **validated and enforced by the interpreter**.

---

## The Interpreter (The Core)

Entitir’s interpreter is the **kernel of the system**.

It guarantees:

* semantic validation
* type safety
* permission enforcement
* bounded execution
* deterministic behavior
* auditability

LLMs **never bypass** the interpreter.

---

## AI Integration Philosophy

Entitir is **AI-compatible but AI-safe**.

### LLMs MAY

* generate ontology queries (structured)
* propose action plans
* explain ontology state

### LLMs MAY NOT

* invent fields or actions
* mutate state directly
* bypass policies
* access raw datasets implicitly

LLMs generate **intent**.
Entitir enforces **authority**.

---

## Events (Non-Authoritative)

Entitir emits events **after** state transitions:

* dataset updated
* ontology state changed
* action executed

Events:

* notify
* integrate
* audit

Events **never decide**.

---

## Consistent Guarantees

Entitir enforces the following invariants:

* **Determinism** – same state, same result
* **Explainability** – every decision has a “why”
* **Permission Safety** – no data or action leakage
* **Temporal Consistency** – decisions are time-anchored
* **Boundedness** – no unbounded scans or traversals
* **Reversibility** – compensating actions where possible

If a system violates these, it is **not Entitir-compatible**.

---

## Project Status

🚧 **Early design & kernel implementation phase**

Current priorities:

* ontology operation specification
* interpreter invariants
* JSON operation schema
* validation and rejection rules

---

## Roadmap (High Level)

* [ ] Ontology schema definition
* [ ] JSON operation spec (v1)
* [ ] Interpreter core
* [ ] SQL / storage compilation adapters
* [ ] Policy engine
* [ ] Action execution framework
* [ ] LLM integration examples

---

## Philosophy (Read This Before Contributing)

> Ontology defines meaning.
> Interpreter defines authority.
> AI defines intent.

If you agree with this separation, you will feel at home here.

---

## License

Open-source.
Apache-2.0.

---
