---
layout: page
title:  About
permalink: /about/
---

*Evented I/O streams for Swift (yeah, a Node.js port)*.

To get started, jump over to our [Start](/start/) page.

    import net
    
    net.createServer { sock in
      sock.write("Welcome to Noze.io!\r\n")
      sock | sock
    }
    .listen(1337)

### Contact

Hey, we love feedback. Join the mailing list, Slack channel or just drop us
an email to tell us why this is crap (or not?).

- [Mailing List](https://groups.google.com/forum/#!forum/nozeio)
- [Slack](http://slack.noze.io)
- [info@noze.io](mailto:info@noze.io)


### Supported Swift Versions


| OS    | Swift | GCD     | Xcode                                                      | Make | SPM  |
| ----- | ----- | ------- | ---------------------------------------------------------- | ---- | ---- |
| macOS | 2.2.1 | builtin | [7.3.1](https://developer.apple.com/xcode/download/)       | 👍🏻  | 👎  |
| macOS | 2.3   | builtin | [8.0b](https://developer.apple.com/xcode/download/)        | 👍🏻  | 👎  |
| macOS | 3p3   | Objective-GCD | [8.0b](https://developer.apple.com/xcode/download/)        | 👍🏻  | 👍  |
| tuxOS | 2.2.1 | [snapshot](https://github.com/helje5/swift-corelibs-libdispatch) |  | 👍🏻  | 👎  |
| tuxOS | 3p2   | [upstream](https://github.com/apple/swift-corelibs-libdispatch/tree/experimental/foundation) |  | 👍🏻  | 👍  |

Noze.io supports Swift 2.2.1 in addition to the latest Swift 3 'preview'.
Swift 2.2.1 is still the only stable Swift version.
Supporting both makes some code in Noze ugly, but it is still required to make 
the usage of Noze.io smooth. Always found it a pain to use Swift 3 projects ...
(as you carefully need to select specific Swift 3 dumps as well as GCD).


### Status

- *Not for production*. Consider the version numbers.

- We chose the traditional Swift approach:
  Make something barely usable, though demoable,
  and release it with a 2.0 version tag.
  Then hope that the community kicks in and fills open spots.

- It already has
  [leftpad](https://github.com/NozeIO/Noze.io/tree/develop/Sources/leftpad).

- Implements primarily the happy path. Errors will error. Presumably this
  will improve over time.

- A huge strength of Node is the npm package environment and the
  <a href="http://heathersfilm.tripod.com/script.txt" target="ext">myriad</a>s 
  of packages available for it.
  Or wait, is it? Well, at least we have
  [leftpad](https://github.com/NozeIO/Noze.io/tree/develop/Sources/leftpad)
  included.
  And we hope that the [Swift package](https://swift.org/package-manager/)
  environment is going to grow as well.

- There are tons of open ends in Noze.io. We welcome contributions of all kinds.
  Grep the code for FIXME and TODO.
  
### Want to help?

Nice, we need lotz of help. Ideas:

- There are tons of Node tutorials, redo such for Noze.io. Or well, first check 
  whether they work in Noze.io in the first place and what is missing!
- There are a lot of node modules, many are really easy to implement. Same for
  core Node API, we still miss a lot of that.
- Tests & Examples
- Do some benchmarking with Apache Bench
- Packages, brew and apt
- Need Raspi installation instructions

### Who

Noze.io is brought to you by
[The Always Right Institute](http://www.alwaysrightinstitute.com)
and
[ZeeZide](http://zeezide.de).
We wouldn't be sad if more people would like to join the effort :-)


<hr />

<div style="text-align: right;">
  <i><a href="http://zeezide.com/contact.html">Imprint</a></i>
</div>
