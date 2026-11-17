# Experiment plan

## Protocol fixed before model training

Issue forecasts on the hour for requests in the following 60 minutes. Sort all eligible zones by descending predicted long-wait count, breaking ties by ascending TLC LocationID. Select ten zones. Evaluate the same zone-hours and label definition for every candidate.

| Stage | Decision and acceptance condition |
| --- | --- |
| Acquire | Fetch the twelve 2024 HVFHV files plus the zone lookup. Record exact bytes, SHA-256 and row counts. |
| Audit and aggregate | Every source row gets one disposition. Accepted rows and delayed rows reconcile to the request/pickup-hour aggregate. |
| Build the panel | Generate every eligible zone-hour. Verify issue-time cutoffs, zero-volume rows, split boundaries and the handling of clock changes. |
| Fit | Use January–August for training, September–October for validation and November–December for the final comparison. Give the last request cohort in each split time to mature. |
| Select | Tune MLlib Poisson regularization and tree depth using validation capture at ten. Break a tie with lower validation MAE, then candidate name. Keep training dates fixed for the reported test comparison. |
| Compare | Score the frozen model families and baselines on the same test rows. Fit the history-only ablation with the full model's chosen settings. Report a worse model or negative ablation as observed. |
| Demonstrate | Reproduce batch counts with a real trip replay and restart its checkpoint. Time the aggregation at several input sizes and execution settings. |
| Present | Use the technical report, results artifacts and viva walkthrough to connect each rubric item to observable evidence. |

## Primary endpoint

For each test hour select exactly ten zones. Sum the observed long waits in selected zone-hours, then divide by all observed long waits in all test zone-hours. Pool the numerator and denominator across hours. A busy hour contributes more rides. A perfect-information ranking using the final labels gives an attainable ceiling for ten alerts. Report the model score, the ceiling, and the gap to a weekly repeat.

MAE and RMSE measure count calibration and expose large misses. Compare the distribution of the global ten slots and their burden coverage by borough. Summarize uncertainty in the difference from the weekly baseline by resampling whole calendar weeks of paired results. Temporal dependence and a short holdout limit the interpretation of this interval.

## What would change the interpretation

- If the weekly repeat wins, seasonal concentration explains more held-out performance than the added model complexity can recover.
- If fresh evidence improves count accuracy while capture stays flat, it is changing estimated severity without changing enough top-ten decisions.
- An impressive citywide score can coexist with low coverage in a borough. Allocation by expected count expresses a decision about total burden and needs its geographic breakdown.
- Replay agreement tests event-time bookkeeping. The public archive cannot establish actual operator reporting latency. Single-host measurements quantify local parallelism. Multi-node scaling requires a cluster run.

## Demonstration sequence

Start with the 16:55 request example. Show the source manifest and one row-disposition table. Walk from a request/pickup-hour cell to an issue-time feature vector. Open a real held-out alert list, compare it with the realized labels, and explain a missed zone. End with the measured runtime and checkpoint replay. [Execution guide](engineering.md)
