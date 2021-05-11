## Location tag format details

Below, you will find implementation details and UX recommendations for the `<podcast:location>` tag.

<br>

### Overview

This tag is intended to describe the location of editorial focus for a podcast's content - i.e. "what place is this podcast about?"  It can exist at either the channel level or the item level, or both.

The use-cases for this tag are multiple, in order of complexity:

1. To allow a free-text "location" field to be visible in a podcast app, perhaps during playback.
2. To allow a simple point on a map to be visible in a podcast app.
3. To allow a search for "podcasts/episodes about places near me" - or, for example, a travel or news podcast.
4. To describe a specific place in a programmatic fashion to allow complex geo-aware searches.


It may allow complex searches such as:

- Show me podcasts or episodes about places near me.
- Show me podcasts about train stations in Germany.
- Show me podcasts about mines in West Australia.
- Visa mig podcaster om platser i Kalifornien på svenska - "Show me podcasts about places in California, returning Swedish-language podcasts only" (using the `<language>` tag).


Unlike other elements in the `podcast` namespace, a "place" is not permanent. Places are built, and abandoned, all the time. Buildings are demolished, businesses close.

On the other hand, a point on the earth is permanent, but does not describe anything other than a point. This is not always great when wanting to describe a city, rather than an area within a city,
or a restaurant within a city. "Locations" are also, not always real places, especially in fiction podcasts.

This, therefore, means that the `<podcast:location>` tag is complex and has a number of attributes.

<br>

### Structure

```xml
<podcast:location
  geo="[geo URI]"
  osm="[OSM type][OSM id](#OSM revision)"
>[human-readable place name]</podcast:location>
```

This tag can exist at either the `<channel>` level, or the `<item>` level, or both. The maximum recommended string length of all attribute values is 128 characters.

<br>

#### Tag Node Value **required**

This is meant for podcast apps to display the name of the location that the podcast is about. Examples might be "Houses of Parliament", "Gitmo Nation" or "Ernest Murrow Theater, Chicago". This is not intended to be programmatically parsed and is for display only. For a programmatic designation of the location, use the geo URI or OSM IDs, below.

This value is a maximum of 128 characters. It may describe a real or fictional place. It should be in the same language as the podcast, as indicated in the <language> RSS tag: so a podcast in `en` should read "Eiffel Tower, Paris" and not "La Tour d'Eiffel".

#### `geo` **recommended**

 A geo URI, conformant to [RFC 5870](https://tools.ietf.org/html/rfc5870).

 Examples:

 - `geo:37.786971,-122.399677`, a simple latlon description.
 - `geo:37.786971,-122.399677,250`, a latlon including a height of 250 meters above ground level.
 - `geo:37.786971,-122.399677;u=350`, a latlon with an accuracy ('uncertainty') of 350 meters.

For information that may interest space travelers: the RFC does include an optional coordinate reference system for other planets, though these are not recommended to be used yet by the RFC.

The `geo` attribute is recommended to be used alongside an `osm` attribute. Since OSM IDs are not guaranteed to be permanent (perhaps it's the ID of a building which is later demolished), the geo URI serves as a permanent point.

Data within these attributes must relate to a real place. This tag must not be used for podcasts from or about fictional places. 

<br>

#### `osm` **recommended**

From an [OpenStreetMap](https://en.wikipedia.org/wiki/OpenStreetMap) query. If a value is given for `osm` it must contain both 'type' and 'id'.

 - osm type: A one-character description of the type of OSM point. Valid is "N" (node); "W" (way); "R" (relation).
 - osm id: The ID of the OpenStreetMap feature that is described.
 - osm revision: an _optional_ revision ID for an OSM object, preceded by a hash.

This may describe part of a building, a building or business, a suburb, city, state, or country - anything within the OSM database, using the OpenStreetMap API or a local copy of the data. This is the field that is the best programmatic representation of the place being described. The data within OpenStreetMap is rich and can be used for detailed searches.

Examples:

 - The United States of America: [R148838](https://nominatim.openstreetmap.org/ui/details.html?osmtype=R&osmid=148838)
 - The Eiffel Tower in Paris: [W5013364](https://nominatim.openstreetmap.org/ui/details.html?osmtype=W&osmid=5013364)
 - Paris, but - optionally - the revision made on 8 Jan 2021: [R7444#188](https://www.openstreetmap.org/relation/7444/history)

The `osm` is recommended to be used with a `geo` attribute. Since OSM IDs are not guaranteed to be permanent (perhaps it's the ID of a building which is later demolished), the geo URI serves as a permanent point. Data within these tags must relate to a real place.

If a developer uses the `osm` tag, the canonical latlon is the one returned by OSM. It is intended that the `geo` attribute is used for simple display within a podcast app without any API usage: but for more advanced uses, like a geographic search, developers will ingest the full details from OpenStreetMap. The geo URI also offers a useful fallback should the `osmid` be removed.

The revision number for the edit is a special case that allows for more permanent links, with the drawback that these are not fully supported by Nominatim and require a full OSM database complete with changesets. It is recommended to simply link to the object in OSM normally, without a revision number, and some data consumers may not support the revision number.

 _Caution: Do not use place_id, which is visible in API calls - these are unique to each mirror of the OSM data. You'll be wanting osm_id instead._

### UX Suggestion for Podcast Hosting Platforms

The quality of this data is important to ensure a good listener experience. A podcast publisher should be in no doubt what data is being asked for here, to clarify that this is about a location that is mentioned
in the podcast. The wording of this feature is important to ensure the correct data is available.

![image](https://user-images.githubusercontent.com/231941/103058983-7a857900-45ef-11eb-9b59-7a9aea22288b.png)
![image](https://user-images.githubusercontent.com/231941/103058939-51fd7f00-45ef-11eb-9b0c-0665d7e0aefb.png)

Podcast hosts may also wish to remind podcast publishers to always be cautious about posting public location information. It's possible to check the OSM type to see if it relates to a residential address.

<br>

### Examples

For a podcast that is talking about the Eiffel Tower, but actually made in Birmingham, Alabama, this is what the specification would suggest:

```xml
<podcast:location
  geo="geo:48.858093,2.294694"
  osm="W5013364"
>Eiffel Tower, Paris</podcast:location>
```

For a podcast that is set in Gitmo Nation, a nickname used by the show for the United States of America:

```xml
<podcast:location
  geo="geo:39.7837304,-100.445882;u=3900000"
  osm="R148838"
>Gitmo Nation</podcast:location>
```

The `geo` point uses an optional 'uncertainty' value here of 3,900 km, indicating that the "location" described here is up to 3,900km away from the point given (which is the rough width of the USA). The OSMID
includes a more accurate bounding box and geoJSON.

For a podcast that is about Hogwarts (a fictional location), the `geo` and `osm` must not be entered:

```xml
<podcast:location>Hogwarts</podcast:location>
```

For a podcast from Tesla upon landing on Mars:

```xml
<podcast:location geo="geo:37.786971,-122.399677;crs=Mars-2031">Tesla Base 3</podcast:location>
```

(The coordinate reference system for Mars doesn't yet exist, but this shows the extensibility of this tag.)

<br>

### What This Tag Isn't Built For

For privacy and user experience, this tag is not meant as a description of the physical location of podcast hosts and guests ("I'm doing this podcast in Denver, Colorado!"). The physical location of people are available via the [podcast:person](https://github.com/Podcastindex-org/podcast-namespace#phase-2-open) tag's links to places like Twitter, Facebook, Wikipedia and Podchaser.
