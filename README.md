# Pickup Pressure NYC

At the start of each hour, which ten New York City taxi zones will account for the most ride-hail requests that take at least ten minutes to reach pickup? This Apache Spark project studies that decision using completed trips in the 2024 NYC Taxi and Limousine Commission High Volume For-Hire Vehicle archive.

The experiment gives each forecast only evidence available by its issue time in a historical pickup-event replay. The score is the fraction of observed long waits captured by ten zone alerts per hour. The unit of prediction is a zone-hour. The decision is for an operations analyst reviewing potential service delays.

The inputs are twelve monthly trip files and the official taxi-zone lookup. The city publishes the archive monthly and warns that vendor submissions can be incomplete. The records describe serviced trips and do not measure cancellations. [NYC TLC trip records and data dictionaries](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page)
