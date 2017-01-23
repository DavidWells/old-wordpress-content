---
ID: 4321
post_title: >
  How to Detect Specific Versions of
  Internet Explorer in PHP and Show
  Different Fallback Content
author: David Wells
post_date: 2012-05-26 02:01:33
post_excerpt: ""
layout: post
permalink: >
  http://davidwells.io/how-to-detect-specific-versions-of-internet-explorer-in-php-and-show-different-fallback-content/
published: true
custside-smap:
  - 'a:0:{}'
dsq_thread_id:
  - "2292628814"
MostSharedContent_ShareCount:
  - "0"
---
I needed to detect the specific version of IE people were using for a specific tool I'm building.

The javascript I'm using, <a href="http://popcornjs.org/documentation">popcorn.js</a>, doesn't work in internet explorer 8, 7, and 6. (sad face)

I found a number of different snippets out there to detect if they were using Internet Explorer but nothing that let you see what specific version the visitor was using.

So I put this together. Here is a the script:
<code><pre class="brush: php; gutter: true">&lt;?php $IE6 = (ereg(&#039;MSIE 6&#039;,$_SERVER[&#039;HTTP_USER_AGENT&#039;])) ? true : false;
    $IE7 = (ereg(&#039;MSIE 7&#039;,$_SERVER[&#039;HTTP_USER_AGENT&#039;])) ? true : false;
    $IE8 = (ereg(&#039;MSIE 8&#039;,$_SERVER[&#039;HTTP_USER_AGENT&#039;])) ? true : false;

if (($IE6 == 1) || ($IE7 == 1) || ($IE8 == 1)) {
 // Do fallback stuff that old browsers can do here
   echo &quot;Shit its ie!&quot;;

} else { ?&gt;
// do stuff that real browsers can handle here
&lt;?php } ?&gt;</pre></code>