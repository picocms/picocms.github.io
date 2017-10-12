---
layout: default
title: Code Statistics
headline: Code Statistics
description: Pico's code statistics are powered by [cloc](https://github.com/AlDanial/cloc).
nav-url: /docs/
---

## Pico's core

The following table shows code statistics for Pico's core in the version of the latest release, without all non-essential additions (like the `PicoDeprecated` plugin or Pico's default theme).

{% assign cloc = site.data.clocCore %}
{% include cloc.html %}

## Release package

The following table shows code statistics of everything that is shipped in Pico's latest release package, including all recommended additions.

{% assign cloc = site.data.clocRelease %}
{% include cloc.html %}
