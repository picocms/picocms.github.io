---
toc:
    config:
        _title: Config
        url-rewriting: URL Rewriting
        apache: Apache
        nginx: Nginx
nav: 5
---

## Config

You can override the default Pico settings (and add your own custom settings) by editing `config/config.yml` in the Pico directory. For a brief overview of the available settings and their defaults see [`config/config.yml.template`][ConfigTemplate]. To override a setting, simply copy the line from `config/config.yml.template` to `config/config.yml` and set your custom value. Pico will read all `*.yml` files in the `config/` dir, thus you can even use a distinct settings file to configure your custom theme (e.g. `config/my_theme.yml`).

### URL Rewriting

Pico's default URLs (e.g. http://example.com/pico/?sub/page) already are very user-friendly. Additionally, Pico offers you a URL rewrite feature to make URLs even more user-friendly (e.g. http://example.com/pico/sub/page).

#### Apache

If you're using the Apache web server, URL rewriting should be enabled automatically. If you get an error message from your web server, please make sure to enable the [`mod_rewrite` module][ModRewrite]. Assuming rewritten URLs work, but Pico still shows no rewritten URLs, force URL rewriting by setting `rewrite_url: true` in your `config/config.yml`.

#### Nginx [Learn more…][NginxConfig]{:.learn-more}
{:#nginx}

If you're using Nginx, you can use the following configuration to enable URL rewriting (lines `5` to `8`) and denying access to Pico's internal files (lines `1` to `3`). You'll need to adjust the path (`/pico` on lines `1`, `2`, `5` and `7`) to match your installation directory. Additionally, you'll need to enable URL rewriting by setting `rewrite_url: true` in your `config/config.yml`. The Nginx configuration should provide the *bare minimum* you need for Pico. Nginx is a very extensive subject. If you have any trouble, please read through our [Nginx configuration docs][NginxConfig]. For additional assistance, please refer to the ["Getting Help" section][GettingHelp] below.

```
location ~ ^/pico/((config|content|vendor|composer\.(json|lock|phar))(/|$)|(.+/)?\.(?!well-known(/|$))) {
    try_files /pico/index.php$is_args$args;
}

location /pico/ {
    index index.php;
    try_files $uri $uri/ /pico/index.php$is_args$args;
}
```

[ConfigTemplate]: {{ site.gh_project_url }}/blob/{{ site.gh_project_branch }}/config/config.yml.template
[ModRewrite]: https://httpd.apache.org/docs/current/mod/mod_rewrite.html
[NginxConfig]: {{ site.github.url }}/in-depth/nginx/
[GettingHelp]: {{ site.github.url }}/docs/#getting-help
