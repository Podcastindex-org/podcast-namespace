# Integrity

`<podcast:integrity>`

This element defines a method of verifying integrity of the media given either an [SRI-compliant integrity string](https://www.w3.org/TR/SRI/) (preferred) or a base64 encoded PGP signature. This element is optional within a [`<podcast:alternateEnclosure>`](alternate-enclosure.md) element. It allows to ensure that the file has not been tampered with.

### Parent

[`<podcast:alternateEnclosure>`](alternate-enclosure.md)

### Count

Single

### Attributes

- **type:** (required) Type of integrity, either "sri" or "pgp-signature".
- **value:** (required) Value of the sri string or base64 encoded pgp signature.

### Examples

```xml
<podcast:alternateEnclosure type="video/mp4" length="7924786" bitrate="511276.52" height="720">
    <podcast:source uri="https://example.com/file-720.mp4" />
    <podcast:source uri="ipfs://QmX33FYehk6ckGQ6g1D9D3FqZPix5JpKstKQKbaS8quUFb" />
    <podcast:integrity type="sri" value="sha384-ExVqijgYHm15PqQqdXfW95x+Rs6C+d6E/ICxyQOeFevnxNLR/wtJNrNYTjIysUBo" />
</podcast:alternateEnclosure>
```
