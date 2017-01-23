---
ID: 5253
post_title: >
  Speed up your AVA (or Mocha) testing
  feedback loop using the .only flag
author: David Wells
post_date: 2016-04-22 06:30:26
post_excerpt: ""
layout: post
permalink: >
  http://davidwells.io/speed-up-your-ava-or-mocha-testing-feedback-loop-using-the-only-flag/
published: true
external_post_url:
  - ""
---
Setting up testing and debugging testing can be a real pain in the keister.

As the number of tests you have starts to grow, things get slower and it can be hard to debug certain tests because your terminal output is rather large.

To help mitigate slow tests and speed up your feedback/debug loop simply run the `.only` flag on your test.

This is a quick tip for the [AVA](https://github.com/sindresorhus/ava) test runner. (it will also work if you are using mocha for testing)

By using the `.only` flag on your tests you tell it to ignore all other tests and just run that single test instance.

This is handy for debugging objects and expected values that you are writing your assertions on.

```js
// HoverContent.spec.js
import React from &#039;react&#039; // dope frontend library
import test from &#039;ava&#039; // awesome test runner
import HoverContent from &#039;./HoverContent&#039; // react component
import { mount, shallow } from &#039;enzyme&#039; // great react testing helper lib

// this test won&#039;t run because .only is used on the test below
test(&#039;HoverContent is &lt;div&gt; tag&#039;, (t) =&gt; {
  var hovercontent = shallow(&lt;HoverContent /&gt;)
  t.is(hovercontent.type(), &#039;div&#039;)
})

// this test will be the .only one to run! Huzzah!
test.only(&#039;shows content on mouseEnter&#039;, (t) =&gt; {
  const wrapper = mount(
    &lt;HoverContent content={&lt;span&gt;content here&lt;/span&gt;}&gt;
       Visible before hover
    &lt;/HoverContent&gt;
  )
  console.log(&#039;what the heck is in this&#039;, wrapper)
  wrapper.simulate(&#039;mouseenter&#039;)
  t.true(wrapper.state(&#039;visible&#039;) === true)
})
```

http://www.youtube.com/watch?v=tAymDbi7HSw

[Information on how to using .only with mocha](http://jaketrent.com/post/run-single-mocha-test/)

Hope this helps you! Happy testing!