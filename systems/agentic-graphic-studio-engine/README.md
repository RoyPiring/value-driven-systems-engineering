# Build an Agentic Graphic Studio Engine

> Inside the [Value-Driven Systems Engineering](../../README.md) portfolio · *Solutions and strategy engineered for small and growing business operators.*

## Overview

I built an agentic graphic studio engine to make brand identity work more controlled, traceable, and repeatable. The engine organized the creative process into defined teams, review points, scoring gates, and delivery rules so the work could move from research to final handoff without losing alignment.

The main goal was to avoid the common failure point in AI-assisted design: one long prompt producing assets that cannot be governed. I treated the workflow as a production system instead. Each phase had a clear responsibility, a handoff point, and a standard for proving readiness before the next phase began.

The build used confidence gates, scoring caps, iteration limits, and operator-in-the-loop review to protect quality. Those controls gave the engine a way to move quickly while still keeping decisions accountable and traceable.

The architecture is built across **8 phases**, anchored by **Setting Up the Agentic Design Engine** on the input side and **Extending the Engine with Antigravity Managed Agents** at the end. Each phase is listed in the Implementation section below.

## Architecture

```mermaid
---
title: Build an Agentic Graphic Studio Engine
---
%%{init: {"theme":"base","themeVariables": {"primaryColor":"#1B4332","primaryTextColor":"#F4D03F","primaryBorderColor":"#F4D03F","secondaryColor":"#264653","tertiaryColor":"#2F5233","lineColor":"#F4D03F","fontFamily":"ui-monospace, SFMono-Regular, Menlo, Consolas, monospace","fontSize":"13px"}}}%%
flowchart LR
    classDef datastore fill:#264653,stroke:#F4D03F,stroke-width:2px,color:#FFFFFF
    classDef service fill:#1B4332,stroke:#F4D03F,stroke-width:2px,color:#F4D03F
    classDef event fill:#7B42BC,stroke:#F4D03F,stroke-width:2px,color:#FFFFFF
    classDef io fill:#0d1117,stroke:#F4D03F,stroke-width:1.5px,color:#F4D03F,font-style:italic

    Client[/SMB Client - brand identity request/]
    Package[/Brand Identity Package - client handoff/]

    subgraph Orchestration["Orchestration - Claude Code"]
        Orch(orchestrator.md)
        ConfGate{{Confidence Gate - 0.7 threshold}}
        IterCap{{Iteration Caps}}
        Operator{{Operator-in-the-Loop}}
        Mcp[(.mcp.json - Firecrawl MCP)]
    end

    subgraph Workspace["Workspace - agentic-studio, 28 agents in 8 teams"]
        TeamDirs[(8 Team Directories)]
        RunLogs[(Run Logs - Antigravity Cockpit)]
        Council(Design Council 6.0 to 6.7 to 7.0)
    end

    subgraph ResearchTeam["1. Research Team"]
        Scanner(competitor-scanner agent)
        Firecrawl(Firecrawl - scrape competitors)
        GemGem(Gemini Gem - synthesize intelligence)
        Dossier[(Research Dossier)]
    end
    ResearchGate{{RESEARCH_COMPLETE - 7.5/10 documented exception}}

    subgraph BriefTeam["2. Brief Team"]
        BriefAgent(Creative Brief)
    end

    subgraph ConceptTeam["3. Concept Team - 3 directions"]
        Totality(A - Totality)
        QuietEmber(B - Quiet Ember)
        EmberHour(C - Ember Hour)
    end
    ConceptGate{{CONCEPT_GATE - Ember Hour locked}}

    subgraph GenerationTeam["4. Generation Team - multi-model, component isolation"]
        ChatGPT(ChatGPT Pro)
        Gemini(Gemini Nano Banana Pro)
        Icon[(Icon - isolated file)]
        Wordmark[(Wordmark - isolated file)]
        Background[(Background - isolated file)]
    end

    subgraph ReviewTeam["5. Review Team - 0.7 threshold"]
        ScoreLoop(Scoring Loop - 3-round cap)
    end
    ReviewGate{{REVIEW_GATE - 6 or higher passes}}

    subgraph RefinementTeam["6. Refinement Team"]
        Photopea(Photopea - raster cleanup)
        Inkscape(Inkscape - vector cleanup)
        FigmaLib[(Figma Component Library)]
        Canva(Canva - final lockup)
    end

    subgraph ProductionTeam["7. Production Team"]
        Export(300 DPI + vector export)
        PngAssets[(2 PNGs - 300 DPI)]
        SvgAssets[(3 SVGs - plain vector)]
        Guidelines[(Brand Guidelines)]
        PrintSpec[(Print Specifications)]
        Provenance[(IP Provenance Disclosure)]
    end

    subgraph DeliveryTeam["8. Delivery Team"]
        Checklist(delivery-checklist agent)
    end
    DeliveryGate{{DELIVERY_GATE - 8 of 8 verified}}

    Client -->|kick off build| Orch
    Orch -->|register handoff contracts| TeamDirs
    Orch -->|record gate verdicts| RunLogs
    Orch -->|enforces| ConfGate
    Orch -->|enforces| IterCap
    Operator -.->|approves handoffs| Orch
    Mcp -->|web tools for| Firecrawl
    ConfGate -.->|gates every handoff| ResearchGate
    RunLogs -->|per-round scores| Council

    Orch -->|dispatch research wave| Scanner
    Scanner -->|competitor identities| Firecrawl
    Firecrawl -->|raw market data| GemGem
    GemGem -->|structured brand intelligence| Dossier
    Dossier -->|score the evidence| ResearchGate
    ResearchGate -->|exception recorded, proceed| BriefAgent

    BriefAgent -->|direction A| Totality
    BriefAgent -->|direction B| QuietEmber
    BriefAgent -->|direction C| EmberHour
    Totality -->|judged vs brief| ConceptGate
    QuietEmber -->|judged vs brief| ConceptGate
    EmberHour -->|selected| ConceptGate

    ConceptGate -->|locked direction| ChatGPT
    ConceptGate -->|locked direction| Gemini
    ChatGPT -->|generates| Icon
    ChatGPT -->|generates| Background
    Gemini -->|generates| Wordmark
    Icon -->|per-component score| ScoreLoop
    Wordmark -->|per-component score| ScoreLoop
    Background -->|per-component score| ScoreLoop
    ScoreLoop -->|cap at 3 rounds| ReviewGate

    ReviewGate -->|approved raster| Photopea
    ReviewGate -->|approved vector| Inkscape
    Photopea -->|cleaned icon| FigmaLib
    Inkscape -->|cleaned wordmark| FigmaLib
    FigmaLib -->|controlled parts| Canva
    Canva -->|composited lockup| Export

    Export -->|raster output| PngAssets
    Export -->|vector output| SvgAssets
    Export -->|writes| Guidelines
    Export -->|writes| PrintSpec
    Export -->|writes| Provenance
    PngAssets -->|audited by| Checklist
    SvgAssets -->|audited by| Checklist
    Guidelines -->|audited by| Checklist
    Checklist -->|programmatic audit| DeliveryGate
    DeliveryGate -->|client handoff| Package

    class Mcp,TeamDirs,RunLogs,Dossier,Icon,Wordmark,Background,FigmaLib,PngAssets,SvgAssets,Guidelines,PrintSpec,Provenance datastore
    class Orch,Firecrawl,Scanner,GemGem,BriefAgent,Totality,QuietEmber,EmberHour,ChatGPT,Gemini,ScoreLoop,Photopea,Inkscape,Canva,Export,Council,Checklist service
    class ConfGate,IterCap,Operator,ResearchGate,ConceptGate,ReviewGate,DeliveryGate event
    class Client,Package io
```

