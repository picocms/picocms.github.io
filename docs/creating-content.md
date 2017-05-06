---
layout: docs
title: Creating Content
headline: Creating Content in Pico
description: |
  Markdown and YAML make creating content in Pico a breeze.<br />
  This guide will teach you the basics of these formatting syntaxes<br />
  and how they relate to Pico.
toc:
  simple-but-elegant-content-management:
    _title: Simple But Elegant Content Management
    the-content-folder: The Content Folder
    all-about-urls: All About URLs
  yaml-and-markdown:
    _title: YAML and Markdown
    yaml---yay-a-metadata-lesson: YAML - Yay, A Metadata Lesson
    markdown---because-why-mark-up: Markdown - Because Why Mark *Up*?
    variable-variables: Variable Variables
  lets-get-started:
    _title: Let's Get Started
    somewhere-to-land: Somewhere to Land
    404-heading-not-found: 404 Heading Not Found
    images-downloads-and-other-assets: Images, Downloads, and Other Assets
    to-blog-or-not-to-blog: To Blog or Not to Blog
nav-url: /docs/

---

## Simple But Elegant Content Management

Pico's flat file nature is a simple approach to content management.  Pico has no dashboard or admin panel.  There's no clunky interface for creating posts or pages.

In all their efforts to be all-in-one solutions, many CMS's provide an overwhelming amount of options, metadata, interfaces within interfaces, and other overhead to worry about.

Pico is simple: You just create a file and write.

### The Content Folder

When you first set up Pico, you'll notice you have a `content-sample` folder in your Pico root directory.  This folder provides a set of basic sample pages for testing Pico.  It also includes [Pico's inline Readme][InlineReadme], a simple, quick-start reference to using Pico.  When you're ready to create your own content, create a `content` folder in Pico's root directory.  There's no need to remove the `content-sample` folder, or to reconfigure Pico.  Pico will automatically switch to using the `content` folder if it exists and contains an `index.md` file.

### All About URLs

What if you want to organize your content int subfolders, within the `content` folder?  Pico makes it easy to do just that.  If you were to create a file at `content/subfolder/page.md`, you'd be able to access it at `http://example.com/subfolder/page` (or `http://example.com/?subfolder/page` if you don't have url-rewriting enabled).

But Pico lets you take it a step further.  What if you wanted a page at `http://example.com/subfolder/`?  To do this, you'd simply create your page at `content/subfolder/index.md` and Pico will offer it as the landing page for that folder.

One thing to kep in mind though is if you create a folder, you won't be able to access a page with the same name as that folder.  For example, if you have a `content/news/` folder, Pico will direct visitors there instead of to `content/news.md`.

Still having trouble wrapping your head around these URLs?  Here's a table with some more examples:

<table style="width: 100%; max-width: 40em;">
    <thead>
        <tr>
            <th style="width: 50%;">Physical Location</th>
            <th style="width: 50%;">URL</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>content/index.md</td>
            <td>/</td>
        </tr>
        <tr>
            <td>content/subfolder.md</td>
            <td><del>/subfolder</del> (not accessible, see below)</td>
        </tr>
        <tr>
            <td>content/subfolder/index.md</td>
            <td>/subfolder/ (overrides the file above)</td>
        </tr>
        <tr>
            <td>content/subfolder/page.md</td>
            <td>/subfolder/page</td>
        </tr>
        <tr>
            <td>content/a/very/long/url.md</td>
            <td>/a/very/long/url</td>
        </tr>
    </tbody>
</table>

Now that you know how Pico generates urls, let's learn how to actually *create* that content!

## YAML and Markdown

[YAML][] and [Markdown][] are a common pair in today's world, possibly due to the popularity of GitHub.  YAML provides an easy to understand syntax for your pages' metadata while Markdown provides easy, plain-text formatting of your content.  The pair make a great combination, and once you learn them you'll find it hard to disagree.

