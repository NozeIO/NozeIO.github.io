---
layout: page
title:  Noze.io Promises
permalink: /docs/promises/
---
Promises are a lot like Transform streams. Once they are ready, they push
forward a result. Presumably the sole difference is that promises just carry
one result, while streams, well produce a stream of results.

Look at the example given in the 
[PromiseKit Introduction](http://promisekit.org/introduction/):

    login()                         // returns a Promise
      .then {
        return API.fetchKittens()   // also returns a Promise
      }
      .then { fetchedKittens in
        self.kittens = fetchedKittens
        self.tableView.reloadData() // does not return a promise
      }
      .error { error in
        UIAlertView(…).show()
      }

Loading all kittens into memory before handing them off may not be the right
thing to do. Essentially everytime you have something which produces a 
collection of elements you might rather consider a stream (and get all the
streaming features like buffering, back-pressure control, filtering etc.)

Having said that, it is a waste to produce a full stream if you just need to
carry forward a single value. Like the login() action in the sample.

Noze.io comes with a simple Promise implementation. Not sure whether that makes 
sense.

## Swift Promises

There are a few libraries, I guess we just need a very simple Noze.io specific
implementation. It looks like a Promise should be a generic, encapsulating
arbitrary return values.

Noze.io promises would be a little easier than generic Swift promises as we
don't need support for multithreading (no barriers and such).
