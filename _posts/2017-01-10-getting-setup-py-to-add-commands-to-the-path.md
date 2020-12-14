---
id: 122
title: Getting setup.py to add commands to the path
date: 2017-01-10T07:00:37+10:00
author: Stewart Polley
layout: post
guid: http://stewpolley.local/?p=122
permalink: /2017/01/10/getting-setup-py-to-add-commands-to-the-path/
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
  - Old
  - Python
---
There&#8217;s a number of articles out there that mention this, but they&#8217;re a bit of a pain to follow. Of them all, I found [this set of docs](http://python-packaging.readthedocs.io/en/latest/command-line-scripts.html) to be the most useful, but still lacking.

This may, or may not, have been the case if I had read the entire docs, but that&#8217;s ok. This blog post is mainly designed just as a reference, for people who want to get their answers quickly, as I did, and without trial and error!

For those that want a quick answer, when you&#8217;re using _setuptools_ in your setup function, you want to make use the of the _entry_points_ keyword.

From the example above:

<pre>setup(
    ...
    entry_points = {
        'console_scripts': ['funniest-joke=funniest.command_line:main'],
    }
    ...
)</pre>

As a quick explanation of how this works:

  * _funniest-joke_ &#8211; This is the command that you&#8217;ll enter into your terminal window
  * _funniest.command_line:main_ &#8211; This tells setuptools which file, and which function to call. In this example, it will go into the _funniest_ module, load the _command_line_ file, and execute the _main_ function