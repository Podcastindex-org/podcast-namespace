# Chat

`<podcast:chat>`

This element allows a podcaster to attach information to either the `<channel>`, `<item>` or `<podcast:liveItem>` about where the "official" chat for either the podcast or a specific episode or live event is to be found. Just like `<podcast:socialInteract>` functions for social media, the `<podcast:chat>` tag will function for ephemeral chat. There are many protocols in use across the internet for chat based communication. This tag is meant to be flexible enough to adapt to whichever protocol the podcaster wants to use.

This element's presence at a particular level governs how it should be treated. If found at the `<item>` or `<podcast:liveItem>` level, this should be treated as the information for that specific episode, overriding what is at the `<channel>` level. If this tag is found at the `<channel>` level, it would be considered the chat for the entire podcast and is recommended to be an "always on" chat room experience.

If a podcast has an "always on" style chat service it is recommended to link that at the `<channel>` level and do not use the `<podcast:chat>` tag at the `<item>` or `<podcast:liveItem>` level.

### Parent

`<channel>` or `<item>` or `<podcast:liveItem>`

### Count

Single

### Attributes

- `server` (required) The fqdn of a chat server that serves as the "bootstrap" server to connect to.
- `protocol` (required) The [protocol](../../chatprotocols.txt) in use on the server.
- `accountId` (recommended) The account id of the podcaster on the server or platform being connected to.
- `space` (optional) Some chat systems have a notion of a chat "space" or "room" or "topic". This attribute will serve that purpose.

### Example (IRC):

```xml
<podcast:chat
    server="irc.zeronode.net"
    protocol="irc"
    accountId="@jsmith"
    space="#myawesomepodcast"
/>
```

### Example (XMPP):

```xml
<podcast:chat
    server="jabber.example.com"
    protocol="xmpp"
    accountId="jsmith@jabber.example.org"
    space="myawesomepodcast@jabber.example.org"
/>
```

### Example (Nostr):

```xml
<podcast:chat
    server="relay.example.com"
    protocol="nostr"
    accountId="npub1pvdw7mm7k20t9dn9gful8n5xua5yv8rmgd9wul88qq5dxj80lpxqd39r3u"
    space="#myawesomepodcast_episode129"
/>
```

### Example (Matrix):

```xml
<podcast:chat
    server="example.com"
    protocol="matrix"
    accountId="@bob:example.com"
    space="#general:example.com"
/>
```
