---
id: 126
title: Rendering a JSON string inside a script with Django
date: 2017-12-16T07:03:42+10:00
author: Stewart Polley
layout: post
guid: http://stewpolley.local/?p=126
permalink: /2017/12/16/rendering-a-json-string-inside-a-script-with-django/
mfn-post-hide-content:
  - "0"
mfn-post-slider:
  - "0"
mfn-post-slider-layer:
  - "0"
mfn-post-hide-title:
  - "0"
mfn-post-remove-padding:
  - "0"
mfn-post-intro:
  - 'a:1:{s:9:"post-meta";s:1:"1";}'
mfn-post-hide-image:
  - "0"
mfn-post-love:
  - "1"
image: /wp-content/uploads/2017/12/opengraph-icon-200x200-146x146.png
categories:
  - Django
  - Javascript
  - Old
  - Python
  - Vue
---
Building a list of form submissions in Django last night was causing me some issues. This is likely a common issue for people and something most people know already, but I wasn&#8217;t able to find a nice answer online after my searching, so here&#8217;s my solution for others to benefit from.

My scenario:

  * Build a list of submissions inside the view
  * Pass these into the template, so that Vue could render the details

I could have made the decision to load the page first, then use Axios to get the list of submissions, but I decided aginst that for now.

As such, I had my view setup along these lines:

<pre>from django.shortcuts import render
import json
from myapp.models import MyModel

def my_view(request):
    my_models = myModel.objects.all()
    model_list = []
    for model in my_models:
        new_model = {
            'id': model.id,
            'name': model.name,
            'picture': model.picture
        }
        model_list.append(new_model)

    context={
        'models': json.dumps(model_list)
    }

    return render(request, 'my_template.html')
</pre>

<small>(I would like to use <code>model_to_dict</code> from django.forms.models here but UUIDs don&#8217;t work nicely in my scenario&#8230;)</small>

My Vue JS:

<pre>var models= new Vue(
    {
        el: '#models',
        delimiters: ["[%", "%]"],
        data: {
            models: model_list
        }
    }
);
</pre>

End of course, the template:

<pre>

</pre>

The problem here was though, Django was escaping the JSON string automatically. After doing some googling I found many solutions that said to use the `escapejs` flag in conjunction with `JSON.parse()` but I wasn&#8217;t satisfied with that.

Django has a tag for disabling the auto-escaping it does though. This allows you to do the following:

<pre>&lt;script&gt;

&lt;/script&gt;
</pre>

And voila! Django renders the JSON String in such a way that Javascript can work with it right away, without having to use JSON.parse on it.
