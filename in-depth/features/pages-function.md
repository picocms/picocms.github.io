---
layout: docs
title: Pages Function | Pico Features
headline: Using Pico's `pages()` function
description: Creating pages with Pico is stupidly simple - but how to access them?
toc:
    directory-path-meets-url: Directory path meets URL
    picos-page-tree: Pico's page tree
    using-picos-pages-function: Using Pico's `pages()` function
    including-the-whole-family: Including the whole family
    some-more-things-to-know: Some more things to know
nav-url: /docs/
gh_release: v2.1.0
---

Creating new pages with Pico definitely is stupidly simple: Simply create a `.md` file in Pico's `content` directory, add a YAML header and some Markdown content and you're good to go to admire your new page in your favorite browser. Navigating to a single page shows the contents of the matching `.md` file in Pico's `content` dir - for example, if you navigate to `https://example.com/pico/sub/page`, you'll see the contents of `content/sub/page.md`. Easy, isn't it? But what if you're creating your own theme and you want to access not just the requested page, but also the contents of any other page. The most basic example is your website's navigation: You surely want some sort of navigation in your theme, right? But how does this work?

## Directory path meets URL

You can access any other page using Pico's `pages()` Twig function. But first we have to take a step back. How does Pico manage its pages? Using some examples is better than writing a thousands words, so for the means of this tutorial just think of the following Markdown files in your `content` directory:

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

Every `.md` file in your `content` directory represents a single page (e.g. `https://example.com/pico/shop/shipping` shows `shop/shipping.md`). The `index.md` files have a special meaning: they will be shown if you navigate to the folder path. So for example, if you navigate to `https://example.com/pico/shop`, `shop/index.md` is being shown. This gets very important when dealing with Pico's `pages()` function: Even though `shop/index.md` and `shop/shipping.md` are in the same directory, `shop/index.md` is actually the *parent* page of `shop/shipping.md`. Just think of `shop/index.md` as it would be `shop.md`, both files would be accessible via `https://example.com/pico/shop` (by the way, if both files exist, `index.md` wins).

## Pico's page tree

Pico basically replicates your `content` directory and stores it in a tree structure - called *Pico's page tree*. As we explained before, `shop/index.md` (or `shop.md` otherwise) is the parent of `shop/shipping.md`. The same is true for `index.md`: It's not just the parent of both `shop/index.md` and `contact.md`, but also the "founder" of your website (i.e. your website's landing page at `https://example.com/pico/`). But what about the `shop/flowers` directory, there's neither a `shop/flowers/index.md` nor `shop/flowers.md`? If you navigate to `https://example.com/pico/shop/flowers` you'll see Pico's 404 error page. For Pico's page tree this doesn't really matter, `shop/flowers` exists, but just doesn't represent a page. Pico's page tree will look like the following:

```
node `/`
├── node `shop`
│   ├── node `flowers`
│   │   ├── node `daisies`
│   │   ├── node `red-roses`
│   │   └── node `tiger-lilies`
│   └── node `shipping`
└── node `contact`
```

If you work with Pico's `pages()` function, you work with Pico's page tree. Just keep this structure in mind: `index.md` files coalesce with their matching folder and if there's no `index.md`, Pico's page tree doesn't really care. All pages and folders are called *nodes*.

You want to learn more about Pico's page tree? You don't really have to know this if you just want to use Pico's `pages()` function, but it might be good to know. Head over to [Pico's page tree documentation][FeaturesPageTree] for more information.

## Using Pico's `pages()` function

If you use Pico's `pages()` function, you basically take a node's position and look at its environment. Just imagine you sit in your backyard. Everything is fine and you have your whole family over. Your parent, whose name was `/`, called you `shop`. A friend of yours comes by and asks about your children. You proudly list your two children: `flowers` (full name: `shop/flowers`) and `shipping` (full name: `shop/shipping`).

