---
name: code-architecture-drawio
description: Generate architecture diagrams in draw.io XML by reading and understanding project codebases, mapping services/modules/data flows/dependencies, and producing layered diagrams with explicit legends. Use when users request architecture analysis, system decomposition, or draw.io-compatible XML outputs from source code.
---

# Code Architecture to draw.io XML

Produce a reliable architecture diagram from real code evidence, not assumptions.

## Workflow

1. Scope the target system.
   - Confirm repository root, target services, and requested level of detail.
   - Prefer service + internal module/capability granularity when a service is multi-functional.
   - Interface/service nodes may be aggregated by business capability; node labels should describe implemented functionality to avoid over-complex diagrams.

2. Read code and extract architecture facts.
   - Identify deployable services, modules, data stores, MQ topics, third-party dependencies, APIs, and scheduled jobs.
   - Record evidence with file paths.
   - Distinguish:
     - Control invocation relationships.
     - Data flow relationships.
     - Sync vs async messaging.

3. Build a model before drawing.
   - Group nodes by layers/domains (for example: entry, application, domain services, data, external systems).
   - Mark lifecycle status per node:
     - Current (white background)
     - Historical/last phase changed (blue background)
     - Planned (red background)
     - External dependency (yellow background)
     - Deprecated/to remove (strikethrough label)
   - Keep line-minimal diagrams; omit obvious cross-layer lines when layering already communicates dependency.

4. Generate draw.io XML.
   - Start from `assets/drawio-template.xml` and replace placeholder nodes/edges.
   - Keep IDs stable and unique.
   - Save the final diagram as a `.drawio`/`.xml` file (not prose-only output).
   - Ensure XML imports successfully in draw.io.

5. Validate completeness.
   - Verify every key relationship in the architecture summary appears in the XML.
   - Verify legend exists and fully explains node/edge semantics.
   - Verify direction conventions and line styles are consistent.

## Mandatory diagram rules

Apply these rules unless the user explicitly overrides them.

### Naming

- Prefer `service-name` using `appkey` suffix style.
- Omit prefixes such as `com.sankuai.mall` from labels (keep suffix/appkey-identifiable names first).

### Direction and semantics

- Default to data-flow diagrams for producer/processing/query pipelines.
- For query RPC where A queries B, draw arrow from B to A when expressing data flow (data provider -> data consumer).
- Use consistent arrow semantics and explain them in legend.
- Include necessary inter-service links, but do not force full-mesh connectivity when it harms readability.

### Line styles

- Use dashed lines for message-based async links.
- Use solid lines for non-message links.
- Use multiple solid-line colors when needed; explain each color in legend.
- Use blue as primary line color (Google Cloud Platform style preference).

### Node/group styles

- White fill: current-state service/component.
- Blue fill: previous-stage changed/new service.
- Red fill: planning items.
  - Solid-border red item: plan for existing service.
  - Dashed-border red item: not yet built service/application.
- Yellow fill: external dependencies.
- Strikethrough text: removed or planned-for-removal service/component.
- Use dashed small groups for local sub-grouping.
- Use GCP Zones-style background for larger groups.
- Overall layout does not need to be perfectly rectangular; prioritize readability and flow.

### Legend (required)

Always include a dedicated "Legend" area in the diagram that explains:

- All edge types (solid/dashed, color meanings, arrow semantics).
- All node background colors and border/text conventions.
- Optional extensions added for this diagram (for example, reading direction, layer meaning, ownership markers).
- Besides the standard legend, add any extra legend types needed (for example layered meaning, reading direction guidance).

## Output contract

When delivering output:

1. Provide architecture findings summary (service/module/dependency list) with code evidence references.
2. Provide the complete draw.io XML as a `.drawio`/`.xml` file output; if also shown inline, keep it in a fenced `xml` block.
3. Ensure XML contains a legend node/group.
4. Mention assumptions and unknowns explicitly.

## Quality checklist

Before finishing, confirm all are true:

- Diagram granularity is sufficient (service internals are decomposed when needed, while interfaces/services can be capability-aggregated for readability).
- Messaging links are dashed, non-messaging links are solid.
- Main lines are blue unless a documented alternate color is needed.
- Lifecycle statuses use the required background conventions.
- Legend fully explains every visual encoding used.
- XML is importable by draw.io without manual repair.
