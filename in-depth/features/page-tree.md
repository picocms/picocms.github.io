---
layout: docs
title: Page Tree | Pico Features
headline: Using Pico's page tree
description: Recursion isn't that hard...
nav-url: /docs/
gh_release: v2.0.0
---

Did you ever wanted to use a dropdown for your page navigation? It never was that easy! Starting with Pico 2.0 you can access a page's tree node using the `tree_node` element of the page data array (e.g. `{% raw %}{{ current_page.tree_node }}{% endraw %}`). But first you've to understand how Pico's page tree is built up. A picture is worth a thousand words, so before we start explaining the details here's a simple example of how a page tree looks in practice. Just think of the following Markdown files in your `content` directory:

```
content/
├── shop/
│   ├── flowers/
│   │   ├── daisies.md
│   │   ├── red-roses.md
│   │   └── tiger-lilies.md
│   ├── index.md
│   └── shipping.md
├── index.md
└── contact.md
```

As you can see you'll end up having a website with the pages `index`, `contact`, `shop/index`, `shop/shipping`, `shop/flowers/daisies`, `shop/flowers/red-roses` and `shop/flowers/tiger-lilies`. The corresponding page tree looks like the following:

```
root node `/`
├── inner node `shop`
│   ├── inner node `flowers`
│   │   ├── leaf node `daisies`
│   │   ├── leaf node `red-roses`
│   │   └── leaf node `tiger-lilies`
│   └── leaf node `shipping`
└── leaf node `contact`
```

Let's take a look at this. The first thing we've to explain is the terminology: Nodes with children are called "inner nodes", whereas nodes without children are called "leaf nodes". The second thing you might notice is that the pages `index` and `shop/index` seem to be gone. But they aren't gone, `index` is rather represented by the root node `/`,  and `shop/index` is represented by the inner node `shop`. Did you notice that there seems to be no difference between the inner nodes `shop` and `flowers`, even though there's no `shop/flowers/index` page? That's because nodes aren't pages. While all leaf nodes do represent a page (e.g. the leaf node `contact` represents the page `contact`), inner nodes may or may not represent a page. So, if there's a `shop/flowers/daisies.md`, but neither a `shop/flowers/index.md` nor a `shop/flowers.md`, the inner node `flowers` represents no page.

You should always keep this in mind when working with Pico's page tree. If you access a tree node, you get an array with the keys `id`, `page` and `children`. The `id` key contains a string with the node's name. If the node represents a page, the `page` key is a reference to the page's data array. If the node is a inner node, the `children` key is a list of the node's child nodes. The order of a node's children matches the order in Pico's pages array.

You can access a page's tree node using the `tree_node` key in the page's data array. So, to access the root node, use `{% raw %}{{ pages["index"].tree_node }}{% endraw %}`. To access the current page's tree node, use `{% raw %}{{ current_page.tree_node }}{% endraw %}`.

But what can I do with Pico's page tree? Probably the most common task is to build a recursive page navigation like the following:

* Home
* Shop
  * Flowers
    * Daisies
    * Red Roses
    * Tiger Lilies
  * Shipping
* Contact

Implementing this using a [Twig macro](https://twig.symfony.com/doc/tags/macro.html) is pretty straight forward:

{% highlight html %}{% raw %}
{% macro tree(parent) %}
    {% import _self as utils %}
    {% for child in parent.children %}
        <li>
            {% if child.page %}
                <div>{{ child.page.title }}</div>
            {% endif %}
            {% if child.children %}
                <ul>
                    {{ utils.tree(child) }}
                </ul>
            {% endif %}
        </li>
    {% endfor %}
{% endmacro %}

<ul>
    <li>
        <div>{{ pages["index"].title }}</div>
    </li>

    {% import _self as utils %}
    {{ utils.tree(pages["index"].tree_node) }}
</ul>
{% endraw %}{% endhighlight %}

Although Pico's page tree primarily targets theme developers, plugin developers can benefit from the page tree, too. As for Twig you can access a page's tree node using the `tree_node` key in the page's data array (e.g. `$pageData['tree_node']`). As always, there's an event to access (and modify) Pico's page tree: the `onPageTreeBuilt(array &$pageTree)` event is triggered right after the `onCurrentPageDiscovered` event. You can later access the page tree using the [`Pico::getPageTree()` method]({{ site.github.url }}/phpDoc/{{ page.gh_release }}/classes/Pico.html#method_getPageTree), but don't forget about execution order: You can't access the page tree during the `onPagesDiscovered` event because it wasn't built yet. However, this also means that you can add pages to Pico's pages array during the `onPagesDiscovered` event and they will automatically appear in the page tree later. But how does the `$pageTree` array look like? It's a list of all the tree's branches (no matter the depth). Refer to the phpDoc class docs of the [`Pico::buildPageTree()` method]({{ site.github.url }}/phpDoc/{{ page.gh_release }}/classes/Pico.html#method_buildPageTree) for details.
