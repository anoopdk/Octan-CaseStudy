Octan digital transaformation scenario


1. [Project Objective](#project-objective)
2. [Proposed Solution](#proposed-solution)


# Project Objective 

The data platform to be utilized by Octan must be capable of ingesting and processing different data sources like real-time streaming data from plant machine sensors, historical project reports in Excel sheets, third-party SaaS application data securely exposed through API with OAuth authentication, and sensitive financial data from on-prem systems with strong access controls. The platform must enable data scientists and BI engineers to conduct analysis autonomously, with reduced dependency on data engineers. It should also enable secure, role-based access control, with consideration of data sensitivity. The platform should be efficiently scalable and redeployable easily. Last but not least, the solution must be supportable and operationally maintained by Octan's data engineering and infrastructure teams to ensure long-term sustainability and governance.


# Proposed Solution

Before coming up with the final architectural design, which will be covered in more detail in the [Architectural Detail Design](#architectural-detail-design) section, I plan to divide the solution into several parts in this section.

### **High Level Architecture**

The framework follows the **Medallion architecture** that represents a structured data processing approach from source systems through to consumption layers.

**Sources:**
Data is ingested from several heterogeneous systems such as XML files, SQL Server databases, SaaS APIs, and IoT (real-time) feeds based on this exercise. 

**Bronze Layer (Raw):**
Data initially enters raw layer, it's basically an unprocessed data as it was received from source systems. The data is typically loaded batch full or incrementally in mode to provide completeness and traceability historically.

**Silver Layer (Enriched):**
Stores cleaned, standardized, and enriched data. Enhances data quality and consistency to stage data for consumption by analysis and business. Loading continues in batch or incremental mode.

**Gold Layer (Curated):**
Has aggregated and business-ready data sets, which are normally developed in star schemas. This layer supports analytics, BI reporting, and machine learning applications through optimized tables or views.

**Consumption Layer:**
Data Science, Business Intelligence tools, and Machine Learning applications consume data from the Gold layer to present actionable insights and well-informed decisions in the organization.

| Layer | Description | Tool |
|--------|--------------|------|
| **Bronze** | Raw ingestion from source as-is |
| **Silver** | Data cleansing, transaformations & SCD setup |
| **Gold** | Curated, star-model & analytics-ready layer |

Bronze → Silver → Gold ensures progressive refinement and governance.

<img width="861" height="453" alt="Octan--Data Architecture Diagram drawio" src="https://github.com/user-attachments/assets/732ecbe3-3248-41b3-a7da-91fbeadf0b35" />


### **Data Storage & Organisation**

The multi-layered data storage architecture is divided into `Bronze`, `Silver`, and `Gold` layers to deliver a traceable, structured, and business-ready data pipeline in the analytics environment.

**Bronze Layer**:
Raw, unprocessed data is stored in it, being loaded directly from the source systems. Data is preserved in its original form for traceability and debugging. At this stage, limited or no transformation is done. It is primarily utilized by data engineers for ingestion and validation source data.

**Silver Layer**:
The Silver layer consists of cleaned, standardized, and enriched data. It applies transformations such as data cleaning, standardization, enrichment, and derived column creation, producing data that is uniform and ready for analysis. This layer is mostly utilized by data analysts and data scientists. This layer can also have derived tables which are basically built using table look-ups and joins.

**Gold Layer**:
The Gold layer contains aggregated, polished, and business-consumable data sets. It consists of data integration, application of business rules, and star schema data modeling for analytics, reporting, and making decisions. Business users, analysts, and data scientists are the primary consumers of this layer.

![Data Laytout Diagram](https://github.com/user-attachments/assets/8396a753-179c-407f-b130-87acad3c24ff)




### **Metadata-driven Control Flow**


**Control Tables** integrates ingestion configuration, recovery, and audit metadata. These tables sits in a seperate schema, helps to govers and Orchestration the data ingesiton pipelines.

All control tables fall under the `control` schema.

| Layer | Description |
|--------|--------------|
| **Control** | Control tables to drive orchestration |


#### control.control_table_spec

**Purpose:** Specifies static configuration for all source table entities.

#### control.operational_recovery_spec

**Purpose:** Manages backfill and operational recovery runs.




<img width="827" height="1461" alt="Metadata Control Flow Diagram" src="https://github.com/user-attachments/assets/558139e7-c8fc-42f0-ae15-41aa79dc9869" />

#### Pipeline Orchestration 

The aim is to build a parametarized end-to-end orcheatration pipeline that automaties the full Medallion architecture workflow, driven my control tabels via metadata information from Bronze to Silver to Gold layers, ensuring scalable, reuable, and parallel execution capability. 


<img width="792" height="636" alt="image" src="https://github.com/user-attachments/assets/f014e854-2055-41e9-b36e-94b092104aba" />




### **Data Star Schema Model**

<img width="726" height="518" alt="image" src="https://github.com/user-attachments/assets/3d750205-336c-4cb2-8188-abce3f0c4b2e" />


The star schema model is established in the Gold layer.





## **Architectural Detail Design**


#### Databricks

<img width="1034" height="454" alt="image" src="https://github.com/user-attachments/assets/a28305da-3b39-43f2-aa7b-a1b3cfc4815a" />


#### Databricks + ADF

<img width="1502" height="624" alt="image" src="https://github.com/user-attachments/assets/ffe77158-3963-4b61-a6ba-4d44671b3ada" />




## Object level security


#### Top-Down approach 

#### Bottom-Up approach 

<img width="797" height="299" alt="image" src="https://github.com/user-attachments/assets/20008bba-58d8-4d0d-b2c2-ef952dc198bc" />



#### Implementaiton Recommendations
1. Maintain separate schemas: `control`, `bronze`, `silver`, `gold`.
2. Design a **governed, metadata-driven** data ingestion process.
3. Use **Auto Loader** for raw ingestion; **DLT** for transformations.
4. Maintain **control schema & tables** for meta-data driven data processing.
6. Integrate this framework into your CI/CD and data governance workflow.


## BAU and Support Considerations

#### Operational Best Practices

- Follow standard event loggin mechanism to capture failures and error.
- Document processes, how-to articles, log RCA and escalation metrics.
- Adopt least privilage access policy using Unity Catalog ACLs.
- Maintain a strategic approach for backfill operations and request history.





