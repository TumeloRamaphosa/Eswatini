# Architecture

```text
Keenan / Studex operators
          |
          v
Eswatini Command Dashboard
  | opportunities | stakeholders | tasks | decisions | agent activity | health
          |
          v
Buzz group 90bbcbba-9a1e-4b9a-9442-b4227ef16421
          |
          v
Country Chief / Business Ghost
          |
          +-- Mining
          +-- Government & Localisation
          +-- Data Centre & Infrastructure
          +-- Agriculture & Drones
          +-- Telecom
          +-- Commercial
          +-- Research
          +-- Forward-Deployed Engineering
          +-- Risk & Compliance
          |
          v
Service layer: repositories | documents | research | communications | tools
          |
          v
Persistent memory + audit log + notification router + health monitor
```

## Design principles

- One country command surface, not a collection of disconnected agent chats.
- Existing Buzz identities are reused after discovery.
- Every meaningful action emits an auditable event.
- Persistent memory stores decisions, concepts, relationships, and provenance.
- Provider adapters sit behind services so Buzz, GitHub, Notion, and future tools
  can change without rewriting programme rules.
- High-consequence operations require human approval and explicit safety limits.
- The local development environment is the golden template for later Orgo hosting.

