---
layout: docs
title: Accessing HTTP parameters | Pico Features
headline: Accessing HTTP parameters
description: Did you ever wanted to access HTTP GET and HTTP POST parameters in your theme?
toc:
    api: API
    examples: Examples
nav-url: /docs/
---

Starting with Pico 2.0 you can access HTTP GET (i.e. a URL's query string like `?some-variable=my-value`) and HTTP POST (i.e. data of a submitted form) parameters in Twig templates using the `url_param` resp. `form_param` functions. This makes developing awesome themes for your Pico website easier than ever before. Naturally we also expose this API to our plugin developers - so if you're a plugin developer stay where you are. 😊

Pico's HTTP parameters feature basically wraps around [PHP's `filter_var()` function](https://secure.php.net/manual/en/function.filter-var.php). Here's a excerpt from PHP's documentation about the [filter extension](https://secure.php.net/manual/en/intro.filter.php):

> This extension filters data by either validating or sanitizing it. This is especially useful when the data source contains unknown (or foreign) data, like user supplied input. For example, this data may come from an HTML form.
>
> There are two main types of filtering: validation and sanitization.
>
> [Validation](https://php.net/manual/en/filter.filters.validate.php) is used to validate or check if the data meets certain qualifications. For example, passing in `FILTER_VALIDATE_EMAIL` will determine if the data is a valid email address, but will not change the data itself.
>
> [Sanitization](https://php.net/manual/en/filter.filters.sanitize.php) will sanitize the data, so it may alter it by removing undesired characters. For example, passing in `FILTER_SANITIZE_EMAIL` will remove characters that are inappropriate for an email address to contain. That said, it does not validate the data.
>
> Flags are optionally used with both validation and sanitization to tweak behaviour according to need. For example, passing in `FILTER_FLAG_PATH_REQUIRED` while filtering an URL will require a path (like `/foo` in `http://example.org/foo`) to be present.

<small>– [Copyright](https://secure.php.net/manual/en/copyright.php) © 1997-2016 [The PHP Documentation Group](https://secure.php.net/credits.php), released under the [Creative Commons Attribution 3.0](https://creativecommons.org/licenses/by/3.0/) license</small>

**One warning beforehand:** Input validation is hard! Always validate your input data the most paranoid way you can imagine. Always prefer validation filters over sanitization filters; be very careful with sanitization filters, you might create cross-site scripting vulnerabilities!

As a theme developer you can use the `url_param` Twig function to access the value of a HTTP GET parameter. If you rather want to access the value of a HTTP POST parameter, use the `form_param` Twig function. Pass the name of the HTTP parameter as first argument and the validation or sanitization filter as second parameter (e.g. `{{ url_param("page", "int") }}` returns `42` when `?page=42` was requested).

As a plugin developer you can use the `Pico::getUrlParameter()` method to access HTTP GET parameters, and the `Pico::getFormParameter()` method to access HTTP POST parameters. The API is the same: The first argument represents the name of the HTTP parameter, the second argument the validation or sanitization filter to use (e.g. `Pico::getUrlParameter('expand', 'bool')` returns `true` when `?expand=yes` was requested).

## API

The `Pico::getUrlParameter()` function resp. the `url_param` Twig function and the `Pico::getFormParameter()` function resp. the `form_param` Twig function all accept the following parameters:

| Variable Type | Parameter Name | Description |
| ------------- | -------------- | ----------- |
| `mixed` | `$name` | name of the HTTP GET or HTTP POST variable |
| `int` \| `string` | `$filter = ''` | ID (int) or name (string) of the filter to apply; if omitted, all functions will return `false` |
| `mixed` \| `array` | `$options = null` | either a associative array of options to be used by the filter (e.g. `[ 'default': 42 ]`), or a scalar default value that will be returned when the HTTP GET or HTTP POST variable doesn't exist (optional) |
| `int` \| `string` \| `int[]` \| `string[]` | `$flags = null` | either a bitwise disjunction of flags or a string with the significant part of a flag constant (the constant name is the result of `FILTER_FLAG_` and the given string in ASCII-only uppercase); you may also pass an array of flags and flag strings (optional) |

With a validation filter passed in, all functions return the validated value of the HTTP GET or HTTP POST parameter, or, provided that the value wasn't valid, either the given default value or `false`. With a sanitization filter passed in, all functions return the sanitized value of the HTTP GET or HTTP POST parameter. If the HTTP GET or HTTP POST variable doesn't exist, all functions will always return either the provided default value or `null`.

You can't use the `callback` (`FILTER_CALLBACK`) with the `url_param` and `form_param` Twig functions.

For a complete list of all available filters refer to the documentation of [PHP's `filter_var()` function](https://secure.php.net/manual/en/function.filter-var.php):

* [Validation filter](https://secure.php.net/manual/en/filter.filters.validate.php)
* [Sanitization filter](https://php.net/manual/en/filter.filters.sanitize.php)
* [Filter flags](https://secure.php.net/manual/en/filter.filters.flags.php)

## Examples

Pass the boolean HTTP GET parameter `expand` to expand a details section in your template:

```twig
<a href="{{ current_page.id|link('expand=yes') }}">Learn more...</a>

{% if url_param('expand', 'boolean') %}
    You're learning more right now! Isn't that great!?
{% endif %}
```

Ask a user about "the answer" using a HTML form and store his decision in the Twig variable `the_answer`. Use a [regular expression](https://en.wikipedia.org/wiki/Regular_expression) to allow just values that are actually present in the HTML form.

```twig
<form action="" method="POST">
    <label for="the_answer">What is the answer?</label>
    <select id="the_answer" name="the_answer">
        <option></option>
        <option value="band">a Northern Irish hard rock and blues-rock band</option>
        <option value="42">42</option>
        <option value="what">What the hell are you talking about?</option>
    </select>
    <input type="submit" />
</form>

{% set the_answer = form_param('the_answer', 'validate_regexp', { 'regexp': '/^(band|42|what)$/' }) %}
```

Ask a user how much cat pictures he makes a year and claim that his amount is either sad or not impressive at all. Use the Twig variable `amount` and let the parameter default to `0`. Use the `FILTER_VALIDATE_FLOAT` (`float`) filter, but tweak its behaviour by passing the `FILTER_FLAG_ALLOW_THOUSAND` flag - this allows the user to enter their amount with a thousand separator (e.g. `12,345.00`).

```twig
<form action="" method="GET">
    <label for="amount">How much cat pictures do you make a year?</label>
    <input id="amount" name="amount" type="text" />
    <input type="submit" />
</form>

{% set amount = url_param('amount', 'float', 0, 'allow_thousand') %}
{% if amount > 0 %}
    {% if amount < 10 %}
        You make just {{ amount }} cat pictures a year? Oh, that's sad... :-(
    {% else %}
        Impressive... Not! I make {{ amount * 2 }} cat pictures a year!
    {% endif %}
{% endif %}
```
