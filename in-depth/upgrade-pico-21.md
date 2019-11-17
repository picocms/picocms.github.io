---
layout: docs
title: Upgrade to Pico 2.1
headline: Upgrade to Pico 2.1
description: Pico 2.1 - small but mighty!
toc:
    pico-cms-for-nextcloud: Pico CMS for Nextcloud
    upgrading-to-pico-21: Upgrading to Pico 2.1
    use-picos-next-generation-themes: Use Pico's next generation themes
    everything-else-that-was-happening: Everything else that was happening…
    developer-news: Developer News
nav-url: /docs/
gh_release: v2.1.0
redirect_from:
    - /in-depth/upgrade/index.html
    - /upgrade/index.html
    - /upgrade.html
---

Pico 2.1 is a minor release - but minor doesn't mean it's not powerful! We took over development of the [Pico CMS for Nextcloud](#pico-cms-for-nextcloud) project and are happy to introduce [API versioning for themes](#use-picos-next-generation-themes). Making Pico simpler, faster, and more flexible than ever before! Best of all, it's completely backwards compatible!

If you have a question about one of the new features of Pico 2.1, the upgrade process, or about Pico in general, please check out the ["Getting Help" section][GettingHelp] of the docs and don't be afraid to open a new [Issue][Issues] on GitHub.

## Pico CMS for Nextcloud

With Pico 2.1 we took over development of the [Pico CMS for Nextcloud][NextcloudApp] app, the most powerful integration of Pico into another project ever existed. [Nextcloud][] is a free and open-source collaboration platform for creating and using file hosting services. Pico CMS for Nextcloud fully integrates Pico into Nextcloud and allows one to create simple, secure, shareable and amazingly powerful websites with just a few clicks.

* Start a blog
* Share your resume with the world
* Create a plan for world domination and only share with the right friends
* Build a knowledge base and let the smart ones among your colleagues help out

Pico CMS for Nextcloud allows your users to create and manage their own websites. Nextcloud is a very powerful collaboration platform, including WYSIWYG ("What you see is what you get") editors for Markdown. You can sync your website's sources with all your devices and even share access with others for collaborative editing. Creating private websites with limited access is possible, too.

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

3. Check all your custom plugins and themes whether there are updates available and follow the provided upgrade instructions to upgrade them. Pico 2.1 introduces the new API version 3 for both plugins and themes. However, Pico 2.1 is fully backwards-compatible to Pico 2.0 (using API version 2). This is achieved by Pico's official [`PicoDeprecated` plugin][PicoDeprecated]. The `PicoDeprecated` plugin is installed by default, so usually you don't have to do anything. However, if you've removed `PicoDeprecated` from your Pico installation, make sure to either upgrade all your plugins and themes to the latest API version 3, or install `PicoDeprecated` by following the plugin's install instructions.

Please take the opportunity to check whether your webserver is proberly configured, and access to Pico's internal files and directories is denied. Just refer to the ["URL Rewriting" section in the docs][UrlRewriting]. By following the instructions, you will not just enable URL rewriting, but also deny access to Pico's internal files and directories.

That's the *bare minimum* you need to know when upgrading to Pico 2.1. However, there's way more to know. Thus we highly recommend you to keep reading, the things you'll learn, will not disappoint you! For a complete list of what we have changed with Pico 2.1, please refer to our [`CHANGELOG.md`][Changelog].

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
[Composer]: https://getcomposer.org/
[LatestRelease]: {{ site.gh_project_url }}/releases/latest
[UrlRewriting]: {{ site.github.url }}/docs/#url-rewriting
[PicoDeprecated]: https://github.com/picocms/pico-deprecated
[Changelog]: {{ site.gh_project_url }}/blob/{{ page.gh_release }}/CHANGELOG.md
