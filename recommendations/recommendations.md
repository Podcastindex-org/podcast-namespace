# The "podcast:recommendations" Specification

<small>Version 1.0 by Benjamin Bellamy - 2021.03.04</small>

<br>

## Purpose

Podcasting is a tremendous ecosystem brimming with tons of stories and ideas that go freely from any platform to any application.  
That comes with a huge drawback: finding and being found can be harsh.  
Podcast creators struggle to be found while podcast listeners struggle to find content.  
Several platforms are now implementing recommendation engines, but these features are expensive and unattainable for small players. Moreover they are slowly creating closed silos and removing power from content creators.  
This specification is about giving control to content creators on the content they want to recommend, and at the same time providing a free recommendation system to all players.  
It was heavily inspired by all the work previously done by the Fellowship of the PodcastIndex on the chapters and soundbite tags. May they be thanked for it.  
GO PODCASTING!!!

## Specification

- **\<podcast:recommendations url="[url to json file]" type="application/json" language="[language code]" />**

    Channel (optional | multiple)

    Item (optional | multiple)

   This element allows a podcaster to specify a list of recommended content for a podcast or an episode.

   The recommended content can be a web page, a podcast, a podcast episode or a soundbite, so that listeners can eventually subscribe to a podcast, add an episode to playlist, add a soundbite to playlist,…

   There may be several occurences of this tag for the same element (one per language, one per topic, one per provider…)

   - `url` (required): This is the url to the json file.
   - `type` (required): Mime type, must be json.
   - `language` (optional): The language of the recommended episodes (two-letter language codes, with some possible modifiers, such as "en-us"). If there is no language attribute given, the linked file is assumed to be the same language that is specified by the RSS \<language> element. 

   Example:`<podcast:recommendations url="https://domain.tld/recommendation?guid=1234" type="application/json" language="en" />`

## "Recommendations" Object

The recommendations object is a simple JSON object with 2 required properties:

 - `version` (required - string) The version number of the format being used.
 - `recommendations` (required - array) An array of recommendations objects defined below.

#### Optional Attributes:

 - `comment` (optional - string) A comment on this file.
 - `title` (optional - string) The name of the source podcast or source podcast episode. Applies to both Channel and Item.
 - `rss` (optional - string) The RSS URL of the source podcast. Applies to both Channel and Item.
 - `guid` (optional - string) The GUID of the source episode. Applies to Item only.
 - `url` (required - string) The enclosure URL of the source episode. Applies to Item only.

## "Recommendation" Objects

The "recommendation" object takes this basic form:

```
{
	"type": "page",
    	"title": "History of podcasting",
	"image": "https://upload.wikimedia.org/wikipedia/commons/thumb/e/e7/Podcasts_%28iOS%29.svg/440px-Podcasts_%28iOS%29.svg.png",
	"url": "https://en.wikipedia.org/wiki/History_of_podcasting"
}
```

There are 4 required attributes:

 - `type` (required - string) The type of this recommended content (*"page"*, *"podcast"*, *"episode"* or *"soundbite"*).
 - `title` (required - string) The title for this recommended content.
 - `image` (required - string) The image URL for this recommended content. Image must have a 1:1 ratio (square).
 - `url` (required - string) The URL for this recommended content. If recommended content type is *"podcast"* this is the home page of the podcast. If recommended content type is *"episode"* or *"soundbite"* this is the enclosure URL. 

#### Optional Attributes:

 - `displayStartTime` (optional - float) The start time (in seconds) that tells when this recommended content should start being displayed. If `displayStartTime` is omitted, recommendation will be displayed from the beginning. Applies only when called from an *Item* (not from the *Channel*).
 - `displayDuration` (optional - float) The duration (in seconds) that tells when this recommended content should stop being displayed. If `displayDuration` is omitted, recommendation will be displayed until the end. Applies only when called from an *Item* (not from the *Channel*).
 - `rss` (optional - string) The RSS URL of this recommended content. Applies to *"podcast"*, *"episode"* and *"soundbite"* types only.
 - `guid` (optional - string) The GUID of this recommended content. Applies to *"episode"* and *"soundbite"* types only.
 - `startTime` (optional - float) The start time (in seconds) of this recommended content. Applies to *"soundbite"* type only.
 - `duration` (optional - float) The duration (in seconds) of this recommended content. Applies to *"soundbite"* type only.
 - `relevance`  (optional - float) The relevance of this recommended content regarding this Channel or Item. Number must be in [0…1]. 0 is for irrelevant content, 1 is for contents that match perfectly.

