---
layout: page
title:  Noze.io for people who don't know Node
permalink: /noze4nonnode/
---

<img src="/images/noze-128x128.png" align="right" />

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

## Main Queue

An important part is the concept of `ticks`. Being JavaScript, in Node.js
all user code essentially runs in the main thread. If a component wants to do
something later it can call `nextTick()` or `setTimeout()`.

*So why is that kinda good?* Well, the user doesn't have to think as much about
concurrency. All user code is running in the same thread. Although parts of a
function might run in an interleaved way, they never run at the same time. No
need to 'lock' stuff to protect against concurrent access etc.

Noze.io has the same concept. Though it is way easier to do threading if you
need too (e.g. to offload expensive calculations to another CPU).
All event handlers and such of Noze.io run on a single queue - usually the GCD
main-queue.

Pretty much the same thing like with the UIKit/Cocoa main runloop.

*So why is that kinda bad?* Well, in Node you are kinda running a multiuser
server in a single thread. This isn't as bad as usual because all the I/O is
evented and your code won't block waiting for such.
However - if your user code is running an expensive calculation, only this code
block is running - blocking everything else. All the sessions of the other 
users can't run.

*So why isn't that kinda not so bad in Noze.io?* Swift, unlike JavaScript, does
support concurrent execution. One can span thread or just use GCD, which
handles all the pooling for you. In other words the expensive code can be
offloaded to another thread. Of course you then have to deal with concurrency.

Sample:

    let workerQ = DispatchQueue(label      : "de.zeezide", 
                                qos        : .background,
                                attributes : .concurrent)
    
    socket | through2 { chunk, _, end in
      workerQ.async {
        // do something expensive with the chunk
        core.Q.async { end(nil, chunk) } // main Q
      }
    } | socket

Final note: In Noze.io the Noze.io Q is usually, but does not have to be, the
            main queue. Refer to the Noze.io queue using `core.Q`.


## Streams

When you are coming from Cocoa or Java you are probably thinking
`NSInputStream` or `java.io.InputStream`. Sure they are streams, but they are
not exactly like Noze.io streams. Noze.io streams are a little more general.

There are a few key differences:

a) Noze.io streams work on any type of Element, not just on UInt8. You can
   stream strings, you can stream log messages, database records etc etc.
   Those are all handled the same way!
   In contrast a `java.io.InputStream` only reads bytes. You want to read 
   Unicode characters you need to use a completely separate `java.io.Reader`
   class, etc.

b) Noze.io streams emit JavaScript-like events and are non-blocking. If 
   something is available to read - the stream posts an `readable` event, and 
   only then the client consumes from the stream.

c) Noze.io streams all have an internal buffer. (no wrapping in BufferedStream)

d) Noze.io write streams can 'fill' and send an `drain` event if they are ready
   to accept more content - they are able to limit back pressure.


Most of the ideas come from Node.js. Notably Node has two 'versions' of streams:
- v1 - those are push ('flowing') streams, which essentially embed the payload 
       in the event
- v3 - those are pull streams, they send an event, but then the content is 
       `read()` from the stream
Noze.io does v3 streams.


OK, this is something you can conceptually (not all streams may be implemented
yet) do with Noze.io streams - everything is completely asynchronous and 
typesafe:

    var byteCount = 0
    fs.createReadStream("/tmp/inputfile.txt")
      | through2 { buf, _, end in byteCount += buf?.length ?? 0; end(nil, buf) }
      | gzip()
      | crypto.createCipher('aes-256-cbc', '0xDEADBEEF')
      | fs.createWriteStream("/tmp/outfile.gz.aes")
      .onFinish("all done!")

You get the idea. The `|` operators is just an alias for `src.pipe(target)`. The
pipe listens for `readable` events on the source and writes the read data to the
target. If the target gets full, the source is paused and an `drain` event
handler is installed. Thus removing back pressure. Etc.

Note: In Noze.io you can also pipe from regular Swift sequences or Strings.
Example:

    "Hello World!" 
    | spawn("wc", "-l") 
    | concat { data in print("\(data)") }


