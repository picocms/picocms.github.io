---
layout: docs
title: Upgrade to Pico 2.0
headline: Upgrade Pico 1.0 to Pico 2.0
description: Let us take you on a journey to Pico's next evolutionary stage!
toc:
    upgrading-to-pico-20: Upgrading to Pico 2.0
    use-yaml-files-to-configure-pico: Use YAML files to configure Pico
    amazing-new-features-for-theme-developers: Amazing new features for theme developers
    use-picos-next-generation-plugin-system: Use Pico's next generation plugin system
    everything-else-that-was-happening: Everything else that was happening...
    developer-news: Developer News
nav-url: /docs/
gh_release: v2.0.0
redirect_from:
    - /in-depth/upgrade/index.html
    - /upgrade/index.html
    - /upgrade.html
---

We are not exaggerating, when we say that Pico 2.0 is the next evolutionary stage of Pico! From routing, over Pico's plugin and theme system, to cutting of "old habbits" - there's basically not a single aspect of Pico we didn't improve. Making Pico simpler, faster, and more flexible than ever before! Best of all, there are no major breaks in backwards compatibility.

If you have a question about one of the new features of Pico 2.0, the upgrade process, or about Pico in general, please check out the ["Getting Help" section][GettingHelp] of the docs and don't be afraid to open a new [Issue][Issues] on GitHub.

## Upgrading to Pico 2.0

Do you remember when you installed Pico? It was ingeniously simple, wasn't it? Upgrading Pico 1.0 to Pico 2.0 is no difference! Since it's a new major release, the changes are pretty heavy - but this shouldn't affect you much. The upgrade process can be summed up in just one sentence: Install Pico ordinarily as if it were a new website, and copy all of your site-specific files (like assets, the config file, your content files, plugins and themes). For convenience, there are step-by-step instructions provided below.

1. **Create a backup of your Pico installation.** You will need the files later!

2. Empty your installation directory and [install Pico just ordinarily][Install]. Don't forget about uploading hidden files like `.htaccess`!

3. Copy `config/config.php` from your backup to your new Pico installation. It's not strictly necessary, but we highly recommend you to convert your PHP config file to a YAML config file (i.e. `config/config.yml`). Please refer to the ["Use YAML files to configure Pico" section][UpgradeConfig] below for more details.

4. Copy both the `assets` (if present) and the `content` folder from your backup to Pico's installation directory. They are fully compatible, so no changes are necessary. 😊

5. If applicable, copy the folder of your custom theme within the backed `themes` directory to your Pico installation. We made various improvements to Pico's theme system, but the most important change (because it might break your theme) is that Pico now requires Twig templates to use the `.twig` file extension. So, if your custom theme still uses `.html` file extensions (like `index.html`), rename all templates to `*.twig`. If your theme uses includes (e.g. `{%raw %}{% include "header.html" %}{% endraw %}`), you must change the file extension there as well. Naturally this isn't the only change, but none of them is strictly necessary, though highly recommended. So, please refer to the ["Amazing new features for theme developers" section][UpgradeThemes] below for more details. If you're using a 3rd-party theme, make sure to check whether there's a new version available. If you encounter any problems, please refer to the theme developer - the theme might need to be updated to work with Pico 2.0.

6. Provided that you're using plugins, copy all of your plugins from the backed `plugins` directory to your Pico installation. Don't copy the outdated files `00-PicoDeprecated.php`, `01-PicoParsePagesContent.php`, `02-PicoExcerpt.php` and `DummyPlugin.php`. Pico 2.0 heavily improves the plugin system and unfortunately also introduces some backwards-incompatible changes. The most important change is that Pico no longer tries to interpret any file in the `plugins` directory as plugin, but this also means that it might ignore a plugin or stumble over a superfluous file. If all of your plugins consist of just a single file (i.e. `plugins/<plugin name>.php`), you're likely good to go. However, if you're copying folders to your `plugins` directory, please note that Pico now only interprets `plugins/<plugin name>/<plugin name>.php` as plugin. So make sure to check whether there are updates available for the plugins you use. If one of your plugins stops working after upgrading to Pico 2.0, please refer to the plugin developer - the plugin needs to be updated. Please refer to the ["Use Pico's next generation plugin system" section][UpgradePlugins] below for more details.

## Use YAML files to configure Pico

