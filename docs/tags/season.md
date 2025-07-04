# Season

`<podcast:season>`

This element allows for identifying which episodes in a podcast are part of a particular "season", with an optional season name attached.

### Parent

`<item>`

### Count

Single

### Node Value

The node value is an integer, and represents the season "number". It is required.

### Attributes

- `name` (optional): This is the "name" of the season. If this attribute is present, applications are free to **not** show the season number to the end user, and may use it simply for chronological sorting and grouping purposes.

Please do not exceed `128 characters` for the name attribute.

### Examples

```xml
<podcast:season>5</podcast:season>
```

```xml
<podcast:season name="Race for the Whitehouse 2020">3</podcast:season>
```

```xml
<podcast:season name="Egyptology: The 19th Century">1</podcast:season>
```

```xml
<podcast:season name="The Yearling - Chapter 3">3</podcast:season>
```
