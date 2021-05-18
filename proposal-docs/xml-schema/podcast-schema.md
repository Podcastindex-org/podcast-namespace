# XML Schema Formal Definition of Podcast Index Namespace

A schema in XML is a definition of the elements and attributes in a particular realm known as a namespace. The namespace for the Podcast Index extension is `https://podcastindex.org/namespace/1.0`.

The namespace has a formal definition, called the XML Schema (XSD), which is designed to be readable by a machine. It also has an informal definition, which is the list of elements and their function and use. This is human-readable, and helps the developer to understand the namespace elements' purpose and use. The Podcast Index informal schema is found at `https://github.com/Podcastindex-org/podcast-namespace/blob/main/docs/1.0.md`. This was originally called the "document type definition", but has been renamed to avoid confusion with an older, less versatile, XML schema language of the same name.

#Getting Started

First, you'll need have the formal XML Schema document (`podcast.xsd`) on your machine. When an XML document is created, it must have a root element. Since the Podcast namespace elements are designed to exist within the RSS document namespace, the Podcast namespace doesn't need, and doesn't have, a root element.

For this reason, there's another XML Schema document, `podcast-wrapper.xsd`. This is simply a wrapper schema that defines a non-canonical element called `<test-wrapper>` that acts as the root. Inside this element can appear any of the other Podcast Index namespace tags. The wrapper schema is strictly for understanding and manually authoring the tags of the Podcast Index.

To turn on validation, you'll need the `xml-model` processing instruction at the top of the document, followed by the wrapper tag that includes the namespace declaration:

```xml
<?xml-model href="podcast-wrapper.xsd"?>
<test-wrapper xmlns="https://podcastindex.org/namespace/1.0" xmlns:podcast="https://podcastindex.org/namespace/1.0">



</test-wrapper>
```

Make sure you have an end-tag for the test-wrapper element, and save your document with a `.xml` extension so the XML add-on will know what to do. If your document is not in the same directory as the two XSD files, you'll need to adjust the `xml-model` processing instruction accordingly.

Now you're ready to go. Simply put your cursor between the start- and end-tags and press the `<` key. You'll see a list of valid elements to select from.

![Inserting a tag](first-child-tag.png)

For more information on specifics, continue with this document. But you should have enough to get started exploring.

#Namespace Specifics (and a short tutorial)

A namespace usually takes the form of a URL to make it globally unique. The namespace for this extension is `https://podcastindex.org/namespace/1.0`. In order to differentiate an element in one namespace from one having the same name in another namespace, it is necessary to preface the element name with the appropriate namespace.

Suppose, for example, an XML document is required that uses two different namespaces. One from our Podcast namespace and one from a namespace defining tags for a yearbook. They both have an element called `person`, but these are two separate elements with two different definitions and usages.

This is done by including the namespace and a colon before the name of the element:

```xml
<https://podcastindex.org/namespace/1.0:person>Person in the Podcast namespace</https://podcastindex.org/namespace/1.0:person>
```

This is uniquely different from this:
```xml
<http://yearbook.com/namespace:person>Person in the Yearbook namespace</https://yearbook.com/namespace:person>
```

But you can see that putting this namespace in front of each start tag and end tag will make the document long and hard to read. This is why the concept of namespace aliases was developed.

In the top of a document, you can specify the namespace and give it a (hopefully shorter) alias, which is then used at the beginning of each start tag and end tag. This is called the XML Namespace Declaration (`xmlns`):

```xml
<doc xmlns:podcast="https://podcastindex.org/namespace/1.0"
     xmlns:yearbook="http://yearbook.com/namespace:person">
     <podcast:person>Person in the Podcast namespace</podcast:person>
     <yearbook:person>Person in the Yearbook namespace</yearbook:person>
```

These are two verifiably distinct elements that can be handled separately with no confusion by the application.

Note that the namespace alias is local to the document. The following snippet is syntactially identical to the previous one:

```xml
<doc xmlns:p="https://podcastindex.org/namespace/1.0"
     xmlns:y="http://yearbook.com/namespace:person">
     <p:person>Person in the Podcast namespace</p:person>
     <y:person>Person in the Yearbook namespace</y:person>
```

