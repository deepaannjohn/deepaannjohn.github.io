---
title: Logging in nodejs
date: "2020-01-22T22:12:03.284Z"
description: "Logging in nodejs applications"
---

One of the main package used for logging in nodejs apps is [log4js](https://www.npmjs.com/package/log4js).

**log4js**

Out of the box it supports the following features:

* coloured console logging to stdout or stderr
* file appender, with configurable log rolling based on file size or date
* a logger for connect/express servers
* configurable log message layout/patterns
* different log levels for different log categories (make some parts of your app log as DEBUG, others only ERRORS, etc.)

**Usage**
Minimalist version
var log4js = require('log4js');
var logger = log4js.getLogger();
logger.level = 'debug'; // default level is OFF - which means no logs at all.
logger.debug("Some debug messages");


[Setting level, category, appenders](https://log4js-node.github.io/log4js-node/api.html)


