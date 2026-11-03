# Model interpretation

![Supplied Airline model](Schema.png)

The diagram shows Customer Loyalty History on the one side of a one-to-many relationship to Customer Flight Activity, with filtering toward activity. Calendar connects to loyalty history through one solid and one dashed relationship and to activity through a dashed relationship. A disconnected Measures Table contains calculations.

The exported activity measures name Calendar[Date] and Customer Flight Activity[Flight Booked Start of Month] in USERELATIONSHIP. The cancellation measure names Calendar[Date] and Customer Loyalty History[Cancellation Start of Month]. The exact active Calendar-to-history endpoint is not exposed by this screenshot; enrollment date is a hypothesis to verify, not a confirmed property.

These date roles need explicit interpretation: activity month, enrollment month, cancellation month, and membership cutoff are different questions. Activating a date role must be tested against other active filtering paths. CLV has no date role explicitly selected in its expression, so its period interpretation depends on the model context.

Before certifying the model, inspect relationship endpoints, cardinalities, active status and directions; test dimension uniqueness, unmatched fact keys, duplicate member-month records, and date coverage. The screenshot alone cannot explain the tier/headline discrepancy.

See [DAX review](../Dax%20Measures/README.md) and [validation record](../Docs/Validation.md).
