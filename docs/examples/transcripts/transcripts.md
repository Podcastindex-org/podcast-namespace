## Transcript File Format Details

This is the initial spec for the podcast transcript format. There are four possible formats detailed below.

Some transcript implementations are done in-browser. [CORS headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)
are required to make these files available from other websites. [A CORS tester is available here](https://cors-test.codehappy.dev/),
to ensure that transcripts are available within browser-based players.

- **Want to support only one format?** WebVTT is used by Apple Podcasts for ingest, and also natively supported by web browsers. Because the WebVTT format is the most flexible, it's an ideal choice if you can only support one format.

The examples given below are just for convenience. In production you should ensure you are conforming to the actual spec for each format as defined in its own documentation.

## WebVTT

The [Web Video Text Tracks Format (WebVTT)](https://www.w3.org/TR/webvtt1/) is designed for use in HTML on the web. You can use the [<track> element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/track) in your own web-based players to make closed-captions appear on a web-page.

A VTT file contains medium-fidelity timestamps. It differs from the SRT format (below) because you can optionally add speaker names, including them in a voice span tag `<v>` at the beginning of each caption when they change, as in the snippet below. Apple Podcasts supports these speaker names, and will ingest them into its transcript tool.

While there is no defined maximum line-length, to ensure that displaying WebVTT as closed-captions can work well, a maximum line-length of 65 characters is recommended. If you're using whisper-cpp or equivalent, `--split-on-word --max-len 65` would be a method of achieving this.

The full specification includes formatting features; these are typically not used in podcasting applications.

#### Snippet:

```vtt
WEBVTT

00:00:00.000 --> 00:00:05.000
<v John>Podcasting 2.0 is really changing the game.

00:00:05.000 --> 00:00:10.000
<v Tom>Yeah, absolutely. The new features are incredible.

00:00:10.000 --> 00:00:15.000
It's amazing how it's empowering creators like never before.

00:00:15.000 --> 00:00:20.000
And the enhanced monetization options are a game-changer.

00:00:20.000 --> 00:00:25.000
<v John>Exactly, Tom. It's revolutionizing the industry.

00:00:25.000 --> 00:00:30.000
<v Tom>No doubt about it. Podcasting 2.0 is the future.

00:00:30.000 --> 00:00:35.000
<v John>Couldn't agree more, Tom. The future looks bright.
```

Example file: [example.vtt](example.vtt)

#### Web browser support example

This example code will add an audio player on a web page, and display the accompanying WebVTT file as the audio plays. (Note that this basic code will not show speaker names).

```html
<div style="height:111px;text-align:center;">
  <audio id="vttplayer" controls preroll="none" src="https://podnews.net/audio/podnews240125.mp3?_from=P20spec">
  <track default src="https://podnews.net/audio/podnews240125.mp3.vtt">
  </track>
 </audio>
 <br>
 <div style="text-align:center;" id="vtt">
 </div>
</div>

<script>
document.getElementById('vttplayer').textTracks[0].addEventListener('cuechange', function() {
  document.getElementById('vtt').innerText = this.activeCues[0].text;
});
</script>
```

## SRT

The SRT format was designed for video captions but provides a suitable solution for podcast transcripts. The SRT format contains medium-fidelity timestamps and are a
popular export option from transcription services. An SRT file can be generated programmatically from a VTT file (and vice-versa).

SRT transcripts used for podcasts should adhere to the following specifications:

#### Properties:

- Max number of lines: 2
- Max characters per line: 32
- Speaker names (optional): Start a new card when the speaker changes. Include the speaker's name, followed by a colon.

#### Snippet:

```srt
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

Example file: [example.srt](example.srt)

## JSON

The JSON representation is a flexible format that accomodates various degrees of fidelity in a concise way. At the most precise, it enables word-by-word highlighting. This format for podcast transcripts should adhere to the following specifications.

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
      "endTime": 2.5,
      "body": "father.\n"
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

Example file: [example.json](example.json)

## HTML

The HTML transcript format provides a solution when a transcript is available but no or limited timecode data is available. HTML transcript files are considered low-fidelity and are designed to serve as an accessibility aid and provide searchable episode content. The HTML format used for podcast transcripts should adhere to the following specifications.

#### HTML tags used:

- `<cite>`: Name of the speaker (if available)
- `<time>`: Start time of monologue (if available)
- `<p>`: Content of monologue

#### Snippet:

```html
<cite>Kevin:</cite>
<time>0:00</time>
<p>
  We have an update planned where we would like to give the ability to upload an
  artwork file for these videos
</p>
<cite>Alban :</cite>
<time>0:09</time>
<p>You're triggering Tom right now with a hey, here's a cool feature.</p>
```

Example file: [example.html](example.html)
