# Person

`<podcast:person>`

This element specifies a person of interest to the podcast. It is primarily intended to identify people like hosts, co-hosts and guests. Although, it is flexible enough to allow fuller credits to be given using the roles and groups that are listed in the [Podcast Taxonomy Project](https://podcasttaxonomy.com/)

### Parent

`<channel>` (for a podcast) or `<item>` (for an individual episode)

It is suggested that `<channel>` is always populated, and `<item>` is populated where needed for an individual episode. Where present, people information in `<item>` wholly replaces all information from the `<channel>`.

Publishers are expected to use the `<podcast:person>` element in the `<channel>` parent to set the _regular_ people involved in the podcast: the detail that would be expected to be seen in an overview of the show.

Publishers are expected to use the `<podcast:person>` in the `<item>` parent to **replace** all existing information for an individual episode.

#### For example: _Terry and June_

The fictional podcast _Terry and June_ is normally hosted by Terry Scott and June Whitfield. Within `<channel>`, Terry Scott and June Whitfield are listed as the hosts. A podcast directory, or podcast app, should show Terry Scott and June Whitfield as the hosts of this show.

For one episode, _Terry and June_ was hosted by Reginald Marsh and June Whitfield (Terry was away). In this case, the `<item>` for this episode should contain Reginald Marsh and June Whitfield as the hosts of this episode. A podcast app, when playing this episode, should show only Reginald Marsh and June Whitfield as the hosts of this episode. Because people information in `<item>` replaces all existing people information in `<channel>`, Terry Scott should not be visible as a host of this episode.

#### For example: _Big Daddy_

The fictional podcast _Big Daddy Interviews_ is hosted by Big Daddy, a wrestler. Within `<channel>`, Big Daddy is listed as the host. A podcast directory, or podcast app, should show Big Daddy as the host of this show.

For one episode, _Big Daddy Interviews_ had a guest of Sid James. In this case, the `<item>` for this episode should contain Sid James as a guest, **and** Big Daddy as the host of this episode. Because people information in `<item>` replaces all existing people information in `<channel>`, Big Daddy should be re-stated as the host of this episode.

### Count

Multiple

### Node value

This is the full name or alias of the person. This value cannot be blank. Please do not exceed `128 characters` for the node value or it may be truncated by aggregators.

### Attributes

- `role` (optional): Used to identify what role the person serves on the show or episode. This should be a reference to an official role within the Podcast Taxonomy Project list (see below). If `role` is missing then "host" is assumed.
- `group` (optional): This should be a reference to an official group within the Podcast Taxonomy Project list. If `group` is not present, then "cast" is assumed.
- `img` (optional): This is the url of a picture or avatar of the person.
- `href` (optional): The url to a relevant resource of information about the person, such as a homepage or third-party profile platform. Please see the [example feed](https://github.com/Podcastindex-org/podcast-namespace/blob/main/example.xml) for possible choices of what to use here.

The `role` and `group` attributes are case-insensitive. So, "Host" is the same as "host", and "Cover Art Designer" is the same as "cover art designer".

The full taxonomy list is [here](https://github.com/Podcastindex-org/podcast-namespace/blob/main/taxonomy.json) as a json file.

### Examples

```xml
<podcast:person
        href="https://example.com/johnsmith/blog"
        img="http://example.com/images/johnsmith.jpg"
>John Smith</podcast:person>
```

```xml
<podcast:person
        role="guest"
        href="https://www.imdb.com/name/nm0427852888/"
        img="http://example.com/images/janedoe.jpg"
>Jane Doe</podcast:person>
```

```xml
<podcast:person
        role="guest"
        href="https://example.wikipedia/alicebrown"
        img="http://example.com/images/alicebrown.jpg"
>Alice Brown</podcast:person>
```

```xml
<podcast:person
        group="writing"
        role="guest"
        href="https://example.wikipedia/alicebrown"
        img="http://example.com/images/alicebrown.jpg"
>Alice Brown</podcast:person>
```

```xml
<podcast:person
        group="visuals"
        role="Cover Art Designer"
        href="https://example.com/artist/beckysmith"
>Becky Smith</podcast:person>
```
