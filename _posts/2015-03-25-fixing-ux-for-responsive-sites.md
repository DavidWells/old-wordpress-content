---
ID: 4967
post_title: Fixing UX for Responsive Sites
author: David Wells
post_date: 2015-03-25 21:31:54
post_excerpt: ""
layout: post
permalink: >
  http://davidwells.io/fixing-ux-for-responsive-sites/
published: true
cta_content_placement:
  - below
---
<blockquote>Preface: this post isn't against responsive design, far from it. It's about fixing the glaring User experience issues faced on most responsive sites.</blockquote>
Lets face it responsive sites can be really, really, really shitty.

How many times have you been on your phone, clicked through a link on twitter only to see a smashed bullshit responsive design?

It's hard to read, it's unfamiliar, it's a pain to scroll, and it's might be hellaciously ugly.

It doesn't need to be this way.

<strong>Why not give visitors the choice of what type of mobile experience they would like with your site?</strong>
<h2>Why?</h2>
<h3>UX Problems with responsive sites:</h3>
<ul>
	<li>Users might be used to the desktop experience &amp; the responsive layout is frustrating to re-learn</li>
	<li>Text is harder to read because there are like 4 words per line</li>
	<li>Scroll hell - Touch scrolling over and over and over to get to where you want to go</li>
	<li>Design limitations - typically desktop views of marketing sites look far superior</li>
	<li>New unfamiliar mobile navigation</li>
	<li>No <a href="https://www.google.com/search?q=hamburger+menu+research&amp;oq=hamburger+menu+research+&amp;aqs=chrome..69i57.9329j0j4&amp;sourceid=chrome&amp;es_sm=119&amp;ie=UTF-8">one knows what a hamburger menu</a> is... still...</li>
	<li>etc</li>
</ul>
Not all responsive sites are created equally, some are great and do a fantastic job of addressing these issues.

The other 99% of the internet with shit responsive sites is what we are addressing.

Why not solve all of these issues with responsive sites with one simple trick?

<h2>Introducing Responsible.js</h2>

The plague of shitty responsive sites has been nagging at me for a while now.

I decided to take a stab at fixing the issue by creating <a href="https://github.com/DavidWells/responsible">responsible.js</a>.

https://www.youtube.com/watch?v=VtSvk8xnmBE

<a href="http://davidwells.tv/code/responsible"><img class=" size-full wp-image-4968 aligncenter" src="http://www.davidwells.tv/wp-content/uploads/2015/03/responsible.gif" alt="responsible" width="367" height="617" /></a>

<a href="https://github.com/DavidWells/responsible">Responsible.js</a> is a pretty simple trick turned into a lightweight javascript module.
<h3>How does it work?</h3>
All it does is deactivate the responsive CSS on the website and reset the device viewport with javascript when the visitor opts for the desktop version of the site.

That's it.

What this allows for is a seamless user experience switching from a mobile view to the normal desktop view of the site.

No page reloads, no crazy shenanigans, <strong>just a good user experience </strong>.

<h3 id="give-visitors-a-better-experience">Give Visitors a better experience</h3>

People deserve the ability to choose how they wish to consume your website.

If they want to view the mobile responsive version, let them. (it's the default behavior)

If they want to view the desktop version of the site, let them.

Some folks don't mind pinching and zooming to have a desktop like mobile experience.

The ones that do care, they simply use the default mobile view.
<h3><a href="http://davidwells.tv/code/responsible">View the Demo here</a></h3>
Open demo on mobile device or emulate mobile on devtools.
<h3>Whats the catch?</h3>
Aha, there is one catch! To get this working, you will need to separate your responsive CSS into a separate file.

So any mediaQuery targeting a mobile device, you would place into a new css file (responsive.css)

This might require some additional build steps for ya, but the UX gain is crazy awesome.
<h3>Pull Requests Welcome</h3>
I open sourced the library on <a href="https://github.com/DavidWells/responsible">Github</a>. Feel free to fork and contribute.