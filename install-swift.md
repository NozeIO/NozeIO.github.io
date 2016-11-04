---
layout: page
title:  Install Swift
permalink: /install-swift/
---

You have the choice between:

- macOS and Ubuntu (14.04, 15.10 or 16.04)
- on macOS between using Xcode or some plain editor (Emacs, vi, TextMate, etc)

If you have a Mac, the easiest is plain [Xcode 8](#xcode) with Swift 3.0.

Due to a [bug](/swift3-note/) in the Linux Swift 3.0.0 release you
need to use 
[Swift 3.0.1](https://swift.org/download/#releases)
or later on Linux.

## Xcode

Already have Xcode 8 installed? Make sure it is up to date.
Need Xcode? Available for free at
[developer.apple.com](https://developer.apple.com/xcode/downloads/).

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

## Linux: Install dependencies

    sudo apt-get install -y \
       libcurl4-openssl-dev \
       clang make git libicu52 \
       autoconf libtool pkg-config \
       libblocksruntime-dev \
       libkqueue-dev \
       libpthread-workqueue-dev \
       systemtap-sdt-dev \
       libbsd-dev libbsd0 libbsd0-dbg

### Ubuntu 14.04 with Swift 3.0.1

    swiftenv install https://swift.org/builds/swift-3.0.1-release/ubuntu1404/swift-3.0.1-RELEASE/swift-3.0.1-RELEASE-ubuntu14.04.tar.gz

### Ubuntu 15.10 with Swift 3.0.1

    swiftenv install https://swift.org/builds/swift-3.0.1-release/ubuntu1510/swift-3.0.1-RELEASE/swift-3.0.1-RELEASE-ubuntu15.10.tar.gz

### Ubuntu 16.04 with Swift 3.0.1

    swiftenv install https://swift.org/builds/swift-3.0.1-release/ubuntu1604/swift-3.0.1-RELEASE/swift-3.0.1-RELEASE-ubuntu16.04.tar.gz

### macOS with Swift 3.0

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
