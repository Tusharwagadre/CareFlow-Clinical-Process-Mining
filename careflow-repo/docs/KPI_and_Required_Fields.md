# KPI Definitions and Required Fields

**Owner:** Tushar (Power BI)
**Status:** Draft for team agreement
**Data:** Synthetic ED sepsis pathway event log (9,855 events, 1,428 cases). All values below are illustrative checks from this synthetic file, **not clinical findings**, and must be reconciled with the Python analysis.

## 1. Fields

### Confirmed fields (in the CSV)

| Field | Type | Used for |
|---|---|---|
| Case ID | Text | Counting cases, grouping events |
| Event Index | Whole number | Step order within a case |
| Activity | Text | Volumes, flow, outcomes |
| Timestamp | Date/time (UTC) | Date slicer, durations |
| Next Activity | Text | Transitions (blank on a case's last event) |
| Elapsed to Next (min) | Whole number | Delay between consecutive events (blank on last event) |
| Resource | Text | Workload by staff member |
| Department | Text | Slicer and volume breakdown |
| Status | Text | Event result (e.g. Sample Rejected, Case Concluded) |

### Proposed fields (to be created, not in the CSV)

| Field | How to create | Owner |
|---|---|---|
| Variant (path text) | Join each case's activities in order (Power Query or PM4Py output) | Tushar / Sakshi |
| Case Start / Case End | Min and max Timestamp per Case ID (DAX measure) | Tushar |
| Rework flag | Case has Activity = "Specimen Hemolysis / Re-draw Order" | Tushar |

### Not available (do not build KPIs on these)

Triage severity, patient outcome or mortality, sepsis score, event end time, waiting time separate from service time.

## 2. Assumptions

- `Elapsed to Next (min)` = minutes from the start of an event to the start of the next event. It mixes working time and waiting time.
- Pathway duration = last event timestamp minus first event timestamp. It ends at the **start** of the final event.
- Case outcome = the final activity of the case.

## 3. KPI list

| # | KPI | Definition | Required columns | Illustrative value* |
|---|---|---|---|---|
| 1 | Total Cases | Distinct Case ID | Case ID | 1,428 |
| 2 | Total Events | Count of rows | Case ID | 9,855 |
| 3 | Events per Case | Total Events / Total Cases | Case ID | 6.9 |
| 4 | Avg Pathway Duration (min) | Average of (last - first Timestamp) per case | Case ID, Timestamp | 253 |
| 5 | Time to Antibiotic (min) | Average of (Empiric Antibiotic timestamp - ED Registration timestamp) per case | Case ID, Activity, Timestamp | 164 |
| 6 | Avg Elapsed to Next (min) | Average of Elapsed to Next, by activity or transition | Elapsed to Next, Activity, Next Activity | 42.9 |
| 7 | Rework Rate (%) | Cases with a "Specimen Hemolysis / Re-draw Order" step / Total Cases | Case ID, Activity | 12.0% |
| 8 | Outcome Mix (%) | Cases ending in Inpatient Handover, ICU Transfer or Discharge / Total Cases | Case ID, Activity | 57.4% / 14.0% / 28.6% |
| 9 | Bottleneck Activity | Activity or transition with the highest average Elapsed to Next | Activity, Next Activity, Elapsed to Next | Antibiotic step (about 89 min) |

*Computed from the synthetic CSV for definition checks only. Confirm in Power BI and against Sakshi's results.

## 4. DAX (table name `EventLog`)

```DAX
Total Cases = DISTINCTCOUNT ( EventLog[Case ID] )

Total Events = COUNTROWS ( EventLog )

Events per Case = DIVIDE ( [Total Events], [Total Cases] )

Avg Pathway Duration (min) =
AVERAGEX (
    VALUES ( EventLog[Case ID] ),
    CALCULATE (
        DATEDIFF ( MIN ( EventLog[Timestamp] ), MAX ( EventLog[Timestamp] ), MINUTE )
    )
)

Avg Elapsed to Next (min) = AVERAGE ( EventLog[Elapsed to Next (min)] )

Rework Cases =
CALCULATE (
    DISTINCTCOUNT ( EventLog[Case ID] ),
    EventLog[Activity] = "Specimen Hemolysis / Re-draw Order"
)

Rework Rate = DIVIDE ( [Rework Cases], [Total Cases] )

Time to Antibiotic (min) =
AVERAGEX (
    VALUES ( EventLog[Case ID] ),
    VAR reg =
        CALCULATE ( MIN ( EventLog[Timestamp] ), EventLog[Activity] = "ED Registration & Triage" )
    VAR abx =
        CALCULATE ( MIN ( EventLog[Timestamp] ), EventLog[Activity] = "Empiric Antibiotic / Targeted Therapy" )
    RETURN DATEDIFF ( reg, abx, MINUTE )
)

Inpatient Rate =
DIVIDE (
    CALCULATE ( DISTINCTCOUNT ( EventLog[Case ID] ), EventLog[Activity] = "Inpatient Ward Bed Handover" ),
    [Total Cases]
)
```

Similar measures for **ICU Rate** (`"Direct ICU Bed Transfer"`) and **Discharge Rate** (`"Discharge to Outpatient Care"`).

**Caution:** the Rework and Time to Antibiotic measures set their own Activity filter. Do not put `Activity` on the axis of a visual that shows them.

## 5. Limitations

- Synthetic data: results show the dashboard works, not real hospital performance.
- Waiting and working time cannot be separated.
- Time to Antibiotic is a pathway timestamp difference, not a clinical compliance measure.
- Timestamps are UTC; decide whether to display local time.
