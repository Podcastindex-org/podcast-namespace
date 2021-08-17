

# The "podcast:hiveAccount" Sepcification

<small>Version 1.0 by Brian of London - 2021.06.08</small>

<br>

## Purpose

Allows for the easy inclusion of a definitive Hive Account to which Value for Value payments can be routed.

## Specification

For the `<podcast:hiveAccount>` tag there are no attributes, the tag just wraps the Hive account name string.

## Example

```xml
 <podcast:hiveAccount>no-agenda</podcast:hiveAccount>
```

## More Information

More information about the Hive Blockchain can be found at the [Dev Portal for Hive](https://developers.hive.io/#introduction-welcome)


# Alternate idea (but not quite as good)
## It could be done with FUNDING tag!

<podcast:funding hiveAccname="brianoflondon">Support Brian of London </podcast:funding>

This could even be done via URL, but

<podcast:funding url="https://hive.blog/@brianoflondon">Support Brian of London</podcast:funding>

however that would need to be passed on in the TLV records.