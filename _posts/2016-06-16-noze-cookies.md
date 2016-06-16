---
layout: post
title: Noze.io 0.2.8 ∙ 🍪 & 🐞
tags: nozeio linux swift server side
---

<span style="color:#A4CC85">W</span>
<span style="color:#C257A7">W</span>
<span style="color:#DBA074">D</span>
<span style="color:#E35552">C</span> week.
WWDC adjustments. Plus some support for HTTP cookies as well as 
two rather major bug fixes.

Well, I guess we are not supposed to talk about specific WWDC changes. So lets
leave it at this: This release should work with the lastest stuff w/o extra
modifications.
Make sure to use the same Swift version in Noze.io and Swift (Noze.io works in
both, Swift 0.2.x and 0.3.x, but you may need to change the project to the
version you use, e.g. 
[in here](https://github.com/NozeIO/Noze.io/blob/0.2.8/xcconfig/Base.xcconfig)).

There was a major bug in the Swift 2.2 build for Linux. The examples would just
crash instead of working beautifully. This should be fixed now, it was due to
an API difference in the Swift 2 `libdispatch` mapping on Linux.
Speaking about `libdispatch`, the official master branch seems to be in flux
and doesn't seem to compile properly. 
Until this is back to a working state, we switched to a 
[snapshot](https://github.com/helje5/swift-corelibs-libdispatch.git).

### Changes in 0.2.8

- Update to Swift 3.0 Preview 1 & 2.3
- xcconfig
  - link against the latest iOS/macOS SDK (was 10.11)
  - within the Xcode beta, use Swift 2.3
  - enable whole-module optimization for release builds
  - set migration flags in Xcode project
  - `make run` works again within the `Samples` \(fixed `LD_LIBRARY_PATH`\)
- `xsys` module
  - no more `rand`, rewrite stuff to use `arc4random_uniform`
    - and a hack imp of `arc4random_uniform` for Linux, which doesn't export it
  - `time_t` extensions got bitz of documentation, added init(utc:)
  - export `strchr`
- `events` module
  - major fix in processing `once` events
- `fs` module
  - fix for consistent GCD crash when using Swift 2.x on Linux
- `http` module
  - added `Cookies` class which can be used to read and write cookies
  - [httpd-cookies](https://github.com/NozeIO/Noze.io/blob/master/Samples/httpd-cookies/main.swift)
     example to demonstrate the use of that
- `connect` module
  - new `cookieParser` middleware \(access via `req.cookies["x"]`\)
  - a very simple `session` middleware, the
    [express-simple](https://github.com/NozeIO/Noze.io/blob/master/Samples/express-simple/Sources/main.swift)
    example has a small click-counter demo for this

### Compiling against Noze.io in an Xcode workspace

There is a [step-by-step guide](/docs/create-own-httpd-xcode-s2)
on using an
[Xcode workspace](https://developer.apple.com/library/ios/featuredarticles/XcodeConcepts/Concept-Workspace.html)
to compile your own project against Noze.io.

This is just a headz-up on the
*Key tumbling stone*: Remember to adjust the `Build Configuration` name.
Noze.io as a cross-platform project uses 
`AppKitDebug`, `AppKitRelease`, `UIKitDebug` etc,
The Xcode default is just `Debug` and `Release`.
If that doesn't match, Xcode won't find the Noze.io libraries.

### Getting the stuff

Well, as usual, head over to
[GitHub](https://github.com/NozeIO/Noze.io/releases/tag/0.2.8)
and checkout the `master` branch or fetch the `0.2.8` tag.
Happz nozing!

_P.S.: Excited about Friday morning? Is it going to be amazing or just
       enterprisy-blue?_ 😁

hh
