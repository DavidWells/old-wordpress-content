---
ID: 5283
post_title: 'Fixing broken use tags from inline SVG sprites [http vs. https]'
author: David Wells
post_date: 2016-05-10 19:45:56
post_excerpt: ""
layout: post
permalink: >
  http://davidwells.io/fixing-broken-use-tags-from-inline-svg-sprites/
published: true
external_post_url:
  - ""
---
Inlined SVG sprites are the icons of tomorrow. 

If you haven't used them. You should.

Read this post for more info on [using inline SVGs as your Icons](https://css-tricks.com/svg-sprites-use-better-icon-fonts/).

**This post**, however, is a tale of woe from using inlined SVGs and how servers and browser security can muck them up.

Leave now if you don't have this problem... 

This post is totally random but hopefully will help people who run into the same issue.

# The Problem

Inlined SVG icon `<use>` tags referencing `<defs>` in an SVG sprite were not appearing and/or "broken". 

The `use` references were broken for no apparent reason and all the icons were blank. Wtf internet! 

http://www.youtube.com/watch?v=QyL6wjlXO1I

This was only happening in a couple random places and the culprit was different server configs.

This is the error you might encounter in your browser console.
```
Unsafe attempt to load URL http://dev.yourapp.com/exchange/#xchange-color from frame with URL https://dev.yourapp.com/exchange/#!/. Domains, protocols and ports must match.
```

The ports **http** and **https** don't match, so the browser thinks it is loading from an insecure path. 

This causes inlined `<use>` tags to be empty, blank, ghost towns.

# Example

Let's say your app is running at http://dev.yourapp.com/exchange/

You see a warning like this in chrome: 
```
Unsafe attempt to load URL http://dev.yourapp.com/exchange/#xchange-color from frame with URL https://dev.yourapp.com/exchange/#!/. Domains, protocols and ports must match.
```

The `#xchange-color` reference is from the SVG use tag 

```html
&lt;span class=&#039;My-Dope-Inlined-SVG&#039;&gt;
  &lt;svg&gt;
    &lt;use xlink:href=&quot;#xchange-color&quot;&gt;&lt;/use&gt;
  &lt;/svg&gt;
&lt;span&gt;
``` 

The browser thinks it's requesting from `http://dev.yourapp.com/exchange/#xchange-color` while the current url is **https**://dev.yourapp.com/exchange/. Ports are mismatched.

But, the SVG is inlined into the bottom of the DOM (business as usual).

```html
&lt;!-- This is the sprite SVG at the bottom of the page --&gt;
&lt;svg height=&quot;0&quot; id=&quot;svg-sprite&quot; style=&quot;display: none&quot; width=&quot;0&quot;&gt;
&lt;defs&gt;
    &lt;symbol id=&quot;xchange-color&quot; viewbox=&quot;0 0 26 26&quot;&gt;
        &lt;path d=
        &quot;M5.306 7.427C4.17 8.993 3.5 10.918 3.5 13s.67 4.007 1.806 5.573l-1.072 1.072C2.832 17.799 2 15.497 2 13s.832-4.799 2.234-6.645l1.072 1.072zm16.46-1.072l-1.072 1.072C21.83 8.993 22.5 10.918 22.5 13s-.67 4.007-1.806 5.573l1.072 1.072C23.168 17.799 24 15.497 24 13s-.832-4.799-2.234-6.645z&quot; fill=&quot;#114459&quot;&gt;&lt;/path&gt;
    &lt;/symbol&gt;
&lt;/defs&gt;
&lt;/svg&gt;
```

Everything should be working. But nope, empty `<use>` tags.

Sidenote: The Nginx server is 301 redirecting requests from the `https://dev.yourapp.com/exchange` to `https://dev.yourapp.com/exchange/`. This might be the issue. I wasn't able to mess about with it.

# Why?

The actual root cause of this issue is still unclear to me because I didn't have access to this server =)

# The Client Side Solution

The real solution is fixing the issue with the server, but you know...

So, instead let's fix it client side!!!!

1. Loop over your inlined <use> references
2. Check if they actually have a height/width (aka are visible) with `getBoundingClientRect`
3. If they aren't visible, grab the svg path from the sprite & re-inline it in place of the empty `<use>` tag
4. Profit

# Code
```js
/* Usage: */
// Grab your &lt;use&gt; tags and loop over them with something like this
var useTag = document.querySelectorAll(&#039;use&#039;)
if(useTag &amp;&amp; useTag.length) {
  // This is just checking first &lt;use&gt;
  const dimenisons = useTag[0].getBoundingClientRect();
  // If use tag is 0px tall, the inlined &lt;use&gt; reference failed. 
  if(dimenisons.height === 0) {
    fixVisiblyEmptyUseTag(useTag[0])
  }
}

function fixVisiblyEmptyUseTag(useTag) {
  // Grab the use tag xlink:href for finding the corresponding symbol
  var id = useTag.getAttribute(&#039;xlink:href&#039;).replace(&quot;#&quot;, &#039;&#039;)
  // Grab parentNode to inline the already inlined SVG
  var parentNode = useTag.parentNode
  // kill broken use tag
  parentNode.removeChild(useTag);
  // Set class on parentNode to find this later
  parentNode.setAttribute(&#039;class&#039;, &#039;svg-&#039; + id)
  // grab the actual SVG from the &lt;defs&gt; 
  var inlineSVG = document.getElementById(id)
  // grab the actual SVG paths to inject in place of &lt;use&gt; tag
  var inlineSVGPaths = inlineSVG.innerHTML
  // grab viewbox attributes to make sure icons are correct size
  var inlineSVGViewBox = inlineSVG.getAttribute(&#039;viewBox&#039;)
  parentNode.setAttribute(&#039;viewBox&#039;, inlineSVGViewBox)
  // Create a dummy receptacle
  var receptacle = document.createElement(&#039;div&#039;);
  // Wrap the svg string to a svg object (string)
  var svgfragment = &#039;&lt;svg&gt;&#039; + inlineSVGPaths + &#039;&lt;/svg&gt;&#039;;
  // Add all svg to the receptacle
  receptacle.innerHTML = &#039;&#039; + svgfragment;
  // Splice the childs of the SVG inside the receptacle to the SVG at the body
  Array.prototype.slice.call(receptacle.childNodes[0].childNodes).forEach(function (el) {
    // grab the matching parentNodes and insert the SVG inline in place of &lt;use&gt; tag
    var SVGsOnPage = document.querySelectorAll(&#039;.svg-&#039; + id)
    for (var i = 0; i &lt; SVGsOnPage.length; i++) {
      SVGsOnPage[i].appendChild(el);
    }
  });
}
```

# Hopefully you never run into this!

# Happy SVGing