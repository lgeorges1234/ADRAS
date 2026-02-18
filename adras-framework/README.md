A.D.R.A.S. Framework
Advanced Document-driven Reconnaissance & Audit System

A.D.R.A.S. is an agentic offensive security orchestrator built on LangGraph. It automates the initial phases of a penetration test (Information Gathering, Threat Modeling, and Vulnerability Evaluation) by bridging a Windows-based Control Plane with a Kali Linux Data Plane via SSH.

🤖 System Architecture & Graph Flow
The system operates as a Cyclic Directed Acyclic Graph (DAG). Unlike linear scripts, A.D.R.A.S. can backtrack to the Documentalist agent to refine its strategy based on discovered services.

Key Agents
Intake Analyst: Ingests user-provided files and scopes the engagement.

OSINT Specialist: Performs passive reconnaissance (Shodan, GitHub, WHOIS).

The Strategist (Router): The decision engine that evaluates stealth_level vs. discovered assets to choose the next node.

Documentalist (Phase 0): A RAG-powered agent that parses local Nmap documentation and NSE scripts to prevent hallucinations.

Recon/Auditor: Executes targeted scans and evaluates vulnerabilities.

🛡️ Operational Constraints: Stealth Scaling
The framework adheres to a 1-10 Operational Footprint scale:

Lvl 1-3 (Passive): Zero packets sent to target; OSINT only.

Lvl 4-7 (Balanced): Version detection, non-intrusive scripts, T3 timing.

Lvl 8-10 (Aggressive): Full vulnerability exploitation scripts and T5 timing.