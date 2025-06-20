# License

`<podcast:license>`

This element defines a license that is applied to the audio/video content of a single episode or the audio/video of the podcast as a whole. Custom licenses must always include a url attribute. Implementors are encouraged to read the license tag companion [document](../examples/license/license.md) for a more complete picture of what this tag is intended to accomplish.

### Parent

`<channel>` or `<item>`

### Count

Single

### Node Value

The node value must be a lower-cased reference to a license "identifier" defined in the companion [license list](../examples/license/licenses.json) file if the license being used is a well-known, public license. Or, if it is a custom license, it must be a free-form abbreviation of the name of the license as you reference it publicly. Please do not exceed `128 characters` for the node value, or it may be truncated by aggregators.

### Attributes

- **url:** (optional) This is a URL that points to the full, legal language of the license being referenced. This attribute is optional for well-known public licenses. For new or custom licenses, it is required.

### Examples

```xml
<podcast:license>CC-BY-NC-ND-4.0</podcast:license>
```

```xml
<podcast:license url="https://example.org/mypodcastlicense/full.pdf">my-podcast-license-v1</podcast:license>
```
