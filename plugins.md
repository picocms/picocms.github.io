---
layout: default
title: Plugins
headline: Pico Plugins
description: |
  Want to extend the functionality of Pico?  Below is a collection of our community-developed plugins.<br>
  Would you like to create your own plugin? Learn how in [our documentation](/docs/#plugins)!
nav: 5
redirect_from:
  - /customization/index.html
  - /customization.html
  - /plugins.html

portfolio:
  view: card
  categories:
    admin: Admin
    official: Official
    theming: Theming
    utility: Utility
  defaultThumbnail: plugins/images/thumbnails/plugin.png
  defaultImage: plugins/images/fullsize/plugin.svg

galleries:
  nextcloud:
    headline: Pico CMS for Nextcloud
    description: Create simple, secure, shareable and amazingly powerful websites with just a few clicks!
    style: carousel-box
    images:
      -
        heading: Manage your websites
        description: Managing your websites can't be easier - all in one place!
        thumbnail: /style/images/docs/pico-cms-for-nextcloud/thumbnails/list_websites.png
        fullsize: /style/images/docs/pico-cms-for-nextcloud/fullsize/list_websites.png
      -
        heading: Create new websites
        description: Creating a new personal website is just a few clicks away.
        thumbnail: /style/images/docs/pico-cms-for-nextcloud/thumbnails/new_website.png
        fullsize: /style/images/docs/pico-cms-for-nextcloud/fullsize/new_website.png
      -
        heading: Example website
        description: Pico CMS for Nextcloud utilized the brand new Pico 2.1.
        thumbnail: /style/images/docs/pico-cms-for-nextcloud/thumbnails/website.png
        fullsize: /style/images/docs/pico-cms-for-nextcloud/fullsize/website.png
      -
        heading: Manage custom themes
        description: Add custom themes for greater individuality and style.
        thumbnail: /style/images/docs/pico-cms-for-nextcloud/thumbnails/custom_themes.png
        fullsize: /style/images/docs/pico-cms-for-nextcloud/fullsize/custom_themes.png
      -
        heading: Manage custom plugins
        description: Add custom plugins to reach for Pico's full potential.
        thumbnail: /style/images/docs/pico-cms-for-nextcloud/thumbnails/custom_plugins.png
        fullsize: /style/images/docs/pico-cms-for-nextcloud/fullsize/custom_plugins.png
      -
        heading: Manage custom templates
        description: Make it easier for users to create new websites.
        thumbnail: /style/images/docs/pico-cms-for-nextcloud/thumbnails/custom_templates.png
        fullsize: /style/images/docs/pico-cms-for-nextcloud/fullsize/custom_templates.png
---

{% include portfolio.html portfolio=site.plugins %}

---

## Pico CMS for Nextcloud

Probably the most powerful Pico "plugin" is [Pico CMS for Nextcloud][NextcloudApp]. To be more precise, Pico CMS for Nextcloud is no plugin, it's an integration, the most powerful integration of Pico into another project ever existed. [Nextcloud][] is a free and open-source collaboration platform for creating and using file hosting services. Pico CMS for Nextcloud fully integrates Pico into Nextcloud and allows one to create simple, secure, shareable and amazingly powerful websites with just a few clicks.

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

[Nextcloud]: https://nextcloud.com/
[NextcloudApp]: https://apps.nextcloud.com/apps/cms_pico
[NextcloudDownload]: https://nextcloud.com/install/#instructions-server
[NextcloudInstall]: https://docs.nextcloud.com/server/stable/admin_manual/installation/
[NextcloudChat]: https://webchat.freenode.net/?channels=nextcloud
[NextcloudHelp]: http://help.nextcloud.com/
[NextcloudCredits]: https://github.com/daita