That's exactly what is happening if you call Pico's `pages()` function in a Twig template: It returns the child pages of the node whose position you take. You can pass the node's name whose position you want to take as first parameter. So for example, `pages("shop")` returns the data of page `shop/shipping.md` (it would also return `shop/flowers/index.md` if it existed). With the data of these pages you can do whatever you want, you might want to create a list of all the page titles:

{% raw %}<pre><code>&lt;ul&gt;
    {% for page in pages(&quot;shop&quot;) %}
        &lt;li&gt;&lt;a href=&quot;{{ page.url }}&quot;&gt;{{ page.title }}&lt;/a&gt;&lt;/li&gt;
    {% endfor %}
&lt;/ul&gt;</code></pre>{% endraw %}

This will result in the following HTML markup:

```
<ul>
    <li><a href="https://example.com/pico/shop/flowers">Flowers</a></li>
    <li><a href="https://example.com/pico/shop/shipping">Shipping</a></li>
</ul>
```

If you want to take the position of `flowers` (full name: `shop/flowers`), the `pages()` function (i.e. `{% raw %}{% for page in pages("shop/flowers") %}…{% endfor %}{% endraw %}`) will return the data of the pages `shop/flowers/daisies.md`, `shop/flowers/red-roses.md` and `shop/flowers/tiger-lilies.md`. But what if you want to take the position of `shipping` (full name: `shop/shipping`)? It has no children! No worries, `pages("shop/shipping")` simply returns nothing. If you don't pass any value to Pico's `pages()` function, the position of the root node is assumed and the data of the pages `shop/index.md` and `contact.md` is being returned.

You naturally don't have to add literal `pages("shop")` calls to your Twig template. Most of the time you don't want a literal `shop` (or any other literal name), but some variable instead. You should know this: All page identifiers are valid node names. For example, if you navigate to `https://example.com/pico/shop`, Pico's `current_page` variable will contain the data of the `shop/index.md` page - and the identifier of this page is stored in the `{% raw %}{{ current_page.id }}{% endraw %}` variable. It's value is `shop/index`. Pico's `pages()` function is intelligent enough to understand that you're talking about the `shop` node. So `{% raw %}{% for page in pages(current_page.id) %}…{% endfor %}{% endraw %}` is totally valid and returns the data of all child pages of the current page. If there are no child pages, it simply returns nothing.

## Including the whole family

Let's get back to your backyard and your lovely family. Another friend comes by and asks about your (you're `shop` if you have forgot) descendants. Again you're super proud and list your child `flowers` (full name: `shop/flowers`), your grandchildren `daisies` (full name: `shop/flowers/daisies`), `red-roses` (full name: `shop/flowers/red-roses`) and `tiger-lilies` (full name: `shop/flowers/tiger-lilies`), as well as your other child `shipping` (full name: `shop/shipping`).

That's what is happening if you pass the `depth` parameter to Pico's `pages()` function. If you don't pass this parameter, you ask for a node's children only. If you pass `depth=1` (like `{% raw %}{% for page in pages("shop", depth=1) %}…{% endfor %}{% endraw %}`), you ask for a node's children and grandchildren. If you pass any higher integer, you'll get even more generations of a node's descendants. If you want to return all descendants, pass `depth=null`.

If you've got grandchildren, people might ask about your grandchildren only. Since your child `shipping` has no children yet, it's all up to `flowers`. That's what the `depthOffset` parameter is for. If you don't pass this parameter, you won't skip any generations. If you pass `depthOffset=1`, you'll get a list of your grandchildren only. However, there's one hurdle to take: You must make sure that the `depth` parameter doesn't exclude the generation you're looking for with the `depthOffset` parameter. So if you're looking for your grandchildren, you must pass `depth=1` first, otherwise the `pages()` function won't even take your child's children into consideration. Now you can pass `depthOffset=1` to skip your children and return your grandchildren only. So for example, `pages("shop", depth=1, depthOffset=1)` returns `daisies` (page `shop/flowers/daisies.md`), `red-roses` (page `shop/flowers/red-roses.md`) and `tiger-lilies` (page `shop/flowers/tiger-lilies.md`).