Configuring Pico is now easier than ever before. Up to this point, Pico used a regular PHP file, the programming language Pico is written in, for its configuration (`config/config.php`). However, dealing with PHP isn't "stupidly simple", especially for non-developers: It has some pretty strict and unapparent syntax rules, and just the slightest typo results in a PHP error and breaking your whole website. That's not what we want!

For this reason we're happy to anounce YAML config files! As a Pico user you know YAML from the YAML header of your content files, thus it was the obvious choice. But we didn't stop there. Rather than having just a single config file, you can now use a arbitrary number of config files. Simply create a `.yml` file in Pico's `config` dir and you're good to go. This allows you to add some structure to your configuration, like a separate config file for your theme (`config/my_theme.yml`).

Just take a look at the `config/config.yml.template` and create your own config file `config/config.yml`. Even though we still support the old PHP config file (`config/config.php`), we highly recommend you to replace it by a appropiate `config/config.yml`. We will likely drop support of `config/config.php` in Pico's next major release.

Please note that Pico loads config files in a special way you should be aware of. First of all it loads the main config file `config/config.yml`, and then any other `*.yml` file in Pico's `config` dir in alphabetical order. The file order is crucial: Configiguration values which have been set already, cannot be overwritten by a succeeding file. For example, if you set `site_title: Pico` in `config/a.yml` and `site_title: My awesome site!` in `config/b.yml`, your site title will be "Pico".

## Amazing new features for theme developers

Theme developers have always been a subject close to our hearts, and with Pico 2.0 we make them more powerful than ever before. Sure, we love Pico's plugin system, but let's face it: PHP isn't as easy as Twig. Thus we introduce two amazing new features, allowing theme developers to do things only plugin developers could do before: Accessing HTTP GET and HTTP POST parameters, and accessing pages using a tree structure. If your theme stops working after upgrading to Pico 2.0, please refer to the theme developer - the theme needs to be updated.

