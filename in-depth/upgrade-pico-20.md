---
layout: docs
title: Upgrade
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

[GettingHelp]: {{ site.github.url }}/docs/#getting-help
[Issues]: {{ site.gh_project_url }}/issues
[Install]: {{ site.github.url }}/docs/#install
[UpgradeConfig]: #use-yaml-files-to-configure-pico
[UpgradeThemes]: #amazing-new-features-for-theme-developers
[UpgradePlugins]: #use-picos-next-generation-plugin-system
