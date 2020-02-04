---
title: node.js - a few notes
date: "2020-01-29T23:50:37.121Z"
---

**Child Process in node.js**
(Excellent article by Samer Buna on child process)[https://jscomplete.com/learn/node-beyond-basics/child-processes]

**Unit testing node using chai, mocha, sinon**
Mocha - most commom framework used for node.js testing
This is simply a test runner.

Chai - Assertion library used along with mocha
General usage:
import 'chai from 'chai'
const expect = chai.expect  // creat a short cut for expect 
import chaiAsPromised from 'chai-as-promised'
chai.use(chaiAsPromised)

Sinon - mocking library most commonly used in JS

Gulp, Grunt -  task runners

