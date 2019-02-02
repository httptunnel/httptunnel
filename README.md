# httptunnel

[![Build Status](https://travis-ci.org/httptunnel/httptunnel.svg?branch=master)](https://travis-ci.org/httptunnel/httptunnel)

httptunnel exposes your localhost to the world for easy testing and sharing! No need to mess with DNS or deploy just to have others test out your changes.

Great for working with browser testing tools like browserling or external api callback services like twilio which require a public url for callbacks.

## installation ##

```
npm install -g httptunnel
```

This will install the httptunnel module globally and add the 'lt' client cli tool to your PATH.

## use ##

Assuming your local server is running on port 8000, just use the ```lt``` command to start the tunnel.

```
lt --port 8000
```

Thats it! It will connect to the tunnel server, setup the tunnel, and tell you what url to use for your testing. This url will remain active for the duration of your session; so feel free to share it with others for happy fun time!

You can restart your local server all you want, ```lt``` is smart enough to detect this and reconnect once it is back.

### arguments

Below are some common arguments. See `lt --help` for additional arguments

* `--subdomain` request a named subdomain on the httptunnel server (default is random characters)
* `--local-host` proxy to a hostname other than localhost

You may also specify arguments via env variables.  E.x.

```
PORT=3000 lt
```

## API ##

The httptunnel client is also usable through an API (for test integration, automation, etc)

### httptunnel(port [,opts], fn)

Creates a new httptunnel to the specified local `port`. `fn` will be called once you have been assigned a public httptunnel url. `opts` can be used to request a specific `subdomain`.

```javascript
var httptunnel = require('httptunnel');

var tunnel = httptunnel(port, function(err, tunnel) {
    if (err) ...

    // the assigned public url for your tunnel
    // i.e. https://abcdefgjhij.httptunnel.tech
    tunnel.url;
});

tunnel.on('close', function() {
    // tunnels are closed
});
```

### opts

* `subdomain` A *string* value requesting a specific subdomain on the proxy server. **Note** You may not actually receive this name depending on availability.
* `local_host` Proxy to this hostname instead of `localhost`. This will also cause the `Host` header to be re-written to this value in proxied requests.

### Tunnel

The `tunnel` instance returned to your callback emits the following events

|event|args|description|
|----|----|----|
|request|info|fires when a request is processed by the tunnel, contains _method_ and _path_ fields|
|error|err|fires when an error happens on the tunnel|
|close||fires when the tunnel has closed|

The `tunnel` instance has the following methods

|method|args|description|
|----|----|----|
|close||close the tunnel|

## License ##
MIT
