---
ID: 5211
post_title: >
  How to programmatically import your
  external blog post links into WordPress
author: David Wells
post_date: 2016-02-10 19:13:14
post_excerpt: ""
layout: post
permalink: >
  http://davidwells.io/how-to-import-your-external-links-and-posts-into-wordpress-via-javascript/
published: true
external_post_url:
  - ""
---
I recently imported all the links to external posts I'd written into this blog.

## Why?

I wanted to collect/showcase all the content I've written over the ages in one place.

## Problem

Some content was located in other WordPress sites and other posts were not.

So, WordPress XML imports wouldn't work for everything.

I also didn't want to duplicate all the post_meta and content for all the posts but rather just link out to them.

## Solution?

Glad you asked!

In the end, I decided to simply use javascript to grab an array of the post titles, dates and URLs via the browser console.

# Step 1. Grab the data

Grab your post data.

From the WordPress admin page `http://www.inboundnow.com/wp-admin/edit.php?post_type=post&amp;author=3` viewing my posts.

`http://www.site.com/wp-admin/edit.php?post_type=post&amp;author=YOUR_AUTHOR_ID`

Set the 'Number of items per page' to as high as you need to view all the posts at once, then run this script in the browser console.

```javascript
var row = document.querySelectorAll(&#039;#the-list tr&#039;)

var data = [];
for (var i = 0; i &lt; row.length; i++) {
  var title = row[i].querySelectorAll(&#039;.row-title&#039;)[0].innerText;
  var link = row[i].querySelectorAll(&#039;.view a&#039;)[0].getAttribute(&#039;href&#039;);
  var date = row[i].querySelectorAll(&#039;.column-date abbr&#039;)[0].getAttribute(&#039;title&#039;);
  data[i] = { url: link, title: title, date: date}
};
console.log(JSON.stringify(data)) // blam all of the posts in a nice lil javascript array
```

I grabbed all the posts I created while I was at HubSpot from this page `http://blog.hubspot.com/marketing/author/david-wells` using the below script.

```javascript
var article = document.querySelectorAll(&#039;.post-item&#039;);
var data = [];
for (var i = 0; i &lt; article.length; i++) {
  var url = &quot;http:&quot; + article[i].querySelectorAll(&#039;.post-title__link--preview&#039;)[0].getAttribute(&#039;href&#039;);
  var title = article[i].querySelectorAll(&#039;.post-title__link--preview&#039;)[0].innerText;
  var date = article[i].querySelectorAll(&#039;meta&#039;)[1].getAttribute(&#039;content&#039;);
  data[i] = { url: url, title: title, date: date}
};
console.log(JSON.stringify(data))
```

Now I have my data to import!

# Step 2 - Import into Wordpress

This code goes in your functions.php file. Alter it to suite your needs. (aka post author number and category number )

```php
function insert_wordpress_posts_from_json(){
  // Add your json block here
  $json = &#039;[{
  &quot;url&quot;: &quot;http://www.inboundnow.com/bulk-manage-web-leads-directly-wordpress/&quot;,
  &quot;title&quot;: &quot;Bulk Edit &amp; Manage Web Leads Directly from WordPress&quot;,
  &quot;date&quot;: &quot;2014/01/30 2:08:17 pm&quot;
  }, {
  &quot;url&quot;: &quot;http://www.inboundnow.com/create-awesome-lead-tracking-forms-site/&quot;,
  &quot;title&quot;: &quot;How to Create Awesome Lead Tracking Forms for Your Site&quot;,
  &quot;date&quot;: &quot;2014/01/12 12:07:18 am&quot;
  }, {
  &quot;url&quot;: &quot;http://www.inboundnow.com/create-awesome-unordered-lists-with-icons/&quot;,
  &quot;title&quot;: &quot;How to Create Awesome Unordered Lists with Icons for Your WordPress Site&quot;,
  &quot;date&quot;: &quot;2014/01/07 9:54:30 pm&quot;
  }]&#039;;
  // convert to usable PHP array
  $jsonToPHPArray = json_decode($json, true);

  // Loop over that shit
  foreach ($jsonToPHPArray as $key =&gt; $value) {
    // Check if post by that title already exists or not
    $post_exists = post_exists( $value[&#039;title&#039;] );
    if (!$post_exists) {
      // format to correct wordpress specific date  &#039;2010-02-23 18:57:33&#039;
      $postdate = date(&#039;Y-m-d H:i:s&#039;, strtotime($value[&#039;date&#039;]));
      $url = $value[&#039;url&#039;];
      $new_post = array(
      &#039;post_title&#039;    =&gt;   $value[&#039;title&#039;],
      &#039;post_date&#039;     =&gt;   $postdate,
      &#039;post_status&#039;   =&gt;   &#039;publish&#039;,
      // change this to your wordpress id
      &#039;post_author&#039;   =&gt;   3,
      // change this to your correct category ID
      &#039;post_category&#039; =&gt; array( 5 )
      );
      // create post in wordpress. Yay!
      $new_post_id = wp_insert_post($new_post);
      // add custom post meta to imported posts
      add_post_meta( $new_post_id, &#039;what_ever_post_meta_you_want&#039;, true );
      // This is to be google friendly when using Yoast WordPress SEO
      add_post_meta( $new_post_id, &#039;_yoast_wpseo_canonical&#039;, $url );
    }
  }
}
// I am triggering in the admin head, but you can fire this however you want
add_action(&#039;admin_head&#039;, &#039;insert_wordpress_posts_from_json&#039;);
```

Wordpress and PHP have some shitty memory limits so I kept the array to around 25 items per import.

Have fun!

P.S there are probably other ways to do this. This worked for me.