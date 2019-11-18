---
layout: docs
title: Upgrade to Pico 2.1
headline: Upgrade Pico 2.0 to Pico 2.1
description: Pico 2.1 - small but mighty!
toc:
    pico-cms-for-nextcloud: Pico CMS for Nextcloud
    upgrading-to-pico-21: Upgrading to Pico 2.1
    use-picos-next-generation-themes: Use Pico's next generation themes
    everything-else-that-was-happening: Everything else that was happening…
    developer-news: Developer News
more:
    /in-depth/upgrade-pico-10: Upgrade to Pico 1.0
    /in-depth/upgrade-pico-20: Upgrade to Pico 2.0

galleries:
    logo:
        style: magnify
        images:
            -
                heading: Pico's new logo
                description: It's stupidly simple, isn't it?
                thumbnail: /style/images/docs/upgrade-pico-21/thumbnails/logo.png
                fullsize: /style/images/docs/upgrade-pico-21/fullsize/logo.png
                styles: "float: right; margin-left: 2em; border: 1px solid #CCC;"
    nextcloud:
        headline: Pico CMS for Nextcloud
        description: Create simple, secure, shareable and amazingly powerful websites with just a few clicks!
        style: carousel-box
        images:
            -
                heading: Manage your websites
                description: Managing your websites can't be easier - all in one place!
                thumbnail: /style/images/docs/upgrade-pico-21/thumbnails/list_websites.png
                fullsize: /style/images/docs/upgrade-pico-21/fullsize/list_websites.png
            -
                heading: Create new websites
                description: Creating a new personal website is just a few clicks away.
                thumbnail: /style/images/docs/upgrade-pico-21/thumbnails/new_website.png
                fullsize: /style/images/docs/upgrade-pico-21/fullsize/new_website.png
            -
                heading: Example website
                description: Pico CMS for Nextcloud utilized the brand new Pico 2.1.
                thumbnail: /style/images/docs/upgrade-pico-21/thumbnails/website.png
                fullsize: /style/images/docs/upgrade-pico-21/fullsize/website.png
            -
                heading: Manage custom themes
                description: Add custom themes for greater individuality and style.
                thumbnail: /style/images/docs/upgrade-pico-21/thumbnails/custom_themes.png
                fullsize: /style/images/docs/upgrade-pico-21/fullsize/custom_themes.png
            -
                heading: Manage custom plugins
                description: Add custom plugins to reach for Pico's full potential.
                thumbnail: /style/images/docs/upgrade-pico-21/thumbnails/custom_plugins.png
                fullsize: /style/images/docs/upgrade-pico-21/fullsize/custom_plugins.png
            -
                heading: Manage custom templates
                description: Make it easier for users to create new websites.
                thumbnail: /style/images/docs/upgrade-pico-21/thumbnails/custom_templates.png
                fullsize: /style/images/docs/upgrade-pico-21/fullsize/custom_templates.png

nav-url: /docs/
gh_release: v2.1.0
redirect_from:
    - /in-depth/upgrade/index.html
    - /upgrade/index.html
    - /upgrade.html
---

{% include gallery.html gallery='logo' %}

Pico 2.1 is a minor release - but minor doesn't mean it's not powerful! We took over development of the [Pico CMS for Nextcloud][UpgradeNextcloudApp] project and are happy to introduce [API versioning for themes][UpgradeThemes]. Making Pico simpler, faster, and more flexible than ever before! Best of all, it's completely backwards compatible!

Pico now has an official logo! A picture is worth a thousand words and thanks to [@type76][LogoCredits] and his amazing work, we can finally convey Pico's idea of creating websites using a single picture: Stupidly simple and blazing fast, making the web easy!

If you have a question about one of the new features of Pico 2.1, the upgrade process, or about Pico in general, please check out the ["Getting Help" section][GettingHelp] of the docs and don't be afraid to open a new [Issue][Issues] on GitHub.

## Pico CMS for Nextcloud

With Pico 2.1 we took over development of the [Pico CMS for Nextcloud][NextcloudApp] app, the most powerful integration of Pico into another project ever existed. [Nextcloud][] is a free and open-source collaboration platform for creating and using file hosting services. Pico CMS for Nextcloud fully integrates Pico into Nextcloud and allows one to create simple, secure, shareable and amazingly powerful websites with just a few clicks.

* Start a blog
* Share your resume with the world
* Create a plan for world domination and only share with the right friends
* Build a knowledge base and let the smart ones among your colleagues help out

