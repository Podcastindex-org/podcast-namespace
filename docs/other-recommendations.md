# Other Recommendations from the Creators of the RSS Namespace Extension for Podcasting

While the wholistic RSS namespace for podcasting is meant to synthesize the fragmented world of podcast namespaces, there are some existing standards and defacto-standards we want do endorse:

## Episode GUID

For all new episodes, we recommended to use

- either a permanent URI, e.g. `https://example.com/ep0003`
- or a [Universally unique identifier (UUID)](https://en.wikipedia.org/wiki/Universally_unique_identifier) e.g. `7c029615-a810-5214-9342-eee73f58435d`

The GUID of an episode should never change, even if a new version of the episode is being published, otherwise this episode will be duplicated downstream.

Consumers of podcast feeds should fall back to the enclosure URL or the namespaced UUIDv5 (SHA1 Hash with ns:URL) of the enclosure URL, if the value is not set – see https://github.com/Podcastindex-org/podcast-namespace/issues/186#issuecomment-932742468 for more details.

#### Examples

Only one entry per `<item>` is valid:

```xml
<guid>https://example.com/ep0003</guid>
<guid isPermaLink="false">7c029615-a810-5214-9342-eee73f58435d</guid>
```

(`isPermaLink` is optional, its default value is true –)

## Episode Description and Summary

If you use `<itunes:summary>`, it should be an actual summary of the episode, in one or two sentences, and not a copy of the description. Be aware that Apple dropped `<itunes:summary>` from [their spec](https://help.apple.com/itc/podcasts_connect/#/itcb54353390) but many other clients still support it.

The [original RSS specification](https://cyber.harvard.edu/rss/rss.html#hrelementsOfLtitemgt) defined `description` as follows:

> A channel may contain any number of `<item>`s. An item may represent a "story" -- much like a story in a newspaper or magazine; if so its description is a synopsis of the story, and the link points to the full story. An item may also be complete in itself, if so, the description contains the text (entity-encoded HTML is allowed; see examples) …

There also exists `<content:encoded>` for dedicated HTML episode notes [[rssboard.org]](https://www.rssboard.org/rss-profile#namespace-elements-content-encoded), but more and more publishers switch to only having `<description>`.

When your description contains HTML, we recommend to wrap it into `<![CDATA[` and `]]>`. See https://podnews.net/article/html-episode-notes-in-podcast-rss and https://podnews.net/article/how-podcast-show-notes-display for more information about supported HTML tags in different clients. Typical are `<p>`, `<br/>`, `<a href=""> </a>`, `<ul>` and `<li>`.

## Feed Paging and Archiving (RFC5005)

To be able to put more metadata into RSS feeds, while keeping the full archive of all old episodes becomes a challange for some podcasters and podcast clients. A typical workaround in the industry is to only render the full metadata of the newest episodes, where there is already a proper solution since 2007: [RFC5005](https://tools.ietf.org/html/rfc5005)

There are already a few players implementing RFC5005 for a while, but . Adoption from clients is sporadic. A new/different standard wouldn't help though because I'd say RFC5005 does all that's required. We need to be louder about the existence of the standard and ask for it's implementation from all sides.

#### Examples

Excerpt from https://feeds.metaebene.me/freakshow/mp3 feed page 2,
parent `<channel>`:

```xml
<atom:link rel="next" href="https://freakshow.fm/feed/mp3?paged=3" />
<atom:link rel="prev" href="https://freakshow.fm/feed/mp3" />
<atom:link rel="first" href="https://freakshow.fm/feed/mp3" />
<atom:link rel="last" href="https://freakshow.fm/feed/mp3?paged=9" />
```

## WebSub and Podping.cloud

TBD
