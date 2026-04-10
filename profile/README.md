# GraphReFly

**Reactive graph protocol for human + LLM co-operation.**

GraphReFly makes agent workflows reactive, resumable, and causally explainable — the harness layer between your LLM and production.

---

### The problem

LLM agents today are fragile: state is lost between sessions, errors are unexplainable ("the model said so"), and changes have unknown blast radius. Most agent frameworks give you **frozen snapshots** — not a **living workspace**.

GraphReFly is a reactive graph runtime where:
- **State pushes** — no polling, no stale context, no re-reading
- **Causality persists** — not just *what* happened, but *why*
- **Humans and LLMs co-operate symmetrically** — describe in natural language, review visually, run persistently, ask "why?"

### The bet

Long-running human + LLM reactive co-operation will become the dominant software pattern. GraphReFly is the minimal correct substrate for it.

---

### Architecture at a glance

```
Natural language → GraphSpec → Flow view → Run → Persist → Explain
```

- **GraphSpec** — declarative, LLM-readable/writable, structurally diffable graph representation
- **Two-phase push propagation** — glitch-free, diamond-safe, consistent derived state
- **`describe()` + `observe()`** — full topology + values + causal chains, live at any moment
- **`autoCheckpoint` + `snapshot/restore`** — close the app, reopen, resume exactly where you left off
- **Actor/Guard ABAC** — per-node access control for multi-tenant and multi-agent scenarios

### Harness engineering

GraphReFly is the **reactive harness layer for agent workflows**. The eight requirements of a production agent harness don't map one-to-one with eight APIs — they cluster into a handful of composed blocks that sit on top of the reactive graph primitives:

| Need | GraphReFly |
|---|---|
| Context & state | `persistentState()` — `autoCheckpoint` + `snapshot` / `restore` + incremental diff |
| Agent memory | `agentMemory()` — `distill` + vectors + knowledge graph + tiers, with OpenViking decay |
| Control flow & resilience | `resilientPipeline()` — encodes the correct `rateLimiter → breaker → retry → timeout → fallback` ordering |
| Execution & policy | `guardedExecution()` — Actor / Guard ABAC + `policy()` + `budgetGate` + scoped describe |
| Observability & causality | `graphLens()` — reactive topology, health, flow, and `why(node)` causal chains as structured data |
| Human governance | `gate` — reactive `pending` / `isOpen` with `approve` / `reject` / `modify(fn, n)` |
| Verification | Multi-model eval harness with regression gates |
| Continuous improvement | Strategy model: `rootCause × intervention → successRate` |

The library computes structured facts reactively; LLMs and UIs render them. Natural language is never the library's job — which is what keeps the whole stack model-agnostic and testable.

### Repositories

| Repo | Description |
|---|---|
| [graphrefly](https://github.com/graphrefly/graphrefly) | Behavior spec (`GRAPHREFLY-SPEC.md`) and composition guide |
| [graphrefly-ts](https://github.com/graphrefly/graphrefly-ts) | TypeScript implementation — `@graphrefly/graphrefly-ts` on npm |
| [graphrefly-py](https://github.com/graphrefly/graphrefly-py) | Python implementation (parity track) |

### Key concepts

- **`node`** — minimal compute unit with declared dependencies. State, derived, producer, operator, or effect.
- **Explicit edges** — dependency as data. Enables structural diff, causal trace, LLM-safe composition.
- **Propagation** — change upstream → downstream notified automatically. No manual coordination.
- **GraphSpec** — JSON schema simple enough that if a junior dev can write it by hand from a 1-page guide, an LLM can generate it zero-shot. No fine-tuning needed.

### Why not plain functions?

Plain functions work for linear flows. Problems appear with multiple sources, concurrent updates, feedback loops, and persistent state — exactly the scenarios agent workflows produce. GraphSpec constrains composition to structural operations, reducing the error space like SQL constrains database operations.

### Who is this for?

- **Agent builders** who need observable, resumable, causally explainable workflows
- **Teams** running LLMs in production who need audit trails and policy enforcement
- **Anyone** drowning in information who wants a reactive reduction engine — massive info → actionable items

---

<sub>Built by [David Chen](https://github.com/clfhhc). Pre-1.0 — APIs evolving fast.</sub>
