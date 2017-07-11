---
layout: docs
title: Installing and Configuring Pico
headline: Installing and Configuring Pico
description: |
  Installing Pico couldn't be easier.<br />
  This guide will cover everything you need to know to get Pico up and running.
toc:
  downloading-and-installing-pico:
    _title: Downloading and Installing Pico
    developer-instructions: Developer Instructions
  url-rewriting:
    _title: URL Rewriting
    apache: Apache
    nginx: Nginx
  configuring-pico: Configuring Pico
nav-url: /guide/
nav: 4
galleries:
  standalone_1:
    style: magnify
    images:
      -
        heading: Installation is Easy!
        description: |
          Simply upload Pico's files to your server and you're done!<br>
          This is what Pico's folder looks like after installation.
        thumbnail: /style/images/docs/about/thumbnails/pico_folder.png
        fullsize: /style/images/docs/about/fullsize/pico_folder.png
        styles: "float: right; margin-left: 2em;"
  standalone_2:
    style: magnify
    images:
      -
        heading: Configuration is Easy Too!
        description: Configuring Pico is as simple as uncommenting a few lines in the included config template.
        thumbnail: /style/images/docs/about/thumbnails/config.png
        fullsize: /style/images/docs/about/fullsize/config.png
        styles: "float: right; margin-left: 2em;"
---

Installing Pico is as simple as uploading Pico's directory to your webserver.  Seriously.  But while we could leave it at that, this guide will dive deeper into the subject to cover all you'd ever need to know about the process.

If you are upgrading your Pico installation, please read our [Upgrading][] guide instead.

## Downloading and Installing Pico

{% include gallery.html gallery='standalone_1' %}

The best way to get yourself a copy of Pico is to download the [Latest Release][] from our GitHub Page.  This pre-bundled release comes with all the dependencies necessary for Pico to run.  The only system requirement of your webserver is that it's running **at least PHP 5.3 or newer**.

Once you have the latest release, simply upload all the extracted files to the `httpdocs` directory (e.g. `/var/www/html`) of your server.  If you are using Apache, make sure you upload our included `.htaccess` file for a worry-free configuration.  **Please Note**: Depending on your OS (specifically Linux and macOS), after you've extracted the files, `.htaccess` may appear hidden by your file manager.

* Include download button?

### Developer Instructions

Optionally, you could chose to install Pico using Git and Composer.  This method is more difficult, and targeted mainly at advanced users and developers.

Open a shell on your server and navigate to the location you'd like to install Pico.  From there, use Git to clone Pico's Git repository to your server:

```
$ git clone {{ site.gh_project_url }}.git
```

Note that this will give you a copy of Pico's `master` branch, which could be *unstable*.  In general, we would *not* consider this to be ready for production use.  Bugs may exist and breaks could occur, so use at your own risk.

Next, download [Composer][] and run it with the `install` option:

```
$ curl -sS https://getcomposer.org/installer | php
$ php composer.phar install
```

This will download and install all of the dependencies required to run Pico.

Pico is also available on [Packagist.org][] and may be included in other projects via `composer require picocms/pico`.

## URL Rewriting

Pico's default URL's are already pretty user friendly out of the box.  By default, they look like this: `http://example.com/pico/?sub/page`.  You'll note the `?` after the Pico directory.  This `?` in the URL is not ideal, and honestly a little "unsightly", especially on a production website.

This is where Pico's optional URL Rewriting comes in.  It takes the `?` out of the URL, turning `http://example.com/pico/?sub/page` into `http://example.com/pico/sub/page`.

To take advantage of this feature, you'll need a webserver that supports rewriting.  Nginx typically comes configured with a built-in `http_rewrite` module, while Apache requires `mod_rewrite` to be enabled.  If you are running Pico on a shared webhosting account, then support for these features depends on the way your individual hosting provider has configured their server.

Provided your server has rewrite functionality available, both Apache and Nginx can be configured to use it to rewrite Pico's URL's.

### Apache

If you're using Apache, you're in luck: Our included `.htaccess` file should take care of all the necessary configuration automatically.  If you receive an error message from your webserver, please make sure that the [`mod_rewrite` module][ModRewrite] is enabled.

