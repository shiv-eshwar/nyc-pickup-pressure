# Pickup Pressure NYC

## The decision

A city mobility analyst can investigate ten pickup zones each hour. At 17:00, which ten zones are likely to account for the most requests made from 17:00 up to 18:00 that will take at least ten minutes to reach pickup? A ranked list and an expected count per zone give the analyst a place to start. One selected zone costs one review slot, regardless of its size.

The outcome is measurable on archived trips. It describes the burden of long waits among serviced rides. An operator would need a separate intervention study to estimate whether dispatch changes shorten those waits. Cancellations, unserved demand and individual passenger identity are outside the coverage of these records. [TLC trip dictionary](https://www.nyc.gov/assets/tlc/downloads/pdf/data_dictionary_trip_records_hvfhs.pdf)

## What makes the study difficult

The clock on a request and the clock on its outcome differ. A rider requests a car at 16:55 and is picked up at 17:12. That ride eventually adds one long wait to the 16:00 request cohort. At 17:00 the replay has seen no pickup for it. A simple shift of the final 16:00 counts would quietly give the forecaster the answer for rides still in progress.

The project builds two hourly histories from the same trips. One assigns outcomes to the hour of request. The other records when those outcomes first become observable at pickup. Joining them by forecast issue time lets us test whether fresh, partially observed counts improve on older, fully observed history.

The proposed contribution is an empirical decision benchmark. It connects the two clocks, a fixed attention budget, a seasonality baseline, and a geographic audit using a single reproducible protocol. Existing research already addresses ride-hail waiting, demand forecasting and geographic differences. The interesting question is how much extra delay burden a time-correct forecast can locate. [Prior work](literature.md)

## Population and outcome

- **Study population.** All twelve 2024 NYC High Volume For-Hire Vehicle monthly files, restricted to pickups in the five city boroughs. The official lookup defines eligible zones. Every eligible zone is scored at every retained hour, including hours with zero trips.
- **Wait.** Pickup time minus requested pickup time in the source. Keep intervals from 0 through 120 minutes, including the endpoints. At least 600 seconds qualifies as a long wait. The upper bound defines the cohort and a finite time when an hourly label is complete. Scheduled pickups and reporting errors can affect this proxy.
- **Label.** For one zone and one request hour, count the eligible completed records with long waits. All such waits receive equal weight. A count target prioritizes absolute burden. A small zone can have a high delay rate and receive few alerts.
- **Availability.** In the replay the pickup event reveals the request time, pickup time and pickup zone. TLC releases archival files monthly, typically with a two-month delay. An operational deployment would need an event feed with comparable fields and timing. [Publisher and release policy](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page)
- **Design choices.** Ten minutes, ten review slots and the two-hour cap are study parameters. The cap, excluded rows, label maturity and DST policy are recorded with the results. No passenger-level identifying data is needed.

## Questions the experiments answer

1. How much observed long-wait burden falls in the ten zones picked each hour? Compare a weekly repeat and a recent mature count with Spark MLlib Poisson and gradient-boosted tree regressors.
2. How much do newly observed pickups and partially observed request cohorts add? Remove just those features and fit the same model settings on the same dates.
3. Which boroughs receive the alert slots, and what fraction of each borough's observed burden falls inside them?
4. How do input volume, worker threads and shuffle partitions affect the cost of producing the hourly counts? Can a checkpointed event-time replay reproduce batch pickup-hour counts?

## Fit to the 30-mark rubric

| Component | Marks | Evidence to defend |
| --- | ---: | --- |
| Problem and literature | 2 | Operational question, timestamp counterexample and comparison with the closest prior work |
| Collection and preprocessing | 4 | Publisher manifest, checksums, exclusive rejection counts, zone join, dense panel and temporal exclusions |
| Apache Spark MLlib | 7 | DataFrame transformations, vector assembly, two estimator families, validation grid and saved pipelines |
| Evaluation and comparison | 3 | Chronological test, capture at ten, MAE, RMSE, oracle ceiling, ablation and borough breakdown |
| Scalability and streaming | 4 | Timed Spark workloads, partition settings, real event-time windows, checkpoint restart and batch parity |
| Viva | 10 | Trace a late pickup, defend the denominator, interpret an error and demonstrate Spark execution |

## Reading guide

The [dataset card](datasets.md) identifies every input. The [architecture](architecture.md) traces a trip into a forecast. The [engineering guide](engineering.md) explains how to run and verify it. The [experiment plan](plan.md) fixes the comparisons, and the [progress log](progress.md) records their actual status.
