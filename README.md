<div align="center">

# Autonomous SRE Command Center
### Multi-Agent Kubernetes Incident Response with Gemini 4 Argon, Google ADK, Neo4j Knowledge Graphs & MCP

[![Gemini 4 Argon](https://img.shields.io/badge/Model-Gemini%204%20Argon-4285F4?style=for-the-badge&logo=google&logoColor=white)](https://ai.google.dev/)
[![Google ADK](https://img.shields.io/badge/Orchestration-Google%20ADK-34A853?style=for-the-badge&logo=googlecloud&logoColor=white)](https://google.github.io/adk-docs/)
[![Neo4j](https://img.shields.io/badge/Knowledge%20Graph-Neo4j%205.x-008CC1?style=for-the-badge&logo=neo4j&logoColor=white)](https://neo4j.com/)
[![MCP](https://img.shields.io/badge/Tooling-Model%20Context%20Protocol-FF6F00?style=for-the-badge)](https://modelcontextprotocol.io/)
[![Kubernetes](https://img.shields.io/badge/Target-Kubernetes%20%2F%20GKE-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

**Compress 45-minute Kubernetes war-room investigations into a 30-second, graph-grounded, human-verified remediation workflow.**

[Features](#-key-features) • [Architecture](#-system-architecture) • [Chaos Scenarios](#-chaos-engineering-testbed) • [Quick Start](#-quick-start) • [Production Roadmap](#-full-stack-production-roadmap)

</div>

---

## Overview

When a production Kubernetes cluster begins throwing 5xx errors at 3:00 AM, the bottleneck isn't restarting a pod—it is **long-horizon context correlation**. Site Reliability Engineers lose critical minutes jumping between Prometheus metrics, `kubectl describe` events, container logs, and zero-trust NetworkPolicies just to separate cascading symptoms from the underlying root cause.

The **Autonomous SRE Command Center** combines **Gemini 4 Argon (`gemini-4-argon`)** with **Google Agent Development Kit (ADK)**, a **Neo4j Kubernetes Knowledge Graph**, and **Model Context Protocol (MCP)** tools to automate multi-hop root cause analysis, calculate deterministic blast-radius scores, and synthesize least-privilege remediations behind a strict **Human-in-the-Loop (HITL)** governance gate.

---

## Key Features

- **Real-Time GKE Cluster Telemetry Matrix:** Live monitoring of CPU, Memory, Network I/O, 5xx error rates, GKE nodes (`gke-prod-node-1..3`), namespaces, deployments, pods, and CloudSQL connection pools.
- **Neo4j Kubernetes Knowledge Graph & Blast Radius Engine:** Models `(Cluster)->(Node)->(Namespace)->(Deployment)->(Pod)->(Service)->(Database)` relationships with interactive multi-hop blast-radius highlighting and an inspectable Cypher query drawer.
- **5 Specialized Google ADK Agents (`gemini-4-argon`):**
  1. **Incident Investigation Agent (`thinking_level="LOW"`):** High-speed triage correlating Kubernetes events, container logs, and Prometheus metrics.
  2. **Root Cause Analysis (RCA) Agent (`thinking_level="HIGH"`):** Executes multi-hop Cypher traversals in Neo4j to isolate root faults from upstream cascading failures.
  3. **Security & Zero-Trust Agent (`thinking_level="HIGH"`):** Audits proposed operations against Kubernetes RBAC, NetworkPolicies, and Google SAIF guardrails.
  4. **Remediation Agent (Structured JSON):** Generates surgical `kubectl`, Helm, or pool-scaling remediation commands.
  5. **Verification Agent (`thinking_level="LOW"`):** Validates post-remediation telemetry and SLO recovery.
- **8 Built-in Chaos Engineering Fault Injectors:** One-click simulation of real-world Kubernetes outages.
- **Human-in-the-Loop (HITL) Approval & RBAC Gate:** Enforces `ADMIN`, `OPERATOR`, and `READ-ONLY` role boundaries. Exploits Gemini 4 Argon's native irreversible-action pause so no cluster mutation occurs without explicit human sign-off.
- **Cryptographic Audit Trail:** Immutable log of every chaos event, MCP tool call, Cypher query, and operator approval signature.

---

## System Architecture

```mermaid
flowchart TB
    subgraph UI["Autonomous SRE Command Center (React + Tailwind)"]
        Telemetry["Real-Time GKE Telemetry"]
        GraphUI["Interactive Neo4j Topology & Blast Radius"]
        Timeline["Live ADK Reasoning & MCP Timeline"]
        HITL["RBAC & HITL Approval Gate"]
    end

    subgraph ADK["Google ADK Multi-Agent Orchestrator (gemini-4-argon)"]
        Inv["1. Investigation Agent\n(thinking_level: LOW)"]
        RCA["2. Root Cause Analysis Agent\n(thinking_level: HIGH)"]
        Sec["3. Security & Zero-Trust Agent\n(thinking_level: HIGH)"]
        Rem["4. Remediation Agent\n(Structured Output)"]
        Ver["5. Verification Agent\n(thinking_level: LOW)"]

        Inv --> RCA --> Sec --> Rem --> Ver
    end

    subgraph MCP["Model Context Protocol (MCP) Tool Layer"]
        K8sMCP["k8s-inspect-mcp\n(Events, Logs, Describe)"]
        Neo4jMCP["neo4j-cypher-mcp\n(Multi-Hop Graph Queries)"]
        PromMCP["prom-metrics-mcp\n(Golden Signals)"]
        ExecMCP["k8s-remediate-mcp\n(Gated Patch/Rollout)"]
    end

    subgraph Infra["Target Infrastructure & Graph Store"]
        GKE[("GKE Cluster\n(Nodes, Pods, NetPol)")]
        Neo4j[("Neo4j 5.x\nKnowledge Graph")]
        CloudSQL[("CloudSQL\nConnection Pool")]
    end

    UI <--> ADK
    ADK <--> MCP
    HITL -.->|"Cryptographic Approval Required"| ExecMCP
    K8sMCP & ExecMCP <--> GKE
    Neo4jMCP <--> Neo4j
    PromMCP <--> GKE & CloudSQL