Pico CMS for Nextcloud allows your users to create and manage their own websites. Nextcloud is a very powerful collaboration platform, including WYSIWYG ("What you see is what you get") editors for Markdown. You can sync your website's sources with all your devices and even share access with others for collaborative editing. Creating private websites with limited access is possible, too.

{% include gallery.html gallery='nextcloud' %}

Pico CMS for Nextcloud gives you everything you would expect from a extremely powerful admin interface for Pico. Just give it a try!

1. First you'll have to download and install [Nextcloud][]. [Download Nextcloud server][NextcloudDownload] and follow the [install instructions][NextcloudInstall] in Nextcloud's admin manual. Unfortunately it's not as easy as installing Pico. If you need any help, head over to Nextcloud's IRC channel [#nextcloud on Freenode IRC][NextcloudChat] or the [Nextcloud forums][NextcloudHelp].

2. After installing Nextcloud on your server, head over to the Apps management page of your Nextcloud. You can now either search for "Pico CMS" or check the "Tools" section to find Pico CMS for Nextcloud in [Nextcloud's app store][NextcloudApp]. Hit the "Download and enable" button and you're ready to go!

3. Navigate to Nextcloud's settings page. As an admin you'll find two "Pico CMS" sections in your Nextcloud settings - one below "Personal", another below "Administration". The latter allows you to add custom themes, plugins and templates to Pico, as well as tweaking some advanced settings. The "Pico CMS" section below "Personal" exists for all Nextcloud users and allows one to create personal websites.

Without any question, Nextcloud is a huge project, it couldn't be further away from Pico's "stupidly simple" aspiration. By very popular request, we thought about how to realize advanced stuff like admin interfaces, WYSIWYG editors, collaborative editing, authentication and user management. We want to keep Pico as stupidly simple and blazing fast as we all know and love it. Furthermore we didn't want to re-invent the wheel. Thus we chose to officially support the Pico CMS for Nextcloud app that was previously created by [Maxence Lange][NextcloudCredits]. He did an absolutely amazing job we can only support.

Pico itself will stay as it is - a stupidly simple, blazing fast, flat file CMS to make the web easy. Using Pico CMS for Nextcloud is and will always be fully optional.

## Upgrading to Pico 2.1

Pico 2.1 is a minor release, making the upgrade process as easy as replacing just a few files. As a user, you shouldn't have to consider anything special when upgrading a existing Pico 2.0 installation to Pico 2.1. If you're currently running Pico 1.0, you must [upgrade to Pico 2.0][UpgradePico20] first. If you're currently running Pico 0.8 or 0.9, [upgrade to Pico 1.0][UpgradePico10] first. No matter what, make sure to **create a backup of your Pico installation before upgrading**. After that you can follow the regular [Upgrade instructions][Upgrade]. For convenience, these instructions are also provided below:

1. **Create a backup of your Pico installation.**

2. Think about how you've installed Pico in the past. Did you use [Composer][] or one of Pico's pre-bundled releases?

    - If you've used Composer to install Pico, upgrading Pico is no more than running a single command. Open a shell and navigate to Pico's install directory within the `httpdocs` directory (e.g. `/var/www/html/pico`) of your server. You can now upgrade Pico using the following command:

        ```shell
        $ php composer.phar update
        ```

        That's it! Composer will automatically update Pico and all plugins and themes you've installed using Composer.

   - If you've used one of Pico's pre-bundled releases, the upgrade steps are dead simple, too. First you'll have to delete the `vendor` directory of your Pico installation (e.g. if you've installed Pico to `/var/www/html/pico`, delete `/var/www/html/pico/vendor`). Then [download the latest Pico release][LatestRelease] and upload all files to your existing Pico installation directory. You will be prompted whether you want to overwrite files like `index.php`, `.htaccess`, ... - simply hit "Yes". That's it!

3. Check all your custom plugins and themes whether there are updates available and follow the provided upgrade instructions to upgrade them. Pico 2.1 introduces the new API version 3 for both plugins and themes. However, Pico 2.1 is fully backwards-compatible to Pico 2.0 (using API version 2). This is achieved by Pico's official [`PicoDeprecated` plugin][PicoDeprecated]. The `PicoDeprecated` plugin is installed by default, so usually you don't have to do anything. However, if you've removed `PicoDeprecated` from your Pico installation before, make sure to either upgrade all your plugins and themes to the latest API version 3, or install `PicoDeprecated` by following the plugin's install instructions.

4. Starting with Pico 2.1 we rely on [Parsedown][] v1.8 and [Parsedown Extra][ParsedownExtra] v0.8 to parse your Markdown files. The latest versions of Parsedown fix some known issues, but also change some behaviour to better match the Markdown standards. Due to this you might experience minor issues with your content files. These cases should be very rare and are caused by non-standard markup, but they exist. Thus you should check your content files for parsing issues after upgrading to Pico 2.1. Refer to the ["Everything else that was happening…" section below][UpgradeMisc] for more info.

Please take the opportunity to check whether your webserver is proberly configured, and access to Pico's internal files and directories is denied. Just refer to the ["URL Rewriting" section in the docs][UrlRewriting]. By following the instructions, you will not just enable URL rewriting, but also deny access to Pico's internal files and directories.

That's the *bare minimum* you need to know when upgrading to Pico 2.1. However, there's way more to know. Thus we highly recommend you to keep reading, the things you'll learn, will not disappoint you!

## Use Pico's next generation themes

Starting with Pico 2.1 there's API versioning for themes! All themes should now include a `pico-theme.yml`; just refer to the [`pico-theme.yml` of Pico's default theme][PicoThemeConfig] for an example. `pico-theme.yml` allows you to specify the theme's API version, to tweak Pico's default [Twig][] config, to register custom meta headers and to introduce arbitrary theme config variables.

