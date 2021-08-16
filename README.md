### Introduction

This module proxies MJPEG streams (from an IP camera, for example) to a given endpoint of your choice using [express.js](https://expressjs.com/).

### Events

* `error` If the stream errors.

* `addClient` When a new client begins consuming the proxy stream.

* `dropClient` When a client closes the connection with the proxy stream.

### Example Usage

```javascript
var express = require("express");
var proxy = require("express-mjpeg-proxy").Proxy;
var url = "Insert MJPEG stream here";

var server = express();
server.get("/", new proxy(url).requestHandler);
server.listen(8080);

```

### How to run

Run `npm start <STREAM URL> [PORT]` to start a sample express.js server running the stream. Note that you will need to have express.js installed in the package already (`npm install express`) in order for this to work.

### Details

When a new Proxy object is created, it will establish a connection to the MJPEG stream. Any new data from the stream will be sent to any client in the `clients` array.
The argument can either be a string (in the form a URL) or an object acceptable by [http.request](https://nodejs.org/api/http.html#http_http_request_options_callback):

```javascript
var proxy = new Proxy("URL"); // Works
var proxy = new Proxy({
	host: "...",
	port: "80",
	method: "GET"
	path: "/cam.mjpeg"
}); // Also works

```

When requestHandler is executed, it will add the new client to the clients array. When the client closes the connection, it will be removed from the clients array. requestHandler will also send headers to every new client that preserve the boundary value in the Content-Type header.
