##Reporting

Reporting occurs at the line level. The publisher must support the following GET calls to generate a click and impression report.

| URI                                         | Description                                                       |
|---------------------------------------------|-------------------------------------------------------------------|
| /accounts/{id}/orders/{id}/lines/stats      | Aggregates the impressions and clicks for all lines in the order. |
| /accounts/{id}/orders/{id}/lines/{id}/stats | Aggregates the impressions and clicks for the specified line.     |

The following identifies the properties of the report.

| Property          | Type    | Required/Optional | Description                                                                                          |
|-------------------|---------|-------------------|------------------------------------------------------------------------------------------------------|
| Clicks            | Long    | Required          | The number of clicks to date. The value must be zero if no clicks have occurred.                     |
| CTR               | Short   | Optional          | The click through rate to date. The formula to calculate CTR is (clicks / impressions) * 100.        |
| ImpressionsServed | Long    | Required          | The number of impressions served to date. The value must be zero if no impressions have been served. |
| ReportDate        | String  | Required          | The data and time of the report. The date and time is reported in the order’s time zone.             |
| Spend             | Decimal | Optional          | The amount spent to date.                                                                            |
