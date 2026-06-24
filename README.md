# Decoupled AI Agent Platform via n8n & Model Context Protocol (MCP)

This repository contains a production-ready, decoupled automation architecture utilizing the **Model Context Protocol (MCP)** within n8n. By dividing the system into an isolated **MCP Server** and a conversational **MCP Client**, this setup allows an AI Agent to securely invoke remote tool ecosystems (Gmail integration) over standard network protocols.

---

## 🧭 System Architecture & Framework

Instead of forcing a single workflow to hold all credentials, logic, and triggers, this layout implements an advanced decoupled client-server architecture:
