# DGMARL Field-Test Logs

This repository preserves original output logs from field testing of a DGMARL traffic-signal controller. It accompanies the manuscript review and provides archival evidence for examining the controller's software operation.

The field trials were short-duration operational validation trials, not comparative traffic-performance experiments. These records alone do not establish improvements in delay, stops, queues, or throughput.

## Repository contents

`raw_logs/` contains 2,393 original CSV and TXT output files. Analysis scripts and derived reports are excluded. The archive includes runs beyond the three manuscript trial windows.

| Filename prefix | Files | Record family |
| --- | ---: | --- |
| `dgmarl_control_` | 117 | Policy and updated-action records |
| `dgmarl_cycles_fieldtest_` | 117 | Software cycle records |
| `dgmarl_node_cycle_stat_` | 117 | Node cycle statistics |
| `dgmarl_node_detailed_stat_` | 117 | Detailed node/signal-group state records |
| `dgmarl_pushbutton_history_fieldtest_` | 114 | Pedestrian request/service history |
| `dgmarl_signalstatus_` | 1,694 | Generated signal-status histories |
| `dgmarl_VehsInNet_fieldtest_` | 117 | Vehicle-count and temporal-demand records |

Record layouts may vary by run, and some files do not contain column headers. Filename descriptions are a navigation aid, not a complete data dictionary. Interpret ambiguous fields only with the corresponding field-testing source code or a verified schema.

## Reported manuscript trials

| Trial | Date | Reported local time window | Nominal duration |
| --- | --- | --- | --- |
| 1 | 2023-10-20 | 14:56–15:01 (2:56–3:01 PM) | 5 min |
| 2 | 2023-10-23 | 09:45–09:50 (9:45–9:50 AM) | 5 min |
| 3 | 2023-10-23 | 13:30–13:45 (1:30–1:45 PM) | 15 min |

Author-provided run identifiers for locating candidate records are 1149, 1150, 1152, and 1153 for October 20, and 1154 and 1155 for October 23. These identifiers do not establish overlap with a trial. Verify timestamps inside each file before including records. Some files cover only part of a reported window or fall outside it entirely.

For reproducible windowed summaries, use start-inclusive, end-exclusive intervals and report the first and last included timestamps for each source and intersection. Establish timestamp units and timezone before comparing different record families; do not silently mix epoch timestamps with timezone-naive local timestamps. Counts describe available records only and must not be extrapolated to unlogged portions.

## Intersections and incremental deployment

| Node ID | Intersection |
| --- | --- |
| 3 | Broad St. |
| 4 | Market St. |
| 5 | Georgia Ave. |
| 6 | Lindsay St. |

The author reports commissioning in this order: Broad; Broad + Market; Georgia added; Lindsay added to complete the four-intersection deployment. Do not assume all four intersections were physically deployed in every run. A node's appearance in a software log does not by itself prove active physical control. Correspondence between deployment stages and manuscript trials requires timestamped configuration or other deployment evidence; it is not asserted by this README. Other node identifiers may appear in the archive.

## Interpretation notes

- **Policy versus updated actions:** Keep raw policy outputs and post-processing action codes separate. Equality is described as "logged action-code unchanged" and inequality as "logged action-code changed." Neither establishes the reason for a change or proves a particular constraint intervention.
- **Signal-status history:** The author-provided output-code mapping is `0 = RED`, `1 = AMBER`, `2 = GREEN`, and `3 = NEXT_GREEN`. `NEXT_GREEN` is an upcoming-green software state, not a physical signal indication. Do not apply this mapping automatically to differently encoded fields in other record families.
- **Physical actuation:** Generated GREEN entries, output-code changes, and software cycle starts are not independently verified physical phase transitions. Controller-facing records do not establish delivery or acknowledgment without corresponding communication records.
- **Pedestrians:** Request/service activations describe software recognition and service state. Where direct pairing is supported, delays are recognition-to-software-service delays, not verified button-press-to-WALK times. Repeated active samples are not separate requests.
- **Vehicle records:** The author-provided headerless layout for `dgmarl_VehsInNet_fieldtest_` files is `[sg_id, veh_count, nodeid, curr_timestamp, current_t]`. `current_t` is reconstructed temporal demand, not measured vehicle waiting time. Repeated vehicle-count samples must not be summed as unique vehicles or throughput.
- **Timing:** Differences between successive output timestamps describe observed record spacing, not a configured loop period or confirmed command-delivery rate. Observed CV data-update intervals are not physical camera frame rates.
- **Safety and communication:** Missing timestamps or signal-code changes alone do not establish communication failures, fallback, flashing red, or conflicting physical indications. Such claims require explicit supporting records and appropriate signal/conflict definitions.

## Guidance for review and reuse

For each reported metric, identify the exact source filenames, node/signal-group scope, timestamp filtering, available coverage, counting unit, and any deduplication or event-pairing rules. Mark unsupported metrics as unavailable rather than zero. Preserve source ordering when checking repeated or backward timestamps, and do not join unrelated runs solely because their timestamps are close.

This repository is an archival log collection, not a complete runnable controller implementation or a comparative performance dataset. To make references reproducible, cite the repository URL together with the commit identifier used for analysis.
