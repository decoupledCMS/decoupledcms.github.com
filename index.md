---
layout: default
title: Decoupling Content Management
---

# Decoupling Content Management

Traditional content management systems are monolithic beasts. Just to make your website editable you need to accept the
web framework imposed by the system, the templating engine used by the system, and the editing tools used by the system.
Want to have a better user interface? Be prepared to rewrite your whole website, and to the pain of having to migrate
content between different storage systems.

But none of this should be necessary. When web editing tools were more immature, it made sense for the same people to
build the whole stack from database content models to web page generation and editing tools. But that was ten years ago,
now we could do better.

Here is how a traditional CMS looks like:

![cms-monolithic-approach.png][1]

As you can see, the whole system is a monolithic block. The CMS provides content storage, routing, templating, editing
tools, the kitchen sink. Probably you're even tied to a particular relational database for content storage. Want to use
a cool new editor like [Aloha][2], or a different templating engine, or maybe a trendy NoSQL storage back-end? You'll
have to convince the whole CMS project or vendor to switch over.

A much better picture would be something like the following:

![cms-decoupled-approach.png][3]

In this scenario, the concept of Content Management is decoupled. There is a [content repository][4] that manages
content models and how to store them. This could be something like [JCR][5], [PHPCR][6], [CouchDB][7] or [Midgard2][8].
Then there is a web framework, responsible of matching URL requests to particular content and generating corresponding
web pages. This could be [Drupal][9], TYPO3 [Flow][10] or [Neos][11], [Django][12], [CodeIgniter][13], [Midgard MVC][14], or something
similar. And finally there is the web editing tool. The web editing tool provides an interface for managing contents of
the web pages. This includes features like rich text editing, workflows and image handling.

The web editing tools have traditionally been part of the web framework, the framework serving forms and toolbars to the
user as part of the generated web pages. But with modern browsers you could throw forms out of the window and just
[make pages editable][2] as they are.

 [1]: http://bergie.iki.fi/files/1e03f6a5bcbe4223f6a11e0a60db5207a8570387038_cms-monolithic-approach.png "cms-monolithic-approach.png"
 [2]: http://aloha-editor.org/
 [3]: http://bergie.iki.fi/files/1e03f6a6cfe27003f6a11e0a60db5207a8570387038_cms-decoupled-approach.png "cms-decoupled-approach.png"
 [4]: http://bergie.iki.fi/blog/why_you_should_use_a_content_repository_for_your_application/
 [5]: http://jackrabbit.apache.org/
 [6]: https://fosswiki.liip.ch/display/jackalope/Home
 [7]: http://couchdb.apache.org/
 [8]: http://www.midgard-project.org/midgard2/
 [9]: http://drupal.org/
 [10]: http://flow.typo3.org/
 [11]: http://neos.typo3.org/
 [12]: http://www.djangoproject.com/
 [13]: http://codeigniter.com/
 [14]: https://github.com/bergie/midgardmvc_core/blob/master/documentation/index.markdown
