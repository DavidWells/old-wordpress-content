---
ID: 5240
post_title: 'Publishing flat npm packages for easier import paths &#038; smaller consumer bundle sizes'
author: David Wells
post_date: 2016-04-16 20:56:53
post_excerpt: ""
layout: post
permalink: >
  http://davidwells.io/publishing-flat-npm-packages-for-easier-import-paths-smaller-consumer-bundle-sizes/
published: true
external_post_url:
  - ""
---
I recently published a library on npm named `react-dom-primitives`.

It's a library that abstracts away base DOM nodes from jsx with the future goal on hot swapping out base DOM nodes for, let's say, react native components.

For example, you would use the `react-dom-primitives` `<P>` component instead of `<p>` in your render method. Then if you want to render your component in react native, you can swap the `<P>` component for the rn equivalent `<Text>` component under the hood.

Anywho, I was trying to find a way to publish an npm package so consumers could include these DOM primitives easily and not have to include the entire library if they just want a couple pieces.

Lodash is a good example of this. Where you can include only what you want from the entire library. This keeps your final build smaller.

### Paths before publishing flattly:
```js
import P from &#039;react-dom-primitives/lib/P&#039;
```

### Paths after publishing flattly:
```js
import P from &#039;react-dom-primitives/P&#039;
```

## How?

Well it turns out whatever directory you run `npm publish` from will be the package uploaded NPM. Duh....

So, instead of publishing from the root directory of the project (including your built `dist` or `lib` folder) you just need to copy over your package.json file into the directory of your built output and publish from there.

So my build script `npm run build` will run the normal build process, create the `/lib/` folder with the built output, then copy the current package.json file into the `/lib/` directory

Then I run `npm run dist` and I `cd` into the `lib` folder and `npm publish` from there.

```json
// package.json
 &quot;scripts&quot;: {
    &quot;start&quot;: &quot;node server.js&quot;,
    &quot;clean-lib&quot;: &quot;node_modules/.bin/rimraf ./lib&quot;,
    &quot;build&quot;: &quot;npm run clean-lib &amp;&amp; webpack --config webpack.production.config.js --colors --progress --inline &amp;&amp; npm run build:utils &amp;&amp; npm run build:index &amp;&amp; npm run copypackage&quot;,
    &quot;build:utils&quot;: &quot;webpack --config webpack.production.utils.config.js --colors --progress --inline&quot;,
    &quot;build:index&quot;: &quot;babel src/primatives/index.js --out-file lib/index.js&quot;,
    &quot;copypackage&quot;: &quot;cp -rf package.json lib&quot;,
    &quot;dist&quot;: &quot;cd lib &amp;&amp; npm publish&quot;
  },
```

This provides a much nicer way for developers to include only specific files from your package and will reduce their overall bundle size.

If you have nested dependencies that you also need to be flat. Check out [this post](https://medium.com/@jbscript/publishing-flat-modules-to-npm-4367f5e0c10d) for a cool gulp driven way to do that.