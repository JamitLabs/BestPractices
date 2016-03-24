---
layout: page
---

{% for post in site.posts %}
  [{{ post.title }}]({{ post.url | prepend: site.baseurl }})
{% endfor %}
