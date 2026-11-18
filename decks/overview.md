# Octan Digital Transformation Data Platform

> Zynx Technologies | 3-slide delivery overview

---

## Slide 1: Context & problem

- Zynx x Energy & Industrial Client
- Engagement: Solution Design / Case Delivery
- The Energy & Industrial Client (Octan scenario) needed a scalable data platform that could ingest IoT plant sensors, Excel project reports, OAuth-protected SaaS APIs, and sensitive on-prem financial data, while enabling autonomous analytics under strong access control.
- Engagement levels: Discovery & Problem Framing -> Target Architecture Design -> Data Modelling (Star Schema) -> Orchestration Blueprint

---

## Slide 2: Solution architecture & stack

- Zynx designed a medallion lakehouse target architecture with bronze/silver/gold refinement, star-schema curated models, orchestration diagrams, and operational ownership guidance suitable for the client's data engineering and infrastructure teams.
- Stack: Medallion architecture, Lakehouse design, Star schema, Streaming + batch, RBAC / sensitivity controls
- Architecture documented in docs/03-architecture.md with Mermaid diagrams

---

## Slide 3: Implementation highlights & inspection

- Key evidence: case-study/exercise.md, case-study/diagrams/
- Open docs/audit/PROJECT-FACTSHEET.md for the audit index
- Repository standardised to Zynx industrial portfolio structure
- Portfolio tag target: 1.0.0-portfolio
