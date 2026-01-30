# The Shared SoundBites Specification

<small>Version 0.01 by John Chidgey - 2022.05.18</small>

<br>

## Purpose

SoundBites to date have been placed in the podcast source of truth: the RSS feed. Whether they remain embedded in the XML or converted into a referenced JSON file (as per PC2.0 Chapters) as a potential future evolution should be considered to reduce potential feed bloat/hosting bandwidth considerations. The podcaster controls and owns their RSS feed (or should) and as such controls the flow of value and approval of that feeds' content.

SoundBites remain a burden (however slight) to the podcaster to create and embed in their feed. Traditionally SoundBites have been shared via applications, encoding as separate audio files and attaching/posting to social media platforms, or via embedding them in a website and shared as a link. All existing methods do not allow the podcast creator to easily import them in their RSS feed, nor to reward those that created them. (Soundbiters)

To accomplish this, a standard SoundBite sharing format could permit sharing in a simple form that can be imported by the podcaster using local tools or easily added to hosting provider platforms, as well as played in any web/device app. Such a format should contain the existing elements of the SoundBite tag, but also reference the audio file and value address for V4V.

In this way the podcaster could present a new incentivisation pathway for V4V redistribution, with any playback of the SoundBite they incorporate into the RSS feed getting a set split of streaming value of an agreed fixed limit (per minute is not useful as clips vary in length and longer clips would incentivise poor behaviours) between the SoundBiter and the podcaster.

## Requirements

Audio only, Constant BitRate encoded audio file.

## Implementation Challenges

The onus on playhead position and duration for audio/video remains on the client application, and if the podcaster chooses a non-linear format (eg VBR encoding) then playhead position and duration can be difficult to determine. There are many video formats in use which could be problematic, hence restricting this to Audio/MPEG3 at Constant BitRate (CBR) is the best place to start as a requirement for use in this standard. It might make more sense (TBD) for the podcaster to specify the MP3 to use if Alternate enclosure is used - in some cases primary download references for statistics will be thrown out significantly if the same file is used for play/scrub to position for soundclips.

## SoundBite Tag Ammendments

Group soundbites under a podcast:soundbites element, with each soundbite being an individual element beneath that, with the option to use JSON (per the Chapters standard). With the introduction of the alternate enclosure tag and to allow editing without needing to query the RSS feed item directly, linking to a source audio file of truth will reduce ambiguity and guarantee timestamps are correctly aligned.

#### Parent
`<item>`

#### Count
Single

#### Attributes
 - **open (OPTIONAL)**: Default = TRUE. Boolean: If TRUE, open to accepting soundbite submissions for this item/episode. If FALSE, any submissions will be rejected.
 - **split (OPTIONAL):** The number of shares of the payment this recipient will receive, hence 50 = equal amount to podcaster and to soundbiter, 1 all to soundbiter, 0 all to podcaster.
 - **url (OPTIONAL):** The URL where the soundbites file is located.
 - **type (OPTIONAL):** Mime type of file - JSON preferred, 'application/json+soundbites'.

#### Examples
`<podcast:soundbites open="true" split="50" url="https://example.com/episode1/soundbites.json" type="application/json+soundbites" />`

## The episode is open for submissions, however none currently exist or have been accepted into the feed

`<podcast:soundbites open="true" split="50" />`

<br><br>

### Soundbites Element

The `valueRecipient` tag designates various destinations for payments to be sent to during consumption of the enclosed

#### Parent
`<podcast:soundbite>`

#### Count
Multiple

#### Attributes
 - **startTime:** (UNCHANGED) The time where the soundbite begins
 - **duration:** (UNCHANGED) How long is the soundbite (recommended between 15 and 120 seconds)
 - **title:** (Now required, was a node value now a named attribute) Used as free form string from the podcast creator to specify a title for the soundbite. Please do not exceed `128 characters` for the title value or it may be truncated by aggregators.
 - **url**: Source Audio file URL

 ## Value Attributes

 - **name (OPTIONAL):** A free-form string that designates who or what this recipient is.
 - **type (OPTIONAL/REQUIRED):** This is the service slug of the cryptocurrency or protocol layer hat represents the type of receiving address that will receive the payment.
 - **method (OPTIONAL):** This is the transport mechanism that will be used. (TBD: Was Keysend now blank in podcast:value?)
 - **address (OPTIONAL/REQUIRED):** This denotes the receiving address of the payee.
 - **customKey (OPTIONAL):** The name of a custom record key to send along with the payment.
 - **customValue (OPTIONAL):** A custom value to pass along with the payment. This is considered the value that belongs to the `customKey`.

#### Value notes:

The value tag attribute: 'split' is defined at the top level and should be equal for all soundBites in a given feed, which is set by the Podcaster. Both the Podcaster(s) and the soundBite(r) should be compensated for their effort and hence a 50/50 split (0.5) should be default.

The fee is a per play amount before the split and is derived from the podcast:value tags `suggested` attribute, applied over the recommended maximum duration of a soundbite (nominally 120 seconds). For a client to process a soundbite value split therefore, it is mandatory to have a podcast:value block defined as well.

  TBD 1: For Lightning, 1 sat/min streaming rate, 50/50 split is only 1 sat/soundbite which is too small to route. So we consider a 10x multipler by default to avoid this? How much do we value soundbiters - I think that's probably fair?)
  TBD 2: Which is the Podcaster true recipient? In a multi-host podcast there could be multiple therefore perhaps an split between all parties defined in the value block for this item.

