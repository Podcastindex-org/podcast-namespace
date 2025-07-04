# Image

`<podcast:image>`

This tag allows for the delivering of images of various sizes and use cases.  It is cross-compatible with the `<itunes:image>` tag but expands the use cases for delivering more than just square channel or episode art.

### Parent
`<channel>` or `<item>` or `<podcast:liveItem>`

### Count
Multiple

### Attributes
- href (required): The URL to the media you want to embed.
- alt (recommended): A clear and concise accessibility focused text replacement for the image’s content.
- aspect-ratio (recommended): A ratio value such as 1/1, 16/9, 4/1 following the equivalent [CSS syntax](https://developer.mozilla.org/en-US/docs/Web/CSS/aspect-ratio). This allows podcasters to provide multiple art directions for the same media.
- width (recommended): The width of the asset in pixels.
- height (optional): The height of the asset in pixels.
- type (optional): Mime type of the media such as image/jpeg or video/mp4.
- purpose (optional): An unordered set of case-insensative, space-separated tokens following the equivalent [W3C syntax](https://html.spec.whatwg.org/multipage/common-microsyntaxes.html#space-separated-tokens). This value should not exceed 128 characters. This allows podcasters to indicate the suggested uses of this media.

### Notes
The only required attribute is `href`, specifying the url of the asset being delivered.  If no further attributes are defined, such as `aspect-ratio` or `purpose`, the image is assumed to be square (1:1).  For accessibility, `alt` is also encouraged.

The `aspect-ratio` and `width` are also encouraged, as they give app developers important information about how they can display your image within their UI.  A `height` attribute is also available, but not critical if `aspect-ratio` and `width` are provided.

The `type` attribute is available for use if the asset being delivered is not a typical image format (JPG/PNG/etc).  For instance, if it's a video file, or something a-typical like a WEBP or TIFF image.

The `purpose` type is encouraged to provide an expected use case to app developers as a display hint.  Why was this `<podcast:image>` included in the feed?  What did the podcast creator intend to be done with it?  Is a particular app or platform media asset being targeted?  See a fuller explanation of the `purpose` attribute below.

### Purpose Tokens
The purpose attribute gives podcasters the flexibility to indicate how specific images should be used by podcast apps and other platforms. This attribute accepts a space-separated list of tokens, where each token corresponds to a potential use case or context. While podcast apps may choose whether to honor these tokens, they allow podcasters to signal intent for how their images should be utilized across different platforms and contexts.

Anyone may define a token and its requirements. For instance, an app might define a token like `truefans/hero` to specify an image that fits their hero banner layout, providing guidelines for aspect ratios, resolution, safe areas, or text presence. A podcaster following these guidelines would include the `truefans/hero` token in the purpose attribute for any media following those guidelines. Here are some suggestions for generic tokens that multiple apps might adopt:
| Token | Guidelines | Prior Art |
|-|-|-|
| artwork | Recommended for representing the show/episode. Should contain your show name and key art. | [Apple’s show artwork](https://podcasters.apple.com/support/896-artwork-requirements#shows) |
| social | Recommended for social preview images. Should contain a landscape version of your artwork. | [Meta’s OG:image](https://developers.facebook.com/docs/sharing/webmasters/images/) |
| canvas | Recommended for immersive Now Playing screens. Should contain your artwork and expect to be overlaid with UI elements. | [Apple’s Full Page Show Art](https://podcasters.apple.com/support/866-promotional-artwork#show-tall)<br>[Spotify’s Canvas](https://support.spotify.com/us/artists/article/canvas-guidelines/) |
| cover | Recommended for hero to complement your artwork | [YouTube’s channel banner](https://support.google.com/youtube/answer/12950272?hl=en) |
| publisher | Recommended for publisher logos. Should be legible at small sizes. | [Apple’s Channel Icon](https://podcasters.apple.com/support/896-artwork-requirements#channels) |
| circular | Recommended for times when an image is expected to be cropped to a full circle. This should be a "circle safe" image. |

### Demo Tool
In addition to the examples below, a [demo tool](https://nathangathright.github.io/podcastimage/) is provided to help get started with a few common image tag scenarios.  The tool is provided by Nathan Gathright. 

## Examples

### The most basic use cases
Standard square image.  Assumed to be 1/1 aspect ratio intended for either podcast art or episode art, depending on if it's in the `<channel>` or `<item>`.
```xml
<podcast:image href="https://example.com/images/pci_square-massive.jpg" />
```

### Using `alt` to describe the image
```xml
<podcast:image
  alt="An antenna emanating signal waves"
  href="https://example.com/images/ep1/pci_square-massive.jpg"
/>
```

### Using `type` to provide a video file
```xml
<podcast:image
  type="video/mp4"
  href="https://example.com/images/ep1/pci_square-massive.mp4"
/>
```

### Using `width` and `height` instead of giving an aspect ratio
```xml
<podcast:image  
  width="1200"
  height="400"
  href="https://example.com/images/ep1/pci_portrait.mp4"
/>
```

### Using `aspect-ratio`
```xml
<podcast:image
  aspect-ratio="1/1"
  href="https://example.com/images/ep1/pci_square.jpg"
/>
<podcast:image
  aspect-ratio="16/9"
  href="https://example.com/images/ep1/pci_landscape.jpg"
/>
<podcast:image
  aspect-ratio="9/16"
  width="1200"
  href="https://example.com/images/ep1/pci_portrait.mp4"
/>
```

### Using `purpose`
```xml
<podcast:image
  purpose="artwork"
  href="https://example.com/images/today_explained.jpg"
/>
<podcast:image
  purpose="publisher"
  href="https://example.com/images/vox_media.jpg"
/>
```

### Using `purpose` to target a specific platform (ex. [Apple](https://podcasters.apple.com/support/5522-show-hero-template))
```xml
<podcast:image
  purpose="apple/showcase-hero"
  href="https://example.com/images/today_explained.jpg"
/>
```

### Using `aspect-ratio` and `purpose` to provide multiple art directions
Multiple image tags in the feed allow apps to select from multiple aspect ratios and media formats to display.
```xml
<podcast:image
  purpose="artwork"
  aspect-ratio="1/1"
  href="https://example.com/images/ep1/pci_square-massive.jpg"
/>
<podcast:image
  purpose="artwork social"
  aspect-ratio="16/9"
  href="https://example.com/images/ep1/pci_landscape-massive.jpg"
/>
```

### More complex examples
```xml
<podcast:image
  alt="An antenna emanating signal waves"
  purpose="artwork"
  type="image/jpeg"
  aspect-ratio="1/1"
  href="https://example.com/images/ep1/pci_square-massive.jpg"
/>
<podcast:image
  alt="An antenna emanating signal waves"
  purpose="artwork social"
  type="image/jpeg"
  aspect-ratio="16/9"
  href="https://example.com/images/ep1/pci_landscape-massive.jpg"
/>
<podcast:image
  alt="An antenna emanating signal waves"
  purpose="artwork canvas"
  type="image/jpeg"
  aspect-ratio="9/16"
  href="https://example.com/images/ep1/pci_portrait-massive.jpg"
/>
<podcast:image
  alt="An antenna emanating signal waves"
  purpose="artwork"
  type="video/mp4"
  aspect-ratio="1/1"
  href="https://example.com/images/ep1/pci_square-massive.mp4"
/>
<podcast:image
  alt="An antenna emanating signal waves"
  purpose="artwork social"
  type="video/mp4"
  aspect-ratio="16/9"
  href="https://example.com/images/ep1/pci_landscape-massive.mp4"
/>
<podcast:image
  alt="An antenna emanating signal waves"
  purpose="artwork canvas"
  type="video/mp4"
  aspect-ratio="9/16"
  href="https://example.com/images/ep1/pci_portrait-massive.mp4"
/>
```

### Attribution

* [Nathan Gathright](https://nathangathright.com/) - <small>Author</small>
* [James Cridland](https://james.cridland.net/) - <small>Contributor</small>
* [Daniel J. Lewis](https://danieljlewis.com/) - <small>Contributor</small>
* [Dave Jones](https://davejones.blog) - <small>Contributor</small>