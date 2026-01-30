# The podcast:chapters API Specification

<small>Version 1.0 by Benjamin Bellamy - 2021.03.11</small>

<br>
This version is a first draft. It will be updated. It may move somewhere else.

## Purpose
The PodcastIndex namespace allows all podcast platforms (hosting, index, players…) to speak the same language and interact together.  
These interactions are limited to communications through the RSS feeds.  
This document describes a new type of interaction between actors who use a same PodcastIndex namespace tag: [Podcast:Chapters](/docs/examples/chapters/jsonChapters.md).  
It was initiated by [David Norman](https://podcastindex.social/@hypercatcher) for [Hypercatcher](https://hypercatcher.com/) and [Benjamin Bellamy](https://podcastindex.social/@benjaminbellamy) for [Castopod](https://castopod.org/) so that HyperCatcher and Castopod are able to interact together and create a seamless experience for podcasters.

We hope this will open a path to more collaborations between platforms which use the PodcastIndex namespace.  
(The podcast:transcript API is probably the next to be specified…)

Note that the purpose of this specification is **not** to define “**how**” to manage the chapter service but “**who**” manages the chapter service, so that **any** podcaster is able to choose **any** provider among the ones able to provide the chapter service.  
Then that provider is free to use IPFS, centralised https or whatever makes sense to him — and to his users.
This spec is **not** an extension of the [Podcast:Chapters](/docs/examples/chapters/jsonChapters.md) tag, it sits next to it.

## Example
To help making things clear, let's take an example:
A podcaster, we'll call her Eve, is hosting a podcast on Castopod. She is using HyperCatcher to manage the chapters.

### Without the podcast:chapters API
Whenever Eve wants to publish a new episode, she has to:
- Go to her Castopod admin panel, save the episode, then publish it so that it is visible in the RSS feed.
- Go to her HyperCatcher dashboard, click "Edit", click "Fetch Episodes" so that HyperCatcher gets this new episode.
- Click on the newly fetched episode, copy the "Url for Json".
- Go back to Castopod admin panel, edit the episode, paste the previously copied "Url for Json" into the appropriate field, hoping that no platform had fetched the RSS in the meantime, before the chapter Url was pasted.

Later, Eve will wait for users to edit chapters, she will go back to HyperCatcher dashboard, Accept (or not) the community chapters.

### With the podcast:chapters API
Whenever Eve wants to publish a new episode, she has to:
- Go to her Castopod admin panel, save the episode. (Castopod will automatically call the podcast:chapters API at Hypercatcher, send the new episode and get from HyperCatcher the public "URL for Json" and insert it automatically in the RSS feed, get the private URL to the episode in the HyperCatcher dashboard and display the link on Castopod Dashboard.)
- That's it.

Later, Eve will wait for users to edit chapters, she will go back to HyperCatcher dashboard, Accept (or not) the community chapters.  
Because Castopod knows the private URL to the episode in the HyperCatcher dashboard, a link from the episode edit page in Castopod to the episode in HyperCatcher will be displayed. And we think that this is pretty cool.

## Technical Specification
This API uses REST.

It involves 2 parties:
- a Poscast Hosting service.
- a Chapters Provider service.

### Endpoints
This API has only one endpoint (so far), hosted at the Chapters Provider.

#### AddNewEpisode
- `POST /AddNewEpisode`
This adds a new Episode to the chapter provider.  
The endpoint URL may be defined by the chapter provider. The Podcast Hosting service must provide a way to specify this endpoint URL on its configuration panel.  

Parameters:
- `rss`: RSS feed URL
- `guid`: New Episode GUID
- `enclosure_url`: New episode enclosure url

Response:
- `status`: true or false
- `jsonUrl`: Url for Json
- `episodeUrl`: Url for episode on dashboard

### Authentication
Thir API will use the mechanism already used by the [PodcastIndex.org API](https://podcastindex-org.github.io/docs-api/#auth).  
The Chapters Provider will provide a couple `apiKey` and `apiSecret` which will be displayed on the user dashboard so that the podcaster can copy and paste them on his Podcast Hosting configuration panel.  
Fields:
- `User-Agent`: Mandatory  
Please identify the system/product you are using to make this request.  
Example: Castopod/1.0
- `X-Auth-Key`: Mandatory  
Your API key string  
Example: UXKCGDSYGUUEVQJSYDZH
- `X-Auth-Date`: Mandatory  
The current unix epoch time as a string. 5 minute window.  
This value is an integer; round down if needed. The value shall not include a decimal point.  
Example: 1613713388
- `Authorization`: Mandatory  
A SHA-1 hash of the X-Auth-Key, the corresponding secret and the X-Auth-Date value concatenated as a string. The resulting hash should be encoded as a hexadecimal value, two digits per byte, using lower case letters for the hex digits "a" through "f".  
Example: UXKCGDSYGUUEVQJSYDZH  
The Authorization header is computed with something like this (pseudo-code):
```
authHeader = sha1(apiKey+apiSecret+unixTime)
```

Discussion here:
- https://github.com/Podcastindex-org/podcast-namespace/issues/209
- https://podcastindex.social/web/statuses/105872943999299482