### YAML - Yay, A Metadata Lesson

Writing your metadata in YAML is as easy as writing it out.  Each of your content files in Pico will begin with a "YAML Header".  It will look something like this:

```
---
Title: Welcome
Description: Welcome to my website, where we discuss the finer points of Pico.
Author: Joe Bloggs
Date: 2013/01/01
Robots: noindex,nofollow
Template: index
---
```

Simple, huh?  And it gets better too.  These are all the variables that Pico supports by default, but none of them are *required* for your page to work.  The only "required" item on the list above is "Title", because Pico's Default Theme uses the page Title in order to link to it.

Each of these metadata fields are utilized by your Pico theme. Some themes may add or require additional fields in order to function.  We'll go into that in more detail about these when we discuss [Theming with Twig][Theming].  For now, stick with Title, Description, Author, and Date, as these are things most pages should probably have.

### Markdown - Because Why Mark *Up*?

Markdown makes formatting your pages simple and intuitive.  According to its creator, John Gruber, "Markdown allows you to write using an easy-to-read, easy-to-write plain text format, then convert it to structurally valid XHTML (or HTML)."

Sounds good, right?  When writing your Pico content files, you get to use the formatting power of [Markdown], [Markdown Extra][MarkdownExtra], and regular HTML.

For simplicity, you'll want to stick with Markdown/Markdown Extra wherever you can, but on occasion you may want more control.  You can use regular HTML code within your document, but be aware that within HTML tags, Pico will not process Markdown.

### Variable Variables

When writing your content, there are also some variables you can use to make your life easier.  By inserting these into your markdown, you can use their values dynamically in your pages.

* `%site_title%` - The title of your Pico site.
* `%base_url%` - The URL of your Pico site.  Internal links can be specified using `%base_url%/sub/page` (`%base_url%?sub/page` without URL rewriting).
* `%theme_url%` - The URL to the folder of your current theme.
* `%meta.*%` - By using the `meta` variable, you can access any YAML variable of the current page, for example, `%meta.author%` would be replaced with `Joe Bloggs` in our YAML example above.

## Let's Get Started

Now that we know the basics of creating pages in Pico, we can start generating our own content.  Remember, every page starts with a YAML header beore the markdown starts.  If you're ever unsure of how this should look, you can check out some of the files in `content-sample`.

### Somewhere to Land

The first page you'll want to create is your "index" or "landing page".  This is the file that will be loaded when a user visits the `base_url` of your Pico site.  Start by creating a blank file named `index.md` in the `content` folder.  In the previous section, we discussed how to use YAML and Markdown to create your content.  Use what you've learned to create a landing page worth visiting!

### 404 Heading Not Found

If Pico cannot find a given file, it'll instead show `content/404.md`.  You can customize this file to create your own, personal 404 error.

You can also add `404.md` to any subfolder and it'll become the 404 page for that folder.  For example, if you wanted to have a special 404 error page for your blog, you might create this file at `content/blog/404.md`.

### Images, Downloads, and Other Assets

So, you're typing away, creating some content, when you think, "I'd like to put an image here".  You link to an image within your content folder, only to discover that it the image generates a 404 error.

If you're using our recommended [Apache][ApacheConfig] or [Nginx][NginxConfig], this is because we deny access to the content folder by default for security reasons.  You probably don't want somebody else probing around in your site's content folder either.

Instead, all images, downloads, and other website assets should be placed in the `assets` folder.  You'll find this location in the root of your Pico install.  How you choose to organize this folder is up to you though.

It may seem counter-intuitive at first, but ultimately, we feel it's tidier this way.  It stops your content folder from being overrun with files that don't belong there.

It's easy to link to an item in your assets folder, just use `%base_url%/assets/` in your Markdown.  For example `![Image Title](%base_url%/assets/image.png)`.

### To Blog or Not to Blog

