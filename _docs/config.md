---
toc:
    config:
        _title: Config
        url-rewriting:
            _title: URL Rewriting
            apache: Apache
            nginx: Nginx
            lighttpd: Lighttpd
nav: 5
---

## Config

Configuring Pico really is stupidly simple: Just create a `config/config.yml` to override the default Pico settings (and add your own custom settings). Take a look at the [`config/config.yml.template`][ConfigTemplate] for a brief overview of the available settings and their defaults. To override a setting, simply copy the line from `config/config.yml.template` to `config/config.yml` and set your custom value.

But we didn't stop there. Rather than having just a single config file, you can use an arbitrary number of config files. Simply create a `.yml` file in Pico's `config` dir, and you're good to go. This allows you to add some structure to your config, like a separate config file for your theme (`config/my_theme.yml`).

Please note that Pico loads config files in a special way you should be aware of. It loads the main config file `config/config.yml`, and then any other `*.yml` file in Pico's `config` dir in alphabetical order. The file order is crucial: Config values which have been set already, cannot be overwritten by a succeeding file. For example, if you set `site_title: Pico` in `config/a.yml` and `site_title: My awesome site!` in `config/b.yml`, your site title will be "Pico".

Since YAML files are plain text files, users might read your Pico config by navigating to `https://example.com/pico/config/config.yml`. This is no problem in the first place, but might get a problem if you use plugins that require you to store security-relevant data in the config (like credentials). Thus, you should *always* make sure to configure your webserver to deny access to Pico's `config` dir. Just refer to the ["URL Rewriting" section below][UrlRewriting]. By following the instructions, you will not just enable URL rewriting, but also deny access to Pico's `config` dir.

### URL Rewriting

Pico's default URLs (e.g. https://example.com/pico/?sub/page) already are very user-friendly. Additionally, Pico offers you a URL rewrite feature to make URLs even more user-friendly (e.g. https://example.com/pico/sub/page). Below you'll find some basic info about how to configure your webserver proberly to enable URL rewriting. For additional assistance, please refer to the ["Getting Help" section][GettingHelp] below.

#### Apache

If you're using the Apache web server, URL rewriting should be enabled automatically. If URL rewriting doesn't work (you're getting `404 Not Found` error messages from Apache), please make sure to enable the [`mod_rewrite` module][ModRewrite] and to enable `.htaccess` overrides. You might have to set the [`AllowOverride` directive][AllowOverride] to `AllowOverride All` in your virtual host config file or global `httpd.conf`/`apache.conf`. Assuming rewritten URLs work, but Pico still shows no rewritten URLs, force URL rewriting by setting `rewrite_url: true` in your `config/config.yml`. If you rather get a `500 Internal Server Error` no matter what you do, try removing the `Options` directive from Pico's `.htaccess` file (it's the last line).

#### Nginx [Learn more…][NginxConfig]{:.learn-more}
{:#nginx}

If you're using Nginx, you can use the following config to enable URL rewriting (lines `5` to `8`) and denying access to Pico's internal files (lines `1` to `3`). You'll need to adjust the path (`/pico` on lines `1`, `2`, `5` and `7`) to match your installation directory. Additionally, you'll need to enable URL rewriting by setting `rewrite_url: true` in your `config/config.yml`. The Nginx config should provide the *bare minimum* you need for Pico. Nginx is a very extensive subject. If you have any trouble, please read through our [Nginx config docs][NginxConfig].

```
location ~ ^/pico/((config|content|vendor|composer\.(json|lock|phar))(/|$)|(.+/)?\.(?!well-known(/|$))) {
    try_files /pico/index.php$is_args$args =404;
}

location /pico/ {
    index index.php;
    try_files $uri $uri/ /pico/index.php$is_args$args;
}
```

#### Lighttpd

Pico runs smoothly on Lighttpd. You can use the following config to enable URL rewriting (lines `6` to `9`) and denying access to Pico's internal files (lines `1` to `4`). Make sure to adjust the path (`/pico` on lines `2`, `3` and `7`) to match your installation directory, and let Pico know about available URL rewriting by setting `rewrite_url: true` in your `config/config.yml`. The config below should provide the *bare minimum* you need for Pico.

```
url.rewrite-once = (
    "^/pico/(config|content|vendor|composer\.(json|lock|phar))(/|$)" => "/pico/index.php",
    "^/pico/(.+/)?\.(?!well-known(/|$))" => "/pico/index.php"
)

url.rewrite-if-not-file = (
    "^/pico(/|$)" => "/pico/index.php"
)
```

[ConfigTemplate]: {{ site.gh_project_url }}/blob/{{ site.gh_project_branch }}/config/config.yml.template
[UrlRewriting]: {{ site.github.url }}/docs/#url-rewriting
[ModRewrite]: https://httpd.apache.org/docs/current/mod/mod_rewrite.html
[AllowOverride]: https://httpd.apache.org/docs/current/mod/core.html#allowoverride
[NginxConfig]: {{ site.github.url }}/in-depth/nginx/
[GettingHelp]: {{ site.github.url }}/docs/#getting-help
