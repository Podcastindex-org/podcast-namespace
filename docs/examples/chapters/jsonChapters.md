## JSON Chapters Format
<small>Version 1.2 - Updated on 2021.04.15</small>

<br><br>

This is the initial spec for a json chapters format that can be referenced in an RSS feed using the `<podcast:chapters>` tag of
the "podcast" namespace.  This file can reside on any publicly accessible url.  See the podcast namespace documentation for
details on the format of the tag.

This type of file should be served with a Content-type of 'application/json+chapters'.  Chapter order is assumed to be
in ascending order based on the `startTime`.

<br>

## "Chapters" Object

The chapters object is a simple JSON object with only 2 required properties:

 - `version` (required - string) The version number of the format being used
 - `chapters` (required - array) An array of chapter objects defined below

#### Optional Attributes:

 - `author` (optional - string) The name of the author of this podcast episode.
 - `title` (optional - string) The title of this podcast episode.
 - `podcastName` (optional - string) The name of the podcast this episode belongs to.
 - `description` (optional - string) A description of this episode.
 - `fileName` (optional - string) The name of the audio file these chapters apply to.
 - `waypoints` (optional - boolean) If this property is present, the locations in a chapter object should be displayed with a route between locations.

<br>

## The "Chapter" Object

The "chapter" object takes this basic form:

```json
{
    "startTime": 94,
    "title": "Donation Segment"
}
```

There is only one required attribute:

 - `startTime` (required - float) The starting time of the chapter, expressed in seconds with float precision for fractions of a second.

<br>

#### Optional Attributes:

 - `title` (optional - string) The title of this chapter.
 - `img` (optional - string) The url of an image to use as chapter art.
 - `url` (optional - string) The url of a web page or supporting document that's related to the topic of this chapter.
 - `toc` (optional - boolean) If this property is present and set to false, this chapter should not display visibly to the user in either the table of contents or as a jump-to point in the user interface.  It should be considered a "silent" chapter marker for the purpose of meta-data only.  If this property is set to `true` or not present at all, this should be considered a normal chapter for display to the user.  The name "toc" is short for "table of contents".
 - `endTime` (optional - float) The end time of the chapter, expressed in seconds with float precision for fractions of a second.
 - `location` (optional - object) This object defines an optional location that is tied to this chapter.  It follows the structure of the [location](https://github.com/Podcastindex-org/podcast-namespace/blob/main/location/location.md) tag in the XML namespace.

<br>

## The Location Object:

The "location" object takes this basic form:

```json
{
    "name": "Eiffel Tower, Paris",
    "geo": "geo:48.858093,2.294694"
}
```

It is intended to provide for rich location-based experiences tied to a point of time within a podcast episode, or other feed based media.  For example, a "walking tour" may include latitude and longitude waypoints along side the image within chapters markers as someone listens to the tour podcast.  This
would allow apps to show a map with markers within the UI as the tour progresses.  Or, perhaps a "History of the Middle East" podcast might expose a map to highlight where certain landmarks are when being discussed.

There are two required attributes:

 - `name` (required - string) A human readable place name.
 - `geo` (required - string) A simple latitude,longitude given in geoURI format, conformant to [RFC 5870](https://tools.ietf.org/html/rfc5870).

#### Optional Attributes:

 - `osm` (optional - string) An OpenStreetMaps query string.  Please see the [location](https://github.com/Podcastindex-org/podcast-namespace/blob/main/location/location.md) XML tag specification for full details.

<br>

## Basic example

Here is what a very basic chapters file may look like:

```json
{
    "version": "1.2.0",
    "chapters":
    [
        {
            "startTime": 0,
            "title": "Intro"
        },
        {
            "startTime": 168,
            "title": "Hearing Aids",
            "img": "https://example.com/images/hearing_aids.jpg"
        },
        {
            "startTime": 260,
            "title": "Progress Report"
        },
        {
            "startTime": 410,
            "title": "Namespace",
            "img": "https://example.com/images/namepsace_example.jpg",
            "url": "https://github.com/Podcastindex-org/podcast-namespace"
        },
        {
            "startTime": 3990,
            "title": "Just Break Up",
            "img": "https://example.com/images/justbreakuppod.png"
        },
        {
            "startTime": 5510,
            "title": "The Big Players"
        },
        {
            "startTime": 5854,
            "title": "Spread the Word"
        },
        {
            "startTime": 6089,
            "title": "Outro"
        }
    ]
}
```

In this simple form, the chapter objects are treated as sequential just based on their index as a function of being
ordered in ascending fashion based on their `startTime`.  In a very simple example such as this nothing is present except the `startTime`,
`title`, and a few bits of optional meta-data.

<br><br>

## More complex example

In this more robust example, we can bring in more meta-data about the podcast episode, and provide a way for this file to exist more easily in a web
context for something like an embedded HTML5 player on a website.  Also there is an example of a "silent" chapter that has no presence in the visible
chapter list, but allows for different artwork to be shown:

```json
{
    "version": "1.2.0",
    "author": "John Doe",
    "title": "Episode 7 - Making Progress",
    "podcastName": "John's Awesome Podcast",
    "chapters":
    [
        {
            "startTime": 0,
            "title": "Intro"
        },
        {
            "startTime": 168,
            "title": "Hearing Aids"
        },
        {
            "startTime": 260,
            "title": "Progress Report"
        },
        {
            "startTime": 410,
            "title": "Namespace",
            "img": "https://example.com/images/namepsace_example.jpg",
            "url": "https://github.com/Podcastindex-org/podcast-namespace"
        },
        {
            "startTime": 3990,
            "title": "Just Break Up",
            "img": "https://example.com/images/justbreakuppod.png",
            "url": "https://twitter.com/justbreakuppod"
        },
        {
            "startTime": 4826,
            "img": "https://example.com/images/parisfrance.jpg",
            "toc": false,
            "location": {
                "name": "Eiffel Tower, Paris",
                "geo": "geo:42.3417649,-70.9661596"
            }
        },
        {
            "startTime": 5510,
            "title": "The Big Players"
        },
        {
            "startTime": 5854,
            "title": "Spread the Word"
        },
        {
            "startTime": 6089,
            "title": "Outro"
        }
    ]
}
```

<br><br>

## Note about privacy

Clearly, when pulling in web based data there is the chance that this functionality could be used for tracking.  As a safeguard against that, apps
using chapters are encouraged to prompt the user to acknowledge whether they trust the podcast in question before displaying the content.  Trusted
podcasts can then be remembered for the future.
