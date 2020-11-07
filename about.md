---
title: About
permalink: "/about/"
layout: page
---

![](https://mcusercontent.com/85324cdc1b9bed72902e27fa0/images/8e74d3ba-8792-452f-b656-67ca8f00cb9c.png)

---

### Contact Python Programming

Become an author of [pythonprogramming.org](https://pythonprogramming.org) by emailing us.

**email:** [Email](mailto:pythonprogrammingorg@gmail.com)

Follow us on Twitter: [https://twitter.com/pythonprogramm9](https://twitter.com/pythonprogramm9)

This blog is hosted on GitHub pages and is [open-source](https://github.com/singh1114/pythonprogramming).

We are using the [Best GH-pages theme](https://github.com/singh1114/best).

---

### Our Authors

<div class="original_source">
  {% for author_page in site.authors %}
    <div class="linked_post_div" style="margin: 10px 0px; font-size: 14.5px;">
      <article class="post">
        <h3>About Author &middot; <a href="{{ author_page.permalink }}">{{ author_page.title }}</a></h3>
        {% if author_page.description %}
          {{ author_page.description }}
          <br>
        {% else %}
          {{ author_page.excerpt }}
        {% endif %}
      </article>
    </div>
  {% endfor %}
</div>
