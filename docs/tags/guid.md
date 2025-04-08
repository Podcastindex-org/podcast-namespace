# Guid

`<podcast:guid>`

This element is used to declare a unique, global identifier for a podcast. The value is a UUIDv5, and is easily generated from the RSS feed url, with the **protocol scheme and trailing slashes stripped off**, combined with a unique "podcast" namespace which has a UUID of `ead4c236-bf58-58c6-a2c6-a6b28d128cb6`. Tools like [this one](https://www.uuidtools.com/v5) can help generate these values by hand. Or, language libraries like [this one](https://github.com/sporkmonger/uuidtools) in Ruby are widely available. Specifically for podcasts, [this tool from RSS Blue](https://tools.rssblue.com/podcast-guid) can help generate a GUID by hand.

A podcast gets assigned a podcast:guid once in its lifetime using its current feed url (at the time of assignment) as the seed value. That GUID is then meant to follow the podcast from then on, for the duration of its life, even if the feed url changes. This means that when a podcast moves from one hosting platform to another, its podcast:guid should be discovered by the new host and imported into the new platform for inclusion into the feed.

Using this pattern, podcasts can maintain a consistent identity across the open RSS ecosystem without a central authority.

**Tips:**

- All podcasts in the Podcast Index have already been assigned a GUID; but if one exists in the RSS feed, that value is canonical.
- You can programmatically spot a GUID: it is 36 characters long, and contains four hyphen characters.
- Be aware that Amazon Music also uses separate UUIDv5 identifiers within their podcast directory, which are calculated differently and unrelated to this specification.
- The following regular expression (regex) will match a GUID:

```re
[0-9a-fA-F]{8}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{12}`
```

### Parent

`<channel>`

### Count

Single

### Node Value

The node value is a UUIDv5 string.

### Examples

Example GUID for feed url `mp3s.nashownotes.com/pc20rss.xml`:

```xml
<podcast:guid>917393e3-1b1e-5cef-ace4-edaa54e1f810</podcast:guid>
```

Example GUID for feed url `podnews.net/rss`:

```xml
<podcast:guid>9b024349-ccf0-5f69-a609-6b82873eab3c</podcast:guid>
```

### Guid-enabled fast-follow share links

The `podcast:guid` value above enables podcasters to produce a link that can share a podcast on a variety of different platforms.

The format of the link is `https://(a podcast website link)#fastfollow-(type):(a podcast guid)`

`type` is currently `podcast`, but may be extended in future.

A working example is https://podnews.net/podcast/i8xe9/listen#fastfollow-podcast:9b024349-ccf0-5f69-a609-6b82873eab3c or the QR code given below.

![podnews-qr](https://user-images.githubusercontent.com/231941/127796108-d819de43-6c0e-4c7b-9579-ed1f19989443.png)

When scanned on a mobile phone's camera app, this link will go to the specified podcast website. Behavior of this website is up to the creator: some may use a default homepage, others may sniff the useragent and open a default podcast app on a device. In the working example, above, an iPhone user may be taken to Apple Podcasts; an Android user may be taken to Google Podcasts; and another device will be given a page with a player.

When scanned on a QR code reader inside a podcast app, like [CurioCaster](https://curiocaster.com/), the app can parse the `<podcast:guid>` value from the URL, allowing the podcast to be opened within the application.
