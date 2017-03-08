---
title: RSS Feed Generation
description: |
  By using the following code in a separate `feed.twig` template (in your theme folder), you can automatically generate an RSS feed from your Pico pages.<br /> <br />

  Simply create a "feed.md" file in your content folder with the header "Template: feed", and anyone who visits "/feed" on your site will recieve an RSS feed instead of a regular page.
---

<h5>Code</h5>

{% highlight html %}{% raw %}
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
  {% set RFC822 = "D, d M Y H:i:s O" %}
  <channel>
    <title>{{ site_title | e }}</title>
    <description>{{ pages.index.meta.description | e }}</description>
    <link>{{ base_url }}/</link>
    <atom:link href="{{ base_url ~ "/feed" }}" rel="self" type="application/rss+xml"/>
    <pubDate>{{ "now" | date(RFC822) }}</pubDate>
    <lastBuildDate>{{ "now" | date(RFC822) }}</lastBuildDate>
    <generator>Pico</generator>
    {% for page in pages if page.id not in ['index','feed'] and not end %}
      <item>
        <title>{{ page.title | e }}</title>
        <description>{{ page.id | content | e }}</description>
        <pubDate>{{ page.date | date(RFC822) }}</pubDate>
        <link>{{ page.url }}</link>
        <guid isPermaLink="true">{{ page.url }}</guid>
        {% for cat in page.meta.categories %}
          <category>{{ cat | e }}</category>
        {% endfor %}
      </item>
      {% if loop.index == 10 %}{% set end = true %}{% endif %}
    {% endfor %}
  </channel>
</rss>
{% endraw %}{% endhighlight %}
