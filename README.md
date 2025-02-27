# TCP in React Native

node's [net](https://nodejs.org/api/net.html) API in React Native

This module is used by [Peel](http://www.peel.com/)

## Install

- Create a new react-native project. [Check react-native getting started](http://facebook.github.io/react-native/docs/getting-started.html#content)

- In your project dir:

```
npm install react-native-tcp --save
```

**Note for iOS:** If your react-native version < 0.40 install with this tag instead:

```
npm install react-native-tcp@3.1.0 --save
```

## if using Cocoapods

Update the following line with your path to `node_modules/` and add it to your
podfile:

```ruby
pod 'TcpSockets', :path => '../node_modules/react-native-tcp'
```

## Link in the native dependency

```
react-native link react-native-tcp
```

## Additional dependencies

### Due to limitations in the react-native packager, streams need to be hacked in with [rn-nodeify](https://www.npmjs.com/package/rn-nodeify)

1. install rn-nodeify as a dev-dependency
   `npm install --save-dev rn-nodeify`
2. run rn-nodeify manually
   `rn-nodeify --install stream,process,util --hack`
3. optionally you can add this as a postinstall script
   `"postinstall": "rn-nodeify --install stream,process,util --hack"`

## Usage

### package.json

_only if you want to write require('net') in your javascript_

```json
{
  "browser": {
    "net": "react-native-tcp"
  }
}
```

### JS

_see/run [index.ios.js/index.android.js](examples/rctsockets) for a complete example, but basically it's just like net_

```js
var net = require("net");
// OR, if not shimming via package.json "browser" field:
// var net = require('react-native-tcp')

var server = net
  .createServer(function (socket) {
    socket.write("excellent!");
  })
  .listen(12345);

var client = net.createConnection(12345);

client.on("error", function (error) {
  console.log(error);
});

client.on("data", function (data) {
  console.log("message was received", data);
});
```

### TODO

add select tests from node's tests for net

PR's welcome!

### Note

**Problem**: My issue was that I am restricted to pulling packages from NPMJS and GitHub was not allowed. However, the version of react-native-tcp (4.0.0) on NPMJS was tied to an older version, and I was hitting into the dependency issue caused by TcpSocket's dependency on CocoaAsyncSocket. This dependency issue was only fixed on the latest version on Master branch, that is NOT linked to NPMJS.

**Solution**: Fork said version, remove TcpSocket's dependency on CocoaAsyncSocket from TcpSocket.podspec, maintain it on GitHub and publish on NPMJS as a scoped public package "react-native-tcp-no-cocoa" for people who are restricted to only pulling packages from NPMJS.

### Credits

All credits to original authors

_forked from [react-native-tcp](https://github.com/aprock/react-native-tcp)_

_originally forked from [react-native-udp](https://github.com/tradle/react-native-udp)_