You can also specify a namespace to be the default namespace. Any element or attribute that is not prefixed is said to be a member of the default namespace. So the following snipped is syntactically identical to the first two:

```xml
<doc xmlns:p="https://podcastindex.org/namespace/1.0"
     xmlns="http://yearbook.com/namespace:person">
     <p:person>Person in the Podcast namespace</p:person>
     <person>Person in the Yearbook namespace</person>
```

This is, in fact, how the RSS feed works. To add our Podcast namespace to this feed, we must assume that the RSS schema is a member of the default namespace, so we must declare our namespace as a guest with an alias:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<rss xmlns:podcast="https://podcastindex.org/namespace/1.0"
     xmlns:itunes="http://www.itunes.com/dtds/podcast-1.0.dtd" version="2.0">
    <channel>
        <title>Podcasting 2.0 Namespace Example</title>
        <description>This is a fake show that exists only as an example of the "podcast" namespace tag usage.</description>
        ...
        <podcast:locked owner="podcastowner@example.com">yes</podcast:locked>
        <podcast:funding url="https://example.com/donate">Support the show!</podcast:funding>
        <podcast:location geo="geo:30.2672,97.7431" osm="R113314">Austin, TX</podcast:location>

        <itunes:author>John Doe</itunes:author>
        <itunes:explicit>no</itunes:explicit>
        <itunes:type>episodic</itunes:type>
        <itunes:category text="News"/>
        <itunes:category text="Technology"/>
        ...
```

Notice that two `xmlns` declarations are in the start tag, `rss`. Also notice how they are given their own namespace alias. You can see in the example that those elements in the podcast namespace are prefixed differently than those in the iTunes namespace.

But where is the namespace for RSS? This is where we get into implementation-specific shortcuts. Technically, the RSS namespace should have an `xmlns` declaration, which would indicate which namespace all of the non-prefixed tags belong to. But in the implementation of this, the implementor can infer the RSS namespace without it being explicitly specified.

#Using the Podcast Index XML Schema

One of the advantages of having a formalized schema is that tools that support schemas will be able to assist the developer in creating documents and applications.

Visual Studio Code, for example, has several XML add-ins that support auto-complete processing of tags defined in an arbitrary XML Schema. For those not familiar with the Microsoft ecosystem, this is called Intellisense. VS Code is a free editor that is similar to Eclipse and Atom.

Pressing the `<` key will bring up a list of elements that are valid at that point in the document. Since the Podcast namespace is just a flat list of elements (with the exception of value, which has a single sub-element), then you get a list of all elements.

![Inserting a tag](start-tag-completion.png)

It also shows documentation that exists in the schema to help you select what element you want. Picking the element will insert the start- and end-tags and put the cursor in the text area:

![Inserting a tag](tag-inserted.png)

Hovering over any element or attribute name will also pop up the documentation associated with each object:

![Hovering over a tag](tag-hover.png)

The XML extension used to create these screen shots is the `redhat.vscode-xml` add-on available in Visual Studio Code. While this particular extension auto-completes all required attributes, it does not prompt you for a list optional attributes. Perhaps they will add this functionality, or I'll update this document if I should find an extension that provides that functionality.

#To Do

* This schema defines only the tags specified by the Podcast Index namespace. A helpful addition to this effort would be to integrate the formal RSS and iTunes namespaces into the project and provide a single, validatable authoring environment for developers to learn the syntax and also to validate instances programmatically.

* The wording in the annotation/documentation tags was taken directly out of the tag documentation on the project site. The descriptions might not be appropriate for use in an editor as a quick reference. The wording could probably be tweaked to make it more appropriate for editing and app development tools.

* The schema has all of the annotation/documentation tags and content that makes it helpful for integrating into an editor that provides auto-complete authoring. These elements make the schema much larger. Perhaps a second schema could be created that strips out all of this material for a slimmed-down version for machine use. For example, this:
```xml
<xs:attribute name="url" use="required">
    <xs:annotation>
        <xs:documentation>
            URL of the podcast transcript.
        </xs:documentation>
    </xs:annotation>
</xs:attribute>
```
...would become this:
```xml
<xs:attribute name="url" use="required"/>
```

