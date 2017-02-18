# entry-webpack-plugin
## Disclaimer
This plugin uses UNDOCUMENTED APIs and it comes with ABSOLUTELY NO WRRANTY, as
always. Only for stupids who use RC or beta tools in their productions.

## `whoami`
entry-webpack-plugin is a plugin to create an entrypoint for use in other
modules.
For example, your HTML file to be loaded with
[html-loader](https://github.com/webpack-contrib/html-loader) may have the
following line.

```HTML
<script>${require("entry-webpack-plugin/loader!./index.js")}</script>
```

...and you have `index.js` like this:
```JavaScript
console.log("Hello, World!");
```

That will result in a straightforward output:
```HTML
<script>!function(n){function r(t){if(e[t])return e[t].exports;var o=e[t]={i:t,l:!1,exports:{}};return n[t].call(o.exports,o,o.exports,r),o.l=!0,o.exports}var e={};return r.m=n,r.c=e,r.i=function(n){return n},r.d=function(n,e,t){r.o(n,e)||Object.defineProperty(n,e,{configurable:!1,enumerable:!0,get:t})},r.n=function(n){var e=n&&n.__esModule?function(){return n.default}:function(){return n};return r.d(e,"a",e),e},r.o=function(n,r){return Object.prototype.hasOwnProperty.call(n,r)},r.p="/",r(r.s=0)}([function(n,r){console.log("Hello, World!")}]);</script>
```

`index.js` is compiled as an entrypoint and the output includes the runtime
and bundled modules so that the result is completely legitimate.
If you don't want the script to be inlined, you can use
[file-loader](https://github.com/webpack-contrib/file-loader) and
[extract-loader](https://github.com/peerigon/extract-loader). You may also
use other loaders and plugins to take advantage of the power of Webpack.

![UNLIMITED POWER OF WEBPACK](http://i.giphy.com/kqGNZmuPC2BZS.gif)

## Basic Use
First, install this plugin.

```
npm install https://github.com/kagucho/entry-webpack-plugin.git
```

Then, add the plugin to your `options.plugins`
```JavaScript
require("entry-webpack-plugin/plugin")
```

Now you can `require` entrypoints in your HTML/JavaScript/whatever.

```
require("entry-webpack-plugin/loader!path/to/your/entrypoint")
```

That's it.

## Using with extract-webpack-plugin
If you love this crazy plugin, you will certainly love
extract-text-webpack-plugin. Fortunately, entry-webpack-plugin can be used with
the plugin.

Setup extract-text-webpack-plugin as usual. If you extract texts into a file
named `css`, you can `require` it with a query:

```
require("entry-webpack-plugin/loader?css!path/to/your/entrypoint")
```

If you `require` a file, entry-webpack-plugin will remove the file from the
output, so you will not have redundant files.

See `sample` directory for the example. It may help you understand.

## Problems
This plugin cannot handle multiple entrypoints correctly since each child
compilers are independent. So you cannot use something like
[CommonChunkPlugin](https://webpack.js.org/plugins/commons-chunk-plugin/).

Although it is possible to implement a plugin/loader to handle all entrypoints
in a child compiler, I decided not to do that because users is likely to use
another child compiler in other loaders/plugins such as extract-loader to make
the final output. (e.g. combination of extract-loader and html-loader)

I consider that is a limitation of the architecture of Webpack. To solve the
issue, you need to make the feature to create entrypoint independent from the
core as loaders are.

## Acknowledgement
This plugin was made for a talk in [webpack-sake](https://ng-sake.connpass.com/event/50773/).
Thanks for everyone who listened to the talk about this hacky trick.