If you find that, for whatever reason, rewritten URL's work properly but Pico is still inserting a `?` into your URL's, you can force Pico to use rewritten ones by setting `$config['rewrite_url'] = true;` in your [Pico Config](#configuring-pico) (`config/config.php`).

### Nginx [Learn more…][NginxConfig]{:.learn-more}
{:#nginx}

If you're using Nginx, configuration is a little more involved.  We have an entire [Nginx Guide][NginxConfig] to help walk you through the process.

The following configuration excerpt is somewhat of a "tl;dr" version of a very extensive topic.  It should provide the *bare minimum* configuration you need for Pico.  If you have any trouble, please read all of the aforementioned [Nginx Guide][NginxConfig].  For additional assistance, please refer to our ["Getting Help"][GettingHelp] page.

```
location ~ /pico/(\.htaccess|\.git|config|content|content-sample|lib|vendor|CHANGELOG\.md|composer\.(json|lock)) {
	return 404;
}

location ~ ^/pico(.*) {
	index index.php;
	try_files $uri $uri/ /pico/index.php?$1&$args;
}
```

Lines `1` to `3` of this example are for denying access to Pico's internal files.  This is recommend for security reasons, and is included in `.htaccess` for our Apache users.

Lines `5` to `8` provide the URL rewriting scheme.

You'll need to adjust the path of `/pico` on lines `1`, `5`, and `7` to match your own Pico installation directory.

Additionally, you'll need to either add `fastcgi_param PICO_URL_REWRITING 1;` to your [PHP location block][NginxPHP] or specify `$config['rewrite_url'] = true;` in your [Pico Config](#configuring-pico) (`config/config.php`).

## Configuring Pico

{% include gallery.html gallery='standalone_2' %}

Pico is ready to go, right out of the box.  However, unless you want your website to be called "Pico", there's at least one configuration option you'll want to set.

To configure Pico, start by navigating to the `config` directory and copying/moving Pico's included `config.php.template` to `config.php`.

Next, in your new `config.php`, you can specify the name of your website by changing the line:

`// $config['site_title'] = 'Pico';`

You'll also want to *uncomment* this line by removing the `//` at the beginning.

That's it!  That's the only configuration that's needed.  You'll find `config.php` is pretty well documented [inline][ConfigTemplate], so we won't go into detail about the rest of the options here.

Other interesting options you may want to configure include theme, date format, and page order.  A custom theme or plugin may have it's own options for you to add to `config.php`, so we've labeled a section at the bottom where you can add them.

---

## Up Next...

So, we've shown you everything you need to setup Pico on your server.  Now it's time for the fun stuff.  In the next guide, you'll learn how to create some content.  Click through to proceed to the next article.

<p class="aligncenter" markdown="1"><a href="{{ site.github.url }}/guide/creating-content" class="button">Creating Content</a></p>


[Upgrading]: {{ site.github.url }}/in-depth/upgrade/
[Latest Release]: {{ site.gh_project_url }}/releases/latest
[Composer]: https://getcomposer.org/
[Packagist.org]: http://packagist.org/packages/picocms/pico
[ModRewrite]: https://httpd.apache.org/docs/current/mod/mod_rewrite.html
[NginxConfig]: {{ site.github.url }}/in-depth/nginx/
[NginxPHP]: {{ site.github.url }}/in-depth/nginx/#php-configuration
[GettingHelp]: {{ site.github.url }}/docs/#getting-help
[ConfigTemplate]: {{ site.gh_project_url }}/blob/{{ site.gh_project_branch }}/config/config.php.template

{% comment %}

* Need to figure out how to make this button work using Markdown...

[NextGuide]: {{ site.github.url }}/docs/creating-content
<p class="aligncenter" markdown="1"> [Creating Content][NextGuide]{.button} </p>

---

## Testing Pico

* Is there really a use case for this?  Who has PHP installed but no webserver?  Maybe a PHP developer on their local box... but they'd probably already know about php's built-in server.  I don't see a point to including non-"production ready" instructions in this document.


## Run

You have nothing to consider specially, simply navigate to your Pico install using your favorite web browser. Pico's default contents will explain how to use your brand new, stupidly simple, blazing fast, flat file CMS.

### You don't have a web server?
Starting with PHP 5.4 the easiest way to *try* Pico is using PHP's own [built-in web server][PHPServer]. **Please Note** that PHP's built-in web server is for development and testing purposes only!  It is **not** suitable for production use.

* If you decide you like Pico, but need help setting up a more permanent solution, please see our [some section below about server configuration]().
* Also, only mentioning PHP's internal server seems a bit like a wasted page.  Do we even need a "Run" section?


* If they don't have a web server, perhaps we should recommend one or link to a guide for setting up their own.
* I feel like not using PHP's internal web server should be more emphasized... or maybe not even recommended in the first place...
* Wait... if they don't have a web server... why do they have PHP?  Why not just recommend downloading Apache or something?*
  * To be honest, I have no idea what installing Apache on Windows would look like.  I guess you'd probably need to download PHP separately or something.  Sometimes I forget how backwards they do things. ;P

#### Step 1
Navigate to Pico's installation directory using a shell.

#### Step 2
Start PHPs built-in web server:
<pre><code>$ php -S 127.0.0.1:8080</code></pre>

#### Step 3
Access Pico from http://localhost:8080.

[PHPServer]: http://php.net/manual/en/features.commandline.webserver.php

---
---

Here's some quotes from that thread:

    Page Topics

        An "Installing Pico" page that covers, in-depth, the concepts of installing and configuring Pico, your webserver (well, links to external resources anyway) and etc. Everything up until you're looking at Pico's sample content.
        A "Creating Content" page that goes over YAML and Markdown.
        Perhaps a general, "Usage" type page that covers things like how URLs work, how to lay out and organize your website, separating content and assets, more about config options, etc.
        Customization: An in-depth guide to installing community themes, creating your own, understanding Twig, Pico variables, everything. Not plugins though.
        A separate page for Plugins (and plugin development).
        Maybe a combination of "Getting Help" and "Contributing" (or "Getting Involved"). It should be a generally community-focused page. On that note, I think it would be great if we eventually had a place for non-development, non-bug related community discussions. I don't want to say a "forum" because that feels like a really outdated notion, but a "place" to develop a community nonetheless.

    Table of Contents

    When on the Overview Page:

    * Installing Pico
    * Creating Content
    * Customization
    * Getting Help

    When on Creating Content Page:

    * Installing Pico
    * Creating Content
       * YAML
       * Markdown
       * Etc
    * Customization
    * Getting Help

* Hmm, [GettingHelp][] is going to be refereed to a LOT... maybe I should write that ahead of time.

{% endcomment %}
