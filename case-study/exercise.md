Octan digital transaformation scenario


1. [Project Objective](#project-objective)
2. [Current State](#current-state)
3. [Future State](#future-state)
4. [Key Requirements](key-requirments)
5. [Proposed Solution](#proposed-solution)


# Project Objective 

placeholder



# Current State

placeholder




# Future State

placeholder




# Key Requirements

placeholder

## Data sources
### 1. Streaming data from plant machinery sensors

### 2. Historical project status reports in Excel files

### 3. Data from 3rd party SaaS applications(API with OAuth)

### 4. Finance data from on-premises finance system



## Data Access & Consumption

### 1. Data Scientists and BI Engineers

### 2. Role based access control


## Deployment
### Terraform for IaaS depoyment
### DevOps for CI/CD deployment


## BAU Support
### Infrastructure support
### Data platform support



# Proposed Solution

Before coming up with the final architectural design, which will be covered in more detail in the [Architectural Detail Design section](architectural-detail-design), I plan to divide the solution into several parts in this section.

### High Level Architecture


The framework follows the **Medallion architecture**:

| Layer | Description | Tool |
|--------|--------------|------|
| **Bronze** | Raw ingestion from source as-is |
| **Silver** | Data cleansing, transaformations & SCD setup |
| **Gold** | Curated, star-model & analytics-ready layer |

Bronze → Silver → Gold ensures progressive refinement and governance.

<img width="861" height="453" alt="Octan--Data Architecture Diagram drawio" src="https://github.com/user-attachments/assets/732ecbe3-3248-41b3-a7da-91fbeadf0b35" />


### Data Storage & Organisation

![Data Laytout Diagram](https://github.com/user-attachments/assets/8396a753-179c-407f-b130-87acad3c24ff)

Placeholder


### Metadata-driven Control Flow


**Control Tables** integrates ingestion configuration, recovery, and audit metadata. These tables sits in a seperate schema, helps to govers and Orchestration the data ingesiton pipelines.

All control tables fall under the `control` schema.

| Layer | Description |
|--------|--------------|
| **Control** | Control tables to drive orchestration |


### control.control_table_spec

**Purpose:** Specifies static configuration for all source entities.

### control.operational_recovery_spec

**Purpose:** Manages backfill and operational recovery runs.

### Standard Incremental Flow

- Entity configuration from `control.control_table_spec`.

- Auto Loader (cloudFiles) reads data in real-time → Bronze table.

- Metadata columns added: `_ingest_ts`, `_source_system`, `_file_name`.

- Persistent checkpoint used for state tracking.

- Run logged to `control.load_audit`.

### Controlled Backfill Flow

- Adds a row to `control.operational_recovery_spec` with: `entity_name`, `refresh_start`, `refresh_end`, `status='INITIATED'`.

- Job picks it up during run-time and joins both the tables to fetch the entity informaiton and backfill for the sanme

- In logic, we create a temporary checkpoint, so that we dont over-write the produciton checkpoint, instead create seperate `backfill/checkpoint` for this activity.

- Loads files for date range using `triggerOnce`.

- Writes to staging table (`<entity>_staging`).

- Verifies:
  - Rows > 0

  - Primary key uniqueness (if exists)

- Performs atomic swap:

  - Rename production to backup

  - Rename staging to production

- Updates status to `COMPLETE` and logs in `load_audit`.

<img width="827" height="1461" alt="Metadata Control Flow Diagram" src="https://github.com/user-attachments/assets/558139e7-c8fc-42f0-ae15-41aa79dc9869" />


Placeholder


### Data Stat Schema Model

![Start Schema Model drawio](https://github.com/user-attachments/assets/00af671d-43be-4d71-a4a8-9f3f89cc2891)


placeholder

## Architectural Detail Design

placeholder


#### Design Considerations 

## BAU and Support Considerations



