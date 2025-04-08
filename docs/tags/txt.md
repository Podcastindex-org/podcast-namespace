# Txt

`<podcast:txt>`

This element holds free-form text and is modeled after the DNS "[TXT](https://en.wikipedia.org/wiki/TXT_record)" record. It's meant to allow for usages that might be niche or otherwise not rise to the level of needing a dedicated tag. Just like TXT records in DNS allowed for new things like [SPF](https://en.wikipedia.org/wiki/Sender_Policy_Framework#Implementation) to evolve, this tag can allow novel techniques to be created and sandboxed without a formalization process.

### Parent

`<channel>` or `<item>`

### Count

Multiple

### Attributes

- **purpose** (optional) A service specific string that will be used to denote what purpose this tag serves. This could be something like "example.com" if it's a third party hosting platform needing to insert this data, or something like "verify", "release" or any other free form bit of info that is useful to the end consumer that needs it. The free form nature of this tag requires that this attribute is also free formed. This value should not exceed `128 characters`.

### Purposes

The following are a list of strings known to be in common use. This list is in no way exhaustive. As new purposes come into common use, this list will be updated by the community to reflect that.

- `verify` - The node value is expected to contain a string that is given by a third party platform to a podcaster in order to prove that they are the owner of the feed and are in control of it. This is meant to replace the need for emails to exist in feeds. See example section below.

### Node value

This is a free form string. Please do not exceed `4000 characters` for the node value or it may be truncated by aggregators.

### Examples

```xml
<podcast:txt>naj3eEZaWVVY9a38uhX8FekACyhtqP4JN</podcast:txt>
```

```xml
<podcast:txt purpose="verify">S6lpp-7ZCn8-dZfGc-OoyaG</podcast:txt>
```

```xml
<podcast:txt purpose="release">2022-10-26T04:45:30.742Z</podcast:txt>
```
