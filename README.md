# The "podcast" Namespace

A wholistic rss namespace for podcasting that is meant to synthesize the fragmented world of podcast namespaces.  The broad goal is to create a single, compact, efficient
namespace that is easily extensible, community controlled/authored and addresses the needs of the independent podcast industry now and in the future.
Our hope is that this namespace
will become the framework that the independent podcast community needs to deliver new functionality to apps and aggregators.


<br><br>


## Current Roadmap

**Phase 1** - [Closed] Comment period closed on `11/15/20` and 5 tags were adopted.

**Phase 2** - [Closed] Comment period closed on `1/31/21` and 4 tags were adopted.

**Phase 3** - [Open] Proposals welcome.  No dates assigned.

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

<br><br>

### <u>Phase 2 (Closed on 1/31/21)</u>

<br>

The following tags have been formally adopted into the namespace.  They are fully documented in the XMLNS document located [here](docs/1.0.md).  Please see that file for
full implementation details.

- **\<podcast:person>** <br>
- **\<podcast:location>** <br>
- **\<podcast:season>** <br>
- **\<podcast:episode>** <br>

<br><br>


### <u>Phase 3 (Open)</u>

The following tags should be considered purely as proposals.  They should not be relied upon or implemented except for testing purposes and experimentation.

<br>

- **<podcast:license url="[https://urlofdetailledlicense]">**[license slug]**\</podcast:license>**

    Channel or Item

    (optional | single)

    This element defines the license that is applied to the audio content of the episode or the audio of the podcast as a whole.  The node value
    should be a reference to a slug defined in the license slug file.

    - `url` (optional) This is a url that points to the full license details for this license.

    Example: <podcast:license url="https://creativecommons.org/licenses/by/4.0/">cc-by-4.0</podcast:license>

<br>

- **\<podcast:id platform="[service slug]" id="[platform id]" url="[link to the podcast page on the service]" />**

   Channel

   (optional | multiple)

   See "[IDs](#user-content-ids)" in this document for an explanation.

   - `platform` (required) This is the service slug of the platform.
   - `id` (required) This is the unique identifier for this podcast on the respective platform.
   - `url` (optional) A url to the page for this podcast on the respective platform.

<br>


- **\<podcast:social platform="[service slug]" url="[link to social media account]">**[social media handle]**\</podcast:social>**

   Channel or Item

   (optional | multiple)

   This element lists social media accounts for this podcast.  The service slugs should be community written into the accompanying serviceslugs.txt file.

   The maximum recommended string length of the node value is 128 characters.

<br>

- **\<podcast:category>**[category Name]**\</podcast:category>**

   Channel

   (optional | multiple)

   See "Categories" in this document for an explanation.  There can be up to a total of 9 categories defined.

   There can be a maximum of 9 category elements defined in a feed.  Any number greater than that should be discarded.

   Category names are defined in the accompanying "categories.json" file in this repository.  They should be referenced in the element by their textual name.
   The characters can be in any case.  This list of categories aims to replicate the current standard but also eliminate as much as possible compound, heirarchical
   naming and the use of ampersands.  Thus, "Health & Fitness" becomes "Health" and "Fitness" as two distinct categories.  And, "Religion & Spirituality" becomes
   two separate categories.  Again, they are different things that don't always go together.  Splitting them allows for more flexible combinations.  And, avoiding
   ampersands makes xml encoding errors less likely.

<br>

- **\<podcast:contentRating>**[rating letter]**\</podcast:contentRating>**

  Channel or Item

  (optional | single)

  Specifies the generally accepted rating letter of G, PG, PG-13, R or X.  Or, perhaps an age rating system like all, 14, 19, adult.  Needs discussion.

<br>

- **\<podcast:previousUrl>**[url this feed was imported from]**\</podcast:previousUrl>**

   Channel

   (optional | multiple)

   Lists the previous url of this feed before it was imported.  Any time a feed is moved, an additional **\<podcast:previousUrl>** element
   should be added to the channel, to create a paper trail of all the previous urls this feed has lived at.  This way, aggregators can easily deduplicate their feed lists.

<br>

- **\<podcast:alternateEnclosure url="[url of media asset]" type="[mime type]" length="[(int)]" bitrate="[(float)]" title="[(string)]" rel="[(string)]" />**

   Channel (optional | single)

   Item (optional | multiple)

   This element is meant to provide alternate versions of an enclosure, such as low or high bitrate, or alternate formats or alternate uri schemes, like IPFS or live streaming.
   There may be multiple alternateEnclosure elements in an item, but there must be no more than one in a channel.  The presence of this element at the channel level would be
   useful for adding a video/audio trailer or intro media that introduces the listener to the podcast.  For instance, in a podcast of an audiobook, this could be the book's
   introduction or preface.  The alternateEnclosure element always refers to an "alternate" media version.  The standard RSS enclosure element is always the default media to be played.

   An `<enclosure>` tag must be present along with this tag within the item.

   - `url` (required) This is the url to the media asset.
   - `type` (required) Mime type of the media asset.
   - `length` (required) Length of the file in bytes.
   - `bitrate` (optional) Encoding bitrate of media asset.
   - `title` (required) Alternate assets need a title since main title will apply to primary asset.
   - `rel` (optional) Indicates what the purpose of this enclosure is. Like "lowbitrate" for a small file to use over cellular.

<br>

- **\<podcast:indexers>** + **\<podcast:block>[domain, bot or service slug]\</podcast:block>**

   Channel (optional | single)

   The "indexers" element is meant as a container for one or more `<podcast:block>` elements which send a signal to podcast aggregators whether they are allowed to pull and parse
   this feed.  If the aggregator is listed as blocked, it should take that as a signal by the feed owner to not index/aggregate this feed.

   *Note: this element needs a lot more discussion and work.  This is just a placeholder for discussion.*

<br>

- **\<podcast:images srcset="[url to image] [pixelwidth(int)]w,
                             [url to image] [pixelwidth(int)]w,
                             [url to image] [pixelwidth(int)]w,
                             [url to image] [pixelwidth(int)]w" />**

   Channel or Item

   (optional | single)

   This points to a group of images, separated by commas - each with a pixel width declared after them.  It is highly recommended that the images referenced
   be square (1:1 ratio), as this is the industry standard for podcast album art, and what podcast apps expect to work with.  The srcset attribute is designed
   to work like the ```srcset``` attribute in the HTML5 specification.  Suggested widths are 1500px, 600px, 300px and 150px.  See the example feed in this
   repo for an example of how this looks in practice.

   All attributes are required.

<br>

- **\<podcast:contact type="[feedback or advertising or abuse]">**[email address or url]**\</podcast:contact>**

   Channel

   (optional | multiple)

   This element allows for listing different contact methods for the podcast owner.  This could be for general feedback, advertising inquiries, abuse reports, etc.  Only one element of each "type"
   is allowed.

   All attributes are required.

<br>

- **\<podcast:value>**[one or more "valueRecipient" elements]**\</podcast:value>**

   Details for this tag are now located in dedicated documentation [here](value/value.md).

- **\<podcast:valueRecipient />**

   Details for this tag are now located in dedicated documentation [here](value/value.md).


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
