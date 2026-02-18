# Block

`<podcast:block>`

This element allows a podcaster to express which platforms are allowed to publicly display this feed and its contents. In its basic form, it is a direct drop-in replacement for the `<itunes:block>` tag, but allows for greater flexibility by the inclusion of the `id` attribute and by including multiple copies of itself in the feed.

Platforms should not ingest a feed for public display/use if their slug exists in the `id` of a `yes` block tag, or if an unbounded `yes` block tag exists (meaning block all public ingestion). Conversely, if a platform finds their slug in the `id` of a `no` block tag, they are free to ingest that feed for public display/usage.

In plain language, the sequence of discovery an ingesting platform should use is as follows:

1. Does `<podcast:block id="[myslug]">no</podcast:block>` exist in this feed? Safe to ingest.
2. Does `<podcast:block id="[myslug]">yes</podcast:block>` exist in this feed? Do not ingest.
3. Does `<podcast:block>yes</podcast:block>` exist in this feed? Do not ingest.

### Parent

`<channel>`

### Count

Multiple

### Attributes

- **id** (optional) A single entry from the [service slug list](/serviceslugs.txt).

### Node value

The node value must be "yes" or "no".

### Examples

```xml
<!-- This means "block everything" -->
<podcast:block>yes</podcast:block>
```

```xml
<!-- This means "block nothing" (same as not present) -->
<podcast:block>no</podcast:block>
```

```xml
<!-- This means "block only google and amazon" -->
<podcast:block id="google">yes</podcast:block>
<podcast:block id="amazon">yes</podcast:block>
```

```xml
<!-- This means "block every platform _except_ google and amazon" -->
<podcast:block>yes</podcast:block>
<podcast:block id="google">no</podcast:block>
<podcast:block id="amazon">no</podcast:block>
```
