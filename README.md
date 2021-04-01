# The "podcast" Namespace

A wholistic rss namespace for podcasting that is meant to synthesize the fragmented world of podcast namespaces.  The broad goal is to create a single, compact, efficient
namespace that is easily extensible, community controlled/authored and addresses the needs of the independent podcast industry now and in the future.
Our hope is that this namespace
will become the framework that the independent podcast community needs to deliver new functionality to apps and aggregators.


<br><br>


## Current Roadmap

**Phase 1** - [Closed] Comment period closed on `11/15/20` and 5 tags were adopted.

**Phase 2** - [Closed] Comment period closed on `1/31/21` and 4 tags were adopted.

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

The following tags should be considered purely as work in progress proposals.  They should not be relied upon or implemented except for testing purposes and experimentation.

<br>

### **\<podcast:trailer>** - <small>[Discuss](https://github.com/Podcastindex-org/podcast-namespace/issues/84)</small>

<br>

<b>

```
<podcast:trailer
 pubdate="[date of release(RFC 2822)]"
 url="[uri of audio/video file(string)]"
 length="[file size in bytes(number)]"
 type="[mime type(string)]"
 season="[season number(number)]"
>
[Title of Trailer(string)]
</podcast:trailer>
```

</b>

Channel

(optional | multiple)

This element is used to define the location of an audio or video file to be used as a trailer for the entire podcast or a specific season.  There can be more than one trailer present in the channel of the feed.  If there is more than one listed, the most recent one according to it's `pubdate` should be chosen by default within podcast apps.  If the `season` attribute is present, then the `<podcast:season>` element it references must also be present within the feed.

- `pubdate` (required) The date the trailer was published.
- `url` (required) This is a url that points to the audio or video file to be played.
- `length` (recommended) The length of the file in bytes.
- `type` (recommended) The mime type of the file.
- `season` (optional) If this attribute is present it specifies that this trailer is for a particular season number.

Example:
```
<podcast:trailer pubdate="Thu, 01 Apr 2021 08:00:00 EST" url="https://example.org/trailers/teaser" length="12345678" type="audio/mp3">Coming April 1st, 2021</podcast:trailer>
```

Example with Season Linkage:
```
<podcast:trailer pubdate="Thu, 01 Apr 2021 08:00:00 EST" url="https://example.org/trailers/season4teaser" length="12345678" type="video/mp4" season="4">Season 4: Race for the Whitehouse</podcast:trailer>

(combined with)

<podcast:season name="Race for the Whitehouse">4</podcast:trailer>
```


<br><br>


## Current Proposals

A list of the current proposed tags can be found in the issues section [here](https://github.com/Podcastindex-org/podcast-namespace/labels/proposal).

<br><br>


## Verification for Importing and Transferring

If the "locked" element is present and set to "yes", podcasting hosts should not allow importing of this feed until the email listed in the element's owner="" attribute is
contacted and subsequently changes the value of the element to "no".


<br><br>


## IDs

Their can be multiple **\<podcast:id>** elements to indicate a listing on multiple platforms, directories, hosts, apps and services.  The "platform" attribute shall be a slug
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