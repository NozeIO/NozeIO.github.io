---
layout: default
title: Noze.io - Evented I/O streams for Swift
tags: nodejs swift server side streams
---

<p>
  <img src="https://img.shields.io/badge/swift-3-blue.svg" />
  <img src="https://img.shields.io/badge/swift-4-blue.svg" />
  <img src="https://img.shields.io/badge/os-macOS-green.svg?style=flat" />
  <img src="https://img.shields.io/badge/os-iOS-green.svg?style=flat"   />
  <img src="https://img.shields.io/badge/os-tuxOS-green.svg?style=flat" />
  <img src="https://api.travis-ci.org/NozeIO/Noze.io.svg?branch=master&style=flat" />
</p>

**Noze.io** is an attempt to carry over the
[Node.js](http://nodejs.org/)
ideas into *pure*
[Swift](http://swift.org/).
It uses
[libdispatch](https://github.com/apple/swift-corelibs-libdispatch")
for event-driven, non-blocking I/O.
**Noze.io** is built around type-safe back-pressure aware pull-streams
(using Swift generics)
operating on batches of items. Instead of working with just bytes,
deal with batches of Unicode lines or database records or HTML
responses or - you get the idea.
Be efficient: Stream everything and ßatch.

A focus is to keep the API similar to Node. Not always possible -
Swift is not JavaScript - but pretty close.
It comes with rechargeables included, <b>Noze.io</b> is self-contained and
doesn't require any extra dependencies.
No extra C modules, <em>pure</em> Swift.<br />
Haz awezome modules such as:
[cows](https://github.com/NozeIO/Noze.io/tree/master/Sources/cows),
[leftpad](https://github.com/NozeIO/Noze.io/tree/master/Sources/leftpad),
[express](https://github.com/NozeIO/Noze.io/tree/master/Sources/express), and
[redis](https://github.com/NozeIO/Noze.io/tree/master/Sources/redis).

**Noze.io** works in Cocoa environments as well as on Linux.
It uses Swift 3 or 4 on macOS or tuxOS.
Head over to our [Start](http://noze.io/start/) page 
for install instructions.

*Is it a good idea?* You [tell us](/about/).
Not sure, but we think it might be,
because:
*a)*
While Swift looks like JavaScript, it is actually a very
high performance, statically typed and AOT-compiled language,
*b)*
Code often looks better in Swift, mostly due to the trailing-closure syntax,
*c)*
No monkey patching while still providing extensions.
~~There are cons too.~~

#### Shows us some code!

There is a reasonably large collection of simple, focused:
[Noze.io examples](https://github.com/NozeIO/Noze.io/tree/master/Samples). 
But here you go, the "standard" Node example, a
HelloWorld httpd:

```swift
import http

http.createServer { req, res in 
  res.writeHead(200, [ "Content-Type": "text/html" ])
  res.end("<h1>Hello World</h1>")
}
.listen(1337)
```

An echo daemon, just piping the in-end of a socket into its own out-end:

```swift
import net

net.createServer { sock in
  sock.write("Welcome to Noze.io!\r\n")
  sock | sock
}
.listen(1337)
```

More complex stuff including a 
[Todo-MVC backend](https://github.com/NozeIO/Noze.io/blob/master/Samples/todo-mvc-redis/main.swift)
can be found in the
[Noze.io examples](https://github.com/NozeIO/Noze.io/tree/master/Samples).
Like what you see? Head over to our
[Start page](/start/)
to get started.

<hr />

<div class="posts">
  {% for post in site.posts %}
    <article class="post">

      <h1><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h1>

      <div class="entry">
        {{ post.excerpt }}
      </div>
      
      <div class="date">
        <table border="0" width="100%"> <!-- old skool -->
          <tr>
            <td>{{ post.date | date: "%B %e, %Y" }}</td>
            <td style="text-align:right;"><a href="{{ site.baseurl }}{{ post.url }}" class="read-more">Read More</a></td>
          </tr>
        </table>
      </div>
    </article>
  {% endfor %}
</div>
