---
layout: page
title:  Step-by-step httpd using Xcode / Swift 2.2
permalink: /docs/create-own-httpd-xcode-s2
---

Step-by-step guide to create a new Cocoa Xcode application project which
contains an Hello-World httpd server implemented using
[Noze.io](http://noze.io/).
It shows how to create the Cocoa project, how to create a Workspace and how
to link the Cocoa app to Noze.io.
This is the code we want to run inside the app:

    http.createServer { req, res in
      res.writeHead(200, [ "Content-Type": "text/html" ])
      res.end("<h1>Hello World</h1>")
    }
    .listen(1337)

A simple HTTP server which just prints "Hello World". To connect to the
server, use [http://localhost:1337/](http://localhost:1337/).

*Key tumbling stone*: Remember to adjust the `Build Configuration` name.
Noze.io as a cross-platform project uses 
`AppKitDebug`, `AppKitRelease`, `UIKitDebug` etc,
The Xcode default is just `Debug` and `Release`.

### Create a new project

The standard steps:

1. Select "New Project" in the Xcode file menu,
2. select "Cocoa Application" as the template,
3. give it a kewl name.

![](/images/create-own-httpd-xcode-s2/01-xcode-project.jpeg)
![](/images/create-own-httpd-xcode-s2/02-xcode-project.jpeg)
![](/images/create-own-httpd-xcode-s2/03-xcode-project.jpeg)
![](/images/create-own-httpd-xcode-s2/04-xcode-project.jpeg)

### Create a Workspace

A workspace is a container for multiple projects. The advantage is that
projects embedded into a workspace can directly use the build products
of other projects in the same container.
In this case the 'project' is our Cocoa app and the 'other project' is
Noze.io, which we want to link against.

Steps:

1. Select "New Workspace" in the Xcode file menu,
2. give it an awezome name,
3. with the new workspace selected, chose "Add Files to 'Workspace'" in the
   Xcode file menu,
4. add the Cocoa application xcodeproj we created above,
5. add the Noze.io.xcodeproj.

![](/images/create-own-httpd-xcode-s2/10-xcode-workspace.jpeg)
![](/images/create-own-httpd-xcode-s2/11-xcode-workspace.jpeg)
![](/images/create-own-httpd-xcode-s2/12-xode-workspace-addfiles.jpeg)
![](/images/create-own-httpd-xcode-s2/13-xcode-workspace-addproject.jpeg)
![](/images/create-own-httpd-xcode-s2/14-xcode-workspace-add-nozeio.jpeg)
![](/images/create-own-httpd-xcode-s2/15-xcode-workspace-done.jpeg)


### Add code and link to Noze.io

Next add the code to the `AppDelegate.swift` of your app, in the
`applicationDidFinishLauncing()` function as shown in the screenshot:

    http.createServer { req, res in
      res.writeHead(200, [ "Content-Type": "text/html" ])
      res.end("<h1>Hello World</h1>")
    }
    .listen(1337)

And

    import http

at the top of the file. Xcode should show an error that it can't import
`http`.

The workspace/project configuration needs a few adjustments, steps:

1. Make sure to select the scheme belonging to your application in the
   Xcode scheme popup. Xcode will auto-create plenty of schemes for all
   the Noze.io modules. Ignore/delete the ones you don't like.
2. Select your Cocoa application project, the Target, and the 'Build Phases'
   tab.
3. Open the "Link Binary with Libraries" section and hit the "+" button.
4. Add the following libs from the workspace:
     `libstreams.dylib`, `libnet.dylib`, `libhttp.dylib`.
5. Select the Cocoa application project, then the "Info" tab:
   rename the configuration `Debug` to `AppKitDebug`.
6. Build & Run

![](/images/create-own-httpd-xcode-s2/20-xcode-add-code.jpeg)
![](/images/create-own-httpd-xcode-s2/21-xcode-switch-scheme.jpeg)
![](/images/create-own-httpd-xcode-s2/22-xcode-build-phases.jpeg)
![](/images/create-own-httpd-xcode-s2/23-xcode-build-add-deps.jpeg)
![](/images/create-own-httpd-xcode-s2/24-xcode-build-deps-done.jpeg)
![](/images/create-own-httpd-xcode-s2/25-xcode-rename-config.jpeg)

### Mission accomplished.

![](/images/create-own-httpd-xcode-s2/30-xcode-it-lives.jpeg)

Depending on which Noze.io modules you use, just add the required modules
to the "Link Binary with Libraries" list. Or just add all if you want to
play with everything ;-)

Need help with anything? Feel free to join either the mailing list or the
Slack:

- [Mailing List](https://groups.google.com/forum/#!forum/nozeio)
- [Slack](http://slack.noze.io)
