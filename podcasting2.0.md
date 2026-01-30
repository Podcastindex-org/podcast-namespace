# Podcasting 2.0

Podcasting 2.0 is a set of forward looking ideas combined with the technology to realize them. It's a vision for what the podcast listener experience can and should
be. That experience has stagnated for over a decade, with almost all of the improvements coming in isolated sections of the ecosystem. There hasn't been a single,
unified vision from the podcasting community acting together with one voice. So, we've ended up with fragments of innovation across the podcasting landscape with no
central driving goal in mind. Podcasting 2.0 is the expression of what that goal could be.

Stated eloquently, the aim of Podcasting 2.0 is this:

> "I think our focus should be 100% on improving the podcasting experience in an open-standard way that allows every player to innovate faster
> and better than any one company could do on their own. This is our best bet at avoiding one company emerging as the monopoly of podcasting."
>
> --Tom Rossi [Tom Rossi](https://podcastindex.social/@tomrossi7/105839063781381384)

Closed ecosystems can not innovate any better or faster than open systems. We should know this by now. The open world of RSS based podcasting can not only
keep pace with closed systems, it can exceed them easily. Podcasting 2.0 is simply the technological expression of this idea. We can make a better podcasting
experience for listeners than they can get behind any walled garden - no matter how high or expensive those walls are.

There are three parts to Podcasting 2.0:

1. The "podcast" namespace
2. Web app friendliness
3. Value for Value

## Step 1. Adopt the "podcast" Namespace

To be a Podcasting 2.0 compliant podcast you need to first declare the "podcast"
[namespace](/docs/1.0.md) in your feed if you self-host your podcast. If you
use a hosting company for your podcast, check [PodcastIndex.org/apps](https://podcastindex.org/apps) or [Podcasting2.org/apps](https://podcasting2.org/apps) for a list of hosts that now support the new namespace.

The namespace gives you (and your listeners) access to many new features:

- Transcripts: You can deliver a text transcript along with your episode to make your content more accessible to those with hearing challenges, or for those
  learning your language.
- Funding: This points listeners back to a donation or membership page that they can click on to join or donate money to your show.
- Chapters: MP3 files have had the ability to embed chapters for many years. But, now you can create a "chapters file" that gets delivered along with your
  episode to allow rich content like images, embedded web pages, titles and silent markers. This chapters file lives on the web, so it can be
  changed later after publishing without uploading a new audio file or changing your episode.
- Soundbites: Specify short bits of your episode to serve as an intro or a teaser for your show.
- Persons: You can give multiple bio's in each episode that have short "about" descriptions of the people on that episode (like hosts, guests, etc.). Did you
  interview someone cool? Point to their head shot image and link to their Wikipedia page or their blog. It makes searching for people within podcasts
  easy and enjoyable.
- Location: Is your podcast about a specific place? Tag it's location right in the episode or podcast feed to let people know. It makes your show more
  discoverable on the web.
- Named Seasons: Seasons have been around for a while, but now you can name them. This way you can avoid the hassle of trying to cram everything in your show title.
- Trailers: Trailers can exist as a separate audio/video file, outside of an actual episode. They can be tied to specific seasons for promotional consistency.
- License: Want to protect your work from derivative use, or the other way around? You can assign a specific license to the content of the entire podcast, or different
  licenses for each episode. You can use preexisting open source licenses or write one yourself.
- Alternate Enclosures: It's now possible to deliver alternative versions of content in the same podcast feed. You can assign video versions of an audio podcast. Or,
  maybe a low (or high) bandwidth version of the main audio/video file. Or, perhaps you want to deliver a version of your content over another
  method like IPFS or WebTorrent.

## Step 2. Be Web App Friendly

Next, you need to confirm that your feed does not have "mixed content", and that it supports CORS where necessary. Again, if you use a hosting company for your podcast
just [make sure](https://podcastindex.org/apps) they are supporting Podcasting 2.0 features. If your host isn't on that list, send them a message and let them know you
want these new features.

If you self-host your podcast, take note of these two things...

#### Mixed Content

"Mixed content" is what happens when part of your podcast (like your feed) is served securely and other parts of it (like the images or the audio) are not. When this
happens, web-based podcast players cannot play your content or see your images. Modern web browsers are very strict about security and "mixed content" is blocked by
most of them. Make sure your entire podcast is served over HTTPS with a valid certificate.

#### CORS

Another problem that can hamper your content under certain circumstances (like embedded pages in chapters) is CORS. If you serve content along with your podcast that
requires cross-origin access, please be sure to enable the correct CORS policies on the domain the content is served from. You can find details
[here](https://developer.mozilla.org/docs/Web/HTTP/Guides/CORS).

Web-based podcast apps, PWA's (Progressive Web Apps) and Browser Extension based apps are critical to Podcasting 2.0, so the above changes are very important.

## Step 3. Value for Value

The final step is monetizing your content with cryptocurrency so your listeners can support you directly with no middle-men. This allows your content to be truly free from the pressures of advertising. Advertising serves a necessary role in any free market, but it does come with a cost. That cost is censorship - whether it's direct censorship by the advertisers themselves or self-censorship as you restrict your speech as to not offend the advertisers.

Because of these issues, we've created a way to receive cryptocurrency payments directly from your listeners to you using the experimental `<podcast:value>` tag in your podcast feed. Because it is experimental, it is not currently supported by any major podcast hosting companies. But, if you are so inclined, you can start using it in your feed today so that your podcast will show up on apps that support it like:

- [Breez](https://Breez.technology)
- [Podfriend](https://podfriend.com)
- [CurioCaster](https://curiocaster.com)
- [Podverse](https://podverse.fm)
- [podStation](https://podstation.github.io)
- [Sphinx.chat](https://sphinx.chat)
- [Castamatic](https://castamatic.com)
- [Fountain.fm](https://fountain.fm)

If you can't add the `<podcast:value>` tag to your feed manually, we also have created [a site](https://podcasterwallet.com) that can help you put a value tag directly into the Podcast Index database for your feed. Any apps that use the Podcast Index will see your value tag and be able to stream micropayments to you.

The `<podcast:value>` tag is still early and experimental. But, it does work today. There are more details about it in this [blog post](https://blog.podcastindex.org/html/AnotherWay-lJmNWj9T490hdmPmz5M4GV1Tlw6rDF.html), and in the official [whitepaper](docs/examples/value/value.md). The Podcasting 2.0 community is also always willing to lend you a hand and some advice on the [podcastindex.social](https://podcastindex.social) discussion server.
