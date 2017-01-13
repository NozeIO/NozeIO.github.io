---
layout: post
title: State of the Union 2016
tags: nozeio linux swift server side
---

2016 - the year we [announced Noze.io](/announce/).
Let's review what happened wrt "Server Side Swift",
what we accomplished in **Noze.io**
and consider where we might go in the future.

## Server Side Swift in 2016

Without question 2016 was the year in which "Server Side Swift" became a thing.
Neither a big thing (we actually checked that via Adwords, very few people
look for it) nor a deployed thing, but a thing nevertheless.
And we still think that Swift may be a sound option for server side development 
for various reasons.

Today there is a pretty large set of server frameworks written for Swift:
there is
[Perfect](https://github.com/PerfectlySoft/Perfect),
[Vapor](https://github.com/vapor/vapor),
[Kitura](https://github.com/IBM-Swift/Kitura),
[Zewo](https://github.com/zewo/Zewo),
and many many more.
[This article](https://theswiftdev.com/2016/11/09/server-side-swift/)
is a pretty good summary with the proper outcome.<br />
I think it is reasonable fair to say that you quite likely don't want to use any
of them on a production system just yet.
Unless you are really 1337, know very well what you are doing or
[hire us](http://zeezide.com/en/services/services.html) to help you with it ;-)

Finally, at the end of the year, the
[Server APIs Project](https://swift.org/server-apis/)
was formed as an official top-level Swift project.
But more on that later.

### Swift 3

In 2016 most of the server frameworks targetted Swift 3,
despite Swift 3 not being released until late September.
Due to that using any of those frameworks was a *major pain*, with unstable
and incompatible Swift 3 preview releases. It didn't integrate into Xcode very
well. Let alone Linux, which required a painful setup to get `libdispatch`
support.
In retrospect we think it was a big mistake to push Server Side Swift before
Swift 3 was ready - 
the developer experience was absymal and probably has left a pretty
bad first impression.

When we presented Noze.io in June, we specifically tried to provide a good user
experience by fully supporting the latest Swift release - 2.x - as well as the
latest Swift 3 snapshot available.
The other thing we did is make Noze.io fully self contained (requires no
external packages).
It worked out of the box in Xcode, using a `make` based build, and even with the 
Swift 3 Package Manager.

So a key event for Server Side Swift was 
[the release of Swift 3](https://swift.org/blog/swift-3-0-released/)
on both macOS and GNU/Linux.
Most of the pain is gone today and you are provided with a reasonably stable 
platform to develop on.

As Swift 3 happend, we 
[released Noze.io 0.5.0](http://localhost:4000/swift301/)
as a Swift 3 only framework, dropping support for the now legacy Swift 2.x.

### Server APIs Project

End of October Chris Bailey of IBM
[announced](https://lists.swift.org/pipermail/swift-server-dev/Week-of-Mon-20161024/000000.html) 
the Swift
[Server APIs Project](https://swift.org/server-apis/).
You can read about it on the home page. The basic idea is that there will be a
standard set of server-centric APIs for Swift which all of the frameworks can
use (HTTP, TLS, etc.).

Though we have some (probably correct :-) conspiracy theories on how and why this 
was formed, we don't know for sure.
It presents itself as an effort open to anyone - people can take
part in discussions on the 
[mailing list](https://lists.swift.org/pipermail/swift-server-dev/),
take part in conference calls and
register themselves as
[Stakeholders](https://github.com/swift-server/work-group#stakeholders).
As of today 55 people enlisted as such though 10 of them are Apple
(just think about what that might imply).
There was some activity on the mailing list after the project was 
announced, but right now it seems rather dead.

While the [Always Right Institute](http://www.alwaysrightinstitute.com)
is actively taking part in the mailing list discussions,
providing valuable and *right* input,
we didn't enlist **Noze.io** as a 'Stakeholder'. Why is that?

There are various reasons but one is that we think that it will be very hard
to come up with reusable APIs for the various frameworks.
Such are just too different in their approaches (even agreeing on how
HTTP requests/responses are setup is a thing!).
Noze.io is no different.
The core concept of Noze.io are the generic pull streams and everything 
'server' lives around that. Hence either the effort results in something which
is like or better than Noze.io, or the APIs it produces are likely of limited 
value to Noze.io.

So here is what we *predict* for the effort in 2017:
Apple is already building stuff the way they like and with their own
usecases and server side background in mind.
This will be presented to the community which can request some tweaks and 
otherwise use it or not.<br />
Is that a bad thing? No. It certainly is a good thing to have something along 
the lines of Python
[`http.server`](https://docs.python.org/3/library/http.server.html) in Swift.
It just very likely won't be relevant to Noze.io.

### Outlook: Swift 4 or 5

What about upcoming Swift versions?
The big goal for Swift 4 seems to be API compatibility,
which is important but less relevant on the server.

There are three things of interest with regards to a future Swift version:

1. Asynchronous Programming
2. Reflection
3. Enhanced Generics

#### Asynchronous Programming

Something which may not hit Swift 4, but doesn't look too unlikely for Swift 5
is language support for asynchronous programming.
You know, the async/await thing
[JavaScript just got](https://www.twilio.com/blog/2015/10/asyncawait-the-hero-javascript-deserved.html), 
and which C# and other languages support for a long time.

This is going to affect Server Side Swift a *lot*.
It will dictate how asynchronous calls are implemented (could be done via
libdispatch, but not necessarily).
And it will likely result in a rewrite of major amounts of code.
Particularily in a callback intensive codebase like Noze.io (and applications
using such).

#### Reflection

Reflection is a bit counter to the static nature of Swift but kinda necessary 
to implement a lot of things required in server programming 'the usual way'.
Templates, ORMs, etc.
Swift has some support for reflection, but it is very limited in Swift 3.

Noze.io comes with a simple
[KVC implementation](https://github.com/NozeIO/Noze.io/blob/master/Sources/mustache/SimpleKVC.swift)
to drive its
[Mustache](https://github.com/NozeIO/Noze.io/tree/master/Sources/mustache)
templates. Again, very limited compare to what you can do in, say,
[SOPE](http://sope.opengroupware.org).

Should reflection become a feature in Swift 4, that may open up quite a few
possibilities.

#### Enhanced Generics

Generics in Swift 3 have a lot of limitations. Something which affects Noze.io
a lot is the lack of "materialized protocols".
When we started the "Generics version" of Noze.io
(there have been a few different attempts before we settled on this)
the plan was to have a protocol to represent a "bucket of items".

Something like this:

    protocol Bucket<T>                     { ... ops ... }
    extension Array        : Bucket        { ... imp of ops ... }
    extension DispatchData : Bucket<UInt8> { ... imp of ops ... }
    
    let buckets : Bucket<UInt8> = socket.read() // no-go

Unfortunately that doesn't fly in Swift 3 and is the reason why we are using
arrays (e.g. `[UInt8]`) to transport data from/to streams.
This is a major performance issue in Noze.io.

Summary: There is some hope that maybe even Swift 4 may provide a more flexible
generics implementation.

## Noze.io in 2016

Well, and what happened with Noze.io in 2016. First of all - it 
[got released](/announce/) 
on June 6th, 2016 :-)
Besides the implementation of our streams
that version tagged 0.2 already included major modules like `leftpad`.
It came with a great 
[echo daemon](https://github.com/NozeIO/Noze.io/blob/master/Samples/echod/main.swift), 
and even included a simple 
[IRC server](https://github.com/NozeIO/Noze.io/tree/master/Samples/miniirc)
example.

As mentioned we started out as a dual Swift 2.2/3.0preview project and 
Noze.io 0.5 switched to Swift 3 only when that got released.
We added and enhanced quite a lot of modules:
most importantly
[cows](https://github.com/NozeIO/Noze.io/blob/master/Sources/cows/README.md),
but also worth mentioning are
[connect](https://github.com/NozeIO/Noze.io/tree/master/Sources/connect),
[express](https://github.com/NozeIO/Noze.io/tree/master/Sources/express) or
[redis](https://github.com/NozeIO/Noze.io/tree/master/Sources/redis).

The Noze.io
[TodoMVC](https://github.com/NozeIO/noze-todomvc)
project is a great demo which brings together many of the features.

<img style="border: 1px solid #AAA; padding: 0.5em;"
     src="https://camo.githubusercontent.com/002a3b73696677a31147049666b46826146a7370/687474703a2f2f6e6f7a652e696f2f696d616765732f73637265656e73686f742d746f646f2d6d76632d72656469732d312e6a706567" />

Summary: We are quite happy about how **Noze.io** turned out. Sure, it still has
a lot of flaws and is more a neat demo than a viable toolkit for production 
apps,
but we think the goal of replicating (and improving) core Node ideas in Swift
has been accomplished.

What we think sets Noze.io apart from all the other frameworks is the thorough
focus on 
**typesafe streaming of arbitrary objects**
instead of 'just' providing a web application layer.
It really is more an 'Async I/O Foundation library' than an application server.
Noze.io still seems to be pretty unique in that regard.

While it currently lacks a lot to actually perform in reality,
conceptually Noze.io provides superior scalability for large scale applications.

Another thing Noze.io did exceptionally well is lowering the barrier of entry
for Node developers.
We kept the API
[as similar as possible](http://noze.io/start/#if-you-know-nodejs).
And all that in a Swifty, type-safe manner instead of reimplementating 
JavaScript in Swift / resorting to `Any`.

Was it popular in 2016? Well, we are happy about each of the 152 GitHub stars we 
received. Thanks a lot!
That doesn't compare to the 8k [Vapor](https://vapor.codes) has,
but then we aren't as good in coming up with bold claims :-)
Worth noting: What didn't really happen are external contributions.

## Noze.io in the Year of the Trump

Assuming no one pushes the red button, what are the plans for 2017.
There are no specific plans.
But there are certainly a lot of things to do:

#### Leaks

Noze.io has some leaks.
Those need to be found and be eliminated.
This is no fun work.

#### Performance

Right now it is futile to performance test Noze.io.
Nothing, again, nothing has been done to actually make it fast.
All the focus was on the design.
Premature optimization is the root of all evil.
Your:

    ab -n 100000 -c 10000 http://...

is going to fail miserably.
If only because the listen queue is set to a ridiculously low value of 5 :-) 

Since the conceptual implementation is done, now is the time to optimize the 
code. There are a few areas:

- do not use Array to carry payloads, avoid copying
- drop Source/Target objects, overdesign which eats performance
- http_parser, we should use the C version, or maybe an optimized Swift one
- checkout `libuv` as a replacement for `libdispatch`
- use Instruments

#### TLS

Seriously, it is 2017, we should have TLS support. Unfortunately that is harder
than necessary given that Apple doesn't ship OpenSSL anymore.
We already started playing with OpenSSL and how to hook it up with asynchronous
I/O, which should be possible.
Boring work, needs to be done.

#### HTTP/2

HTTP/2 is an interesting topic. We have some design ideas on how to integrate 
HTTP/2 with our streaming core.
Implementing the protocol itself should not be
super difficult, it doesn't look like using a separate library would be 
necessary.
One aspect which is a little difficult to deal with are streaming priorities
as we have no I/O scheduler. Shouldn't matter for a simple implementation.
The bigger issue is that real-life HTTP/2 requires TLS, see above.

#### PostgreSQL Protocol Support

We have a prototype of an own, stream based, PostgreSQL protocol implementation 
working. It can login, execute simple queries, etc.
Not in shape yet for public consumption, but we'll see that rather sooner than
later :-)
Why? Because [PostgreSQL](https://www.postgresql.org) is really, really nice.

#### Make Driver

We'd really like to see `noze` in `brew`. You know, this kind of thing:

    brew install noze
    noze create app todo-mvc
    noze serve

Which then tracks and recompiles the app when the sources are edited, etc.

### What would *you* like to see?

- Do you even care about the cool streaming which is the core of Noze.io?
- Or is the feature you like most the Node compatibility?
- What would you like to see in Noze.io?

[Let us know](/about/#contact)!

## Summary

Now that Chris Lattner has left the Swift team at Apple,
it is safe to say that
Swift can be considered pretty much dead and 
we can copy all the code to `/dev/null`.
Why did we even write all that State-of-the-Union stuff?
No idea, seriously.

Yet there is no question, this statement is *right*:
2017 is going to be the year of [Swifter](http://swifter-lang.org).

... also, we wish everyone, well most of you, a Happy New Year and Happy Nozing!
