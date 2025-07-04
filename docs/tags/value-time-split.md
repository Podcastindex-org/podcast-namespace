# Value Time Split

`<podcast:valueTimeSplit>`

This element allows different value splits for a certain period of time. It is a combination of the concept of [`<podcast:soundbite>`](soundbite.md) and [`<podcast:remoteItem>`](remote-item.md) where a start time and a duration is supplied with alternative value recipients. The alternative value recipients are not required to be remote, as the recipients may not have an RSS feed/item of their own to reference.

The `<podcast:valueTimeSplit>` element allows time-based changes of value recipient information during playback of a feed's enclosure content.

This can either contain one or more [`<podcast:valueRecipient>`](value-recipient.md) tags _or_ exactly one [`<podcast:remoteItem>`](remote-item.md) tag. If a [`<podcast:remoteItem>`](remote-item.md) tag is present, the `remotePercentage` attribute can be defined to control how much the remote value block's [`<podcast:valueRecipient>`](value-recipient.md) tags will receive as a whole on top of the default, non-fee [`<podcast:valueRecipient>`](value-recipient.md) tags.

If the remote value block contains any `<podcast:valueTimeSplit>` tags, they should be ignored even if the `remoteStartTime` indicates it's in a position that would invoke them. When referencing a remote value block, only the root level block splits should be used and any `<podcast:valueTimeSplit>` children are to be ignored.

Fees from the default [`<podcast:valueRecipient>`](value-recipient.md) tags should remain to be calculated before anything is taken out from `<podcast:valueTimeSplit>`.

### Parent

`<podcast:value>`

### Count

Multiple

### Node Value

A single [`<podcast:remoteItem>`](remote-item.md) element OR one or more [`<podcast:valueRecipient>`](value-recipient.md) elements.

### Attributes

- `startTime` **(required)**: The time, in seconds, to stop using the currently active value recipient information and start using the value recipient information contained in this element.
- `duration` **(required)**: How many seconds the playback app should use this element's value recipient information before switching back to the value recipient information of the parent feed.
- `remoteStartTime` (optional): The time in the remote item where the value split begins. Allows the timestamp to be set correctly in value metadata. If not defined, defaults to 0.
- `remotePercentage` (optional): The percentage of the payment the remote recipients will receive if a [`<podcast:remoteItem>`](remote-item.md) is present. If not defined, defaults to 100. If the value is less than 0, 0 is assumed. If the value is greater than 100, 100 is assumed.

### Example (Remote Item)

```xml
<rss xmlns:podcast="https://github.com/Podcastindex-org/podcast-namespace/blob/main/docs/1.0.md" version="2.0">
   <channel>
      <title>Metal Showcase</title>
      <description>A great playlist of my favorite metal tracks.</description>
      <link>https://example.com/rss-metal-showcase.xml</link>
      <language>en</language>
      <pubDate>Fri, 21 Apr 2023 18:56:30 -0500</pubDate>
      <podcast:medium>music</podcast:medium>
      <item>
         <title>Special interview with Torcon VII</title>
         <otherTagsHere>...</otherTagsHere>
         <podcast:value type="lightning" method="keysend">
            <podcast:valueRecipient name="Alice (Podcaster)" type="node" address="02d5c1bf8b940dc9cadca86d1b0a3c37fbe39cee4c7e839e33bef9174531d27f52" split="95" />
            <podcast:valueRecipient name="Hosting Provider" type="node" address="03ae9f91a0cb8ff43840e3c322c4c61f019d8c1c3cea15a25cfc425ac605e61a4a" split="5" fee="true" />

            <podcast:valueTimeSplit startTime="60" duration="237" remotePercentage="95">
               <podcast:remoteItem itemGuid="https://podcastindex.org/podcast/4148683#1" feedGuid="a94f5cc9-8c58-55fc-91fe-a324087a655b" medium="music" />
            </podcast:valueTimeSplit>

            <podcast:valueTimeSplit startTime="330" duration="53" remoteStartTime="174" remotePercentage="95">
               <podcast:remoteItem itemGuid="https://podcastindex.org/podcast/4148683#3" feedGuid="a94f5cc9-8c58-55fc-91fe-a324087a655b" medium="music" />
            </podcast:valueTimeSplit>
         </podcast:value>
      </item>
   </channel>
</rss>
```

### Example (Locally Specified)

```xml
<rss xmlns:podcast="https://github.com/Podcastindex-org/podcast-namespace/blob/main/docs/1.0.md" version="2.0">
   <channel>
      <title>Cool Pod</title>
      <description>This is a cool pod</description>
      <link>https://example.com/rss-cool-pod.xml</link>
      <language>en</language>
      <pubDate>Fri, 21 Apr 2023 18:56:30 -0500</pubDate>
      <podcast:medium>podcast</podcast:medium>
      <item>
         <title>Adam Hates the word "pod" (and I do, too)</title>
         <otherTagsHere>...</otherTagsHere>
         <podcast:value type="lightning" method="keysend">
            <podcast:valueRecipient name="Alice (Podcaster)" type="node" address="02d5c1bf8b940dc9cadca86d1b0a3c37fbe39cee4c7e839e33bef9174531d27f52" split="95" />
            <podcast:valueRecipient name="Hosting Provider" type="node" address="03ae9f91a0cb8ff43840e3c322c4c61f019d8c1c3cea15a25cfc425ac605e61a4a" split="5" fee="true" />

            <podcast:valueTimeSplit startTime="63" duration="388">
               <podcast:valueRecipient name="Alice (Podcaster)" type="node" address="02d5c1bf8b940dc9cadca86d1b0a3c37fbe39cee4c7e839e33bef9174531d27f52" split="85" />
               <podcast:valueRecipient name="Jimbob (Guest)" type="node" address="02dd306e68c46681aa21d88a436fb35355a8579dd30201581cefa17cb179fc4c15" split="10" />
            </podcast:valueTimeSplit>

            <podcast:valueTimeSplit startTime="367" duration="246">
               <podcast:valueRecipient name="Alice (Podcaster)" type="node" address="02d5c1bf8b940dc9cadca86d1b0a3c37fbe39cee4c7e839e33bef9174531d27f52" split="85" />
               <podcast:valueRecipient name="Bobjim (Guest)" type="node" address="032f4ffbbafffbe51726ad3c164a3d0d37ec27bc67b29a159b0f49ae8ac21b8508" split="10" />
            </podcast:valueTimeSplit>
         </podcast:value>
      </item>
   </channel>
</rss>
```
