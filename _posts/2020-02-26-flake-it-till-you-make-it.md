---
layout: post
title: Flake it till you make it
subtitle: Excerpt from Soulshaping by Jeff Brown
cover-img: /assets/img/path.jpg
tags: [books, test]
---

{% if page.htmlwidgets %}
{% for html_dep in site.static_files %}
  {% if html_dep.path contains 'htmlwidgets_deps/' %}
    {% assign start = "<script src=" | append: {{site.baseurl}} %}
    {{html_dep.path | prepend: start | append: "></script>" }}
    {% endif %}
  {% endfor %}
{% endif %}

<div>
    <a href="https://plotly.com/~Terrence.bosco/19/?share_key=HFvJQNk68TH37MjaCRatPL" target="_blank" title="ticket_time_break_down copy" style="display: block; text-align: center;"><img src="https://plotly.com/~Terrence.bosco/19.png?share_key=HFvJQNk68TH37MjaCRatPL" alt="ticket_time_break_down copy" style="max-width: 100%;width: 600px;"  width="600" onerror="this.onerror=null;this.src='https://plotly.com/404.png';" /></a>
    <script data-plotly="Terrence.bosco:19" sharekey-plotly="HFvJQNk68TH37MjaCRatPL" src="https://plotly.com/embed.js" async></script>
</div>


    
