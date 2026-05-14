<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Build a HIPAA-Aware AI Intake System

**Project Link:** [View Project](https://learn.nextwork.org/projects/bae88da1-febb-4622-bad0-5a4ee5d381f5)

**Author:** Roy Piring Jr  
**Email:** rpiringhawaii@gmail.com

---

![Image](https://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/bae88da1-febb-4622-bad0-5a4ee5d381f5_r3t8wq5n)

## Why This Project Exists

### Project vision and strategy

This project builds a HIPAA-aware repository, clinic-intake-ai, as the foundation for an AI-powered patient intake system.

The goal is to automate triage and form-fill for a small healthcare practice while keeping costs under $100 per provider per month. The system is designed from the start around compliance, ensuring Protected Health Information (PHI) is never exposed during development or testing.

## Scaffolding a Production-Grade Repository

### Industry-standard folder structure and naming conventions

The repository is structured to support maintainability and compliance from day one.

Folders follow consistent kebab-case naming and separate concerns across documentation, prompts, outreach, and future infrastructure. Initial commits include CONVENTIONS.md, .gitignore, and the full scaffold, establishing clear standards before any system logic is added.

![Image](https://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/bae88da1-febb-4622-bad0-5a4ee5d381f5_r3t8wq5n)

### Conventional Commits and CONVENTIONS.md

Conventional commits make changes easy to scan and understand.

Using prefixes like feat, fix, and chore removes guesswork when reviewing history. Kebab-case naming ensures compatibility across systems and aligns with cloud tooling. These standards reduce friction as the project grows.

## Building a HIPAA-Aware Prompt Library

### Standardized templates for diagrams and outreach

The prompt library defines how AI is used safely and consistently.

Templates are created for workflow diagrams, topology diagrams, ADRs, and outreach messages. Each template includes constraints that prevent exposure of sensitive data while maintaining clarity and structure.

### Security guardrails baked into every prompt

All prompts enforce the use of synthetic data only.

This prevents accidental exposure of PHI, which would violate HIPAA if used without a Business Associate Agreement. Guardrails ensure the system can be developed and documented safely before handling real data.

![Image](https://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/bae88da1-febb-4622-bad0-5a4ee5d381f5_w5h9bv2m)

### Version-controlled, not ephemeral

Prompt templates are stored in the repository rather than reused ad hoc.

This ensures consistency across outputs and creates a record of how AI is used in the system. It also allows prompts to evolve with the project while maintaining traceability.

## Writing the 7-Block README with Live Mermaid Diagrams

### Plain-English system overview at 7th-grade reading level

The README is structured to communicate clearly to both technical and non-technical readers.

It includes system overview, data handling, cost limits, HIPAA posture, architecture, and next steps. Mermaid diagrams are embedded directly to visualize workflows and system boundaries.

![Image](https://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/bae88da1-febb-4622-bad0-5a4ee5d381f5_r3n8w5jt)

### HIPAA posture and cost ceiling documented

The README explicitly states what is and is not compliant.

It avoids overclaiming by listing completed controls and gaps. This transparency reduces legal risk and builds trust with potential users by setting clear expectations.

![Image](https://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/bae88da1-febb-4622-bad0-5a4ee5d381f5_d9c1f7kz)

## Documenting the Architecture Decision: AWS Bedrock Under BAA

### Why Bedrock, and what was rejected

The system design includes topology and data-flow diagrams to define structure and behavior.

Stub scripts outline future ingestion and processing logic. These artifacts clarify how data moves through the system and where compliance boundaries exist.

### MADR format with honest trade-offs

The ADR documents why AWS Bedrock is selected.

OpenAI is rejected due to lack of a Business Associate Agreement for standard API use. This is a hard requirement under HIPAA, making compliance a gating factor in architecture decisions.

![Image](https://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/bae88da1-febb-4622-bad0-5a4ee5d381f5_xt9c2wfn)

### Locking in the decision before writing a line of code

Documenting decisions early preserves context.

It ensures that future contributors understand constraints such as cost limits and compliance requirements without reverse-engineering them from code. This reduces ambiguity as the system evolves.

## Creating Mermaid Diagrams and Operations Script Stubs

![Image](https://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/bae88da1-febb-4622-bad0-5a4ee5d381f5_qr7t2vbf)

### System topology and data-flow sequence diagrams

Topology diagrams show system structure, while sequence diagrams show data flow.
Together, they define both what exists and how it behaves. This dual view ensures the architecture is clear before implementation begins.

### Stub scripts that hold structure for future iterations

Script stubs define where logic will live before it is implemented.

This prevents structure from being decided under pressure later. It also makes cost considerations visible early through placeholder scripts like cost-check.sh.

## Building the Pilot-Outreach List and Pitch Deck

### Warm contacts committed to version control from day one

Warm contacts committed to version control from day one

Outreach is tracked directly in the repository.

Contacts are stored using initials to protect identity in a public repo. A pitch deck is generated to communicate value clearly to practice managers.

![Image](https://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/bae88da1-febb-4622-bad0-5a4ee5d381f5_r3w8n5yt)

### Visual pitch deck designed for practice managers, not engineers

The pitch avoids technical language.

It focuses on time savings, cost reduction, and workflow improvement. This ensures the message aligns with the audience and increases the chance of engagement.

## Drafting the Coffee-Meeting Pitch and Validating the Repo

### A real message to a real person, sent the day the repo goes live

A leave-behind document is created for in-person conversations.

It focuses on clarity and trust, avoiding technical language and aligning with the audience’s expectations.

![Image](https://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/bae88da1-febb-4622-bad0-5a4ee5d381f5_r9t4bx7q)

### 18-file checklist and Mermaid render verification

Validation confirms the repository structure is mostly complete.

File counts exceed expectations, but a few expected artifacts are missing. Naming conventions are consistent across folders, ensuring alignment with documented standards.

![Image](https://learn.nextwork.org/refreshed_maroon_timid_jujube/uploads/bae88da1-febb-4622-bad0-5a4ee5d381f5_h5jk2np8)

### First outreach attempt logged in pitch-log.md

The outreach log tracks engagement attempts.

At this stage, no outreach has been sent, and the log remains empty. The next step is sending the pitch and recording the interaction for accountability.

## Secret Mission: Two-Page Leave-Behind for Practice Managers

### Polished leave-behind with Mermaid workflow visual

The leave-behind differs from the README in tone and purpose.

It removes technical detail and focuses on workflow and value. This ensures the message is accessible and builds trust during early conversations.

## Reflections and What Comes Next

### Customer-first thinking before a single line of code

Customer-first thinking before a single line of code

This project prioritizes audience and compliance before implementation.

Tools include Claude Code, Mermaid, and Conventional Commits, while concepts focus on audience-aware communication and structured decision-making.

### What this repo proves to a hiring manager or collaborator

This project shows the ability to design systems with compliance, structure, and audience in mind.

It demonstrates how to translate technical systems into clear, usable artifacts for both engineers and non-technical stakeholders.

### What iteration 2 will build

This project took about 1 hour. The main challenge was creating a pitch that communicates value clearly without using technical language.

The next step is building the actual system and focusing on user-facing design for healthcare workflows.

---

*Built with [NextWork](https://learn.nextwork.org) - [View this project](https://learn.nextwork.org/projects/bae88da1-febb-4622-bad0-5a4ee5d381f5)*
