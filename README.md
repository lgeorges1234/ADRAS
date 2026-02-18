# 🛡️ A.D.R.A.S. Framework
### **Advanced Document-driven Reconnaissance & Audit System**

## 📖 1. Project Overview
**A.D.R.A.S.** (Advanced Document-driven Reconnaissance & Audit System) is a sophisticated, agentic offensive security orchestrator. It is designed to automate the critical "Pre-Exploitation" phases of a vulnerability assessment, including **Information Gathering**, **Threat Modeling**, and **Vulnerability Evaluation**.

Unlike traditional static scripts, A.D.R.A.S. leverages a **Recursive Reasoning Loop**. It doesn't just execute commands; it analyzes the output, consults its internal knowledge base of security manuals, and dynamically adjusts its strategy. The system is built to bridge the gap between high-level LLM intelligence (Control Plane) and low-level security tool execution (Data Plane).

---

## 🏗️ 2. Architecture: The Hybrid Execution Model
A.D.R.A.S. separates logic from execution to ensure safety, modularity, and cross-platform compatibility.

### **A. Control Plane (Windows Workstation)**
* **The Brain**: Runs the LangGraph orchestrator and the LLM.
* **Logic Center**: Manages the global state, processes RAG (Retrieval-Augmented Generation) lookups, and decides the next tactical move.
* **Environment**: Python 3.12 LTS.

### **B. Data Plane (Kali Linux)**
* **The Muscle**: A remote or virtualized Kali Linux instance pre-loaded with auditing tools (Nmap, Metasploit, etc.).
* **Execution**: Receives commands from the Control Plane and returns raw telemetry.

### **C. The SSH Bridge (Command Orchestration)**
Communication is handled via a secure, asynchronous SSH tunnel using the `asyncssh` library. 



1.  **Generation**: An agent identifies a target and selects a tool (e.g., Nmap).
2.  **Dispatch**: The command is sent via SSH to the Data Plane.
3.  **Return**: The stdout/stderr or file output is sent back to the Control Plane for parsing.

---

## 🧠 3. Memory & Data Management
A.D.R.A.S. implements a dual-layer memory system to differentiate between session-specific data and long-term project intelligence.

### **Layer 1: Short-Term State (Conversation & Session)**
* **Storage**: `adras-framework/data/core/checkpoints.db` (SQLite).
* **Purpose**: Persistent "Thread" memory. It stores the current graph state, message history, and the progress of the current scan. This allows the system to resume an audit even after a crash or restart.

### **Layer 2: Long-Term Memory (Project & Global Intelligence)**
* **Storage**: `adras-framework/data/intelligence/` (FAISS Vector Store).
* **Purpose**: The "Cumulative Knowledge Base." 
    * **Tool Manuals**: Vectorized documentation for Nmap, NSE scripts, and man pages.
    * **Experience**: Historical findings from past audits (e.g., recurring vulnerabilities found on specific subnets).

### **D. Folder Organization**
```text
ADRAS/
├── assets/                 # INTAKE: Raw user files (PDFs, TXT, CSV)
├── venvadras/              # Python 3.12 Environment
├── .env                    # Secrets (API Keys, SSH Credentials)
└── adras-framework/
    ├── main.py             # System Entry Point
    ├── agents/             # Specialized Agent Logic (Recon, OSINT, etc.)
    ├── core/               # Graph Definitions & SSH Connection Logic
    └── data/
        ├── core/           # SQLite Checkpointers (Short-term)
        ├── intelligence/   # Vector Stores & Manuals (Long-term)
        ├── raw_outputs/    # Raw Telemetry (XML/JSON/Logs)
        └── intake/         # Normalized Engagement Scope