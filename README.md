# Intune Security Copilot Readiness Agent

A public demo AI agent that helps endpoint teams investigate Intune compliance, patching, app deployment, and configuration policy issues using synthetic data and LLM-driven workflows.

## Why this exists

Organizations with Microsoft Security Copilot capacity want practical, repeatable ways to apply AI to endpoint management scenarios. This project demonstrates how Security Copilot-style promptbooks, curated data retrieval, and agentic workflows can turn Intune operational signals into actionable insights.

The project is designed as a public, portfolio-ready example of AI/LLM solution architecture for endpoint management.

## What it demonstrates

- LLM promptbook design for Intune investigations
- Agent workflow patterns for triage and summarization
- Graph/Intune-style data connector design
- Synthetic device-management telemetry
- Risk scoring, trend summaries, and recommended actions
- Executive-ready dashboard and reporting concepts

## Public data statement

This repository uses synthetic/sample data only.

It does not contain customer data, tenant identifiers, internal Microsoft data, production credentials, or screenshots from real environments.

## Core scenarios

The initial agent focuses on five Intune operations scenarios:

1. Device estate overview by platform
2. Non-compliant device investigation
3. Windows patch status for the previous month
4. Latest app deployment failures
5. Configuration policy failures affecting many devices

## Conceptual architecture

```text
Synthetic Intune Data
        |
        v
API / Query Layer
        |
        v
Agent Workflow
        |
        v
Promptbook Reasoning
        |
        v
Dashboard / Report
```

The optional future connector pattern is:

```text
Customer Intune Tenant
        |
        v
Microsoft Graph / Intune Reporting APIs
        |
        v
Security Copilot Plugin or External Connector
        |
        v
Promptbook / Agent Workflow
        |
        v
Insights Dashboard / Monthly Report
```

## Security Copilot positioning

This project does not attempt to duplicate native Intune Security Copilot or Copilot Explorer experiences.

Instead, it demonstrates a complementary adoption package:

- Promptbooks for repeatable Intune investigations
- Optional plugin/query layer for structured endpoint data
- Dashboard and monthly summary for customer-facing reporting
- Workshop/readiness content for implementation planning

Security Copilot capacity units are treated as the consumption model for Copilot experiences, not as something the project directly builds on.

## Planned project structure

```text
.
|-- README.md
|-- docs/
|   |-- architecture.md
|   |-- workshop-flow.md
|   `-- implementation-plan.md
|-- promptbooks/
|   |-- tenant-health-overview.md
|   |-- compliance-investigation.md
|   |-- windows-patch-analysis.md
|   |-- app-deployment-failures.md
|   `-- configuration-profile-failures.md
|-- data/
|   `-- synthetic/
|-- src/
|   |-- agent/
|   |-- connectors/
|   `-- reporting/
|-- dashboard/
`-- samples/
```

## Roadmap

- [ ] Create promptbook MVP
- [ ] Define synthetic Intune data schema
- [ ] Build synthetic data generator
- [ ] Prototype agent workflow
- [ ] Create dashboard/report mockup
- [ ] Add optional Microsoft Graph connector design
- [ ] Package a customer workshop/readiness guide

## Design documents

- [MVP Dashboard Contract](docs/mvp-dashboard-contract.md)
- [Synthetic Data Schema](data/synthetic/schema.md)

## Intended audience

This repository is intended for customers, partners, technical stakeholders, and portfolio reviewers who want to see how AI/LLM workflows can be applied to endpoint management and security operations scenarios.

## License

License to be added.
