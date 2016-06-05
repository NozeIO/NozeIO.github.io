---
layout: page
title:  Start
permalink: /start/
---

<img src="/images/noze-128x128.png" align="right" />

To get started with Noze.io we recommend

- that you get a [Swift installation](/install-swift)
  on either MacOS or Ubuntu
- that you [download](#download-nozeio) the Noze.io sources,
- and that you look at the [examples](#examples).
- You might also want to skim over some [documentation](#documentation).

## Download Noze.io

Noze.io is hosted on GitHub, you can download the thing from the
[Noze.io project site](https://github.com/NozeIO/Noze.io/tree/master), 
quicklinks: Clone to a local directory (recommended) via ...

    git clone https://github.com/NozeIO/Noze.io.git
    
... or [download as zip](https://github.com/NozeIO/Noze.io/archive/master.zip).
    
To build Noze.io in Xcode, just open the contained `Noze.io.xcodeproj` and
select the desired scheme.

To build Noze.io in a Unix shell, just type `make`. If you are using a Swift 3
drop, you can also call `swift build`.

## Examples

There is a reasonably large collection of simple and focused examples over here:
[Noze.io examples](https://github.com/NozeIO/Noze.io/tree/master/Samples/).
Three of them which may provide a good starting point:

- [express-simple](https://github.com/NozeIO/Noze.io/tree/master/Samples/express-simple/Sources/main.swift)
  is a more complex example using Mustache templates, middleware, forms,
  routes and JSON.
- [connect-git](https://github.com/NozeIO/Noze.io/tree/master/Samples/connect-git/main.swift)
  is a demo for typed streaming. `git` is invoked as a subprocess, its streamed
  byte output is converted into streams of Unicode lines which are parsed into
  streams of log-structs, which are emitted into an HTML stream.
- [echod](https://github.com/NozeIO/Noze.io/tree/master/Samples/echod/main.swift)
  is the simplest echo daemon possible.
  
How to build the examples?
In Xcode, choose the desired `samples-xyz` scheme (e.g. 
`samples-express-simple`) and build&run.
On the shell, just type `make` or `make samples` in the Noze.io root folder.
  

## Documentation

There isn't much Noze.io specific documentation yet. Help is very much
appreciated :-)

### If you know Node.js

If you don't know Swift yet, checkout the
[Swift Tour](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/)
by Apple. Then come back.
Welcome back, you now know basic Swift, congratulations.

Noze.io is pretty similar to Node.js. Import `fs`, `http`, `net`, etc
and most functions are named the same like in Node.js. There is `concat`,
`nextTick` and `console.log`.

It is, however, not exactly the same. Differences are being collected in
[Noze.io for Node people](/noze4node/).
  
Node original:

    fs.readFile('example_log.txt', function (err, logData) {
      if (err) { console.error("failed:", err); return; }
      console.log("got data:", logData);
    });

The same in Noze.io:

    fs.readFile("example_log.txt") { err, logData in
      if err != nil { console.error("failed:", err); return }
      console.log("got data:", logData)
    }


### If you don't know Node.js

If you don't know Node.js yet, having a look at some Node.js or Express.js 
tutorials/documentation isn't the worst idea.
Two nice ones:
[Stream Handbook](https://github.com/substack/stream-handbook)
and
[Express Hello World](http://expressjs.com/en/starter/hello-world.html).

In short: Noze.io is built around asynchronous streams which emit events when
data is available or when a stream is ready to write data. Those streams can
be piped into each other, just like streams on the Unix shell
(think `ls|sort|uniq`).

An example from
[connect-git](https://github.com/NozeIO/Noze.io/tree/master/Samples/connect-git/main.swift):

    let s = spawn("git", "log", "-100", "--pretty=format:%H|%an|<%ae>|%ad")
      | readlines
      | through2(linesToRecords)
      | through2(recordsToHTML)
      | response

You get the point. All this is running asynchronously and with back-pressure
control (that is, if an output stream is busy and can't take more input, the
input stream will pause).

More information can be found in [Noze.io for Non-Node people](/noze4nonnode).
