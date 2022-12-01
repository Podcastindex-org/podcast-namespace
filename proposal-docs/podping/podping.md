# The "podcast:podping" Specification

<small>Version 1.0 by Brian of London - 2021.06.08</small>

<br>

## Purpose

The Podping notification system is rapidly developing as a new standard for signalling new episodes of podcasts to reduce constant polling. Once a new episode or entirely new podcast is sent out as a podping on the Hive blockchain, aggregators and apps can query the feed.

However, as pointed out in issue https://github.com/Podcastindex-org/podcast-namespace/issues/258, there is, as yet, no way to know which feeds are using Podping.

This tag proposal will allow feed owners and the hosts of multiple feeds, to signal that future updates will be sent via Podping and there is no need to speculatively poll this rss feed.

An additional benefit will derive if feeds signal the name or names of the Hive accounts authorized to send Podpings. These authorized Hive accounts will be included in a `<podcast:hiveAccount>` tag

## API Requirements

This tag can also contribute to a future API endpoint for the PodcastIndex which can easily return whether or not any given RSS feed is using Podping and return the Hive accounts authorized to send pings.

## Specification

For the `<podcast:podping>` tag there is only one optional attribute `usesPodping` which will usually be set to `True` though could be set to `False` to specifically opt out of using Poding and indicate a feed must be polled by legacy RSS polling methods.

For the optional but helpful `<podcast:hiveAccount>` tag there is one attribute, a single value with a single Hive account which is allowed to issue `podpings` for this feed.

## Example

```xml
<podcast:podping>
    <podcast:hiveAccount account="podping.aaa"/>
    <podcast:hiveAccount account="podping.bbb"/>
    <podcast:hiveAccount account="podping.ccc"/>
</podcast:podping>
```

## Example

```xml
 <podcast:podping usesPodping="true"/>
```

or

```xml
 <podcast:podping></podcast:podping>
```
