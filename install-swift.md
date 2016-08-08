---
layout: page
title:  Install Swift
permalink: /install-swift/
---

You have the choice between:

- macOS and Ubuntu (14.04 or 15.10)
- Swift 2.2/2.3 and the latest Swift 3 preview
- On macOS between using Xcode or some plain editor (Emacs, vi, TextMate, etc)

If you have a Mac, the easiest is plain [Xcode](#xcode) with Swift 2.2/2.3.

If you live on Linux, Swift 2.2 is more stable than 3. On Linux remember to
install [libdispatch](#linux-install-libdispatch)!

## Xcode

Already have Xcode 7.3 installed? Make sure it is up to date.
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

**Important**: On Linux you also need to install
[libdispatch](#linux-install-libdispatch),
on macOS this is included in all builds.

### Ubuntu 14.04 with Swift 2.2.1

    swiftenv install https://swift.org/builds/swift-2.2.1-release/ubuntu1404/swift-2.2.1-RELEASE/swift-2.2.1-RELEASE-ubuntu14.04.tar.gz

### Ubuntu 15.10 with Swift 2.2.1

    swiftenv install https://swift.org/builds/swift-2.2.1-release/ubuntu1510/swift-2.2.1-RELEASE/swift-2.2.1-RELEASE-ubuntu15.10.tar.gz

### Ubuntu with Swift 3 Preview 4

    swiftenv install https://swift.org/builds/swift-3.0-preview-4/ubuntu1404/swift-3.0-PREVIEW-4/swift-3.0-PREVIEW-4-ubuntu14.04.tar.gz

### macOS with Swift 3 Preview 2

Install an Xcode 8 beta, available for free at
[developer.apple.com](https://developer.apple.com/xcode/download/).

Note: This drop uses the 'objectified' libdispatch API.

## Linux: Install libdispatch

This can be a little messy as there is no final release of `libdispatch` for
Linux. It seems to work fine for me nevertheless.

Update: 2016-07-04:
For Swift 2.2.1 use our
[libdispatch snapshot](https://github.com/helje5/swift-corelibs-libdispatch.git).
For Swift 3preview4 use the `experimental/foundation` branch.
This: `02da60f1d26bd6f97b4ffd5053aa057aee700978` version is used on Travis and
seems to work fine.

1. Make sure you have Swift installed (can you run `swift`?).
2. Grab libdispatch
3. Install prerequisites
3. Configure, patch, compile and install it.
4. Make sure it works.

Install dependencies:

    sudo apt-get install -y \
       clang make git libicu52 \
       autoconf libtool pkg-config \
       libblocksruntime-dev \
       libkqueue-dev \
       libpthread-workqueue-dev \
       systemtap-sdt-dev \
       libbsd-dev libbsd0 libbsd0-dbg

Swift 2.2.1:

    git clone --recursive https://github.com/helje5/swift-corelibs-libdispatch.git
    cd swift-corelibs-libdispatch

Swift 3 Preview 4:

    git clone https://github.com/apple/swift-corelibs-libdispatch.git
    cd swift-corelibs-libdispatch
    git checkout 02da60f1d26bd6f97b4ffd5053aa057aee700978
    git submodule init
    git submodule update

Build:
  
    TT_SWIFT_BINARY=`swiftenv which swift`
    TT_SNAP_DIR=`echo $TT_SWIFT_BINARY | sed "s|/usr/bin/swift||g"`
    
    export CC=clang
    export LD=ld.gold
    ./autogen.sh
    ./configure --with-swift-toolchain=${TT_SNAP_DIR}/usr \
                --prefix=${TT_SNAP_DIR}/usr
    make all

*Swift 2.2.1*: If the `make all` fails with errors, try this:

    wget https://raw.githubusercontent.com/NozeIO/Noze.io/master/xcconfig/dispatch.h-patched-swift3
    cp dispatch.h-patched-swift3 dispatch/dispatch.h

*Swift 3 Preview 4*: If the `make all` crashes `ld` (the linker aborts), you may
need to explicitly link with `ld.gold`.
Sometimes the linker setting doesn't seem to be picked up properly.
This does it for me on Trusty:

    pushd /usr/bin
    sudo ln -sf ld.gold ld
    popd

Still doesn't compile? [Ask for help](/about/#contact) on Slack or mailinglist!

Finally install it:

    make install

Make sure it works, if the thing below doesn't, Noze.io won't work either:

    swift -Xcc -fblocks -Xlinker -ldispatch
    import Dispatch
    let Q = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0)!
    dispatch_async(Q, { print("Hello!"); })
    import Glibc
    sleep(5)

Doesn't work? [Ask for help](/about/#contact) on Slack or mailinglist!

It does work? Head on to [Download Noze.io](/start/#download-nozeio)
