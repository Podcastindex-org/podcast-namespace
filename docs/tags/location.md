# Location

`<podcast:location>`

This tag is intended to describe the location of editorial focus, or the source of production 
for a podcast's content (i.e. "what place is this podcast about?"). The tag has many use cases 
and is one of the more complex ones. You are **highly encouraged** to read the 
full [implementation document](https://github.com/Podcastindex-org/podcast-namespace/blob/main/location/location.md) 
before starting to code for it.

### Parent

`<item>` or `<channel>` or `<liveItem>`

### Count

Multiple

### Node Value

This is a free-form string meant to be a human readable location. It may conform to conventional location 
verbiage (i.e. "Austin, TX"), but it shouldn't be depended on to be parseable in any specific way. This value 
cannot be blank. Please do not exceed `128 characters` for the node value or it may be truncated by aggregators.  This
value is mostly intended as a "display" value and it's highly encouraged to use the other recommended attributes to
define the actual location parameters.

### Attributes

- **rel:** (recommended) The `rel` attribute can contain one of the following possible values:
  - `"subject"` (default) - The location refers to what/where the content is about.
  - `"creator"` - The location refers to where the content was recorded or produced.
- **geo:** (recommended) This is a latitude and longitude given in ["geo" notation](https://github.com/Podcastindex-org/podcast-namespace/blob/main/location/location.md#geo-recommended) (i.e. "geo:30.2672,97.7431").
- **osm:** (recommended) The Open Street Map identifier of this place, given using the OSM notation (i.e. "R113314")
- **country:** (recommended) 

### Examples

- A recording about Austin Texas in the US:
```xml
<podcast:location geo="geo:30.2672,97.7431" osm="R113314">Austin, TX</podcast:location>
```

- A recording about the Birmingham Civil Rights Museum in Birmingham, AL:
```xml
<podcast:location geo="geo:33.51601,-86.81455" osm="R6930627" country="us">Birmingham Civil Rights Museum</podcast:location>
```

- A show that is RECORDED IN Austin, TX - about a themepark in Australia:
```xml
<podcast:location geo="geo:30.2672,97.7431" rel="creator" osm="R113314">Austin, TX</podcast:location>
<podcast:location geo="geo:-27.86159,153.3169" rel="subject" osm="W43678282">Dreamworld (Queensland)</podcast:location>
```

- A podcast episode that is about both Dreamworld *and* the Eiffel Tower:
```xml
<podcast:location geo="geo:48.858093,2.294694" osm="W5013364" country="fr" rel="subject">Eiffel Tower, Paris</podcast:location>
<podcast:location geo="geo:-27.86159,153.3169" rel="subject" osm="W43678282" country="au">Dreamworld (Queensland)</podcast:location>
```