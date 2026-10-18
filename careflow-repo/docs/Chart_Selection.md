# Chart Selection

**Owner:** Tushar (Power BI)
**Status:** Draft (Week 1, Day 4 task)

| Business question | Visual | Fields | Why | What the viewer learns | Limitations |
|---|---|---|---|---|---|
| How many events per activity? | Clustered bar (horizontal, sorted) | Activity, Total Events | Easy comparison of long category names | Which steps are most frequent | Counts include repeats from rework |
| How many cases per activity? | Bar | Activity, Total Cases | Separates "cases touched" from "events" | Which steps few cases reach (CT, consult, ICU) | None beyond data scope |
| Who handles the work? | Bar | Resource, Total Events | Shows workload spread | Uneven workload across staff | Resource is a name or role, not a shift |
| What is the daily volume? | Line | Timestamp (date), Total Cases | Shows trend over time | Busy and quiet days | Synthetic dates |
| Which pathways occur most? | Table with data bars | Variant, Total Cases, Avg Pathway Duration | 7 variants fit a table well | Dominant path and rare paths | Needs the Variant field (proposed) |
| What share ends in each outcome? | Donut or 100% stacked bar | Final Activity, Total Cases | Part-of-whole with 3 values | Inpatient / ICU / Discharge mix | Final activity only, no clinical outcome |
| Where are the delays? | Bar sorted descending | Activity, Avg Elapsed to Next | Ranks steps by delay | Slowest steps | Mixes working and waiting time |
| Which transitions are slowest? | Matrix with conditional formatting | Activity, Next Activity, Avg Elapsed | Two-way comparison with colour | Hot-spot transitions | Rare transitions have few cases |
| How spread are the delays? | Column histogram (bins) | Elapsed to Next (min) | Shows variation, not just averages | Whether averages hide outliers | Binning needs setup |
| How does rework affect duration? | Clustered bar | Rework flag, Avg Pathway Duration | Direct with/without comparison | Time cost of re-draws | Correlation only |
| What does the patient journey look like? | See options below | Activity, Next Activity, Case ID | Process flow | Main paths and branches | Not a native Power BI visual |

## Process-flow options

| Option | Pros | Cons |
|---|---|---|
| Sankey custom visual (AppSource) | Interactive, responds to slicers | Third-party; check availability, licence and organisation policy; shows flows, not a true process model |
| PM4Py map as an image | Real process map, simple | Static, does not respond to slicers |
| Python visual with PM4Py | Draws the map inside the report | Works in Desktop; PM4Py is not supported in the Power BI Service; needs Python and Graphviz |
| Native fallback (matrix of Activity x Next Activity, Decomposition Tree) | Always available | Does not draw a flow diagram |

**Recommendation (to confirm with the mentor):** native visuals for pages 1, 2, 4, 5; Sankey or PM4Py image for page 3.

## Reference check

All visuals use confirmed CSV columns, except the "Variant" and "Rework flag" fields, which are proposed (see `KPI_and_Required_Fields.md`).
