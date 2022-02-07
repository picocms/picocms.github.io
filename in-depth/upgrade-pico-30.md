---
layout: docs
title: Upgrade to Pico 3.0
headline: Upgrade Pico 2.1 to Pico 3.0
description: A small update with a lot of external changes!
toc:
    twig-updates-times-two: Twig Updates Times Two
    yaml-changes-and-urls: YAML Changes and URLs
    parsedown-downgrade: Parsedown… downgrade
    upgrading-to-pico-30:
      _title: Upgrading to Pico 3.0
      pre-bundled: Pre-Bundled
      composer: Composer
      additional-concerns: Additional Concerns
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

Pico 3.0 is now the latest release of PicoCMS.  3.0 is a major version update, but brings with it only minor changes to Pico's core.  What it does bring it might be the biggest "minor change" Pico's had in a while, with majorly updated dependencies.  These updates include switching to Twig v3.x (from v1.x!), Symfony YAML v5.x (from v2.8!), but also a slight step back from parsedown v1.8-beta to the stable 1.7 line.

The main reason for these updated dependencies is to finally add support for PHP 8.0.  This should be great news to the majority of Pico users, however, it does also mean Pico now **requires PHP 7.2.5** or newer and that we will no longer be supporting older PHP 5 based environments.

If you have any questions about Pico 3.0, the upgrade process, or if you experience compatibility issues with the upgrade, please see the ["Getting Help" section][GettingHelp] of the docs, and don't be afraid to open a new [Issue][Issues] on GitHub.

## Twig Updates Times Two

Skipping 2.0 altogether, Pico now ships with the Twig 3.x line.  This will hopefully be a smooth transition, but there could be some Theme incompatibility since we have not just [one](https://twig.symfony.com/doc/1.x/deprecated.html), but [two](https://twig.symfony.com/doc/2.x/deprecated.html) rounds of **Deprecated Features** to be on the lookout for.

This also brings a bit of new Twig functionality to Pico though.  If you've ever run into an unsupported Twig filter or function due to Pico's older Twig version, now's the time to try it again!

If you encounter issues with existing Pico Themes, be sure to let their respective developers know (as well as pointing them toward this page)!


## YAML Changes and URLs

Pico now uses the current v5.x version of Symfony YAML.  This change should go mostly unnoticed, however if you have issues with your YAML after updating, you should check Symfony YAML's [Changelog](https://github.com/symfony/yaml/blob/5.4/CHANGELOG.md).

**Of Important Note** to Pico users however is that starting an unquoted string with `%` is no longer allowed!

This means that any urls in your YAML metadata that start with a variable (eg `%base_url%`, `%assets_url%`), must now be quoted! For example:

```yml
Logo: "%assets_url%/img/pico-white.svg"
```

If you were doing this already, you won't notice a change.  But, if you haven't been using quotes, you'll likely have to search out and fix these.

If your editor has a "Find in Files" feature, you can probably find them all with a simple search for `: %` in your content folder.

## Parsedown... downgrade

Due to the long development time on Parsedown's version 1.8, which is still in beta, we've decided to revert to using Parsedown 1.7.x for the time being.  This shouldn't have much of an impact on Pico users, however we do recommend you give your content files a quick look over to make sure they still render correctly in the older version.

---

## Upgrading to Pico 3.0

Upgrading Pico is as easy as always.  Before you begin, make sure to **create a backup of your Pico installation** in case you encounter problems!  Always better to be safe than sorry.

### Pre-Bundled

If you're using a Pre-Bundled release, the upgrade process is the same as a fresh install.  Start by grabbing the [new version][LatestRelease] from GitHub.  Move or rename your old Pico folder.  Extract the files from the new release into an empty folder, and name it as desired.  Migrate over your personal files (config, content, assets, themes, and plugins) to the new Pico install and you're good to go.

While you're at it, you might want to check if any of your Themes and Plugins have been updated.  If you encounter any issues with them, be sure to contact their respective developers and let them know!

### Composer

If you installed via Composer, you can upgrade with one command.  Just navigate to your Pico directory and run:

```shell
$ php composer.phar update
```

That's it!  If you have any manually installed Themes or Plugins (as in, *not* installed via Composer), you might want to check if any of them have been updated.  If you encounter any issues with Themes or Plugins, be sure to contact their respective developers and let them know!

### Additional Concerns

As mentioned above, there may be some **compatibility issues** to be on the lookout for this time around.  Since we're upgrading Twig by *two* versions in this release, it might introduce bugs or errors with some Themes.  Be sure to contact the Theme's developer if you discover an incompatibility!  (In the meantime, feel free to inquire on [GitHub][Issues], and we'll see if we can help you through it.)

The upgrade to Symfony YAML might cause you issues if you haven't been quoting your URLs, so get some quotes wrapped around those!

And finally, the Parsedown downgrade might effect the rendering of your content in some edge cases, so give your pages a quick look-over to make sure your content is all still good.

[LatestRelease]: {{ site.gh_project_url }}/releases/latest
[GettingHelp]: {{ site.github.url }}/docs/#getting-help
[Issues]: {{ site.gh_project_url }}/issues