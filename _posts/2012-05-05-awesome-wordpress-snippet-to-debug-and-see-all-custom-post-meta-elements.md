---
ID: 4314
post_title: >
  Awesome WordPress Snippet to Debug and
  See all Custom Post Meta Elements
author: David Wells
post_date: 2012-05-05 03:39:43
post_excerpt: ""
layout: post
permalink: >
  http://davidwells.io/awesome-wordpress-snippet-to-debug-and-see-all-custom-post-meta-elements/
published: true
custside-smap:
  - 'a:0:{}'
dsq_thread_id:
  - "2294071903"
MostSharedContent_ShareCount:
  - "0"
---
Ran across this script earlier today.

It helped be see a huge list of custom post fields aka, wordpress post meta, to use in a WordPress Custom Post template.

Place it in the single.php or custom page template loop and you can echo out all of the available post variables.
<pre class="brush: php; gutter: true">&lt;h3&gt;All Post Meta&lt;/h3&gt;

&lt;?php $getPostCustom=get_post_custom(); // Get all the data ?&gt;

&lt;?php
    foreach($getPostCustom as $name=&gt;$value) {

        echo &quot;&lt;strong&gt;&quot;.$name.&quot;&lt;/strong&gt;&quot;.&quot;  =&gt;  &quot;;

        foreach($value as $nameAr=&gt;$valueAr) {
                echo &quot;&lt;br /&gt;     &quot;;
                echo $nameAr.&quot;  =&gt;  &quot;;
                echo var_dump($valueAr);
        }

        echo &quot;&lt;br /&gt;&lt;br /&gt;&quot;;

    }
?&gt;</pre>
<p style="text-align: center;"><a href="http://www.davidwells.tv/wp-content/uploads/2012/05/all-variables.png"><img class=" wp-image-4316 aligncenter" title="all variables" src="http://www.davidwells.tv/wp-content/uploads/2012/05/all-variables-1024x768.png" alt="" width="614" height="461" /></a></p>
I highly recommend saving it.

Once you echo out all of your custom post meta you can set up conditional logic like:
<pre class="brush: actionscript3; gutter: true">&lt;?php if (get_post_meta($post-&gt;ID, &#039;landing_page_checkbox&#039;, true) == &quot;&quot;) {
  echo &quot;#menu {display:none;}&quot;; } ?&gt;</pre>
This code above is hiding my sites navigation is the custom meta box is checked.