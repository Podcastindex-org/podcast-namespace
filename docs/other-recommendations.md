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

There also exists `<content:encoded>` for dedicated HTML episode notes [(rssboard.org)](https://www.rssboard.org/rss-profile#namespace-elements-content-encoded), but more and more publishers switch to only having `<description>`.

When your description contains HTML, we recommend to wrap it into `<![CDATA[` and `]]>`. See https://podnews.net/article/html-episode-notes-in-podcast-rss and https://podnews.net/article/how-podcast-show-notes-display for more information about supported HTML tags in different clients. Typical are `<p>`, `<br/>`, `<a href=""> </a>`, `<ul>` and `<li>`.

## Feed Paging and Archiving (RFC5005)

Putting detailed episode metadata into RSS feeds while keeping the full archive of all old episodes, becomes a challenge for some podcasters and podcast clients. A typical workaround in the industry is to only render the full metadata of the newest episodes, even though there has been a proper solution since 2007: [RFC5005](https://datatracker.ietf.org/doc/html/rfc5005)

There are already a few podcast clients implementing RFC5005 for a while, but adoption from clients in general is sporadic. A new/different standard wouldn't help though because RFC5005 does all that's required. We need to be louder about the existence of the standard and ask for it's implementation from all sides.

#### Examples

Excerpt from https://feeds.metaebene.me/freakshow/mp3 feed page 2,
parent `<channel>`:

```xml
<atom:link rel="next" href="https://feeds.metaebene.me/freakshow/mp3?paged=3" />
<atom:link rel="prev" href="https://feeds.metaebene.me/freakshow/mp3" />
<atom:link rel="first" href="https://feeds.metaebene.me/freakshow/mp3" />
<atom:link rel="last" href="https://feeds.metaebene.me/freakshow/mp3?paged=9" />
```

## HTTP Conditional Requests (Last-Modified / ETag)

Requested in [issue #90](https://github.com/Podcastindex-org/podcast-namespace/issues/90).

Podcast apps and aggregators typically re-fetch a feed's full RSS/XML body on every poll, even when nothing in the feed has changed. At the scale of an aggregator like Podcast Index — polling millions of feeds on a regular schedule — that adds up to a lot of wasted bandwidth for hosts and publishers, and it limits how often any one feed can affordably be checked.

HTTP has had a solution for this since the 1990s: conditional requests. We recommend podcast hosts and feed publishers support both of the following response headers on feed requests:

- `Last-Modified` — an [RFC 7231](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Last-Modified) timestamp of when the feed content last actually changed
- `ETag` — an opaque token (commonly a hash of the feed body) that changes whenever the feed content changes

And honor the corresponding request headers on the next fetch:

- `If-Modified-Since` — the client sends back the `Last-Modified` value it saw last time
- `If-None-Match` — the client sends back the `ETag` value it saw last time

If neither has changed since the last fetch, the server should respond `304 Not Modified` with an empty body instead of resending the whole feed. If the feed did change, it should respond `200 OK` with the full body and updated headers, as normal.

#### Examples

```
# First poll
GET /feed.xml HTTP/1.1
Host: example.com

HTTP/1.1 200 OK
Last-Modified: Wed, 21 Oct 2026 07:28:00 GMT
ETag: "33a64df551"

# Next poll
GET /feed.xml HTTP/1.1
Host: example.com
If-Modified-Since: Wed, 21 Oct 2026 07:28:00 GMT
If-None-Match: "33a64df551"

HTTP/1.1 304 Not Modified
```

A couple of practical notes:

- Only update `Last-Modified`/`ETag` when the feed content itself changes, not every time the feed happens to be regenerated or re-cached. A publishing tool that bumps `Last-Modified` on every cache refresh defeats the purpose, since clients will never see a match and will keep re-downloading the full feed anyway.
- Most CDNs and podcast hosting platforms already handle conditional requests correctly if configured — it's worth confirming the feature is actually enabled and passed through rather than assuming it's on, since some caching or proxy layers strip these headers by default.

## WebSub and Podping.cloud

TBD
