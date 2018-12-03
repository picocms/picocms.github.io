---
toc:
    customization:
        _title: Customization
        themes: Themes
        plugins: Plugins
nav: 4
---

## Customization

Pico is highly customizable in two different ways: On the one hand you can change Pico's appearance by using themes, on the other hand you can add new functionality by using plugins. Doing the former includes changing Pico's HTML, CSS and JavaScript, the latter mostly consists of PHP programming.

This is all Greek to you? Don't worry, you don't have to spend time on these techie talk - it's very easy to use one of the great themes or plugins others developed and released to the public. Please refer to the next sections for details.

### Themes

You can create themes for your Pico installation in the `themes` folder. Check out the default theme for an example. Pico uses [Twig][] for template rendering. You can select your theme by setting the `theme` option in `config/config.yml` to the name of your theme folder.

All themes must include an `index.twig` file to define the HTML structure of the theme. Below are the Twig variables that are available to use in your theme. Please note that paths (e.g. `{% raw %}{{ base_dir }}{% endraw %}`) and URLs (e.g. `{% raw %}{{ base_url }}{% endraw %}`) don't have a trailing slash.

* `{% raw %}{{ site_title }}{% endraw %}` - Shortcut to the site title (see `config/config.yml`)
* `{% raw %}{{ config }}{% endraw %}` - Contains the values you set in `config/config.yml` (e.g. `{% raw %}{{ config.theme }}{% endraw %}` becomes `default`)
* `{% raw %}{{ base_dir }}{% endraw %}` - The path to your Pico root directory
* `{% raw %}{{ base_url }}{% endraw %}` - The URL to your Pico site; use Twig's `link` filter to specify internal links (e.g. `{% raw %}{{ "sub/page"|link }}{% endraw %}`), this guarantees that your link works whether URL rewriting is enabled or not
* `{% raw %}{{ theme_dir }}{% endraw %}` - The path to the currently active theme
* `{% raw %}{{ theme_url }}{% endraw %}` - The URL to the currently active theme
* `{% raw %}{{ version }}{% endraw %}` - Pico's current version string (e.g. `2.0.0`)
* `{% raw %}{{ meta }}{% endraw %}` - Contains the meta values of the current page
    * `{% raw %}{{ meta.title }}{% endraw %}`
    * `{% raw %}{{ meta.description }}{% endraw %}`
    * `{% raw %}{{ meta.author }}{% endraw %}`
    * `{% raw %}{{ meta.date }}{% endraw %}`
    * `{% raw %}{{ meta.date_formatted }}{% endraw %}`
    * `{% raw %}{{ meta.time }}{% endraw %}`
    * `{% raw %}{{ meta.robots }}{% endraw %}`
    * ...
* `{% raw %}{{ content }}{% endraw %}` - The content of the current page after it has been processed through Markdown
* `{% raw %}{{ pages }}{% endraw %}` - A collection of all the content pages in your site
    * `{% raw %}{{ page.id }}{% endraw %}` - The relative path to the content file (unique ID)
    * `{% raw %}{{ page.url }}{% endraw %}` - The URL to the page
    * `{% raw %}{{ page.title }}{% endraw %}` - The title of the page (YAML header)
    * `{% raw %}{{ page.description }}{% endraw %}` - The description of the page (YAML header)
    * `{% raw %}{{ page.author }}{% endraw %}` - The author of the page (YAML header)
    * `{% raw %}{{ page.time }}{% endraw %}` - The [Unix timestamp][UnixTimestamp] derived from the `Date` header
    * `{% raw %}{{ page.date }}{% endraw %}` - The date of the page (YAML header)
    * `{% raw %}{{ page.date_formatted }}{% endraw %}` - The formatted date of the page as specified by the `date_format` parameter in your `config/config.yml`
    * `{% raw %}{{ page.raw_content }}{% endraw %}` - The raw, not yet parsed contents of the page; use Twig's `content` filter to get the parsed contents of a page by passing its unique ID (e.g. `{% raw %}{{ "sub/page"|content }}{% endraw %}`)
    * `{% raw %}{{ page.meta }}{% endraw %}`- The meta values of the page (see `{% raw %}{{ meta }}{% endraw %}` above)
    * `{% raw %}{{ page.previous_page }}{% endraw %}` - The data of the respective previous page
    * `{% raw %}{{ page.next_page }}{% endraw %}` - The data of the respective next page
    * `{% raw %}{{ page.tree_node }}{% endraw %}` - The page's node in Pico's page tree
* `{% raw %}{{ prev_page }}{% endraw %}` - The data of the previous page (relative to `current_page`)
* `{% raw %}{{ current_page }}{% endraw %}` - The data of the current page (see `{% raw %}{{ pages }}{% endraw %}` above)
* `{% raw %}{{ next_page }}{% endraw %}` - The data of the next page (relative to `current_page`)

Pages can be used like the following:

{% raw %}<pre><code>&lt;ul class=&quot;nav&quot;&gt;
    {% for page in pages if not page.hidden %}
        &lt;li&gt;&lt;a href=&quot;{{ page.url }}&quot;&gt;{{ page.title }}&lt;/a&gt;&lt;/li&gt;
    {% endfor %}
&lt;/ul&gt;</code></pre>{% endraw %}

