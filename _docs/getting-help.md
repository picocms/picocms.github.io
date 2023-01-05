---
toc:
    getting-help:
        _title: Getting Help
        getting-help-as-a-user: ... for users
        getting-help-as-a-developer: ... for developers
        you-still-need-help-or-experience-a-problem-with-pico: ... if all else fails
nav: 6
---

## Getting Help

### Getting Help as a user

If you want to get started using Pico, please refer to the [user docs][HelpUserDocs] (you're reading them right now!). Please read the [upgrade notes][HelpUpgrade] if you want to upgrade to a new Pico version. You can find officially supported [plugins][OfficialPlugins] and [themes][OfficialThemes] here on our website. A greater choice of third-party plugins can be found in our [Wiki][WikiPlugins]. If you want to create your own plugin or theme, please refer to the "Getting Help as a developer" section below.

### Getting Help as a developer

If you're a developer, please refer to the ["Contributing" section][HelpDevContribute] below and our [contribution guidelines][ContributionGuidelines]. To get you started with creating a plugin or theme, check out Pico's [`DummyPlugin`][PicoDummyPlugin], and [Pico's default theme][PicoThemeGit].

### You still need help or experience a problem with Pico?

When the docs can't answer your question, you can get help by joining us on [#picocms on Libera.Chat][LiberaChat] ([logs][LiberaChatLogs]). When you're experiencing problems with Pico, please don't hesitate to create a new [Issue][Issues] on GitHub. Concerning problems with plugins or themes, please refer to the website of the developer of this plugin or theme.

**Before creating a new Issue,** please make sure the problem wasn't reported yet using [GitHubs search engine][IssuesSearch]. Please describe your issue as clear as possible and always include the *Pico version* you're using. Provided that you're using *plugins*, include a list of them too. We need information about the *actual and expected behavior*, the *steps to reproduce* the problem, and what steps you have taken to resolve the problem by yourself (i.e. *your own troubleshooting*).

[HelpUpgrade]: {{ site.github.url }}/in-depth/upgrade/
[HelpUserDocs]: {{ site.github.url }}/docs/
[HelpDevContribute]: {{ site.github.url }}/docs/#contributing
[OfficialPlugins]: {{ site.github.url }}/plugins/
[OfficialThemes]: {{ site.github.url }}/themes/
[WikiPlugins]: https://github.com/picocms/Pico/wiki/Pico-Plugins
[PicoThemeGit]: https://github.com/picocms/pico-theme
[PicoDummyPlugin]: https://github.com/picocms/Pico/blob/master/plugins/DummyPlugin.php
[Issues]: {{ site.gh_project_url }}/issues
[IssuesSearch]: {{ site.gh_project_url }}/search?type=Issues
[ContributionGuidelines]: {{ site.gh_project_url }}/blob/{{ site.gh_project_branch }}/CONTRIBUTING.md
[LiberaChat]: {{ site.github.url }}/irc
[LiberaChatLogs]: {{ site.github.url }}/irc-logs
