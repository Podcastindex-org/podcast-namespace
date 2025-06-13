<hr>

<hr>

<hr>

# THIS IS NOT THE FINAL SPECIFICATION

You [want to look here for that](/docs/tags/social-interact.md)

<hr>

<hr>

<hr>

# The "podcast:social" Specification

<small>Version 1.0 by Benjamin Bellamy - 2021.04.13</small>

**This is not the final specification. You [want to look here for that](/docs/tags/social-interact.md)**

<br />

## Purpose

Social networks (Facebook, Instagram, Twitter, Mastodon…) and discussion platforms (Slack, Discord, XMPP/Jabber, IRC, Matrix…) are a convenient way
for podcasters to interact with their audience, and for listeners to interact with podcasters.
Thanks to them listeners can comment, share or like podcast episodes.
The purpose of this specification is to allow podcast apps to know where they should guide the listeners to make these interactions - and the onboarding process
necessary to make them possible - as seamless as possible.
Of course not all podcast apps would implement all platforms. Each one would implement the one(s) they want to provide their users a better interaction with.

There are three elements:

- **"podcast:social"** for the \<channel> element: tells the user **which platform(s)** is/are used for this podcast.
- **"podcast:socialSignUp"** for the \<podcast:social> element: tells the user **where to sign up** on this platform.
- **"podcast:socialInteract"** for the \<item> element: tells the user where to **interact with this specific episode**.

## Elements

### Social Element

- **\<podcast:social platform="[platform_id]" protocol="[protocol_name]" accountId="[podcast_account_id]" accountUrl="[podcast_account_url]" priority="[platform_priority]">**[one or more "podcast:socialSignUp" elements]**\</podcast:social>**

  Channel (optional | multiple)

  This element allows a podcaster to specify one or more platforms where listeners can interact.
  There may be several occurences of this tag for the same element (on several platforms, the podcast may have several accounts on the same plaforms…)

  - `platform` (required): This is the platform id. It can be one of the following:
    - castopod
    - mastodon
    - peertube
    - facebook
    - twitter
    - instagram
    - slack
    - discord
    - cast.garden
    - 3speak
    - peakd.com
    - …
  - `protocol` (required): This is the protocol name. It can be one of the following:
    - activitypub
    - xmpp
    - irc
    - matrix
    - facebook
    - twitter
    - instagram
    - slack
    - discord
    - hive
    - …
  - `accountId` (required): The podcast ID on this platform.
  - `accountUrl` (required): The podcast URL on this platform.
  - `priority` (optional): This platform priority (useful if the podcaster wants to tell which platform is preferred, lower is better)

  Examples:

  - `<podcast:social platform="twitter" protocol="twitter" accountId="@PodcastindexOrg" accountUrl="https://twitter.com/PodcastindexOrg"></podcast:social>`
  - `<podcast:social platform="mastodon" protocol="activitypub" accountId="@podcastindex@noagendasocial.com" accountUrl="https://noagendasocial.com/@podcastindex"></podcast:social>`

### SocialSignUp Element

- **\<podcast:socialSignUp homeUrl="[home_url]" signUpUrl="sign_up_url" priority="[platform_priority]" />**

  podcast:social (optional | multiple)

  This element allows easy onboarding for listeners on social/discussion platforms, especially for decentralized ones (such as Matrix or ActivityPub) where podcasters and listeners can register on different servers.

  - `homeUrl` (required): The platform home URL.
  - `signUpUrl` (required): The platform sign up URL.
  - `priority` (optional): This platform priority (useful if the podcaster wants to tell which platform is preferred, lower is better)

  Examples:

  - `<podcast:socialSignUp homeUrl="https://twitter.com/" signUpUrl="https://twitter.com/login" priority="1" />`
  - `<podcast:socialSignUp homeUrl="https://podcastindex.social/public" signUpUrl="https://podcastindex.social/auth/sign_up" priority="2" />`

### SocialInteract Element

