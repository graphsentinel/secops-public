---
layout: default
title: "SFAMDF Architecture Diagrams"
description: "Visual architecture reference for the Secure, Federated, Adaptable Multi-Agent Defense Framework"
---

# SFAMDF Architecture Visualizations

Visual reference diagrams for the SFAMDF framework components, data flows, and security patterns.

---

## 1. High-Level Defense Architecture

![secops-mcp-arch.svg](./secops-mcp-arch.svg)

---

## 2. Orchestrator-Operator Hierarchy

```mermaid
flowchart TD
    subgraph Strategic["Strategic Layer"]
        O[Orchestrator Agent]
    end

    subgraph Tactical["Tactical Layer"]
        direction LR
        SO1[Sub-Orchestrator\nDomain A]
        SO2[Sub-Orchestrator\nDomain B]
    end

    subgraph Execution["Execution Layer"]
        direction LR
        subgraph Simple["Simple Operators"]
            S1[Deterministic\nFile Reader]
            S2[Deterministic\nAPI Caller]
            S3[Deterministic\nDB Query]
        end
        subgraph Complex["Complex Operators"]
            C1[Generative\nAnalyzer]
            C2[Optimization\nPlanner]
            C3[Generative\nWriter]
        end
    end

    O -->|Delegate| SO1
    O -->|Delegate| SO2
    SO1 -->|Assign| S1
    SO1 -->|Assign| S2
    SO1 -->|Assign| C1
    SO2 -->|Assign| S3
    SO2 -->|Assign| C2
    SO2 -->|Assign| C3

    S1 -.->|Report| SO1
    S2 -.->|Report| SO1
    C1 -.->|Report| SO1
    S3 -.->|Report| SO2
    C2 -.->|Report| SO2
    C3 -.->|Report| SO2
    SO1 -.->|Aggregate| O
    SO2 -.->|Aggregate| O
```

---

## 3. Threat Model - Compromised OODA Loop

```mermaid
flowchart LR
    subgraph OODA["Agent Decision Loop"]
        direction TB
        OBS[OBSERVE\nPerception]
        ORI[ORIENT\nContext]
        DEC[DECIDE\nLogic]
        ACT[ACT\nExecution]
        OBS --> ORI --> DEC --> ACT
        ACT -.->|Feedback| OBS
    end

    subgraph Attacks["Attack Vectors"]
        A1[Adversarial\nExamples]
        A2[Prompt\nInjection]
        A3[Data\nPoisoning]
        A4[Context\nManipulation]
        A5[Reward\nHacking]
        A6[Objective\nMisalignment]
        A7[Tool\nConfusion]
        A8[Output\nHijacking]
    end

    A1 -->|Corrupt| OBS
    A2 -->|Corrupt| OBS
    A3 -->|Corrupt| ORI
    A4 -->|Corrupt| ORI
    A5 -->|Corrupt| DEC
    A6 -->|Corrupt| DEC
    A7 -->|Corrupt| ACT
    A8 -->|Corrupt| ACT
```

---

## 4. AgentBound Framework

```mermaid
flowchart TB
    subgraph Control["Control Plane"]
        AM[AgentManifest]
        TR[Trust Registry]
        PE[Policy Engine]
    end

    subgraph Enforcement["Enforcement Plane"]
        AB[AgentBox Container]
        subgraph Sandbox["Isolated Sandbox"]
            AG[Agent Process]
            MCP[MCP Client]
        end
        SC[seccomp Profile]
        AA[AppArmor Policy]
        NS[Network Filter]
    end

    subgraph Resources["Protected Resources"]
        FS[(File System)]
        NET[Network]
        IPC[IPC Channels]
    end

    AM -->|Define Permissions| PE
    TR -->|Verify Identity| PE
    PE -->|Configure| AB
    AB --> SC & AA & NS
    SC & AA & NS --> Sandbox
    AG --> MCP
    MCP -.->|Intercepted| NS
    MCP -.->|Intercepted| SC
    AG -.->|Intercepted| AA
    NS -->|Filtered| NET
    AA -->|Controlled| FS
    SC -->|Restricted| IPC
```

---

## 5. Defense-in-Depth Layers

```mermaid
flowchart TB
    subgraph L1["Layer 1: Protocol Principles"]
        P1[User Consent]
        P2[Data Privacy]
        P3[Tool Safety]
    end

    subgraph L2["Layer 2: Server Hardening"]
        H1[Authentication]
        H2[Encryption]
        H3[Isolation]
        H4[Secrets Mgmt]
    end

    subgraph L3["Layer 3: Gateway Enforcement"]
        G1[Allowlists]
        G2[Schema Validation]
        G3[Instruction Strip]
        G4[Tier Controls]
    end

    subgraph L4["Layer 4: Runtime Monitoring"]
        M1[Behavioral Baseline]
        M2[Anomaly Detection]
        M3[Honeypot Tools]
        M4[Audit Logging]
    end

    subgraph L5["Layer 5: Agentic Defense"]
        D1[Security Agents]
        D2[Shadow Tooling]
        D3[Counter-Poisoning]
        D4[Circuit Breakers]
    end

    L1 --> L2 --> L3 --> L4 --> L5
```

---

## 6. Conflict Resolution Flow

```mermaid
sequenceDiagram
    participant A1 as Agent A
    participant A2 as Agent B
    participant ARB as Arbitrator
    participant GOV as Governance
    participant LOG as Audit Log

    A1->>A2: Request Resource X
    A2->>A2: Check Local State

    alt No Conflict
        A2-->>A1: Grant Access
        A1->>LOG: Log Access
    else Priority Resolution
        A2->>GOV: Check Priorities
        GOV-->>A2: A1 > A2
        A2-->>A1: Yield Resource
        A2->>LOG: Log Yield
    else Consensus Required
        A1->>ARB: Escalate Conflict
        A2->>ARB: Submit Position
        ARB->>GOV: Evaluate Policies
        GOV-->>ARB: Decision Criteria
        ARB->>A1: Ruling
        ARB->>A2: Ruling
        ARB->>LOG: Log Decision
    end
```

---

## 7. Security Maturity Model

```mermaid
flowchart LR
    subgraph Maturity["SFAMDF Security Maturity Levels"]
        L0[Level 0\nChaos]
        L1[Level 1\nAwareness]
        L2[Level 2\nFoundation]
        L3[Level 3\nDefense\nin Depth]
        L4[Level 4\nContinuous]
        L5[Level 5\nAdaptive]
    end

    L0 -->|Basic\nAllowlists| L1
    L1 -->|Gateway +\nGuardrails| L2
    L2 -->|Layered\nControls| L3
    L3 -->|Red Team +\nCanary| L4
    L4 -->|Automated\nResponse| L5
```

---

## Navigation

- [Back to SFAMDF Overview](../sfamdf/)
- [Reactive vs Proactive Comparison](../comparison/)
- [Home](../)

---

<small>
*These diagrams are designed for inclusion in the SFAMDF Whitepaper and related presentations.*
</small>
