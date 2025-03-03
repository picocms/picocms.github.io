---
toc:
    install:
        _title: Install
        i-want-to-use-composer: Using Composer
        i-want-to-use-a-pre-bundled-release: Using a pre-bundled release
        im-a-developer: I'm a developer
components:
    PicoTheme: https://github.com/picocms/pico-theme
    PicoDeprecated: https://github.com/picocms/pico-deprecated
    PicoComposer: https://github.com/picocms/pico-composer
nav: 1
---

## Install

Installing Pico is dead simple - and done in seconds! If you have access to a shell on your server (i.e. SSH access), we recommend using [Composer][]. If not, use a pre-bundled release. If you don't know what "SSH access" is, head over to the pre-bundled release. 😇

Pico requires PHP 5.3.6+ and the PHP extensions `dom` and `mbstring` to be enabled.

### I want to use Composer

Starting with Pico 2.0 we recommend installing Pico using Composer whenever possible. Trust us, you won't regret it when it comes to upgrading Pico! Anyway, if you don't want to use Composer, or if you simply can't use Composer because you don't have access to a shell on your server, don't despair, installing Pico using a pre-bundled release is still easier than everything you know!

#### Step 1

Open a shell and navigate to the `httpdocs` directory (e.g. `/var/www/html`) of your server. Download Composer and run it with the `create-project` option to install it to the desired directory (e.g. `/var/www/html/pico`):

```shell
$ curl -sSL https://getcomposer.org/installer | php
$ php composer.phar create-project picocms/pico-composer pico
```

#### Step 2

What second step? There's no second step. That's it! Open your favorite web browser and navigate to your brand new, stupidly simple, blazing fast, flat file CMS! Pico's sample contents will explain how to create your own contents. 😊

### I want to use a pre-bundled release

Do you know the feeling: You want to install a new website, so you upload all files of your favorite CMS and run the setup script - just to find out that you forgot about creating the SQL database first? Later the setup script tells you that the file permissions are wrong. Heck, what does this even mean? Forget about it, Pico is different!

#### Step 1

[Download the latest Pico release][LatestRelease] and upload all files to the desired install directory of Pico within the `httpdocs` directory (e.g. `/var/www/html/pico`) of your server.

#### Step 2

Okay, here's the catch: There's no catch. That's it! Open your favorite web browser and navigate to your brand new, stupidly simple, blazing fast, flat file CMS! Pico's sample contents will explain how to create your own contents. 😊

### I want to manage my website using a Git repository

Git is a very powerful distributed version-control system - and it can be used to establish a nice workflow around your Pico website. Using a Git repository for your website aids content creation and deployment, including collaborative editing and version control. If you want to manage your website in a Git repository, you use a Composer-based installation.

1. Fork [Pico's Composer starter project][PicoComposerGit] using [GitHub's fork button][HelpFork]. If you don't want to use GitHub you aren't required to, you can choose whatever Git server you want. Forking manually just requires some extra steps: First clone the Git repository locally, add your Git server as a remote and push the repository to this new remote.

2. Clone your fork locally and add your contents and assets. You can edit Pico's `composer.json` to include 3rd-party plugins and themes, or simply add your own plugins and themes to Pico's `plugins` resp. `themes` directories. Don't forget to commit your changes and push them to your Git server.

3. Open a shell on your webserver and navigate to the `httpdocs` directory (e.g. `/var/www/html`). Download Composer, clone your Git repository to the desired directory (e.g. `/var/www/html/pico`) and install Pico's dependencies using Composer's `install` option:

    ```shell
    $ curl -sSL https://getcomposer.org/installer | php
    $ git clone https://github.com/<YOUR_USERNAME>/<YOUR_REPOSITORY> pico
    $ php composer.phar --working-dir=pico install
    ```

