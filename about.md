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
- [Slack](https://nozeio.slack.com)
- [info@noze.io](mailto:info@noze.io)

### Status

- We chose the traditional Swift approach:
  Make something barely usable, though demoable,
  and release it with a 2.0 version tag.
  Then hope that the community kicks in and fills open spots.
  Well kinda. It's pretty good already! 😉

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
