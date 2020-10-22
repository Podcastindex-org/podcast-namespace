This is the initial spec for a json chapter format that can be referenced in an RSS feed using the "podcast" namespace.

This type of file should be served with a type of 'application/audio-chapters+json';

The "chapters" property is an array of "chapter" objects taking this form:

```
{
    "startTime": 94,
    "title": "Donation Segment",
    "img": "http://example.com/podcast/episode/chapter_art2.jpg",
    "url": "http://example.com/link/to/funding_platform"
}
```

Attributes:

 - `startTime` (required - float) The time, expressed in seconds with float precision for fractions of a second.
 - `title` (required - string) The title of this chapter.
 - `img` (optional - string) The url of an image to use as chapter art.
 - `url` (optional - string) The url of a web page or supporting document that's related to the topic of this chapter.
