# Publisher

`<podcast:publisher>`

This element allows a podcast feed to link to it's "publisher feed" parent. This is useful when a parent publishing entity wants to attest ownership over all of the podcast feeds it owns/publishes. This element must contain exactly one `<podcast:remoteItem medium="publisher">` element pointing to the publisher feed. For widest compatibility, it is highly recommended to use the `feedUrl` attribute of the [`<podcast:remoteItem>`](remoteItem.md) element in this capacity.

For complete implementation details regarding publisher feeds and how to create them, please see the full publisher feed [documentation](../../publishers/publishers.md) and the `publisher` medium [here](./medium.md#medium).

### Parent

`<channel>`

### Count

Single

### Example:

```xml
<channel>
    <title><![CDATA[It's A Mood]]></title>
    <description>A value4value happenstance music show.</description>
    <link>https://example.org/itsamood</link>
    <generator>Sovereign Feeds</generator>
    <podcast:person href="https://example.org/mikeneumann" group="cast" role="host" img="https://example.org/mikeneumann/image.jpg">Mike Neumann</podcast:person>
    <podcast:guid>469b403f-db2d-574c-9db9-96dbb4f6561c</podcast:guid>
    <podcast:medium>podcast</podcast:medium>
    <podcast:publisher>
        <podcast:remoteItem medium="publisher" feedGuid="003af0a0-6a45-55cf-b765-68e3d349551a" feedUrl="https://agilesetmedia.com/assets/static/feeds/publisher.xml"/>
    </podcast:publisher>
    <item>
        <title><![CDATA[Runnin']]></title>
        <pubDate>Wed, 03 Apr 2024 02:06:28 +0000</pubDate>
        ...
    </item>
</channel>
```
