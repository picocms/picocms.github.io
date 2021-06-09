---
toc:
    customization:
        _title: Customization
        themes:
            _title: Themes
            dealing-with-pages: Dealing with pages
            twig-filters-and-functions: Twig filters and functions
        plugins: Plugins
nav: 4
---

## Customization

Pico is highly customizable in two different ways: On the one hand you can change Pico's appearance by using themes, on the other hand you can add new functionality by using plugins. Doing the former includes changing Pico's HTML, CSS and JavaScript, the latter mostly consists of PHP programming.

This is all Greek to you? Don't worry, you don't have to spend time on these techie talk - it's very easy to use one of the great themes or plugins others developed and released to the public. Please refer to the next sections for details.

### Themes

You can create themes for your Pico installation in the `themes` folder. Pico uses [Twig][] for template rendering. You can select your theme by setting the `theme` option in `config/config.yml` to the name of your theme folder.

[Pico's default theme][PicoTheme] isn't really intended to be used for a productive website, it's rather a starting point for creating your own theme. If the default theme isn't sufficient for you, and you don't want to create your own theme, you can use one of the great themes third-party developers and designers created in the past. As with plugins, you can find themes on our [themes][OfficialThemes] page.

All themes must include an `index.twig` file to define the HTML structure of the theme, and a `pico-theme.yml` to set the necessary config parameters. Just refer to Pico's default theme as an example. You can use different templates for different content files by specifying the `Template` meta header. Simply add e.g. `Template: blog` to the YAML header of a content file and Pico will use the `blog.twig` template in your theme folder to display the page.

Below are the Twig variables that are available to use in themes. Please note that URLs (e.g. `{% raw %}{{ base_url }}{% endraw %}`) never include a trailing slash.

* `{% raw %}{{ site_title }}{% endraw %}` - Shortcut to the site title (see `config/config.yml`)
* `{% raw %}{{ config }}{% endraw %}` - Contains the values you set in `config/config.yml` (e.g. `{% raw %}{{ config.theme }}{% endraw %}` becomes `default`)
* `{% raw %}{{ base_url }}{% endraw %}` - The URL to your Pico site; use Twig's `link` filter to specify internal links (e.g. `{% raw %}{{ "sub/page"|link }}{% endraw %}`), this guarantees that your link works whether URL rewriting is enabled or not
* `{% raw %}{{ theme_url }}{% endraw %}` - The URL to the currently active theme
* `{% raw %}{{ assets_url }}{% endraw %}` - The URL to Pico's `assets` directory
* `{% raw %}{{ themes_url }}{% endraw %}` - The URL to Pico's `themes` directory; don't confuse this with `{% raw %}{{ theme_url }}{% endraw %}`
* `{% raw %}{{ plugins_url }}{% endraw %}` - The URL to Pico's `plugins` directory
* `{% raw %}{{ version }}{% endraw %}` - Pico's current version string (e.g. `2.0.0`)
* `{% raw %}{{ meta }}{% endraw %}` - Contains the meta values of the current page
    * `{% raw %}{{ meta.title }}{% endraw %}` - The `Title` YAML header
    * `{% raw %}{{ meta.description }}{% endraw %}` - The `Description` YAML header
    * `{% raw %}{{ meta.author }}{% endraw %}` - The `Author` YAML header
    * `{% raw %}{{ meta.date }}{% endraw %}` - The `Date` YAML header
    * `{% raw %}{{ meta.date_formatted }}{% endraw %}` - The formatted date of the page as specified by the `date_format` parameter in your `config/config.yml`
    * `{% raw %}{{ meta.time }}{% endraw %}` - The [Unix timestamp][UnixTimestamp] derived from the `Date` YAML header
    * `{% raw %}{{ meta.robots }}{% endraw %}` - The `Robots` YAML header
    * ...
* `{% raw %}{{ content }}{% endraw %}` - The content of the current page after it has been processed through Markdown
* `{% raw %}{{ previous_page }}{% endraw %}` - The data of the previous page, relative to `current_page`
* `{% raw %}{{ current_page }}{% endraw %}` - The data of the current page; refer to the "Pages" section below for details
* `{% raw %}{{ next_page }}{% endraw %}` - The data of the next page, relative to `current_page`

To call assets from your theme, use `{% raw %}{{ theme_url }}{% endraw %}`. For instance, to include the CSS file `themes/my_theme/example.css`, add `{% raw %}<link rel="stylesheet" href="{{ theme_url }}/example.css" type="text/css" />{% endraw %}` to your `index.twig`. This works for arbitrary files in your theme's folder, including images and JavaScript files.

Please note that Twig escapes HTML in all strings before outputting them. So for example, if you add `headline: My <strong>favorite</strong> color` to the YAML header of a page and output it using `{% raw %}{{ meta.headline }}{% endraw %}`, you'll end up seeing `My <strong>favorite</strong> color` - yes, including the markup! To actually get it parsed, you must use `{% raw %}{{ meta.headline|raw }}{% endraw %}` (resulting in the expected <code>My <strong>favorite</strong> color</code>). Notable exceptions to this are Pico's `content` variable (e.g. `{% raw %}{{ content }}{% endraw %}`), Pico's `content` filter (e.g. `{% raw %}{{ "sub/page"|content }}{% endraw %}`), and Pico's `markdown` filter, they all are marked as HTML safe.

#### Dealing with pages

There are several ways to access Pico's pages list. You can access the current page's data using the `current_page` variable, or use the `prev_page` and/or `next_page` variables to access the respective previous/next page in Pico's pages list. But more importantly there's the `pages()` function. No matter how you access a page, it will always consist of the following data:

* `{% raw %}{{ id }}{% endraw %}` - The relative path to the content file (unique ID)
* `{% raw %}{{ url }}{% endraw %}` - The URL to the page
* `{% raw %}{{ title }}{% endraw %}` - The title of the page (`Title` YAML header)
* `{% raw %}{{ description }}{% endraw %}` - The description of the page (`Description` YAML header)
* `{% raw %}{{ author }}{% endraw %}` - The author of the page (`Author` YAML header)
* `{% raw %}{{ date }}{% endraw %}` - The date of the page (`Date` YAML header)
* `{% raw %}{{ date_formatted }}{% endraw %}` - The formatted date of the page as specified by the `date_format` parameter in your `config/config.yml`
* `{% raw %}{{ time }}{% endraw %}` - The [Unix timestamp][UnixTimestamp] derived from the page's date
* `{% raw %}{{ raw_content }}{% endraw %}` - The raw, not yet parsed contents of the page; use the filter to get the parsed contents of a page by passing its unique ID (e.g. `{% raw %}{{ "sub/page"|content }}{% endraw %}`)
* `{% raw %}{{ meta }}{% endraw %}` - The meta values of the page (see global `{% raw %}{{ meta }}{% endraw %}` above)
* `{% raw %}{{ prev_page }}{% endraw %}` - The data of the respective previous page
* `{% raw %}{{ next_page }}{% endraw %}` - The data of the respective next page
* `{% raw %}{{ tree_node }}{% endraw %}` - The page's node in Pico's page tree; check out Pico's [page tree documentation][FeaturesPageTree] for details

Pico's `pages()` function is the best way to access all of your site's pages. It uses Pico's page tree to easily traverse a subset of Pico's pages list. It allows you to filter pages and to build recursive menus (like dropdowns). By default, `pages()` returns a list of all main pages (e.g. `content/page.md` and `content/sub/index.md`, but not `content/sub/page.md` or `content/index.md`). If you want to return all pages below a specific folder (e.g. `content/blog/`), pass the folder name as first parameter to the function (e.g. `pages("blog")`). Naturally you can also pass variables to the function. For example, to return a list of all child pages of the current page, use `pages(current_page.id)`. Check out the following code snippet:

{% raw %}<pre><code>&lt;section class=&quot;articles&quot;&gt;
    {% for page in pages(current_page.id) if not page.hidden %}
        &lt;article&gt;
            &lt;h2&gt;&lt;a href=&quot;{{ page.url }}&quot;&gt;{{ page.title }}&lt;/a&gt;&lt;/h2&gt;
            {{ page.id|content }}
        &lt;/article&gt;
    {% endfor %}
&lt;/section&gt;</code></pre>{% endraw %}

The `pages()` function is very powerful and also allows you to return not just a page's child pages by passing the `depth` and `depthOffset` params. For example, if you pass `pages(depthOffset=-1)`, the list will also include Pico's main index page (i.e. `content/index.md`). This one is commonly used to create a theme's main navigation. If you want to learn more, head over to Pico's complete [`pages()` function documentation][FeaturesPagesFunction].

If you want to access the data of a particular page, use Pico's `pages` variable. Just take `content/_meta.md` in Pico's sample contents for an example: `content/_meta.md` contains some meta data you might want to use in your theme. If you want to output the page's `tagline` meta value, use `{% raw %}{{ pages["_meta"].meta.logo }}{% endraw %}`. Don't ever try to use Pico's `pages` variable as an replacement for Pico's `pages()` function. Its usage looks very similar, it will kinda work and you might even see it being used in old themes, but be warned: It slows down Pico. Always use Pico's `pages()` function when iterating Pico's page list instead (e.g. `{% raw %}{% for page in pages() %}…{% endfor %}{% endraw %}`).

#### Twig filters and functions

Additional to [Twig][]'s extensive list of filters, functions and tags, Pico also provides some useful additional filters and functions to make theming even easier.

* Pass the unique ID of a page to the `link` filter to return the page's URL (e.g. `{% raw %}{{ "sub/page"|link }}{% endraw %}` gets `https://example.com/pico/?sub/page`).
* You can replace URL placeholders (like `%base_url%`) in arbitrary strings using the `url` filter. This is helpful together with meta variables, e.g. if you add `image: %assets_url%/stock.jpg` to the YAML header of a page, `{% raw %}{{ meta.image|url }}{% endraw %}` will return `https://example.com/pico/assets/stock.jpg`.
* To get the parsed contents of a page, pass its unique ID to the `content` filter (e.g. `{% raw %}{{ "sub/page"|content }}{% endraw %}`).
* You can parse any Markdown string using the `markdown` filter. For example, you might use Markdown in the `description` meta variable and later parse it in your theme using `{% raw %}{{ meta.description|markdown }}{% endraw %}`. You can also pass meta data as parameter to replace `%meta.*%` placeholders (e.g. `{% raw %}{{ "Written by *%meta.author%*"|markdown(meta) }}{% endraw %}` yields "Written by *John Doe*"). However, please note that all contents will be wrapped inside HTML paragraph elements (i.e. `<p>…</p>`). If you want to parse just a single line of Markdown markup, pass the `singleLine` param to the `markdown` filter (e.g. `{% raw %}{{ "This really is a *single* line"|markdown(singleLine=true) }}{% endraw %}`).
* Arrays can be sorted by one of its keys using the `sort_by` filter (e.g. `{% raw %}{% for page in pages|sort_by([ 'meta', 'nav' ]) %}...{% endfor %}{% endraw %}` iterates through all pages, ordered by the `nav` meta header; please note the `[ 'meta', 'nav' ]` part of the example, it instructs Pico to sort by `page.meta.nav`). Items which couldn't be sorted are moved to the bottom of the array; you can specify `bottom` (move items to bottom; default), `top` (move items to top), `keep` (keep original order) or `remove` (remove items) as second parameter to change this behavior.
* You can return all values of a given array key using the `map` filter (e.g. `{% raw %}{{ pages|map("title") }}{% endraw %}` returns all page titles).
* Use the `url_param` and `form_param` Twig functions to access HTTP GET (i.e. a URL's query string like `?some-variable=my-value`) and HTTP POST (i.e. data of a submitted form) parameters. This allows you to implement things like pagination, tags and categories, dynamic pages, and even more - with pure Twig! Simply head over to our [introductory page for accessing HTTP parameters][FeaturesHttpParams] for details.

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
[FeaturesPagesFunction]: {{ site.github.url }}/in-depth/features/pages-function/
[PicoTheme]: https://github.com/picocms/pico-theme
[WikiPlugins]: {{ site.gh_project_url }}/wiki/Pico-Plugins
[OfficialPlugins]: {{ site.github.url }}/plugins/
[OfficialThemes]: {{ site.github.url }}/themes/
[PluginDocs]: {{ site.github.url }}/development/
[PluginUpgrade]: {{ site.github.url }}/development/#upgrade
