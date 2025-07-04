# Locked

`<podcast:locked>`

This tag may be set to `yes` or `no`. The purpose is to tell other podcast hosting platforms whether they are allowed to import this feed. A value of `yes` means that any attempt to import this feed into a new platform should be rejected.

### Parent

`<channel>`

### Count

Single

### Node value

The node value must be "yes" or "no".

### Attributes

- `owner` (optional): The owner attribute is an email address that can be used to verify ownership of this feed during move and import operations. This could be a public email or a virtual email address at the hosting provider that redirects to the owner's true email address.

### Examples

```xml
<podcast:locked>yes</podcast:locked>
```

```xml
<podcast:locked owner="email@example.com">no</podcast:locked>
```
