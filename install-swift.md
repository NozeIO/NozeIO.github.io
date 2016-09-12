---
layout: page
title:  Install Swift
permalink: /install-swift/
---

You have the choice between:

- macOS and Ubuntu (Trusty/14.04 or Wily/15.10)
- on macOS between using Xcode or some plain editor (Emacs, vi, TextMate, etc)

If you have a Mac, the easiest is plain [Xcode](#xcode) 8.

## Xcode

Already have Xcode 8 installed? Make sure it is up to date.
Need Xcode? Available for free at
[developer.apple.com](https://developer.apple.com/xcode/download/).

Once you got it, head over to [Download Noze.io](/start/#download-nozeio).

## Linux or macOS "Unix-style"

[A manual tarball install of Swift](https://swift.org/download/#releases)
is just fine,
but [swiftenv](https://github.com/kylef/swiftenv) seems to be useful.
Swiftenv [installation steps](https://github.com/kylef/swiftenv#installation):

    git clone https://github.com/kylef/swiftenv.git ~/.swiftenv
    echo 'export SWIFTENV_ROOT="$HOME/.swiftenv"' >> ~/.bash_profile
    echo 'export PATH="$SWIFTENV_ROOT/bin:$PATH"' >> ~/.bash_profile
    echo 'eval "$(swiftenv init -)"' >> ~/.bash_profile

### Ubuntu 14.04 with Swift 3 GMc 1

    swiftenv install https://swift.org/builds/swift-3.0-GM-CANDIDATE/ubuntu1404/swift-3.0-GM-CANDIDATE/swift-3.0-GM-CANDIDATE-ubuntu14.04.tar.gz

### Ubuntu 15.10 with Swift 3 GMc 1

    swiftenv install https://swift.org/builds/swift-3.0-GM-CANDIDATE/ubuntu1510/swift-3.0-GM-CANDIDATE/swift-3.0-GM-CANDIDATE-ubuntu15.10.tar.gz

### macOS with Swift 3 GMc 1

Install Xcode 8, available for free at
[developer.apple.com](https://developer.apple.com/xcode/download/).

### Test Swift Installation

Make sure it works, if the thing below doesn't, Noze.io won't work either:

    swift
    import Dispatch
    let Q = DispatchQueue.global()
    Q.async { print("Hello!"); }
    import Glibc
    sleep(5)

Doesn't work? [Ask for help](/about/#contact) on Slack or mailinglist!
It does work? Head on to [Download Noze.io](/start/#download-nozeio)
