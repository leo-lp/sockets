# Socks

![Swift](http://img.shields.io/badge/swift-3.0-brightgreen.svg)
[![Build Status](https://travis-ci.org/vapor/core.svg?branch=master)](https://travis-ci.org/vapor/socks)
[![CircleCI](https://circleci.com/gh/vapor/core.svg?style=shield)](https://circleci.com/gh/vapor/socks)
[![Code Coverage](https://codecov.io/gh/vapor/core/branch/master/graph/badge.svg)](https://codecov.io/gh/vapor/socks)
[![Codebeat](https://codebeat.co/badges/a793ad97-47e3-40d9-82cf-2aafc516ef4e)](https://codebeat.co/projects/github-com-vapor-socks)
[![Slack Status](http://vapor.team/badge.svg)](http://vapor.team)

The package provides two libraries: `SocksCore` and `Socks`.
- `SocksCore` is just a Swift wrapper of the Berkeley sockets API with minimal differences. It is meant to be an easy way to use the low level API without having to deal with Swift/C interop.
- `Socks` is a library providing common usecases built on top of `SocksCore` - a simple `TCPClient`, `SynchronousTCPServer` etc.

If you're building a HTTP server, you'll probably want to use the `TCPClient`, without having to worry about its implementation details. However, if you need the low-level sockets API, just import `SocksCore` and use that instead.

> Pure-Swift Sockets. Linux & OS X ready.

## Usage
	
### A Simple TCP Client

[Full code](https://github.com/vapor/socks/blob/master/Sources/SocksExampleTCPClient/main.swift)

```swift
	import Socks

	let address = InternetAddress(hostname: "google.com", port: 80)
	do {
	    let client = try TCPClient(address: address)
	    try client.send(bytes: "GET /\r\n\r\n".toBytes())
	    let str = try client.receiveAll().toString()
	    try client.close()
	    print("Received: \n\(str)")
	} catch {
	    print("Error \(error)")
	}
```

### A Simple Echo Server

[Full code](https://github.com/vapor/socks/blob/master/Sources/SocksExampleTCPServer/main.swift)

```swift
    import Socks

	do {
	    let server = try SynchronousTCPServer(port: 8080)
	    print("Listening on \"\(server.address.hostname)\" (\(server.address.addressFamily)) \(server.address.port)")
	    
	    try server.startWithHandler { (client) in
	        // echo
	        let data = try client.receiveAll()
	        try client.send(bytes: data)
	        try client.close()
	        print("Echoed: \(try data.toString())")
	    }
	} catch {
	    print("Error \(error)")
	}
```

## 📖 Documentation

Visit the Vapor web framework's [documentation](http://docs.vapor.codes) for instructions on how to use this package.

## 💧 Community

Join the welcoming community of fellow Vapor developers in [slack](http://vapor.team).

## 🔧 Compatibility

This package has been tested on macOS and Ubuntu.

## 👥 Authors

Honza Dvorsky - http://honzadvorsky.com, [@czechboy0](http://twitter.com/czechboy0)  
Matthias Kreileder - [@matthiaskr1](https://twitter.com/matthiaskr1)
