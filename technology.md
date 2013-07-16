---
layout: default
title: Technology
---

# Technology

## Common representation of content on HTML level

How would the communication between the web editing tool and the backend work, then?

![cms-decoupled-communications.png][14]

First of all, the web editing tool has to understand the contents of the page. It has to understand what parts of the
page should be editable, and how they connect together. If there is a list of news for instance, the tool needs to
understand it enough to enable users to add new news items. The easy way of accomplishing this is to add some semantic
annotations to the HTML pages. These annotations could be handled via [Microformats][15], [HTML5 microdata][16], but the
most power lies with [RDFa][17].

RDFa is a way to describe the meaning of particular HTML elements using simple attributes. For example:

        News item title
        News item contents

Here we get all the necessary information for [making a blog entry editable][18]:

*   **typeof** tells us the type of the editable object. On typical CMSs this would map to a content model or a database table
*   **about** gives us the identifier of a particular object. On typical CMSs this would be the object identifier or database row primary key
*   **property** ties a particular HTML element to a property of the content object. On a CMS this could be a database column

As a side effect, we also manage to make our page more [understandable to search engines][19] and other semantic tools.
So the annotations are not just needed for UI, but also for SEO.

## Common representation of content on JavaScript level

Having contents of a page described via RDFa makes it very easy to extract the content model into JavaScript. We can
have a common utility library for doing this, but we also should have a common way of keeping track of these content
objects. Enter [Backbone.js][20]:

> Backbone supplies structure to JavaScript-heavy applications by providing models with key-value binding and custom
events, collections with a rich API of enumerable functions, views with declarative event handling, and connects it all
to your existing application over a RESTful JSON interface. With Backbone, the content extracted from the RDFa-annotated
HTML page is easily manageable via JavaScript. Consider for example:

{% highlight javascript %}
objectInstance.set({title: 'Hello, world'});
objectInstance.save(null, {
    success: function(savedModel, response) {
        alert("Your article '" %2B savedModel.get('title') %2B "' was saved to server");
    }
});
{% endhighlight %}

This JS would work across all the different CMS implementations. Backbone.js provides a [quite nice RESTful
implementation][21] of communicating with the server with JSON, but it can be easily overridden with CMS-specific
implementation by just implementing a new Backbone.Sync method. Look for example at the
[localStorage Backbone.js sync implementation][22].

## Content repositories

The purpose of the content repository layer is to separate the server side business logic from the actual storage. By
doing this it becomes possible to reuse the same business logic with radically different storage implementations. For
example a CMS like Drupal that is used from very small sites to some of the biggest CMS sites in the world faces a
difficult challenge in trying to find an optimal solution for all user groups. In the end the storage layer ends up
being a compromise rather than an optimal solution for each target group. If Drupal would leverage a clearly defined
storage API, ideally based on an independent standard like [PHPCR][1], users would be able to choose the implementation
that best fits their scalability requirements that also match the available hardware and software infrastructure.

For example smaller sites might choose to use SQLite for persistence, while a larger site might prefer to leverage
a solution like [Jackrabbit][2]. Yet other sites might prefer using the file system for persistence but want to hook
in a full text search indexing solution like [Solr][3] or [ElasticSearch][4]. The key thing is that any of these choices
should only require changes in the configuration, but not in any actual business logic. Via a feature discovery API
the business logic can automatically adjust itself to leverage optional features.

## New possibilities for collaboration

Once the different Content Management Systems describe their content with RDFa, and provide an unified JavaScript API to
it, lots of things become possible. While most systems probably want to have their own look-and-feel, still many
features can be shared. Consider for example:

*   Using browser's localStorage for storing drafts of content edited by user. Never lose content!
*   Collaborative editing via [XMPP][23] or WebSockets
*   Versioning and undo
*   Semantic enrichment of content using tools like [Apache Stanbol][24]

All of these would be quite hard to implement by an individual CMS project. But if we have a common JS layer available,
the effort can be shared by all CMS projects implementing these ideas.

In the same way once a CMS is using a content repository it suddenly becomes possible to collaborate on its
implementation with other projects, thereby increasing the choices for users and reducing the required development
resources from each project.

## Inline editing vs. traditional form based editing

Obviously when decoupling the content authoring experience it is critical to also ensure that content is actually
managed as content rather than as "final pages" with a specific representation linked to a single page (on a single
device). As such it is important to realize that just because one is using Javascript rather than native HTML-forms
for editing, it is not necessary to use a WYSIWIG approach. In this sense even when using tools like [create.js][30] it
might still make sense to explicitly not use inline editing or use inline editing without WYSIWIG. As such the concepts
described on this page obviously apply to not only editing in the frontend but also editing in a backend system. For
example a key advantage to using inline editing over form based, specifically ``textarea`` form fields, is that many
users find it needlessly constraining to use a fixed size field for editing when in the end the content on the pages
is allowed to flow freely. Furthermore browsers still lack many widgets f.e. for maps, dates and more application
specific content. However it should be noted that the key here is decoupling and not the specific representation of
editing tools in the client. As such even when using RDFa or other similar semantic markup to describe the content it
can at times still make sense to render the editing UI using standard HTML form elements.

## History

There have been prior efforts at doing something similar. In the early 2000s, [OSCOM][25] made the [Twingle tool][26]
that was able to edit and save content with multiple CMSs. Then there was the [Atom Publishing Protocol][27] and the
[Neutrol Protocol][28] efforts, and also [CMIS][29]. But all of these mandated that the systems would have to implement
some particular server-side protocol. The advantage of the approach promoted here is that the only server-side change
needed is adding RDFa annotations to HTML templates, and then the rest happens on the JavaScript level.

 [1]: http://phpcr.github.com
 [2]: http://jackrabbit.apache.org/
 [3]: http://lucene.apache.org/solr/
 [4]: http://www.elasticsearch.org/
 [14]: http://bergie.iki.fi/files/1e03f6a7c83d8dc3f6a11e0a60db5207a8570387038_cms-decoupled-communications.png "cms-decoupled-communications.png"
 [15]: http://microformats.org/
 [16]: http://dev.w3.org/html5/md/
 [17]: http://en.wikipedia.org/wiki/RDFa
 [18]: http://bergie.iki.fi/blog/using_rdfa_to_make_a_web_page_editable/
 [19]: http://bergie.iki.fi/blog/google-s_rich_snippets_will_lead_us_into_semantic_web/
 [20]: http://documentcloud.github.com/backbone/
 [21]: http://documentcloud.github.com/backbone/#Sync
 [22]: https://github.com/jasondavies/Backbone.localStorage/blob/master/backbone.localStorage.js
 [23]: http://wave-protocol.googlecode.com/hg/whitepapers/operational-transform/operational-transform.html
 [24]: http://incubator.apache.org/stanbol/
 [25]: http://bergie.iki.fi/blog/the-doubtful-future-of-oscom/
 [26]: http://www.zope-europe.org/events/0303/oscomsprintzurich
 [27]: http://www.atomenabled.org/developers/protocol/
 [28]: http://bergie.iki.fi/blog/neutron_protocol-separating_ui_from_the_cms/
 [29]: http://en.wikipedia.org/wiki/Content_Management_Interoperability_Services
 [30]: http://createjs.org
