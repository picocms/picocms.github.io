---
layout: docs
title: Creating Content
headline: Creating Content in Pico
description: |
  Markdown and YAML make creating content in Pico a breeze.<br />
  This guide will teach you the basics of these formatting syntaxes<br />
  and how they relate to Pico.
toc:
  downloading-and-installing-pico:
    _title: Downloading and Installing Pico
    developer-instructions: Developer Instructions
  url-rewriting:
    _title: URL Rewriting
    apache: Apache
    nginx: Nginx
  configuring-pico: Configuring Pico
nav-url: /docs/

---

## Simple but Elegant Content Management

Pico's flat file nature is a simple approach to content management.  Pico has no dashboard or admin panel.  There's no clunky interface for creating posts or pages.

In all their efforts to be all-in-one solutions, many CMS's provide an overwhelming amount of options, metadata, interfaces within interfaces, and other overhead to worry about.

Pico is simple: You just create a file and write.

### The Content Folder

When you first set up Pico, you'll notice you have a `content-sample` folder in your Pico root directory.  This folder provides a set of basic sample pages for testing Pico.  It also includes [Pico's inline Readme][InlineReadme], a simple, quick-start reference to using Pico.  When you're ready to create your own content, create a `content` folder in Pico's root directory.  There's no need to remove the `content-sample` folder, or to reconfigure Pico.  Pico will automatically switch to using the `content` folder if it exists.

### All about URLs

## YAML and Markdown

[YAML][] and [Markdown][] are a common pair in today's world.  YAML provides an easy to understand syntax for your pages' metadata while Markdown provides easy, plain-text formatting of your content.  The pair make a great combination, and once you learn them you'll find it hard to disagree.

* possibly due to the popularity of Jekyll... need a better reason than that.

### YAML - Yay, A Metadata Lesson

Writing your metadata in YAML is as easy as writing it out.  Each of your content files in Pico will begin with a "YAML Header".  It will look something like this:

```
---
Title: Welcome
Description: This description will go in the meta description tag
Author: Joe Bloggs
Date: 2013/01/01
Robots: noindex,nofollow
Template: index
---
```

* Date format?  Update Example?

Simple, huh?  And it gets better too.  These are all the variables that Pico supports by default, but none of them are *required* for your page to work.  The only "required" item on the list above is "Title", but only because Pico's Default Theme uses the page Title in order to link to it.

Each of these metadata fields are utilized by your Pico theme, and some themes may add or require additional fields in order to function.

* Needs more depth.  Reference About page and expand on examples.  Link to cookbook?

### Markdown - Because Why Mark *Up*?

```
### Text File Markup

Text files are marked up using [Markdown][] and [Markdown Extra][MarkdownExtra]. They can also contain regular HTML.

At the top of text files you can place a block comment and specify certain meta attributes of the page using [YAML][] (the "YAML header"). For example:


These values will be contained in the `{% raw %}{{ meta }}{% endraw %}` variable in themes (see below).

There are also certain variables that you can use in your text files:

* `%site_title%` - The title of your Pico site
* `%base_url%` - The URL to your Pico site; internal links can be specified using `%base_url%?sub/page`
* `%theme_url%` - The URL to the currently used theme
* `%meta.*%` - Access any meta variable of the current page, e.g. `%meta.author%` is replaced with `Joe Bloggs`
```

## Let's Get Started

### Somewhere to Land

The first page you'll want to create is your "index" or "landing page".  This is the file that will be loaded when a user visits the `base_url` of your Pico site.  Start by creating a blank file named `index.md` in the `content` folder.  In the previous section, we discussed how to use YAML and Markdown to create your content.  Use what you've learned to create a landing page worth visiting!

### 404 Heading Not found

```
If a file cannot be found, the file `content/404.md` will be shown. You can add `404.md` files to any directory. So, for example, if you wanted to use a special error page for your blog, you could simply create `content/blog/404.md`.

```

### Images, Downloads, and Other Assets

```
As a common practice, we recommend you to separate your contents and assets (like images, downloads, etc.). We even deny access to your `content` directory by default. If you want to use some assets (e.g. a image) in one of your content files, you should create an `assets` folder in Pico's root directory and upload your assets there. You can then access them in your markdown using `%base_url%/assets/` for example: `![Image Title](%base_url%/assets/image.png)`
```

* provide empty assets folder?

### To Blog or Not to Blog

[InlineReadme]: needs-url
---

## Creating Content (All about URL's)

If you create a folder within the content folder (e.g. `content/sub`) and put an `index.md` inside it, you can access that folder at the URL `http://example.com/?sub`. If you want another page within the sub folder, simply create a text file with the corresponding name and you will be able to access it (e.g. `content/sub/page.md` is accessible from the URL `http://example.com/?sub/page`). Below we've shown some examples of locations and their corresponding URLs:

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
            <td>content/sub.md</td>
            <td><del>?sub</del> (not accessible, see below)</td>
        </tr>
        <tr>
            <td>content/sub/index.md</td>
            <td>?sub (same as above)</td>
        </tr>
        <tr>
            <td>content/sub/page.md</td>
            <td>?sub/page</td>
        </tr>
        <tr>
            <td>content/a/very/long/url.md</td>
            <td>?a/very/long/url</td>
        </tr>
    </tbody>
</table>



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

[Markdown]: http://daringfireball.net/projects/markdown/syntax
[MarkdownExtra]: https://michelf.ca/projects/php-markdown/extra/
[YAML]: https://en.wikipedia.org/wiki/YAML
