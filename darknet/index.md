---
layout: darknet
title: "Darknet: Open Source Neural Networks in C"
permalink: /darknet/
---

Darknet is an open source neural network framework written in C and CUDA. It is fast, easy to install, and supports CPU and GPU computation. You can find the source on [GitHub](https://github.com/pjreddie/darknet) or you can read more about what Darknet can do right here:

{% for post in darknet %}
    <a href="{% post.url %}">
        <div class=post>
        {% if post.logo %}
            <img class=logo src="{{ post.logo }}"></img>
        {% endif %}
        <h3>{{ post.title }}</h3>
            {% if post.description %}
                <p>{{ post.description }}</p>
            {% endif %}
        </div>
    </a>
{% endfor %}

## Cite ##

If you use Darknet in your research please cite it:

```
@misc{darknet13,
  author =   {Joseph Redmon},
  title =    {Darknet: Open Source Neural Networks in C},
  howpublished = {\url{http://pjreddie.com/darknet/}},
  year = {2013--2016}
}
```