Another very important change is that we now switched Twig's `autoescape` feature on by default. It causes Twig to escape all HTML markup before outputting a variable. So for example, if you add `headline: My <strong>favorite</strong> color` to the YAML header of a page and output it using `{% raw %}{{ meta.headline }}{% endraw %}`, you'll end up seeing `My <strong>favorite</strong> color` - yes, including the markup! To actually get it parsed, you must use `{% raw %}{{ meta.headline|raw }}{% endraw %}` (resulting in the expected <code>My <strong>favorite</strong> color</code>). Notable exceptions to this are Pico's `content` variable (e.g. `{% raw %}{{ content }}{% endraw %}`), Pico's `content` filter (e.g. `{% raw %}{{ "sub/page"|content }}{% endraw %}`), and Pico's `markdown` filter, they all are marked as HTML safe.

Equally important is Pico's new `pages()` Twig function. It replaces the raw usage of Pico's `pages` variable and should be used to access Pico's pages array. Starting with Pico 3.0 we will even deprecate the use of the `pages` variable. Even though the usage of the new `pages()` Twig function (e.g. `{% raw %}{% for page in pages() %}…{% endfor %}{% endraw %}`) looks very similar to Pico's `pages` variable, it actually utilizes [Pico's page tree][FeaturesPageTree]. The function returns all pages of a tree branch. So, for example, calling `pages()` without any parameters returns the pages `page.md` and `sub/index.md`. If you want to return the pages of a specific branch, simply pass the branch's name as parameter (e.g. `pages("sub")` returns the pages `sub/page.md` and `sub/sub/index.md`). Since all page identifiers are valid branch names, you can safely pass any page's ID to Pico's `pages()` function (e.g. `pages(current_page.id)` returns all child pages of the current page). The `pages()` function additionally accepts the `depth`, `depthOffset` and `offset` parameters, allowing you to tweak what is being returned. For example, most themes will use something like `{% raw %}{% for page in pages(depthOffset=-1) %}…{% endfor %}{% endraw %}` for its main menu. Passing `depthOffset = -1` causes Pico's `pages()` function to include the branch's base page. In the given example the function will additionally return `index.md` next to the usual `page.md` and `sub/index.md`. However, using these parameters is rather advanced stuff. Head over to the [in-depth article about Pico's `pages()` function][FeaturesPagesFunction] to learn more.

As with basically all Pico releases, we also improved some of Pico's Twig filters and functions. Themes can now use the new `url` Twig filter. It allows you to replace URL placeholders (like `%base_url%`) in arbitrary strings. This is helpful together with meta variables, e.g. if you add `image: %assets_url%/stock.jpg` to the YAML header of a page, `{% raw %}{{ meta.image|url }}{% endraw %}` will return `https://example.com/pico/assets/stock.jpg`. Pico's `markdown` Twig filter now supports parsing a single line of Markdown. `{% raw %}{{ "Written by *John Doe*"|markdown() }}{% endraw %}` will return `<p>Written by <strong>John Doe</strong></p>`. If you want to get rid of the HTML paragraphs (i.e. `<p>…</p>`) simply pass the `singleLine` param to the `markdown` filter (e.g. `{% raw %}{{ "Written by *John Doe*"|markdown(singleLine=true) }}{% endraw %}`).

## Everything else that was happening…

For convenience we are introducing some new config variables with Pico 2.1. You can now manually specify the URLs to your `themes` (config `themes_url`; previously named `theme_url`), `plugins` (config `plugins_url`) and `assets` (config `assets_url`) directories. You can furthermore use these variables in both your Markdown files (using the placeholders `%themes_url%`, `%plugins_url%` and `%assets_url%`), in Pico's new `url` Twig filter (using the same placeholders), and in your Twig templates (using the Twig variables `{% raw %}{{ themes_url }}{% endraw %}`, `{% raw %}{ plugins_url }}{% endraw %}` and `{% raw %}{{ assets_url }}{% endraw %}`). Speaking of placeholders in your Markdown files: You can now use the `%config.*%` placeholders in your Markdown files to access your scalar config variables in Pico's `config/config.yml` (e.g. `%config.my_custom_setting%`).

Starting with Pico 2.1 we're using [Parsedown][] v1.8 and [Parsedown Extra][ParsedownExtra] v0.8. Unfortunately both are still in beta (even though they are in beta for a very long time now). Parsedown's previous stable releases had some know issues which were fixed in the current beta releases. Both beta releases appear to be as stable as their respective previous stable release, thus we made the decision that using these beta releases is reasonable. However, it's very important to note that some behavior changed. These cases should be very rare and are caused by non-standard markup, but they exist. Thus you should check your content files for parsing issues after upgrading to Pico 2.1.

Pico's sample contents now includes a `theme.md`. It shows that pages can be hidden in the main menu, but more importantly it aids theme development. The sample page shows basically every format that is possible with Markdown, so if you want to develop or test a custom theme, simply navigate to `theme.md` and check whether all examples show decent.

[Pico's default theme][PicoTheme] was upgraded to use API version 3 and now uses all of Pico's latest improvements regarding its themes system. Apart from that you probably won't notice any difference. Another thing you very likely won't even notice is that we've completely refactored Pico's official [`PicoDeprecated` plugin][PicoDeprecated]. It's installed by default and is responsible for maintaining backwards compatibility of newer Pico releases with old plugins and themes. To put it simple: `PicoDeprecated` is the reason why you can still use plugins and themes written for Pico 0.8. We upgraded the plugin to use API version 3, but more importantly we split up the its functionality into multiple compatibility plugins. `PicoDeprecated` compatibility plugins are responsible for maintaining compatibility to a particular API version and are loaded on demand only. So for example, if you install a plugin using API version 1, `PicoDeprecated` will load its compatibility plugin for plugins using API version 1. There are two compatibility plugins per API version, one for plugins and one for themes. `PicoDeprecated` even allows 3rd-party plugin developers to load their own compatibility plugins.

## Developer News

Even though Pico 2.1 is a minor release, it's quite a lot, isn't it? There's a great number of new features and possibilities. But that's not all! There are many more improvements and changes that don't affect the average Pico user, but developers. Below you will find a complete, more tech-oriented list of changes. It's a extract of Pico's [`CHANGELOG.md`][Changelog]. Please note that Pico's `CHANGELOG.md` isn't supposed to be read as-is; it's rather an supplement to the actual code changes.

If you have a question about one of the new features of Pico 2.1, please check out the ["Getting Help" section][GettingHelp] of the docs and don't be afraid to open a new [Issue][Issues] on GitHub.

<div class="toggle">
<h4 class="title">Pico themes</h4>
<div class="togglebox">
<div markdown="1">

```
* [New] Introduce API versioning for themes and support theme-specific configs
        using the new `pico-theme.yml` in a theme's directory; `pico-theme.yml`
        allows a theme to influence Pico's Twig config, to register known meta
        headers and to provide defaults for theme config params
* [New] Add `pages` Twig function to deal with Pico's page tree; this function
        replaces the raw usage of Pico's `pages` array in themes
* [New] Add `url` Twig filter to replace URL placeholders (e.g. `%base_url%`)
        in strings using the new `Pico::substituteUrl()` method
* [Changed] Enable Twig's `autoescape` feature by default; outputting a
            variable now causes Twig to escape HTML markup; Pico's `content`
            variable is a notable exception, as it is marked as being HTML safe
* [Changed] Rename `prev_page` Twig variable to `previous_page`
* [Changed] Mark `markdown` and `content` Twig filters as well as the `content`
            variable as being HTML safe
* [Changed] Add `$singleLine` param to `markdown` Twig filter as well as the
            `Pico::parseFileContent()` method to parse just a single line of
            Markdown input
* [Removed] Remove superfluous `base_dir` and `theme_dir` Twig variables
```

</div>
</div>
</div>

<div class="toggle">
<h4 class="title">Pico API</h4>
<div class="togglebox">
<div markdown="1">

```
* [New] Introduce API version 3
* [New] Add `onThemeLoading` and `onThemeLoaded` events
* [New] Add `debug` config param and the `Pico::isDebugModeEnabled()` method,
        checking the `PICO_DEBUG` environment variable, to enable debugging
* [New] Add new `Pico::getNormalizedPath()` method to normalize a path; this
        method should be used to prevent content dir breakouts when dealing
        with paths provided by user input
* [New] Add new `Pico::getUrlFromPath()` method to guess a URL from a file path
* [New] Add new `Pico::getAbsoluteUrl()` method to make a relative URL absolute
* [Changed] Add `$basePath` and `$endSlash` params to `Pico::getAbsolutePath()`
* [Changed] Add `AbstractPicoPlugin::configEnabled()` method to check whether
            a plugin should be enabled or disabled based on Pico's config
* [Changed] Deprecate the use of `AbstractPicoPlugin::__call()`, use
            `PicoPluginInterface::getPico()` instead
* [Changed] Deprecate `Pico::getBaseThemeUrl()`
* [Changed] Update to Twig 1.36 as last version supporting PHP 5.3, use a
            Composer-based installation to use a newer Twig version
* [Removed] Remove `PicoPluginInterface::__construct()`
```

</div>
</div>
</div>

<div class="toggle">
<h4 class="title">Miscellaneous</h4>
<div class="togglebox">
<div markdown="1">

```
* [New] Add `assets_dir`, `assets_url` and `plugins_url` config params
* [New] Add `%config.*%` Markdown placeholders for scalar config params and the
        `%assets_url%`, `%themes_url%` and `%plugins_url%` placeholders
* [New] Add `assets_url`, `themes_url` and `plugins_url` Twig variables
* [New] Add `content-sample/theme.md` for theme testing purposes
* [New] #505: Create pre-built `.zip` release archives
* [Fixed] #461: Proberly handle content files with a UTF-8 BOM
* [Changed] Rename `theme_url` config param to `themes_url`; the `theme_url`
            Twig variable and Markdown placeholder are kept unchanged
* [Changed] Update to Parsedown Extra 0.8 and Parsedown 1.8 (both still beta)
* [Changed] Replace various `file_exists` calls with proper `is_file` calls
* [Changed] Refactor release & build system
* [Changed] Improve Pico docs and PHP class docs
```

</div>
</div>
</div>

[UpgradeNextcloudApp]: #pico-cms-for-nextcloud
[UpgradeThemes]: #use-picos-next-generation-themes
[UpgradeMisc]: #everything-else-that-was-happening

[GettingHelp]: {{ site.github.url }}/docs/#getting-help
[Issues]: {{ site.gh_project_url }}/issues

[Nextcloud]: https://nextcloud.com/
[NextcloudApp]: https://apps.nextcloud.com/apps/cms_pico
[NextcloudDownload]: https://nextcloud.com/install/#instructions-server
[NextcloudInstall]: https://docs.nextcloud.com/server/stable/admin_manual/installation/
[NextcloudChat]: https://webchat.freenode.net/?channels=nextcloud
[NextcloudHelp]: http://help.nextcloud.com/
[NextcloudCredits]: https://github.com/daita

[Upgrade]: {{ site.github.url }}/docs/#upgrade
[UpgradePico10]: {{ site.github.url }}/in-depth/upgrade-pico-10
[UpgradePico20]: {{ site.github.url }}/in-depth/upgrade-pico-20
[LatestRelease]: {{ site.gh_project_url }}/releases/latest
[UrlRewriting]: {{ site.github.url }}/docs/#url-rewriting

[PicoThemeConfig]: https://github.com/picocms/pico-theme/blob/{{ page.gh_release }}/pico-theme.yml
[FeaturesPageTree]: {{ site.github.url }}/in-depth/features/page-tree/
[FeaturesPagesFunction]: {{ site.github.url }}/in-depth/features/pages-function/
[LogoCredits]: https://github.com/type76
[Changelog]: {{ site.gh_project_url }}/blob/{{ page.gh_release }}/CHANGELOG.md

[PicoTheme]: https://github.com/picocms/pico-theme
[PicoDeprecated]: https://github.com/picocms/pico-deprecated

[Composer]: https://getcomposer.org/
[Twig]: https://twig.symfony.com/
[Parsedown]: https://parsedown.org/
[ParsedownExtra]: https://parsedown.org/extra/
