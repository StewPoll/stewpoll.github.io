---
title: Home | Stewpolley.com
layout: default
---
<ul class="post-previews">
    {% for post in site.posts %}
    <li class="post-preview">
        <div class="preview-image-container">
        {% if post.thumbnail %}
            <img class="preview-image" src="{{ post.thumbnail }}">
        {% elsif post.image %}
            <img class="preview-image" src="{{ post.image }}">
        {% else %}
            <img class="preview-image" src="{{ '/assets/images/Logo-512.png' | relative_url }}">
        {% endif %}
        </div>
        <div class="preview-content">
            <a href="{{ post.url }}">
                <h2>{{ post.title }}</h2>
            </a>
            <p>{{ post.excerpt }} </p>
        </div>
    </li>
    {% endfor %}
</ul>
