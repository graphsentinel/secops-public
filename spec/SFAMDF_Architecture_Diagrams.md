---
title: "SFAMDF Architecture Diagrams"
---

# SFAMDF Architecture Visualizations

## 1. High-Level Defense Architecture

```mermaid
flowchart TB
    subgraph Users["👤 Users & Applications"]
        U1[Human Users]
        U2[Application Clients]
    end

    subgraph Orchestration["🎯 Orchestration Layer"]
        ORC[Orchestrator Agent]
        GOV[Governance Engine]
        TR[Trust Registry]
    end

    subgraph Gateway["🛡️ Security Gateway Layer"]
        PG[Policy Gateway]
        IV[Input Validator]
        OS[Output Sanitizer]
        FW[Flow Watcher]
    end

    subgraph Operators["⚙️ Operator Agent Layer"]
        direction LR
        OP1[Deterministic\nOperator]
        OP2[Generative\nOperator]
        OP3[Optimization\nOperator]
    end

    subgraph Runtime["📦 AgentBox Runtime"]
        AB1[AgentBox\nSandbox 1]
        AB2[AgentBox\nSandbox 2]
        AB3[AgentBox\nSandbox 3]
    end

    subgraph MCP["🔌 MCP Server Layer"]
        MCP1[Internal\nMCP Server]
        MCP2[Partner\nMCP Server]
        MCP3[Public\nMCP Server]
    end

    subgraph Tools["🔧 External Tools"]
        T1[(Database)]
        T2[API]
        T3[File System]
    end

    subgraph Monitoring["👁️ Security Monitoring"]
        SA[Security Agent]
        AD[Anomaly Detector]
        HP[Honeypot Tools]
        AL[Audit Logger]
    end

    U1 --> ORC
    U2 --> ORC
    ORC <--> GOV
    GOV <--> TR
    ORC --> PG
    PG --> IV
    IV --> OS
    OS --> FW
    FW --> OP1 & OP2 & OP3
    OP1 --> AB1
    OP2 --> AB2
    OP3 --> AB3
    AB1 --> MCP1
    AB2 --> MCP2
    AB3 --> MCP3
    MCP1 --> T1
    MCP2 --> T2
    MCP3 --> T3
    
    PG -.-> SA
    FW -.-> AD
    AB1 & AB2 & AB3 -.-> AL
    MCP3 -.-> HP
    SA --> AD
    AD --> AL
    HP --> SA
```

## 2. Orchestrator-Operator Hierarchy

```mermaid
flowchart TD
    subgraph Strategic["Strategic Layer"]
        O[🎯 Orchestrator Agent]
        style O fill:#2B579A,color:#fff
    end

    subgraph Tactical["Tactical Layer"]
        direction LR
        SO1[Sub-Orchestrator\nDomain A]
        SO2[Sub-Orchestrator\nDomain B]
        style SO1 fill:#5B9BD5,color:#fff
        style SO2 fill:#5B9BD5,color:#fff
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

## 3. Threat Model - Compromised OODA Loop

```mermaid
flowchart LR
    subgraph OODA["Agent Decision Loop"]
        direction TB
        OBS[👁️ OBSERVE\nPerception]
        ORI[🧭 ORIENT\nContext]
        DEC[🧠 DECIDE\nLogic]
        ACT[⚡ ACT\nExecution]
        OBS --> ORI --> DEC --> ACT
        ACT -.->|Feedback| OBS
    end

    subgraph Attacks["Attack Vectors"]
        A1[🔴 Adversarial\nExamples]
        A2[🔴 Prompt\nInjection]
        A3[🔴 Data\nPoisoning]
        A4[🔴 Context\nManipulation]
        A5[🔴 Reward\nHacking]
        A6[🔴 Objective\nMisalignment]
        A7[🔴 Tool\nConfusion]
        A8[🔴 Output\nHijacking]
    end

    A1 -->|Corrupt| OBS
    A2 -->|Corrupt| OBS
    A3 -->|Corrupt| ORI
    A4 -->|Corrupt| ORI
    A5 -->|Corrupt| DEC
    A6 -->|Corrupt| DEC
    A7 -->|Corrupt| ACT
    A8 -->|Corrupt| ACT

    style A1 fill:#D73B3E,color:#fff
    style A2 fill:#D73B3E,color:#fff
    style A3 fill:#D73B3E,color:#fff
    style A4 fill:#D73B3E,color:#fff
    style A5 fill:#D73B3E,color:#fff
    style A6 fill:#D73B3E,color:#fff
    style A7 fill:#D73B3E,color:#fff
    style A8 fill:#D73B3E,color:#fff
```

## 4. AgentBound Framework

```mermaid
flowchart TB
    subgraph Control["Control Plane"]
        AM[📋 AgentManifest]
        TR[🔐 Trust Registry]
        PE[⚖️ Policy Engine]
    end

    subgraph Enforcement["Enforcement Plane"]
        AB[📦 AgentBox Container]
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

    style AM fill:#70AD47,color:#fff
    style AB fill:#5B9BD5,color:#fff
    style PE fill:#ED7D31,color:#fff
```

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

    style L1 fill:#2B579A,color:#fff
    style L2 fill:#5B9BD5,color:#fff
    style L3 fill:#70AD47,color:#fff
    style L4 fill:#ED7D31,color:#fff
    style L5 fill:#D73B3E,color:#fff
```

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

    style L0 fill:#D73B3E,color:#fff
    style L1 fill:#ED7D31,color:#fff
    style L2 fill:#FFC000,color:#000
    style L3 fill:#92D050,color:#000
    style L4 fill:#00B0F0,color:#fff
    style L5 fill:#2B579A,color:#fff
```

## 8. Tool Classification Matrix

```mermaid
quadrantChart
    title Tool Risk Assessment
    x-axis Low Capability --> High Capability
    y-axis Low Trust --> High Trust
    quadrant-1 Monitor Closely
    quadrant-2 Full Isolation
    quadrant-3 Standard Controls
    quadrant-4 Privileged Access
    
    Internal-DB: [0.8, 0.85]
    Partner-API: [0.6, 0.5]
    Public-Weather: [0.2, 0.3]
    External-Search: [0.4, 0.2]
    Admin-Tool: [0.9, 0.9]
    File-Reader: [0.3, 0.7]
```

## 9. Implementation Timeline

```mermaid
gantt
    title SFAMDF Implementation Roadmap
    dateFormat  YYYY-MM
    section Phase 1: Foundation
    Hardening Standards    :done, p1a, 2026-01, 30d
    Threat Audit          :done, p1b, after p1a, 30d
    Structured Logging    :active, p1c, after p1b, 30d
    section Phase 2: Integration
    AgentBound Pilot      :p2a, 2026-04, 60d
    Gateway Deployment    :p2b, after p2a, 60d
    Anomaly Detection     :p2c, after p2b, 60d
    CI/CD Security        :p2d, after p2c, 60d
    section Phase 3: Maturity
    Production Mandate    :p3a, 2027-01, 90d
    Trust Registry        :p3b, after p3a, 90d
    Security Agents       :p3c, after p3b, 90d
    SOAR Integration      :p3d, after p3c, 90d
```

---

*These diagrams are designed for inclusion in the SFAMDF Whitepaper and related presentations.*
