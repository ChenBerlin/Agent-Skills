# Code reading checklist for architecture extraction

Use this checklist to gather architecture evidence before drawing:

1. Service entrypoints
   - HTTP/gRPC router registrations
   - Main/bootstrap files
   - Deployment descriptors and startup scripts

2. Internal modules/capabilities
   - Package boundaries
   - Domain/service/application layers
   - Batch/scheduler/task workers

3. Storage and state
   - Database clients and table ownership
   - Cache usage
   - Search/index stores

4. Integration dependencies
   - Outbound RPC/HTTP clients
   - MQ producer/consumer/topic bindings
   - Event subscriptions
   - Third-party SDK usage

5. Runtime topology hints
   - Container/deployment manifests
   - Environment variable and config keys
   - Service discovery names

6. Lifecycle and planning markers
   - TODO/FIXME/Deprecated markers
   - Feature flags for staged rollout
   - Migration adapters and fallback branches

Capture file evidence for each extracted element and avoid inferring undocumented dependencies.
