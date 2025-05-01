## Location tag format details and implementation guide

_Revised in 2025_

Below, you will find implementation details and UX recommendations for the `<podcast:location>` tag.

### Overview

This tag is intended to describe the location of either:  
a) the editorial focus for a podcast's content - i.e. "what place is this podcast about?"  
b) the place of production for a podcast's content - i.e. "where was this podcast made?"

It can exist in the `channel` level, the `item` level or the `liveItem` level (i.e. it can exist to give a location for a whole podcast, or for one specific episode).

If a location exists in the `channel` level, a location of the same type (creator or subject) in the `item` or `liveItem` level may overwrite it for specific items. (i.e. "this podcast is normally recorded in Austin TX, but this episode was recorded in Birmingham AL").

### Use cases

The use-cases for this tag are multiple, in order of complexity:

1. To allow a free-text "location" field to be visible in a podcast app, perhaps during playback.
2. To allow a podcast app to aggregate shows "about" or "from" a country.
3. To allow a simple point on a map to be visible in a podcast app.
4. To allow a search for "podcasts/episodes about places near me" - or, for example, a travel or news podcast.
5. To describe a specific place in a programmatic fashion to allow complex geo-aware searches.

It may allow complex searches such as:

- Show me podcasts or episodes about places near me.
- Show me podcasts about train stations in Germany.
- Show me podcasts about mines in West Australia.
- Show me podcasts recorded in my state.
- Visa mig podcaster om platser i Kalifornien på svenska - "Show me podcasts about places in California, returning Swedish-language podcasts only" (using the `<language>` tag).

Unlike other elements in the `podcast` namespace, a "place" is not permanent. Places are built, and abandoned, all the time. Buildings are demolished, businesses close. On the other hand, a point on the earth is permanent, but does not describe anything other than a point. This is not always great when wanting to describe a city, rather than an area within a city, or a restaurant within a city.

The `<podcast:location>` tag is therefore complex and has a number of attributes.

