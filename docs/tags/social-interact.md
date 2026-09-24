# Social Interact

`<podcast:socialInteract>`

The `socialInteract` tag allows a podcaster to attach the url of a "root post" of a comment thread to an episode, or to the podcast as a whole. This "root post" is treated as the canonical location of where the comments and discussion around the episode or podcast will take place. This can be thought of as the "official" social media comment space for the episode or podcast. If a protocol such as "activitypub" is used, or some other protocol that allows programmatic API access, these comments can be directly pulled into the app, and replies can be posted back to the thread from the app itself.

If multiple `socialInteract` tags are given for an `<item>` or the `<channel>`, the `priority` attribute is strongly recommended to give the app an indication as to which comments to display first.

This tag can also be used as a signal to platforms and apps that the podcaster does not want public comments shown alongside the episode or podcast. For this purpose a `protocol` value of "disabled" can be specified, with no other attributes or node value present.

### Channel-level vs. item-level usage

- At the `<channel>` level, `socialInteract` identifies the podcast's official social media account(s) or profile(s) — the general place where the show is present and discussed. The `uri` may point to a profile page rather than a specific post when used this way.
- At the `<item>` level, `socialInteract` identifies the specific "root post" of the comment thread for that episode, as originally specified.
- The informal `podcast:follow` tag, sometimes used for social links, is not part of this namespace spec. Channel-level `socialInteract` is the recommended, spec-formal way to declare a podcast's social accounts.

### Platforms without an interactive protocol

Many social platforms (e.g. Instagram, TikTok) have no API for pulling comments into an app or posting replies back — `socialInteract` can still be used to point to these accounts:

- If the platform isn't yet listed in [socialprotocols.txt](/socialprotocols.txt), add it as a new line there and use it as the `protocol` value. Don't repurpose an unrelated protocol (e.g. don't use `"activitypub"` for a plain Instagram link).
- A `protocol` value with no interactive API is still valid; it simply tells the app that no programmatic comment-pull/reply is available for that entry, only a link-out to the account.

### Parent

`<item>` or `<channel>`

### Count

Multiple

### Attributes

- `protocol` **(required)**: The [protocol](/socialprotocols.txt) in use for interacting with the comment root post.
- `uri` **(required)**: The uri/url of the root post, or for channel-level social identity, the account/profile url.
- `accountId` (recommended): The account id (on the commenting platform) of the account that created this root post.
- `accountUrl` (optional): The public url (on the commenting platform) of the account that created this root post.
- `priority` (optional): When multiple socialInteract tags are present, this integer gives order of priority. A lower number means higher priority.

Example (simple):

```xml
<podcast:socialInteract
        protocol="activitypub"
        uri="https://podcastindex.social/@dave/105079274766075912"
        accountId="@dave"
/>
```

Example (complex):

```xml
<podcast:socialInteract
        priority="1"
        protocol="activitypub"
        uri="https://podcastindex.social/@dave/105079274766075912"
        accountId="@dave"
        accountUrl="https://podcastindex.social/@dave"
/>
<podcast:socialInteract
        priority="2"
        protocol="twitter"
        uri="https://twitter.com/PodcastindexOrg/status/1507120226361647115"
        accountId="@podcastindexorg"
        accountUrl="https://twitter.com/PodcastindexOrg"
/>
```

Example (disabled):

```xml
<podcast:socialInteract protocol="disabled" />
```

- For **activitypub**, Mastodon or Pleroma's posting API returns a URI (a fully-formed URL with a GUID in it), and a URL (the HTML page where the comment lives). While both of these are acceptable values for the `uri` field referenced in the `socialInteract` specification, we'd recommend using the URI value.
