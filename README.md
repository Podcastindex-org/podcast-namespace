# The "podcast" namespace

A wholistic rss namespace for podcasting that is meant to synthesize the fragmented world of podcast namespaces.  The broad goal is to create one namespace
to rule them all, that is easily extensible, community controlled/authored and addresses the needs of the indie podcast industry now and in the future.
The large podcast platforms have shown virtually no interest in extending their namespaces for new functionality in many years.  Our hope is that this namespace
will become the framework that the indie podcast community needs to deliver new functionality to apps and aggregators.


## Goal #1 - Eliminate Redundancy

There is significant overlap amongst the many existing podcast namespaces.  Each platform and publisher has created their own namespace to give their respective
system and audience the metadata they need in the way they want it delivered.


## Goal #2 - Avoid Attributes and sub-elements

Attributes in xml elements should be used only where absolutely needed.  The preference is to create a new element type, rather than reuse the same element with
different attributes.  For example, instead of using **\<podcast:image type="Large">**, we would use **\<podcast:imageLarge>**.  This makes the corresponding
aggregator code easier and more linear.  Sub-elements should be avoided if at all possible.


## Goal #3 - Use RSS Native Elements

The RSSv2.0 specification already has a robust set of defined elements.  We should aim to use those instead of creating new ones.  Instead of creating a **\<podcast:owner>**
element, we can use the already existing **\<managingEditor>** element that contains both name and email address.  In situations like item-level images, where the RSS
spec never defined that element, it is appropriate to define new ones.


## Goal #4 - Keep Exisiting Conventions

Reinventing the wheel helps nobody.  When at all possible, existing conventions should be maintained.  For example, it would make sense to turn **\<podcast:explicit>** into
a unary element, where it's existence is taken as a "yes" and it's absence as a "no".  But, that has never been the standard.  And, given as how this namespace will probably
sit alongside at least one other namespace, it makes sense to keep existing conventions in place.


## Goal #5 - Be General

There is no way to address every possible metadata point that each platform would want.  That is not the aim.  Instead we focus on defining the elements that would be useful
to the broadest set of apps, publishers, platforms and aggregators.  Individual parties can keep their respective supplemental namespaces small and targeted as an adjunct to
this larger namespace.


## Element List

### Phase 1 (Open)
- **\<podcast:imageLarge size="[pixel width]">**[url to a large image file]**\</podcast:imageLarge>** - This is assumed to point to an image that is 1000px or larger in size.
   The image must be square (1:1 ratio).  The image content may differ from other images specified in the feed where appropriate.  The "size" attribute is mandatory.
- **\<podcast:imageMedium size="[pixel width]">**[url to a medium image file]**\</podcast:imageMedium>** - This is assumed to point to an image that is 300px to 999px in size.
   The image must be square (1:1 ratio).  The image content may differ from other images specified in the feed where appropriate.  The "size" attribute is mandatory.
- **\<podcast:imageSmall size="[pixel width]">**[url to a small image file]**\</podcast:imageSmall>** - This is assumed to point to an image that is 299px or less in size.
   The image must be square (1:1 ratio).  The image content may differ from other images specified in the feed where appropriate.  The "size" attribute is mandatory.
- **\<podcast:category>**[Category Name]**\</podcast:category>** - This is a channel-level element.  See "Categories" in this document for an explanation.  There can be up to a total of 9 categories defined.
- **\<podcast:location osm_id="[place ID]">**[CountryCode|Locality]**\</podcast:location>** - The ISO 3166-1 alpha-2 country code; a pipe as separator; then a humanly-readable place name as preferred by the publisher.
   The (mandatory) parameter osm_id is the OpenStreetMap ID for that place, using OpenStreetMap's API.
- **\<podcast:locked>**[yes|no]**\</podcast:locked>** - This is a channel-level element.  This tells other podcast platforms whether they are allowed to import this feed.  A value of "yes" means that any attempt to import
   this feed into a new platform should be rejected.  It is expected that podcast hosting providers will enable a toggle in their GUI to allow their users to turn
   feed transfer lock on or off.
- **\<podcast:email>**[email address]**\</podcast:email>** - This is a channel-level element.  An email address that can be used to verify ownership of this feed during move and import operations.  This could be a public email or a
   virtual email address at the hosting provider that redirects to the owner's true email address.
- **\<podcast:previousUrl>**[url this feed was imported from]**\</podcast:previousUrl>** - This is a channel-level element.  Lists the previous url of this feed before it was imported.  Any time a feed is moved, an additional **\<podcast:previousUrl>** element
   should be added to the channel, to create a paper trail of all the previous urls this feed has lived at.  This way, aggregators can easily deduplicate their feed lists.
