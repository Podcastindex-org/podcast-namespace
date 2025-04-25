# Soundbite

`<podcast:soundbite>`

Points to one or more soundbites within a podcast episode. The intended use includes episodes previews, discoverability, audiogram generation, episode highlights, etc. It should be assumed that the audio/video source of the soundbite is the audio/video given in the item's [`<enclosure>`](https://cyber.harvard.edu/rss/rss.html#ltenclosuregtSubelementOfLtitemgt) element.

### Parent

`<item>`

### Count

Multiple

### Node value

This is a free form string from the podcast creator to specify a title for the soundbite. If the podcaster does not provide a value for the soundbite title, then leave the value blank, and podcast apps can decide to use the episode title or some other placeholder value in its place. Please do not exceed `128 characters` for the node value or it may be truncated by aggregators.

### Attributes

- **startTime (required):** The time where the soundbite begins
- **duration (required):** How long is the soundbite (recommended between 15 and 120 seconds)

### Examples

```xml
<podcast:soundbite startTime="73.0" duration="60.0" />
```

```xml
<podcast:soundbite startTime="1234.5" duration="42.25">Why the Podcast Namespace Matters</podcast:soundbite>
```
