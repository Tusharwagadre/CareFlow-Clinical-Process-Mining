# Power BI Dashboard Plan and Wireframe

**Owner:** Tushar (Power BI)
**Status:** Draft wireframe (Week 1, Day 3 task)
**Canvas:** 16:9 (1280 x 720). A "SYNTHETIC DEMO DATA" label sits in the footer of every page.

## Global slicers (every page)

Date (from Timestamp), Department, Activity (where it does not conflict with the visuals).

## Page 1 — Executive Overview

**Question:** How is the pathway performing overall?

```
+--------------------------------------------------------------+
| Title + slicers: Date | Department                            |
+-----------+-----------+-----------+-----------+--------------+
| Total     | Avg       | Time to   | Rework    | Outcome      |
| Cases     | Pathway   | Antibiotic| Rate      | Mix (donut)  |
|           | Duration  |           |           |              |
+-----------+-----------+-----------+-----------+              |
| Cases per day (line)              | Events by | (cont.)      |
|                                   | Activity  |              |
+-----------------------------------+-----------+--------------+
```

Fields: Case ID, Timestamp, Activity, Elapsed to Next. KPIs 1, 4, 5, 7, 8.

## Page 2 — Activity Volumes and Workload

**Question:** Where is the work happening and who carries it?

```
+--------------------------------------------------------------+
| Slicers                                                      |
+--------------------------------+-----------------------------+
| Events by Activity (bar)       | Events by Department (bar)  |
+--------------------------------+-----------------------------+
| Events by Resource (bar)       | Activity x Department       |
|                                | (matrix)                    |
+--------------------------------+-----------------------------+
```

Fields: Activity, Department, Resource, Case ID.

## Page 3 — Patient Journey and Variants

**Question:** Which paths do patients actually follow?

```
+--------------------------------------------------------------+
| Slicers                                                      |
+--------------------------------------------------------------+
| Process flow (Sankey custom visual or PM4Py image)           |
+--------------------------------+-----------------------------+
| Variants table: path, cases,   | Cases by variant (bar)      |
| % of cases, avg duration       |                             |
+--------------------------------+-----------------------------+
```

Fields: Activity, Next Activity, Case ID, Variant (proposed). Process-map options and trade-offs are in `Chart_Selection.md`.

## Page 4 — Delays and Bottlenecks

**Question:** Which steps and transitions take the longest?

```
+--------------------------------------------------------------+
| Slicers                                                      |
+--------------------------------+-----------------------------+
| Avg Elapsed by Activity (bar,  | Avg Elapsed by transition   |
| sorted high to low)            | (matrix: Activity x Next)   |
+--------------------------------+-----------------------------+
| Elapsed distribution (column)  | Top 3 slowest (table)       |
+--------------------------------+-----------------------------+
```

Fields: Activity, Next Activity, Elapsed to Next (min). KPIs 6, 9.

## Page 5 — Rework and Quality

**Question:** How often does rework happen and what does it cost in time?

```
+--------------------------------------------------------------+
| Slicers                                                      |
+-----------+--------------------------------------------------+
| Rework    | Pathway duration: with vs without rework (bar)   |
| Rate card |                                                  |
+-----------+--------------------+-----------------------------+
| Status counts (bar)            | Rework cases by Resource    |
+--------------------------------+-----------------------------+
```

Fields: Status, Activity, Case ID, Resource. KPIs 4, 7.

## Design rules

- Two accent colours plus greys; one colour for "attention" items.
- KPI cards top-left, detail below.
- Same fonts, same card style and same KPI names as `KPI_and_Required_Fields.md`.
- Slicers at the top, titles state the question each visual answers.

## Open items

- Confirm the process-map approach with the mentor (custom visual or PM4Py image).
- Confirm UTC vs local time display.
- Pages are proposed; adjust after team review on Day 7.
