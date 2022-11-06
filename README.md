<a href="http://www.perfumejs.com/">
  <img src="https://github.com/Zizzamia/perfume.js/blob/master/docs/src/assets/perfume-logo-v5-0-0.png"
  align="left" width="200" alt="Perfume.js logo" />
</a>

# [Perfume.js v8.0.0](http://perfumejs.com)

[![Current version](https://img.shields.io/github/tag/zizzamia/perfume.js?color=3498DB&label=version)](https://www.npmjs.org/package/perfume.js) [![Test Coverage](https://api.codeclimate.com/v1/badges/f813d2f45b274d93b8c5/test_coverage)](https://codeclimate.com/github/Zizzamia/perfume.js/test_coverage) <img alt="No dependencies" src="https://img.shields.io/badge/dependencies-none-27ae60.svg"> [![Build Status](https://travis-ci.org/Zizzamia/perfume.js.svg?branch=master)](https://travis-ci.org/Zizzamia/perfume.js) [![NPM Downloads](http://img.shields.io/npm/dm/perfume.js.svg)](https://www.npmjs.org/package/perfume.js) [![gzip size](https://img.badgesize.io/https://unpkg.com/perfume.js?compression=gzip&label=JS+gzip+size)](https://unpkg.com/perfume.js) [![brotli size](https://img.badgesize.io/https://unpkg.com/perfume.js?compression=brotli&label=JS+brotli+size)](https://unpkg.com/perfume.js)

> <b>Page Speed</b> is a feature, and to deliver it we need to understand the many factors and fundamental limitations that are at play. If we can measure it, we can improve it.

<br />
<br />
<br />
<br />

English | [简体中文](./README-zh_CN.md) | [Italian](./README-it.md) | [한국어](./README-ko.md)

## Why Perfume.js?

Perfume is a tiny, web performance monitoring library that reports field data back to your favorite analytics tool.

- ⏰ Supports latest Performance APIs for precise metrics
- 🚀 Device data enrichment
- 🔨 Cross browser tested
- 🚿 Filters out false positive/negative results
- 🤙 Only 2.2Kb gzip
- 🏅 Web Vitals Score
- 🛰 Flexible analytics tool
- ⚡️ Waste-zero ms with [requestIdleCallback](https://developers.google.com/web/updates/2015/08/using-requestidlecallback) strategy built-in
  <br />

## The latest in metrics & Real User Measurement

**Perfume** leverages the latest Performance APIs to collect **field data** that allows us to understand what real-world users are actually experiencing.

- Navigation Timing
- Navigator Interface
- Resource Timing
- Element Timing
- Service Worker Status
- StorageManager interface
- First Contentful Paint ([FCP](https://web.dev/first-contentful-paint/))
- Largest Contentful Paint ([LCP](https://web.dev/lcp/))
- First Input Delay ([FID](https://web.dev/fid/))
- Cumulative Layout Shift ([CLS](https://web.dev/cls/))
- Total Blocking Time ([TBT](https://web.dev/tbt/))
- Navigation Total Blocking Time (NTBT)
- [Web Vitals Score](https://web.dev/vitals/)


<br />
At Coinbase, we use [Perfume.js to capture a high-level scoring system](https://www.coinbase.com/blog/performance-vitals-a-unified-scoring-system-to-guide-performance-health-and-prioritization) that is clear, trusted, and easy to understand.

Summarizing the performance health of an application into a reliable and consistent score helps increase urgency and directs company attention and resources towards addressing each performance opportunity.
<br />


## Perfume.js vs [Web Vitals](https://github.com/GoogleChrome/web-vitals)

**Perfume** leverages the [Web Vitals](https://github.com/GoogleChrome/web-vitals) library to collect all the standardized performance metrics. It explores new metrics like Navigation Total Blocking Time and dimensions like Low-End Devices, Service Worker status to understand your data better.

So don't worry, Perfume.js is a superset of Web Vitals, a bit like Typescript is a superset of Javascript.

## Installing

npm (https://www.npmjs.com/package/perfume.js):

    npm install perfume.js --save

### Importing library

You can import the generated bundle to use the whole library generated:

```javascript
import Perfume from 'perfume.js';
```

Universal Module Definition:

```javascript
import Perfume from 'node_modules/perfume.js/dist/perfume.umd.min.js';
```

<br />

## Quick start

Metrics like **Navigation Timing**, **Network Information**, **FP**, **FCP**, **FID**, **LCP**, **CLS** and **TBT** are default reported with Perfume; All results will be reported to the `analyticsTracker` callback, and the code below is just one way for you to organize your tracking, feel free to tweak it suit your needs.

🚀 Visit [perfumejs.com](http://perfumejs.com/) for a live demo on how the metrics work. 🌕

```javascript
const perfume = new Perfume({
  analyticsTracker: options => {
    const {
      attribution,
      metricName,
      data,
      navigatorInformation,
      rating
    } = options;
    switch (metricName) {
      case 'navigationTiming':
        if (data && data.timeToFirstByte) {
          myAnalyticsTool.track('navigationTiming', data);
        }
        break;
      case 'networkInformation':
        if (data && data.effectiveType) {
          myAnalyticsTool.track('networkInformation', data);
        }
        break;
      case 'storageEstimate':
        myAnalyticsTool.track('storageEstimate', data);
        break;
      case 'TTFB':
        myAnalyticsTool.track('timeToFirstByte', { duration: data });
        break;
      case 'RT':
        myAnalyticsTool.track('redirectTime', { duration: data });
        break;
      case 'FCP':
        myAnalyticsTool.track('firstContentfulPaint', { duration: data });
        break;
      case 'FID':
        myAnalyticsTool.track('firstInputDelay', { duration: data });
        break;
      case 'LCP':
        myAnalyticsTool.track('largestContentfulPaint', { duration: data });
        break;
      case 'CLS':
        myAnalyticsTool.track('cumulativeLayoutShift', { value: data });
        break;
      case 'TBT':
        myAnalyticsTool.track('totalBlockingTime', { duration: data });
        break;
      case 'elPageTitle':
        myAnalyticsTool.track('elementTimingPageTitle', { duration: data });
        break;
      default:
        myAnalyticsTool.track(metricName, { duration: data });
        break;
    }
  },
});
```

In a world with widely varying device capabilities, a one-size-fits-all event doesn’t always work. Perfume adds **data enrichment** to all events so we can better understand the real world experiences:

- **deviceMemory**: the user's device memory (RAM).
- **hardwareConcurrency**: the number of logical CPU processor cores on the user's device.
- **serviceWorkerStatus**: status of the service worker between controlled, supported and unsupported.

Based on the Navigator APIs the library can help us differentiate between a low-end and a high-end device/experience:

- **isLowEndDevice**: combination of the score of RAM and CPU.
- **isLowEndExperience**: combination of the score of RAM, CPU, NetworkStatus and SaveData.

## Performance audits

Coo coo coo [cool](https://www.youtube.com/watch?v=zDcbpFimUc8), let's learn something new.

### Navigation Timing

Navigation Timing collects performance metrics for the life and timings of a network request.

Perfume helps expose some of the key metrics you might need.

<ul>
  <li><b>Redirect time</b>: Page redirects aren't totally inconsequential, but they might not be something you run into very often. Still, redirects add latency to requests, so measuring them may be worth the effort.</li>
  <li><b>DNS lookup</b>: When a user requests a URL, the Domain Name System (DNS) is queried to translate a domain to an IP address.</li>
  <li><b>Header size</b>: HTTP header size</li>
  <li><b>Fetch time</b>: Cache seek plus response time</li>
  <li><b>Worker time</b>: Service worker time plus response time</li>
  <li><b>Total time</b>: Request plus response time (network only)</li>
  <li><b>Download time</b>: Response time only (download)</li>
  <li><b>Time to First Byte</b>: The amount of time it takes after the client sends an HTTP GET request to receive the first byte of the requested resource from the server.
    It is the largest web page load time component taking 40 to 60% of total web page latency.</li>
</ul>

```javascript
// Perfume.js: navigationTiming { ... timeToFirstByte: 192.65 }
```

### First Paint ([FP](https://medium.com/@zizzamia/first-contentful-paint-with-a-touch-of-perfume-js-cd11dfd2e18f))

**FP** is the exact time taken for the browser to render anything as visually different from what was on the screen before navigation, e.g. a background change after a long blank white screen time.

```javascript
// Perfume.js: fp 1482.00 ms
```

### First Contentful Paint ([FCP](https://medium.com/@zizzamia/first-contentful-paint-with-a-touch-of-perfume-js-cd11dfd2e18f))

**FCP** is the exact time taken for the browser to render the first bit of content from the DOM, which can be anything from an important image, text, or even the small SVG at the bottom of the page.

```javascript
// Perfume.js: fcp 2029.00 ms
```

### Largest Contentful Paint (LCP)

**LCP** is an important, user-centric metric for measuring perceived load speed because it marks the point in the page load timeline when the page's main content has likely loaded—a fast LCP helps reassure the user that the page is useful.

We end the Largest Contentful Paint measure at two points: when First Input Delay happen and when the page's lifecycle state changes to hidden.

```javascript
// Perfume.js: lcp 2429.00 ms
```

### First Input Delay (FID)

**FID** measures the time from when a user first interacts with your site (i.e. when they click a link, tap on a button) to the time when the browser is actually able to respond to that interaction.

```javascript
// Perfume.js: fid 3.20 ms
```

### Cumulative Layout Shift (CLS)

**CLS** is an important, user-centric metric for measuring visual stability because it helps quantify how often users experience unexpected layout shifts—a low CLS helps ensure that the page is delightful.

We end the Cumulative Layout Shift measure at two points: when First Input Delay happen and when the page's lifecycle state changes to hidden.

```javascript
// Perfume.js: cls 0.13
```

### Total Blocking Time (TBT)

**TBT** is an important, user-centric metric for measuring load responsiveness because it helps quantify the severity of how non-interactive a page is prior to it becoming reliably interactive—a low TBT helps ensure that the page is usable.

We end the Total Blocking Time measure 10 seconds after FID.

```javascript
// Perfume.js: tbt 347.07 ms
```

### Navigation Total Blocking Time (NTBT)

This metric measures the amount of time the application may be blocked from processing code during the 2s window after a user navigates from page A to page B. The NTBT metric is the summation of the blocking time of all long tasks in the 2s window after this method is invoked.

Because this library is navigation agnostic, we have this method to mark when the navigation starts.

If this method is called before the 2s window ends; it will trigger a new NTBT measurement and interrupt the previous one.

```javascript
import { createBrowserHistory } from 'history';
export const history = createBrowserHistory();

const perfume = new Perfume({
  analyticsTracker: ({ metricName, data }) => {
    myAnalyticsTool.track(metricName, data);
  })
});

// React custom history
history.listen(() => {
  // Measure NTBT at the beginning of each navigation
  perfume.markNTBT();
});

// Perfume.js: ntbt 78 ms
```

### Resource Timing

Resource Timing collects performance metrics for document-dependent resources. Stuff like style sheets, scripts, images, et cetera.
Perfume helps expose all PerformanceResourceTiming entries and groups data data consumption by Kb used.

```javascript
const perfume = new Perfume({
  resourceTiming: true,
  analyticsTracker: ({ metricName, data }) => {
    myAnalyticsTool.track(metricName, data);
  })
});
// Perfume.js: dataConsumption { "css": 185.95, "fetch": 0, "img": 377.93, ... , "script": 8344.95 }
```

### Annotate metrics in the DevTools

**Performance.mark** ([User Timing API](https://developer.mozilla.org/en-US/docs/Web/API/User_Timing_API)) is used to create an application-defined peformance entry in the browser's performance entry buffer.

```javascript
const perfume = new Perfume({
  analyticsTracker: ({ metricName, data }) => {
    myAnalyticsTool.track(metricName, data);
  })
});
perfume.start('fibonacci');
fibonacci(400);
perfume.end('fibonacci');
// Perfume.js: fibonacci 0.14 ms
```

![Performance Mark](https://github.com/Zizzamia/perfume.js/blob/master/docs/src/assets/performance-mark.png)

### Component First Paint

This metric mark the point, immediately after creating a **new component**, when the browser renders pixels to the screen.

```javascript
const perfume = new Perfume({
  analyticsTracker: ({ metricName, data }) => {
    myAnalyticsTool.track(metricName, data);
  })
});
perfume.start('togglePopover');
$(element).popover('toggle');
perfume.endPaint('togglePopover');
// Perfume.js: togglePopover 10.54 ms
```

![Performance](https://github.com/Zizzamia/perfume.js/blob/master/docs/src/assets/performance-cfp.png)

### Element Timing

Track when image elements and text nodes are displayed on screen using the [emerging](https://chromestatus.com/features#elementtiming) [Element Timing API](https://wicg.github.io/element-timing/) specification by simply adding the `elementtiming` attribute with a descriptive value of your choice to HTML elements you would like to measure:

```html
<h1 elementtiming="elPageTitle" class="title">Perfume.js</h1>
<img
  elementtiming="elHeroLogo"
  alt="Perfume.js logo"
  src="https://zizzamia.github.io/perfume/assets/perfume-logo-v5-0-0.png"
/>
```

```javascript
const perfume = new Perfume({
  elementTiming: true,
  analyticsTracker: ({ metricName, data }) => {
    myAnalyticsTool.track(metricName, data);
  })
});

// Perfume.js: elPageTitle 256.00 ms
// Perfume.js: elHeroLogo 1234.00 ms
```

## Web Vitals Score

Perfume will expose for all major metrics the vitals score, those can be used to improve your [SEO and Google page rank](https://webmasters.googleblog.com/2020/05/evaluating-page-experience.html).

| Web Vitals                                |   Good | Needs Improvement |      Poor |
| ----------------------------------------- | -----: | ----------------: | --------: |
| Time to First Byte (ttfb)                 | 0-200 |            201-500 |  Over 500 |
| Fist Paint (fp)                           | 0-2000 |         2001-4000 | Over 4000 |
| First Contentful Paint (fcp)              | 0-2000 |         2001-4000 | Over 4000 |
| Largest Contentful Paint (lcp)            | 0-2500 |         2501-4000 | Over 4000 |
| First Input Delay (fid)                   |  0-100 |           101-300 |  Over 300 |
| Cumulative Layout Shift (cls)             |  0-0.1 |         0.11-0.25 | Over 0.25 |
| Total Blocking Time (tbt)                 |  0-200 |           201-600 |  Over 600 |
| Navigation Total Blocking Time (tbt)      |  0-200 |           201-600 |  Over 600 |

## Perfume custom options

Default options provided to Perfume.js constructor.

```javascript
const options = {
  resourceTiming: false,
  elementTiming: false,
  analyticsTracker: options => {},
  maxMeasureTime: 30000,
};
```

## Use Google Analytics

A quick way to see your page speed results on your web app is by using Google Analytics. Those GA events will show on Behavior > Site Speed > User Timings. For testing you might want to see them coming live on Realtime > Events.

Have fun ✨

```javascript
const metricNames = ['TTFB', 'RT', 'FCP', 'LCP', 'FID', 'CLS', 'TBT'];
new Perfume({
  analyticsTracker: ({ attribution, metricName, data, navigatorInformation, rating }) => {
    if (metricNames.includes(metricName)) {
      ga('send', 'event', {
        eventCategory: 'Perfume.js',
        eventAction: metricName,
        // Google Analytics metrics must be integers, so the value is rounded
        eventValue: metricName === 'cls' ? data * 1000 : data,
        eventLabel: navigatorInformation.isLowEndExperience ? 'lowEndExperience' : 'highEndExperience',
        // Use a non-interaction event to avoid affecting bounce rate
        nonInteraction: true,
      });
    }
  })
});
```

To connect with additional analytics providers, checkout the [analytics plugin for Perfume.js](https://getanalytics.io/plugins/perfumejs/).

<br />

## Develop

- `npm run test`: Run test suite
- `npm run build`: Generate bundles and typings
- `npm run lint`: Lints code
<br />

## Plugins

- [Perfume.js plugin for GatsbyJS](https://github.com/NoriSte/gatsby-plugin-perfume.js)
- [Perfume.js plugin for Analytics](https://github.com/DavidWells/analytics/tree/master/packages/analytics-plugin-perfumejs)
<br />

## Perfume is used by

- [Conio](https://business.conio.com/)
- [Coinbase](https://www.coinbase.com)
- [Coinbase Pro](https://pro.coinbase.com)
- [Coinbase Custody](https://custody.coinbase.com)
- [Financial-Times](https://github.com/Financial-Times/n-tracking)
- [Hearst](https://www.cosmopolitan.com/)
- [Plan](https://getplan.co)
- Add your company name :)
<br />

## Credits and Specs

Made with ☕️ by [@zizzamia](https://twitter.com/zizzamia) and
I want to thank some friends and projects for the work they did:

- [Leraging the Performance Metrics that Most Affect User Experience](https://developers.google.com/web/updates/2017/06/user-centric-performance-metrics) for documenting this new User-centric performance metrics;ev
- [Performance Timeline Level 2](https://w3c.github.io/performance-timeline/) the definition of _PerformanceObserver_ in that specification;
- [The Contributors](https://github.com/Zizzamia/perfume.js/graphs/contributors) for their much appreciated Pull Requests and bug reports;
- **you** for the star you'll give this project 😉 and for supporting me by giving my project a try 😄

### Contributors

This project exists thanks to all the people who contribute.
<img src="https://opencollective.com/perfumejs/contributors.svg?width=890&button=false" />

### Backers

Thank you to all our backers! 🙏 [[Become a backer](https://opencollective.com/perfumejs#backer)]

<a href="https://opencollective.com/perfumejs#backers" target="_blank"><img src="https://opencollective.com/perfumejs/backers.svg?width=890"></a>

<br />

## Copyright and license

Code and documentation copyright 2022 [Leonardo Zizzamia](https://twitter.com/Zizzamia). Code released under the [MIT license](LICENSE). Docs released under Creative Commons.

## Team

<table>
  <tbody>
    <tr>
      <td align="center" valign="top">
        <a href="https://twitter.com/Zizzamia">
          <img width="100" height="100" src="https://github.com/zizzamia.png?s=150">
        </a>
        <br />
        <a href="https://twitter.com/Zizzamia">Leonardo Zizzamia</a>
      </td>
     </tr>
  </tbody>
</table>