The diagram shows the topology and data flow of the system as built. The full architectural narrative, with screenshots and prose, lives in [`documents/agentic-graphic-studio-engine.md`](./documents/agentic-graphic-studio-engine.md).

## Implementation

This system is built across **8 phases**:

1. **Setting Up the Agentic Design Engine**
2. **Scaffolding 28 Agents Across 8 Teams**
3. **Running Two-Phase Brand Research with Confidence Gating**
4. **Generating the Creative Brief and Concept Directions**
5. **Multi-Model Identity Asset Generation with Component Isolation**
6. **Refining and Compositing the Brand Identity**
7. **Packaging the Production Identity Deliverables**
8. **Extending the Engine with Antigravity Managed Agents**

For the full walkthrough with screenshots and step-by-step content, see [`documents/agentic-graphic-studio-engine.md`](./documents/agentic-graphic-studio-engine.md).

## Validation

Each build phase below is documented in [`documents/agentic-graphic-studio-engine.md`](./documents/agentic-graphic-studio-engine.md), with screenshots, configuration, and notes as captured during the build:

- ✅ Setting Up the Agentic Design Engine
- ✅ Scaffolding 28 Agents Across 8 Teams
- ✅ Running Two-Phase Brand Research with Confidence Gating
- ✅ Generating the Creative Brief and Concept Directions
- ✅ Multi-Model Identity Asset Generation with Component Isolation
- ✅ Refining and Compositing the Brand Identity
- ✅ Packaging the Production Identity Deliverables
- ✅ Extending the Engine with Antigravity Managed Agents
