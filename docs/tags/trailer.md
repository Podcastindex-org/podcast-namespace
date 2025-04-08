# Trailer

`<podcast:trailer>`

This element is used to define the location of an audio or video file to be used as a trailer for the entire podcast or a specific season. There can be more than one trailer present in the channel of the feed. This element is basically just like an [`<enclosure>`](https://cyber.harvard.edu/rss/rss.html#ltenclosuregtSubelementOfLtitemgt) with the extra `pubdate` and `season` attributes added.

If there is more than one trailer tag present in the channel, the most recent one (according to its `pubdate`) should be chosen as the preview by default within podcast apps.

### Parent

`<channel>`

### Count

Multiple

### Node Value

The node value is a string, which is the title of the trailer. It is required. Please do not exceed `128 characters` for the node value or it may be truncated by aggregators.

### Attributes

- **url:** (required) This is a url that points to the audio or video file to be played. This attribute is a string.
- **pubdate:** (required) The date the trailer was published. This attribute is an RFC2822 formatted date string.
- **length:** (recommended) The length of the file in bytes. This attribute is a number.
- **type:** (recommended) The mime type of the file. This attribute is a string.
- **season:** (optional) If this attribute is present it specifies that this trailer is for a particular season number. This attribute is a number.

If the `season` attribute is present, it must be a number that matches the format of the `<podcast:season>` tag. So, for a podcast that has 3 published seasons, a new `<podcast:trailer season="4">` tag can be put in the channel to later be matched up with a `<podcast:season>4<podcast:season>` tag when it is published within a new `<item>`.

#### Examples

```xml
<podcast:trailer
        pubdate="Thu, 01 Apr 2021 08:00:00 EST"
        url="https://example.org/trailers/teaser"
        length="12345678"
        type="audio/mp3
">Coming April 1st, 2021</podcast:trailer>
```

```xml
<podcast:trailer
        pubdate="Thu, 01 Apr 2021 08:00:00 EST"
        url="https://example.org/trailers/season4teaser"
        length="12345678"
        type="video/mp4"
        season="4"
>Season 4: Race for the Whitehouse</podcast:trailer>

(later matches with)

<podcast:season name="Race for the Whitehouse">4</podcast:season>
```
