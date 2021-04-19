## Transcript File Format Details

This is the initial spec for the podcast transcript format.  There are three possible formats detailed below.


<br><br>


## HTML

The HTML transcript format provides a solution when a transcript is available but no or limited timecode data is available. HTML transcript files are considered low-fidelity and are
designed to serve as an accessibility aid and provide searchable episode content. The HTML format used for podcast transcripts should adhere to the following specifications.

#### HTML tags used:
- `<cite>`: Name of the speaker (if available)
- `<time>`: Start time of monologue (if available)
- `<p>`: Content of monologue

#### Snippet:
```html
<cite>Kevin:</cite>
<time>0:00</time>
<p>We have an update planned where we would like to give the ability to upload an artwork file for these videos</p>
<cite>Alban :</cite>
<time>0:09</time>
<p>You're triggering Tom right now with a hey, here's a cool feature.</p>
```

Example file:  [example.html](example.html)

<br><br>


## JSON

The JSON representation is a flexible format that accomodates various degrees of fidelity in a concise way. This format for podcast transcripts should adhere to the following specifications.

#### Elements included in this representation:
- `<version>`: The version of JSON transcript specification
- `<segments>`: An array of dialogue elements (segments)
- `<speaker>`: Speaker
- `<startTime>`: Start time for the segment
- `<endTime>`: End time for the segment (if available)
- `<body>`: Dialogue content

#### Snippet:
```json
{
  "version": "1.0.0",
  "segments": [
    {
      "speaker": "Darth Vader",
      "startTime": 0.5,
      "endTime": 0.75,
      "body": "I"
    },
    {
      "speaker": "Darth Vader",
      "startTime": 1,
      "endTime": 1.25,
      "body": "am"
    },
    {
      "speaker": "Darth Vader",
      "startTime": 1.5,
      "endTime": 2.0,
      "body": "your"
    },
    {
      "speaker": "Darth Vader",
      "startTime": 2.25,
      "endTime": 2.50,
      "body": "father."
    },
    {
      "speaker": "Luke",
      "startTime": 2.75,
      "endTime": 3.0,
      "body": "Nooooo"
    }
  ]
}
```

Example file:  [example.json](example.json)

<br><br>


## SRT

The SRT format was designed for video captions but provides a suitable solution for podcast transcripts. The SRT format contains medium-fidelity timestamps and are a
popular export option from transcription services. SRT transcripts used for podcasts should adhere to the following specifications.

#### Properties:
- Max number of lines: 2
- Max characters per line: 32
- Speaker names (optional): Start a new card when the speaker changes. Include the speaker's name, followed by a colon.

#### Snippet:
```
1
00:00:00,000 --> 00:00:02,760
Sarah: In today's episode,
you'll learn whether or not you

2
00:00:02,760 --> 00:00:06,090
should have a podcast trailer.
And if so, what should you

3
00:00:06,090 --> 00:00:11,610
include in one? Welcome to
Podcasting Q&A, where you learn

4
00:00:11,610 --> 00:00:15,750
the best tips and strategies to
launch, grow and monetize your

5
00:00:15,750 --> 00:00:18,630
podcast. This week's question
comes from Gillian.

6
00:00:19,080 --> 00:00:21,450
Gillian: Hi Buzzsprout, Gillian
here from breaking through

7
00:00:21,450 --> 00:00:25,350
careers podcast. My question is,
do we need a podcast trailer?
```

Example file:  [example.srt](example.srt)


## WebVTT

Web Video Text Tracks Format (WebVTT) are an alternative to SRT primarily designed for the use in HTML on the web.  It is supported in all major web browsers and is similar enough to SRT to be converted.

### Differences from SRT taken from [Wikipedia](https://en.wikipedia.org/wiki/WebVTT):
- WebVTT's first line starts with WEBVTT after the optional UTF-8 byte order mark
- There is space for optional header data between the first line and the first cue
- Timecode fractional values are separated by a full stop instead of a comma
- Timecode hours are optional
- The frame numbering/identification preceding the timecode is optional
- Comments identified by the word NOTE can be added
- Metadata information can be added in a JSON-style format
- Chapter information can be optionally specified
- Only supports extended characters as UTF-8
- CSS in a separate file defined in the companion HTML document for C tags is used instead of the FONT tag
- Cue settings allow the customization of cue positioning on the video

#### Properties:
- Max number of lines: 2
- Max characters per line: 32
- Speaker names (optional): Start a new card when the speaker changes. Include the speaker's name, followed by a colon.

#### Snippet:
```
WEBVTT

00:00:00.000 --> 00:00:02.760
Sarah: In today's episode,
you'll learn whether or not you

00:00:02.760 --> 00:00:06.090
should have a podcast trailer.
And if so, what should you

00:00:06.090 --> 00:00:11.610
include in one? Welcome to
Podcasting Q&A, where you learn

00:00:11.610 --> 00:00:15.750
the best tips and strategies to
launch, grow and monetize your

00:00:15.750 --> 00:00:18.630
podcast. This week's question
comes from Gillian.

00:00:19.080 --> 00:00:21.450
Gillian: Hi Buzzsprout, Gillian
here from breaking through

00:00:21.450 --> 00:00:25.350
careers podcast. My question is,
do we need a podcast trailer?
```

Example file:  [example.vtt](example.vtt)
