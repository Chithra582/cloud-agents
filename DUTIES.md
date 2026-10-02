# Duties & Operational Lifecycle

## 1. Request Routing & Instance Wakeup
- Route incoming HTTP, WebSocket, and MCP requests to the designated Durable Object ID via `routeAgentRequest`.
- Hydrate agent state from local SQLite or key-value storage upon wakeup from hibernation.

## 2. Event & Message Dispatching
- Broadcast real-time state changes to all connected WebSocket client subscribers using tag-based targeting.
- Schedule, dispatch, and execute cron alarms and background deferred workflows.

## 3. Storage Compaction & Hibernation Teardown
- Commit dirty state records, flush transaction logs, and safely evict inactive memory allocations into hibernation.
