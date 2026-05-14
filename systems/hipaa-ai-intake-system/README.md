# Build a HIPAA-Aware AI Intake System

> Inside the [Value-Driven Systems Engineering](../../README.md) portfolio · *Solutions and strategy engineered for small and growing business operators.*

## Overview

This project builds a HIPAA-aware repository, clinic-intake-ai, as the foundation for an AI-powered patient intake system.

The goal is to automate triage and form-fill for a small healthcare practice while keeping costs under $100 per provider per month. The system is designed from the start around compliance, ensuring Protected Health Information (PHI) is never exposed during development or testing.

The architecture is built across **8 phases**, anchored by **Scaffolding a Production-Grade Repository** on the input side and **Two-Page Leave-Behind for Practice Managers** at the end. Each phase is listed in the Implementation section below.

## Architecture

```mermaid
---
title: Build a HIPAA-Aware AI Intake System
---
%%{init: {"theme":"base","themeVariables": {"primaryColor":"#1B4332","primaryTextColor":"#F4D03F","primaryBorderColor":"#F4D03F","secondaryColor":"#264653","tertiaryColor":"#2F5233","lineColor":"#F4D03F","fontFamily":"ui-monospace, SFMono-Regular, Menlo, Consolas, monospace","fontSize":"13px"}}}%%
flowchart LR
    classDef datastore fill:#264653,stroke:#F4D03F,stroke-width:2px,color:#FFFFFF
    classDef service fill:#1B4332,stroke:#F4D03F,stroke-width:2px,color:#F4D03F
    classDef event fill:#7B42BC,stroke:#F4D03F,stroke-width:2px,color:#FFFFFF
    classDef io fill:#0d1117,stroke:#F4D03F,stroke-width:1.5px,color:#F4D03F,font-style:italic







    subgraph PatientFacing["Patient-Facing Layer"]
        Patient[/"Patient Intake Form"/]
        PracticeMgr[/"Practice Manager"/]
    end

    subgraph PromptGuardrails["HIPAA-Aware Prompt Library"]
        PromptLib("Prompt Templates (synthetic-data-only)")
        Guardrails("Security Guardrails")
    end

    subgraph PHIHandling["PHI Handling (Under BAA)"]
        IntakeSvc("clinic-intake-ai Service")
        Bedrock("AWS Bedrock (BAA-covered LLM)")
        Rejected("OpenAI - REJECTED (no BAA)")
    end

    subgraph EncryptedStorage["Version-Controlled Repo & Storage"]
        Repo[("clinic-intake-ai Repository")]
        OutreachLog[("pitch-log.md / Outreach (initials only)")]
    end

    subgraph ComplianceAudit["Compliance & Audit"]
        ADR("MADR ADR: Bedrock-under-BAA")
        Conventions("CONVENTIONS.md / .gitignore")
        CostCheck{{"cost-check.sh ($100/provider cap)"}}
        Readme("7-Block README (HIPAA posture)")
    end

    Patient -- "PHI submission" --> IntakeSvc
    IntakeSvc -- "synthetic-data-only prompt" --> PromptLib
    PromptLib -- "guardrail-enforced template" --> Guardrails
    Guardrails -- "PHI-safe prompt" --> Bedrock
    Bedrock -. "BAA-covered inference" .-> IntakeSvc
    IntakeSvc -. "blocked: no BAA" .-x Rejected

    IntakeSvc -- "de-identified artifacts" --> Repo
    Repo -- "initials-only contacts" --> OutreachLog

    Conventions --> Repo
    ADR --> Repo
    Readme -- "documents posture & gaps" --> Repo
    CostCheck -- "budget event" --> IntakeSvc

    PracticeMgr -- "leave-behind / pitch deck" --> Readme
class Repo,OutreachLog datastore
class CostCheck event

    class Repo,OutreachLog datastore
    class PromptLib,Guardrails,IntakeSvc,Bedrock,Rejected,ADR,Conventions,Readme service
    class CostCheck event
    class Patient,PracticeMgr io
```

The diagram shows the topology and data flow of the system as built. The full architectural narrative, with screenshots and prose, lives in [`documents/hipaa-ai-intake-system.md`](./documents/hipaa-ai-intake-system.md).

## Implementation

This system is built across **8 phases**:

1. **Scaffolding a Production-Grade Repository**
2. **Building a HIPAA-Aware Prompt Library**
3. **Writing the 7-Block README with Live Mermaid Diagrams**
4. **Documenting the Architecture Decision: AWS Bedrock Under BAA**
5. **Creating Mermaid Diagrams and Operations Script Stubs**
6. **Building the Pilot-Outreach List and Pitch Deck**
7. **Drafting the Coffee-Meeting Pitch and Validating the Repo**
8. **Two-Page Leave-Behind for Practice Managers**

For the full walkthrough with screenshots and step-by-step content, see [`documents/hipaa-ai-intake-system.md`](./documents/hipaa-ai-intake-system.md).

## Validation

Build outcomes verified end-to-end. Each phase below is captured with screenshots, configuration, and observable behavior in [`documents/hipaa-ai-intake-system.md`](./documents/hipaa-ai-intake-system.md):

- ✅ Scaffolding a Production-Grade Repository
- ✅ Building a HIPAA-Aware Prompt Library
- ✅ Writing the 7-Block README with Live Mermaid Diagrams
- ✅ Documenting the Architecture Decision: AWS Bedrock Under BAA
- ✅ Creating Mermaid Diagrams and Operations Script Stubs
- ✅ Building the Pilot-Outreach List and Pitch Deck
- ✅ Drafting the Coffee-Meeting Pitch and Validating the Repo
- ✅ Two-Page Leave-Behind for Practice Managers
