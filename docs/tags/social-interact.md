# Social Interact

`<podcast:socialInteract>`

The `socialInteract` tag allows podcasters to associate social media information with an individual episode or the podcast as a whole. When the specified protocol supports programmatic API access, such as ActivityPub, podcast apps can retrieve comments directly and allow listeners to post replies back to the original discussion.

At the `<item>` level, the tag links an episode to the URL of a “root post” where discussion about that episode takes place. This root post serves as the canonical or official social media thread for comments and conversation related to the episode.

At the `<channel>` level, the tag can similarly link to a root post for discussion about the podcast as a whole. It can also be used more broadly to identify the podcast’s general social media account or accounts when there is no specific discussion thread.

If multiple `socialInteract` tags are given for an `<item>` or the `<channel>`, the `priority` attribute is strongly recommended to give the app an indication as to which comments to display first.

This tag can also be used as a signal to platforms and apps that the podcaster does not want public comments shown alongside the episode or podcast. For this purpose a `protocol` value of "disabled" can be specified, with no other attributes or node value present.

### Parent

`<item>` or `<channel>`

### Count

Multiple

### Attributes

- `protocol` **(required)**: The [protocol](/socialprotocols.txt) in use for interacting with the comment root post. If the platform has no interactive protocol (e.g. Instagram, TikTok), use the platform's name as the `protocol` value.
- `uri` **(required)**: The uri/url of root post comment.
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
