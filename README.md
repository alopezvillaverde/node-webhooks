# node-webhooks [![Build Status](https://travis-ci.org/roccomuso/node-webhooks.svg?branch=master)](https://travis-ci.org/roccomuso/node-webhooks) [![NPM Version](https://img.shields.io/npm/v/node-webhooks.svg)](https://www.npmjs.com/package/node-webhooks)

=====================

## What WebHooks are used for

> Webhooks are "user-defined HTTP callbacks". They are usually triggered by some event, such as pushing code to a repository or a comment being posted to a blog. When that event occurs, the source site makes an HTTP request to the URI configured for the webhook. Users can configure them to cause events on one site to invoke behaviour on another. The action taken may be anything. Common uses are to trigger builds with continuous integration systems or to notify bug tracking systems. Since they use HTTP, they can be integrated into web services without adding new infrastructure.

## Install

    npm install node-webhooks --save

Supporting Node.js 0.12 or above.

## How it works

When a webHook is triggered it will send an HTTPS POST request to the attached URLs, containing a JSON-serialized Update (the one specified when you call the **trigger** method).

## Debug

This module makes use of the popular [debug](https://github.com/visionmedia/debug) package.Use the env variable to enable debug: <code>DEBUG=node-webhooks</code>.
To launch the example and enable debug: <code>DEBUG=node-webhooks node example.js</code>

## Usage

```javascript

// Initialize WebHooks module.
var WebHooks = require('node-webhooks');


var webHooks = new WebHooks({
    db: './webHooksDB.json', // json file that store webhook URLs
});

// sync instantation - add a new webhook called 'shortname1'
webHooks.add('shortname1', 'http://127.0.0.1:9000/prova/other_url').then(function(){
	// done
}).catch(function(err){
	console.log(err);
});

// add another webHook
webHooks.add('shortname2', 'http://127.0.0.1:9000/prova2/').then(function(){
	// done
}).catch(function(err){
	console.log(err);
});

// remove a single url attached to the given shortname
// webHooks.remove('shortname3', 'http://127.0.0.1:9000/query/').catch(function(err){console.error(err);});

// if no url is provided, remove all the urls attached to the given shortname
// webHooks.remove('shortname3').catch(function(err){console.error(err);});

// trigger a specific webHook
webHooks.trigger('shortname1', {data: 123});
webHooks.trigger('shortname2', {data: 123456}, {header: 'header'}); // payload will be sent as POST request with JSON body (Content-Type: application/json) and custom header

```

## API examples

webHooks are useful whenever you need to make sure that an external service get updates from your app.
You can easily develop in your APP this kind of webHooks entry-points.

- <code>GET /api/webhook/get</code>
Return the whole webHook DB file.

- <code>GET /api/webhook/get/[WebHookShortname]</code>
Return the selected WebHook.

- <code>POST /api/webhook/add/[WebHookShortname]</code>
Add a new URL for the selected webHook. Requires JSON params:

- <code>GET /api/webhook/delete/[WebHookShortname]</code>
Remove all the urls attached to the selected webHook.

- <code>POST /api/webhook/delete/[WebHookShortname]</code>
Remove only one single url attached to the selected webHook.
A json body with the url parameter is required: { "url": "http://..." }

- <code>POST /api/webhook/trigger/[WebHookShortname]</code>
Trigger a webHook. It requires a JSON body that will be turned over to the webHook URLs. You can also provide custom headers.



### Author

Rocco Musolino - hackerstribe.com
