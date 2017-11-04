---
layout: default
title: Code Statistics
headline: Code Statistics
description: Pico's code statistics are powered by [cloc](https://github.com/AlDanial/cloc).
nav-url: /docs/
---

<div class="table-responsive">
    <table>
        <thead>
            <tr>
                <th class="nowrap" style="width: 52%;">Language</th>
                <th class="nowrap" style="width: 12%;">Files</th>
                <th class="nowrap" style="width: 12%;">Blank</th>
                <th class="nowrap" style="width: 12%;">Comment</th>
                <th class="nowrap" style="width: 12%;">Code</th>
            </tr>
        </thead>
        <tbody>
            {% for item in site.data.cloc %}
                {% if item[0] != "SUM" %}
                    <tr>
                        <td>{{ item[0] }}</td>
                        <td class="align-right">{{ item[1].nFiles }}</td>
                        <td class="align-right">{{ item[1].blank }}</td>
                        <td class="align-right">{{ item[1].comment }}</td>
                        <td class="align-right">{{ item[1].code }}</td>
                    </tr>
                {% else %}
                    <tr>
                        <td></td>
                        <td>
                            <span class="amount small align-right">
                                <span>Σ</span>
                                {{ item[1].nFiles }}
                            </span>
                        </td>
                        <td>
                            <span class="amount small align-right">
                                <span>Σ</span>
                                {{ item[1].blank }}
                            </span>
                        </td>
                        <td>
                            <span class="amount small align-right">
                                <span>Σ</span>
                                {{ item[1].comment }}
                            </span>
                        </td>
                        <td>
                            <span class="amount small align-right">
                                <span>Σ</span>
                                {{ item[1].code }}
                            </span>
                        </td>
                    </tr>
                {% endif %}
            {% endfor %}
        </tbody>
    </table>
</div>

