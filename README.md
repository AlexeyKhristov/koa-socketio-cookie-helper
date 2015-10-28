[![Build Status](https://travis-ci.org/anilanar/koa-ws-cookie-helper.svg?branch=master)](https://travis-ci.org/anilanar/koa-ws-cookie-helper) [![Coverage Status](http://img.shields.io/coveralls/anilanar/koa-ws-cookie-helper.svg)](https://coveralls.io/r/anilanar/koa-ws-cookie-helper?branch=master)
[![NPM version](https://badge.fury.io/js/koa-socketio-cookie-helper.svg)](http://badge.fury.io/js/koa-socketio-cookie-helper)

#koa-socketio-cookie-helper

koa.js + socket.io helper for accessing cookies from a WebSocket connection

## WebSocket

This is a fork of [anilanar/koa-ws-cookie-helper](https://github.com/anilanar/koa-ws-cookie-helper) and for `ws` module should be used original library

## Installation

```js
$ npm install koa-socketio-cookie-helper
```

## Example

```js
var app = require('koa')();
var server = require('http').createServer(app.callback());
var cookieHelper = require('koa-ws-cookie-helper');
app.keys = ['secret'];
var io = require('socket.io')(server);
io.on('connection', function(){
});
io.use(function (socket, next) {

  // tries to get value of cookie named 'foo'
  // e.g. for 'foo=bar', returns 'bar'
  var cookieValue = cookieHelper.get(socket, 'sid', app.keys);

  // if you use unsigned cookies or 
  // don't care if the cookie is legit or not
  // omit the keys parameter
  var cookieValue = cookieHelper.get(socket, 'sid');

});
server.listen(3000);
```

## How does it work?
Browsers put any cookies from the same host into initial Socket.io handshake headers. This lets typical HTTP-session mechanisms to work with Socket.io. [koa.js](http://github.com/koajs/koa) makes use of [Cookies](http://github.com/expressjs/cookies) module to handle cookie management. Signed cookies are signed and verified using [Keygrip](http://github.com/expressjs/keygrip). This module combines methods used by them to parse cookies in `ws.handshake.headers.cookie`.

## API
There is only a single public function:

###get(ws, name, [keys])
Returns value of the cookie named `name`, using keyset `keys`. Cookie is searched in `'Cookie'` entry of handshake headers. If no cookie could be found or if the signed cookie does not match with any key in the keyset, returns `undefined`.

## License
MIT