---
layout: darknet
title: "Darknet: Open Source Neural Networks in C"
permalink: /darknet/
---

Darknet is an open source neural network framework written in C and CUDA. It is fast, easy to install, and supports CPU and GPU computation. You can find the source on [GitHub](https://github.com/pjreddie/darknet) or you can read more about what Darknet can do right here:

{% for page in darknet %}
    <a href="{% page.url %}">
        <div class=post>
        {% if page.logo %}
            <img class=logo src="{{ page.logo }}"></img>
        {% endif %}
        <h3>{{ page.title }}</h3>
            {% if page.description %}
                <p>{{ page.description }}</p>
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
  howpublished = {\url{https://pjreddie.com/darknet/}},
  year = {2013--2016}
}
```

