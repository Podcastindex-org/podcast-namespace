# Images <small>(Deprecated)</small>

`<podcast:images>`

*Important: This tag is now deprecated.  The new [podcast:image](image.md) tag should be used instead*

This tag, when present, allows for specifying many different image sizes in a compact way at either the episode or channel level. The syntax is borrowed from the HTML5 [srcset](https://html.spec.whatwg.org/multipage/images.html#srcset-attributes) syntax. It allows for describing multiple image sources with width and pixel hints directly in the attribute. Although the HTML5 `srcset` attribute allows relative urls, absolute urls are required in this tag - since the feed url may not represent an appropriate base url for relativization.

### Parent

`<channel>` or `<item>`

### Count

Single

### Attributes

- **srcset** (required) A string that denotes each image url followed by a space and the pixel width, with each one separated by a comma. See the example for a clear view of the syntax.

### Examples

Example of specifying four different image sizes:

```xml
<podcast:images
    srcset="https://example.com/images/ep1/pci_avatar-massive.jpg 1500w,
            https://example.com/images/ep1/pci_avatar-middle.jpg 600w,
            https://example.com/images/ep1/pci_avatar-small.jpg 300w,
            https://example.com/images/ep1/pci_avatar-tiny.jpg 150w"
/>
```
