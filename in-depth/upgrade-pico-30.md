---
layout: docs
title: Upgrade to Pico 3.0
headline: Upgrade Pico 2.1 to Pico 3.0
description: A small update with a lot of external changes!
toc:
    how-to-upgrade: How to upgrade
    upgrade-to-twig-3.3: Upgrade to Twig 3.3
    upgrade-to-symfony-yaml-5.4: Upgrade to Symfony YAML 5.4
    downgrade-to-parsedown-1.7: Downgrade to Parsedown 1.7
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

You want to learn more about Pico's latest major installment, Pico 3.0?  Pico 3.0 is a major release, but brings relatively small changes to Pico's core.  What it does bring might be the biggest "minor change" Pico had in a while: majorly updated dependencies.  These updates include switching to [Twig 3.3][TwigDocs] (from Twig 1.44), to [Symfony YAML 5.4][YamlDocs]) (from YAML 2.8), but also a slight step back from [Parsedown][] 1.8-beta to the stable Parsedown 1.7 line.

The main reason for these updates is to better support PHP 8.0.  This should be great news to the majority of Pico users, however, it does also mean that Pico now requires PHP 7.2.5 or newer and that Pico no longer supports (ancient) PHP 5 based environments.

If you have any questions about Pico 3.0, the upgrade process, or if you experience compatibility issues with the upgrade, please see the ["Getting Help" section][GettingHelp] of the docs, and don't be afraid to open a new [Issue][Issues] on GitHub.

## How to upgrade

Pico 3.0 is a major release, thus **it will likely break your website** at first.  Check out the changes below to learn what to change to fix the expected issues.  This documentation assumes that you've upgraded to Pico 2.1 before; if you're running an older version, check out the upgrade docs to their respective successor first ([to Pico 1.0][UpgradePico10], [to Pico 2.0][UpgradePico20], and [to Pico 2.1][UpgradePico21]).  No matter what, make sure to **create a backup of your Pico installation before upgrading**.  After that you can follow the regular [Upgrade instructions][Upgrade] for major releases.  For convenience, these instructions are also provided below:

1. **Create a backup of your Pico installation.**

2. Think about how you've installed Pico in the past. Did you use [Composer][] or one of Pico's pre-bundled releases?

    - If you've used Composer to install Pico, upgrading Pico itself is no more than running a single command.  Open a shell and navigate to Pico's install directory within the `httpdocs` directory (e.g. `/var/www/html/pico`) of your server.  You can now upgrade Pico using the following command:

        ```shell
        $ php composer.phar update
        ```

        That's it! Composer will automatically update Pico and all plugins and themes you've installed using Composer.

   - If you've used one of Pico's pre-bundled releases, the upgrade steps are dead simple, too.  First you'll have to delete the `vendor` directory of your Pico installation (e.g. if you've installed Pico to `/var/www/html/pico`, delete `/var/www/html/pico/vendor`).  Then [download the latest Pico release][LatestRelease] and upload all files to your existing Pico installation directory.  You will be prompted whether you want to overwrite files like `index.php`, `.htaccess`, ... - simply hit "Yes". That's it!

3. Check all your custom plugins and themes whether there are updates available and follow the provided upgrade instructions to upgrade them.  Pico 3.0 introduces the new API version 4 for both plugins and themes. However, Pico 3.0 is mostly backwards-compatible to Pico 2.1 (using API version 3) and earlier.  This is achieved by Pico's official [`PicoDeprecated` plugin][PicoDeprecated].  The `PicoDeprecated` plugin is installed by default, so usually you don't have to do anything.  However, if you've removed `PicoDeprecated` from your Pico installation before, make sure to either upgrade all your plugins and themes to the latest API version 4, or install `PicoDeprecated` by following the plugin's install instructions.

4. The most important change of Pico 3.0 are updated dependencies - namely [Twig 3.3][TwigDocs], to [Symfony YAML 5.4][YamlDocs]), and [Parsedown 1.7][Parsedown].  The new versions of Twig and YAML will very likely cause some issues for you!  Refer to the ["Upgrade to Twig 3.3"][UpgradeTwig], ["Upgrade to Symfony YAML 5.4"][UpgradeYaml], and ["Downgrade to Parsedown 1.7"][UpgradeParsedown] sections below to learn what has changed and how to fix the issues.

Please take the opportunity to check whether your webserver is proberly configured, and access to Pico's internal files and directories is denied.  Just refer to the ["URL Rewriting" section in the docs][UrlRewriting].  By following the instructions, you will not just enable URL rewriting, but also deny access to Pico's internal files and directories.

## Upgrade to Twig 3.3

Skipping Twig 2 altogether, Pico now ships with the [Twig 3.3][TwigDocs] template engine for theming.  This update will likely cause some issues, for a complete list please refer to the deprecated features as of [Twig 1.x][TwigDeprecated1] and [Twig 2.x][TwigDeprecated2].  Most changes affect plugin developers only, but a few changes might also break your custom theme, most notably the following:

