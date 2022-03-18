---
heading: Pages List
categories:
  - theming
description: A nested pages list.
link: https://github.com/nliautaud/pico-pages-list
info:
  By: "[nliautaud](https://github.com/nliautaud)"
  License: "[The MIT License](https://github.com/nliautaud/pico-pages-list/blob/master/LICENCE.md)"
---

Create a nested HTML navigation tree in your theme using `{% raw %}{{ nested_pages | navigation }}{% endraw %}`.

Features:
- Nested hierarchy
- Supports page filtering
- Creates page links with page title or page ID
- Shows category slug if no index page exists
- Provides utility css classes (`.is-page`, `.is-directory`, `.is-current`, `.is-active`, `.has-childs`) for menu building or filtering
