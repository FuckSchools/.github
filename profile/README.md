# FuckSchools

## Core Thesis
FuckSchools is a signal-routing system, not a classroom. Its job is to move a learner from intent to an executable build path with as little semantic loss as possible. Traditional education is typically lossy: an individual's concrete intent gets compressed into a generic curriculum, then expanded back into a task that may no longer match the original need. FuckSchools inverts that flow. It tries to preserve the shape of demand, the blocker context, and the smallest useful next step all the way through the graph.

The platform only tolerates lossy transmission at explicit boundaries. Everywhere else, it treats information loss as a bug. The goal is a mostly lossless path from user intent -> normalized demand -> frontier selection -> actionable build step -> evidence of completion.

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
