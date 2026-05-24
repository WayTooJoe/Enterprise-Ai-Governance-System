# Technical Architecture: Neural Risk Management Engine

## 1. System Overview
The Neural Risk Management Engine is a headless, event-driven orchestration pipeline. It replaces manual GRC processes with an autonomous, platform-agnostic framework capable of real-time telemetry ingestion, deterministic schema normalization, and automated governance lifecycle management.

## 2. Data Flow & Integrity Logic
The pipeline utilizes a **Synchronous Promotion Pattern** to enforce strict data separation and non-repudiable audit trails.

1. **Immutable Ingestion:** Raw telemetry is persisted to the `API Staging` table immediately upon arrival. This provides a non-repudiable chain of custody for the original, unaltered payload.
2. **Neural Serialization:** The n8n workflow performs in-flight normalization via an OpenAI-backed Information Extractor. It uses a 7-attribute constrained prompt system to enforce deterministic JSON output, stripping "LLM fluff" and ensuring data is strictly typed for the ledger.
3. **Dynamic Policy Logic:** The engine performs a real-time lookup in the `Compliance Thresholds` table, filtering by `Business_Unit` and `Control_ID` to apply multi-tenant mathematical tolerances.
4. **Atomic Promotion:** Upon successful validation, the workflow performs an atomic `Create Record` into the `Evidence Vault`. This ensures the Vault remains a 'clean' record of validated compliance data, distinct from the 'dirty' raw ingestion logs.

## 3. Relational Data Schema
The architecture employs a highly normalized schema to ensure referential integrity across the governance lifecycle:

* **AI Asset Inventory:** The primary registry linking systems to business owners and risk tiers.
* **NIST AI RMF Core:** Maps outcomes to framework categories.
* **Evidence Vault:** The junction table that binds Assets, Controls, and Verdicts into a queryable audit ledger.
* **Compliance Thresholds:** A decoupled policy matrix for multi-tenant risk tolerance (e.g., varying Disparate Impact thresholds by Business Unit).
* **Audit Findings (CAPA):** An isolated entity tracking remediation lifecycles, ensuring every "FAIL" verdict generates a strictly managed incident record.

## 4. Governance Automation Layer (Airtable)
The system employs event-driven automation to ensure compliance drift is never ignored:

* **Risk Alert Automation:** Real-time event-driven notification triggers. When the Evidence Vault records a `FAIL` verdict, the system instantly alerts model owners via email.
* **Control Recertification:** Implements a lifecycle heartbeat. The system monitors `Control_Expiration_Date` and triggers automated recertification tasks, preventing compliance rot.
* **Internal Routing:** Handles the logical re-classification and silo-ing of data as it matures through the pipeline.

## 5. Governance & HITL Workflow
While telemetry extraction and threshold verification operate at zero-touch machine speed, the system mandates **Human-in-the-Loop (HITL) Oversight**. The `Evidence Vault` status is non-final until a `Risk_Analyst` updates the `Remediation_Status` field to `Accepted` or `Resolved`, ensuring every automated finding is tied to human accountability.
