---
toc:
    install:
        _title: Install
        i-want-to-use-composer: Using Composer
        i-want-to-use-a-pre-bundled-release: Using a pre-bundled release
        im-a-developer: I'm a developer
nav: 1
---

## Install

Installing Pico is dead simple - and done in seconds! If you have access to a shell on your server (i.e. SSH access), we recommend using [Composer][]. If not, use a pre-bundled release. If you don't know what "SSH access" is, head over to the pre-bundled release. 😇

Pico requires PHP 5.3.6+

### I want to use Composer

Starting with Pico 2.0 we recommend installing Pico using Composer whenever possible. Trust us, you won't regret it when it comes to upgrading Pico! Anyway, if you don't want to use Composer, or if you simply can't use Composer because you don't have access to a shell on your server, don't despair, installing Pico using a pre-bundled release is still easier than everything you know!

#### Step 1

Open a shell and navigate to the desired install directory of Pico within the `httpdocs` directory (e.g. `/var/www/html`) of your server. Download Composer and run it with the `create-project` option:

```shell
$ curl -sSL https://getcomposer.org/installer | php
$ php composer.phar create-project picocms/pico-composer .
```

#### Step 2

What second step? There's no second step. That's it! Open your favorite web browser and navigate to your brand new, stupidly simple, blazing fast, flat file CMS! Pico's default contents will explain how to create your own contents. 😊

### I want to use a pre-bundled release

Do you know the feeling: You want to install a new website, so you upload all files of your favorite CMS and run the setup script - just to find out that you forgot about creating the SQL database first? Later the setup script tells you that the file permissions are wrong. Heck, what does this even mean? Forget about it, Pico is different!

#### Step 1

[Download the latest Pico release][LatestRelease] and upload all files to the `httpdocs` directory (e.g. `/var/www/html`) of your server.

#### Step 2

Okay, here's the catch: There's no catch. That's it! Open your favorite web browser and navigate to your brand new, stupidly simple, blazing fast, flat file CMS! Pico's default contents will explain how to create your own contents. 😊

### I'm a developer

So, you're one of these amazing folks making all of this possible? We love you guys! As a developer we recommend you to clone [Pico's Git repository][PicoGit] and use Composer to install its dependencies. You can find both [Pico][PicoPackagist] and [Pico's Composer starter project][PicoComposerPackagist] on [Packagist.org][Packagist]. Using Pico's Git repository is different from using one of the installation methods elucidated above, because it uses Pico as the Composer root package. Furthermore it gives you the current development version of Pico, what is likely *unstable* and *not ready for production use*!

Open a shell and navigate to the desired install directory of Pico within the `httpdocs` directory (e.g. `/var/www/html`) of your server. You can now clone Pico's Git repository, download Composer and install Pico's dependencies as follows:

```shell
$ git clone {{ site.gh_project_url }}.git .
$ curl -sSL https://getcomposer.org/installer | php
$ php composer.phar install
```

[Composer]: https://getcomposer.org/
[LatestRelease]: {{ site.gh_project_url }}/releases/latest
[PicoGit]: {{ site.gh_project_url }}
[PicoPackagist]: https://packagist.org/packages/picocms/pico
[PicoComposerPackagist]: https://packagist.org/packages/picocms/pico-composer
[Packagist]: https://packagist.org/
