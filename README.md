# The "podcast" namespace

A wholistic rss namespace for podcasting that is meant to synthesize the fragmented world of podcast namespaces.  The broad goal is to create one namespace
to rule them all, that is easily extensible, community controlled/authored and addresses the needs of the indie podcast industry now and in the future.
The large podcast platforms have shown virtually no interest in extending their namespaces for new functionality in many years.  Our hope is that this namespace
will become the framework that the indie podcast community needs to deliver new functionality to apps and aggregators.


## Goal #1 - Eliminate Redundancy

There is significant overlap amongst the many existing podcast namespaces.  Each platform and publisher has created their own namespace to give their respective
system and audience the metadata they need in the way they want it delivered.


## Goal #2 - Avoid Attributes

Attributes in xml elements should be used only where absolutely needed.  The preference is to create a new element type, rather than reuse the same element with
different attributes.  For example, instead of using **\<podcast:image type="Large">**, we would use **\<podcast:imageLarge>**.  This makes the corresponding
aggregator code easier and more linear.


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


## Element List (current)

- **\<podcast:imageLarge>**[url to a large image file]**\</podcast:imageLarge>** - This is assumed to point to an image that is 1000px or larger in size
- **\<podcast:imageMedium>**[url to a medium image file]**\</podcast:imageMedium>** - This is assumed to point to an image that is 300px to 999px in size
- **\<podcast:imageSmall>**[url to a small image file]**\</podcast:imageSmall>** - This is assumed to point to an image that is 299px or less in size
- **\<podcast:category>**[Category Name]**\</podcast:category>** - This is a channel-level element.  See "Categories" in this document for an explanation.  There can be up to a total of 9 categories defined.
- **\<podcast:location>**[CountryCode|Locality]**\</podcast:location>** - The country code and locality name given with a pipe as a separator
- **\<podcast:locked>**[yes|no]**\</podcast:locked>** - This is a channel-level element.  This tells other podcast platforms whether they are allowed to import this feed.  A value of "yes" means that any attempt to import
   this feed into a new platform should be rejected.  It is expected that podcast hosting providers will enable a toggle in their GUI to allow their users to turn
   feed transfer lock on or off.
- **\<podcast:email>**[email address]**\</podcast:email>** - This is a channel-level element.  An email address that can be used to verify ownership of this feed during move and import operations.  This could be a public email or a
   virtual email address at the hosting provider that redirects to the owner's true email address.
- **\<podcast:previousUrl>**[url this feed was imported from]**\</podcast:previousUrl>** - This is a channel-level element.  Lists the previous url of this feed before it was imported.  Any time a feed is moved, an additional **\<podcast:previousUrl>** element
   should be added to the channel, to create a paper trail of all the previous urls this feed has lived at.  This way, aggregators can easily deduplicate their feed lists.
- **\<podcast:newFeedUrl>**[url this feed was imported from]**\</podcast:newFeedUrl>** - This is a channel-level element.  If the feed moved, or was imported to a different hosting platform, this element can specify the new location.
- **\<podcast:id platform="[host slug]">**[the id string]**\</podcast:id>** - This is a channel-level element.  See "ID's" in this document for an explanation.


## Element List (proposed)
- **\<podcast:captions>** - This is an item-level element to contain information about closed captions within the episode.
- **\<podcast:transcripts>** - This is an item-level element to contain a transcript of an episode.
- **\<podcast:alternateEnclosure type="[mime type]" length="[(int)]" bitrate="[(float)]" [live]>**[uri of media asset]**\</podcast:alternateEnclosure>** - This is an item-level element that is meant to provide alternate versions of an enclosure, such as low or
  high bitrate, or alternate formats or alternate uri schemes, like IPFS or live streaming.


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

Their can be multiple **\<podcast:id>** elements to indicate a listing on multiple platforms, directories, hosts, apps and services.  If no "platform" attribute is given, the id string in the element is
considered to be the registered Podcastindex.org ID.  If the "platform" attribute is present, the element's value is taken as the ID of this podcast on that respective platform.  The following slugs
can be used:

- blubrry
- captivate
- fireside
- transistor
- libsyn
- itunes
- google
- spotify
- anchor

More should be added over time by the community as needed.  This is just a starter list.


## Example feed

There is an example feed in this repository showing the podcastindex namespace side by side with the Apple itunes namespace.