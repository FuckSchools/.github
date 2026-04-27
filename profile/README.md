# FuckSchools — v3.1

## Core Thesis
FuckSchools is a signal-routing system, not a classroom. Its job is to move a learner from intent to an executable build path with as little semantic loss as possible. Traditional education is typically lossy: an individual's concrete intent gets compressed into a generic curriculum, then expanded back into a task that may no longer match the original need. FuckSchools inverts that flow. It tries to preserve the shape of demand, the blocker context, and the smallest useful next step all the way through the graph.

The platform only tolerates lossy transmission at explicit boundaries. Everywhere else, it treats information loss as a bug. The goal is a mostly lossless path from user intent → normalized demand → frontier selection → actionable build step → evidence of completion.

## Axioms
- The platform does not teach.
- It listens for intent before it emits content.
- Demand is the primary signal.
- Curriculum is a fallback, not the organizing principle.
- The smallest useful next step is preferred.
- Knowledge transfer between people is expensive.
- Signal loss is a design defect unless a boundary requires it.
- Lossless transmission of intent is the ideal.
- Legibility matters more than ceremony.
- Ceremony may exist only when it improves coordination.
- Hard blocks are first-class graph facts.
- Traversal is demand-driven, never syllabus-driven.
- The frontier is more important than the history behind it.
- State should be explicit, persisted, and inspectable.
- Completion is backed by evidence, not vibes.
- Repetition should be derived from blockers, not from arbitrary loops.
- Revisit only when new evidence changes the frontier.
- Build momentum matters more than credential accumulation.
- Recursive decomposition is preferred over monolithic tasks.
- Only the next useful frontier matters.

## Node Types
- Root: the top-level project entry point for a tree.
- Need: a concrete user demand or objective that initiated traversal.
- Goal: the target build outcome the tree is trying to reach.
- Constraint: a rule, limitation, or scope boundary that narrows traversal.
- Blocker: an unresolved hard stop that prevents progress.
- Step: the smallest actionable unit that can be executed immediately.
- Evidence: a proof artifact that confirms a step or completion state.
- Project: the bounded execution container for a traversal.
- Session: a live traversal episode tied to one project.
- Thread: a conversational or operational stream inside a session.
- Frontier: the next unresolved node that should receive attention.
- CompletionState: the current status of a node, step, or blocker.
- GoalContext: the structured carrier of active goal state, constraints, and frontier metadata passed between layers during traversal.

## Architecture — Layer Ownership

### Backend
The backend owns observation, traversal, and persistence. It operates the **Passive Observation Layer**, which collects MCP signals from active sessions without interrupting execution flow. It runs **DDRT** (Demand-Driven Recursive Traversal) to decompose goals into the smallest executable steps and drives graph traversal. All state mutations, blocker resolution, and evidence persistence are backend responsibilities.

### Frontend
The frontend owns rendering and event surface. It implements the **Native Renderer**, which consumes traversal state and emits **EventTrace** records for every meaningful user interaction. EventTrace is the canonical audit log of the render layer, enabling replay, debugging, and behavioral analysis without touching backend state.

### .github (this repository)
This repository is the organization-level documentation hub for FuckSchools. It owns the canonical specification, architecture decisions, and shared contracts that all repositories in the org depend on.

## v3.1 Structural Contracts

**Zod Structural Enforcement** is applied at all layer boundaries. Every payload crossing a layer boundary — MCP signal ingress, GoalContext handoff, EventTrace emission — must pass Zod schema validation before being processed. Schema violations are hard errors, not warnings. This ensures that information loss at boundaries is explicit and caught at the earliest possible point.

## v3.1 Copilot Fixes

This version incorporates targeted Copilot **security and performance fixes**:
- Copilot-suggested completions are gated through the same Zod schemas as external inputs; no raw Copilot output is trusted at a boundary.
- Observation loop performance is improved by batching MCP signal flushes rather than emitting on every tick.
- GoalContext objects are treated as immutable snapshots; Copilot-assisted mutations that bypass the canonical update path are rejected.