4. If you update your website's contents, simply commit your changes and push them to your Git server. Open a shell on your webserver and navigate to Pico's install directory within the `httpdocs` directory (e.g. `/var/www/html/pico`) of your server. Pull all changes from your Git server and update Pico's dependencies using Composer's `update` option:

    ```shell
    $ git pull
    $ php composer.phar update
    ```

### I'm a developer

So, you're one of these amazing people making all of this possible? We love you folks! As a developer we recommend you to clone [Pico's Git repository][PicoGit] as well as the Git repositories of [Pico's default theme][PicoThemeGit] and the [`PicoDeprecated` plugin][PicoDeprecatedGit]. You can set up your workspace using [Pico's Composer starter project][PicoComposerGit] and include all of Pico's components using local packages.

Using Pico's Git repositories is different from using one of the installation methods elucidated above. It gives you the current development version of Pico, what is likely *unstable* and *not ready for production use*!

1. Open a shell and navigate to the desired directory of Pico's development workspace within the `httpdocs` directory (e.g. `/var/www/html/pico`) of your server. Download and extract Pico's Composer starter project into the `workspace` directory:

    ```shell
    $ curl -sSL {{ page.components.PicoComposer }}/archive/master.tar.gz | tar xz
    $ mv pico-composer-master workspace
    ```

2. Clone the Git repositories of all Pico components (Pico's core, Pico's default theme and the `PicoDeprecated` plugin) into the `components` directory:

    ```shell
    $ mkdir components
    $ git clone {{ site.gh_project_url }}.git components/pico
    $ git clone {{ page.components.PicoTheme }}.git components/pico-theme
    $ git clone {{ page.components.PicoDeprecated }}.git components/pico-deprecated
    ```

3. Instruct Composer to use the local Git repositories as replacement for the `picocms/pico` (Pico's core), `picocms/pico-theme` (Pico's default theme) and `picocms/pico-deprecated` (the `PicoDeprecated` plugin) packages. Update the `composer.json` of your development workspace (i.e. `workspace/composer.json`) accordingly:

    ```json
    {
        "repositories": [
            {
                "type": "path",
                "url": "../components/pico",
                "options": { "symlink": true }
            },
            {
                "type": "path",
                "url": "../components/pico-theme",
                "options": { "symlink": true }
            },
            {
                "type": "path",
                "url": "../components/pico-deprecated",
                "options": { "symlink": true }
            }
        ],
        "require": {
            "picocms/pico": "dev-master",
            "picocms/pico-theme": "dev-master",
            "picocms/pico-deprecated": "dev-master",
            "picocms/composer-installer": "^1.0"
        }
    }
    ```

4. Download Composer and run it with the `install` option:

    ```shell
    $ curl -sSL https://getcomposer.org/installer | php
    $ php composer.phar --working-dir=workspace install
    ```

You can now open your web browser and navigate to Pico's development workspace. All changes you make to Pico's components will automatically be reflected in the development workspace.

By the way, you can also find all of Pico's components on [Packagist.org][Packagist]: [Pico's core][PicoPackagist], [Pico's default theme][PicoThemePackagist], the [`PicoDeprecated` plugin][PicoDeprecatedPackagist] and [Pico's Composer starter project][PicoComposerPackagist].

[Composer]: https://getcomposer.org/
[LatestRelease]: {{ site.gh_project_url }}/releases/latest
[PicoGit]: {{ site.gh_project_url }}
[PicoThemeGit]: {{ page.components.PicoTheme }}
[PicoDeprecatedGit]: {{ page.components.PicoDeprecated }}
[PicoComposerGit]: {{ page.components.PicoComposer }}
[Packagist]: https://packagist.org/
[PicoPackagist]: https://packagist.org/packages/picocms/pico
[PicoThemePackagist]: https://packagist.org/packages/picocms/pico-theme
[PicoDeprecatedPackagist]: https://packagist.org/packages/picocms/pico-deprecated
[PicoComposerPackagist]: https://packagist.org/packages/picocms/pico-composer
[HelpFork]: https://help.github.com/en/github/getting-started-with-github/fork-a-repo
