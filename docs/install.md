---
layout: docs
title: Install Pico
headline: Install Pico
#description: |
toc:
  install-pico: Install Pico
  create-content: Create Content
  design-themes: Design Themes
  code-plugins: Code Plugins
nav-url: /docs/
nav: 3
---

<i class="icon-terminal big-icon"></i>

```
$ curl -sS https://getcomposer.org/installer | php
$ php composer.phar install
```

---

<i class="icon-folder big-icon"></i>

Download the [Latest Release]({{ site.gh_project_url }}/releases/latest).

Extract to the `httpdocs` directory (e.g. `/var/www/html`) of your server.

---

Webserver must be running **at least PHP 5.3+**.
