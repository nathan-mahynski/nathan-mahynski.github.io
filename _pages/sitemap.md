---
layout: archive
title: "Sitemap"
permalink: /sitemap/
author_profile: true
header:
  image_description: ""
  overlay_image: /assets/img/logo/logo_wide.png
  overlay_filter: 0.35 # same as adding an opacity of 0.5 to a black background
---

A list of all the posts and pages found on the site. 

## Pages
---
{% for post in site.pages %}
  {% include archive-single.html %}
{% endfor %}

<!--
<h2>Posts</h2>
{% for post in site.posts %}
  {% include archive-single.html %}
{% endfor %}
-->

## Collections
---
{% for collection in site.collections %}
{% unless collection.output == false or collection.label == "posts" %}
  {% capture label %}{{ collection.label }}{% endcapture %}
  {% if label != written_label %}
  <h2>{{ label }}</h2>
  {% capture written_label %}{{ label }}{% endcapture %}
  {% endif %}
{% endunless %}
{% for post in collection.docs %}
  {% unless collection.output == false or collection.label == "posts" %}
  {% include archive-single.html %}
  {% endunless %}
{% endfor %}
{% endfor %}

{% capture written_label %}'None'{% endcapture %}
