## Location tag format details

Below, you will find implementation details and UX recommendations for the `<podcast:location>` tag.

<br>

### Format

- **\<podcast:location name="[humanly readable place name]" (geo="[geoURI]") (osmid="[OSM type][OSM id]") />** (finalized)

   Channel or Item (optional | multiple)

   - `name` (required) This is a free-form string meant to be a human readable location.  It may conform to conventional location verbiage (i.e. "Austin, TX"), but it shouldn't be depended on to be parseable in any specific way.
   - `geo` (optional) This is a latitude and longitude given in "geo" notation (i.e. "geo:30.2672,97.7431").
   - `osmid` (optional) The Open Street Map identifier of this place, given using the OSM notation (i.e. "R113314")

   The maximum recommended string length of all attribute values is 128 characters.


<br><br>


This tag is intended to describe the location of editorial focus for a podcast's content - i.e. "what place is this podcast about?"  It can exist at either the channel level or the item level, or both.

The use-cases for this tag are multiple, in order of complexity:

1. To allow a free-text "location" field to be visible in a podcast app, perhaps during playback
2. To allow a simple point on a map to be visible in a podcast app
3. To allow a search for "podcasts/episodes about places near me" - for, for example, a travel or news podcast
4. To describe a specific place in a programmatic fashion to allow complex geo-aware searches


It may allow very complex searches such as:

- Show me podcasts or episodes about places near me
- Show me podcasts about train stations in Germany
- Show me podcasts about mines in West Australia
- Visa mig podcaster om platser i Kalifornien på svenska - "Show me podcasts about places in Californian, returning Swedish-language podcasts only" (using the <language> tag)


Unlike other elements in the "podcast" namespace, a "place" is not permanent. Places are built, and abandoned, all the time. Buildings are demolished, businesses close.

On the other hand, a point on the earth is permanent, but does not describe anything other than a point. This is not always great when wanting to describe a city, rather than an area in a city,
or a restaurant within a city. "Locations" are also, not always real places, especially in fiction podcasts.

This, therefore, means that the podcast:location tag is complex and has a number of attributes.

<br>

### Structure

```
<podcast:location
  name="[humanly readable place name]"
  geo="[geoURI]"
  osmid="[OSM type][OSM id]"
>
```

\- **mandatory**: `name="[Humanly readable name]"` - this is meant for podcast apps to display the name of the location that the podcast is about. Examples might be "Houses of Parliament", "Gitmo Nation" or
"Ernest Murrow Theater, Chicago"). This is not intended to be programmatically parsed and is for display only. For a programmatic designation of the location, use OSM IDs, below.

This field is a maximum of 64 characters. It may describe a real or fictional place. It should be in the same language as the podcast, as indicated in the <language> RSS tag: so a podcast in en should
read Eiffel Tower, Paris and not La Tour d'Eiffel.

\- **recommended**: `geo="[geoURI]"` - a geo URI, conformant to [RFC 5870](https://tools.ietf.org/html/rfc5870).

Examples:

- geo:37.786971,-122.399677 (a simple latlon description)
- geo:37.786971,-122.399677,250 (a latlon including a height of 250 meters above ground level)
- geo:37.786971,-122.399677;u=350 (a latlon with an accuracy ('uncertainty') of 350 meters).
- For information that may interest space travellers: the RFC does include an optional coordinate reference system for other planets, though these are not recommended to be used yet by the RFC.

`geo` is recommended to be used alongside an OSMID. Since OSM IDs are not guaranteed to be permanent (perhaps it's the ID of a building which is later demolished), the geoURI serves as a permanent point.
Exceptions are podcasts from, or about, fictional places. Data within these tags must relate to a real place.

\- **recommended**: `osmid="[OSM type][OSM id]"` - from an OpenStreetMap query. If a value is given for osmid it must contain both 'type' and 'id'. osm type: A one-character description of the type of OSM point.
Valid is "N" (node); "W" (way); "R" (relation). osm id: The ID of the OpenStreetMap feature that is described.

This may describe part of a building, a building or business, a suburb, city, state, or country - anything within the OSM database, using the OpenStreetMap API or a local copy of the data. This is the field
that is the best programmatic representation of the place being described. The data within OpenStreetMap is rich and can be used for detailed searches.

Examples:

- The United States of America: [R148838](https://nominatim.openstreetmap.org/ui/details.html?osmtype=R&osmid=148838)
- The Eiffel Tower in Paris: [W5013364](https://nominatim.openstreetmap.org/ui/details.html?osmtype=W&osmid=5013364)

The `osmid` is recommended to be used alongside a geo tag. Since OSM IDs are not guaranteed to be permanent (perhaps it's the ID of a building which is later demolished), the geoURI serves as a permanent
point. Exceptions are podcasts from, or about, fictional places. Data within these tags must relate to a real place.

If a developer uses the osmid tag, the canonical latlon is the one returned by OSM. It is intended that the geo tag is used for simple display within a podcast app without any API usage: but for more advanced
 uses, like a geographic search, developers will ingest the full details from OpenStreetMap. The geoURI also offers a useful fallback should the osmid be removed.

_Caution: our definition of osmid is what OpenStreetMap call "OSM type and OSM id". It must start with an alphabetical representation of the type, then the numerical ID. Do not use place_id, which is visible in
API calls - these are unique to each mirror of the OSM data._


### UX Suggestion for Podcast Hosting Platforms

The quality of this data is important to ensure a good listener experience. A podcast publisher should be in no doubt what data is being asked for here, to clarify that this is about a location that is mentioned
in the podcast. The wording of this feature is important to ensure the correct data is available.

![image](https://user-images.githubusercontent.com/1498236/101383942-6c113080-387f-11eb-9cc2-a5a4e5dd19de.png)

Podcast hosts may also wish to remind podcast publishers to always be cautious about posting public location information. It's possible to check the OSM type to see if it relates to a residential address.


### Examples

For a podcast that is talking about the Eiffel Tower, (but actually made in Birmingham AL), this is what the specification would suggest:

```
<podcast:location
  name="Eiffel Tower, Paris"
  geo="geo:48.858093,2.294694"
  osmid="W5013364"
>
```

For a podcast that is set in Gitmo Nation, a nickname used by the show for the United States of America:

```
<podcast:location
  name="Gitmo Nation"
  geo="geo:39.7837304,-100.445882;u=3900000"
  osmid="R148838"
>
```

The geo point uses an optional 'uncertainty' value here of 3,900 km, indicating that the "location" described here is up to 3,900km away from the point given (which is the rough width of the USA). The OSMID
includes a more accurate bounding box and geoJSON.

For a podcast that is about Hogwarts, a fictional location, geo and osmid must not be entered.

```
<podcast:location
  name="Hogwarts"
>
```

For a podcast from Tesla upon landing on Mars

```
<podcast:location
  name="Tesla Base 3"
  geo="geo:37.786971,-122.399677;crs=Mars-2031"
>
```

(The co-ordinate reference system from Mars doesn't yet exist, but this shows the extensibility of this tag).


### What This Tag Isn't Built For

For privacy and user experience, this tag is not meant as a description of the physical location of podcast hosts and guests ("I'm doing this podcast in Denver, Colorado!"). The physical location of people
are available via the podcast:people tag's links to places like Twitter, Facebook, Wikipedia and Podchaser.