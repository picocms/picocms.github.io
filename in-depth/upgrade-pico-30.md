---
layout: docs
title: Upgrade to Pico 3.0
headline: Upgrade Pico 2.1 to Pico 3.0
description: A small update with a lot of external changes.
toc:
    how-to-upgrade: How to upgrade
    upgrade-to-twig-33: Upgrade to Twig 3.3
    upgrade-to-symfony-yaml-54: Upgrade to Symfony YAML 5.4
    downgrade-to-parsedown-17: Downgrade to Parsedown 1.7
    everything-else-that-was-happening: Everything else that was happening…
more:
    /in-depth/upgrade-pico-10: Upgrade to Pico 1.0
    /in-depth/upgrade-pico-20: Upgrade to Pico 2.0
    /in-depth/upgrade-pico-21: Upgrade to Pico 2.1
nav-url: /docs/
gh_release: v3.0.0
redirect_from:
    - /in-depth/upgrade/index.html
    - /upgrade/index.html
    - /upgrade.html
---

You want to learn more about Pico's latest major installment, Pico 3.0?  Pico 3.0 is a major release, but brings relatively small changes to Pico's core.  What it does bring might be the biggest "minor change" Pico had in a while: majorly updated dependencies.  These updates include switching to [Twig 3.3][Twig] (from Twig 1.44) and [Symfony YAML 5.4][Yaml] (from YAML 2.8), but also a slight step back from [Parsedown][] 1.8-beta to the stable Parsedown 1.7 line.

The main reason for these updates is to better support PHP 8.0.  This should be great news to the majority of Pico users, however, it does also mean that Pico now requires PHP 7.2.5 or later, and that Pico no longer supports (ancient) PHP 5 based environments.

If you have any questions about Pico 3.0, the upgrade process, or if you experience compatibility issues with the upgrade, please check out the ["Getting Help" section][GettingHelp] of the docs, and don't be afraid to open a new [Issue][Issues] on GitHub.

## How to upgrade

Pico 3.0 is a major release, thus **it will likely break your website** at first.  Check out the following sections below to learn what to change to fix the expected issues.  This documentation assumes that you've upgraded to Pico 2.1 before; if you're running an older version, check out the upgrade docs to their respective successor first (to [Pico 1.0][UpgradePico10], to [Pico 2.0][UpgradePico20], and to [Pico 2.1][UpgradePico21]).  No matter what, make sure to **create a backup of your Pico installation before upgrading**.  After that you can follow the regular [upgrade instructions][Upgrade].  For convenience, these instructions are also provided below:

1. **Create a backup of your Pico installation.**

2. Think about how you've installed Pico in the past. Did you use [Composer][] or one of Pico's pre-bundled releases?

    - If you've used Composer to install Pico, upgrading Pico itself is no more than running a single command.  Open a shell and navigate to Pico's install directory within the `httpdocs` directory (e.g. `/var/www/html/pico`) of your server.  You can now upgrade Pico using the following command:

        ```shell
        $ php composer.phar update
        ```

        That's it! Composer will automatically update Pico and all plugins and themes you've installed using Composer.

   - If you've used one of Pico's pre-bundled releases, the upgrade steps are dead simple, too.  First you'll have to delete the `vendor` directory of your Pico installation (e.g. if you've installed Pico to `/var/www/html/pico`, delete `/var/www/html/pico/vendor`).  Then [download the latest Pico release][LatestRelease] and upload all files to your existing Pico installation directory.  You will be prompted whether you want to overwrite files like `index.php`, `.htaccess`, ... - simply hit "Yes". That's it!

3. Check all your custom plugins and themes whether there are updates available and follow the provided upgrade instructions to upgrade them.  Pico 3.0 introduces the new API version 4 for both plugins and themes. However, Pico 3.0 is mostly backwards-compatible to Pico 2.1 (using API version 3) and earlier.  This is achieved by Pico's official [`PicoDeprecated` plugin][PicoDeprecated].  The `PicoDeprecated` plugin is installed by default, so usually you don't have to do anything.  However, if you've removed `PicoDeprecated` from your Pico installation before, make sure to either upgrade all your plugins and themes to the latest API version 4, or install `PicoDeprecated` by following the plugin's install instructions.

