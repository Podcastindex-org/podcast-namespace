# The "podcast" Namespace

A wholistic rss namespace for podcasting that is meant to synthesize the fragmented world of podcast namespaces.  The broad goal is to create a single, compact, efficient
namespace that is easily extensible, community controlled/authored and addresses the needs of the independent podcast industry now and in the future.
Our hope is that this namespace
will become the framework that the independent podcast community needs to deliver new functionality to apps and aggregators.


<br><br>


## Current Roadmap

**Phase 1** - [Closed] Comment period closed on `11/15/20` and 5 tags were **formalized**.

**Phase 2** - [Closed] Comment period closed on `1/31/21` and 4 tags were **formalized**.

**Phase 3** - [Open] Proposals welcome.  This phase will close on June 1st, 2021.  At that time, any tags with full agreement will be reviewed for
                     formalization.  Any tags with concerns or questions will be pushed forward to the next phase.  Current tag proposals under
                     consideration are listed [here](#phase-3-open).

<br><br>


## Legend

**Formalized** - This tag is frozen and listed in the XMLNS document.  Any future changes to it's definition must maintain backwards compatibility.

**Finalized** - The tag is structurally stable and implementation testing should be considered safe.  Any breaking changes will be widely communicated.

**Open** - The tag/phase is open for discussion and collaboration.

**Required** - This tag or attribute must be present.

**Optional** - This tag or attribute may be left out.

**Recommended** - This tag or attribute is technically optional, but is strongly recommended to be present for the tag to function as fully intended.


<br><br>


## Tag Adoption Process

To be adopted as an official part of the namespace, there must be consensus around a tag's usefulness and either commitment to adoption by at least 1 host and 1 app, or a recognition
that the tag is already being used in the wild.

It is ALWAYS ok to delay a tag to a future Phase if there is any concern about it.  That is to be expected and encouraged.

When a Phase comes to a close, there will be a full review of any tags currently open for comment and questions will be asked to gather consensus before final adoption.  No tags
will be adopted by fiat, or if there are unresolved questions.  They will just be moved to the next Phase for further comment and refinement.

Tags that are proposals or rough ideas should be expected to have syntax problems or typos.  Those should be refined away as they are worked on.  If they are not, that is a good idea
that the tag in question isn't being seen as useful and should be considered for dropping.

We are not a "standards body".  It is a community driven project where all stake holders are encouraged to participate, so that many voices are heard.  This is an open-source
project to be built fully in the open.  Discussions also take place on [podcastindex.social](https://podcastindex.social) where anyone is free to register and participate.


<br><br>


## Goal #1 - Eliminate Redundancy

There is significant overlap amongst the many existing podcast namespaces.  Each platform and publisher has created their own namespace to give their respective
system and audience the metadata they need in the way they want it delivered.


## Goal #2 - Keep "required" tags and attributes minimal

The only required tags should be those that solve an overwhelming need in the industry.  Requiring tags is a roadblock to adoption and should be avoided.  Attributes
should also only be required when they are key to the functionality of the tag.


## Goal #3 - Keep Exisiting Conventions

Reinventing the wheel helps nobody.  When at all possible, existing conventions should be maintained.  For example, it would make sense to turn **\<podcast:explicit>** into
a unary element, where it's existence is taken as a "yes" and it's absence as a "no".  But, that has never been the standard.  And, given as how this namespace will probably
sit alongside at least one other namespace, it makes sense to keep existing conventions in place.


## Goal #4 - Be General... to a point

There is no way to address every possible metadata point that each platform would want.  That is not the aim.  Instead we focus on defining the elements that would be useful
to the broadest set of apps, publishers, platforms and aggregators.  Individual parties can keep their respective supplemental namespaces small and targeted as an adjunct to
this larger namespace.  But, we don't want to be so general that the spec becomes overly complicated.  A beautiful, "perfect" spec is not important.  Solving real problems is.


## The Guiding Principles

Our guiding principles for development of this namespace are the "[Rules for Standards Makers](http://scripting.com/2017/05/09/rulesForStandardsmakers.html)" by Dave Winer.
Please read it before contributing if you aren't familiar with it.


## Official XMLNS Definition

To see the formalized tags, the official definition file is [here](docs/1.0.md).


## Supporting Platforms and Apps

To see a list of platforms and apps that currently implement some or all of these tags, see the list [here](docs/element-support.md).


## Example Feed

There is an example feed [example.xml](example.xml) in this repository showing the podcastindex namespace side by side with the Apple itunes namespace.

<br><br>

## Element List

### <u>Phase 1 (Closed on 11/15/20)</u>

<br>

The following tags have been formally adopted into the namespace.  They are fully documented in the XMLNS document located [here](docs/1.0.md).  Please see that file for
full implementation details.

- **\<podcast:locked>** <br>
- **\<podcast:transcript>** <br>
- **\<podcast:funding>** <br>
- **\<podcast:chapters>** <br>
- **\<podcast:soundbite>** <br>

<br>

### <u>Phase 2 (Closed on 1/31/21)</u>

<br>

The following tags have been formally adopted into the namespace.  They are fully documented in the XMLNS document located [here](docs/1.0.md).  Please see that file for
full implementation details.

- **\<podcast:person>** <br>
- **\<podcast:location>** <br>
- **\<podcast:season>** <br>
- **\<podcast:episode>** <br>

<br>


### <u>Phase 3 (Open - Closes 6/1/21)</u>

The following tags have been finalized ahead of formal consideration and review on June 1st, 2021.

<br>

### **\<podcast:trailer>** - <small>[Finalized](https://github.com/Podcastindex-org/podcast-namespace/issues/84)</small>

<br>

<b>

```xml
<podcast:trailer
 url="[uri of audio/video file(string)]"
 length="[file size in bytes(number)]"
 type="[mime type(string)]"
 pubdate="[date of release(RFC 2822)]"
 season="[season number(number)]"
>
[Title of Trailer(string)]
</podcast:trailer>
```

</b>

Channel

(optional | multiple)

This element is used to define the location of an audio or video file to be used as a trailer for the entire podcast or a specific season.  There can be more than one trailer present in the channel of the
feed.  If there is more than one present, the most recent one (according to it's `pubdate`) should be chosen as the preview by default within podcast apps.  If the `season` attribute is present, it must be a number that matches
the format of the `<podcast:season>` tag.  So, for a podcast that has 3 published seasons, a new `<podcast:trailer season="4">` tag can be put in the channel to later be matched up with a `<podcast:season>4<podcast:season>`
tag when it's published within a new `<item>`.

This element is basically just like an `<enclosure>` with the extra `pubdate` and `season` attributes added.

- `url` (required) This is a url that points to the audio or video file to be played.
- `pubdate` (required) The date the trailer was published.
- `length` (recommended) The length of the file in bytes.
- `type` (recommended) The mime type of the file.
- `season` (optional) If this attribute is present it specifies that this trailer is for a particular season number.

Example:
```xml
<podcast:trailer pubdate="Thu, 01 Apr 2021 08:00:00 EST" url="https://example.org/trailers/teaser" length="12345678" type="audio/mp3">Coming April 1st, 2021</podcast:trailer>
```

Example with Season Linkage:
```xml
<podcast:trailer pubdate="Thu, 01 Apr 2021 08:00:00 EST" url="https://example.org/trailers/season4teaser" length="12345678" type="video/mp4" season="4">Season 4: Race for the Whitehouse</podcast:trailer>

(later matches with)

<podcast:season name="Race for the Whitehouse">4</podcast:season>
```

<br>

### **\<podcast:license>** - <small>[Finalized](https://github.com/Podcastindex-org/podcast-namespace/issues/177)</small>

<br>

<b>

```xml
<podcast:license
 url="[https://urlofdetailledlicense]"
>
[license slug(string)]
</podcast:license>
```

</b>

Channel or Item

(optional | single)

This element defines the license that is applied to the audio/video content of the episode or the audio/video of the podcast as a whole.  The node value
should be a lower-cased reference to a license "identifier" defined in the [SPDX License List](https://spdx.org/licenses/) file or, if it's a custom license, it
can be a free form abbreviation of the name of the license.  Custom licenses should always include a url attribute reference.

- `url` (optional) This is a url that points to the full license details for this license.

Example:
```xml
<podcast:license url="https://creativecommons.org/licenses/by/4.0/">cc-by-4.0</podcast:license>
```

<br>


### **\<podcast:alternateEnclosure>** - <small>[Finalized](https://github.com/Podcastindex-org/podcast-namespace/issues/174#issue-798007719)</small>
<br>

<b>

```xml
<podcast:alternateEnclosure
 type="[mime type]"
 length="[(int)]"
 bitrate="[(float)]"
 height="[(int)]"
 lang="[(string)]"
 title="[(string)]"
 rel="[(string)]"
 codecs="[(string)]"
 default="[(boolean)]"
 >
[one or more <podcast:source> elements]
</podcast:alternateEnclosure>
```

</b>

Item

(optional | multiple)

This element defines a media file. One or more <podcast:source> tags must be contained within this element to list available methods to obtain the file. This is meant to provide different
versions of a media file -- such as low or high bitrate, alternate formats (different codecs or video), alternate URI schemes (IPFS or live streaming), or alternate download types not
indicated by the URI and type (like torrents).

This is a complex tag.  The full documentation is [here](https://github.com/Podcastindex-org/podcast-namespace/blob/main/proposal-docs/alternateEnclosure/alternateEnclosure.md).  Please
read that document to understand and comment on this proposal.

Example:
```xml
<enclosure url="https://best-podcast.com/file-0.mp3" length="43200000" type="audio/mpeg" />

<podcast:alternateEnclosure type="audio/mpeg" length="43200000" bitrate="128000" default="true" title="Standard">
    <podcast:source uri="https://best-podcast.com/file-0.mp3" />
    <podcast:source uri="ipfs://someRandomMpegFile" />
</podcast:alternateEnclosure>

<podcast:alternateEnclosure type="audio/opus" length="32400000" bitrate="96000" title="High quality">
    <podcast:source uri="https://best-podcast.com/file-high.opus" />
    <podcast:source uri="ipfs://someRandomHighBitrateOpusFile" />
</podcast:alternateEnclosure>

<podcast:alternateEnclosure type="audio/aac" length="54000000" bitrate="160000" title="High quality AAC">
    <podcast:source uri="https://best-podcast.com/file-proprietary.aac" />
    <podcast:source uri="ipfs://someRandomProprietaryAACFile" />
</podcast:alternateEnclosure>

<podcast:alternateEnclosure type="audio/opus" length="5400000" bitrate="16000" title="Low bandwidth">
    <podcast:source uri="https://best-podcast.com/file-low.opus" />
    <podcast:source uri="ipfs://someRandomLowBitrateOpusFile" />
</podcast:alternateEnclosure>
```

Example:
```xml
<podcast:alternateEnclosure type="audio/mpeg" length="2490970" bitrate="160707.74">
    <podcast:source uri="https://best-podcast.com/file-0.mp3" />
    <podcast:source uri="ipfs://QmdwGqd3d2gFPGeJNLLCshdiPert45fMu84552Y4XHTy4y" />
    <podcast:source uri="https://best-podcast.com/file-0.torrent" contentType="application/x-bittorrent" />
    <podcast:source uri="http://somerandom.onion/file-0.mp3" />
</podcast:alternateEnclosure>

<podcast:alternateEnclosure type="video/mp4" length="10562995" bitrate="681483.55" height="1080">
    <podcast:source uri="https://best-podcast.com/file-1080.mp4" />
    <podcast:source uri="ipfs://QmfQKJcp2xdByEt8mzWr1AJUhwvb9rdWPoacvdq2roDhgh" />
    <podcast:source uri="https://best-podcast.com/file-1080.torrent" contentType="application/x-bittorrent" />
    <podcast:source uri="http://somrandom.onion/file-1080.mp4" />
</podcast:alternateEnclosure>

<podcast:alternateEnclosure type="video/mp4" length="7924786" bitrate="511276.52" height="720">
    <podcast:source uri="https://best-podcast.com/file-720.mp4" />
    <podcast:source uri="ipfs://QmX33FYehk6ckGQ6g1D9D3FqZPix5JpKstKQKbaS8quUFb" />
    <podcast:source uri="https://best-podcast.com/file-720.torrent" contentType="application/x-bittorrent" />
    <podcast:source uri="http://somrandom.onion/file-720.mp4" />
</podcast:alternateEnclosure>
```


<br><br>


### <u>Phase 4 (Open for Proposals)</u>

The following tags should be considered purely as work in progress proposals.  They should not be relied upon or implemented except for testing purposes and experimentation.


### **\<podcast:recommendations>** - <small>[Discuss](https://github.com/Podcastindex-org/podcast-namespace/issues/205)</small>

<br>

<b>

```xml
<podcast:recommendations
 url="[url to json file(string)]"
 type="application/json"
 language="[language code(string)]"
>
[optional comments(string)]
</podcast:recommendations>
```

</b>

Channel or Item

(optional | multiple)

This element allows a podcaster (or third party, with podcater permission) to specify a list of recommended content for a podcast or an episode. The recommended content can be a
web page, a podcast, a podcast episode or a soundbite, so that listeners can eventually subscribe to a podcast, add an episode to playlist, add a soundbite to playlist, etc.

This is a complex tag.  The full documentation is [here](https://github.com/Podcastindex-org/podcast-namespace/blob/main/proposal-docs/recommendations/recommendations.md).  Please
read that document to understand and comment on this proposal.

Example:
```xml
<podcast:recommendations url="https://domain.tld/recommendation?guid=1234" type="application/json" />
```

Example:
```xml
<podcast:recommendations url="https://domain.tld/recommendation?guid=1234" type="application/json" language="en">Some other cool podcasts</podcast:recommendations>
```

<br>



## Other Proposals

A list of the current proposed tags can be found in the issues section [here](https://github.com/Podcastindex-org/podcast-namespace/labels/proposal).

<br><br>


## Verification for Importing and Transferring

If the "locked" element is present and set to "yes", podcasting hosts should not allow importing of this feed until the email listed in the element's owner="" attribute is
contacted and subsequently changes the value of the element to "no".


<br><br>


## IDs

There can be multiple **\<podcast:id>** elements to indicate a listing on multiple platforms, directories, hosts, apps and services.  The "platform" attribute shall be a slug
representing the platform, directory, host, app or service. The slugs will look like this:

- blubrry
- captivate
- podcastindex
- fireside
- transistor
- libsyn
- buzzsprout
- apple
- google
- spotify
- anchor
- overcast

More should be added by the community as needed.  This is just a starter list.  The full list is [here](serviceslugs.txt).


## Badges and Media

![Podcast Namespace](images/podcastindex-namespace-final.svg)
