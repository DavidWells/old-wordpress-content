---
ID: 5250
post_title: >
  Create and Deploy Static Sites and Demos
  in Seconds
author: David Wells
post_date: 2016-04-21 06:25:54
post_excerpt: ""
layout: post
permalink: >
  http://davidwells.io/create-deploy-static-sites-demos-seconds/
published: true
external_post_url:
  - ""
---
[Surge.sh](http://surge.sh) is one of my favorite developer tools.

It will allow you to launch static sites & demos to a live web URL in literally seconds from your terminal.

# See for yourself

http://www.youtube.com/watch?v=LZA8QVLOinE

----

# Step 1: Install it
```bash
# Install surge.sh on your machine
 npm install --global surge
# In your project directory, just run…
$ surge
```

# Step 2: deploy

`cd` into your projects directory and then run `surge`. 

Point it to the correct file paths and boom. Done.

Your site will be live.

# Pro Tips

Avoid retyping your custom domain everything with an npm script with `surge --domain your-surge-subdomain-you-chose.surge.sh`

Example package.json

```json
{
  &quot;name&quot;: &quot;your-project-name&quot;,
  &quot;version&quot;: &quot;1.0.0&quot;,
  &quot;description&quot;: &quot;&quot;,
  &quot;scripts&quot;: {
    &quot;start&quot;: &quot;node server.js&quot;,
    &quot;deploy&quot;: &quot;surge --domain apple-login.surge.sh&quot;,
  },
}
```

They also allow for mapping custom domains for... FREE! 

[Instructions on how to setup your custom domain](http://surge.sh/help/adding-a-custom-domain)

After setting up your CNAME or A Record for your custom domain, run `echo your-own-domain.com > CNAME` in your project's root directory to avoid retyping it. After doing so, you can simply deploy to your custom domain using the `surge` command.

Great service. Amazingly easy and simple.

This is a must use for any developer trying to share a demo or stand up a static site without dealing with setting up your own server.

## Serverless for the win!