## Basic example

Here is what a very basic recommendations file may look like:

```
{
	"version": "1.0",
	"recommendations":
	[
		{
			"type": "page",
		    	"title": "History of podcasting",
			"image": "https://upload.wikimedia.org/wikipedia/commons/thumb/e/e7/Podcasts_%28iOS%29.svg/440px-Podcasts_%28iOS%29.svg.png",
			"url": "https://en.wikipedia.org/wiki/History_of_podcasting"
		},
		{
			"type": "podcast",
			"title": "Podcasting 2.0",
			"image": "https://noagendaassets.com/enc/1601061118.678_pciavatar.jpg",
			"url": "https://podcastindex.org/podcast/920666",
			"rss": "http://mp3s.nashownotes.com/pc20rss.xml"
		},
		{
			"type": "episode",
			"title": "Episode 26: Manning Battlestations",
			"image": "https://noagendaassets.com/enc/1601061118.678_pciavatar.jpg",
			"url": "https://mp3s.nashownotes.com/PC20-26-2021-02-26-Final.mp3",
			"rss": "http://mp3s.nashownotes.com/pc20rss.xml",
			"guid": "PC2026"
		},
		{
			"type": "soundbite",
			"title": "GO PODCASTING!!!",
			"image": "https://noagendaassets.com/enc/1601061118.678_pciavatar.jpg",
			"url": "https://mp3s.nashownotes.com/PC20-26-2021-02-26-Final.mp3",
			"rss": "http://mp3s.nashownotes.com/pc20rss.xml",
			"guid": "PC2026",
			"startTime": 4737.0,
			"duration": 5.0
		}
	]
}
```

## More complex example

```
{
	"version": "1.0",
	"title": "Podnews podcasting news",
	"rss": "https://podnews.net/rss",
	"recommendations":
	[
		{
			"displayStartTime": 0.0,
			"displayDuration": 120.0,
			"type": "page",
		    	"title": "History of podcasting",
			"image": "https://en.wikipedia.org/wiki/Podcast#/media/File:Podcasts_(iOS).svg",
			"url": "https://en.wikipedia.org/wiki/History_of_podcasting",
			"relevance": 0.8
		},
		{
			"displayStartTime": 120.50,
			"displayDuration": 60.0,
			"type": "podcast",
			"title": "Podcasting 2.0",
			"image": "https://noagendaassets.com/enc/1601061118.678_pciavatar.jpg",
			"url": "https://podcastindex.org/podcast/920666",
			"rss": "http://mp3s.nashownotes.com/pc20rss.xml",
			"relevance": 0.7
		},
		{
			"displayStartTime": 240.60,
			"displayDuration": 180.0,
			"type": "episode",
			"title": "Episode 26: Manning Battlestations",
			"image": "https://noagendaassets.com/enc/1601061118.678_pciavatar.jpg",
			"url": "https://mp3s.nashownotes.com/PC20-26-2021-02-26-Final.mp3",
			"rss": "http://mp3s.nashownotes.com/pc20rss.xml",
			"guid": "PC2026",
			"relevance": 0.5
		},
		{
			"displayStartTime": 3600.10,
			"displayDuration": 60.0,
			"type": "soundbite",
			"title": "GO PODCASTING!!!",
			"image": "https://noagendaassets.com/enc/1601061118.678_pciavatar.jpg",
			"url": "https://mp3s.nashownotes.com/PC20-26-2021-02-26-Final.mp3",
			"rss": "http://mp3s.nashownotes.com/pc20rss.xml",
			"guid": "PC2026",
			"startTime": 4737.0,
			"duration": 5.0,
			"relevance": 0.9
		}
	]
}
```

## Note about privacy

When pulling in web based data there is the chance that this functionality could be used for tracking.  
As a safeguard against that, apps should:

- Block all cookies.
- Allow users to ignore `displayStartTime` and `displayDuration` if they want to (which would display all recommendations at the same time, from beginning to end).