Besides using the `{% raw %}{{ pages }}{% endraw %}` list, you can also access pages using Pico's page tree. The page tree allows you to iterate through Pico's pages using a tree structure, so you can e.g. iterate just a page's direct children. It allows you to build recursive menus (like dropdowns) and to filter pages more easily. Just head over to Pico's [page tree documentation][FeaturesPageTree] for details.

To call assets from your theme, use `{% raw %}{{ theme_url }}{% endraw %}`. For instance, to include the CSS file `themes/my_theme/example.css`, add `{% raw %}<link rel="stylesheet" href="{{ theme_url }}/example.css" type="text/css" />{% endraw %}` to your `index.twig`. This works for arbitrary files in your theme's folder, including images and JavaScript files.

Additional to Twig's extensive list of filters, functions and tags, Pico also provides some useful additional filters to make theming easier.

* Pass the unique ID of a page to the `link` filter to return the page's URL (e.g. `{% raw %}{{ "sub/page"|link }}{% endraw %}` gets `https://example.com/pico/?sub/page`).
* To get the parsed contents of a page, pass its unique ID to the `content` filter (e.g. `{% raw %}{{ "sub/page"|content }}{% endraw %}`).
* You can parse any Markdown string using the `markdown` filter (e.g. you can use Markdown in the `description` meta variable and later parse it in your theme using `{% raw %}{{ meta.description|markdown }}{% endraw %}`). You can pass meta data as parameter to replace `%meta.*%` placeholders (e.g. `{% raw %}{{ "Written *by %meta.author%*"|markdown(meta) }}{% endraw %}` yields "Written by *John Doe*").
* Arrays can be sorted by one of its keys using the `sort_by` filter (e.g. `{% raw %}{% for page in pages|sort_by([ 'meta', 'nav' ]) %}...{% endfor %}{% endraw %}` iterates through all pages, ordered by the `nav` meta header; please note the `[ 'meta', 'nav' ]` part of the example, it instructs Pico to sort by `page.meta.nav`). Items which couldn't be sorted are moved to the bottom of the array; you can specify `bottom` (move items to bottom; default), `top` (move items to top), `keep` (keep original order) or `remove` (remove items) as second parameter to change this behavior.
* You can return all values of a given array key using the `map` filter (e.g. `{% raw %}{{ pages|map("title") }}{% endraw %}` returns all page titles).
* Use the `url_param` and `form_param` Twig functions to access HTTP GET (i.e. a URL's query string like `?some-variable=my-value`) and HTTP POST (i.e. data of a submitted form) parameters. This allows you to implement things like pagination, tags and categories, dynamic pages, and even more - with pure Twig! Simply head over to our [introductory page for accessing HTTP parameters][FeaturesHttpParams] for details.

You can use different templates for different content files by specifying the `Template` meta header. Simply add e.g. `Template: blog` to the YAML header of a content file and Pico will use the `blog.twig` template in your theme folder to display the page.

Pico's default theme isn't really intended to be used for a productive website, it's rather a starting point for creating your own theme. If the default theme isn't sufficient for you, and you don't want to create your own theme, you can use one of the great themes third-party developers and designers created in the past. As with plugins, you can find themes in [our Wiki][WikiThemes] and on [our website][OfficialThemes].

### Plugins

#### Plugins for users

Officially tested plugins can be found at [{{ site.github.url }}/plugins/][OfficialPlugins], but there are many awesome third-party plugins out there! A good start point for discovery is [our Wiki][WikiPlugins].

Pico makes it very easy for you to add new features to your website using plugins. Just like Pico, you can install plugins either using [Composer][] (e.g. `composer require phrozenbyte/pico-file-prefixes`), or manually by uploading the plugin's file (just for small plugins consisting of a single file, e.g. `PicoFilePrefixes.php`) or directory (e.g. `PicoFilePrefixes`) to your `plugins` directory. We always recommend you to use Composer whenever possible, because it makes updating both Pico and your plugins way easier. Anyway, depending on the plugin you want to install, you may have to go through some more steps (e.g. specifying config variables) to make the plugin work. Thus you should always check out the plugin's docs or `README.md` file to learn the necessary steps.

Plugins which were written to work with Pico 1.0 and later can be enabled and disabled through your `config/config.yml`. If you want to e.g. disable the `PicoDeprecated` plugin, add the following line to your `config/config.yml`: `PicoDeprecated.enabled: false`. To force the plugin to be enabled, replace `false` by `true`.

#### Plugins for developers

You're a plugin developer? We love you guys! You can find tons of information about how to develop plugins at [{{ site.github.url }}/development/][PluginDocs]. If you've developed a plugin before and want to upgrade it to Pico 2.0, refer to the [upgrade section of the docs][PluginUpgrade].

[Twig]: https://twig.sensiolabs.org/documentation
[UnixTimestamp]: https://en.wikipedia.org/wiki/Unix_timestamp
[Composer]: https://getcomposer.org/
[FeaturesHttpParams]: {{ site.github.url }}/in-depth/features/http-params/
[FeaturesPageTree]: {{ site.github.url }}/in-depth/features/page-tree/
[WikiThemes]: {{ site.gh_project_url }}/wiki/Pico-Themes
[WikiPlugins]: {{ site.gh_project_url }}/wiki/Pico-Plugins
[OfficialPlugins]: {{ site.github.url }}/plugins/
[OfficialThemes]: {{ site.github.url }}/themes/
[PluginDocs]: {{ site.github.url }}/development/
[PluginUpgrade]: {{ site.github.url }}/development/#upgrade
