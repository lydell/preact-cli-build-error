# Bad preact-cli build error

1. `npm ci`
2. `npm run dev`

The build should fail, because `src/index.js` intentionally has an error: It
unconditionally tries to access `window`.

preact-cli@2.2.1 output (good):

```
ReferenceError: window is not defined
method: JkW7
at: /home/lydell/stuff/preact-simple-test/src/index.js:3:12

Source code:

import { h } from 'preact';
console.log(window.location);
export default () => <p>Hello, world!</p>;

This is most likely caused by using DOM or Web APIs.
Pre-render runs in node and has no access to globals available in browsers.

Consider wrapping code producing error in: 'if (typeof window !== "undefined") { ... }'

Alternatively use 'preact build --no-prerender' to disable prerendering.

See https://github.com/developit/preact-cli#pre-rendering for further information.
```

preact-cli@3.0.0-next.14 output (bad):

```
(node:16457) DeprecationWarning: Tapable.plugin is deprecated. Use new API on `.hooks` instead
Build [===============] 98% (1.4s) after emitting ssr-bundle.js ⏤  3.86 kB (+3.86 kB)

Build [===============] 98% (1.2s) after emitting ssr-build/ssr-bundle.js ⏤  0 B (-3.86 kB)
         bundle.048c7.js ⏤  3.98 kB (+3.98 kB)
      polyfills.e99bc.js ⏤  2 kB (+2 kB)
              index.html ⏤  415 B (+415 B)

Total precache size is about 77.7 kB for 5 resources.
✖ ERROR Template execution failed: TypeError: sourceMapConsumer.originalPositionFor is not a function
✖ ERROR   TypeError: sourceMapConsumer.originalPositionFor is not a function
  
  - prerender.js:54 handlePrerenderError
    [preact-simple-test]/[preact-cli]/lib/lib/webpack/prerender.js:54:32
  
  - prerender.js:36 module.exports
    [preact-simple-test]/[preact-cli]/lib/lib/webpack/prerender.js:36:3
  
  - render-html-plugin.js:44 Object.ssr
    [preact-simple-test]/[preact-cli]/lib/lib/webpack/render-html-plugin.js:44:31
  
  - template.html:129 L6k0.module.exports
    [preact-simple-test]/[preact-cli]/lib/resources/template.html:129:37
  
  - index.js:284 Promise.resolve.then
    [preact-simple-test]/[html-webpack-plugin]/index.js:284:18
  
  - es6-promise.js:409 tryCatch
    [preact-simple-test]/[es6-promise]/dist/es6-promise.js:409:12
  
  - es6-promise.js:424 invokeCallback
    [preact-simple-test]/[es6-promise]/dist/es6-promise.js:424:13
  
  - es6-promise.js:176 
    [preact-simple-test]/[es6-promise]/dist/es6-promise.js:176:14
  
  - es6-promise.js:128 flush
    [preact-simple-test]/[es6-promise]/dist/es6-promise.js:128:5
  
  - next_tick.js:61 process._tickCallback
    internal/process/next_tick.js:61:11
  

(node:16457) UnhandledPromiseRejectionWarning: Build failed! null
(node:16457) UnhandledPromiseRejectionWarning: Unhandled promise rejection. This error originated either by throwing inside of an async function without a catch block, or by rejecting a promise which was not handled with .catch(). (rejection id: 1)
(node:16457) [DEP0018] DeprecationWarning: Unhandled promise rejections are deprecated. In the future, promise rejections that are not handled will terminate the Node.js process with a non-zero exit code.
```
