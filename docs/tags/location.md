# Location

`<podcast:location>`

_Revised in 2025:_ This tag is intended to describe the location of editorial focus, or the source of production
for a podcast's content; answering the question "where is this podcast about?" or "where was the podcast made?".
The tag has many use cases and is one of the more complex ones.

> [!IMPORTANT]
> You are **highly encouraged** to read the
> full [implementation document](../examples/location/location.md)
> before starting to code for it. This document includes rationale and example code.

### Parent

`<item>` or `<channel>` or `<liveItem>`

### Count

Multiple

### Node Value

This is a free-form string meant to be a human readable location. It may conform to conventional location
verbiage (i.e. "Austin, TX"), but it shouldn't be depended on to be parseable in any specific way. This value
cannot be blank. Please do not exceed `128 characters` for the node value or it may be truncated by aggregators. This
value is mostly intended as a "display" value and it's **highly encouraged** to use the other recommended attributes to
define the actual location parameters.

### Attributes

- **rel:** (recommended) The `rel` attribute can contain one of the following possible values:
  - `"subject"` (default) - The location refers to what/where the content is about.
  - `"creator"` - The location refers to where the content was recorded or produced.
- **geo:** (recommended) A latitude and longitude in geoURI form, following [RFC5870](https://datatracker.ietf.org/doc/html/rfc5870) (i.e. "geo:30.2672,97.7431").
- **osm:** (recommended) The [OpenStreetMap](https://www.openstreetmap.org/#map=13/41.39239/2.14036) identifier of this place. Made by taking the first character of the [OSM object type](https://locationiq.com/glossary/osm-type) (Node, Way, Relation), followed by the ID. (i.e. "R113314")
- **country:** (recommended) A two-letter code for the country, following [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)

> [!NOTE]
> While all elements are "recommended", **the location tag works best when all elements are populated.** The [implementation document](../examples/location/location.md) goes into more detail. An example location generator (entirely in JavaScript) is [over here](https://jamescridland.github.io/podcast-location-generator/).

### Examples

- A podcast _made in_ [Austin TX](https://www.openstreetmap.org/relation/113314) in the US:

```xml
<podcast:location
 rel="subject"
 geo="geo:30.2711286,-97.7436995"
 osm="R113314"
 country="US"
>Austin</podcast:location>
```

- A podcast about the [Birmingham Civil Rights Museum](https://www.openstreetmap.org/relation/6930627) in Birmingham, AL:

```xml
<podcast:location
 rel="subject"
 geo="geo:33.5159981,-86.8146098"
 osm="R6930627"
 country="US"
>Birmingham Civil Rights Museum</podcast:location>
```

- A podcast _made in_ [Marlow, England](https://www.openstreetmap.org/relation/3727240) - _about_ [Dreamworld](https://www.openstreetmap.org/relation/16317988), a themepark in Australia:

```xml
<podcast:location
 rel="creator"
 geo="geo:51.5718706,-0.7769654"
 osm="R3727240"
 country="GB"
>Marlow</podcast:location>
<podcast:location
 rel="subject"
 geo="geo:-27.8611449,153.3162701"
 osm="R16317988"
 country="AU"
>Dreamworld</podcast:location>
```
