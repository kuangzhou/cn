---
layout: page_no_comment
title: 标签
header: Posts By Tags 
group: no_navigation
permalink: /tags/
---
{% include JB/setup %}


<div class="accordion-box" id="cat-accordion">
{% for tags in site.tags %} 
  <h3 id="{{ tags[0] }}-ref">{{ tags[0] | join: "/" }} &times; {{tags[1].size}}</h3>
  <div>
    <ul>
    {% assign pages_list = tag[0] %}  
    {% include JB/tag_list %}
    </ul>
  </div>
{% endfor %}
</div>