There is nothing stopping a podcaster from changing this split after multiple soundBites have been created however those creating the soundBite that are motivated by this would observe no value from that effort and would stop contributing in future in that was the case.

Value attributes are all optional, however if they are to be used, optional/required indicates they are required if value is to be used. Some soundbiter(s) may be happy with name attribution, no attribution whatsoever and not have a streaming value destination available to them.

#### XML Examples

`<podcast:soundbites split="50" url="https://example.com/episode1/soundbites.json" type="application/json+soundbites" />`

```
<podcast:soundbites split="50">
  <podcast:soundbite
    startTime="1234.5"
    duration="42.25"
    title="Why the Podcast Namespace Matters"
    url="https://example.com/ashow/E001-anEpisode.mp3"
    name="A Soundbiter"
    type="node"
    address="02d5c1bf8b940dc9cadca86d1b0a3c37fbe39cee4c7e839e33bef9174531d27f52"
    customKey="[optional key to pass(mixed)]"
    customValue="[optional value to pass(mixed)]"
  />
  <podcast:soundbite
    startTime="134.5"
    duration="30.0"
    title="Why Soundbites Matter"
    url="https://example.com/ashow/E001-anEpisode.mp3"
    name="A Soundbiter again"
    type="node"
    address="02d5c1bf8b940dc9cadca86d1b0a3c37fbe39cee4c7e839e33bef9174531d27f52"
    customKey="[optional key to pass(mixed)]"
    customValue="[optional value to pass(mixed)]"
  />
</podcast:soundbites>
```

#### JSON Server File Example

```
{
	"version" : "1.0.0",
	"soundbites" :
	[
		{
      "startTime" : 1234.5,
      "duration" : 42.25,
      "title" : "Why the Podcast Namespace Matters",
      "url" : "https://example.com/ashow/E001-anEpisode.mp3",
      {
        "name" : "A Soundbiter",
        "type" : "node",
        "address" : "02d5c1bf8b940dc9cadca86d1b0a3c37fbe39cee4c7e839e33bef9174531d27f52",
        "customKey" : "[optional key to pass(mixed)]",
        "customValue" : "[optional value to pass(mixed)]"
      }
		},
    {
      "startTime" : 134.5,
      "duration" : 30.0,
      "title" : "Why Soundbites Matter",
      "url" : "https://example.com/ashow/E001-anEpisode.mp3",
      {
        "name" : "A Soundbiter again",
        "type" : "node",
        "address" : "02d5c1bf8b940dc9cadca86d1b0a3c37fbe39cee4c7e839e33bef9174531d27f52",
        "customKey" : "[optional key to pass(mixed)]",
        "customValue" : "[optional value to pass(mixed)]"
      }
    }
	]
}
```

## Soundbite Sharing Specification

Sharing a soundbite has to be easy for client or web applications to implement and for podcast hosts or podcast servers to ingest with tools to add/insert into the RSS/JSON for the source of truth RSS Feed.

Two methods are suggested for sharing: JSON File and a Query string URL. In either scenario, only one soundbite may be shared per file/string. Each soundbite will be parsed individually.

#### Sharing Attributes

When submitting a Soundbite to the podcaster, it is necessary to include both the RSS Feed URL and the Episode number to make it easier to correlate the entry with the podcasters show. It should be possible to reverse look-up based on the Audio file URL provided however this is kinder approach to those implementing this and it should be transparent to the Soundbite creator anyhow.

 - **feed**: The RSS Feed of the podcast the Soundbite is for
 - **episode**: The episode number of the podcast the Soundbite is for

#### JSON Sharing File Example

```
{
  "startTime" : 134.5,
  "duration" : 30.0,
  "title" : "Why Soundbites Matter",
  "url" : "https://example.com/ashow/E001-anEpisode.mp3",
  "feed" : "https://example.com/ashow/feed.xml",
  "episode" : 10,
  {
    "name" : "A Soundbiter again",
    "type" : "node",
    "address" : "02d5c1bf8b940dc9cadca86d1b0a3c37fbe39cee4c7e839e33bef9174531d27f52",
    "customKey" : "[optional key to pass(mixed)]",
    "customValue" : "[optional value to pass(mixed)]"
  }
}
```

#### Query String Formatting

String formatting shall comply with RFC3986.

#### Proposal for Web Library

Loads a audio from a RSS Feed Item, play to position, pause, start in flag, play to stop flag, scrape EMail from RSS Feed, send EMail from webpage.

#### GoHugo Template Converter JSON

Develop this and publish it

#### Server Component (Soundbitten)

Accepts API calls from a client, presents the soundbites for a podcaster to select from, download the ones you want to keep and name them in JSON file for inclusion/import into RSS feed.

#### Client Component (Soundbiter)

Sends API call to Soundbitten service.


Discussion here:
- https://github.com/Podcastindex-org/podcast-namespace/issues/205
- https://podcastindex.social/web/statuses/105833620038854052
