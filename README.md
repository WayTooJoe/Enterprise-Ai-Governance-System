# Neural Risk Management 
The system bridges static compliance frameworks with live engineering operations, providing real-time visibility into AI models, datasets, and third-party vendors. It automates telemetry ingestion, schema validation, and evidence logging into an immutable vault, replacing manual processes.
---

##  Problem Statement
In enterprise GRC environments, scaling continuous control monitoring faces two major bottlenecks:
1. **Schema Chaos:** Security tools, model evaluation frameworks, and cloud scanners dump telemetry in highly variable, deeply nested, or unstructured JSON formats. Traditional rigid code blocks break when schemas shift.
2. **Monolithic Thresholds:** Different business units have entirely different risk appetites. Forcing a single pass/fail metric across the entire organization means high-risk domains (like HR/Hiring) are evaluated under the same parameters as lower-risk operational domains (like Corporate infrastructure), creating massive compliance gaps.

---

## The Architecture & Solution

This engine replaces manual spreadsheet reviews with a headless, event-driven data pipeline:

* **Headless Ingestion Gateway:** A production-ready n8n webhook captures live payloads (e.g., AWS SageMaker model evaluations, bias monitoring scans) on the fly.
* **Structured AI Data Normalization:** Utilizing a standalone OpenAI-backed *Information Extractor* node, the pipeline completely muzzles conversational LLM fluff. It forces the model to act as a strict data serialization API, dynamically extracting asset metadata, compliance tags (`MEAS-3.1`), and stringifying the raw payload.
* **Dynamic Business Unit Inference:** The engine analyzes deep infrastructure strings (like `registry_uri` or deployment paths) to autonomously infer the owning department (e.g., tracking `customer-insights` directly to `Finance`) when explicit metadata is missing.
* **Multi-Tenant Policy Evaluation:** The pipeline queries a decoupled relational policy matrix. Instead of hardcoding thresholds, n8n dynamically fetches the exact mathematical bounds (Disparate Impact ratio min/max, Demographic Parity difference limits) assigned specifically to *that* framework control and *that* business unit combination.

---

## Relational Database Schema Design (Airtable)

The architecture maps data across a highly structured, relational enterprise compliance ledger:
* **AI Asset Inventory:** Centralized tracking of enterprise models, risk tiers, and business owners.
* **NIST AI RMF Core:** Reference framework mapping functions, categories, and master control descriptions.
* **Compliance Thresholds (Policy Engine):** Multi-tenant parameter boundaries. Enforces a stricter risk-tolerance boundary for HR compliance versus Finance operations for identical controls.
* **API Staging / Evidence Vault:** The final execution ledger capturing automated continuous audit logs, runtime timestamps, and deterministic pass/fail results.

---

##  Repository Contents

* [📥 Download the Production n8n Workflow File](./Neural_Risk_Management.json): The complete blueprint. You can import this file straight into your n8n instance to review the node connections.: The complete, production-ready n8n blueprint. Import this file straight into your n8n instance to review the node connections.
