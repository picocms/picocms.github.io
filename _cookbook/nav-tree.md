---
title: Navigation Tree
description: How to create a navigation tree for pages?
---

<div class="one-half">
    <h5>Pico Plugin</h5>

    {% highlight php %}{% raw %}
class PicoPageTreePlugin extends AbstractPicoPlugin
{
    protected $pageTree = array();

    public function onPagesLoaded(
        array &$pages,
        array &$currentPage = null,
        array &$previousPage = null,
        array &$nextPage = null
    ) {
        foreach ($pages as &$page) {
            $id = (substr($page['id'], -6) === '/index') ? substr($page['id'], 0, -6) : $page['id'];
            $parent = (($pos = strrpos($id, '/')) !== false) ? $parent = substr($id, 0, $pos) : '';

            $this->pageTree[$parent][$page['id']] = &$page;
        }
    }

    public function onPageRendering(Twig_Environment &$twig, array &$twigVariables, &$templateName)
    {
        $twigVariables['pageTree'] = $this->pageTree;
    }
}
    {% endraw %}{% endhighlight %}
</div>

<div class="one-half last">
    <h5>Twig</h5>

    {% highlight html %}{% raw %}
{% macro rnav(tree, parent = "") %}
    {% import _self as macros %}
    {% if tree[parent] %}
        <ul>
            {% for page in tree[parent] %}
                <li>
                    <a href="{{ page.url }}">{{ page.title }}</a>

                    {% set sub = page.id|slice(-6) == "/index" ? page.id|slice(0, -6) : page.id %}
                    {{ macros.rnav(tree, sub) }}
                </li>
            {% endfor %}
        </ul>
    {% endif %}
{% endmacro %}

<nav id="nav">
    {% import _self as macros %}
    {{ macros.rnav(pageTree) }}
</nav>
    {% endraw %}{% endhighlight %}
</div>

<div class="clear"></div>
