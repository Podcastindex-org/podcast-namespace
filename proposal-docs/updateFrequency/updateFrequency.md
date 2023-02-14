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
* **rrule (optional):** A recurrence rule as defined in [iCalendar RFC 5545 Section 3.3.10](https://www.rfc-editor.org/rfc/rfc5545#section-3.3.10). Podcasters should not be expected to be able to write a valid recurrence rule themselves. There are [several libraries](https://github.com/topics/rrule) to generate valid recurrence rules from form data or natural language text inputs.

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
<podcast:updateFrequency rrule="FREQ=DAILY;INTERVAL=1;WKST=MO;BYDAY=MO,TU,WE,TH,FR">Every weekday</podcast:updateFrequency>
<podcast:updateFrequency rrule="FREQ=WEEKLY;BYDAY=MO,WE">Every Monday and Wednesday</podcast:updateFrequency>
<podcast:updateFrequency rrule="FREQ=WEEKLY;BYDAY=FR;BYMONTHDAY=13">Every friday the 13th</podcast:updateFrequency>
<podcast:updateFrequency rrule="FREQ=YEARLY;BYDAY=+4TH;BYMONTH=11">Every year on American Thanksgiving</podcast:updateFrequency>
```

Limited-run series can set expectations clearly:
```xml
<podcast:updateFrequency rrule="FREQ=WEEKLY;INTERVAL=2;COUNT=10">Every other week for 10 episodes</podcast:updateFrequency>
<podcast:updateFrequency rrule="FREQ=WEEKLY;UNTIL=20231231;BYDAY=MO">Every Monday until Dec 31, 2023</podcast:updateFrequency>
```

Completed series with no intention to release further episodes:
```xml
<podcast:updateFrequency completed="true">That’s all folks!</podcast:updateFrequency>
```