- **\<podcast:socialInteract platform="platform_id" protocol="protocol_name" accountId="podcast_account_id" pubDate="publication_date" priority="platform_priority">**[URL to this episode on this platform]**</podcast:social>**

  Item or Channel (optional | multiple)

  This element allows listeners to interact with (comment, share, like, review…) an episode, or a podcast.

  - `platform` (required): This is the platform id. It can be one of the following:
    - castopod
    - mastodon
    - peertube
    - facebook
    - twitter
    - instagram
    - slack
    - discord
    - cast.garden
    - 3speak
    - peakd
    - fountain
    - none _(to indicate a strong opt-out preference)_
    - …
  - `protocol` (required): This is the protocol name. It can be one of the following:
    - activitypub
    - facebook
    - twitter
    - instagram
    - slack
    - discord
    - xmpp
    - irc
    - matrix
    - hive
    - lightningcomments (see #347 for protocol description)
    - …
  - `accountId` (required): The podcast ID on this platform.
  - `pubDate` (optional): publication date on this platform. This can be useful when there are several interactions for the same platform for the same episode (for instance, two Tweets about the same episode). Format must be [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601).
  - `priority` (optional): This platform priority (useful if the podcaster wants to tell which platform is preferred, lower is better)
  - element's content: URL to the social media post on this platform corresponding to this episode (if at the `<item>` level) or for this podcast (if at the `channel` level), or a short reason for apps to display when comments are disabled (if `platform="none"`)

  Examples:

  - `<podcast:socialInteract platform="twitter" protocol="twitter" accountId="@Podverse" priority="2" pubDate="2021-04-14T10:25:42Z">https://twitter.com/Podverse/status/1375624446296395781</podcast:socialInteract>`
  - `<podcast:socialInteract priority="1" platform="castopod" protocol="activitypub" accountId="@heloise@lespoesiesdheloise.fr" pubDate="2021-04-08T20:07:13+0000">https://lespoesiesdheloise.fr/@heloise/notes/e4b3d7f3-e84b-40c6-b828-f5537f0c3659</podcast:socialInteract>`
  - `<podcast:socialInteract priority="1" platform="fountain" protocol="lightningcomments" accountId="123868c219bdb51a33560d854d500fe7d3123a1ad9e05dd89d0007e11313588123">https://api.fountain.fm/v1/comments?feed=221233&amp;episode=1230123071</podcast:socialInteract>`

  Or to opt out:

  - `<podcast:socialInteract platform="none">Comments disabled for this episode</podcast:socialInteract>`

## Full RSS feed example

```xml
<?xml version="1.0" encoding="UTF-8"?>
<rss xmlns:itunes="http://www.itunes.com/dtds/podcast-1.0.dtd" xmlns:podcast="https://podcastindex.org/namespace/1.0" xmlns:content="http://purl.org/rss/1.0/modules/content/" version="2.0">
  <channel>
    <atom:link xmlns:atom="http://www.w3.org/2005/Atom" href="https://lespoesiesdheloise.fr/@heloise/feed.xml" rel="self" type="application/rss+xml"/>
    <title>Les Poésies d’Héloïse</title>
    […]

    <podcast:social priority="1" platform="castopod" protocol="activitypub" accountId="@heloise@lespoesiesdheloise.fr" accountUrl="https://lespoesiesdheloise.fr/@heloise">
      <podcast:socialSignUp priority="1" homeUrl="https://enfants-et-famille.podcasts.chat/public" signUpUrl="https://enfants-et-famille.podcasts.chat/auth/sign_up" />
      <podcast:socialSignUp priority="2" homeUrl="https://mamot.fr/public" signUpUrl="https://mamot.fr/auth/sign_up" />
      <podcast:socialSignUp priority="3" homeUrl="https://podcastindex.social/public" signUpUrl="https://podcastindex.social/auth/sign_up" />
    </podcast:social>
    <podcast:social priority="2" platform="mastodon" protocol="activitypub" accountId="@heloise@lespoesiesdheloise.fr" accountUrl="https://enfants-et-famille.podcasts.chat/web/accounts/5">
      <podcast:socialSignUp priority="1" homeUrl="https://enfants-et-famille.podcasts.chat/public" signUpUrl="https://enfants-et-famille.podcasts.chat/auth/sign_up"/>
    </podcast:social>
    <podcast:social priority="666" platform="facebook" protocol="facebook" accountId="LesPoesiesDHeloise" accountUrl="https://www.facebook.com/LesPoesiesDHeloise">
      <podcast:socialSignUp homeUrl="https://www.facebook.com/" signUpUrl="https://www.facebook.com/r.php?display=page" />
    </podcast:social>

    <item>
      <title>Oisillon bleu</title>
      <enclosure url="https://lespoesiesdheloise.fr/audio/AQAAAFoAAAC4gSwAnlI2AEwAAABk.q1c/podcasts/heloise/oisillon-bleu.mp3" length="3560094" type="audio/mpeg"/>
      <guid>https://lespoesiesdheloise.fr/episode_2019-04-10_lpdh_oisillonbleu</guid>
      <pubDate>Wed, 10 Apr 2019 14:15:00 +0000</pubDate>
      <link>https://lespoesiesdheloise.fr/@heloise/episodes/oisillon-bleu</link>
      […]

      <podcast:socialInteract priority="1" platform="castopod" protocol="activitypub" accountId="@heloise@lespoesiesdheloise.fr" pubDate="2021-04-14T10:25:42Z">https://lespoesiesdheloise.fr/@heloise/notes/4ba8df51-d67d-405d-a475-6471e1235c1c</podcast:socialInteract>
      <podcast:socialInteract priority="33" platform="fountain" protocol="lightningcomments" accountId="123868c219bdb51a33560d854d500fe7d3123a1ad9e05dd89d0007e11313588123" pubDate="2021-04-14T10:25:42Z">https://api.fountain.fm/v1/comments?feed=221233&amp;episode=1230123071</podcast:socialInteract>
      <podcast:socialInteract priority="666" platform="facebook" protocol="facebook" accountId="LesPoesiesDHeloise" pubDate="2021-04-14T10:25:42Z">https://www.facebook.com/LesPoesiesDHeloise/posts/399766303947452</podcast:socialInteract>

    </item>
    […]

  </channel>
</rss>
```

Discussion here:

- https://github.com/Podcastindex-org/podcast-namespace/issues/153
- https://podcastindex.social/web/statuses/106065482252134072