Starting with Pico 2.0 you can use the `url_param` and `form_param` Twig functions to access HTTP GET (i.e. a URL's query string like `?some-variable=my-value`) and HTTP POST (i.e. data of a submitted form) parameters. This allows you to implement things like pagination, tags and categories, dynamic pages, and even more - with pure Twig! Simply head over to our [introductory page for accessing HTTP parameters][FeaturesHttpParams] for details.

The second major improvement for theme developers is the introduction of page trees. Did you ever wanted to use dropdowns for your page navigation? Well, this wasn't that easy before... But no longer! Starting with Pico 2.0 you can use Pico's built-in page tree to implement awesome new features like recursive menus. The page tree even makes things like iterating through just a page's direct child pages way easier. Just head over to our [page tree documentation][FeaturesPageTree] for details.

Besides adding amazing new features we always strive for improving Pico's existing features. One of our main goals for Pico 2.0 was to improve consistency. The only backwards-incompatible change that affects theme developers is that Pico now requires all Twig templates to use the `.twig` file extension. This change allows a way better integration of plugins and themes: Many plugins generate HTML markup that is hard-coded into the plugin's PHP source code. This isn't a very flexible approach, so we rather recommend plugin developers to create small Twig template to parse the HTML markup. Using a a consistent file extension now allows you, the theme developer, to overwrite a plugin's Twig template. If you're still using the `.html` file extension, you don't have to do more than just change all file extensions to `.twig`. If your theme uses includes (e.g. `{%raw %}{% include "header.html" %}{% endraw %}`), you must change the file extension there as well.

Another very important change is the way Pico handles YAML meta headers: Starting with Pico 2.0 we no longer lower a meta header's key unsolicited. If a user adds `SomeKey: value` to a page's meta data, you must use `{% raw %}{{ meta["SomeKey"] }}{% endraw %}` (rather than `{% raw %}{{ meta["somkey"] }}{% endraw %}` as of Pico 1.0) to access the meta header's value. There will be a transitional phase in which we preserve backwards compatibility with Pico's official `PicoDeprecated` plugin: With Pico 2.0 you can access a meta header like `SomeKey: value` through both `{% raw %}{{ meta["SomeKey"] }}{% endraw %}` (recommended) and `{% raw %}{{ meta["somkey"] }}{% endraw %}` (deprecated). However, please note that this transitional phase will end with Pico's next major release (i.e. Pico 3.0).

Besides the bigger new features, Pico 2.0 introduces some more smaller improvements and changes:

* The `markdown` Twig filter now supports passing meta data to replace `%meta.*%` placeholders (e.g. `{% raw %}{{ "Written *by %meta.author%*"|markdown(meta) }}{% endraw %}` yields "Written by *John Doe*")
* The `sory_by` Twig filter now supports the `remove` fallback, telling Pico to remove all pages that couldn't be sorted (e.g. `{% raw %}{% for page in pages|sort_by("time", "remove") %}{% endraw %}` iterates through all pages with a valid `Date` meta header)
* Besides accessing the current page's respective previous and next page, you can now access the respective next and previous page for any page (e.g. `{% raw %}{{ pages["sub/page"].next_page.title }}{% endraw %}`)
* The `rewrite_url` and `is_front_page` Twig variables have been removed (use `{% raw %}{{ config.rewrite_url }}{% endraw %}` and `{% raw %}{{ current_page.id == "index" }}{% endraw %}` instead)
* You can now access Pico's current version string (e.g. `2.0.0`) using the `{% raw %}{{ version }}{% endraw %}` Twig variable and `%version%` Markdown placeholder

## Use Pico's next generation plugin system

One of Pico's greatest strenghts is its expandability - but what would this be without plugin developers? Yes, we're talking about those awesome guys making so much possible! Pico 2.0 introduces a vast number of improvements for plugin developers. If you're not a plugin developer, most of the changes won't affect you directly, however, two major changes are important for every Pico user: Starting with Pico 2.0 we load plugins in a different way, what might affect how the plugins you use work. If one of your plugins stops working after upgrading to Pico 2.0, please refer to the plugin developer - the plugin needs to be updated.

Pico 2.0 no longer tries to load arbitrary files in the `plugins` directory as plugin. If you use a very old plugin that doesn't follow our plugin recommendations, Pico might stumble over a superfluous file or ignore a plugin. Plugins consisting of just a single file (i.e. `plugins/<plugin name>.php`) aren't affected by this, however, if you use folders in your `plugins` directory, note that starting with Pico 2.0 only the `plugins/<plugin name>/<plugin name>.php` file is interpreted as plugin. Make sure to remove superfluous files in your `plugins` directory and to upgrade all of your plugins when upgrading to Pico 2.0.

The second major change to Pico's plugin system is in which order plugins are executed. Starting with Pico 2.0 we use a plugin dependency topology to change the execution order of plugins. Previously plugins were loaded in strict alphabetical order, so plugin `A` was executed before plugin `B`. However, what if plugin `A` depends on plugin `B`? This usually means that plugin `B` does things that plugin `A` depends on, and thus plugin `B` must be run *before* plugin `A`. Previously you had to ensure this yourself by renaming the plugin and adding numeric prefixes. This is no longer necessary. Pico now loads plugins, on which other plugins depend, before their dependants, so plugin `B` is executed before plugin `A` automatically.

That's the important stuff for any Pico user. If you're one of our splendid plugin developers, please refer to the ["Developer News" section][UpgradeDevs] below for a complete list of all improvements.

## Everything else that was happening...

But wait, there's more than that! There's a pretty big number of other important changes to Pico. First of all, we've updated our dependencies. Pico 2.0 now depends on Twig 1.35, Symfony YAML 2.8 and Parsedown 1.6.

The most obvious improvement of Pico 2.0 is likely Pico's new default theme. It doesn't look much different, but is actually rewritten from scratch. We've designed Pico's new default theme as the perfect starting point for creating your own theme. It demonstrates some of Pico's features and comes with a sleek design. This doesn't only apply to the theme's appearance, but also to its code: the Twig, HTML, CSS and JavaScript code was written with simplicity as main goal, despite being fully compatible with any modern browser. Simply copy the theme's directory (`themes/default` to e.g. `themes/my_theme`) and start editing your theme! Please note that Pico's default theme is no longer maintained in Pico's core repository, but the [`picocms/pico-theme` repository][PicoTheme].

As in Pico 1.0, we heavily improved Pico's routing system with Pico 2.0. One feature frequently requested was dropping the superfluous `/index` part from Pico's URLs. Thus the page `content/sub/index.md` now corresponds to the URL `https://example.com/pico/sub` rather than `https://example.com/pico/sub/index`. Another source of confusion was enabling URL rewriting for webservers other than Apache. This is a thing of the past now! Pico 2.0 supports the `REQUEST_URI` routing method, allowing your webserver to simply rewrite all requests to just `index.php`. This makes enabling URL rewriting easier than ever before. For example, if you're using nginx, a simple `try_files` directive is sufficient to enable URL rewriting. Just refer to the ["URL Rewriting" section in the docs][UrlRewriting] for details.

Pico 2.0 also changes its behavior regarding the `content` directory. First of all, Pico now searches for `content/index.md` when trying to auto-detect the `content` directory. A empty `content` directory no longer confuses Pico. More importantly, Pico now treats content files and directories prefixed by `_` as hidden. If a user requests a hidden page, e.g. `https://example.com/pico/my/_hidden/page`, Pico responds with a 404 page, even though the content file `content/my/_hidden/page.md` exists. Hidden pages are still in Pico's pages list (the `Pico::$pages` array), but are moved to the end when sorted alphabetically or by date. This allows you to specify content files specifically meant to be included, like `content/_meta.md` in Pico's sample contents. Speaking of 404 pages: Pico now comes with a built-in 404 page as fallback when no `content/404.md` is found. Pico furthermore no longer responds with a `200 OK` HTTP status code when explicitly requesting a 404 page.

As described earlier (see the ["Use YAML files to configure Pico" section][UpgradeConfig] above), you can now use YAML files to configure Pico. However, not just the config format changed, there are some additional config parameters, too! Starting with Pico 2.0, you can sort Pico's pages list (the `Pico::$pages` array) by arbitrary meta values. This was a frequently requested feature and allows you to e.g. sort pages by author. Simply set `pages_order_by: meta` in your `config/config.yml` and set the `pages_order_by_meta` config parameter to the meta header you want to sort pages by (e.g. `pages_order_by_meta: author`). Speaking of sorting pages, if you specify a sorting method Pico's core doesn't recognise (i.e. something else than `alpha`, `date` or `meta`), it leaves sorting the pages list up to a plugin.

But that's not all! Pico 2.0 allows you to configure Parsedown and change how your Markdown files are parsed. You can now use the original `Parsedown` parser instead of `ParsedownExtra`, enable breaks or escaping, or disable auto-linking URLs in your markup. Just take a look at the `content_config` config parameters in `config/config.yml.template` for more information. Last but not least, you can now use the `theme_url` config parameter to manually specify the URL to Pico's `themes` directory.

Pico 2.0 also introduces some more smaller improvements and changes:

* You can now explicitly specify the `date_formatted` (a formatted date string) and `time` (a [Unix timestamp][UnixTimestamp]) meta headers in a page's YAML Front Matter
* As announced earlier, Pico 2.0 drops the `PicoParsePagesContent` (use the `content` Twig filter instead, e.g. `{% raw %}{{ "sub/page"|content }}{% endraw %}`) and `PicoExcerpt` (use the `description` meta header instead, e.g. `{% raw %}{{ meta.description }}{% endraw %}` in Twig templates or `%meta.description%` in Markdown files) plugins
* Pico now uses the relative path between `index.php` and `Pico::$themesDir` when guessing Pico's theme URL
* Pico's `.htaccess` file for Apache webservers now includes a directive to deny access to all "dot files" (i.e. files beginning with a `.`, e.g. the `.git` directory) by default; you should update your webserver config if you use another webserver
* Pico's official `PicoDeprecated` plugin is no longer maintained in Pico's core repository, but the [`picocms/pico-deprecated` repository][PicoDeprecated]

[GettingHelp]: {{ site.github.url }}/docs/#getting-help
[Issues]: {{ site.gh_project_url }}/issues
[Install]: {{ site.github.url }}/docs/#install
[UpgradeConfig]: #use-yaml-files-to-configure-pico
[UpgradeThemes]: #amazing-new-features-for-theme-developers
[UpgradePlugins]: #use-picos-next-generation-plugin-system
[UpgradeDevs]: #developer-news
[FeaturesHttpParams]: {{ site.github.url }}/in-depth/features/http-params/
[FeaturesPageTree]: {{ site.github.url }}/in-depth/features/page-tree/
[PicoTheme]: https://github.com/picocms/pico-theme
[PicoDeprecated]: https://github.com/picocms/pico-deprecated
[UrlRewriting]: {{ site.github.url }}/docs/#url-rewriting
[UnixTimestamp]: https://en.wikipedia.org/wiki/Unix_timestamp
