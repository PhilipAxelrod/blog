---
title: Philip Axelrod's Tech Shenanigans
description: Philip Axelrod's tech blog
---

## Welcome to my tech blog!

I am a rising senior studying mathematics 
at the College of Creative Studies at UC Santa Barbara, but my interests are not
limited to math. Checkout some of the tech projects I've been working on! Whether you're interested in
data science and artificial intelligence, or web development and scripting, or spatial localization, I guarantee
you'll find something interesting in my projects. Checkout my blog posts below!

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>

Oh, and by the way: This webpage is written with Github Pages and Jekyll.