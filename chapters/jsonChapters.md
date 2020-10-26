This is the initial spec for a json chapter format that can be referenced in an RSS feed using the "podcast" namespace.

This type of file should be served with a type of 'application/audio-chapters+json';

The "chapters" property is an array of "chapter" objects which take this basic form:

```
{
    "startTime": 94,
    "title": "Donation Segment"
}
```

Chapter Object Attributes:

 - `startTime` (required - float) The time, expressed in seconds with float precision for fractions of a second.
 - `title` (required - string) The title of this chapter.
 - `img` (optional - string) The url of an image to use as chapter art.
 - `url` (optional - string) The url of a web page or supporting document that's related to the topic of this chapter.
 - `id` (optional - string) Can be useful for software that expects an "ID" value for a chapter.  For example, "ch1",
                            "ch2", etc.
 - `toc` (optional - boolean) If this property is present and set to false, this chapter should not display visibly to
                              the user in either the table of contents or as a jump-to point in the user interface.  It
                              should be considered a "silent" chapter marker for the purpose of meta-data only.  If this
                              property is set to `true` or not present at all, this should be considered a normal chapter
                              for display to the user.  The name "toc" is short for "table of contents".
 - `hasArtwork` (optional - string) This is an alternate way to specify an image for a chapter, as a relative url to the
                                    current base.  If this property is present, it's value is considered the file name of
                                    the image file relative to the current url.  For example, if the chapters.json file
                                    was located at "example.com/podcast/chapters.json", and a value of "ch1.jpg" was given
                                    for this property, the file "example.com/podcast/ch1.jpg" would be expected to exist.



Other attributes at the parent level:

 - `author` (optional - string) The name of the author of this podcast episode.
 - `title` (optional - string) The title of this podcast episode.
 - `podcastName` (optional - string) The name of the podcast this episode belongs to.
 - `description` (optional - string) A description of this episode.
 - `fileName` (optional - string) The name of the audio file these chapters apply to.


## Basic example

Here is what a very basic chapters file may look like:

```
{
    "version": "1.0.0",
    "chapters":
    [
        {
            "startTime": 0,
            "title": "Intro"
        },
        {
            "startTime": 168,
            "title": "Hearing Aids",
            "img": "https://example.com/images/hearing_aids.jpg"
        },
        {
            "startTime": 260,
            "title": "Progress Report"
        },
        {
            "startTime": 410,
            "title": "Namespace",
            "img": "https://example.com/images/namepsace_example.jpg",
            "url": "https://github.com/Podcastindex-org/podcast-namespace"
        },
        {
            "startTime": 3990,
            "title": "Just Break Up",
            "img": "https://example.com/images/justbreakuppod.png"
        },
        {
            "startTime": 5510,
            "title": "The Big Players"
        },
        {
            "startTime": 5854,
            "title": "Spread the Word"
        },
        {
            "startTime": 6089,
            "title": "Outro"
        }
    ]
}
```

In this simple form, the chapter objects have no ID's and are treated as sequential just based on their index as a function of being
ordered in ascending fashing based on their `startTime`.  In a very simple example such as this nothing is present except the `startTime`,
`title`, and a few bits of optional meta-data.

<br><br>

## More complex example

In this more robust example, we can bring in more meta-data about the podcast episode, and provide a way for this file to exist more easily in a web
context for something like an embedded HTML5 player on a website:

```
{
    "version": "1.0.0",
    "author": "John Doe",
    "title": "Episode 7 - Making Progress",
    "podcastName": "John's Awesome Podcast",
    "chapters":
    [
        {
            "startTime": 0,
            "title": "Intro",
            "id": "ch0"
        },
        {
            "startTime": 168,
            "title": "Hearing Aids",
            "id": "ch1",
            "hasArtwork": "chapter1.jpg"
        },
        {
            "startTime": 260,
            "title": "Progress Report",
            "id": "ch2"
        },
        {
            "startTime": 410,
            "title": "Namespace",
            "img": "https://example.com/images/namepsace_example.jpg",
            "url": "https://github.com/Podcastindex-org/podcast-namespace",
            "id": "ch3"
        },
        {
            "startTime": 3990,
            "title": "Just Break Up",
            "img": "https://example.com/images/justbreakuppod.png",
            "url": "https://twitter.com/justbreakuppod",
            "id": "ch4",
            "hasArtwork": false
        },
        {
            "startTime": 4826,
            "title": "Boston Harbor",
            "img": "https://example.com/images/bostonharbor.jpg",
            "id": "ch5",
            "toc": false
        },
        {
            "startTime": 5510,
            "title": "The Big Players",
            "id": "ch6"
        },
        {
            "startTime": 5854,
            "title": "Spread the Word",
            "id": "ch7",
            "hasArtwork": "chapter6.jpg"
        },
        {
            "startTime": 6089,
            "title": "Outro",
            "id": "ch8"
        }
    ]
}
```