Pico is not blogging software.  That being said, It is fairly easy to create a blog using Pico, as long as you're aware of it's limitations.  Pico lacks many standard blogging features, such as authentication, tagging, pagination, and social features.  There are third party plugins available to help bridge this gap though.

In the not-too-distant future, `Pico 2.0` will ship with optional, but officially supported blogging plugins and tools.  We've even got an official editor in the works, so stay tuned.

In the meantime, see what you can do with the current version.  You'll find that Pico is incredibly extendable.

Check out our [Cookbook][] for some tips and code snippets that will help you use Pico for [blogging][CookbookBlogging].

---

## Up Next...

You've installed Pico and now you've created a simple page or two.  What now?  Well, Pico's default theme is a little sparse.  It's not really meant to be used in production.  The default theme is more of a platform for you to customize to meet your own needs.

How do you do this though?  That's what our next guide will cover.  You'll learn how to customize Pico using the powerful template engine, Twig.  Click through to proceed to the next article.  We'll see you there.

<p class="aligncenter" markdown="1"><a href="{{ site.github.url }}/docs/theming-with-twig" class="button">Theming with Twig</a></p>

[InlineReadme]: {{ site.gh_project_url }}/blob/master/content-sample/index.md
[YAML]: https://en.wikipedia.org/wiki/YAML
[Markdown]: http://daringfireball.net/projects/markdown/syntax
[MarkdownExtra]: https://michelf.ca/projects/php-markdown/extra/
[Cookbook]: {{ site.github.url }}/cookbook/
[CookbookBlogging]: {{ site.github.url }}/cookbook/#BloggingWithPico
[Theming]: {{ site.github.url }}/docs/theming-with-twig

{% comment %}

* Move existing [blogging section](http://picocms.org/docs/#blogging) to cookbook.

---

### Blogging

Pico is not blogging software - but makes it very easy for you to use it as a blog. You can find many plugins out there implementing typical blogging features like authentication, tagging, pagination and social plugins. See the below Plugins section for details.

If you want to use Pico as a blogging software, you probably want to do something like the following:
<ol>
    <li>
        Put all your blog articles in a separate <code>blog</code> folder in your <code>content</code> directory. All these articles should have both a <code>Date</code> and <code>Template</code> meta header, the latter with e.g. <code>blog-post</code> as value (see Step 2).
    </li>
    <li>
        Create a new Twig template called <code>blog-post.twig</code> (this must match the <code>Template</code> meta header from Step 1) in your theme directory. This template probably isn't very different from your default <code>index.twig</code>, it specifies ow your article pages will look like.
    </li>
    <li>
        Create a <code>blog.md</code> in your <code>content</code> folder and set its <code>Template</code> meta header to e.g. <code>blog</code>. Also create a <code>blog.twig</code> in your theme directory. This template will show a list of your articles, so you probably want to do something like this:

        {% raw %}<pre><code>{% for page in pages|sort_by("time")|reverse %}
    {% if page.id starts with &quot;blog/&quot; %}
        &lt;div class=&quot;post&quot;&gt;
            &lt;h3&gt;&lt;a href=&quot;{{ page.url }}&quot;&gt;{{ page.title }}&lt;/a&gt;&lt;/h3&gt;
            &lt;p class=&quot;date&quot;&gt;{{ page.date_formatted }}&lt;/p&gt;
            &lt;p class=&quot;excerpt&quot;&gt;{{ page.description }}&lt;/p&gt;
        &lt;/div&gt;
    {% endif %}
{% endfor %}</code></pre>{% endraw %}
    </li>
    <li>
        Make sure to exclude blog articles from your page navigation. You can achieve this by adding <code>{&#37; if not (page.id starts with "blog/") &#37;}...{&#37; endif &#37;}</code> to the navigation loop (<code>{&#37; for page in pages &#37;}...{&#37; endfor &#37;}</code>) in your theme's <code>index.twig</code>.
    </li>
</ol>
{% endcomment %}
