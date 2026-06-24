# Decoupled AI Agent Platform via n8n & Model Context Protocol (MCP)

This repository contains a production-ready, decoupled automation architecture utilizing the **Model Context Protocol (MCP)** within n8n. By dividing the system into an isolated **MCP Server** and a conversational **MCP Client**, this setup allows an AI Agent to securely invoke remote tool ecosystems (Gmail integration) over standard network protocols.

---

## 🧭 System Architecture & Framework

Instead of forcing a single workflow to hold all credentials, logic, and triggers, this layout implements an advanced decoupled client-server architecture:
┌────────────────────────┐              ┌────────────────────────┐
│       MCP CLIENT       │              │       MCP SERVER       │
│                        │              │                        │
│  Chat Trigger          │              │  MCP Server Trigger    │
│         │              │              │         │              │
│  AI Agent (Gemini)     │ ──(HTTPS)──> │  Gmail API Tools       │
│  Memory Buffer Window  │              │  - Fetch Mails         │
│  MCP Client Tool       │              │  - Send Mails          │
└────────────────────────┘              └────────────────────────┘

### 1. The MCP Server (`workflows/MCP_Server.json`)
Acts as a secure, stateless execution gatekeeper. It listens via an **MCP Server Trigger** and exposes two operational tools directly derived from your connected **Gmail API infrastructure**:
* `Get many messages in Gmail` (Data extraction)
* `Send a message in Gmail` (Action dispatching)

### 2. The MCP Client (`workflows/MCP_Client.json`)
Acts as the customer-facing brain. It intercepts user commands via a **Chat Trigger**, maintains state through a **Memory Buffer Window**, and leverages **Google Gemini (`gemini-2.5-flash-lite`)**. When the agent determines it needs external data or actions, it invokes the **MCP Client Tool** node to remotely run tools hosted by the Server.

---

## 🛠️ Prerequisites & Credentials

Before taking this infrastructure live, you must possess an instance of n8n supporting advanced LangChain nodes, along with verified account access for:

* **Google Gemini API Key:** Powers the primary LLM reasoning capability on the Client.
* **Gmail API Config (OAuth2):** Attached exclusively to the Server workflow to authorize mailbox queries and email distribution.

---

## 🚀 Step-by-Step Deployment & Configuration

### Step 1: Deploy the MCP Server
1. Create a blank workflow canvas in your n8n instance.
2. Open the options menu (`...`) in the top-right corner and select **Import from File**.
3. Choose the `workflows/MCP_Server.json` file.
4. Open the **MCP Server Trigger** node and define a custom text identifier in the **Path** property (e.g., `gmail-mcp-bridge`).
5. Authorize your **Gmail OAuth2** credentials within both Gmail Tool nodes.
6. Toggle the workflow to **Active**. 
7. Copy your server's production webhook URL base. It will follow this structure:  
   `https://YOUR_SUBDOMAIN.app.n8n.cloud/mcp/YOUR_MCP_SERVER_PATH`

### Step 2: Deploy the MCP Client
1. Create a second blank workflow canvas in your n8n instance.
2. Select **Import from File** from the options menu (`...`) and choose `workflows/MCP_Client.json`.
3. Link your **Google Gemini** API account within the language model node.
4. Open the **MCP Client** node and paste the exact production URL you copied from Step 1 into the **Endpoint URL** field.
