
<p align="center">

    <img src="https://cloud.githubusercontent.com/assets/1016365/10639063/138338bc-7806-11e5-8057-d34c75f3cafc.png" alt="Universal Angular 2" height="320"/>

</p>

# Angular 2 Universal Starter [![Universal Angular 2](https://img.shields.io/badge/universal-angular2-brightgreen.svg?style=flat)](https://github.com/angular/universal)
> Server-Side Rendering for Angular 2

A minimal Angular 2 starter for Universal JavaScript using TypeScript 2 and Webpack 2

> If you're looking for the Angular Universal repo go to [**angular/universal**](https://github.com/angular/universal)  

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

## Universal "Gotchas"

> When building Universal components in Angular 2 there are a few things to keep in mind.

 - To use `templateUrl` or `styleUrls` you must use **`angular2-template-loader`** in your TS loaders.
    - This is already setup within this starter repo. Look at the webpack.config file [here](https://github.com/angular/universal-starter/blob/master/webpack.config.ts) for details & implementation.
 - **`window`**, **`document`**, **`navigator`**, and other browser types - _do not exist on the server_ - so using them, or any library that uses them (jQuery for example) will not work. You do have some options, if you truly need some of this functionality:
    - If you need to use them, consider limiting them to only your main.client and wrapping them situationally with the imported *isBrowser / isNode* features from Universal.  `import { isBrowser, isNode } from 'angular2-universal'`;
    - Another option is using `DOM` from ["@angular/platform-browser"](https://github.com/angular/angular/blob/e3687706c71beb7c9dbdae1bbb5fbbcea588c476/modules/%40angular/platform-browser/src/dom/dom_adapter.ts#L34)
 - **Don't manipulate the nativeElement directly**. Use the _Renderer_. We do this to ensure that in any environment we're able to change our view.
```
constructor(element: ElementRef, renderer: Renderer) {
  renderer.setElementStyle(element.nativeElement, 'font-size', 'x-large');
}
```
 - The application runs XHR requests on the server & once again on the Client-side (when the application bootstraps)
    - Use a [UniversalCache](https://github.com/angular/universal-starter/blob/master/src/+app/shared/api.service.ts#L47-L71) instead of regular Http, to save certain requests so they aren't re-ran again on the Client.
 - Know the difference between attributes and properties in relation to the DOM.
 - Keep your directives stateless as much as possible. For stateful directives, you may need to provide an attribute that reflects the corresponding property with an initial string value such as url in img tag. For our native <img src=""> element the src attribute is reflected as the src property of the element type HTMLImageElement.

## Installation

* `npm install`

## Serve

* `npm start` to build your client app and start a web server
* `npm run build` to prepare a distributable bundle

## Development
* run `npm start` and `npm run watch` in two separate terminals to build your client app, start a web server, and allow file changes to update in realtime

## Watch files
* `npm run watch` to build your client app and start a web server

## AoT and Prod
* `npm run build:prod:ngc` to compile the ngfactory files and build prod

## Edge case of server compatibility with Promise polyfills

If you have node modules with promise polyfill dependency on server - there is chance to get the following exception:
```
Error: Zone.js has detected that ZoneAwarePromise `(window|global).Promise` has been overwritten.
```
It occurs because [Zone.js](https://github.com/angular/zone.js/) Promise implementation is not
detected as Promise by some polyfills (e.g. [es6-promise](https://github.com/stefanpenner/es6-promise) before 4.x).

To sort it out, you need such polyfills initialized before zone.js. Zone.js is initialized in 'angular2-universal-polyfills'
import of [server.ts](https://github.com/angular/universal-starter/blob/master/src/server.ts#L4). So import problematic
modules before this line.

### Documentation
[Design Doc](https://docs.google.com/document/d/1q6g9UlmEZDXgrkY88AJZ6MUrUxcnwhBGS0EXbVlYicY)

### Videos
Angular 2 Universal Patterns - ng-conf, May 2016  
[![Angular 2 Universal Patterns](http://img.youtube.com/vi/TCj_oC3m6_U/0.jpg)](https://www.youtube.com/watch?v=TCj_oC3m6_U)

Angular Universal Source Code - ReadTheSource, January 2016  
[![Angular Universal Source Code](http://img.youtube.com/vi/qOjtFjXoebY/0.jpg)](https://www.youtube.com/watch?v=qOjtFjXoebY)

Full Stack Angular 2 - AngularConnect, Oct 2015  
[![Full Stack Angular 2](https://img.youtube.com/vi/MtoHFDfi8FM/0.jpg)](https://www.youtube.com/watch?v=MtoHFDfi8FM)

Angular 2 Server Rendering - Angular U, July 2015  
[![Angular 2 Server Rendering](http://img.youtube.com/vi/0wvZ7gakqV4/0.jpg)](http://www.youtube.com/watch?v=0wvZ7gakqV4)

## [preboot.js](https://github.com/angular/preboot)
> Control server-rendered page and transfer state before client-side web app loads to the client-side-app.

# License
[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg?style=flat)](/LICENSE)
