# Location

`<podcast:location>`

This tag is intended to describe the location of editorial focus for a podcast's content (i.e. "what place is this podcast about?"). The tag has many use cases and is one of the more complex ones. You are **highly encouraged** to read the full [implementation document](https://github.com/Podcastindex-org/podcast-namespace/blob/main/location/location.md) before starting to code for it.

### Parent

`<item>` or `<channel>`

### Count

Single

### Node Value

This is a free-form string meant to be a human readable location. It may conform to conventional location verbiage (i.e. "Austin, TX"), but it shouldn't be depended on to be parseable in any specific way. This value cannot be blank. Please do not exceed `128 characters` for the node value or it may be truncated by aggregators.

### Attributes

- **geo:** (recommended) This is a latitude and longitude given in ["geo" notation](https://github.com/Podcastindex-org/podcast-namespace/blob/main/location/location.md#geo-recommended) (i.e. "geo:30.2672,97.7431").
- **osm:** (recommended) The Open Street Map identifier of this place, given using the OSM notation (i.e. "R113314")

### Examples

```xml
<podcast:location geo="geo:30.2672,97.7431" osm="R113314">Austin, TX</podcast:location>
```

```xml
<podcast:location geo="geo:33.51601,-86.81455" osm="R6930627">Birmingham Civil Rights Museum</podcast:location>
```

```xml
<podcast:location geo="geo:-27.86159,153.3169" osm="W43678282">Dreamworld (Queensland)</podcast:location>
```