Did you notice that you never included yourself? Simply pass `depthOffset=-1` to Pico's `pages()` function. `pages("shop", depthOffset=-1)` returns the pages `shop/index.md` (i.e. you) as well as your child page `shop/shipping.md` (again no `shop/flowers/index.md` because this page doesn't exist). You'll see `depthOffset=-1` in a lot of themes - it's often used to create a website's navigation, just starting from the root node:

{% raw %}<pre><code>&lt;ul&gt;
    {% for page in pages(depthOffset=-1) if page.title and not page.hidden %}
        &lt;li{% if page.id == current_page.id %} class=&quot;active&quot;{% endif %}&gt;
            &lt;a href=&quot;{{ page.url }}&quot;&gt;{{ page.title }}&lt;/a&gt;
        &lt;/li&gt;
    {% endfor %}
&lt;/ul&gt;</code></pre>{% endraw %}

## Some more things to know

Even though this tutorial is about Pico's `pages()` function, we should also talk about Pico's `pages` variable. Even though its usage looks very similar and you will often see it being used just like Pico's `pages()` function in old themes (e.g. `{% raw %}{% for page in pages %}…{% endfor %}{% endraw %}`), you should *never* use Pico's `pages` variable in a `for` loop. You remember Pico being blazing fast? By using Pico's `pages` variable in a `for` loop you'll take any chance from Pico being blazing fast. Always use Pico's `pages()` function instead. Use Pico's `pages` variable for just one thing only: Accessing the data of a single page whose name you know. Just take `content/_meta.md` in Pico's sample contents: If you want to print the page's `tagline` meta value, use `{% raw %}{{ pages["_meta"].meta.tagline }}{% endraw %}`. That's what Pico's `pages` variable is for, nothing else.

At some other place you might have heard that Pico's `pages()` function also accepts the `offset` parameter. The `offset` parameter is pretty advanced stuff, and you'll have to understand how Pico's `pages()` function resp. the underlying `PicoTwigExtension::pagesFunction()` PHP function works internally. Usually you don't have to know this. There's only one common use case: Accessing a page's parent page. Remember the example above: You're `shop` and you want to learn more about your parent. Just pass `offset=-1` (i.e. `pages("shop", offset=-1)`), it will return `index`. Keep in mind that the `pages()` function returns pages only, so `pages("shop/flowers/daisies", offset=-1)` will return nothing, because there's no `shop/flowers/index.md` page.

Anyway, if you really want to learn about the advanced stuff of the `offset` parameter: it is a shortcut to both `depth` and `depthOffset`. Its value is added to the values of both `depth` and `depthOffset` before they are being passed to `PicoTwigExtension::pagesFunction()`. It defaults to `offset=1`, so `PicoTwigExtension::pagesFunction()` actually receives `depth=1` and `depthOffset=1` as default parameters. `depth=1` tells the function to return the zeroth generation (the node asked for) and the first generation (the node's children). `depthOffset=1` tells the function to exclude the zeroth generation from the return value. That's the reason why `pages(depthOffset=-1)` actually works: due to the default `offset=1`, `PicoTwigExtension::pagesFunction()` is actually invoked with `depth=1` and `depthOffset=0`, telling the function to return both the zeroth and first generations and not to exclude any generation. You can indeed use the `offset` parameter in your Twig templates, but overwriting `offset` also affects `depth` and `depthOffset`. So for example, passing `offset=0` with neither `depth` nor `depthOffset` returns the page data of the asked node only. Check out Pico's [PHPDoc class docs][PHPDoc] for more details.

[FeaturesPageTree]: {{ site.github.url }}/in-depth/features/page-tree/
[PHPDoc]: {{ site.github.url }}/phpDoc/{{ page.gh_release }}/classes/PicoTwigExtension.html#method_pagesFunction
