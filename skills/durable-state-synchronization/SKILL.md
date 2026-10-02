---
name: durable-state-synchronization
description: Manages ACID transactional state persistence and reactive delta synchronization using Cloudflare Durable Objects and SQLite.
license: Apache-2.0
---

# Durable State Synchronization

## Overview
This skill orchestrates transactional state updates, SQLite table migrations, and real-time state broadcasts across client frontends.

## Capabilities
- Executes atomic read-modify-write operations on embedded SQLite tables.
- Emits serialized state diffs to subscribed client connections upon mutation.
- Maintains single-writer consistency across globally distributed edge access points.
