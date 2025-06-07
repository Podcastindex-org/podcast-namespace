# Content Link

`<podcast:contentLink>`

The `contentLink` tag is used to indicate that the content being delivered by the parent element can be found at an external location instead of, or in addition to, the tag itself within an app. In most instances it is used as a fallback link for the user to use when the app itself can't handle a certain content delivery directly.

For instance, perhaps a podcast feed specifies a [`<podcast:liveItem>`](live-item.md) to deliver a live stream to apps. The feed may also give a `<podcast:contentLink>` pointing to YouTube and Twitch versions of the live stream as well, just in case the listener uses an app that doesn't fully support live streaming content.

### Parent

`<item>` or [`<podcast:liveItem>`](live-item.md)

### Count

Multiple

### Node Value

The node value is a free-form string meant to explain to the user where this content link points and/or the nature of its purpose.

### Attributes

- **href** (required) A string that is the uri pointing to content outside of the application.

### Examples

(under `<item>`)

```xml
<podcast:contentLink href="https://www.youtube.com/watch?v=8c7HWZROxD8">Watch this episode on YouTube!</podcast:contentLink>
```

(under [`<podcast:liveItem>`](live-item.md))

```xml
<podcast:contentLink href="https://youtube.com/blahblah/livestream">Live on YouTube!</podcast:contentLink>
```

```xml
<podcast:contentLink href="https://twitter.com/statuses/somepost">Chat on Twitter!</podcast:contentLink>
```
