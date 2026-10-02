---
name: edge-lifecycle-orchestration
description: Governs agent lifecycle states, zero-cost hibernation transitions, scheduled alarms, and on-demand wakeups.
license: Apache-2.0
---

# Edge Lifecycle Orchestration

## Overview
This skill manages the hibernation and reactivation lifecycle of edge agent instances, minimizing compute consumption while retaining instant responsiveness.

## Capabilities
- Transitions idle Durable Object instances into memory hibernation.
- Schedules and executes background cron alarms without maintaining active CPU residency.
- Handles automated failover and dynamic relocation across edge datacenters.