An [example location generator](https://jamescridland.github.io/podcast-location-generator/) is available. It's entirely written in JavaScript and uses open APIs. You are welcome to use the code in yur project.

### Structure

```xml
<podcast:location
  rel="subject|creator"
  geo="[geo URI]"
  osm="[OSM type (as initial)][OSM id](#OSM revision)"
  country="[ISO 3166-1 alpha-2 country code]"
>[human-readable place name]</podcast:location>
```

#### Tag Node Value **required**

This is meant for podcast apps to display the name of the location that the podcast is about. Examples might be "Houses of Parliament", "Gitmo Nation" or "Ernest Murrow Theater, Chicago". This is not intended to be programmatically parsed and is for display only. For a programmatic designation of the location, use the geo URI and OSM IDs, below.

This value is a maximum of 128 characters. It may describe a real or fictional place. It should be in the same language as the podcast, as indicated in the <language> RSS tag: so a podcast in `en` should read "Eiffel Tower, Paris" and not "La Tour d'Eiffel".

### Attributes

All attributes are **recommended** to ensure a low barrier to entry; but the location tag works best when they are all used. Particularly, the OSM ID allows rich querying of podcast data, which could result in significantly enhanced podcast discovery.

#### `rel` **recommended**

The `rel` attribute can contain one of the following possible values:
   - `"subject"` (default) - The location refers to what/where the content is about.
   - `"creator"` - The location refers to where the content was recorded or produced.

The specification defaults to "subject" for legacy reasons, but we would suggest that your code defaults to "creator" (i.e. where the podcaster is recording this show), since this is anticipated to be the main use of this feature.

#### `geo` **recommended**

A geo URI, conformant to [RFC 5870](https://tools.ietf.org/html/rfc5870).

Examples:

 - `geo:37.786971,-122.399677`, a simple latlon description.
 - `geo:37.786971,-122.399677,250`, a latlon including a height of 250 meters above ground level.
 - `geo:37.786971,-122.399677;u=350`, a latlon with an accuracy ('uncertainty') of 350 meters.

For information that may interest space travelers: the RFC does include an optional coordinate reference system for other planets, though these are not recommended to be used yet by the RFC. We use a geo URI here to allow for this.

The `geo` attribute is recommended to be used alongside an `osm` attribute. Since OSM IDs are not guaranteed to be permanent (perhaps it's the ID of a building which is later demolished), the geo URI serves as a permanent point.

Data within these attributes must relate to a real place. This tag must not be used for podcasts from or about fictional places. 

>[!TIP]
>**This only describes a point on the ground.** It can be used to place pins on a dynamically generated map; however, you may wish to consider how you highlight a podcast about Notre Dame Cathedral, vs a podcast about all of Paris. The official geo URIs for each are almost the same lat/lon on the ground.
>
>**Be careful of user privacy.** geo URIs can be very detailed; but a podcast creator probably doesn't wish to make their home address visible. It's therefore expected that a podcast creator who lives in Barcelona would type "Barcelona" for the location tag, or a suburb of the city, and not a home address. You may wish to facilitate this in your UX.

#### `osm` **recommended**

A link to an object in [OpenStreetMap](https://www.openstreetmap.org). OpenStreetMap is a free, user-edited map, used by many companies including Amazon and Apple. You can download full data dumps of OpenStreetMap for your own use, or use available APIs to access the data. APIs include [Nominatim](https://nominatim.org/release-docs/develop/api/Overview/), which is free (but check the terms of use); and paid-for versions like [LocationIQ](https://locationiq.com).

If a value is given for `osm` it must contain both 'type' and 'id'.

 - osm type: A one-character description of the type of OSM point. Valid is "N" (node); "W" (way); "R" (relation).
 - osm id: The ID of the OpenStreetMap feature that is described.
 - osm revision: an _optional_ revision ID for an OSM object, preceded by a hash. (It's expected that you're unlikely to use this).

This may describe part of a building, a building or business, a suburb, city, state, or country - anything within the OSM database, using the OpenStreetMap API or a local copy of the data. This is the field that is the best programmatic representation of the place being described. The data within OpenStreetMap is rich and can be used for detailed searches.

Examples:

 - The United States of America: [R148838](https://nominatim.openstreetmap.org/ui/details.html?osmtype=R&osmid=148838)
 - The Eiffel Tower in Paris: [W5013364](https://nominatim.openstreetmap.org/ui/details.html?osmtype=W&osmid=5013364)
 - Paris, but - optionally - the revision made on 8 Jan 2021: [R7444#188](https://www.openstreetmap.org/relation/7444/history)

The `osm` is recommended to be used with a `geo` attribute. Since OSM IDs are not guaranteed to be permanent (perhaps it's the ID of a building which is later demolished), the geo URI serves as a permanent point. Data within these tags must relate to a real place.

If a developer uses the `osm` tag, **the canonical latlon is the one returned by OSM**. It is intended that the `geo` attribute is used for simple display within a podcast app without any API usage: but for more advanced uses, like a geographic search, developers will ingest the full details from OpenStreetMap. The geo URI also offers a useful fallback should the `osmid` be removed.

The revision number for the edit is a special case that allows for more permanent links, with the drawback that these are not fully supported by Nominatim and require a full OSM database complete with changesets. It is recommended to simply link to the object in OSM normally, without a revision number, and some data consumers may not support the revision number.

>[!CAUTION]
>Do not use place_id, which is visible in API calls - these are unique to each mirror of the OSM data. You'll be wanting osm_id instead.

>[!TIP]
>**Be careful of user privacy.** A podcast creator probably doesn't wish to make their home address visible. It's therefore expected that a podcast creator who lives in Barcelona would type "Barcelona" for the location tag, or a suburb of the city, and not a home address. You can request that geolocation APIs only return data types like suburbs or cities.
>
>**Don't do this automatically from a lat/lon** - you have no idea whether the user intends the closest object from a lat/lon pair to be the element they want. Without accurate data, you might be linking to [Piccadilly Circus](https://www.openstreetmap.org/relation/8250949), a tourist attraction; [Piccadilly Circus](https://www.openstreetmap.org/node/3638779316#map=16/51.50982/-0.13454), a railway station, [Piccadilly Circus](https://www.openstreetmap.org/way/4376571#map=19/51.509957/-0.134754), a road, [Piccadilly Lights](https://www.openstreetmap.org/way/289643428), an advertising hoarding pretending it's a tourist attraction, [the fountain in Piccadilly Circus](https://www.openstreetmap.org/way/583928480), a [kiosk on Piccadilly Circus](https://www.openstreetmap.org/way/1197088737) full of overpriced tourist tat, or a [closed-circuit camera overlooking Piccadilly Circus](https://www.openstreetmap.org/node/2931935294) to help the rozzers fight crime.

#### `country` **recommended**

A two-letter code for the country, following [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)

Use of either the geo URI or the OSM ID requires an API call at some point to resolve it to an object containing information about that object. That allows for rich podcast discovery by location; but has a drawback that it may be computationally difficult for some podcast apps.

This simple value allows podcast apps to display a flag in the UX to show where a podcast is about, or where it was recorded, without an API call.

>[!TIP]
>The ISO 3166-1 standard for the United Kingdom is `GB` and not what you thought it was.

### UX Suggestion for Podcast Hosting Platforms

The quality of this data is important to ensure a good listener experience.

A podcast publisher should be in no doubt what data is being asked for here - and for it to be clear what it is intended to describe - a "place of recording" or a "subject of a podcast". It is important to ensure that this data is not junk.

As mentioned above, podcast hosts may also wish to remind podcast publishers to always be cautious about posting public location information.

### Examples

There is [simple generator code here](https://jamescridland.github.io/podcast-location-generator/) which uses JavaScript and may be used by your own projects.

#### A podcast made in Austin TX in the US:
```xml
<podcast:location
 rel="subject"
 geo="geo:30.2711286,-97.7436995"
 osm="R113314"
 country="US"
>Austin</podcast:location>
```

#### A podcast made in Marlow, England, about Dreamworld, a themepark in Australia

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

#### A podcast recorded in Gitmo Nation:

Gitmo Nation is a nickname used by the show for the United States of America. So we link to the US in the data, but call it Gitmo Nation in the node value.

```xml
<podcast:location
  rel="creator"
  geo="geo:39.7837304,-100.445882;u=3900000"
  osm="R148838"
  country="US"
>Gitmo Nation</podcast:location>
```

The `geo` point uses an optional 'uncertainty' value here of 3,900 km, indicating that the "location" described here is up to 3,900km away from the point given (which is the rough width of the USA). The OSMID includes a more accurate bounding box and geoJSON.

#### A podcast that is about Hogwarts (a fictional location):

Since this is not a real location, the `geo` and `osm` must not be entered. But it can still correctly say it's about Hogwarts.

```xml
<podcast:location
  rel="subject"
>Hogwarts</podcast:location>
```

### Why use OpenStreetMap?

Here's a typical tag for a podcast that is about the Eiffel Tower in France.

```
<podcast:location
 rel="subject"
 geo="geo:48.8529371,2.3500501"
 osm="W201611261"
 country="FR"
>
Cathedral of Notre Dame
</podcast:location>
```

While the osm element is written in the specification as recommended but optional, you *lose a lot of the functionality* of this tag if you omit it, so it's, as it says, recommended.

OpenStreetMap is an open, free map that everyone can use. (You can download it if you want a copy, too). It lists a great deal of detail (and you can quickly add things it doesn't list, too). In the example above, it is set to "W" (for 'way', one of the three OpenStreetMap object types), and then the ID of 201611261. [It links here](https://www.openstreetmap.org/way/201611261#map=19/48.852937/2.349870) - it's Notre Dame Cathedral in Paris.

Now, check this one:

```
<podcast:location
 rel="subject"
 geo="geo:48.8588897,2.3200410"
 osm="R7444"
 country="FR"
>
Paris
</podcast:location>
```

This has a really similar lat/lon - it's within 100 meters or so of the other point. You might be forgiven for thinking that it's the same place. But it isn't. It's a podcast [about the city of Paris](https://www.openstreetmap.org/relation/7444#map=12/48.8341/2.3512) - the OSM "Relation" object, ID 7444.

So, one is about "Paris", while one is about a big church. And, while the lat/lon is almost identical, these two are clearly not covering the same place. It could be wrong to put these two as pins on a map, for example.

### Overpass and new ways of finding podcasts

Let's just go back to [Notre Dame Cathedral](https://www.openstreetmap.org/way/201611261#map=19/48.853274/2.348932). Take a look at the data on the left-hand side of OpenStreetMap's page. It's got lots of information about it - it's a cathedral, and it's built in the gothic style.

So, let's consider how you'd make an app that automatically grabs all podcast episodes about cathedrals in France, as one example.

[This link](https://overpass-turbo.eu/s/22Jd) searches OpenStreetMap for all cathedrals in France. On the right is a map - above it, you can switch it to "data", where you'll see all the data about individual cathedrals in France, including [R1373890](https://www.openstreetmap.org/relation/1373890#map=13/49.42320/1.12893), which is a cathedral in Rouen. **Is there a podcast about R1373890? That's a simple database lookup.**

Oh, and remember that Notre Dame was in the gothic style? Let's find [all cathedrals in France in a gothic style](https://overpass-turbo.eu/s/22Jf).

OpenStreetMap is very powerful - you can get it to return, for example, "[all wineries in Adelaide](https://overpass-turbo.eu/s/22Ji)", and very quickly build a wine website covering the area - with links to podcasts about them, where it makes sense. (And, by the way, most AI tools will accept a prompt like "Write a query for Overpass showing all wineries in Adelaide").

Know that this information is there for every object in OpenStreetMap - but also know that *you can't do this with a lat/lon*. 

**You might not build this into a podcast app.** It might be on a gothic cathedral fan website. It might be a website about wine, or breweries, or railway stations. But the `podcast:location` tag offers really powerful, exciting ways into podcasts that we've simply never had before. If - and only if - we use the OSM IDs.

### Feature champion

If you have queries or questions about implementation, feel free to email the feature champion of this tag, James Cridland, at james@crid.land

