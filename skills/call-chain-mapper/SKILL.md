---
name: call-chain-mapper
description: Analyze a codebase and produce complete project call chains grouped by business capability, covering entry points (HTTP/RPC/MQ/scheduled jobs), layered paths (entry/service/gateway/repo), and deduplicated similar flows. Use when users ask for invocation chain mapping, dependency path documentation, or architecture call-flow summaries from source code.
---

# Call Chain Mapper

Build a business-oriented invocation chain map from code evidence, then explain each chain in clear prose.

## Workflow

1. Define scope and stack assumptions.
   - Confirm repository root, language/framework, and whether output should cover all modules or selected domains.
   - If uncertain, start from runnable entry modules and dependency manifests.

2. Discover all entry points.
   - Enumerate external-facing and trigger-based entrances, including but not limited to:
     - HTTP controllers/routes
     - RPC providers/handlers
     - MQ consumer groups/listeners
     - Scheduled jobs/cron tasks
     - CLI batch jobs and workflow engines (if present)
   - Record code evidence (file path + symbol/class/function).

3. Trace layered call paths.
   - For each entry, follow the main execution path across layers:
     - Entry layer (controller/handler/listener/job)
     - Service layer (application/domain services)
     - Gateway/adapter layer (RPC client, MQ producer, HTTP client, third-party SDK)
     - Repo/data layer (DAO/repository/mapper/query layer)
   - Track branching conditions and async handoffs when they change business outcomes.

4. Aggregate highly similar chains.
   - Merge chains when only surface fields differ (for example route name, RPC method name, DTO shape) but business logic is substantially identical.
   - Keep one canonical chain and list merged variants under “Equivalent Entrances”.
   - Do not merge if business intent, side effects, or critical dependencies differ.

5. Group chains by business capability.
   - Organize output into functional modules (for example: Order Lifecycle, Payment, Risk Control, Notification).
   - Place each chain into exactly one primary module; cross-reference secondary impacts if needed.

6. Produce textual chain documentation.
   - Use the output format in `references/output-template.md`.
   - For each chain, include:
     - Module name and chain name
     - Entrances and trigger type
     - Ordered layered steps (Entry -> Service -> Gateway -> Repo)
     - Key dependencies and side effects (DB write, outbound RPC, MQ publish)
     - Aggregation notes (merged variants)
     - Known gaps/assumptions

## Evidence and quality rules

- Ground every chain in code evidence; avoid architecture assumptions without source proof.
- Prefer breadth first (cover all entrances), then depth (critical chains in detail).
- Explicitly mark unresolved dynamic dispatch/reflection/DI ambiguity.
- When frameworks auto-wire routes or consumers, cite the registration source.
- Keep chain names business-meaningful, not only class/method names.

## Minimum output checklist

Before finishing, verify all are true:

- All discovered HTTP/RPC/MQ/scheduler entrances are represented.
- Every chain includes entry/service/gateway/repo (or explicitly states missing layer).
- Repetitive chains are merged with clear “Equivalent Entrances”.
- Chains are grouped by business module rather than technical package only.
- Unknowns and assumptions are explicitly listed.
