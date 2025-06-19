# The "podcast:recommendations" Specification

<small>Version 1.0 by Benjamin Bellamy - 2021.08.31</small>

<br>

## Purpose

Podcasting is a tremendous ecosystem brimming with tons of stories and ideas that go freely from any platform to any application. That comes
with a huge drawback: finding and being found can be harsh. Podcast creators struggle to be found while podcast listeners struggle to find content.

Several platforms are now implementing recommendation engines, but these features are expensive and unattainable for small players. Moreover they
are slowly creating closed silos and removing power from content creators. This specification is about giving control to content creators on the
content they want to recommend, and at the same time providing a free recommendation system to all players.

<br><br>

## Specification

```xml
<podcast:recommendations
    url="[url to json file(string)]"
    type="application/json"
    language="[language code(string)]"
/>
[Optionnal comments(string)]
</podcast:recommendations>
```

Channel (optional | multiple)

Item (optional | multiple)

This element allows a podcaster to specify a list of recommended content for a podcast or an episode.

The recommended content can be a web page, a podcast, a podcast episode or a soundbite, so that listeners can eventually subscribe to a podcast, add
an episode to playlist, add a soundbite to playlist, etc.

There may be several occurences of this tag for the same element (one per language, one per topic, one per provider).

#### Attributes

- `url` (required): This is the url to the json file.
- `type` (required): Mime type, must be json.
- `language` (optional): The language of the recommended episodes (two-letter language codes, with some possible modifiers, such as "en-us"). If there
  is no language attribute given, the linked file is assumed to be the same language that is specified by the RSS `<language>` element.

#### Examples

- `<podcast:recommendations url="https://domain.tld/recommendation?guid=1234" type="application/json" />`
- `<podcast:recommendations url="https://domain.tld/recommendation?guid=1234" type="application/json" language="en" />`

<br><br>

## JSON File Specification

The recommendations file contains a simple JSON object with 2 required properties and a few optional properties. This top-level object
is simply a container for the main payload, which is an array of `recommendation` objects contained in the `recommendations` property.

#### Required Attributes

- `version` (required - string) The version number of the format being used.
- `recommendations` (required - array) An array of `recommendations` objects defined below.

#### Optional Attributes

- `comment` (optional - string) A comment on this file.
- `title` (optional - string) The name of the source podcast or **source** podcast episode. Applies to both Channel and Item.
- `feed` (optional - string) The RSS URL of the **source** podcast. Applies to both podcast Channel and podcast Item.
- `guid` (optional - string) The GUID of the **source** element. Applies to both podcast Channel and podcast Item.
- `url` (optionnal - string) The enclosure URL of the **source** medium.

#### Example

```json
{
	"version": "1.0",
	"title": "Podnews podcasting news",
	"feed": "https://podnews.net/rss",
	"guid": "9b024349-ccf0-5f69-a609-6b82873eab3c",
	"recommendations": [
	    ...
	]
}
```

<br><br>

## The "recommendations" Array

Each recommendation is defined in a `recommendation` object that resides within the `recommendations` array. It is meant to structure data
that might otherwise be present - buried within shownotes HTML.

#### Required attributes:

- `linkType` (required - string) The link type of this recommended content, it can be:
  - 'generic',
  - 'feed',
  - 'item',
  - 'none'
- `medium` (required - string) The [medium](/docs/tags/medium.md) type. It can be:
  - `podcast`,
  - `music`,
  - `video`,
  - `film`,
  - `audiobook`,
  - `newsletter`,
  - `blog`
- `title` (required - string) The title for this recommended content.

#### Optional Attributes:

- `url` (optional - string) The URL for this recommended content. If recommended content type is _"feed"_ this is the home page of the podcast/medium.
  If recommended content type is _"feed-item"_ this is the enclosure URL.
- `image` (optional - string) The image URL for this recommended content. Image must have a 1:1 ratio (square).
- `displayStartTime` (optional - float) The start time (in seconds) that tells when this recommended content should start being displayed. If `displayStartTime`
  is omitted, recommendation will be displayed from the beginning. Applies only when called from an _Item_ (not from the _Channel_).
- `displayDuration` (optional - float) The duration (in seconds) that tells when this recommended content should stop being displayed. If `displayDuration` is
  omitted, recommendation will be displayed until the end. Applies only when called from an _Item_ (not from the _Channel_).
- `feed` (optional - string) The RSS URL of this recommended content. Applies to _"feed"_, _"feed-item"_ types.
- `guid` (optional - string) The GUID of this recommended content. Applies to _"feed"_ and _"feed-item"_ types.
- `startTime` (optional - float) The start time (in seconds) of this recommended content. Applies to _"feed-item"_ type only.
- `duration` (optional - float) The duration (in seconds) of this recommended content. Applies to _"feed-item"_ type only.
- `motive` (optional - string) The reason why this content is recommended. It can be:
  - `references` (content that was used when creating this podcast, similar to the Wikipedia References paragraph),
  - `additional content` (content that provides extra information),
  - `acknowledgment` (thanking people),
  - `advertising` (sponsored content),
  - `audience exchange` (exchanging audiences between podcasts),
  - `content-based recommendation` (content related thanks to semantic indexation),
  - `audience-based recommendation` (people who liked this also liked…),
  - `made by the same people` (the creators of this podcast also made that…)
- `relevance` (optional - float) The relevance of this recommended content regarding this Channel or Item. Number must be in [0…1]. 0 is for irrelevant content, 1 is
  for contents that match perfectly.

