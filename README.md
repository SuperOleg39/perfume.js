<a href="http://www.perfumejs.com/">
  <img src="https://github.com/Zizzamia/perfume.js/blob/master/docs/src/assets/perfume-logo-v0-8-1.png" align="left" width="262" />
</a>

# [Perfume.js v0.8.1](http://perfumejs.com)

[![NPM version](https://badge.fury.io/js/perfume.js.svg)](https://www.npmjs.org/package/perfume.js) [![Build Status](https://travis-ci.org/Zizzamia/perfume.js.svg?branch=master)](https://travis-ci.org/Zizzamia/perfume.js) [![NPM Downloads](http://img.shields.io/npm/dm/perfume.js.svg)](https://www.npmjs.org/package/perfume.js) [![Test Coverage](https://api.codeclimate.com/v1/badges/f813d2f45b274d93b8c5/test_coverage)](https://codeclimate.com/github/Zizzamia/perfume.js/test_coverage) [![JS gzip size](https://img.badgesize.io/https://unpkg.com/perfume.js?compression=gzip&label=JS+gzip+size)](https://unpkg.com/perfume.js)

> Perfume is a JavaScript library for measuring Short and Long Script, First (Contentful) Paint ([FP/FCP](https://medium.com/@zizzamia/first-contentful-paint-with-a-touch-of-perfume-js-cd11dfd2e18f)), First Input Delay (FID), Time to Interactive ([TTI](https://medium.com/@zizzamia/time-to-interactive-with-rum-862ba874392c)), Component First Paint (CFM), annotating them to the DevTools timeline and reporting the results to Google Analytics.

<br />
<br />
<br />
<br />

## User-centric performance metrics

**Perfume** leverage the latest W3C Performance Drafts (like PerformanceObserver), for measuring performance that matters! ⚡️

* First Paint (FP)
* First Contentful Paint (FCP)
* First Input Delay (FID)
* Time to Interactive (TTI)
* Component First Paint (CFP)

![Performance Metrics load timeline](https://github.com/Zizzamia/perfume.js/blob/master/docs/src/assets/perf-metrics-load-timeline.png)

## Installing

npm (https://www.npmjs.com/package/perfume.js):

    npm install perfume.js --save-dev

## Importing library

You can import the generated bundle to use the whole library generated by this starter:

```javascript
import Perfume from 'perfume.js';
```

Additionally, you can import the transpiled modules from `dist/es` in case you have a modular library:

```javascript
import Perfume from 'node_modules/perfume.js/dist/es/perfume';
```

Universal Module Definition:

```javascript
import Perfume from 'node_modules/perfume.js/perfume.umd.js';
```

## Start measuring

### First Paint ([FP](https://medium.com/@zizzamia/first-contentful-paint-with-a-touch-of-perfume-js-cd11dfd2e18f))

**FP** is the exact time the browser renders anything as visually different from what was on the screen before navigation, e.g. a background change after a long blank white screen time.

```javascript
const perfume = new Perfume({
  firstPaint: true
});
// ⚡️ Perfume.js: First Paint 1482.00 ms
```

### First Contentful Paint ([FCP](https://medium.com/@zizzamia/first-contentful-paint-with-a-touch-of-perfume-js-cd11dfd2e18f))

**FCP** is the exact time the browser renders the first bit of content from the DOM, which can be anything from an important image, text, or even the small SVG at the bottom of the page.

```javascript
const perfume = new Perfume({
  firstContentfulPaint: true
});
// ⚡️ Perfume.js: First Contentful Paint 2029.00 ms
```

### First Input Delay (FID)

**FID** measures the time from when a user first interacts with your site (i.e. when they click a link, tap on a button) to the time when the browser is actually able to respond to that interaction..

```javascript
const perfume = new Perfume({
  firstInputDelay: true
});
// ⚡️ Perfume.js: First Input Delay 3.20 ms
```

### Time to Interactive ([TTI](https://medium.com/@zizzamia/time-to-interactive-with-rum-862ba874392c))

The metric marks the point at which your application is both visually rendered and capable of reliably responding to user input. An application could be unable to respond to user input for a couple of reasons:

* The JavaScript needed to make the components on the page work hasn't yet loaded;
* There are long tasks blocking the main thread.
  The **TTI** metric identifies the point at which the page's initial JavaScript is loaded and the main thread is idle (free of long tasks).

```javascript
const perfume = new Perfume({
  timeToInteractive: true
});
// ⚡️ Perfume.js: Time to interactive 2452.07 ms
```

### Annotate metrics in the DevTools

**Performance.mark** ([User Timing API](https://developer.mozilla.org/en-US/docs/Web/API/User_Timing_API)) is used to create an application-defined peformance entry in the browser's performance entry buffer.

```javascript
perfume.start('fibonacci');
fibonacci(400);
perfume.end('fibonacci');
// ⚡️ Perfume.js: fibonacci 0.14 ms
```

![Performance Mark](https://github.com/Zizzamia/perfume.js/blob/master/docs/src/assets/performance-mark.png)

### Component First Paint (CFP)

This metric mark the point, immediately after creating a **new component**, when the browser renders pixels to the screen.

```javascript
perfume.start('togglePopover');
$(element).popover('toggle');
perfume.endPaint('togglePopover');
// ⚡️ Perfume.js: togglePopover 10.54 ms
```

![Performance CFP](https://github.com/Zizzamia/perfume.js/blob/master/docs/src/assets/performance-cfp.png)

### Custom Logging

Save the duration and print it out exactly the way you want it.

```javascript
const perfume = new Perfume({
  logPrefix: '🍹 HayesValley.js:'
});
perfume.start('fibonacci');
fibonacci(400);
const duration = this.perfume.end('fibonacci');
perfume.log('Custom logging', duration);
// 🍹 HayesValley.js: Custom logging 0.14 ms
```

### Google Analytics

To enable Perfume to send your measures to Google Analytics User timing, set the option `enable:true` and a custom [user timing variable](https://developers.google.com/analytics/devguides/collection/analyticsjs/field-reference#timingVar) `timingVar:"name"`.

```javascript
const perfume = new Perfume({
  googleAnalytics: {
    enable: true,
    timingVar: 'userId'
  }
});
```

![Performance Analytics](https://github.com/Zizzamia/perfume.js/blob/master/docs/src/assets/performance-analytics.png)

### Generic analytics platform support

Configurable analytics callback to use Perfume.js with any platform.

```javascript
const perfume = new Perfume({
  analyticsLogger: (metricName, duration) => {
    myAnalyticsTool.track(metricName, duration);
  })
});
```

### Default Options

Default options provided to Perfume.js constructor.

```javascript
const options = {
  firstPaint: false,
  firstContentfulPaint: false,
  timeToInteractive: false,
  analyticsLogger: undefined,
  googleAnalytics: {
    enable: false,
    timingVar: 'name'
  },
  logging: true,
  logPrefix: '⚡️ Perfume.js:',
  warning: false
};
```

### Utilities

Perfume.js expose some methods and properties which may be useful to people extending the library.

```javascript
// First Paint value
perfume.firstPaintDuration;

// First Contentful Paint value
perfume.firstContentfulPaintDuration;

// Time To Interactive value (Async)
perfume.observeTimeToInteractive();

// Send Custom User timing measure to Google Analytics
perfume.sendTiming(metricName, duration);
```

## Develop

* `npm start`: Run `npm run build` in watch mode
* `npm run test`: Run test suite
* `npm run test:watch`: Run test suite in [interactive watch mode](http://facebook.github.io/jest/docs/cli.html#watch)
* `npm run build`: Generate bundles and typings
* `npm run lint`: Lints code

## Articles

* [First (Contentful) Paint with a touch of Perfume(.js)](https://medium.com/@zizzamia/first-contentful-paint-with-a-touch-of-perfume-js-cd11dfd2e18f)
* [Time to Interactive with RUM](https://medium.com/@zizzamia/time-to-interactive-with-rum-862ba874392c)

## Credits and Specs

Made with ☕️ by [@zizzamia](https://twitter.com/zizzamia) and
I want to thank some friends and projects for the work they did:

* [Leveraging the Performance Metrics that Most Affect User Experience](https://developers.google.com/web/updates/2017/06/user-centric-performance-metrics) for documenting this new User-centric performance metrics;
* [Performance Timeline Level 2](https://w3c.github.io/performance-timeline/) the definition of _PerformanceObserver_ in that specification;
* [The Contributors](https://github.com/Zizzamia/perfume.js/graphs/contributors) for their much appreciated Pull Requests and bug reports;
* **you** for the star you'll give this project 😉 and for supporting me by giving my project a try 😄

## Copyright and license

Code and documentation copyright 2018 [Leonardo Zizzamia](https://twitter.com/Zizzamia). Code released under the [MIT license](LICENSE). Docs released under Creative Commons.
