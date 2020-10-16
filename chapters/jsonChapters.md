This is the initial spec for a json chapter format that can be referenced in an RSS feed using the "podcast" namespace.namespace

The "chapters" property is an array of "chapter" objects taking this form:

```
{
    "marker": 94,
    "title": "Donation Segment",
    "img": "http://example.com/podcast/episode/chapter_art2.jpg",
    "url": "http://example.com/link/to/funding_platform"
}
```

Where "marker" (int - required) is the time in seconds, "title" (string - optional) is the chapter title, "img" (string - optional) is a link to an image to use as chapter art and "url" (string - optional) is a link to a
supporting document like would be in shownotes.