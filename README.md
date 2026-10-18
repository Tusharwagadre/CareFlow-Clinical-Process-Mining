# CareFlow — Clinical Pathway Process Mining

**Domain:** Healthcare Operations
**Program:** Infotact Solutions Internship — Group 9
**Status:** Week 1 in progress (dataset understanding and project setup)

## 1. Project overview

CareFlow analyses how patients move through a clinical pathway by turning hospital event logs into process-mining insights. The current dataset covers an **Emergency Department sepsis pathway** and is **synthetic demo data** (no real patients).

The project aims to reveal:
- the actual activity sequences (patient journeys) and process variants,
- delays between consecutive activities and likely bottlenecks,
- rework (for example, repeated blood draws) and how it affects pathway duration,
- operational KPIs presented in an interactive Power BI dashboard.

## 2. Objectives

1. Understand and profile the event log and document data quality.
2. Define a standard event-log schema.
3. Build the data pipeline (BigQuery and dbt) for a standardized event log.
4. Discover the process model and transition-time statistics with PM4Py.
5. Present findings in a Power BI dashboard with clear KPIs.

## 3. Technology stack

| Tool | Purpose |
|---|---|
| Google BigQuery | Storage of the raw and standardized event log |
| dbt | Transformation and testing of event-log models |
| PM4Py (Python) | Process discovery and transition-time analysis |
| Power BI | Dashboard and KPI reporting |
| GitHub | Version control and collaboration |

## 4. Team and responsibilities

| Member | Role |
|---|---|
| Satyam | Dataset structure and data-quality analysis |
| Vansh | BigQuery and dbt |
| Sakshi | Process mining with PM4Py |
| Tushar | Power BI dashboard development |

## 5. Dataset (summary)

- File: CareFlow filtered event log, Emergency Department Sepsis Pathway (synthetic demo).
- 9,855 events, 1,428 cases, 11 activities, 6 departments, 7 pathway variants.
- Columns: `Case ID`, `Event Index`, `Activity`, `Timestamp`, `Next Activity`, `Elapsed to Next (min)`, `Resource`, `Department`, `Status`.
- Timestamps are in UTC.
- Licence and redistribution terms: **to be confirmed with the team** before the data file is committed.

## 6. Repository structure

```
CareFlow/
├── README.md
├── assets/          # screenshots and wireframe images
├── docs/            # plans, KPI definitions, reports
├── powerbi/         # .pbix report and dashboard notes
└── (team folders for dbt, notebooks, etc.)
```

## 7. Workflow

- Each member works on their **own branch** (Tushar: `tushar-dashboard`).
- Commit small, meaningful changes with clear messages (e.g. `docs: add KPI definitions`).
- Open a **pull request** to merge into `main`; another member reviews it.
- Never commit credentials, API keys, or identifiable patient data.

## 8. Current status

| Area | Status |
|---|---|
| Dataset received | Completed |
| Power BI KPI list and required fields | Draft |
| Dashboard wireframe | Draft |
| Chart selection | Draft |
| Power BI report | Not started |
| BigQuery / dbt / PM4Py | Owned by teammates (see their branches) |

## 9. Documentation index

- [KPI and Required Fields](docs/KPI_and_Required_Fields.md)
- [Dashboard Wireframe](docs/Dashboard_Wireframe.md)
- [Chart Selection](docs/Chart_Selection.md)