4. The most important change of Pico 3.0 are updated dependencies - namely [Twig 3.3][Twig], [Symfony YAML 5.4][Yaml], and [Parsedown 1.7][Parsedown].  The new versions of Twig and YAML will very likely cause some issues for you!  Refer to the ["Upgrade to Twig 3.3"][UpgradeTwig], ["Upgrade to Symfony YAML 5.4"][UpgradeYaml], and ["Downgrade to Parsedown 1.7"][UpgradeParsedown] sections below to learn what has changed and how to fix the issues.  For everything else that was changed, refer to the ["Everything else that was happening…" section][UpgradeMisc] below.

Please take the opportunity to check whether your webserver is proberly configured, and access to Pico's internal files and directories is denied.  Just refer to the ["URL Rewriting" section in the docs][UrlRewriting].  By following the instructions, you will not just enable URL rewriting, but also deny access to Pico's internal files and directories.

## Upgrade to Twig 3.3

Skipping Twig 2 altogether, Pico now ships with the [Twig 3.3][Twig] template engine for theming.  This update will likely cause some issues, for a complete list of deprecated features please refer to the docs as of [Twig 1.x][TwigDeprecated1] and [Twig 2.x][TwigDeprecated2].  Most changes affect plugin developers only, but a few changes might also break your custom theme, most notably the following:

* Pico's `map` Twig filter has been removed.  Use Twig's new built-in `map` filter (for advanced usage) or `column` filter (for simple usages) instead.
* Pico's `markdown` Twig filter now throws an error when invalid data (e.g. an array) is passed.
* You can no longer add an `if` condition to the `for` loop (i.e. `{% raw %}{% for … in … if … %}{% endraw %}`), as previously used by Pico's default theme and many 3rd-party themes.  Use a `filter` filter or an `{% raw %}{% if … %}{% endraw %}` condition inside the `{% raw %}{% for … in … %}{% endraw %}` body instead.
* The `{% raw %}{% spaceless %}{% endraw %}` tag was deprecated in favour of the `spaceless` filter.  You can use `{% raw %}{% apply spaceless %}{% endraw %}` as an alternative.
* Speaking of the `{% raw %}{% apply … %}{% endraw %}` tag, the `{% raw %}{% filter … %}{% endraw %}` tag was deprecated in favour of the `{% raw %}{% apply … %}{% endraw %}` tag.
* The `sameas` and `divisibleby` tests are deprecated in favor of `same as` and `divisible by` tests respectively.
* The `{% raw %}{% raw %}{% endraw %}` tag (don't confuse this with the `raw` filter) was deprecated.  Use `{% raw %}{% verbatim %}{% endraw %}` instead.
* The `_self` global variable now returns the current template name instead of the current `\Twig\Template` object.  `_self` was often used to retrieve the current template's name, so simply replace `{% raw %}{{ _self.templateName }}{% endraw %}` by `{% raw %}{{ _self }}{% endraw %}`.

Twig 3.3 also brings many new Twig features to Pico 3.0 though.  If you've ever run into an unsupported Twig filter or function due to Pico's older Twig version, now's the time to try it again!  Please refer to [Twig's documentation][Twig] for a complete list of all features.

If you encounter issues with existing 3rd-party Pico themes, be sure to let their respective developers know and point them toward this page.

## Upgrade to Symfony YAML 5.4

Pico now utilizes the latest [Symfony YAML 5.4][Yaml] release to parse YAML.  This change should go mostly unnoticed, but might still cause some minor issues with the YAML Frontmatters your Markdown files, or your config files.  Please check out Symfony's [changelog][YamlChangelog] for a complete list of changes.  Most notably the following might cause some minor issues:

