---
toc:
    creating-content:
        _title: Creating Content
        text-file-markup: Text File Markup
        blogging: Blogging
nav: 3
---

## Creating Content

Pico is a flat file CMS. This means there is no administration backend or database to deal with. You simply create `.md` files in the `content` folder and those files become your pages. For example, creating a file called `index.md` will make it show as your main landing page.

When you install Pico, it comes with some sample contents that will display until you add your own content. Simply add some `.md` files to your `content` folder in Pico's root directory. No configuration is required, Pico will automatically use the `content` folder as soon as you create your own `index.md`. Just check out [Pico's sample contents][SampleContents] for an example!

If you create a folder within the content directory (e.g. `content/sub`) and put an `index.md` inside it, you can access that folder at the URL `https://example.com/pico/?sub`. If you want another page within the sub folder, simply create a text file with the corresponding name and you will be able to access it (e.g. `content/sub/page.md` is accessible from the URL `https://example.com/pico/?sub/page`). Below we've shown some examples of locations and their corresponding URLs:

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
            <td>content/theme.md</td>
            <td>?theme (hidden in menu)</td>
        </tr>
        <tr>
            <td>content/a/very/long/url.md</td>
            <td>?a/very/long/url</td>
        </tr>
    </tbody>
</table>

If a file cannot be found, the file `content/404.md` will be shown. You can add `404.md` files to any directory. So, for example, if you wanted to use a special error page for your blog, you could simply create `content/blog/404.md`.

Pico strictly separates contents of your website (the Markdown files in your `content` directory) and how these contents should be displayed (the Twig templates in your `themes` directory). However, not every file in your `content` directory might actually be a distinct page. For example, some themes (including Pico's default theme) use some special "hidden" file to manage meta data (like `_meta.md` in Pico's sample contents). Some other themes use a `_footer.md` to represent the contents of the website's footer. The common point is the `_`: all files and directories prefixed by a `_` in your `content` directory are hidden. These pages can't be accessed from a web browser, Pico will show a 404 error page instead.

As a common practice, we recommend you to separate your contents and assets (like images, downloads, etc.). We even deny access to your `content` directory by default. If you want to use some assets (e.g. a image) in one of your content files, files, use Pico's `assets` folder. You can then access them in your Markdown using the `%assets_url%` placeholder, for example: `![Image Title](%assets_url%/image.png)`

### Text File Markup

Text files are marked up using [Markdown][] and [Markdown Extra][MarkdownExtra]. They can also contain regular HTML.

At the top of text files you can place a block comment and specify certain meta attributes of the page using [YAML][] (the "YAML header"). For example:

<pre><code>---
Title: Welcome
Description: This description will go in the meta description tag
Author: Joe Bloggs
Date: 2001-04-25
Robots: noindex,nofollow
Template: index
---</code></pre>

These values will be contained in the `{% raw %}{{ meta }}{% endraw %}` variable in themes (see below). Meta headers sometimes have a special meaning: For instance, Pico not only passes through the `Date` meta header, but rather evaluates it to really "understand" when this page was created. This comes into play when you want to sort your pages not just alphabetically, but by date. Another example is the `Template` meta header: It controls what Twig template Pico uses to display this page (e.g. if you add `Template: blog`, Pico uses `blog.twig`).

In an attempt to separate contents and styling, we recommend you to not use inline CSS in your Markdown files. You should rather add appropriate CSS classes to your theme. For example, you might want to add some CSS classes to your theme to rule how much of the available space a image should use (e.g. `img.small { width: 80%; }`). You can then use these CSS classes in your Markdown files, for example: `![Image Title](%assets_url%/image.png) {.small}`

There are also certain variables that you can use in your text files:

* `%site_title%` - The title of your Pico site
* `%base_url%` - The URL to your Pico site; internal links can be specified using `%base_url%?sub/page`
* `%theme_url%` - The URL to the currently used theme
* `%assets_url%` - The URL to Pico's `assets` directory
* `%themes_url%` - The URL to Pico's `themes` directory; don't confuse this with `%theme_url%`
* `%plugins_url%` - The URL to Pico's `plugins` directory
* `%version%` - Pico's current version string (e.g. `2.0.0`)
* `%meta.*%` - Access any meta variable of the current page, e.g. `%meta.author%` is replaced with `Joe Bloggs`
* `%config.*%` - Access any scalar config variable, e.g. `%config.theme%` is replaced with `default`

### Blogging

Pico is not blogging software - but makes it very easy for you to use it as a blog. You can find many plugins out there implementing typical blogging features like authentication, tagging, pagination and social plugins. See the below Plugins section for details.

If you want to use Pico as a blogging software, you probably want to do something like the following:

<ol>
    <li>
        Put all your blog articles in a separate <code>blog</code> folder in your <code>content</code> directory. All these articles should have a <code>Date</code> meta header.
    </li>
    <li>
        Create a <code>blog.md</code> or <code>blog/index.md</code> in your <code>content</code> directory. Add <code>Template: blog-index</code> to the YAML header of this page. It will later show a list of all your blog articles (see step 3).
    </li>
    <li>
        Create the new Twig template <code>blog-index.twig</code> (the file name must match the <code>Template</code> meta header from Step 2) in your theme directory. This template probably isn't very different from your default <code>index.twig</code> (i.e. copy <code>index.twig</code>), it will create a list of all your blog articles. Add the following Twig snippet to <code>blog-index.twig</code> near <code>{% raw %}{{ content }}{% endraw %}</code>:

{% raw %}<pre><code>{% for page in pages(&quot;blog&quot;)|sort_by(&quot;time&quot;)|reverse if not page.hidden %}
    &lt;div class=&quot;post&quot;&gt;
        &lt;h3&gt;&lt;a href=&quot;{{ page.url }}&quot;&gt;{{ page.title }}&lt;/a&gt;&lt;/h3&gt;
        &lt;p class=&quot;date&quot;&gt;{{ page.date_formatted }}&lt;/p&gt;
        &lt;p class=&quot;excerpt&quot;&gt;{{ page.description }}&lt;/p&gt;
    &lt;/div&gt;
{% endfor %}</code></pre>{% endraw %}
    </li>
</ol>

[SampleContents]: {{ site.gh_project_url }}/tree/{{ site.gh_project_branch }}/content-sample
[Markdown]: https://daringfireball.net/projects/markdown/syntax
[MarkdownExtra]: https://michelf.ca/projects/php-markdown/extra/
[YAML]: https://en.wikipedia.org/wiki/YAML
