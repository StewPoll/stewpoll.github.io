---
id: 215
title: 'Django: Setting Cookie and returning a rendered template &#8211; an example'
date: 2018-01-13T18:27:00+10:00
author: Stewart Polley
layout: post
guid: http://stewpolley.local/?p=215
permalink: /2018/01/13/django-setting-cookie-and-returning-a-rendered-template-an-example/
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
  - "0"
mfn-meta-seo-title:
  - 'Django: Setting Cookie and returning a rendered template - an example'
mfn-meta-seo-description:
  - How to set a cookie with Django and return rendered template
mfn-meta-seo-keywords:
  - cookie django render template
mfn-meta-seo-og-image:
  - http://stewpolley.local/wp-content/uploads/2018/01/cookies.jpeg
image: /wp-content/uploads/2018/01/cookies-219x146.jpeg
categories:
  - Django
  - Python
---
While trying to figure out how to set cookies with Django (something I&#8217;ve been lucky enough to not have to do) I was of course searching &#8220;How to set a cookie with Django&#8221; <a href="https://stackoverflow.com/questions/1622793/django-cookies-how-can-i-set-them" target="_blank" rel="noopener">this</a> was the first result I found. Now that&#8217;s a great link and all, and it points to the [relevant Django Docs](https://docs.djangoproject.com/en/dev/ref/request-response/).

However, there was a particular example missing. Returning a rendered template, with a cookie. Fortunately, it didn&#8217;t take long to figure it out, but I felt the fact that an exact example of this was missing as a little annoying. So for anyone else wondering &#8220;How do I return a rendered template AND set a cookie&#8221; here&#8217;s an example.

<pre>from django.shortcuts import render

def my_view(request):
    response = render(request, 'index.html')
    response.set_cookie('my_cookie', 'Here is my cookie!')
    return response</pre>

With any luck this will help at least one other person out there!

Of course, please read up the [set_cookie](https://docs.djangoproject.com/en/dev/ref/request-response/#django.http.HttpResponse.set_cookie) and [set\_signed\_cookie](https://docs.djangoproject.com/en/dev/ref/request-response/#django.http.HttpResponse.set_signed_cookie) docs for more info on what you can do here.