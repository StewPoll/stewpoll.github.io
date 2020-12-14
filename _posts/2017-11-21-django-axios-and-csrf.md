---
id: 129
title: Django, Axios and CSRF
date: 2017-11-21T07:05:19+10:00
author: Stewart Polley
layout: post
guid: http://stewpolley.local/?p=129
permalink: /2017/11/21/django-axios-and-csrf/
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
  - Old
  - Python
---
I am currently building a simple Django based web-app. Part of this app is a registration form. It has a rather nice UI, built from Vue, and I&#8217;m POSTing the data back to Django using Axios.

One great thing about Django is it&#8217;s built-in CSRF protection. While I&#8217;m not an expert and can&#8217;t say it&#8217;s the best CSRF protection, at the very least it&#8217;s nice that it has _something_ built in.

When posting via Axios though, you need to send through the CSRF token, somehow. Everyone was saying you had to post it back, but it took me a while to figure out _where_ you should be placing it. Eventually I found this snippet from the [official Django Docs](https://docs.djangoproject.com/en/2.0/ref/csrf/#acquiring-the-token-if-csrf-use-sessions-is-false).

> The CSRF header name is `HTTP_X_CSRFTOKEN` by default, but you can customize it using the `CSRF_HEADER_NAME` setting.

This answered my questions, and I ended up with this little bit of JS inside my Vue method.

<pre>submit: function() {
    data: {...}; // The exact data doesn't matter
    csrftoken = Cookies.get('csrftoken'); // Using <a href="https://github.com/js-cookie/js-cookie/">JS Cookies library</a>
    headers = {X_CSRFTOKEN: csrftoken};
    axios.post(url,data,{headers: headers});
}</pre>

Now of course I had a .then and .catch section after axios&#8217; .post, but that should at least give you some idea of how you can do it.

### Update:

After reviewing this and how Django works, I&#8217;ve made a quick change to this document. Django adds &#8220;HTTP_&#8221; to all header names, and converts all dashes to underscores. So if your client sends a `'X-XSRF-TOKEN'` header, the setting should be `'HTTP_X_XSRF_TOKEN'.` In order to make use of the default setting, you need to send through X_CSRFTOKEN or X-CSRFTOKEN as the header name. If you use X-CSRFTOKEN you need to wrap it in &#8216; in the JS code. IE:

<pre>headers = {'X-CSRFTOKEN': csrftoken};</pre>

Now you can avoid traps that I fell into.