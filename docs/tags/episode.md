# Episode

`<podcast:episode>`

This element exists largely for compatibility with the [`<podcast:season>`](season.md) tag. But, it also allows for a similar idea to what "name" functions as in that element.

### Parent

`<item>`

### Count

Single

### Node Value

The node value is a decimal number. It is required.

### Attributes

- **display:** (optional) - If this attribute is present, podcast apps and aggregators are encouraged to show its value instead of the purely numerical node value. This attribute is a string.

The episode numbers are decimal, so numbering such as `100.5` is acceptable if there was a special mini-episode published between two other episodes. In that scenario, the number would help with proper chronological sorting, while the `display` attribute could specify an alternate special "number" (a moniker) to display for the episode in a podcast player app UI.

Please do not exceed `32 characters` for the display attribute.

### Examples

```xml
<podcast:episode>3</podcast:episode>
```

```xml
<podcast:episode>315.5</podcast:episode>
```

```xml
<podcast:episode display="Ch.3">204</podcast:episode>
```

```xml
<podcast:episode display="Day 5">9</podcast:episode>
```
