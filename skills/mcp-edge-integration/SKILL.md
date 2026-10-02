---
name: mcp-edge-integration
description: Integrates Model Context Protocol (MCP) servers and clients at the edge to expose persistent tools and resources.
license: Apache-2.0
---

# MCP Edge Integration

## Overview
This skill enables edge agents to function as lightweight, distributed MCP servers or clients, interconnecting external AI models with persistent edge tools.

## Capabilities
- Exposes callable agent methods as registered MCP tools over SSE and Streamable HTTP.
- Serializes and deserializes JSON-RPC 2.0 messages within edge Worker isolates.
- Authenticates external LLM queries and validates input schemas.