- **\<podcast:newFeedUrl>**[the url the feed now lives at]**\</podcast:newFeedUrl>** - This is a channel-level element.  If the feed moved, or was imported to a different hosting platform, this element may exist and specify the new location.  It may refer
   to itself as a confirmation to aggregators that they now have the most current url.
- **\<podcast:id platform="[service slug]">**[the id string]**\</podcast:id>** (optional|multiple) - This is a channel-level element.  See "ID's" in this document for an explanation.

- **\<podcast:host href="[url of bio/wiki/blog/etc.]" img="[link to image/headshot]">**[name of person]**\</podcast:host>** (optional|multiple) - This is an item-level element.  It identifies a host
   of a podcast episode.  All other attributes are optional but recommended for disambiguation and good meta-data for apps.
- **\<podcast:guest href="[url of bio/wiki/blog/etc.]" img="[link to image/headshot]">**[name of person]**\</podcast:guest>** (optional|multiple) - This is an item-level element.  It identifies a guest
   in a podcast episode.  All other attributes are optional but recommended for disambiguation and good meta-data for apps.
- **\<podcast:contentRating>**[rating letter]**\</podcast:contentRating>** - This is a channel, or item-level element specifying a generally accepted rating letter of G, PG, PG-13, R or X.

- **\<podcast:transcript language="[language code]" type="[mime type]">**[url to a file or website]**\</podcast:transcript>** - This is an item-level element.  Links to an external file containing a transcript.  The
   mime type of the file should be given - such as text/plain, text/html, etc.
- **\<podcast:captions language="[language code]" type="text/srt">**[url to a SRT captions file]**\</podcast:captions>** - This is an item-level element.  Links to an industry standard closed-caption/subtitle file format.
- **\<podcast:alternateEnclosure type="[mime type]" length="[(int)]" bitrate="[(float)]" title="[(string)]" [live]>**[uri of media asset]**\</podcast:alternateEnclosure>** - This is a channel or item-level
   element that is meant to provide alternate versions of an enclosure, such as low or
   high bitrate, or alternate formats or alternate uri schemes, like IPFS or live streaming.  The title attribute is optional.  The "live" attribute is unary - its presence indicates that
   the uri refers to a live stream.  There may be multiple alternateEnclosure elements in an item, but there must be no more than one in a channel.  The presence of this element at the
   channel level would be useful for adding a video/audio trailer or intro media that introduces the listener to the podcast.  For instance, in a podcast of an audiobook, this could be the book's
   introduction or preface.  The alternateEnclosure element always refers to an "alternate" media version.  The standard RSS enclosure element is always the default media to be
   played.


### Phase 2 (Open)
- **\<podcast:social platform="[service slug]" url="[link to social media account]">**[social media handle]**\</podcast:social>** (optional|multiple) - This is a channel-level
   element listing possibly multiple social media accounts for this podcast.  The service slugs should be community written in the accompanying serviceslugs.txt file.
- **\<podcast:funding platform="[service slug]" url="[url for the show at the platform]">**[podcast handle at the platform]**\</podcast:funding>** - (optional|multiple) - This is a
   channel-level element listing multiple possible donation/funding links for the podcast.


## Categories

There can be a maximum of 9 category elements defined in a feed.  Any number greater than that should be discarded.

Category names are defined in the accompanying "categories.json" file
in this repository.  They should be referenced in the element by their textual name.  The characters can be in any case.  This list of categories aims to replicate the current
standard but also eliminate as much as possible compound, heirarchical naming and the use of ampersands.  Thus, "Health & Fitness" becomes "Health" and "Fitness" as two distinct categories.
And, "Religion & Spirituality" becomes two separate categories.  Again, they are different things that don't always go together.  Splitting them allows for more flexible combinations.  And,
avoiding ampersands makes xml encoding errors less likely.



## Verification, importing and moving

If the "locked" element is present and set to "yes", podcasting hosts and platforms should not allow importing of this feed until the **\<podcast:email>** or other defined feed owner (such as **\<managingEditor>**) is
contacted and subsequently sets the "locked" element to "no" or removes it from the feed.

The **\<podcast:previousUrl>** element acts like a relay header in an email envelope.  Each time a feed is imported, an additional **\<podcast:previousUrl>** should be added, and all previous ones preserved.

Once a successful import has taken place, the **\<podcast:newFeedUrl>** element can be put in the old feed as a pointer to the new location.



## ID's

Their can be multiple **\<podcast:id>** elements to indicate a listing on multiple platforms, directories, hosts, apps and services.  The "platform" attribute shall be a slug
representing the platform, directory, host, app or service. The slugs will look like this:

- blubrry
- captivate
- podcastindex
- fireside
- transistor
- libsyn
- itunes
- google
- spotify
- anchor
- overcast

More should be added by the community as needed.  This is just a starter list.


## Example feed

There is an example feed in this repository showing the podcastindex namespace side by side with the Apple itunes namespace.