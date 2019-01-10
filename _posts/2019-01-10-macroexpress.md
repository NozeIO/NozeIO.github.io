---
layout: post
title: 𝓶Express
tags: nozeio linux swift server side swiftnio
---

Looks like more and more people are looking into 
[µExpress](https://www.alwaysrightinstitute.com/microexpress-nio/)
in search for a small, approachable and Express like,
[SwiftNIO](https://github.com/apple/swift-nio)
API.
Yet µExpress was "just" intended as a
[tutorial](https://www.alwaysrightinstitute.com/microexpress-nio/)
on how to create a tiny web framework on top of NIO,
not as a standalone library. 
Should we do 𝓶Express?

There are quite a few well known server side Swift frameworks,
for example
[Kitura](https://www.kitura.io)
by IBM.
Those tend to be pretty big, feature rich and very Swifty - not in a bad way,
just in a "different" way.
There is nothing wrong with that, and we encourage you to look into those!
<br>
*However*, some people are just looking for a quick way to do Node/Express, 
but using Swift. Nothing fancy, just something for doing quick and easy
endpoints, but in a more resource efficient, scalable and reliable way.

As demonstrated by [Noze.io](http://noze.io) and
[µExpress](https://www.alwaysrightinstitute.com/microexpress-nio/),
Swift can look almost exactly like JavaScript Node code:
```swift
import http

http.createServer { req, res in 
  res.writeHead(200, [ "Content-Type": "text/html" ])
  res.end("<h1>Hello World</h1>")
}
.listen(1337)
```

Right now the [Always Right Institute](http://www.alwaysrightinstitute.com)
is providing three solutions for doing Express-like, server side Swift:

- [Noze.io](http://noze.io/)
- [ApacheExpress](http://apacheexpress.io) / [mod_swift](http://mod-swift.org)
- [µExpress](https://www.alwaysrightinstitute.com/microexpress-nio/)


### Noze.io

Noze.io provides two core features: 
a Node/Express like API (see above),
and type-safe back-pressure aware pull-streams.
It is built on top of the async I/O provided by
[libdispatch](https://github.com/apple/swift-corelibs-libdispatch).

It is however, pretty slow 🐌 That has various reasons and is partially
due to being started in the Swift 2 timeframe.
<br>
The plan for Noze.io is to rebuild its typed pull streams on top of 
[SwiftNIO](https://github.com/apple/swift-nio),
maybe as something like `swift-nio-streams`.
However, there is no timeframe for that. ⏳

Summary: Not recommended for production.


### ApacheExpress

[ApacheExpress](http://apacheexpress.io) / [mod_swift](http://mod-swift.org) are
a great way to build Swift server applications.
It can exploit the
proven and battle tested functionality of Apache. 
Any kind of authentication mechanism you can think 
of, rate limiting / anti-DoS modules, or mod_ssl,
mod_h2 for HTTP/2 support, etc.
<br>
Using [ZeeQL](http://zeeql.io) you can even access databases
provided by mod_db.

```swift
LoadSwiftModule ApacheMain /usr/libexec/apache2/mods_demo.so
```

ApacheExpress also provides a Node/Express like API derived from Noze.io.
However, Apache modules operate *synchronously*.
Depending on the scale you are operating on (are you serving hundreds of 
thousands of concurrent connections?),
or if you have a workload which is highly I/O bound and asynchronous by nature
(e.g. a proxy server),
this can be an issue.

Summary: mod_swift / Apex are excellent ways to run Swift code on the server
         in a feature rich and **rock solid** way. *Synchronously*, though.

*(There used to be a time when Apache was consider a "big" server,
today the whole server is smaller than the even the Swift library
you are linking against.)*


### µExpress

Originally µExpress was "just" intended as a
[tutorial](https://www.alwaysrightinstitute.com/microexpress-nio/).
It demonstrates how a tiny but very useful web framework can be trivially
built on top of
[SwiftNIO](https://github.com/apple/swift-nio), in a few hundred lines of code.
Again, it intentionally looks a little like Express:
```swift
import MicroExpress

let app = Express()

app.get("/moo") { req, res, next in
  res.send("Muhhh")
}
app.get("/json") { _, res, _ in
  res.json([ "a": 42, "b": 1337 ])
}
app.get("/") { _, res, _ in
  res.send("Homepage")
}

app.listen(1337)
```

Though it was never really intended as a standalone library,
people seem to start using it for realz.
And even start submitting PRs 😘

*(We are also using µExpress in production because it is so
  convenient for small things ...)*

## 𝓶Express?

What we should really do is Noze.io v2 based on top of NIO, but:

> However, there is no timeframe for that. ⏳

So while we wait for that, should we create a 𝓶Express?

The idea of 𝓶Express would be, that it essentially becomes a thin wrapper
around NIO which provides many more Node.js like APIs. 
Just like Noze.io v2 **without** the type-safe pull-streams.

The appeal here is that 𝓶Express would be a really low hanging fruit.
The core could be forked off µExpress and many Express-like modules
could be quickly migrated over from Noze.io and/or ApacheExpress.
One example being the  middleware implementation which allows nesting of
routers like the real Express (µExpress has just one matching level).

However that should *not* become a fat framework.
The setup could be that of Noze.io: 
A set of tiny Swift modules (static libraries consumed as necessary).
It could build as a standalone Xcode project suitable for being embedded 
in applications, and as a regular SwiftPM project at the same time.

What to people think? Is there value in having that?
A small Express-like framework which is a little more than µ?

To drop your opinions, feel free to come along the
[Noze.io Slack](https://join.slack.com/t/nozeio/shared_invite/enQtNTIwODg1OTQwNjc5LTlmODhiZWI1NzA2M2M1NzY2ZjE5NjcxYWU2NzAzNzJlZGI3ODJiYmI4ZGVmNTQxMjIyOTc4OTM4YTRmMWY1NTk), or at 
[@noze_io](https://twitter.com/noze_io) 🤓
