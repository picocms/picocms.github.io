---
toc:
    upgrade:
        _title: Upgrade
        ive-used-composer-to-install-pico: Using composer
        ive-used-a-pre-bundled-release-to-install-pico: Using a pre-bundled release
        im-a-developer: I'm a developer
nav: 2
---

## Upgrade [Learn more…][HelpUpgrade]{:.learn-more}
{:#upgrade}

Do you remember when you installed Pico? It was ingeniously simple, wasn't it? Upgrading Pico is no difference! The upgrade process differs depending on whether you used [composer][] or a pre-bundled release to install Pico. Please note that you should *always* create a backup of your Pico installation before upgrading!

Pico follows [Semantic Versioning 2.0][SemVer] and uses version numbers like `MAJOR`.`MINOR`.`PATCH`. When we update the `PATCH` version (e.g. `2.0.0` to `2.0.1`), we made backwards-compatible bug fixes. If we change the `MINOR` version (e.g. `2.0` to `2.1`), we added functionality in a backwards-compatible manner. Upgrading Pico is dead simple in both cases. Simply head over to the appropiate Upgrade sections below.

But wait, we forgot to mention what happens when we update the `MAJOR` version (e.g. `2.0` to `3.0`). In this case we made incompatible API changes. We will then provide a appropriate upgrade tutorial, so please head over to the ["Upgrade" page][HelpUpgrade].

### I've used composer to install Pico

Upgrading Pico is dead simple if you've used composer to install Pico. Simply open a shell and navigate to Pico's install directory within the `httpdocs` directory (e.g. `/var/www/html/pico`) of your server. You can now upgrade Pico using just a single command:

```shell
$ php composer.phar update
```

That's it! composer will automatically update Pico and all plugins and themes you've installed using composer. Please make sure to manually update all plugins and themes you've installed manually.

### I've used a pre-bundled release to install Pico

Okay, installing Pico was easy, but upgrading Pico is going to be hard, isn't it? I'm affraid I have to disappoint you... It's just as simple as installing Pico!

First you'll have to delete the `vendor` directory of your Pico installation (e.g. if you've installed Pico to `/var/www/html/pico`, delete `/var/www/html/pico/vendor`). Then [download the latest Pico release][LatestRelease] and upload all files to your existing Pico installation directory. You will be prompted whether you want to overwrite files like `index.php`, `.htaccess`, ... - simply hit "Yes".

That's it! Now that Pico is up-to-date, you need to update all plugins and themes you've installed.

### I'm a developer

As a developer you should be up-to-date already... 😉 For the sake of completeness, if you want to upgrade Pico, simply open a shell and navigate to Pico's install directory within the `httpdocs` directory (e.g. `/var/www/html/pico`) of your server. Then pull the latest commits from [Pico's Git repository][PicoGit] and let composer update your dependencies:

```shell
$ git pull
$ php composer.phar update
```

[composer]: https://getcomposer.org/
[SemVer]: http://semver.org
[HelpUpgrade]: {{ site.github.url }}/in-depth/upgrade/
[LatestRelease]: {{ site.gh_project_url }}/releases/latest
[PicoGit]: {{ site.gh_project_url }}
