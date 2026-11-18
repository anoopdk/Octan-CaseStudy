# Architecture

## Context diagram

```mermaid
flowchart LR
  IoT[Plant sensors] --> B[Bronze]
  XLS[Excel reports] --> B
  API[SaaS APIs OAuth] --> B
  FIN[On-prem finance] --> B
  B --> S[Silver enriched]
  S --> G[Gold star schema]
  G --> DS[Data science]
  G --> BI[BI / reporting]
  G --> ML[ML apps]
```

## Implementation flow

```mermaid
flowchart TB
  P[Problem framing] --> A[High-level architecture]
  A --> D[Storage layout + layers]
  D --> M[Star schema model]
  M --> O[Orchestration design]
```

## Design notes

- Prefer layered medallion / governed platform patterns where applicable.
- Keep configuration and secrets outside Git.
- Preserve auditability: documentation must match repository evidence.