#### Example of Structure

```json
{
  "linkType": "generic",
  "title": "History of podcasting",
  "image": "https://upload.wikimedia.org/wikipedia/commons/thumb/e/e7/Podcasts_%28iOS%29.svg/440px-Podcasts_%28iOS%29.svg.png",
  "url": "https://en.wikipedia.org/wiki/History_of_podcasting"
}
```

#### Basic example

Here is what a very basic recommendations file may look like:

```json
{
  "version": "1.0",
  "recommendations": [
    {
      "linkType": "none",
      "medium": "text",
      "title": "Eat five vegetables every day.",
      "motive": "advertising"
    },
    {
      "linkType": "generic",
      "medium": "html",
      "title": "History of podcasting",
      "image": "https://upload.wikimedia.org/wikipedia/commons/thumb/e/e7/Podcasts_%28iOS%29.svg/440px-Podcasts_%28iOS%29.svg.png",
      "url": "https://en.wikipedia.org/wiki/History_of_podcasting",
      "motive": "additional content"
    },
    {
      "linkType": "feed",
      "medium": "podcast",
      "title": "Podcasting 2.0",
      "image": "https://noagendaassets.com/enc/1601061118.678_pciavatar.jpg",
      "url": "https://podcastindex.org/podcast/920666",
      "feed": "http://mp3s.nashownotes.com/pc20rss.xml",
      "motive": "audience exchange"
    },
    {
      "linkType": "item",
      "medium": "podcast",
      "title": "Episode 26: Manning Battlestations",
      "image": "https://noagendaassets.com/enc/1601061118.678_pciavatar.jpg",
      "url": "https://mp3s.nashownotes.com/PC20-26-2021-02-26-Final.mp3",
      "feed": "http://mp3s.nashownotes.com/pc20rss.xml",
      "guid": "PC2026"
    },
    {
      "linkType": "item",
      "medium": "podcast",
      "title": "GO PODCASTING!!!",
      "image": "https://noagendaassets.com/enc/1601061118.678_pciavatar.jpg",
      "url": "https://mp3s.nashownotes.com/PC20-26-2021-02-26-Final.mp3",
      "feed": "http://mp3s.nashownotes.com/pc20rss.xml",
      "guid": "PC2026",
      "startTime": 4737.0,
      "duration": 5.0
    }
  ]
}
```

## More complex example

```json
{
  "version": "1.0",
  "title": "Podnews podcasting news",
  "feed": "https://podnews.net/rss",
  "guid": "9b024349-ccf0-5f69-a609-6b82873eab3c",
  "recommendations": [
    {
      "linkType": "none",
      "medium": "text",
      "title": "Eat five vegetables every day.",
      "motive": "advertising"
    },
    {
      "displayStartTime": 0.0,
      "displayDuration": 120.0,
      "linkType": "generic",
      "medium": "html",
      "title": "History of podcasting",
      "image": "https://upload.wikimedia.org/wikipedia/commons/thumb/e/e7/Podcasts_%28iOS%29.svg/440px-Podcasts_%28iOS%29.svg.png",
      "url": "https://en.wikipedia.org/wiki/History_of_podcasting",
      "motive": "additional content",
      "relevance": 0.8
    },
    {
      "displayStartTime": 120.5,
      "displayDuration": 60.0,
      "linkType": "feed",
      "medium": "podcast",
      "title": "Podcasting 2.0",
      "image": "https://noagendaassets.com/enc/1601061118.678_pciavatar.jpg",
      "url": "https://podcastindex.org/podcast/920666",
      "feed": "http://mp3s.nashownotes.com/pc20rss.xml",
      "motive": "audience exchange",
      "relevance": 0.7
    },
    {
      "displayStartTime": 240.6,
      "displayDuration": 180.0,
      "linkType": "feed-item",
      "medium": "podcast",
      "title": "Episode 26: Manning Battlestations",
      "image": "https://noagendaassets.com/enc/1601061118.678_pciavatar.jpg",
      "url": "https://mp3s.nashownotes.com/PC20-26-2021-02-26-Final.mp3",
      "feed": "http://mp3s.nashownotes.com/pc20rss.xml",
      "guid": "PC2026",
      "relevance": 0.5
    },
    {
      "displayStartTime": 3600.1,
      "displayDuration": 60.0,
      "linkType": "item",
      "medium": "podcast",
      "title": "GO PODCASTING!!!",
      "image": "https://noagendaassets.com/enc/1601061118.678_pciavatar.jpg",
      "url": "https://mp3s.nashownotes.com/PC20-26-2021-02-26-Final.mp3",
      "feed": "http://mp3s.nashownotes.com/pc20rss.xml",
      "guid": "PC2026",
      "startTime": 4737.0,
      "duration": 5.0,
      "motive": "additional content",
      "relevance": 0.9
    }
  ]
}
```

## Privacy

When pulling in web based data there is the chance that this functionality could be used for tracking.
As a safeguard against that, apps should:

- Block all cookies.
- Allow users to ignore `displayStartTime` and `displayDuration` if they want to.
- Fetch all recommendations at the same time disregarding `displayStartTime` so that HTTP requests cannot be used as a way of measuring who listens to what.

Discussion here:

- https://github.com/Podcastindex-org/podcast-namespace/issues/205
- https://podcastindex.social/web/statuses/105833620038854052