* Unquoted YAML values must no longer start with a percent sign (`%`), at sign (`@`), grave accent (`&#96;`), vertical bar (`|`), or greater-than sign (`>`).  This will most likely cause some issues with Pico's URL placeholders (e.g. `%base_url%`, or `%assets_url%`), as you must now quote these strings.  For example, `Logo: %assets_url%/img/pico-white.svg` must be replaced by `Logo: "%assets_url%/img/pico-white.svg"`.
* When surrounding strings with double-quotes, you must now escape the backslash character (`\`; e.g. `class: "Foo\Bar"` must be replaced by `class: "Foo\\Bar"`).
* Symfony YAML streamlined the use of mappings (i.e. `key: value` pairs): You can no longer use non-string mapping keys (replace e.g. `123: integer` by `"123": integer`), as they now cause errors.  Duplicate mapping keys (i.e. using the same key twice) will now also cause an error.  Symfony YAML now additionally requires a whitespace after the colon (`:`) separating key and value of a mapping (replace e.g. `key:value` by `key: value`).
* You can no longer use mappings inside multi-line strings.
* Support for the comma (`,`) as a group separator for floats has been dropped.  Use the underscore (`_`) instead (i.e. use `1_234.56` instead of `1,234.56`).
* Symfony YAML changed the behaviour of various custom (non-standard) YAML tags (e.g. `!php/object`).  Check out the changelog for more details.

## Downgrade to Parsedown 1.7

In regards of Pico's Markdown parser - [Parsedown][] resp. [Parsedown Extra][ParsedownExtra] - we perform a downgrade to Parsedown 1.7.  We upgraded to Parsedown 1.8-beta with Pico 2.1 due to its major improvements regarding its improved support for [CommonMark][], but considering its long development time and abandonment in favour of Parsedown 2.0 (also still in development), we decided to switch back to the more commonly used stable Parsedown 1.7.  This shouldn't have much of an impact on Pico users, however, we do recommend you give your content files a quick look to make sure they still render correctly in the older Parsedown 1.7 version.

## Everything else that was happening…

As explained earlier, Pico 3.0 comes with a rather small number of changes to its core. Both [Pico's default theme][PicoTheme] and Pico's official [`PicoDeprecated` plugin][PicoDeprecated] now use API version 4, even though this API version doesn't really change much apart from the already mentioned above.

Pico now prints an error message when the designated theme directory doesn't exist. As a convinience feature, you can now use the `%page_id%`, `%page_url%`, and `%page_path%` placeholders in your Markdown files. They will be replaced by the current page's ID (e.g. `sub/index` for `content/sub/index.md`, `sub/page` for `content/sub/page.md`, …), its URL (`sub` resp. `sub/page` in our examples), or its directory path (`sub` in both examples) respectively. This is useful for e.g. asset management: You can now put assets for pages in the `content/sub/` directory in the `assets/sub/` directory, and reference them using `%assets_url%/%page_path%/my_asset.png`.

And that's basically it. However, please note that this document doesn't cover all improvements and changes, because they simply don't affect the average Pico user, but developers. Below you will find a complete, more tech-oriented list of changes. It's a extract of Pico's [`CHANGELOG.md`][Changelog]. Please note that Pico's `CHANGELOG.md` isn't supposed to be read as-is; it's rather an supplement to the actual code changes.

If you have a question about one of the new features of Pico 3.0, please check out the ["Getting Help" section][GettingHelp] of the docs and don't be afraid to open a new [Issue][Issues] on GitHub.

<div class="toggle">
<h4 class="title">Pico's <code>CHANGELOG.md</code></h4>
<div class="togglebox">
<div markdown="1">

```
* [New] Pico 3.0 is a major release, but comes with relatively small changes
        to Pico's core; its major change are updated dependencies (see below)
* [New] Introduce API version 4 (with barely noticable changes, see below)
* [New] Add new continous integration (CI) pipeline using GitHub Actions
* [New] Add new build script and Makefile to simplify Pico's build and release
        process; see `CONTRIBUTING.md` for details
* [New] Add `%page_id%`, `%page_url%` and `%page_path%` Markdown placeholders
        to replace the current page's ID, URL, and containing directory resp.
* [New] `Pico::prepareFileContent()` and `Pico::substituteFileContent()` both
        now receive the (optional) `$pageId` argument for the new `%page_*%`
        Markdown placeholders
* [Changed] ! Pico now requires PHP 7.2.5 or later (this includes full PHP 8
            support, also see #528, #534, #608)
* [Changed] ! Pico now depends on Twig 3.3, skipping Twig 2.x altogether; this
            is a BC-breaking change, as Twig 2.x and 3.x changed and removed
            some commonly used features; check out Twig's changelog and
            deprecation notices for details
* [Changed] ! Pico now depends on Symfony YAML 5.4, skipping various milestones
            in between; this is a BC-breaking change, because Symfony YAML
            changed its behaviour multiple times in between; check out Symfony
            YAML's changelog for details
* [Changed] ! Pico downgrades to Parsedown 1.7.4 and Parsedown Extra 0.8.1;
            this is a BC-breaking change in theory, but shouldn't have much of
            an impact in real-life scenarios
* [Changed] #603: Pico's `markdown` Twig filter now raises an error if an
            invalid variable type (e.g. an array) is passed
* [Changed] Enable PHP strict typing for Pico's internal classes; Pico's
            `PicoPluginInterface` interface and `AbstractPicoPlugin` class
            don't use strict typing to maintain BC, but you can (and should)
            enable it for your plugin (see `DummyPlugin` for an example)
* [Changed] Various other code improvements due to the upgrade to PHP 7.2
* [Fixed] #602: Fix contents and meta data of meta pages (pages starting with
          an `_`) getting replaced by the 404 page when being requested
* [Fixed] Add a proper error message for a missing theme directory
* [Removed] ! Remove Pico's `map` Twig filter; it conflicts with Twig's `map`
            filter and can be replaced by Twig's `column` or `map` filter
```

</div>
</div>
</div>

[UpgradeTwig]: #upgrade-to-twig-33
[UpgradeYaml]: #upgrade-to-symfony-yaml-54
[UpgradeParsedown]: #downgrade-to-parsedown-17
[UpgradeMisc]: #everything-else-that-was-happening

[GettingHelp]: {{ site.github.url }}/docs/#getting-help
[Issues]: {{ site.gh_project_url }}/issues
[Changelog]: {{ site.gh_project_url }}/blob/{{ page.gh_release }}/CHANGELOG.md

[Upgrade]: {{ site.github.url }}/docs/#upgrade
[UpgradePico10]: {{ site.github.url }}/in-depth/upgrade-pico-10
[UpgradePico20]: {{ site.github.url }}/in-depth/upgrade-pico-20
[UpgradePico21]: {{ site.github.url }}/in-depth/upgrade-pico-21
[LatestRelease]: {{ site.gh_project_url }}/releases/latest
[UrlRewriting]: {{ site.github.url }}/docs/#url-rewriting

[Twig]: https://twig.symfony.com/doc/3.x/
[TwigDeprecated1]: https://twig.symfony.com/doc/1.x/deprecated.html
[TwigDeprecated2]: https://twig.symfony.com/doc/2.x/deprecated.html
[Yaml]: https://symfony.com/doc/5.4/components/yaml.html
[YamlChangelog]: https://github.com/symfony/yaml/blob/5.4/CHANGELOG.md
[Parsedown]: https://parsedown.org/
[ParsedownExtra]: https://parsedown.org/extra/
[CommonMark]: https://commonmark.org/

[Composer]: https://getcomposer.org/
[PicoTheme]: https://github.com/picocms/pico-theme
[PicoDeprecated]: https://github.com/picocms/pico-deprecated
