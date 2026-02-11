# Call Chain Output Template

Use this template when delivering call-chain analysis.

## 1) Scope

- Repository/Service:
- Commit/Branch (if known):
- Coverage boundary (all modules or selected modules):
- Tech stack assumptions:

## 2) Entry Inventory (Full List)

Create a table with one row per discovered entrance.

| Entry Type | Entry Name | Implementation Symbol | Module | Notes |
|---|---|---|---|---|
| HTTP | `POST /orders/create` | `OrderController#create` | order | |
| RPC | `OrderRpcService.createOrder` | `OrderRpcServiceImpl#createOrder` | order | |
| MQ Consumer | `order_timeout_consumer_group` | `OrderTimeoutConsumer#onMessage` | order | |
| Scheduler | `orderReconcileJob` | `OrderReconcileJob#execute` | finance | |

## 3) Business Module View

Group call chains by business capability.

---

### Module: <Business Capability Name>

#### Chain: <Business Action>

- **Trigger Type**: HTTP / RPC / MQ / Scheduler / Other
- **Primary Entrances**:
  - `<entry-1>`
- **Equivalent Entrances (Merged Variants)**:
  - `<entry-variant-1>` (difference: method name/DTO only)

**Layered Chain (ordered):**

1. **Entry Layer**: `<class#method>` - receives `<input>` and performs `<validation/auth/routing>`
2. **Service Layer**: `<class#method>` - executes `<business rule>`
3. **Gateway Layer**: `<class#method>` - calls `<external rpc/http/mq>`
4. **Repo Layer**: `<class#method>` - reads/writes `<table/collection/cache>`
5. **Async/Side Effects (if any)**: publishes `<topic>` / schedules `<job>`

**Dependencies and effects:**

- Reads:
- Writes:
- Outbound calls:
- MQ publish:
- Transaction boundary:

**Aggregation rationale:**

- Why entrances are merged:
- Why they are not split:

**Known gaps/assumptions:**

- `<dynamic dispatch unresolved path>`
- `<missing runtime config>`

---

## 4) Cross-Module Handoffs

List important chains that cross module boundaries.

| From Module | To Module | Through | Purpose |
|---|---|---|---|

## 5) Summary

- Total entrances found:
- Total canonical chains after aggregation:
- High-risk / high-complexity chains:
