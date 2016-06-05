---
layout: page
title:  Noze.io for Node people
permalink: /noze4node/
---

<img src="/images/noze-128x128.png" align="right" />

If you don't know Swift yet, checkout the
[Swift Tour](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/)
by Apple. Then come back.

Welcome back, you now know basic Swift, congratulations.

Noze.io is pretty similar to Node.js. You can import `fs`, `http`, `net`, etc
and most functions should be named the same like in Node.js. There is `concat`,
`nextTick` and `console.log`.
It is, however, not exactly the same. A few key differences:

- Modules are named similar but work a little different
  - they are neither objects nor functions 
  - you don't `require` them, but `import` them
  - you can't rename them (ala `let blub = require('blob')`)
  - they do not carry resources
  - they have no `__dirname`
- Get used to trailing closures, just attach a block to an event 
  (`stream.onReadable { ... }`, not `stream.on('readable', function() { })`)
- Noze.io only implements Node v3 streams, that is: the `readable` event and
  the matching `read()` method. The old-style `data` events are not available.
- Swift uses ARC while Node.js has a GC. You can mostly ignore that, but once
  your code gets more complex you need to consider retain cycles and such.
- Noze.io streams are typed and always work in batches. In Node.js only byte
  streams process batches, the Node.js "object-mode" is one-object-at-a-time.
- Being typed, a 'byte' (UInt8) stream and a `Character` or `String` stream
  are different things. Hence no `encoding` parameters and such.
  If you have a byte stream but you want to work with chars,
  pipe it through `utf8` or `readlines` (e.g. `stdin | readlines`).
- Noze.io overloads the pipe `|` operator (this is the only one), it is the
  same like the `.pipe()` method (which is also available).
- In Noze.io you can directly pipe from sequences into streams, like:
  `allFetchedRecords | record2html | response`

### Examples
  
Node original:

    fs.readFile('example_log.txt', function (err, logData) {
      if (err) { console.error("failed:", err); return; }
      console.log("got data:", logData);
    });

The same in Noze.io:

    fs.readFile("example_log.txt") { err, logData in
      if err { console.error("failed:", err); return }
      console.log("got data:", logData)
    }
