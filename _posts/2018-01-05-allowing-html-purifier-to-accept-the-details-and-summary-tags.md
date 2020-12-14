---
id: 188
title: Allowing HTML Purifier to accept the details and summary tags.
date: 2018-01-05T17:48:09+10:00
author: Stewart Polley
layout: post
guid: http://stewpolley.local/?p=188
permalink: /2018/01/05/allowing-html-purifier-to-accept-the-details-and-summary-tags/
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
image: /wp-content/uploads/2018/01/800px-PHP-logo.svg-260x140.png
categories:
  - PHP
tags:
  - php
---
The [HTML Purifier library](http://htmlpurifier.org/) is a great way to filter user-supplied HTML, normally from WYSIWYG Rich Text editors, and remove potentially malicious content. It can also be used to ensure that certain elements _always_ get saved with particular attribues. Ie, ensuring any user-supplied link has `target="_blank"` and `rel="nofollow"`. Depending on your use case, you may even want to prevent users from inserting non HTTPS links etc.

One downside to using a library for this, is if you want to use a newer HTML elements, such as [details](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/details) and [summary](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/summary), the library may not yet know about them! This is currently the case with HTML Purifier and these tags. [Most browsers support the](https://caniuse.com/#feat=details), with IE and Edge being the main exceptions.

These tags allow a really simple pure HTML only way of having an expandable section on your page. [Here&#8217;s an example](https://codepen.io/anon/pen/aELqbP).

Now that we know what these tags elements are, let&#8217;s look at how to implement them in HTML Purifier.

<pre>$config = \HTMLPurifier_Config::createDefault();
# Other configuration options you may use
$def = $config-&gt;getHTMLDefinition(true);
$def-&gt;addElement(
    'details',
    'Block',
    'Flow',
    'Common',
    array(
        'open' =&gt; new \HTMLPurifier_AttrDef_HTML_Bool(true)
    )
);
$def-&gt;addElement('summary', 'Inline', 'Inline', 'Common');</pre>

It&#8217;s not super-obvious when reading [their docs](http://htmlpurifier.org/docs/enduser-customize.html#addAttribute), at least it wasn&#8217;t for myself, but you need to use `new \HTMLPurifier_AttrDef_HTML_Bool(true)` for the `'open'` element. Simply using `'open' => 'Bool'` doesn&#8217;t work, neither does `'open' => new \HTMLPurifier_AttrDef_HTML_Bool()`.

Hopefully this will make it easier for someone else out there to add these super-cool elements into their list of allowed elements.