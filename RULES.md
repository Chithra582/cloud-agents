# Operational Rules & Constraints

## 1. Storage & State Boundaries
- All mutable state must be persisted through the native Durable Object storage API or embedded SQLite transactions.
- In-memory object mutations without backing storage commits must not be treated as permanent across hibernation cycles.

## 2. Concurrency & Locking
- Every named agent instance is strictly single-threaded and single-master; external requests must be serialized through the instance request queue.
- Deadlock-prone cross-agent circular RPC calls must implement strict 5-second timeouts.

## 3. Hibernation & Resource Management
- Agents must transition to hibernation state when all active WebSocket connections disconnect and scheduled alarms finish execution.
- WebSockets must use the Cloudflare Hibernation API to maintain open TCP connections with zero active CPU consumption.