* You can no longer add an `if` condition to the `for` loop (i.e. `{% raw %}{% for … in … if … %}{% endraw %}`), as previously used by Pico's default theme and many 3rd-party themes.  Use a `filter` filter or an `{% raw %}{% if … %}{% endraw %}` condition inside the `{% raw %}{% for … in … %}{% endraw %}` body instead.
* The `{% raw %}{% spaceless %}{% endraw %}` tag was deprecated in favour of the `spaceless` filter.  You can use `{% raw %}{% apply spaceless %}{% endraw %}` as an alternative.
* Speaking of the `{% raw %}{% apply … %}{% endraw %}` tag, the `{% raw %}{% filter … %}{% endraw %}` tag was deprecated in favour of the `{% raw %}{% apply … %}{% endraw %}` tag.
* The `sameas` and `divisibleby` tests are deprecated in favor of `same as` and `divisible by` tests respectively.
* The `{% raw %}{% raw %}{% endraw %}` tag (don't confuse this with the `raw` filter) was deprecated.  Use `{% raw %}{% verbatim %}{% endraw %}` instead.
* The `_self` global variable now returns the current template name instead of the current `\Twig\Template` object.  `_self` was often used to retrieve the current template's name, so simply replace `{% raw %}{{ _self.templateName }}{% endraw %}` by `{% raw %}{{ _self }}{% endraw %}`.

This also brings a bit of new Twig functionality to Pico though.  If you've ever run into an unsupported Twig filter or function due to Pico's older Twig version, now's the time to try it again!  Please refer to [Twig's documentation][TwigDocs] for a complete list of all features.

If you encounter issues with existing 3rd-party Pico themes, be sure to let their respective developers know and point them toward this page.

## Upgrade to Symfony YAML 5.4

Pico now utilizes the latest [Symfony YAML 5.4][YamlDocs]) release to parse YAML.  This change should go mostly unnoticed, but might still cause some minor issues with the YAML Frontmatters your Markdown files, or your config files.  Please check out Symfony's [changelog][YamlChangelog] for a complete list of changes.  Most notably the following might require some changes:

* Unquoted YAML values must no longer start with a percent sign (`%`), at sign (`@`), grave accent (```), vertical bar (`|`), or greater-than sign (`>`).  This will most likely cause some issues with Pico's URL placeholders (e.g. `%base_url%`, or `%assets_url%`), as you must now quote these strings.  For example, `Logo: %assets_url%/img/pico-white.svg` must be replaced by `Logo: "%assets_url%/img/pico-white.svg"`.
* When surrounding strings with double-quotes, you must now escape the backslash character (`\`; e.g. `class: "Foo\Bar"` must be replaced by `class: "Foo\\Bar"`).
* Symfony YAML streamlined the use of mappings (i.e. `key: value` pairs): You can no longer use non-string mapping keys (replace e.g. `123: integer` by `"123": integer`), as they now cause errors.  Duplicate mapping keys (i.e. using the same key twice) will now also cause an error.  Symfony YAML now additionally requires a whitespace after the colon (`:`) separating key and value of a mapping (replace e.g. `key:value` by `key: value`).
* You can no longer use mappings inside multi-line strings.
* Support for the comma (`,`) as a group separator for floats has been dropped.  Use the underscore (`_`) instead (i.e. use `1_234.56` instead of `1,234.56`).
* Symfony YAML changed the behaviour of various custom (non-standard) YAML tags (e.g. `!php/object`).  Check out the changelog for more details.

## Downgrade to Parsedown 1.7

In regards of Pico's Markdown parser - [Parsedown][] - we perform a downgrade to Parsedown 1.7.  We upgraded to Parsedown 1.8-beta with Pico 2.1 due to its major improvements regarding its improved support for [CommonMark][], but considering its long development time and abandonment in favour of Parsedown 2.0 (also still in development), we decided to switch back to the more commonly used stable Parsedown 1.7.  This shouldn't have much of an impact on Pico users, however we do recommend you give your content files a quick look over to make sure they still render correctly in the older Parsedown 1.7 version.

[UpgradeTwig]: #upgrade-to-twig-3.3
[UpgradeYaml]: #upgrade-to-symfony-yaml-5.4
[UpgradeParsedown]: #downgrade-to-parsedown-1.7

[GettingHelp]: {{ site.github.url }}/docs/#getting-help
[Issues]: {{ site.gh_project_url }}/issues

[Upgrade]: {{ site.github.url }}/docs/#upgrade
[UpgradePico10]: {{ site.github.url }}/in-depth/upgrade-pico-10
[UpgradePico20]: {{ site.github.url }}/in-depth/upgrade-pico-20
[UpgradePico21]: {{ site.github.url }}/in-depth/upgrade-pico-21
[LatestRelease]: {{ site.gh_project_url }}/releases/latest

[TwigDocs]: https://twig.symfony.com/doc/3.x/
[TwigDeprecated1]: https://twig.symfony.com/doc/1.x/deprecated.html
[TwigDeprecated2]: https://twig.symfony.com/doc/2.x/deprecated.html
[YamlDocs]: https://symfony.com/doc/5.4/components/yaml.html
[YamlChangelog]: https://github.com/symfony/yaml/blob/5.4/CHANGELOG.md
[Parsedown]: https://parsedown.org/
[CommonMark]: https://commonmark.org/

[PicoDeprecated]: https://github.com/picocms/pico-deprecated
