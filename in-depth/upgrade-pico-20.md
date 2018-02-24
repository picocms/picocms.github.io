---
layout: docs
title: Upgrade to Pico 2.0
headline: Upgrade Pico 1.0 to Pico 2.0
description: Let us take you on a journey to Pico's next evolutionary stage!
toc:
    upgrading-to-pico-20: Upgrading to Pico 2.0
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

2. Empty your installation directory and [install Pico just ordinarily][Install]. Don't forget about hidden files like `.htaccess`!

3. Copy `config/config.php` from your backup to your new Pico installation. It's not strictly necessary, but we highly recommend you to convert your PHP config file to a YAML config file (i.e. `config/config.yml`). Please refer to the ["Use YAML files to configure Pico" section][UpgradeConfig] below for more details.

4. Copy both the `assets` (if present) and the `content` folder from your backup to Pico's installation directory. They are fully compatible, so no changes are necessary. 😊

5. If applicable, copy the folder of your custom theme within the backed `themes` directory to your Pico installation. We made various improvements to Pico's theme system, but the most important change (because it might break your theme) is that Pico now requires Twig templates to use the `.twig` file extension. So, if your custom theme still uses `.html` file extensions (like `index.html`), rename all templates to `*.twig`. Naturally this isn't the only change, but none of them is strictly necessary, though highly recommended. So, please refer to the ["Amazing new features for theme developers" section][UpgradeThemes] below for more details.

6. Provided that you're using plugins, copy all of your plugins from the backed `plugins` directory to your Pico installation. Don't copy the outdated files `00-PicoDeprecated.php`, `01-PicoParsePagesContent.php`, `02-PicoExcerpt.php` and `DummyPlugin.php`. Pico 2.0 heavily improves the plugin system and unfortunately also introduces some backwards-incompatible changes. The most important change is that Pico no longer tries to interpret any file in the `plugins` directory as plugin, but this also means that it might ignore a plugin or stumble over a superfluous file. If all of your plugins consist of just a single file (i.e. `plugins/<plugin name>.php`), you're likely good to go. However, if you're copying folders to your `plugins` directory, please note that Pico now only interprets `plugins/<plugin name>/<plugin name>.php` as plugin. If one of your plugins stops working after upgrading to Pico 2.0, please refer to the plugin developer - the plugin needs to be updated. Please refer to the ["Use Pico's next generation plugin system" section][UpgradePlugins] below for more details.

## Use YAML files to configure Pico

Configuring Pico is now easier than ever before. Up to this point, Pico used a regular PHP file, the programming language Pico is written in, for its configuration (`config/config.php`). However, dealing with PHP isn't "stupidly simple", especially for non-developers: It has some pretty strict and unapparent syntax rules, and just the slightest typo results in a PHP error and breaking your whole website. That's not what we want!

For this reason we're happy to anounce YAML config files! As a Pico user you know YAML from the YAML header of your content files, thus it was the obvious choice. But we didn't stop there. Rather than having just a single config file, you can now use a arbitrary number of config files. Simply create a `.yml` file in Pico's `config` dir and you're good to go. This allows you to add some structure to your configuration, like a separate config file for your theme (`config/my_theme.yml`).

Just take a look at the `config/config.yml.template` and create your own config file `config/config.yml`. Even though we still support the old PHP config file (`config/config.php`), we highly recommend you to replace it by a appropiate `config/config.yml`. We will likely drop support of `config/config.php` in Pico's next major release.

Please note that Pico loads config files in a special way you should be aware of. First of all it loads the main config file `config/config.yml`, and then any other `*.yml` file in Pico's `config` dir in alphabetical order. The file order is crucial: Configiguration values which have been set already, cannot be overwritten by a succeeding file. For example, if you set `site_title: Pico` in `config/a.yml` and `site_title: My awesome site!` in `config/b.yml`, your site title will be "Pico".

## Amazing new features for theme developers

Theme developers have always been a subject close to our hearts, and with Pico 2.0 we make them more powerful than ever before. Sure, we love Pico's plugin system, but let's face it: PHP isn't as easy as Twig. Thus we introduce two amazing new features, allowing theme developers to do things only plugin developers could do before: Accessing HTTP GET and HTTP POST parameters, and accessing pages using a tree structure.

Starting with Pico 2.0 you can use the `url_param` and `form_param` Twig functions to access HTTP GET (i.e. a URL's query string like `?some-variable=my-value`) and HTTP POST (i.e. data of a submitted form) parameters. This allows you to implement things like pagination, tags and categories, dynamic pages, and even more - with pure Twig! Simply head over to our [introductory page for accessing HTTP parameters][FeaturesHttpParams] for details.

[GettingHelp]: {{ site.github.url }}/docs/#getting-help
[Issues]: {{ site.gh_project_url }}/issues
[Install]: {{ site.github.url }}/docs/#install
[UpgradeConfig]: #use-yaml-files-to-configure-pico
[UpgradeThemes]: #amazing-new-features-for-theme-developers
[UpgradePlugins]: #use-picos-next-generation-plugin-system
[FeaturesHttpParams]: {{ site.github.url }}/in-depth/features/http-params/
