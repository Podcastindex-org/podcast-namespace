# The `<podcast:updateFrequency>` Specification

<small>Version 1.0 by [Nathan Gathright](https://github.com/nathangathright) - 2023.02.13</small>

> A tag to provide podcasters a way to express their intended schedule for future releases.

## Purpose
In the spirit of taking platform-specific innovations and making them accessible to the open ecosystem of RSS, the intent of this proposal is to replicate the purpose of the “Update Frequency” field in Apple Podcasts Connect as well as replace the `<itunes:complete>` tag for podcasts that have no intention to release further episodes.

Additionally, apps like Overcast, Pocket Casts, and Podchaser analyze the intervals between past episodes to estimate a prediction for future releases. Under certain conditions or naive algorithms, this can be inaccurate:
* New podcasts that only have a trailer
* Limited-run podcasts nearing the end of a season
* “Daily” podcasts that only release on weekdays
* Podcasts with consistent releases at irregular intervals (every Monday and Wednesday)

If podcasters can unambiguously communicate their release schedule, then more apps can provide accurate information to their listeners.

## Parent
`<channel>`

## Count
Single

## Node value

The node value is a free-form string, which might be displayed alongside other information about the podcast. Please do not exceed `128 characters` for the node value or it may be truncated by aggregators.

## Attributes
* **complete (optional):** Boolean specifying if the podcast has no intention to release further episodes. If not set, this should be assumed to be false.
* **dtstart (optional):** The `date` or `datetime` the recurrence rule begins as an [ISO8601-fornmatted](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toISOString) string. If the `rrule` contains a value for `COUNT`, then this attribute is required. If the `rrule` contains a value for `UNTIL`, then the value of this attribute must be formatted to the same date/datetime standard.
* **rrule (recommended):** A recurrence rule as defined in [iCalendar RFC 5545 Section 3.3.10](https://www.rfc-editor.org/rfc/rfc5545#section-3.3.10).

## Examples

Recreating most of Apple Podcasts Connect’s “Update Frequency” values is easily achieved:
```xml
<podcast:updateFrequency rrule="FREQ=DAILY">Daily</podcast:updateFrequency>
<podcast:updateFrequency rrule="FREQ=WEEKLY">Weekly</podcast:updateFrequency>
<podcast:updateFrequency rrule="FREQ=WEEKLY;INTERVAL=2">Biweekly</podcast:updateFrequency>
<podcast:updateFrequency rrule="FREQ=MONTHLY">Monthly</podcast:updateFrequency>
<podcast:updateFrequency rrule="FREQ=MONTHLY;INTERVAL=2">Bimonthly</podcast:updateFrequency>
<podcast:updateFrequency rrule="FREQ=MONTHLY">Monthly</podcast:updateFrequency>
<podcast:updateFrequency rrule="FREQ=YEARLY">Yearly</podcast:updateFrequency>
```

However, greater precision can be easily communicated:
```xml
<podcast:updateFrequency rrule="FREQ=DAILY;INTERVAL=1;BYDAY=MO,TU,WE,TH,FR">Every weekday</podcast:updateFrequency>
<podcast:updateFrequency rrule="FREQ=WEEKLY;BYDAY=MO,WE">Every Monday and Wednesday</podcast:updateFrequency>
<podcast:updateFrequency rrule="FREQ=WEEKLY;BYDAY=FR;BYMONTHDAY=13">Every friday the 13th</podcast:updateFrequency>
<podcast:updateFrequency rrule="FREQ=YEARLY;BYDAY=+4TH;BYMONTH=11">Every year on American Thanksgiving</podcast:updateFrequency>
```

Limited-run podcasts can indicate when they’ll go on hiatus by setting an UNTIL date or a COUNT:
```xml
<podcast:updateFrequency rrule="FREQ=WEEKLY;INTERVAL=2;BYDAY=MO;COUNT=10" dtstart="2023-08-28T00:00:00.000Z">Every other Monday for 10 episodes starting on Aug 28, 2023</podcast:updateFrequency>
<podcast:updateFrequency rrule="FREQ=WEEKLY;UNTIL=2023-12-31T00:00:00.000Z;BYDAY=MO">Every Monday until Dec 31, 2023</podcast:updateFrequency>
```

Podcasts currently on hiatus can indicate their intention to resume production by setting a DTSTART value in the future:
```xml
<podcast:updateFrequency dtstart="2025-01-01T00:00:00.000Z" rrule="FREQ=WEEKLY">Weekly, starting in 2025</podcast:updateFrequency>
```

Complete podcasts with no intention to release further episodes:
```xml
<podcast:updateFrequency complete="true">That’s all folks!</podcast:updateFrequency>
```
